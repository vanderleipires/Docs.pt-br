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
