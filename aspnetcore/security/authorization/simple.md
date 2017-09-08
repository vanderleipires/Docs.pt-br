---
title: "Simples de autorização"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: 013ce0d9ac1e9c1b6bb541b9fa66218c3fd799bb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="simple-authorization"></a><span data-ttu-id="8a5ca-103">Simples de autorização</span><span class="sxs-lookup"><span data-stu-id="8a5ca-103">Simple Authorization</span></span>

<a name=security-authorization-simple></a>

<span data-ttu-id="8a5ca-104">No MVC é controlada por meio de `AuthorizeAttribute` atributo e seus vários parâmetros.</span><span class="sxs-lookup"><span data-stu-id="8a5ca-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="8a5ca-105">Em sua aplicação mais simples de `AuthorizeAttribute` de atributo para um controlador ou ação limites acesse o controlador ou ação para qualquer usuário autenticado.</span><span class="sxs-lookup"><span data-stu-id="8a5ca-105">At its simplest applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="8a5ca-106">Por exemplo, o código a seguir limita o acesso para o `AccountController` para qualquer usuário autenticado.</span><span class="sxs-lookup"><span data-stu-id="8a5ca-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="8a5ca-107">Se você deseja aplicar a autorização para uma ação em vez do controlador simplesmente aplicar o `AuthorizeAttribute` de atributo para a ação em si;</span><span class="sxs-lookup"><span data-stu-id="8a5ca-107">If you want to apply authorization to an action rather than the controller simply apply the `AuthorizeAttribute` attribute to the action itself;</span></span>

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

<span data-ttu-id="8a5ca-108">Somente usuários autenticados podem acessar a função de logout.</span><span class="sxs-lookup"><span data-stu-id="8a5ca-108">Now only authenticated users can access the logout function.</span></span>

<span data-ttu-id="8a5ca-109">Você também pode usar o `AllowAnonymousAttribute` atributo para permitir o acesso por usuários não autenticados para ações individuais; por exemplo</span><span class="sxs-lookup"><span data-stu-id="8a5ca-109">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions; for example</span></span>

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

<span data-ttu-id="8a5ca-110">Isso permitiria que somente usuários autenticados para o `AccountController`, exceto para o `Login` ação, que pode ser acessada por todos os usuários, independentemente de seu status de autenticado ou anônimo / não autenticado.</span><span class="sxs-lookup"><span data-stu-id="8a5ca-110">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="8a5ca-111">`[AllowAnonymous]`Ignora todas as declarações de autorização.</span><span class="sxs-lookup"><span data-stu-id="8a5ca-111">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="8a5ca-112">Se você aplicar combinar `[AllowAnonymous]` e qualquer `[Authorize]` atributo e os atributos de autorizar sempre serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="8a5ca-112">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="8a5ca-113">Por exemplo, se você aplicar `[AllowAnonymous]` no controlador de nível qualquer `[Authorize]` atributos no mesmo controlador, ou em qualquer ação dentro dele serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="8a5ca-113">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
