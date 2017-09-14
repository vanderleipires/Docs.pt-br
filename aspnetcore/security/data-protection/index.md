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
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>Proteção de Dados no ASP.NET Core: APIs de consumidor, configuração, APIs de extensibilidade e implementação

* [Introdução à Proteção de Dados](introduction.md)

* [Introdução às APIs de Proteção de Dados](using-data-protection.md)

* [APIs de consumidor](consumer-apis/index.md)

  * [Visão geral das APIs de consumidor](consumer-apis/overview.md)

  * [Cadeias de caracteres de finalidade](consumer-apis/purpose-strings.md)

  * [Multilocação e hierarquia de finalidade](consumer-apis/purpose-strings-multitenancy.md)

  * [Hash de senha](consumer-apis/password-hashing.md)

  * [Limitando o tempo de vida de cargas protegidas](consumer-apis/limited-lifetime-payloads.md)

  * [Desprotegendo cargas cujas chaves foram revogadas](consumer-apis/dangerous-unprotect.md)

* [Configuração](configuration/index.md)

  * [Configurando a Proteção de Dados](configuration/overview.md)

  * [Configurações padrão](configuration/default-settings.md)

  * [Política para todo o computador](configuration/machine-wide-policy.md)

  * [Cenários sem reconhecimento de DI](configuration/non-di-scenarios.md)

* [APIs de extensibilidade](extensibility/index.md)

  * [Extensibilidade da criptografia básica](extensibility/core-crypto.md)

  * [Extensibilidade de gerenciamento de chaves](extensibility/key-management.md)

  * [APIs diversas](extensibility/misc-apis.md)

* [Implementação](implementation/index.md)

  * [Detalhes da criptografia autenticada.](implementation/authenticated-encryption-details.md)

  * [Derivação de subchaves e criptografia autenticada](implementation/subkeyderivation.md)

  * [Cabeçalhos de contexto](implementation/context-headers.md)

  * [Gerenciamento de chaves](implementation/key-management.md)

  * [Provedores de armazenamento de chaves](implementation/key-storage-providers.md)

  * [Criptografia de chave em repouso](implementation/key-encryption-at-rest.md)

  * [Imutabilidade de chave e alteração de configurações](implementation/key-immutability.md)

  * [Formato do armazenamento de chaves](implementation/key-storage-format.md)

  * [Provedores de proteção de dados efêmeros](implementation/key-storage-ephemeral.md)

* [Compatibilidade](compatibility/index.md)

  * [Compartilhando cookies entre aplicativos](compatibility/cookie-sharing.md)

  * [Substituindo <machineKey> no ASP.NET](compatibility/replacing-machinekey.md)
