---
title: "Auxiliar de Marca de Cache Distribuído no ASP.NET Core"
author: pkellner
description: Mostra como trabalhar com o Auxiliar de Marca de Cache
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 710477732b865e2e3821102d34545bbd4e0a5919
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="distributed-cache-tag-helper"></a>Auxiliar de Marca de Cache Distribuído

Por [Peter Kellner](http://peterkellner.net) 


O Auxiliar de Marca de Cache Distribuído fornece a capacidade de melhorar consideravelmente o desempenho do aplicativo ASP.NET Core armazenando seu conteúdo em cache em uma fonte de cache distribuído.

O Auxiliar de Marca de Cache Distribuído herda da mesma classe base do Auxiliar de Marca de Cache.  Todos os atributos associados ao Auxiliar de Marca de Cache também funcionarão no Auxiliar de Marca Distribuído.


O Auxiliar de Marca de Cache Distribuído segue o **Princípio de Dependências Explícitas**, conhecido como **Injeção de Construtor**.  Especificamente, o contêiner de interface `IDistributedCache` é passado para o construtor do Auxiliar de Marca de Cache Distribuído.  Se nenhuma implementação concreta específica de `IDistributedCache` tiver sido criada em `ConfigureServices`, geralmente encontrada em startup.cs, o Auxiliar de Marca de Cache Distribuído usará o mesmo provedor em memória para armazenar os dados em cache que o Auxiliar de Marca de Cache básico.

## <a name="distributed-cache-tag-helper-attributes"></a>Atributos do auxiliar de marca de cache distribuído

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>prioridade expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by habilitada

Consulte Auxiliar de Marca de Cache para obter as definições. O Auxiliar de Marca de Cache Distribuído herda da mesma classe do Auxiliar de Marca de Cache. Portanto, todos esses atributos são comuns ao Auxiliar de Marca de Cache.

- - -

### <a name="name-required"></a>nome (obrigatório)

| Tipo de atributo    | Valor de exemplo     |
|----------------   |----------------   |
| cadeia de caracteres    | "my-distributed-cache-unique-key-101"     |

O atributo `name` obrigatório é usado como uma chave para esse cache armazenado de cada instância de um Auxiliar de Marca de Cache Distribuído.  Ao contrário do Auxiliar de Marca de Cache básico que atribui uma chave a cada instância do Auxiliar de Marca de Cache com base no nome da página do Razor e na localização do auxiliar de marca na página do Razor, o Auxiliar de Marca de Cache Distribuído somente localiza suas chaves no atributo `name`

Exemplo de uso:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Implementações de IDistributedCache do Auxiliar de Marca de Cache Distribuído

Há duas implementações de `IDistributedCache` internas do ASP.NET Core.  Uma é baseada no **SQL Server** e a outra, no **Redis**. Os detalhes dessas implementações podem ser encontrados no recurso referenciado abaixo intitulado "Trabalhando com um cache distribuído". Ambas as implementações envolvem a definição de uma instância de `IDistributedCache` no **startup.cs** do ASP.NET Core.

Não há nenhum atributo de marca especificamente associado ao uso de uma implementação específica de `IDistributedCache`.



- - -



## <a name="additional-resources"></a>Recursos adicionais

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
