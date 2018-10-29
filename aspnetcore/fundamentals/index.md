---
title: Conceitos básicos do ASP.NET Core
author: rick-anderson
description: Descubra os conceitos fundamentais para a criação de aplicativos do ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/index
ms.openlocfilehash: 56344315acc59003248ffaf1e61455b94a93a545
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090713"
---
# <a name="aspnet-core-fundamentals"></a>Conceitos básicos do ASP.NET Core

Um aplicativo ASP.NET Core é um aplicativo de console que cria um servidor Web em seu método `Program.Main`. O método `Main` é o *ponto de entrada gerenciado* do aplicativo:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

O host do .NET Core:

* Carrega o [tempo de execução do .NET Core](https://github.com/dotnet/coreclr).
* Usa o primeiro argumento de linha de comando como o caminho para o binário gerenciado que contém o ponto de entrada (`Main`) e inicia a execução do código.

O método `Main` invoca [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), que segue o [padrão de construtor](https://wikipedia.org/wiki/Builder_pattern) para criar um host da Web. O construtor tem métodos que definem o servidor Web (por exemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) e a classe de inicialização (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). No exemplo anterior, o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é alocado automaticamente. O host Web do ASP.NET Core tenta executar no IIS, se disponível. Outros servidores Web como [HTTP.sys](xref:fundamentals/servers/httpsys) podem ser usados ao chamar o método de extensão apropriado. `UseStartup` é explicado em mais detalhes na próxima seção.

<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, o tipo de retorno da invocação de `WebHost.CreateDefaultBuilder`, fornece muitos métodos opcionais. Alguns desses métodos incluem `UseHttpSys` para hospedar o aplicativo em HTTP.sys e <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar o diretório de conteúdo raiz. Os métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> e <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> compilam o objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda o aplicativo e começa a escutar solicitações HTTP.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

O host do .NET Core:

* Carrega o [tempo de execução do .NET Core](https://github.com/dotnet/coreclr).
* Usa o primeiro argumento de linha de comando como o caminho para o binário gerenciado que contém o ponto de entrada (`Main`) e inicia a execução do código.

O método `Main` usa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, que segue o [padrão de construtor](https://wikipedia.org/wiki/Builder_pattern) para criar um host de aplicativo Web. O construtor tem métodos que definem o servidor Web (por exemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) e a classe de inicialização (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). No exemplo anterior, o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é usado. Outros servidores Web como [WebListener](xref:fundamentals/servers/weblistener) podem ser usados ao chamar o método de extensão apropriado. `UseStartup` é explicado em mais detalhes na próxima seção.

O `WebHostBuilder` fornece muitos métodos opcionais, incluindo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>, para hospedagem no IIS e no IIS Express, e <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>, para especificar o diretório do conteúdo raiz. Os métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> e <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> compilam o objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda o aplicativo e começa a escutar solicitações HTTP.

::: moniker-end

## <a name="startup"></a>Inicialização

O método `UseStartup` em `WebHostBuilder` especifica a classe `Startup` para seu aplicativo:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

É na classe `Startup` que você define o pipeline de tratamento de solicitação e é nela que todos os serviços que o aplicativo precisa estão configurados. A classe `Startup` deve ser pública e conter os seguintes métodos:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> define os [Serviços](#dependency-injection-services) usados pelo seu aplicativo (por exemplo, ASP.NET Core MVC, Entity Framework Core, Identity etc.). <xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> define o [middleware](xref:fundamentals/middleware/index) chamado no pipeline de solicitação.

Para obter mais informações, consulte <xref:fundamentals/startup>.

## <a name="content-root"></a>Raiz do conteúdo

A raiz do conteúdo é o caminho base para qualquer conteúdo usado pelo aplicativo, tal como [Razor Pages](xref:razor-pages/index), exibições do MVC e ativos estáticos. Por padrão, a raiz do conteúdo é a mesma localização que o caminho base do aplicativo para o executável que hospeda o aplicativo.

## <a name="web-root-webroot"></a>Diretório base (webroot)

O webroot de um aplicativo é o diretório do projeto que contém recursos públicos e estáticos como CSS, JavaScript e arquivos de imagem. Por padrão, *wwwroot* é o webroot.

Para arquivos (*.cshtml*) do Razor, o til-barra `~/` aponta para o webroot. Caminhos que começam com `~/` são denominados caminhos virtuais.

## <a name="dependency-injection-services"></a>Injeção de dependência (serviços)

Um *serviço* é um componente que é destinado ao consumo comum em um aplicativo. Os serviços são disponibilizados por meio de DI ([injeção de dependência](xref:fundamentals/dependency-injection)). O ASP.NET Core inclui um contêiner nativo de IoC (Inversão de Controle) que dá suporte à [injeção de construtor](xref:mvc/controllers/dependency-injection#constructor-injection) por padrão. É possível substituir o contêiner padrão, se desejado. Além do [benefício de seu acoplamento flexível](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), a DI disponibiliza serviços, tais como [registro em log](xref:fundamentals/logging/index), por todo o seu aplicativo.

Para obter mais informações, consulte <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Middleware

No ASP.NET Core, você compõe o pipeline de solicitação usando o [middleware](xref:fundamentals/middleware/index). O middleware do ASP.NET Core executa operações assíncronas em um `HttpContext` e então invoca o próximo middleware no pipeline ou encerra a solicitação.

Por convenção, um componente de middleware chamado "XYZ" é adicionado ao pipeline invocando-se um método de extensão `UseXYZ` no método `Configure`.

O ASP.NET Core inclui um conjunto de middleware interno, e você pode escrever seu próprio middleware personalizado. A [OWIN (Open Web Interface para .NET)](xref:fundamentals/owin), que permite que aplicativos Web sejam desacoplados de servidores Web, é compatível com aplicativos ASP.NET Core.

Para obter mais informações, consulte <xref:fundamentals/middleware/index> e <xref:fundamentals/owin>.

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Iniciar solicitações HTTP

O <xref:System.Net.Http.IHttpClientFactory> está disponível para acessar instâncias de <xref:System.Net.Http.HttpClient> para fazer solicitações HTTP.

Para obter mais informações, consulte <xref:fundamentals/http-requests>.

::: moniker-end

## <a name="environments"></a>Ambientes

Os ambientes, como *Desenvolvimento* e *Produção*, são uma noção de primeira classe no ASP.NET Core e podem ser definidos usando uma variável de ambiente, um arquivo de configurações e um argumento de linha de comando.

Para obter mais informações, consulte <xref:fundamentals/environments>.

## <a name="hosting"></a>Hospedagem

Os aplicativos ASP.NET Core configuram e iniciam um *host*, que é responsável pelo gerenciamento de inicialização e de tempo de vida do aplicativo.

Para obter mais informações, consulte <xref:fundamentals/host/index>.

## <a name="servers"></a>Servidores

O modelo de hospedagem do ASP.NET Core não escuta diretamente as solicitações. O modelo de host se baseia em uma implementação do servidor HTTP para encaminhar a solicitação ao aplicativo. A solicitação encaminhada é empacotada como um conjunto de objetos de recurso que podem ser acessados por meio de interfaces. O ASP.NET Core inclui um servidor Web gerenciado e de plataforma cruzada chamado [Kestrel](xref:fundamentals/servers/kestrel). O Kestrel normalmente é executado por trás de um servidor Web de produção, assim como [IIS](https://www.iis.net/) ou [Nginx](http://nginx.org) em uma configuração de proxy reverso. O Kestrel também pode ser executado como um servidor de borda voltado para o público exposto diretamente à Internet no ASP.NET Core 2.0 ou posterior.

Para obter mais informações, consulte <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Configuração

O ASP.NET Core usa um modelo de configuração com base nos pares de nome-valor. O modelo de configuração não baseado em <xref:System.Configuration> ou em *web.config*. A configuração obtém as definições de um conjunto ordenado de provedores de configuração. Os provedores internos de configuração dão suporte a uma variedade de formatos de arquivo (XML, JSON, INI), variáveis de ambiente e argumentos de linha de comando. Você também pode escrever seus próprios provedores de configuração personalizados.

Para obter mais informações, consulte <xref:fundamentals/configuration/index>.

## <a name="logging"></a>Registrando em log

O ASP.NET Core dá suporte a uma API de registro em log que funciona com uma variedade de provedores de logs. Os provedores internos dão suporte ao envio de logs para um ou mais destinos. As estruturas de registro em log de terceiros podem ser usadas.

Para obter mais informações, consulte <xref:fundamentals/logging/index>.

## <a name="error-handling"></a>Tratamento de erros

O ASP.NET Core tem cenários internos para tratamento de erros em aplicativos, incluindo uma página de exceção de desenvolvedor, páginas de erro personalizadas, páginas de código de status estático e tratamento de exceções de inicialização.

Para obter mais informações, consulte <xref:fundamentals/error-handling>.

## <a name="routing"></a>Roteamento

O ASP.NET Core oferece cenários para roteamento de solicitações de aplicativo para manipuladores de rotas.

Para obter mais informações, consulte <xref:fundamentals/routing>.

## <a name="file-providers"></a>Provedores de arquivo

O ASP.NET Core resume o acesso de sistema de arquivos por meio do uso de Provedores de Arquivo, que oferece uma interface comum para trabalhar com arquivos entre plataformas.

Para obter mais informações, consulte <xref:fundamentals/file-providers>.

## <a name="static-files"></a>Arquivos estáticos

O middleware de arquivos estáticos fornece arquivos estáticos, tais como arquivos HTML, CSS, de imagem e JavaScript.

Para obter mais informações, consulte <xref:fundamentals/static-files>.

## <a name="session-and-app-state"></a>Estado de sessão e de aplicativo

O ASP.NET Core oferece várias abordagens para preservar o estado de sessão e de aplicativo enquanto um usuário procura um aplicativo Web.

Para obter mais informações, consulte <xref:fundamentals/app-state>.

## <a name="globalization-and-localization"></a>Globalização e localização

Criar um site multilíngue com o ASP.NET Core permite que seu site alcance um público maior. O ASP.NET Core fornece serviços e middleware para localização de conteúdo em diferentes idiomas e culturas.

Para obter mais informações, consulte <xref:fundamentals/localization>.

## <a name="request-features"></a>Recursos de solicitação

Os detalhes de implementação de servidor Web relacionados a solicitações HTTP e respostas são definidos em interfaces. Essas interfaces são usadas pelas implementações de servidor e pelo middleware para criar e modificar o pipeline de hospedagem do aplicativo.

Para obter mais informações, consulte <xref:fundamentals/request-features>.

## <a name="background-tasks"></a>Tarefas em segundo plano

As tarefas em segundo plano são implementadas como *serviços hospedados*. Um serviço hospedado é uma classe com lógica de tarefa em segundo plano que implementa a interface <xref:Microsoft.Extensions.Hosting.IHostedService>.

Para obter mais informações, consulte <xref:fundamentals/host/hosted-services>.

## <a name="access-httpcontext"></a>Acessar o HttpContext

`HttpContext` está disponível automaticamente durante o processamento de solicitações com Razor Pages e com o MVC. Em circunstâncias em que `HttpContext` não está prontamente disponível, você pode acessar o `HttpContext` por meio da interface <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> e sua implementação padrão, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.

Para obter mais informações, consulte <xref:fundamentals/httpcontext>.

## <a name="websockets"></a>WebSockets

O [WebSocket](https://wikipedia.org/wiki/WebSocket) é um protocolo que permite canais de comunicação persistentes bidirecionais em conexões TCP. Ele é usado em aplicativos como chat, cotações de ações, jogos e em qualquer lugar que desejar funcionalidade em tempo real em um aplicativo Web. O ASP.NET Core dá suporte a cenários de soquete da Web.

Para obter mais informações, consulte <xref:fundamentals/websockets>.

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a>Metapacote Microsoft.AspNetCore.App

O metapacote [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) simplifica o gerenciamento de pacotes.

Para obter mais informações, consulte <xref:fundamentals/metapackage-app>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a>Metapacote Microsoft.AspNetCore.All

O [Metapacote do Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core inclui:

* Todos os pacotes com suporte da equipe do ASP.NET Core.
* Todos os pacotes compatíveis com o Entity Framework Core.
* Dependências internas e de terceiros usadas por ASP.NET Core e pelo Entity Framework Core.

Para obter mais informações, consulte <xref:fundamentals/metapackage>.

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a>Tempo de execução do .NET Core vs do .NET Framework

Um aplicativo do ASP.NET Core pode atingir o tempo de execução do .NET Core ou do .NET Framework.

Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Escolher entre o ASP.NET Core e o ASP.NET

Para obter mais informações sobre como escolher entre ASP.NET Core e ASP.NET, consulte <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.
