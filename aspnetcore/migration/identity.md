---
title: "Migrando de autenticação e identidade"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0db145cb-41a5-448a-b889-72e2d789ad7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/identity
ms.openlocfilehash: 6972329e3d434e1b67131843118f2f18229807b9
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="migrating-authentication-and-identity"></a>Migrando de autenticação e identidade

<a name="migration-identity"></a>

Por [Steve Smith](https://ardalis.com/)

No anterior do artigo é [migrados de configuração de um projeto ASP.NET MVC para MVC do ASP.NET Core](configuration.md). Neste artigo, migrar os recursos de gerenciamento de registro, logon e usuário.

## <a name="configure-identity-and-membership"></a>Defina a identidade e associação

No ASP.NET MVC, os recursos de autenticação e identidade são configurados usando a identidade do ASP.NET em Startup.Auth.cs e IdentityConfig.cs, localizado na pasta App_Start. No ASP.NET MVC de núcleo, esses recursos são configurados no *Startup.cs*.

Instalar o `Microsoft.AspNetCore.Identity.EntityFrameworkCore` e `Microsoft.AspNetCore.Authentication.Cookies` pacotes do NuGet.

Em seguida, abra Startup.cs e atualize o `ConfigureServices()` método para usar os serviços de identidade do Entity Framework:

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

Neste ponto, há dois tipos referenciados no código acima que nós ainda não foram migrados do projeto ASP.NET MVC: `ApplicationDbContext` e `ApplicationUser`. Criar um novo *modelos* pasta no ASP.NET Core do projeto e adicionar duas classes a ele correspondente a esses tipos. Você encontrará o ASP.NET MVC versões dessas classes em `/Models/IdentityModels.cs`, mas vamos usar um arquivo por classe no projeto migrado porque é mais clara.

ApplicationUser.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

ApplicationDbContext.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

O projeto da Web do ASP.NET Core MVC Starter não inclui mais personalização de usuários ou o ApplicationDbContext. Ao migrar um aplicativo real, você também precisará migrar todas as propriedades personalizadas e métodos de classes de DbContext e usuário do seu aplicativo, bem como quaisquer outras classes de modelo que utiliza o seu aplicativo (por exemplo, se o DbContext tem um DbSet<Album>, obviamente você precisará migrar a classe álbum).

Com esses arquivos no local, o arquivo Startup.cs pode ser feito para compilar atualizando seu usando instruções:

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

Nosso aplicativo agora está pronto para dar suporte a serviços de autenticação e identidade – ele só precisa ter esses recursos expostos aos usuários.

## <a name="migrate-registration-and-login-logic"></a>Migrar o registro e a lógica de logon

Com os serviços de identidade configurados para o aplicativo e o acesso a dados configurado usando o Entity Framework e o SQL Server, você agora está pronto para adicionar suporte para registro e logon para o aplicativo. Lembre-se de que [anterior no processo de migração](mvc.md#migrate-layout-file) é comentada uma referência para loginpartial no layout. cshtml. Agora é hora para retornar a esse código, remova os comentários e adicione os controladores de necessários e modos de exibição para dar suporte à funcionalidade de logon.

Atualizar layout. cshtml; Remova o @Html.Partial linha:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Agora, adicione uma nova página de exibição MVC chamado loginpartial para a pasta exibições/compartilhadas:

Atualizar loginpartial. cshtml com o código a seguir (substitua todo o seu conteúdo):

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

Neste ponto, você deve ser capaz de atualizar o site no navegador.

## <a name="summary"></a>Resumo

ASP.NET Core introduz alterações para os recursos de identidade do ASP.NET. Neste artigo, você viu como migrar os recursos de gerenciamento de usuário e autenticação de uma identidade do ASP.NET para o ASP.NET Core.
