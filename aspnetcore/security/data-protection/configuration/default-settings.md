---
title: Gerenciamento de chaves de proteção de dados e o tempo de vida no núcleo do ASP.NET
author: rick-anderson
description: Saiba mais sobre o gerenciamento de chaves de proteção de dados e o tempo de vida no núcleo do ASP.NET.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: b43c14af015d5e03f46200c51a1218a581b1de0c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
ms.locfileid: "28887284"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Gerenciamento de chaves de proteção de dados e o tempo de vida no núcleo do ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>Gerenciamento de chaves

O aplicativo tenta detectar seu ambiente operacional e controlar a configuração de chave por conta própria.

1. Se o aplicativo é hospedado em [aplicativos do Azure](https://azure.microsoft.com/services/app-service/), as chaves são mantidas para o *%HOME%\ASP.NET\DataProtection-Keys* pasta. Esta pasta é apoiada pelo repositório de rede e é sincronizada em todos os computadores que hospedam o aplicativo.
   * As chaves não são protegidas em repouso.
   * O *DataProtection chaves* pasta fornece o anel de chave para todas as instâncias de um aplicativo em um slot de implantação única.
   * Slots de implantação separado, como preparação e produção, não compartilhem um anel de chave. Quando você alternar entre os slots de implantação, por exemplo, troca de preparo para produção ou usando A / B teste, qualquer aplicativo usando a proteção de dados não será capaz de descriptografar dados armazenados usando o anel de chave dentro do slot anterior. Isso leva a usuários que estão sendo conectados fora de um aplicativo que usa a autenticação de cookie padrão do ASP.NET Core, pois ele usa a proteção de dados para proteger seus cookies. Se você desejar independente de slot chave anéis, usar um provedor de anel de chave externa, como armazenamento Blob do Azure, Cofre de chaves do Azure, um repositório SQL, ou o cache Redis.

1. Se o perfil de usuário estiver disponível, as chaves são mantidas para o *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* pasta. Se o sistema operacional for Windows, as chaves são criptografadas em repouso usando a DPAPI.

1. Se o aplicativo é hospedado no IIS, as chaves são persistentes no registro HKLM em uma chave do registro especial que é ACLed apenas para a conta de processo do operador. As chaves são criptografadas em repouso usando a DPAPI.

1. Se nenhuma dessas condições corresponderem, as chaves não são mantidas fora do processo atual. Quando o processo é desligado, todos os geradas chaves serão perdidas.

O desenvolvedor está sempre no controle total e pode substituir como e onde as chaves são armazenadas. As três primeiras opções acima devem fornecer padrões bom para a maioria dos aplicativos semelhante a como o ASP.NET  **\<machineKey >** rotinas de geração automática funcionaram no passado. A opção de fallback, final é o único cenário que requer que o desenvolvedor especifique [configuração](xref:security/data-protection/configuration/overview) inicial se quiserem persistência de chave, mas essa fallback ocorra apenas em situações raras.

Ao hospedar em um contêiner do Docker, chaves devem ser persistidas em uma pasta que é um volume de Docker (um volume compartilhado ou um volume montado de host que persiste além do tempo de vida do contêiner) ou em um provedor externo, como [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ou [Redis](https://redis.io/). Um provedor externo também é útil em cenários de farm da web se os aplicativos não podem acessar um volume compartilhado de rede (consulte [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) para obter mais informações).

> [!WARNING]
> Se o desenvolvedor substitui as regras descritas acima e aponta o sistema de proteção de dados em um repositório de chave específico, a criptografia automática de chaves em repouso está desabilitada. Proteção pode ser habilitada novamente por meio de [configuração](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Vida útil da chave

Por padrão, as chaves têm um tempo de vida de 90 dias. Quando uma chave expira, o aplicativo gera uma nova chave automaticamente e define a nova chave como a chave ativa. Como chaves desativadas permanecerem no sistema, seu aplicativo pode descriptografar os dados protegidos com eles. Consulte [gerenciamento de chaves](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) para obter mais informações.

## <a name="default-algorithms"></a>Algoritmos padrão

O algoritmo de proteção de carga padrão usado é AES-256-CBC para confidencialidade e HMACSHA256 autenticidade. Uma chave mestra de 512 bits, alterada a cada 90 dias, é usada para derivar as duas chaves subdiretório usadas para esses algoritmos em uma base por carga. Consulte [subchave derivação](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) para obter mais informações.

## <a name="see-also"></a>Consulte também

* [Extensibilidade de gerenciamento de chaves](xref:security/data-protection/extensibility/key-management)
