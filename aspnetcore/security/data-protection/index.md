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
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>Proteção de Dados no ASP.NET Core: APIs de consumidor, configuração, APIs de extensibilidade e implementação

* [Introdução à proteção de dados](introduction.md)

* [Introdução às APIs de proteção de dados](using-data-protection.md)

* [APIs de consumidor](consumer-apis/index.md)

  * [Visão geral das APIs de consumidor](consumer-apis/overview.md)

  * [Cadeias de caracteres de finalidade](consumer-apis/purpose-strings.md)

  * [Multilocação e hierarquia de finalidade](consumer-apis/purpose-strings-multitenancy.md)

  * [Hash de senha](consumer-apis/password-hashing.md)

  * [Limitando o tempo de vida de cargas protegidas](consumer-apis/limited-lifetime-payloads.md)

  * [Desprotegendo cargas cujas chaves foram revogadas](consumer-apis/dangerous-unprotect.md)

* [Configuração](configuration/index.md)

  * [Configurar a proteção de dados](configuration/overview.md)

  * [Configurações padrão](configuration/default-settings.md)

  * [Política ampla de computador](configuration/machine-wide-policy.md)

  * [Cenários sem reconhecimento de DI](configuration/non-di-scenarios.md)

* [APIs de extensibilidade](extensibility/index.md)

  * [Extensibilidade da criptografia básica](extensibility/core-crypto.md)

  * [Extensibilidade de gerenciamento de chaves](extensibility/key-management.md)

  * [APIs diversas](extensibility/misc-apis.md)

* [Implementação](implementation/index.md)

  * [Detalhes de criptografia autenticada](implementation/authenticated-encryption-details.md)

  * [Derivação de subchaves e criptografia autenticada](implementation/subkeyderivation.md)

  * [Cabeçalhos de contexto](implementation/context-headers.md)

  * [Gerenciamento de chaves](implementation/key-management.md)

  * [Provedores de armazenamento de chaves](implementation/key-storage-providers.md)

  * [Criptografia de chave em repouso](implementation/key-encryption-at-rest.md)

  * [Imutabilidade de chave e alteração de configurações](implementation/key-immutability.md)

  * [Formato do armazenamento de chaves](implementation/key-storage-format.md)

  * [Provedores de proteção de dados efêmeros](implementation/key-storage-ephemeral.md)

* [Compatibilidade](compatibility/index.md)

  * [Substituindo <machineKey> no ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
