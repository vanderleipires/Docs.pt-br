---
title: Proteção de Dados no ASP.NET Core
author: rick-anderson
description: Este documento serve como um sumário para os diversos tópicos sobre proteção de dados do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071688"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="105da-103">Proteção de Dados no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="105da-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="105da-104">Introdução à proteção de dados</span><span class="sxs-lookup"><span data-stu-id="105da-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="105da-105">Introdução às APIs de proteção de dados</span><span class="sxs-lookup"><span data-stu-id="105da-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="105da-106">APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="105da-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="105da-107">Visão geral das APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="105da-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="105da-108">Cadeias de caracteres de finalidade</span><span class="sxs-lookup"><span data-stu-id="105da-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="105da-109">Multilocação e hierarquia de finalidade</span><span class="sxs-lookup"><span data-stu-id="105da-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="105da-110">Senhas hash</span><span class="sxs-lookup"><span data-stu-id="105da-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="105da-111">Limitar o tempo de vida de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="105da-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="105da-112">Desproteger cargas cujas chaves foram revogadas</span><span class="sxs-lookup"><span data-stu-id="105da-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="105da-113">Configuração</span><span class="sxs-lookup"><span data-stu-id="105da-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="105da-114">Configurar a proteção de dados do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="105da-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="105da-115">Configurações padrão</span><span class="sxs-lookup"><span data-stu-id="105da-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="105da-116">Política ampla de computador</span><span class="sxs-lookup"><span data-stu-id="105da-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="105da-117">Cenários sem reconhecimento de DI</span><span class="sxs-lookup"><span data-stu-id="105da-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="105da-118">APIs de extensibilidade</span><span class="sxs-lookup"><span data-stu-id="105da-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="105da-119">Extensibilidade da criptografia básica</span><span class="sxs-lookup"><span data-stu-id="105da-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="105da-120">Extensibilidade de gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="105da-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="105da-121">APIs diversas</span><span class="sxs-lookup"><span data-stu-id="105da-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="105da-122">Implementação</span><span class="sxs-lookup"><span data-stu-id="105da-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="105da-123">Detalhes de criptografia autenticada</span><span class="sxs-lookup"><span data-stu-id="105da-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="105da-124">Derivação de subchaves e criptografia autenticada</span><span class="sxs-lookup"><span data-stu-id="105da-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="105da-125">Cabeçalhos de contexto</span><span class="sxs-lookup"><span data-stu-id="105da-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="105da-126">Gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="105da-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="105da-127">Provedores de armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="105da-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="105da-128">Criptografia de chave em repouso</span><span class="sxs-lookup"><span data-stu-id="105da-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="105da-129">Imutabilidade de chave e configurações</span><span class="sxs-lookup"><span data-stu-id="105da-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="105da-130">Formato do armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="105da-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="105da-131">Provedores de proteção de dados efêmeros</span><span class="sxs-lookup"><span data-stu-id="105da-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="105da-132">Compatibilidade</span><span class="sxs-lookup"><span data-stu-id="105da-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="105da-133">Substituição do ASP.NET <machineKey> no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="105da-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
