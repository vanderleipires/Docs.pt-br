---
title: "Autorização baseada em modo de exibição"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 58cafcfdc7946e82d1e0ea5de95e0e497b1b6bcf
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="view-based-authorization"></a>Autorização baseada em modo de exibição

<a name="security-authorization-views"></a>

Geralmente um desenvolvedor deve mostrar, ocultar ou modificar uma interface do usuário com base na identidade do usuário atual. Você pode acessar o serviço de autorização em modos de exibição do MVC por meio de [injeção de dependência](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection). Injetar o serviço de autorização em um uso de exibição Razor o `@inject` diretiva, por exemplo `@inject IAuthorizationService AuthorizationService` (requer `@using Microsoft.AspNetCore.Authorization`). Se você quiser que o serviço de autorização em cada exibição, em seguida, coloque o `@inject` diretiva no `_ViewImports.cshtml` do arquivo no `Views` directory. Para obter mais informações sobre injeção de dependência para modos de exibição, consulte [injeção de dependência para modos de exibição](../../mvc/views/dependency-injection.md).

Depois que você tiver inserido o serviço de autorização usá-lo chamando o `AuthorizeAsync` método exatamente da mesma maneira que você deve verificar durante [autorização com base em recursos](resourcebased.md#security-authorization-resource-based-imperative).

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

Em alguns casos, o recurso será o modelo de exibição, e você pode chamar `AuthorizeAsync` em exatamente da mesma maneira que você deve verificar durante [autorização com base em recursos](resourcebased.md#security-authorization-resource-based-imperative);

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
   @if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
   @if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```
---

Aqui você pode ver que o modelo é passado como a autorização de recursos deve levar em consideração.

>[!WARNING]
>Não confie em Mostrar ou ocultar partes da sua interface do usuário, como seu único método de autorização. Ocultando uma interface de usuário elemento não significa que um usuário não pode acessá-lo. Você também deve autorizar o usuário dentro do seu código do controlador.
