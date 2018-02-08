---
title: "Middleware de Reconfiguração de URL no ASP.NET Core"
author: guardrex
description: "Saiba mais sobre a reconfiguração de URL e o redirecionamento com o Middleware de Reconfiguração de URL em aplicativos ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 08/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/url-rewriting
ms.openlocfilehash: ca1f2f366bcf12cd3df83c3cdefa460cb9a68e2a
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/01/2018
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="7c752-103">Middleware de Reconfiguração de URL no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c752-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="7c752-104">Por [Luke Latham](https://github.com/guardrex) e [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="7c752-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="7c752-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7c752-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7c752-106">A reconfiguração de URL é o ato de modificar URLs de solicitação com base em uma ou mais regras predefinidas.</span><span class="sxs-lookup"><span data-stu-id="7c752-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="7c752-107">A reconfiguração de URL cria uma abstração entre locais de recursos e seus endereços, de forma que os locais e endereços não estejam totalmente vinculados.</span><span class="sxs-lookup"><span data-stu-id="7c752-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="7c752-108">Há vários cenários em que a reconfiguração de URL é útil:</span><span class="sxs-lookup"><span data-stu-id="7c752-108">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="7c752-109">Mover ou substituir recursos de servidor temporária ou permanentemente mantendo localizadores estáveis para esses recursos</span><span class="sxs-lookup"><span data-stu-id="7c752-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="7c752-110">Dividir o processamento de solicitação em diferentes aplicativos ou áreas de um aplicativo</span><span class="sxs-lookup"><span data-stu-id="7c752-110">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="7c752-111">Remover, adicionar ou reorganizar segmentos de URL em solicitações de entrada</span><span class="sxs-lookup"><span data-stu-id="7c752-111">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="7c752-112">Otimizar URLs públicas para SEO (otimização do mecanismo de pesquisa)</span><span class="sxs-lookup"><span data-stu-id="7c752-112">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="7c752-113">Permitir o uso de URLs públicas amigáveis para ajudar as pessoas a prever o conteúdo que encontrarão seguindo um link</span><span class="sxs-lookup"><span data-stu-id="7c752-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="7c752-114">Redirecionar solicitações não seguras para pontos de extremidade seguros</span><span class="sxs-lookup"><span data-stu-id="7c752-114">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="7c752-115">Impedir hotlinking de imagem</span><span class="sxs-lookup"><span data-stu-id="7c752-115">Preventing image hotlinking</span></span>

<span data-ttu-id="7c752-116">Defina regras para alterar a URL de várias maneiras, incluindo regex, regras do módulo mod_rewrite do Apache, regras do Módulo de Reconfiguração do IIS e uso de uma lógica de regra personalizada.</span><span class="sxs-lookup"><span data-stu-id="7c752-116">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="7c752-117">Este documento apresenta a reconfiguração de URL com instruções sobre como usar o Middleware de Reconfiguração de URL em aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7c752-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="7c752-118">A reconfiguração de URL pode reduzir o desempenho de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7c752-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="7c752-119">Sempre que possível, você deve limitar o número e a complexidade de regras.</span><span class="sxs-lookup"><span data-stu-id="7c752-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="7c752-120">Redirecionamento e reconfiguração de URL</span><span class="sxs-lookup"><span data-stu-id="7c752-120">URL redirect and URL rewrite</span></span>
<span data-ttu-id="7c752-121">A diferença entre os termos *redirecionamento de URL* e *reconfiguração de URL* pode parecer sutil no primeiro momento, mas tem implicações importantes no fornecimento de recursos aos clientes.</span><span class="sxs-lookup"><span data-stu-id="7c752-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="7c752-122">O Middleware de Reconfiguração de URL do ASP.NET Core pode atender às necessidades de ambos.</span><span class="sxs-lookup"><span data-stu-id="7c752-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="7c752-123">Um *redirecionamento de URL* é uma operação do lado do cliente, na qual o cliente é instruído a acessar um recurso em outro endereço.</span><span class="sxs-lookup"><span data-stu-id="7c752-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="7c752-124">Isso exige a ida e vinda para o servidor.</span><span class="sxs-lookup"><span data-stu-id="7c752-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="7c752-125">A URL de redirecionamento retornada para o cliente é exibida na barra de endereços do navegador quando o cliente faz uma nova solicitação para o recurso.</span><span class="sxs-lookup"><span data-stu-id="7c752-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="7c752-126">Se `/resource` é *redirecionado* para `/different-resource`, o cliente solicita `/resource`.</span><span class="sxs-lookup"><span data-stu-id="7c752-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="7c752-127">O servidor responde que o cliente deve obter o recurso em `/different-resource` com um código de status que indica que o redirecionamento é temporário ou permanente.</span><span class="sxs-lookup"><span data-stu-id="7c752-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="7c752-128">O cliente executa uma nova solicitação para o recurso na URL de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="7c752-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Um ponto de extremidade de serviço WebAPI foi alterado temporariamente da versão 1 (v1) para a versão 2 (v2) no servidor.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="7c752-134">Ao redirecionar solicitações para uma URL diferente, indique se o redirecionamento é permanente ou temporário.</span><span class="sxs-lookup"><span data-stu-id="7c752-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="7c752-135">O código de status 301 (Movido Permanentemente) é usado quando o recurso tem uma nova URL permanente e você deseja instruir o cliente que todas as solicitações futuras para o recurso devem usar a nova URL.</span><span class="sxs-lookup"><span data-stu-id="7c752-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="7c752-136">*O cliente pode armazenar a resposta em cache quando um código de status 301 é recebido.*</span><span class="sxs-lookup"><span data-stu-id="7c752-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="7c752-137">O código de status 302 (Encontrado) é usado quando o redirecionamento é temporário ou geralmente sujeito à alteração, como aquele em que o cliente não deve armazenar e reutilizar a URL de redirecionamento no futuro.</span><span class="sxs-lookup"><span data-stu-id="7c752-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="7c752-138">Para obter mais informações, consulte [RFC 2616: definições de código de status](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="7c752-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="7c752-139">Uma *reconfiguração de URL* é uma operação do servidor usada para fornecer um recurso de outro endereço de recurso.</span><span class="sxs-lookup"><span data-stu-id="7c752-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="7c752-140">A reconfiguração de uma URL não exige a ida e vinda para o servidor.</span><span class="sxs-lookup"><span data-stu-id="7c752-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="7c752-141">A URL reconfigurada não é retornada para o cliente e não será exibida na barra de endereços do navegador.</span><span class="sxs-lookup"><span data-stu-id="7c752-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="7c752-142">Quando `/resource` é *reconfigurado* para `/different-resource`, o cliente solicita `/resource` e o servidor busca *internamente* o recurso em `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="7c752-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="7c752-143">Embora o cliente possa ter a capacidade de recuperar o recurso na URL reconfigurada, o cliente não será informado de que o recurso existe na URL reconfigurada quando fizer a solicitação e receber a resposta.</span><span class="sxs-lookup"><span data-stu-id="7c752-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Um ponto de extremidade de serviço WebAPI foi alterado da versão 1 (v1) para a versão 2 (v2) no servidor.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="7c752-148">Aplicativo de exemplo de reconfiguração de URL</span><span class="sxs-lookup"><span data-stu-id="7c752-148">URL rewriting sample app</span></span>
<span data-ttu-id="7c752-149">Explore os recursos do Middleware de Reconfiguração de URL com o [aplicativo de exemplo de reconfiguração de URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span><span class="sxs-lookup"><span data-stu-id="7c752-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="7c752-150">O aplicativo aplica as regras de reconfiguração e redirecionamento e mostra a URL reconfigurada ou redirecionada.</span><span class="sxs-lookup"><span data-stu-id="7c752-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="7c752-151">Quando usar o Middleware de Reconfiguração de URL</span><span class="sxs-lookup"><span data-stu-id="7c752-151">When to use URL Rewriting Middleware</span></span>
<span data-ttu-id="7c752-152">Use o Middleware de Reconfiguração de URL quando não puder usar o [módulo de Reconfiguração de URL](https://www.iis.net/downloads/microsoft/url-rewrite) com o IIS no Windows Server, o [módulo mod_rewrite do Apache](https://httpd.apache.org/docs/2.4/rewrite/) no Apache Server, a [reconfiguração de URL no Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) ou quando o aplicativo estiver hospedado no [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente chamado [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="7c752-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="7c752-153">As principais razões para o uso das tecnologias de reconfiguração de URL baseadas em servidor no IIS, Apache ou Nginx são que o middleware não dá suporte aos recursos completos desses módulos e o desempenho do middleware provavelmente não corresponderá ao desempenho dos módulos.</span><span class="sxs-lookup"><span data-stu-id="7c752-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="7c752-154">No entanto, há alguns recursos dos módulos de servidor que não funcionam com projetos do ASP.NET Core, como as restrições `IsFile` e `IsDirectory` do módulo de Reconfiguração de IIS.</span><span class="sxs-lookup"><span data-stu-id="7c752-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="7c752-155">Nesses cenários, use o middleware.</span><span class="sxs-lookup"><span data-stu-id="7c752-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="7c752-156">Pacote</span><span class="sxs-lookup"><span data-stu-id="7c752-156">Package</span></span>
<span data-ttu-id="7c752-157">Para incluir o middleware em seu projeto, adicione uma referência ao pacote [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/).</span><span class="sxs-lookup"><span data-stu-id="7c752-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="7c752-158">Esse recurso está disponível para aplicativos direcionados ao ASP.NET Core 1.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="7c752-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="7c752-159">Extensão e opções</span><span class="sxs-lookup"><span data-stu-id="7c752-159">Extension and options</span></span>
<span data-ttu-id="7c752-160">Estabeleça as regras de reconfiguração e redirecionamento de URL criando uma instância da classe `RewriteOptions` com métodos de extensão para cada uma das regras.</span><span class="sxs-lookup"><span data-stu-id="7c752-160">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="7c752-161">Encadeie várias regras na ordem em que deseja que sejam processadas.</span><span class="sxs-lookup"><span data-stu-id="7c752-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="7c752-162">As `RewriteOptions` são passadas para o Middleware de Reconfiguração de URL, conforme ele é adicionado ao pipeline de solicitação com `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="7c752-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c752-163">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c752-163">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c752-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c752-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a><span data-ttu-id="7c752-165">Redirecionamento de URL</span><span class="sxs-lookup"><span data-stu-id="7c752-165">URL redirect</span></span>
<span data-ttu-id="7c752-166">Use `AddRedirect` para redirecionar as solicitações.</span><span class="sxs-lookup"><span data-stu-id="7c752-166">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="7c752-167">O primeiro parâmetro contém o regex para correspondência no caminho da URL de entrada.</span><span class="sxs-lookup"><span data-stu-id="7c752-167">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="7c752-168">O segundo parâmetro é a cadeia de caracteres de substituição.</span><span class="sxs-lookup"><span data-stu-id="7c752-168">The second parameter is the replacement string.</span></span> <span data-ttu-id="7c752-169">O terceiro parâmetro, se presente, especifica o código de status.</span><span class="sxs-lookup"><span data-stu-id="7c752-169">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="7c752-170">Se você não especificar o código de status, ele usará como padrão 302 (Encontrado), que indica que o recurso foi movido temporariamente ou substituído.</span><span class="sxs-lookup"><span data-stu-id="7c752-170">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c752-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c752-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c752-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c752-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

<span data-ttu-id="7c752-173">Em um navegador com as ferramentas para desenvolvedores habilitadas, faça uma solicitação para o aplicativo de exemplo com o caminho `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="7c752-173">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="7c752-174">O regex corresponde ao caminho de solicitação em `redirect-rule/(.*)` e o caminho é substituído por `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="7c752-174">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="7c752-175">A URL de redirecionamento é enviada novamente ao cliente com um código de status 302 (Encontrado).</span><span class="sxs-lookup"><span data-stu-id="7c752-175">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="7c752-176">O navegador faz uma nova solicitação na URL de redirecionamento, que é exibida na barra de endereços do navegador.</span><span class="sxs-lookup"><span data-stu-id="7c752-176">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="7c752-177">Como nenhuma regra no aplicativo de exemplo corresponde à URL de redirecionamento, a segunda solicitação recebe uma resposta 200 (OK) do aplicativo e o corpo da resposta mostra a URL de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="7c752-177">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="7c752-178">Uma ida e vinda é feita para o servidor quando uma URL é *redirecionada*.</span><span class="sxs-lookup"><span data-stu-id="7c752-178">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="7c752-179">Tenha cuidado ao estabelecer as regras de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="7c752-179">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="7c752-180">As regras de redirecionamento são avaliadas em cada solicitação para o aplicativo, incluindo após um redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="7c752-180">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="7c752-181">É fácil criar acidentalmente um loop de redirecionamentos infinitos.</span><span class="sxs-lookup"><span data-stu-id="7c752-181">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="7c752-182">Solicitação original: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="7c752-182">Original Request: `/redirect-rule/1234/5678`</span></span>

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="7c752-184">A parte da expressão contida nos parênteses é chamada um *grupo de captura*.</span><span class="sxs-lookup"><span data-stu-id="7c752-184">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="7c752-185">O ponto (`.`) da expressão significa *corresponder a qualquer caractere*.</span><span class="sxs-lookup"><span data-stu-id="7c752-185">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="7c752-186">O asterisco (`*`) indica *corresponder ao caractere zero precedente ou mais vezes*.</span><span class="sxs-lookup"><span data-stu-id="7c752-186">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="7c752-187">Portanto, os dois últimos segmentos de caminho da URL, `1234/5678`, são capturados pelo grupo de captura `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="7c752-187">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="7c752-188">Qualquer valor que você fornecer na URL de solicitação após `redirect-rule/` é capturado por esse único grupo de captura.</span><span class="sxs-lookup"><span data-stu-id="7c752-188">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="7c752-189">Na cadeia de caracteres de substituição, os grupos capturados são injetados na cadeia de caracteres com o cifrão (`$`) seguido do número de sequência da captura.</span><span class="sxs-lookup"><span data-stu-id="7c752-189">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="7c752-190">O primeiro valor de grupo de captura é obtido com `$1`, o segundo com `$2` e eles continuam em sequência para os grupos de captura no regex.</span><span class="sxs-lookup"><span data-stu-id="7c752-190">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="7c752-191">Há apenas um grupo capturado no regex da regra de redirecionamento no aplicativo de exemplo, para que haja apenas um grupo injetado na cadeia de caracteres de substituição, que é `$1`.</span><span class="sxs-lookup"><span data-stu-id="7c752-191">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="7c752-192">Quando a regra é aplicada, a URL se torna `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="7c752-192">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="7c752-193">Redirecionamento de URL para um ponto de extremidade seguro</span><span class="sxs-lookup"><span data-stu-id="7c752-193">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="7c752-194">Use `AddRedirectToHttps` para redirecionar solicitações HTTP para o mesmo host e caminho usando HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="7c752-194">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="7c752-195">Se o código de status não for fornecido, o middleware usará como padrão 302 (Encontrado).</span><span class="sxs-lookup"><span data-stu-id="7c752-195">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="7c752-196">Se a porta não for fornecida, o middleware usará `null` como padrão, o que significa que o protocolo é alterado para `https://` e o cliente acessa o recurso na porta 443.</span><span class="sxs-lookup"><span data-stu-id="7c752-196">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="7c752-197">O exemplo mostra como definir o código de status como 301 (Movido Permanentemente) e alterar a porta para 5001.</span><span class="sxs-lookup"><span data-stu-id="7c752-197">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
<span data-ttu-id="7c752-198">Use `AddRedirectToHttpsPermanent` para redirecionar solicitações não seguras para o mesmo host e caminho com o protocolo HTTPS seguro (`https://` na porta 443).</span><span class="sxs-lookup"><span data-stu-id="7c752-198">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="7c752-199">O middleware define o código de status como 301 (Movido Permanentemente).</span><span class="sxs-lookup"><span data-stu-id="7c752-199">The middleware sets the status code to 301 (Moved Permanently).</span></span>

<span data-ttu-id="7c752-200">O aplicativo de exemplo pode demonstrar como usar `AddRedirectToHttps` ou `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="7c752-200">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="7c752-201">Adicione o método de extensão às `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="7c752-201">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="7c752-202">Faça uma solicitação não segura para o aplicativo em qualquer URL.</span><span class="sxs-lookup"><span data-stu-id="7c752-202">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="7c752-203">Ignore o aviso de segurança do navegador que informa que o certificado autoassinado não é confiável.</span><span class="sxs-lookup"><span data-stu-id="7c752-203">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="7c752-204">Solicitação original usando `AddRedirectToHttps(301, 5001)`: `/secure`</span><span class="sxs-lookup"><span data-stu-id="7c752-204">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="7c752-206">Solicitação original usando `AddRedirectToHttpsPermanent`: `/secure`</span><span class="sxs-lookup"><span data-stu-id="7c752-206">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="7c752-208">Reconfiguração de URL</span><span class="sxs-lookup"><span data-stu-id="7c752-208">URL rewrite</span></span>
<span data-ttu-id="7c752-209">Use `AddRewrite` para criar uma regra para a reconfiguração de URLs.</span><span class="sxs-lookup"><span data-stu-id="7c752-209">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="7c752-210">O primeiro parâmetro contém o regex para correspondência no caminho da URL de entrada.</span><span class="sxs-lookup"><span data-stu-id="7c752-210">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="7c752-211">O segundo parâmetro é a cadeia de caracteres de substituição.</span><span class="sxs-lookup"><span data-stu-id="7c752-211">The second parameter is the replacement string.</span></span> <span data-ttu-id="7c752-212">O terceiro parâmetro, `skipRemainingRules: {true|false}`, indica para o middleware se ele deve ou não ignorar regras de reconfiguração adicionais se a regra atual é aplicada.</span><span class="sxs-lookup"><span data-stu-id="7c752-212">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c752-213">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c752-213">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c752-214">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c752-214">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

<span data-ttu-id="7c752-215">Solicitação original: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="7c752-215">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando a solicitação e resposta](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="7c752-217">A primeira coisa que é observada no regex é o acento circunflexo (`^`) no início da expressão.</span><span class="sxs-lookup"><span data-stu-id="7c752-217">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="7c752-218">Isso significa que a correspondência começa no início do caminho da URL.</span><span class="sxs-lookup"><span data-stu-id="7c752-218">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="7c752-219">No exemplo anterior com a regra de redirecionamento, `redirect-rule/(.*)`, não há nenhum acento circunflexo no início do regex; portanto, qualquer caractere pode preceder `redirect-rule/` no caminho para uma correspondência bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="7c752-219">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="7c752-220">Caminho</span><span class="sxs-lookup"><span data-stu-id="7c752-220">Path</span></span>                               | <span data-ttu-id="7c752-221">Corresponder a</span><span class="sxs-lookup"><span data-stu-id="7c752-221">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="7c752-222">Sim</span><span class="sxs-lookup"><span data-stu-id="7c752-222">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="7c752-223">Sim</span><span class="sxs-lookup"><span data-stu-id="7c752-223">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="7c752-224">Sim</span><span class="sxs-lookup"><span data-stu-id="7c752-224">Yes</span></span>   |

<span data-ttu-id="7c752-225">A regra de reconfiguração, `^rewrite-rule/(\d+)/(\d+)`, corresponde apenas a caminhos se eles são iniciados com `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="7c752-225">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="7c752-226">Observe a diferença na correspondência entre a regra de reconfiguração abaixo e a regra de redirecionamento acima.</span><span class="sxs-lookup"><span data-stu-id="7c752-226">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="7c752-227">Caminho</span><span class="sxs-lookup"><span data-stu-id="7c752-227">Path</span></span>                              | <span data-ttu-id="7c752-228">Corresponder a</span><span class="sxs-lookup"><span data-stu-id="7c752-228">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="7c752-229">Sim</span><span class="sxs-lookup"><span data-stu-id="7c752-229">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="7c752-230">Não</span><span class="sxs-lookup"><span data-stu-id="7c752-230">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="7c752-231">Não</span><span class="sxs-lookup"><span data-stu-id="7c752-231">No</span></span>    |

<span data-ttu-id="7c752-232">Após a parte `^rewrite-rule/` da expressão, há dois grupos de captura, `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="7c752-232">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="7c752-233">O `\d` significa *corresponder a um dígito (número)*.</span><span class="sxs-lookup"><span data-stu-id="7c752-233">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="7c752-234">O sinal de adição (`+`) significa *corresponder a um ou mais caracteres anteriores*.</span><span class="sxs-lookup"><span data-stu-id="7c752-234">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="7c752-235">Portanto, a URL precisa conter um número seguido de uma barra "/" seguida de outro número.</span><span class="sxs-lookup"><span data-stu-id="7c752-235">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="7c752-236">Esses grupos de captura são injetados na URL reconfigurada como `$1` e `$2`.</span><span class="sxs-lookup"><span data-stu-id="7c752-236">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="7c752-237">A cadeia de caracteres de substituição da regra de reconfiguração coloca os grupos capturados na cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="7c752-237">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="7c752-238">O caminho solicitado de `/rewrite-rule/1234/5678` foi reconfigurado para obter o recurso em `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="7c752-238">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="7c752-239">Se uma cadeia de consulta estiver presente na solicitação original, ela será preservada quando a URL for reconfigurada.</span><span class="sxs-lookup"><span data-stu-id="7c752-239">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="7c752-240">Não há nenhuma ida e vinda para o servidor para obtenção do recurso.</span><span class="sxs-lookup"><span data-stu-id="7c752-240">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="7c752-241">Se o recurso existir, ele será buscado e retornado ao cliente com um código de status 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="7c752-241">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="7c752-242">Como o cliente não é redirecionado, a URL na barra de endereços do navegador não é alterada.</span><span class="sxs-lookup"><span data-stu-id="7c752-242">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="7c752-243">Quanto ao cliente, a operação de reconfiguração de URL nunca ocorreu.</span><span class="sxs-lookup"><span data-stu-id="7c752-243">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="7c752-244">Use `skipRemainingRules: true` sempre que possível, porque as regras de correspondência são um processo caro e reduzem o tempo de resposta do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7c752-244">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="7c752-245">Para a resposta mais rápida do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="7c752-245">For the fastest app response:</span></span>
> * <span data-ttu-id="7c752-246">Ordene as regras de reconfiguração da regra com correspondência mais frequente para a regra com correspondência menos frequente.</span><span class="sxs-lookup"><span data-stu-id="7c752-246">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="7c752-247">Ignore o processamento das regras restantes quando ocorrer uma correspondência e nenhum processamento de regra adicional for necessário.</span><span class="sxs-lookup"><span data-stu-id="7c752-247">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="7c752-248">mod_rewrite do Apache</span><span class="sxs-lookup"><span data-stu-id="7c752-248">Apache mod_rewrite</span></span>
<span data-ttu-id="7c752-249">Aplique as regras do mod_rewrite do Apache com `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="7c752-249">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="7c752-250">Verifique se o arquivo de regras foi implantado com o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7c752-250">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="7c752-251">Para obter mais informações e exemplos de regras de mod_rewrite, consulte [mod_rewrite do Apache](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="7c752-251">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c752-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c752-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7c752-253">Um `StreamReader` é usado para ler as regras do arquivo de regras *ApacheModRewrite.txt*.</span><span class="sxs-lookup"><span data-stu-id="7c752-253">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c752-254">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c752-254">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7c752-255">O primeiro parâmetro usa um `IFileProvider`, que é fornecido por meio da [Injeção de Dependência](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="7c752-255">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="7c752-256">O `IHostingEnvironment` é injetado para fornecer o `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="7c752-256">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="7c752-257">O segundo parâmetro é o caminho para o arquivo de regras, que é *ApacheModRewrite.txt* no aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="7c752-257">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

<span data-ttu-id="7c752-258">O aplicativo de exemplo redireciona solicitações de `/apache-mod-rules-redirect/(.\*)` para `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="7c752-258">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="7c752-259">O código de status da resposta é 302 (Encontrado).</span><span class="sxs-lookup"><span data-stu-id="7c752-259">The response status code is 302 (Found).</span></span>

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

<span data-ttu-id="7c752-260">Solicitação original: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="7c752-260">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="7c752-262">Variáveis de servidor compatíveis</span><span class="sxs-lookup"><span data-stu-id="7c752-262">Supported server variables</span></span>
<span data-ttu-id="7c752-263">O middleware dá suporte às seguintes variáveis de servidor do mod_rewrite do Apache:</span><span class="sxs-lookup"><span data-stu-id="7c752-263">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="7c752-264">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="7c752-264">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="7c752-265">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="7c752-265">HTTP_ACCEPT</span></span>
* <span data-ttu-id="7c752-266">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="7c752-266">HTTP_CONNECTION</span></span>
* <span data-ttu-id="7c752-267">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="7c752-267">HTTP_COOKIE</span></span>
* <span data-ttu-id="7c752-268">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="7c752-268">HTTP_FORWARDED</span></span>
* <span data-ttu-id="7c752-269">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="7c752-269">HTTP_HOST</span></span>
* <span data-ttu-id="7c752-270">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="7c752-270">HTTP_REFERER</span></span>
* <span data-ttu-id="7c752-271">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="7c752-271">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="7c752-272">HTTPS</span><span class="sxs-lookup"><span data-stu-id="7c752-272">HTTPS</span></span>
* <span data-ttu-id="7c752-273">IPV6</span><span class="sxs-lookup"><span data-stu-id="7c752-273">IPV6</span></span>
* <span data-ttu-id="7c752-274">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="7c752-274">QUERY_STRING</span></span>
* <span data-ttu-id="7c752-275">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="7c752-275">REMOTE_ADDR</span></span>
* <span data-ttu-id="7c752-276">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="7c752-276">REMOTE_PORT</span></span>
* <span data-ttu-id="7c752-277">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="7c752-277">REQUEST_FILENAME</span></span>
* <span data-ttu-id="7c752-278">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="7c752-278">REQUEST_METHOD</span></span>
* <span data-ttu-id="7c752-279">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="7c752-279">REQUEST_SCHEME</span></span>
* <span data-ttu-id="7c752-280">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="7c752-280">REQUEST_URI</span></span>
* <span data-ttu-id="7c752-281">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="7c752-281">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="7c752-282">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="7c752-282">SERVER_ADDR</span></span>
* <span data-ttu-id="7c752-283">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="7c752-283">SERVER_PORT</span></span>
* <span data-ttu-id="7c752-284">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="7c752-284">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="7c752-285">TIME</span><span class="sxs-lookup"><span data-stu-id="7c752-285">TIME</span></span>
* <span data-ttu-id="7c752-286">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="7c752-286">TIME_DAY</span></span>
* <span data-ttu-id="7c752-287">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="7c752-287">TIME_HOUR</span></span>
* <span data-ttu-id="7c752-288">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="7c752-288">TIME_MIN</span></span>
* <span data-ttu-id="7c752-289">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="7c752-289">TIME_MON</span></span>
* <span data-ttu-id="7c752-290">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="7c752-290">TIME_SEC</span></span>
* <span data-ttu-id="7c752-291">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="7c752-291">TIME_WDAY</span></span>
* <span data-ttu-id="7c752-292">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="7c752-292">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="7c752-293">Regras do Módulo de Reconfiguração de URL do IIS</span><span class="sxs-lookup"><span data-stu-id="7c752-293">IIS URL Rewrite Module rules</span></span>
<span data-ttu-id="7c752-294">Para usar regras que se aplicam ao Módulo de Reconfiguração de URL do IIS, use `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="7c752-294">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="7c752-295">Verifique se o arquivo de regras foi implantado com o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7c752-295">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="7c752-296">Não instrua o middleware a usar o arquivo *web.config* quando ele estiver em execução no IIS do Windows Server.</span><span class="sxs-lookup"><span data-stu-id="7c752-296">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="7c752-297">Com o IIS, essas regras devem ser armazenadas fora do *web.config* para evitar conflitos com o módulo de Reconfiguração do IIS.</span><span class="sxs-lookup"><span data-stu-id="7c752-297">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="7c752-298">Para obter mais informações e exemplos de regras do Módulo de Reconfiguração de URL do IIS, consulte [Usando o Módulo de Reconfiguração de URL 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) e [Referência de configuração do Módulo de Reconfiguração de URL](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="7c752-298">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c752-299">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c752-299">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7c752-300">Um `StreamReader` é usado para ler as regras do arquivo de regras *IISUrlRewrite.xml*.</span><span class="sxs-lookup"><span data-stu-id="7c752-300">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c752-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c752-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7c752-302">O primeiro parâmetro usa um `IFileProvider`, enquanto o segundo é o caminho para o arquivo de regras XML, que é *IISUrlRewrite.xml* no aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="7c752-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

<span data-ttu-id="7c752-303">O aplicativo de exemplo reconfigura as solicitações de `/iis-rules-rewrite/(.*)` para `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="7c752-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="7c752-304">A resposta é enviada ao cliente com um código de status 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="7c752-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

<span data-ttu-id="7c752-305">Solicitação original: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="7c752-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando a solicitação e resposta](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="7c752-307">Caso você tenha um Módulo de Reconfiguração do IIS ativo com regras no nível do servidor configuradas que poderiam afetar o aplicativo de maneiras indesejadas, desabilite o Módulo de Reconfiguração do IIS em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7c752-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="7c752-308">Para obter mais informações, consulte [Desabilitando módulos do IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="7c752-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="7c752-309">Recursos sem suporte</span><span class="sxs-lookup"><span data-stu-id="7c752-309">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c752-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c752-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7c752-311">O middleware liberado com o ASP.NET Core 2.x não dá suporte aos seguintes recursos do Módulo de Reconfiguração de URL do IIS:</span><span class="sxs-lookup"><span data-stu-id="7c752-311">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="7c752-312">Regras de saída</span><span class="sxs-lookup"><span data-stu-id="7c752-312">Outbound Rules</span></span>
* <span data-ttu-id="7c752-313">Variáveis de servidor personalizadas</span><span class="sxs-lookup"><span data-stu-id="7c752-313">Custom Server Variables</span></span>
* <span data-ttu-id="7c752-314">Curingas</span><span class="sxs-lookup"><span data-stu-id="7c752-314">Wildcards</span></span>
* <span data-ttu-id="7c752-315">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="7c752-315">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c752-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c752-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7c752-317">O middleware liberado com o ASP.NET Core 1.x não dá suporte aos seguintes recursos do Módulo de Reconfiguração de URL do IIS:</span><span class="sxs-lookup"><span data-stu-id="7c752-317">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="7c752-318">Regras globais</span><span class="sxs-lookup"><span data-stu-id="7c752-318">Global Rules</span></span>
* <span data-ttu-id="7c752-319">Regras de saída</span><span class="sxs-lookup"><span data-stu-id="7c752-319">Outbound Rules</span></span>
* <span data-ttu-id="7c752-320">Mapas de Reconfiguração</span><span class="sxs-lookup"><span data-stu-id="7c752-320">Rewrite Maps</span></span>
* <span data-ttu-id="7c752-321">Ação de CustomResponse</span><span class="sxs-lookup"><span data-stu-id="7c752-321">CustomResponse Action</span></span>
* <span data-ttu-id="7c752-322">Variáveis de servidor personalizadas</span><span class="sxs-lookup"><span data-stu-id="7c752-322">Custom Server Variables</span></span>
* <span data-ttu-id="7c752-323">Curingas</span><span class="sxs-lookup"><span data-stu-id="7c752-323">Wildcards</span></span>
* <span data-ttu-id="7c752-324">Action:CustomResponse</span><span class="sxs-lookup"><span data-stu-id="7c752-324">Action:CustomResponse</span></span>
* <span data-ttu-id="7c752-325">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="7c752-325">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="7c752-326">Variáveis de servidor compatíveis</span><span class="sxs-lookup"><span data-stu-id="7c752-326">Supported server variables</span></span>
<span data-ttu-id="7c752-327">O middleware dá suporte às seguintes variáveis de servidor do Módulo de Reconfiguração de URL do IIS:</span><span class="sxs-lookup"><span data-stu-id="7c752-327">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="7c752-328">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="7c752-328">CONTENT_LENGTH</span></span>
* <span data-ttu-id="7c752-329">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="7c752-329">CONTENT_TYPE</span></span>
* <span data-ttu-id="7c752-330">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="7c752-330">HTTP_ACCEPT</span></span>
* <span data-ttu-id="7c752-331">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="7c752-331">HTTP_CONNECTION</span></span>
* <span data-ttu-id="7c752-332">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="7c752-332">HTTP_COOKIE</span></span>
* <span data-ttu-id="7c752-333">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="7c752-333">HTTP_HOST</span></span>
* <span data-ttu-id="7c752-334">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="7c752-334">HTTP_REFERER</span></span>
* <span data-ttu-id="7c752-335">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="7c752-335">HTTP_URL</span></span>
* <span data-ttu-id="7c752-336">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="7c752-336">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="7c752-337">HTTPS</span><span class="sxs-lookup"><span data-stu-id="7c752-337">HTTPS</span></span>
* <span data-ttu-id="7c752-338">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="7c752-338">LOCAL_ADDR</span></span>
* <span data-ttu-id="7c752-339">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="7c752-339">QUERY_STRING</span></span>
* <span data-ttu-id="7c752-340">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="7c752-340">REMOTE_ADDR</span></span>
* <span data-ttu-id="7c752-341">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="7c752-341">REMOTE_PORT</span></span>
* <span data-ttu-id="7c752-342">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="7c752-342">REQUEST_FILENAME</span></span>
* <span data-ttu-id="7c752-343">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="7c752-343">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="7c752-344">Também obtenha um `IFileProvider` por meio de um `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="7c752-344">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="7c752-345">Essa abordagem pode fornecer maior flexibilidade para o local dos arquivos de regras de reconfiguração.</span><span class="sxs-lookup"><span data-stu-id="7c752-345">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="7c752-346">Verifique se os arquivos de regras de reconfiguração são implantados no servidor no caminho fornecido.</span><span class="sxs-lookup"><span data-stu-id="7c752-346">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="7c752-347">Regra baseada em método</span><span class="sxs-lookup"><span data-stu-id="7c752-347">Method-based rule</span></span>
<span data-ttu-id="7c752-348">Use `Add(Action<RewriteContext> applyRule)` para implementar sua própria lógica de regra em um método.</span><span class="sxs-lookup"><span data-stu-id="7c752-348">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="7c752-349">O `RewriteContext` expõe o `HttpContext` para uso no método.</span><span class="sxs-lookup"><span data-stu-id="7c752-349">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="7c752-350">O `context.Result` determina como o processamento de pipeline adicional é manipulado.</span><span class="sxs-lookup"><span data-stu-id="7c752-350">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="7c752-351">context.Result</span><span class="sxs-lookup"><span data-stu-id="7c752-351">context.Result</span></span>                       | <span data-ttu-id="7c752-352">Ação</span><span class="sxs-lookup"><span data-stu-id="7c752-352">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="7c752-353">`RuleResult.ContinueRules` (padrão)</span><span class="sxs-lookup"><span data-stu-id="7c752-353">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="7c752-354">Continuar aplicando regras</span><span class="sxs-lookup"><span data-stu-id="7c752-354">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="7c752-355">Parar de aplicar regras e enviar a resposta</span><span class="sxs-lookup"><span data-stu-id="7c752-355">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="7c752-356">Parar de aplicar regras e enviar o contexto para o próximo middleware</span><span class="sxs-lookup"><span data-stu-id="7c752-356">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c752-357">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c752-357">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c752-358">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c752-358">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

<span data-ttu-id="7c752-359">O aplicativo de exemplo demonstra um método que redireciona as solicitações para caminhos que terminam com *.xml*.</span><span class="sxs-lookup"><span data-stu-id="7c752-359">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="7c752-360">Se você faz uma solicitação para `/file.xml`, ela é redirecionada para `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="7c752-360">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="7c752-361">O código de status é definido como 301 (Movido Permanentemente).</span><span class="sxs-lookup"><span data-stu-id="7c752-361">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="7c752-362">Para um redirecionamento, é necessário definir explicitamente o código de status da resposta; caso contrário, será retornado um código de status 200 (OK) e o redirecionamento não ocorrerá no cliente.</span><span class="sxs-lookup"><span data-stu-id="7c752-362">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c752-363">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c752-363">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c752-364">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c752-364">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

<span data-ttu-id="7c752-365">Solicitação original: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="7c752-365">Original Request: `/file.xml`</span></span>

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas para file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="7c752-367">Regra baseada em IRule</span><span class="sxs-lookup"><span data-stu-id="7c752-367">IRule-based rule</span></span>
<span data-ttu-id="7c752-368">Use `Add(IRule)` para implementar sua própria lógica de regra em uma classe que é derivada de `IRule`.</span><span class="sxs-lookup"><span data-stu-id="7c752-368">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="7c752-369">O uso de uma `IRule` fornece maior flexibilidade em relação ao uso da abordagem de regra baseada em método.</span><span class="sxs-lookup"><span data-stu-id="7c752-369">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="7c752-370">A classe derivada pode incluir um construtor, no qual você pode passar parâmetros para o método `ApplyRule`.</span><span class="sxs-lookup"><span data-stu-id="7c752-370">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c752-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c752-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c752-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c752-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

<span data-ttu-id="7c752-373">Os valores dos parâmetros no aplicativo de exemplo para a `extension` e o `newPath` são verificados para atender a várias condições.</span><span class="sxs-lookup"><span data-stu-id="7c752-373">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="7c752-374">A `extension` precisa conter um valor que precisa ser *.png*, *.jpg* ou *.gif*.</span><span class="sxs-lookup"><span data-stu-id="7c752-374">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="7c752-375">Se o `newPath` não é válido, uma `ArgumentException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="7c752-375">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="7c752-376">Se você faz uma solicitação para *image.png*, ela é redirecionada para `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="7c752-376">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="7c752-377">Se você faz uma solicitação para *image.jpg*, ela é redirecionada para `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="7c752-377">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="7c752-378">O código de status é definido como 301 (Movido Permanentemente) e o `context.Result` é definida para parar o processamento de regras e enviar a resposta.</span><span class="sxs-lookup"><span data-stu-id="7c752-378">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c752-379">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c752-379">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c752-380">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c752-380">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

<span data-ttu-id="7c752-381">Solicitação original: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="7c752-381">Original Request: `/image.png`</span></span>

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas para image.png](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="7c752-383">Solicitação original: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="7c752-383">Original Request: `/image.jpg`</span></span>

![Janela do navegador com as Ferramentas para Desenvolvedores acompanhando as solicitações e respostas para image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="7c752-385">Exemplos do regex</span><span class="sxs-lookup"><span data-stu-id="7c752-385">Regex examples</span></span>

| <span data-ttu-id="7c752-386">Goal</span><span class="sxs-lookup"><span data-stu-id="7c752-386">Goal</span></span> | <span data-ttu-id="7c752-387">Cadeia de caracteres do regex &</span><span class="sxs-lookup"><span data-stu-id="7c752-387">Regex String &</span></span><br><span data-ttu-id="7c752-388">Exemplo de correspondência</span><span class="sxs-lookup"><span data-stu-id="7c752-388">Match Example</span></span> | <span data-ttu-id="7c752-389">Cadeia de caracteres de substituição &</span><span class="sxs-lookup"><span data-stu-id="7c752-389">Replacement String &</span></span><br><span data-ttu-id="7c752-390">Exemplo de saída</span><span class="sxs-lookup"><span data-stu-id="7c752-390">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="7c752-391">Reconfigurar o caminho na cadeia de consulta</span><span class="sxs-lookup"><span data-stu-id="7c752-391">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="7c752-392">Barra "/" à direita da faixa</span><span class="sxs-lookup"><span data-stu-id="7c752-392">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="7c752-393">Impor barra "/" à direita</span><span class="sxs-lookup"><span data-stu-id="7c752-393">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="7c752-394">Evitar a reconfiguração de solicitações específicas</span><span class="sxs-lookup"><span data-stu-id="7c752-394">Avoid rewriting specific requests</span></span> | <span data-ttu-id="7c752-395">`^(.*)(?<!\.axd)$` ou `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="7c752-395">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="7c752-396">Sim: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="7c752-396">Yes: `/resource.htm`</span></span><br><span data-ttu-id="7c752-397">Não: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="7c752-397">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="7c752-398">Reorganizar segmentos de URL</span><span class="sxs-lookup"><span data-stu-id="7c752-398">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="7c752-399">Substituir um segmento de URL</span><span class="sxs-lookup"><span data-stu-id="7c752-399">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="7c752-400">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7c752-400">Additional resources</span></span>
* [<span data-ttu-id="7c752-401">Inicialização de aplicativos</span><span class="sxs-lookup"><span data-stu-id="7c752-401">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="7c752-402">Middleware</span><span class="sxs-lookup"><span data-stu-id="7c752-402">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="7c752-403">Expressões regulares no .NET</span><span class="sxs-lookup"><span data-stu-id="7c752-403">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="7c752-404">Linguagem de expressão regular – referência rápida</span><span class="sxs-lookup"><span data-stu-id="7c752-404">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="7c752-405">mod_rewrite do Apache</span><span class="sxs-lookup"><span data-stu-id="7c752-405">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="7c752-406">Usando o Módulo de Reconfiguração de URL 2.0 (para IIS)</span><span class="sxs-lookup"><span data-stu-id="7c752-406">Using Url Rewrite Module 2.0 (for IIS)</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="7c752-407">Referência de configuração do Módulo de Reconfiguração de URL</span><span class="sxs-lookup"><span data-stu-id="7c752-407">URL Rewrite Module Configuration Reference</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="7c752-408">Fórum do Módulo de Reconfiguração de URL do IIS</span><span class="sxs-lookup"><span data-stu-id="7c752-408">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="7c752-409">Manter uma estrutura de URL simples</span><span class="sxs-lookup"><span data-stu-id="7c752-409">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="7c752-410">10 dicas e truques de reconfiguração de URL</span><span class="sxs-lookup"><span data-stu-id="7c752-410">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="7c752-411">Inserir ou não inserir uma barra "/"</span><span class="sxs-lookup"><span data-stu-id="7c752-411">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
