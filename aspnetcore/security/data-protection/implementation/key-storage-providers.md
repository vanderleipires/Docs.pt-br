---
title: Provedores de armazenamento de chaves no ASP.NET Core
author: rick-anderson
description: Saiba mais sobre provedores de armazenamento de chaves no ASP.NET Core e como configurar locais de armazenamento de chaves.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 0e64a65ab1d65efa9f2e4d36a23663b607f206d7
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402062"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="4bcc2-103">Provedores de armazenamento de chaves no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4bcc2-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="4bcc2-104">O sistema de proteção de dados [emprega um mecanismo de descoberta por padrão](xref:security/data-protection/configuration/default-settings) para determinar onde as chaves de criptografia devem ser persistente.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="4bcc2-105">O desenvolvedor pode substituir o mecanismo de descoberta padrão e especificar manualmente o local.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="4bcc2-106">Se você especificar um local de persistência de chave explícita, o sistema de proteção de dados cancela o registro a criptografia de chave padrão no mecanismo de rest, portanto, as chaves não são criptografadas em repouso.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="4bcc2-107">É recomendável que você adicionalmente [especificar um mecanismo de criptografia de chave explícita](xref:security/data-protection/implementation/key-encryption-at-rest) para implantações de produção.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="4bcc2-108">Sistema de arquivos</span><span class="sxs-lookup"><span data-stu-id="4bcc2-108">File system</span></span>

<span data-ttu-id="4bcc2-109">Para configurar um repositório chave baseadas no sistema de arquivos, chame o [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) rotina de configuração, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="4bcc2-110">Fornecer um [DirectoryInfo](/dotnet/api/system.io.directoryinfo) apontando para o repositório em que as chaves devem ser armazenadas:</span><span class="sxs-lookup"><span data-stu-id="4bcc2-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="4bcc2-111">O `DirectoryInfo` pode apontar para um diretório no computador local, ou ele pode apontar para uma pasta em um compartilhamento de rede.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="4bcc2-112">Se apontando para um diretório no computador local (e o cenário é que apenas os aplicativos no computador local exigem acesso para usar esse repositório), considere o uso de [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (no Windows) para criptografar as chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="4bcc2-113">Caso contrário, considere o uso de um [certificado x. 509](xref:security/data-protection/implementation/key-encryption-at-rest) para criptografar as chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="4bcc2-114">O Azure e Redis</span><span class="sxs-lookup"><span data-stu-id="4bcc2-114">Azure and Redis</span></span>

<span data-ttu-id="4bcc2-115">O [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) e [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) pacotes permitem armazenar chaves de proteção de dados no armazenamento do Azure ou um cache Redis.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) and [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) packages allow storing data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="4bcc2-116">As chaves podem ser compartilhadas entre várias instâncias de um aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="4bcc2-117">Aplicativos podem compartilhar cookies de autenticação ou proteção CSRF em vários servidores.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="4bcc2-118">Para configurar o provedor de armazenamento de BLOBs do Azure, chame um dos [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) sobrecargas:</span><span class="sxs-lookup"><span data-stu-id="4bcc2-118">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

<span data-ttu-id="4bcc2-119">Para configurar o Redis, chame um dos [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) sobrecargas:</span><span class="sxs-lookup"><span data-stu-id="4bcc2-119">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

<span data-ttu-id="4bcc2-120">Para mais informações, consulte os seguintes tópicos:</span><span class="sxs-lookup"><span data-stu-id="4bcc2-120">For more information, see the following topics:</span></span>

* [<span data-ttu-id="4bcc2-121">ConnectionMultiplexer stackexchange. Redis</span><span class="sxs-lookup"><span data-stu-id="4bcc2-121">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="4bcc2-122">Cache Redis do Azure</span><span class="sxs-lookup"><span data-stu-id="4bcc2-122">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="4bcc2-123">exemplos de ASPNET/DataProtection</span><span class="sxs-lookup"><span data-stu-id="4bcc2-123">aspnet/DataProtection samples</span></span>](https://github.com/aspnet/DataProtection/tree/master/samples)

## <a name="registry"></a><span data-ttu-id="4bcc2-124">Registro</span><span class="sxs-lookup"><span data-stu-id="4bcc2-124">Registry</span></span>

<span data-ttu-id="4bcc2-125">**Só se aplica às implantações do Windows.**</span><span class="sxs-lookup"><span data-stu-id="4bcc2-125">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="4bcc2-126">Às vezes, o aplicativo pode não ter acesso de gravação para o sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-126">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="4bcc2-127">Considere um cenário em que um aplicativo é executado como uma conta de serviço virtual (como *w3wp.exe*da identidade do pool de aplicativo).</span><span class="sxs-lookup"><span data-stu-id="4bcc2-127">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="4bcc2-128">Nesses casos, o administrador pode provisionar uma chave do registro que seja acessível pela identidade da conta de serviço.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-128">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="4bcc2-129">Chame o [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) método de extensão, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-129">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="4bcc2-130">Fornecer um [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) apontando para o local onde as chaves de criptografia devem ser armazenadas:</span><span class="sxs-lookup"><span data-stu-id="4bcc2-130">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="4bcc2-131">É recomendável usar [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) para criptografar as chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-131">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a><span data-ttu-id="4bcc2-132">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4bcc2-132">Entity Framework Core</span></span>

<span data-ttu-id="4bcc2-133">O [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) pacote fornece um mecanismo para armazenar chaves de proteção de dados para um banco de dados usando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-133">The [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package provides a mechanism for storing data protection keys to a database using Entity Framework Core.</span></span> <span data-ttu-id="4bcc2-134">O `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` pacote do NuGet deve ser adicionado ao arquivo de projeto, ele não é parte do [metapacote Microsoft](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4bcc2-134">The `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet package must be added to the project file, it's not part of the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4bcc2-135">Com esse pacote, as chaves podem ser compartilhadas entre várias instâncias de um aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="4bcc2-135">With this package, keys can be shared across multiple instances of a web app.</span></span>

<span data-ttu-id="4bcc2-136">Para configurar o provedor do EF Core, chame o [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) método:</span><span class="sxs-lookup"><span data-stu-id="4bcc2-136">To configure the EF Core provider, call the [`PersistKeysToDbContext<TContext>`](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) method:</span></span>

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

<span data-ttu-id="4bcc2-137">O parâmetro genérico, `TContext`, deve herdar de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) e [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span><span class="sxs-lookup"><span data-stu-id="4bcc2-137">The generic parameter, `TContext`, must inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span></span>

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

::: moniker-end

## <a name="custom-key-repository"></a><span data-ttu-id="4bcc2-138">Repositório de chave personalizado</span><span class="sxs-lookup"><span data-stu-id="4bcc2-138">Custom key repository</span></span>

<span data-ttu-id="4bcc2-139">Se os mecanismos de caixa de entrada não forem apropriados, o desenvolvedor pode especificar seu próprio mecanismo de persistência de chave, fornecendo um personalizado [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span><span class="sxs-lookup"><span data-stu-id="4bcc2-139">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
