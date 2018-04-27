---
title: Configurar a proteção de dados do ASP.NET Core
author: rick-anderson
description: Saiba como configurar a proteção de dados no ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 07/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 300feb42dff7f1bb86bab6fedf3f657273ced8be
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2018
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="50247-103">Configurar a proteção de dados do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="50247-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="50247-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="50247-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="50247-105">Quando o sistema de proteção de dados é inicializado, ele se aplica [configurações padrão](xref:security/data-protection/configuration/default-settings) com base no ambiente operacional.</span><span class="sxs-lookup"><span data-stu-id="50247-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="50247-106">Essas configurações são geralmente apropriadas para os aplicativos executados em um único computador.</span><span class="sxs-lookup"><span data-stu-id="50247-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="50247-107">Há casos em que um desenvolvedor talvez queira alterar as configurações padrão:</span><span class="sxs-lookup"><span data-stu-id="50247-107">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="50247-108">O aplicativo é distribuído em vários computadores.</span><span class="sxs-lookup"><span data-stu-id="50247-108">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="50247-109">Por motivos de conformidade.</span><span class="sxs-lookup"><span data-stu-id="50247-109">For compliance reasons.</span></span>

<span data-ttu-id="50247-110">Nessas situações, o sistema de proteção de dados oferece uma API de configuração avançada.</span><span class="sxs-lookup"><span data-stu-id="50247-110">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="50247-111">Semelhante aos arquivos de configuração, o anel de chave de proteção de dados devem ser protegido usando as permissões apropriadas.</span><span class="sxs-lookup"><span data-stu-id="50247-111">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="50247-112">Você pode optar por criptografar chaves em repouso, mas isso não impede que os invasores criar novas chaves.</span><span class="sxs-lookup"><span data-stu-id="50247-112">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="50247-113">Consequentemente, a segurança de seu aplicativo é afetada.</span><span class="sxs-lookup"><span data-stu-id="50247-113">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="50247-114">O local de armazenamento configurado com proteção de dados deve ter seu acesso limitado para o aplicativo em si, semelhante à forma como você protegerá os arquivos de configuração.</span><span class="sxs-lookup"><span data-stu-id="50247-114">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="50247-115">Por exemplo, se você optar por armazenar o anel de chave no disco, use as permissões do sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="50247-115">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="50247-116">Certifique-se somente a identidade sob qual seu aplicativo web é executado tem leitura, gravação e criação de acesso a esse diretório.</span><span class="sxs-lookup"><span data-stu-id="50247-116">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="50247-117">Se você usar o armazenamento de tabela do Azure, apenas o aplicativo web deve ter a capacidade de ler, gravar ou criar novas entradas no repositório de tabela, etc.</span><span class="sxs-lookup"><span data-stu-id="50247-117">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="50247-118">O método de extensão [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) retorna um [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="50247-118">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="50247-119">`IDataProtectionBuilder` expõe métodos de extensão que você pode encadear opções de configurar a proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="50247-119">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

## <a name="persistkeystofilesystem"></a><span data-ttu-id="50247-120">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="50247-120">PersistKeysToFileSystem</span></span>

<span data-ttu-id="50247-121">Para armazenar chaves em um compartilhamento UNC, em vez de no *% LOCALAPPDATA %* local padrão, configurar o sistema com [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="50247-121">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="50247-122">Se você alterar o local de persistência de chave, o sistema não automaticamente criptografa as chaves em repouso, desde que ele não sabe se a DPAPI é um mecanismo de criptografia apropriados.</span><span class="sxs-lookup"><span data-stu-id="50247-122">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="50247-123">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="50247-123">ProtectKeysWith\*</span></span>

<span data-ttu-id="50247-124">Você pode configurar o sistema para proteger as chaves em repouso chamando qualquer o [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) APIs de configuração.</span><span class="sxs-lookup"><span data-stu-id="50247-124">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="50247-125">Considere o exemplo a seguir, que armazena as chaves em um compartilhamento UNC e criptografa essas chaves em repouso com um certificado x. 509 específico:</span><span class="sxs-lookup"><span data-stu-id="50247-125">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="50247-126">Consulte [chave de criptografia em repouso](xref:security/data-protection/implementation/key-encryption-at-rest) para obter mais exemplos e obter mais informações sobre os mecanismos de criptografia de chave interna.</span><span class="sxs-lookup"><span data-stu-id="50247-126">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="50247-127">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="50247-127">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="50247-128">Para configurar o sistema para usar uma vida útil de 14 dias em vez do padrão 90 dias, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="50247-128">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="50247-129">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="50247-129">SetApplicationName</span></span>

<span data-ttu-id="50247-130">Por padrão, o sistema de proteção de dados isola aplicativos umas das outras, mesmo que compartilham o mesmo repositório de chave físico.</span><span class="sxs-lookup"><span data-stu-id="50247-130">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="50247-131">Isso impede que os aplicativos Noções básicas sobre cargas protegidos uns dos outros.</span><span class="sxs-lookup"><span data-stu-id="50247-131">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="50247-132">Para compartilhar cargas protegidas entre dois aplicativos, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) com o mesmo valor para cada aplicativo:</span><span class="sxs-lookup"><span data-stu-id="50247-132">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="50247-133">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="50247-133">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="50247-134">Você pode ter um cenário onde você não deseja que um aplicativo para reverter automaticamente chaves (criar novas chaves), como eles se aproximarem expiração.</span><span class="sxs-lookup"><span data-stu-id="50247-134">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="50247-135">Um exemplo disso pode ser configurados em um relacionamento primário/secundário, em que apenas o aplicativo principal é responsável por questões de gerenciamento de chaves e aplicativos secundários simplesmente tem uma exibição somente leitura do anel de chave de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="50247-135">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="50247-136">Os aplicativos secundários podem ser configurados para tratar o anel de chave como somente leitura ao configurar o sistema com [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="50247-136">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="50247-137">Isolamento de aplicativo</span><span class="sxs-lookup"><span data-stu-id="50247-137">Per-application isolation</span></span>

<span data-ttu-id="50247-138">Quando o sistema de proteção de dados é fornecido por um host do ASP.NET Core, ela isola automaticamente aplicativos umas das outras, mesmo se esses aplicativos estejam executando sob a mesma conta de processo de trabalho e estiver usando o mesmo material de chave mestra.</span><span class="sxs-lookup"><span data-stu-id="50247-138">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="50247-139">Isso é semelhante ao modificador IsolateApps do System. Web  **\<machineKey >** elemento.</span><span class="sxs-lookup"><span data-stu-id="50247-139">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="50247-140">O mecanismo de isolamento funciona, considerando cada aplicativo no computador local como um locatário exclusivo, assim o [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) raiz para qualquer aplicativo determinado automaticamente inclui a ID do aplicativo como um discriminador.</span><span class="sxs-lookup"><span data-stu-id="50247-140">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="50247-141">ID exclusiva do aplicativo vem de um dos seguintes locais:</span><span class="sxs-lookup"><span data-stu-id="50247-141">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="50247-142">Se o aplicativo é hospedado no IIS, o identificador exclusivo é o caminho de configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="50247-142">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="50247-143">Se um aplicativo é implantado em um ambiente de farm da web, esse valor deve ser estável, supondo que os ambientes de IIS são configurados da mesma forma em todas as máquinas do web farm.</span><span class="sxs-lookup"><span data-stu-id="50247-143">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="50247-144">Se o aplicativo não está hospedado no IIS, o identificador exclusivo é o caminho físico do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="50247-144">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="50247-145">O identificador exclusivo é projetado para sobreviver a reinicializações &mdash; do aplicativo individual e da própria máquina.</span><span class="sxs-lookup"><span data-stu-id="50247-145">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="50247-146">Esse mecanismo de isolamento assume que os aplicativos não são mal-intencionados.</span><span class="sxs-lookup"><span data-stu-id="50247-146">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="50247-147">Um aplicativo mal-intencionado sempre pode afetar qualquer outro aplicativo em execução sob a mesma conta de processo de trabalho.</span><span class="sxs-lookup"><span data-stu-id="50247-147">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="50247-148">Em um ambiente de hospedagem compartilhado que os aplicativos são mutuamente confiáveis, o provedor de hospedagem deve tomar medidas para garantir o isolamento de nível de sistema operacional entre os aplicativos, incluindo a separação de repositórios de chave de base de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="50247-148">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="50247-149">Se o sistema de proteção de dados não é fornecido por um host do ASP.NET Core (por exemplo, se você instanciá-la por meio de `DataProtectionProvider` tipo concreto) isolamento do aplicativo é desabilitado por padrão.</span><span class="sxs-lookup"><span data-stu-id="50247-149">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="50247-150">Quando o isolamento de aplicativos estiver desabilitado, feitos pelo mesmo material de chave de todos os aplicativos podem compartilhar cargas desde que eles fornecem apropriada [fins](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="50247-150">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="50247-151">Para fornecer isolamento de aplicativos nesse ambiente, chame o [SetApplicationName](#setapplicationname) método na configuração do objeto e forneça um nome exclusivo para cada aplicativo.</span><span class="sxs-lookup"><span data-stu-id="50247-151">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="50247-152">Algoritmos de alteração com UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="50247-152">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="50247-153">A pilha de proteção de dados permite que você altere o algoritmo padrão usado por chaves geradas recentemente.</span><span class="sxs-lookup"><span data-stu-id="50247-153">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="50247-154">A maneira mais simples de fazer isso é chamar [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) de retorno de chamada de configuração:</span><span class="sxs-lookup"><span data-stu-id="50247-154">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50247-155">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50247-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50247-156">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50247-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="50247-157">O padrão EncryptionAlgorithm é AES-256-CBC e o padrão ValidationAlgorithm é HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="50247-157">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="50247-158">A política padrão pode ser definida por um administrador do sistema por meio de um [abrangendo política](xref:security/data-protection/configuration/machine-wide-policy), mas uma chamada explícita para `UseCryptographicAlgorithms` substitui a política padrão.</span><span class="sxs-lookup"><span data-stu-id="50247-158">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="50247-159">Chamando `UseCryptographicAlgorithms` permite que você especifique o algoritmo desejado em uma lista predefinida de interno.</span><span class="sxs-lookup"><span data-stu-id="50247-159">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="50247-160">Você não precisa se preocupar sobre a implementação do algoritmo.</span><span class="sxs-lookup"><span data-stu-id="50247-160">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="50247-161">No cenário acima, o sistema de proteção de dados tenta usar a implementação de CNG do AES se em execução no Windows.</span><span class="sxs-lookup"><span data-stu-id="50247-161">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="50247-162">Caso contrário, ele reverterá para o gerenciado [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) classe.</span><span class="sxs-lookup"><span data-stu-id="50247-162">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="50247-163">Você pode especificar manualmente uma implementação por meio de uma chamada para [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="50247-163">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="50247-164">Alterar algoritmos não afeta as chaves existentes do anel de chave.</span><span class="sxs-lookup"><span data-stu-id="50247-164">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="50247-165">Ela afeta apenas as chaves geradas recentemente.</span><span class="sxs-lookup"><span data-stu-id="50247-165">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="50247-166">Especificando algoritmos gerenciados personalizados</span><span class="sxs-lookup"><span data-stu-id="50247-166">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50247-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50247-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="50247-168">Para especificar algoritmos gerenciados personalizados, crie um [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instância que aponta para os tipos de implementação:</span><span class="sxs-lookup"><span data-stu-id="50247-168">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50247-169">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50247-169">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="50247-170">Para especificar algoritmos gerenciados personalizados, crie um [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instância que aponta para os tipos de implementação:</span><span class="sxs-lookup"><span data-stu-id="50247-170">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

<span data-ttu-id="50247-171">Geralmente o \*propriedades de tipo devem apontar para concreto, podem ser instanciadas (por meio de um construtor sem parâmetros público) implementações de [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) e [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), embora o sistema caso especial, como alguns valores `typeof(Aes)` para sua conveniência.</span><span class="sxs-lookup"><span data-stu-id="50247-171">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="50247-172">O SymmetricAlgorithm deve ter um comprimento de chave de ≥ 128 bits e um tamanho de bloco de ≥ 64 bits e ele deve oferecer suporte à criptografia de modo CBC com preenchimento de PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="50247-172">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="50247-173">O KeyedHashAlgorithm deve ter um tamanho de resumo de > = 128 bits, e ele deve oferecer suporte a chaves de comprimento igual ao comprimento de resumo do algoritmo de hash.</span><span class="sxs-lookup"><span data-stu-id="50247-173">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="50247-174">O KeyedHashAlgorithm não é estritamente necessária para ser HMAC.</span><span class="sxs-lookup"><span data-stu-id="50247-174">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="50247-175">Especificação de algoritmos personalizados de Windows CNG</span><span class="sxs-lookup"><span data-stu-id="50247-175">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50247-176">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50247-176">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="50247-177">Para especificar um algoritmo CNG Windows personalizado usando a criptografia de modo CBC com validação HMAC, crie um [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instância que contém as informações de algoritmos:</span><span class="sxs-lookup"><span data-stu-id="50247-177">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50247-178">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50247-178">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="50247-179">Para especificar um algoritmo CNG Windows personalizado usando a criptografia de modo CBC com validação HMAC, crie um [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instância que contém as informações de algoritmos:</span><span class="sxs-lookup"><span data-stu-id="50247-179">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> <span data-ttu-id="50247-180">O algoritmo de criptografia simétrica de bloco deve ter um comprimento de chave de > = 128 bits, um tamanho de bloco de > = 64 bits, e ele deve oferecer suporte à criptografia de modo CBC com preenchimento de PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="50247-180">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="50247-181">O algoritmo de hash deve ter um tamanho de resumo de > = 128 bits e deve oferecer suporte à que está sendo aberta com o BCRYPT\_ALG\_tratar\_HMAC\_sinalizador de sinalizador.</span><span class="sxs-lookup"><span data-stu-id="50247-181">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="50247-182">O \*propriedades do provedor podem ser definidas para nulo para usar o provedor padrão para o algoritmo especificado.</span><span class="sxs-lookup"><span data-stu-id="50247-182">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="50247-183">Consulte o [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentação para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="50247-183">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50247-184">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50247-184">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="50247-185">Para especificar um algoritmo CNG Windows personalizado usando a criptografia de modo Galois/contador com a validação, crie um [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instância que contém as informações de algoritmos:</span><span class="sxs-lookup"><span data-stu-id="50247-185">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50247-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50247-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="50247-187">Para especificar um algoritmo CNG Windows personalizado usando a criptografia de modo Galois/contador com a validação, crie um [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instância que contém as informações de algoritmos:</span><span class="sxs-lookup"><span data-stu-id="50247-187">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> <span data-ttu-id="50247-188">O algoritmo de criptografia simétrica de bloco deve ter um comprimento de chave de > = 128 bits, um tamanho de bloco de exatamente de 128 bits, e ele deve oferecer suporte à criptografia do GCM.</span><span class="sxs-lookup"><span data-stu-id="50247-188">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="50247-189">Você pode definir o [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) propriedade como nulo para usar o provedor padrão para o algoritmo especificado.</span><span class="sxs-lookup"><span data-stu-id="50247-189">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="50247-190">Consulte o [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentação para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="50247-190">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="50247-191">Especificar outros algoritmos personalizados</span><span class="sxs-lookup"><span data-stu-id="50247-191">Specifying other custom algorithms</span></span>

<span data-ttu-id="50247-192">Embora não exposta como uma API de primeira classe, o sistema de proteção de dados é extensível o suficiente para permitir a especificação de praticamente qualquer tipo de algoritmo.</span><span class="sxs-lookup"><span data-stu-id="50247-192">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="50247-193">Por exemplo, é possível manter todas as chaves contidas dentro de um módulo HSM (Hardware Security) e fornecer uma implementação personalizada do núcleo rotinas de criptografia e descriptografia.</span><span class="sxs-lookup"><span data-stu-id="50247-193">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="50247-194">Consulte [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) na [extensibilidade da criptografia de núcleo](xref:security/data-protection/extensibility/core-crypto) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="50247-194">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="50247-195">Chaves persistentes ao hospedar em um contêiner de Docker</span><span class="sxs-lookup"><span data-stu-id="50247-195">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="50247-196">Ao hospedar em um [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) contêiner, as chaves devem ser mantidas no:</span><span class="sxs-lookup"><span data-stu-id="50247-196">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="50247-197">Uma pasta que é um volume de Docker persiste por mais tempo de vida do contêiner, como um volume compartilhado ou um volume montado de host.</span><span class="sxs-lookup"><span data-stu-id="50247-197">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="50247-198">Um provedor externo, como [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ou [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="50247-198">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="50247-199">Consulte também</span><span class="sxs-lookup"><span data-stu-id="50247-199">See also</span></span>

* [<span data-ttu-id="50247-200">Cenários sem reconhecimento de DI</span><span class="sxs-lookup"><span data-stu-id="50247-200">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="50247-201">Política para todo o computador</span><span class="sxs-lookup"><span data-stu-id="50247-201">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
