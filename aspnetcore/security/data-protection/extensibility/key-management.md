---
title: Extensibilidade de gerenciamento de chaves
author: rick-anderson
description: "Este documento descreve a extensibilidade de gerenciamento de chaves de proteção de dados do ASP.NET Core."
keywords: "Gerenciamento de chaves do ASP.NET Core, proteção de dados"
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 0702e13163c0208e9d2863e711b02ffb257f6260
ms.sourcegitcommit: e641c5794525f983485621860926d8ab4e7360c8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/23/2017
---
# <a name="key-management-extensibility"></a><span data-ttu-id="22b94-104">Extensibilidade de gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="22b94-104">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="22b94-105">Leitura de [gerenciamento de chaves](../implementation/key-management.md#data-protection-implementation-key-management) seção antes de ler esta seção, como explica alguns dos conceitos fundamentais dessas APIs.</span><span class="sxs-lookup"><span data-stu-id="22b94-105">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="22b94-106">Tipos que implementam qualquer uma das seguintes interfaces devem ser thread-safe para chamadores vários.</span><span class="sxs-lookup"><span data-stu-id="22b94-106">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="22b94-107">Chave</span><span class="sxs-lookup"><span data-stu-id="22b94-107">Key</span></span>

<span data-ttu-id="22b94-108">O `IKey` interface é a representação básica de uma chave em criptográfico.</span><span class="sxs-lookup"><span data-stu-id="22b94-108">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="22b94-109">A chave de termo é usada aqui no sentido abstrato, não no sentido de literal de "material de chave criptográfica".</span><span class="sxs-lookup"><span data-stu-id="22b94-109">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="22b94-110">Uma chave tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="22b94-110">A key has the following properties:</span></span>

* <span data-ttu-id="22b94-111">Datas de expiração, a criação e a ativação</span><span class="sxs-lookup"><span data-stu-id="22b94-111">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="22b94-112">Status de revogação</span><span class="sxs-lookup"><span data-stu-id="22b94-112">Revocation status</span></span>

* <span data-ttu-id="22b94-113">Identificador de chave (uma GUID)</span><span class="sxs-lookup"><span data-stu-id="22b94-113">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22b94-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22b94-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="22b94-115">Além disso, `IKey` expõe um `CreateEncryptor` método que pode ser usado para criar um [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instância associada a essa chave.</span><span class="sxs-lookup"><span data-stu-id="22b94-115">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22b94-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22b94-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="22b94-117">Além disso, `IKey` expõe um `CreateEncryptorInstance` método que pode ser usado para criar um [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instância associada a essa chave.</span><span class="sxs-lookup"><span data-stu-id="22b94-117">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="22b94-118">Há uma API para recuperar o material criptográfico bruto de um `IKey` instância.</span><span class="sxs-lookup"><span data-stu-id="22b94-118">There is no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="22b94-119">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="22b94-119">IKeyManager</span></span>

<span data-ttu-id="22b94-120">O `IKeyManager` interface representa um objeto responsável pelo armazenamento de chaves geral, a recuperação e manipulação.</span><span class="sxs-lookup"><span data-stu-id="22b94-120">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="22b94-121">Ela apresenta três operações de alto nível:</span><span class="sxs-lookup"><span data-stu-id="22b94-121">It exposes three high-level operations:</span></span>

* <span data-ttu-id="22b94-122">Crie uma nova chave e mantê-lo para o armazenamento.</span><span class="sxs-lookup"><span data-stu-id="22b94-122">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="22b94-123">Obter todas as chaves de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="22b94-123">Get all keys from storage.</span></span>

* <span data-ttu-id="22b94-124">Revogar uma ou mais chaves e manter as informações de revogação para o armazenamento.</span><span class="sxs-lookup"><span data-stu-id="22b94-124">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="22b94-125">Escrevendo um `IKeyManager` é uma tarefa muito avançada e a maioria dos desenvolvedores não deve tentar a ele.</span><span class="sxs-lookup"><span data-stu-id="22b94-125">Writing an `IKeyManager` is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="22b94-126">Em vez disso, a maioria dos desenvolvedores deve aproveitar os recursos oferecidos pelo [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) classe.</span><span class="sxs-lookup"><span data-stu-id="22b94-126">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="22b94-127">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="22b94-127">XmlKeyManager</span></span>

<span data-ttu-id="22b94-128">O `XmlKeyManager` tipo é a implementação concreta de caixa de entrada da `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="22b94-128">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="22b94-129">Ele fornece vários recursos úteis, incluindo caução de chaves e criptografia de chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="22b94-129">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="22b94-130">As chaves no sistema são representadas como elementos XML (especificamente, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="22b94-130">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="22b94-131">`XmlKeyManager`depende de vários outros componentes no decorrer de atender às suas tarefas:</span><span class="sxs-lookup"><span data-stu-id="22b94-131">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22b94-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22b94-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="22b94-133">`AlgorithmConfiguration`, que determina os algoritmos usados por novas chaves.</span><span class="sxs-lookup"><span data-stu-id="22b94-133">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="22b94-134">`IXmlRepository`, que controla onde as chaves são mantidas no armazenamento.</span><span class="sxs-lookup"><span data-stu-id="22b94-134">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="22b94-135">`IXmlEncryptor`[opcional], que permite a criptografia de chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="22b94-135">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="22b94-136">`IKeyEscrowSink`[opcional], que fornece serviços de caução de chaves.</span><span class="sxs-lookup"><span data-stu-id="22b94-136">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22b94-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22b94-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="22b94-138">`IXmlRepository`, que controla onde as chaves são mantidas no armazenamento.</span><span class="sxs-lookup"><span data-stu-id="22b94-138">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="22b94-139">`IXmlEncryptor`[opcional], que permite a criptografia de chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="22b94-139">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="22b94-140">`IKeyEscrowSink`[opcional], que fornece serviços de caução de chaves.</span><span class="sxs-lookup"><span data-stu-id="22b94-140">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="22b94-141">Abaixo estão os diagramas de alto nível que indicam como esses componentes são conectados em `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="22b94-141">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22b94-142">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22b94-142">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Criação da chave](key-management/_static/keycreation2.png)

   <span data-ttu-id="22b94-144">*Criação de chave / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="22b94-144">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="22b94-145">Na implementação de `CreateNewKey`, o `AlgorithmConfiguration` componente é usado para criar uma única `IAuthenticatedEncryptorDescriptor`, que, em seguida, é serializado como XML.</span><span class="sxs-lookup"><span data-stu-id="22b94-145">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="22b94-146">Se houver um coletor de caução de chaves, o XML bruto de (não criptografado) é fornecido para o coletor para armazenamento de longo prazo.</span><span class="sxs-lookup"><span data-stu-id="22b94-146">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="22b94-147">O XML não criptografado é executado por meio de um `IXmlEncryptor` (se necessário) para gerar o documento XML criptografado.</span><span class="sxs-lookup"><span data-stu-id="22b94-147">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="22b94-148">Este documento criptografado é persistido no armazenamento de longo prazo por meio de `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="22b94-148">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="22b94-149">(Se nenhum `IXmlEncryptor` é configurado, o documento não criptografado é mantido no `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="22b94-149">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22b94-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22b94-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Criação da chave](key-management/_static/keycreation1.png)

   <span data-ttu-id="22b94-152">*Criação de chave / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="22b94-152">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="22b94-153">Na implementação de `CreateNewKey`, o `IAuthenticatedEncryptorConfiguration` componente é usado para criar uma única `IAuthenticatedEncryptorDescriptor`, que, em seguida, é serializado como XML.</span><span class="sxs-lookup"><span data-stu-id="22b94-153">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="22b94-154">Se houver um coletor de caução de chaves, o XML bruto de (não criptografado) é fornecido para o coletor para armazenamento de longo prazo.</span><span class="sxs-lookup"><span data-stu-id="22b94-154">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="22b94-155">O XML não criptografado é executado por meio de um `IXmlEncryptor` (se necessário) para gerar o documento XML criptografado.</span><span class="sxs-lookup"><span data-stu-id="22b94-155">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="22b94-156">Este documento criptografado é persistido no armazenamento de longo prazo por meio de `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="22b94-156">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="22b94-157">(Se nenhum `IXmlEncryptor` é configurado, o documento não criptografado é mantido no `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="22b94-157">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22b94-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22b94-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Recuperação de chave](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22b94-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22b94-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Recuperação de chave](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="22b94-162">*Recuperação de chave / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="22b94-162">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="22b94-163">Na implementação de `GetAllKeys`, representa chaves de documentos XML e revogações são lidas do subjacente `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="22b94-163">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="22b94-164">Se esses documentos são criptografados, o sistema automaticamente descriptografá-los.</span><span class="sxs-lookup"><span data-stu-id="22b94-164">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="22b94-165">`XmlKeyManager`cria apropriada `IAuthenticatedEncryptorDescriptorDeserializer` instâncias para desserializar os documentos de volta para o `IAuthenticatedEncryptorDescriptor` instâncias, que, em seguida, são quebradas em individuais `IKey` instâncias.</span><span class="sxs-lookup"><span data-stu-id="22b94-165">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="22b94-166">Esta coleção de `IKey` instâncias é retornado ao chamador.</span><span class="sxs-lookup"><span data-stu-id="22b94-166">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="22b94-167">Obter mais informações sobre os elementos XML específicos podem ser encontradas na [documento do formato de armazenamento de chaves](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="22b94-167">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="22b94-168">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="22b94-168">IXmlRepository</span></span>

<span data-ttu-id="22b94-169">O `IXmlRepository` interface representa um tipo que pode persistir XML e recuperar o XML de um repositório de backup.</span><span class="sxs-lookup"><span data-stu-id="22b94-169">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="22b94-170">Ela apresenta duas APIs:</span><span class="sxs-lookup"><span data-stu-id="22b94-170">It exposes two APIs:</span></span>

* <span data-ttu-id="22b94-171">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="22b94-171">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="22b94-172">StoreElement (elemento XElement, friendlyName de cadeia de caracteres)</span><span class="sxs-lookup"><span data-stu-id="22b94-172">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="22b94-173">Implementações de `IXmlRepository` não é necessário analisar o XML passando por eles.</span><span class="sxs-lookup"><span data-stu-id="22b94-173">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="22b94-174">Eles devem tratar os documentos XML como opaco e permitir que camadas superiores se preocupar sobre como gerar e analisar os documentos.</span><span class="sxs-lookup"><span data-stu-id="22b94-174">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="22b94-175">Há dois tipos internos concretos que implementam `IXmlRepository`: `FileSystemXmlRepository` e `RegistryXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="22b94-175">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="22b94-176">Consulte o [documento de provedores de armazenamento de chaves](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="22b94-176">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="22b94-177">Registrando um personalizado `IXmlRepository` seria da maneira adequada para usar um armazenamento de backup diferente, por exemplo, o armazenamento de BLOBs do Azure.</span><span class="sxs-lookup"><span data-stu-id="22b94-177">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="22b94-178">Para alterar o repositório padrão do nível de aplicativo, registre um personalizado `IXmlRepository` instância:</span><span class="sxs-lookup"><span data-stu-id="22b94-178">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22b94-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22b94-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22b94-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22b94-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="22b94-181">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="22b94-181">IXmlEncryptor</span></span>

<span data-ttu-id="22b94-182">O `IXmlEncryptor` interface representa um tipo que pode criptografar um elemento XML de texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="22b94-182">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="22b94-183">Ela apresenta uma única API:</span><span class="sxs-lookup"><span data-stu-id="22b94-183">It exposes a single API:</span></span>

* <span data-ttu-id="22b94-184">Criptografar (plaintextElement XElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="22b94-184">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="22b94-185">Se um serializado `IAuthenticatedEncryptorDescriptor` contém todos os elementos marcados como "requer criptografia", em seguida, `XmlKeyManager` executará desses elementos por meio de `IXmlEncryptor`do `Encrypt` método e ele serão mantido o elemento enciphered em vez de elemento de texto sem formatação para o `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="22b94-185">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="22b94-186">A saída de `Encrypt` método é um `EncryptedXmlInfo` objeto.</span><span class="sxs-lookup"><span data-stu-id="22b94-186">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="22b94-187">O objeto é um wrapper que contém ambos os resultantes enciphered `XElement` e o tipo que representa um `IXmlDecryptor` que pode ser usado para o elemento correspondente de decifrar.</span><span class="sxs-lookup"><span data-stu-id="22b94-187">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="22b94-188">Há quatro tipos internos concretos que implementam `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="22b94-188">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="22b94-189">Consulte o [criptografia de chave no documento de rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="22b94-189">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="22b94-190">Para alterar o mecanismo padrão de chave criptografia em repouso todo o aplicativo, registre um personalizado `IXmlEncryptor` instância:</span><span class="sxs-lookup"><span data-stu-id="22b94-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22b94-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22b94-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22b94-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22b94-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="22b94-193">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="22b94-193">IXmlDecryptor</span></span>

<span data-ttu-id="22b94-194">O `IXmlDecryptor` interface representa um tipo que sabe como descriptografar um `XElement` que foi enciphered por meio de um `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="22b94-194">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="22b94-195">Ela apresenta uma única API:</span><span class="sxs-lookup"><span data-stu-id="22b94-195">It exposes a single API:</span></span>

* <span data-ttu-id="22b94-196">Descriptografar (encryptedElement XElement): XElement</span><span class="sxs-lookup"><span data-stu-id="22b94-196">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="22b94-197">O `Decrypt` método desfaz a criptografia executada pelo `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="22b94-197">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="22b94-198">Em geral, cada concreto `IXmlEncryptor` implementação terá um concreto correspondente `IXmlDecryptor` implementação.</span><span class="sxs-lookup"><span data-stu-id="22b94-198">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="22b94-199">Tipos que implementam `IXmlDecryptor` devem ter um dos dois construtores públicos a seguir:</span><span class="sxs-lookup"><span data-stu-id="22b94-199">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="22b94-200">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="22b94-200">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="22b94-201">.ctor()</span><span class="sxs-lookup"><span data-stu-id="22b94-201">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="22b94-202">O `IServiceProvider` passado para o construtor pode ser nulo.</span><span class="sxs-lookup"><span data-stu-id="22b94-202">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="22b94-203">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="22b94-203">IKeyEscrowSink</span></span>

<span data-ttu-id="22b94-204">O `IKeyEscrowSink` interface representa um tipo que pode executar caução de informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="22b94-204">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="22b94-205">Lembre-se de que descritores serializados podem conter informações confidenciais (por exemplo, o material criptográfico), e isso é o que levou à introdução do [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) digite em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="22b94-205">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="22b94-206">No entanto, acidentes acontecem e anéis de chave podem ser excluídos ou corrompidos.</span><span class="sxs-lookup"><span data-stu-id="22b94-206">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="22b94-207">A interface de caução fornece uma trava de escape de emergência, permitindo o acesso ao XML serializado bruto antes que ele é transformado por qualquer configurado [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="22b94-207">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="22b94-208">A interface expõe uma única API:</span><span class="sxs-lookup"><span data-stu-id="22b94-208">The interface exposes a single API:</span></span>

* <span data-ttu-id="22b94-209">Armazenamento (keyId Guid, o elemento de XElement)</span><span class="sxs-lookup"><span data-stu-id="22b94-209">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="22b94-210">Ele é até o `IKeyEscrowSink` implementação para lidar com o elemento fornecido de maneira segura consistente com a política de negócios.</span><span class="sxs-lookup"><span data-stu-id="22b94-210">It is up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="22b94-211">Uma possível implementação pode ser para o coletor de caução criptografar o elemento XML usando um certificado x. 509 corporativo conhecido em que a chave privada do certificado tem foram enviado; o `CertificateXmlEncryptor` tipo pode ajudar com isso.</span><span class="sxs-lookup"><span data-stu-id="22b94-211">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="22b94-212">O `IKeyEscrowSink` implementação também é responsável por manter o elemento fornecido adequadamente.</span><span class="sxs-lookup"><span data-stu-id="22b94-212">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="22b94-213">Por padrão nenhum mecanismo caução estiver habilitado, embora os administradores de servidor podem [configurar isso globalmente](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="22b94-213">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="22b94-214">Também pode ser configurado programaticamente por meio de `IDataProtectionBuilder.AddKeyEscrowSink` método conforme mostrado no exemplo abaixo.</span><span class="sxs-lookup"><span data-stu-id="22b94-214">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="22b94-215">O `AddKeyEscrowSink` espelho de sobrecargas do método de `IServiceCollection.AddSingleton` e `IServiceCollection.AddInstance` sobrecargas, como `IKeyEscrowSink` instâncias devem ser singletons.</span><span class="sxs-lookup"><span data-stu-id="22b94-215">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="22b94-216">Se vários `IKeyEscrowSink` instâncias estiverem registradas, cada um deles será chamado durante a geração de chaves para as chaves podem ser mantidas em garantia para diversos mecanismos simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="22b94-216">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="22b94-217">Há uma API para ler o material de um `IKeyEscrowSink` instância.</span><span class="sxs-lookup"><span data-stu-id="22b94-217">There is no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="22b94-218">Isso é consistente com a teoria de design do mecanismo de caução: destinado disponibilizar o material da chave para uma autoridade confiável e como o aplicativo em si não é uma autoridade confiável, ele não deve ter acesso ao seu próprio material caucionada.</span><span class="sxs-lookup"><span data-stu-id="22b94-218">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="22b94-219">O código de exemplo a seguir demonstra a criação e registrando um `IKeyEscrowSink` onde as chaves são mantidas em garantia, de modo que somente os membros do grupo "administradores de CONTOSODomain" poderá recuperá-los.</span><span class="sxs-lookup"><span data-stu-id="22b94-219">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="22b94-220">Para executar este exemplo, você deve estar em um domínio do Windows 8 / máquina Windows Server 2012 e o controlador de domínio devem ser Windows Server 2012 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="22b94-220">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
