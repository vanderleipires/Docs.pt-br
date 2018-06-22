---
title: A imutabilidade de chave e configurações de chave no núcleo do ASP.NET
author: rick-anderson
description: Obter os detalhes de implementação de imutabilidade principal APIs de proteção de dados do ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 45460e0bdf6ad0a978f984d55b64f562c13fb575
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279448"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>A imutabilidade de chave e configurações de chave no núcleo do ASP.NET

Depois que um objeto é persistido para o armazenamento de backup, sua representação para sempre é fixo. Novos dados podem ser adicionados ao repositório de backup, mas os dados existentes nunca podem ser modificados. O objetivo principal deste comportamento é evitar a corrupção de dados.

Uma consequência esse comportamento é que, quando uma chave é gravada para armazenamento de backup, é imutável. As datas de criação, ativação e expiração nunca podem ser alteradas, embora ele pode ser revogada usando `IKeyManager`. Além disso, suas informações algorítmicos subjacentes, material de chave mestre e criptografia propriedades rest também são imutáveis.

Se o desenvolvedor altera qualquer configuração que afeta a persistência de chave, essas alterações não entram em vigor até a próxima vez que uma chave é gerada, através de uma chamada explícita para `IKeyManager.CreateNewKey` ou por meio do sistema proteção de dados próprio [chave automática geração](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) comportamento. As configurações que afetam a persistência de chave são da seguinte maneira:

* [O tempo de vida de chave padrão](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [A criptografia de chave no mecanismo de rest](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [As informações de algoritmos contidas na chave do](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Se você precisar essas configurações de anteriores a próxima chave automática sem interrupção tempo, considere a possibilidade de fazer uma chamada explícita para `IKeyManager.CreateNewKey` para forçar a criação de uma nova chave. Lembre-se de fornecer uma data de ativação explícita ({agora + 2 dias} é uma boa regra prática para permitir um tempo para que a alteração se propague) e a data de expiração na chamada.

>[!TIP]
> Tocar o repositório de todos os aplicativos devem especificar as mesmas configurações com o `IDataProtectionBuilder` métodos de extensão. Caso contrário, as propriedades da chave persistente dependerá do aplicativo específico que invocou as rotinas de geração de chave.
