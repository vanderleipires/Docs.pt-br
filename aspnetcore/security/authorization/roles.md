---
title: "Autorização baseada em função"
author: rick-anderson
description: "Este documento demonstra como restringir o acesso de controlador e ação do ASP.NET Core passando funções para autorizar atributo."
keywords: "ASP.NET Core, autorização, funções"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5e014da1-8bc0-409b-951a-88b92c661fdf
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: 649b21d99c742843534748b0ba9d7b7b22483a62
ms.sourcegitcommit: 703593d5fd14076e79be2ba75a5b8da12a60ab15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2017
---
# <a name="role-based-authorization"></a><span data-ttu-id="01984-104">Autorização baseada em função</span><span class="sxs-lookup"><span data-stu-id="01984-104">Role based Authorization</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="01984-105">Quando uma identidade é criada ele pode pertencer a uma ou mais funções.</span><span class="sxs-lookup"><span data-stu-id="01984-105">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="01984-106">Por exemplo, Tânia pode pertencer a funções de administrador e usuário embora Scott só pode pertencer à função de usuário.</span><span class="sxs-lookup"><span data-stu-id="01984-106">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="01984-107">Como essas funções são criadas e gerenciadas dependem do armazenamento de backup do processo de autorização.</span><span class="sxs-lookup"><span data-stu-id="01984-107">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="01984-108">Funções são expostas para o desenvolvedor de [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) método o [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) classe.</span><span class="sxs-lookup"><span data-stu-id="01984-108">Roles are exposed to the developer through the [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="01984-109">Adicionando verificações de função</span><span class="sxs-lookup"><span data-stu-id="01984-109">Adding role checks</span></span>

<span data-ttu-id="01984-110">Verificações de autorização baseada em função são declarativas&mdash;o desenvolvedor incorpora dentro de seu código, em relação a um controlador ou uma ação dentro de um controlador, especificando funções que o usuário atual deve ser um membro de acessar o recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="01984-110">Role based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="01984-111">Por exemplo, o código a seguir deve limitar o acesso a todas as ações no `AdministrationController` para os usuários que são membros do `Administrator` grupo.</span><span class="sxs-lookup"><span data-stu-id="01984-111">For example, the following code would limit access to any actions on the `AdministrationController` to users who are a member of the `Administrator` group.</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="01984-112">Você pode especificar várias funções como uma lista separada por vírgulas:</span><span class="sxs-lookup"><span data-stu-id="01984-112">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="01984-113">Esse controlador seria somente acessível por usuários que são membros do `HRManager` função ou o `Finance` função.</span><span class="sxs-lookup"><span data-stu-id="01984-113">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="01984-114">Se você aplicar vários atributos de um usuário ao acessar deve ser um membro de todas as funções especificadas; o exemplo a seguir exige que um usuário deve ser um membro de ambos os `PowerUser` e `ControlPanelUser` função.</span><span class="sxs-lookup"><span data-stu-id="01984-114">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="01984-115">Você pode limitar o acesso mais aplicando atributos de autorização de função adicionais no nível da ação:</span><span class="sxs-lookup"><span data-stu-id="01984-115">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

<span data-ttu-id="01984-116">Em membros de trecho de código anterior do `Administrator` função ou o `PowerUser` função pode acessar o controlador e o `SetTime` ação, mas somente os membros do `Administrator` função pode acessar o `ShutDown` ação.</span><span class="sxs-lookup"><span data-stu-id="01984-116">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="01984-117">Você também pode bloquear um controlador mas permitir o acesso anônimo, não autenticado para ações individuais.</span><span class="sxs-lookup"><span data-stu-id="01984-117">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="01984-118">Verificações de função baseada em política</span><span class="sxs-lookup"><span data-stu-id="01984-118">Policy based role checks</span></span>

<span data-ttu-id="01984-119">Requisitos da função também podem ser expressos usando a nova sintaxe de diretiva, onde um desenvolvedor registra uma política na inicialização como parte da configuração do serviço de autorização.</span><span class="sxs-lookup"><span data-stu-id="01984-119">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="01984-120">Isso normalmente ocorre em `ConfigureServices()` no seu *Startup.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="01984-120">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

<span data-ttu-id="01984-121">As políticas são aplicadas usando o `Policy` propriedade o `AuthorizeAttribute` atributo:</span><span class="sxs-lookup"><span data-stu-id="01984-121">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="01984-122">Se desejar especificar várias funções permitidas em um requisito, você pode especificá-los como parâmetros para o `RequireRole` método:</span><span class="sxs-lookup"><span data-stu-id="01984-122">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="01984-123">Este exemplo autoriza a usuários que pertencem ao `Administrator`, `PowerUser` ou `BackupAdministrator` funções.</span><span class="sxs-lookup"><span data-stu-id="01984-123">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
