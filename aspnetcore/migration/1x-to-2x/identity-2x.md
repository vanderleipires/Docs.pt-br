---
title: Migrar de autenticação e identidade para o ASP.NET 2.0 de núcleo
author: scottaddie
description: Este artigo descreve as etapas mais comuns de identidade e autenticação de 1. x ASP.NET Core migrando para o ASP.NET 2.0 de núcleo.
manager: wpickett
ms.author: scaddie
ms.date: 10/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 0653906996f9f37d436ebefc6a738d2603788d53
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32741453"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Migrar de autenticação e identidade para o ASP.NET 2.0 de núcleo

Por [Scott Addie](https://github.com/scottaddie) e [Kung Hao](https://github.com/HaoK)

ASP.NET Core 2.0 tem um novo modelo para autenticação e [identidade](xref:security/authentication/identity) que simplifica a configuração por meio de serviços. Aplicativos de 1. x do ASP.NET Core que usam autenticação ou identidade podem ser atualizados para usar o novo modelo conforme descrito abaixo.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Middleware de autenticação e serviços
Em projetos de 1. x, a autenticação é configurada por meio do middleware. Um método de middleware é invocado para cada esquema de autenticação que você deseja dar suporte.

O exemplo a seguir 1. x configura a autenticação do Facebook com identidade na *Startup.cs*:

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

Em 2.0 projetos, a autenticação é configurada por meio de serviços. Cada esquema de autenticação é registrada no `ConfigureServices` método *Startup.cs*. O `UseIdentity` método é substituído pelo `UseAuthentication`.

O exemplo a seguir 2.0 configura a autenticação do Facebook com identidade na *Startup.cs*:

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

O `UseAuthentication` método adiciona um componente de middleware de autenticação único que é responsável pela autenticação automática e o tratamento de solicitações de autenticação remota. Ele substitui todos os componentes de middleware individuais com um componente de middleware único, comuns.

Abaixo estão 2.0 instruções de migração de cada esquema de autenticação principal.

### <a name="cookie-based-authentication"></a>Autenticação baseada em cookie
Selecione uma das duas opções abaixo e faça as alterações necessárias na *Startup.cs*:

1. Usar cookies com identidade
    - Substituir `UseIdentity` com `UseAuthentication` no `Configure` método:

        ```csharp
        app.UseAuthentication();
        ```

    - Invocar o `AddIdentity` método o `ConfigureServices` método para adicionar os serviços de autenticação de cookie.
    - Opcionalmente, invoque o `ConfigureApplicationCookie` ou `ConfigureExternalCookie` método o `ConfigureServices` método para ajustar as configurações de cookie de identidade.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Usar cookies sem identidade
    - Substitua o `UseCookieAuthentication` chamada do método de `Configure` método com `UseAuthentication`:
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - Invocar o `AddAuthentication` e `AddCookie` métodos de `ConfigureServices` método:

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

### <a name="jwt-bearer-authentication"></a>Autenticação do portador do JWT
Faça as seguintes alterações em *Startup.cs*:
- Substitua o `UseJwtBearerAuthentication` chamada do método de `Configure` método com `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Invocar o `AddJwtBearer` método o `ConfigureServices` método:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Este trecho de código não usa a identidade, portanto o esquema padrão deve ser definido passando `JwtBearerDefaults.AuthenticationScheme` para o `AddAuthentication` método.

### <a name="openid-connect-oidc-authentication"></a>Autenticação do OpenID conectar (OIDC)
Faça as seguintes alterações em *Startup.cs*:

- Substitua o `UseOpenIdConnectAuthentication` chamada do método de `Configure` método com `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Invocar o `AddOpenIdConnect` método o `ConfigureServices` método:

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

### <a name="facebook-authentication"></a>autenticação do Facebook
Faça as seguintes alterações em *Startup.cs*:
- Substitua o `UseFacebookAuthentication` chamada do método de `Configure` método com `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Invocar o `AddFacebook` método o `ConfigureServices` método:
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Autenticação do Google
Faça as seguintes alterações em *Startup.cs*:
- Substitua o `UseGoogleAuthentication` chamada do método de `Configure` método com `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Invocar o `AddGoogle` método o `ConfigureServices` método:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a>Autenticação de Account da Microsoft
Faça as seguintes alterações em *Startup.cs*:
- Substitua o `UseMicrosoftAccountAuthentication` chamada do método de `Configure` método com `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Invocar o `AddMicrosoftAccount` método o `ConfigureServices` método:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a>Autenticação do Twitter
Faça as seguintes alterações em *Startup.cs*:
- Substitua o `UseTwitterAuthentication` chamada do método de `Configure` método com `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Invocar o `AddTwitter` método o `ConfigureServices` método:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Esquemas de autenticação padrão de configuração
Em 1. x, o `AutomaticAuthenticate` e `AutomaticChallenge` propriedades do [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) classe base foram se destina a ser definido em um esquema de autenticação. Não havia uma maneira válida para impor isso.

No 2.0, essas duas propriedades foram removidas como propriedades individuais `AuthenticationOptions` instância. Eles podem ser configurados no `AddAuthentication` chamada de método do `ConfigureServices` método *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

No trecho de código anterior, o esquema padrão é definido como `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").

Como alternativa, use uma versão sobrecarregada do `AddAuthentication` método para definir mais de uma propriedade. No exemplo a seguir método sobrecarregado, o esquema padrão é definido como `CookieAuthenticationDefaults.AuthenticationScheme`. O esquema de autenticação também pode ser especificado dentro de sua individuais `[Authorize]` atributos ou as políticas de autorização.

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Defina um esquema padrão 2.0 se uma das seguintes condições for verdadeira:
- Você deseja que o usuário a ser conectado automaticamente
- Você usa o `[Authorize]` políticas de autorização ou atributo sem especificar esquemas

Uma exceção a essa regra é o `AddIdentity` método. Este método adiciona cookies para você e define o padrão autenticar e desafio esquemas para o cookie do aplicativo `IdentityConstants.ApplicationScheme`. Além disso, ele define o esquema de entrada padrão para o cookie externo `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Use as extensões de autenticação HttpContext
O `IAuthenticationManager` interface é o ponto de entrada principal para o sistema de autenticação de 1. x. Ele foi substituído por um novo conjunto de `HttpContext` métodos de extensão no `Microsoft.AspNetCore.Authentication` namespace.

Por exemplo, 1. x projetos referência um `Authentication` propriedade:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Em 2.0 projetos, importar o `Microsoft.AspNetCore.Authentication` namespace e exclua o `Authentication` referências de propriedade:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Autenticação do Windows (http. sys / IISIntegration)
Há duas variações da autenticação do Windows:
1. O host só permite que os usuários autenticados
2. O host permite que ambos anônimos e usuários autenticados

A primeira descrita acima é afetada pelas 2.0 alterações.

A segunda descrita acima é afetada pelas 2.0 alterações. Por exemplo, você pode ser permitir que usuários anônimos em seu aplicativo no IIS ou [HTTP.sys](xref:fundamentals/servers/weblistener) mas autorizar usuários no nível do controlador de camada. Nesse cenário, defina o esquema padrão `IISDefaults.AuthenticationScheme` no `ConfigureServices` método *Startup.cs*:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

Falha ao definir o esquema padrão adequadamente impede que a solicitação de autorização ao desafio de trabalho.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>Instâncias de IdentityCookieOptions
Um efeito colateral das 2.0 alterações é o switch usando nomeados opções em vez de instâncias de opções do cookie. A capacidade de personalizar os nomes de esquema de cookie de identidade é removida.

Por exemplo, 1. x projetos usam [injeção de construtor](xref:mvc/controllers/dependency-injection#constructor-injection) para passar um `IdentityCookieOptions` parâmetro em *AccountController.cs*. O esquema de autenticação de cookie externa é acessado da instância fornecida:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

A injeção de construtor mencionados acima se torna desnecessária em 2.0 projetos e o `_externalCookieScheme` campo pode ser excluído:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

O `IdentityConstants.ExternalScheme` constante pode ser usada diretamente:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Adicionar propriedades de navegação IdentityUser POCO
As propriedades de navegação principal do Entity Framework (EF) da base de `IdentityUser` POCO (objeto Plain Old CLR) foram removidas. Seu projeto de 1. x usado essas propriedades, adicione-as manualmente ao projeto 2.0:

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

Para evitar que chaves estrangeiras duplicadas ao executar migrações de núcleo EF, adicione o seguinte ao seu `IdentityDbContext` classe `OnModelCreating` método (após o `base.OnModelCreating();` chamar):

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

## <a name="replace-getexternalauthenticationschemes"></a>Substituir GetExternalAuthenticationSchemes
O método síncrono `GetExternalAuthenticationSchemes` foi removido em favor de uma versão assíncrona. 1. x projetos têm o seguinte código *ManageController.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Esse método é exibida no *cshtml* muito:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

Em 2.0 projetos, use o `GetExternalAuthenticationSchemesAsync` método:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

No *cshtml*, o `AuthenticationScheme` propriedade acessada o `foreach` loop muda para `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Alteração da propriedade ManageLoginsViewModel
Um `ManageLoginsViewModel` objeto é usado no `ManageLogins` ação de *ManageController.cs*. Em do 1. x projetos, o objeto `OtherLogins` é do tipo de retorno de propriedade `IList<AuthenticationDescription>`. Esse tipo de retorno requer uma importação de `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

Em 2.0 projetos, o tipo de retorno é alterado para `IList<AuthenticationScheme>`. Esse novo tipo de retorno requer substituir o `Microsoft.AspNetCore.Http.Authentication` importar com um `Microsoft.AspNetCore.Authentication` importar.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Recursos adicionais
Para obter detalhes adicionais e discussão, consulte o [discussões sobre autenticação 2.0](https://github.com/aspnet/Security/issues/1338) problema no GitHub.
