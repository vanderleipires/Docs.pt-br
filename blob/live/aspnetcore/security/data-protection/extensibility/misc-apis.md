---
title: APIs diversas
author: rick-anderson
description: "Este documento descreve a interface de ISecret de proteção de dados do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 88a08a25abf4c4e1ba0746087b05b1cc8fa13024
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="bcd68-103">APIs diversas</span><span class="sxs-lookup"><span data-stu-id="bcd68-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="bcd68-104">Tipos que implementam qualquer uma das seguintes interfaces devem ser thread-safe para chamadores vários.</span><span class="sxs-lookup"><span data-stu-id="bcd68-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="bcd68-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="bcd68-105">ISecret</span></span>

<span data-ttu-id="bcd68-106">O `ISecret` interface representa um valor de segredo, como o material de chave de criptografia.</span><span class="sxs-lookup"><span data-stu-id="bcd68-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="bcd68-107">Ele contém a superfície de API a seguir:</span><span class="sxs-lookup"><span data-stu-id="bcd68-107">It contains the following API surface:</span></span>

* <span data-ttu-id="bcd68-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="bcd68-108">`Length`: `int`</span></span>

* <span data-ttu-id="bcd68-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="bcd68-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="bcd68-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="bcd68-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="bcd68-111">O `WriteSecretIntoBuffer` método preenche o buffer fornecido com o valor bruto de segredo.</span><span class="sxs-lookup"><span data-stu-id="bcd68-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="bcd68-112">O motivo pelo qual essa API usa o buffer como um parâmetro em vez de retornar um `byte[]` diretamente é que isso permite que o chamador fixar o objeto de buffer, limitar a exposição de segredo para o coletor de lixo gerenciada.</span><span class="sxs-lookup"><span data-stu-id="bcd68-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="bcd68-113">O `Secret` tipo é uma implementação concreta de `ISecret` onde o valor de segredo é armazenado na memória no processo.</span><span class="sxs-lookup"><span data-stu-id="bcd68-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="bcd68-114">Em plataformas Windows, o valor do segredo é criptografado por meio de [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="bcd68-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
