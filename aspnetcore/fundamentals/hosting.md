---
title: "Hospedagem no núcleo do ASP.NET"
author: guardrex
description: "Saiba mais sobre o host da web em ASP.NET Core, que é responsável pelo gerenciamento de inicialização e o tempo de vida do aplicativo."
keywords: Host, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime da web do ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/10/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1a789ff1bc6b3e3af99419e7d74d3fb46bb2345
ms.sourcegitcommit: 368aabde4de3728a8e5a8c016a2ec61f9c0854bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="hosting-in-aspnet-core"></a>Hospedagem no núcleo do ASP.NET

Por [Luke Latham](https://github.com/guardrex)

Aplicativos ASP.NET Core configurar e iniciar um *host*, que é responsável pelo gerenciamento de inicialização e o tempo de vida do aplicativo. No mínimo, o host configura um servidor e um pipeline de processamento de solicitação.

## <a name="setting-up-a-host"></a>Configurando um host

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Criar um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Isso normalmente é executado no ponto de entrada do aplicativo, o `Main` método. Nos modelos de projeto, `Main` está localizado em *Program.cs*. Um típico *Program.cs* chamadas [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para começar a configurar um host:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder`executa as seguintes tarefas:

* Configura [Kestrel](servers/kestrel.md) como o servidor web.
* Define a raiz de conteúdo [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Configuração opcional de cargas de:
  * *appSettings. JSON*.
  * *appSettings. . JSON de {ambiente}*.
  * [Segredos do usuário](xref:security/app-secrets) quando o aplicativo é executado no `Development` ambiente.
  * Variáveis de ambiente.
  * Argumentos de linha de comando.
* Configura [log](xref:fundamentals/logging) para a saída do console e de depuração com [filtragem de log](xref:fundamentals/logging#log-filtering) regras especificadas em uma seção de configuração de log de um *appSettings. JSON* ou *appsettings. . JSON de {ambiente}* arquivo.
* Quando em execução por trás do IIS, permite a integração do IIS, configurando o caminho base e a porta que o servidor deve escutar ao usar o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module). O módulo cria um proxy reverso entre Kestrel e o IIS. Também configura o aplicativo [capturar erros de inicialização](#capture-startup-errors).

O *conteúdo raiz* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC. A raiz de conteúdo padrão é [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory). Isso resulta em usando a pasta raiz do projeto da web como a raiz de conteúdo quando o aplicativo é iniciado na pasta raiz (por exemplo, chamar [dotnet executar](/dotnet/core/tools/dotnet-run) da pasta de projeto). Esse é o padrão usado em [Visual Studio](https://www.visualstudio.com/) e [dotnet novos modelos](/dotnet/core/tools/dotnet-new).

Consulte [configuração no ASP.NET Core](xref:fundamentals/configuration) para obter mais informações sobre a configuração do aplicativo.

> [!NOTE]
> Como uma alternativa ao uso estático `CreateDefaultBuilder` método, criando um host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) é uma abordagem com suporte com o ASP.NET Core 2. x. Consulte a guia de 1. x do ASP.NET Core para obter mais informações.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Criar um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Isso normalmente é executado no ponto de entrada do aplicativo, o `Main` método. Nos modelos de projeto, `Main` está localizado em *Program.cs*. O seguinte *Program.cs* demonstra como usar `WebHostBuilder` para criar o host:

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder`requer um [server que implementa IServer](servers/index.md). Os servidores internos são [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (antes da versão do ASP.NET Core 2.0, o HTTP.sys foi chamado [WebListener](xref:fundamentals/servers/weblistener)). Neste exemplo, o [o método de extensão UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Especifica o servidor Kestrel.

O *conteúdo raiz* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC. A raiz de conteúdo padrão fornecido a `UseContentRoot` é [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Isso resulta em usando a pasta raiz do projeto da web como a raiz de conteúdo quando o aplicativo é iniciado na pasta raiz (por exemplo, chamar [dotnet executar](/dotnet/core/tools/dotnet-run) da pasta de projeto). Esse é o padrão usado em [Visual Studio](https://www.visualstudio.com/) e [dotnet novos modelos](/dotnet/core/tools/dotnet-new).

Para usar o IIS como um proxy reverso, chame [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte da construção do host. `UseIISIntegration`não configure um *servidor*, como [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does. `UseIISIntegration`configura o caminho base e a porta que o servidor deve escutar ao usar o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) para criar um proxy reverso entre Kestrel e o IIS. Para usar o IIS com o ASP.NET Core, você deve especificar `UseKestrel` e `UseIISIntegration`. `UseIISIntegration`ativa somente quando em execução por trás do IIS ou IIS Express. Para obter mais informações, consulte [Introdução ao ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) e [referência de configuração do módulo do ASP.NET Core](xref:hosting/aspnet-core-module).

Uma implementação mínima que configura um host (e um aplicativo ASP.NET Core) inclui a especificação de um servidor e a configuração do canal de solicitação do aplicativo:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

Ao configurar um host, você pode fornecer [configurar](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) métodos. Se você especificar um `Startup` classe, deve definir um `Configure` método. Para obter mais informações, consulte [inicialização do aplicativo no ASP.NET Core](startup.md). Diversas chamadas para `ConfigureServices` acrescentar um ao outro. Diversas chamadas para `Configure` ou `UseStartup` no `WebHostBuilder` substituir as configurações anteriores.

## <a name="host-configuration-values"></a>Valores de configuração do host

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) fornece métodos para definir a maioria dos valores de configuração disponíveis para o host, que também pode ser definida diretamente com [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e a chave associada. Ao definir um valor com `UseSetting`, o valor é definido como uma cadeia de caracteres (entre aspas), independentemente do tipo.

### <a name="capture-startup-errors"></a>Capturar erros de inicialização

Essa configuração controla a captura de erros de inicialização.

**Chave**: captureStartupErrors  
**Tipo**: *bool* (`true` ou `1`)  
**Padrão**: assume como padrão `false` , a menos que o aplicativo é executado com Kestrel por trás do IIS, em que o padrão é `true`.  
**Definido usando**:`CaptureStartupErrors`

Quando `false`, erros durante o resultado de inicialização no host sair. Quando `true`, o host captura exceções durante a inicialização e tenta iniciar o servidor.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>Raiz do conteúdo

Essa configuração determina onde o ASP.NET Core começa a procurar por arquivos de conteúdo, como exibições do MVC. 

**Chave**: contentRoot  
**Tipo**: *cadeia de caracteres*  
**Padrão**: padrão é a pasta em que reside o assembly de aplicativo.  
**Definido usando**:`UseContentRoot`

A conteúdo raiz também é usada como o caminho base para o [configuração Web raiz](#web-root). Se o caminho não existir, o host não será iniciado.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>Erros detalhados

Determina se detalhado erros devem ser capturados.

**Chave**: detailedErrors  
**Tipo**: *bool* (`true` ou `1`)  
**Padrão**: falso  
**Definido usando**:`UseSetting`

Quando habilitado (ou quando o <a href="#environment">ambiente</a> é definido como `Development`), o aplicativo captura exceções detalhadas.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>Ambiente

Define o ambiente do aplicativo.

**Chave**: ambiente  
**Tipo**: *cadeia de caracteres*  
**Padrão**: produção  
**Definido usando**:`UseEnvironment`

Você pode definir o *ambiente* para qualquer valor. Incluem valores definidos pelo Framework `Development`, `Staging`, e `Production`. Valores não diferencia maiusculas de minúsculas. Por padrão, o *ambiente* são lidos a partir de `ASPNETCORE_ENVIRONMENT` variável de ambiente. Ao usar [Visual Studio](https://www.visualstudio.com/), variáveis de ambiente podem ser definidas no *launchSettings.json* arquivo. Para obter mais informações, consulte [Trabalhando com vários ambientes](xref:fundamentals/environments).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>Assemblies de inicialização de hospedagem

Define a hospedagem assemblies de inicialização do aplicativo.

**Chave**: hostingStartupAssemblies  
**Tipo**: *cadeia de caracteres*  
**Padrão**: cadeia de caracteres vazia  
**Definido usando**:`UseSetting`

Uma cadeia delimitada por ponto e vírgula de hospedagem assemblies de inicialização para carregar na inicialização. Esse recurso é novo no ASP.NET 2.0 de núcleo.

Embora o valor de configuração padrão é uma cadeia de caracteres vazia, os assemblies de inicialização hospedagem sempre incluem o assembly do aplicativo. Quando você fornece hospedagem assemblies de inicialização, elas são adicionadas ao assembly do aplicativo de carregamento quando o aplicativo cria seus serviços comuns durante a inicialização.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Este recurso está indisponível no núcleo do ASP.NET 1. x.

---

### <a name="prefer-hosting-urls"></a>Preferir URLs de hospedagem

Indica se o host deve escutar nas URLs configuradas com o `WebHostBuilder` em vez das configuradas com o `IServer` implementação.

**Chave**: preferHostingUrls  
**Tipo**: *bool* (`true` ou `1`)  
**Padrão**: true  
**Definido usando**:`PreferHostingUrls`

Esse recurso é novo no ASP.NET 2.0 de núcleo.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Este recurso está indisponível no núcleo do ASP.NET 1. x.

---

### <a name="prevent-hosting-startup"></a>Impedir a inicialização de hospedagem

Impede o carregamento automático de assemblies de inicialização, incluindo o assembly do aplicativo de hospedagem.

**Chave**: preventHostingStartup  
**Tipo**: *bool* (`true` ou `1`)  
**Padrão**: falso  
**Definido usando**:`UseSetting`

Esse recurso é novo no ASP.NET 2.0 de núcleo.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Este recurso está indisponível no núcleo do ASP.NET 1. x.

---

### <a name="server-urls"></a>URLs de servidor

Indica os endereços IP ou endereços de host com as portas e protocolos que o servidor deve ouvir para solicitações.

**Chave**: urls  
**Tipo**: *cadeia de caracteres*  
**Padrão**: http://localhost:5000/  
**Definido usando**:`UseUrls`

Definir um separados por ponto e vírgula (;) prefixos de lista de URL para o servidor deve responder. Por exemplo, `http://localhost:123`. Use "\*" para indicar que o servidor deve escutar solicitações em qualquer endereço IP ou nome do host usando o protocolo e porta especificada (por exemplo, `http://*:5000`). O protocolo (`http://` ou `https://`) devem ser incluídas com cada URL. Os formatos com suporte variam entre servidores.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel tem sua própria API de configuração do ponto de extremidade. Para obter mais informações, consulte [Implementação do servidor Web Kestrel no ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>Tempo limite de desligamento

Especifica a quantidade de tempo de espera para o host da web para o desligamento.

**Chave**: shutdownTimeoutSeconds  
**Tipo**: *int*  
**Padrão**: 5  
**Definido usando**:`UseShutdownTimeout`

Embora a chave aceita um *int* com `UseSetting` (por exemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), o `UseShutdownTimeout` método de extensão usa um `TimeSpan`. Esse recurso é novo no ASP.NET 2.0 de núcleo.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Este recurso está indisponível no núcleo do ASP.NET 1. x.

---

### <a name="startup-assembly"></a>Assembly de inicialização

Determina o assembly para pesquisar o `Startup` classe.

**Chave**: startupAssembly  
**Tipo**: *cadeia de caracteres*  
**Padrão**: assembly do aplicativo  
**Definido usando**:`UseStartup`

Você pode fazer referência ao assembly por nome (`string`) ou tipo (`TStartup`). Se vários `UseStartup` métodos são chamados, último terá precedência.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a>Raiz da Web

Define o caminho relativo para ativos estático do aplicativo.

**Chave**: webroot  
**Tipo**: *cadeia de caracteres*  
**Padrão**: se não for especificado, o padrão é "(Content Root)/wwwroot", se o caminho existe. Se o caminho não existir, é usado um provedor de arquivo não-operacional.  
**Definido usando**:`UseWebRoot`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>Configuração de substituição

Use [configuração](configuration.md) para configurar o host. No exemplo a seguir, a configuração do host é especificada, opcionalmente, em um *hosting.json* arquivo. Qualquer configuração carregados a partir de *hosting.json* arquivo pode ser substituído pelos argumentos de linha de comando. A configuração interna (no `config`) é usado para configurar o host com `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*Hosting.JSON*:

```json
{
    urls: "http://*:5005"
}
```

Substituindo a configuração fornecida pelo `UseUrls` com *hosting.json* config primeiro argumento de linha de comando config segundo:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*Hosting.JSON*:

```json
{
    urls: "http://*:5005"
}
```

Substituindo a configuração fornecida pelo `UseUrls` com *hosting.json* config primeiro argumento de linha de comando config segundo:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> O `UseConfiguration` método de extensão não está atualmente capaz de analisar uma seção de configuração retornada por `GetSection` (por exemplo, `.UseConfiguration(Configuration.GetSection("section"))`. O `GetSection` método filtra as chaves de configuração para a seção solicitada, mas deixa o nome da seção sobre as chaves (por exemplo, `section:urls`, `section:environment`). O `UseConfiguration` método espera que as chaves para corresponder a `WebHostBuilder` chaves (por exemplo, `urls`, `environment`). A presença do nome da seção nas chaves impede que os valores da seção Configurando o host. Esse problema será corrigido em uma próxima versão. Para obter mais informações e soluções alternativas, consulte [passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).

Para especificar o host executado em uma URL específica, você pode transmitir o valor desejado de um prompt de comando ao executar `dotnet run`. O argumento de linha de comando substitui o `urls` valor o *hosting.json* arquivo e o servidor escuta na porta 8080:

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a>Ordem de importância

Alguns do `WebHostBuilder` configurações primeiro são lidos a partir de variáveis de ambiente, se definido. Essas variáveis de ambiente usam o formato `ASPNETCORE_{configurationKey}`. Para definir as URLs que o servidor escuta em por padrão, você deve definir `ASPNETCORE_URLS`.

Você pode substituir qualquer um desses valores de variável de ambiente com a especificação de configuração (usando `UseConfiguration`) ou definindo o valor explicitamente (usando `UseSetting` ou um dos métodos de extensão explícito, como `UseUrls`). O host usa qualquer opção define o valor da última. Se você deseja definir a URL padrão para um valor programaticamente, mas permitir que ele seja substituído com a configuração, você pode usar a configuração de linha de comando depois de definir a URL. Consulte [configuração substituindo](#overriding-configuration).

## <a name="starting-the-host"></a>Iniciando o host

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**Executar**

O `Run` método inicia o aplicativo web e bloqueia o thread de chamada até que o host é desligado:

```csharp
host.Run();
```

**Iniciar**

Você pode executar o host de maneira sem bloqueio, chamando seu `Start` método:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Se você passar uma lista de URLs para o `Start` método, ele escuta em URLs especificadas:

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

Você pode inicializar e iniciar um novo host usando os padrões pré-configurado de `CreateDefaultBuilder` usando um método estático de conveniência. Esses métodos de iniciar o servidor sem saída de console e [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) aguardar uma quebra (Ctrl-C/SIGINT ou SIGTERM):

**Início (RequestDelegate app)**

Iniciar com um `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Fazer uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Hello World!" `WaitForShutdown`blocos até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida. O aplicativo é exibido o `Console.WriteLine` mensagem e aguarda um pressionamento de tecla sair.

**Iniciar (cadeia de caracteres de url, RequestDelegate do aplicativo)**

Iniciar com uma URL e `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produz o mesmo resultado que **inicial (aplicativo RequestDelegate)**, exceto o aplicativo responde em `http://localhost:8080`.

**Iniciar (ação<IRouteBuilder> routeBuilder)**

Usar uma instância de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar o middleware de roteamento:

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Use as seguintes solicitações de navegador com o exemplo:

| Solicitação                                    | Resposta                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Olá, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Dias de Buenos, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | Gera uma exceção com a cadeia de caracteres "ooops!" |
| `http://localhost:5000/throw`              | Gera uma exceção com a cadeia de caracteres "Uh oh!" |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Olá, mundo!                             |

`WaitForShutdown`blocos até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida. O aplicativo é exibido o `Console.WriteLine` mensagem e aguarda um pressionamento de tecla sair.

**Iniciar (url, a ação de cadeia de caracteres<IRouteBuilder> routeBuilder)**

Use uma URL e uma instância do `IRouteBuilder`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produz o mesmo resultado que **iniciar (ação<IRouteBuilder> routeBuilder)**, exceto o aplicativo responde em `http://localhost:8080`.

**StartWith (ação<IApplicationBuilder> aplicativo)**

Forneça um delegado para configurar um `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Fazer uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Hello World!" `WaitForShutdown`blocos até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida. O aplicativo é exibido o `Console.WriteLine` mensagem e aguarda um pressionamento de tecla sair.

**StartWith (url, a ação de cadeia de caracteres<IApplicationBuilder> aplicativo)**

Forneça uma URL e um delegado para configurar um `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produz o mesmo resultado que **StartWith (ação<IApplicationBuilder> aplicativo)**, exceto o aplicativo responde em `http://localhost:8080`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**Executar**

O `Run` método inicia o aplicativo web e bloqueia o thread de chamada até que o host é desligado:

```csharp
host.Run();
```

**Iniciar**

Você pode executar o host de maneira sem bloqueio, chamando seu `Start` método:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Se você passar uma lista de URLs para o `Start` método, ele escuta em URLs especificadas:


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a>Interface IHostingEnvironment

O [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornece informações sobre o ambiente de hospedagem na web do aplicativo. Você pode usar [injeção de construtor](xref:fundamentals/dependency-injection) para obter o `IHostingEnvironment` para usar suas propriedades e métodos de extensão:

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

Você pode usar um [abordagem baseado em convenção](xref:fundamentals/environments#startup-conventions) para configurar seu aplicativo na inicialização com base no ambiente. Como alternativa, você pode injetar o `IHostingEnvironment` para o `Startup` construtor para uso em `ConfigureServices`:

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> Além de `IsDevelopment` método de extensão, `IHostingEnvironment` oferece `IsStaging`, `IsProduction`, e `IsEnvironment(string environmentName)` métodos. Consulte [trabalhando com vários ambientes](xref:fundamentals/environments) para obter detalhes.

O `IHostingEnvironment` serviço também pode ser inserido diretamente para o `Configure` método para configurar o pipeline de processamento:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

Você pode injetar `IHostingEnvironment` para o `Invoke` método ao criar personalizado [middleware](xref:fundamentals/middleware#writing-middleware):

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a>Interface IApplicationLifetime

O [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite executar atividades de pós-inicialização e desligamento. Três propriedades na interface são tokens de cancelamento que você pode registrar com `Action` métodos para definir os eventos de inicialização e desligamento. Também há um `StopApplication` método.

| Token de cancelamento    | Acionado quando &#8230; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | O host foi totalmente iniciado. |
| `ApplicationStopping` | O host está executando um desligamento normal. Ainda podem estar processando solicitações. Blocos de desligamento até que esse evento seja concluída. |
| `ApplicationStopped`  | O host está concluindo um desligamento normal. Todas as solicitações devem ser processadas completamente. Blocos de desligamento até que esse evento seja concluída. |

| Método            | Ação                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | Encerramento de solicitações do aplicativo atual. |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a>Solucionando problemas de System. ArgumentException

**Aplica-se ao ASP.NET Core 2.0 apenas**

Se você compilar o host injetando `IStartup` diretamente para o contêiner de injeção de dependência em vez de chamar `UseStartup` ou `Configure`, você pode encontrar o seguinte erro: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.

Isso ocorre porque o [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (do assembly atual) é necessário para verificar se há `HostingStartupAttributes`. Se você inserir manualmente `IStartup` para o contêiner de injeção de dependência, adicione a seguinte chamada a sua `WebHostBuilder` com o nome do assembly especificado:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

Como alternativa, adicione uma cópia `Configure` para sua `WebHostBuilder`, que define o `applicationName`(`ApplicationKey`) automaticamente:

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**Observação**: isso só é necessária com a versão 2.0 do ASP.NET Core e somente quando você não chama `UseStartup` ou `Configure`.

Para obter mais informações, consulte [anúncios: Microsoft.Extensions.PlatformAbstractions foi removido (comentário)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e [StartupInjection exemplo](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).

## <a name="additional-resources"></a>Recursos adicionais

* [Publicar no Windows usando o IIS](../publishing/iis.md)
* [Publicar no Linux usando Nginx](../publishing/linuxproduction.md)
* [Publicar no Linux usando o Apache](../publishing/apache-proxy.md)
* [Host em um serviço do Windows](xref:hosting/windows-service)
