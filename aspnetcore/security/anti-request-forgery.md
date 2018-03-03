---
title: "Ataques de evitar intersite solicitar CSRF (falsificação XSRF /) no núcleo do ASP.NET"
author: steve-smith
description: "Saiba como evitar ataques contra aplicativos web em que um site mal-intencionado pode influenciar a interação entre o aplicativo e um navegador cliente."
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 80651a3c3e4c722e0cb96d7cc07de366819f8d1d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="b6807-103">Ataques de evitar intersite solicitar CSRF (falsificação XSRF /) no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b6807-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="b6807-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b6807-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="b6807-105">O ataque de falsificação contra impedir que?</span><span class="sxs-lookup"><span data-stu-id="b6807-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="b6807-106">Falsificação de solicitação entre sites (também conhecido como XSRF ou CSRF, pronunciado *consulte navegação*) é um ataque contra aplicativos web hospedados no qual um site mal-intencionado pode influenciar a interação entre um navegador de cliente e um site da web que relações de confiança esse navegador.</span><span class="sxs-lookup"><span data-stu-id="b6807-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="b6807-107">Esses ataques são possibilitados como navegadores da web enviam alguns tipos de tokens de autenticação automaticamente com todas as solicitações para um site da web.</span><span class="sxs-lookup"><span data-stu-id="b6807-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="b6807-108">Essa forma de exploração do também conhecido como um *ataque de um clique* ou como *sessão riding*, pois aproveita o ataque do usuário do anteriormente autenticados sessão.</span><span class="sxs-lookup"><span data-stu-id="b6807-108">This form of exploit's also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="b6807-109">Um exemplo de um ataque CSRF:</span><span class="sxs-lookup"><span data-stu-id="b6807-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="b6807-110">Um usuário faz logon em `www.example.com`, usando a autenticação de formulários.</span><span class="sxs-lookup"><span data-stu-id="b6807-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="b6807-111">O servidor autentica o usuário e envia uma resposta que inclui um cookie de autenticação.</span><span class="sxs-lookup"><span data-stu-id="b6807-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="b6807-112">O usuário acessa um site mal-intencionado.</span><span class="sxs-lookup"><span data-stu-id="b6807-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="b6807-113">O site mal-intencionado contém um formulário HTML, semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="b6807-113">The malicious site contains an HTML form similar to the following:</span></span>

   ```html
   <h1>You Are a Winner!</h1>
   <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click Me">
   </form>
   ```

<span data-ttu-id="b6807-114">Observe que a ação de formulário envia para o site vulnerável, não para o site mal-intencionado.</span><span class="sxs-lookup"><span data-stu-id="b6807-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="b6807-115">Esta é a parte de "sites" de CSRF.</span><span class="sxs-lookup"><span data-stu-id="b6807-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="b6807-116">O usuário clica no botão Enviar.</span><span class="sxs-lookup"><span data-stu-id="b6807-116">The user clicks the submit button.</span></span> <span data-ttu-id="b6807-117">O navegador inclui automaticamente o cookie de autenticação para o domínio solicitado (o site vulnerável neste caso) com a solicitação.</span><span class="sxs-lookup"><span data-stu-id="b6807-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="b6807-118">A solicitação é executado no servidor com o contexto de autenticação do usuário e pode executar qualquer ação que um usuário autenticado pode fazer.</span><span class="sxs-lookup"><span data-stu-id="b6807-118">The request runs on the server with the user's authentication context and can perform any action that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="b6807-119">Este exemplo requer que o usuário clicar no botão do formulário.</span><span class="sxs-lookup"><span data-stu-id="b6807-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="b6807-120">A página mal-intencionado pode:</span><span class="sxs-lookup"><span data-stu-id="b6807-120">The malicious page could:</span></span>

* <span data-ttu-id="b6807-121">Execute um script que envia automaticamente o formulário.</span><span class="sxs-lookup"><span data-stu-id="b6807-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="b6807-122">Envia o envio de um formulário como uma solicitação AJAX.</span><span class="sxs-lookup"><span data-stu-id="b6807-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="b6807-123">Use um formulário oculto com o CSS.</span><span class="sxs-lookup"><span data-stu-id="b6807-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="b6807-124">Usando o SSL não impede que um ataque CSRF, o site mal-intencionado pode enviar um `https://` solicitação.</span><span class="sxs-lookup"><span data-stu-id="b6807-124">Using SSL doesn't prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="b6807-125">Pontos de extremidade de site que respondem a de destino a alguns ataques `GET` solicitações, nesse caso uma marca de imagem pode ser usado para executar a ação (essa forma de ataque comuns em sites de Fórum que permitem imagens mas bloqueiam JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b6807-125">Some attacks target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="b6807-126">Aplicativos que a alteração de estado `GET` solicitações são vulneráveis contra ataques mal-intencionados.</span><span class="sxs-lookup"><span data-stu-id="b6807-126">Applications that change state with `GET` requests are vulnerable from malicious attacks.</span></span>

<span data-ttu-id="b6807-127">Ataques CSRF são possíveis nos sites da web que usam cookies para autenticação, como navegadores enviam todos os cookies relevantes para o site de destino.</span><span class="sxs-lookup"><span data-stu-id="b6807-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="b6807-128">No entanto, ataques CSRF não estão limitados a exploração de cookies.</span><span class="sxs-lookup"><span data-stu-id="b6807-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="b6807-129">Por exemplo, a autenticação básica e resumida também são vulneráveis.</span><span class="sxs-lookup"><span data-stu-id="b6807-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="b6807-130">Depois que um usuário faz logon com a autenticação básica ou Digest, o navegador envia automaticamente as credenciais, até que a sessão termina.</span><span class="sxs-lookup"><span data-stu-id="b6807-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="b6807-131">Observação: nesse contexto, *sessão* refere-se à sessão do lado do cliente durante o qual o usuário é autenticado.</span><span class="sxs-lookup"><span data-stu-id="b6807-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="b6807-132">É não relacionados para sessões do lado do servidor ou [sessão middleware](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="b6807-132">It's unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="b6807-133">Os usuários podem proteger contra vulnerabilidades CSRF por:</span><span class="sxs-lookup"><span data-stu-id="b6807-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="b6807-134">Log de sites da web quando ele tiver terminado de usá-los.</span><span class="sxs-lookup"><span data-stu-id="b6807-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="b6807-135">Limpar periodicamente os cookies do seu navegador.</span><span class="sxs-lookup"><span data-stu-id="b6807-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="b6807-136">No entanto, as vulnerabilidades CSRF são basicamente um problema com o aplicativo web, não o usuário final.</span><span class="sxs-lookup"><span data-stu-id="b6807-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="b6807-137">Como o ASP.NET MVC de núcleo endereço CSRF?</span><span class="sxs-lookup"><span data-stu-id="b6807-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="b6807-138">ASP.NET Core implementa falsificação-solicitação usando o [pilha de proteção de dados do ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="b6807-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="b6807-139">Proteção de dados do ASP.NET Core deve ser configurada para funcionar em um farm de servidores.</span><span class="sxs-lookup"><span data-stu-id="b6807-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="b6807-140">Consulte [Configurando a proteção de dados](xref:security/data-protection/configuration/overview) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="b6807-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="b6807-141">Configuração de proteção de dados do ASP.NET Core falsificação-solicitação padrão</span><span class="sxs-lookup"><span data-stu-id="b6807-141">ASP.NET Core anti-request-forgery default data protection configuration</span></span> 

<span data-ttu-id="b6807-142">No núcleo do ASP.NET MVC 2.0 a [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injeta tokens antifalsificação para elementos de formulário HTML.</span><span class="sxs-lookup"><span data-stu-id="b6807-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="b6807-143">Por exemplo, a seguinte marcação em um arquivo Razor gerará automaticamente tokens antifalsificação:</span><span class="sxs-lookup"><span data-stu-id="b6807-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="b6807-144">A geração automática de tokens antifalsificação para elementos de formulário HTML acontece quando:</span><span class="sxs-lookup"><span data-stu-id="b6807-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="b6807-145">O `form` marca contém o `method="post"` atributo AND</span><span class="sxs-lookup"><span data-stu-id="b6807-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="b6807-146">O atributo de ação está vazio.</span><span class="sxs-lookup"><span data-stu-id="b6807-146">The action attribute is empty.</span></span> <span data-ttu-id="b6807-147">( `action=""`) OU</span><span class="sxs-lookup"><span data-stu-id="b6807-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="b6807-148">O atributo de ação não for fornecido.</span><span class="sxs-lookup"><span data-stu-id="b6807-148">The action attribute isn't supplied.</span></span> <span data-ttu-id="b6807-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="b6807-149">(`<form method="post">`)</span></span>

<span data-ttu-id="b6807-150">Você pode desabilitar a geração automática de tokens antifalsificação para elementos de formulário HTML por:</span><span class="sxs-lookup"><span data-stu-id="b6807-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="b6807-151">Desabilitando explicitamente `asp-antiforgery`.</span><span class="sxs-lookup"><span data-stu-id="b6807-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="b6807-152">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="b6807-152">For example</span></span>

  ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="b6807-153">Aceitar o elemento de formulário fora de auxiliares de marcação usando o auxiliar de marca [! recusar símbolo](xref:mvc/views/tag-helpers/intro#opt-out).</span><span class="sxs-lookup"><span data-stu-id="b6807-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

  ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="b6807-154">Remover o `FormTagHelper` do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="b6807-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="b6807-155">Você pode remover o `FormTagHelper` de um modo de exibição, adicionando a seguinte diretiva para o modo de exibição do Razor:</span><span class="sxs-lookup"><span data-stu-id="b6807-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

  ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="b6807-156">[Páginas Razor](xref:mvc/razor-pages/index) são protegidos automaticamente contra XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="b6807-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="b6807-157">Você não precisa escrever nenhum código adicional.</span><span class="sxs-lookup"><span data-stu-id="b6807-157">You don't have to write any additional code.</span></span> <span data-ttu-id="b6807-158">Consulte [XSRF/CSRF e páginas Razor](xref:mvc/razor-pages/index#xsrf) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="b6807-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="b6807-159">A abordagem mais comum para proteger contra ataques CSRF é o padrão de token sincronizador (STP).</span><span class="sxs-lookup"><span data-stu-id="b6807-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="b6807-160">STP é uma técnica usada quando o usuário solicita uma página de dados do formulário.</span><span class="sxs-lookup"><span data-stu-id="b6807-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="b6807-161">O servidor envia um token associado com a identidade do usuário atual para o cliente.</span><span class="sxs-lookup"><span data-stu-id="b6807-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="b6807-162">O cliente envia o token para o servidor para verificação.</span><span class="sxs-lookup"><span data-stu-id="b6807-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="b6807-163">Se o servidor recebe um token que não corresponde a identidade do usuário autenticado, a solicitação será rejeitada.</span><span class="sxs-lookup"><span data-stu-id="b6807-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="b6807-164">O token é exclusivo e imprevisível.</span><span class="sxs-lookup"><span data-stu-id="b6807-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="b6807-165">O token também pode ser usado para garantir o sequenciamento adequado de uma série de solicitações (página garantia 1 precede a página 2 que precede a página 3).</span><span class="sxs-lookup"><span data-stu-id="b6807-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="b6807-166">Todos os formulários em modelos do MVC do ASP.NET Core geram tokens antiforgery.</span><span class="sxs-lookup"><span data-stu-id="b6807-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="b6807-167">Os exemplos a seguir da lógica de exibição geram tokens antiforgery:</span><span class="sxs-lookup"><span data-stu-id="b6807-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="b6807-168">Você pode adicionar explicitamente um token antiforgery para um `<form>` elemento sem o uso de auxiliares de marcação com o auxiliar HTML `@Html.AntiForgeryToken`:</span><span class="sxs-lookup"><span data-stu-id="b6807-168">You can explicitly add an antiforgery token to a `<form>` element without using tag helpers with the HTML helper `@Html.AntiForgeryToken`:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="b6807-169">Em cada um dos casos anteriores, o ASP.NET Core adicionará um campo de formulário oculto semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="b6807-169">In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:</span></span>
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw">
```

<span data-ttu-id="b6807-170">ASP.NET Core inclui três [filtros](xref:mvc/controllers/filters) para trabalhar com tokens antiforgery: `ValidateAntiForgeryToken`, `AutoValidateAntiforgeryToken`, e `IgnoreAntiforgeryToken`.</span><span class="sxs-lookup"><span data-stu-id="b6807-170">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: `ValidateAntiForgeryToken`, `AutoValidateAntiforgeryToken`, and `IgnoreAntiforgeryToken`.</span></span>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="b6807-171">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="b6807-171">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="b6807-172">O `ValidateAntiForgeryToken` é um filtro de ação que pode ser aplicado a uma ação individual, um controlador ou globalmente.</span><span class="sxs-lookup"><span data-stu-id="b6807-172">The `ValidateAntiForgeryToken` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="b6807-173">Solicitações feitas para ações com esse filtro aplicado serão bloqueadas, a menos que a solicitação inclui um token antiforgery válido.</span><span class="sxs-lookup"><span data-stu-id="b6807-173">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="b6807-174">O `ValidateAntiForgeryToken` atributo requer um token para solicitações para os métodos de ação decora, incluindo `HTTP GET` solicitações.</span><span class="sxs-lookup"><span data-stu-id="b6807-174">The `ValidateAntiForgeryToken` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="b6807-175">Se você aplicá-lo em larga escala, você pode substituí-la com o `IgnoreAntiforgeryToken` atributo.</span><span class="sxs-lookup"><span data-stu-id="b6807-175">If you apply it broadly, you can override it with the `IgnoreAntiforgeryToken` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="b6807-176">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="b6807-176">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="b6807-177">Aplicativos ASP.NET Core geralmente não geram tokens de antiforgery para métodos HTTP seguros (GET, HEAD, opções e rastreamento).</span><span class="sxs-lookup"><span data-stu-id="b6807-177">ASP.NET Core apps generally don't generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="b6807-178">Em vez de em larga escala aplicar o `ValidateAntiForgeryToken` atributo e, em seguida, substituindo-o com `IgnoreAntiforgeryToken` atributos, você pode usar o ``AutoValidateAntiforgeryToken`` atributo.</span><span class="sxs-lookup"><span data-stu-id="b6807-178">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="b6807-179">Esse atributo funciona de forma idêntica ao `ValidateAntiForgeryToken` de atributos, exceto pelo fato de que não exige tokens para solicitações feitas usando os seguintes métodos HTTP:</span><span class="sxs-lookup"><span data-stu-id="b6807-179">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="b6807-180">OBTER</span><span class="sxs-lookup"><span data-stu-id="b6807-180">GET</span></span>
* <span data-ttu-id="b6807-181">HOME</span><span class="sxs-lookup"><span data-stu-id="b6807-181">HEAD</span></span>
* <span data-ttu-id="b6807-182">OPÇÕES</span><span class="sxs-lookup"><span data-stu-id="b6807-182">OPTIONS</span></span>
* <span data-ttu-id="b6807-183">TRACE</span><span class="sxs-lookup"><span data-stu-id="b6807-183">TRACE</span></span>

<span data-ttu-id="b6807-184">Recomendamos que você use `AutoValidateAntiforgeryToken` em larga escala para cenários de não-API.</span><span class="sxs-lookup"><span data-stu-id="b6807-184">We recommend you use `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="b6807-185">Isso garante que as ações de POSTAGEM são protegidas por padrão.</span><span class="sxs-lookup"><span data-stu-id="b6807-185">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="b6807-186">A alternativa é ignorar tokens antiforgery por padrão, a menos que `ValidateAntiForgeryToken` é aplicado ao método de ação individuais.</span><span class="sxs-lookup"><span data-stu-id="b6807-186">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to the individual action method.</span></span> <span data-ttu-id="b6807-187">É mais provável que neste cenário para um método de ação de POST ser esquerda desprotegida, deixando o seu aplicativo vulnerável a ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="b6807-187">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="b6807-188">POSTAGENS mesmo anônimas devem enviar o token antiforgery.</span><span class="sxs-lookup"><span data-stu-id="b6807-188">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="b6807-189">Observação: APIs não têm um mecanismo automático para enviar a parte do cookie não do token; sua implementação provavelmente depende de sua implementação de código do cliente.</span><span class="sxs-lookup"><span data-stu-id="b6807-189">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="b6807-190">Alguns exemplos são mostrados abaixo.</span><span class="sxs-lookup"><span data-stu-id="b6807-190">Some examples are shown below.</span></span>

<span data-ttu-id="b6807-191">Exemplo (nível de classe):</span><span class="sxs-lookup"><span data-stu-id="b6807-191">Example (class level):</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="b6807-192">Exemplo (global):</span><span class="sxs-lookup"><span data-stu-id="b6807-192">Example (global):</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="b6807-193">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="b6807-193">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="b6807-194">O `IgnoreAntiforgeryToken` filtro é usado para eliminar a necessidade de um token antiforgery estar presente para uma determinada ação (ou controlador).</span><span class="sxs-lookup"><span data-stu-id="b6807-194">The `IgnoreAntiforgeryToken` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="b6807-195">Quando aplicado, substituirá este filtro `ValidateAntiForgeryToken` e/ou `AutoValidateAntiforgeryToken` filtros especificados em um nível mais alto (global ou em um controlador).</span><span class="sxs-lookup"><span data-stu-id="b6807-195">When applied, this filter will override `ValidateAntiForgeryToken` and/or `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="b6807-196">JavaScript e AJAX SPAs</span><span class="sxs-lookup"><span data-stu-id="b6807-196">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="b6807-197">Em aplicativos tradicionais baseados em HTML, antiforgery tokens são passados para o servidor usando campos ocultos do formulário.</span><span class="sxs-lookup"><span data-stu-id="b6807-197">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="b6807-198">Em aplicativos modernos baseados em JavaScript e aplicativos de única página (SPAs), muitas solicitações são feitas por meio de programação.</span><span class="sxs-lookup"><span data-stu-id="b6807-198">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="b6807-199">Essas solicitações do AJAX podem usar outras técnicas (como cabeçalhos de solicitação ou cookies) para enviar o token.</span><span class="sxs-lookup"><span data-stu-id="b6807-199">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="b6807-200">Se os cookies são usados para armazenar os tokens de autenticação e para autenticar solicitações de API no servidor, CSRF será um problema potencial.</span><span class="sxs-lookup"><span data-stu-id="b6807-200">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="b6807-201">No entanto, se o armazenamento local é usado para armazenar o token, CSRF vulnerabilidade pode ser atenuada, desde que os valores do armazenamento local não são enviados automaticamente para o servidor com cada nova solicitação.</span><span class="sxs-lookup"><span data-stu-id="b6807-201">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="b6807-202">Portanto, usando o armazenamento local para armazenar o token antiforgery no cliente e enviar o token como um cabeçalho de solicitação é uma abordagem recomendada.</span><span class="sxs-lookup"><span data-stu-id="b6807-202">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="b6807-203">AngularJS</span><span class="sxs-lookup"><span data-stu-id="b6807-203">AngularJS</span></span>

<span data-ttu-id="b6807-204">AngularJS usa uma convenção para endereço CSRF.</span><span class="sxs-lookup"><span data-stu-id="b6807-204">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="b6807-205">Se o servidor envia um cookie com o nome `XSRF-TOKEN`, o Angular `$http` serviço adicionará o valor desse cookie para um cabeçalho quando ele envia uma solicitação para este servidor.</span><span class="sxs-lookup"><span data-stu-id="b6807-205">If the server sends a cookie with the name `XSRF-TOKEN`, the Angular `$http` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="b6807-206">Esse processo é automático; Você não precisa definir o cabeçalho explicitamente.</span><span class="sxs-lookup"><span data-stu-id="b6807-206">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="b6807-207">O nome do cabeçalho é `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="b6807-207">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="b6807-208">O servidor deve detectar esse cabeçalho e validar seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="b6807-208">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="b6807-209">Para o ASP.NET Core API trabalhe com essa convenção:</span><span class="sxs-lookup"><span data-stu-id="b6807-209">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="b6807-210">Configure seu aplicativo para fornecer um token em um cookie chamado `XSRF-TOKEN`</span><span class="sxs-lookup"><span data-stu-id="b6807-210">Configure your app to provide a token in a cookie called `XSRF-TOKEN`</span></span>
* <span data-ttu-id="b6807-211">Configurar o serviço antiforgery para procurar um cabeçalho chamado `X-XSRF-TOKEN`</span><span class="sxs-lookup"><span data-stu-id="b6807-211">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="b6807-212">[Exibir exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span><span class="sxs-lookup"><span data-stu-id="b6807-212">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="b6807-213">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b6807-213">JavaScript</span></span>

<span data-ttu-id="b6807-214">Usando JavaScript com modos de exibição, você pode criar o token usando um serviço a partir do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="b6807-214">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="b6807-215">Para fazer isso, você deve inserir o `Microsoft.AspNetCore.Antiforgery.IAntiforgery` serviço no modo de exibição e chamada `GetAndStoreTokens`, conforme mostrado:</span><span class="sxs-lookup"><span data-stu-id="b6807-215">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,28)]

<span data-ttu-id="b6807-216">Essa abordagem elimina a necessidade de lidar diretamente com configuração cookies do servidor ou lê-los do cliente.</span><span class="sxs-lookup"><span data-stu-id="b6807-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="b6807-217">O exemplo anterior usa jQuery para ler o valor do campo oculto para o cabeçalho de POSTAGEM de AJAX.</span><span class="sxs-lookup"><span data-stu-id="b6807-217">The preceding example uses jQuery to read the hidden field value for the AJAX POST header.</span></span> <span data-ttu-id="b6807-218">Para usar JavaScript para obter o valor do token, use `document.getElementById('RequestVerificationToken').value`.</span><span class="sxs-lookup"><span data-stu-id="b6807-218">To use JavaScript to obtain the token's value, use `document.getElementById('RequestVerificationToken').value`.</span></span>

<span data-ttu-id="b6807-219">JavaScript pode também acessar tokens fornecidos em cookies e, em seguida, usar o conteúdo do cookie para criar um cabeçalho com o valor do token, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="b6807-219">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="b6807-220">Em seguida, supondo que você construir seu script solicitações para enviar o token em um cabeçalho chamado `X-CSRF-TOKEN`, configure o serviço antiforgery para procurar o `X-CSRF-TOKEN` cabeçalho:</span><span class="sxs-lookup"><span data-stu-id="b6807-220">Then, assuming you construct your script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="b6807-221">O exemplo a seguir usa o jQuery para fazer uma solicitação AJAX com o cabeçalho apropriado:</span><span class="sxs-lookup"><span data-stu-id="b6807-221">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a><span data-ttu-id="b6807-222">Configurando Antiforgery</span><span class="sxs-lookup"><span data-stu-id="b6807-222">Configuring Antiforgery</span></span>

<span data-ttu-id="b6807-223">`IAntiforgery` fornece a API para configurar o sistema antiforgery.</span><span class="sxs-lookup"><span data-stu-id="b6807-223">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="b6807-224">Ele pode ser solicitado no `Configure` método o `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="b6807-224">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="b6807-225">O exemplo a seguir usa o middleware da home page do aplicativo para gerar um token antiforgery e enviá-lo na resposta como um cookie (usando a convenção de nomenclatura Angular da padrão descrita acima):</span><span class="sxs-lookup"><span data-stu-id="b6807-225">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


```csharp
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a><span data-ttu-id="b6807-226">Opções</span><span class="sxs-lookup"><span data-stu-id="b6807-226">Options</span></span>

<span data-ttu-id="b6807-227">Você pode personalizar [opções antiforgery](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b6807-227">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "mydomain.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|<span data-ttu-id="b6807-228">Opção</span><span class="sxs-lookup"><span data-stu-id="b6807-228">Option</span></span>        | <span data-ttu-id="b6807-229">Descrição</span><span class="sxs-lookup"><span data-stu-id="b6807-229">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="b6807-230">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="b6807-230">CookieDomain</span></span>  | <span data-ttu-id="b6807-231">O domínio do cookie.</span><span class="sxs-lookup"><span data-stu-id="b6807-231">The domain of the cookie.</span></span> <span data-ttu-id="b6807-232">Assume o padrão de `null`.</span><span class="sxs-lookup"><span data-stu-id="b6807-232">Defaults to `null`.</span></span> |
|<span data-ttu-id="b6807-233">CookieName</span><span class="sxs-lookup"><span data-stu-id="b6807-233">CookieName</span></span>    | <span data-ttu-id="b6807-234">O nome do cookie.</span><span class="sxs-lookup"><span data-stu-id="b6807-234">The name of the cookie.</span></span> <span data-ttu-id="b6807-235">Se não estiver definida, o sistema irá gerar um nome exclusivo que começa com o `DefaultCookiePrefix` (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="b6807-235">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="b6807-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="b6807-236">CookiePath</span></span>    | <span data-ttu-id="b6807-237">O caminho definido no cookie.</span><span class="sxs-lookup"><span data-stu-id="b6807-237">The path set on the cookie.</span></span> |
|<span data-ttu-id="b6807-238">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="b6807-238">FormFieldName</span></span> | <span data-ttu-id="b6807-239">O nome do campo de formulário oculto usado pelo sistema antiforgery para processar tokens antiforgery nos modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="b6807-239">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="b6807-240">HeaderName</span><span class="sxs-lookup"><span data-stu-id="b6807-240">HeaderName</span></span>    | <span data-ttu-id="b6807-241">O nome do cabeçalho usado pelo sistema antiforgery.</span><span class="sxs-lookup"><span data-stu-id="b6807-241">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="b6807-242">Se `null`, o sistema considerará apenas os dados de formulário.</span><span class="sxs-lookup"><span data-stu-id="b6807-242">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="b6807-243">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="b6807-243">RequireSsl</span></span>    | <span data-ttu-id="b6807-244">Especifica se o SSL é exigido pelo sistema antiforgery.</span><span class="sxs-lookup"><span data-stu-id="b6807-244">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="b6807-245">Assume o padrão de `false`.</span><span class="sxs-lookup"><span data-stu-id="b6807-245">Defaults to `false`.</span></span> <span data-ttu-id="b6807-246">Se `true`, solicitações de não SSL falhará.</span><span class="sxs-lookup"><span data-stu-id="b6807-246">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="b6807-247">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="b6807-247">SuppressXFrameOptionsHeader</span></span> | <span data-ttu-id="b6807-248">Especifica se é suprimir a geração do `X-Frame-Options` cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="b6807-248">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="b6807-249">Por padrão, o cabeçalho é gerado com um valor de "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="b6807-249">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="b6807-250">Assume o padrão de `false`.</span><span class="sxs-lookup"><span data-stu-id="b6807-250">Defaults to `false`.</span></span> |

<span data-ttu-id="b6807-251">Consulte https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="b6807-251">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="b6807-252">Estendendo Antiforgery</span><span class="sxs-lookup"><span data-stu-id="b6807-252">Extending Antiforgery</span></span>

<span data-ttu-id="b6807-253">O [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo permite aos desenvolvedores estender o comportamento do sistema anti-XSRF por dados adicionais do ciclo em cada token.</span><span class="sxs-lookup"><span data-stu-id="b6807-253">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="b6807-254">O [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) método é chamado sempre que um token de campo é gerado e o valor de retorno é inserido no token gerado.</span><span class="sxs-lookup"><span data-stu-id="b6807-254">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="b6807-255">Um implementador poderia retornar um carimbo de hora, um valor de uso único ou qualquer outro valor e, em seguida, chamar [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) para validar dados quando o token é validado.</span><span class="sxs-lookup"><span data-stu-id="b6807-255">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="b6807-256">Nome de usuário do cliente já é inserido nos tokens gerados, portanto, não há necessidade de incluir essas informações.</span><span class="sxs-lookup"><span data-stu-id="b6807-256">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="b6807-257">Se um token inclui dados complementares, mas não `IAntiForgeryAdditionalDataProvider` tiver sido configurado, os dados complementares não são validados.</span><span class="sxs-lookup"><span data-stu-id="b6807-257">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data isn't validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="b6807-258">Princípios básicos</span><span class="sxs-lookup"><span data-stu-id="b6807-258">Fundamentals</span></span>

<span data-ttu-id="b6807-259">Ataques CSRF contam com o comportamento do navegador padrão de envio de cookies associados a um domínio com todas as solicitações feitas a esse domínio.</span><span class="sxs-lookup"><span data-stu-id="b6807-259">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="b6807-260">Esses cookies são armazenados dentro do navegador.</span><span class="sxs-lookup"><span data-stu-id="b6807-260">These cookies are stored within the browser.</span></span> <span data-ttu-id="b6807-261">Eles incluem frequentemente cookies de sessão para usuários autenticados.</span><span class="sxs-lookup"><span data-stu-id="b6807-261">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="b6807-262">Autenticação baseada em cookie é uma forma popular de autenticação.</span><span class="sxs-lookup"><span data-stu-id="b6807-262">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="b6807-263">Sistemas de autenticação baseada em token crescem popularidade, especialmente para SPAs e outros cenários "smart client".</span><span class="sxs-lookup"><span data-stu-id="b6807-263">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="b6807-264">Autenticação baseada em cookie</span><span class="sxs-lookup"><span data-stu-id="b6807-264">Cookie-based authentication</span></span>

<span data-ttu-id="b6807-265">Depois que um usuário autenticado usando o nome de usuário e senha, eles são emitidos um token que pode ser usado para identificá-los e validar que eles foram autenticados.</span><span class="sxs-lookup"><span data-stu-id="b6807-265">Once a user has authenticated using their username and password, they're issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="b6807-266">O token é armazenado como um cookie que acompanha cada solicitação de cliente.</span><span class="sxs-lookup"><span data-stu-id="b6807-266">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="b6807-267">Gerar e validar esse cookie é feito pelo middleware de autenticação de cookie.</span><span class="sxs-lookup"><span data-stu-id="b6807-267">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="b6807-268">ASP.NET Core fornece o cookie [middleware](xref:fundamentals/middleware/index) que serializa um objeto de usuário em um cookie criptografado e, em seguida, em solicitações subsequentes, valida o cookie recria a entidade de segurança e o atribui para a `User` propriedade no `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="b6807-268">ASP.NET Core provides cookie [middleware](xref:fundamentals/middleware/index) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="b6807-269">Quando um cookie é usado, o cookie de autenticação é apenas um contêiner para o tíquete de autenticação de formulários.</span><span class="sxs-lookup"><span data-stu-id="b6807-269">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="b6807-270">O tíquete é passado como o valor do cookie de autenticação de formulários com cada solicitação e é usado pela autenticação de formulários, no servidor, para identificar um usuário autenticado.</span><span class="sxs-lookup"><span data-stu-id="b6807-270">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="b6807-271">Quando um usuário está conectado a um sistema, uma sessão de usuário é criada no lado do servidor e é armazenada em um banco de dados ou algum outro repositório persistente.</span><span class="sxs-lookup"><span data-stu-id="b6807-271">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="b6807-272">O sistema gera uma chave de sessão que aponta para a sessão atual no repositório de dados e ele é enviado como um cookie do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="b6807-272">The system generates a session key that points to the actual session in the data store and it's sent as a client side cookie.</span></span> <span data-ttu-id="b6807-273">O servidor web verifica essa chave de sessão sempre que um usuário solicita um recurso que requer autorização.</span><span class="sxs-lookup"><span data-stu-id="b6807-273">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="b6807-274">O sistema verifica se a sessão de usuário associado tem o privilégio para acessar o recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="b6807-274">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="b6807-275">Nesse caso, a solicitação continuará.</span><span class="sxs-lookup"><span data-stu-id="b6807-275">If so, the request continues.</span></span> <span data-ttu-id="b6807-276">Caso contrário, a solicitação retorna como não autorizados.</span><span class="sxs-lookup"><span data-stu-id="b6807-276">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="b6807-277">Nessa abordagem, os cookies são usados para fazer com que o aplicativo parece estar com monitoração de estado, pois ele é capaz de "lembrar" que o usuário foi autenticado anteriormente com o servidor.</span><span class="sxs-lookup"><span data-stu-id="b6807-277">In this approach, cookies are used to make the application appear to be stateful, since it's able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="b6807-278">Tokens de usuário</span><span class="sxs-lookup"><span data-stu-id="b6807-278">User tokens</span></span>

<span data-ttu-id="b6807-279">Autenticação baseada em token não armazena a sessão no servidor.</span><span class="sxs-lookup"><span data-stu-id="b6807-279">Token-based authentication doesn't store session on the server.</span></span> <span data-ttu-id="b6807-280">Quando um usuário está conectado, eles são emitiu um token (não é um token antiforgery).</span><span class="sxs-lookup"><span data-stu-id="b6807-280">When a user is logged in, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="b6807-281">Esse token contém os dados que é necessária para validar o token.</span><span class="sxs-lookup"><span data-stu-id="b6807-281">This token holds the data that's required to validate the token.</span></span> <span data-ttu-id="b6807-282">Ele também contém informações de usuário na forma de [declarações](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span><span class="sxs-lookup"><span data-stu-id="b6807-282">It also contains user information in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="b6807-283">Quando um usuário deseja acessar um recurso de servidor que requer autenticação, o token é enviado para o servidor com um cabeçalho de autorização adicionais na forma de portador {token}.</span><span class="sxs-lookup"><span data-stu-id="b6807-283">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="b6807-284">Isso torna o aplicativo sem monitoração de estado como em cada solicitação subsequente o token é passado na solicitação de validação do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="b6807-284">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="b6807-285">Esse token não *criptografado*; em vez disso, ele é *codificado*.</span><span class="sxs-lookup"><span data-stu-id="b6807-285">This token isn't *encrypted*; rather it's *encoded*.</span></span> <span data-ttu-id="b6807-286">No lado do servidor, o token pode ser decodificado para acessar as informações não processadas dentro do token.</span><span class="sxs-lookup"><span data-stu-id="b6807-286">On the server-side, the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="b6807-287">Para enviar o token em solicitações subsequentes, ou armazená-lo no armazenamento local do navegador ou em um cookie.</span><span class="sxs-lookup"><span data-stu-id="b6807-287">To send the token in subsequent requests, either store it in the browser's local storage or in a cookie.</span></span> <span data-ttu-id="b6807-288">Não se preocupe vulnerabilidade XSRF se o token é armazenado no armazenamento local, mas é um problema se o token é armazenado em um cookie.</span><span class="sxs-lookup"><span data-stu-id="b6807-288">Don't worry about XSRF vulnerability if the token is stored in the local storage, but it's an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="b6807-289">Vários aplicativos são hospedados em um domínio</span><span class="sxs-lookup"><span data-stu-id="b6807-289">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="b6807-290">Embora `example1.cloudapp.net` e `example2.cloudapp.net` são hosts diferentes, há uma relação de confiança implícita entre os hosts sob o `*.cloudapp.net` domínio.</span><span class="sxs-lookup"><span data-stu-id="b6807-290">Although `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there's an implicit trust relationship between hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="b6807-291">Essa relação de confiança implícita permite que os hosts potencialmente não confiáveis afetam uns dos outros cookies (as políticas de mesma origem que governam as solicitações do AJAX não necessariamente se aplicam a cookies HTTP).</span><span class="sxs-lookup"><span data-stu-id="b6807-291">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="b6807-292">O tempo de execução do ASP.NET Core fornece alguma redução em que o nome de usuário é inserido para o token de campo.</span><span class="sxs-lookup"><span data-stu-id="b6807-292">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token.</span></span> <span data-ttu-id="b6807-293">Mesmo que um subdomínio mal-intencionado seja capaz de substituir um token de sessão, ele não é possível gerar um token de campo válido para o usuário.</span><span class="sxs-lookup"><span data-stu-id="b6807-293">Even if a malicious subdomain is able to overwrite a session token, it can't generate a valid field token for the user.</span></span> <span data-ttu-id="b6807-294">Quando hospedado em um ambiente desse tipo, as rotinas de anti-XSRF internas ainda não é possível proteger contra sequestro de sessão ou logon CSRF ataques.</span><span class="sxs-lookup"><span data-stu-id="b6807-294">When hosted in such an environment, the built-in anti-XSRF routines still can't defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="b6807-295">Ambientes de hospedagem compartilhados são vunerable sequestro de sessão, logon CSRF e outros ataques.</span><span class="sxs-lookup"><span data-stu-id="b6807-295">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="b6807-296">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="b6807-296">Additional resources</span></span>

* <span data-ttu-id="b6807-297">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [Abrir projeto de segurança de aplicativo da Web](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="b6807-297">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
