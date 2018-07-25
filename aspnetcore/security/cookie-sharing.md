---
title: Compartilhar cookies entre aplicativos com o ASP.NET e ASP.NET Core
author: rick-anderson
description: Aprenda a compartilhar cookies de autenticação entre o ASP.NET 4. x e aplicativos ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: ed3496db3f7a63a704f0e57faef6b2f6c085a8bd
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228592"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="69867-103">Compartilhar cookies entre aplicativos com o ASP.NET e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69867-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="69867-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="69867-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="69867-105">Sites geralmente consistem em aplicativos web individuais trabalhando juntos.</span><span class="sxs-lookup"><span data-stu-id="69867-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="69867-106">Para fornecer uma experiência de logon único (SSO), aplicativos web dentro de um site devem compartilhar cookies de autenticação.</span><span class="sxs-lookup"><span data-stu-id="69867-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="69867-107">Para dar suporte a esse cenário, a pilha de proteção de dados permite o compartilhamento de autenticação de cookie do Katana e tíquetes de autenticação de cookie do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="69867-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="69867-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="69867-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="69867-109">O exemplo ilustra o cookie compartilhando entre três aplicativos que usam a autenticação de cookie:</span><span class="sxs-lookup"><span data-stu-id="69867-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="69867-110">Aplicativo do ASP.NET Core 2.0 as páginas do Razor sem usar [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="69867-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="69867-111">Aplicativo MVC do ASP.NET Core 2.0 com o ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="69867-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="69867-112">Aplicativo MVC do ASP.NET Framework 4.6.1 com o ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="69867-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="69867-113">Nos exemplos a seguir:</span><span class="sxs-lookup"><span data-stu-id="69867-113">In the examples that follow:</span></span>

* <span data-ttu-id="69867-114">O nome do cookie de autenticação é definido como um valor comum de `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="69867-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="69867-115">O `AuthenticationType` é definido como `Identity.Application` explicitamente ou por padrão.</span><span class="sxs-lookup"><span data-stu-id="69867-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="69867-116">Um nome de aplicativo comum é usado para permitir que o sistema de proteção de dados compartilhar as chaves de proteção de dados (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="69867-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="69867-117">`Identity.Application` é usado como o esquema de autenticação.</span><span class="sxs-lookup"><span data-stu-id="69867-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="69867-118">Qualquer esquema é usada, ele deve ser usado de forma consistente *dentro e entre* os aplicativos de cookie compartilhado como o esquema padrão ou ao defini-lo.</span><span class="sxs-lookup"><span data-stu-id="69867-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="69867-119">O esquema é usado ao criptografar e descriptografar cookies, portanto, um esquema consistente deve ser usado entre aplicativos.</span><span class="sxs-lookup"><span data-stu-id="69867-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="69867-120">Um comum [chave da proteção de dados](xref:security/data-protection/implementation/key-management) armazenamento local é usado.</span><span class="sxs-lookup"><span data-stu-id="69867-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="69867-121">O aplicativo de exemplo usa uma pasta chamada *token de autenticação* na raiz da solução para manter as chaves de proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="69867-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="69867-122">Em aplicativos ASP.NET Core, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) é usado para definir o local de armazenamento de chaves.</span><span class="sxs-lookup"><span data-stu-id="69867-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="69867-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) é usado para configurar um nome de aplicativo compartilhado comum.</span><span class="sxs-lookup"><span data-stu-id="69867-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="69867-124">No aplicativo do .NET Framework, o middleware de autenticação de cookie usa uma implementação [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="69867-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="69867-125">`DataProtectionProvider` fornece serviços de proteção de dados para a criptografia e descriptografia de dados de conteúdo do cookie de autenticação.</span><span class="sxs-lookup"><span data-stu-id="69867-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="69867-126">O `DataProtectionProvider` instância é isolada do sistema de proteção de dados usado por outras partes do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69867-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="69867-127">[DataProtectionProvider.Create (DirectoryInfo, uma ação\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) aceita uma [DirectoryInfo](/dotnet/api/system.io.directoryinfo) para especificar o local para armazenamento de chaves de proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="69867-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="69867-128">O aplicativo de exemplo fornece o caminho da *token de autenticação* pasta a ser `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="69867-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="69867-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) define o nome comum do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69867-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="69867-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) exige as [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="69867-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="69867-131">Para obter esse pacote para o ASP.NET Core 2.1 e posteriores aplicativos, fazer referência a [metapacote Microsoft](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="69867-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="69867-132">Ao direcionar o .NET Framework, adicione uma referência de pacote ao `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="69867-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="69867-133">Compartilhar cookies de autenticação entre aplicativos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69867-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="69867-134">Ao usar a identidade do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="69867-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69867-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69867-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="69867-136">No `ConfigureServices` método, use o [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) método de extensão para configurar o serviço de proteção de dados para cookies.</span><span class="sxs-lookup"><span data-stu-id="69867-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="69867-137">Chaves de proteção de dados e o nome do aplicativo devem ser compartilhados entre aplicativos.</span><span class="sxs-lookup"><span data-stu-id="69867-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="69867-138">Em aplicativos de exemplo, `GetKeyRingDirInfo` retorna o local de armazenamento de chaves comuns para o [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) método.</span><span class="sxs-lookup"><span data-stu-id="69867-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="69867-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) para configurar um nome de aplicativo compartilhado comum (`SharedCookieApp` no exemplo).</span><span class="sxs-lookup"><span data-stu-id="69867-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="69867-140">Para obter mais informações, consulte [Configurando a proteção de dados](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="69867-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="69867-141">Ao hospedar aplicativos que compartilham cookies entre subdomínios, especifique um domínio comum na [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) propriedade.</span><span class="sxs-lookup"><span data-stu-id="69867-141">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="69867-142">Compartilhar cookies entre aplicativos no `contoso.com`, como `first_subdomain.contoso.com` e `second_subdomain.contoso.com`, especifique o `Cookie.Domain` como `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="69867-142">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="69867-143">Consulte a *CookieAuthWithIdentity.Core* do projeto na [código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="69867-143">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69867-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69867-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="69867-145">No `Configure` método, use o [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) configurar:</span><span class="sxs-lookup"><span data-stu-id="69867-145">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="69867-146">O serviço de proteção de dados para cookies.</span><span class="sxs-lookup"><span data-stu-id="69867-146">The data protection service for cookies.</span></span>
* <span data-ttu-id="69867-147">O `AuthenticationScheme` para coincidir com o ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="69867-147">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="69867-148">Ao usar cookies diretamente:</span><span class="sxs-lookup"><span data-stu-id="69867-148">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69867-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69867-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="69867-150">Chaves de proteção de dados e o nome do aplicativo devem ser compartilhados entre aplicativos.</span><span class="sxs-lookup"><span data-stu-id="69867-150">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="69867-151">Em aplicativos de exemplo, `GetKeyRingDirInfo` retorna o local de armazenamento de chaves comuns para o [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) método.</span><span class="sxs-lookup"><span data-stu-id="69867-151">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="69867-152">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) para configurar um nome de aplicativo compartilhado comum (`SharedCookieApp` no exemplo).</span><span class="sxs-lookup"><span data-stu-id="69867-152">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="69867-153">Para obter mais informações, consulte [Configurando a proteção de dados](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="69867-153">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="69867-154">Ao hospedar aplicativos que compartilham cookies entre subdomínios, especifique um domínio comum na [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) propriedade.</span><span class="sxs-lookup"><span data-stu-id="69867-154">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="69867-155">Compartilhar cookies entre aplicativos no `contoso.com`, como `first_subdomain.contoso.com` e `second_subdomain.contoso.com`, especifique o `Cookie.Domain` como `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="69867-155">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="69867-156">Consulte a *CookieAuth.Core* do projeto na [código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="69867-156">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69867-157">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69867-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="69867-158">Criptografar as chaves de proteção de dados em repouso</span><span class="sxs-lookup"><span data-stu-id="69867-158">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="69867-159">Para implantações de produção, configure o `DataProtectionProvider` para criptografar as chaves em repouso com DPAPI ou um X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="69867-159">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="69867-160">Ver [chave de criptografia em repouso](xref:security/data-protection/implementation/key-encryption-at-rest) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="69867-160">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69867-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69867-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69867-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69867-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="69867-163">Compartilhamento de cookies de autenticação entre o ASP.NET 4. x e aplicativos ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69867-163">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="69867-164">Os aplicativos do ASP.NET 4.x que usam o middleware de autenticação de cookie do Katana podem ser configurados para gerar os cookies de autenticação que são compatíveis com o middleware de autenticação de cookie do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="69867-164">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="69867-165">Isso permite atualizar aplicativos individuais de um grande site gradativamente, fornecendo uma experiência de SSO suave em todo o site.</span><span class="sxs-lookup"><span data-stu-id="69867-165">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="69867-166">Quando um aplicativo usa o middleware de autenticação de cookie do Katana, ele chama `UseCookieAuthentication` do projeto *Startup.Auth.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="69867-166">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="69867-167">Projetos de aplicativo web do ASP.NET 4.x criado com o Visual Studio 2013 e posterior, use o middleware de autenticação de cookie do Katana por padrão.</span><span class="sxs-lookup"><span data-stu-id="69867-167">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span> <span data-ttu-id="69867-168">Embora `UseCookieAuthentication` está obsoleto e sem suporte para aplicativos ASP.NET Core, chamando `UseCookieAuthentication` em um aplicativo do ASP.NET 4.x que usa o Katana middleware de autenticação de cookie é válido.</span><span class="sxs-lookup"><span data-stu-id="69867-168">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana cookie authentication middleware is valid.</span></span>

<span data-ttu-id="69867-169">Um aplicativo do ASP.NET 4. x deve ter como destino do .NET Framework 4.5.1 ou superior.</span><span class="sxs-lookup"><span data-stu-id="69867-169">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="69867-170">Caso contrário, não conseguir instalar os pacotes NuGet necessários.</span><span class="sxs-lookup"><span data-stu-id="69867-170">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="69867-171">Para compartilhar cookies de autenticação entre um aplicativo do ASP.NET 4.x e um aplicativo ASP.NET Core, configurar o aplicativo ASP.NET Core, conforme mencionado acima e, em seguida, configurar o aplicativo do ASP.NET 4.x, seguindo estas etapas:</span><span class="sxs-lookup"><span data-stu-id="69867-171">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x app by following these steps:</span></span>

1. <span data-ttu-id="69867-172">Instale o pacote [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) em cada aplicativo do ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="69867-172">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="69867-173">Na *Startup.Auth.cs*, localize a chamada para `UseCookieAuthentication` e modificá-lo da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="69867-173">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="69867-174">Altere o nome do cookie para corresponder ao nome usado pelo middleware de autenticação de cookie do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="69867-174">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="69867-175">Fornecer uma instância de um `DataProtectionProvider` inicializado para o local de armazenamento de chaves de proteção de dados comuns.</span><span class="sxs-lookup"><span data-stu-id="69867-175">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="69867-176">Certifique-se de que o nome do aplicativo é definido como o nome de aplicativo comum usado por todos os aplicativos que compartilham os cookies, `SharedCookieApp` no aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="69867-176">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="69867-177">Consulte a *CookieAuthWithIdentity.NETFramework* do projeto na [código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="69867-177">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="69867-178">Ao gerar uma identidade de usuário, o tipo de autenticação deve corresponder ao tipo definido em `AuthenticationType` definido com `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="69867-178">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="69867-179">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="69867-179">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a><span data-ttu-id="69867-180">Usar um banco de dados de usuário comum</span><span class="sxs-lookup"><span data-stu-id="69867-180">Use a common user database</span></span>

<span data-ttu-id="69867-181">Confirme que o sistema de identidade para cada aplicativo está apontado para o mesmo banco de dados do usuário.</span><span class="sxs-lookup"><span data-stu-id="69867-181">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="69867-182">Caso contrário, o sistema de identidade gera falhas em tempo de execução quando ele tenta corresponder as informações no cookie de autenticação em relação as informações em seu banco de dados.</span><span class="sxs-lookup"><span data-stu-id="69867-182">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69867-183">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="69867-183">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
