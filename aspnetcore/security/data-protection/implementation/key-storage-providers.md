---
title: Provedores de armazenamento de chaves
author: rick-anderson
description: Provedores de armazenamento de chaves
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: d7cbb4786be0acf9679f43466460c3833f1db6fb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="key-storage-providers"></a><span data-ttu-id="c1598-103">Provedores de armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="c1598-103">Key storage providers</span></span>

<a name="data-protection-implementation-key-storage-providers"></a>

<span data-ttu-id="c1598-104">Por padrão, o sistema de proteção de dados [emprega uma heurística](xref:security/data-protection/configuration/default-settings) para determinar onde o material de chave de criptografia deve ser persistente.</span><span class="sxs-lookup"><span data-stu-id="c1598-104">By default the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="c1598-105">O desenvolvedor pode substituir a heurística e especificar manualmente o local.</span><span class="sxs-lookup"><span data-stu-id="c1598-105">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="c1598-106">Se você especificar um local de persistência de chave explícita, o sistema de proteção de dados irá cancelar o registro a criptografia de chave padrão no mecanismo de rest que a heurística fornecida, para que as chaves não serão criptografadas em repouso.</span><span class="sxs-lookup"><span data-stu-id="c1598-106">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="c1598-107">É recomendável que você adicionalmente [especificar um mecanismo de criptografia de chave explícita](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) para aplicativos de produção.</span><span class="sxs-lookup"><span data-stu-id="c1598-107">It's recommended that you additionally [specify an explicit key encryption mechanism](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="c1598-108">O sistema de proteção de dados é fornecido com vários provedores de armazenamento de chaves da caixa de entrada.</span><span class="sxs-lookup"><span data-stu-id="c1598-108">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="c1598-109">Sistema de arquivos</span><span class="sxs-lookup"><span data-stu-id="c1598-109">File system</span></span>

<span data-ttu-id="c1598-110">Estimamos que muitos aplicativos usarão um repositório chave baseado no sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="c1598-110">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="c1598-111">Para configurar isso, chame o [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) rotina de configuração, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="c1598-111">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="c1598-112">Forneça um `DirectoryInfo` apontando para o repositório onde as chaves devem ser armazenadas.</span><span class="sxs-lookup"><span data-stu-id="c1598-112">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="c1598-113">O `DirectoryInfo` pode apontar para um diretório no computador local ou ela pode apontar para uma pasta em um compartilhamento de rede.</span><span class="sxs-lookup"><span data-stu-id="c1598-113">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="c1598-114">Se apontando para um diretório no computador local (e o cenário é que apenas os aplicativos no computador local precisa usar esse repositório), considere o uso de [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para criptografar as chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="c1598-114">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="c1598-115">Caso contrário, considere o uso de um [certificado x. 509](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para criptografar as chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="c1598-115">Otherwise consider using an [X.509 certificate](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="c1598-116">Azure e Redis</span><span class="sxs-lookup"><span data-stu-id="c1598-116">Azure and Redis</span></span>

<span data-ttu-id="c1598-117">O `Microsoft.AspNetCore.DataProtection.AzureStorage` e `Microsoft.AspNetCore.DataProtection.Redis` pacotes permitem armazenar as chaves de proteção de dados no armazenamento do Azure ou um cache Redis.</span><span class="sxs-lookup"><span data-stu-id="c1598-117">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="c1598-118">As chaves podem ser compartilhadas entre várias instâncias de um aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="c1598-118">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="c1598-119">Seu aplicativo ASP.NET Core pode compartilhar os cookies de autenticação ou proteção CSRF entre vários servidores.</span><span class="sxs-lookup"><span data-stu-id="c1598-119">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="c1598-120">Para configurar o Azure, chame um do [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) sobrecargas conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="c1598-120">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="c1598-121">Consulte também o [código de teste do Azure](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="c1598-121">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="c1598-122">Para configurar o Redis, chame um do [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) sobrecargas conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="c1598-122">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

<span data-ttu-id="c1598-123">Para obter mais informações, consulte o seguinte:</span><span class="sxs-lookup"><span data-stu-id="c1598-123">See the following for more information:</span></span>

- [<span data-ttu-id="c1598-124">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="c1598-124">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="c1598-125">Cache Redis do Azure</span><span class="sxs-lookup"><span data-stu-id="c1598-125">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="c1598-126">[Código de teste de redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="c1598-126">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="c1598-127">Registro</span><span class="sxs-lookup"><span data-stu-id="c1598-127">Registry</span></span>

<span data-ttu-id="c1598-128">Às vezes, o aplicativo talvez não tenha acesso de gravação para o sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="c1598-128">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="c1598-129">Considere um cenário em que um aplicativo está em execução como uma conta de serviço virtual (como a identidade do pool de aplicativos do w3wp.exe).</span><span class="sxs-lookup"><span data-stu-id="c1598-129">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="c1598-130">Nesses casos, o administrador pode ter provisionado uma chave do registro ACLed apropriado para a identidade da conta de serviço.</span><span class="sxs-lookup"><span data-stu-id="c1598-130">In these cases, the administrator may have provisioned a registry key that's appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="c1598-131">Chamar o [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) rotina de configuração, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="c1598-131">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="c1598-132">Forneça um `RegistryKey` apontando para o local onde os valores/chaves de criptografia deve ser armazenados.</span><span class="sxs-lookup"><span data-stu-id="c1598-132">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="c1598-133">Se você usar o registro do sistema como um mecanismo de persistência, considere o uso de [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para criptografar as chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="c1598-133">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="c1598-134">Repositório de chave personalizado</span><span class="sxs-lookup"><span data-stu-id="c1598-134">Custom key repository</span></span>

<span data-ttu-id="c1598-135">Se os mecanismos de caixa de entrada não forem apropriados, o desenvolvedor pode especificar seu próprio mecanismo de persistência de chave, fornecendo um personalizado `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="c1598-135">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
