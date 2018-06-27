---
title: Simples de autorização no núcleo do ASP.NET
author: rick-anderson
description: Saiba como usar o atributo de autorização para restringir o acesso a ações e controladores do ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961117"
---
# <a name="simple-authorization-in-aspnet-core"></a>Simples de autorização no núcleo do ASP.NET

<a name="security-authorization-simple"></a>

No MVC é controlada por meio de `AuthorizeAttribute` atributo e seus vários parâmetros. Em sua forma mais simples, aplicando o `AuthorizeAttribute` de atributo para um controlador ou ação limites acesse o controlador ou ação para qualquer usuário autenticado.

Por exemplo, o código a seguir limita o acesso para o `AccountController` para qualquer usuário autenticado.

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

Se você deseja aplicar a autorização para uma ação em vez do controlador, aplique a `AuthorizeAttribute` de atributo para a ação em si:

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

Agora, somente usuários autenticados podem acessar o `Logout` função.

Você também pode usar o `AllowAnonymous` atributo para permitir o acesso por usuários não autenticados para ações individuais. Por exemplo:

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

Isso permitiria que somente usuários autenticados para o `AccountController`, exceto para o `Login` ação, que pode ser acessada por todos os usuários, independentemente de seu status de autenticado ou anônimo / não autenticado.

> [!WARNING]
> `[AllowAnonymous]` Ignora todas as declarações de autorização. Se você combinar `[AllowAnonymous]` e `[Authorize]` atributo, o `[Authorize]` atributos são ignorados. Por exemplo, se você aplicar `[AllowAnonymous]` no nível do controlador, qualquer `[Authorize]` atributos no mesmo controlador (ou em qualquer ação dentro dele) é ignorada.
