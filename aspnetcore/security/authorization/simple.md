---
title: "Simples de autorização"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: 91f402b1cbf73e212418d197a8a7230ce22e9e1d
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="simple-authorization"></a>Simples de autorização

<a name="security-authorization-simple"></a>

No MVC é controlada por meio de `AuthorizeAttribute` atributo e seus vários parâmetros. Em sua aplicação mais simples de `AuthorizeAttribute` de atributo para um controlador ou ação limites acesse o controlador ou ação para qualquer usuário autenticado.

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

Se você deseja aplicar a autorização para uma ação em vez do controlador simplesmente aplicar o `AuthorizeAttribute` de atributo para a ação em si;

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

Somente usuários autenticados podem acessar a função de logout.

Você também pode usar o `AllowAnonymousAttribute` atributo para permitir o acesso por usuários não autenticados para ações individuais; por exemplo

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
