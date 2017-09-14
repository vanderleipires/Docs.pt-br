---
title: "Proteção de Dados no ASP.NET Core"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 39f2ca96f8542de033274ea957b5c7736948c981
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="ac999-103">Proteção de Dados no ASP.NET Core: APIs de consumidor, configuração, APIs de extensibilidade e implementação</span><span class="sxs-lookup"><span data-stu-id="ac999-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="ac999-104">Introdução à Proteção de Dados</span><span class="sxs-lookup"><span data-stu-id="ac999-104">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="ac999-105">Introdução às APIs de Proteção de Dados</span><span class="sxs-lookup"><span data-stu-id="ac999-105">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="ac999-106">APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="ac999-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="ac999-107">Visão geral das APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="ac999-107">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="ac999-108">Cadeias de caracteres de finalidade</span><span class="sxs-lookup"><span data-stu-id="ac999-108">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="ac999-109">Multilocação e hierarquia de finalidade</span><span class="sxs-lookup"><span data-stu-id="ac999-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="ac999-110">Hash de senha</span><span class="sxs-lookup"><span data-stu-id="ac999-110">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="ac999-111">Limitando o tempo de vida de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="ac999-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="ac999-112">Desprotegendo cargas cujas chaves foram revogadas</span><span class="sxs-lookup"><span data-stu-id="ac999-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="ac999-113">Configuração</span><span class="sxs-lookup"><span data-stu-id="ac999-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="ac999-114">Configurando a Proteção de Dados</span><span class="sxs-lookup"><span data-stu-id="ac999-114">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="ac999-115">Configurações padrão</span><span class="sxs-lookup"><span data-stu-id="ac999-115">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="ac999-116">Política para todo o computador</span><span class="sxs-lookup"><span data-stu-id="ac999-116">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="ac999-117">Cenários sem reconhecimento de DI</span><span class="sxs-lookup"><span data-stu-id="ac999-117">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="ac999-118">APIs de extensibilidade</span><span class="sxs-lookup"><span data-stu-id="ac999-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="ac999-119">Extensibilidade da criptografia básica</span><span class="sxs-lookup"><span data-stu-id="ac999-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="ac999-120">Extensibilidade de gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="ac999-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="ac999-121">APIs diversas</span><span class="sxs-lookup"><span data-stu-id="ac999-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="ac999-122">Implementação</span><span class="sxs-lookup"><span data-stu-id="ac999-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="ac999-123">Detalhes da criptografia autenticada.</span><span class="sxs-lookup"><span data-stu-id="ac999-123">Authenticated encryption details.</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="ac999-124">Derivação de subchaves e criptografia autenticada</span><span class="sxs-lookup"><span data-stu-id="ac999-124">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="ac999-125">Cabeçalhos de contexto</span><span class="sxs-lookup"><span data-stu-id="ac999-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="ac999-126">Gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="ac999-126">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="ac999-127">Provedores de armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="ac999-127">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="ac999-128">Criptografia de chave em repouso</span><span class="sxs-lookup"><span data-stu-id="ac999-128">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="ac999-129">Imutabilidade de chave e alteração de configurações</span><span class="sxs-lookup"><span data-stu-id="ac999-129">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="ac999-130">Formato do armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="ac999-130">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="ac999-131">Provedores de proteção de dados efêmeros</span><span class="sxs-lookup"><span data-stu-id="ac999-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="ac999-132">Compatibilidade</span><span class="sxs-lookup"><span data-stu-id="ac999-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="ac999-133">Compartilhando cookies entre aplicativos</span><span class="sxs-lookup"><span data-stu-id="ac999-133">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="ac999-134">Substituindo <machineKey> no ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ac999-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
