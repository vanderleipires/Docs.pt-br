---
title: Tempo de vida e gerenciamento de chaves
author: rick-anderson
description: Descreve o tempo de vida e gerenciamento de chaves.
keywords: Gerenciamento, DPAPI, DataProtection de chaves do ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ef7dad2a-7029-4ae5-8f06-1fbebedccaa4
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: c361af7d336fc0f7651e5d2f28d71515e2949c65
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="key-management-and-lifetime"></a>Tempo de vida e gerenciamento de chaves

<a name=data-protection-default-settings></a>

## <a name="key-management"></a>Gerenciamento de chaves

O sistema tenta detectar seu ambiente operacional e fornecer a BOM padrões de comportamentos de configuração de zero. A heurística usada é a seguinte:

1. Se o sistema está sendo hospedado no Azure Web Sites, as chaves são mantidas para a pasta "% HOME%\ASP.NET\DataProtection-Keys". Esta pasta é apoiada pelo repositório de rede e é sincronizada em todos os computadores que hospedam o aplicativo. As chaves não são protegidas em repouso. Essa pasta fornece o anel de chave para todas as instâncias de um aplicativo em um slot de implantação única. Slots de implantação separado, como preparação e produção, não compartilharão um anel de chave. Quando você alternar entre os slots de implantação, por exemplo, troca de preparo para produção ou usando A / B teste, qualquer sistema usando a proteção de dados não poderá descriptografar dados armazenados usando o anel de chave dentro do slot anterior. Isso levará a usuários que estão sendo conectados fora de um aplicativo ASP.NET que usa o middleware de cookie padrão do ASP.NET, pois ele usa a proteção de dados para proteger seus cookies. Se você desejar independente de slot chave anéis, usar um provedor de anel de chave externa, como armazenamento Blob do Azure, Cofre de chaves do Azure, um repositório SQL, ou o cache Redis.

2. Se o perfil de usuário estiver disponível, as chaves são mantidas para a pasta "% LOCALAPPDATA%\ASP.NET\DataProtection-Keys". Além disso, se o sistema operacional for Windows, eles serão criptografados em repouso usando a DPAPI.

3. Se o aplicativo está hospedado no IIS, as chaves são persistentes no registro HKLM em uma chave do registro especial que é ACLed apenas para a conta de processo do operador. As chaves são criptografadas em repouso usando a DPAPI.

4. Se corresponder a nenhuma dessas condições, as chaves não são mantidas fora do processo atual. Quando o processo é desligado, todos os geradas chaves serão perdidas.

O desenvolvedor está sempre no controle total e pode substituir como e onde as chaves são armazenadas. As três primeiras opções acima devem bom padrões para a maioria dos aplicativos semelhante a como o ASP.NET <machineKey> rotinas de geração automática funcionaram no passado. Final, opção de failback outono é o único cenário que requer que o desenvolvedor especifique realmente [configuração](overview.md) inicial se quiserem persistência de chave, mas este retorno só podem ocorrer em situações raras.

>[!WARNING]
> Se o desenvolvedor substitui essa heurística e aponta o sistema de proteção de dados em um repositório de chave específico, a criptografia automática de chaves em repouso será desabilitada. Em repouso proteção pode ser habilitada novamente por meio de [configuração](overview.md).

## <a name="key-lifetime"></a>Vida útil da chave

Chaves por padrão tem um tempo de vida de 90 dias. Quando uma chave expira, o sistema será automaticamente gere uma nova chave e definir a nova chave como a chave ativa. Como chaves desativadas permanecem no sistema ainda poderá descriptografar os dados protegidos com eles. Consulte [gerenciamento de chaves](../implementation/key-management.md#data-protection-implementation-key-management-expiration) para obter mais informações.

## <a name="default-algorithms"></a>Algoritmos padrão

O algoritmo de proteção de carga padrão usado é AES-256-CBC para confidencialidade e HMACSHA256 autenticidade. Uma chave mestra de 512 bits, revertida a cada 90 dias, é usada para derivar as duas chaves subdiretório usadas para esses algoritmos em uma base por carga. Consulte [subchave derivação](../implementation/subkeyderivation.md#data-protection-implementation-subkey-derivation-aad) para obter mais informações.
