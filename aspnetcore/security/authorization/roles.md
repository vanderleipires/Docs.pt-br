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
# <a name="role-based-authorization"></a>Autorização baseada em função

<a name="security-authorization-role-based"></a>

Quando uma identidade é criada ele pode pertencer a uma ou mais funções. Por exemplo, Tânia pode pertencer a funções de administrador e usuário embora Scott só pode pertencer à função de usuário. Como essas funções são criadas e gerenciadas dependem do armazenamento de backup do processo de autorização. Funções são expostas para o desenvolvedor de [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) método o [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) classe.

## <a name="adding-role-checks"></a>Adicionando verificações de função

Verificações de autorização baseada em função são declarativas&mdash;o desenvolvedor incorpora dentro de seu código, em relação a um controlador ou uma ação dentro de um controlador, especificando funções que o usuário atual deve ser um membro de acessar o recurso solicitado.

Por exemplo, o código a seguir deve limitar o acesso a todas as ações no `AdministrationController` para os usuários que são membros do `Administrator` grupo.

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

Você pode especificar várias funções como uma lista separada por vírgulas:

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

Esse controlador seria somente acessível por usuários que são membros do `HRManager` função ou o `Finance` função.

Se você aplicar vários atributos de um usuário ao acessar deve ser um membro de todas as funções especificadas; o exemplo a seguir exige que um usuário deve ser um membro de ambos os `PowerUser` e `ControlPanelUser` função.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

Você pode limitar o acesso mais aplicando atributos de autorização de função adicionais no nível da ação:

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

Em membros de trecho de código anterior do `Administrator` função ou o `PowerUser` função pode acessar o controlador e o `SetTime` ação, mas somente os membros do `Administrator` função pode acessar o `ShutDown` ação.

Você também pode bloquear um controlador mas permitir o acesso anônimo, não autenticado para ações individuais.

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

## <a name="policy-based-role-checks"></a>Verificações de função baseada em política

Requisitos da função também podem ser expressos usando a nova sintaxe de diretiva, onde um desenvolvedor registra uma política na inicialização como parte da configuração do serviço de autorização. Isso normalmente ocorre em `ConfigureServices()` no seu *Startup.cs* arquivo.

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

As políticas são aplicadas usando o `Policy` propriedade o `AuthorizeAttribute` atributo:

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

Se desejar especificar várias funções permitidas em um requisito, você pode especificá-los como parâmetros para o `RequireRole` método:

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

Este exemplo autoriza a usuários que pertencem ao `Administrator`, `PowerUser` ou `BackupAdministrator` funções.
