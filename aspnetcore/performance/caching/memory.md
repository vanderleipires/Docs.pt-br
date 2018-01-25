---
title: "Cache de memória no núcleo do ASP.NET"
author: rick-anderson
description: "Saiba como armazenar em cache os dados na memória no núcleo do ASP.NET."
ms.author: riande
manager: wpickett
ms.date: 12/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/memory
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 12d3982ce1abf74a575dae5362b037bd4ecc3d0f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="in-memory-caching-in-aspnet-core"></a>Cache de memória no núcleo do ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), e [Steve Smith](https://ardalis.com/)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Noções básicas de cache

O cache pode melhorar significativamente o desempenho e a escalabilidade de um aplicativo, reduzindo o trabalho necessário para gerar o conteúdo. Armazenamento em cache funciona melhor com dados que raramente são alterados. O cache faz uma cópia dos dados que podem ser retornadas muito mais rápido do que a partir da origem. Você deve escrever e testar seu aplicativo para nunca dependem dos dados armazenados em cache.

ASP.NET Core dá suporte a vários caches diferentes. O cache mais simples se baseia o [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), que representa um cache armazenado na memória do servidor web. Aplicativos que são executados em um farm de servidores devem garantir que as sessões estão aderência ao usar o cache na memória. Sessões Autoadesivas Certifique-se de que as solicitações subsequentes de um cliente todos os vão para o mesmo servidor. Por exemplo, uso de aplicativos da Web do Azure [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) para rotear todas as solicitações subsequentes para o mesmo servidor.

Sessões não temporária em um farm da web exigem um [cache distribuído](distributed.md) para evitar problemas de consistência de cache. Para alguns aplicativos, um cache distribuído pode dar suporte a mais alta de expansão de um cache na memória. Usar um cache distribuído libera a memória de cache para um processo externo. 

O `IMemoryCache` cache irá remover entradas de cache sob pressão de memória, a menos que o [cache prioridade](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) é definido como `CacheItemPriority.NeverRemove`. Você pode definir o `CacheItemPriority` para ajustar a prioridade de cache remove itens sob pressão de memória.

O cache de memória pode armazenar qualquer objeto. a interface de cache distribuído é limitada a `byte[]`.

## <a name="using-imemorycache"></a>Usando IMemoryCache

O cache de memória é um *service* que é referenciado em seu aplicativo usando [injeção de dependência](../../fundamentals/dependency-injection.md). Chamar `AddMemoryCache` em `ConfigureServices`:

[!code-csharp[Main](memory/sample/WebCache/Startup.cs?highlight=8)] 

Solicitar a `IMemoryCache` instância no construtor:

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-)] 

`IMemoryCache`requer o pacote do NuGet "Microsoft.Extensions.Caching.Memory".

O código a seguir usa [TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) para verificar se a hora atual está no cache. Se o item não está em cache, uma nova entrada é criada e adicionada ao cache com [definir](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_).

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

A hora atual e o tempo em cache é exibida:

[!code-html[Main](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Cache `DateTime` valor permanecerá no cache enquanto houver solicitações dentro do tempo limite (e nenhuma remoção devido à pressão de memória). A imagem abaixo mostra a hora atual e uma hora mais antiga recuperado do cache:

![Exibição de índice com duas vezes diferentes exibido](memory/_static/time.png)

O código a seguir usa [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) e [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) os dados em cache. 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

O código a seguir chama [obter](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) para buscar o tempo em cache:

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

Consulte [IMemoryCache métodos](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) e [CacheExtensions métodos](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) para obter uma descrição dos métodos de cache.

## <a name="using-memorycacheentryoptions"></a>Using MemoryCacheEntryOptions

O exemplo a seguir:

- Define o tempo de expiração absoluta. Isso é o tempo máximo que a entrada pode ser armazenado em cache e impede que o item se tornam muito desatualizados quando a expiração deslizante é renovada continuamente.
- Define uma hora de expiração deslizante. Solicitações que acessam esse item em cache irá redefinir o relógio de expiração deslizante.
- Define a prioridade de cache para `CacheItemPriority.NeverRemove`. 
- Define uma [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) que será chamado depois que a entrada é removida do cache. O retorno de chamada é executado em um thread diferente do código que remove o item do cache.

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a>Dependências de cache

O exemplo a seguir mostra como expirar uma entrada de cache, se uma entrada dependente expirar. Um `CancellationChangeToken` é adicionado ao item em cache. Quando `Cancel` é chamado de `CancellationTokenSource`, ambas as entradas de cache são removidas. 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Usando um `CancellationTokenSource` permite que várias entradas de cache a ser removido como um grupo. Com o `using` padrão no código acima, as entradas de cache criado dentro de `using` bloco herdará as configurações de expiração e gatilhos.

## <a name="additional-notes"></a>Observações adicionais

- Ao usar um retorno de chamada para preencher novamente um item de cache:

  - Várias solicitações podem encontrar o valor de chave em cache vazio porque o retorno de chamada não foi concluída. 
  - Isso pode resultar em vários threads popular novamente o item em cache.

- Quando uma entrada de cache é usada para criar outra, o filho copia a entrada de pai tokens de expiração e as configurações de expiração do tempo. O filho não está expirada pela remoção manual ou atualização da entrada do pai.

## <a name="additional-resources"></a>Recursos adicionais

* [Trabalhando com um cache distribuído](xref:performance/caching/distributed)
* [Detectar alterações com tokens de alteração](xref:fundamentals/primitives/change-tokens)
* [Cache de resposta](xref:performance/caching/response)
* [Middleware de Cache de Resposta](xref:performance/caching/middleware)
* [Auxiliar de marca de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Auxiliar de marca de cache distribuído](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
