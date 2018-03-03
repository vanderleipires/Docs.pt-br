---
title: Compartilhamento de cookies entre aplicativos
author: rick-anderson
description: "Saiba como compartilhar os cookies de autenticação entre o ASP.NET 4. x e aplicativos ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: f026a287601a51e544482b95c5283e8ee960d656
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="sharing-cookies-among-apps"></a>Compartilhamento de cookies entre aplicativos

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)

Sites geralmente consistem em aplicativos web individuais trabalhando juntos. Para fornecer uma experiência de logon único (SSO), aplicativos web dentro de um site devem compartilhar os cookies de autenticação. Para dar suporte a esse cenário, a pilha de proteção de dados permite o compartilhamento de autenticação de cookie Katana e permissões de autenticação de cookie do ASP.NET Core.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

O exemplo ilustra o cookie compartilhamento entre três aplicativos que usam autenticação de cookie:

* Aplicativo do ASP.NET Core 2.0 Razor páginas sem usar [a identidade do ASP.NET Core](xref:security/authentication/identity)
* Aplicativo do ASP.NET Core 2.0 MVC com a identidade do ASP.NET Core
* Aplicativo do MVC do ASP.NET Framework 4.6.1 com a identidade do ASP.NET

Nos exemplos a seguirem:

* O nome do cookie de autenticação é definido como um valor comum de `.AspNet.SharedCookie`.
* O `AuthenticationType` é definido como `Identity.Application` explicitamente ou por padrão.
* [CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) é usado como o esquema de autenticação. A constante resolve para um valor de `Cookies`.
* O middleware de autenticação de cookie usa uma implementação de [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` fornece serviços de proteção de dados para a criptografia e descriptografia de dados de carga do cookie de autenticação. O `DataProtectionProvider` instância é isolada do sistema de proteção de dados usado por outras partes do aplicativo.
  * Um comum [chave de proteção de dados](xref:security/data-protection/implementation/key-management) armazenamento local é usado. O aplicativo de exemplo usa uma pasta chamada *token de autenticação* na raiz da solução para armazenar as chaves de proteção de dados.
  * [DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) aceita um [DirectoryInfo](/dotnet/api/system.io.directoryinfo) para uso com os cookies de autenticação. O aplicativo de exemplo fornece o caminho do *token de autenticação* pasta `DirectoryInfo`.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requer o [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pacote NuGet. Para obter esse pacote para o ASP.NET Core 2.0 e posteriores aplicativos, fazer referência a [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. Durante o direcionamento do .NET Framework, adicione uma referência de pacote para `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Compartilhar os cookies de autenticação entre aplicativos do ASP.NET Core

Ao usar a identidade do ASP.NET Core:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

No `ConfigureServices` método, use o [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) método de extensão para configurar o serviço de proteção de dados para cookies.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Consulte o *CookieAuthWithIdentity.Core* project no [código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

No `Configure` método, use o [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) configurar:

* O serviço de proteção de dados para cookies.
* O `AuthenticationScheme` para coincidir com o ASP.NET 4. x.

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = 
        "ApplicationCookie";

    var protectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"));

    options.Cookies.ApplicationCookie.DataProtectionProvider = 
        protectionProvider;

    options.Cookies.ApplicationCookie.TicketDataFormat = 
        new TicketDataFormat(protectionProvider.CreateProtector(
            "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", 
            "Cookies", 
            "v2"));
});
```

---

Ao usar cookies diretamente:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

Consulte o *CookieAuth.Core* project no [código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a>Criptografar chaves de proteção de dados em repouso

Para implantações de produção, configure o `DataProtectionProvider` para criptografar as chaves em repouso com a DPAPI ou um X509Certificate. Consulte [chave de criptografia em repouso](xref:security/data-protection/implementation/key-encryption-at-rest) para obter mais informações.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(
        new DirectoryInfo(@"PATH_TO_KEY_RING"),
        configure =>
        {
            configure.ProtectKeysWithCertificate("thumbprint");
        })
});
```

---

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Compartilhamento de cookies de autenticação entre o ASP.NET 4. x e aplicativos ASP.NET Core

ASP.NET 4. x aplicativos que usam o middleware de autenticação de cookie Katana podem ser configurados para gerar os cookies de autenticação que são compatíveis com o middleware de autenticação de cookie do ASP.NET Core. Isso permite atualizar aplicativos individuais de um grande site gradativamente, proporcionando uma experiência de SSO suave em todo o site.

> [!TIP]
> Quando um aplicativo usa Katana middleware de autenticação de cookie, ele chama `UseCookieAuthentication` do projeto *Startup.Auth.cs* arquivo. Projetos de aplicativo web do ASP.NET 4. x criadas com o Visual Studio 2013 e depois, usar o middleware de autenticação de cookie Katana por padrão.

> [!NOTE]
> Um aplicativo do ASP.NET 4. x deve ter como destino do .NET Framework 4.5.1 ou posterior. Caso contrário, os pacotes do NuGet necessário falharem na instalação.

Para compartilhar os cookies de autenticação entre aplicativos do ASP.NET 4. x e ASP.NET Core, configure o aplicativo do ASP.NET Core, conforme mencionado acima e configure os aplicativos do ASP.NET 4. x, seguindo as etapas abaixo.

1. Instalar o pacote [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) em cada aplicativo do ASP.NET 4. x.

2. Em *Startup.Auth.cs*, localize a chamada para `UseCookieAuthentication` e modificá-lo da seguinte maneira. Altere o nome do cookie para corresponder ao nome usado pelo middleware de autenticação de cookie do ASP.NET Core. Fornecer uma instância de um `DataProtectionProvider` inicializado para o local de armazenamento de chaves de proteção de dados comuns.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

Consulte o *CookieAuthWithIdentity.NETFramework* project no [código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).

Ao gerar uma identidade de usuário, o tipo de autenticação deve corresponder ao tipo definido na `AuthenticationType` com `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Definir o `CookieManager` à interoperabilidade `ChunkingCookieManager` para o formato das partes seja compatível.

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
    CookieName = ".AspNetCore.Cookies",
    // CookieName = ".AspNetCore.ApplicationCookie", (if using ASP.NET Identity)
    // CookiePath = "...", (if necessary)
    // ...
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create(
                new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", 
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});
```

---

## <a name="use-a-common-user-database"></a>Usar um banco de dados comum do usuário

Confirme que o sistema de identidade para cada aplicativo é apontado para o mesmo banco de dados do usuário. Caso contrário, o sistema de identidade produz falhas em tempo de execução quando ele tenta coincidir com as informações no cookie de autenticação com as informações no banco de dados.
