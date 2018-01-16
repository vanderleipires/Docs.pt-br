---
title: "Proteção de Dados no ASP.NET Core"
author: rick-anderson
description: "Este documento serve como um sumário para os diversos tópicos sobre proteção de dados do ASP.NET Core."
keywords: "ASP.NET Core, proteção de dados"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="ffc42-104">Proteção de Dados no ASP.NET Core: APIs de consumidor, configuração, APIs de extensibilidade e implementação</span><span class="sxs-lookup"><span data-stu-id="ffc42-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="ffc42-105">Introdução à proteção de dados</span><span class="sxs-lookup"><span data-stu-id="ffc42-105">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="ffc42-106">Introdução às APIs de proteção de dados</span><span class="sxs-lookup"><span data-stu-id="ffc42-106">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="ffc42-107">APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="ffc42-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="ffc42-108">Visão geral das APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="ffc42-108">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="ffc42-109">Cadeias de caracteres de finalidade</span><span class="sxs-lookup"><span data-stu-id="ffc42-109">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="ffc42-110">Multilocação e hierarquia de finalidade</span><span class="sxs-lookup"><span data-stu-id="ffc42-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="ffc42-111">Hash de senha</span><span class="sxs-lookup"><span data-stu-id="ffc42-111">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="ffc42-112">Limitando o tempo de vida de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="ffc42-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="ffc42-113">Desprotegendo cargas cujas chaves foram revogadas</span><span class="sxs-lookup"><span data-stu-id="ffc42-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="ffc42-114">Configuração</span><span class="sxs-lookup"><span data-stu-id="ffc42-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="ffc42-115">Configurar a proteção de dados</span><span class="sxs-lookup"><span data-stu-id="ffc42-115">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="ffc42-116">Configurações padrão</span><span class="sxs-lookup"><span data-stu-id="ffc42-116">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="ffc42-117">Política ampla de computador</span><span class="sxs-lookup"><span data-stu-id="ffc42-117">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="ffc42-118">Cenários sem reconhecimento de DI</span><span class="sxs-lookup"><span data-stu-id="ffc42-118">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="ffc42-119">APIs de extensibilidade</span><span class="sxs-lookup"><span data-stu-id="ffc42-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="ffc42-120">Extensibilidade da criptografia básica</span><span class="sxs-lookup"><span data-stu-id="ffc42-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="ffc42-121">Extensibilidade de gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="ffc42-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="ffc42-122">APIs diversas</span><span class="sxs-lookup"><span data-stu-id="ffc42-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="ffc42-123">Implementação</span><span class="sxs-lookup"><span data-stu-id="ffc42-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="ffc42-124">Detalhes de criptografia autenticada</span><span class="sxs-lookup"><span data-stu-id="ffc42-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="ffc42-125">Derivação de subchaves e criptografia autenticada</span><span class="sxs-lookup"><span data-stu-id="ffc42-125">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="ffc42-126">Cabeçalhos de contexto</span><span class="sxs-lookup"><span data-stu-id="ffc42-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="ffc42-127">Gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="ffc42-127">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="ffc42-128">Provedores de armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="ffc42-128">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="ffc42-129">Criptografia de chave em repouso</span><span class="sxs-lookup"><span data-stu-id="ffc42-129">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="ffc42-130">Imutabilidade de chave e alteração de configurações</span><span class="sxs-lookup"><span data-stu-id="ffc42-130">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="ffc42-131">Formato do armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="ffc42-131">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="ffc42-132">Provedores de proteção de dados efêmeros</span><span class="sxs-lookup"><span data-stu-id="ffc42-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="ffc42-133">Compatibilidade</span><span class="sxs-lookup"><span data-stu-id="ffc42-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="ffc42-134">Compartilhar cookies entre aplicativos</span><span class="sxs-lookup"><span data-stu-id="ffc42-134">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="ffc42-135">Substituindo <machineKey> no ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ffc42-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
