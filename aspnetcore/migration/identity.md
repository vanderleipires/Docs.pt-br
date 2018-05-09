---
title: Migrar de autenticação e identidade para o ASP.NET Core
author: ardalis
description: Saiba como migrar de autenticação e identidade de um projeto ASP.NET MVC para um projeto MVC do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: 2a80274e9056b41e370f199c7d41865db5fcedd7
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Migrar de autenticação e identidade para o ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

O artigo anterior, nós [migrados de configuração de um projeto ASP.NET MVC para MVC do ASP.NET Core](xref:migration/configuration). Neste artigo, migrar os recursos de gerenciamento de registro, logon e usuário.

## <a name="configure-identity-and-membership"></a>Defina a identidade e associação

No ASP.NET MVC, recursos de autenticação e identidade são configurados usando a identidade do ASP.NET em *Startup.Auth.cs* e *IdentityConfig.cs*, localizado no *App_Start* pasta. No ASP.NET MVC de núcleo, esses recursos são configurados no *Startup.cs*.

Instalar o `Microsoft.AspNetCore.Identity.EntityFrameworkCore` e `Microsoft.AspNetCore.Authentication.Cookies` pacotes do NuGet.

Em seguida, abra *Startup.cs* e atualize o `Startup.ConfigureServices` método para usar os serviços de identidade do Entity Framework:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

Neste ponto, há dois tipos referenciados no código acima que nós ainda não foram migrados do projeto ASP.NET MVC: `ApplicationDbContext` e `ApplicationUser`. Criar um novo *modelos* pasta no ASP.NET Core do projeto e adicionar duas classes a ele correspondente a esses tipos. Você encontrará o ASP.NET MVC versões dessas classes em */Models/IdentityModels.cs*, mas vamos usar um arquivo por classe no projeto migrado porque é mais clara.

*ApplicationUser.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

O projeto da Web do ASP.NET Core MVC Starter não inclui mais personalização de usuários, ou o `ApplicationDbContext`. Ao migrar um aplicativo real, você também precisa migrar todas as propriedades personalizadas e métodos do usuário do aplicativo e `DbContext` classes, bem como quaisquer outras classes de modelo que utiliza seu aplicativo. Por exemplo, se seu `DbContext` tem um `DbSet<Album>`, você precisa para migrar o `Album` classe.

Com esses arquivos no local, o *Startup.cs* arquivo pode ser feito para compilar atualizando seu `using` instruções:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Nosso aplicativo agora está pronto para dar suporte a serviços de autenticação e identidade. Ele só precisa ter esses recursos expostos aos usuários.

## <a name="migrate-registration-and-login-logic"></a>Migrar a lógica de registro e logon

Com os serviços de identidade configurados para o aplicativo e o acesso a dados configurado usando o Entity Framework e o SQL Server, estamos prontos para adicionar suporte para registro e logon para o aplicativo. Lembre-se de que [anterior no processo de migração](xref:migration/mvc#migrate-the-layout-file) é comentada uma referência a *loginpartial* na *cshtml*. Agora é hora para retornar a esse código, remova os comentários e adicione os controladores de necessários e modos de exibição para dar suporte à funcionalidade de logon.

Remova o `@Html.Partial` linha *cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Agora, adicione um novo modo de exibição Razor chamado *loginpartial* para o *exibições/compartilhadas* pasta:

Atualização *loginpartial. cshtml* com o código a seguir (substitua todo o seu conteúdo):

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
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

ASP.NET Core introduz alterações para os recursos de identidade do ASP.NET. Neste artigo, você viu como migrar os recursos de gerenciamento de usuário e autenticação de identidade do ASP.NET para o ASP.NET Core.
