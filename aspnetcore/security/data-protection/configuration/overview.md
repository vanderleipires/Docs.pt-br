---
title: Configurar a proteção de dados do ASP.NET Core
author: rick-anderson
description: Saiba como configurar a proteção de dados no ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 803b81f5f69496900791ca1d1976f70f8c266f29
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555411"
---
# <a name="configure-aspnet-core-data-protection"></a>Configurar a proteção de dados do ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Quando o sistema de proteção de dados é inicializado, ele se aplica [configurações padrão](xref:security/data-protection/configuration/default-settings) com base no ambiente operacional. Essas configurações são geralmente apropriadas para os aplicativos executados em um único computador. Há casos em que um desenvolvedor talvez queira alterar as configurações padrão:

* O aplicativo é distribuído em vários computadores.
* Por motivos de conformidade.

Nessas situações, o sistema de proteção de dados oferece uma API de configuração avançada.

> [!WARNING]
> Semelhante aos arquivos de configuração, o anel de chave de proteção de dados devem ser protegido usando as permissões apropriadas. Você pode optar por criptografar chaves em repouso, mas isso não impede que os invasores criar novas chaves. Consequentemente, a segurança de seu aplicativo é afetada. O local de armazenamento configurado com proteção de dados deve ter seu acesso limitado para o aplicativo em si, semelhante à forma como você protegerá os arquivos de configuração. Por exemplo, se você optar por armazenar o anel de chave no disco, use as permissões do sistema de arquivos. Certifique-se somente a identidade sob qual seu aplicativo web é executado tem leitura, gravação e criação de acesso a esse diretório. Se você usar o armazenamento de tabela do Azure, apenas o aplicativo web deve ter a capacidade de ler, gravar ou criar novas entradas no repositório de tabela, etc.
>
> O método de extensão [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) retorna um [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder). `IDataProtectionBuilder` expõe métodos de extensão que você pode encadear opções de configurar a proteção de dados.

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a>ProtectKeysWithAzureKeyVault

Para armazenar as chaves no [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurar o sistema com [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) no `Startup` classe:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Defina o local de armazenamento do anel de chave (por exemplo, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)). O local deve ser definido como chamar `ProtectKeysWithAzureKeyVault` implementa um [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) que desabilita as configurações de proteção automática de dados, incluindo o local de armazenamento de chave de anel. O exemplo anterior usa o armazenamento de BLOBs do Azure para manter o anel de chave. Para obter mais informações, consulte [provedores de armazenamento de chaves: Azure e Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis). Você também pode persistir o anel de chave localmente com [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).

O `keyIdentifier` é o identificador de chave de Cofre de chaves usado para criptografia de chave (por exemplo, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).

`ProtectKeysWithAzureKeyVault` sobrecargas:

* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permite o uso de um [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) para ativar o sistema de proteção de dados usar o Cofre de chaves.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permite o uso de um `ClientId` e [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) para ativar o sistema de proteção de dados usar o Cofre de chaves.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permite o uso de um `ClientId` e `ClientSecret` para ativar o sistema de proteção de dados usar o Cofre de chaves.

::: moniker-end

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

Para armazenar chaves em um compartilhamento UNC, em vez de no *% LOCALAPPDATA %* local padrão, configurar o sistema com [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> Se você alterar o local de persistência de chave, o sistema não automaticamente criptografa as chaves em repouso, desde que ele não sabe se a DPAPI é um mecanismo de criptografia apropriados.

## <a name="protectkeyswith"></a>ProtectKeysWith\*

Você pode configurar o sistema para proteger as chaves em repouso chamando qualquer o [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) APIs de configuração. Considere o exemplo a seguir, que armazena as chaves em um compartilhamento UNC e criptografa essas chaves em repouso com um certificado x. 509 específico:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

Consulte [chave de criptografia em repouso](xref:security/data-protection/implementation/key-encryption-at-rest) para obter mais exemplos e obter mais informações sobre os mecanismos de criptografia de chave interna.

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

Para configurar o sistema para usar uma vida útil de 14 dias em vez do padrão 90 dias, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

Por padrão, o sistema de proteção de dados isola aplicativos umas das outras, mesmo que compartilham o mesmo repositório de chave físico. Isso impede que os aplicativos Noções básicas sobre cargas protegidos uns dos outros. Para compartilhar cargas protegidas entre dois aplicativos, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) com o mesmo valor para cada aplicativo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

Você pode ter um cenário onde você não deseja que um aplicativo para reverter automaticamente chaves (criar novas chaves), como eles se aproximarem expiração. Um exemplo disso pode ser configurados em um relacionamento primário/secundário, em que apenas o aplicativo principal é responsável por questões de gerenciamento de chaves e aplicativos secundários simplesmente tem uma exibição somente leitura do anel de chave de aplicativos. Os aplicativos secundários podem ser configurados para tratar o anel de chave como somente leitura ao configurar o sistema com [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>Isolamento de aplicativo

Quando o sistema de proteção de dados é fornecido por um host do ASP.NET Core, ela isola automaticamente aplicativos umas das outras, mesmo se esses aplicativos estejam executando sob a mesma conta de processo de trabalho e estiver usando o mesmo material de chave mestra. Isso é semelhante ao modificador IsolateApps do System. Web  **\<machineKey >** elemento.

O mecanismo de isolamento funciona, considerando cada aplicativo no computador local como um locatário exclusivo, assim o [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) raiz para qualquer aplicativo determinado automaticamente inclui a ID do aplicativo como um discriminador. ID exclusiva do aplicativo vem de um dos seguintes locais:

1. Se o aplicativo é hospedado no IIS, o identificador exclusivo é o caminho de configuração do aplicativo. Se um aplicativo é implantado em um ambiente de farm da web, esse valor deve ser estável, supondo que os ambientes de IIS são configurados da mesma forma em todas as máquinas do web farm.

2. Se o aplicativo não está hospedado no IIS, o identificador exclusivo é o caminho físico do aplicativo.

O identificador exclusivo é projetado para sobreviver a reinicializações &mdash; do aplicativo individual e da própria máquina.

Esse mecanismo de isolamento assume que os aplicativos não são mal-intencionados. Um aplicativo mal-intencionado sempre pode afetar qualquer outro aplicativo em execução sob a mesma conta de processo de trabalho. Em um ambiente de hospedagem compartilhado que os aplicativos são mutuamente confiáveis, o provedor de hospedagem deve tomar medidas para garantir o isolamento de nível de sistema operacional entre os aplicativos, incluindo a separação de repositórios de chave de base de aplicativos.

Se o sistema de proteção de dados não é fornecido por um host do ASP.NET Core (por exemplo, se você instanciá-la por meio de `DataProtectionProvider` tipo concreto) isolamento do aplicativo é desabilitado por padrão. Quando o isolamento de aplicativos estiver desabilitado, feitos pelo mesmo material de chave de todos os aplicativos podem compartilhar cargas desde que eles fornecem apropriada [fins](xref:security/data-protection/consumer-apis/purpose-strings). Para fornecer isolamento de aplicativos nesse ambiente, chame o [SetApplicationName](#setapplicationname) método na configuração do objeto e forneça um nome exclusivo para cada aplicativo.

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>Algoritmos de alteração com UseCryptographicAlgorithms

A pilha de proteção de dados permite que você altere o algoritmo padrão usado por chaves geradas recentemente. A maneira mais simples de fazer isso é chamar [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) de retorno de chamada de configuração:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

O padrão EncryptionAlgorithm é AES-256-CBC e o padrão ValidationAlgorithm é HMACSHA256. A política padrão pode ser definida por um administrador do sistema por meio de um [abrangendo política](xref:security/data-protection/configuration/machine-wide-policy), mas uma chamada explícita para `UseCryptographicAlgorithms` substitui a política padrão.

Chamando `UseCryptographicAlgorithms` permite que você especifique o algoritmo desejado em uma lista predefinida de interno. Você não precisa se preocupar sobre a implementação do algoritmo. No cenário acima, o sistema de proteção de dados tenta usar a implementação de CNG do AES se em execução no Windows. Caso contrário, ele reverterá para o gerenciado [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) classe.

Você pode especificar manualmente uma implementação por meio de uma chamada para [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).

> [!TIP]
> Alterar algoritmos não afeta as chaves existentes do anel de chave. Ela afeta apenas as chaves geradas recentemente.

### <a name="specifying-custom-managed-algorithms"></a>Especificando algoritmos gerenciados personalizados

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para especificar algoritmos gerenciados personalizados, crie um [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instância que aponta para os tipos de implementação:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para especificar algoritmos gerenciados personalizados, crie um [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instância que aponta para os tipos de implementação:

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

Geralmente o \*propriedades de tipo devem apontar para concreto, podem ser instanciadas (por meio de um construtor sem parâmetros público) implementações de [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) e [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), embora o sistema caso especial, como alguns valores `typeof(Aes)` para sua conveniência.

> [!NOTE]
> O SymmetricAlgorithm deve ter um comprimento de chave de ≥ 128 bits e um tamanho de bloco de ≥ 64 bits e ele deve oferecer suporte à criptografia de modo CBC com preenchimento de PKCS #7. O KeyedHashAlgorithm deve ter um tamanho de resumo de > = 128 bits, e ele deve oferecer suporte a chaves de comprimento igual ao comprimento de resumo do algoritmo de hash. O KeyedHashAlgorithm não é estritamente necessária para ser HMAC.

### <a name="specifying-custom-windows-cng-algorithms"></a>Especificação de algoritmos personalizados de Windows CNG

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para especificar um algoritmo CNG Windows personalizado usando a criptografia de modo CBC com validação HMAC, crie um [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instância que contém as informações de algoritmos:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para especificar um algoritmo CNG Windows personalizado usando a criptografia de modo CBC com validação HMAC, crie um [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instância que contém as informações de algoritmos:

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
> O algoritmo de criptografia simétrica de bloco deve ter um comprimento de chave de > = 128 bits, um tamanho de bloco de > = 64 bits, e ele deve oferecer suporte à criptografia de modo CBC com preenchimento de PKCS #7. O algoritmo de hash deve ter um tamanho de resumo de > = 128 bits e deve oferecer suporte à que está sendo aberta com o BCRYPT\_ALG\_tratar\_HMAC\_sinalizador de sinalizador. O \*propriedades do provedor podem ser definidas para nulo para usar o provedor padrão para o algoritmo especificado. Consulte o [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentação para obter mais informações.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para especificar um algoritmo CNG Windows personalizado usando a criptografia de modo Galois/contador com a validação, crie um [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instância que contém as informações de algoritmos:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para especificar um algoritmo CNG Windows personalizado usando a criptografia de modo Galois/contador com a validação, crie um [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instância que contém as informações de algoritmos:

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
> O algoritmo de criptografia simétrica de bloco deve ter um comprimento de chave de > = 128 bits, um tamanho de bloco de exatamente de 128 bits, e ele deve oferecer suporte à criptografia do GCM. Você pode definir o [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) propriedade como nulo para usar o provedor padrão para o algoritmo especificado. Consulte o [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentação para obter mais informações.

### <a name="specifying-other-custom-algorithms"></a>Especificar outros algoritmos personalizados

Embora não exposta como uma API de primeira classe, o sistema de proteção de dados é extensível o suficiente para permitir a especificação de praticamente qualquer tipo de algoritmo. Por exemplo, é possível manter todas as chaves contidas dentro de um módulo HSM (Hardware Security) e fornecer uma implementação personalizada do núcleo rotinas de criptografia e descriptografia. Consulte [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) na [extensibilidade da criptografia de núcleo](xref:security/data-protection/extensibility/core-crypto) para obter mais informações.

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Chaves persistentes ao hospedar em um contêiner de Docker

Ao hospedar em um [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) contêiner, as chaves devem ser mantidas no:

* Uma pasta que é um volume de Docker persiste por mais tempo de vida do contêiner, como um volume compartilhado ou um volume montado de host.
* Um provedor externo, como [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ou [Redis](https://redis.io/).

## <a name="see-also"></a>Consulte também

* [Cenários sem reconhecimento de DI](xref:security/data-protection/configuration/non-di-scenarios)
* [Política para todo o computador](xref:security/data-protection/configuration/machine-wide-policy)
