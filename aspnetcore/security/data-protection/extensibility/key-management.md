---
title: Extensibilidade de gerenciamento de chaves
author: rick-anderson
description: "Este documento descreve a extensibilidade de gerenciamento de chaves de proteção de dados do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 11/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: bcc4984efcee9a6ffd0f3b503a38089c78adf5e8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="key-management-extensibility"></a>Extensibilidade de gerenciamento de chaves

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> Leitura de [gerenciamento de chaves](../implementation/key-management.md#data-protection-implementation-key-management) seção antes de ler esta seção, como explica alguns dos conceitos fundamentais dessas APIs.

>[!WARNING]
> Tipos que implementam qualquer uma das seguintes interfaces devem ser thread-safe para chamadores vários.

## <a name="key"></a>Chave

O `IKey` interface é a representação básica de uma chave em criptográfico. A chave de termo é usada aqui no sentido abstrato, não no sentido de literal de "material de chave criptográfica". Uma chave tem as seguintes propriedades:

* Datas de expiração, a criação e a ativação

* Status de revogação

* Identificador de chave (uma GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Além disso, `IKey` expõe um `CreateEncryptor` método que pode ser usado para criar um [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instância associada a essa chave.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Além disso, `IKey` expõe um `CreateEncryptorInstance` método que pode ser usado para criar um [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instância associada a essa chave.

---

> [!NOTE]
> Há uma API para recuperar o material criptográfico bruto de um `IKey` instância.

## <a name="ikeymanager"></a>IKeyManager

O `IKeyManager` interface representa um objeto responsável pelo armazenamento de chaves geral, a recuperação e manipulação. Ela apresenta três operações de alto nível:

* Crie uma nova chave e mantê-lo para o armazenamento.

* Obter todas as chaves de armazenamento.

* Revogar uma ou mais chaves e manter as informações de revogação para o armazenamento.

>[!WARNING]
> Escrevendo um `IKeyManager` é uma tarefa muito avançada e a maioria dos desenvolvedores não deve tentar. Em vez disso, a maioria dos desenvolvedores deve aproveitar os recursos oferecidos pelo [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) classe.

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

O `XmlKeyManager` tipo é a implementação concreta de caixa de entrada da `IKeyManager`. Ele fornece vários recursos úteis, incluindo caução de chaves e criptografia de chaves em repouso. As chaves no sistema são representadas como elementos XML (especificamente, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` depende de vários outros componentes no decorrer de atender às suas tarefas:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* `AlgorithmConfiguration`, que determina os algoritmos usados por novas chaves.

* `IXmlRepository`, que controla onde as chaves são mantidas no armazenamento.

* `IXmlEncryptor` [opcional], que permite a criptografia de chaves em repouso.

* `IKeyEscrowSink` [opcional], que fornece serviços de caução de chaves.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `IXmlRepository`, que controla onde as chaves são mantidas no armazenamento.

* `IXmlEncryptor` [opcional], que permite a criptografia de chaves em repouso.

* `IKeyEscrowSink` [opcional], que fornece serviços de caução de chaves.

---

Abaixo estão os diagramas de alto nível que indicam como esses componentes são conectados em `XmlKeyManager`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Criação da chave](key-management/_static/keycreation2.png)

   *Criação de chave / CreateNewKey*

Na implementação de `CreateNewKey`, o `AlgorithmConfiguration` componente é usado para criar uma única `IAuthenticatedEncryptorDescriptor`, que, em seguida, é serializado como XML. Se houver um coletor de caução de chaves, o XML bruto de (não criptografado) é fornecido para o coletor para armazenamento de longo prazo. O XML não criptografado é executado por meio de um `IXmlEncryptor` (se necessário) para gerar o documento XML criptografado. Este documento criptografado é persistido no armazenamento de longo prazo por meio de `IXmlRepository`. (Se nenhum `IXmlEncryptor` é configurado, o documento não criptografado é mantido no `IXmlRepository`.)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Criação da chave](key-management/_static/keycreation1.png)

   *Criação de chave / CreateNewKey*

Na implementação de `CreateNewKey`, o `IAuthenticatedEncryptorConfiguration` componente é usado para criar uma única `IAuthenticatedEncryptorDescriptor`, que, em seguida, é serializado como XML. Se houver um coletor de caução de chaves, o XML bruto de (não criptografado) é fornecido para o coletor para armazenamento de longo prazo. O XML não criptografado é executado por meio de um `IXmlEncryptor` (se necessário) para gerar o documento XML criptografado. Este documento criptografado é persistido no armazenamento de longo prazo por meio de `IXmlRepository`. (Se nenhum `IXmlEncryptor` é configurado, o documento não criptografado é mantido no `IXmlRepository`.)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Recuperação de chave](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Recuperação de chave](key-management/_static/keyretrieval1.png)

---

   *Recuperação de chave / GetAllKeys*

Na implementação de `GetAllKeys`, representa chaves de documentos XML e revogações são lidas do subjacente `IXmlRepository`. Se esses documentos são criptografados, o sistema automaticamente descriptografá-los. `XmlKeyManager` cria apropriada `IAuthenticatedEncryptorDescriptorDeserializer` instâncias para desserializar os documentos de volta para o `IAuthenticatedEncryptorDescriptor` instâncias, que, em seguida, são quebradas em individuais `IKey` instâncias. Esta coleção de `IKey` instâncias é retornado ao chamador.

Obter mais informações sobre os elementos XML específicos podem ser encontradas na [documento do formato de armazenamento de chaves](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

O `IXmlRepository` interface representa um tipo que pode persistir XML e recuperar o XML de um repositório de backup. Ela apresenta duas APIs:

* GetAllElements() : IReadOnlyCollection<XElement>

* StoreElement (elemento XElement, friendlyName de cadeia de caracteres)

Implementações de `IXmlRepository` não é necessário analisar o XML passando por eles. Eles devem tratar os documentos XML como opaco e permitir que camadas superiores se preocupar sobre como gerar e analisar os documentos.

Há dois tipos internos concretos que implementam `IXmlRepository`: `FileSystemXmlRepository` e `RegistryXmlRepository`. Consulte o [documento de provedores de armazenamento de chaves](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) para obter mais informações. Registrando um personalizado `IXmlRepository` seria da maneira adequada para usar um armazenamento de backup diferente, por exemplo, o armazenamento de BLOBs do Azure.

Para alterar o repositório padrão do nível de aplicativo, registre um personalizado `IXmlRepository` instância:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

O `IXmlEncryptor` interface representa um tipo que pode criptografar um elemento XML de texto sem formatação. Ela apresenta uma única API:

* Criptografar (plaintextElement XElement): EncryptedXmlInfo

Se um serializado `IAuthenticatedEncryptorDescriptor` contém todos os elementos marcados como "requer criptografia", em seguida, `XmlKeyManager` executará desses elementos por meio de `IXmlEncryptor`do `Encrypt` método e ele serão mantido o elemento enciphered em vez de elemento de texto sem formatação para o `IXmlRepository`. A saída de `Encrypt` método é um `EncryptedXmlInfo` objeto. O objeto é um wrapper que contém ambos os resultantes enciphered `XElement` e o tipo que representa um `IXmlDecryptor` que pode ser usado para o elemento correspondente de decifrar.

Há quatro tipos internos concretos que implementam `IXmlEncryptor`:
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

Consulte o [criptografia de chave no documento de rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para obter mais informações.

Para alterar o mecanismo padrão de chave criptografia em repouso todo o aplicativo, registre um personalizado `IXmlEncryptor` instância:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a>IXmlDecryptor

O `IXmlDecryptor` interface representa um tipo que sabe como descriptografar um `XElement` que foi enciphered por meio de um `IXmlEncryptor`. Ela apresenta uma única API:

* Descriptografar (encryptedElement XElement): XElement

O `Decrypt` método desfaz a criptografia executada pelo `IXmlEncryptor.Encrypt`. Em geral, cada concreto `IXmlEncryptor` implementação terá um concreto correspondente `IXmlDecryptor` implementação.

Tipos que implementam `IXmlDecryptor` devem ter um dos dois construtores públicos a seguir:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> O `IServiceProvider` passado para o construtor pode ser nulo.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

O `IKeyEscrowSink` interface representa um tipo que pode executar caução de informações confidenciais. Lembre-se de que descritores serializados podem conter informações confidenciais (por exemplo, o material criptográfico), e isso é o que levou à introdução do [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) digite em primeiro lugar. No entanto, acidentes acontecem e anéis de chave podem ser excluídos ou corrompidos.

A interface de caução fornece uma trava de escape de emergência, permitindo o acesso ao XML serializado bruto antes que ele é transformado por qualquer configurado [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor). A interface expõe uma única API:

* Armazenamento (keyId Guid, o elemento de XElement)

Ele é até o `IKeyEscrowSink` implementação para lidar com o elemento fornecido de maneira segura consistente com a política de negócios. Uma possível implementação pode ser para o coletor de caução criptografar o elemento XML usando um certificado x. 509 corporativo conhecido em que a chave privada do certificado tem foram enviado; o `CertificateXmlEncryptor` tipo pode ajudar com isso. O `IKeyEscrowSink` implementação também é responsável por manter o elemento fornecido adequadamente.

Por padrão nenhum mecanismo caução estiver habilitado, embora os administradores de servidor podem [configurar isso globalmente](xref:security/data-protection/configuration/machine-wide-policy). Também pode ser configurado programaticamente por meio de `IDataProtectionBuilder.AddKeyEscrowSink` método conforme mostrado no exemplo abaixo. O `AddKeyEscrowSink` espelho de sobrecargas do método de `IServiceCollection.AddSingleton` e `IServiceCollection.AddInstance` sobrecargas, como `IKeyEscrowSink` instâncias devem ser singletons. Se vários `IKeyEscrowSink` instâncias estiverem registradas, cada um deles será chamado durante a geração de chaves para as chaves podem ser mantidas em garantia para diversos mecanismos simultaneamente.

Há uma API para ler o material de um `IKeyEscrowSink` instância. Isso é consistente com a teoria de design do mecanismo de caução: destinado disponibilizar o material da chave para uma autoridade confiável e como o aplicativo em si não é uma autoridade confiável, ele não deve ter acesso ao seu próprio material caucionada.

O código de exemplo a seguir demonstra a criação e registrando um `IKeyEscrowSink` onde as chaves são mantidas em garantia, de modo que somente os membros do grupo "administradores de CONTOSODomain" poderá recuperá-los.

> [!NOTE]
> Para executar este exemplo, você deve estar em um domínio do Windows 8 / máquina Windows Server 2012 e o controlador de domínio devem ser Windows Server 2012 ou posterior.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
