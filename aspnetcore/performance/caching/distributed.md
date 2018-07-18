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
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Trabalhar com um cache distribuído no ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Os caches distribuídos podem melhorar o desempenho e escalabilidade de aplicativos do ASP.NET Core, especialmente quando hospedado na nuvem ou um farm de servidores.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>O que é um cache distribuído

Um cache distribuído é compartilhado por vários servidores de aplicativo (consulte [Noções básicas de Cache](memory.md#caching-basics)). As informações em cache não são armazenadas na memória dos servidores web individual e os dados armazenados em cache estão disponíveis em todos os servidores do aplicativo. Isso fornece várias vantagens:

1. Dados armazenados em cache são coerentes em todos os servidores web. Os usuários não veem resultados diferentes dependendo de qual web server lida com sua solicitação

2. Dados armazenados em cache sobrevive reinicializações do servidor web e implantações. Servidores web individuais podem ser removidos ou adicionados sem afetar o cache.

3. O armazenamento de dados de origem tem menos solicitações feitas a ele (de com vários caches na memória ou não armazenar em cache em todos os).

> [!NOTE]
> Se usar um Cache distribuído do SQL Server, algumas dessas vantagens são somente verdadeiras se uma instância de banco de dados separado é usada para o cache do que para dados de origem do aplicativo.

Como qualquer cache, um cache distribuído pode melhorar consideravelmente capacidade de resposta do aplicativo, como normalmente os dados podem ser recuperados do cache muito mais rápido do que de um banco de dados relacional (ou serviço da web).

Configuração de cache é específico da implementação. Este artigo descreve como configurar ambos Redis e distribuídas do SQL Server armazena em cache. Independentemente de qual implementação for selecionada, o aplicativo interage com o cache usando um comum `IDistributedCache` interface.

## <a name="the-idistributedcache-interface"></a>A Interface de IDistributedCache

O `IDistributedCache` interface inclui métodos síncronos e assíncronos. A interface permite que os itens a serem adicionados, recuperados e removido da implementação de cache distribuído. O `IDistributedCache` interface inclui os seguintes métodos:

**Get, GetAsync**

Usa uma chave de cadeia de caracteres e recupera um item em cache como um `byte[]` se encontrada no cache.

**Set, SetAsync**

Adiciona um item (como `byte[]`) para o cache usando uma chave de cadeia de caracteres.

**Atualização de RefreshAsync**

Atualiza um item no cache com base em sua chave, redefinindo seu tempo limite de expiração deslizante (se houver).

**Remove, RemoveAsync**

Remove uma entrada de cache com base em sua chave.

Para usar o `IDistributedCache` interface:

   1. Adicione os pacotes NuGet necessários para seu arquivo de projeto.

   2. Configurar a implementação específica do `IDistributedCache` em seu `Startup` da classe `ConfigureServices` método e adicioná-lo para o contêiner existe.

   3. Do aplicativo do [Middleware](xref:fundamentals/middleware/index) ou classes do controlador MVC, solicitar uma instância de `IDistributedCache` do construtor. A instância será fornecida pelo [injeção de dependência](../../fundamentals/dependency-injection.md) (DI).

> [!NOTE]
> Não é necessário usar um tempo de vida Singleton ou com escopo para `IDistributedCache` instâncias (pelo menos para as implementações internas). Você também pode criar uma instância sempre que talvez seja necessário um (em vez de usar [injeção de dependência](../../fundamentals/dependency-injection.md)), mas isso pode tornar seu código mais difícil de testar e viola a [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/).

O exemplo a seguir mostra como usar uma instância de `IDistributedCache` em um componente de middleware simples:

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

No código acima, o valor armazenado em cache é lido, mas nunca foi escrito. Neste exemplo, o valor só é definido quando um servidor é inicializado e não é alterado. Em um cenário de vários servidor, o servidor mais recente para iniciar substituirá os valores anteriores que foram definidos por outros servidores. O `Get` e `Set` métodos usam o `byte[]` tipo. Portanto, o valor de cadeia de caracteres deve ser convertido usando `Encoding.UTF8.GetString` (para `Get`) e `Encoding.UTF8.GetBytes` (para `Set`).

O código a seguir da *Startup.cs* mostra o valor que está sendo definido:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> Uma vez que `IDistributedCache` está configurado na `ConfigureServices` método, ele está disponível para o `Configure` método como um parâmetro. Adicioná-lo como um parâmetro permitirá que a instância configurada ser fornecido por meio da DI.

## <a name="using-a-redis-distributed-cache"></a>Usando um cache Redis distribuído

[Redis](https://redis.io/) é um repositório de dados na memória do código-fonte aberto, que geralmente é usado como um cache distribuído. Você pode usá-lo localmente, e você pode configurar uma [Cache Redis do Azure](https://azure.microsoft.com/services/cache/) para seus aplicativos ASP.NET Core hospedados no Azure. Seu aplicativo ASP.NET Core configura a implementação de cache usando um `RedisDistributedCache` instância.

Você configura a implementação do Redis na `ConfigureServices` e acessá-lo no código do aplicativo, solicitando uma instância de `IDistributedCache` (consulte o código acima).

No código de exemplo, uma `RedisCache` implementação é usada quando o servidor está configurado para um `Staging` ambiente. Portanto, o `ConfigureStagingServices` método configura o `RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> Para instalar o Redis em seu computador local, instale o pacote do chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) e execute `redis-server` em um prompt de comando.

## <a name="using-a-sql-server-distributed-cache"></a>Usando um SQL Server de cache distribuído

A implementação de SqlServerCache permite que o cache distribuído usar um banco de dados do SQL Server como seu armazenamento de backup. Para criar o SQL Server a tabela que você pode usar a ferramenta de cache de sql, a ferramenta cria uma tabela com o nome e o esquema que você especificar.

::: moniker range="< aspnetcore-2.1"

Adicione `SqlConfig.Tools` para o `<ItemGroup>` elemento do arquivo de projeto e execute `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Teste SqlConfig.Tools executando o seguinte comando:

```console
dotnet sql-cache create --help
```

SqlConfig.Tools exibe o uso e opções de Ajuda do comando.

Crie uma tabela no SQL Server executando o `sql-cache create` comando:

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

A tabela criada tem o esquema a seguir:

![Tabela de Cache do SQL Server](distributed/_static/SqlServerCacheTable.png)

Como todas as implementações de cache, seu aplicativo deve obter e definir valores de cache usando uma instância de `IDistributedCache`, e não um `SqlServerCache`. O exemplo implementa `SqlServerCache` no ambiente de produção (portanto, ele é configurado em `ConfigureProductionServices`).

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> O `ConnectionString` (e, opcionalmente, `SchemaName` e `TableName`) normalmente devem ser armazenados fora do controle do código-fonte (como UserSecrets), pois podem conter credenciais.

## <a name="recommendations"></a>Recomendações

Ao decidir qual implementação de `IDistributedCache` é ideal para seu aplicativo, escolha entre Redis e do SQL Server com base no ambiente e sua infraestrutura existente, seus requisitos de desempenho e experiência da sua equipe. Se sua equipe for mais confortável com o Redis, é uma excelente opção. Se sua equipe preferir do SQL Server, você pode ter certeza em que a implementação também. Observe que uma solução tradicional de cache armazena dados na memória que permite a recuperação rápida de dados. Você deve armazenar dados usados em um cache e armazenar todos os dados em um armazenamento persistente de back-end, como o SQL Server ou do armazenamento do Azure. Cache redis é uma solução de cache que fornece alta taxa de transferência e baixa latência em comparação com o Cache de SQL.

## <a name="additional-resources"></a>Recursos adicionais

* [Redis Cache no Azure](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Banco de dados SQL no Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
