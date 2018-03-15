---
title: "Proteção de Dados no ASP.NET Core"
author: rick-anderson
description: "Este documento serve como um sumário para os diversos tópicos sobre proteção de dados do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: e08dea63f012c4a758f2e5561c4930d09cfee0ac
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="aab9a-103">Proteção de Dados no ASP.NET Core: APIs de consumidor, configuração, APIs de extensibilidade e implementação</span><span class="sxs-lookup"><span data-stu-id="aab9a-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="aab9a-104">Introdução à proteção de dados</span><span class="sxs-lookup"><span data-stu-id="aab9a-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="aab9a-105">Introdução às APIs de proteção de dados</span><span class="sxs-lookup"><span data-stu-id="aab9a-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="aab9a-106">APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="aab9a-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="aab9a-107">Visão geral das APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="aab9a-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="aab9a-108">Cadeias de caracteres de finalidade</span><span class="sxs-lookup"><span data-stu-id="aab9a-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="aab9a-109">Multilocação e hierarquia de finalidade</span><span class="sxs-lookup"><span data-stu-id="aab9a-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="aab9a-110">Hash de senha</span><span class="sxs-lookup"><span data-stu-id="aab9a-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="aab9a-111">Limitando o tempo de vida de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="aab9a-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="aab9a-112">Desprotegendo cargas cujas chaves foram revogadas</span><span class="sxs-lookup"><span data-stu-id="aab9a-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="aab9a-113">Configuração</span><span class="sxs-lookup"><span data-stu-id="aab9a-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="aab9a-114">Configurar a proteção de dados</span><span class="sxs-lookup"><span data-stu-id="aab9a-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="aab9a-115">Configurações padrão</span><span class="sxs-lookup"><span data-stu-id="aab9a-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="aab9a-116">Política ampla de computador</span><span class="sxs-lookup"><span data-stu-id="aab9a-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="aab9a-117">Cenários sem reconhecimento de DI</span><span class="sxs-lookup"><span data-stu-id="aab9a-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="aab9a-118">APIs de extensibilidade</span><span class="sxs-lookup"><span data-stu-id="aab9a-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="aab9a-119">Extensibilidade da criptografia básica</span><span class="sxs-lookup"><span data-stu-id="aab9a-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="aab9a-120">Extensibilidade de gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="aab9a-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="aab9a-121">APIs diversas</span><span class="sxs-lookup"><span data-stu-id="aab9a-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="aab9a-122">Implementação</span><span class="sxs-lookup"><span data-stu-id="aab9a-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="aab9a-123">Detalhes de criptografia autenticada</span><span class="sxs-lookup"><span data-stu-id="aab9a-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="aab9a-124">Derivação de subchaves e criptografia autenticada</span><span class="sxs-lookup"><span data-stu-id="aab9a-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="aab9a-125">Cabeçalhos de contexto</span><span class="sxs-lookup"><span data-stu-id="aab9a-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="aab9a-126">Gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="aab9a-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="aab9a-127">Provedores de armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="aab9a-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="aab9a-128">Criptografia de chave em repouso</span><span class="sxs-lookup"><span data-stu-id="aab9a-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="aab9a-129">Imutabilidade de chave e alteração de configurações</span><span class="sxs-lookup"><span data-stu-id="aab9a-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="aab9a-130">Formato do armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="aab9a-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="aab9a-131">Provedores de proteção de dados efêmeros</span><span class="sxs-lookup"><span data-stu-id="aab9a-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="aab9a-132">Compatibilidade</span><span class="sxs-lookup"><span data-stu-id="aab9a-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="aab9a-133">Substituindo <machineKey> no ASP.NET</span><span class="sxs-lookup"><span data-stu-id="aab9a-133">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
