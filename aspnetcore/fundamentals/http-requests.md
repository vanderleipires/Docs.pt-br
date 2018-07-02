---
title: Iniciar solicitações HTTP
author: stevejgordon
description: Saiba mais sobre como usar a interface IHttpClientFactory para gerenciar instâncias de HttpClient lógicas no ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/22/2018
uid: fundamentals/http-requests
ms.openlocfilehash: e56c7a3ed80cc08103f6178859a1a99f1a5ec068
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327516"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="8e743-103">Iniciar solicitações HTTP</span><span class="sxs-lookup"><span data-stu-id="8e743-103">Initiate HTTP requests</span></span>

<span data-ttu-id="8e743-104">Por [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) e [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="8e743-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="8e743-105">Um [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) pode ser registrado e usado para configurar e criar instâncias de [HttpClient](/dotnet/api/system.net.http.httpclient) em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8e743-105">An [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="8e743-106">Ele oferece os seguintes benefícios:</span><span class="sxs-lookup"><span data-stu-id="8e743-106">It offers the following benefits:</span></span>

* <span data-ttu-id="8e743-107">Fornece um local central para nomear e configurar instâncias lógicas de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="8e743-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="8e743-108">Por exemplo, um cliente "github" pode ser registrado e configurado para acessar o GitHub.</span><span class="sxs-lookup"><span data-stu-id="8e743-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="8e743-109">Um cliente padrão pode ser registrado para outras finalidades.</span><span class="sxs-lookup"><span data-stu-id="8e743-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="8e743-110">Codifica o conceito de middleware de saída por meio da delegação de manipuladores no `HttpClient` e fornece extensões para que o middleware baseado no Polly possa aproveitar esse recurso.</span><span class="sxs-lookup"><span data-stu-id="8e743-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="8e743-111">Gerencia o pooling e o tempo de vida das instâncias de `HttpClientMessageHandler` subjacentes para evitar problemas de DNS comuns que ocorrem no gerenciamento manual de tempos de vida de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="8e743-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="8e743-112">Adiciona uma experiência de registro em log configurável (via `ILogger`) para todas as solicitações enviadas por meio de clientes criados pelo alocador.</span><span class="sxs-lookup"><span data-stu-id="8e743-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="8e743-113">Padrões de consumo</span><span class="sxs-lookup"><span data-stu-id="8e743-113">Consumption patterns</span></span>

<span data-ttu-id="8e743-114">Há várias maneiras de usar o `IHttpClientFactory` em um aplicativo:</span><span class="sxs-lookup"><span data-stu-id="8e743-114">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="8e743-115">Uso básico</span><span class="sxs-lookup"><span data-stu-id="8e743-115">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="8e743-116">Clientes nomeados</span><span class="sxs-lookup"><span data-stu-id="8e743-116">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="8e743-117">Clientes com tipo</span><span class="sxs-lookup"><span data-stu-id="8e743-117">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="8e743-118">Clientes gerados</span><span class="sxs-lookup"><span data-stu-id="8e743-118">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="8e743-119">Nenhum deles é estritamente superiores ao outro.</span><span class="sxs-lookup"><span data-stu-id="8e743-119">None of them are strictly superior to another.</span></span> <span data-ttu-id="8e743-120">A melhor abordagem depende das restrições do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8e743-120">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="8e743-121">Uso básico</span><span class="sxs-lookup"><span data-stu-id="8e743-121">Basic usage</span></span>

<span data-ttu-id="8e743-122">O `IHttpClientFactory` pode ser registrado chamando o método de extensão `AddHttpClient` em `IServiceCollection`, dentro do método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8e743-122">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="8e743-123">Depois de registrado, o código pode aceitar um `IHttpClientFactory` em qualquer lugar em que os serviços possam ser injetados com [DI](xref:fundamentals/dependency-injection) (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="8e743-123">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="8e743-124">O `IHttpClientFactory` pode ser usado para criar uma instância de `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="8e743-124">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="8e743-125">Usar o `IHttpClientFactory` dessa forma é uma ótima maneira de refatorar um aplicativo existente.</span><span class="sxs-lookup"><span data-stu-id="8e743-125">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="8e743-126">Não há nenhum impacto na maneira em que o `HttpClient` é usado.</span><span class="sxs-lookup"><span data-stu-id="8e743-126">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="8e743-127">Nos locais em que as instâncias de `HttpClient` são criadas no momento, substitua essas ocorrências por uma chamada a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="8e743-127">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="8e743-128">Clientes nomeados</span><span class="sxs-lookup"><span data-stu-id="8e743-128">Named clients</span></span>

<span data-ttu-id="8e743-129">Se um aplicativo exige vários usos distintos do `HttpClient`, cada com uma configuração diferente, uma opção é usar **clientes nomeados**.</span><span class="sxs-lookup"><span data-stu-id="8e743-129">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="8e743-130">A configuração de um `HttpClient` nomeado pode ser especificada durante o registro em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8e743-130">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="8e743-131">No código anterior, `AddHttpClient` é chamado, fornecendo o nome "github".</span><span class="sxs-lookup"><span data-stu-id="8e743-131">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="8e743-132">Esse cliente tem algumas configurações padrão aplicadas, ou seja, o endereço básico e dois cabeçalhos necessários para trabalhar com a API do GitHub.</span><span class="sxs-lookup"><span data-stu-id="8e743-132">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="8e743-133">Toda vez que o `CreateClient` é chamado, uma nova instância de `HttpClient` é criada e a ação de configuração é chamada.</span><span class="sxs-lookup"><span data-stu-id="8e743-133">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="8e743-134">Para consumir um cliente nomeado, um parâmetro de cadeia de caracteres pode ser passado para `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="8e743-134">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="8e743-135">Especifique o nome do cliente a ser criado:</span><span class="sxs-lookup"><span data-stu-id="8e743-135">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="8e743-136">No código anterior, a solicitação não precisa especificar um nome do host.</span><span class="sxs-lookup"><span data-stu-id="8e743-136">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="8e743-137">Ela pode passar apenas o caminho, pois o endereço básico configurado para o cliente é usado.</span><span class="sxs-lookup"><span data-stu-id="8e743-137">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="8e743-138">Clientes com tipo</span><span class="sxs-lookup"><span data-stu-id="8e743-138">Typed clients</span></span>

<span data-ttu-id="8e743-139">Os clientes com tipo fornecem as mesmas funcionalidade que os clientes nomeados sem a necessidade de usar cadeias de caracteres como chaves.</span><span class="sxs-lookup"><span data-stu-id="8e743-139">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="8e743-140">A abordagem de cliente com tipo fornece a ajuda do IntelliSense e do compilador durante o consumo de clientes.</span><span class="sxs-lookup"><span data-stu-id="8e743-140">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="8e743-141">Eles fornecem um único local para configurar e interagir com um determinado `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="8e743-141">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="8e743-142">Por exemplo, um único cliente com tipo pode ser usado para um único ponto de extremidade de back-end e encapsular toda a lógica que lida com esse ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="8e743-142">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="8e743-143">Outra vantagem é que eles funcionam com a DI e podem ser injetados no local necessário no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8e743-143">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="8e743-144">Um cliente com tipo aceita um parâmetro `HttpClient` em seu construtor:</span><span class="sxs-lookup"><span data-stu-id="8e743-144">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="8e743-145">No código anterior, a configuração é movida para o cliente com tipo.</span><span class="sxs-lookup"><span data-stu-id="8e743-145">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="8e743-146">O objeto `HttpClient` é exposto como uma propriedade pública.</span><span class="sxs-lookup"><span data-stu-id="8e743-146">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="8e743-147">É possível definir métodos específicos da API que expõem a funcionalidade `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="8e743-147">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="8e743-148">O método `GetAspNetDocsIssues` encapsula o código necessário para consultar e analisar os últimos problemas em aberto de um repositório GitHub.</span><span class="sxs-lookup"><span data-stu-id="8e743-148">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="8e743-149">Para registrar um cliente com tipo, o método de extensão `AddHttpClient` genérico pode ser usado em `Startup.ConfigureServices`, especificando a classe do cliente com tipo:</span><span class="sxs-lookup"><span data-stu-id="8e743-149">To register a typed client, the generic `AddHttpClient` extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="8e743-150">O cliente com tipo é registrado como transitório com a DI.</span><span class="sxs-lookup"><span data-stu-id="8e743-150">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="8e743-151">O cliente com tipo pode ser injetado e consumido diretamente:</span><span class="sxs-lookup"><span data-stu-id="8e743-151">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="8e743-152">Se preferir, a configuração de um cliente com tipo poderá ser especificada durante o registro em `Startup.ConfigureServices` e não no construtor do cliente com tipo:</span><span class="sxs-lookup"><span data-stu-id="8e743-152">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="8e743-153">É possível encapsular totalmente o `HttpClient` dentro de um cliente com tipo.</span><span class="sxs-lookup"><span data-stu-id="8e743-153">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="8e743-154">Em vez de o expor como uma propriedade, é possível fornecer métodos públicos que chamam a instância de `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="8e743-154">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="8e743-155">No código anterior, o `HttpClient` é armazenado como um campo privado.</span><span class="sxs-lookup"><span data-stu-id="8e743-155">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="8e743-156">Todo o acesso para fazer chamadas externas passa pelo método `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="8e743-156">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="8e743-157">Clientes gerados</span><span class="sxs-lookup"><span data-stu-id="8e743-157">Generated clients</span></span>

<span data-ttu-id="8e743-158">O `IHttpClientFactory` pode ser usado em combinação com outras bibliotecas de terceiros, como a [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="8e743-158">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="8e743-159">Refit é uma biblioteca REST para .NET.</span><span class="sxs-lookup"><span data-stu-id="8e743-159">Refit is a REST library for .NET.</span></span> <span data-ttu-id="8e743-160">Ela converte APIs REST em interfaces dinâmicas.</span><span class="sxs-lookup"><span data-stu-id="8e743-160">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="8e743-161">Uma implementação da interface é gerada dinamicamente pelo `RestService` usando `HttpClient` para fazer as chamadas de HTTP externas.</span><span class="sxs-lookup"><span data-stu-id="8e743-161">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="8e743-162">Uma interface e uma resposta são definidas para representar a API externa e sua resposta:</span><span class="sxs-lookup"><span data-stu-id="8e743-162">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="8e743-163">Um cliente com tipo pode ser adicionado usando a Refit para gerar a implementação:</span><span class="sxs-lookup"><span data-stu-id="8e743-163">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="8e743-164">A interface definida pode ser consumida, quando necessário, com a implementação fornecida pela DI e pela Refit:</span><span class="sxs-lookup"><span data-stu-id="8e743-164">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="8e743-165">Middleware de solicitação de saída</span><span class="sxs-lookup"><span data-stu-id="8e743-165">Outgoing request middleware</span></span>

<span data-ttu-id="8e743-166">O `HttpClient` já tem o conceito de delegar manipuladores que podem ser vinculados para solicitações HTTP de saída.</span><span class="sxs-lookup"><span data-stu-id="8e743-166">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="8e743-167">O `IHttpClientFactory` facilita a definição dos manipuladores a serem aplicados a cada cliente nomeado.</span><span class="sxs-lookup"><span data-stu-id="8e743-167">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="8e743-168">Ele permite o registro e o encadeamento de vários manipuladores para criar um pipeline do middleware de solicitação saída.</span><span class="sxs-lookup"><span data-stu-id="8e743-168">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="8e743-169">Cada um desses manipuladores é capaz de executar o trabalho antes e após a solicitação de saída.</span><span class="sxs-lookup"><span data-stu-id="8e743-169">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="8e743-170">Esse padrão é semelhante ao pipeline do middleware de entrada no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8e743-170">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="8e743-171">O padrão fornece um mecanismo para gerenciar interesses paralelos em relação às solicitações HTTP, incluindo o armazenamento em cache, o tratamento de erro, a serialização e o registro em log.</span><span class="sxs-lookup"><span data-stu-id="8e743-171">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="8e743-172">Para criar um manipulador, defina uma classe derivando-a de `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="8e743-172">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="8e743-173">Substitua o método `SendAsync` para executar o código antes de passar a solicitação para o próximo manipulador no pipeline:</span><span class="sxs-lookup"><span data-stu-id="8e743-173">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="8e743-174">O código anterior define um manipulador básico.</span><span class="sxs-lookup"><span data-stu-id="8e743-174">The preceding code defines a basic handler.</span></span> <span data-ttu-id="8e743-175">Ele verifica se um cabeçalho X-API-KEY foi incluído na solicitação.</span><span class="sxs-lookup"><span data-stu-id="8e743-175">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="8e743-176">Se o cabeçalho estiver ausente, isso poderá evitar a chamada de HTTP e retornar uma resposta adequada.</span><span class="sxs-lookup"><span data-stu-id="8e743-176">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="8e743-177">Durante o registro, um ou mais manipuladores podem ser adicionados à configuração de um `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="8e743-177">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="8e743-178">Essa tarefa é realizada por meio de métodos de extensão no `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8e743-178">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="8e743-179">No código anterior, o `ValidateHeaderHandler` é registrado com a DI.</span><span class="sxs-lookup"><span data-stu-id="8e743-179">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="8e743-180">O manipulador **precisa** ser registrado na DI como transitório.</span><span class="sxs-lookup"><span data-stu-id="8e743-180">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="8e743-181">Depois de registrado, o `AddHttpMessageHandler` poderá ser chamado, passando o tipo para o manipulador.</span><span class="sxs-lookup"><span data-stu-id="8e743-181">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="8e743-182">Vários manipuladores podem ser registrados na ordem em que eles devem ser executados.</span><span class="sxs-lookup"><span data-stu-id="8e743-182">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="8e743-183">Cada manipulador encapsula o próximo manipulador até que o `HttpClientHandler` final execute a solicitação:</span><span class="sxs-lookup"><span data-stu-id="8e743-183">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="8e743-184">Usar manipuladores baseados no Polly</span><span class="sxs-lookup"><span data-stu-id="8e743-184">Use Polly-based handlers</span></span>

<span data-ttu-id="8e743-185">O `IHttpClientFactory` integra-se a uma biblioteca de terceiros popular chamada [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="8e743-185">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="8e743-186">O Polly é uma biblioteca abrangente de tratamento de falha transitória e de resiliência para .NET.</span><span class="sxs-lookup"><span data-stu-id="8e743-186">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="8e743-187">Ela permite que os desenvolvedores expressem políticas, como Repetição, Disjuntor, Tempo Limite, Isolamento de Bulkhead e Fallback de maneira fluente e thread-safe.</span><span class="sxs-lookup"><span data-stu-id="8e743-187">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="8e743-188">Os métodos de extensão são fornecidos para habilitar o uso de políticas do Polly com instâncias de `HttpClient` configuradas.</span><span class="sxs-lookup"><span data-stu-id="8e743-188">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="8e743-189">As extensões do Polly estão disponíveis no pacote do NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="8e743-189">The Polly extensions are available in the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="8e743-190">Esse pacote não está incluído no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8e743-190">This package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="8e743-191">Para usar as extensões, um `<PackageReference />` explícito deve ser incluído no projeto.</span><span class="sxs-lookup"><span data-stu-id="8e743-191">To use the extensions, an explicit `<PackageReference />` should be included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="8e743-192">Depois de restaurar este pacote, os métodos de extensão ficam disponíveis para permitir a adição de manipuladores baseados no Polly aos clientes.</span><span class="sxs-lookup"><span data-stu-id="8e743-192">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="8e743-193">Tratar falhas transitórias</span><span class="sxs-lookup"><span data-stu-id="8e743-193">Handle transient faults</span></span>

<span data-ttu-id="8e743-194">As falhas mais comuns que podem ocorrer ao fazer chamadas de HTTP externas são as transitórias.</span><span class="sxs-lookup"><span data-stu-id="8e743-194">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="8e743-195">Um método de extensão conveniente chamado `AddTransientHttpErrorPolicy` é incluído para permitir que uma política seja definido para tratar erros transitórios.</span><span class="sxs-lookup"><span data-stu-id="8e743-195">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="8e743-196">As políticas configuradas com esse método de extensão tratam `HttpRequestException`, respostas HTTP 5xx e respostas HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="8e743-196">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="8e743-197">A extensão `AddTransientHttpErrorPolicy` pode ser usada em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8e743-197">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8e743-198">A extensão fornece acesso a um objeto `PolicyBuilder` configurado para tratar erros que representam uma possível falha transitória:</span><span class="sxs-lookup"><span data-stu-id="8e743-198">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="8e743-199">No código anterior, uma política `WaitAndRetryAsync` é definida.</span><span class="sxs-lookup"><span data-stu-id="8e743-199">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="8e743-200">As solicitações com falha são repetidas até três vezes com um atraso de 600 ms entre as tentativas.</span><span class="sxs-lookup"><span data-stu-id="8e743-200">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="8e743-201">Selecionar políticas dinamicamente</span><span class="sxs-lookup"><span data-stu-id="8e743-201">Dynamically select policies</span></span>

<span data-ttu-id="8e743-202">Existem métodos de extensão adicionais que podem ser usados para adicionar manipuladores baseados no Polly.</span><span class="sxs-lookup"><span data-stu-id="8e743-202">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="8e743-203">Uma dessas extensões é a `AddPolicyHandler`, que tem várias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="8e743-203">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="8e743-204">Uma sobrecarga permite que a solicitação seja inspecionada durante a definição da política a ser aplicada:</span><span class="sxs-lookup"><span data-stu-id="8e743-204">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="8e743-205">No código anterior, se a solicitação de saída é um GET, é aplicado um tempo limite de 10 segundos.</span><span class="sxs-lookup"><span data-stu-id="8e743-205">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="8e743-206">Para qualquer outro método HTTP, é usado um tempo limite de 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="8e743-206">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="8e743-207">Adicionar vários manipuladores do Polly</span><span class="sxs-lookup"><span data-stu-id="8e743-207">Add multiple Polly handlers</span></span>

<span data-ttu-id="8e743-208">É comum aninhar políticas do Polly para fornecer uma funcionalidade avançada:</span><span class="sxs-lookup"><span data-stu-id="8e743-208">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="8e743-209">No exemplo anterior, dois manipuladores são adicionados.</span><span class="sxs-lookup"><span data-stu-id="8e743-209">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="8e743-210">O primeiro usa a extensão `AddTransientHttpErrorPolicy` para adicionar uma política de repetição.</span><span class="sxs-lookup"><span data-stu-id="8e743-210">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="8e743-211">As solicitações com falha são repetidas até três vezes.</span><span class="sxs-lookup"><span data-stu-id="8e743-211">Failed requests are retried up to three times.</span></span> <span data-ttu-id="8e743-212">A segunda chamada a `AddTransientHttpErrorPolicy` adiciona uma política de disjuntor.</span><span class="sxs-lookup"><span data-stu-id="8e743-212">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="8e743-213">As solicitações externas adicionais são bloqueadas por 30 segundos quando ocorrem cinco tentativas com falha consecutivamente.</span><span class="sxs-lookup"><span data-stu-id="8e743-213">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="8e743-214">As políticas de disjuntor são políticas com estado.</span><span class="sxs-lookup"><span data-stu-id="8e743-214">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="8e743-215">Todas as chamadas por meio desse cliente compartilham o mesmo estado do circuito.</span><span class="sxs-lookup"><span data-stu-id="8e743-215">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="8e743-216">Adicionar políticas do registro do Polly</span><span class="sxs-lookup"><span data-stu-id="8e743-216">Add policies from the Polly registry</span></span>

<span data-ttu-id="8e743-217">Uma abordagem para gerenciar as políticas usadas com frequência é defini-las uma única vez e registrá-las em um `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="8e743-217">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="8e743-218">É fornecido um método de extensão que permite que um manipulador seja adicionado usando uma política do registro:</span><span class="sxs-lookup"><span data-stu-id="8e743-218">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="8e743-219">No código anterior, um PolicyRegistry é adicionado ao `ServiceCollection` e duas políticas são registradas nele.</span><span class="sxs-lookup"><span data-stu-id="8e743-219">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="8e743-220">Para usar uma política do registro, o método `AddPolicyHandlerFromRegistry` é usado, passando o nome da política a ser aplicada.</span><span class="sxs-lookup"><span data-stu-id="8e743-220">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="8e743-221">Mais informações sobre o `IHttpClientFactory` e as integrações do Polly podem ser encontradas no [Wiki do Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="8e743-221">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="8e743-222">HttpClient e gerenciamento de tempo de vida</span><span class="sxs-lookup"><span data-stu-id="8e743-222">HttpClient and lifetime management</span></span>

<span data-ttu-id="8e743-223">Toda vez que o `CreateClient` é chamado no `IHttpClientFactory`, uma nova instância de um `HttpClient` é retornada.</span><span class="sxs-lookup"><span data-stu-id="8e743-223">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="8e743-224">Haverá um `HttpMessageHandler` por cliente nomeado.</span><span class="sxs-lookup"><span data-stu-id="8e743-224">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="8e743-225">O `IHttpClientFactory` criará um pool com as instâncias de `HttpMessageHandler` criadas pelo alocador para reduzir o consumo de recursos.</span><span class="sxs-lookup"><span data-stu-id="8e743-225">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="8e743-226">Uma instância de `HttpMessageHandler` poderá ser reutilizada do pool ao criar uma nova instância de `HttpClient` se seu tempo de vida não tiver expirado.</span><span class="sxs-lookup"><span data-stu-id="8e743-226">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="8e743-227">O pooling de manipuladores é interessante porque cada manipulador normalmente gerencia suas próprias conexões de HTTP subjacentes. Criar mais manipuladores do que o necessário pode resultar em atrasos de conexão.</span><span class="sxs-lookup"><span data-stu-id="8e743-227">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="8e743-228">Alguns manipuladores também mantêm as conexões abertas indefinidamente, o que pode impedir que o manipulador reaja a alterações de DNS.</span><span class="sxs-lookup"><span data-stu-id="8e743-228">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="8e743-229">O tempo de vida padrão do manipulador é de 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="8e743-229">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="8e743-230">O valor padrão pode ser substituído para cada cliente nomeado.</span><span class="sxs-lookup"><span data-stu-id="8e743-230">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="8e743-231">Para substituí-lo, chame `SetHandlerLifetime` no `IHttpClientBuilder` que é retornado ao criar o cliente:</span><span class="sxs-lookup"><span data-stu-id="8e743-231">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="8e743-232">Registrando em log</span><span class="sxs-lookup"><span data-stu-id="8e743-232">Logging</span></span>

<span data-ttu-id="8e743-233">Os clientes criado pelo `IHttpClientFactory` registram mensagens de log para todas as solicitações.</span><span class="sxs-lookup"><span data-stu-id="8e743-233">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="8e743-234">Você precisará habilitar o nível apropriado de informações em sua configuração de registro em log para ver as mensagens de log padrão.</span><span class="sxs-lookup"><span data-stu-id="8e743-234">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="8e743-235">Os registros em log adicionais, como o registro em log dos cabeçalhos de solicitação, estão incluídos somente no nível de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="8e743-235">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="8e743-236">A categoria de log usada para cada cliente inclui o nome do cliente.</span><span class="sxs-lookup"><span data-stu-id="8e743-236">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="8e743-237">Um cliente chamado "MyNamedClient", por exemplo, registra mensagens com uma categoria de `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="8e743-237">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="8e743-238">As mensagens com o sufixo "LogicalHandler" ocorrem fora do pipeline do manipulador de solicitação.</span><span class="sxs-lookup"><span data-stu-id="8e743-238">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="8e743-239">Na solicitação, as mensagens são registradas em log antes que qualquer outro manipulador no pipeline a tenha processado.</span><span class="sxs-lookup"><span data-stu-id="8e743-239">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="8e743-240">Na resposta, as mensagens são registradas depois que qualquer outro manipulador do pipeline tenha recebido a resposta.</span><span class="sxs-lookup"><span data-stu-id="8e743-240">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="8e743-241">O registro em log também ocorre dentro do pipeline do manipulador de solicitação.</span><span class="sxs-lookup"><span data-stu-id="8e743-241">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="8e743-242">No caso do exemplo de "MyNamedClient", essas mensagens são registradas em log na categoria de log `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="8e743-242">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="8e743-243">Para a solicitação, isso ocorre depois que todos os outros manipuladores são executados e imediatamente antes que a solicitação seja enviada pela rede.</span><span class="sxs-lookup"><span data-stu-id="8e743-243">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="8e743-244">Na resposta, esse registro em log inclui o estado da resposta antes que ela seja retornada por meio do pipeline do manipulador.</span><span class="sxs-lookup"><span data-stu-id="8e743-244">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="8e743-245">Habilitar o registro em log fora e dentro do pipeline permite a inspeção das alterações feitas por outros manipuladores do pipeline.</span><span class="sxs-lookup"><span data-stu-id="8e743-245">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="8e743-246">Isso pode incluir, por exemplo, alterações nos cabeçalhos de solicitação ou no código de status da resposta.</span><span class="sxs-lookup"><span data-stu-id="8e743-246">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="8e743-247">Incluir o nome do cliente na categoria de log permite a filtragem de log para clientes nomeados específicos, quando necessário.</span><span class="sxs-lookup"><span data-stu-id="8e743-247">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="8e743-248">Configurar o HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="8e743-248">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="8e743-249">Talvez seja necessário controlar a configuração do `HttpMessageHandler` interno usado por um cliente.</span><span class="sxs-lookup"><span data-stu-id="8e743-249">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="8e743-250">Um `IHttpClientBuilder` é retornado ao adicionar clientes nomeados ou com tipo.</span><span class="sxs-lookup"><span data-stu-id="8e743-250">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="8e743-251">O método de extensão `ConfigurePrimaryHttpMessageHandler` pode ser usado para definir um representante.</span><span class="sxs-lookup"><span data-stu-id="8e743-251">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="8e743-252">O representante que é usado para criar e configurar o `HttpMessageHandler` primário usado pelo cliente:</span><span class="sxs-lookup"><span data-stu-id="8e743-252">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
