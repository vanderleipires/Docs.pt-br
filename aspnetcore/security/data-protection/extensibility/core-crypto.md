---
title: "Extensibilidade da criptografia de núcleo"
author: rick-anderson
description: "Explica IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer e a fábrica de nível superior."
ms.author: riande
manager: wpickett
ms.date: 8/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: b82c30fe40c4badc74645dafa9f0d13f6ffae031
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="core-cryptography-extensibility"></a>Extensibilidade da criptografia de núcleo

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> Tipos que implementam qualquer uma das seguintes interfaces devem ser thread-safe para chamadores vários.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

O **IAuthenticatedEncryptor** interface é o bloco de construção básico do subsistema de criptografia. Em geral, há um IAuthenticatedEncryptor por chave e a instância IAuthenticatedEncryptor encapsula todas as material de chave de criptografia e algoritmos informações necessárias para executar operações criptográficas.

Como o nome sugere, o tipo é responsável por fornecer serviços de criptografia e descriptografia autenticados. Expõe as APIs a seguir.

* Descriptografar (ArraySegment<byte> texto cifrado, ArraySegment<byte> additionalAuthenticatedData): byte]

* Criptografar (ArraySegment<byte> texto sem formatação, ArraySegment<byte> additionalAuthenticatedData): byte]

O método Encrypt retorna um blob que inclui o texto não criptografado enciphered e uma marca de autenticação. A marca de autenticação deve abranger os dados adicionais autenticados (AAD), embora o AAD em si não precisa ser recuperável da carga final. O método Decrypt valida a marca de autenticação e retorna a carga deciphered. Todas as falhas (exceto ArgumentNullException e semelhante) devem ser homogenized para CryptographicException.

> [!NOTE]
> A própria instância IAuthenticatedEncryptor, na verdade, não precisa conter o material da chave. Por exemplo, a implementação pudesse ser delegado a um HSM para todas as operações.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>Como criar um IAuthenticatedEncryptor

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O **IAuthenticatedEncryptorFactory** interface representa um tipo que sabe como criar um [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instância. Sua API é o seguinte.

* CreateEncryptorInstance (chave IKey): IAuthenticatedEncryptor

Para qualquer instância específica de IKey os qualquer intermediária autenticada criada por seu método CreateEncryptorInstance deve ser consideradas equivalentes, como o exemplo de código abaixo.

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

O **IAuthenticatedEncryptorDescriptor** interface representa um tipo que sabe como criar um [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instância. Sua API é o seguinte.

* CreateEncryptorInstance() : IAuthenticatedEncryptor

* ExportToXml() : XmlSerializedDescriptorInfo

Como IAuthenticatedEncryptor, supõe-se que uma instância de IAuthenticatedEncryptorDescriptor para encapsular uma chave específica. Isso significa que para qualquer instância específica de IAuthenticatedEncryptorDescriptor qualquer intermediária autenticada criada por seu método CreateEncryptorInstance deve ser considerada equivalentes, como o exemplo de código abaixo.

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor (ASP.NET Core 2. x somente)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O **IAuthenticatedEncryptorDescriptor** interface representa um tipo que sabe como exportar em si para XML. Sua API é o seguinte.

* ExportToXml() : XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>Serialização XML

A principal diferença entre IAuthenticatedEncryptor e IAuthenticatedEncryptorDescriptor é que o descritor sabe como criar o Criptografador e fornecê-lo com os argumentos válidos. Considere um IAuthenticatedEncryptor cuja implementação depende do SymmetricAlgorithm e KeyedHashAlgorithm. Trabalho do Criptografador é consumir esses tipos, mas não necessariamente souber onde esses tipos de origem, para que ele realmente não é possível gravar uma descrição adequada do como recriar a mesmo se o aplicativo for reiniciado. O descritor de atua como um nível superior sobre isso. Desde que o descritor sabe como criar a instância do Criptografador (por exemplo, sabe como criar os algoritmos necessários), ele pode serializar esse conhecimento em formato XML para que a instância do Criptografador pode ser recriada após a redefinição de um aplicativo.

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

O descritor de pode ser serializado por meio de sua rotina de ExportToXml. Esta rotina retorna um XmlSerializedDescriptorInfo que contém duas propriedades: a representação de XElement do descritor e o tipo que representa um [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) que pode ser usado para lembrar Esse descritor fornecido o XElement correspondente.

O descritor de serializado pode conter informações confidenciais, como o material de chave de criptografia. O sistema de proteção de dados tem suporte interno para criptografar informações antes de ele tem persistidos para armazenamento. Para tirar proveito disso, o descritor deve marcar o elemento que contém informações confidenciais com o nome do atributo "requiresEncryption" (xmlns "http://schemas.asp.net/2015/03/dataProtection"), valor "true".

>[!TIP]
> Há um auxiliar de API para a configuração deste atributo. Chame o método de extensão que XElement.markasrequiresencryption() localizado no namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.

Também pode haver casos onde o descritor serializado não contêm informações confidenciais. Considere novamente o caso de uma chave de criptografia armazenada em um HSM. Não é possível gravar o descritor de material de chave ao serializar em si, pois o HSM não irá expor o material em forma de texto sem formatação. Em vez disso, o descritor pode gravar a versão de chave de linha de chave (se o HSM permite exportar dessa forma) ou o identificador exclusivo do HSM para a chave.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

O **IAuthenticatedEncryptorDescriptorDeserializer** interface representa um tipo que sabe como desserializar uma instância IAuthenticatedEncryptorDescriptor de um XElement. Ela expõe um único método:

* ImportFromXml (elemento de XElement): IAuthenticatedEncryptorDescriptor

O método ImportFromXml usa o XElement foi retornado por [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) e cria um equivalente a IAuthenticatedEncryptorDescriptor original.

Tipos que implementam IAuthenticatedEncryptorDescriptorDeserializer devem ter um dos dois construtores públicos a seguir:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> O IServiceProvider transmitido ao construtor pode ser nulo.

## <a name="the-top-level-factory"></a>A fábrica de nível superior

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O **AlgorithmConfiguration** classe representa um tipo que sabe como criar [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instâncias. Ela expõe uma única API.

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

Pense AlgorithmConfiguration como a fábrica de nível superior. A configuração funciona como um modelo. Ela inclui informações de algoritmos (por exemplo, essa configuração produz descritores com uma chave mestra de AES-128-GCM), mas ainda não está associado com uma chave específica.

Quando CreateNewDescriptor é chamado, novo material de chave é criada somente para essa chamada e um novo IAuthenticatedEncryptorDescriptor é produzido que encapsula o material de chave e as informações de algoritmos necessária para consumir o material. O material da chave pode ser criado no software (e mantido na memória), ele pode ser criado e mantido em um HSM e assim por diante. O ponto fundamental é que as duas chamadas para CreateNewDescriptor nunca devem criar instâncias de IAuthenticatedEncryptorDescriptor equivalentes.

O tipo de AlgorithmConfiguration serve como ponto de entrada para rotinas de criação da chave como [chave automática](../implementation/key-management.md#key-expiration-and-rolling). Para alterar a implementação para todas as chaves futuras, defina a propriedade de AuthenticatedEncryptorConfiguration em KeyManagementOptions.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

O **IAuthenticatedEncryptorConfiguration** interface representa um tipo que sabe como criar [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instâncias. Ela expõe uma única API.

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

Pense IAuthenticatedEncryptorConfiguration como a fábrica de nível superior. A configuração funciona como um modelo. Ela inclui informações de algoritmos (por exemplo, essa configuração produz descritores com uma chave mestra de AES-128-GCM), mas ainda não está associado com uma chave específica.

Quando CreateNewDescriptor é chamado, novo material de chave é criada somente para essa chamada e um novo IAuthenticatedEncryptorDescriptor é produzido que encapsula o material de chave e as informações de algoritmos necessária para consumir o material. O material da chave pode ser criado no software (e mantido na memória), ele pode ser criado e mantido em um HSM e assim por diante. O ponto fundamental é que as duas chamadas para CreateNewDescriptor nunca devem criar instâncias de IAuthenticatedEncryptorDescriptor equivalentes.

O tipo de IAuthenticatedEncryptorConfiguration serve como ponto de entrada para rotinas de criação da chave como [chave automática](../implementation/key-management.md#key-expiration-and-rolling). Para alterar a implementação para todas as chaves futuras, registre um singleton IAuthenticatedEncryptorConfiguration no contêiner de serviço.

---
