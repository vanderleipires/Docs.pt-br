---
title: "Injeção de dependência em manipuladores de requisito"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 37d197d7696a6e91fa236b2defc577959c95c49f
ms.sourcegitcommit: 0a70706a3814d2684f3ff96095d1e8291d559cc7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a>Injeção de dependência em manipuladores de requisito

<a name=security-authorization-di></a>

[Manipuladores de autorização devem ser registrados](policies.md#security-authorization-policies-based-handler-registration) na coleção durante a configuração do serviço (usando [injeção de dependência](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).

Suponha que você tenha um repositório de regras que você deseja avaliar dentro de um manipulador de autorização e esse repositório foi registrado na coleção de serviço.  A autorização será resolver e injetar que seu construtor.

Por exemplo, se você quiser usar o ASP. NET do log de infraestrutura que você deseja inserir `ILoggerFactory` para o manipulador. Tal um manipulador pode parecer com:

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

Você deve registrar o manipulador com `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
   ```

Uma instância do manipulador será criado quando o aplicativo é iniciado e será DI injetar registrado `ILoggerFactory` para seu construtor.

> [!NOTE]
> Manipuladores que usam o Entity Framework não devem ser registrados como singletons.
