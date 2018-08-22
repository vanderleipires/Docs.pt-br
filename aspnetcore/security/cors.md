---
title: Habilitar solicitações entre origens (CORS) no ASP.NET Core
author: rick-anderson
description: Saiba como CORS como padrão para permitir ou rejeitar solicitações entre origens em um aplicativo ASP.NET Core.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2018
ms.locfileid: "41830098"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="ed60e-103">Habilitar solicitações entre origens (CORS) no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed60e-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="ed60e-104">Por [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ed60e-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ed60e-105">Segurança do navegador impede que uma página da web faça solicitações AJAX para outro domínio.</span><span class="sxs-lookup"><span data-stu-id="ed60e-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="ed60e-106">Essa restrição é chamada de *política de mesma origem*e impede que um site mal-intencionado lendo dados confidenciais de outro site.</span><span class="sxs-lookup"><span data-stu-id="ed60e-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="ed60e-107">No entanto, às vezes, você talvez queira permitir que outros sites fazer solicitações entre origens em sua API da web.</span><span class="sxs-lookup"><span data-stu-id="ed60e-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="ed60e-108">[Entre o compartilhamento de recursos de origem](http://www.w3.org/TR/cors/) (CORS) é um padrão W3C que permite que um servidor relaxar a política de mesma origem.</span><span class="sxs-lookup"><span data-stu-id="ed60e-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="ed60e-109">Usando o CORS, um servidor pode explicitamente permitir algumas solicitações entre origens enquanto rejeita outras.</span><span class="sxs-lookup"><span data-stu-id="ed60e-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="ed60e-110">O CORS é mais seguro e flexível que técnicas anteriores, como [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="ed60e-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="ed60e-111">Este tópico mostra como habilitar o CORS em um aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed60e-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="ed60e-112">O que é "mesma origem"?</span><span class="sxs-lookup"><span data-stu-id="ed60e-112">What is "same origin"?</span></span>

<span data-ttu-id="ed60e-113">Duas URLs têm a mesma origem se eles tiverem as portas, hosts e esquemas idênticas.</span><span class="sxs-lookup"><span data-stu-id="ed60e-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="ed60e-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="ed60e-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="ed60e-115">Essas duas URLs têm a mesma origem:</span><span class="sxs-lookup"><span data-stu-id="ed60e-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="ed60e-116">Essas URLs tem diferentes origens que o anterior dois:</span><span class="sxs-lookup"><span data-stu-id="ed60e-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="ed60e-117">`http://example.net` -Domínio diferente</span><span class="sxs-lookup"><span data-stu-id="ed60e-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="ed60e-118">`http://www.example.com/foo.html` -Subdomínio diferente</span><span class="sxs-lookup"><span data-stu-id="ed60e-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="ed60e-119">`https://example.com/foo.html` -Esquema diferente</span><span class="sxs-lookup"><span data-stu-id="ed60e-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="ed60e-120">`http://example.com:9000/foo.html` -Porta diferente</span><span class="sxs-lookup"><span data-stu-id="ed60e-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="ed60e-121">Internet Explorer não considera a porta ao comparar as origens.</span><span class="sxs-lookup"><span data-stu-id="ed60e-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="enable-cors"></a><span data-ttu-id="ed60e-122">Habilitar o CORS</span><span class="sxs-lookup"><span data-stu-id="ed60e-122">Enable CORS</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="ed60e-123">Para configurar o CORS para o seu aplicativo, adicione o `Microsoft.AspNetCore.Cors` pacote ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="ed60e-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

::: moniker-end

<span data-ttu-id="ed60e-124">Chame [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) em `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ed60e-124">Call [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="ed60e-125">Habilitando CORS com middleware</span><span class="sxs-lookup"><span data-stu-id="ed60e-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="ed60e-126">Para habilitar o CORS, adicione o middleware do CORS ao pipeline de solicitação usando o `UseCors` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="ed60e-126">To enable CORS, add the CORS middleware to the request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="ed60e-127">O middleware do CORS deve preceder quaisquer pontos de extremidade definidos em seu aplicativo onde você deseja dar suporte a solicitações entre origens (por exemplo, antes de qualquer chamada para `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="ed60e-127">The CORS middleware must precede any defined endpoints in your app where you want to support cross-origin requests (For example, before any call to `UseMvc`).</span></span>

<span data-ttu-id="ed60e-128">Uma política entre origens pode ser especificada ao adicionar o middleware do CORS usando o [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) classe.</span><span class="sxs-lookup"><span data-stu-id="ed60e-128">A cross-origin policy can be specified when adding the CORS middleware using the [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) class.</span></span> <span data-ttu-id="ed60e-129">Há duas formas de fazer isso.</span><span class="sxs-lookup"><span data-stu-id="ed60e-129">There are two ways to do this.</span></span> <span data-ttu-id="ed60e-130">A primeira é chamar `UseCors` com um lambda:</span><span class="sxs-lookup"><span data-stu-id="ed60e-130">The first is to call `UseCors` with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="ed60e-131">**Observação:** a URL deve ser especificada sem uma barra à direita (`/`).</span><span class="sxs-lookup"><span data-stu-id="ed60e-131">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="ed60e-132">Se a URL termina com `/`, a comparação retornará `false` e nenhum cabeçalho será retornado.</span><span class="sxs-lookup"><span data-stu-id="ed60e-132">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="ed60e-133">O lambda utiliza um `CorsPolicyBuilder` objeto.</span><span class="sxs-lookup"><span data-stu-id="ed60e-133">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="ed60e-134">Você encontrará uma lista da [opções de configuração](#cors-policy-options) mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="ed60e-134">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="ed60e-135">Neste exemplo, a política permite que solicitações entre origens de `http://example.com` e sem outras origens.</span><span class="sxs-lookup"><span data-stu-id="ed60e-135">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="ed60e-136">CorsPolicyBuilder tem uma API fluente, portanto, é possível encadear chamadas de método:</span><span class="sxs-lookup"><span data-stu-id="ed60e-136">CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="ed60e-137">A segunda abordagem é definir uma ou mais políticas CORS e, em seguida, selecione a política por nome em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ed60e-137">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="ed60e-138">Este exemplo adiciona uma política CORS denominada "AllowSpecificOrigin".</span><span class="sxs-lookup"><span data-stu-id="ed60e-138">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="ed60e-139">Para selecionar a política, passe o nome para `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="ed60e-139">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="ed60e-140">Habilitando CORS no MVC</span><span class="sxs-lookup"><span data-stu-id="ed60e-140">Enabling CORS in MVC</span></span>

<span data-ttu-id="ed60e-141">Como alternativa, você pode usar o MVC para aplicar um CORS específico por ação, por controlador ou globalmente para todos os controladores.</span><span class="sxs-lookup"><span data-stu-id="ed60e-141">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="ed60e-142">Ao usar o MVC para habilitar o CORS os mesmos serviços CORS são usados, mas não é o middleware do CORS.</span><span class="sxs-lookup"><span data-stu-id="ed60e-142">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="ed60e-143">Por ação</span><span class="sxs-lookup"><span data-stu-id="ed60e-143">Per action</span></span>

<span data-ttu-id="ed60e-144">Para especificar uma política CORS para uma ação específica, adicione o `[EnableCors]` atributo à ação.</span><span class="sxs-lookup"><span data-stu-id="ed60e-144">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="ed60e-145">Especifique o nome da política.</span><span class="sxs-lookup"><span data-stu-id="ed60e-145">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="ed60e-146">Por controlador</span><span class="sxs-lookup"><span data-stu-id="ed60e-146">Per controller</span></span>

<span data-ttu-id="ed60e-147">Para especificar a política CORS para um controlador específico, adicione o `[EnableCors]` atributo à classe do controlador.</span><span class="sxs-lookup"><span data-stu-id="ed60e-147">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="ed60e-148">Especifique o nome da política.</span><span class="sxs-lookup"><span data-stu-id="ed60e-148">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="ed60e-149">Globalmente</span><span class="sxs-lookup"><span data-stu-id="ed60e-149">Globally</span></span>

<span data-ttu-id="ed60e-150">Você pode habilitar o CORS globalmente para todos os controladores, adicionando o `CorsAuthorizationFilterFactory` filtro à coleção de filtros globais:</span><span class="sxs-lookup"><span data-stu-id="ed60e-150">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="ed60e-151">A ordem de precedência é: ação, controlador, global.</span><span class="sxs-lookup"><span data-stu-id="ed60e-151">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="ed60e-152">As políticas no nível de ação têm precedência sobre as políticas no nível do controlador e políticas no nível do controlador têm precedência sobre as políticas globais.</span><span class="sxs-lookup"><span data-stu-id="ed60e-152">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="ed60e-153">Desabilitar o CORS</span><span class="sxs-lookup"><span data-stu-id="ed60e-153">Disable CORS</span></span>

<span data-ttu-id="ed60e-154">Para desabilitar CORS para um controlador ou ação, use o `[DisableCors]` atributo.</span><span class="sxs-lookup"><span data-stu-id="ed60e-154">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="ed60e-155">Opções de política de CORS</span><span class="sxs-lookup"><span data-stu-id="ed60e-155">CORS policy options</span></span>

<span data-ttu-id="ed60e-156">Esta seção descreve as várias opções que podem ser definidas em uma política CORS.</span><span class="sxs-lookup"><span data-stu-id="ed60e-156">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="ed60e-157">Defina as origens permitidas</span><span class="sxs-lookup"><span data-stu-id="ed60e-157">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="ed60e-158">Defina os métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="ed60e-158">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="ed60e-159">Definir os cabeçalhos de solicitação permitido</span><span class="sxs-lookup"><span data-stu-id="ed60e-159">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="ed60e-160">Definir os cabeçalhos de resposta exposto</span><span class="sxs-lookup"><span data-stu-id="ed60e-160">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="ed60e-161">Credenciais nas solicitações entre origens</span><span class="sxs-lookup"><span data-stu-id="ed60e-161">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="ed60e-162">Definir o tempo de expiração de simulação</span><span class="sxs-lookup"><span data-stu-id="ed60e-162">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="ed60e-163">Para algumas opções, pode ser útil ler [funciona como o CORS](#how-cors-works) primeiro.</span><span class="sxs-lookup"><span data-stu-id="ed60e-163">For some options, it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="ed60e-164">Defina as origens permitidas</span><span class="sxs-lookup"><span data-stu-id="ed60e-164">Set the allowed origins</span></span>

<span data-ttu-id="ed60e-165">Para permitir que um ou mais origens específicas:</span><span class="sxs-lookup"><span data-stu-id="ed60e-165">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="ed60e-166">Para permitir que todas as origens:</span><span class="sxs-lookup"><span data-stu-id="ed60e-166">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="ed60e-167">Considere cuidadosamente antes de permitir que solicitações de qualquer origem.</span><span class="sxs-lookup"><span data-stu-id="ed60e-167">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="ed60e-168">Isso significa que, literalmente, qualquer site possa fazer chamadas AJAX à API.</span><span class="sxs-lookup"><span data-stu-id="ed60e-168">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="ed60e-169">Defina os métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="ed60e-169">Set the allowed HTTP methods</span></span>

<span data-ttu-id="ed60e-170">Para permitir que todos os métodos HTTP:</span><span class="sxs-lookup"><span data-stu-id="ed60e-170">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="ed60e-171">Isso afeta solicitações preliminares e cabeçalho Access-Control-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="ed60e-171">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="ed60e-172">Definir os cabeçalhos de solicitação permitido</span><span class="sxs-lookup"><span data-stu-id="ed60e-172">Set the allowed request headers</span></span>

<span data-ttu-id="ed60e-173">Uma solicitação de simulação de CORS pode incluir um cabeçalho Access-Control-Request-Headers, listando os cabeçalhos HTTP definidos pelo aplicativo (os chamados "author cabeçalhos de solicitação").</span><span class="sxs-lookup"><span data-stu-id="ed60e-173">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="ed60e-174">Para cabeçalhos específicos de lista de permissões:</span><span class="sxs-lookup"><span data-stu-id="ed60e-174">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="ed60e-175">Para permitir que todos os cabeçalhos de solicitação do autor:</span><span class="sxs-lookup"><span data-stu-id="ed60e-175">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="ed60e-176">Navegadores não estão totalmente consistentes em como eles definir Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="ed60e-176">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="ed60e-177">Se você definir os cabeçalhos para algo diferente de "\*", você deve incluir pelo menos "accept", "content-type" e "origem", além de quaisquer cabeçalhos personalizados que você deseja dar suporte.</span><span class="sxs-lookup"><span data-stu-id="ed60e-177">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="ed60e-178">Definir os cabeçalhos de resposta exposto</span><span class="sxs-lookup"><span data-stu-id="ed60e-178">Set the exposed response headers</span></span>

<span data-ttu-id="ed60e-179">Por padrão, o navegador não expõe todos os cabeçalhos de resposta para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ed60e-179">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="ed60e-180">(Consulte [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Os cabeçalhos de resposta que estão disponíveis por padrão são:</span><span class="sxs-lookup"><span data-stu-id="ed60e-180">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="ed60e-181">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="ed60e-181">Cache-Control</span></span>

* <span data-ttu-id="ed60e-182">Idioma do conteúdo</span><span class="sxs-lookup"><span data-stu-id="ed60e-182">Content-Language</span></span>

* <span data-ttu-id="ed60e-183">Tipo de conteúdo</span><span class="sxs-lookup"><span data-stu-id="ed60e-183">Content-Type</span></span>

* <span data-ttu-id="ed60e-184">Expira</span><span class="sxs-lookup"><span data-stu-id="ed60e-184">Expires</span></span>

* <span data-ttu-id="ed60e-185">Última modificação</span><span class="sxs-lookup"><span data-stu-id="ed60e-185">Last-Modified</span></span>

* <span data-ttu-id="ed60e-186">Pragma</span><span class="sxs-lookup"><span data-stu-id="ed60e-186">Pragma</span></span>

<span data-ttu-id="ed60e-187">A especificação CORS chama esses *cabeçalhos de resposta simples*.</span><span class="sxs-lookup"><span data-stu-id="ed60e-187">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="ed60e-188">Para disponibilizar outros cabeçalhos para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="ed60e-188">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="ed60e-189">Credenciais nas solicitações entre origens</span><span class="sxs-lookup"><span data-stu-id="ed60e-189">Credentials in cross-origin requests</span></span>

<span data-ttu-id="ed60e-190">Credenciais exigem tratamento especial em uma solicitação CORS.</span><span class="sxs-lookup"><span data-stu-id="ed60e-190">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="ed60e-191">Por padrão, o navegador não envia todas as credenciais com uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="ed60e-191">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="ed60e-192">As credenciais incluem cookies, bem como os esquemas de autenticação HTTP.</span><span class="sxs-lookup"><span data-stu-id="ed60e-192">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="ed60e-193">Para enviar as credenciais com uma solicitação entre origens, o cliente deve definir XMLHttpRequest.withCredentials como true.</span><span class="sxs-lookup"><span data-stu-id="ed60e-193">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="ed60e-194">Usando XMLHttpRequest diretamente:</span><span class="sxs-lookup"><span data-stu-id="ed60e-194">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="ed60e-195">No jQuery:</span><span class="sxs-lookup"><span data-stu-id="ed60e-195">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="ed60e-196">Além disso, o servidor deve permitir que as credenciais.</span><span class="sxs-lookup"><span data-stu-id="ed60e-196">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="ed60e-197">Para permitir credenciais entre origens:</span><span class="sxs-lookup"><span data-stu-id="ed60e-197">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="ed60e-198">Agora, a resposta HTTP incluirá um cabeçalho Access-Control-Allow-Credentials, que informa ao navegador que o servidor permite que as credenciais para uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="ed60e-198">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="ed60e-199">Se o navegador envia as credenciais, mas a resposta não inclui um cabeçalho Access-Control-Allow-Credentials válido, o navegador não expõe a resposta para o aplicativo e a solicitação AJAX falhar.</span><span class="sxs-lookup"><span data-stu-id="ed60e-199">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="ed60e-200">Tenha cuidado ao permitindo credenciais entre origens.</span><span class="sxs-lookup"><span data-stu-id="ed60e-200">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="ed60e-201">Um site em outro domínio pode enviar credenciais do usuário conectado para o aplicativo em nome do usuário sem o conhecimento do usuário.</span><span class="sxs-lookup"><span data-stu-id="ed60e-201">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="ed60e-202">A especificação CORS também declara que a configuração origens `"*"` (todas as origens) é inválida se o `Access-Control-Allow-Credentials` cabeçalho está presente.</span><span class="sxs-lookup"><span data-stu-id="ed60e-202">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="ed60e-203">Definir o tempo de expiração de simulação</span><span class="sxs-lookup"><span data-stu-id="ed60e-203">Set the preflight expiration time</span></span>

<span data-ttu-id="ed60e-204">O cabeçalho Access-Control-Max-Age Especifica quanto tempo a resposta à solicitação de simulação pode ser armazenados em cache.</span><span class="sxs-lookup"><span data-stu-id="ed60e-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="ed60e-205">Para definir esse cabeçalho:</span><span class="sxs-lookup"><span data-stu-id="ed60e-205">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="ed60e-206">Como funciona o CORS</span><span class="sxs-lookup"><span data-stu-id="ed60e-206">How CORS works</span></span>

<span data-ttu-id="ed60e-207">Esta seção descreve o que acontece em uma solicitação CORS no nível das mensagens HTTP.</span><span class="sxs-lookup"><span data-stu-id="ed60e-207">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="ed60e-208">É importante entender o funcionamento de CORS para que a política CORS pode ser configurada corretamente e depurada quando comportamentos inesperados ocorrerem.</span><span class="sxs-lookup"><span data-stu-id="ed60e-208">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="ed60e-209">A especificação CORS apresenta vários novos cabeçalhos HTTP que permitem que solicitações entre origens.</span><span class="sxs-lookup"><span data-stu-id="ed60e-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="ed60e-210">Se um navegador oferece suporte a CORS, ele define esses cabeçalhos automaticamente para solicitações entre origens.</span><span class="sxs-lookup"><span data-stu-id="ed60e-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="ed60e-211">Código JavaScript personalizado não é necessário para habilitar o CORS.</span><span class="sxs-lookup"><span data-stu-id="ed60e-211">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="ed60e-212">Aqui está um exemplo de uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="ed60e-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="ed60e-213">O `Origin` cabeçalho fornece o domínio do site que está fazendo a solicitação:</span><span class="sxs-lookup"><span data-stu-id="ed60e-213">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="ed60e-214">Se o servidor permite que a solicitação, ele define o cabeçalho Access-Control-Allow-Origin na resposta.</span><span class="sxs-lookup"><span data-stu-id="ed60e-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="ed60e-215">O valor desse cabeçalho corresponde ao cabeçalho da origem da solicitação, ou é o valor de curinga "\*", o que significa que qualquer origem é permitida:</span><span class="sxs-lookup"><span data-stu-id="ed60e-215">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="ed60e-216">Se a resposta não incluir o cabeçalho Access-Control-Allow-Origin, a solicitação AJAX falha.</span><span class="sxs-lookup"><span data-stu-id="ed60e-216">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="ed60e-217">Especificamente, o navegador não permite a solicitação.</span><span class="sxs-lookup"><span data-stu-id="ed60e-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="ed60e-218">Mesmo se o servidor retorna uma resposta bem-sucedida, o navegador não disponibiliza a resposta para o aplicativo cliente.</span><span class="sxs-lookup"><span data-stu-id="ed60e-218">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="ed60e-219">Solicitações de simulação</span><span class="sxs-lookup"><span data-stu-id="ed60e-219">Preflight Requests</span></span>

<span data-ttu-id="ed60e-220">Para algumas solicitações CORS, o navegador envia uma solicitação adicional, chamada de uma "solicitação de simulação," antes de enviar a solicitação real para o recurso.</span><span class="sxs-lookup"><span data-stu-id="ed60e-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="ed60e-221">O navegador pode ignorar a solicitação de simulação se as seguintes condições forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="ed60e-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="ed60e-222">O método de solicitação é GET, HEAD ou POST, e</span><span class="sxs-lookup"><span data-stu-id="ed60e-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="ed60e-223">O aplicativo não definir quaisquer cabeçalhos de solicitação que não seja Accept, Accept-Language, Content-Language, Content-Type ou última--ID do evento, e</span><span class="sxs-lookup"><span data-stu-id="ed60e-223">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="ed60e-224">O cabeçalho Content-Type (se definido) é um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="ed60e-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="ed60e-225">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="ed60e-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="ed60e-226">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="ed60e-226">multipart/form-data</span></span>

  * <span data-ttu-id="ed60e-227">texto/simples</span><span class="sxs-lookup"><span data-stu-id="ed60e-227">text/plain</span></span>

<span data-ttu-id="ed60e-228">A regra sobre cabeçalhos de solicitação se aplica aos cabeçalhos que o aplicativo define chamando setRequestHeader no objeto XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="ed60e-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="ed60e-229">(A especificação CORS chama esses cabeçalhos de solicitação"autor".) A regra não se aplica aos cabeçalhos, que o navegador pode definir, como o agente do usuário, o Host ou o comprimento do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="ed60e-229">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="ed60e-230">Aqui está um exemplo de uma solicitação de simulação:</span><span class="sxs-lookup"><span data-stu-id="ed60e-230">Here is an example of a preflight request:</span></span>

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="ed60e-231">A solicitação de simulação usa o método HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="ed60e-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="ed60e-232">Ele inclui dois cabeçalhos especiais:</span><span class="sxs-lookup"><span data-stu-id="ed60e-232">It includes two special headers:</span></span>

* <span data-ttu-id="ed60e-233">Access-Control-Request-Method: O método HTTP que será usado para a solicitação real.</span><span class="sxs-lookup"><span data-stu-id="ed60e-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="ed60e-234">Access-Control-Request-Headers: Uma lista de cabeçalhos de solicitação que o aplicativo definido na solicitação real.</span><span class="sxs-lookup"><span data-stu-id="ed60e-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="ed60e-235">(Novamente, isso não inclui os cabeçalhos que define o navegador.)</span><span class="sxs-lookup"><span data-stu-id="ed60e-235">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="ed60e-236">Aqui está um exemplo de resposta, supondo que o servidor permite que a solicitação:</span><span class="sxs-lookup"><span data-stu-id="ed60e-236">Here is an example response, assuming that the server allows the request:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="ed60e-237">A resposta inclui um cabeçalho Access-Control-Allow-Methods que lista os métodos permitidos e, opcionalmente, um cabeçalho Access-Control-Allow-Headers, que lista os cabeçalhos permitidos.</span><span class="sxs-lookup"><span data-stu-id="ed60e-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="ed60e-238">Se a solicitação de simulação for bem-sucedida, o navegador envia a solicitação real, conforme descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="ed60e-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
