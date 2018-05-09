---
title: Migrar do ClaimsPrincipal.Current
author: mjrousos
description: Saiba como afastar ClaimsPrincipal.Current para recuperar declarações no núcleo do ASP.NET e a identidade do usuário autenticado atual.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/claimsprincipal-current
ms.openlocfilehash: ea43d17e76380baf57cd9debbc508e8812cfa4a6
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migrar do ClaimsPrincipal.Current

Em projetos do ASP.NET, é comum usar [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) recuperar atual autenticado a identidade do usuário e declarações. No núcleo do ASP.NET, esta propriedade não está definida. Código que foi dependem precisa ser atualizado para obter a identidade do usuário autenticado atual por meio de uma maneira diferente.

## <a name="context-specific-data-instead-of-static-data"></a>Dados de contexto específico em vez de dados estáticos

Ao usar o ASP.NET Core, os valores de `ClaimsPrincipal.Current` e `Thread.CurrentPrincipal` não estão definidos. Essas propriedades representam o estado estático, que normalmente evita o ASP.NET Core. Em vez disso, arquitetura do ASP.NET Core é recuperar dependências (como a identidade do usuário atual) de coleções de contexto específico de serviço (usando sua [injeção de dependência](xref:fundamentals/dependency-injection) modelo (DI)). Além disso, `Thread.CurrentPrincipal` é thread estático, portanto ele não pode persistir as alterações em alguns cenários assíncronas (e `ClaimsPrincipal.Current` apenas chama `Thread.CurrentPrincipal` por padrão).

Para entender as classificações do thread de problemas de membros estáticos podem levar a cenários assíncronos, considere o seguinte trecho de código:

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

Os conjuntos de código de exemplo anterior `Thread.CurrentPrincipal` e verifica seu valor antes e depois aguardando uma chamada assíncrona. `Thread.CurrentPrincipal` é específico para o *thread* no qual ele está definido e o método provavelmente retomar a execução em um thread diferente após o await. Consequentemente, `Thread.CurrentPrincipal` está presente quando ele é verificado, mas nulo após a chamada a `await Task.Yield()`.

Obter a identidade do usuário atual da coleção de serviço de injeção de dependência do aplicativo é mais testável, muito, desde que as identidades de teste podem ser facilmente inseridas.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Recuperar o usuário atual em um aplicativo do ASP.NET Core

Há várias opções para recuperar o atual usuário autenticado `ClaimsPrincipal` no ASP.NET Core no lugar de `ClaimsPrincipal.Current`:

* **ControllerBase.User**. Controladores MVC podem acessar o usuário autenticado atual com seus [usuário](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) propriedade.
* **HttpContext**. Componentes com acesso ao atual `HttpContext` (middleware, por exemplo) pode obter o usuário atual `ClaimsPrincipal` de [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Passado do chamador**. Bibliotecas sem acesso ao atual `HttpContext` são chamados de controladores ou componentes de middleware e pode ter a identidade do usuário atual passada como um argumento.
* **IHttpContextAccessor**. O projeto ASP.NET que estão sendo migrado para o ASP.NET Core pode ser muito grande para passar facilmente a identidade do usuário atual para todos os locais necessários. Nesses casos, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) pode ser usado como uma solução alternativa. `IHttpContextAccessor` é capaz de acessar atual `HttpContext` (se houver). Uma solução de curto prazo para obter a identidade do usuário atual no código que ainda não foi atualizado para funcionar com a arquitetura de controlado por DI do ASP.NET Core seria:

  * Verifique `IHttpContextAccessor` disponível no contêiner de DI chamando [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) em `Startup.ConfigureServices`.
  * Obter uma instância de `IHttpContextAccessor` durante a inicialização e armazená-lo em uma variável estática. A instância é disponibilizada para o código que anteriormente estava recuperando o usuário atual de uma propriedade estática.
  * Recuperar o usuário atual `ClaimsPrincipal` usando `HttpContextAccessor.HttpContext?.User`. Se esse código for usado fora do contexto de uma solicitação HTTP, o `HttpContext` é nulo.

O último opção usando `IHttpContextAccessor`, infringir princípios do ASP.NET Core (preferindo dependências injetadas dependências estáticas). Planejar eventualmente remover a dependência estática `IHttpContextAccessor` auxiliar. Pode ser uma ponte útil, no entanto, ao migrar grandes aplicativos ASP.NET existentes que foram anteriormente usando `ClaimsPrincipal.Current`.
