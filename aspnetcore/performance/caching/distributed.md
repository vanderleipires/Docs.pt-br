---
title: Trabalhar com um cache distribuído no ASP.NET Core
author: ardalis
description: Saiba como usar o ASP.NET Core distribuído cache para melhorar o desempenho do aplicativo e a escalabilidade, especialmente em um ambiente de farm de servidor ou de nuvem.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 861664fcad576c11abe052837b72367eb2b9479a
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095675"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="37d24-103">Trabalhar com um cache distribuído no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37d24-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="37d24-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="37d24-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="37d24-105">Os caches distribuídos podem melhorar o desempenho e escalabilidade de aplicativos do ASP.NET Core, especialmente quando hospedado na nuvem ou um farm de servidores.</span><span class="sxs-lookup"><span data-stu-id="37d24-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in the cloud or a server farm.</span></span>

<span data-ttu-id="37d24-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37d24-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="37d24-107">O que é um cache distribuído</span><span class="sxs-lookup"><span data-stu-id="37d24-107">What is a distributed cache</span></span>

<span data-ttu-id="37d24-108">Um cache distribuído é compartilhado por vários servidores de aplicativo (consulte [Noções básicas de Cache](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="37d24-108">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="37d24-109">As informações em cache não são armazenadas na memória dos servidores web individual e os dados armazenados em cache estão disponíveis em todos os servidores do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="37d24-109">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="37d24-110">Isso fornece várias vantagens:</span><span class="sxs-lookup"><span data-stu-id="37d24-110">This provides several advantages:</span></span>

1. <span data-ttu-id="37d24-111">Dados armazenados em cache são coerentes em todos os servidores web.</span><span class="sxs-lookup"><span data-stu-id="37d24-111">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="37d24-112">Os usuários não veem resultados diferentes dependendo de qual web server lida com sua solicitação</span><span class="sxs-lookup"><span data-stu-id="37d24-112">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="37d24-113">Dados armazenados em cache sobrevive reinicializações do servidor web e implantações.</span><span class="sxs-lookup"><span data-stu-id="37d24-113">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="37d24-114">Servidores web individuais podem ser removidos ou adicionados sem afetar o cache.</span><span class="sxs-lookup"><span data-stu-id="37d24-114">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="37d24-115">O armazenamento de dados de origem tem menos solicitações feitas a ele (de com vários caches na memória ou não armazenar em cache em todos os).</span><span class="sxs-lookup"><span data-stu-id="37d24-115">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="37d24-116">Se usar um Cache distribuído do SQL Server, algumas dessas vantagens são somente verdadeiras se uma instância de banco de dados separado é usada para o cache do que para dados de origem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="37d24-116">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="37d24-117">Como qualquer cache, um cache distribuído pode melhorar consideravelmente capacidade de resposta do aplicativo, como normalmente os dados podem ser recuperados do cache muito mais rápido do que de um banco de dados relacional (ou serviço da web).</span><span class="sxs-lookup"><span data-stu-id="37d24-117">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="37d24-118">Configuração de cache é específico da implementação.</span><span class="sxs-lookup"><span data-stu-id="37d24-118">Cache configuration is implementation specific.</span></span> <span data-ttu-id="37d24-119">Este artigo descreve como configurar ambos Redis e distribuídas do SQL Server armazena em cache.</span><span class="sxs-lookup"><span data-stu-id="37d24-119">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="37d24-120">Independentemente de qual implementação for selecionada, o aplicativo interage com o cache usando um comum `IDistributedCache` interface.</span><span class="sxs-lookup"><span data-stu-id="37d24-120">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="37d24-121">A Interface de IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="37d24-121">The IDistributedCache Interface</span></span>

<span data-ttu-id="37d24-122">O `IDistributedCache` interface inclui métodos síncronos e assíncronos.</span><span class="sxs-lookup"><span data-stu-id="37d24-122">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="37d24-123">A interface permite que os itens a serem adicionados, recuperados e removido da implementação de cache distribuído.</span><span class="sxs-lookup"><span data-stu-id="37d24-123">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="37d24-124">O `IDistributedCache` interface inclui os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="37d24-124">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="37d24-125">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="37d24-125">**Get, GetAsync**</span></span>

<span data-ttu-id="37d24-126">Usa uma chave de cadeia de caracteres e recupera um item em cache como um `byte[]` se encontrada no cache.</span><span class="sxs-lookup"><span data-stu-id="37d24-126">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="37d24-127">**Set, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="37d24-127">**Set, SetAsync**</span></span>

<span data-ttu-id="37d24-128">Adiciona um item (como `byte[]`) para o cache usando uma chave de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="37d24-128">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="37d24-129">**Atualização de RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="37d24-129">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="37d24-130">Atualiza um item no cache com base em sua chave, redefinindo seu tempo limite de expiração deslizante (se houver).</span><span class="sxs-lookup"><span data-stu-id="37d24-130">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="37d24-131">**Remove, RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="37d24-131">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="37d24-132">Remove uma entrada de cache com base em sua chave.</span><span class="sxs-lookup"><span data-stu-id="37d24-132">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="37d24-133">Para usar o `IDistributedCache` interface:</span><span class="sxs-lookup"><span data-stu-id="37d24-133">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="37d24-134">Adicione os pacotes NuGet necessários para seu arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="37d24-134">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="37d24-135">Configurar a implementação específica do `IDistributedCache` em seu `Startup` da classe `ConfigureServices` método e adicioná-lo para o contêiner existe.</span><span class="sxs-lookup"><span data-stu-id="37d24-135">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="37d24-136">Do aplicativo do [Middleware](xref:fundamentals/middleware/index) ou classes do controlador MVC, solicitar uma instância de `IDistributedCache` do construtor.</span><span class="sxs-lookup"><span data-stu-id="37d24-136">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="37d24-137">A instância será fornecida pelo [injeção de dependência](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="37d24-137">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="37d24-138">Não é necessário usar um tempo de vida Singleton ou com escopo para `IDistributedCache` instâncias (pelo menos para as implementações internas).</span><span class="sxs-lookup"><span data-stu-id="37d24-138">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="37d24-139">Você também pode criar uma instância sempre que talvez seja necessário um (em vez de usar [injeção de dependência](../../fundamentals/dependency-injection.md)), mas isso pode tornar seu código mais difícil de testar e viola a [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="37d24-139">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="37d24-140">O exemplo a seguir mostra como usar uma instância de `IDistributedCache` em um componente de middleware simples:</span><span class="sxs-lookup"><span data-stu-id="37d24-140">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="37d24-141">No código acima, o valor armazenado em cache é lido, mas nunca foi escrito.</span><span class="sxs-lookup"><span data-stu-id="37d24-141">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="37d24-142">Neste exemplo, o valor só é definido quando um servidor é inicializado e não é alterado.</span><span class="sxs-lookup"><span data-stu-id="37d24-142">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="37d24-143">Em um cenário de vários servidor, o servidor mais recente para iniciar substituirá os valores anteriores que foram definidos por outros servidores.</span><span class="sxs-lookup"><span data-stu-id="37d24-143">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="37d24-144">O `Get` e `Set` métodos usam o `byte[]` tipo.</span><span class="sxs-lookup"><span data-stu-id="37d24-144">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="37d24-145">Portanto, o valor de cadeia de caracteres deve ser convertido usando `Encoding.UTF8.GetString` (para `Get`) e `Encoding.UTF8.GetBytes` (para `Set`).</span><span class="sxs-lookup"><span data-stu-id="37d24-145">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="37d24-146">O código a seguir da *Startup.cs* mostra o valor que está sendo definido:</span><span class="sxs-lookup"><span data-stu-id="37d24-146">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="37d24-147">Uma vez que `IDistributedCache` está configurado na `ConfigureServices` método, ele está disponível para o `Configure` método como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="37d24-147">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="37d24-148">Adicioná-lo como um parâmetro permitirá que a instância configurada ser fornecido por meio da DI.</span><span class="sxs-lookup"><span data-stu-id="37d24-148">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="37d24-149">Usando um cache Redis distribuído</span><span class="sxs-lookup"><span data-stu-id="37d24-149">Using a Redis distributed cache</span></span>

<span data-ttu-id="37d24-150">[Redis](https://redis.io/) é um repositório de dados na memória do código-fonte aberto, que geralmente é usado como um cache distribuído.</span><span class="sxs-lookup"><span data-stu-id="37d24-150">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="37d24-151">Você pode usá-lo localmente, e você pode configurar uma [Cache Redis do Azure](https://azure.microsoft.com/services/cache/) para seus aplicativos ASP.NET Core hospedados no Azure.</span><span class="sxs-lookup"><span data-stu-id="37d24-151">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="37d24-152">Seu aplicativo ASP.NET Core configura a implementação de cache usando um `RedisDistributedCache` instância.</span><span class="sxs-lookup"><span data-stu-id="37d24-152">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="37d24-153">Você configura a implementação do Redis na `ConfigureServices` e acessá-lo no código do aplicativo, solicitando uma instância de `IDistributedCache` (consulte o código acima).</span><span class="sxs-lookup"><span data-stu-id="37d24-153">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="37d24-154">No código de exemplo, uma `RedisCache` implementação é usada quando o servidor está configurado para um `Staging` ambiente.</span><span class="sxs-lookup"><span data-stu-id="37d24-154">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="37d24-155">Portanto, o `ConfigureStagingServices` método configura o `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="37d24-155">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="37d24-156">Para instalar o Redis em seu computador local, instale o pacote do chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) e execute `redis-server` em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="37d24-156">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="37d24-157">Usando um SQL Server de cache distribuído</span><span class="sxs-lookup"><span data-stu-id="37d24-157">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="37d24-158">A implementação de SqlServerCache permite que o cache distribuído usar um banco de dados do SQL Server como seu armazenamento de backup.</span><span class="sxs-lookup"><span data-stu-id="37d24-158">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="37d24-159">Para criar o SQL Server a tabela que você pode usar a ferramenta de cache de sql, a ferramenta cria uma tabela com o nome e o esquema que você especificar.</span><span class="sxs-lookup"><span data-stu-id="37d24-159">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="37d24-160">Adicione `SqlConfig.Tools` para o `<ItemGroup>` elemento do arquivo de projeto e execute `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="37d24-160">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="37d24-161">Teste SqlConfig.Tools executando o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="37d24-161">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="37d24-162">SqlConfig.Tools exibe o uso e opções de Ajuda do comando.</span><span class="sxs-lookup"><span data-stu-id="37d24-162">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="37d24-163">Crie uma tabela no SQL Server executando o `sql-cache create` comando:</span><span class="sxs-lookup"><span data-stu-id="37d24-163">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="37d24-164">A tabela criada tem o esquema a seguir:</span><span class="sxs-lookup"><span data-stu-id="37d24-164">The created table has the following schema:</span></span>

![Tabela de Cache do SQL Server](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="37d24-166">Como todas as implementações de cache, seu aplicativo deve obter e definir valores de cache usando uma instância de `IDistributedCache`, e não um `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="37d24-166">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="37d24-167">O exemplo implementa `SqlServerCache` no ambiente de produção (portanto, ele é configurado em `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="37d24-167">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="37d24-168">O `ConnectionString` (e, opcionalmente, `SchemaName` e `TableName`) normalmente devem ser armazenados fora do controle do código-fonte (como UserSecrets), pois podem conter credenciais.</span><span class="sxs-lookup"><span data-stu-id="37d24-168">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="37d24-169">Recomendações</span><span class="sxs-lookup"><span data-stu-id="37d24-169">Recommendations</span></span>

<span data-ttu-id="37d24-170">Ao decidir qual implementação de `IDistributedCache` é ideal para seu aplicativo, escolha entre Redis e do SQL Server com base no ambiente e sua infraestrutura existente, seus requisitos de desempenho e experiência da sua equipe.</span><span class="sxs-lookup"><span data-stu-id="37d24-170">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="37d24-171">Se sua equipe for mais confortável com o Redis, é uma excelente opção.</span><span class="sxs-lookup"><span data-stu-id="37d24-171">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="37d24-172">Se sua equipe preferir do SQL Server, você pode ter certeza em que a implementação também.</span><span class="sxs-lookup"><span data-stu-id="37d24-172">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="37d24-173">Observe que uma solução tradicional de cache armazena dados na memória que permite a recuperação rápida de dados.</span><span class="sxs-lookup"><span data-stu-id="37d24-173">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="37d24-174">Você deve armazenar dados usados em um cache e armazenar todos os dados em um armazenamento persistente de back-end, como o SQL Server ou do armazenamento do Azure.</span><span class="sxs-lookup"><span data-stu-id="37d24-174">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="37d24-175">Cache redis é uma solução de cache que fornece alta taxa de transferência e baixa latência em comparação com o Cache de SQL.</span><span class="sxs-lookup"><span data-stu-id="37d24-175">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="37d24-176">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="37d24-176">Additional resources</span></span>

* [<span data-ttu-id="37d24-177">Redis Cache no Azure</span><span class="sxs-lookup"><span data-stu-id="37d24-177">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="37d24-178">Banco de dados SQL no Azure</span><span class="sxs-lookup"><span data-stu-id="37d24-178">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
