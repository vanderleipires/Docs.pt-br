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
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="f5df2-104">Proteção de Dados no ASP.NET Core: APIs de consumidor, configuração, APIs de extensibilidade e implementação</span><span class="sxs-lookup"><span data-stu-id="f5df2-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="f5df2-105">Introdução à Proteção de Dados</span><span class="sxs-lookup"><span data-stu-id="f5df2-105">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="f5df2-106">Introdução às APIs de Proteção de Dados</span><span class="sxs-lookup"><span data-stu-id="f5df2-106">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="f5df2-107">APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="f5df2-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="f5df2-108">Visão geral das APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="f5df2-108">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="f5df2-109">Cadeias de caracteres de finalidade</span><span class="sxs-lookup"><span data-stu-id="f5df2-109">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="f5df2-110">Multilocação e hierarquia de finalidade</span><span class="sxs-lookup"><span data-stu-id="f5df2-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="f5df2-111">Hash de senha</span><span class="sxs-lookup"><span data-stu-id="f5df2-111">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="f5df2-112">Limitando o tempo de vida de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="f5df2-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="f5df2-113">Desprotegendo cargas cujas chaves foram revogadas</span><span class="sxs-lookup"><span data-stu-id="f5df2-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="f5df2-114">Configuração</span><span class="sxs-lookup"><span data-stu-id="f5df2-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="f5df2-115">Configurando a Proteção de Dados</span><span class="sxs-lookup"><span data-stu-id="f5df2-115">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="f5df2-116">Configurações padrão</span><span class="sxs-lookup"><span data-stu-id="f5df2-116">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="f5df2-117">Política para todo o computador</span><span class="sxs-lookup"><span data-stu-id="f5df2-117">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="f5df2-118">Cenários sem reconhecimento de DI</span><span class="sxs-lookup"><span data-stu-id="f5df2-118">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="f5df2-119">APIs de extensibilidade</span><span class="sxs-lookup"><span data-stu-id="f5df2-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="f5df2-120">Extensibilidade da criptografia básica</span><span class="sxs-lookup"><span data-stu-id="f5df2-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="f5df2-121">Extensibilidade de gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="f5df2-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="f5df2-122">APIs diversas</span><span class="sxs-lookup"><span data-stu-id="f5df2-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="f5df2-123">Implementação</span><span class="sxs-lookup"><span data-stu-id="f5df2-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="f5df2-124">Detalhes de criptografia autenticada</span><span class="sxs-lookup"><span data-stu-id="f5df2-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="f5df2-125">Derivação de subchaves e criptografia autenticada</span><span class="sxs-lookup"><span data-stu-id="f5df2-125">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="f5df2-126">Cabeçalhos de contexto</span><span class="sxs-lookup"><span data-stu-id="f5df2-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="f5df2-127">Gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="f5df2-127">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="f5df2-128">Provedores de armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="f5df2-128">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="f5df2-129">Criptografia de chave em repouso</span><span class="sxs-lookup"><span data-stu-id="f5df2-129">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="f5df2-130">Imutabilidade de chave e alteração de configurações</span><span class="sxs-lookup"><span data-stu-id="f5df2-130">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="f5df2-131">Formato do armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="f5df2-131">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="f5df2-132">Provedores de proteção de dados efêmeros</span><span class="sxs-lookup"><span data-stu-id="f5df2-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="f5df2-133">Compatibilidade</span><span class="sxs-lookup"><span data-stu-id="f5df2-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="f5df2-134">Compartilhando cookies entre aplicativos</span><span class="sxs-lookup"><span data-stu-id="f5df2-134">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="f5df2-135">Substituindo <machineKey> no ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f5df2-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
