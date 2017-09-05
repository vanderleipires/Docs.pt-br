---
title: "Configurando a proteção de dados"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0e4881a3-a94d-4e35-9c1c-f025d65dcff0
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 39fab796c24456d61a6a103c4a3f7a8722b4718c
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2017
---
# <a name="configuring-data-protection"></a><span data-ttu-id="a42d9-103">Configurando a proteção de dados</span><span class="sxs-lookup"><span data-stu-id="a42d9-103">Configuring data protection</span></span>

<a name=data-protection-configuring></a>

<span data-ttu-id="a42d9-104">Quando o sistema de proteção de dados é inicializado aplica alguns [configurações padrão](default-settings.md#data-protection-default-settings) com base no ambiente operacional.</span><span class="sxs-lookup"><span data-stu-id="a42d9-104">When the data protection system is initialized it applies some [default settings](default-settings.md#data-protection-default-settings) based on the operational environment.</span></span> <span data-ttu-id="a42d9-105">Essas configurações são geralmente bons para aplicativos em execução em um único computador.</span><span class="sxs-lookup"><span data-stu-id="a42d9-105">These settings are generally good for applications running on a single machine.</span></span> <span data-ttu-id="a42d9-106">Há alguns casos em que um desenvolvedor talvez queira alterar essas (talvez porque o aplicativo é distribuído em vários computadores ou por motivos de conformidade), e para esses cenários, o sistema de proteção de dados oferece uma API de configuração avançada.</span><span class="sxs-lookup"><span data-stu-id="a42d9-106">There are some cases where a developer may want to change these (perhaps because their application is spread across multiple machines or for compliance reasons), and for these scenarios the data protection system offers a rich configuration API.</span></span>

<a name=data-protection-configuration-callback></a>

<span data-ttu-id="a42d9-107">Há um método de extensão AddDataProtection que retorna um IDataProtectionBuilder que expõe os métodos de extensão que você pode encadear para configurar a proteção de dados de várias opções.</span><span class="sxs-lookup"><span data-stu-id="a42d9-107">There is an extension method AddDataProtection which returns an IDataProtectionBuilder which itself exposes extension methods that you can chain together to configure various data protection options.</span></span> <span data-ttu-id="a42d9-108">Por exemplo, para armazenar chaves em um compartilhamento UNC, em vez de % LOCALAPPDATA % (o padrão), configure o sistema da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="a42d9-108">For instance, to store keys at a UNC share instead of %LOCALAPPDATA% (the default), configure the system as follows:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> <span data-ttu-id="a42d9-109">Se você alterar o local de persistência de chave, o sistema não automaticamente criptografará chaves em repouso desde que ele não sabe se a DPAPI é um mecanismo de criptografia apropriados.</span><span class="sxs-lookup"><span data-stu-id="a42d9-109">If you change the key persistence location, the system will no longer automatically encrypt keys at rest since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

<a name=configuring-x509-certificate></a>

<span data-ttu-id="a42d9-110">Você pode configurar o sistema para proteger as chaves em repouso chamando qualquer o ProtectKeysWith\* APIs de configuração.</span><span class="sxs-lookup"><span data-stu-id="a42d9-110">You can configure the system to protect keys at rest by calling any of the ProtectKeysWith\* configuration APIs.</span></span> <span data-ttu-id="a42d9-111">Considere o exemplo a seguir, que armazena as chaves em um compartilhamento UNC e criptografa essas chaves em repouso com um certificado x. 509 específico.</span><span class="sxs-lookup"><span data-stu-id="a42d9-111">Consider the example below, which stores keys at a UNC share and encrypts those keys at rest with a specific X.509 certificate.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="a42d9-112">Consulte [chave criptografia em repouso](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para obter mais exemplos e para obter mais informações sobre os mecanismos de criptografia de chave interna.</span><span class="sxs-lookup"><span data-stu-id="a42d9-112">See [key encryption at rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more examples and for discussion on the built-in key encryption mechanisms.</span></span>

<span data-ttu-id="a42d9-113">Para configurar o sistema para usar um tempo de vida de chave no padrão de 14 dias, em vez de 90 dias, considere o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="a42d9-113">To configure the system to use a default key lifetime of 14 days instead of 90 days, consider the following example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

<span data-ttu-id="a42d9-114">Por padrão o sistema de proteção de dados isola os aplicativos uns dos outros, mesmo que compartilham o mesmo repositório de chave físico.</span><span class="sxs-lookup"><span data-stu-id="a42d9-114">By default the data protection system isolates applications from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="a42d9-115">Isso impede que os aplicativos Noções básicas sobre cargas protegidos uns dos outros.</span><span class="sxs-lookup"><span data-stu-id="a42d9-115">This prevents the applications from understanding each other's protected payloads.</span></span> <span data-ttu-id="a42d9-116">Para compartilhar cargas protegidas entre dois aplicativos diferentes, configurar o sistema passando o mesmo nome de aplicativo para aplicativos como no exemplo abaixo:</span><span class="sxs-lookup"><span data-stu-id="a42d9-116">To share protected payloads between two different applications, configure the system passing in the same application name for both applications as in the below example:</span></span>

<a name=data-protection-code-sample-application-name></a>

<!-- literal_block {"ids": ["data-protection-code-sample-application-name"], "linenos": false, "names": ["data-protection-code-sample-application-name"], "xml:space": "preserve", "language": "csharp"} -->

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("my application");
}
```

<a name=data-protection-configuring-disable-automatic-key-generation></a>

<span data-ttu-id="a42d9-117">Por fim, você pode ter um cenário onde você não desejar uma aplicação para reverter automaticamente chaves como eles se aproximarem expiração.</span><span class="sxs-lookup"><span data-stu-id="a42d9-117">Finally, you may have a scenario where you do not want an application to automatically roll keys as they approach expiration.</span></span> <span data-ttu-id="a42d9-118">Um exemplo disso pode ser configurados em um relacionamento primário / secundário, em que apenas o aplicativo principal é responsável por questões de gerenciamento de chaves, e todos os aplicativos secundários simplesmente tem uma exibição somente leitura do anel de chave de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="a42d9-118">One example of this might be applications set up in a primary / secondary relationship, where only the primary application is responsible for key management concerns, and all secondary applications simply have a read-only view of the key ring.</span></span> <span data-ttu-id="a42d9-119">Os aplicativos secundários podem ser configurados para tratar o anel de chave como somente leitura ao configurar o sistema como abaixo:</span><span class="sxs-lookup"><span data-stu-id="a42d9-119">The secondary applications can be configured to treat the key ring as read-only by configuring the system as below:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name=data-protection-configuration-per-app-isolation></a>

## <a name="per-application-isolation"></a><span data-ttu-id="a42d9-120">Isolamento de aplicativo</span><span class="sxs-lookup"><span data-stu-id="a42d9-120">Per-application isolation</span></span>

<span data-ttu-id="a42d9-121">Quando o sistema de proteção de dados é fornecido por um host do ASP.NET Core, ele será automaticamente isolar aplicativos umas das outras, mesmo se esses aplicativos estejam executando sob a mesma conta de processo de trabalho e estiver usando o mesmo material de chave mestra.</span><span class="sxs-lookup"><span data-stu-id="a42d9-121">When the data protection system is provided by an ASP.NET Core host, it will automatically isolate applications from one another, even if those applications are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="a42d9-122">Isso é semelhante ao modificador IsolateApps do System. Web <machineKey> elemento.</span><span class="sxs-lookup"><span data-stu-id="a42d9-122">This is somewhat similar to the IsolateApps modifier from System.Web's <machineKey> element.</span></span>

<span data-ttu-id="a42d9-123">O funcionamento do mecanismo de isolamento, considerando cada aplicativo no computador local como um locatário exclusivo, portanto o IDataProtector raiz para um determinado aplicativo automaticamente inclui a ID do aplicativo como um discriminador.</span><span class="sxs-lookup"><span data-stu-id="a42d9-123">The isolation mechanism works by considering each application on the local machine as a unique tenant, thus the IDataProtector rooted for any given application automatically includes the application ID as a discriminator.</span></span> <span data-ttu-id="a42d9-124">ID exclusiva do aplicativo vem de um dos dois locais.</span><span class="sxs-lookup"><span data-stu-id="a42d9-124">The application's unique ID comes from one of two places.</span></span>

1. <span data-ttu-id="a42d9-125">Se o aplicativo está hospedado no IIS, o identificador exclusivo é o caminho de configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a42d9-125">If the application is hosted in IIS, the unique identifier is the application's configuration path.</span></span> <span data-ttu-id="a42d9-126">Se um aplicativo é implantado em um ambiente de farm, esse valor deve ser estável, supondo que os ambientes de IIS são configurados da mesma forma em todos os computadores no farm.</span><span class="sxs-lookup"><span data-stu-id="a42d9-126">If an application is deployed in a farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the farm.</span></span>

2. <span data-ttu-id="a42d9-127">Se o aplicativo não está hospedado no IIS, o identificador exclusivo é o caminho físico do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a42d9-127">If the application is not hosted in IIS, the unique identifier is the physical path of the application.</span></span>

<span data-ttu-id="a42d9-128">O identificador exclusivo é projetado para sobreviver a reinicializações - do aplicativo individual e da própria máquina.</span><span class="sxs-lookup"><span data-stu-id="a42d9-128">The unique identifier is designed to survive resets - both of the individual application and of the machine itself.</span></span>

<span data-ttu-id="a42d9-129">Esse mecanismo de isolamento assume que os aplicativos não são mal-intencionados.</span><span class="sxs-lookup"><span data-stu-id="a42d9-129">This isolation mechanism assumes that the applications are not malicious.</span></span> <span data-ttu-id="a42d9-130">Um aplicativo mal-intencionado sempre pode afetar qualquer outro aplicativo em execução sob a mesma conta de processo de trabalho.</span><span class="sxs-lookup"><span data-stu-id="a42d9-130">A malicious application can always impact any other application running under the same worker process account.</span></span> <span data-ttu-id="a42d9-131">Em um ambiente de hospedagem compartilhado onde os aplicativos são mutuamente confiáveis, o provedor de hospedagem deve tomar medidas para garantir o isolamento de nível de sistema operacional entre aplicativos, incluindo separando os aplicativos subjacente repositórios de chave.</span><span class="sxs-lookup"><span data-stu-id="a42d9-131">In a shared hosting environment where applications are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between applications, including separating the applications' underlying key repositories.</span></span>

<span data-ttu-id="a42d9-132">Se o sistema de proteção de dados não é fornecido por um host do ASP.NET Core (por exemplo, se o desenvolvedor cria-se através do tipo concreto DataProtectionProvider), o isolamento do aplicativo é desabilitado por padrão e todos os aplicativos com o apoio da mesma chave material pode compartilhar cargas desde que eles fornecem as finalidades apropriadas.</span><span class="sxs-lookup"><span data-stu-id="a42d9-132">If the data protection system is not provided by an ASP.NET Core host (e.g., if the developer instantiates it himself via the DataProtectionProvider concrete type), application isolation is disabled by default, and all applications backed by the same keying material can share payloads as long as they provide the appropriate purposes.</span></span> <span data-ttu-id="a42d9-133">Para fornecer isolamento de aplicativos nesse ambiente, chame o método SetApplicationName no objeto de configuração, consulte o [exemplo de código](#data-protection-code-sample-application-name) acima.</span><span class="sxs-lookup"><span data-stu-id="a42d9-133">To provide application isolation in this environment, call the SetApplicationName method on the configuration object, see the [code sample](#data-protection-code-sample-application-name) above.</span></span>

<a name=data-protection-changing-algorithms></a>

## <a name="changing-algorithms"></a><span data-ttu-id="a42d9-134">Algoritmos de alteração</span><span class="sxs-lookup"><span data-stu-id="a42d9-134">Changing algorithms</span></span>

<span data-ttu-id="a42d9-135">A pilha de proteção de dados permite alterar o algoritmo padrão usado por chaves geradas recentemente.</span><span class="sxs-lookup"><span data-stu-id="a42d9-135">The data protection stack allows changing the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="a42d9-136">A maneira mais simples de fazer isso é chamar UseCryptographicAlgorithms de retorno de chamada de configuração, como no exemplo abaixo.</span><span class="sxs-lookup"><span data-stu-id="a42d9-136">The simplest way to do this is to call UseCryptographicAlgorithms from the configuration callback, as in the below example.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a42d9-137">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="a42d9-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a42d9-138">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="a42d9-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="a42d9-139">O padrão EncryptionAlgorithm e ValidationAlgorithm são AES-256-CBC e HMACSHA256, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="a42d9-139">The default EncryptionAlgorithm and ValidationAlgorithm are AES-256-CBC and HMACSHA256, respectively.</span></span> <span data-ttu-id="a42d9-140">A política padrão pode ser definida por um administrador do sistema por meio de [política de computador ampla](machine-wide-policy.md), mas uma chamada explícita para UseCryptographicAlgorithms substituirá a política padrão.</span><span class="sxs-lookup"><span data-stu-id="a42d9-140">The default policy can be set by a system administrator via [Machine Wide Policy](machine-wide-policy.md), but an explicit call to UseCryptographicAlgorithms will override the default policy.</span></span>

<span data-ttu-id="a42d9-141">Chamar UseCryptographicAlgorithms permitirá que o desenvolvedor especificar o algoritmo desejado (em uma lista predefinida de interna) e o desenvolvedor não precisa se preocupar sobre a implementação do algoritmo.</span><span class="sxs-lookup"><span data-stu-id="a42d9-141">Calling UseCryptographicAlgorithms will allow the developer to specify the desired algorithm (from a predefined built-in list), and the developer does not need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="a42d9-142">Por exemplo, no cenário anterior, o sistema de proteção de dados tentará usar a implementação CNG AES se em execução no Windows, caso contrário, ele voltará para a classe System.Security.Cryptography.Aes gerenciada.</span><span class="sxs-lookup"><span data-stu-id="a42d9-142">For instance, in the scenario above the data protection system will attempt to use the CNG implementation of AES if running on Windows, otherwise it will fall back to the managed System.Security.Cryptography.Aes class.</span></span>

<span data-ttu-id="a42d9-143">O desenvolvedor pode especificar manualmente uma implementação se desejado por meio de uma chamada para UseCustomCryptographicAlgorithms, como mostrado a seguir exemplos.</span><span class="sxs-lookup"><span data-stu-id="a42d9-143">The developer can manually specify an implementation if desired via a call to UseCustomCryptographicAlgorithms, as show in the below examples.</span></span>

>[!TIP]
> <span data-ttu-id="a42d9-144">Algoritmos de alteração não afetam as chaves existentes do anel de chave.</span><span class="sxs-lookup"><span data-stu-id="a42d9-144">Changing algorithms does not affect existing keys in the key ring.</span></span> <span data-ttu-id="a42d9-145">Ela afeta apenas as chaves geradas recentemente.</span><span class="sxs-lookup"><span data-stu-id="a42d9-145">It only affects newly-generated keys.</span></span>

<a name=data-protection-changing-algorithms-custom-managed></a>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="a42d9-146">Especificando algoritmos gerenciados personalizados</span><span class="sxs-lookup"><span data-stu-id="a42d9-146">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a42d9-147">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="a42d9-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a42d9-148">Para especificar algoritmos gerenciados personalizados, crie uma instância de ManagedAuthenticatedEncryptorConfiguration que aponta para os tipos de implementação.</span><span class="sxs-lookup"><span data-stu-id="a42d9-148">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptorConfiguration instance that points to the implementation types.</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptorConfiguration()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a42d9-149">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="a42d9-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a42d9-150">Para especificar algoritmos gerenciados personalizados, crie uma instância de ManagedAuthenticatedEncryptionSettings que aponta para os tipos de implementação.</span><span class="sxs-lookup"><span data-stu-id="a42d9-150">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptionSettings instance that points to the implementation types.</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptionSettings()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

<span data-ttu-id="a42d9-151">Geralmente o \*propriedades de tipo devem apontar para concreto, podem ser instanciadas (por meio de um construtor sem parâmetros público) implementações de SymmetricAlgorithm e KeyedHashAlgorithm, embora alguns valores, os casos especiais de sistema como typeof(Aes) para conveniência .</span><span class="sxs-lookup"><span data-stu-id="a42d9-151">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of SymmetricAlgorithm and KeyedHashAlgorithm, though the system special-cases some values like typeof(Aes) for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="a42d9-152">O SymmetricAlgorithm deve ter um comprimento de chave de ≥ 128 bits e um tamanho de bloco de ≥ 64 bits e ele deve oferecer suporte à criptografia de modo CBC com preenchimento de PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="a42d9-152">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="a42d9-153">O KeyedHashAlgorithm deve ter um tamanho de resumo de > = 128 bits, e ele deve oferecer suporte a chaves de comprimento igual ao comprimento de resumo do algoritmo de hash.</span><span class="sxs-lookup"><span data-stu-id="a42d9-153">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="a42d9-154">O KeyedHashAlgorithm não é estritamente necessária para ser HMAC.</span><span class="sxs-lookup"><span data-stu-id="a42d9-154">The KeyedHashAlgorithm is not strictly required to be HMAC.</span></span>

<a name=data-protection-changing-algorithms-cng></a>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="a42d9-155">Especificação de algoritmos personalizados de Windows CNG</span><span class="sxs-lookup"><span data-stu-id="a42d9-155">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a42d9-156">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="a42d9-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a42d9-157">Para especificar um algoritmo personalizado do Windows CNG usando criptografia de modo CBC + validação HMAC, crie uma instância de CngCbcAuthenticatedEncryptorConfiguration que contém as informações de algoritmos.</span><span class="sxs-lookup"><span data-stu-id="a42d9-157">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a42d9-158">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="a42d9-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a42d9-159">Para especificar um algoritmo personalizado do Windows CNG usando criptografia de modo CBC + validação HMAC, crie uma instância de CngCbcAuthenticatedEncryptionSettings que contém as informações de algoritmos.</span><span class="sxs-lookup"><span data-stu-id="a42d9-159">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> <span data-ttu-id="a42d9-160">O algoritmo de criptografia simétrica de bloco deve ter um comprimento de chave de ≥ 128 bits e um tamanho de bloco de ≥ 64 bits, e ele deve oferecer suporte à criptografia de modo CBC com preenchimento de PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="a42d9-160">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="a42d9-161">O algoritmo de hash deve ter um tamanho de resumo de > = 128 bits e deve oferecer suporte à que está sendo aberto com o sinalizador BCRYPT_ALG_HANDLE_HMAC_FLAG.</span><span class="sxs-lookup"><span data-stu-id="a42d9-161">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT_ALG_HANDLE_HMAC_FLAG flag.</span></span> <span data-ttu-id="a42d9-162">O \*propriedades do provedor podem ser definidas para nulo para usar o provedor padrão para o algoritmo especificado.</span><span class="sxs-lookup"><span data-stu-id="a42d9-162">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="a42d9-163">Consulte o [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentação para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="a42d9-163">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a42d9-164">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="a42d9-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a42d9-165">Para especificar um algoritmo personalizado do Windows CNG usando criptografia de modo Galois/contador + validação, crie uma instância de CngGcmAuthenticatedEncryptorConfiguration que contém as informações de algoritmos.</span><span class="sxs-lookup"><span data-stu-id="a42d9-165">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a42d9-166">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="a42d9-166">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a42d9-167">Para especificar um algoritmo personalizado do Windows CNG usando criptografia de modo Galois/contador + validação, crie uma instância de CngGcmAuthenticatedEncryptionSettings que contém as informações de algoritmos.</span><span class="sxs-lookup"><span data-stu-id="a42d9-167">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> <span data-ttu-id="a42d9-168">O algoritmo de criptografia simétrica de bloco deve ter um comprimento de chave de ≥ 128 bits e um tamanho de bloco de exatamente de 128 bits e ele deve oferecer suporte à criptografia do GCM.</span><span class="sxs-lookup"><span data-stu-id="a42d9-168">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="a42d9-169">A propriedade EncryptionAlgorithmProvider pode ser definida como null para usar o provedor padrão para o algoritmo especificado.</span><span class="sxs-lookup"><span data-stu-id="a42d9-169">The EncryptionAlgorithmProvider property can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="a42d9-170">Consulte o [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentação para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="a42d9-170">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="a42d9-171">Especificar outros algoritmos personalizados</span><span class="sxs-lookup"><span data-stu-id="a42d9-171">Specifying other custom algorithms</span></span>

<span data-ttu-id="a42d9-172">Embora não exposta como uma API de primeira classe, o sistema de proteção de dados é extensível o suficiente para permitir a especificação de praticamente qualquer tipo de algoritmo.</span><span class="sxs-lookup"><span data-stu-id="a42d9-172">Though not exposed as a first-class API, the data protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="a42d9-173">Por exemplo, é possível manter todas as chaves contidas em um HSM e fornecer uma implementação personalizada do núcleo rotinas de criptografia e descriptografia.</span><span class="sxs-lookup"><span data-stu-id="a42d9-173">For example, it is possible to keep all keys contained within an HSM and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="a42d9-174">Consulte IAuthenticatedEncryptorConfiguration na seção principal de extensibilidade de criptografia para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="a42d9-174">See IAuthenticatedEncryptorConfiguration in the core cryptography extensibility section for more information.</span></span>

### <a name="see-also"></a><span data-ttu-id="a42d9-175">Consulte também</span><span class="sxs-lookup"><span data-stu-id="a42d9-175">See also</span></span>

* [<span data-ttu-id="a42d9-176">Cenários com suporte a DI não</span><span class="sxs-lookup"><span data-stu-id="a42d9-176">Non DI Aware Scenarios</span></span>](non-di-scenarios.md)
* [<span data-ttu-id="a42d9-177">Política de largura de máquina</span><span class="sxs-lookup"><span data-stu-id="a42d9-177">Machine Wide Policy</span></span>](machine-wide-policy.md)
