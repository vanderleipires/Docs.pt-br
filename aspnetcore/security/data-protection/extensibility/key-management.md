---
title: Extensibilidade de gerenciamento de chaves
author: rick-anderson
description: "Este documento descreve a extensibilidade de gerenciamento de chaves de proteção de dados do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: d748ee9d3edf9eed4285fab447d5b379dfcd937c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="key-management-extensibility"></a><span data-ttu-id="81c7b-103">Extensibilidade de gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="81c7b-103">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="81c7b-104">Leitura de [gerenciamento de chaves](../implementation/key-management.md#data-protection-implementation-key-management) seção antes de ler esta seção, como explica alguns dos conceitos fundamentais dessas APIs.</span><span class="sxs-lookup"><span data-stu-id="81c7b-104">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="81c7b-105">Tipos que implementam qualquer uma das seguintes interfaces devem ser thread-safe para chamadores vários.</span><span class="sxs-lookup"><span data-stu-id="81c7b-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="81c7b-106">Chave</span><span class="sxs-lookup"><span data-stu-id="81c7b-106">Key</span></span>

<span data-ttu-id="81c7b-107">O `IKey` interface é a representação básica de uma chave em criptográfico.</span><span class="sxs-lookup"><span data-stu-id="81c7b-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="81c7b-108">A chave de termo é usada aqui no sentido abstrato, não no sentido de literal de "material de chave criptográfica".</span><span class="sxs-lookup"><span data-stu-id="81c7b-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="81c7b-109">Uma chave tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="81c7b-109">A key has the following properties:</span></span>

* <span data-ttu-id="81c7b-110">Datas de expiração, a criação e a ativação</span><span class="sxs-lookup"><span data-stu-id="81c7b-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="81c7b-111">Status de revogação</span><span class="sxs-lookup"><span data-stu-id="81c7b-111">Revocation status</span></span>

* <span data-ttu-id="81c7b-112">Identificador de chave (uma GUID)</span><span class="sxs-lookup"><span data-stu-id="81c7b-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81c7b-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81c7b-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="81c7b-114">Além disso, `IKey` expõe um `CreateEncryptor` método que pode ser usado para criar um [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instância associada a essa chave.</span><span class="sxs-lookup"><span data-stu-id="81c7b-114">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81c7b-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81c7b-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="81c7b-116">Além disso, `IKey` expõe um `CreateEncryptorInstance` método que pode ser usado para criar um [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instância associada a essa chave.</span><span class="sxs-lookup"><span data-stu-id="81c7b-116">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="81c7b-117">Há uma API para recuperar o material criptográfico bruto de um `IKey` instância.</span><span class="sxs-lookup"><span data-stu-id="81c7b-117">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="81c7b-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="81c7b-118">IKeyManager</span></span>

<span data-ttu-id="81c7b-119">O `IKeyManager` interface representa um objeto responsável pelo armazenamento de chaves geral, a recuperação e manipulação.</span><span class="sxs-lookup"><span data-stu-id="81c7b-119">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="81c7b-120">Ela apresenta três operações de alto nível:</span><span class="sxs-lookup"><span data-stu-id="81c7b-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="81c7b-121">Crie uma nova chave e mantê-lo para o armazenamento.</span><span class="sxs-lookup"><span data-stu-id="81c7b-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="81c7b-122">Obter todas as chaves de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="81c7b-122">Get all keys from storage.</span></span>

* <span data-ttu-id="81c7b-123">Revogar uma ou mais chaves e manter as informações de revogação para o armazenamento.</span><span class="sxs-lookup"><span data-stu-id="81c7b-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="81c7b-124">Escrevendo um `IKeyManager` é uma tarefa muito avançada e a maioria dos desenvolvedores não deve tentar.</span><span class="sxs-lookup"><span data-stu-id="81c7b-124">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="81c7b-125">Em vez disso, a maioria dos desenvolvedores deve aproveitar os recursos oferecidos pelo [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) classe.</span><span class="sxs-lookup"><span data-stu-id="81c7b-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="81c7b-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="81c7b-126">XmlKeyManager</span></span>

<span data-ttu-id="81c7b-127">O `XmlKeyManager` tipo é a implementação concreta de caixa de entrada da `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="81c7b-127">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="81c7b-128">Ele fornece vários recursos úteis, incluindo caução de chaves e criptografia de chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="81c7b-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="81c7b-129">As chaves no sistema são representadas como elementos XML (especificamente, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="81c7b-129">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="81c7b-130">`XmlKeyManager`depende de vários outros componentes no decorrer de atender às suas tarefas:</span><span class="sxs-lookup"><span data-stu-id="81c7b-130">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81c7b-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81c7b-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="81c7b-132">`AlgorithmConfiguration`, que determina os algoritmos usados por novas chaves.</span><span class="sxs-lookup"><span data-stu-id="81c7b-132">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="81c7b-133">`IXmlRepository`, que controla onde as chaves são mantidas no armazenamento.</span><span class="sxs-lookup"><span data-stu-id="81c7b-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="81c7b-134">`IXmlEncryptor`[opcional], que permite a criptografia de chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="81c7b-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="81c7b-135">`IKeyEscrowSink`[opcional], que fornece serviços de caução de chaves.</span><span class="sxs-lookup"><span data-stu-id="81c7b-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81c7b-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81c7b-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="81c7b-137">`IXmlRepository`, que controla onde as chaves são mantidas no armazenamento.</span><span class="sxs-lookup"><span data-stu-id="81c7b-137">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="81c7b-138">`IXmlEncryptor`[opcional], que permite a criptografia de chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="81c7b-138">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="81c7b-139">`IKeyEscrowSink`[opcional], que fornece serviços de caução de chaves.</span><span class="sxs-lookup"><span data-stu-id="81c7b-139">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="81c7b-140">Abaixo estão os diagramas de alto nível que indicam como esses componentes são conectados em `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="81c7b-140">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81c7b-141">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81c7b-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Criação da chave](key-management/_static/keycreation2.png)

   <span data-ttu-id="81c7b-143">*Criação de chave / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="81c7b-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="81c7b-144">Na implementação de `CreateNewKey`, o `AlgorithmConfiguration` componente é usado para criar uma única `IAuthenticatedEncryptorDescriptor`, que, em seguida, é serializado como XML.</span><span class="sxs-lookup"><span data-stu-id="81c7b-144">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="81c7b-145">Se houver um coletor de caução de chaves, o XML bruto de (não criptografado) é fornecido para o coletor para armazenamento de longo prazo.</span><span class="sxs-lookup"><span data-stu-id="81c7b-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="81c7b-146">O XML não criptografado é executado por meio de um `IXmlEncryptor` (se necessário) para gerar o documento XML criptografado.</span><span class="sxs-lookup"><span data-stu-id="81c7b-146">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="81c7b-147">Este documento criptografado é persistido no armazenamento de longo prazo por meio de `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="81c7b-147">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="81c7b-148">(Se nenhum `IXmlEncryptor` é configurado, o documento não criptografado é mantido no `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="81c7b-148">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81c7b-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81c7b-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Criação da chave](key-management/_static/keycreation1.png)

   <span data-ttu-id="81c7b-151">*Criação de chave / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="81c7b-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="81c7b-152">Na implementação de `CreateNewKey`, o `IAuthenticatedEncryptorConfiguration` componente é usado para criar uma única `IAuthenticatedEncryptorDescriptor`, que, em seguida, é serializado como XML.</span><span class="sxs-lookup"><span data-stu-id="81c7b-152">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="81c7b-153">Se houver um coletor de caução de chaves, o XML bruto de (não criptografado) é fornecido para o coletor para armazenamento de longo prazo.</span><span class="sxs-lookup"><span data-stu-id="81c7b-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="81c7b-154">O XML não criptografado é executado por meio de um `IXmlEncryptor` (se necessário) para gerar o documento XML criptografado.</span><span class="sxs-lookup"><span data-stu-id="81c7b-154">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="81c7b-155">Este documento criptografado é persistido no armazenamento de longo prazo por meio de `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="81c7b-155">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="81c7b-156">(Se nenhum `IXmlEncryptor` é configurado, o documento não criptografado é mantido no `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="81c7b-156">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81c7b-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81c7b-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Recuperação de chave](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81c7b-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81c7b-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Recuperação de chave](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="81c7b-161">*Recuperação de chave / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="81c7b-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="81c7b-162">Na implementação de `GetAllKeys`, representa chaves de documentos XML e revogações são lidas do subjacente `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="81c7b-162">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="81c7b-163">Se esses documentos são criptografados, o sistema automaticamente descriptografá-los.</span><span class="sxs-lookup"><span data-stu-id="81c7b-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="81c7b-164">`XmlKeyManager`cria apropriada `IAuthenticatedEncryptorDescriptorDeserializer` instâncias para desserializar os documentos de volta para o `IAuthenticatedEncryptorDescriptor` instâncias, que, em seguida, são quebradas em individuais `IKey` instâncias.</span><span class="sxs-lookup"><span data-stu-id="81c7b-164">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="81c7b-165">Esta coleção de `IKey` instâncias é retornado ao chamador.</span><span class="sxs-lookup"><span data-stu-id="81c7b-165">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="81c7b-166">Obter mais informações sobre os elementos XML específicos podem ser encontradas na [documento do formato de armazenamento de chaves](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="81c7b-166">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="81c7b-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="81c7b-167">IXmlRepository</span></span>

<span data-ttu-id="81c7b-168">O `IXmlRepository` interface representa um tipo que pode persistir XML e recuperar o XML de um repositório de backup.</span><span class="sxs-lookup"><span data-stu-id="81c7b-168">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="81c7b-169">Ela apresenta duas APIs:</span><span class="sxs-lookup"><span data-stu-id="81c7b-169">It exposes two APIs:</span></span>

* <span data-ttu-id="81c7b-170">GetAllElements() : IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="81c7b-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="81c7b-171">StoreElement (elemento XElement, friendlyName de cadeia de caracteres)</span><span class="sxs-lookup"><span data-stu-id="81c7b-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="81c7b-172">Implementações de `IXmlRepository` não é necessário analisar o XML passando por eles.</span><span class="sxs-lookup"><span data-stu-id="81c7b-172">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="81c7b-173">Eles devem tratar os documentos XML como opaco e permitir que camadas superiores se preocupar sobre como gerar e analisar os documentos.</span><span class="sxs-lookup"><span data-stu-id="81c7b-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="81c7b-174">Há dois tipos internos concretos que implementam `IXmlRepository`: `FileSystemXmlRepository` e `RegistryXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="81c7b-174">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="81c7b-175">Consulte o [documento de provedores de armazenamento de chaves](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="81c7b-175">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="81c7b-176">Registrando um personalizado `IXmlRepository` seria da maneira adequada para usar um armazenamento de backup diferente, por exemplo, o armazenamento de BLOBs do Azure.</span><span class="sxs-lookup"><span data-stu-id="81c7b-176">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="81c7b-177">Para alterar o repositório padrão do nível de aplicativo, registre um personalizado `IXmlRepository` instância:</span><span class="sxs-lookup"><span data-stu-id="81c7b-177">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81c7b-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81c7b-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81c7b-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81c7b-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="81c7b-180">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="81c7b-180">IXmlEncryptor</span></span>

<span data-ttu-id="81c7b-181">O `IXmlEncryptor` interface representa um tipo que pode criptografar um elemento XML de texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="81c7b-181">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="81c7b-182">Ela apresenta uma única API:</span><span class="sxs-lookup"><span data-stu-id="81c7b-182">It exposes a single API:</span></span>

* <span data-ttu-id="81c7b-183">Criptografar (plaintextElement XElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="81c7b-183">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="81c7b-184">Se um serializado `IAuthenticatedEncryptorDescriptor` contém todos os elementos marcados como "requer criptografia", em seguida, `XmlKeyManager` executará desses elementos por meio de `IXmlEncryptor`do `Encrypt` método e ele serão mantido o elemento enciphered em vez de elemento de texto sem formatação para o `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="81c7b-184">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="81c7b-185">A saída de `Encrypt` método é um `EncryptedXmlInfo` objeto.</span><span class="sxs-lookup"><span data-stu-id="81c7b-185">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="81c7b-186">O objeto é um wrapper que contém ambos os resultantes enciphered `XElement` e o tipo que representa um `IXmlDecryptor` que pode ser usado para o elemento correspondente de decifrar.</span><span class="sxs-lookup"><span data-stu-id="81c7b-186">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="81c7b-187">Há quatro tipos internos concretos que implementam `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="81c7b-187">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="81c7b-188">Consulte o [criptografia de chave no documento de rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="81c7b-188">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="81c7b-189">Para alterar o mecanismo padrão de chave criptografia em repouso todo o aplicativo, registre um personalizado `IXmlEncryptor` instância:</span><span class="sxs-lookup"><span data-stu-id="81c7b-189">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81c7b-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81c7b-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81c7b-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81c7b-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="81c7b-192">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="81c7b-192">IXmlDecryptor</span></span>

<span data-ttu-id="81c7b-193">O `IXmlDecryptor` interface representa um tipo que sabe como descriptografar um `XElement` que foi enciphered por meio de um `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="81c7b-193">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="81c7b-194">Ela apresenta uma única API:</span><span class="sxs-lookup"><span data-stu-id="81c7b-194">It exposes a single API:</span></span>

* <span data-ttu-id="81c7b-195">Descriptografar (encryptedElement XElement): XElement</span><span class="sxs-lookup"><span data-stu-id="81c7b-195">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="81c7b-196">O `Decrypt` método desfaz a criptografia executada pelo `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="81c7b-196">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="81c7b-197">Em geral, cada concreto `IXmlEncryptor` implementação terá um concreto correspondente `IXmlDecryptor` implementação.</span><span class="sxs-lookup"><span data-stu-id="81c7b-197">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="81c7b-198">Tipos que implementam `IXmlDecryptor` devem ter um dos dois construtores públicos a seguir:</span><span class="sxs-lookup"><span data-stu-id="81c7b-198">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="81c7b-199">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="81c7b-199">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="81c7b-200">.ctor()</span><span class="sxs-lookup"><span data-stu-id="81c7b-200">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="81c7b-201">O `IServiceProvider` passado para o construtor pode ser nulo.</span><span class="sxs-lookup"><span data-stu-id="81c7b-201">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="81c7b-202">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="81c7b-202">IKeyEscrowSink</span></span>

<span data-ttu-id="81c7b-203">O `IKeyEscrowSink` interface representa um tipo que pode executar caução de informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="81c7b-203">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="81c7b-204">Lembre-se de que descritores serializados podem conter informações confidenciais (por exemplo, o material criptográfico), e isso é o que levou à introdução do [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) digite em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="81c7b-204">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="81c7b-205">No entanto, acidentes acontecem e anéis de chave podem ser excluídos ou corrompidos.</span><span class="sxs-lookup"><span data-stu-id="81c7b-205">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="81c7b-206">A interface de caução fornece uma trava de escape de emergência, permitindo o acesso ao XML serializado bruto antes que ele é transformado por qualquer configurado [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="81c7b-206">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="81c7b-207">A interface expõe uma única API:</span><span class="sxs-lookup"><span data-stu-id="81c7b-207">The interface exposes a single API:</span></span>

* <span data-ttu-id="81c7b-208">Armazenamento (keyId Guid, o elemento de XElement)</span><span class="sxs-lookup"><span data-stu-id="81c7b-208">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="81c7b-209">Ele é até o `IKeyEscrowSink` implementação para lidar com o elemento fornecido de maneira segura consistente com a política de negócios.</span><span class="sxs-lookup"><span data-stu-id="81c7b-209">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="81c7b-210">Uma possível implementação pode ser para o coletor de caução criptografar o elemento XML usando um certificado x. 509 corporativo conhecido em que a chave privada do certificado tem foram enviado; o `CertificateXmlEncryptor` tipo pode ajudar com isso.</span><span class="sxs-lookup"><span data-stu-id="81c7b-210">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="81c7b-211">O `IKeyEscrowSink` implementação também é responsável por manter o elemento fornecido adequadamente.</span><span class="sxs-lookup"><span data-stu-id="81c7b-211">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="81c7b-212">Por padrão nenhum mecanismo caução estiver habilitado, embora os administradores de servidor podem [configurar isso globalmente](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="81c7b-212">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="81c7b-213">Também pode ser configurado programaticamente por meio de `IDataProtectionBuilder.AddKeyEscrowSink` método conforme mostrado no exemplo abaixo.</span><span class="sxs-lookup"><span data-stu-id="81c7b-213">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="81c7b-214">O `AddKeyEscrowSink` espelho de sobrecargas do método de `IServiceCollection.AddSingleton` e `IServiceCollection.AddInstance` sobrecargas, como `IKeyEscrowSink` instâncias devem ser singletons.</span><span class="sxs-lookup"><span data-stu-id="81c7b-214">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="81c7b-215">Se vários `IKeyEscrowSink` instâncias estiverem registradas, cada um deles será chamado durante a geração de chaves para as chaves podem ser mantidas em garantia para diversos mecanismos simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="81c7b-215">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="81c7b-216">Há uma API para ler o material de um `IKeyEscrowSink` instância.</span><span class="sxs-lookup"><span data-stu-id="81c7b-216">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="81c7b-217">Isso é consistente com a teoria de design do mecanismo de caução: destinado disponibilizar o material da chave para uma autoridade confiável e como o aplicativo em si não é uma autoridade confiável, ele não deve ter acesso ao seu próprio material caucionada.</span><span class="sxs-lookup"><span data-stu-id="81c7b-217">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="81c7b-218">O código de exemplo a seguir demonstra a criação e registrando um `IKeyEscrowSink` onde as chaves são mantidas em garantia, de modo que somente os membros do grupo "administradores de CONTOSODomain" poderá recuperá-los.</span><span class="sxs-lookup"><span data-stu-id="81c7b-218">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="81c7b-219">Para executar este exemplo, você deve estar em um domínio do Windows 8 / máquina Windows Server 2012 e o controlador de domínio devem ser Windows Server 2012 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="81c7b-219">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
