---
title: APIs diversas
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 541dd721a00495632f0d633577b55933c9be03fa
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="miscellaneous-apis"></a>APIs diversas

<a name=data-protection-extensibility-mics-apis></a>

>[!WARNING]
> Tipos que implementam qualquer uma das seguintes interfaces devem ser thread-safe para chamadores vários.

## <a name="isecret"></a>ISecret

A interface ISecret representa um valor de segredo, como o material de chave de criptografia. Ele contém a superfície de API a seguir.

* Comprimento: int

* Descarte: void

* WriteSecretIntoBuffer (ArraySegment<byte> buffer): void

O método WriteSecretIntoBuffer preenche o buffer fornecido com o valor bruto de segredo. O motivo pelo qual que essa API usa o buffer como um parâmetro em vez de retornar que um byte [] diretamente é que isso permite que o chamador fixar o objeto de buffer, limitar a exposição de segredo para o coletor de lixo gerenciada.

O tipo secreto é uma implementação concreta de ISecret onde o valor de segredo é armazenado na memória no processo. Em plataformas Windows, o valor do segredo é criptografado por meio de [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
