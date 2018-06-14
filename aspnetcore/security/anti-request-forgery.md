---
title: Ataques de evitar intersite solicitar CSRF (falsificação XSRF /) no núcleo do ASP.NET
author: steve-smith
description: Saiba como evitar ataques contra aplicativos web em que um site mal-intencionado pode influenciar a interação entre o aplicativo e um navegador cliente.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 3bca96f4a2e247eeeb93140df93221371d88d4d3
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341854"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="7c46d-103">Ataques de evitar intersite solicitar CSRF (falsificação XSRF /) no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7c46d-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="7c46d-104">Por [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c46d-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7c46d-105">Falsificação de solicitação entre sites (também conhecido como XSRF ou CSRF, pronunciado *consulte navegação*) é um ataque contra aplicativos web hospedados no qual um aplicativo web mal-intencionado pode influenciar a interação entre o navegador do cliente e um aplicativo web que confia que Navegador.</span><span class="sxs-lookup"><span data-stu-id="7c46d-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="7c46d-106">Esses ataques são possíveis, como navegadores da web enviam alguns tipos de tokens de autenticação automaticamente com todas as solicitações para um site.</span><span class="sxs-lookup"><span data-stu-id="7c46d-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="7c46d-107">Essa forma de exploração é também conhecido como um *ataque de um clique* ou *sessão riding* porque o ataque aproveita o usuário do autenticado anteriormente sessão.</span><span class="sxs-lookup"><span data-stu-id="7c46d-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="7c46d-108">Um exemplo de um ataque CSRF:</span><span class="sxs-lookup"><span data-stu-id="7c46d-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="7c46d-109">Um usuário entra no `www.good-banking-site.com` usando a autenticação de formulários.</span><span class="sxs-lookup"><span data-stu-id="7c46d-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="7c46d-110">O servidor autentica o usuário e envia uma resposta que inclui um cookie de autenticação.</span><span class="sxs-lookup"><span data-stu-id="7c46d-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="7c46d-111">O site é vulnerável a ataques, pois ele confia qualquer solicitação recebida com um cookie de autenticação válido.</span><span class="sxs-lookup"><span data-stu-id="7c46d-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="7c46d-112">O usuário acessa um site mal-intencionado, `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="7c46d-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="7c46d-113">O site mal-intencionado, `www.bad-crook-site.com`, contém um formulário HTML semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="7c46d-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="7c46d-114">Observe que o formulário `action` postagens para o site vulnerável, não para o site mal-intencionado.</span><span class="sxs-lookup"><span data-stu-id="7c46d-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="7c46d-115">Esta é a parte de "sites" de CSRF.</span><span class="sxs-lookup"><span data-stu-id="7c46d-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="7c46d-116">O usuário seleciona o botão de envio.</span><span class="sxs-lookup"><span data-stu-id="7c46d-116">The user selects the submit button.</span></span> <span data-ttu-id="7c46d-117">O navegador faz a solicitação e inclui automaticamente o cookie de autenticação para o domínio solicitado, `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="7c46d-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="7c46d-118">A solicitação é executada `www.good-banking-site.com` server com o contexto de autenticação do usuário e podem executar qualquer ação que um usuário autenticado tem permissão para executar.</span><span class="sxs-lookup"><span data-stu-id="7c46d-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="7c46d-119">Além do cenário em que o usuário seleciona o botão para enviar o formulário, o site mal-intencionado pode:</span><span class="sxs-lookup"><span data-stu-id="7c46d-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="7c46d-120">Execute um script que envia automaticamente o formulário.</span><span class="sxs-lookup"><span data-stu-id="7c46d-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="7c46d-121">Envie o envio do formulário como uma solicitação AJAX.</span><span class="sxs-lookup"><span data-stu-id="7c46d-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="7c46d-122">Oculte o formulário usando CSS.</span><span class="sxs-lookup"><span data-stu-id="7c46d-122">Hide the form using CSS.</span></span>

<span data-ttu-id="7c46d-123">Esses cenários alternativos não exigem qualquer entrada do usuário que não seja inicialmente visitando o site mal-intencionado ou ação.</span><span class="sxs-lookup"><span data-stu-id="7c46d-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="7c46d-124">Usar o HTTPS não impede que um ataque CSRF.</span><span class="sxs-lookup"><span data-stu-id="7c46d-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="7c46d-125">O site mal-intencionado pode enviar um `https://www.good-banking-site.com/` solicitação tão fácil quanto que pode enviar uma solicitação não segura.</span><span class="sxs-lookup"><span data-stu-id="7c46d-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="7c46d-126">Alguns ataques de destino pontos de extremidade que respondem às solicitações GET, caso em que uma marca de imagem pode ser usada para executar a ação.</span><span class="sxs-lookup"><span data-stu-id="7c46d-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="7c46d-127">Essa forma de ataque é comum em sites de Fórum que permitem imagens mas bloqueiam JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c46d-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="7c46d-128">Aplicativos que alteram o estado em solicitações GET, onde variáveis ou os recursos são alterados, são vulneráveis a ataques mal-intencionados.</span><span class="sxs-lookup"><span data-stu-id="7c46d-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="7c46d-129">**Solicitações GET que mudam de estado são seguros. Uma prática recomendada é nunca alterar estado em uma solicitação GET.**</span><span class="sxs-lookup"><span data-stu-id="7c46d-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="7c46d-130">Ataques CSRF são possíveis em relação a aplicativos web que usam cookies de autenticação porque:</span><span class="sxs-lookup"><span data-stu-id="7c46d-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="7c46d-131">Os navegadores armazenam os cookies emitidos por um aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="7c46d-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="7c46d-132">Armazenado cookies incluem cookies de sessão para usuários autenticados.</span><span class="sxs-lookup"><span data-stu-id="7c46d-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="7c46d-133">Navegadores enviam que todos os cookies associados a um domínio ao aplicativo web cada solicitação, independentemente de como a solicitação ao aplicativo foi gerada dentro do navegador.</span><span class="sxs-lookup"><span data-stu-id="7c46d-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="7c46d-134">No entanto, os ataques CSRF não estão limitadas a exploração de cookies.</span><span class="sxs-lookup"><span data-stu-id="7c46d-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="7c46d-135">Por exemplo, a autenticação básica e resumida também são vulneráveis.</span><span class="sxs-lookup"><span data-stu-id="7c46d-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="7c46d-136">Depois que um usuário entra com autenticação básica ou Digest, o navegador envia automaticamente as credenciais até que a sessão&dagger; termina.</span><span class="sxs-lookup"><span data-stu-id="7c46d-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="7c46d-137">&dagger;Nesse contexto, *sessão* refere-se à sessão do lado do cliente durante o qual o usuário é autenticado.</span><span class="sxs-lookup"><span data-stu-id="7c46d-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="7c46d-138">É não relacionados para sessões do lado do servidor ou [Middleware de sessão do ASP.NET Core](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="7c46d-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="7c46d-139">Os usuários podem proteger contra vulnerabilidades CSRF tomando precauções:</span><span class="sxs-lookup"><span data-stu-id="7c46d-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="7c46d-140">Inscreva-se de aplicativos da web quando terminar de usá-los.</span><span class="sxs-lookup"><span data-stu-id="7c46d-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="7c46d-141">Limpar os cookies do navegador periodicamente.</span><span class="sxs-lookup"><span data-stu-id="7c46d-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="7c46d-142">No entanto, as vulnerabilidades CSRF são basicamente um problema com o aplicativo web, não o usuário final.</span><span class="sxs-lookup"><span data-stu-id="7c46d-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="7c46d-143">Conceitos básicos de autenticação</span><span class="sxs-lookup"><span data-stu-id="7c46d-143">Authentication fundamentals</span></span>

<span data-ttu-id="7c46d-144">Autenticação baseada em cookie é uma forma popular de autenticação.</span><span class="sxs-lookup"><span data-stu-id="7c46d-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="7c46d-145">Sistemas de autenticação baseada em token estão crescendo popularidade, especialmente para aplicativos de página única (SPAs).</span><span class="sxs-lookup"><span data-stu-id="7c46d-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="7c46d-146">Autenticação baseada em cookie</span><span class="sxs-lookup"><span data-stu-id="7c46d-146">Cookie-based authentication</span></span>

<span data-ttu-id="7c46d-147">Quando um usuário é autenticado usando o nome de usuário e senha, eles são emitidos um token, que contém um tíquete de autenticação que pode ser usado para autenticação e autorização.</span><span class="sxs-lookup"><span data-stu-id="7c46d-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="7c46d-148">O token é armazenado como um cookie que acompanha cada solicitação de cliente.</span><span class="sxs-lookup"><span data-stu-id="7c46d-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="7c46d-149">Gerar e validar esse cookie é executada pelo Middleware de autenticação de Cookie.</span><span class="sxs-lookup"><span data-stu-id="7c46d-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="7c46d-150">O [middleware](xref:fundamentals/middleware/index) serializa um objeto de usuário em um cookie criptografado.</span><span class="sxs-lookup"><span data-stu-id="7c46d-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="7c46d-151">Em solicitações subsequentes, o middleware valida o cookie, recria a entidade de segurança e atribui a entidade de segurança de [usuário](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) propriedade [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="7c46d-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="7c46d-152">Autenticação baseada em token</span><span class="sxs-lookup"><span data-stu-id="7c46d-152">Token-based authentication</span></span>

<span data-ttu-id="7c46d-153">Quando um usuário é autenticado, eles são emitiu um token (não é um token antiforgery).</span><span class="sxs-lookup"><span data-stu-id="7c46d-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="7c46d-154">O token contém informações de usuário na forma de [declarações](/dotnet/framework/security/claims-based-identity-model) ou um token de referência que aponta o aplicativo para o estado do usuário mantido no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7c46d-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="7c46d-155">Quando um usuário tenta acessar um recurso que requer autenticação, o token é enviado para o aplicativo com um cabeçalho de autorização adicionais na forma de token de portador.</span><span class="sxs-lookup"><span data-stu-id="7c46d-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="7c46d-156">Isso torna o aplicativo sem monitoração de estado.</span><span class="sxs-lookup"><span data-stu-id="7c46d-156">This makes the app stateless.</span></span> <span data-ttu-id="7c46d-157">Em cada solicitação subsequente, o token é passado na solicitação de validação do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="7c46d-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="7c46d-158">Esse token não *criptografado*; ele tem *codificado*.</span><span class="sxs-lookup"><span data-stu-id="7c46d-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="7c46d-159">No servidor, o token é decodificado para acessar suas informações.</span><span class="sxs-lookup"><span data-stu-id="7c46d-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="7c46d-160">Para enviar o token em solicitações subsequentes, armazene o token no armazenamento local do navegador.</span><span class="sxs-lookup"><span data-stu-id="7c46d-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="7c46d-161">Não se preocupar sobre vulnerabilidade CSRF se o token é armazenado no armazenamento local do navegador.</span><span class="sxs-lookup"><span data-stu-id="7c46d-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="7c46d-162">CSRF é uma preocupação quando o token é armazenado em um cookie.</span><span class="sxs-lookup"><span data-stu-id="7c46d-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="7c46d-163">Vários aplicativos hospedados em um domínio</span><span class="sxs-lookup"><span data-stu-id="7c46d-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="7c46d-164">Ambientes de hospedagem compartilhados são vulneráveis a sequestro de sessão, logon CSRF e outros ataques.</span><span class="sxs-lookup"><span data-stu-id="7c46d-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="7c46d-165">Embora `example1.contoso.net` e `example2.contoso.net` são hosts diferentes, há uma relação de confiança implícita entre os hosts sob o `*.contoso.net` domínio.</span><span class="sxs-lookup"><span data-stu-id="7c46d-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="7c46d-166">Essa relação de confiança implícita permite que os hosts potencialmente não confiáveis afetam uns dos outros cookies (as políticas de mesma origem que governam as solicitações do AJAX não necessariamente se aplicam a cookies HTTP).</span><span class="sxs-lookup"><span data-stu-id="7c46d-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="7c46d-167">Ataques que exploram cookies confiáveis entre aplicativos hospedados no mesmo domínio podem ser evitados por compartilhamento não domínios.</span><span class="sxs-lookup"><span data-stu-id="7c46d-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="7c46d-168">Quando cada aplicativo está hospedado em seu próprio domínio, não há nenhuma relação de confiança implícita de cookie para explorar.</span><span class="sxs-lookup"><span data-stu-id="7c46d-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="7c46d-169">Configuração de ASP.NET Core antiforgery</span><span class="sxs-lookup"><span data-stu-id="7c46d-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="7c46d-170">ASP.NET Core implementa usando antiforgery [proteção de dados do ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="7c46d-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="7c46d-171">A pilha de proteção de dados deve ser configurada para funcionar em um farm de servidores.</span><span class="sxs-lookup"><span data-stu-id="7c46d-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="7c46d-172">Consulte [Configurando a proteção de dados](xref:security/data-protection/configuration/overview) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="7c46d-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="7c46d-173">No ASP.NET Core 2.0 ou posterior, o [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injeta antiforgery tokens em elementos de formulário HTML.</span><span class="sxs-lookup"><span data-stu-id="7c46d-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="7c46d-174">A seguinte marcação em um arquivo Razor gera automaticamente tokens antiforgery:</span><span class="sxs-lookup"><span data-stu-id="7c46d-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="7c46d-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) gera tokens antiforgery por padrão, se o método do formulário não é GET.</span><span class="sxs-lookup"><span data-stu-id="7c46d-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="7c46d-176">A geração automática de tokens antiforgery para elementos de formulário HTML acontece quando o `<form>` marca contém o `method="post"` atributo e qualquer uma das opções a seguir forem verdadeira:</span><span class="sxs-lookup"><span data-stu-id="7c46d-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="7c46d-177">O atributo de ação está vazio (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="7c46d-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="7c46d-178">O atributo de ação não for fornecido (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="7c46d-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="7c46d-179">Geração automática de tokens antiforgery para elementos de formulário HTML pode ser desabilitada:</span><span class="sxs-lookup"><span data-stu-id="7c46d-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="7c46d-180">Desabilitar explicitamente tokens antiforgery com o `asp-antiforgery` atributo:</span><span class="sxs-lookup"><span data-stu-id="7c46d-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="7c46d-181">O elemento de formulário é desativado-out de auxiliares de marcação usando o auxiliar de marca [! recusar símbolo](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="7c46d-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="7c46d-182">Remover o `FormTagHelper` do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="7c46d-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="7c46d-183">O `FormTagHelper` podem ser removidas de um modo de exibição, adicionando a seguinte diretiva para o modo de exibição do Razor:</span><span class="sxs-lookup"><span data-stu-id="7c46d-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="7c46d-184">[Páginas Razor](xref:mvc/razor-pages/index) são protegidos automaticamente contra XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="7c46d-184">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="7c46d-185">Para obter mais informações, consulte [XSRF/CSRF e páginas Razor](xref:mvc/razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="7c46d-185">For more information, see [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf).</span></span>

<span data-ttu-id="7c46d-186">A abordagem mais comum para proteger contra ataques CSRF é usar o *padrão de Token sincronizador* (STP).</span><span class="sxs-lookup"><span data-stu-id="7c46d-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="7c46d-187">STP é usado quando o usuário solicita uma página de dados do formulário:</span><span class="sxs-lookup"><span data-stu-id="7c46d-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="7c46d-188">O servidor envia um token associado com a identidade do usuário atual para o cliente.</span><span class="sxs-lookup"><span data-stu-id="7c46d-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="7c46d-189">O cliente envia o token para o servidor para verificação.</span><span class="sxs-lookup"><span data-stu-id="7c46d-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="7c46d-190">Se o servidor recebe um token que não corresponde a identidade do usuário autenticado, a solicitação será rejeitada.</span><span class="sxs-lookup"><span data-stu-id="7c46d-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="7c46d-191">O token é exclusivo e imprevisível.</span><span class="sxs-lookup"><span data-stu-id="7c46d-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="7c46d-192">O token também pode ser usado para garantir o sequenciamento adequado de uma série de solicitações (por exemplo, garantindo a sequência de solicitação de: a página 1 &ndash; página 2 &ndash; página 3).</span><span class="sxs-lookup"><span data-stu-id="7c46d-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="7c46d-193">Todos os formulários em modelos do MVC do ASP.NET Core e páginas Razor geram tokens antiforgery.</span><span class="sxs-lookup"><span data-stu-id="7c46d-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="7c46d-194">O seguinte par de exemplos de exibição gera tokens antiforgery:</span><span class="sxs-lookup"><span data-stu-id="7c46d-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="7c46d-195">Adicionar explicitamente um token antiforgery para um `<form>` elemento sem o uso de auxiliares de marcação com o auxiliar HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="7c46d-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="7c46d-196">Em cada um dos casos anteriores, o ASP.NET Core adiciona um campo de formulário oculto semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="7c46d-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="7c46d-197">ASP.NET Core inclui três [filtros](xref:mvc/controllers/filters) para trabalhar com tokens antiforgery:</span><span class="sxs-lookup"><span data-stu-id="7c46d-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="7c46d-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="7c46d-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="7c46d-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="7c46d-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="7c46d-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="7c46d-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="7c46d-201">Opções de antiforgery</span><span class="sxs-lookup"><span data-stu-id="7c46d-201">Antiforgery options</span></span>

<span data-ttu-id="7c46d-202">Personalizar [opções antiforgery](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) em `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7c46d-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="7c46d-203">Opção</span><span class="sxs-lookup"><span data-stu-id="7c46d-203">Option</span></span> | <span data-ttu-id="7c46d-204">Descrição</span><span class="sxs-lookup"><span data-stu-id="7c46d-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="7c46d-205">Cookie</span><span class="sxs-lookup"><span data-stu-id="7c46d-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="7c46d-206">Determina as configurações usadas para criar o cookie antiforgery.</span><span class="sxs-lookup"><span data-stu-id="7c46d-206">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="7c46d-207">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="7c46d-207">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="7c46d-208">O domínio do cookie.</span><span class="sxs-lookup"><span data-stu-id="7c46d-208">The domain of the cookie.</span></span> <span data-ttu-id="7c46d-209">Assume o padrão de `null`.</span><span class="sxs-lookup"><span data-stu-id="7c46d-209">Defaults to `null`.</span></span> <span data-ttu-id="7c46d-210">Essa propriedade está obsoleta e será removida em uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="7c46d-210">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="7c46d-211">A alternativa recomendada é Cookie.Domain.</span><span class="sxs-lookup"><span data-stu-id="7c46d-211">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="7c46d-212">CookieName</span><span class="sxs-lookup"><span data-stu-id="7c46d-212">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="7c46d-213">O nome do cookie.</span><span class="sxs-lookup"><span data-stu-id="7c46d-213">The name of the cookie.</span></span> <span data-ttu-id="7c46d-214">Se não estiver definida, o sistema gera um nome exclusivo que começa com o [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="7c46d-214">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="7c46d-215">Essa propriedade está obsoleta e será removida em uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="7c46d-215">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="7c46d-216">A alternativa recomendada é Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="7c46d-216">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="7c46d-217">CookiePath</span><span class="sxs-lookup"><span data-stu-id="7c46d-217">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="7c46d-218">O caminho definido no cookie.</span><span class="sxs-lookup"><span data-stu-id="7c46d-218">The path set on the cookie.</span></span> <span data-ttu-id="7c46d-219">Essa propriedade está obsoleta e será removida em uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="7c46d-219">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="7c46d-220">A alternativa recomendada é Cookie.Path.</span><span class="sxs-lookup"><span data-stu-id="7c46d-220">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="7c46d-221">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="7c46d-221">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="7c46d-222">O nome do campo de formulário oculto usado pelo sistema antiforgery para processar tokens antiforgery nos modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="7c46d-222">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="7c46d-223">HeaderName</span><span class="sxs-lookup"><span data-stu-id="7c46d-223">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="7c46d-224">O nome do cabeçalho usado pelo sistema antiforgery.</span><span class="sxs-lookup"><span data-stu-id="7c46d-224">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="7c46d-225">Se `null`, o sistema considerará apenas os dados de formulário.</span><span class="sxs-lookup"><span data-stu-id="7c46d-225">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="7c46d-226">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="7c46d-226">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="7c46d-227">Especifica se o SSL é exigido pelo sistema antiforgery.</span><span class="sxs-lookup"><span data-stu-id="7c46d-227">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="7c46d-228">Se `true`, falham de solicitações de não SSL.</span><span class="sxs-lookup"><span data-stu-id="7c46d-228">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="7c46d-229">Assume o padrão de `false`.</span><span class="sxs-lookup"><span data-stu-id="7c46d-229">Defaults to `false`.</span></span> <span data-ttu-id="7c46d-230">Essa propriedade está obsoleta e será removida em uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="7c46d-230">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="7c46d-231">A alternativa recomendada é definir Cookie.SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="7c46d-231">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="7c46d-232">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="7c46d-232">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="7c46d-233">Especifica se é suprimir a geração do `X-Frame-Options` cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="7c46d-233">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="7c46d-234">Por padrão, o cabeçalho é gerado com um valor de "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="7c46d-234">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="7c46d-235">Assume o padrão de `false`.</span><span class="sxs-lookup"><span data-stu-id="7c46d-235">Defaults to `false`.</span></span> |

<span data-ttu-id="7c46d-236">Para obter mais informações, consulte [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="7c46d-236">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="7c46d-237">Configurar recursos antiforgery com IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="7c46d-237">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="7c46d-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) fornece a API para configurar recursos antiforgery.</span><span class="sxs-lookup"><span data-stu-id="7c46d-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="7c46d-239">`IAntiforgery` pode ser solicitados no `Configure` método o `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="7c46d-239">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="7c46d-240">O exemplo a seguir usa o middleware da home page do aplicativo para gerar um token antiforgery e enviá-lo na resposta como um cookie (usando a convenção de nomenclatura Angular da padrão descrita mais adiante neste tópico):</span><span class="sxs-lookup"><span data-stu-id="7c46d-240">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="7c46d-241">Exigir validação antiforgery</span><span class="sxs-lookup"><span data-stu-id="7c46d-241">Require antiforgery validation</span></span>

<span data-ttu-id="7c46d-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) é um filtro de ação que pode ser aplicado a uma ação individual, um controlador ou globalmente.</span><span class="sxs-lookup"><span data-stu-id="7c46d-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="7c46d-243">Solicitações feitas para ações com esse filtro aplicado são bloqueadas, a menos que a solicitação inclui um token antiforgery válido.</span><span class="sxs-lookup"><span data-stu-id="7c46d-243">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="7c46d-244">O `ValidateAntiForgeryToken` atributo requer um token para solicitações para os métodos de ação decora, incluindo solicitações HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="7c46d-244">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="7c46d-245">Se o `ValidateAntiForgeryToken` atributo é aplicado em controladores do aplicativo, ele pode ser substituído com o `IgnoreAntiforgeryToken` atributo.</span><span class="sxs-lookup"><span data-stu-id="7c46d-245">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="7c46d-246">ASP.NET Core não oferece suporte à inclusão automaticamente tokens antiforgery solicitações GET.</span><span class="sxs-lookup"><span data-stu-id="7c46d-246">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="7c46d-247">Validar automaticamente tokens antiforgery para métodos HTTP não seguros somente</span><span class="sxs-lookup"><span data-stu-id="7c46d-247">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="7c46d-248">Aplicativos ASP.NET Core não geram tokens de antiforgery para métodos HTTP seguros (GET, HEAD, opções e rastreamento).</span><span class="sxs-lookup"><span data-stu-id="7c46d-248">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="7c46d-249">Em vez de em larga escala aplicar o `ValidateAntiForgeryToken` atributo e, em seguida, substituindo-o com `IgnoreAntiforgeryToken` atributos, o [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) atributo pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="7c46d-249">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="7c46d-250">Esse atributo funciona de forma idêntica ao `ValidateAntiForgeryToken` de atributos, exceto pelo fato de que não exige tokens para solicitações feitas usando os seguintes métodos HTTP:</span><span class="sxs-lookup"><span data-stu-id="7c46d-250">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="7c46d-251">OBTER</span><span class="sxs-lookup"><span data-stu-id="7c46d-251">GET</span></span>
* <span data-ttu-id="7c46d-252">HOME</span><span class="sxs-lookup"><span data-stu-id="7c46d-252">HEAD</span></span>
* <span data-ttu-id="7c46d-253">OPÇÕES</span><span class="sxs-lookup"><span data-stu-id="7c46d-253">OPTIONS</span></span>
* <span data-ttu-id="7c46d-254">TRACE</span><span class="sxs-lookup"><span data-stu-id="7c46d-254">TRACE</span></span>

<span data-ttu-id="7c46d-255">Recomendamos o uso de `AutoValidateAntiforgeryToken` em larga escala para cenários de não-API.</span><span class="sxs-lookup"><span data-stu-id="7c46d-255">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="7c46d-256">Isso garante que as ações de POSTAGEM são protegidas por padrão.</span><span class="sxs-lookup"><span data-stu-id="7c46d-256">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="7c46d-257">A alternativa é ignorar tokens antiforgery por padrão, a menos que `ValidateAntiForgeryToken` é aplicado a métodos de ação individual.</span><span class="sxs-lookup"><span data-stu-id="7c46d-257">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="7c46d-258">Ele provavelmente neste cenário para um método de ação de POSTAGEM deve ser desprotegido por engano, deixando o aplicativo vulnerável a ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="7c46d-258">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="7c46d-259">Todas as postagens devem enviar o token antiforgery.</span><span class="sxs-lookup"><span data-stu-id="7c46d-259">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="7c46d-260">APIs não tem um mecanismo automático para enviar a parte do cookie não do token.</span><span class="sxs-lookup"><span data-stu-id="7c46d-260">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="7c46d-261">A implementação provavelmente depende a implementação de código do cliente.</span><span class="sxs-lookup"><span data-stu-id="7c46d-261">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="7c46d-262">Alguns exemplos são mostrados abaixo:</span><span class="sxs-lookup"><span data-stu-id="7c46d-262">Some examples are shown below:</span></span>

<span data-ttu-id="7c46d-263">Exemplo de nível de classe:</span><span class="sxs-lookup"><span data-stu-id="7c46d-263">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="7c46d-264">Exemplo global:</span><span class="sxs-lookup"><span data-stu-id="7c46d-264">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="7c46d-265">Substituição global ou atributos antiforgery controlador</span><span class="sxs-lookup"><span data-stu-id="7c46d-265">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="7c46d-266">O [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtro é usado para eliminar a necessidade de um token de antiforgery para uma determinada ação (ou controlador).</span><span class="sxs-lookup"><span data-stu-id="7c46d-266">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="7c46d-267">Quando aplicado, esse filtro substitui `ValidateAntiForgeryToken` e `AutoValidateAntiforgeryToken` filtros especificados em um nível mais alto (global ou em um controlador).</span><span class="sxs-lookup"><span data-stu-id="7c46d-267">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="7c46d-268">Tokens de atualização após a autenticação</span><span class="sxs-lookup"><span data-stu-id="7c46d-268">Refresh tokens after authentication</span></span>

<span data-ttu-id="7c46d-269">Tokens devem ser atualizados depois que o usuário é autenticado ao redirecionar o usuário para uma exibição ou página das páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="7c46d-269">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="7c46d-270">JavaScript e AJAX SPAs</span><span class="sxs-lookup"><span data-stu-id="7c46d-270">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="7c46d-271">Em aplicativos tradicionais baseados em HTML, antiforgery tokens são passados para o servidor usando campos ocultos do formulário.</span><span class="sxs-lookup"><span data-stu-id="7c46d-271">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="7c46d-272">Em aplicativos modernos baseados em JavaScript e SPAs, muitas solicitações são feitas por meio de programação.</span><span class="sxs-lookup"><span data-stu-id="7c46d-272">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="7c46d-273">Essas solicitações do AJAX podem usar outras técnicas (como cabeçalhos de solicitação ou cookies) para enviar o token.</span><span class="sxs-lookup"><span data-stu-id="7c46d-273">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="7c46d-274">Se os cookies são usados para armazenar os tokens de autenticação e para autenticar solicitações de API no servidor, CSRF é um problema potencial.</span><span class="sxs-lookup"><span data-stu-id="7c46d-274">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="7c46d-275">Se o armazenamento local é usado para armazenar o token, vulnerabilidade CSRF pode ser reduzida porque os valores do armazenamento local não são enviados automaticamente para o servidor com cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="7c46d-275">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="7c46d-276">Portanto, usando o armazenamento local para armazenar o token antiforgery no cliente e enviar o token como um cabeçalho de solicitação é uma abordagem recomendada.</span><span class="sxs-lookup"><span data-stu-id="7c46d-276">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="7c46d-277">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7c46d-277">JavaScript</span></span>

<span data-ttu-id="7c46d-278">Usando JavaScript com modos de exibição, o token pode ser criado usando um serviço a partir do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="7c46d-278">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="7c46d-279">Injetar o [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) serviço no modo de exibição e chamada [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="7c46d-279">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="7c46d-280">Essa abordagem elimina a necessidade de lidar diretamente com configuração cookies do servidor ou lê-los do cliente.</span><span class="sxs-lookup"><span data-stu-id="7c46d-280">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="7c46d-281">O exemplo anterior usa JavaScript para ler o valor do campo oculto para o cabeçalho de POSTAGEM de AJAX.</span><span class="sxs-lookup"><span data-stu-id="7c46d-281">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="7c46d-282">JavaScript também pode acessar tokens em cookies e usar o conteúdo do cookie para criar um cabeçalho com o valor do token.</span><span class="sxs-lookup"><span data-stu-id="7c46d-282">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="7c46d-283">Supondo que o script solicitações para enviar o token em um cabeçalho chamado `X-CSRF-TOKEN`, configure o serviço antiforgery para procurar o `X-CSRF-TOKEN` cabeçalho:</span><span class="sxs-lookup"><span data-stu-id="7c46d-283">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="7c46d-284">O exemplo a seguir usa JavaScript para fazer uma solicitação AJAX com o cabeçalho apropriado:</span><span class="sxs-lookup"><span data-stu-id="7c46d-284">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="7c46d-285">AngularJS</span><span class="sxs-lookup"><span data-stu-id="7c46d-285">AngularJS</span></span>

<span data-ttu-id="7c46d-286">AngularJS usa uma convenção para endereço CSRF.</span><span class="sxs-lookup"><span data-stu-id="7c46d-286">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="7c46d-287">Se o servidor envia um cookie com o nome `XSRF-TOKEN`, o AngularJS `$http` serviço adiciona o valor do cookie para um cabeçalho quando ele envia uma solicitação ao servidor.</span><span class="sxs-lookup"><span data-stu-id="7c46d-287">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="7c46d-288">Esse processo é automático.</span><span class="sxs-lookup"><span data-stu-id="7c46d-288">This process is automatic.</span></span> <span data-ttu-id="7c46d-289">O cabeçalho não precisa ser definidos explicitamente.</span><span class="sxs-lookup"><span data-stu-id="7c46d-289">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="7c46d-290">O nome do cabeçalho é `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="7c46d-290">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="7c46d-291">O servidor deve detectar esse cabeçalho e validar seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="7c46d-291">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="7c46d-292">Para o ASP.NET Core API trabalhe com essa convenção:</span><span class="sxs-lookup"><span data-stu-id="7c46d-292">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="7c46d-293">Configure seu aplicativo para fornecer um token em um cookie chamado `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="7c46d-293">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="7c46d-294">Configurar o serviço antiforgery para procurar um cabeçalho chamado `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="7c46d-294">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="7c46d-295">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7c46d-295">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="7c46d-296">Estender antiforgery</span><span class="sxs-lookup"><span data-stu-id="7c46d-296">Extend antiforgery</span></span>

<span data-ttu-id="7c46d-297">O [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo permite aos desenvolvedores estender o comportamento do sistema anti-CSRF por dados adicionais do ciclo em cada token.</span><span class="sxs-lookup"><span data-stu-id="7c46d-297">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="7c46d-298">O [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) método é chamado sempre que um token de campo é gerado e o valor de retorno é inserido no token gerado.</span><span class="sxs-lookup"><span data-stu-id="7c46d-298">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="7c46d-299">Um implementador poderia retornar um carimbo de hora, um valor de uso único ou qualquer outro valor e, em seguida, chamar [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) para validar dados quando o token é validado.</span><span class="sxs-lookup"><span data-stu-id="7c46d-299">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="7c46d-300">Nome de usuário do cliente já é inserido nos tokens gerados, portanto, não há necessidade de incluir essas informações.</span><span class="sxs-lookup"><span data-stu-id="7c46d-300">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="7c46d-301">Se um token inclui dados complementares, mas não `IAntiForgeryAdditionalDataProvider` é configurado, os dados complementares não são validados.</span><span class="sxs-lookup"><span data-stu-id="7c46d-301">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c46d-302">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7c46d-302">Additional resources</span></span>

* <span data-ttu-id="7c46d-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [Abrir projeto de segurança de aplicativo da Web](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="7c46d-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
