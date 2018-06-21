---
title: Proteção de Dados no ASP.NET Core
author: rick-anderson
description: Este documento serve como um sumário para os diversos tópicos sobre proteção de dados do ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277118"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="6e1ef-103">Proteção de Dados no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e1ef-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="6e1ef-104">Introdução à proteção de dados</span><span class="sxs-lookup"><span data-stu-id="6e1ef-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="6e1ef-105">Introdução às APIs de proteção de dados</span><span class="sxs-lookup"><span data-stu-id="6e1ef-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="6e1ef-106">APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="6e1ef-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="6e1ef-107">Visão geral das APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="6e1ef-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="6e1ef-108">Cadeias de caracteres de finalidade</span><span class="sxs-lookup"><span data-stu-id="6e1ef-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="6e1ef-109">Multilocação e hierarquia de finalidade</span><span class="sxs-lookup"><span data-stu-id="6e1ef-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="6e1ef-110">Senhas hash</span><span class="sxs-lookup"><span data-stu-id="6e1ef-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="6e1ef-111">Limitar o tempo de vida de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="6e1ef-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="6e1ef-112">Desproteger cargas cujas chaves foram revogadas</span><span class="sxs-lookup"><span data-stu-id="6e1ef-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="6e1ef-113">Configuração</span><span class="sxs-lookup"><span data-stu-id="6e1ef-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="6e1ef-114">Configurar a proteção de dados do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e1ef-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="6e1ef-115">Configurações padrão</span><span class="sxs-lookup"><span data-stu-id="6e1ef-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="6e1ef-116">Política ampla de computador</span><span class="sxs-lookup"><span data-stu-id="6e1ef-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="6e1ef-117">Cenários sem reconhecimento de DI</span><span class="sxs-lookup"><span data-stu-id="6e1ef-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="6e1ef-118">APIs de extensibilidade</span><span class="sxs-lookup"><span data-stu-id="6e1ef-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="6e1ef-119">Extensibilidade da criptografia básica</span><span class="sxs-lookup"><span data-stu-id="6e1ef-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="6e1ef-120">Extensibilidade de gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="6e1ef-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="6e1ef-121">APIs diversas</span><span class="sxs-lookup"><span data-stu-id="6e1ef-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="6e1ef-122">Implementação</span><span class="sxs-lookup"><span data-stu-id="6e1ef-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="6e1ef-123">Detalhes de criptografia autenticada</span><span class="sxs-lookup"><span data-stu-id="6e1ef-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="6e1ef-124">Derivação de subchaves e criptografia autenticada</span><span class="sxs-lookup"><span data-stu-id="6e1ef-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="6e1ef-125">Cabeçalhos de contexto</span><span class="sxs-lookup"><span data-stu-id="6e1ef-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="6e1ef-126">Gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="6e1ef-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="6e1ef-127">Provedores de armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="6e1ef-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="6e1ef-128">Criptografia de chave em repouso</span><span class="sxs-lookup"><span data-stu-id="6e1ef-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="6e1ef-129">Imutabilidade de chave e configurações</span><span class="sxs-lookup"><span data-stu-id="6e1ef-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="6e1ef-130">Formato do armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="6e1ef-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="6e1ef-131">Provedores de proteção de dados efêmeros</span><span class="sxs-lookup"><span data-stu-id="6e1ef-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="6e1ef-132">Compatibilidade</span><span class="sxs-lookup"><span data-stu-id="6e1ef-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="6e1ef-133">Substituição do ASP.NET <machineKey> no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e1ef-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
