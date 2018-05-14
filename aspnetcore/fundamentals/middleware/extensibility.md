---
title: Ativação de middleware baseada em alocador no ASP.NET Core
author: guardrex
description: Saiba como usar um middleware fortemente tipado com uma implementação de ativação baseada em alocador no ASP.NET Core.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 76ba257abfb11e0c2950b974f837c6ae5818a6a1
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>Ativação de middleware baseada em alocador no ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) é um ponto de extensibilidade para a ativação de [middleware](xref:fundamentals/middleware/index).

Os métodos de extensão `UseMiddleware` verificam se o tipo registrado de um middleware implementa `IMiddleware`. Se isso acontecer, a instância `IMiddlewareFactory` registrada no contêiner será usada para resolver a implementação `IMiddleware` em vez de usar a lógica de ativação de middleware baseada em convenção. O middleware é registrado como um serviço com escopo ou transitório no contêiner de serviço do aplicativo.

Benefícios:

* Ativação por solicitação (injeção de serviços com escopo)
* Tipagem forte de middleware

`IMiddleware` é ativado por solicitação, de modo que os serviços com escopo possam ser injetados no construtor do middleware.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

O aplicativo de exemplo demonstra o middleware ativado por:

* Convenção. Para obter mais informações sobre a ativação de middleware convencional, consulte o tópico [Middleware](xref:fundamentals/middleware/index).
* Uma implementação de [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware). A [classe MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) padrão ativa o middleware.

As implementações de middleware funcionam de forma idêntica e registram o valor fornecido por um parâmetro de cadeia de caracteres de consulta (`key`). Os middlewares usam um contexto de banco de dados injetado (um serviço com escopo) para registrar o valor de cadeia de caracteres de consulta em um banco de dados em memória.

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) define o middleware para o pipeline de solicitação do aplicativo. O método [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) manipula as solicitações e retorna uma `Task` que representa a execução do middleware.

Middleware ativado por convenção:

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Middleware ativado por `MiddlewareFactory`:

[!code-csharp[](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

As extensões são criadas para os middlewares:

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Não é possível passar objetos para o middleware ativado por alocador com `UseMiddleware`:

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

O middleware ativado por alocador é adicionado ao contêiner interno em *Startup.cs*:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

Ambos os middlewares estão registrados no pipeline de processamento de solicitação em `Configure`:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) fornece métodos para a criação do middleware. A implementação de alocador do middleware é registrada no contêiner como um serviço com escopo.

A implementação `IMiddlewareFactory` padrão, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), foi encontrada no pacote [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) ([fonte de referência](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).

## <a name="additional-resources"></a>Recursos adicionais

* [Middleware](xref:fundamentals/middleware/index)
* [Ativação de middleware com um contêiner de terceiros](xref:fundamentals/middleware/extensibility-third-party-container)
