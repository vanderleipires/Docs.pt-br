---
title: "Migrando de autenticação e identidade principal do ASP.NET 2.0"
author: scottaddie
description: "Este artigo descreve as etapas mais comuns de identidade e autenticação de 1. x ASP.NET Core migrando para o ASP.NET 2.0 de núcleo."
ms.author: scaddie
manager: wpickett
ms.date: 10/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 72ad31438a344fb5fa2b357c709b923b8077e742
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="c302d-103">Migrando de autenticação e identidade do núcleo do ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="c302d-103">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="c302d-104">Por [Scott Addie](https://github.com/scottaddie) e [Kung Hao](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="c302d-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="c302d-105">ASP.NET Core 2.0 tem um novo modelo para autenticação e [identidade](xref:security/authentication/identity) que simplifica a configuração por meio de serviços.</span><span class="sxs-lookup"><span data-stu-id="c302d-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="c302d-106">Aplicativos de 1. x do ASP.NET Core que usam autenticação ou identidade podem ser atualizados para usar o novo modelo conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="c302d-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="c302d-107">Middleware de autenticação e serviços</span><span class="sxs-lookup"><span data-stu-id="c302d-107">Authentication Middleware and Services</span></span>
<span data-ttu-id="c302d-108">Em projetos de 1. x, a autenticação é configurada por meio do middleware.</span><span class="sxs-lookup"><span data-stu-id="c302d-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="c302d-109">Um método de middleware é invocado para cada esquema de autenticação que você deseja dar suporte.</span><span class="sxs-lookup"><span data-stu-id="c302d-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="c302d-110">O exemplo a seguir 1. x configura a autenticação do Facebook com identidade na *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c302d-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions { 
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
} 
```

<span data-ttu-id="c302d-111">Em 2.0 projetos, a autenticação é configurada por meio de serviços.</span><span class="sxs-lookup"><span data-stu-id="c302d-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="c302d-112">Cada esquema de autenticação é registrada no `ConfigureServices` método *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c302d-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="c302d-113">O `UseIdentity` método é substituído pelo `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="c302d-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="c302d-114">O exemplo a seguir 2.0 configura a autenticação do Facebook com identidade na *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c302d-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="c302d-115">O `UseAuthentication` método adiciona um componente de middleware de autenticação único que é responsável pela autenticação automática e o tratamento de solicitações de autenticação remota.</span><span class="sxs-lookup"><span data-stu-id="c302d-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="c302d-116">Ele substitui todos os componentes de middleware individuais com um componente de middleware único, comuns.</span><span class="sxs-lookup"><span data-stu-id="c302d-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="c302d-117">Abaixo estão 2.0 instruções de migração de cada esquema de autenticação principal.</span><span class="sxs-lookup"><span data-stu-id="c302d-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="c302d-118">Autenticação baseada em cookie</span><span class="sxs-lookup"><span data-stu-id="c302d-118">Cookie-based Authentication</span></span>
<span data-ttu-id="c302d-119">Selecione uma das duas opções abaixo e faça as alterações necessárias na *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c302d-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="c302d-120">Usar cookies com identidade</span><span class="sxs-lookup"><span data-stu-id="c302d-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="c302d-121">Substituir `UseIdentity` com `UseAuthentication` no `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="c302d-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="c302d-122">Invocar o `AddIdentity` método o `ConfigureServices` método para adicionar os serviços de autenticação de cookie.</span><span class="sxs-lookup"><span data-stu-id="c302d-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="c302d-123">Opcionalmente, invoque o `ConfigureApplicationCookie` ou `ConfigureExternalCookie` método o `ConfigureServices` método para ajustar as configurações de cookie de identidade.</span><span class="sxs-lookup"><span data-stu-id="c302d-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="c302d-124">Usar cookies sem identidade</span><span class="sxs-lookup"><span data-stu-id="c302d-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="c302d-125">Substitua o `UseCookieAuthentication` chamada do método de `Configure` método com `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="c302d-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="c302d-126">Invocar o `AddAuthentication` e `AddCookie` métodos de `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="c302d-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User, 
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options => 
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="c302d-127">Autenticação do portador do JWT</span><span class="sxs-lookup"><span data-stu-id="c302d-127">JWT Bearer Authentication</span></span>
<span data-ttu-id="c302d-128">Faça as seguintes alterações em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c302d-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="c302d-129">Substitua o `UseJwtBearerAuthentication` chamada do método de `Configure` método com `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="c302d-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="c302d-130">Invocar o `AddJwtBearer` método o `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="c302d-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="c302d-131">Este trecho de código não usa a identidade, portanto o esquema padrão deve ser definido passando `JwtBearerDefaults.AuthenticationScheme` para o `AddAuthentication` método.</span><span class="sxs-lookup"><span data-stu-id="c302d-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="c302d-132">OpenID Connect (OIDC) autenticação</span><span class="sxs-lookup"><span data-stu-id="c302d-132">OpenID Connect (OIDC) Authentication</span></span>
<span data-ttu-id="c302d-133">Faça as seguintes alterações em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c302d-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="c302d-134">Substitua o `UseOpenIdConnectAuthentication` chamada do método de `Configure` método com `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="c302d-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="c302d-135">Invocar o `AddOpenIdConnect` método o `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="c302d-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options => 
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options => 
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a><span data-ttu-id="c302d-136">Autenticação do Facebook</span><span class="sxs-lookup"><span data-stu-id="c302d-136">Facebook Authentication</span></span>
<span data-ttu-id="c302d-137">Faça as seguintes alterações em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c302d-137">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="c302d-138">Substitua o `UseFacebookAuthentication` chamada do método de `Configure` método com `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="c302d-138">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="c302d-139">Invocar o `AddFacebook` método o `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="c302d-139">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="c302d-140">Google Authentication</span><span class="sxs-lookup"><span data-stu-id="c302d-140">Google Authentication</span></span>
<span data-ttu-id="c302d-141">Faça as seguintes alterações em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c302d-141">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="c302d-142">Substitua o `UseGoogleAuthentication` chamada do método de `Configure` método com `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="c302d-142">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="c302d-143">Invocar o `AddGoogle` método o `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="c302d-143">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="c302d-144">Microsoft Account Authentication</span><span class="sxs-lookup"><span data-stu-id="c302d-144">Microsoft Account Authentication</span></span>
<span data-ttu-id="c302d-145">Faça as seguintes alterações em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c302d-145">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="c302d-146">Substitua o `UseMicrosoftAccountAuthentication` chamada do método de `Configure` método com `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="c302d-146">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="c302d-147">Invocar o `AddMicrosoftAccount` método o `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="c302d-147">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="c302d-148">Autenticação do Twitter</span><span class="sxs-lookup"><span data-stu-id="c302d-148">Twitter Authentication</span></span>
<span data-ttu-id="c302d-149">Faça as seguintes alterações em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c302d-149">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="c302d-150">Substitua o `UseTwitterAuthentication` chamada do método de `Configure` método com `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="c302d-150">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="c302d-151">Invocar o `AddTwitter` método o `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="c302d-151">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="c302d-152">Esquemas de autenticação padrão de configuração</span><span class="sxs-lookup"><span data-stu-id="c302d-152">Setting Default Authentication Schemes</span></span>
<span data-ttu-id="c302d-153">Em 1. x, o `AutomaticAuthenticate` e `AutomaticChallenge` propriedades do [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) classe base foram se destina a ser definido em um esquema de autenticação.</span><span class="sxs-lookup"><span data-stu-id="c302d-153">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="c302d-154">Não havia uma maneira válida para impor isso.</span><span class="sxs-lookup"><span data-stu-id="c302d-154">There was no good way to enforce this.</span></span>

<span data-ttu-id="c302d-155">No 2.0, essas duas propriedades foram removidas como propriedades individuais `AuthenticationOptions` instância.</span><span class="sxs-lookup"><span data-stu-id="c302d-155">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="c302d-156">Eles podem ser configurados no `AddAuthentication` chamada de método do `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c302d-156">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="c302d-157">No trecho de código anterior, o esquema padrão é definido como `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span><span class="sxs-lookup"><span data-stu-id="c302d-157">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="c302d-158">Como alternativa, use uma versão sobrecarregada do `AddAuthentication` método para definir mais de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="c302d-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="c302d-159">No exemplo a seguir método sobrecarregado, o esquema padrão é definido como `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="c302d-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="c302d-160">O esquema de autenticação também pode ser especificado dentro de sua individuais `[Authorize]` atributos ou as políticas de autorização.</span><span class="sxs-lookup"><span data-stu-id="c302d-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="c302d-161">Defina um esquema padrão 2.0 se uma das seguintes condições for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="c302d-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="c302d-162">Você deseja que o usuário a ser conectado automaticamente</span><span class="sxs-lookup"><span data-stu-id="c302d-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="c302d-163">Você usa o `[Authorize]` políticas de autorização ou atributo sem especificar esquemas</span><span class="sxs-lookup"><span data-stu-id="c302d-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="c302d-164">Uma exceção a essa regra é o `AddIdentity` método.</span><span class="sxs-lookup"><span data-stu-id="c302d-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="c302d-165">Este método adiciona cookies para você e define o padrão autenticar e desafio esquemas para o cookie do aplicativo `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="c302d-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="c302d-166">Além disso, ele define o esquema de entrada padrão para o cookie externo `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="c302d-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="c302d-167">Use as extensões de autenticação HttpContext</span><span class="sxs-lookup"><span data-stu-id="c302d-167">Use HttpContext Authentication Extensions</span></span>
<span data-ttu-id="c302d-168">O `IAuthenticationManager` interface é o ponto de entrada principal para o sistema de autenticação de 1. x.</span><span class="sxs-lookup"><span data-stu-id="c302d-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="c302d-169">Ele foi substituído por um novo conjunto de `HttpContext` métodos de extensão no `Microsoft.AspNetCore.Authentication` namespace.</span><span class="sxs-lookup"><span data-stu-id="c302d-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="c302d-170">Por exemplo, 1. x projetos referência um `Authentication` propriedade:</span><span class="sxs-lookup"><span data-stu-id="c302d-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="c302d-171">Em 2.0 projetos, importar o `Microsoft.AspNetCore.Authentication` namespace e exclua o `Authentication` referências de propriedade:</span><span class="sxs-lookup"><span data-stu-id="c302d-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="c302d-172">Autenticação do Windows (http. sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="c302d-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="c302d-173">Há duas variações da autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="c302d-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="c302d-174">O host só permite que os usuários autenticados</span><span class="sxs-lookup"><span data-stu-id="c302d-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="c302d-175">O host permite que ambos anônimos e usuários autenticados</span><span class="sxs-lookup"><span data-stu-id="c302d-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="c302d-176">A primeira descrita acima é afetada pelas 2.0 alterações.</span><span class="sxs-lookup"><span data-stu-id="c302d-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="c302d-177">A segunda descrita acima é afetada pelas 2.0 alterações.</span><span class="sxs-lookup"><span data-stu-id="c302d-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="c302d-178">Por exemplo, você pode ser permitir que usuários anônimos em seu aplicativo no IIS ou [HTTP.sys](xref:fundamentals/servers/weblistener) mas autorizar usuários no nível do controlador de camada.</span><span class="sxs-lookup"><span data-stu-id="c302d-178">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="c302d-179">Nesse cenário, defina o esquema padrão `IISDefaults.AuthenticationScheme` no `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c302d-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="c302d-180">Falha ao definir o esquema padrão adequadamente impede que a solicitação de autorização ao desafio de trabalho.</span><span class="sxs-lookup"><span data-stu-id="c302d-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="c302d-181">IdentityCookieOptions Instances</span><span class="sxs-lookup"><span data-stu-id="c302d-181">IdentityCookieOptions Instances</span></span>
<span data-ttu-id="c302d-182">Um efeito colateral das 2.0 alterações é o switch usando nomeados opções em vez de instâncias de opções do cookie.</span><span class="sxs-lookup"><span data-stu-id="c302d-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="c302d-183">A capacidade de personalizar os nomes de esquema de cookie de identidade é removida.</span><span class="sxs-lookup"><span data-stu-id="c302d-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="c302d-184">Por exemplo, 1. x projetos usam [injeção de construtor](xref:mvc/controllers/dependency-injection#constructor-injection) para passar um `IdentityCookieOptions` parâmetro em *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="c302d-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="c302d-185">O esquema de autenticação de cookie externa é acessado da instância fornecida:</span><span class="sxs-lookup"><span data-stu-id="c302d-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="c302d-186">A injeção de construtor mencionados acima se torna desnecessária em 2.0 projetos e o `_externalCookieScheme` campo pode ser excluído:</span><span class="sxs-lookup"><span data-stu-id="c302d-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="c302d-187">O `IdentityConstants.ExternalScheme` constante pode ser usada diretamente:</span><span class="sxs-lookup"><span data-stu-id="c302d-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="c302d-188">Adicionar propriedades de navegação IdentityUser POCO</span><span class="sxs-lookup"><span data-stu-id="c302d-188">Add IdentityUser POCO Navigation Properties</span></span>
<span data-ttu-id="c302d-189">As propriedades de navegação principal do Entity Framework (EF) da base de `IdentityUser` POCO (objeto Plain Old CLR) foram removidas.</span><span class="sxs-lookup"><span data-stu-id="c302d-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="c302d-190">Seu projeto de 1. x usado essas propriedades, adicione-as manualmente ao projeto 2.0:</span><span class="sxs-lookup"><span data-stu-id="c302d-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

<span data-ttu-id="c302d-191">Para evitar que chaves estrangeiras duplicadas ao executar migrações de núcleo EF, adicione o seguinte ao seu `IdentityDbContext` classe `OnModelCreating` método (após o `base.OnModelCreating();` chamar):</span><span class="sxs-lookup"><span data-stu-id="c302d-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="c302d-192">Substituir GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="c302d-192">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="c302d-193">O método síncrono `GetExternalAuthenticationSchemes` foi removido em favor de uma versão assíncrona.</span><span class="sxs-lookup"><span data-stu-id="c302d-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="c302d-194">1. x projetos têm o seguinte código *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="c302d-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="c302d-195">Esse método é exibida no *cshtml* muito:</span><span class="sxs-lookup"><span data-stu-id="c302d-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="c302d-196">Em 2.0 projetos, use o `GetExternalAuthenticationSchemesAsync` método:</span><span class="sxs-lookup"><span data-stu-id="c302d-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="c302d-197">No *cshtml*, o `AuthenticationScheme` propriedade acessada o `foreach` loop muda para `Name`:</span><span class="sxs-lookup"><span data-stu-id="c302d-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="c302d-198">Alteração da propriedade ManageLoginsViewModel</span><span class="sxs-lookup"><span data-stu-id="c302d-198">ManageLoginsViewModel Property Change</span></span>
<span data-ttu-id="c302d-199">Um `ManageLoginsViewModel` objeto é usado no `ManageLogins` ação de *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="c302d-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="c302d-200">Em do 1. x projetos, o objeto `OtherLogins` é do tipo de retorno de propriedade `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="c302d-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="c302d-201">Esse tipo de retorno requer uma importação de `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="c302d-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="c302d-202">Em 2.0 projetos, o tipo de retorno é alterado para `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="c302d-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="c302d-203">Esse novo tipo de retorno requer substituir o `Microsoft.AspNetCore.Http.Authentication` importar com um `Microsoft.AspNetCore.Authentication` importar.</span><span class="sxs-lookup"><span data-stu-id="c302d-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="c302d-204">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="c302d-204">Additional Resources</span></span>
<span data-ttu-id="c302d-205">Para obter detalhes adicionais e discussão, consulte o [discussões sobre autenticação 2.0](https://github.com/aspnet/Security/issues/1338) problema no GitHub.</span><span class="sxs-lookup"><span data-stu-id="c302d-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
