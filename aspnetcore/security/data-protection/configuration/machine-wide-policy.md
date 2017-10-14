---
title: "Política de largura de máquina"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: fde8f75422c9dd84311a65b21e1e38b47fbe0306
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="machine-wide-policy"></a><span data-ttu-id="70a04-103">Política de largura de máquina</span><span class="sxs-lookup"><span data-stu-id="70a04-103">Machine Wide Policy</span></span>

<a name="data-protection-configuration-machinewidepolicy"></a>

<span data-ttu-id="70a04-104">Quando em execução no Windows, o sistema de proteção de dados tem suporte limitado para configurar a política de todo o computador padrão para todos os aplicativos que consomem a proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="70a04-104">When running on Windows, the data protection system has limited support for setting default machine-wide policy for all applications which consume data protection.</span></span> <span data-ttu-id="70a04-105">A ideia geral é que um administrador pode querer alterar alguma configuração padrão (como os algoritmos usados ou chave tempo de vida) sem a necessidade de atualizar manualmente cada aplicativo na máquina.</span><span class="sxs-lookup"><span data-stu-id="70a04-105">The general idea is that an administrator might wish to change some default setting (such as algorithms used or key lifetime) without needing to manually update every application on the machine.</span></span>

>[!WARNING]
> <span data-ttu-id="70a04-106">O administrador do sistema pode definir a política padrão, mas eles não é possível impor a ele.</span><span class="sxs-lookup"><span data-stu-id="70a04-106">The system administrator can set default policy, but they cannot enforce it.</span></span> <span data-ttu-id="70a04-107">O desenvolvedor do aplicativo sempre pode substituir qualquer valor com um dos seus próprios escolhendo.</span><span class="sxs-lookup"><span data-stu-id="70a04-107">The application developer can always override any value with one of their own choosing.</span></span> <span data-ttu-id="70a04-108">A política padrão afeta somente os aplicativos onde o desenvolvedor não tiver especificado um valor explícito para a configuração em particular.</span><span class="sxs-lookup"><span data-stu-id="70a04-108">The default policy only affects applications where the developer has not specified an explicit value for some particular setting.</span></span>

## <a name="setting-default-policy"></a><span data-ttu-id="70a04-109">Configuração de política padrão</span><span class="sxs-lookup"><span data-stu-id="70a04-109">Setting default policy</span></span>

<span data-ttu-id="70a04-110">Para definir a política padrão, um administrador pode definir os valores conhecidos no registro do sistema sob a chave a seguir.</span><span class="sxs-lookup"><span data-stu-id="70a04-110">To set default policy, an administrator can set known values in the system registry under the following key.</span></span>

<span data-ttu-id="70a04-111">A chave do registro:`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span><span class="sxs-lookup"><span data-stu-id="70a04-111">Reg key: `HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span></span>

<span data-ttu-id="70a04-112">Se você estiver em um sistema operacional de 64 bits e deseja afetam o comportamento de aplicativos de 32 bits, lembre-se também configurar o equivalente Wow6432Node a chave acima.</span><span class="sxs-lookup"><span data-stu-id="70a04-112">If you're on a 64-bit operating system and want to affect the behavior of 32-bit applications, remember also to configure the Wow6432Node equivalent of the above key.</span></span>

<span data-ttu-id="70a04-113">Os valores com suporte são:</span><span class="sxs-lookup"><span data-stu-id="70a04-113">The supported values are:</span></span>

* <span data-ttu-id="70a04-114">EncryptionType [string] - Especifica quais algoritmos devem ser usados para proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="70a04-114">EncryptionType [string] - specifies which algorithms should be used for data protection.</span></span> <span data-ttu-id="70a04-115">Esse valor deve ser "CNG CBC", "CNG GCM" ou "Gerenciado" e é descrito mais detalhadamente [abaixo](#data-protection-encryption-types).</span><span class="sxs-lookup"><span data-stu-id="70a04-115">This value must be "CNG-CBC", "CNG-GCM", or "Managed" and is described in more detail [below](#data-protection-encryption-types).</span></span>

* <span data-ttu-id="70a04-116">DefaultKeyLifetime [DWORD] - Especifica o tempo de vida para chaves geradas recentemente.</span><span class="sxs-lookup"><span data-stu-id="70a04-116">DefaultKeyLifetime [DWORD] - specifies the lifetime for newly-generated keys.</span></span> <span data-ttu-id="70a04-117">Esse valor é especificado em dias e deve ser ≥ 7.</span><span class="sxs-lookup"><span data-stu-id="70a04-117">This value is specified in days and must be ≥ 7.</span></span>

* <span data-ttu-id="70a04-118">KeyEscrowSinks [string] - Especifica os tipos que serão usados para caução de chaves.</span><span class="sxs-lookup"><span data-stu-id="70a04-118">KeyEscrowSinks [string] - specifies the types which will be used for key escrow.</span></span> <span data-ttu-id="70a04-119">Esse valor é uma lista separada por ponto-e-vírgula de Coletores de caução de chaves, onde cada elemento na lista é o nome qualificado do assembly de um tipo que implementa IKeyEscrowSink.</span><span class="sxs-lookup"><span data-stu-id="70a04-119">This value is a semicolon-delimited list of key escrow sinks, where each element in the list is the assembly qualified name of a type which implements IKeyEscrowSink.</span></span>

<a name="data-protection-encryption-types"></a>

### <a name="encryption-types"></a><span data-ttu-id="70a04-120">Tipos de criptografia</span><span class="sxs-lookup"><span data-stu-id="70a04-120">Encryption types</span></span>

<span data-ttu-id="70a04-121">Se EncryptionType é "CNG CBC", o sistema será configurado para usar uma codificação de bloco simétrica de modo CBC para confidencialidade e HMAC autenticidade com serviços fornecidos pelo Windows CNG (consulte [especificando algoritmos personalizados de Windows CNG](overview.md#data-protection-changing-algorithms-cng)para obter mais detalhes).</span><span class="sxs-lookup"><span data-stu-id="70a04-121">If EncryptionType is "CNG-CBC", the system will be configured to use a CBC-mode symmetric block cipher for confidentiality and HMAC for authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="70a04-122">Os seguintes valores adicionais são suportados, cada uma correspondendo a uma propriedade do tipo CngCbcAuthenticatedEncryptionSettings:</span><span class="sxs-lookup"><span data-stu-id="70a04-122">The following additional values are supported, each of which corresponds to a property on the CngCbcAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="70a04-123">EncryptionAlgorithm [string] - o nome de um algoritmo de criptografia simétrica bloco entendido pelo CNG.</span><span class="sxs-lookup"><span data-stu-id="70a04-123">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="70a04-124">Esse algoritmo será aberto no modo CBC.</span><span class="sxs-lookup"><span data-stu-id="70a04-124">This algorithm will be opened in CBC mode.</span></span>

* <span data-ttu-id="70a04-125">EncryptionAlgorithmProvider [string] - o nome da implementação do provedor CNG, que pode produzir o algoritmo EncryptionAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="70a04-125">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="70a04-126">EncryptionAlgorithmKeySize [DWORD] - o comprimento (em bits) da chave derivada para o algoritmo de criptografia simétrica de bloco.</span><span class="sxs-lookup"><span data-stu-id="70a04-126">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="70a04-127">HashAlgorithm [string] - o nome de um algoritmo de hash entendido pelo CNG.</span><span class="sxs-lookup"><span data-stu-id="70a04-127">HashAlgorithm [string] - the name of a hash algorithm understood by CNG.</span></span> <span data-ttu-id="70a04-128">Esse algoritmo será aberto no modo HMAC.</span><span class="sxs-lookup"><span data-stu-id="70a04-128">This algorithm will be opened in HMAC mode.</span></span>

* <span data-ttu-id="70a04-129">HashAlgorithmProvider [string] - o nome da implementação do provedor CNG, que pode produzir o algoritmo HashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="70a04-129">HashAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm HashAlgorithm.</span></span>

<span data-ttu-id="70a04-130">Se EncryptionType é "CNG GCM", o sistema será configurado para usar uma codificação de bloco simétrica de modo Galois/contador para autenticidade e confidencialidade com serviços fornecidos pelo Windows CNG (consulte [especificando algoritmos personalizados de Windows CNG](overview.md#data-protection-changing-algorithms-cng)para obter mais detalhes).</span><span class="sxs-lookup"><span data-stu-id="70a04-130">If EncryptionType is "CNG-GCM", the system will be configured to use a Galois/Counter Mode symmetric block cipher for confidentiality and authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="70a04-131">Os seguintes valores adicionais são suportados, cada uma correspondendo a uma propriedade do tipo CngGcmAuthenticatedEncryptionSettings:</span><span class="sxs-lookup"><span data-stu-id="70a04-131">The following additional values are supported, each of which corresponds to a property on the CngGcmAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="70a04-132">EncryptionAlgorithm [string] - o nome de um algoritmo de criptografia simétrica bloco entendido pelo CNG.</span><span class="sxs-lookup"><span data-stu-id="70a04-132">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="70a04-133">Esse algoritmo será aberto no modo de Galois/contador.</span><span class="sxs-lookup"><span data-stu-id="70a04-133">This algorithm will be opened in Galois/Counter Mode.</span></span>

* <span data-ttu-id="70a04-134">EncryptionAlgorithmProvider [string] - o nome da implementação do provedor CNG, que pode produzir o algoritmo EncryptionAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="70a04-134">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="70a04-135">EncryptionAlgorithmKeySize [DWORD] - o comprimento (em bits) da chave derivada para o algoritmo de criptografia simétrica de bloco.</span><span class="sxs-lookup"><span data-stu-id="70a04-135">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

<span data-ttu-id="70a04-136">Se EncryptionType for "gerenciado", o sistema será configurado para usar um SymmetricAlgorithm gerenciado para confidencialidade e KeyedHashAlgorithm autenticidade (consulte [especificando personalizado gerenciado algoritmos](overview.md#data-protection-changing-algorithms-custom-managed) para obter mais detalhes).</span><span class="sxs-lookup"><span data-stu-id="70a04-136">If EncryptionType is "Managed", the system will be configured to use a managed SymmetricAlgorithm for confidentiality and KeyedHashAlgorithm for authenticity (see [Specifying custom managed algorithms](overview.md#data-protection-changing-algorithms-custom-managed) for more details).</span></span> <span data-ttu-id="70a04-137">Os seguintes valores adicionais são suportados, cada uma correspondendo a uma propriedade do tipo ManagedAuthenticatedEncryptionSettings:</span><span class="sxs-lookup"><span data-stu-id="70a04-137">The following additional values are supported, each of which corresponds to a property on the ManagedAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="70a04-138">EncryptionAlgorithmType [string] - o nome qualificado do assembly de um tipo que implementa SymmetricAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="70a04-138">EncryptionAlgorithmType [string] - the assembly-qualified name of a type which implements SymmetricAlgorithm.</span></span>

* <span data-ttu-id="70a04-139">EncryptionAlgorithmKeySize [DWORD] - o comprimento (em bits) da chave para derivar o algoritmo de criptografia simétrica.</span><span class="sxs-lookup"><span data-stu-id="70a04-139">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric encryption algorithm.</span></span>

* <span data-ttu-id="70a04-140">ValidationAlgorithmType [string] - o nome de um tipo que implementa KeyedHashAlgorithm qualificado de assembly.</span><span class="sxs-lookup"><span data-stu-id="70a04-140">ValidationAlgorithmType [string] - the assembly-qualified name of a type which implements KeyedHashAlgorithm.</span></span>

<span data-ttu-id="70a04-141">Se EncryptionType tiver qualquer outro valor (que não seja nulo / vazio), o sistema de proteção de dados lançará uma exceção durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="70a04-141">If EncryptionType has any other value (other than null / empty), the data protection system will throw an exception at startup.</span></span>

>[!WARNING]
> <span data-ttu-id="70a04-142">Ao configurar uma configuração de política padrão que envolve os nomes de tipo (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), os tipos devem estar disponíveis para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="70a04-142">When configuring a default policy setting that involves type names (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), the types must be available to the application.</span></span> <span data-ttu-id="70a04-143">Na prática, isso significa que, para aplicativos executados em CLR de área de trabalho, os assemblies que contêm esses tipos devem ser GACed.</span><span class="sxs-lookup"><span data-stu-id="70a04-143">In practice, this means that for applications running on Desktop CLR, the assemblies which contain these types should be GACed.</span></span> <span data-ttu-id="70a04-144">Para aplicativos do ASP.NET Core em execução em [.NET Core](https://www.microsoft.com/net/core), os pacotes que contêm esses tipos devem ser instalados.</span><span class="sxs-lookup"><span data-stu-id="70a04-144">For ASP.NET Core applications running on [.NET Core](https://www.microsoft.com/net/core), the packages which contain these types should be installed.</span></span>
