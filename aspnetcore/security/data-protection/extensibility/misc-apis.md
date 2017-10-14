---
title: APIs diversas
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 72274a0da94a14bcd5af11d245ba626214fa2607
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="da0a6-103">APIs diversas</span><span class="sxs-lookup"><span data-stu-id="da0a6-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="da0a6-104">Tipos que implementam qualquer uma das seguintes interfaces devem ser thread-safe para chamadores vários.</span><span class="sxs-lookup"><span data-stu-id="da0a6-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="da0a6-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="da0a6-105">ISecret</span></span>

<span data-ttu-id="da0a6-106">A interface ISecret representa um valor de segredo, como o material de chave de criptografia.</span><span class="sxs-lookup"><span data-stu-id="da0a6-106">The ISecret interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="da0a6-107">Ele contém a superfície de API a seguir.</span><span class="sxs-lookup"><span data-stu-id="da0a6-107">It contains the following API surface.</span></span>

* <span data-ttu-id="da0a6-108">Comprimento: int</span><span class="sxs-lookup"><span data-stu-id="da0a6-108">Length : int</span></span>

* <span data-ttu-id="da0a6-109">Descarte: void</span><span class="sxs-lookup"><span data-stu-id="da0a6-109">Dispose() : void</span></span>

* <span data-ttu-id="da0a6-110">WriteSecretIntoBuffer (ArraySegment<byte> buffer): void</span><span class="sxs-lookup"><span data-stu-id="da0a6-110">WriteSecretIntoBuffer(ArraySegment<byte> buffer) : void</span></span>

<span data-ttu-id="da0a6-111">O método WriteSecretIntoBuffer preenche o buffer fornecido com o valor bruto de segredo.</span><span class="sxs-lookup"><span data-stu-id="da0a6-111">The WriteSecretIntoBuffer method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="da0a6-112">O motivo pelo qual que essa API usa o buffer como um parâmetro em vez de retornar que um byte [] diretamente é que isso permite que o chamador fixar o objeto de buffer, limitar a exposição de segredo para o coletor de lixo gerenciada.</span><span class="sxs-lookup"><span data-stu-id="da0a6-112">The reason this API takes the buffer as a parameter rather than returning a byte[] directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="da0a6-113">O tipo secreto é uma implementação concreta de ISecret onde o valor de segredo é armazenado na memória no processo.</span><span class="sxs-lookup"><span data-stu-id="da0a6-113">The Secret type is a concrete implementation of ISecret where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="da0a6-114">Em plataformas Windows, o valor do segredo é criptografado por meio de [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="da0a6-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
