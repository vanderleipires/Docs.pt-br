---
title: Extensibilidade de gerenciamento de chaves
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: ce23931e72404347ebc17c69ae90e70cd15328bc
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="key-management-extensibility"></a>Extensibilidade de gerenciamento de chaves

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> Leitura de [gerenciamento de chaves](../implementation/key-management.md#data-protection-implementation-key-management) seção antes de ler esta seção, como explica alguns dos conceitos fundamentais dessas APIs.

>[!WARNING]
> Tipos que implementam qualquer uma das seguintes interfaces devem ser thread-safe para chamadores vários.

## <a name="key"></a>Chave

A interface IKey é a representação básica de uma chave em criptográfico. A chave de termo é usada aqui no sentido abstrato, não no sentido de literal de "material de chave criptográfica". Uma chave tem as seguintes propriedades:

* Datas de expiração, a criação e a ativação

* Status de revogação

* Identificador de chave (uma GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Além disso, IKey expõe um método CreateEncryptor que pode ser usado para criar um [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instância associada a essa chave.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Além disso, IKey expõe um método CreateEncryptorInstance que pode ser usado para criar um [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instância associada a essa chave.

---

> [!NOTE]
> Há uma API para recuperar o material criptográfico bruto de uma instância de IKey.

## <a name="ikeymanager"></a>IKeyManager

A interface IKeyManager representa um objeto responsável pela manipulação, recuperação e armazenamento de chave gerais. Ela apresenta três operações de alto nível:

* Crie uma nova chave e mantê-lo para o armazenamento.

* Obter todas as chaves de armazenamento.

* Revogar uma ou mais chaves e manter as informações de revogação para o armazenamento.

>[!WARNING]
> Escrevendo um IKeyManager é uma tarefa muito avançada e a maioria dos desenvolvedores não deve tentar a ele. Em vez disso, a maioria dos desenvolvedores deve aproveitar os recursos oferecidos pelo [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) classe.

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

O tipo de XmlKeyManager é a implementação concreta de caixa de entrada de IKeyManager. Ele fornece vários recursos úteis, incluindo caução de chaves e criptografia de chaves em repouso. As chaves no sistema são representadas como elementos XML (especificamente, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview).

XmlKeyManager depende de vários outros componentes no decorrer de atender às suas tarefas:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* AlgorithmConfiguration, que determina os algoritmos usados por novas chaves.

* IXmlRepository, que controla onde as chaves são mantidas no armazenamento.

* IXmlEncryptor [opcional], que permite a criptografia de chaves em repouso.

* IKeyEscrowSink [opcional], que fornece serviços de caução de chaves.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* IXmlRepository, que controla onde as chaves são mantidas no armazenamento.

* IXmlEncryptor [opcional], que permite a criptografia de chaves em repouso.

* IKeyEscrowSink [opcional], que fornece serviços de caução de chaves.

---

Abaixo estão os diagramas de alto nível que indicam como esses componentes são conectados em XmlKeyManager.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Criação da chave](key-management/_static/keycreation2.png)

   *Criação de chave / CreateNewKey*

Na implementação de CreateNewKey, o componente de AlgorithmConfiguration é usado para criar um IAuthenticatedEncryptorDescriptor exclusivo, que é então serializado como XML. Se houver um coletor de caução de chaves, o XML bruto de (não criptografado) é fornecido para o coletor para armazenamento de longo prazo. O XML não criptografado é, em seguida, executar um IXmlEncryptor (se necessário) para gerar o documento XML criptografado. Este documento criptografado é persistido para armazenamento de longo prazo por meio de IXmlRepository. (Se nenhum IXmlEncryptor estiver configurado, o documento não criptografado é mantido no IXmlRepository.)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Criação da chave](key-management/_static/keycreation1.png)

   *Criação de chave / CreateNewKey*

Na implementação de CreateNewKey, o componente de IAuthenticatedEncryptorConfiguration é usado para criar um IAuthenticatedEncryptorDescriptor exclusivo, que é então serializado como XML. Se houver um coletor de caução de chaves, o XML bruto de (não criptografado) é fornecido para o coletor para armazenamento de longo prazo. O XML não criptografado é, em seguida, executar um IXmlEncryptor (se necessário) para gerar o documento XML criptografado. Este documento criptografado é persistido para armazenamento de longo prazo por meio de IXmlRepository. (Se nenhum IXmlEncryptor estiver configurado, o documento não criptografado é mantido no IXmlRepository.)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Recuperação de chave](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Recuperação de chave](key-management/_static/keyretrieval1.png)

---

   *Recuperação de chave / GetAllKeys*

Na implementação de GetAllKeys, os documentos XML que representa as chaves e revogações são lidas do IXmlRepository subjacente. Se esses documentos são criptografados, o sistema automaticamente descriptografá-los. XmlKeyManager cria as instâncias de IAuthenticatedEncryptorDescriptorDeserializer apropriadas para desserializar os documentos de volta para instâncias de IAuthenticatedEncryptorDescriptor, que, em seguida, são quebradas em instâncias individuais do IKey. Esta coleção de instâncias de IKey é retornada ao chamador.

Obter mais informações sobre os elementos XML específicos podem ser encontradas na [documento do formato de armazenamento de chaves](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

A interface IXmlRepository representa um tipo que pode manter o XML e recuperar o XML de um repositório de backup. Ela apresenta duas APIs:

* GetAllElements(): IReadOnlyCollection<XElement>

* StoreElement (elemento XElement, friendlyName de cadeia de caracteres)

Implementações de IXmlRepository não é necessário analisar o XML passando por eles. Eles devem tratar os documentos XML como opaco e permitir que camadas superiores se preocupar sobre como gerar e analisar os documentos.

Há dois tipos internos concretos que implementam IXmlRepository: FileSystemXmlRepository e RegistryXmlRepository. Consulte o [documento de provedores de armazenamento de chaves](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) para obter mais informações. Registrando um IXmlRepository personalizado deve ser da maneira adequada para usar outro repositório de backup, por exemplo, o armazenamento de BLOBs do Azure. Para alterar o nível de aplicativo do repositório padrão, registre um singleton personalizado IXmlRepository no provedor de serviço.

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

A interface IXmlEncryptor representa um tipo que pode criptografar um elemento XML de texto sem formatação. Ela apresenta uma única API:

* Criptografar (plaintextElement XElement): EncryptedXmlInfo

Se um IAuthenticatedEncryptorDescriptor serializado contém todos os elementos marcados como "requer criptografia", em seguida, XmlKeyManager será executado desses elementos por meio do método de criptografia do IXmlEncryptor configurado e persistirá o elemento enciphered em vez disso que o elemento de texto sem formatação para o IXmlRepository. A saída do método de criptografia é um objeto EncryptedXmlInfo. Esse objeto é um wrapper que contém o XElement enciphered resultante e o tipo que representa um IXmlDecryptor que pode ser usado para o elemento correspondente de decifrar.

Há quatro tipos internos concretos que implementam IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor e NullXmlEncryptor. Consulte o [criptografia de chave no documento de rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para obter mais informações. Para alterar o mecanismo padrão de chave criptografia em repouso todo o aplicativo, registre um singleton personalizado IXmlEncryptor no provedor de serviço.

## <a name="ixmldecryptor"></a>IXmlDecryptor

A interface IXmlDecryptor representa um tipo que sabe como descriptografar um XElement que foi enciphered por meio de um IXmlEncryptor. Ela apresenta uma única API:

* Descriptografar (encryptedElement XElement): XElement

O método Decrypt desfaz a criptografia executada pelo IXmlEncryptor.Encrypt. Geralmente, cada implementação concreta de IXmlEncryptor terá uma implementação concreta de IXmlDecryptor correspondente.

Tipos que implementam IXmlDecryptor devem ter um dos dois construtores públicos a seguir:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> O IServiceProvider transmitido ao construtor pode ser nulo.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

A interface IKeyEscrowSink representa um tipo que pode executar caução de informações confidenciais. Lembre-se de que descritores serializados podem conter informações confidenciais (por exemplo, o material criptográfico), e isso é o que levou à introdução do [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) digite em primeiro lugar. No entanto, acidentes acontecem e keyrings podem ser excluídos ou corrompidos.

A interface de caução fornece uma trava de escape de emergência, permitindo o acesso ao XML serializado bruto antes que ele é transformado por qualquer configurado [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor). A interface expõe uma única API:

* Armazenamento (keyId Guid, o elemento de XElement)

Cabe a implementação de IKeyEscrowSink para lidar com o elemento fornecido de maneira segura consistente com a política de negócios. Uma possível implementação pode ser para o coletor de caução criptografar o elemento XML usando um certificado x. 509 corporativo conhecido em que a chave privada do certificado tem foram enviado; o tipo de CertificateXmlEncryptor pode ajudar com isso. A implementação de IKeyEscrowSink também é responsável por manter o elemento fornecido adequadamente.

Por padrão nenhum mecanismo caução estiver habilitado, embora os administradores de servidor podem [configurar isso globalmente](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy). Também pode ser configurado programaticamente por meio de *IDataProtectionBuilder.AddKeyEscrowSink* método conforme mostrado no exemplo abaixo. O *AddKeyEscrowSink* espelho de sobrecargas do método de *IServiceCollection.AddSingleton* e *IServiceCollection.AddInstance* sobrecargas, como IKeyEscrowSink instâncias devem ser singletons. Se várias instâncias de IKeyEscrowSink estiver registradas, cada um deles será chamado durante a geração de chave para que chaves podem ser mantidas em garantia para diversos mecanismos simultaneamente.

Há uma API para ler o material de uma instância de IKeyEscrowSink. Isso é consistente com a teoria de design do mecanismo de caução: destinado disponibilizar o material da chave para uma autoridade confiável e como o aplicativo em si não é uma autoridade confiável, ele não deve ter acesso ao seu próprio material caucionada.

O código de exemplo a seguir demonstra Criando e registrando um IKeyEscrowSink onde as chaves são mantidas em garantia, de modo que somente os membros de "CONTOSODomain Admins" poderá recuperá-los.

> [!NOTE]
> Para executar este exemplo, você deve estar em um domínio do Windows 8 / máquina Windows Server 2012 e o controlador de domínio devem ser Windows Server 2012 ou posterior.

[!code-none[Main](key-management/samples/key-management-extensibility.cs)]
