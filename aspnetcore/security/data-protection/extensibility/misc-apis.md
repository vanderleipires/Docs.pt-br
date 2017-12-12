---
title: APIs diversas
author: rick-anderson
description: "Este documento descreve a interface de ISecret de proteção de dados do ASP.NET Core."
keywords: "ASP.NET Core, proteção de dados, ISecret"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: f5d6920f9f229bd480a76c952dab30efb7d9eff5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="miscellaneous-apis"></a>APIs diversas

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Tipos que implementam qualquer uma das seguintes interfaces devem ser thread-safe para chamadores vários.

## <a name="isecret"></a>ISecret

O `ISecret` interface representa um valor de segredo, como o material de chave de criptografia. Ele contém a superfície de API a seguir:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

O `WriteSecretIntoBuffer` método preenche o buffer fornecido com o valor bruto de segredo. O motivo pelo qual essa API usa o buffer como um parâmetro em vez de retornar um `byte[]` diretamente é que isso permite que o chamador fixar o objeto de buffer, limitar a exposição de segredo para o coletor de lixo gerenciada.

O `Secret` tipo é uma implementação concreta de `ISecret` onde o valor de segredo é armazenado na memória no processo. Em plataformas Windows, o valor do segredo é criptografado por meio de [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
