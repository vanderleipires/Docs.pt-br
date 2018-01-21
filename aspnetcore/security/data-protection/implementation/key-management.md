---
title: Gerenciamento de chaves
author: rick-anderson
description: "Este documento descreve os detalhes da implementação do ASP.NET Core dados proteção gerenciamento APIs."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 53adb067751917a9539a310bb7d91e599696f213
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="key-management"></a>Gerenciamento de chaves

<a name="data-protection-implementation-key-management"></a>

O sistema de proteção de dados gerencia automaticamente o tempo de vida das chaves mestras usado para proteger e Desproteger cargas. Cada chave pode existir em um dos quatro estágios:

* Criado - a chave existe em anel chave, mas ainda não foi ativada. A chave não deve ser usada para novas operações de proteção até que tenha decorrido tempo suficiente que a chave tenha tido a oportunidade de se propague para todos os computadores que estão consumindo anel essa chave.

* Ativo - a chave existe do anel de chave e deve ser usado para todas as operações de proteção de novo.

* A chave expirada - ficou seu tempo de vida natural e não deve ser usada para novas operações de proteção.

* A chave revogada - estiver comprometida e não deve ser usada para novas operações de proteção.

Todas as chaves expiradas, ativas e criadas podem ser usadas para desproteger cargas de entrada. Chaves revogadas por padrão não podem ser usadas para desproteger cargas, mas o desenvolvedor do aplicativo pode [substituir esse comportamento](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect) se necessário.

>[!WARNING]
> O desenvolvedor pode ser tentado para excluir uma chave do anel de chave (por exemplo, excluindo o arquivo correspondente do sistema de arquivos). Nesse ponto, todos os dados protegidos pela chave é indecifráveis permanentemente e não há nenhuma substituição de emergência como ocorre com chaves revogadas. Exclusão de uma chave é realmente destrutivo comportamento e, consequentemente, o sistema de proteção de dados não expõe nenhuma API de primeira classe para executar esta operação.

## <a name="default-key-selection"></a>Seleção de chave padrão

Quando o sistema de proteção de dados lê o anel de chave do repositório de backup, ele tentará localizar uma chave "padrão" do anel de chave. A chave padrão é usada para novas operações de proteção.

A heurística geral é que o sistema de proteção de dados escolhe a chave com a data mais recente de ativação como a chave padrão. (Há um pequeno fator para permitir o relógio do servidor-para-servidor distorção.) Se a chave expirou ou foi revogada, e se o aplicativo não tiver desabilitado automática da chave de geração, uma nova chave será gerada com a ativação imediata pelo [chave expiração e sem interrupção](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) diretiva abaixo.

O motivo pelo qual o sistema de proteção de dados gera uma nova chave imediatamente em vez de fazer fallback para uma chave diferente é que a nova geração de chave deve ser tratada como uma expiração implícita de todas as chaves que foram ativados antes da nova chave. A ideia geral é que novas chaves podem ter sido configuradas com algoritmos diferentes ou mecanismos de criptografia em repouso que as chaves antigas, e o sistema deve preferir a configuração atual em vez de fazer o fallback.

Há uma exceção. Se o desenvolvedor do aplicativo tiver [desabilitado a geração automática de chaves](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), em seguida, o sistema de proteção de dados deve escolher algo como a chave padrão. Neste cenário de fallback, o sistema escolherá a chave não revogado com a data de ativação mais recente, com preferência para chaves que tem tido tempo para ser propagada para outros computadores no cluster. O sistema de fallback pode acabar escolhendo uma chave padrão expiradas como resultado. O sistema de fallback nunca escolherá uma chave revogada como a chave padrão e se o anel de chave está vazio ou foi revogado cada chave, em seguida, o sistema produzirá um erro na inicialização.

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>Expiração e sem interrupção

Quando uma chave é criada, ele recebe automaticamente uma data de ativação de {agora + 2 dias} e uma data de expiração de {agora + 90 dias}. O atraso de 2 dias antes da ativação fornece o momento chave se propague através do sistema. Ou seja, permite que outros aplicativos apontando para o armazenamento de backup observar a chave em seu próximo período de atualização automática, portanto, maximizando a probabilidade de que quando a chave de anel faz se tornar ativo foi propagado para todos os aplicativos que talvez precisem usá-la.

Se a chave padrão expirará dentro de 2 dias e o anel de chave ainda não tiver uma chave que ficará ativa após a expiração da chave padrão, o sistema de proteção de dados persistirá automaticamente uma nova chave para o anel de chave. Essa nova chave tem uma data de ativação de {data de validade da chave padrão} e a data de expiração de {agora + 90 dias}. Isso permite que o sistema automaticamente reverter chaves regularmente sem interrupção do serviço.

Pode haver circunstâncias em que uma chave será criada com a ativação imediata. Um exemplo seria quando o aplicativo não tiver sido executada por um tempo e todas as chaves do anel de chave expiradas. Quando isso acontece, a chave é fornecida uma data de ativação de {agora} sem o atraso de ativação de 2 dias normal.

O tempo de vida de chave padrão é de 90 dias, embora isso é configurado como no exemplo a seguir.

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

Um administrador também pode alterar o padrão para todo o sistema, embora uma chamada explícita para `SetDefaultKeyLifetime` substituirá qualquer política de todo o sistema. O tempo de vida de chave padrão não pode ser menor do que 7 dias.

## <a name="automatic-key-ring-refresh"></a>Atualização automática anel de chave

Quando o sistema de proteção de dados inicializado, ele lê o anel de chave do repositório subjacente e armazena em cache na memória. Esse cache permite proteger e Desproteger operações continuem sem atingir o armazenamento de backup. O sistema verifica automaticamente o repositório de backup para alterações de aproximadamente a cada 24 horas ou quando a chave padrão atual expirar, o que ocorrer primeiro.

>[!WARNING]
> Os desenvolvedores devem muito raramente (se) precisa usar APIs de gerenciamento de chave diretamente. O sistema de proteção de dados executará gerenciamento automático de chaves, conforme descrito acima.

O sistema de proteção de dados expõe uma interface `IKeyManager` que pode ser usado para inspecionar e fazer alterações para o anel de chave. O sistema de DI que forneceu a instância do `IDataProtectionProvider` também pode fornecer uma instância de `IKeyManager` para seu consumo. Como alternativa, você pode extrair o `IKeyManager` diretamente do `IServiceProvider` como no exemplo a seguir.

Qualquer operação que modifica o anel de chave (Criando uma nova chave explicitamente ou uma revogação) invalida o cache na memória. A próxima chamada para `Protect` ou `Unprotect` fará com que o sistema de proteção de dados reler o anel de chave e recriar o cache.

O exemplo a seguir demonstra como usar o `IKeyManager` interface para inspecionar e manipular o anel de chave, incluindo a revogação existente chaves e gerar uma nova chave manualmente.

[!code-csharp[Main](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>Armazenamento de chaves

O sistema de proteção de dados tem uma heurística na qual ele tenta deduzir um local de armazenamento de chave apropriado e criptografia no mecanismo de rest automaticamente. Isso também é configurável pelo desenvolvedor do aplicativo. Os documentos a seguir discutem as implementações de caixa de entrada desses mecanismos:

* [Na caixa de provedores de armazenamento de chaves](key-storage-providers.md#data-protection-implementation-key-storage-providers)

* [Na caixa de criptografia de chave nos provedores de rest](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)
