---
title: Middleware ASP.NET Core
author: rick-anderson
description: "Saiba mais sobre o ASP.NET Core middleware e o pipeline de solicitação."
ms.author: riande
manager: wpickett
ms.date: 01/22/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: ef130e736e2f32fa134156d979ce5bfbedcae828
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="5f5c1-103">Conceitos básicos de Middleware do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f5c1-103">ASP.NET Core Middleware Fundamentals</span></span>

<a name="fundamentals-middleware"></a>

<span data-ttu-id="5f5c1-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="5f5c1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="5f5c1-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5f5c1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="5f5c1-106">O que é middleware?</span><span class="sxs-lookup"><span data-stu-id="5f5c1-106">What is middleware?</span></span>

<span data-ttu-id="5f5c1-107">Middleware é um software que é gerado em um pipeline de aplicativo para lidar com solicitações e respostas.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-107">Middleware is software that is assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="5f5c1-108">Cada componente:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-108">Each component:</span></span>

* <span data-ttu-id="5f5c1-109">Escolhe se deseja transmitir a solicitação para o próximo componente no pipeline.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="5f5c1-110">Pode executar o trabalho antes e após o próximo componente de pipeline é invocado.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="5f5c1-111">Solicitação delegados são usados para criar o pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="5f5c1-112">Os delegados de solicitação lidar com cada solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="5f5c1-113">Solicitar delegados são configurados usando [executar](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [mapa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), e [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="5f5c1-114">Um delegado de solicitação individual pode ser especificado em linha como um método anônimo (chamado de middleware em linha) ou ela pode ser definida em uma classe reutilizável.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="5f5c1-115">Essas classes reutilizáveis e métodos anônimos em linha são *middleware*, ou *componentes de middleware*.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="5f5c1-116">Cada componente de middleware no pipeline de solicitação é responsável por chamar o próximo componente no pipeline ou curto-circuito da cadeia, se apropriado.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="5f5c1-117">[Migrando módulos HTTP Middleware](../migration/http-modules.md) explica a diferença entre pipelines de solicitação no núcleo do ASP.NET e as versões anteriores e fornece mais exemplos de middleware.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-117">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="5f5c1-118">Criando um pipeline de middleware com IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="5f5c1-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="5f5c1-119">O pipeline de solicitação do ASP.NET Core consiste em uma sequência de representantes de solicitação, chamado um após o outro, como este diagrama mostra (o thread de execução segue as setas pretas):</span><span class="sxs-lookup"><span data-stu-id="5f5c1-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Padrão de processamento de solicitação mostra uma solicitação que chegam, por meio de três middlewares e a resposta sair do aplicativo de processamento.](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="5f5c1-123">Cada representante pode executar operações antes e após o próximo delegado.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="5f5c1-124">Um delegado também pode optar por não passar uma solicitação para o próximo delegado, que é chamado de curto-circuito do pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="5f5c1-125">Curto-circuito geralmente é desejável, porque ela evita trabalho desnecessário.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="5f5c1-126">Por exemplo, o middleware de arquivo estático pode retornar uma solicitação para um arquivo estático e o restante do pipeline de curto-circuito.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="5f5c1-127">Delegados de tratamento de exceção precisam ser chamado no início do pipeline, para que podem detectar exceções que ocorrem em etapas posteriores do pipeline.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="5f5c1-128">O aplicativo do ASP.NET Core possíveis mais simples define um delegado de solicitação única que manipula todas as solicitações.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="5f5c1-129">Neste caso, não inclui um pipeline de solicitação real.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="5f5c1-130">Em vez disso, uma única função anônima é chamada em resposta a cada solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="5f5c1-131">A primeira [aplicativo. Executar](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegado encerra o pipeline.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="5f5c1-132">É possível encadear vários representantes de solicitação junto com [aplicativo. Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="5f5c1-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="5f5c1-133">O `next` parâmetro representa o delegado Avançar no pipeline.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="5f5c1-134">(Lembre-se de que você pode curto-circuito do pipeline por *não* chamando o *próximo* parâmetro.) Você normalmente pode executar ações antes e após o próximo representante, conforme este exemplo demonstra:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="5f5c1-135">Não chame `next.Invoke` depois que a resposta foi enviada ao cliente.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-135">Do not call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="5f5c1-136">Altera para `HttpResponse` depois que a resposta foi iniciada lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="5f5c1-137">Por exemplo, as alterações, como definição de cabeçalhos, etc., o código de status lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="5f5c1-138">Gravar o corpo da resposta após a chamada `next`:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="5f5c1-139">Pode causar uma violação do protocolo.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-139">May cause a protocol violation.</span></span> <span data-ttu-id="5f5c1-140">Por exemplo, gravar mais do que o indicado `content-length`.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="5f5c1-141">Pode corromper o formato do corpo.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-141">May corrupt the body format.</span></span> <span data-ttu-id="5f5c1-142">Por exemplo, gravando um rodapé HTML em um arquivo CSS.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="5f5c1-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) é uma dica útil para indicar se cabeçalhos terem sido enviados e/ou o corpo foi gravado.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="5f5c1-144">Ordenando</span><span class="sxs-lookup"><span data-stu-id="5f5c1-144">Ordering</span></span>

<span data-ttu-id="5f5c1-145">A ordem em que foram adicionados a componentes de middleware de `Configure` método define a ordem na qual eles são chamados em solicitações e ordem inversa para a resposta.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-145">The order that middleware components are added in the `Configure` method defines the order in which they are invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="5f5c1-146">Essa ordem é crítico para segurança, desempenho e funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="5f5c1-147">O método de configurar (mostrado abaixo) adiciona os seguintes componentes de middleware:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="5f5c1-148">Tratamento de exceção/erros</span><span class="sxs-lookup"><span data-stu-id="5f5c1-148">Exception/error handling</span></span>
2. <span data-ttu-id="5f5c1-149">Servidor de arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="5f5c1-149">Static file server</span></span>
3. <span data-ttu-id="5f5c1-150">Autenticação</span><span class="sxs-lookup"><span data-stu-id="5f5c1-150">Authentication</span></span>
4. <span data-ttu-id="5f5c1-151">MVC</span><span class="sxs-lookup"><span data-stu-id="5f5c1-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5f5c1-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5f5c1-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5f5c1-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5f5c1-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="5f5c1-154">No código acima, `UseExceptionHandler` é o primeiro componente de middleware adicionado ao pipeline, portanto, ele captura todas as exceções que ocorrem em chamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="5f5c1-155">O middleware de arquivo estático é chamado no início do pipeline para que possa manipular solicitações e sem passar através dos componentes restantes de curto-circuito.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="5f5c1-156">Fornece o middleware de arquivo estático **sem** verificações de autorização.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="5f5c1-157">Todos os arquivos servidas por ele, incluindo aqueles em *wwwroot*, publicamente disponíveis.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="5f5c1-158">Consulte [trabalhando com arquivos estáticos](xref:fundamentals/static-files) para uma abordagem proteger arquivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5f5c1-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5f5c1-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="5f5c1-160">Se a solicitação não é manipulada pelo middleware de arquivo estático, ele é passado para o middleware de identidade (`app.UseAuthentication`), que executa a autenticação.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-160">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="5f5c1-161">Identidade não curto-circuito solicitações não autenticadas.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-161">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="5f5c1-162">Embora identidade autentica solicitações, autorização (e rejeição) ocorrem somente após MVC seleciona uma página Razor específico ou controlador e ação.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5f5c1-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5f5c1-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5f5c1-164">Se a solicitação não é manipulada pelo middleware de arquivo estático, ele é passado para o middleware de identidade (`app.UseIdentity`), que executa a autenticação.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-164">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="5f5c1-165">Identidade não curto-circuito solicitações não autenticadas.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-165">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="5f5c1-166">Embora identidade autentica solicitações, autorização (e rejeição) ocorrem somente após MVC seleciona um controlador específico e a ação.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="5f5c1-167">O exemplo a seguir demonstra um middleware de ordem em que as solicitações para arquivos estáticos são manipuladas pelo middleware de arquivo estático antes do middleware de compactação de resposta.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="5f5c1-168">Arquivos estáticos não são compactados com essa ordenação do middleware.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="5f5c1-169">As respostas MVC de [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) podem ser compactados.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

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

### <a name="use-run-and-map"></a><span data-ttu-id="5f5c1-170">Use, executar e mapa</span><span class="sxs-lookup"><span data-stu-id="5f5c1-170">Use, Run, and Map</span></span>

<span data-ttu-id="5f5c1-171">Configurar o pipeline HTTP usando `Use`, `Run`, e `Map`.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="5f5c1-172">O `Use` método pode curto-circuito do pipeline (ou seja, se ele não chama um `next` delegado de solicitação).</span><span class="sxs-lookup"><span data-stu-id="5f5c1-172">The `Use` method can short-circuit the pipeline (that is, if it does not call a `next` request delegate).</span></span> <span data-ttu-id="5f5c1-173">`Run`é uma convenção e alguns componentes de middleware podem expor `Run[Middleware]` métodos que são executados no final do pipeline.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="5f5c1-174">`Map*`extensões são usadas como uma convenção de ramificação do pipeline.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="5f5c1-175">[Mapa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) ramificações o pipeline de solicitação com base na correspondência do caminho da solicitação em questão.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="5f5c1-176">Se o caminho da solicitação inicia com o caminho especificado, a ramificação é executada.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="5f5c1-177">A tabela a seguir mostra as solicitações e respostas de `http://localhost:1234` usando o código anterior:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="5f5c1-178">Solicitação</span><span class="sxs-lookup"><span data-stu-id="5f5c1-178">Request</span></span> | <span data-ttu-id="5f5c1-179">Resposta</span><span class="sxs-lookup"><span data-stu-id="5f5c1-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="5f5c1-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="5f5c1-180">localhost:1234</span></span> | <span data-ttu-id="5f5c1-181">Saudação de mapa não delegado.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="5f5c1-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="5f5c1-182">localhost:1234/map1</span></span> | <span data-ttu-id="5f5c1-183">Teste 1 de mapa</span><span class="sxs-lookup"><span data-stu-id="5f5c1-183">Map Test 1</span></span> |
| <span data-ttu-id="5f5c1-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="5f5c1-184">localhost:1234/map2</span></span> | <span data-ttu-id="5f5c1-185">Teste 2 de mapa</span><span class="sxs-lookup"><span data-stu-id="5f5c1-185">Map Test 2</span></span> |
| <span data-ttu-id="5f5c1-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="5f5c1-186">localhost:1234/map3</span></span> | <span data-ttu-id="5f5c1-187">Saudação de mapa não delegado.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="5f5c1-188">Quando `Map` é usado, o segmento de caminho correspondente (s) serão removidos do `HttpRequest.Path` e anexado a `HttpRequest.PathBase` para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="5f5c1-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) ramificações do pipeline de solicitação com base no resultado do predicado em questão.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="5f5c1-190">Nenhum predicado de tipo `Func<HttpContext, bool>` pode ser usado para mapear solicitações para uma nova ramificação do pipeline.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="5f5c1-191">No exemplo a seguir, um predicado é usado para detectar a presença de uma variável de cadeia de caracteres de consulta `branch`:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="5f5c1-192">A tabela a seguir mostra as solicitações e respostas de `http://localhost:1234` usando o código anterior:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="5f5c1-193">Solicitação</span><span class="sxs-lookup"><span data-stu-id="5f5c1-193">Request</span></span> | <span data-ttu-id="5f5c1-194">Resposta</span><span class="sxs-lookup"><span data-stu-id="5f5c1-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="5f5c1-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="5f5c1-195">localhost:1234</span></span> | <span data-ttu-id="5f5c1-196">Saudação de mapa não delegado.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="5f5c1-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="5f5c1-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="5f5c1-198">Ramificação usada = mestre</span><span class="sxs-lookup"><span data-stu-id="5f5c1-198">Branch used = master</span></span>|

<span data-ttu-id="5f5c1-199">`Map`dá suporte a aninhamento, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-199">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="5f5c1-200">`Map`pode também corresponder a vários segmentos de uma vez, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="5f5c1-201">Middleware interno</span><span class="sxs-lookup"><span data-stu-id="5f5c1-201">Built-in middleware</span></span>

<span data-ttu-id="5f5c1-202">ASP.NET Core é fornecido com os seguintes componentes de middleware, bem como uma descrição da ordem em que eles devem ser adicionados:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="5f5c1-203">Middleware</span><span class="sxs-lookup"><span data-stu-id="5f5c1-203">Middleware</span></span> | <span data-ttu-id="5f5c1-204">Descrição</span><span class="sxs-lookup"><span data-stu-id="5f5c1-204">Description</span></span> | <span data-ttu-id="5f5c1-205">Pedido</span><span class="sxs-lookup"><span data-stu-id="5f5c1-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="5f5c1-206">Autenticação</span><span class="sxs-lookup"><span data-stu-id="5f5c1-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="5f5c1-207">Dá suporte à autenticação.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-207">Provides authentication support.</span></span> | <span data-ttu-id="5f5c1-208">Antes de `HttpContext.User` é necessária.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="5f5c1-209">Terminal para retornos de chamada de OAuth.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="5f5c1-210">CORS</span><span class="sxs-lookup"><span data-stu-id="5f5c1-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="5f5c1-211">Define o compartilhamento de recursos entre origens.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="5f5c1-212">Antes de componentes que usam o CORS.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="5f5c1-213">Diagnóstico</span><span class="sxs-lookup"><span data-stu-id="5f5c1-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="5f5c1-214">Configura o diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-214">Configures diagnostics.</span></span> | <span data-ttu-id="5f5c1-215">Antes de componentes que geram erros.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="5f5c1-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="5f5c1-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="5f5c1-217">Encaminha cabeçalhos como proxy para a solicitação atual.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="5f5c1-218">Antes dos componentes que consomem os campos atualizados (exemplos: esquema, Host, ClientIP, método).</span><span class="sxs-lookup"><span data-stu-id="5f5c1-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="5f5c1-219">Cache de resposta</span><span class="sxs-lookup"><span data-stu-id="5f5c1-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="5f5c1-220">Fornece suporte para as respostas em cache.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-220">Provides support for caching responses.</span></span> | <span data-ttu-id="5f5c1-221">Antes de componentes que requerem armazenamento em cache.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="5f5c1-222">Compactação de resposta</span><span class="sxs-lookup"><span data-stu-id="5f5c1-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="5f5c1-223">Fornece suporte para a compactação de respostas.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="5f5c1-224">Antes de componentes que requerem a compactação.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="5f5c1-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="5f5c1-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="5f5c1-226">Fornece suporte de localização.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-226">Provides localization support.</span></span> | <span data-ttu-id="5f5c1-227">Antes de componentes importantes da localização.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="5f5c1-228">Roteamento</span><span class="sxs-lookup"><span data-stu-id="5f5c1-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="5f5c1-229">Define e restringe as rotas de solicitação.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="5f5c1-230">Terminal de rotas correspondentes.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="5f5c1-231">Sessão</span><span class="sxs-lookup"><span data-stu-id="5f5c1-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="5f5c1-232">Fornece suporte para gerenciar sessões de usuário.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="5f5c1-233">Antes de componentes que requerem a sessão.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="5f5c1-234">Arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="5f5c1-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="5f5c1-235">Fornece suporte para servir arquivos estáticos e pesquisa no diretório.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="5f5c1-236">Terminal se corresponder a uma solicitação de arquivos.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="5f5c1-237">Regravação de URL</span><span class="sxs-lookup"><span data-stu-id="5f5c1-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="5f5c1-238">Fornece suporte para URLs de regravação e redirecionar solicitações.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="5f5c1-239">Antes dos componentes que consomem a URL.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="5f5c1-240">WebSockets</span><span class="sxs-lookup"><span data-stu-id="5f5c1-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="5f5c1-241">Permite que o protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="5f5c1-242">Antes de componentes que são necessárias para aceitar solicitações de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="5f5c1-243">Middleware de gravação</span><span class="sxs-lookup"><span data-stu-id="5f5c1-243">Writing middleware</span></span>

<span data-ttu-id="5f5c1-244">Middleware geralmente é encapsulado em uma classe e exposto com um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="5f5c1-245">Considere o middleware a seguir, que define a cultura para a solicitação atual da cadeia de consulta:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="5f5c1-246">Observação: O código de exemplo acima é usado para demonstrar a criação de um componente de middleware.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="5f5c1-247">Consulte [ globalização e localização](xref:fundamentals/localization) para suporte de localização interna do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="5f5c1-248">Você pode testar o middleware passando na cultura, por exemplo `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="5f5c1-249">O código a seguir move o delegado de middleware para uma classe:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="5f5c1-250">O seguinte método de extensão expõe o middleware por meio de [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="5f5c1-250">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="5f5c1-251">O código a seguir chama o middleware de `Configure`:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-251">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="5f5c1-252">Middleware deve seguir o [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/) expondo suas dependências em seu construtor.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-252">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="5f5c1-253">Middleware é construído uma vez por *tempo de vida do aplicativo*.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-253">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="5f5c1-254">Consulte *dependências por solicitação* abaixo se você precisa compartilhar serviços com middleware dentro de uma solicitação.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-254">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="5f5c1-255">Componentes de middleware podem resolver as dependências de injeção de dependência por meio de parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-255">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="5f5c1-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)também pode aceitar parâmetros adicionais diretamente.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="5f5c1-257">Dependências por solicitação</span><span class="sxs-lookup"><span data-stu-id="5f5c1-257">Per-request dependencies</span></span>

<span data-ttu-id="5f5c1-258">Porque o middleware é construído durante a inicialização do aplicativo, não por solicitação, *escopo* serviços de tempo de vida usados pelo middleware construtores não são compartilhados com outros tipos de dependência inseridos durante cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-258">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="5f5c1-259">Se você deve compartilhar uma *escopo* entre seu middleware e outros tipos de serviço, adicione esses serviços para o `Invoke` assinatura do método.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-259">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="5f5c1-260">O `Invoke` método pode aceitar parâmetros adicionais que são preenchidos, injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="5f5c1-260">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="5f5c1-261">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5f5c1-261">For example:</span></span>

```c#
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

## <a name="resources"></a><span data-ttu-id="5f5c1-262">Recursos</span><span class="sxs-lookup"><span data-stu-id="5f5c1-262">Resources</span></span>

* [<span data-ttu-id="5f5c1-263">Código de exemplo usado deste documento</span><span class="sxs-lookup"><span data-stu-id="5f5c1-263">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="5f5c1-264">Migrando módulos HTTP para Middleware</span><span class="sxs-lookup"><span data-stu-id="5f5c1-264">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="5f5c1-265">Inicialização de aplicativos</span><span class="sxs-lookup"><span data-stu-id="5f5c1-265">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="5f5c1-266">Recursos de solicitação</span><span class="sxs-lookup"><span data-stu-id="5f5c1-266">Request Features</span></span>](request-features.md)
