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
# <a name="key-management-extensibility"></a><span data-ttu-id="170e5-103">Extensibilidade de gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="170e5-103">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="170e5-104">Leitura de [gerenciamento de chaves](../implementation/key-management.md#data-protection-implementation-key-management) seção antes de ler esta seção, como explica alguns dos conceitos fundamentais dessas APIs.</span><span class="sxs-lookup"><span data-stu-id="170e5-104">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="170e5-105">Tipos que implementam qualquer uma das seguintes interfaces devem ser thread-safe para chamadores vários.</span><span class="sxs-lookup"><span data-stu-id="170e5-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="170e5-106">Chave</span><span class="sxs-lookup"><span data-stu-id="170e5-106">Key</span></span>

<span data-ttu-id="170e5-107">A interface IKey é a representação básica de uma chave em criptográfico.</span><span class="sxs-lookup"><span data-stu-id="170e5-107">The IKey interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="170e5-108">A chave de termo é usada aqui no sentido abstrato, não no sentido de literal de "material de chave criptográfica".</span><span class="sxs-lookup"><span data-stu-id="170e5-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="170e5-109">Uma chave tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="170e5-109">A key has the following properties:</span></span>

* <span data-ttu-id="170e5-110">Datas de expiração, a criação e a ativação</span><span class="sxs-lookup"><span data-stu-id="170e5-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="170e5-111">Status de revogação</span><span class="sxs-lookup"><span data-stu-id="170e5-111">Revocation status</span></span>

* <span data-ttu-id="170e5-112">Identificador de chave (uma GUID)</span><span class="sxs-lookup"><span data-stu-id="170e5-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="170e5-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="170e5-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="170e5-114">Além disso, IKey expõe um método CreateEncryptor que pode ser usado para criar um [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instância associada a essa chave.</span><span class="sxs-lookup"><span data-stu-id="170e5-114">Additionally, IKey exposes a CreateEncryptor method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="170e5-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="170e5-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="170e5-116">Além disso, IKey expõe um método CreateEncryptorInstance que pode ser usado para criar um [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instância associada a essa chave.</span><span class="sxs-lookup"><span data-stu-id="170e5-116">Additionally, IKey exposes a CreateEncryptorInstance method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="170e5-117">Há uma API para recuperar o material criptográfico bruto de uma instância de IKey.</span><span class="sxs-lookup"><span data-stu-id="170e5-117">There is no API to retrieve the raw cryptographic material from an IKey instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="170e5-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="170e5-118">IKeyManager</span></span>

<span data-ttu-id="170e5-119">A interface IKeyManager representa um objeto responsável pela manipulação, recuperação e armazenamento de chave gerais.</span><span class="sxs-lookup"><span data-stu-id="170e5-119">The IKeyManager interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="170e5-120">Ela apresenta três operações de alto nível:</span><span class="sxs-lookup"><span data-stu-id="170e5-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="170e5-121">Crie uma nova chave e mantê-lo para o armazenamento.</span><span class="sxs-lookup"><span data-stu-id="170e5-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="170e5-122">Obter todas as chaves de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="170e5-122">Get all keys from storage.</span></span>

* <span data-ttu-id="170e5-123">Revogar uma ou mais chaves e manter as informações de revogação para o armazenamento.</span><span class="sxs-lookup"><span data-stu-id="170e5-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="170e5-124">Escrevendo um IKeyManager é uma tarefa muito avançada e a maioria dos desenvolvedores não deve tentar a ele.</span><span class="sxs-lookup"><span data-stu-id="170e5-124">Writing an IKeyManager is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="170e5-125">Em vez disso, a maioria dos desenvolvedores deve aproveitar os recursos oferecidos pelo [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) classe.</span><span class="sxs-lookup"><span data-stu-id="170e5-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="170e5-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="170e5-126">XmlKeyManager</span></span>

<span data-ttu-id="170e5-127">O tipo de XmlKeyManager é a implementação concreta de caixa de entrada de IKeyManager.</span><span class="sxs-lookup"><span data-stu-id="170e5-127">The XmlKeyManager type is the in-box concrete implementation of IKeyManager.</span></span> <span data-ttu-id="170e5-128">Ele fornece vários recursos úteis, incluindo caução de chaves e criptografia de chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="170e5-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="170e5-129">As chaves no sistema são representadas como elementos XML (especificamente, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview).</span><span class="sxs-lookup"><span data-stu-id="170e5-129">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview).</span></span>

<span data-ttu-id="170e5-130">XmlKeyManager depende de vários outros componentes no decorrer de atender às suas tarefas:</span><span class="sxs-lookup"><span data-stu-id="170e5-130">XmlKeyManager depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="170e5-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="170e5-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="170e5-132">AlgorithmConfiguration, que determina os algoritmos usados por novas chaves.</span><span class="sxs-lookup"><span data-stu-id="170e5-132">AlgorithmConfiguration, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="170e5-133">IXmlRepository, que controla onde as chaves são mantidas no armazenamento.</span><span class="sxs-lookup"><span data-stu-id="170e5-133">IXmlRepository, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="170e5-134">IXmlEncryptor [opcional], que permite a criptografia de chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="170e5-134">IXmlEncryptor [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="170e5-135">IKeyEscrowSink [opcional], que fornece serviços de caução de chaves.</span><span class="sxs-lookup"><span data-stu-id="170e5-135">IKeyEscrowSink [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="170e5-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="170e5-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="170e5-137">IXmlRepository, que controla onde as chaves são mantidas no armazenamento.</span><span class="sxs-lookup"><span data-stu-id="170e5-137">IXmlRepository, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="170e5-138">IXmlEncryptor [opcional], que permite a criptografia de chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="170e5-138">IXmlEncryptor [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="170e5-139">IKeyEscrowSink [opcional], que fornece serviços de caução de chaves.</span><span class="sxs-lookup"><span data-stu-id="170e5-139">IKeyEscrowSink [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="170e5-140">Abaixo estão os diagramas de alto nível que indicam como esses componentes são conectados em XmlKeyManager.</span><span class="sxs-lookup"><span data-stu-id="170e5-140">Below are high-level diagrams which indicate how these components are wired together within XmlKeyManager.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="170e5-141">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="170e5-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Criação da chave](key-management/_static/keycreation2.png)

   <span data-ttu-id="170e5-143">*Criação de chave / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="170e5-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="170e5-144">Na implementação de CreateNewKey, o componente de AlgorithmConfiguration é usado para criar um IAuthenticatedEncryptorDescriptor exclusivo, que é então serializado como XML.</span><span class="sxs-lookup"><span data-stu-id="170e5-144">In the implementation of CreateNewKey, the AlgorithmConfiguration component is used to create a unique IAuthenticatedEncryptorDescriptor, which is then serialized as XML.</span></span> <span data-ttu-id="170e5-145">Se houver um coletor de caução de chaves, o XML bruto de (não criptografado) é fornecido para o coletor para armazenamento de longo prazo.</span><span class="sxs-lookup"><span data-stu-id="170e5-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="170e5-146">O XML não criptografado é, em seguida, executar um IXmlEncryptor (se necessário) para gerar o documento XML criptografado.</span><span class="sxs-lookup"><span data-stu-id="170e5-146">The unencrypted XML is then run through an IXmlEncryptor (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="170e5-147">Este documento criptografado é persistido para armazenamento de longo prazo por meio de IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="170e5-147">This encrypted document is persisted to long-term storage via the IXmlRepository.</span></span> <span data-ttu-id="170e5-148">(Se nenhum IXmlEncryptor estiver configurado, o documento não criptografado é mantido no IXmlRepository.)</span><span class="sxs-lookup"><span data-stu-id="170e5-148">(If no IXmlEncryptor is configured, the unencrypted document is persisted in the IXmlRepository.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="170e5-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="170e5-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Criação da chave](key-management/_static/keycreation1.png)

   <span data-ttu-id="170e5-151">*Criação de chave / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="170e5-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="170e5-152">Na implementação de CreateNewKey, o componente de IAuthenticatedEncryptorConfiguration é usado para criar um IAuthenticatedEncryptorDescriptor exclusivo, que é então serializado como XML.</span><span class="sxs-lookup"><span data-stu-id="170e5-152">In the implementation of CreateNewKey, the IAuthenticatedEncryptorConfiguration component is used to create a unique IAuthenticatedEncryptorDescriptor, which is then serialized as XML.</span></span> <span data-ttu-id="170e5-153">Se houver um coletor de caução de chaves, o XML bruto de (não criptografado) é fornecido para o coletor para armazenamento de longo prazo.</span><span class="sxs-lookup"><span data-stu-id="170e5-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="170e5-154">O XML não criptografado é, em seguida, executar um IXmlEncryptor (se necessário) para gerar o documento XML criptografado.</span><span class="sxs-lookup"><span data-stu-id="170e5-154">The unencrypted XML is then run through an IXmlEncryptor (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="170e5-155">Este documento criptografado é persistido para armazenamento de longo prazo por meio de IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="170e5-155">This encrypted document is persisted to long-term storage via the IXmlRepository.</span></span> <span data-ttu-id="170e5-156">(Se nenhum IXmlEncryptor estiver configurado, o documento não criptografado é mantido no IXmlRepository.)</span><span class="sxs-lookup"><span data-stu-id="170e5-156">(If no IXmlEncryptor is configured, the unencrypted document is persisted in the IXmlRepository.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="170e5-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="170e5-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Recuperação de chave](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="170e5-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="170e5-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Recuperação de chave](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="170e5-161">*Recuperação de chave / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="170e5-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="170e5-162">Na implementação de GetAllKeys, os documentos XML que representa as chaves e revogações são lidas do IXmlRepository subjacente.</span><span class="sxs-lookup"><span data-stu-id="170e5-162">In the implementation of GetAllKeys, the XML documents representing keys and revocations are read from the underlying IXmlRepository.</span></span> <span data-ttu-id="170e5-163">Se esses documentos são criptografados, o sistema automaticamente descriptografá-los.</span><span class="sxs-lookup"><span data-stu-id="170e5-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="170e5-164">XmlKeyManager cria as instâncias de IAuthenticatedEncryptorDescriptorDeserializer apropriadas para desserializar os documentos de volta para instâncias de IAuthenticatedEncryptorDescriptor, que, em seguida, são quebradas em instâncias individuais do IKey.</span><span class="sxs-lookup"><span data-stu-id="170e5-164">XmlKeyManager creates the appropriate IAuthenticatedEncryptorDescriptorDeserializer instances to deserialize the documents back into IAuthenticatedEncryptorDescriptor instances, which are then wrapped in individual IKey instances.</span></span> <span data-ttu-id="170e5-165">Esta coleção de instâncias de IKey é retornada ao chamador.</span><span class="sxs-lookup"><span data-stu-id="170e5-165">This collection of IKey instances is returned to the caller.</span></span>

<span data-ttu-id="170e5-166">Obter mais informações sobre os elementos XML específicos podem ser encontradas na [documento do formato de armazenamento de chaves](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="170e5-166">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="170e5-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="170e5-167">IXmlRepository</span></span>

<span data-ttu-id="170e5-168">A interface IXmlRepository representa um tipo que pode manter o XML e recuperar o XML de um repositório de backup.</span><span class="sxs-lookup"><span data-stu-id="170e5-168">The IXmlRepository interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="170e5-169">Ela apresenta duas APIs:</span><span class="sxs-lookup"><span data-stu-id="170e5-169">It exposes two APIs:</span></span>

* <span data-ttu-id="170e5-170">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="170e5-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="170e5-171">StoreElement (elemento XElement, friendlyName de cadeia de caracteres)</span><span class="sxs-lookup"><span data-stu-id="170e5-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="170e5-172">Implementações de IXmlRepository não é necessário analisar o XML passando por eles.</span><span class="sxs-lookup"><span data-stu-id="170e5-172">Implementations of IXmlRepository don't need to parse the XML passing through them.</span></span> <span data-ttu-id="170e5-173">Eles devem tratar os documentos XML como opaco e permitir que camadas superiores se preocupar sobre como gerar e analisar os documentos.</span><span class="sxs-lookup"><span data-stu-id="170e5-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="170e5-174">Há dois tipos internos concretos que implementam IXmlRepository: FileSystemXmlRepository e RegistryXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="170e5-174">There are two built-in concrete types which implement IXmlRepository: FileSystemXmlRepository and RegistryXmlRepository.</span></span> <span data-ttu-id="170e5-175">Consulte o [documento de provedores de armazenamento de chaves](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="170e5-175">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="170e5-176">Registrando um IXmlRepository personalizado deve ser da maneira adequada para usar outro repositório de backup, por exemplo, o armazenamento de BLOBs do Azure.</span><span class="sxs-lookup"><span data-stu-id="170e5-176">Registering a custom IXmlRepository would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span> <span data-ttu-id="170e5-177">Para alterar o nível de aplicativo do repositório padrão, registre um singleton personalizado IXmlRepository no provedor de serviço.</span><span class="sxs-lookup"><span data-stu-id="170e5-177">To change the default repository application-wide, register a custom singleton IXmlRepository in the service provider.</span></span>

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="170e5-178">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="170e5-178">IXmlEncryptor</span></span>

<span data-ttu-id="170e5-179">A interface IXmlEncryptor representa um tipo que pode criptografar um elemento XML de texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="170e5-179">The IXmlEncryptor interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="170e5-180">Ela apresenta uma única API:</span><span class="sxs-lookup"><span data-stu-id="170e5-180">It exposes a single API:</span></span>

* <span data-ttu-id="170e5-181">Criptografar (plaintextElement XElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="170e5-181">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="170e5-182">Se um IAuthenticatedEncryptorDescriptor serializado contém todos os elementos marcados como "requer criptografia", em seguida, XmlKeyManager será executado desses elementos por meio do método de criptografia do IXmlEncryptor configurado e persistirá o elemento enciphered em vez disso que o elemento de texto sem formatação para o IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="170e5-182">If a serialized IAuthenticatedEncryptorDescriptor contains any elements marked as "requires encryption", then XmlKeyManager will run those elements through the configured IXmlEncryptor's Encrypt method, and it will persist the enciphered element rather than the plaintext element to the IXmlRepository.</span></span> <span data-ttu-id="170e5-183">A saída do método de criptografia é um objeto EncryptedXmlInfo.</span><span class="sxs-lookup"><span data-stu-id="170e5-183">The output of the Encrypt method is an EncryptedXmlInfo object.</span></span> <span data-ttu-id="170e5-184">Esse objeto é um wrapper que contém o XElement enciphered resultante e o tipo que representa um IXmlDecryptor que pode ser usado para o elemento correspondente de decifrar.</span><span class="sxs-lookup"><span data-stu-id="170e5-184">This object is a wrapper which contains both the resultant enciphered XElement and the Type which represents an IXmlDecryptor which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="170e5-185">Há quatro tipos internos concretos que implementam IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor e NullXmlEncryptor.</span><span class="sxs-lookup"><span data-stu-id="170e5-185">There are four built-in concrete types which implement IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor, and NullXmlEncryptor.</span></span> <span data-ttu-id="170e5-186">Consulte o [criptografia de chave no documento de rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="170e5-186">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span> <span data-ttu-id="170e5-187">Para alterar o mecanismo padrão de chave criptografia em repouso todo o aplicativo, registre um singleton personalizado IXmlEncryptor no provedor de serviço.</span><span class="sxs-lookup"><span data-stu-id="170e5-187">To change the default key-encryption-at-rest mechanism application-wide, register a custom singleton IXmlEncryptor in the service provider.</span></span>

## <a name="ixmldecryptor"></a><span data-ttu-id="170e5-188">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="170e5-188">IXmlDecryptor</span></span>

<span data-ttu-id="170e5-189">A interface IXmlDecryptor representa um tipo que sabe como descriptografar um XElement que foi enciphered por meio de um IXmlEncryptor.</span><span class="sxs-lookup"><span data-stu-id="170e5-189">The IXmlDecryptor interface represents a type that knows how to decrypt an XElement that was enciphered via an IXmlEncryptor.</span></span> <span data-ttu-id="170e5-190">Ela apresenta uma única API:</span><span class="sxs-lookup"><span data-stu-id="170e5-190">It exposes a single API:</span></span>

* <span data-ttu-id="170e5-191">Descriptografar (encryptedElement XElement): XElement</span><span class="sxs-lookup"><span data-stu-id="170e5-191">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="170e5-192">O método Decrypt desfaz a criptografia executada pelo IXmlEncryptor.Encrypt.</span><span class="sxs-lookup"><span data-stu-id="170e5-192">The Decrypt method undoes the encryption performed by IXmlEncryptor.Encrypt.</span></span> <span data-ttu-id="170e5-193">Geralmente, cada implementação concreta de IXmlEncryptor terá uma implementação concreta de IXmlDecryptor correspondente.</span><span class="sxs-lookup"><span data-stu-id="170e5-193">Generally each concrete IXmlEncryptor implementation will have a corresponding concrete IXmlDecryptor implementation.</span></span>

<span data-ttu-id="170e5-194">Tipos que implementam IXmlDecryptor devem ter um dos dois construtores públicos a seguir:</span><span class="sxs-lookup"><span data-stu-id="170e5-194">Types which implement IXmlDecryptor should have one of the following two public constructors:</span></span>

* <span data-ttu-id="170e5-195">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="170e5-195">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="170e5-196">.ctor()</span><span class="sxs-lookup"><span data-stu-id="170e5-196">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="170e5-197">O IServiceProvider transmitido ao construtor pode ser nulo.</span><span class="sxs-lookup"><span data-stu-id="170e5-197">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="170e5-198">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="170e5-198">IKeyEscrowSink</span></span>

<span data-ttu-id="170e5-199">A interface IKeyEscrowSink representa um tipo que pode executar caução de informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="170e5-199">The IKeyEscrowSink interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="170e5-200">Lembre-se de que descritores serializados podem conter informações confidenciais (por exemplo, o material criptográfico), e isso é o que levou à introdução do [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) digite em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="170e5-200">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="170e5-201">No entanto, acidentes acontecem e keyrings podem ser excluídos ou corrompidos.</span><span class="sxs-lookup"><span data-stu-id="170e5-201">However, accidents happen, and keyrings can be deleted or become corrupted.</span></span>

<span data-ttu-id="170e5-202">A interface de caução fornece uma trava de escape de emergência, permitindo o acesso ao XML serializado bruto antes que ele é transformado por qualquer configurado [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="170e5-202">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="170e5-203">A interface expõe uma única API:</span><span class="sxs-lookup"><span data-stu-id="170e5-203">The interface exposes a single API:</span></span>

* <span data-ttu-id="170e5-204">Armazenamento (keyId Guid, o elemento de XElement)</span><span class="sxs-lookup"><span data-stu-id="170e5-204">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="170e5-205">Cabe a implementação de IKeyEscrowSink para lidar com o elemento fornecido de maneira segura consistente com a política de negócios.</span><span class="sxs-lookup"><span data-stu-id="170e5-205">It is up to the IKeyEscrowSink implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="170e5-206">Uma possível implementação pode ser para o coletor de caução criptografar o elemento XML usando um certificado x. 509 corporativo conhecido em que a chave privada do certificado tem foram enviado; o tipo de CertificateXmlEncryptor pode ajudar com isso.</span><span class="sxs-lookup"><span data-stu-id="170e5-206">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the CertificateXmlEncryptor type can assist with this.</span></span> <span data-ttu-id="170e5-207">A implementação de IKeyEscrowSink também é responsável por manter o elemento fornecido adequadamente.</span><span class="sxs-lookup"><span data-stu-id="170e5-207">The IKeyEscrowSink implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="170e5-208">Por padrão nenhum mecanismo caução estiver habilitado, embora os administradores de servidor podem [configurar isso globalmente](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy).</span><span class="sxs-lookup"><span data-stu-id="170e5-208">By default no escrow mechanism is enabled, though server administrators can [configure this globally](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy).</span></span> <span data-ttu-id="170e5-209">Também pode ser configurado programaticamente por meio de *IDataProtectionBuilder.AddKeyEscrowSink* método conforme mostrado no exemplo abaixo.</span><span class="sxs-lookup"><span data-stu-id="170e5-209">It can also be configured programmatically via the *IDataProtectionBuilder.AddKeyEscrowSink* method as shown in the sample below.</span></span> <span data-ttu-id="170e5-210">O *AddKeyEscrowSink* espelho de sobrecargas do método de *IServiceCollection.AddSingleton* e *IServiceCollection.AddInstance* sobrecargas, como IKeyEscrowSink instâncias devem ser singletons.</span><span class="sxs-lookup"><span data-stu-id="170e5-210">The *AddKeyEscrowSink* method overloads mirror the *IServiceCollection.AddSingleton* and *IServiceCollection.AddInstance* overloads, as IKeyEscrowSink instances are intended to be singletons.</span></span> <span data-ttu-id="170e5-211">Se várias instâncias de IKeyEscrowSink estiver registradas, cada um deles será chamado durante a geração de chave para que chaves podem ser mantidas em garantia para diversos mecanismos simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="170e5-211">If multiple IKeyEscrowSink instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="170e5-212">Há uma API para ler o material de uma instância de IKeyEscrowSink.</span><span class="sxs-lookup"><span data-stu-id="170e5-212">There is no API to read material from an IKeyEscrowSink instance.</span></span> <span data-ttu-id="170e5-213">Isso é consistente com a teoria de design do mecanismo de caução: destinado disponibilizar o material da chave para uma autoridade confiável e como o aplicativo em si não é uma autoridade confiável, ele não deve ter acesso ao seu próprio material caucionada.</span><span class="sxs-lookup"><span data-stu-id="170e5-213">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="170e5-214">O código de exemplo a seguir demonstra Criando e registrando um IKeyEscrowSink onde as chaves são mantidas em garantia, de modo que somente os membros de "CONTOSODomain Admins" poderá recuperá-los.</span><span class="sxs-lookup"><span data-stu-id="170e5-214">The following sample code demonstrates creating and registering an IKeyEscrowSink where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="170e5-215">Para executar este exemplo, você deve estar em um domínio do Windows 8 / máquina Windows Server 2012 e o controlador de domínio devem ser Windows Server 2012 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="170e5-215">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-none[Main](key-management/samples/key-management-extensibility.cs)]
