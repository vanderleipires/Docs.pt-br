---
title: Hospedando em ASP.NET Core | Microsoft Docs
author: ardalis
description: "Introdução aos hosts da web em ASP.NET Core."
keywords: ASP.NET Core, host da web, IWebHost
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a>Introdução ao ASP.NET Core de hospedagem

Por [Steve Smith](http://ardalis.com)

Para executar um aplicativo ASP.NET Core, é necessário configurar e iniciar um host usando `WebHostBuilder`.

## <a name="what-is-a-host"></a>O que é um Host?

Aplicativos do ASP.NET Core exigem um *host* no qual executar. Um host deve implementar o `IWebHost` interface, que expõe a coleções de recursos e serviços, e um `Start` método. O host é normalmente criado usando uma instância de um `WebHostBuilder`, que cria e retorna um `WebHost` instância. O `WebHost` referencia o servidor que manipulará as solicitações. Saiba mais sobre [servidores](servers/index.md).

### <a name="what-is-the-difference-between-a-host-and-a-server"></a>Qual é a diferença entre um host e um servidor?

O host é responsável pelo gerenciamento de inicialização e o tempo de vida do aplicativo. O servidor é responsável para aceitar solicitações HTTP. Parte da responsabilidade do host inclui garantir a que serviços do aplicativo e o servidor estiverem disponível e adequadamente configurado. Você pode pensar o host como um wrapper em torno do servidor. O host está configurado para usar um servidor específico. o servidor não está ciente do seu host.

## <a name="setting-up-a-host"></a>Configurando um Host

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Criar um host usando uma instância de `WebHostBuilder`. Isso geralmente é feito no ponto de entrada do aplicativo: `public static void Main` (que nos modelos de projeto está localizado em um *Program.cs* arquivo). Um típico *Program.cs*, conforme mostrado abaixo, demonstra como usar um `WebHostBuilder` para criar um host.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]

O `WebHostBuilder` é responsável por criar o host que serão inicializar o servidor para o aplicativo. `WebHostBuilder`requer que você forneça um servidor que implementa `IServer` (`UseKestrel` no código acima). `UseKestrel`Especifica que o servidor de Kestrel será usado pelo aplicativo.

O servidor *conteúdo raiz* determina onde ele procura por arquivos de conteúdo, como modo de exibição MVC. A raiz de conteúdo padrão é a pasta na qual o aplicativo é executado.

> [!NOTE]
> Especificando `Directory.GetCurrentDirectory` como a raiz de conteúdo usarão pasta raiz do projeto da web como raiz de conteúdo do aplicativo quando o aplicativo for iniciado a partir desta pasta (por exemplo, chamar `dotnet run` da pasta de projeto da web). Esse é o padrão usado no Visual Studio e `dotnet new` modelos.

Para usar o IIS como um proxy reverso, chame `UseIISIntegration` como parte da construção do host. 

Observe que `UseIISIntegration` não configurar uma *servidor*, como `UseKestrel` does. Para usar o IIS com o ASP.NET Core, você deve especificar `UseKestrel` e `UseIISIntegration`. `UseKestrel`cria o servidor web e hospeda o aplicativo. `UseIISIntegration`examina as variáveis de ambiente usadas pelo IIS/IISExpress e define as configurações como a porta a ser escutada e os cabeçalhos para usar.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Criar um host usando uma instância de `WebHostBuilder`. Isso geralmente é feito no ponto de entrada do aplicativo: `public static void Main` (que nos modelos de projeto está localizado em um *Program.cs* arquivo). Um típico *Program.cs*, conforme mostrado abaixo, chamadas `CreateDefaultbuilder` para criar um host:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

`CreateDefaultbuilder`cria uma instância de `WebHostBuilder` para criar o host que inicializa o servidor para o aplicativo. O host requer uma [server que implementa IServer](servers/index.md). Os servidores internos são [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` usam Kestrel por padrão.

`CreateDefaultbuilder`executa tarefas de configuração, além de configurar Kestrel como o servidor web:

* Define a raiz de conteúdo `Directory.GetCurrentDirectory`.
* Configuração de cargas de:
  * *appSettings. JSON*
  * *appSettings. \<EnvironmentName >. JSON*.
  * segredos do usuário quando o aplicativo é executado no ambiente de desenvolvimento
  * variáveis de ambiente
  * argumentos de linha de comando fornecidos
* Configura o registro para a saída do console e de depuração, com as regras especificadas em uma seção de configuração de log de filtragem.
* Permite a integração do IIS.
* Adiciona a página de exceção do desenvolvedor quando o aplicativo é executado no ambiente de desenvolvimento.

O servidor *conteúdo raiz* determina onde ele procura por arquivos de conteúdo, como modo de exibição MVC. A raiz de conteúdo padrão é a pasta na qual o aplicativo é executado.

> [!NOTE]
> Especificando `Directory.GetCurrentDirectory` como a raiz de conteúdo usarão pasta raiz do projeto da web como raiz de conteúdo do aplicativo quando o aplicativo for iniciado a partir desta pasta (por exemplo, chamar `dotnet run` da pasta de projeto da web). Esse é o padrão usado no Visual Studio e `dotnet new` modelos.

Quando você usar o IIS como um proxy reverso, ASP.NET Core automaticamente chama `UseIISIntegration` como parte da construção do host. Para obter mais informações, consulte [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module).

Observe que `UseIISIntegration` não configurar uma *servidor*, como `UseKestrel` does. `UseKestrel`cria o servidor web e hospeda o aplicativo. `UseIISIntegration`examina as variáveis de ambiente usadas pelo IIS/IISExpress e define as configurações como a porta a ser escutada e os cabeçalhos para usar.

---

Uma implementação mínima de configuração de um host (e um aplicativo ASP.NET Core) inclui apenas um servidor e a configuração do canal de solicitação do aplicativo:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> Ao configurar um host, você pode fornecer `Configure` e `ConfigureServices` métodos, em vez de ou além de especificar um `Startup` classe (que também deve definir esses métodos - consulte [inicialização do aplicativo](startup.md)). Diversas chamadas para `ConfigureServices` acrescentará um ao outro; chamadas para `Configure` ou `UseStartup` substituirá as configurações anteriores.

## <a name="configuring-a-host"></a>Configurando um Host

O `WebHostBuilder` fornece métodos para definir a maioria dos valores de configuração disponíveis para o host, que também pode ser definido diretamente usando `UseSetting` e a chave associada.

### <a name="host-configuration-values"></a>Valores de configuração do host

**Capturar erros de inicialização**`bool`

Chave: `captureStartupErrors`. Assume o padrão de `false`. Quando `false`, erros durante o resultado de inicialização no host sair. Quando `true`, o host irá capturar todas as exceções do `Startup` classe e tente iniciar o servidor. Ele exibirá uma página de erro (genérico ou detalhados, com base na configuração de erros detalhados, abaixo) para cada solicitação. Definido usando o `CaptureStartupErrors` método.

Observação: Quando seu aplicativo é executado com Kestrel e o IIS, o comportamento padrão é capturar erros de inicialização. 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

**Raiz de conteúdo**`string`

Chave: `contentRoot`. O padrão é a pasta onde o assembly de aplicativo reside (para Kestrel; IIS usará a raiz do projeto web por padrão). Essa configuração determina onde ASP.NET Core começará a procurar pelos arquivos de conteúdo, como exibições do MVC. Também é usado como o caminho base para o [Web raiz configuração](#web-root-setting). Definido usando o `UseContentRoot` método. Caminho deve existir ou o host não será iniciado.

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

**Erros detalhados**`bool`

Chave: `detailedErrors`. Assume o padrão de `false`. Quando `true` (ou quando o ambiente está definido como "Desenvolvimento"), o aplicativo exibirá detalhes de exceções de inicialização, em vez de apenas uma página de erro genérica. Definido usando `UseSetting`.

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

Quando erros detalhados está definido como `false` e capturar erros de inicialização é `true`, uma página de erro genérica é exibida em resposta a cada solicitação ao servidor.

![Página de erro genérica](hosting/_static/generic-error-page.png)

Quando erros detalhados está definido como `true` e capturar erros de inicialização é `true`, uma página de erro detalhada é exibida em resposta a cada solicitação ao servidor.

![Página de erro detalhadas](hosting/_static/detailed-error-page.png)

**Ambiente**`string`

Chave: `environment`. O padrão é "Produção". Pode ser definida como qualquer valor. Valores definidos pelo Framework incluem "Desenvolvimento", "Preparação" e "Produção". Valores não diferenciam maiusculas de minúsculas. Consulte [trabalhando com vários ambientes](environments.md). Definido usando o `UseEnvironment` método.

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> Por padrão, o ambiente é lido a partir de `ASPNETCORE_ENVIRONMENT` variável de ambiente. Ao usar o Visual Studio, as variáveis de ambiente podem ser definidas no *launchSettings.json* arquivo.

<a id="server-urls"></a>

**URLs de servidor**`string`

Chave: `urls`. Definido como um ponto e vírgula (;) separados de prefixos de lista de URL para o servidor deve responder. Por exemplo, `http://localhost:123`. O nome do domínio/host pode ser substituído por "\*" para indicar que o servidor deve escutar solicitações em qualquer endereço IP ou usando a porta especificada e o protocolo de host (por exemplo, `http://*:5000` ou `https://*:5001`). O protocolo (`http://` ou `https://`) devem ser incluídas com cada URL. Os prefixos são interpretados pelo servidor configurado; os formatos com suporte variam entre servidores.

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

No ASP.NET 2.0 de núcleo, Kestrel tem sua própria API de configuração do ponto de extremidade e não oferece suporte a `https://` no `urls` cadeia de caracteres. Para obter mais informações, consulte [Introdução ao Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

**Assembly de inicialização**`string`

Chave: `startupAssembly`. Determina o assembly para pesquisar o `Startup` classe. Definido usando o `UseStartup` método. Em vez disso, podem fazer referência tipo específico usando `WebHostBuilder.UseStartup<StartupType>`. Se vários `UseStartup` métodos são chamados, último terá precedência.

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

**Web raiz**`string`

Chave: `webroot`. Se não especificado, o padrão é `(Content Root Path)\wwwroot`, se ele existir. Se esse caminho não existir, é usado um provedor de arquivo não-operacional. Definido usando `UseWebRoot`.

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a>Substituindo a configuração

Use [configuração](configuration.md) para definir valores de configuração a ser usado pelo host. Esses valores podem ser substituídos posteriormente. Isso é especificado usando `UseConfiguration`.

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

No exemplo acima, os argumentos de linha de comando podem ser passados para configurar o host ou definições de configuração, opcionalmente, podem ser especificadas em uma *hosting.json* arquivo. Para especificar o host executado em uma URL específica, você pode transmitir o valor desejado de um prompt de comando:

```console
dotnet run --urls "http://*:5000"
```

O `Run` método inicia o aplicativo web e bloqueia o thread de chamada até que o host é desligado.

```csharp
host.Run();
```

Você pode executar o host de maneira sem bloqueio, chamando seu `Start` método:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Passar uma lista de URLs para o `Start` método e ele escutará em URLs especificadas:

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

Os formatos de URL que são válidos aqui dependem do servidor que você está usando. Para obter mais informações, consulte [URLs de servidor](#server-urls) anteriormente neste artigo.

> [!NOTE]
> O `UseConfiguration` método de extensão não está atualmente capaz de analisar uma seção de configuração retornada por `GetSection` (por exemplo, `.UseConfiguration(Configuration.GetSection("section"))`. O `GetSection` método filtra as chaves de configuração para a seção solicitada, mas deixa o nome da seção sobre as chaves (por exemplo, `section:urls`, `section:environment`). O `UseConfiguration` método espera que as chaves para corresponder a `WebHostBuilder` chaves (por exemplo, `urls`, `environment`). A presença do nome da seção nas chaves impede que os valores da seção Configurando o host. Esse problema será corrigido em uma próxima versão. Para obter mais informações e soluções alternativas, consulte [passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).

### <a name="ordering-importance"></a>Ordem de importância

`WebHostBuilder`as configurações são lidas primeiro de determinadas variáveis de ambiente, se definido. Essas variáveis de ambiente devem usar o formato `ASPNETCORE_{configurationKey}`, portanto, por exemplo para definir as URLs de servidor escutará em por padrão, você definiria `ASPNETCORE_URLS`.

Você pode substituir qualquer um desses valores de variável de ambiente com a especificação de configuração (usando `UseConfiguration`) ou definindo o valor explicitamente (usando `UseUrls` para a instância). O host usará qualquer opção define o valor da última. Por esse motivo, `UseIISIntegration` devem aparecer após `UseUrls`, porque ele substitui o URL com um dinamicamente fornecida pelo IIS. Se você quiser programaticamente definir a URL padrão para um valor, mas permitir que ele seja substituído com a configuração, você poderá configurar o host da seguinte maneira:

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a>Recursos adicionais

* [Publicar no Windows usando o IIS](../publishing/iis.md)
* [Publicar no Linux usando Nginx](../publishing/linuxproduction.md)
* [Publicar no Linux usando o Apache](../publishing/apache-proxy.md)
* [Host em um serviço do Windows](xref:hosting/windows-service)

