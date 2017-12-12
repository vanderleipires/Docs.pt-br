---
title: Formato de armazenamento de chaves
author: tdykstra
description: "Este documento explica os detalhes da implementação do formato de armazenamento de chaves de proteção de dados ASP.NET Core."
keywords: "Armazenamento de chaves do ASP.NET Core, proteção de dados"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e8996478-f7bf-4b58-bab4-7fdb5d8556c5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 4ebad05f7d55e954463ce5e277b419a7d6f773c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="key-storage-format"></a><span data-ttu-id="e2bf0-104">Formato de armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="e2bf0-104">Key Storage Format</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="e2bf0-105">Objetos são armazenados em repouso na representação XML.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-105">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="e2bf0-106">O diretório padrão para armazenamento de chaves é % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-106">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="e2bf0-107">O \<chave > elemento</span><span class="sxs-lookup"><span data-stu-id="e2bf0-107">The \<key> element</span></span>

<span data-ttu-id="e2bf0-108">Chaves existirem como objetos de nível superior no repositório de chave.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-108">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="e2bf0-109">Por convenção, as chaves têm o nome do arquivo **chave-{guid}. XML**, onde {guid} é a id da chave.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-109">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="e2bf0-110">Cada arquivo contém uma única chave.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-110">Each such file contains a single key.</span></span> <span data-ttu-id="e2bf0-111">O formato do arquivo é da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-111">The format of the file is as follows.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

<span data-ttu-id="e2bf0-112">O \<chave > elemento contém os seguintes atributos e elementos filho:</span><span class="sxs-lookup"><span data-stu-id="e2bf0-112">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="e2bf0-113">A id de chave. Esse valor é tratado como autoritativo; o nome do arquivo é simplesmente uma sutileza para facilitar a leitura humana.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-113">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="e2bf0-114">A versão do \<chave > elemento fixado no momento em 1.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-114">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="e2bf0-115">Datas de expiração, ativação e criação da chave.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-115">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="e2bf0-116">Um \<descritor > elemento, que contém informações sobre a implementação de criptografia autenticada contida dentro dessa chave.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-116">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="e2bf0-117">No exemplo acima, id da chave é {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, ele foi criado e ativado em 19 de março de 2015, e ele tem um tempo de vida de 90 dias.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-117">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="e2bf0-118">(Ocasionalmente, a data de ativação pode ser um pouco antes da data de criação, como neste exemplo.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-118">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="e2bf0-119">Isso ocorre devido a um nit em como as APIs de trabalho e é inofensivas na prática.)</span><span class="sxs-lookup"><span data-stu-id="e2bf0-119">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="e2bf0-120">O \<descritor > elemento</span><span class="sxs-lookup"><span data-stu-id="e2bf0-120">The \<descriptor> element</span></span>

<span data-ttu-id="e2bf0-121">Externa \<descritor > elemento contém um deserializerType de atributo, que é o nome qualificado do assembly de um tipo que implementa IAuthenticatedEncryptorDescriptorDeserializer.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-121">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="e2bf0-122">Esse tipo é responsável por ler interna \<descritor > elemento e para analisar as informações contidas em.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-122">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="e2bf0-123">O formato específico do \<descritor > elemento depende da implementação do Criptografador autenticado encapsulada pela chave e cada tipo desserializador espera um formato ligeiramente diferente para isso.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-123">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="e2bf0-124">Em geral, no entanto, esse elemento conterá informações de algoritmos (nomes, tipos, OIDs, ou semelhante) e o material de chave secreta.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-124">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="e2bf0-125">No exemplo acima, o descritor especifica que esta chave encapsula a criptografia AES-256-CBC + HMACSHA256 validação.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-125">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="e2bf0-126">O \<encryptedSecret > elemento</span><span class="sxs-lookup"><span data-stu-id="e2bf0-126">The \<encryptedSecret> element</span></span>

<span data-ttu-id="e2bf0-127">Um <encryptedSecret> elemento que contém o formulário criptografado do material de chave secreto pode estar presente se [criptografia de segredos em repouso está ativada](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="e2bf0-127">An <encryptedSecret> element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span></span> <span data-ttu-id="e2bf0-128">O atributo decryptorType será o nome qualificado do assembly de um tipo que implementa IXmlDecryptor.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-128">The attribute decryptorType will be the assembly-qualified name of a type which implements IXmlDecryptor.</span></span> <span data-ttu-id="e2bf0-129">Esse tipo é responsável por ler interna <encryptedKey> elemento e descriptografá-los para recuperar o texto não criptografado original.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-129">This type is responsible for reading the inner <encryptedKey> element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="e2bf0-130">Assim como acontece com \<descritor >, o formato específico do <encryptedSecret> elemento depende do mecanismo de criptografia em repouso em uso.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-130">As with \<descriptor>, the particular format of the <encryptedSecret> element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="e2bf0-131">No exemplo acima, a chave mestra é criptografada usando a DPAPI do Windows por comentário.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-131">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="e2bf0-132">O \<revogação > elemento</span><span class="sxs-lookup"><span data-stu-id="e2bf0-132">The \<revocation> element</span></span>

<span data-ttu-id="e2bf0-133">Revogações existem como objetos de nível superior no repositório de chave.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-133">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="e2bf0-134">Por convenção revogações tem o nome do arquivo **. XML de revogação-{timestamp}** (para revogar todas as chaves antes de uma data específica) ou **revogação-{guid}. XML** (para revogar uma chave específica).</span><span class="sxs-lookup"><span data-stu-id="e2bf0-134">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="e2bf0-135">Cada arquivo contém um único \<revogação > elemento.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-135">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="e2bf0-136">Para revogações de chaves individuais, o conteúdo do arquivo será abaixo.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-136">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="e2bf0-137">Nesse caso, somente a chave especificada é revogada.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-137">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="e2bf0-138">Se a id de chave é "*", no entanto, como no exemplo abaixo, todas as chaves cuja data de criação está antes da data de revogação especificada serão revogadas.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-138">If the key id is "*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="e2bf0-139">O \<motivo > elemento nunca é lidos pelo sistema.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-139">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="e2bf0-140">Ele é simplesmente um local conveniente para armazenar um motivo legível revogação.</span><span class="sxs-lookup"><span data-stu-id="e2bf0-140">It is simply a convenient place to store a human-readable reason for revocation.</span></span>
