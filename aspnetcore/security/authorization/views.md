---
title: "Autorização baseada em modo de exibição no ASP.NET MVC de núcleo"
author: rick-anderson
description: "Este documento demonstra como injetar e utilizar o serviço de autorização dentro de um modo de exibição Razor do ASP.NET Core."
keywords: "ASP.NET Core, autorização de Razor de autorização, IAuthorizationService,"
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 756431f398c29376ab0ecd6c4f4d1db4f8022b0b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="view-based-authorization"></a>Autorização baseada em modo de exibição

Um desenvolvedor quer geralmente Mostrar, ocultar ou modificar uma interface do usuário com base na identidade do usuário atual. Você pode acessar o serviço de autorização em modos de exibição do MVC por meio de [injeção de dependência](xref:fundamentals/dependency-injection#fundamentals-dependency-injection). Para injetar o serviço de autorização em um modo de exibição Razor, use o `@inject` diretiva:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Se você deseja que o serviço de autorização em cada exibição, coloque o `@inject` diretiva para o *viewimports. cshtml* arquivo do *exibições* diretório. Para obter mais informações, consulte [injeção de dependência para modos de exibição](xref:mvc/views/dependency-injection).

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
