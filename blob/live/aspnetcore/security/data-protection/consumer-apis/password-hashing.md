---
title: Hash de senha
author: rick-anderson
description: "Este documento explica como hash senhas usando as APIs de proteção de dados ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: e97d17b5f6de2e0ddcde6d51618675388b43911a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="password-hashing"></a><span data-ttu-id="d2a2e-103">Hash de senha</span><span class="sxs-lookup"><span data-stu-id="d2a2e-103">Password Hashing</span></span>

<span data-ttu-id="d2a2e-104">O código de proteção de dados base inclui um pacote *Microsoft.AspNetCore.Cryptography.KeyDerivation* que contém funções de derivação de chave de criptografia.</span><span class="sxs-lookup"><span data-stu-id="d2a2e-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="d2a2e-105">Este pacote é um componente autônomo e não tem nenhuma dependência no restante do sistema de proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="d2a2e-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="d2a2e-106">Ele pode ser usado independentemente completamente.</span><span class="sxs-lookup"><span data-stu-id="d2a2e-106">It can be used completely independently.</span></span> <span data-ttu-id="d2a2e-107">A origem existe junto com o código de proteção de dados base como uma conveniência.</span><span class="sxs-lookup"><span data-stu-id="d2a2e-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="d2a2e-108">O pacote atualmente oferece um método `KeyDerivation.Pbkdf2` que permite o hash de uma senha usando o [PBKDF2 algoritmo](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="d2a2e-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="d2a2e-109">Essa API é muito semelhante à existente do .NET Framework [Rfc2898DeriveBytes tipo](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), mas há três diferenças importantes:</span><span class="sxs-lookup"><span data-stu-id="d2a2e-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="d2a2e-110">O `KeyDerivation.Pbkdf2` método dá suporte ao consumo vários PRFs (atualmente `HMACSHA1`, `HMACSHA256`, e `HMACSHA512`), enquanto o `Rfc2898DeriveBytes` digite dá suporte apenas à `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="d2a2e-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="d2a2e-111">O `KeyDerivation.Pbkdf2` método detecta o sistema operacional atual e tenta escolha a implementação mais otimizada da rotina, fornecendo um desempenho muito melhor em determinados casos.</span><span class="sxs-lookup"><span data-stu-id="d2a2e-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="d2a2e-112">(No Windows 8, ele oferece cerca de 10 vezes a taxa de transferência `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="d2a2e-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="d2a2e-113">O `KeyDerivation.Pbkdf2` método requer que o chamador pode especificar todos os parâmetros (salt, PRF e contagem de iteração).</span><span class="sxs-lookup"><span data-stu-id="d2a2e-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="d2a2e-114">O `Rfc2898DeriveBytes` tipo fornece valores padrão para eles.</span><span class="sxs-lookup"><span data-stu-id="d2a2e-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="d2a2e-115">Consulte o código-fonte para o ASP.NET Core Identity `PasswordHasher` caso de uso de tipo para um mundo real.</span><span class="sxs-lookup"><span data-stu-id="d2a2e-115">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
