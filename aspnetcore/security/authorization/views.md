---
title: Autorização baseada em modo de exibição no ASP.NET MVC de núcleo
author: rick-anderson
description: Este documento demonstra como injetar e utilizar o serviço de autorização dentro de um modo de exibição Razor do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/views
ms.openlocfilehash: dad59a297efb4648755436fbd07742f95af97fb2
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30076627"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>Autorização baseada em modo de exibição no ASP.NET MVC de núcleo

Um desenvolvedor quer geralmente Mostrar, ocultar ou modificar uma interface do usuário com base na identidade do usuário atual. Você pode acessar o serviço de autorização em modos de exibição do MVC por meio de [injeção de dependência](xref:fundamentals/dependency-injection#fundamentals-dependency-injection). Para injetar o serviço de autorização em um modo de exibição Razor, use o `@inject` diretiva:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Se você deseja que o serviço de autorização em cada exibição, coloque o `@inject` diretiva para o *viewimports. cshtml* arquivo do *exibições* diretório. Para obter mais informações, consulte [Injeção de dependência em exibições](xref:mvc/views/dependency-injection).

Usar o serviço de autorização injetado para invocar `AuthorizeAsync` exatamente da mesma forma que você deve verificar durante [autorização baseada em recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

Em alguns casos, o recurso será o modelo de exibição. Invocar `AuthorizeAsync` exatamente da mesma forma que você deve verificar durante [autorização baseada em recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

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

No código anterior, o modelo é passado como um recurso de que avaliação de política deve levar em consideração.

> [!WARNING]
> Não confie na alternância de visibilidade de elementos de interface do usuário do aplicativo como a verificação de autorização exclusiva. Ocultar um elemento de interface do usuário pode completamente impede o acesso a sua ação de controlador associado. Por exemplo, considere o botão no trecho de código anterior. Um usuário pode invocar o `Edit` URL do método de ação se saiba o recurso relativo é */Document/Edit/1*. Por esse motivo, o `Edit` método de ação deve executar sua própria verificação de autorização.
