---
title: Distributed auxiliar de marca de Cache | Microsoft Docs
author: pkellner
description: Mostra como trabalhar com o auxiliar de marca de Cache
keywords: "ASP.NET Core, auxiliar de marcação"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 462c3775677924fc7b9b715cd6de75fe53ada89e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="distributed-cache-tag-helper"></a>Auxiliar de marca de Cache distribuído

Por [Peter Kellner](http://peterkellner.net) 


O auxiliar de marca de Cache distribuído fornece a capacidade de melhorar drasticamente o desempenho do seu aplicativo ASP.NET Core armazenando em cache o conteúdo a uma fonte de cache distribuído.

O auxiliar de marca de Cache distribuído herda da classe base mesmo como o auxiliar de marca de Cache.  Todos os atributos associados com o auxiliar de marca de Cache também funcionará no auxiliar de marca distribuídas.


O auxiliar de marca de Cache distribuído segue o **princípio de dependências explícitas** conhecido como **injeção de construtor**.  Especificamente, o `IDistributedCache` contêiner de interface é passado para o construtor de Distributed Cache marca do auxiliar.  Se nenhuma implementação concreta específica de `IDistributedCache` foi criado no `ConfigureServices`, geralmente encontrado em startup.cs, em seguida, o auxiliar de marca de Cache distribuído usará o mesmo provedor de memória para armazenar dados em cache como o auxiliar de marca de Cache basic.

## <a name="distributed-cache-tag-helper-attributes"></a>Distributed atributos de auxiliar de marca de Cache

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>habilitado expira em expira após expirar deslizantes variar por cabeçalho variam por consulta variam por rota variam por cookie variam por usuário variam por prioridade

Consulte auxiliar de marca de Cache de definições. Auxiliar de marca de Cache distribuído herda a mesma classe auxiliar de marca de Cache para que todos esses atributos são comuns do auxiliar de marca de Cache.

- - -

### <a name="name-required"></a>nome (obrigatório)

| Tipo de atributo    | Valor de exemplo     |
|----------------   |----------------   |
| cadeia de caracteres    | "my-distributed-cache-unique-key-101"     |

Obrigatório `name` atributo é usado como uma chave de cache armazenado para cada instância de um auxiliar de marca de Cache distribuído.  Diferentemente de básica auxiliar de marca de Cache que atribui uma chave para cada instância do auxiliar de marca de Cache com base no nome da página Razor e local do auxiliar de marca na página de razor, o auxiliar de marca de Cache distribuído somente bases chave do atributo`name`

Exemplo de uso:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Distributed implementações de IDistributedCache de auxiliar de marca de Cache

Há duas implementações de `IDistributedCache` interna do ASP.NET Core.  Uma é baseada em **do Sql Server** e a outra é baseada em **Redis**. Os detalhes dessas implementações podem ser encontrados no recurso referenciado abaixo nomeada "Trabalhando com um cache distribuído". Ambas as implementações envolvem a definição de uma instância de `IDistributedCache` no ASP.NET Core **startup.cs**.

Nenhum atributos de marca especificamente associadas ao uso de qualquer implementação específica de `IDistributedCache`.



- - -



## <a name="additional-resources"></a>Recursos adicionais

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
