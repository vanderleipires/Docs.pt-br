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
# <a name="configuring-data-protection"></a>Configurando a proteção de dados

<a name=data-protection-configuring></a>

Quando o sistema de proteção de dados é inicializado aplica alguns [configurações padrão](default-settings.md#data-protection-default-settings) com base no ambiente operacional. Essas configurações são geralmente bons para aplicativos em execução em um único computador. Há alguns casos em que um desenvolvedor talvez queira alterar essas (talvez porque o aplicativo é distribuído em vários computadores ou por motivos de conformidade), e para esses cenários, o sistema de proteção de dados oferece uma API de configuração avançada.

<a name=data-protection-configuration-callback></a>

Há um método de extensão AddDataProtection que retorna um IDataProtectionBuilder que expõe os métodos de extensão que você pode encadear para configurar a proteção de dados de várias opções. Por exemplo, para armazenar chaves em um compartilhamento UNC, em vez de % LOCALAPPDATA % (o padrão), configure o sistema da seguinte maneira:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> Se você alterar o local de persistência de chave, o sistema não automaticamente criptografará chaves em repouso desde que ele não sabe se a DPAPI é um mecanismo de criptografia apropriados.

<a name=configuring-x509-certificate></a>

Você pode configurar o sistema para proteger as chaves em repouso chamando qualquer o ProtectKeysWith\* APIs de configuração. Considere o exemplo a seguir, que armazena as chaves em um compartilhamento UNC e criptografa essas chaves em repouso com um certificado x. 509 específico.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

Consulte [chave criptografia em repouso](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para obter mais exemplos e para obter mais informações sobre os mecanismos de criptografia de chave interna.

Para configurar o sistema para usar um tempo de vida de chave no padrão de 14 dias, em vez de 90 dias, considere o exemplo a seguir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

Por padrão o sistema de proteção de dados isola os aplicativos uns dos outros, mesmo que compartilham o mesmo repositório de chave físico. Isso impede que os aplicativos Noções básicas sobre cargas protegidos uns dos outros. Para compartilhar cargas protegidas entre dois aplicativos diferentes, configurar o sistema passando o mesmo nome de aplicativo para aplicativos como no exemplo abaixo:

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

Por fim, você pode ter um cenário onde você não desejar uma aplicação para reverter automaticamente chaves como eles se aproximarem expiração. Um exemplo disso pode ser configurados em um relacionamento primário / secundário, em que apenas o aplicativo principal é responsável por questões de gerenciamento de chaves, e todos os aplicativos secundários simplesmente tem uma exibição somente leitura do anel de chave de aplicativos. Os aplicativos secundários podem ser configurados para tratar o anel de chave como somente leitura ao configurar o sistema como abaixo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name=data-protection-configuration-per-app-isolation></a>

## <a name="per-application-isolation"></a>Isolamento de aplicativo

Quando o sistema de proteção de dados é fornecido por um host do ASP.NET Core, ele será automaticamente isolar aplicativos umas das outras, mesmo se esses aplicativos estejam executando sob a mesma conta de processo de trabalho e estiver usando o mesmo material de chave mestra. Isso é semelhante ao modificador IsolateApps do System. Web <machineKey> elemento.

O funcionamento do mecanismo de isolamento, considerando cada aplicativo no computador local como um locatário exclusivo, portanto o IDataProtector raiz para um determinado aplicativo automaticamente inclui a ID do aplicativo como um discriminador. ID exclusiva do aplicativo vem de um dos dois locais.

1. Se o aplicativo está hospedado no IIS, o identificador exclusivo é o caminho de configuração do aplicativo. Se um aplicativo é implantado em um ambiente de farm, esse valor deve ser estável, supondo que os ambientes de IIS são configurados da mesma forma em todos os computadores no farm.

2. Se o aplicativo não está hospedado no IIS, o identificador exclusivo é o caminho físico do aplicativo.

O identificador exclusivo é projetado para sobreviver a reinicializações - do aplicativo individual e da própria máquina.

Esse mecanismo de isolamento assume que os aplicativos não são mal-intencionados. Um aplicativo mal-intencionado sempre pode afetar qualquer outro aplicativo em execução sob a mesma conta de processo de trabalho. Em um ambiente de hospedagem compartilhado onde os aplicativos são mutuamente confiáveis, o provedor de hospedagem deve tomar medidas para garantir o isolamento de nível de sistema operacional entre aplicativos, incluindo separando os aplicativos subjacente repositórios de chave.

Se o sistema de proteção de dados não é fornecido por um host do ASP.NET Core (por exemplo, se o desenvolvedor cria-se através do tipo concreto DataProtectionProvider), o isolamento do aplicativo é desabilitado por padrão e todos os aplicativos com o apoio da mesma chave material pode compartilhar cargas desde que eles fornecem as finalidades apropriadas. Para fornecer isolamento de aplicativos nesse ambiente, chame o método SetApplicationName no objeto de configuração, consulte o [exemplo de código](#data-protection-code-sample-application-name) acima.

<a name=data-protection-changing-algorithms></a>

## <a name="changing-algorithms"></a>Algoritmos de alteração

A pilha de proteção de dados permite alterar o algoritmo padrão usado por chaves geradas recentemente. A maneira mais simples de fazer isso é chamar UseCryptographicAlgorithms de retorno de chamada de configuração, como no exemplo abaixo.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

O padrão EncryptionAlgorithm e ValidationAlgorithm são AES-256-CBC e HMACSHA256, respectivamente. A política padrão pode ser definida por um administrador do sistema por meio de [política de computador ampla](machine-wide-policy.md), mas uma chamada explícita para UseCryptographicAlgorithms substituirá a política padrão.

Chamar UseCryptographicAlgorithms permitirá que o desenvolvedor especificar o algoritmo desejado (em uma lista predefinida de interna) e o desenvolvedor não precisa se preocupar sobre a implementação do algoritmo. Por exemplo, no cenário anterior, o sistema de proteção de dados tentará usar a implementação CNG AES se em execução no Windows, caso contrário, ele voltará para a classe System.Security.Cryptography.Aes gerenciada.

O desenvolvedor pode especificar manualmente uma implementação se desejado por meio de uma chamada para UseCustomCryptographicAlgorithms, como mostrado a seguir exemplos.

>[!TIP]
> Algoritmos de alteração não afetam as chaves existentes do anel de chave. Ela afeta apenas as chaves geradas recentemente.

<a name=data-protection-changing-algorithms-custom-managed></a>

### <a name="specifying-custom-managed-algorithms"></a>Especificando algoritmos gerenciados personalizados

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Para especificar algoritmos gerenciados personalizados, crie uma instância de ManagedAuthenticatedEncryptorConfiguration que aponta para os tipos de implementação.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Para especificar algoritmos gerenciados personalizados, crie uma instância de ManagedAuthenticatedEncryptionSettings que aponta para os tipos de implementação.

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

Geralmente o \*propriedades de tipo devem apontar para concreto, podem ser instanciadas (por meio de um construtor sem parâmetros público) implementações de SymmetricAlgorithm e KeyedHashAlgorithm, embora alguns valores, os casos especiais de sistema como typeof(Aes) para conveniência .

> [!NOTE]
> O SymmetricAlgorithm deve ter um comprimento de chave de ≥ 128 bits e um tamanho de bloco de ≥ 64 bits e ele deve oferecer suporte à criptografia de modo CBC com preenchimento de PKCS #7. O KeyedHashAlgorithm deve ter um tamanho de resumo de > = 128 bits, e ele deve oferecer suporte a chaves de comprimento igual ao comprimento de resumo do algoritmo de hash. O KeyedHashAlgorithm não é estritamente necessária para ser HMAC.

<a name=data-protection-changing-algorithms-cng></a>

### <a name="specifying-custom-windows-cng-algorithms"></a>Especificação de algoritmos personalizados de Windows CNG

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Para especificar um algoritmo personalizado do Windows CNG usando criptografia de modo CBC + validação HMAC, crie uma instância de CngCbcAuthenticatedEncryptorConfiguration que contém as informações de algoritmos.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Para especificar um algoritmo personalizado do Windows CNG usando criptografia de modo CBC + validação HMAC, crie uma instância de CngCbcAuthenticatedEncryptionSettings que contém as informações de algoritmos.

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
> O algoritmo de criptografia simétrica de bloco deve ter um comprimento de chave de ≥ 128 bits e um tamanho de bloco de ≥ 64 bits, e ele deve oferecer suporte à criptografia de modo CBC com preenchimento de PKCS #7. O algoritmo de hash deve ter um tamanho de resumo de > = 128 bits e deve oferecer suporte à que está sendo aberto com o sinalizador BCRYPT_ALG_HANDLE_HMAC_FLAG. O \*propriedades do provedor podem ser definidas para nulo para usar o provedor padrão para o algoritmo especificado. Consulte o [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentação para obter mais informações.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Para especificar um algoritmo personalizado do Windows CNG usando criptografia de modo Galois/contador + validação, crie uma instância de CngGcmAuthenticatedEncryptorConfiguration que contém as informações de algoritmos.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Para especificar um algoritmo personalizado do Windows CNG usando criptografia de modo Galois/contador + validação, crie uma instância de CngGcmAuthenticatedEncryptionSettings que contém as informações de algoritmos.

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
> O algoritmo de criptografia simétrica de bloco deve ter um comprimento de chave de ≥ 128 bits e um tamanho de bloco de exatamente de 128 bits e ele deve oferecer suporte à criptografia do GCM. A propriedade EncryptionAlgorithmProvider pode ser definida como null para usar o provedor padrão para o algoritmo especificado. Consulte o [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentação para obter mais informações.

### <a name="specifying-other-custom-algorithms"></a>Especificar outros algoritmos personalizados

Embora não exposta como uma API de primeira classe, o sistema de proteção de dados é extensível o suficiente para permitir a especificação de praticamente qualquer tipo de algoritmo. Por exemplo, é possível manter todas as chaves contidas em um HSM e fornecer uma implementação personalizada do núcleo rotinas de criptografia e descriptografia. Consulte IAuthenticatedEncryptorConfiguration na seção principal de extensibilidade de criptografia para obter mais informações.

### <a name="see-also"></a>Consulte também

* [Cenários com suporte a DI não](non-di-scenarios.md)
* [Política de largura de máquina](machine-wide-policy.md)
