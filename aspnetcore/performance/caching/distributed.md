---
title: "Trabalhando com um Cache distribuído"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 870f082d-6d43-453d-b311-45f3aeb4d2c5
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/distributed
ms.openlocfilehash: 09a1a30de38b9eb40d4fa6a684a7d43ac3e0413c
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="working-with-a-distributed-cache"></a><span data-ttu-id="b4735-103">Trabalhando com um cache distribuído</span><span class="sxs-lookup"><span data-stu-id="b4735-103">Working with a distributed cache</span></span>

<span data-ttu-id="b4735-104">Por [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="b4735-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="b4735-105">Os caches distribuídos podem melhorar o desempenho e escalabilidade de aplicativos do ASP.NET Core, especialmente quando hospedados em um ambiente de farm de servidor ou de nuvem.</span><span class="sxs-lookup"><span data-stu-id="b4735-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="b4735-106">Este artigo explica como trabalhar com implementações e abstrações de cache distribuído internos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b4735-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

[<span data-ttu-id="b4735-107">Exibir ou baixar o código de exemplo</span><span class="sxs-lookup"><span data-stu-id="b4735-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample)

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="b4735-108">O que é um Cache distribuído</span><span class="sxs-lookup"><span data-stu-id="b4735-108">What is a Distributed Cache</span></span>

<span data-ttu-id="b4735-109">Um cache distribuído é compartilhado por vários servidores de aplicativo (consulte [Noções básicas de cache](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="b4735-109">A distributed cache is shared by multiple app servers (see [Caching Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="b4735-110">As informações em cache não são armazenadas na memória dos servidores web individuais, e os dados armazenados em cache estão disponíveis para todos os servidores do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b4735-110">The information in the cache is not stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="b4735-111">Isso oferece várias vantagens:</span><span class="sxs-lookup"><span data-stu-id="b4735-111">This provides several advantages:</span></span>

1. <span data-ttu-id="b4735-112">Dados armazenados em cache são coerentes em todos os servidores web.</span><span class="sxs-lookup"><span data-stu-id="b4735-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="b4735-113">Os usuários não ver resultados diferentes dependendo de qual web server manipula a solicitação</span><span class="sxs-lookup"><span data-stu-id="b4735-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="b4735-114">Dados armazenados em cache sobrevive reinicializações do servidor web e implantações.</span><span class="sxs-lookup"><span data-stu-id="b4735-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="b4735-115">Servidores web individuais podem ser removidas ou adicionadas sem afetar o cache.</span><span class="sxs-lookup"><span data-stu-id="b4735-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="b4735-116">O repositório de dados de origem tem menos solicitações feitas a ele (de cache em todos os com vários caches de memória ou não).</span><span class="sxs-lookup"><span data-stu-id="b4735-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="b4735-117">Se usar um Cache distribuído do SQL Server, algumas dessas vantagens são somente verdadeiras se uma instância separada do banco de dados é usada para o cache de dados de origem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b4735-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="b4735-118">Como qualquer cache, um cache distribuído pode melhorar drasticamente capacidade de resposta do aplicativo, como normalmente os dados podem ser recuperados do cache muito mais rápido do que de um banco de dados relacional (ou serviço da web).</span><span class="sxs-lookup"><span data-stu-id="b4735-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="b4735-119">Configuração de cache é específico da implementação.</span><span class="sxs-lookup"><span data-stu-id="b4735-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="b4735-120">Este artigo descreve como configurar ambos Redis e distribuídas do SQL Server armazena em cache.</span><span class="sxs-lookup"><span data-stu-id="b4735-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="b4735-121">Independentemente de qual implementação for selecionada, o aplicativo interage com o cache usando uma comum `IDistributedCache` interface.</span><span class="sxs-lookup"><span data-stu-id="b4735-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="b4735-122">A Interface IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="b4735-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="b4735-123">O `IDistributedCache` interface inclui métodos síncronos e assíncronos.</span><span class="sxs-lookup"><span data-stu-id="b4735-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="b4735-124">A interface permite que itens sejam adicionados, recuperar e removido da implementação de cache distribuído.</span><span class="sxs-lookup"><span data-stu-id="b4735-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="b4735-125">O `IDistributedCache` interface inclui os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="b4735-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="b4735-126">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="b4735-126">**Get, GetAsync**</span></span>

<span data-ttu-id="b4735-127">Usa uma chave de cadeia de caracteres e recupera um item em cache como um `byte[]` se encontrado no cache.</span><span class="sxs-lookup"><span data-stu-id="b4735-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="b4735-128">**Conjunto de SetAsync**</span><span class="sxs-lookup"><span data-stu-id="b4735-128">**Set, SetAsync**</span></span>

<span data-ttu-id="b4735-129">Adiciona um item (como `byte[]`) para o cache usando uma chave de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="b4735-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="b4735-130">**Atualização de RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="b4735-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="b4735-131">Atualiza um item em cache com base em sua chave, redefinir seu tempo limite de expiração deslizante (se houver).</span><span class="sxs-lookup"><span data-stu-id="b4735-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="b4735-132">**Remover RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="b4735-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="b4735-133">Remove uma entrada de cache com base em sua chave.</span><span class="sxs-lookup"><span data-stu-id="b4735-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="b4735-134">Para usar o `IDistributedCache` interface:</span><span class="sxs-lookup"><span data-stu-id="b4735-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="b4735-135">Adicione os pacotes do NuGet necessários para o arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="b4735-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="b4735-136">Configurar a implementação específica de `IDistributedCache` no seu `Startup` da classe `ConfigureServices` método e adicione-o para o contêiner existe.</span><span class="sxs-lookup"><span data-stu-id="b4735-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="b4735-137">O aplicativo [`Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache' a partir do construtor.</span><span class="sxs-lookup"><span data-stu-id="b4735-137">From the app's [`Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache\` from the constructor.</span></span> <span data-ttu-id="b4735-138">A instância será fornecida pelo [injeção de dependência](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="b4735-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="b4735-139">Não é necessário usar um tempo de vida Singleton ou escopo para `IDistributedCache` instâncias (pelo menos para as implementações internas).</span><span class="sxs-lookup"><span data-stu-id="b4735-139">There is no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="b4735-140">Você também pode criar uma instância sempre que talvez seja necessário um (em vez de usar [injeção de dependência](../../fundamentals/dependency-injection.md)), mas isso pode tornar seu código mais difícil de teste e viola o [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="b4735-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="b4735-141">O exemplo a seguir mostra como usar uma instância de `IDistributedCache` em um componente de middleware simples:</span><span class="sxs-lookup"><span data-stu-id="b4735-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

<span data-ttu-id="b4735-142">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]</span><span class="sxs-lookup"><span data-stu-id="b4735-142">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]</span></span>

<span data-ttu-id="b4735-143">No código acima, o valor armazenado em cache é de leitura, mas nunca foi gravado.</span><span class="sxs-lookup"><span data-stu-id="b4735-143">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="b4735-144">Neste exemplo, o valor é definido apenas quando um servidor é inicializado e não é alterado.</span><span class="sxs-lookup"><span data-stu-id="b4735-144">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="b4735-145">Em um cenário de vários servidores, o servidor mais recente para iniciar substituirá quaisquer valores anteriores que foram definidas por outros servidores.</span><span class="sxs-lookup"><span data-stu-id="b4735-145">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="b4735-146">O `Get` e `Set` métodos usam o `byte[]` tipo.</span><span class="sxs-lookup"><span data-stu-id="b4735-146">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="b4735-147">Portanto, o valor de cadeia de caracteres deve ser convertido usando `Encoding.UTF8.GetString` (para `Get`) e `Encoding.UTF8.GetBytes` (para `Set`).</span><span class="sxs-lookup"><span data-stu-id="b4735-147">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="b4735-148">O código a seguir de *Startup.cs* mostra o valor que está sendo definido:</span><span class="sxs-lookup"><span data-stu-id="b4735-148">The following code from *Startup.cs* shows the value being set:</span></span>

<span data-ttu-id="b4735-149">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]</span><span class="sxs-lookup"><span data-stu-id="b4735-149">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]</span></span>

> [!NOTE]
> <span data-ttu-id="b4735-150">Como `IDistributedCache` é configurado no `ConfigureServices` método, ele está disponível para o `Configure` método como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b4735-150">Since `IDistributedCache` is configured in the `ConfigureServices` method, it is available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="b4735-151">Adicioná-lo como um parâmetro permitirá que a instância configurada ser fornecido por meio de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="b4735-151">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="b4735-152">Usar um Redis Cache distribuído</span><span class="sxs-lookup"><span data-stu-id="b4735-152">Using a Redis Distributed Cache</span></span>

<span data-ttu-id="b4735-153">[Redis](http://redis.io) é um repositório de dados na memória do código-fonte aberto, que geralmente é usado como um cache distribuído.</span><span class="sxs-lookup"><span data-stu-id="b4735-153">[Redis](http://redis.io) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="b4735-154">Você pode usá-lo localmente, e você pode configurar um [Cache Redis do Azure](https://azure.microsoft.com/services/cache/) para seus aplicativos hospedados do Azure ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b4735-154">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="b4735-155">Seu aplicativo ASP.NET Core configura a implementação de cache usando uma `RedisDistributedCache` instância.</span><span class="sxs-lookup"><span data-stu-id="b4735-155">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="b4735-156">Configurar a implementação do Redis em `ConfigureServices` e acessá-lo no código do aplicativo solicitando uma instância de `IDistributedCache` (consulte o código acima).</span><span class="sxs-lookup"><span data-stu-id="b4735-156">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="b4735-157">No código de exemplo, um `RedisCache` implementação é usada quando o servidor está configurado para um `Staging` ambiente.</span><span class="sxs-lookup"><span data-stu-id="b4735-157">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="b4735-158">Assim, o `ConfigureStagingServices` método configura o `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="b4735-158">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

<span data-ttu-id="b4735-159">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]</span><span class="sxs-lookup"><span data-stu-id="b4735-159">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]</span></span>

> [!NOTE]
> <span data-ttu-id="b4735-160">Para instalar o Redis em seu computador local, instale o pacote chocolatey [http://chocolatey.org/packages/redis-64/](http://chocolatey.org/packages/redis-64/) e executar `redis-server` em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="b4735-160">To install Redis on your local machine, install the chocolatey package [http://chocolatey.org/packages/redis-64/](http://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="b4735-161">Usando um SQL Server de Cache distribuído</span><span class="sxs-lookup"><span data-stu-id="b4735-161">Using a SQL Server Distributed Cache</span></span>

<span data-ttu-id="b4735-162">A implementação de SqlServerCache permite que o cache distribuído usar um banco de dados do SQL Server como seu armazenamento de backup.</span><span class="sxs-lookup"><span data-stu-id="b4735-162">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="b4735-163">Para SQL Server de criar tabela, você pode usar a ferramenta de cache de sql, a ferramenta cria uma tabela com o nome e o esquema que você especificar.</span><span class="sxs-lookup"><span data-stu-id="b4735-163">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

<span data-ttu-id="b4735-164">Para usar a ferramenta de cache de sql, adicione `SqlConfig.Tools` para o `<ItemGroup>` elemento o *. csproj* de arquivo e execute a restauração de dotnet.</span><span class="sxs-lookup"><span data-stu-id="b4735-164">To use the sql-cache tool, add `SqlConfig.Tools` to the `<ItemGroup>` element of the *.csproj* file and run dotnet restore.</span></span>

<span data-ttu-id="b4735-165">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]</span><span class="sxs-lookup"><span data-stu-id="b4735-165">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]</span></span>

<span data-ttu-id="b4735-166">Teste SqlConfig.Tools executando o seguinte comando</span><span class="sxs-lookup"><span data-stu-id="b4735-166">Test SqlConfig.Tools by running the following command</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

<span data-ttu-id="b4735-167">ferramenta de cache de SQL exibirá a Ajuda de uso e opções de comando, agora você pode criar tabelas no sql server, executando o comando "criar cache de sql":</span><span class="sxs-lookup"><span data-stu-id="b4735-167">sql-cache tool  will display usage, options and command help, now you can create tables into sql server, running "sql-cache create" command :</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

<span data-ttu-id="b4735-168">A tabela criada tem o esquema a seguir:</span><span class="sxs-lookup"><span data-stu-id="b4735-168">The created table has the following schema:</span></span>

![Tabela de Cache do SQL Server](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="b4735-170">Como todas as implementações de cache, seu aplicativo deve obter e definir valores de cache usando uma instância de `IDistributedCache`, não um `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="b4735-170">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="b4735-171">O exemplo implementa `SqlServerCache` no `Production` ambiente (para que ele é configurado em `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="b4735-171">The sample implements `SqlServerCache` in the `Production` environment (so it is configured in `ConfigureProductionServices`).</span></span>

<span data-ttu-id="b4735-172">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]</span><span class="sxs-lookup"><span data-stu-id="b4735-172">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]</span></span>

> [!NOTE]
> <span data-ttu-id="b4735-173">O `ConnectionString` (e, opcionalmente, `SchemaName` e `TableName`) normalmente devem ser armazenadas fora do controle de origem (como UserSecrets), pois eles podem conter credenciais.</span><span class="sxs-lookup"><span data-stu-id="b4735-173">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="b4735-174">Recomendações</span><span class="sxs-lookup"><span data-stu-id="b4735-174">Recommendations</span></span>

<span data-ttu-id="b4735-175">Ao decidir qual implementação de `IDistributedCache` é ideal para seu aplicativo, escolha entre Redis e do SQL Server com base em sua infraestrutura existente e ambiente, seus requisitos de desempenho e experiência da sua equipe.</span><span class="sxs-lookup"><span data-stu-id="b4735-175">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="b4735-176">Se sua equipe mais confortável com Redis, é uma excelente opção.</span><span class="sxs-lookup"><span data-stu-id="b4735-176">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="b4735-177">Se sua equipe prefere SQL Server, você pode ter certeza de que implementação também.</span><span class="sxs-lookup"><span data-stu-id="b4735-177">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="b4735-178">Observe que uma solução tradicional de cache armazena dados na memória que permite a rápida recuperação de dados.</span><span class="sxs-lookup"><span data-stu-id="b4735-178">Note that A traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="b4735-179">Você deve armazenar dados usados em um cache e armazenar todos os dados em um armazenamento persistente de back-end, como o SQL Server ou do armazenamento do Azure.</span><span class="sxs-lookup"><span data-stu-id="b4735-179">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="b4735-180">Cache redis é uma solução de cache que oferece alta taxa de transferência e baixa latência em comparação com o Cache de SQL.</span><span class="sxs-lookup"><span data-stu-id="b4735-180">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

<span data-ttu-id="b4735-181">Recursos adicionais:</span><span class="sxs-lookup"><span data-stu-id="b4735-181">Additional resources:</span></span>

* [<span data-ttu-id="b4735-182">No cache de memória</span><span class="sxs-lookup"><span data-stu-id="b4735-182">In memory caching</span></span>](memory.md)
* [<span data-ttu-id="b4735-183">Redis Cache no Azure</span><span class="sxs-lookup"><span data-stu-id="b4735-183">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="b4735-184">Banco de dados do SQL Azure</span><span class="sxs-lookup"><span data-stu-id="b4735-184">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
