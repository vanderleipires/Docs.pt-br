---
title: "Conceitos básicos do ASP.NET Core"
author: rick-anderson
description: "Descubra os conceitos fundamentais para a criação de aplicativos do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d977c13eb5f4cbe8bac261733bdc747e6c19b2a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-fundamentals"></a>Conceitos básicos do ASP.NET Core

Um aplicativo ASP.NET Core é um aplicativo de console que cria um servidor Web em seu método `Main`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

O método `Main` invoca `WebHost.CreateDefaultBuilder`, que segue o padrão de construtor para criar um host de aplicativo Web. O construtor tem métodos que definem o servidor Web (por exemplo, `UseKestrel`) e a classe de inicialização (`UseStartup`). No exemplo anterior, o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é alocado automaticamente. O host Web do ASP.NET Core tenta executar no IIS, se disponível. Outros servidores Web como [HTTP.sys](xref:fundamentals/servers/httpsys) podem ser usados ao chamar o método de extensão apropriado. `UseStartup` é explicado em mais detalhes na próxima seção.

`IWebHostBuilder`, o tipo de retorno da invocação de `WebHost.CreateDefaultBuilder`, fornece muitos métodos opcionais. Alguns desses métodos incluem `UseHttpSys` para hospedar o aplicativo em HTTP.sys e `UseContentRoot` para especificar o diretório de conteúdo raiz. Os métodos `Build` e `Run` compilam o objeto `IWebHost` que hospeda o aplicativo e começa a escutar solicitações HTTP.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

O método `Main` usa `WebHostBuilder`, que segue o padrão de construtor para criar um host de aplicativo Web. O construtor tem métodos que definem o servidor Web (por exemplo, `UseKestrel`) e a classe de inicialização (`UseStartup`). No exemplo anterior, o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é usado. Outros servidores Web como [WebListener](xref:fundamentals/servers/weblistener) podem ser usados ao chamar o método de extensão apropriado. `UseStartup` é explicado em mais detalhes na próxima seção.

O `WebHostBuilder` fornece muitos métodos opcionais, incluindo `UseIISIntegration`, para hospedagem no IIS e no IIS Express, e `UseContentRoot`, para especificar o diretório do conteúdo raiz. Os métodos `Build` e `Run` compilam o objeto `IWebHost` que hospeda o aplicativo e começa a escutar solicitações HTTP.

---

## <a name="startup"></a>Inicialização

O método `UseStartup` em `WebHostBuilder` especifica a classe `Startup` para seu aplicativo:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

É na classe `Startup` que você define o pipeline de tratamento de solicitação e é nela que todos os serviços que o aplicativo precisa estão configurados. A classe `Startup` deve ser pública e conter os seguintes métodos:

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

`ConfigureServices` define os [Serviços](#dependency-injection-services) usados pelo seu aplicativo (por exemplo, ASP.NET Core MVC, Entity Framework Core, Identity etc.). `Configure` define o [middleware](xref:fundamentals/middleware) para o pipeline de solicitação.

Para obter mais informações, veja [Inicialização do aplicativo](xref:fundamentals/startup).

## <a name="content-root"></a>Raiz do conteúdo

A raiz do conteúdo é o caminho base para qualquer conteúdo usado pelo aplicativo, tal como exibições, [Páginas do Razor](xref:mvc/razor-pages/index) e ativos estáticos. Por padrão, a raiz do conteúdo é o mesmo caminho base do aplicativo para o executável que hospeda o aplicativo.

## <a name="web-root"></a>Raiz da Web

A raiz Web de um aplicativo é o diretório do projeto que contém recursos públicos e estáticos como CSS, JavaScript e arquivos de imagem.

## <a name="dependency-injection-services"></a>Injeção de Dependência (Serviços)

Um serviço é um componente que é destinado ao consumo comum em um aplicativo. Os serviços são disponibilizados por meio de DI ([injeção de dependência](xref:fundamentals/dependency-injection)). O ASP.NET Core inclui um contêiner nativo de IoC (**I**nversão **d**e **C**ontrole) que dá suporte a injeção de [construtor](xref:mvc/controllers/dependency-injection#constructor-injection) por padrão. É possível substituir o contêiner nativo padrão se desejar. Além do benefício de seu acoplamento flexível, a DI disponibiliza serviços por todo o seu aplicativo (por exemplo, [registrar em log](xref:fundamentals/logging/index)).

Para obter mais informações, consulte [Injeção de dependência](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Middleware

No ASP.NET Core, você compõe o pipeline de solicitação usando o [middleware](xref:fundamentals/middleware). O middleware do ASP.NET Core executa lógica assíncrona em um `HttpContext` e então invoca o próximo middleware na sequência ou encerra a solicitação diretamente. Um componente de middleware chamado "XYZ" é adicionado ao invocar um `UseXYZ` método de extensão no método `Configure`.

O ASP.NET Core vem com um conjunto avançado de middleware interno:

* [Arquivos estáticos](xref:fundamentals/static-files)
* [Roteamento](xref:fundamentals/routing)
* [Autenticação](xref:security/authentication/index)
* [Middleware de compactação de resposta](xref:performance/response-compression)
* [Middleware de regravação de URL](xref:fundamentals/url-rewriting)

O middleware com base em [OWIN](http://owin.org) está disponível para aplicativos do ASP.NET Core e é possível escrever seu próprio middleware personalizado.

Para obter mais informações, consulte [Middleware](xref:fundamentals/middleware) e [OWIN (Interface da Web Aberta para .NET)](xref:fundamentals/owin).

## <a name="environments"></a>Ambientes

Os ambientes, como "Desenvolvimento" e "Produção", são uma noção de primeira classe no ASP.NET Core e podem ser definidos usando variáveis de ambiente.

Para obter mais informações, consulte [Trabalhando com vários ambientes](xref:fundamentals/environments).

## <a name="configuration"></a>Configuração

O ASP.NET Core usa um modelo de configuração com base nos pares de nome-valor. O modelo de configuração não baseado em `System.Configuration` ou em *web.config*. A configuração obtém as definições de um conjunto ordenado de provedores de configuração. Os provedores internos de configuração dão suporte a uma variedade de formatos de arquivo (XML, JSON, INI) e variáveis de ambiente para habilitar a configuração do ambiente. Você também pode escrever seus próprios provedores de configuração personalizados.

Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration/index).

## <a name="logging"></a>Registrando em log

O ASP.NET Core dá suporte a uma API de registro em log que funciona com uma variedade de provedores de logs. Os provedores internos dão suporte ao envio de logs para um ou mais destinos. As estruturas de registro em log de terceiros podem ser usadas.

[Registro em log](xref:fundamentals/logging/index)

## <a name="error-handling"></a>Tratamento de erros

O ASP.NET Core tem recursos internos para manipular erros em aplicativos, incluindo uma página de exceção de desenvolvedor, páginas de erro personalizadas, páginas de código de status estático e tratamento de exceções de inicialização.

Para obter mais informações, veja [Tratamento de erro](xref:fundamentals/error-handling).

## <a name="routing"></a>Roteamento

O ASP.NET Core oferece recursos para roteamento de solicitações de aplicativo para manipuladores de rotas.

Para obter mais informações, consulte [Roteamento](xref:fundamentals/routing).

## <a name="file-providers"></a>Provedores de arquivo

O ASP.NET Core resume o acesso de sistema de arquivos por meio do uso de Provedores de Arquivo, que oferece uma interface comum para trabalhar com arquivos entre plataformas.

Para obter mais informações, consulte [Provedores de Arquivos](xref:fundamentals/file-providers).

## <a name="static-files"></a>Arquivos estáticos

O middleware de arquivos estáticos fornece arquivos estáticos, como HTML, CSS, imagem e JavaScript.

Para obter mais informações, consulte [Trabalhando com arquivos estáticos](xref:fundamentals/static-files).

## <a name="hosting"></a>Hospedagem

Os aplicativos ASP.NET Core configuram e iniciam um *host*, que é responsável pelo gerenciamento de inicialização e de tempo de vida do aplicativo.

Para obter mais informações, consulte [Hospedagem](xref:fundamentals/hosting).

## <a name="session-and-application-state"></a>Estado de sessão e de aplicativo

O estado de sessão é um recurso do ASP.NET Core que você pode usar para salvar e armazenar dados de usuário enquanto o usuário navega seu aplicativo Web.

Para obter mais informações, consulte [Estado de sessão e aplicativo](xref:fundamentals/app-state).

## <a name="servers"></a>Servidores

O modelo de hospedagem do ASP.NET Core não escuta diretamente as solicitações. O modelo de host se baseia em uma implementação do servidor HTTP para encaminhar a solicitação ao aplicativo. A solicitação encaminhada é empacotada como um conjunto de objetos de recurso que podem ser acessados por meio de interfaces. O ASP.NET Core inclui um servidor Web gerenciado e de plataforma cruzada chamado [Kestrel](xref:fundamentals/servers/kestrel). O Kestrel normalmente é executado atrás de um servidor Web de produção, como [IIS](https://www.iis.net/) ou [Nginx](http://nginx.org). O Kestrel pode ser executado como um servidor de borda.

Para obter mais informações, consulte [Servidores](xref:fundamentals/servers/index) e os seguintes tópicos:

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente chamado [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>Globalização e localização

Criar um site multilíngue com o ASP.NET Core permite que seu site alcance um público maior. O ASP.NET Core fornece serviços e middleware para localização em diferentes idiomas e culturas.

Para obter mais informações, consulte [Globalização e localização](xref:fundamentals/localization).

## <a name="request-features"></a>Recursos de solicitação

Os detalhes de implementação de servidor Web relacionados a solicitações HTTP e respostas são definidos em interfaces. Essas interfaces são usadas pelas implementações de servidor e pelo middleware para criar e modificar o pipeline de hospedagem do aplicativo.

Para obter mais informações, consulte [Solicitar Recursos](xref:fundamentals/request-features).

## <a name="open-web-interface-for-net-owin"></a>OWIN (Open Web Interface para .NET)

O ASP.NET Core dá suporte para OWIN (Open Web Interface para .NET). O OWIN permite que os aplicativos Web sejam separados dos servidores Web.

Para obter mais informações, consulte [OWIN (Interface da Web Aberta para .NET)](xref:fundamentals/owin).

## <a name="websockets"></a>WebSockets

O [WebSocket](https://wikipedia.org/wiki/WebSocket) é um protocolo que permite canais de comunicação persistentes bidirecionais em conexões TCP. Ele é usado em aplicativos como chat, cotações de ações, jogos e em qualquer lugar que desejar funcionalidade em tempo real em um aplicativo Web. O ASP.NET Core dá suporte a recursos de soquete da Web.

Para obter mais informações, consulte [WebSockets](xref:fundamentals/websockets).

## <a name="microsoftaspnetcoreall-metapackage"></a>Metapacote Microsoft.AspNetCore.All

O [Metapacote do Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core inclui:

* Todos os pacotes com suporte da equipe do ASP.NET Core.
* Todos os pacotes com suporte pelo Entity Framework Core. 
* Dependências internas e de terceiros usadas por ASP.NET Core e pelo Entity Framework Core.

Para obter mais informações, consulte [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

## <a name="net-core-vs-net-framework-runtime"></a>Tempo de execução do .NET Core vs do .NET Framework

Um aplicativo do ASP.NET Core pode atingir o tempo de execução do .NET Core ou do .NET Framework.

Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Escolher entre o ASP.NET Core e o ASP.NET

Para obter mais informações sobre como escolher entre ASP.NET Core e ASP.NET, consulte [Escolha entre ASP.NET Core e ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).
