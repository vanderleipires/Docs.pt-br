---
title: Ativação de middleware baseada em alocador no ASP.NET Core
author: guardrex
description: Saiba como usar um middleware fortemente tipado com uma implementação de ativação baseada em alocador no ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 44987dbc20b0419865a23e64b60a5dc3f436743a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277066"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="0d08c-103">Ativação de middleware baseada em alocador no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d08c-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="0d08c-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0d08c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0d08c-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) é um ponto de extensibilidade para a ativação de [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="0d08c-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="0d08c-106">Os métodos de extensão `UseMiddleware` verificam se o tipo registrado de um middleware implementa `IMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="0d08c-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="0d08c-107">Se isso acontecer, a instância `IMiddlewareFactory` registrada no contêiner será usada para resolver a implementação `IMiddleware` em vez de usar a lógica de ativação de middleware baseada em convenção.</span><span class="sxs-lookup"><span data-stu-id="0d08c-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="0d08c-108">O middleware é registrado como um serviço com escopo ou transitório no contêiner de serviço do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0d08c-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="0d08c-109">Benefícios:</span><span class="sxs-lookup"><span data-stu-id="0d08c-109">Benefits:</span></span>

* <span data-ttu-id="0d08c-110">Ativação por solicitação (injeção de serviços com escopo)</span><span class="sxs-lookup"><span data-stu-id="0d08c-110">Activation per request (injection of scoped services)</span></span>
* <span data-ttu-id="0d08c-111">Tipagem forte de middleware</span><span class="sxs-lookup"><span data-stu-id="0d08c-111">Strong typing of middleware</span></span>

<span data-ttu-id="0d08c-112">`IMiddleware` é ativado por solicitação, de modo que os serviços com escopo possam ser injetados no construtor do middleware.</span><span class="sxs-lookup"><span data-stu-id="0d08c-112">`IMiddleware` is activated per request, so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="0d08c-113">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0d08c-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0d08c-114">O aplicativo de exemplo demonstra o middleware ativado por:</span><span class="sxs-lookup"><span data-stu-id="0d08c-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="0d08c-115">Convenção.</span><span class="sxs-lookup"><span data-stu-id="0d08c-115">Convention.</span></span> <span data-ttu-id="0d08c-116">Para obter mais informações sobre a ativação de middleware convencional, consulte o tópico [Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="0d08c-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="0d08c-117">Uma implementação de [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware).</span><span class="sxs-lookup"><span data-stu-id="0d08c-117">An [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="0d08c-118">A [classe MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) padrão ativa o middleware.</span><span class="sxs-lookup"><span data-stu-id="0d08c-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="0d08c-119">As implementações de middleware funcionam de forma idêntica e registram o valor fornecido por um parâmetro de cadeia de caracteres de consulta (`key`).</span><span class="sxs-lookup"><span data-stu-id="0d08c-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="0d08c-120">Os middlewares usam um contexto de banco de dados injetado (um serviço com escopo) para registrar o valor de cadeia de caracteres de consulta em um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="0d08c-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="0d08c-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="0d08c-121">IMiddleware</span></span>

<span data-ttu-id="0d08c-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) define o middleware para o pipeline de solicitação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0d08c-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="0d08c-123">O método [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) manipula as solicitações e retorna uma `Task` que representa a execução do middleware.</span><span class="sxs-lookup"><span data-stu-id="0d08c-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="0d08c-124">Middleware ativado por convenção:</span><span class="sxs-lookup"><span data-stu-id="0d08c-124">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="0d08c-125">Middleware ativado por `MiddlewareFactory`:</span><span class="sxs-lookup"><span data-stu-id="0d08c-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

<span data-ttu-id="0d08c-126">As extensões são criadas para os middlewares:</span><span class="sxs-lookup"><span data-stu-id="0d08c-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="0d08c-127">Não é possível passar objetos para o middleware ativado por alocador com `UseMiddleware`:</span><span class="sxs-lookup"><span data-stu-id="0d08c-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

<span data-ttu-id="0d08c-128">O middleware ativado por alocador é adicionado ao contêiner interno em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0d08c-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

<span data-ttu-id="0d08c-129">Ambos os middlewares estão registrados no pipeline de processamento de solicitação em `Configure`:</span><span class="sxs-lookup"><span data-stu-id="0d08c-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="0d08c-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="0d08c-130">IMiddlewareFactory</span></span>

<span data-ttu-id="0d08c-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) fornece métodos para a criação do middleware.</span><span class="sxs-lookup"><span data-stu-id="0d08c-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="0d08c-132">A implementação de alocador do middleware é registrada no contêiner como um serviço com escopo.</span><span class="sxs-lookup"><span data-stu-id="0d08c-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="0d08c-133">A implementação `IMiddlewareFactory` padrão, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), foi encontrada no pacote [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) ([fonte de referência](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span><span class="sxs-lookup"><span data-stu-id="0d08c-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package ([reference source](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d08c-134">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="0d08c-134">Additional resources</span></span>

* [<span data-ttu-id="0d08c-135">Middleware</span><span class="sxs-lookup"><span data-stu-id="0d08c-135">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="0d08c-136">Ativação de middleware com um contêiner de terceiros</span><span class="sxs-lookup"><span data-stu-id="0d08c-136">Middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
