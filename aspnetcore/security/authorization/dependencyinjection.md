---
title: "Injeção de dependência em manipuladores de requisito"
author: rick-anderson
description: "Este documento descreve como injetar manipuladores de requisito de autorização em um aplicativo do ASP.NET Core usando a injeção de dependência."
keywords: "ASP.NET Core, injeção de dependência, o manipulador de autorização"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: b5e590cc63387553af7385b611cdf8cd6b255db7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="913b9-104">Injeção de dependência em manipuladores de requisito</span><span class="sxs-lookup"><span data-stu-id="913b9-104">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="913b9-105">[Manipuladores de autorização devem ser registrados](policies.md#handler-registration) na coleção durante a configuração do serviço (usando [injeção de dependência](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="913b9-105">[Authorization handlers must be registered](policies.md#handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="913b9-106">Suponha que você tenha um repositório de regras que você deseja avaliar dentro de um manipulador de autorização e esse repositório foi registrado na coleção de serviço.</span><span class="sxs-lookup"><span data-stu-id="913b9-106">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="913b9-107">A autorização será resolver e injetar que seu construtor.</span><span class="sxs-lookup"><span data-stu-id="913b9-107">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="913b9-108">Por exemplo, se você quiser usar o ASP. NET do log de infraestrutura que você deseja inserir `ILoggerFactory` para o manipulador.</span><span class="sxs-lookup"><span data-stu-id="913b9-108">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="913b9-109">Tal um manipulador pode parecer com:</span><span class="sxs-lookup"><span data-stu-id="913b9-109">Such a handler might look like:</span></span>

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

<span data-ttu-id="913b9-110">Você deve registrar o manipulador com `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="913b9-110">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="913b9-111">Uma instância do manipulador será criado quando o aplicativo é iniciado e será DI injetar registrado `ILoggerFactory` para seu construtor.</span><span class="sxs-lookup"><span data-stu-id="913b9-111">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="913b9-112">Manipuladores que usam o Entity Framework não devem ser registrados como singletons.</span><span class="sxs-lookup"><span data-stu-id="913b9-112">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
