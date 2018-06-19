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
ms.locfileid: "33851437"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="0b91f-103">Migrar de autenticação e identidade para o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b91f-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="0b91f-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0b91f-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0b91f-105">O artigo anterior, nós [migrados de configuração de um projeto ASP.NET MVC para MVC do ASP.NET Core](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="0b91f-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="0b91f-106">Neste artigo, migrar os recursos de gerenciamento de registro, logon e usuário.</span><span class="sxs-lookup"><span data-stu-id="0b91f-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="0b91f-107">Defina a identidade e associação</span><span class="sxs-lookup"><span data-stu-id="0b91f-107">Configure Identity and Membership</span></span>

<span data-ttu-id="0b91f-108">No ASP.NET MVC, recursos de autenticação e identidade são configurados usando a identidade do ASP.NET em *Startup.Auth.cs* e *IdentityConfig.cs*, localizado no *App_Start* pasta.</span><span class="sxs-lookup"><span data-stu-id="0b91f-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="0b91f-109">No ASP.NET MVC de núcleo, esses recursos são configurados no *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0b91f-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="0b91f-110">Instalar o `Microsoft.AspNetCore.Identity.EntityFrameworkCore` e `Microsoft.AspNetCore.Authentication.Cookies` pacotes do NuGet.</span><span class="sxs-lookup"><span data-stu-id="0b91f-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="0b91f-111">Em seguida, abra *Startup.cs* e atualize o `Startup.ConfigureServices` método para usar os serviços de identidade do Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="0b91f-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="0b91f-112">Neste ponto, há dois tipos referenciados no código acima que nós ainda não foram migrados do projeto ASP.NET MVC: `ApplicationDbContext` e `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="0b91f-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="0b91f-113">Criar um novo *modelos* pasta no ASP.NET Core do projeto e adicionar duas classes a ele correspondente a esses tipos.</span><span class="sxs-lookup"><span data-stu-id="0b91f-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="0b91f-114">Você encontrará o ASP.NET MVC versões dessas classes em */Models/IdentityModels.cs*, mas vamos usar um arquivo por classe no projeto migrado porque é mais clara.</span><span class="sxs-lookup"><span data-stu-id="0b91f-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="0b91f-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="0b91f-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="0b91f-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="0b91f-116">*ApplicationDbContext.cs*:</span></span>

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

<span data-ttu-id="0b91f-117">O projeto da Web do ASP.NET Core MVC Starter não inclui mais personalização de usuários, ou o `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="0b91f-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="0b91f-118">Ao migrar um aplicativo real, você também precisa migrar todas as propriedades personalizadas e métodos do usuário do aplicativo e `DbContext` classes, bem como quaisquer outras classes de modelo que utiliza seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0b91f-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="0b91f-119">Por exemplo, se seu `DbContext` tem um `DbSet<Album>`, você precisa para migrar o `Album` classe.</span><span class="sxs-lookup"><span data-stu-id="0b91f-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="0b91f-120">Com esses arquivos no local, o *Startup.cs* arquivo pode ser feito para compilar atualizando seu `using` instruções:</span><span class="sxs-lookup"><span data-stu-id="0b91f-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="0b91f-121">Nosso aplicativo agora está pronto para dar suporte a serviços de autenticação e identidade.</span><span class="sxs-lookup"><span data-stu-id="0b91f-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="0b91f-122">Ele só precisa ter esses recursos expostos aos usuários.</span><span class="sxs-lookup"><span data-stu-id="0b91f-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="0b91f-123">Migrar a lógica de registro e logon</span><span class="sxs-lookup"><span data-stu-id="0b91f-123">Migrate registration and login logic</span></span>

<span data-ttu-id="0b91f-124">Com os serviços de identidade configurados para o aplicativo e o acesso a dados configurado usando o Entity Framework e o SQL Server, estamos prontos para adicionar suporte para registro e logon para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0b91f-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="0b91f-125">Lembre-se de que [anterior no processo de migração](xref:migration/mvc#migrate-the-layout-file) é comentada uma referência a *loginpartial* na *cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0b91f-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="0b91f-126">Agora é hora para retornar a esse código, remova os comentários e adicione os controladores de necessários e modos de exibição para dar suporte à funcionalidade de logon.</span><span class="sxs-lookup"><span data-stu-id="0b91f-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="0b91f-127">Remova o `@Html.Partial` linha *cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0b91f-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="0b91f-128">Agora, adicione um novo modo de exibição Razor chamado *loginpartial* para o *exibições/compartilhadas* pasta:</span><span class="sxs-lookup"><span data-stu-id="0b91f-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="0b91f-129">Atualização *loginpartial. cshtml* com o código a seguir (substitua todo o seu conteúdo):</span><span class="sxs-lookup"><span data-stu-id="0b91f-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="0b91f-130">Neste ponto, você deve ser capaz de atualizar o site no navegador.</span><span class="sxs-lookup"><span data-stu-id="0b91f-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="0b91f-131">Resumo</span><span class="sxs-lookup"><span data-stu-id="0b91f-131">Summary</span></span>

<span data-ttu-id="0b91f-132">ASP.NET Core introduz alterações para os recursos de identidade do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0b91f-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="0b91f-133">Neste artigo, você viu como migrar os recursos de gerenciamento de usuário e autenticação de identidade do ASP.NET para o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0b91f-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
