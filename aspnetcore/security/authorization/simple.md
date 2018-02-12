---
title: "Simples de autorização"
author: rick-anderson
description: "Este documento explica como usar o atributo de autorização para restringir o acesso a ações e controladores do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: 503ebc665efd460a85f49844ddc847eb12114308
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/11/2018
---
# <a name="simple-authorization"></a>Simples de autorização

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

>[!WARNING]
> `[AllowAnonymous]`Ignora todas as declarações de autorização. Se você aplicar combinar `[AllowAnonymous]` e qualquer `[Authorize]` atributo e os atributos de autorizar sempre serão ignorados. Por exemplo, se você aplicar `[AllowAnonymous]` no controlador de nível qualquer `[Authorize]` atributos no mesmo controlador, ou em qualquer ação dentro dele serão ignorados.
