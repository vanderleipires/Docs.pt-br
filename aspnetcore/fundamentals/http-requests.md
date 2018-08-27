---
title: Iniciar solicitações HTTP
author: stevejgordon
description: Saiba mais sobre como usar a interface IHttpClientFactory para gerenciar instâncias de HttpClient lógicas no ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/07/2018
uid: fundamentals/http-requests
ms.openlocfilehash: 2a1bf78edb5068d8b10d66e5ef306b1ad4395da6
ms.sourcegitcommit: 15d7bd0b2c4e6fe9ac335d658bab71a45ca5bc72
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41751654"
---
# <a name="initiate-http-requests"></a>Iniciar solicitações HTTP

Por [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) e [Steve Gordon](https://github.com/stevejgordon)

Um [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) pode ser registrado e usado para configurar e criar instâncias de [HttpClient](/dotnet/api/system.net.http.httpclient) em um aplicativo. Ele oferece os seguintes benefícios:

* Fornece um local central para nomear e configurar instâncias lógicas de `HttpClient`. Por exemplo, um cliente *github* pode ser registrado e configurado para acessar o GitHub. Um cliente padrão pode ser registrado para outras finalidades.
* Codifica o conceito de middleware de saída por meio da delegação de manipuladores no `HttpClient` e fornece extensões para que o middleware baseado no Polly possa aproveitar esse recurso.
* Gerencia o pooling e o tempo de vida das instâncias de `HttpClientMessageHandler` subjacentes para evitar problemas de DNS comuns que ocorrem no gerenciamento manual de tempos de vida de `HttpClient`.
* Adiciona uma experiência de registro em log configurável (via `ILogger`) para todas as solicitações enviadas por meio de clientes criados pelo alocador.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Pré-requisitos

Os projetos direcionados ao .NET Framework exigem a instalação do pacote do NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/). Os projetos destinados ao .Net Core e a referência ao [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) já incluem o pacote `Microsoft.Extensions.Http`.

## <a name="consumption-patterns"></a>Padrões de consumo

Há várias maneiras de usar o `IHttpClientFactory` em um aplicativo:

* [Uso básico](#basic-usage)
* [Clientes nomeados](#named-clients)
* [Clientes com tipo](#typed-clients)
* [Clientes gerados](#generated-clients)

Nenhum deles é estritamente superiores ao outro. A melhor abordagem depende das restrições do aplicativo.

### <a name="basic-usage"></a>Uso básico

O `IHttpClientFactory` pode ser registrado chamando o método de extensão `AddHttpClient` em `IServiceCollection`, dentro do método `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

Depois de registrado, o código pode aceitar um `IHttpClientFactory` em qualquer lugar em que os serviços possam ser injetados com [DI](xref:fundamentals/dependency-injection) (injeção de dependência). O `IHttpClientFactory` pode ser usado para criar uma instância de `HttpClient`:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Usar o `IHttpClientFactory` dessa forma é uma ótima maneira de refatorar um aplicativo existente. Não há nenhum impacto na maneira em que o `HttpClient` é usado. Nos locais em que as instâncias `HttpClient` são criadas no momento, substitua essas ocorrências por uma chamada a [CreateClient](/dotnet/api/system.net.http.ihttpclientfactory.createclient).

### <a name="named-clients"></a>Clientes nomeados

Quando um aplicativo exige vários usos distintos do `HttpClient`, cada um com uma configuração diferente, uma opção é usar **clientes nomeados**. A configuração de um `HttpClient` nomeado pode ser especificada durante o registro em `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

No código anterior, `AddHttpClient` é chamado, fornecendo o nome *github*. Esse cliente tem algumas configurações padrão aplicadas, ou seja, o endereço básico e dois cabeçalhos necessários para trabalhar com a API do GitHub.

Toda vez que o `CreateClient` é chamado, uma nova instância de `HttpClient` é criada e a ação de configuração é chamada.

Para consumir um cliente nomeado, um parâmetro de cadeia de caracteres pode ser passado para `CreateClient`. Especifique o nome do cliente a ser criado:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

No código anterior, a solicitação não precisa especificar um nome do host. Ela pode passar apenas o caminho, pois o endereço básico configurado para o cliente é usado.

### <a name="typed-clients"></a>Clientes com tipo

Os clientes com tipo fornecem as mesmas funcionalidade que os clientes nomeados sem a necessidade de usar cadeias de caracteres como chaves. A abordagem de cliente com tipo fornece a ajuda do IntelliSense e do compilador durante o consumo de clientes. Eles fornecem um único local para configurar e interagir com um determinado `HttpClient`. Por exemplo, um único cliente com tipo pode ser usado para um único ponto de extremidade de back-end e encapsular toda a lógica que lida com esse ponto de extremidade. Outra vantagem é que eles funcionam com a DI e podem ser injetados no local necessário no aplicativo.

Um cliente com tipo aceita um parâmetro `HttpClient` em seu construtor:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

No código anterior, a configuração é movida para o cliente com tipo. O objeto `HttpClient` é exposto como uma propriedade pública. É possível definir métodos específicos da API que expõem a funcionalidade `HttpClient`. O método `GetAspNetDocsIssues` encapsula o código necessário para consultar e analisar os últimos problemas em aberto de um repositório GitHub.

Para registrar um cliente com tipo, o método de extensão `AddHttpClient` genérico pode ser usado em `Startup.ConfigureServices`, especificando a classe do cliente com tipo:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

O cliente com tipo é registrado como transitório com a DI. O cliente com tipo pode ser injetado e consumido diretamente:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Se preferir, a configuração de um cliente com tipo poderá ser especificada durante o registro em `Startup.ConfigureServices` e não no construtor do cliente com tipo:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

É possível encapsular totalmente o `HttpClient` dentro de um cliente com tipo. Em vez de o expor como uma propriedade, é possível fornecer métodos públicos que chamam a instância de `HttpClient` internamente.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

No código anterior, o `HttpClient` é armazenado como um campo privado. Todo o acesso para fazer chamadas externas passa pelo método `GetRepos`.

### <a name="generated-clients"></a>Clientes gerados

O `IHttpClientFactory` pode ser usado em combinação com outras bibliotecas de terceiros, como a [Refit](https://github.com/paulcbetts/refit). Refit é uma biblioteca REST para .NET. Ela converte APIs REST em interfaces dinâmicas. Uma implementação da interface é gerada dinamicamente pelo `RestService` usando `HttpClient` para fazer as chamadas de HTTP externas.

Uma interface e uma resposta são definidas para representar a API externa e sua resposta:

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

Um cliente com tipo pode ser adicionado usando a Refit para gerar a implementação:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

A interface definida pode ser consumida, quando necessário, com a implementação fornecida pela DI e pela Refit:

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a>Middleware de solicitação de saída

O `HttpClient` já tem o conceito de delegar manipuladores que podem ser vinculados para solicitações HTTP de saída. O `IHttpClientFactory` facilita a definição dos manipuladores a serem aplicados a cada cliente nomeado. Ele permite o registro e o encadeamento de vários manipuladores para criar um pipeline do middleware de solicitação saída. Cada um desses manipuladores é capaz de executar o trabalho antes e após a solicitação de saída. Esse padrão é semelhante ao pipeline do middleware de entrada no ASP.NET Core. O padrão fornece um mecanismo para gerenciar interesses paralelos em relação às solicitações HTTP, incluindo o armazenamento em cache, o tratamento de erro, a serialização e o registro em log.

Para criar um manipulador, defina uma classe derivando-a de `DelegatingHandler`. Substitua o método `SendAsync` para executar o código antes de passar a solicitação para o próximo manipulador no pipeline:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

O código anterior define um manipulador básico. Ele verifica se um cabeçalho `X-API-KEY` foi incluído na solicitação. Se o cabeçalho estiver ausente, isso poderá evitar a chamada de HTTP e retornar uma resposta adequada.

Durante o registro, um ou mais manipuladores podem ser adicionados à configuração de um `HttpClient`. Essa tarefa é realizada por meio de métodos de extensão no [IHttpClientBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.ihttpclientbuilder).

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

No código anterior, o `ValidateHeaderHandler` é registrado com a DI. O manipulador **precisa** ser registrado na DI como transitório. Depois de registrado, é possível chamar [AddHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.addhttpmessagehandler), passando o tipo para o manipulador.

Vários manipuladores podem ser registrados na ordem em que eles devem ser executados. Cada manipulador encapsula o próximo manipulador até que o `HttpClientHandler` final execute a solicitação:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a>Usar manipuladores baseados no Polly

O `IHttpClientFactory` integra-se a uma biblioteca de terceiros popular chamada [Polly](https://github.com/App-vNext/Polly). O Polly é uma biblioteca abrangente de tratamento de falha transitória e de resiliência para .NET. Ela permite que os desenvolvedores expressem políticas, como Repetição, Disjuntor, Tempo Limite, Isolamento de Bulkhead e Fallback de maneira fluente e thread-safe.

Os métodos de extensão são fornecidos para habilitar o uso de políticas do Polly com instâncias de `HttpClient` configuradas. As extensões do Polly estão disponíveis no pacote do NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/). Esse pacote não está incluído no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Para usar as extensões, um `<PackageReference />` explícito deve ser incluído no projeto.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/HttpClientFactorySample.csproj?highlight=9)]

Depois de restaurar este pacote, os métodos de extensão ficam disponíveis para permitir a adição de manipuladores baseados no Polly aos clientes.

### <a name="handle-transient-faults"></a>Tratar falhas transitórias

As falhas mais comuns ocorrem quando as chamadas HTTP externas são transitórias. Um método de extensão conveniente chamado `AddTransientHttpErrorPolicy` é incluído para permitir que uma política seja definido para tratar erros transitórios. As políticas configuradas com esse método de extensão tratam `HttpRequestException`, respostas HTTP 5xx e respostas HTTP 408.

A extensão `AddTransientHttpErrorPolicy` pode ser usada em `Startup.ConfigureServices`. A extensão fornece acesso a um objeto `PolicyBuilder` configurado para tratar erros que representam uma possível falha transitória:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

No código anterior, uma política `WaitAndRetryAsync` é definida. As solicitações com falha são repetidas até três vezes com um atraso de 600 ms entre as tentativas.

### <a name="dynamically-select-policies"></a>Selecionar políticas dinamicamente

Existem métodos de extensão adicionais que podem ser usados para adicionar manipuladores baseados no Polly. Uma dessas extensões é a `AddPolicyHandler`, que tem várias sobrecargas. Uma sobrecarga permite que a solicitação seja inspecionada durante a definição da política a ser aplicada:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

No código anterior, se a solicitação de saída é um GET, é aplicado um tempo limite de 10 segundos. Para qualquer outro método HTTP, é usado um tempo limite de 30 segundos.

### <a name="add-multiple-polly-handlers"></a>Adicionar vários manipuladores do Polly

É comum aninhar políticas do Polly para fornecer uma funcionalidade avançada:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

No exemplo anterior, dois manipuladores são adicionados. O primeiro usa a extensão `AddTransientHttpErrorPolicy` para adicionar uma política de repetição. As solicitações com falha são repetidas até três vezes. A segunda chamada a `AddTransientHttpErrorPolicy` adiciona uma política de disjuntor. As solicitações externas adicionais são bloqueadas por 30 segundos quando ocorrem cinco tentativas com falha consecutivamente. As políticas de disjuntor são políticas com estado. Todas as chamadas por meio desse cliente compartilham o mesmo estado do circuito.

### <a name="add-policies-from-the-polly-registry"></a>Adicionar políticas do registro do Polly

Uma abordagem para gerenciar as políticas usadas com frequência é defini-las uma única vez e registrá-las em um `PolicyRegistry`. É fornecido um método de extensão que permite que um manipulador seja adicionado usando uma política do registro:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

No código anterior, duas políticas são registradas quando `PolicyRegistry` é adicionado ao `ServiceCollection`. Para usar uma política do registro, o método `AddPolicyHandlerFromRegistry` é usado, passando o nome da política a ser aplicada.

Mais informações sobre o `IHttpClientFactory` e as integrações do Polly podem ser encontradas no [Wiki do Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>HttpClient e gerenciamento de tempo de vida

Uma nova instância de `HttpClient` é chamada, sempre que `CreateClient` é chamado na `IHttpClientFactory`. Haverá um [HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler) para cada cliente nomeado. `IHttpClientFactory` cria pools com as instâncias de `HttpMessageHandler` criadas pelo alocador para reduzir o consumo de recursos. Uma instância de `HttpMessageHandler` poderá ser reutilizada no pool, ao criar uma nova instância de `HttpClient`, se o respectivo tempo de vida não tiver expirado.

O pooling de manipuladores é preferível porque normalmente cada manipulador gerencia as próprias conexões HTTP subjacentes. Criar mais manipuladores do que o necessário pode resultar em atrasos de conexão. Alguns manipuladores também mantêm as conexões abertas indefinidamente, o que pode impedir que o manipulador reaja a alterações de DNS.

O tempo de vida padrão do manipulador é de 2 minutos. O valor padrão pode ser substituído para cada cliente nomeado. Para substituí-lo, chame [SetHandlerLifetime](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.sethandlerlifetime) no `IHttpClientBuilder` que é retornado ao criar o cliente:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

Não é necessário descartar o cliente. O descarte cancela as solicitações de saída e garante que a determinada instância de `HttpClient` não seja mais usada depois de chamar [Dispose](/dotnet/api/system.idisposable.dispose#System_IDisposable_Dispose). `IHttpClientFactory` rastreia e descarta recursos usados pelas instâncias de `HttpClient`. Geralmente, as instâncias de `HttpClient` podem ser tratadas como objetos do .NET e não exigem descarte.

Manter uma única instância de `HttpClient` ativa por uma longa duração é um padrão comum usado, antes do início de `IHttpClientFactory`. Esse padrão se torna desnecessário após a migração para `IHttpClientFactory`.

## <a name="logging"></a>Registrando em log

Os clientes criado pelo `IHttpClientFactory` registram mensagens de log para todas as solicitações. Habilite o nível apropriado de informações na configuração de log para ver as mensagens de log padrão. Os registros em log adicionais, como o registro em log dos cabeçalhos de solicitação, estão incluídos somente no nível de rastreamento.

A categoria de log usada para cada cliente inclui o nome do cliente. Um cliente chamado *MyNamedClient*, por exemplo, registra mensagens com uma categoria de `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. Mensagens sufixadas com *LogicalHandler* ocorrem fora do pipeline do manipulador de solicitações. Na solicitação, as mensagens são registradas em log antes que qualquer outro manipulador no pipeline a tenha processado. Na resposta, as mensagens são registradas depois que qualquer outro manipulador do pipeline tenha recebido a resposta.

O registro em log também ocorre dentro do pipeline do manipulador de solicitações. No caso do exemplo de *MyNamedClient*, essas mensagens são registradas em log na categoria de log `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. Para a solicitação, isso ocorre depois que todos os outros manipuladores são executados e imediatamente antes que a solicitação seja enviada pela rede. Na resposta, esse registro em log inclui o estado da resposta antes que ela seja retornada por meio do pipeline do manipulador.

Habilitar o registro em log dentro e fora do pipeline permite a inspeção das alterações feitas por outros manipuladores do pipeline. Isso pode incluir, por exemplo, alterações nos cabeçalhos de solicitação ou no código de status da resposta.

Incluir o nome do cliente na categoria de log permite a filtragem de log para clientes nomeados específicos, quando necessário.

## <a name="configure-the-httpmessagehandler"></a>Configurar o HttpMessageHandler

Talvez seja necessário controlar a configuração do `HttpMessageHandler` interno usado por um cliente.

Um `IHttpClientBuilder` é retornado ao adicionar clientes nomeados ou com tipo. O método de extensão [ConfigurePrimaryHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.configureprimaryhttpmessagehandler) pode ser usado para definir um delegado. O representante que é usado para criar e configurar o `HttpMessageHandler` primário usado pelo cliente:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]
