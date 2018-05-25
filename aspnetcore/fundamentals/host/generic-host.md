---
title: Host Genérico .NET
author: guardrex
description: Saiba mais sobre o Host Genérico no .NET, que é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/generic-host
ms.openlocfilehash: 15f4a689b2756d2bfb6320ab31f2e8d63af51a09
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
---
# <a name="net-generic-host"></a>Host Genérico .NET

Por [Luke Latham](https://github.com/guardrex)

Aplicativos ASP.NET Core configuram e inicializam um *host*. O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo. Este tópico aborda o Host Genérico do ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), que é útil para a hospedagem de aplicativos que não processam solicitações HTTP. Para cobertura do host da Web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), veja o tópico [Host da Web](xref:fundamentals/host/web-host).

O objetivo do Host Genérico é separar o pipeline HTTP da API de host da Web para permitir maior gama de cenários de host. Sistema de mensagens, tarefas em segundo plano e outras cargas de trabalho não HTTP com base no benefício do Host Genérico de recursos abrangentes, como configuração, DI (injeção de dependência) e log.

O Host Genérico é novo no ASP.NET Core 2.1 e não é adequado para cenários de hospedagem na Web. Para cenários de hospedagem na Web, use o [host da Web](xref:fundamentals/host/web-host). O Host Genérico está em desenvolvimento para substituir o host da Web em uma versão futura e atuar como API do host principal em cenários HTTP e não HTTP.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

Ao executar o aplicativo de exemplo no [Visual Studio Code](https://code.visualstudio.com/), use um *terminal externo ou integrado*. Não execute o exemplo em um `internalConsole`.

Para definir o console no Visual Studio Code:

1. Abra o arquivo *.vscode/launch.json*.
1. Na configuração **Inicialização do .NET Core (console)**, localize a entrada **console**. Defina o valor como `externalTerminal` ou `integratedTerminal`.

## <a name="introduction"></a>Introdução

A biblioteca do Host Genérico está disponível no [namespace Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) e é fornecida pelo [pacote NuGet Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/). O pacote `Microsoft.Extensions.Hosting` está incluído no metapacote [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) é o ponto de entrada para a execução de código. Cada implementação do `IHostedService` é executada na ordem do [registro de serviço em ConfigureServices](#configureservices). [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) é chamado em cada `IHostedService` quando o host é iniciado, e [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) é chamado na ordem inversa de registro quando o host é desligado normalmente.

## <a name="set-up-a-host"></a>Configurar um host

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) é o principal componente usado por aplicativos e bibliotecas para inicializar, compilar e executar o host:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a>Configuração do host

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) conta com as seguintes abordagens para definir os valores de configuração do host:

* Construtor de configuração
* Configuração do método de extensão

### <a name="configuration-builder"></a>Construtor de configuração

A configuração do construtor de host é criada chamando [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) na implementação [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder). `ConfigureHostConfiguration` usa um [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) para criar um [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para o host. O construtor de configuração inicializa o [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) para uso no processo de compilação do aplicativo. `ConfigureHostConfiguration` pode ser chamado várias vezes com resultados aditivos. O host usa a opção que define um valor por último.

*hostsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Exemplo da configuração `HostBuilder` usando `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> Atualmente, o método de extensão [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) não é capaz de analisar uma seção de configuração retornada por [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (por exemplo, `.AddConfiguration(Configuration.GetSection("section"))`). O método `GetSection` filtra as chaves de configuração da seção solicitada, mas deixa o nome da seção nas chaves (por exemplo, `section:environment`). O método `AddConfiguration` espera que as chaves correspondam às chaves `HostBuilder` (por exemplo, `environment`). A presença do nome da seção nas chaves impede que os valores da seção configurem o host. Esse problema será corrigido em uma próxima versão. Para obter mais informações e soluções alternativas, consulte [Passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).

### <a name="extension-method-configuration"></a>Configuração do método de extensão

Métodos de extensão são chamados na implementação `IHostBuilder` para configurar a raiz do conteúdo e o ambiente.

#### <a name="content-root"></a>Raiz do conteúdo

Essa configuração determina onde o host começa a procurar por arquivos de conteúdo.

**Chave**: contentRoot  
**Tipo**: *string*  
**Padrão**: o padrão é a pasta em que o assembly do aplicativo reside.  
**Definido usando**: `UseContentRoot`  
**Variável de ambiente**: `ASPNETCORE_CONTENTROOT`

Se o caminho não existir, o host não será iniciado.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>Ambiente

Define o [ambiente](xref:fundamentals/environments) do aplicativo.

**Chave**: ambiente  
**Tipo**: *string*  
**Padrão**: Production  
**Definido usando**: `UseEnvironment`  
**Variável de ambiente**: `ASPNETCORE_ENVIRONMENT`

O ambiente pode ser definido como qualquer valor. Os valores definidos pela estrutura incluem `Development`, `Staging` e `Production`. Os valores não diferenciam maiúsculas de minúsculas. Por padrão, o *Ambiente* é lido da variável de ambiente `ASPNETCORE_ENVIRONMENT`. Ao usar o [Visual Studio](https://www.visualstudio.com/), variáveis de ambiente podem ser definidas no arquivo *launchSettings.json*. Para obter mais informações, veja [Usar vários ambientes](xref:fundamentals/environments).

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

A configuração do construtor de aplicativos é criada chamando [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) na implementação [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder). `ConfigureAppConfiguration` usa um [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) para criar um [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para o aplicativo. `ConfigureAppConfiguration` pode ser chamado várias vezes com resultados aditivos. O aplicativo usa a opção que define um valor por último. A configuração criada pelo `ConfigureAppConfiguration` está disponível em [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) para operações seguintes e em [Serviços](/dotnet/api/microsoft.extensions.hosting.ihost.services).

Exemplo da configuração de aplicativo usando `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> Atualmente, o método de extensão [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) não é capaz de analisar uma seção de configuração retornada por [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (por exemplo, `.AddConfiguration(Configuration.GetSection("section"))`). O método `GetSection` filtra as chaves de configuração da seção solicitada, mas deixa o nome da seção nas chaves (por exemplo, `section:Logging:LogLevel:Default`). O método `AddConfiguration` espera uma correspondência exata para chaves de configuração (por exemplo, `Logging:LogLevel:Default`). A presença do nome da seção nas chaves impede que os valores da seção configurem o aplicativo. Esse problema será corrigido em uma próxima versão. Para obter mais informações e soluções alternativas, consulte [Passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).

## <a name="configureservices"></a>ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adiciona serviços ao contêiner [injeção de dependência](xref:fundamentals/dependency-injection) do aplicativo. `ConfigureServices` pode ser chamado várias vezes com resultados aditivos.

Um serviço hospedado é uma classe com lógica de tarefa em segundo plano que implementa a interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). Saiba mais no tópico [Tarefas em segundo plano com serviços hospedados](xref:fundamentals/host/hosted-services).

O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa o método de extensão `AddHostedService` para adicionar um serviço para eventos de tempo de vida, `LifetimeEventsHostedService`, e uma tarefa em segundo plano programada, `TimedHostedService`, para o aplicativo:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adiciona um delegado para configurar o [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) fornecido. `ConfigureLogging` pode ser chamado várias vezes com resultados aditivos.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) escuta `Ctrl+C`/SIGINT ou SIGTERM e chama [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) para iniciar o processo de desligamento. `UseConsoleLifetime` desbloqueia extensões como [RunAsync](#runasync) e [WaitForShutdownAsync](#waitforshutdownasync). [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) é previamente registrado como a implementação de tempo de vida padrão. O último tempo de vida registrado é usado.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Configuração do contêiner

Para dar suporte à conexão de outros contêineres, o host pode aceitar um [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1). O fornecimento de um alocador não faz parte do registro de contêiner de injeção de dependência, mas, em vez disso, um host intrínseco é usado para criar o contêiner de DI concreto. [UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) substitui o alocador padrão usado para criar o provedor de serviços do aplicativo.

A configuração de contêiner personalizado é gerenciada pelo método [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer). `ConfigureContainer` fornece uma experiência fortemente tipada para configurar o contêiner sobre a API do host subjacente. `ConfigureContainer` pode ser chamado várias vezes com resultados aditivos.

Crie um contêiner de serviço para o aplicativo:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Forneça um alocador de contêiner de serviço:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Use o alocador e configure o contêiner de serviço personalizado para o aplicativo:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Extensibilidade

Extensibilidade de host é executada com métodos de extensão em `IHostBuilder`. O exemplo a seguir mostra como um método de extensão estende uma implementação `IHostBuilder` com [RabbitMQ](https://www.rabbitmq.com/). O método de extensão (em outro lugar no aplicativo) registra um RabbitMQ `IHostedService`:

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a>Gerenciar o host

A implementação [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) é responsável por iniciar e parar as implementações `IHostedService` que estão registradas no contêiner de serviço.

### <a name="run"></a>Executar

[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) executa o aplicativo e bloqueia o thread de chamada até que o host seja desligado:

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a>RunAsync

[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) executa o aplicativo e retorna um `Task` que é concluído quando o token de cancelamento ou o desligamento é disparado:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a>RunConsoleAsync

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) habilita o suporte do console, compila e inicia o host e aguarda `Ctrl+C`/SIGINT ou SIGTERM desligar.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a>Start e StopAsync

[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) inicia o host de forma síncrona.

[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) tenta parar o host dentro do tempo limite oferecido.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a>StartAsync e StopAsync

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) inicia o aplicativo.

[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) para o aplicativo.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a>WaitForShutdown

[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) é disparado por meio de [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), como [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (escuta `Ctrl+C`/SIGINT ou SIGTERM). `WaitForShutdown` chama [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) retorna um `Task` que é concluído quando o desligamento é disparado por meio do token fornecido e chama [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a>Controle externo

O controle externo do host pode ser obtido usando os métodos que podem ser chamados externamente:

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) é chamado no início de [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), que aguarda até que ele seja concluído antes de continuar. Isso pode ser usado para atrasar a inicialização até que seja sinalizado por um evento externo.

## <a name="ihostingenvironment-interface"></a>Interface IHostingEnvironment

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) fornece informações sobre o ambiente de hospedagem do aplicativo. Use a [injeção de construtor](xref:fundamentals/dependency-injection) para obter o `IHostingEnvironment` para usar suas propriedades e métodos de extensão:

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

Para obter mais informações, veja [Usar vários ambientes](xref:fundamentals/environments).

## <a name="iapplicationlifetime-interface"></a>Interface IApplicationLifetime

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) permite atividades pós-inicialização e desligamento, inclusive solicitações de desligamento normal. Três propriedades na interface são tokens de cancelamento usados para registrar métodos `Action` que definem eventos de inicialização e desligamento.

| Token de cancelamento | Acionado quando&#8230; |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | O host foi iniciado totalmente. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | O host está concluindo um desligamento normal. Todas as solicitações devem ser processadas. O desligamento é bloqueado até que esse evento seja concluído. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | O host está executando um desligamento normal. Solicitações ainda podem estar sendo processadas. O desligamento é bloqueado até que esse evento seja concluído. |

O construtor injeta o serviço `IApplicationLifetime` em qualquer classe. O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa injeção de construtor em uma classe `LifetimeEventsHostedService` (uma implementação `IHostedService`) para registrar os eventos.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) solicita o término do aplicativo. A classe a seguir usa `StopApplication` para desligar normalmente um aplicativo quando o método `Shutdown` da classe é chamado:

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="additional-resources"></a>Recursos adicionais

* [Tarefas em segundo plano com serviços hospedados](xref:fundamentals/host/hosted-services)
* [Hospedagem de exemplos do repositório no GitHub](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
