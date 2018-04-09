---
title: Simples de autorização no núcleo do ASP.NET
author: rick-anderson
description: Saiba como usar o atributo de autorização para restringir o acesso a ações e controladores do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: cef5cb146c6c1ff052430748a9a64c6a822d6fa3
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="e78f4-103">Simples de autorização no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e78f4-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="e78f4-104">No MVC é controlada por meio de `AuthorizeAttribute` atributo e seus vários parâmetros.</span><span class="sxs-lookup"><span data-stu-id="e78f4-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="e78f4-105">Em sua forma mais simples, aplicando o `AuthorizeAttribute` de atributo para um controlador ou ação limites acesse o controlador ou ação para qualquer usuário autenticado.</span><span class="sxs-lookup"><span data-stu-id="e78f4-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="e78f4-106">Por exemplo, o código a seguir limita o acesso para o `AccountController` para qualquer usuário autenticado.</span><span class="sxs-lookup"><span data-stu-id="e78f4-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="e78f4-107">Se você deseja aplicar a autorização para uma ação em vez do controlador, aplique a `AuthorizeAttribute` de atributo para a ação em si:</span><span class="sxs-lookup"><span data-stu-id="e78f4-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="e78f4-108">Agora, somente usuários autenticados podem acessar o `Logout` função.</span><span class="sxs-lookup"><span data-stu-id="e78f4-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="e78f4-109">Você também pode usar o `AllowAnonymous` atributo para permitir o acesso por usuários não autenticados para ações individuais.</span><span class="sxs-lookup"><span data-stu-id="e78f4-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="e78f4-110">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e78f4-110">For example:</span></span>

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

<span data-ttu-id="e78f4-111">Isso permitiria que somente usuários autenticados para o `AccountController`, exceto para o `Login` ação, que pode ser acessada por todos os usuários, independentemente de seu status de autenticado ou anônimo / não autenticado.</span><span class="sxs-lookup"><span data-stu-id="e78f4-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="e78f4-112">`[AllowAnonymous]` Ignora todas as declarações de autorização.</span><span class="sxs-lookup"><span data-stu-id="e78f4-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="e78f4-113">Se você aplicar combinar `[AllowAnonymous]` e qualquer `[Authorize]` atributo e os atributos de autorizar sempre serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="e78f4-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="e78f4-114">Por exemplo, se você aplicar `[AllowAnonymous]` no controlador de nível qualquer `[Authorize]` atributos no mesmo controlador, ou em qualquer ação dentro dele serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="e78f4-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
