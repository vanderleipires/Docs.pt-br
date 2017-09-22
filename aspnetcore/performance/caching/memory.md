---
title: "Cache de memória no núcleo do ASP.NET"
author: rick-anderson
description: "Mostra como armazenar em cache os dados na memória no núcleo do ASP.NET."
keywords: "ASP.NET Core, o desempenho do cache, na memória,"
ms.author: riande
manager: wpickett
ms.date: 12/14/2016
ms.topic: article
ms.assetid: 819511cf-d33e-410a-b5a9-bef7fa64d2f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/memory
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b5dca6a81a66ce2a8771f1a16e63834d6504d8b6
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-in-memory-caching-in-aspnet-core"></a><span data-ttu-id="ac956-104">Introdução ao cache na memória no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ac956-104">Introduction to in-memory caching in ASP.NET Core</span></span>

<span data-ttu-id="ac956-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ac956-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

[<span data-ttu-id="ac956-106">Exibir ou baixar o código de exemplo</span><span class="sxs-lookup"><span data-stu-id="ac956-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample)

## <a name="caching-basics"></a><span data-ttu-id="ac956-107">Noções básicas de cache</span><span class="sxs-lookup"><span data-stu-id="ac956-107">Caching basics</span></span>

<span data-ttu-id="ac956-108">O cache pode melhorar significativamente o desempenho e a escalabilidade de um aplicativo, reduzindo o trabalho necessário para gerar o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="ac956-108">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="ac956-109">Armazenamento em cache funciona melhor com dados que raramente são alterados.</span><span class="sxs-lookup"><span data-stu-id="ac956-109">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="ac956-110">O cache faz uma cópia dos dados que podem ser retornadas muito mais rápido do que a partir da origem.</span><span class="sxs-lookup"><span data-stu-id="ac956-110">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="ac956-111">Você deve escrever e testar seu aplicativo para nunca dependem dos dados armazenados em cache.</span><span class="sxs-lookup"><span data-stu-id="ac956-111">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="ac956-112">ASP.NET Core dá suporte a vários caches diferentes.</span><span class="sxs-lookup"><span data-stu-id="ac956-112">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="ac956-113">O cache mais simples se baseia o [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), que representa um cache armazenado na memória do servidor web.</span><span class="sxs-lookup"><span data-stu-id="ac956-113">The simplest cache is based on the [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="ac956-114">Aplicativos que são executados em um farm de servidores devem garantir que as sessões estão aderência ao usar o cache na memória.</span><span class="sxs-lookup"><span data-stu-id="ac956-114">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="ac956-115">Sessões Autoadesivas Certifique-se de que as solicitações subsequentes de um cliente todos os vão para o mesmo servidor.</span><span class="sxs-lookup"><span data-stu-id="ac956-115">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="ac956-116">Por exemplo, uso de aplicativos da Web do Azure [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) para rotear todas as solicitações subsequentes para o mesmo servidor.</span><span class="sxs-lookup"><span data-stu-id="ac956-116">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="ac956-117">Sessões não temporária em um farm da web exigem um [cache distribuído](distributed.md) para evitar problemas de consistência de cache.</span><span class="sxs-lookup"><span data-stu-id="ac956-117">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="ac956-118">Para alguns aplicativos, um cache distribuído pode dar suporte a mais alta de expansão de um cache na memória.</span><span class="sxs-lookup"><span data-stu-id="ac956-118">For some apps, a distributed cache can support higher scale out than an in-memory cache.</span></span> <span data-ttu-id="ac956-119">Usar um cache distribuído libera a memória de cache para um processo externo.</span><span class="sxs-lookup"><span data-stu-id="ac956-119">Using a distributed cache offloads the cache memory to an external process.</span></span> 

<span data-ttu-id="ac956-120">O `IMemoryCache` cache irá remover entradas de cache sob pressão de memória, a menos que o [cache prioridade](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) é definido como `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="ac956-120">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="ac956-121">Você pode definir o `CacheItemPriority` para ajustar a prioridade de cache remove itens sob pressão de memória.</span><span class="sxs-lookup"><span data-stu-id="ac956-121">You can set the `CacheItemPriority` to adjust the priority the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="ac956-122">O cache de memória pode armazenar qualquer objeto. a interface de cache distribuído é limitada a `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="ac956-122">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="ac956-123">Usando IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="ac956-123">Using IMemoryCache</span></span>

<span data-ttu-id="ac956-124">O cache de memória é um *service* que é referenciado em seu aplicativo usando [injeção de dependência](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ac956-124">In-memory caching is a *service* that is referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="ac956-125">Chamar `AddMemoryCache` em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ac956-125">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[Main](memory/sample/WebCache/Startup.cs?highlight=8)] 

<span data-ttu-id="ac956-126">Solicitar a `IMemoryCache` instância no construtor:</span><span class="sxs-lookup"><span data-stu-id="ac956-126">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-)] 

<span data-ttu-id="ac956-127">`IMemoryCache`requer o pacote do NuGet "Microsoft.Extensions.Caching.Memory".</span><span class="sxs-lookup"><span data-stu-id="ac956-127">`IMemoryCache` requires NuGet package "Microsoft.Extensions.Caching.Memory".</span></span>

<span data-ttu-id="ac956-128">O código a seguir usa [TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) para verificar se a hora atual está no cache.</span><span class="sxs-lookup"><span data-stu-id="ac956-128">The following code uses [TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if the current time is in the cache.</span></span> <span data-ttu-id="ac956-129">Se o item não é armazenado em cache, uma nova entrada é criada e adicionada ao cache com [definir](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_).</span><span class="sxs-lookup"><span data-stu-id="ac956-129">If the item is not cached, a new entry is created and added to the cache with [Set](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_).</span></span>

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="ac956-130">A hora atual e o tempo em cache é exibida:</span><span class="sxs-lookup"><span data-stu-id="ac956-130">The current time and the cached time is displayed:</span></span>

[!code-html[Main](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="ac956-131">Cache `DateTime` valor permanecerá no cache enquanto houver solicitações dentro do tempo limite (e nenhuma remoção devido à pressão de memória).</span><span class="sxs-lookup"><span data-stu-id="ac956-131">The cached `DateTime` value will remain in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="ac956-132">A imagem abaixo mostra a hora atual e uma hora mais antiga recuperado do cache:</span><span class="sxs-lookup"><span data-stu-id="ac956-132">The image below shows the current time and an older time retrieved from cache:</span></span>

![Exibição de índice com duas vezes diferentes exibido](memory/_static/time.png)

<span data-ttu-id="ac956-134">O código a seguir usa [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) e [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) os dados em cache.</span><span class="sxs-lookup"><span data-stu-id="ac956-134">The following code uses [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="ac956-135">O código a seguir chama [obter](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) para buscar o tempo em cache:</span><span class="sxs-lookup"><span data-stu-id="ac956-135">The following code calls [Get](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="ac956-136">Consulte [IMemoryCache métodos](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) e [CacheExtensions métodos](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) para obter uma descrição dos métodos de cache.</span><span class="sxs-lookup"><span data-stu-id="ac956-136">See [IMemoryCache methods](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="using-memorycacheentryoptions"></a><span data-ttu-id="ac956-137">Usando MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="ac956-137">Using MemoryCacheEntryOptions</span></span>

<span data-ttu-id="ac956-138">O exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="ac956-138">The following sample:</span></span>

- <span data-ttu-id="ac956-139">Define o tempo de expiração absoluta.</span><span class="sxs-lookup"><span data-stu-id="ac956-139">Sets the absolute expiration time.</span></span> <span data-ttu-id="ac956-140">Isso é o tempo máximo que a entrada pode ser armazenado em cache e impede que o item se tornam muito desatualizados quando a expiração deslizante é renovada continuamente.</span><span class="sxs-lookup"><span data-stu-id="ac956-140">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="ac956-141">Define uma hora de expiração deslizante.</span><span class="sxs-lookup"><span data-stu-id="ac956-141">Sets a sliding expiration time.</span></span> <span data-ttu-id="ac956-142">Solicitações que acessam esse item em cache irá redefinir o relógio de expiração deslizante.</span><span class="sxs-lookup"><span data-stu-id="ac956-142">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="ac956-143">Define a prioridade de cache para `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="ac956-143">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span> 
- <span data-ttu-id="ac956-144">Define uma [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) que será chamado depois que a entrada é removida do cache.</span><span class="sxs-lookup"><span data-stu-id="ac956-144">Sets a [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="ac956-145">O retorno de chamada é executado em um thread diferente do código que remove o item do cache.</span><span class="sxs-lookup"><span data-stu-id="ac956-145">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a><span data-ttu-id="ac956-146">Dependências de cache</span><span class="sxs-lookup"><span data-stu-id="ac956-146">Cache dependencies</span></span>

<span data-ttu-id="ac956-147">O exemplo a seguir mostra como expirar uma entrada de cache, se uma entrada dependente expirar.</span><span class="sxs-lookup"><span data-stu-id="ac956-147">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="ac956-148">Um `CancellationChangeToken` é adicionado ao item em cache.</span><span class="sxs-lookup"><span data-stu-id="ac956-148">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="ac956-149">Quando `Cancel` é chamado de `CancellationTokenSource`, ambas as entradas de cache são removidas.</span><span class="sxs-lookup"><span data-stu-id="ac956-149">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span> 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="ac956-150">Usando um `CancellationTokenSource` permite que várias entradas de cache a ser removido como um grupo.</span><span class="sxs-lookup"><span data-stu-id="ac956-150">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="ac956-151">Com o `using` padrão no código acima, as entradas de cache criado dentro de `using` bloco herdará as configurações de expiração e gatilhos.</span><span class="sxs-lookup"><span data-stu-id="ac956-151">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="ac956-152">Observações adicionais</span><span class="sxs-lookup"><span data-stu-id="ac956-152">Additional notes</span></span>

- <span data-ttu-id="ac956-153">Ao usar um retorno de chamada para preencher novamente um item de cache:</span><span class="sxs-lookup"><span data-stu-id="ac956-153">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="ac956-154">Várias solicitações podem encontrar o valor de chave em cache vazio porque o retorno de chamada não foi concluída.</span><span class="sxs-lookup"><span data-stu-id="ac956-154">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span> 
  - <span data-ttu-id="ac956-155">Isso pode resultar em vários threads popular novamente o item em cache.</span><span class="sxs-lookup"><span data-stu-id="ac956-155">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="ac956-156">Quando uma entrada de cache é usada para criar outra, o filho copia a entrada de pai tokens de expiração e as configurações de expiração do tempo.</span><span class="sxs-lookup"><span data-stu-id="ac956-156">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="ac956-157">O filho não está expirada pela remoção manual ou atualização da entrada do pai.</span><span class="sxs-lookup"><span data-stu-id="ac956-157">The child is not expired by manual removal or updating of the parent entry.</span></span>

### <a name="other-resources"></a><span data-ttu-id="ac956-158">Outros recursos</span><span class="sxs-lookup"><span data-stu-id="ac956-158">Other Resources</span></span>

* [<span data-ttu-id="ac956-159">Trabalhando com um cache distribuído</span><span class="sxs-lookup"><span data-stu-id="ac956-159">Working with a Distributed Cache</span></span>](distributed.md)
* [<span data-ttu-id="ac956-160">Middleware de cache de resposta</span><span class="sxs-lookup"><span data-stu-id="ac956-160">Response caching middleware</span></span>](middleware.md)
