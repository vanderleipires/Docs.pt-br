---
title: Cache de memória no núcleo do ASP.NET
author: rick-anderson
description: Saiba como armazenar em cache os dados na memória do ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/14/2016
uid: performance/caching/memory
ms.openlocfilehash: 5a7085269e255ae233a0e7eeb860a04b2c6bede3
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077580"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="ef263-103">Cache de memória no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ef263-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="ef263-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ef263-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ef263-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ef263-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="ef263-106">Noções básicas de cache</span><span class="sxs-lookup"><span data-stu-id="ef263-106">Caching basics</span></span>

<span data-ttu-id="ef263-107">O cache pode melhorar significativamente o desempenho e a escalabilidade de um aplicativo, reduzindo o trabalho necessário para gerar o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="ef263-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="ef263-108">O armazenamento em cache funciona melhor com dados que raramente são alterados.</span><span class="sxs-lookup"><span data-stu-id="ef263-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="ef263-109">O cache faz uma cópia dos dados que pode ser retornada muito mais rapidamente do que a partir da origem.</span><span class="sxs-lookup"><span data-stu-id="ef263-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="ef263-110">Você deve escrever e testar seu aplicativo para que ele nunca dependa de dados armazenados em cache.</span><span class="sxs-lookup"><span data-stu-id="ef263-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="ef263-111">O ASP.NET Core dá suporte a vários caches diferentes.</span><span class="sxs-lookup"><span data-stu-id="ef263-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="ef263-112">O cache mais simples se baseia o [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), que representa um cache armazenado na memória do servidor web.</span><span class="sxs-lookup"><span data-stu-id="ef263-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="ef263-113">Aplicativos que são executados em um farm de servidores devem garantir que as sessões sejam aderentes ao usar o cache na memória.</span><span class="sxs-lookup"><span data-stu-id="ef263-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="ef263-114">Sessões aderentes garantem que todas as solicitações subsequentes de um cliente vão para o mesmo servidor.</span><span class="sxs-lookup"><span data-stu-id="ef263-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="ef263-115">Por exemplo, uso de aplicativos da Web do Azure [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) para rotear todas as solicitações subsequentes para o mesmo servidor.</span><span class="sxs-lookup"><span data-stu-id="ef263-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="ef263-116">Sessões não temporária em um farm da web exigem um [cache distribuído](distributed.md) para evitar problemas de consistência de cache.</span><span class="sxs-lookup"><span data-stu-id="ef263-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="ef263-117">Para alguns aplicativos, um cache distribuído pode dar suporte a mais alta de expansão de um cache na memória.</span><span class="sxs-lookup"><span data-stu-id="ef263-117">For some apps, a distributed cache can support higher scale out than an in-memory cache.</span></span> <span data-ttu-id="ef263-118">Usar um cache distribuído libera a memória de cache para um processo externo.</span><span class="sxs-lookup"><span data-stu-id="ef263-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="ef263-119">O cache `IMemoryCache` removerá entradas de cache sob pressão de memória, a menos que a [prioridade de cache](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) seja definida como `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="ef263-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="ef263-120">Você pode definir o `CacheItemPriority` para ajustar a prioridade com que o cache remove itens sob pressão de memória.</span><span class="sxs-lookup"><span data-stu-id="ef263-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="ef263-121">O cache de memória pode armazenar qualquer objeto, enquanto a interface de cache distribuída é limitada a`byte[]`.</span><span class="sxs-lookup"><span data-stu-id="ef263-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="ef263-122">Usando IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="ef263-122">Using IMemoryCache</span></span>

<span data-ttu-id="ef263-123">O cache de memória é um *service* que é referenciado em seu aplicativo usando [injeção de dependência](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ef263-123">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="ef263-124">Chamar `AddMemoryCache` em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ef263-124">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="ef263-125">Solicitar a `IMemoryCache` instância no construtor:</span><span class="sxs-lookup"><span data-stu-id="ef263-125">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ef263-126">`IMemoryCache` requer o pacote NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="ef263-126">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ef263-127">`IMemoryCache` requer o pacote NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), que está disponível na [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="ef263-127">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="ef263-128">`IMemoryCache` requer o pacote NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), que está disponível na [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ef263-128">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="ef263-129">O código a seguir usa [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) para verificar se um horário está no cache.</span><span class="sxs-lookup"><span data-stu-id="ef263-129">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="ef263-130">Se um tempo não é armazenado em cache, uma nova entrada é criada e adicionada ao cache com [definir](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="ef263-130">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="ef263-131">A hora atual e o tempo em cache são exibidos:</span><span class="sxs-lookup"><span data-stu-id="ef263-131">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="ef263-132">O valor `DateTime` em cache permanecerá no cache enquanto houver solicitações dentro do tempo limite (e nenhuma remoção devido à pressão de memória).</span><span class="sxs-lookup"><span data-stu-id="ef263-132">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="ef263-133">A imagem abaixo mostra a hora atual e uma hora mais antiga recuperada do cache:</span><span class="sxs-lookup"><span data-stu-id="ef263-133">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Exibição de índice com duas vezes diferentes exibido](memory/_static/time.png)

<span data-ttu-id="ef263-135">O código a seguir usa [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) e [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) para fazer o cache dos dados. </span><span class="sxs-lookup"><span data-stu-id="ef263-135">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="ef263-136">O código a seguir chama o método [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) para buscar a hora em cache:</span><span class="sxs-lookup"><span data-stu-id="ef263-136">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="ef263-137">Consulte [IMemoryCache métodos](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) e [CacheExtensions métodos](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) para obter uma descrição dos métodos de cache.</span><span class="sxs-lookup"><span data-stu-id="ef263-137">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="using-memorycacheentryoptions"></a><span data-ttu-id="ef263-138">Usando MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="ef263-138">Using MemoryCacheEntryOptions</span></span>

<span data-ttu-id="ef263-139">O exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="ef263-139">The following sample:</span></span>

- <span data-ttu-id="ef263-140">Define o tempo de expiração absoluta.</span><span class="sxs-lookup"><span data-stu-id="ef263-140">Sets the absolute expiration time.</span></span> <span data-ttu-id="ef263-141">Isso é o tempo máximo que a entrada pode ser armazenado em cache e impede que o item se tornam muito desatualizados quando a expiração deslizante é renovada continuamente.</span><span class="sxs-lookup"><span data-stu-id="ef263-141">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="ef263-142">Define uma hora de expiração deslizante.</span><span class="sxs-lookup"><span data-stu-id="ef263-142">Sets a sliding expiration time.</span></span> <span data-ttu-id="ef263-143">Solicitações que acessam esse item em cache irá redefinir o relógio de expiração deslizante.</span><span class="sxs-lookup"><span data-stu-id="ef263-143">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="ef263-144">Define a prioridade de cache para `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="ef263-144">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="ef263-145">Define uma [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) que será chamado depois que a entrada é removida do cache.</span><span class="sxs-lookup"><span data-stu-id="ef263-145">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="ef263-146">O retorno de chamada é executado em um thread diferente do código que remove o item do cache.</span><span class="sxs-lookup"><span data-stu-id="ef263-146">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="cache-dependencies"></a><span data-ttu-id="ef263-147">Dependências de cache</span><span class="sxs-lookup"><span data-stu-id="ef263-147">Cache dependencies</span></span>

<span data-ttu-id="ef263-148">O exemplo a seguir mostra como expirar uma entrada de cache, se uma entrada dependente expirar.</span><span class="sxs-lookup"><span data-stu-id="ef263-148">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="ef263-149">Um `CancellationChangeToken` é adicionado ao item em cache.</span><span class="sxs-lookup"><span data-stu-id="ef263-149">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="ef263-150">Quando `Cancel` é chamado de `CancellationTokenSource`, ambas as entradas de cache são removidas.</span><span class="sxs-lookup"><span data-stu-id="ef263-150">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="ef263-151">Usando um `CancellationTokenSource` permite que várias entradas de cache a ser removido como um grupo.</span><span class="sxs-lookup"><span data-stu-id="ef263-151">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="ef263-152">Com o `using` padrão no código acima, as entradas de cache criado dentro de `using` bloco herdará as configurações de expiração e gatilhos.</span><span class="sxs-lookup"><span data-stu-id="ef263-152">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="ef263-153">Observações adicionais</span><span class="sxs-lookup"><span data-stu-id="ef263-153">Additional notes</span></span>

- <span data-ttu-id="ef263-154">Ao usar um retorno de chamada para preencher novamente um item de cache:</span><span class="sxs-lookup"><span data-stu-id="ef263-154">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="ef263-155">Várias solicitações podem encontrar o valor de chave em cache vazio porque o retorno de chamada não foi concluída.</span><span class="sxs-lookup"><span data-stu-id="ef263-155">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="ef263-156">Isso pode resultar em vários threads popular novamente o item em cache.</span><span class="sxs-lookup"><span data-stu-id="ef263-156">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="ef263-157">Quando uma entrada de cache é usada para criar outra, o filho copia a entrada de pai tokens de expiração e as configurações de expiração do tempo.</span><span class="sxs-lookup"><span data-stu-id="ef263-157">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="ef263-158">O filho não está expirada pela remoção manual ou atualização da entrada do pai.</span><span class="sxs-lookup"><span data-stu-id="ef263-158">The child isn't expired by manual removal or updating of the parent entry.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef263-159">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="ef263-159">Additional resources</span></span>

* [<span data-ttu-id="ef263-160">Trabalhar com um cache distribuído</span><span class="sxs-lookup"><span data-stu-id="ef263-160">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="ef263-161">Detectar alterações com tokens de alteração</span><span class="sxs-lookup"><span data-stu-id="ef263-161">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="ef263-162">Cache de resposta</span><span class="sxs-lookup"><span data-stu-id="ef263-162">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="ef263-163">Middleware de Cache de Resposta</span><span class="sxs-lookup"><span data-stu-id="ef263-163">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="ef263-164">Auxiliar de marca de cache</span><span class="sxs-lookup"><span data-stu-id="ef263-164">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="ef263-165">Auxiliar de marca de cache distribuído</span><span class="sxs-lookup"><span data-stu-id="ef263-165">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
