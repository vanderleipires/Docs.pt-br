---
title: Middleware do ASP.NET Core
author: rick-anderson
description: Saiba mais sobre o middleware do ASP.NET Core e o pipeline de solicitação.
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 016f15c13470db53252941acafa25a3c6caf8db5
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="47831-103">Middleware do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47831-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="47831-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="47831-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="47831-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="47831-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="47831-106">O que é um middleware?</span><span class="sxs-lookup"><span data-stu-id="47831-106">What is middleware?</span></span>

<span data-ttu-id="47831-107">O middleware é um software montado em um pipeline de aplicativo para manipular solicitações e respostas.</span><span class="sxs-lookup"><span data-stu-id="47831-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="47831-108">Cada componente:</span><span class="sxs-lookup"><span data-stu-id="47831-108">Each component:</span></span>

* <span data-ttu-id="47831-109">Escolhe se deseja passar a solicitação para o próximo componente no pipeline.</span><span class="sxs-lookup"><span data-stu-id="47831-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="47831-110">Pode executar o trabalho antes e depois de o próximo componente no pipeline ser invocado.</span><span class="sxs-lookup"><span data-stu-id="47831-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="47831-111">Os delegados de solicitação são usados para criar o pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="47831-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="47831-112">Os delegados de solicitação manipulam cada solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="47831-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="47831-113">Os delegados de solicitação são configurados usando os métodos de extensão [Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions), [Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) e [Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="47831-113">Request delegates are configured using [Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions), [Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions), and [Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="47831-114">Um delegado de solicitação individual pode ser especificado em linha como um método anônimo (chamado do middleware em linha) ou pode ser definido em uma classe reutilizável.</span><span class="sxs-lookup"><span data-stu-id="47831-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="47831-115">Essas classes reutilizáveis e os métodos anônimos em linha são o *middleware* ou *componentes do middleware*.</span><span class="sxs-lookup"><span data-stu-id="47831-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="47831-116">Cada componente do middleware no pipeline de solicitação é responsável por invocar o próximo componente no pipeline ou por ligar a cadeia em curto-circuito, se apropriado.</span><span class="sxs-lookup"><span data-stu-id="47831-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="47831-117">A seção [Migrating HTTP Modules to Middleware](xref:migration/http-modules) (Migrar módulos HTTP para o Middleware) explica a diferença entre pipelines de solicitação no ASP.NET Core e no ASP.NET 4.x e fornece mais exemplos do middleware.</span><span class="sxs-lookup"><span data-stu-id="47831-117">[Migrate HTTP Modules to Middleware](xref:migration/http-modules) explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="47831-118">Criando um pipeline do middleware com o IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="47831-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="47831-119">O pipeline de solicitação do ASP.NET Core consiste em uma sequência de delegados de solicitação, chamados um após o outro, como mostrado neste diagrama (o thread de execução segue as setas pretas):</span><span class="sxs-lookup"><span data-stu-id="47831-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Padrão de processamento de solicitação mostrando a chegada de uma solicitação, processada por meio de três middlewares e a resposta que sai do aplicativo.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="47831-123">Cada delegado pode executar operações antes e depois do próximo delegado.</span><span class="sxs-lookup"><span data-stu-id="47831-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="47831-124">Um delegado também pode optar por não transmitir uma solicitação ao próximo delegado, o que também é chamado de ligar o pipeline de solicitação em curto-circuito.</span><span class="sxs-lookup"><span data-stu-id="47831-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="47831-125">O curto-circuito geralmente é desejável porque ele evita trabalho desnecessário.</span><span class="sxs-lookup"><span data-stu-id="47831-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="47831-126">Por exemplo, o middleware de arquivo estático pode retornar uma solicitação para um arquivo estático e ligar o restante do pipeline em curto-circuito.</span><span class="sxs-lookup"><span data-stu-id="47831-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="47831-127">Os delegados de tratamento de exceção precisam ser chamados no início do pipeline, para que possam detectar exceções que ocorrem em etapas posteriores do pipeline.</span><span class="sxs-lookup"><span data-stu-id="47831-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="47831-128">O aplicativo ASP.NET Core mais simples possível define um delegado de solicitação única que controla todas as solicitações.</span><span class="sxs-lookup"><span data-stu-id="47831-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="47831-129">Este caso não inclui um pipeline de solicitação real.</span><span class="sxs-lookup"><span data-stu-id="47831-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="47831-130">Em vez disso, uma única função anônima é chamada em resposta a cada solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="47831-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/sample/Middleware/Startup.cs)]

<span data-ttu-id="47831-131">O primeiro delegado [app.Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions) encerra o pipeline.</span><span class="sxs-lookup"><span data-stu-id="47831-131">The first [app.Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="47831-132">É possível encadear vários delegados de solicitação junto com o [app.Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="47831-132">You can chain multiple request delegates together with [app.Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="47831-133">O parâmetro `next` representa o próximo delegado no pipeline.</span><span class="sxs-lookup"><span data-stu-id="47831-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="47831-134">(Lembre-se de que você pode ligar o pipeline em curto-circuito ao *não* chamar o *próximo* parâmetro.) Normalmente, você pode executar ações antes e depois do próximo delegado, conforme este exemplo demonstra:</span><span class="sxs-lookup"><span data-stu-id="47831-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="47831-135">Não chame `next.Invoke` depois que a resposta tiver sido enviada ao cliente.</span><span class="sxs-lookup"><span data-stu-id="47831-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="47831-136">Altera para `HttpResponse` depois que a resposta foi iniciada e lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="47831-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="47831-137">Por exemplo, mudanças como a configuração de cabeçalhos, o código de status, etc., lançarão uma exceção.</span><span class="sxs-lookup"><span data-stu-id="47831-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="47831-138">Gravar no corpo da resposta após a chamada `next`:</span><span class="sxs-lookup"><span data-stu-id="47831-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="47831-139">Pode causar uma violação do protocolo.</span><span class="sxs-lookup"><span data-stu-id="47831-139">May cause a protocol violation.</span></span> <span data-ttu-id="47831-140">Por exemplo, gravar mais do que o `content-length` indicado.</span><span class="sxs-lookup"><span data-stu-id="47831-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="47831-141">Pode corromper o formato do corpo.</span><span class="sxs-lookup"><span data-stu-id="47831-141">May corrupt the body format.</span></span> <span data-ttu-id="47831-142">Por exemplo, gravar um rodapé HTML em um arquivo CSS.</span><span class="sxs-lookup"><span data-stu-id="47831-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="47831-143">[HttpResponse.HasStarted](/dotnet/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) é uma dica útil para indicar se os cabeçalhos foram enviados e/ou o corpo foi gravado.</span><span class="sxs-lookup"><span data-stu-id="47831-143">[HttpResponse.HasStarted](/dotnet/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="47831-144">Ordenando</span><span class="sxs-lookup"><span data-stu-id="47831-144">Ordering</span></span>

<span data-ttu-id="47831-145">A ordem em que os componentes do middleware são adicionados ao método `Configure` define a ordem em que eles são invocados nas solicitações e a ordem inversa para a resposta.</span><span class="sxs-lookup"><span data-stu-id="47831-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="47831-146">Essa ordem é crítica para a segurança, o desempenho e a funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="47831-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="47831-147">O método Configure (mostrado abaixo) adiciona os seguintes componentes de middleware:</span><span class="sxs-lookup"><span data-stu-id="47831-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="47831-148">Exceção/tratamento de erro</span><span class="sxs-lookup"><span data-stu-id="47831-148">Exception/error handling</span></span>
2. <span data-ttu-id="47831-149">Servidor de arquivos estático</span><span class="sxs-lookup"><span data-stu-id="47831-149">Static file server</span></span>
3. <span data-ttu-id="47831-150">Autenticação</span><span class="sxs-lookup"><span data-stu-id="47831-150">Authentication</span></span>
4. <span data-ttu-id="47831-151">MVC</span><span class="sxs-lookup"><span data-stu-id="47831-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="47831-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="47831-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="47831-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="47831-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="47831-154">No código acima, `UseExceptionHandler` é o primeiro componente de middleware adicionado ao pipeline—portanto, ele captura qualquer exceção que ocorra em chamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="47831-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="47831-155">O middleware de arquivo estático é chamado no início do pipeline para que possa controlar as solicitações e o curto-circuito sem passar pelos componentes restantes.</span><span class="sxs-lookup"><span data-stu-id="47831-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="47831-156">O middleware de arquivo estático não fornece **nenhuma** verificação de autorização.</span><span class="sxs-lookup"><span data-stu-id="47831-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="47831-157">Todos os arquivos atendidos, incluindo aqueles em *wwwroot*, estão disponíveis publicamente.</span><span class="sxs-lookup"><span data-stu-id="47831-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="47831-158">Veja [Arquivos estáticos](xref:fundamentals/static-files) para conhecer uma abordagem para proteger arquivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="47831-158">See [Static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="47831-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="47831-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="47831-160">Se a solicitação não for controlada pelo middleware de arquivo estático, ela será transmitida para o middleware de identidade (`app.UseAuthentication`), que executa a autenticação.</span><span class="sxs-lookup"><span data-stu-id="47831-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="47831-161">A identidade não liga as solicitações não autenticadas em curto-circuito.</span><span class="sxs-lookup"><span data-stu-id="47831-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="47831-162">Embora a identidade autentique as solicitações, a autorização (e a rejeição) ocorre somente depois que o MVC seleciona uma Página Razor específica ou um controlador e uma ação.</span><span class="sxs-lookup"><span data-stu-id="47831-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="47831-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="47831-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="47831-164">Se a solicitação não for controlada pelo middleware de arquivo estático, ela será transmitida para o middleware de identidade (`app.UseIdentity`), que executa a autenticação.</span><span class="sxs-lookup"><span data-stu-id="47831-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="47831-165">A identidade não liga as solicitações não autenticadas em curto-circuito.</span><span class="sxs-lookup"><span data-stu-id="47831-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="47831-166">Embora a identidade autentique as solicitações, a autorização (e a rejeição) ocorre somente depois que o MVC seleciona um controlador específico e uma ação.</span><span class="sxs-lookup"><span data-stu-id="47831-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="47831-167">O exemplo a seguir demonstra uma solicitação de middleware na qual os pedidos de arquivos estáticos são tratados pelo middleware de arquivo estático antes do middleware de compactação de resposta.</span><span class="sxs-lookup"><span data-stu-id="47831-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="47831-168">Os arquivos estáticos não são compactados com esse pedido de middleware.</span><span class="sxs-lookup"><span data-stu-id="47831-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="47831-169">As respostas do MVC de [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) podem ser compactadas.</span><span class="sxs-lookup"><span data-stu-id="47831-169">The MVC responses from [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="47831-170">Use, Run e Map</span><span class="sxs-lookup"><span data-stu-id="47831-170">Use, Run, and Map</span></span>

<span data-ttu-id="47831-171">Você pode configurar o pipeline de HTTP usando `Use`, `Run` e `Map`.</span><span class="sxs-lookup"><span data-stu-id="47831-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="47831-172">O método `Use` pode ligar o pipeline em curto-circuito (ou seja, se ele não chamar um delegado de solicitação `next`).</span><span class="sxs-lookup"><span data-stu-id="47831-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="47831-173">`Run` é uma convenção e alguns componentes de middleware podem expor os métodos `Run[Middleware]` que são executados no final do pipeline.</span><span class="sxs-lookup"><span data-stu-id="47831-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="47831-174">As extensões `Map*` são usadas como uma convenção de ramificação do pipeline.</span><span class="sxs-lookup"><span data-stu-id="47831-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="47831-175">[Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) ramifica o pipeline de solicitação com base na correspondência do caminho da solicitação em questão.</span><span class="sxs-lookup"><span data-stu-id="47831-175">[Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="47831-176">Se o caminho da solicitação iniciar com o caminho especificado, o branch será executado.</span><span class="sxs-lookup"><span data-stu-id="47831-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="47831-177">A tabela a seguir mostra as solicitações e as respostas de `http://localhost:1234` usando o código anterior:</span><span class="sxs-lookup"><span data-stu-id="47831-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="47831-178">Solicitação</span><span class="sxs-lookup"><span data-stu-id="47831-178">Request</span></span> | <span data-ttu-id="47831-179">Resposta</span><span class="sxs-lookup"><span data-stu-id="47831-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="47831-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="47831-180">localhost:1234</span></span> | <span data-ttu-id="47831-181">Saudação do delegado diferente de Map.</span><span class="sxs-lookup"><span data-stu-id="47831-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="47831-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="47831-182">localhost:1234/map1</span></span> | <span data-ttu-id="47831-183">Teste de Map 1</span><span class="sxs-lookup"><span data-stu-id="47831-183">Map Test 1</span></span> |
| <span data-ttu-id="47831-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="47831-184">localhost:1234/map2</span></span> | <span data-ttu-id="47831-185">Teste de Map 2</span><span class="sxs-lookup"><span data-stu-id="47831-185">Map Test 2</span></span> |
| <span data-ttu-id="47831-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="47831-186">localhost:1234/map3</span></span> | <span data-ttu-id="47831-187">Saudação do delegado diferente de Map.</span><span class="sxs-lookup"><span data-stu-id="47831-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="47831-188">Quando `Map` é usado, os segmentos de caminho correspondentes são removidos do `HttpRequest.Path` e anexados em `HttpRequest.PathBase` para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="47831-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="47831-189">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) ramifica o pipeline de solicitação com base no resultado do predicado em questão.</span><span class="sxs-lookup"><span data-stu-id="47831-189">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="47831-190">Qualquer predicado do tipo `Func<HttpContext, bool>` pode ser usado para mapear as solicitações para um novo branch do pipeline.</span><span class="sxs-lookup"><span data-stu-id="47831-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="47831-191">No exemplo a seguir, um predicado é usado para detectar a presença de uma variável de cadeia de caracteres de consulta `branch`:</span><span class="sxs-lookup"><span data-stu-id="47831-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="47831-192">A tabela a seguir mostra as solicitações e as respostas de `http://localhost:1234` usando o código anterior:</span><span class="sxs-lookup"><span data-stu-id="47831-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="47831-193">Solicitação</span><span class="sxs-lookup"><span data-stu-id="47831-193">Request</span></span> | <span data-ttu-id="47831-194">Resposta</span><span class="sxs-lookup"><span data-stu-id="47831-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="47831-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="47831-195">localhost:1234</span></span> | <span data-ttu-id="47831-196">Saudação do delegado diferente de Map.</span><span class="sxs-lookup"><span data-stu-id="47831-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="47831-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="47831-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="47831-198">Branch usado = mestre</span><span class="sxs-lookup"><span data-stu-id="47831-198">Branch used = master</span></span>|

<span data-ttu-id="47831-199">`Map` é compatível com aninhamento, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="47831-199">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="47831-200">`Map` também pode ser correspondido com vários segmentos de uma vez, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="47831-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="47831-201">Middleware interno</span><span class="sxs-lookup"><span data-stu-id="47831-201">Built-in middleware</span></span>

<span data-ttu-id="47831-202">O ASP.NET Core é fornecido com os componentes de middleware a seguir, bem como com uma descrição da ordem em que eles devem ser adicionados:</span><span class="sxs-lookup"><span data-stu-id="47831-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="47831-203">Middleware</span><span class="sxs-lookup"><span data-stu-id="47831-203">Middleware</span></span> | <span data-ttu-id="47831-204">Descrição</span><span class="sxs-lookup"><span data-stu-id="47831-204">Description</span></span> | <span data-ttu-id="47831-205">Pedido</span><span class="sxs-lookup"><span data-stu-id="47831-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="47831-206">Autenticação</span><span class="sxs-lookup"><span data-stu-id="47831-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="47831-207">Fornece suporte à autenticação.</span><span class="sxs-lookup"><span data-stu-id="47831-207">Provides authentication support.</span></span> | <span data-ttu-id="47831-208">Antes de `HttpContext.User` ser necessário.</span><span class="sxs-lookup"><span data-stu-id="47831-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="47831-209">Terminal para retornos de chamada OAuth.</span><span class="sxs-lookup"><span data-stu-id="47831-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="47831-210">CORS</span><span class="sxs-lookup"><span data-stu-id="47831-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="47831-211">Configura o Compartilhamento de Recursos entre Origens.</span><span class="sxs-lookup"><span data-stu-id="47831-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="47831-212">Antes de componentes que usam o CORS.</span><span class="sxs-lookup"><span data-stu-id="47831-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="47831-213">Diagnóstico</span><span class="sxs-lookup"><span data-stu-id="47831-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="47831-214">Configura o diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="47831-214">Configures diagnostics.</span></span> | <span data-ttu-id="47831-215">Antes dos componentes que geram erros.</span><span class="sxs-lookup"><span data-stu-id="47831-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="47831-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="47831-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="47831-217">Encaminha cabeçalhos como proxy para a solicitação atual.</span><span class="sxs-lookup"><span data-stu-id="47831-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="47831-218">Antes dos componentes que consomem os campos atualizados (exemplos: Esquema, Host, ClientIP, Método).</span><span class="sxs-lookup"><span data-stu-id="47831-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="47831-219">Cache de resposta</span><span class="sxs-lookup"><span data-stu-id="47831-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="47831-220">Fornece suporte para as respostas em cache.</span><span class="sxs-lookup"><span data-stu-id="47831-220">Provides support for caching responses.</span></span> | <span data-ttu-id="47831-221">Antes dos componentes que exigem armazenamento em cache.</span><span class="sxs-lookup"><span data-stu-id="47831-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="47831-222">Compactação de resposta</span><span class="sxs-lookup"><span data-stu-id="47831-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="47831-223">Fornece suporte para a compactação de respostas.</span><span class="sxs-lookup"><span data-stu-id="47831-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="47831-224">Antes dos componentes que exigem compactação.</span><span class="sxs-lookup"><span data-stu-id="47831-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="47831-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="47831-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="47831-226">Fornece suporte à localização.</span><span class="sxs-lookup"><span data-stu-id="47831-226">Provides localization support.</span></span> | <span data-ttu-id="47831-227">Antes dos componentes de localização importantes.</span><span class="sxs-lookup"><span data-stu-id="47831-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="47831-228">Roteamento</span><span class="sxs-lookup"><span data-stu-id="47831-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="47831-229">Define e restringe as rotas de solicitação.</span><span class="sxs-lookup"><span data-stu-id="47831-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="47831-230">Terminal de rotas correspondentes.</span><span class="sxs-lookup"><span data-stu-id="47831-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="47831-231">Sessão</span><span class="sxs-lookup"><span data-stu-id="47831-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="47831-232">Fornece suporte para gerenciar sessões de usuário.</span><span class="sxs-lookup"><span data-stu-id="47831-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="47831-233">Antes de componentes que exigem a sessão.</span><span class="sxs-lookup"><span data-stu-id="47831-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="47831-234">Arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="47831-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="47831-235">Fornece suporte para servir arquivos estáticos e pesquisa no diretório.</span><span class="sxs-lookup"><span data-stu-id="47831-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="47831-236">Terminal, se uma solicitação for correspondente aos arquivos.</span><span class="sxs-lookup"><span data-stu-id="47831-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="47831-237">Regravação de URL </span><span class="sxs-lookup"><span data-stu-id="47831-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="47831-238">Fornece suporte para regravar URLs e redirecionar solicitações.</span><span class="sxs-lookup"><span data-stu-id="47831-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="47831-239">Antes dos componentes que consomem a URL.</span><span class="sxs-lookup"><span data-stu-id="47831-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="47831-240">WebSockets</span><span class="sxs-lookup"><span data-stu-id="47831-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="47831-241">Habilita o protocolo WebSockets.</span><span class="sxs-lookup"><span data-stu-id="47831-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="47831-242">Antes dos componentes que são necessários para aceitar solicitações de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="47831-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="47831-243">Middleware de gravação</span><span class="sxs-lookup"><span data-stu-id="47831-243">Writing middleware</span></span>

<span data-ttu-id="47831-244">O middleware geralmente é encapsulado em uma classe e exposto com um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="47831-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="47831-245">Considere o middleware a seguir, que define a cultura para a solicitação atual da cadeia de caracteres de consulta:</span><span class="sxs-lookup"><span data-stu-id="47831-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="47831-246">Observação: o código de exemplo acima é usado para demonstrar a criação de um componente de middleware.</span><span class="sxs-lookup"><span data-stu-id="47831-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="47831-247">Consulte [ Globalização e localização](xref:fundamentals/localization) para obter suporte de localização interna do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="47831-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="47831-248">Você pode testar o middleware ao transmitir a cultura, por exemplo `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="47831-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="47831-249">O código a seguir move o delegado de middleware para uma classe:</span><span class="sxs-lookup"><span data-stu-id="47831-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

> [!NOTE]
> <span data-ttu-id="47831-250">No ASP.NET Core 1.x, o nome do método `Task` de middleware deve ser `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="47831-250">In ASP.NET Core 1.x, the middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="47831-251">No ASP.NET Core 2.0 ou posterior, o nome pode ser `Invoke` ou `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="47831-251">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

<span data-ttu-id="47831-252">O seguinte método de extensão expõe o middleware por meio do [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="47831-252">The following extension method exposes the middleware through [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="47831-253">O código a seguir chama o middleware de `Configure`:</span><span class="sxs-lookup"><span data-stu-id="47831-253">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="47831-254">O middleware deve seguir o [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/) ao expor suas dependências em seu construtor.</span><span class="sxs-lookup"><span data-stu-id="47831-254">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="47831-255">O middleware é construído uma vez por *tempo de vida do aplicativo*.</span><span class="sxs-lookup"><span data-stu-id="47831-255">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="47831-256">Consulte a seção *Dependências por solicitação* abaixo se você precisar compartilhar serviços com middleware dentro de uma solicitação.</span><span class="sxs-lookup"><span data-stu-id="47831-256">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="47831-257">Os componentes de middleware podem resolver suas dependências, utilizando a injeção de dependência por meio de parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="47831-257">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="47831-258">[`UseMiddleware<T>`](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) também pode aceitar parâmetros adicionais diretamente.</span><span class="sxs-lookup"><span data-stu-id="47831-258">[`UseMiddleware<T>`](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="47831-259">Dependências de pré-solicitação</span><span class="sxs-lookup"><span data-stu-id="47831-259">Per-request dependencies</span></span>

<span data-ttu-id="47831-260">Uma vez que o middleware é construído durante a inicialização do aplicativo, e não por solicitação, os serviços de tempo de vida *com escopo* usados pelos construtores do middleware não são compartilhados com outros tipos de dependência inseridos durante cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="47831-260">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="47831-261">Se você tiver que compartilhar um serviço *com escopo* entre seu serviço de middleware e serviços de outros tipos, adicione esses serviços à assinatura do método `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="47831-261">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="47831-262">O método `Invoke` pode aceitar parâmetros adicionais que são preenchidos pela injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="47831-262">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="47831-263">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="47831-263">For example:</span></span>

```csharp
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="47831-264">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="47831-264">Additional resources</span></span>

* [<span data-ttu-id="47831-265">Migrar módulos HTTP para Middleware</span><span class="sxs-lookup"><span data-stu-id="47831-265">Migrate HTTP Modules to Middleware</span></span>](xref:migration/http-modules)
* [<span data-ttu-id="47831-266">Inicialização de aplicativos</span><span class="sxs-lookup"><span data-stu-id="47831-266">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="47831-267">Recursos de solicitação</span><span class="sxs-lookup"><span data-stu-id="47831-267">Request Features</span></span>](xref:fundamentals/request-features)
* [<span data-ttu-id="47831-268">Ativação de middleware de fábrica</span><span class="sxs-lookup"><span data-stu-id="47831-268">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="47831-269">Ativação de middleware com um contêiner de terceiros</span><span class="sxs-lookup"><span data-stu-id="47831-269">Middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
