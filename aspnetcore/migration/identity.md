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
ms.openlocfilehash: ed96266f06eb473fa3c3e1cc81b2b58fcd89f29e
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="migrating-authentication-and-identity"></a><span data-ttu-id="df0f9-103">Migrando de autenticação e identidade</span><span class="sxs-lookup"><span data-stu-id="df0f9-103">Migrating Authentication and Identity</span></span>

<a name=migration-identity></a>

<span data-ttu-id="df0f9-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="df0f9-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="df0f9-105">No anterior do artigo é [migrados de configuração de um projeto ASP.NET MVC para MVC do ASP.NET Core](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="df0f9-105">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="df0f9-106">Neste artigo, migrar os recursos de gerenciamento de registro, logon e usuário.</span><span class="sxs-lookup"><span data-stu-id="df0f9-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="df0f9-107">Defina a identidade e associação</span><span class="sxs-lookup"><span data-stu-id="df0f9-107">Configure Identity and Membership</span></span>

<span data-ttu-id="df0f9-108">No ASP.NET MVC, os recursos de autenticação e identidade são configurados usando a identidade do ASP.NET em Startup.Auth.cs e IdentityConfig.cs, localizado na pasta App_Start.</span><span class="sxs-lookup"><span data-stu-id="df0f9-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="df0f9-109">No ASP.NET MVC de núcleo, esses recursos são configurados no *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="df0f9-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="df0f9-110">Instalar o `Microsoft.AspNetCore.Identity.EntityFrameworkCore` e `Microsoft.AspNetCore.Authentication.Cookies` pacotes do NuGet.</span><span class="sxs-lookup"><span data-stu-id="df0f9-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="df0f9-111">Em seguida, abra Startup.cs e atualize o `ConfigureServices()` método para usar os serviços de identidade do Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="df0f9-111">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="df0f9-112">Neste ponto, há dois tipos referenciados no código acima que nós ainda não foram migrados do projeto ASP.NET MVC: `ApplicationDbContext` e `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="df0f9-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="df0f9-113">Criar um novo *modelos* pasta no ASP.NET Core do projeto e adicionar duas classes a ele correspondente a esses tipos.</span><span class="sxs-lookup"><span data-stu-id="df0f9-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="df0f9-114">Você encontrará o ASP.NET MVC versões dessas classes em `/Models/IdentityModels.cs`, mas vamos usar um arquivo por classe no projeto migrado porque é mais clara.</span><span class="sxs-lookup"><span data-stu-id="df0f9-114">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="df0f9-115">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="df0f9-115">ApplicationUser.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="df0f9-116">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="df0f9-116">ApplicationDbContext.cs:</span></span>

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

<span data-ttu-id="df0f9-117">O projeto da Web do ASP.NET Core MVC Starter não inclui mais personalização de usuários ou o ApplicationDbContext.</span><span class="sxs-lookup"><span data-stu-id="df0f9-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="df0f9-118">Ao migrar um aplicativo real, você também precisará migrar todas as propriedades personalizadas e métodos de classes de DbContext e usuário do seu aplicativo, bem como quaisquer outras classes de modelo que utiliza o seu aplicativo (por exemplo, se o DbContext tem um DbSet<Album>, obviamente você precisará migrar a classe álbum).</span><span class="sxs-lookup"><span data-stu-id="df0f9-118">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="df0f9-119">Com esses arquivos no local, o arquivo Startup.cs pode ser feito para compilar atualizando seu usando instruções:</span><span class="sxs-lookup"><span data-stu-id="df0f9-119">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="df0f9-120">Nosso aplicativo agora está pronto para dar suporte a serviços de autenticação e identidade – ele só precisa ter esses recursos expostos aos usuários.</span><span class="sxs-lookup"><span data-stu-id="df0f9-120">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="df0f9-121">Migrar o registro e a lógica de logon</span><span class="sxs-lookup"><span data-stu-id="df0f9-121">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="df0f9-122">Com os serviços de identidade configurados para o aplicativo e o acesso a dados configurado usando o Entity Framework e o SQL Server, você agora está pronto para adicionar suporte para registro e logon para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="df0f9-122">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="df0f9-123">Lembre-se de que [anterior no processo de migração](mvc.md#migrate-layout-file) é comentada uma referência para loginpartial no layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="df0f9-123">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="df0f9-124">Agora é hora para retornar a esse código, remova os comentários e adicione os controladores de necessários e modos de exibição para dar suporte à funcionalidade de logon.</span><span class="sxs-lookup"><span data-stu-id="df0f9-124">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="df0f9-125">Atualizar layout. cshtml; Remova o @Html.Partial linha:</span><span class="sxs-lookup"><span data-stu-id="df0f9-125">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="df0f9-126">Agora, adicione uma nova página de exibição MVC chamado loginpartial para a pasta exibições/compartilhadas:</span><span class="sxs-lookup"><span data-stu-id="df0f9-126">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="df0f9-127">Atualizar loginpartial. cshtml com o código a seguir (substitua todo o seu conteúdo):</span><span class="sxs-lookup"><span data-stu-id="df0f9-127">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="df0f9-128">Neste ponto, você deve ser capaz de atualizar o site no navegador.</span><span class="sxs-lookup"><span data-stu-id="df0f9-128">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="df0f9-129">Resumo</span><span class="sxs-lookup"><span data-stu-id="df0f9-129">Summary</span></span>

<span data-ttu-id="df0f9-130">ASP.NET Core introduz alterações para os recursos de identidade do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="df0f9-130">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="df0f9-131">Neste artigo, você viu como migrar os recursos de gerenciamento de usuário e autenticação de uma identidade do ASP.NET para o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df0f9-131">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>
