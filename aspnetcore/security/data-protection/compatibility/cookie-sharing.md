---
title: Compartilhamento de cookies entre aplicativos
author: rick-anderson
description: "Saiba como compartilhar os cookies de autenticação entre o ASP.NET 4. x e aplicativos ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: 034c080c3459cd3a9e6b412492d1b19d9e6348a4
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2018
---
# <a name="sharing-cookies-among-apps"></a><span data-ttu-id="72af8-103">Compartilhamento de cookies entre aplicativos</span><span class="sxs-lookup"><span data-stu-id="72af8-103">Sharing cookies among apps</span></span>

<span data-ttu-id="72af8-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="72af8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="72af8-105">Sites geralmente consistem em aplicativos web individuais trabalhando juntos.</span><span class="sxs-lookup"><span data-stu-id="72af8-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="72af8-106">Para fornecer uma experiência de logon único (SSO), aplicativos web dentro de um site devem compartilhar os cookies de autenticação.</span><span class="sxs-lookup"><span data-stu-id="72af8-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="72af8-107">Para dar suporte a esse cenário, a pilha de proteção de dados permite o compartilhamento de autenticação de cookie Katana e permissões de autenticação de cookie do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="72af8-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="72af8-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="72af8-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="72af8-109">O exemplo ilustra o cookie compartilhamento entre três aplicativos que usam autenticação de cookie:</span><span class="sxs-lookup"><span data-stu-id="72af8-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="72af8-110">Aplicativo do ASP.NET Core 2.0 Razor páginas sem usar [a identidade do ASP.NET Core](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="72af8-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="72af8-111">Aplicativo do ASP.NET Core 2.0 MVC com a identidade do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72af8-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="72af8-112">Aplicativo do MVC do ASP.NET Framework 4.6.1 com a identidade do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="72af8-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="72af8-113">Nos exemplos a seguirem:</span><span class="sxs-lookup"><span data-stu-id="72af8-113">In the examples that follow:</span></span>

* <span data-ttu-id="72af8-114">O nome do cookie de autenticação é definido como um valor comum de `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="72af8-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="72af8-115">O `AuthenticationType` é definido como `Identity.Application` explicitamente ou por padrão.</span><span class="sxs-lookup"><span data-stu-id="72af8-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="72af8-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) é usado como o esquema de autenticação.</span><span class="sxs-lookup"><span data-stu-id="72af8-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) is used as the authentication scheme.</span></span> <span data-ttu-id="72af8-117">A constante resolve para um valor de `Cookies`.</span><span class="sxs-lookup"><span data-stu-id="72af8-117">The constant resolves to a value of `Cookies`.</span></span>
* <span data-ttu-id="72af8-118">O middleware de autenticação de cookie usa uma implementação de [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="72af8-118">The cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="72af8-119">`DataProtectionProvider`fornece serviços de proteção de dados para a criptografia e descriptografia de dados de carga do cookie de autenticação.</span><span class="sxs-lookup"><span data-stu-id="72af8-119">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="72af8-120">O `DataProtectionProvider` instância é isolada do sistema de proteção de dados usado por outras partes do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="72af8-120">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="72af8-121">Um comum [chave de proteção de dados](xref:security/data-protection/implementation/key-management) armazenamento local é usado.</span><span class="sxs-lookup"><span data-stu-id="72af8-121">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="72af8-122">O aplicativo de exemplo usa uma pasta chamada *token de autenticação* na raiz da solução para armazenar as chaves de proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="72af8-122">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
  * <span data-ttu-id="72af8-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) aceita um [DirectoryInfo](/dotnet/api/system.io.directoryinfo) para uso com os cookies de autenticação.</span><span class="sxs-lookup"><span data-stu-id="72af8-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) for use with authentication cookies.</span></span> <span data-ttu-id="72af8-124">O aplicativo de exemplo fornece o caminho do *token de autenticação* pasta `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="72af8-124">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span>
  * <span data-ttu-id="72af8-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requer o [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="72af8-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="72af8-126">Para obter esse pacote para o ASP.NET Core 2.0 e posteriores aplicativos, fazer referência a [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="72af8-126">To obtain this package for ASP.NET Core 2.0 and later apps, reference the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="72af8-127">Durante o direcionamento do .NET Framework, adicione uma referência de pacote para `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="72af8-127">When targeting .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="72af8-128">Compartilhar os cookies de autenticação entre aplicativos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72af8-128">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="72af8-129">Ao usar a identidade do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="72af8-129">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="72af8-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="72af8-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="72af8-131">No `ConfigureServices` método, use o [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) método de extensão para configurar o serviço de proteção de dados para cookies.</span><span class="sxs-lookup"><span data-stu-id="72af8-131">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="72af8-132">Consulte o *CookieAuthWithIdentity.Core* project no [código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="72af8-132">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="72af8-133">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="72af8-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="72af8-134">No `Configure` método, use o [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) configurar:</span><span class="sxs-lookup"><span data-stu-id="72af8-134">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="72af8-135">O serviço de proteção de dados para cookies.</span><span class="sxs-lookup"><span data-stu-id="72af8-135">The data protection service for cookies.</span></span>
* <span data-ttu-id="72af8-136">O `AuthenticationScheme` para coincidir com o ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="72af8-136">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="72af8-137">Ao usar cookies diretamente:</span><span class="sxs-lookup"><span data-stu-id="72af8-137">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="72af8-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="72af8-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="72af8-139">Consulte o *CookieAuth.Core* project no [código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="72af8-139">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="72af8-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="72af8-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="72af8-141">Criptografar chaves de proteção de dados em repouso</span><span class="sxs-lookup"><span data-stu-id="72af8-141">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="72af8-142">Para implantações de produção, configure o `DataProtectionProvider` para criptografar as chaves em repouso com a DPAPI ou um X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="72af8-142">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="72af8-143">Consulte [chave de criptografia em repouso](xref:security/data-protection/implementation/key-encryption-at-rest) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="72af8-143">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="72af8-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="72af8-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="72af8-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="72af8-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="72af8-146">Compartilhamento de cookies de autenticação entre o ASP.NET 4. x e aplicativos ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72af8-146">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="72af8-147">ASP.NET 4. x aplicativos que usam o middleware de autenticação de cookie Katana podem ser configurados para gerar os cookies de autenticação que são compatíveis com o middleware de autenticação de cookie do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="72af8-147">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="72af8-148">Isso permite atualizar aplicativos individuais de um grande site gradativamente, proporcionando uma experiência de SSO suave em todo o site.</span><span class="sxs-lookup"><span data-stu-id="72af8-148">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="72af8-149">Quando um aplicativo usa Katana middleware de autenticação de cookie, ele chama `UseCookieAuthentication` do projeto *Startup.Auth.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="72af8-149">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="72af8-150">Projetos de aplicativo web do ASP.NET 4. x criadas com o Visual Studio 2013 e depois, usar o middleware de autenticação de cookie Katana por padrão.</span><span class="sxs-lookup"><span data-stu-id="72af8-150">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="72af8-151">Um aplicativo do ASP.NET 4. x deve ter como destino do .NET Framework 4.5.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="72af8-151">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="72af8-152">Caso contrário, os pacotes do NuGet necessário falharem na instalação.</span><span class="sxs-lookup"><span data-stu-id="72af8-152">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="72af8-153">Para compartilhar os cookies de autenticação entre aplicativos do ASP.NET 4. x e ASP.NET Core, configure o aplicativo do ASP.NET Core, conforme mencionado acima e configure os aplicativos do ASP.NET 4. x, seguindo as etapas abaixo.</span><span class="sxs-lookup"><span data-stu-id="72af8-153">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="72af8-154">Instalar o pacote [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) em cada aplicativo do ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="72af8-154">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="72af8-155">Em *Startup.Auth.cs*, localize a chamada para `UseCookieAuthentication` e modificá-lo da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="72af8-155">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="72af8-156">Altere o nome do cookie para corresponder ao nome usado pelo middleware de autenticação de cookie do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="72af8-156">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="72af8-157">Fornecer uma instância de um `DataProtectionProvider` inicializado para o local de armazenamento de chaves de proteção de dados comuns.</span><span class="sxs-lookup"><span data-stu-id="72af8-157">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="72af8-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="72af8-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="72af8-159">Consulte o *CookieAuthWithIdentity.NETFramework* project no [código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="72af8-159">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="72af8-160">Ao gerar uma identidade de usuário, o tipo de autenticação deve corresponder ao tipo definido na `AuthenticationType` com `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="72af8-160">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="72af8-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="72af8-161">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="72af8-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="72af8-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="72af8-163">Definir o `CookieManager` à interoperabilidade `ChunkingCookieManager` para o formato das partes seja compatível.</span><span class="sxs-lookup"><span data-stu-id="72af8-163">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="72af8-164">Usar um banco de dados comum do usuário</span><span class="sxs-lookup"><span data-stu-id="72af8-164">Use a common user database</span></span>

<span data-ttu-id="72af8-165">Confirme que o sistema de identidade para cada aplicativo é apontado para o mesmo banco de dados do usuário.</span><span class="sxs-lookup"><span data-stu-id="72af8-165">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="72af8-166">Caso contrário, o sistema de identidade produz falhas em tempo de execução quando ele tenta coincidir com as informações no cookie de autenticação com as informações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="72af8-166">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
