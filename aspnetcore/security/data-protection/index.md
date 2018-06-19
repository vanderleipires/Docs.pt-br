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
# <a name="data-protection-in-aspnet-core"></a>Proteção de Dados no ASP.NET Core

* [Introdução à proteção de dados](xref:security/data-protection/introduction)

* [Introdução às APIs de proteção de dados](xref:security/data-protection/using-data-protection)

* [APIs de consumidor](xref:security/data-protection/consumer-apis/index)

  * [Visão geral das APIs de consumidor](xref:security/data-protection/consumer-apis/overview)

  * [Cadeias de caracteres de finalidade](xref:security/data-protection/consumer-apis/purpose-strings)

  * [Multilocação e hierarquia de finalidade](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [Senhas hash](xref:security/data-protection/consumer-apis/password-hashing)

  * [Limitar o tempo de vida de cargas protegidas](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [Desproteger cargas cujas chaves foram revogadas](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [Configuração](xref:security/data-protection/configuration/index)

  * [Configurar a proteção de dados do ASP.NET Core](xref:security/data-protection/configuration/overview)

  * [Configurações padrão](xref:security/data-protection/configuration/default-settings)

  * [Política ampla de computador](xref:security/data-protection/configuration/machine-wide-policy)

  * [Cenários sem reconhecimento de DI](xref:security/data-protection/configuration/non-di-scenarios)

* [APIs de extensibilidade](xref:security/data-protection/extensibility/index)

  * [Extensibilidade da criptografia básica](xref:security/data-protection/extensibility/core-crypto)

  * [Extensibilidade de gerenciamento de chaves](xref:security/data-protection/extensibility/key-management)

  * [APIs diversas](xref:security/data-protection/extensibility/misc-apis)

* [Implementação](xref:security/data-protection/implementation/index)

  * [Detalhes de criptografia autenticada](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [Derivação de subchaves e criptografia autenticada](xref:security/data-protection/implementation/subkeyderivation)

  * [Cabeçalhos de contexto](xref:security/data-protection/implementation/context-headers)

  * [Gerenciamento de chaves](xref:security/data-protection/implementation/key-management)

  * [Provedores de armazenamento de chaves](xref:security/data-protection/implementation/key-storage-providers)

  * [Criptografia de chave em repouso](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [Imutabilidade de chave e configurações](xref:security/data-protection/implementation/key-immutability)

  * [Formato do armazenamento de chaves](xref:security/data-protection/implementation/key-storage-format)

  * [Provedores de proteção de dados efêmeros](xref:security/data-protection/implementation/key-storage-ephemeral)

* [Compatibilidade](xref:security/data-protection/compatibility/index)

  * [Substituição do ASP.NET <machineKey> no ASP.NET Core](xref:security/data-protection/compatibility/replacing-machinekey)
