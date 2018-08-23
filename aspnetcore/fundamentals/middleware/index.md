---
title: Middleware do ASP.NET Core
author: rick-anderson
description: Saiba mais sobre o middleware do ASP.NET Core e o pipeline de solicitação.
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: 9ba77561ab4f6a8668c480d6e81f2ce7e0193c73
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870941"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="515f6-103">Middleware do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="515f6-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="515f6-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="515f6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="515f6-105">O middleware é um software montado em um pipeline de aplicativo para manipular solicitações e respostas.</span><span class="sxs-lookup"><span data-stu-id="515f6-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="515f6-106">Cada componente:</span><span class="sxs-lookup"><span data-stu-id="515f6-106">Each component:</span></span>

* <span data-ttu-id="515f6-107">Escolhe se deseja passar a solicitação para o próximo componente no pipeline.</span><span class="sxs-lookup"><span data-stu-id="515f6-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="515f6-108">Pode executar o trabalho antes e depois de o próximo componente no pipeline ser invocado.</span><span class="sxs-lookup"><span data-stu-id="515f6-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="515f6-109">Os delegados de solicitação são usados para criar o pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="515f6-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="515f6-110">Os delegados de solicitação manipulam cada solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="515f6-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="515f6-111">Os delegados de solicitação são configurados usando os métodos de extensão <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> e <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="515f6-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="515f6-112">Um delegado de solicitação individual pode ser especificado em linha como um método anônimo (chamado do middleware em linha) ou pode ser definido em uma classe reutilizável.</span><span class="sxs-lookup"><span data-stu-id="515f6-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="515f6-113">Essas classes reutilizáveis e os métodos anônimos em linha são o *middleware*, também chamado de *componentes do middleware*.</span><span class="sxs-lookup"><span data-stu-id="515f6-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="515f6-114">Cada componente de middleware no pipeline de solicitação é responsável por invocar o próximo componente no pipeline ou causar um curto-circuito do pipeline.</span><span class="sxs-lookup"><span data-stu-id="515f6-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="515f6-115"><xref:migration/http-modules> explica a diferença entre pipelines de solicitação no ASP.NET Core e no ASP.NET 4.x e fornece mais exemplos do middleware.</span><span class="sxs-lookup"><span data-stu-id="515f6-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="515f6-116">Criar um pipeline do middleware com o IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="515f6-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="515f6-117">O pipeline de solicitação do ASP.NET Core consiste em uma sequência de delegados de solicitação, chamados um após o outro.</span><span class="sxs-lookup"><span data-stu-id="515f6-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="515f6-118">O diagrama a seguir demonstra o conceito.</span><span class="sxs-lookup"><span data-stu-id="515f6-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="515f6-119">O thread de execução segue as setas pretas.</span><span class="sxs-lookup"><span data-stu-id="515f6-119">The thread of execution follows the black arrows.</span></span>

![Padrão de processamento de solicitação mostrando a chegada de uma solicitação, processada por meio de três middlewares e a resposta que sai do aplicativo.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="515f6-123">Cada delegado pode executar operações antes e depois do próximo delegado.</span><span class="sxs-lookup"><span data-stu-id="515f6-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="515f6-124">Um delegado também pode optar por não transmitir uma solicitação ao próximo delegado, o que também é chamado de *causar um curto-circuito do pipeline de solicitação*.</span><span class="sxs-lookup"><span data-stu-id="515f6-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="515f6-125">O curto-circuito geralmente é desejável porque ele evita trabalho desnecessário.</span><span class="sxs-lookup"><span data-stu-id="515f6-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="515f6-126">Por exemplo, o middleware de arquivos estáticos pode retornar uma solicitação para um arquivo estático e ligar o restante do pipeline em curto-circuito.</span><span class="sxs-lookup"><span data-stu-id="515f6-126">For example, Static Files Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="515f6-127">Os delegados de tratamento de exceção são chamados no início do pipeline para que possam detectar exceções que ocorrem em etapas posteriores do pipeline.</span><span class="sxs-lookup"><span data-stu-id="515f6-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="515f6-128">O aplicativo ASP.NET Core mais simples possível define um delegado de solicitação única que controla todas as solicitações.</span><span class="sxs-lookup"><span data-stu-id="515f6-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="515f6-129">Este caso não inclui um pipeline de solicitação real.</span><span class="sxs-lookup"><span data-stu-id="515f6-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="515f6-130">Em vez disso, uma única função anônima é chamada em resposta a cada solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="515f6-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="515f6-131">O primeiro delegado <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> encerra o pipeline.</span><span class="sxs-lookup"><span data-stu-id="515f6-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="515f6-132">Encadeie vários delegados de solicitação junto com o <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="515f6-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="515f6-133">O parâmetro `next` representa o próximo delegado no pipeline.</span><span class="sxs-lookup"><span data-stu-id="515f6-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="515f6-134">Lembre-se de que você pode causar um curto-circuito no pipeline ao *não* chamar o parâmetro *next*.</span><span class="sxs-lookup"><span data-stu-id="515f6-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="515f6-135">Normalmente, você pode executar ações antes e depois do próximo delegado, conforme o exemplo a seguir demonstra:</span><span class="sxs-lookup"><span data-stu-id="515f6-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="515f6-136">Não chame `next.Invoke` depois que a resposta tiver sido enviada ao cliente.</span><span class="sxs-lookup"><span data-stu-id="515f6-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="515f6-137">Altera para <xref:Microsoft.AspNetCore.Http.HttpResponse> depois de a resposta ser iniciada e lança uma exceção.</span><span class="sxs-lookup"><span data-stu-id="515f6-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="515f6-138">Por exemplo, mudanças como a configuração de cabeçalhos e o código de status lançam uma exceção.</span><span class="sxs-lookup"><span data-stu-id="515f6-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="515f6-139">Gravar no corpo da resposta após a chamada `next`:</span><span class="sxs-lookup"><span data-stu-id="515f6-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="515f6-140">Pode causar uma violação do protocolo.</span><span class="sxs-lookup"><span data-stu-id="515f6-140">May cause a protocol violation.</span></span> <span data-ttu-id="515f6-141">Por exemplo, gravar mais do que o `Content-Length` indicado.</span><span class="sxs-lookup"><span data-stu-id="515f6-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="515f6-142">Pode corromper o formato do corpo.</span><span class="sxs-lookup"><span data-stu-id="515f6-142">May corrupt the body format.</span></span> <span data-ttu-id="515f6-143">Por exemplo, gravar um rodapé HTML em um arquivo CSS.</span><span class="sxs-lookup"><span data-stu-id="515f6-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="515f6-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> é uma dica útil para indicar se os cabeçalhos foram enviados ou o corpo foi gravado.</span><span class="sxs-lookup"><span data-stu-id="515f6-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="515f6-145">Pedido</span><span class="sxs-lookup"><span data-stu-id="515f6-145">Order</span></span>

<span data-ttu-id="515f6-146">A ordem em que os componentes do middleware são adicionados ao método `Startup.Configure` define a ordem em que os componentes de middleware são invocados nas solicitações e a ordem inversa para a resposta.</span><span class="sxs-lookup"><span data-stu-id="515f6-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="515f6-147">A ordem é crítica para a segurança, o desempenho e a funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="515f6-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="515f6-148">O método `Configure` a seguir adiciona os seguintes componentes de middleware:</span><span class="sxs-lookup"><span data-stu-id="515f6-148">The following `Configure` method adds the following middleware components:</span></span>

1. <span data-ttu-id="515f6-149">Exceção/tratamento de erro</span><span class="sxs-lookup"><span data-stu-id="515f6-149">Exception/error handling</span></span>
2. <span data-ttu-id="515f6-150">Servidor de arquivos estático</span><span class="sxs-lookup"><span data-stu-id="515f6-150">Static file server</span></span>
3. <span data-ttu-id="515f6-151">Autenticação</span><span class="sxs-lookup"><span data-stu-id="515f6-151">Authentication</span></span>
4. <span data-ttu-id="515f6-152">MVC</span><span class="sxs-lookup"><span data-stu-id="515f6-152">MVC</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    //   Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="515f6-153">No código acima, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> é o primeiro componente de middleware adicionado ao pipeline.</span><span class="sxs-lookup"><span data-stu-id="515f6-153">In the code above, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="515f6-154">Portanto, o middleware de manipulador de exceção captura todas as exceções que ocorrem em chamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="515f6-154">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="515f6-155">O middleware de arquivos estáticos é chamado no início do pipeline para que possa controlar as solicitações e causar o curto-circuito sem passar pelos componentes restantes.</span><span class="sxs-lookup"><span data-stu-id="515f6-155">Static Files Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="515f6-156">O middleware de arquivos estáticos não fornece **nenhuma** verificação de autorização.</span><span class="sxs-lookup"><span data-stu-id="515f6-156">The Static Files Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="515f6-157">Todos os arquivos atendidos, incluindo aqueles em *wwwroot*, estão disponíveis publicamente.</span><span class="sxs-lookup"><span data-stu-id="515f6-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="515f6-158">Para conhecer uma abordagem para proteger arquivos estáticos, veja <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="515f6-158">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="515f6-159">Se a solicitação não for controlada pelo middleware de arquivos estáticos, ela será transmitida para o middleware de autenticação (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), que executa a autenticação.</span><span class="sxs-lookup"><span data-stu-id="515f6-159">If the request isn't handled by the Static Files Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="515f6-160">A autenticação causa curto-circuito em solicitações não autenticadas.</span><span class="sxs-lookup"><span data-stu-id="515f6-160">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="515f6-161">Embora o middleware de autenticação autentique as solicitações, a autorização (e a rejeição) ocorre somente depois que o MVC seleciona uma Página Razor específica ou um controlador MVC e uma ação.</span><span class="sxs-lookup"><span data-stu-id="515f6-161">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="515f6-162">Se a solicitação não for controlada pelo middleware de arquivos estáticos, ela será transmitida para o middleware de identidade (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), que executará a autenticação.</span><span class="sxs-lookup"><span data-stu-id="515f6-162">If the request isn't handled by Static Files Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="515f6-163">A identidade não liga as solicitações não autenticadas em curto-circuito.</span><span class="sxs-lookup"><span data-stu-id="515f6-163">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="515f6-164">Embora a identidade autentique as solicitações, a autorização (e a rejeição) ocorre somente depois que o MVC seleciona um controlador específico e uma ação.</span><span class="sxs-lookup"><span data-stu-id="515f6-164">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="515f6-165">O exemplo a seguir demonstra uma solicitação de middleware na qual as solicitações de arquivos estáticos são manipuladas pelo middleware de arquivo estático antes de o serem pelo middleware de compactação de resposta.</span><span class="sxs-lookup"><span data-stu-id="515f6-165">The following example demonstrates a middleware order where requests for static files are handled by Static Files Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="515f6-166">Arquivos estáticos não são compactados com este pedido de middleware.</span><span class="sxs-lookup"><span data-stu-id="515f6-166">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="515f6-167">As respostas do MVC de <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> podem ser compactadas.</span><span class="sxs-lookup"><span data-stu-id="515f6-167">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="515f6-168">Use, Run e Map</span><span class="sxs-lookup"><span data-stu-id="515f6-168">Use, Run, and Map</span></span>

<span data-ttu-id="515f6-169">Configure o pipeline de HTTP usando `Use`, `Run` e `Map`.</span><span class="sxs-lookup"><span data-stu-id="515f6-169">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="515f6-170">O método `Use` pode ligar o pipeline em curto-circuito (ou seja, se ele não chamar um delegado de solicitação `next`).</span><span class="sxs-lookup"><span data-stu-id="515f6-170">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="515f6-171">`Run` é uma convenção e alguns componentes de middleware podem expor os métodos `Run[Middleware]` que são executados no final do pipeline.</span><span class="sxs-lookup"><span data-stu-id="515f6-171">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="515f6-172">As extensões <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> são usadas como uma convenção de ramificação do pipeline.</span><span class="sxs-lookup"><span data-stu-id="515f6-172"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="515f6-173">`Map*` ramifica o pipeline de solicitação com base na correspondência do caminho da solicitação em questão.</span><span class="sxs-lookup"><span data-stu-id="515f6-173">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="515f6-174">Se o caminho da solicitação iniciar com o caminho especificado, o branch será executado.</span><span class="sxs-lookup"><span data-stu-id="515f6-174">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="515f6-175">A tabela a seguir mostra as solicitações e as respostas de `http://localhost:1234` usando o código anterior.</span><span class="sxs-lookup"><span data-stu-id="515f6-175">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="515f6-176">Solicitação</span><span class="sxs-lookup"><span data-stu-id="515f6-176">Request</span></span>             | <span data-ttu-id="515f6-177">Resposta</span><span class="sxs-lookup"><span data-stu-id="515f6-177">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="515f6-178">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="515f6-178">localhost:1234</span></span>      | <span data-ttu-id="515f6-179">Saudação do delegado diferente de Map.</span><span class="sxs-lookup"><span data-stu-id="515f6-179">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="515f6-180">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="515f6-180">localhost:1234/map1</span></span> | <span data-ttu-id="515f6-181">Teste de Map 1</span><span class="sxs-lookup"><span data-stu-id="515f6-181">Map Test 1</span></span>                   |
| <span data-ttu-id="515f6-182">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="515f6-182">localhost:1234/map2</span></span> | <span data-ttu-id="515f6-183">Teste de Map 2</span><span class="sxs-lookup"><span data-stu-id="515f6-183">Map Test 2</span></span>                   |
| <span data-ttu-id="515f6-184">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="515f6-184">localhost:1234/map3</span></span> | <span data-ttu-id="515f6-185">Saudação do delegado diferente de Map.</span><span class="sxs-lookup"><span data-stu-id="515f6-185">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="515f6-186">Quando `Map` é usado, os segmentos de caminho correspondentes são removidos do `HttpRequest.Path` e anexados em `HttpRequest.PathBase` para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="515f6-186">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="515f6-187">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) ramifica o pipeline de solicitação com base no resultado do predicado em questão.</span><span class="sxs-lookup"><span data-stu-id="515f6-187">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="515f6-188">Qualquer predicado do tipo `Func<HttpContext, bool>` pode ser usado para mapear as solicitações para um novo branch do pipeline.</span><span class="sxs-lookup"><span data-stu-id="515f6-188">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="515f6-189">No exemplo a seguir, um predicado é usado para detectar a presença de uma variável de cadeia de caracteres de consulta `branch`:</span><span class="sxs-lookup"><span data-stu-id="515f6-189">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="515f6-190">A tabela a seguir mostra as solicitações e as respostas de `http://localhost:1234` usando o código anterior.</span><span class="sxs-lookup"><span data-stu-id="515f6-190">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="515f6-191">Solicitação</span><span class="sxs-lookup"><span data-stu-id="515f6-191">Request</span></span>                       | <span data-ttu-id="515f6-192">Resposta</span><span class="sxs-lookup"><span data-stu-id="515f6-192">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="515f6-193">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="515f6-193">localhost:1234</span></span>                | <span data-ttu-id="515f6-194">Saudação do delegado diferente de Map.</span><span class="sxs-lookup"><span data-stu-id="515f6-194">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="515f6-195">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="515f6-195">localhost:1234/?branch=master</span></span> | <span data-ttu-id="515f6-196">Branch usado = mestre</span><span class="sxs-lookup"><span data-stu-id="515f6-196">Branch used = master</span></span>         |

<span data-ttu-id="515f6-197">`Map` é compatível com aninhamento, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="515f6-197">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

<span data-ttu-id="515f6-198">`Map` também pode ser correspondido com vários segmentos de uma vez:</span><span class="sxs-lookup"><span data-stu-id="515f6-198">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="515f6-199">Middleware interno</span><span class="sxs-lookup"><span data-stu-id="515f6-199">Built-in middleware</span></span>

<span data-ttu-id="515f6-200">O ASP.NET Core é fornecido com os seguintes componentes de middleware.</span><span class="sxs-lookup"><span data-stu-id="515f6-200">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="515f6-201">A coluna *Ordem* fornece observações sobre o posicionamento do middleware no pipeline de solicitação e sob quais condições o middleware podem encerrar a solicitação e impedir que outro middleware processe uma solicitação.</span><span class="sxs-lookup"><span data-stu-id="515f6-201">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="515f6-202">Middleware</span><span class="sxs-lookup"><span data-stu-id="515f6-202">Middleware</span></span> | <span data-ttu-id="515f6-203">Descrição</span><span class="sxs-lookup"><span data-stu-id="515f6-203">Description</span></span> | <span data-ttu-id="515f6-204">Pedido</span><span class="sxs-lookup"><span data-stu-id="515f6-204">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="515f6-205">Autenticação</span><span class="sxs-lookup"><span data-stu-id="515f6-205">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="515f6-206">Fornece suporte à autenticação.</span><span class="sxs-lookup"><span data-stu-id="515f6-206">Provides authentication support.</span></span> | <span data-ttu-id="515f6-207">Antes de `HttpContext.User` ser necessário.</span><span class="sxs-lookup"><span data-stu-id="515f6-207">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="515f6-208">Terminal para retornos de chamada OAuth.</span><span class="sxs-lookup"><span data-stu-id="515f6-208">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="515f6-209">CORS</span><span class="sxs-lookup"><span data-stu-id="515f6-209">CORS</span></span>](xref:security/cors) | <span data-ttu-id="515f6-210">Configura o Compartilhamento de Recursos entre Origens.</span><span class="sxs-lookup"><span data-stu-id="515f6-210">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="515f6-211">Antes de componentes que usam o CORS.</span><span class="sxs-lookup"><span data-stu-id="515f6-211">Before components that use CORS.</span></span> |
| [<span data-ttu-id="515f6-212">Diagnóstico</span><span class="sxs-lookup"><span data-stu-id="515f6-212">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="515f6-213">Configura o diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="515f6-213">Configures diagnostics.</span></span> | <span data-ttu-id="515f6-214">Antes dos componentes que geram erros.</span><span class="sxs-lookup"><span data-stu-id="515f6-214">Before components that generate errors.</span></span> |
| [<span data-ttu-id="515f6-215">Cabeçalhos encaminhados</span><span class="sxs-lookup"><span data-stu-id="515f6-215">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="515f6-216">Encaminha cabeçalhos como proxy para a solicitação atual.</span><span class="sxs-lookup"><span data-stu-id="515f6-216">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="515f6-217">Antes dos componentes que consomem os campos atualizados (exemplos: esquema, host, IP do cliente, método).</span><span class="sxs-lookup"><span data-stu-id="515f6-217">Before components that consume the updated fields (examples: scheme, host, client IP, method).</span></span> |
| [<span data-ttu-id="515f6-218">Substituição do Método HTTP</span><span class="sxs-lookup"><span data-stu-id="515f6-218">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="515f6-219">Permite que uma solicitação de entrada POST substitua o método.</span><span class="sxs-lookup"><span data-stu-id="515f6-219">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="515f6-220">Antes dos componentes que consomem o método atualizado.</span><span class="sxs-lookup"><span data-stu-id="515f6-220">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="515f6-221">Redirecionamento de HTTPS</span><span class="sxs-lookup"><span data-stu-id="515f6-221">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="515f6-222">Redirecione todas as solicitações HTTP para HTTPS (ASP.NET Core 2.1 ou posterior).</span><span class="sxs-lookup"><span data-stu-id="515f6-222">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="515f6-223">Antes dos componentes que consomem a URL.</span><span class="sxs-lookup"><span data-stu-id="515f6-223">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="515f6-224">Segurança de Transporte Estrita de HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="515f6-224">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="515f6-225">Middleware de aprimoramento de segurança que adiciona um cabeçalho de resposta especial (ASP.NET Core 2.1 ou posterior).</span><span class="sxs-lookup"><span data-stu-id="515f6-225">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="515f6-226">Antes das respostas serem enviadas e depois dos componentes que modificam solicitações (por exemplo, Cabeçalhos Encaminhados, Regravação de URL).</span><span class="sxs-lookup"><span data-stu-id="515f6-226">Before responses are sent and after components that modify requests (for example, Forwarded Headers, URL Rewriting).</span></span> |
| [<span data-ttu-id="515f6-227">MVC</span><span class="sxs-lookup"><span data-stu-id="515f6-227">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="515f6-228">Processa as solicitações de Razor Pages/MVC (ASP.NET Core 2.0 ou posterior).</span><span class="sxs-lookup"><span data-stu-id="515f6-228">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="515f6-229">Terminal, se uma solicitação corresponder a uma rota.</span><span class="sxs-lookup"><span data-stu-id="515f6-229">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="515f6-230">OWIN</span><span class="sxs-lookup"><span data-stu-id="515f6-230">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="515f6-231">Interoperabilidade com aplicativos baseados em OWIN, em servidores e em middleware.</span><span class="sxs-lookup"><span data-stu-id="515f6-231">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="515f6-232">Terminal, se o middleware OWIN processa totalmente a solicitação.</span><span class="sxs-lookup"><span data-stu-id="515f6-232">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="515f6-233">Cache de resposta</span><span class="sxs-lookup"><span data-stu-id="515f6-233">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="515f6-234">Fornece suporte para as respostas em cache.</span><span class="sxs-lookup"><span data-stu-id="515f6-234">Provides support for caching responses.</span></span> | <span data-ttu-id="515f6-235">Antes dos componentes que exigem armazenamento em cache.</span><span class="sxs-lookup"><span data-stu-id="515f6-235">Before components that require caching.</span></span> |
| [<span data-ttu-id="515f6-236">Compactação de resposta</span><span class="sxs-lookup"><span data-stu-id="515f6-236">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="515f6-237">Fornece suporte para a compactação de respostas.</span><span class="sxs-lookup"><span data-stu-id="515f6-237">Provides support for compressing responses.</span></span> | <span data-ttu-id="515f6-238">Antes dos componentes que exigem compactação.</span><span class="sxs-lookup"><span data-stu-id="515f6-238">Before components that require compression.</span></span> |
| [<span data-ttu-id="515f6-239">Localização de Solicitação</span><span class="sxs-lookup"><span data-stu-id="515f6-239">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="515f6-240">Fornece suporte à localização.</span><span class="sxs-lookup"><span data-stu-id="515f6-240">Provides localization support.</span></span> | <span data-ttu-id="515f6-241">Antes dos componentes de localização importantes.</span><span class="sxs-lookup"><span data-stu-id="515f6-241">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="515f6-242">Roteamento</span><span class="sxs-lookup"><span data-stu-id="515f6-242">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="515f6-243">Define e restringe as rotas de solicitação.</span><span class="sxs-lookup"><span data-stu-id="515f6-243">Defines and constrains request routes.</span></span> | <span data-ttu-id="515f6-244">Terminal de rotas correspondentes.</span><span class="sxs-lookup"><span data-stu-id="515f6-244">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="515f6-245">Sessão</span><span class="sxs-lookup"><span data-stu-id="515f6-245">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="515f6-246">Fornece suporte para gerenciar sessões de usuário.</span><span class="sxs-lookup"><span data-stu-id="515f6-246">Provides support for managing user sessions.</span></span> | <span data-ttu-id="515f6-247">Antes de componentes que exigem a sessão.</span><span class="sxs-lookup"><span data-stu-id="515f6-247">Before components that require Session.</span></span> |
| [<span data-ttu-id="515f6-248">Arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="515f6-248">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="515f6-249">Fornece suporte para servir arquivos estáticos e pesquisa no diretório.</span><span class="sxs-lookup"><span data-stu-id="515f6-249">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="515f6-250">Terminal, se uma solicitação corresponde a um arquivo.</span><span class="sxs-lookup"><span data-stu-id="515f6-250">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="515f6-251">Regravação de URL</span><span class="sxs-lookup"><span data-stu-id="515f6-251">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="515f6-252">Fornece suporte para regravar URLs e redirecionar solicitações.</span><span class="sxs-lookup"><span data-stu-id="515f6-252">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="515f6-253">Antes dos componentes que consomem a URL.</span><span class="sxs-lookup"><span data-stu-id="515f6-253">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="515f6-254">WebSockets</span><span class="sxs-lookup"><span data-stu-id="515f6-254">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="515f6-255">Habilita o protocolo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="515f6-255">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="515f6-256">Antes dos componentes que são necessários para aceitar solicitações de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="515f6-256">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="515f6-257">Gravar middleware</span><span class="sxs-lookup"><span data-stu-id="515f6-257">Write middleware</span></span>

<span data-ttu-id="515f6-258">O middleware geralmente é encapsulado em uma classe e exposto com um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="515f6-258">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="515f6-259">Considere o middleware a seguir, que define a cultura para a solicitação atual de uma cadeia de caracteres de consulta:</span><span class="sxs-lookup"><span data-stu-id="515f6-259">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="515f6-260">O código de exemplo anterior é usado para demonstrar a criação de um componente de middleware.</span><span class="sxs-lookup"><span data-stu-id="515f6-260">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="515f6-261">Para suporte de localização interna do ASP.NET Core, veja <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="515f6-261">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="515f6-262">Você pode testar o middleware ao transmitir a cultura, por exemplo `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="515f6-262">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="515f6-263">O código a seguir move o delegado de middleware para uma classe:</span><span class="sxs-lookup"><span data-stu-id="515f6-263">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="515f6-264">O nome do método `Task` de middleware precisa ser `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="515f6-264">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="515f6-265">No ASP.NET Core 2.0 ou posterior, o nome pode ser `Invoke` ou `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="515f6-265">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="515f6-266">O seguinte método de extensão expõe o middleware por meio do <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="515f6-266">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="515f6-267">O código a seguir chama o middleware de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="515f6-267">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="515f6-268">O middleware deve seguir o [princípio de dependências explícitas](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) ao expor suas dependências em seu construtor.</span><span class="sxs-lookup"><span data-stu-id="515f6-268">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="515f6-269">O middleware é construído uma vez por *tempo de vida do aplicativo*.</span><span class="sxs-lookup"><span data-stu-id="515f6-269">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="515f6-270">Consulte a seção [Dependências por solicitação](#per-request-dependencies) se você precisar compartilhar serviços com middleware dentro de uma solicitação.</span><span class="sxs-lookup"><span data-stu-id="515f6-270">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="515f6-271">Os componentes de middleware podem resolver suas dependências, utilizando a [DI (injeção de dependência)](xref:fundamentals/dependency-injection) por meio de parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="515f6-271">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="515f6-272">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) também pode aceitar parâmetros adicionais diretamente.</span><span class="sxs-lookup"><span data-stu-id="515f6-272">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="515f6-273">Dependências de pré-solicitação</span><span class="sxs-lookup"><span data-stu-id="515f6-273">Per-request dependencies</span></span>

<span data-ttu-id="515f6-274">Uma vez que o middleware é construído durante a inicialização do aplicativo, e não por solicitação, os serviços de tempo de vida *com escopo* usados pelos construtores do middleware não são compartilhados com outros tipos de dependência inseridos durante cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="515f6-274">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="515f6-275">Se você tiver que compartilhar um serviço *com escopo* entre seu serviço de middleware e serviços de outros tipos, adicione esses serviços à assinatura do método `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="515f6-275">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="515f6-276">O método `Invoke` pode aceitar parâmetros adicionais que são preenchidos pela injeção de dependência:</span><span class="sxs-lookup"><span data-stu-id="515f6-276">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="515f6-277">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="515f6-277">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
