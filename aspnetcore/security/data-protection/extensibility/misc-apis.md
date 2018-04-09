---
title: APIs de proteção de dados de diversos principais do ASP.NET
author: rick-anderson
description: Saiba mais sobre a interface ISecret de proteção de dados do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 484c6a0979a10e7cf2b801873655caa99a42532c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>APIs de proteção de dados de diversos principais do ASP.NET

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
