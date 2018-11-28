---
title: Habilitar solicitações entre origens (CORS) no ASP.NET Core
author: rick-anderson
description: Saiba como CORS como padrão para permitir ou rejeitar solicitações entre origens em um aplicativo ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2018
uid: security/cors
ms.openlocfilehash: f0e01cfa618184d8a3b19c06212dc3914183a2e4
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458537"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="fdaa9-103">Habilitar solicitações entre origens (CORS) no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fdaa9-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="fdaa9-104">Por [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="fdaa9-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="fdaa9-105">Segurança do navegador impede que uma página da web fazendo solicitações para um domínio diferente daquele que atendia a página da web.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="fdaa9-106">Essa restrição é chamada de *política de mesma origem*.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="fdaa9-107">A política de mesma origem impede que um site mal-intencionado leia dados confidenciais de outro site.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="fdaa9-108">Às vezes, você talvez queira permitir que outros sites fazem solicitações entre origens em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="fdaa9-109">[Entre o compartilhamento de recursos de origem](https://www.w3.org/TR/cors/) (CORS) é um padrão W3C que permite que um servidor relaxar a política de mesma origem.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="fdaa9-110">Usando o CORS, um servidor pode explicitamente permitir algumas solicitações entre origens enquanto rejeita outras.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="fdaa9-111">O CORS é mais seguro e flexível que técnicas anteriores, tais como [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="fdaa9-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="fdaa9-112">Este tópico mostra como habilitar o CORS em um aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="fdaa9-113">Mesma origem</span><span class="sxs-lookup"><span data-stu-id="fdaa9-113">Same origin</span></span>

<span data-ttu-id="fdaa9-114">Duas URLs têm a mesma origem se eles têm esquemas idênticas, hosts e portas ([6454 RFC](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="fdaa9-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="fdaa9-115">Essas duas URLs têm a mesma origem:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="fdaa9-116">Essas URLs têm diferentes origens que as duas URLs anteriores:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="fdaa9-117">`https://example.net` &ndash; Domínio diferente</span><span class="sxs-lookup"><span data-stu-id="fdaa9-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="fdaa9-118">`https://www.example.com/foo.html` &ndash; Subdomínio diferente</span><span class="sxs-lookup"><span data-stu-id="fdaa9-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="fdaa9-119">`http://example.com/foo.html` &ndash; Esquema diferente</span><span class="sxs-lookup"><span data-stu-id="fdaa9-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="fdaa9-120">`https://example.com:9000/foo.html` &ndash; Porta diferente</span><span class="sxs-lookup"><span data-stu-id="fdaa9-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="fdaa9-121">Internet Explorer não considera a porta ao comparar as origens.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="fdaa9-122">Registrar os serviços do CORS</span><span class="sxs-lookup"><span data-stu-id="fdaa9-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="fdaa9-123">Referência a [metapacote do Microsoft](xref:fundamentals/metapackage-app) ou adicionar uma referência de pacote para o [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pacote.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="fdaa9-124">Referência a [metapacote Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ou adicionar uma referência de pacote para o [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pacote.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fdaa9-125">Adicionar uma referência de pacote para o [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) pacote.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="fdaa9-126">Chame <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> em `Startup.ConfigureServices` para adicionar serviços do CORS ao contêiner de serviço do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="fdaa9-127">Habilitar o CORS</span><span class="sxs-lookup"><span data-stu-id="fdaa9-127">Enable CORS</span></span>

<span data-ttu-id="fdaa9-128">Depois de registrar os serviços do CORS, use qualquer uma das abordagens a seguir para habilitar o CORS em um aplicativo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="fdaa9-129">[Middleware do CORS](#enable-cors-with-cors-middleware) &ndash; políticas CORS se aplicam globalmente para o aplicativo por meio do middleware.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="fdaa9-130">[O CORS no MVC](#enable-cors-in-mvc) &ndash; políticas aplicar CORS por controlador ou por ação.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="fdaa9-131">Middleware CORS não é usada.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="fdaa9-132">Habilitar o CORS com Middleware CORS</span><span class="sxs-lookup"><span data-stu-id="fdaa9-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="fdaa9-133">Middleware CORS manipula solicitações entre origens para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="fdaa9-134">Para habilitar o CORS Middleware no pipeline de processamento de solicitação, chame o <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> método de extensão no `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="fdaa9-135">Middleware CORS devem preceder quaisquer pontos de extremidade definidos em seu aplicativo onde você deseja dar suporte a solicitações entre origens (por exemplo, antes de chamar `UseMvc` de Middleware de páginas do Razor/MVC).</span><span class="sxs-lookup"><span data-stu-id="fdaa9-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="fdaa9-136">Um *política entre origens* pode ser especificado ao adicionar o Middleware CORS usando o <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> classe.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="fdaa9-137">Há duas abordagens para definir uma política CORS:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="fdaa9-138">Chamar `UseCors` com um lambda:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="fdaa9-139">O lambda utiliza um <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> objeto.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="fdaa9-140">[Opções de configuração](#cors-policy-options), tais como `WithOrigins`, são descritas posteriormente neste tópico.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="fdaa9-141">No exemplo anterior, a política permite que solicitações entre origens de `https://example.com` e sem outras origens.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="fdaa9-142">A URL deve ser especificada sem uma barra à direita (`/`).</span><span class="sxs-lookup"><span data-stu-id="fdaa9-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="fdaa9-143">Se a URL termina com `/`, a comparação retorna `false` e nenhum cabeçalho é retornado.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="fdaa9-144">`CorsPolicyBuilder` tem uma API fluente, portanto, é possível encadear chamadas de método:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="fdaa9-145">Definir uma ou mais políticas CORS e selecione a política por nome em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="fdaa9-146">O exemplo a seguir adiciona uma política CORS definida pelo usuário chamada *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="fdaa9-147">Para selecionar a política, passe o nome para `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="fdaa9-148">Habilitar o CORS no MVC</span><span class="sxs-lookup"><span data-stu-id="fdaa9-148">Enable CORS in MVC</span></span>

<span data-ttu-id="fdaa9-149">Como alternativa, você pode usar o MVC para aplicar políticas CORS específicas por ação ou por controlador.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="fdaa9-150">Ao usar o MVC para habilitar o CORS, os serviços registrados de CORS são usados.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="fdaa9-151">O Middleware do CORS não é usado.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="fdaa9-152">Por ação</span><span class="sxs-lookup"><span data-stu-id="fdaa9-152">Per action</span></span>

<span data-ttu-id="fdaa9-153">Para especificar uma política CORS para uma ação específica, adicione a [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atributo à ação.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="fdaa9-154">Especifique o nome da política.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="fdaa9-155">Por controlador</span><span class="sxs-lookup"><span data-stu-id="fdaa9-155">Per controller</span></span>

<span data-ttu-id="fdaa9-156">Para especificar a política CORS para um controlador específico, adicione a [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) atributo à classe do controlador.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="fdaa9-157">Especifique o nome da política.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="fdaa9-158">A ordem de precedência é:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-158">The precedence order is:</span></span>

1. <span data-ttu-id="fdaa9-159">ação</span><span class="sxs-lookup"><span data-stu-id="fdaa9-159">action</span></span>
1. <span data-ttu-id="fdaa9-160">controlador</span><span class="sxs-lookup"><span data-stu-id="fdaa9-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="fdaa9-161">Desabilitar o CORS</span><span class="sxs-lookup"><span data-stu-id="fdaa9-161">Disable CORS</span></span>

<span data-ttu-id="fdaa9-162">Para desabilitar CORS para um controlador ou ação, use o [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) atributo:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="fdaa9-163">Opções de política de CORS</span><span class="sxs-lookup"><span data-stu-id="fdaa9-163">CORS policy options</span></span>

<span data-ttu-id="fdaa9-164">Esta seção descreve as várias opções que podem ser definidas em uma política CORS.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-164">This section describes the various options that you can set in a CORS policy.</span></span> <span data-ttu-id="fdaa9-165">O <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> método é chamado no `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-165">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> method is called in `Startup.ConfigureServices`.</span></span>

* [<span data-ttu-id="fdaa9-166">Defina as origens permitidas</span><span class="sxs-lookup"><span data-stu-id="fdaa9-166">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="fdaa9-167">Defina os métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="fdaa9-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="fdaa9-168">Definir os cabeçalhos de solicitação permitido</span><span class="sxs-lookup"><span data-stu-id="fdaa9-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="fdaa9-169">Definir os cabeçalhos de resposta exposto</span><span class="sxs-lookup"><span data-stu-id="fdaa9-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="fdaa9-170">Credenciais nas solicitações entre origens</span><span class="sxs-lookup"><span data-stu-id="fdaa9-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="fdaa9-171">Definir o tempo de expiração de simulação</span><span class="sxs-lookup"><span data-stu-id="fdaa9-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="fdaa9-172">Para algumas opções, pode ser útil ler o [funciona como o CORS](#how-cors-works) seção pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-172">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="fdaa9-173">Defina as origens permitidas</span><span class="sxs-lookup"><span data-stu-id="fdaa9-173">Set the allowed origins</span></span>

<span data-ttu-id="fdaa9-174">O middleware do CORS no ASP.NET Core MVC tem algumas maneiras de especificar origens permitidas:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-174">The CORS middleware in ASP.NET Core MVC has a few ways to specify allowed origins:</span></span>

* <span data-ttu-id="fdaa9-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; Permite especificar uma ou mais URLs.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; Allows specifying one or more URLs.</span></span> <span data-ttu-id="fdaa9-176">A URL pode incluir o esquema, nome de host e porta sem nenhuma informação de caminho.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-176">The URL may include the scheme, host name, and port without any path information.</span></span> <span data-ttu-id="fdaa9-177">Por exemplo, `https://example.com`.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-177">For example, `https://example.com`.</span></span> <span data-ttu-id="fdaa9-178">A URL deve ser especificada sem uma barra à direita (`/`).</span><span class="sxs-lookup"><span data-stu-id="fdaa9-178">The URL must be specified without a trailing slash (`/`).</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-25&highlight=4-5)]

* <span data-ttu-id="fdaa9-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Permite que as solicitações CORS de todas as origens com qualquer esquema (`http` ou `https`).</span><span class="sxs-lookup"><span data-stu-id="fdaa9-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=29-33&highlight=4)]

  <span data-ttu-id="fdaa9-180">Considere cuidadosamente antes de permitir que solicitações de qualquer origem.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-180">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="fdaa9-181">Permitir solicitações de qualquer origem significa que *de qualquer site* pode fazer solicitações entre origens ao seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-181">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="fdaa9-182">Especificando `AllowAnyOrigin` e `AllowCredentials` é uma configuração insegura e podem resultar em falsificação de solicitação entre sites.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-182">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="fdaa9-183">O serviço CORS retorna uma resposta inválida do CORS, quando um aplicativo é configurado com os dois métodos.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-183">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="fdaa9-184">Especificando `AllowAnyOrigin` e `AllowCredentials` é uma configuração insegura e podem resultar em falsificação de solicitação entre sites.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-184">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="fdaa9-185">Considere a especificação de uma lista exata de origens se o cliente deve autorizar a mesmo para acessar recursos do servidor.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-185">Consider specifying an exact list of origins if the client must authorize itself to access server resources.</span></span>

  ::: moniker-end

  <span data-ttu-id="fdaa9-186">Essa configuração afeta as solicitações de simulação e o `Access-Control-Allow-Origin` cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-186">This setting affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="fdaa9-187">Para obter mais informações, consulte o [solicitações de simulação](#preflight-requests) seção.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-187">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="fdaa9-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Conjuntos de <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> propriedade da política para ser uma função que permite origens corresponder a um domínio de curinga configurado ao avaliar se a origem é permitida.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcarded domain when evaluating if the origin is allowed.</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="fdaa9-189">Defina os métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="fdaa9-189">Set the allowed HTTP methods</span></span>

<span data-ttu-id="fdaa9-190">Para permitir que todos os métodos HTTP, chamar <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-190">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=46-51&highlight=5)]

<span data-ttu-id="fdaa9-191">Essa configuração afeta as solicitações de simulação e o `Access-Control-Allow-Methods` cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-191">This setting affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="fdaa9-192">Para obter mais informações, consulte o [solicitações de simulação](#preflight-requests) seção.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-192">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="fdaa9-193">Definir os cabeçalhos de solicitação permitido</span><span class="sxs-lookup"><span data-stu-id="fdaa9-193">Set the allowed request headers</span></span>

<span data-ttu-id="fdaa9-194">Para permitir que os cabeçalhos específicos a serem enviados em uma solicitação CORS, chamado *criar cabeçalhos de solicitação*, chame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> e especificar os cabeçalhos permitidos:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-194">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="fdaa9-195">Para permitir que todos os cabeçalhos de solicitação do autor chamar <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-195">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="fdaa9-196">Essa configuração afeta as solicitações de simulação e o `Access-Control-Request-Headers` cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-196">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="fdaa9-197">Para obter mais informações, consulte o [solicitações de simulação](#preflight-requests) seção.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-197">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fdaa9-198">Uma correspondência de política de Middleware do CORS para cabeçalhos específicos especificado por `WithHeaders` só é possível quando os cabeçalhos enviados `Access-Control-Request-Headers` coincidir exatamente com os cabeçalhos indicados na `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-198">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="fdaa9-199">Por exemplo, considere um aplicativo configurado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-199">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="fdaa9-200">Middleware CORS recusar uma solicitação de simulação com o seguinte cabeçalho de solicitação, pois `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) não está listado no `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-200">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="fdaa9-201">O aplicativo retorna um *200 Okey* resposta, mas não envia de volta os cabeçalhos de CORS.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-201">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="fdaa9-202">Portanto, o navegador não tente a solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-202">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="fdaa9-203">Middleware CORS sempre permite que os cabeçalhos de quatro no `Access-Control-Request-Headers` a ser enviado, independentemente dos valores configurados no CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-203">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="fdaa9-204">Esta lista de cabeçalhos inclui:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-204">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="fdaa9-205">Por exemplo, considere um aplicativo configurado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-205">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="fdaa9-206">Middleware CORS responde com êxito a uma solicitação de simulação com o seguinte cabeçalho de solicitação porque `Content-Language` é sempre na lista de permissões:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-206">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="fdaa9-207">Definir os cabeçalhos de resposta exposto</span><span class="sxs-lookup"><span data-stu-id="fdaa9-207">Set the exposed response headers</span></span>

<span data-ttu-id="fdaa9-208">Por padrão, o navegador não expõe todos os cabeçalhos de resposta para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-208">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="fdaa9-209">Para obter mais informações, consulte [W3C Cross-Origin Resource Sharing (terminologia): o cabeçalho de resposta simples](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="fdaa9-209">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="fdaa9-210">Os cabeçalhos de resposta que estão disponíveis por padrão são:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-210">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="fdaa9-211">A especificação CORS chama esses cabeçalhos *cabeçalhos de resposta simples*.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-211">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="fdaa9-212">Para disponibilizar outros cabeçalhos para o aplicativo, chame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-212">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="fdaa9-213">Credenciais nas solicitações entre origens</span><span class="sxs-lookup"><span data-stu-id="fdaa9-213">Credentials in cross-origin requests</span></span>

<span data-ttu-id="fdaa9-214">Credenciais exigem tratamento especial em uma solicitação CORS.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-214">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="fdaa9-215">Por padrão, o navegador não envia as credenciais com uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-215">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="fdaa9-216">As credenciais incluem cookies e esquemas de autenticação HTTP.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-216">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="fdaa9-217">Para enviar as credenciais com uma solicitação entre origens, o cliente deve definir `XMLHttpRequest.withCredentials` para `true`.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-217">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="fdaa9-218">Usando `XMLHttpRequest` diretamente:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-218">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="fdaa9-219">No jQuery:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-219">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="fdaa9-220">Além disso, o servidor deve permitir que as credenciais.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-220">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="fdaa9-221">Para permitir credenciais entre origens, chamar <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-221">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="fdaa9-222">A resposta HTTP inclui um `Access-Control-Allow-Credentials` cabeçalho, que informa ao navegador que o servidor permite que as credenciais para uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-222">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="fdaa9-223">Se o navegador envia as credenciais, mas a resposta não inclui um válido `Access-Control-Allow-Credentials` cabeçalho, o navegador não expõe a resposta para o aplicativo e a solicitação entre origens falhar.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-223">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="fdaa9-224">Tenha cuidado ao permitindo credenciais entre origens.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-224">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="fdaa9-225">Um site em outro domínio pode enviar credenciais do usuário conectado para o aplicativo em nome do usuário sem o conhecimento do usuário.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-225">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="fdaa9-226">A especificação CORS também declara que a configuração origens `"*"` (todas as origens) é inválida se o `Access-Control-Allow-Credentials` cabeçalho está presente.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-226">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="fdaa9-227">Solicitações de simulação</span><span class="sxs-lookup"><span data-stu-id="fdaa9-227">Preflight requests</span></span>

<span data-ttu-id="fdaa9-228">Para algumas solicitações CORS, o navegador envia uma solicitação adicional antes de fazer a solicitação real.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-228">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="fdaa9-229">Essa solicitação é chamada de um *solicitação de simulação*.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-229">This request is called a *preflight request*.</span></span> <span data-ttu-id="fdaa9-230">O navegador pode ignorar a solicitação de simulação se as seguintes condições forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-230">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="fdaa9-231">O método de solicitação é GET, HEAD ou POST.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-231">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="fdaa9-232">O aplicativo não define cabeçalhos de solicitação diferente de `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, ou `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-232">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="fdaa9-233">O `Content-Type` cabeçalho, se definido, tem um dos seguintes valores:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-233">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="fdaa9-234">A regra em cabeçalhos de solicitação definido para a solicitação do cliente se aplica aos cabeçalhos que o aplicativo define chamando `setRequestHeader` sobre o `XMLHttpRequest` objeto.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-234">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="fdaa9-235">A especificação CORS chama esses cabeçalhos *criar cabeçalhos de solicitação*.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-235">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="fdaa9-236">A regra não se aplica aos cabeçalhos, o navegador pode definir, tais como `User-Agent`, `Host`, ou `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-236">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="fdaa9-237">Este é um exemplo de uma solicitação de simulação:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-237">The following is an example of a preflight request:</span></span>

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="fdaa9-238">A solicitação de simulação usa o método HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-238">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="fdaa9-239">Ele inclui dois cabeçalhos especiais:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-239">It includes two special headers:</span></span>

* <span data-ttu-id="fdaa9-240">`Access-Control-Request-Method`: O método HTTP que será usado para a solicitação real.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-240">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="fdaa9-241">`Access-Control-Request-Headers`: Uma lista de cabeçalhos de solicitação que o aplicativo define a solicitação real.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-241">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="fdaa9-242">Conforme mencionado anteriormente, isso não inclui os cabeçalhos que define o navegador, tais como `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-242">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="fdaa9-243">Uma solicitação de simulação de CORS pode incluir um `Access-Control-Request-Headers` cabeçalho, que indica ao servidor, os cabeçalhos que são enviados com a solicitação real.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-243">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="fdaa9-244">Para permitir que os cabeçalhos específicos, chame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-244">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="fdaa9-245">Para permitir que todos os cabeçalhos de solicitação do autor chamar <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-245">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="fdaa9-246">Navegadores não estão totalmente consistentes em como eles definir `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-246">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="fdaa9-247">Se você definir os cabeçalhos para algo diferente de `"*"` (ou use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), você deve incluir pelo menos `Accept`, `Content-Type`, e `Origin`, além de quaisquer cabeçalhos personalizados que você deseja dar suporte.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-247">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="fdaa9-248">Este é um exemplo de resposta à solicitação de simulação (supondo que o servidor permite que a solicitação):</span><span class="sxs-lookup"><span data-stu-id="fdaa9-248">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="fdaa9-249">A resposta inclui um `Access-Control-Allow-Methods` cabeçalho que lista os métodos permitidos e, opcionalmente, um `Access-Control-Allow-Headers` cabeçalho, que lista os cabeçalhos permitidos.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-249">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="fdaa9-250">Se a solicitação de simulação for bem-sucedida, o navegador envia a solicitação real.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-250">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="fdaa9-251">Se a solicitação de simulação for negada, o aplicativo retornar um *200 Okey* resposta, mas não envia de volta os cabeçalhos de CORS.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-251">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="fdaa9-252">Portanto, o navegador não tente a solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-252">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="fdaa9-253">Definir o tempo de expiração de simulação</span><span class="sxs-lookup"><span data-stu-id="fdaa9-253">Set the preflight expiration time</span></span>

<span data-ttu-id="fdaa9-254">O `Access-Control-Max-Age` cabeçalho Especifica quanto tempo a resposta à solicitação de simulação pode ser armazenados em cache.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-254">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="fdaa9-255">Para definir esse cabeçalho, chame <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-255">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="fdaa9-256">Como funciona o CORS</span><span class="sxs-lookup"><span data-stu-id="fdaa9-256">How CORS works</span></span>

<span data-ttu-id="fdaa9-257">Esta seção descreve o que acontece em uma solicitação CORS no nível das mensagens HTTP.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-257">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="fdaa9-258">É importante entender o funcionamento de CORS para que a política CORS pode ser configurada corretamente e depurada quando comportamentos inesperados ocorrerem.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-258">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="fdaa9-259">A especificação CORS apresenta vários novos cabeçalhos HTTP que permitem que solicitações entre origens.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-259">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="fdaa9-260">Se um navegador oferece suporte a CORS, ele define esses cabeçalhos automaticamente para solicitações entre origens.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-260">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="fdaa9-261">Código JavaScript personalizado não é necessário para habilitar o CORS.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-261">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="fdaa9-262">O exemplo a seguir é um exemplo de uma solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-262">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="fdaa9-263">O `Origin` cabeçalho fornece o domínio do site que está fazendo a solicitação:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-263">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="fdaa9-264">Se o servidor permite que a solicitação, ele define o `Access-Control-Allow-Origin` cabeçalho na resposta.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-264">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="fdaa9-265">O valor desse cabeçalho também corresponde a `Origin` cabeçalho da solicitação ou é o valor de curinga `"*"`, o que significa que qualquer origem é permitida:</span><span class="sxs-lookup"><span data-stu-id="fdaa9-265">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="fdaa9-266">Se a resposta não inclui o `Access-Control-Allow-Origin` falha do cabeçalho, a solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-266">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="fdaa9-267">Especificamente, o navegador não permite a solicitação.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-267">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="fdaa9-268">Mesmo se o servidor retorna uma resposta bem-sucedida, o navegador não disponibiliza a resposta para o aplicativo cliente.</span><span class="sxs-lookup"><span data-stu-id="fdaa9-268">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fdaa9-269">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="fdaa9-269">Additional resources</span></span>

* [<span data-ttu-id="fdaa9-270">Cross-Origin Resource Sharing (CORS)</span><span class="sxs-lookup"><span data-stu-id="fdaa9-270">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
