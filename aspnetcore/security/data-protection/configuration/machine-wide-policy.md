---
title: "Política de largura de máquina"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 513726e209401d158ac98d5874942765751ac07d
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="machine-wide-policy"></a>Política de largura de máquina

<a name=data-protection-configuration-machinewidepolicy></a>

Quando em execução no Windows, o sistema de proteção de dados tem suporte limitado para configurar a política de todo o computador padrão para todos os aplicativos que consomem a proteção de dados. A ideia geral é que um administrador pode querer alterar alguma configuração padrão (como os algoritmos usados ou chave tempo de vida) sem a necessidade de atualizar manualmente cada aplicativo na máquina.

>[!WARNING]
> O administrador do sistema pode definir a política padrão, mas eles não é possível impor a ele. O desenvolvedor do aplicativo sempre pode substituir qualquer valor com um dos seus próprios escolhendo. A política padrão afeta somente os aplicativos onde o desenvolvedor não tiver especificado um valor explícito para a configuração em particular.

## <a name="setting-default-policy"></a>Configuração de política padrão

Para definir a política padrão, um administrador pode definir os valores conhecidos no registro do sistema sob a chave a seguir.

A chave do registro:`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`

Se você estiver em um sistema operacional de 64 bits e deseja afetam o comportamento de aplicativos de 32 bits, lembre-se também configurar o equivalente Wow6432Node a chave acima.

Os valores com suporte são:

* EncryptionType [string] - Especifica quais algoritmos devem ser usados para proteção de dados. Esse valor deve ser "CNG CBC", "CNG GCM" ou "Gerenciado" e é descrito mais detalhadamente [abaixo](#data-protection-encryption-types).

* DefaultKeyLifetime [DWORD] - Especifica o tempo de vida para chaves geradas recentemente. Esse valor é especificado em dias e deve ser ≥ 7.

* KeyEscrowSinks [string] - Especifica os tipos que serão usados para caução de chaves. Esse valor é uma lista separada por ponto-e-vírgula de Coletores de caução de chaves, onde cada elemento na lista é o nome qualificado do assembly de um tipo que implementa IKeyEscrowSink.

<a name=data-protection-encryption-types></a>

### <a name="encryption-types"></a>Tipos de criptografia

Se EncryptionType é "CNG CBC", o sistema será configurado para usar uma codificação de bloco simétrica de modo CBC para confidencialidade e HMAC autenticidade com serviços fornecidos pelo Windows CNG (consulte [especificando algoritmos personalizados de Windows CNG](overview.md#data-protection-changing-algorithms-cng)para obter mais detalhes). Os seguintes valores adicionais são suportados, cada uma correspondendo a uma propriedade do tipo CngCbcAuthenticatedEncryptionSettings:

* EncryptionAlgorithm [string] - o nome de um algoritmo de criptografia simétrica bloco entendido pelo CNG. Esse algoritmo será aberto no modo CBC.

* EncryptionAlgorithmProvider [string] - o nome da implementação do provedor CNG, que pode produzir o algoritmo EncryptionAlgorithm.

* EncryptionAlgorithmKeySize [DWORD] - o comprimento (em bits) da chave derivada para o algoritmo de criptografia simétrica de bloco.

* HashAlgorithm [string] - o nome de um algoritmo de hash entendido pelo CNG. Esse algoritmo será aberto no modo HMAC.

* HashAlgorithmProvider [string] - o nome da implementação do provedor CNG, que pode produzir o algoritmo HashAlgorithm.

Se EncryptionType é "CNG GCM", o sistema será configurado para usar uma codificação de bloco simétrica de modo Galois/contador para autenticidade e confidencialidade com serviços fornecidos pelo Windows CNG (consulte [especificando algoritmos personalizados de Windows CNG](overview.md#data-protection-changing-algorithms-cng)para obter mais detalhes). Os seguintes valores adicionais são suportados, cada uma correspondendo a uma propriedade do tipo CngGcmAuthenticatedEncryptionSettings:

* EncryptionAlgorithm [string] - o nome de um algoritmo de criptografia simétrica bloco entendido pelo CNG. Esse algoritmo será aberto no modo de Galois/contador.

* EncryptionAlgorithmProvider [string] - o nome da implementação do provedor CNG, que pode produzir o algoritmo EncryptionAlgorithm.

* EncryptionAlgorithmKeySize [DWORD] - o comprimento (em bits) da chave derivada para o algoritmo de criptografia simétrica de bloco.

Se EncryptionType for "gerenciado", o sistema será configurado para usar um SymmetricAlgorithm gerenciado para confidencialidade e KeyedHashAlgorithm autenticidade (consulte [especificando personalizado gerenciado algoritmos](overview.md#data-protection-changing-algorithms-custom-managed) para obter mais detalhes). Os seguintes valores adicionais são suportados, cada uma correspondendo a uma propriedade do tipo ManagedAuthenticatedEncryptionSettings:

* EncryptionAlgorithmType [string] - o nome qualificado do assembly de um tipo que implementa SymmetricAlgorithm.

* EncryptionAlgorithmKeySize [DWORD] - o comprimento (em bits) da chave para derivar o algoritmo de criptografia simétrica.

* ValidationAlgorithmType [string] - o nome de um tipo que implementa KeyedHashAlgorithm qualificado de assembly.

Se EncryptionType tiver qualquer outro valor (que não seja nulo / vazio), o sistema de proteção de dados lançará uma exceção durante a inicialização.

>[!WARNING]
> Ao configurar uma configuração de política padrão que envolve os nomes de tipo (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), os tipos devem estar disponíveis para o aplicativo. Na prática, isso significa que, para aplicativos executados em CLR de área de trabalho, os assemblies que contêm esses tipos devem ser GACed. Para aplicativos do ASP.NET Core em execução em [.NET Core](https://microsoft.com/net/core), os pacotes que contêm esses tipos devem ser instalados.
