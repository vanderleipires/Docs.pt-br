---
title: "Cenários com suporte a DI não"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a7d8a962-80ff-48e3-96f6-8472b7ba2df9
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 54a930c26f9f48ea0e6f7865e2927bcde0f4d6c0
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="non-di-aware-scenarios"></a>Cenários com suporte a DI não

O sistema de proteção de dados normalmente é projetado [a ser adicionado a um contêiner de serviço](../consumer-apis/overview.md) e a ser fornecido para os componentes dependentes por meio de um mecanismo de injeção de dependência. No entanto, pode haver alguns casos em que isso não é viável, especialmente ao importar o sistema para um aplicativo existente.

Para dar suporte a esses cenários o pacote Microsoft.AspNetCore.DataProtection.Extensions fornece um tipo concreto DataProtectionProvider que oferece uma maneira simples de usar o sistema de proteção de dados sem passar pelo caminhos de código específico de injeção de dependência. O próprio tipo implementa IDataProtectionProvider e criá-la é tão fácil quanto fornecer um DirectoryInfo onde as chaves de criptografia do provedor devem ser armazenadas.

Por exemplo:

[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]

>[!WARNING]
> Por padrão o DataProtectionProvider tipo concreto não criptografa material de chave bruto persisti-los antes do sistema de arquivos. Isso é para dar suporte a cenários onde os pontos de desenvolvedor para uma rede de compartilham, caso em que o sistema de proteção de dados não é possível deduzir automaticamente um mecanismo de criptografia de chave apropriado em repouso.
>
>Além disso, o tipo concreto DataProtectionProvider não [isolar aplicativos](overview.md#data-protection-configuration-per-app-isolation) por padrão, para que todos os aplicativos apontados para o mesmo diretório chave podem compartilhar cargas como sua finalidade correspondem.

O desenvolvedor do aplicativo pode atender a ambas se desejado. O construtor DataProtectionProvider aceita um [retorno de chamada de configuração opcional](overview.md#data-protection-configuration-callback) que pode ser usado para ajustar os comportamentos do sistema. O exemplo a seguir demonstra a restauração isolamento por meio de uma chamada explícita para SetApplicationName e também demonstra como configurar o sistema para criptografar automaticamente chaves persistentes usando Windows DPAPI. Se o diretório aponta para um compartilhamento UNC, você poderá distribuir um certificado compartilhado por todos os computadores relevantes e para configurar o sistema para usar a criptografia baseada em certificado em vez disso, por meio de uma chamada para [ProtectKeysWithCertificate](overview.md#configuring-x509-certificate).

[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]

>[!TIP]
> Instâncias do tipo concreto DataProtectionProvider são caras de criar. Se um aplicativo mantém várias instâncias desse tipo e se eles está apontado no mesmo diretório de armazenamento de chaves, o desempenho do aplicativo pode ser degradado. O uso pretendido é que o desenvolvedor do aplicativo instanciar esse tipo de uma vez, em seguida, mantenha a reutilizar essa referência único tanto quanto possível. O tipo de DataProtectionProvider e todas as instâncias de IDataProtector criadas a partir dele são thread-safe para chamadores vários.
