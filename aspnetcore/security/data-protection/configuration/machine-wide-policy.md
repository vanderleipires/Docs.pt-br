---
title: "Suporte a política de máquina de proteção de dados no ASP.NET Core"
author: rick-anderson
description: "Saiba mais sobre suporte para definição de uma política de todo o computador padrão para todos os aplicativos que consomem a proteção de dados do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 53ded37e9fd5f1a2eaa37935d1c52efb1e9231ac
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Suporte a política de máquina de proteção de dados no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Quando em execução no Windows, o sistema de proteção de dados tem suporte limitado para definir uma política de todo o computador padrão para todos os aplicativos que consomem a proteção de dados do ASP.NET Core. A ideia geral é que um administrador pode querer alterar uma configuração padrão, como os algoritmos usados ou a vida útil da chave, sem a necessidade de atualizar manualmente cada aplicativo no computador.

> [!WARNING]
> O administrador do sistema pode definir a política padrão, mas eles não é possível impor a ele. O desenvolvedor do aplicativo sempre pode substituir qualquer valor com um dos seus próprios escolhendo. A política padrão afeta somente os aplicativos em que o desenvolvedor não foi especificado um valor explícito para uma configuração.

## <a name="setting-default-policy"></a>Configuração de política padrão

Para definir a política padrão, um administrador pode definir os valores conhecidos no registro do sistema na seguinte chave do registro:

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Se você estiver em um sistema operacional de 64 bits e quiser afetar o comportamento de aplicativos de 32 bits, lembre-se de configurar o equivalente Wow6432Node a chave acima.

Os valores com suporte são mostrados abaixo.

| Valor              | Tipo   | Descrição |
| ------------------ | :----: | ----------- |
| EncryptionType     | cadeia de caracteres | Especifica os algoritmos que devem ser usados para proteção de dados. O valor deve ser CBC CNG, GCM CNG ou gerenciado e é descrito com mais detalhes abaixo. |
| DefaultKeyLifetime | DWORD  | Especifica o tempo de vida para chaves geradas recentemente. O valor é especificado em dias e deve ser > = 7. |
| KeyEscrowSinks     | cadeia de caracteres | Especifica os tipos que são usados para caução de chaves. O valor é uma lista separada por ponto-e-vírgula de Coletores de caução de chaves, onde cada elemento na lista é o nome qualificado do assembly de um tipo que implementa [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Tipos de criptografia

Se EncryptionType é CBC CNG, o sistema está configurado para usar uma codificação de bloco simétrica de modo CBC para confidencialidade e HMAC autenticidade com serviços fornecidos pelo Windows CNG (consulte [especificando algoritmos personalizados de Windows CNG](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) para mais detalhes). Os seguintes valores adicionais são suportados, cada uma correspondendo a uma propriedade do tipo CngCbcAuthenticatedEncryptionSettings.

| Valor                       | Tipo   | Descrição |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | cadeia de caracteres | O nome de um algoritmo de criptografia simétrica bloco entendido pelo CNG. Esse algoritmo é aberto no modo CBC. |
| EncryptionAlgorithmProvider | cadeia de caracteres | O nome da implementação do provedor CNG que pode produzir o algoritmo EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | O comprimento (em bits) da chave derivada para o algoritmo de criptografia simétrica de bloco. |
| HashAlgorithm               | cadeia de caracteres | O nome de um algoritmo de hash entendido pelo CNG. Esse algoritmo é aberto no modo HMAC. |
| HashAlgorithmProvider       | cadeia de caracteres | O nome da implementação do provedor CNG que pode produzir o algoritmo HashAlgorithm. |

Se EncryptionType é GCM CNG, o sistema está configurado para usar uma codificação de bloco simétrica de modo Galois/contador para autenticidade e confidencialidade com serviços fornecidos pelo Windows CNG (consulte [especificando algoritmos personalizados de Windows CNG](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) Para obter mais detalhes). Os seguintes valores adicionais são suportados, cada uma correspondendo a uma propriedade do tipo CngGcmAuthenticatedEncryptionSettings.

| Valor                       | Tipo   | Descrição |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | cadeia de caracteres | O nome de um algoritmo de criptografia simétrica bloco entendido pelo CNG. Esse algoritmo é aberto no modo de Galois/contador. |
| EncryptionAlgorithmProvider | cadeia de caracteres | O nome da implementação do provedor CNG que pode produzir o algoritmo EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | O comprimento (em bits) da chave derivada para o algoritmo de criptografia simétrica de bloco. |

Se EncryptionType for gerenciado, o sistema está configurado para usar um SymmetricAlgorithm gerenciado para confidencialidade e KeyedHashAlgorithm autenticidade (consulte [especificando personalizado gerenciado algoritmos](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) para obter mais detalhes). Os seguintes valores adicionais são suportados, cada uma correspondendo a uma propriedade do tipo ManagedAuthenticatedEncryptionSettings.

| Valor                      | Tipo   | Descrição |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | cadeia de caracteres | O nome qualificado do assembly de um tipo que implementa SymmetricAlgorithm. |
| EncryptionAlgorithmKeySize | DWORD  | O comprimento (em bits) da chave para derivar o algoritmo de criptografia simétrica. |
| ValidationAlgorithmType    | cadeia de caracteres | O nome qualificado do assembly de um tipo que implementa KeyedHashAlgorithm. |

Se EncryptionType tiver qualquer valor diferente de nulo ou vazio, o sistema de proteção de dados gera uma exceção durante a inicialização.

> [!WARNING]
> Ao configurar uma configuração de política padrão que envolve os nomes de tipo (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), os tipos devem estar disponíveis para o aplicativo. Isso significa que para aplicativos em execução no CLR de área de trabalho, os assemblies que contêm esses tipos devem estar presentes no Cache de Assembly Global (GAC). Para aplicativos do ASP.NET Core em execução no [.NET Core](https://www.microsoft.com/net/core), os pacotes que contêm esses tipos devem ser instalados.
