---
title: "Conceitos básicos do ASP.NET Core"
author: rick-anderson
description: "Este artigo fornece uma visão geral dos conceitos fundamentais a serem compreendidos ao compilar aplicativos ASP.NET Core."
keywords: "ASP.NET Core, conceitos básicos, visão geral"
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 99fbe0e02be27a0fbbb7ff65bc15713aab58c003
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2017
---
# <a name="aspnet-core-fundamentals-overview"></a>Visão geral de conceitos básicos do ASP.NET Core

Um aplicativo ASP.NET Core é um aplicativo de console que cria um servidor Web em seu método `Main`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

O método `Main` invoca `WebHost.CreateDefaultBuilder`, que segue o padrão de construtor para criar um host de aplicativo Web. O construtor tem métodos que definem o servidor Web (por exemplo, `UseKestrel`) e a classe de inicialização (`UseStartup`). No exemplo anterior, um servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é alocado automaticamente. O host Web do ASP.NET Core tentará executar no IIS, se ele estiver disponível. Outros servidores Web como [HTTP.sys](xref:fundamentals/servers/httpsys) podem ser usados ao chamar o método de extensão apropriado. `UseStartup` é explicado em mais detalhes na próxima seção.

`IWebHostBuilder`, o tipo de retorno da invocação de `WebHost.CreateDefaultBuilder`, fornece muitos métodos opcionais. Alguns desses métodos incluem `UseHttpSys` para hospedar o aplicativo em HTTP.sys e `UseContentRoot` para especificar o diretório de conteúdo raiz. Os métodos `Build` e `Run` compilam o objeto `IWebHost` que hospedará o aplicativo e começam a escutar solicitações HTTP.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

O método `Main` usa `WebHostBuilder`, que segue o padrão de construtor para criar um host de aplicativo Web. O construtor tem métodos que definem o servidor Web (por exemplo, `UseKestrel`) e a classe de inicialização (`UseStartup`). No exemplo anterior, o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é usado. Outros servidores Web como [WebListener](xref:fundamentals/servers/weblistener) podem ser usados ao chamar o método de extensão apropriado. `UseStartup` é explicado em mais detalhes na próxima seção.

`WebHostBuilder` fornece muitos métodos opcionais, incluindo `UseIISIntegration`, para hospedagem no IIS e no IIS Express, e `UseContentRoot` para especificar o diretório do conteúdo raiz. Os métodos `Build` e `Run` compilam o objeto `IWebHost` que hospedará o aplicativo e começam a escutar solicitações HTTP.

---

## <a name="startup"></a>Inicialização

O método `UseStartup` em `WebHostBuilder` especifica a classe `Startup` para seu aplicativo:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

É na classe `Startup` que você define o pipeline de tratamento de solicitação e é nela que todos os serviços necessitados pelo aplicativo estão configurados. A classe `Startup` deve ser pública e conter os seguintes métodos:

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* `ConfigureServices` define os [serviços](#services) usados pelo seu aplicativo (como o ASP.NET Core MVC, Entity Framework Core, Identity, etc.).

* `Configure` define o [middleware](xref:fundamentals/middleware) no pipeline de solicitação.

Para obter mais informações, veja [Inicialização do aplicativo](xref:fundamentals/startup).

## <a name="services"></a>Serviços

Um serviço é um componente que é destinado ao consumo comum em um aplicativo. Os serviços são disponibilizados por meio de DI ([injeção de dependência](xref:fundamentals/dependency-injection)). O ASP.NET Core inclui um contêiner nativo de IoC (inversão de controle) que dá suporte a [injeção de construtor](xref:mvc/controllers/dependency-injection#constructor-injection) por padrão. O contêiner nativo pode ser substituído pelo contêiner de sua escolha. Além do benefício de seu acoplamento flexível, a DI disponibiliza serviços por todo o seu aplicativo. Por exemplo, o [registro em log](xref:fundamentals/logging) está disponível em todo o aplicativo.

Para obter mais informações, consulte [Injeção de dependência](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Middleware

No ASP.NET Core, você compõe o pipeline de solicitação usando [Middleware](xref:fundamentals/middleware). O middleware do ASP.NET Core executa lógica assíncrona em um `HttpContext` e então invoca o próximo middleware na sequência ou encerra a solicitação diretamente. Um componente de middleware chamado "XYZ" é adicionado invocando-se um método de extensão `UseXYZ` no método `Configure`.

O ASP.NET Core vem com um conjunto avançado de middleware interno:

* [Arquivos estáticos](xref:fundamentals/static-files)

* [Roteamento](xref:fundamentals/routing)

* [Autenticação](xref:security/authentication/index)

Você pode usar qualquer middleware com base em [OWIN](http://owin.org) com o ASP.NET Core e você pode escrever seu próprio middleware personalizado.

Para obter mais informações, consulte [Middleware](xref:fundamentals/middleware) e [OWIN (Interface da Web Aberta para .NET)](xref:fundamentals/owin).

## <a name="servers"></a>Servidores

O modelo de hospedagem do ASP.NET Core não escuta solicitações diretamente; em vez disso, ele se baseia em uma implementação do servidor HTTP para encaminhar a solicitação para o aplicativo. A solicitação encaminhada é empacotada como um conjunto de objetos de recurso que você pode acessar por meio de interfaces. O aplicativo compõe esse conjunto em um `HttpContext`. O ASP.NET Core inclui um servidor Web gerenciado e de plataforma cruzada chamado [Kestrel](xref:fundamentals/servers/kestrel). O Kestrel normalmente é executado atrás de um servidor Web de produção como [IIS](https://iis.net) ou [nginx](http://nginx.org).

Para obter mais informações, consulte [Servidores](xref:fundamentals/servers/index) e [Hospedagem](xref:fundamentals/hosting).

## <a name="content-root"></a>Raiz do conteúdo

A raiz do conteúdo é o caminho base para qualquer conteúdo usado pelo aplicativo, tal como exibições, [Páginas do Razor](xref:mvc/razor-pages/index) e ativos estáticos. Por padrão, a conteúdo raiz é o mesmo caminho base do aplicativo para o executável que hospeda o aplicativo. Um local alternativo para a raiz do conteúdo é especificado com `WebHostBuilder`.

## <a name="web-root"></a>Raiz da Web

A raiz de um aplicativo Web é o diretório do projeto que contém recursos públicos e estáticos como CSS, JavaScript e arquivos de imagem. Por padrão, o middleware de arquivos estáticos servirá apenas os arquivos do diretório raiz da Web e seus subdiretórios. Para obter mais informações, consulte [trabalhando com arquivos estáticos](xref:fundamentals/static-files). O caminho da raiz da Web assume o valor */wwwroot* por padrão, mas você pode especificar um local diferente usando o `WebHostBuilder`.

## <a name="configuration"></a>Configuração

O ASP.NET Core usa um novo modelo de configuração para lidar com pares de nome-valor simples. O novo modelo de configuração não se baseia em `System.Configuration` ou *web.config*; em vez disso, ele efetua pull de um conjunto ordenado de provedores de configuração. Os provedores internos de configuração dão suporte a uma variedade de formatos de arquivo (XML, JSON, INI) e variáveis de ambiente para habilitar a configuração do ambiente. Você também pode escrever seus próprios provedores de configuração personalizados.

Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration).

## <a name="environments"></a>Ambientes

Ambientes como "Desenvolvimento" e "Produção" são uma noção de primeira classe no ASP.NET Core e podem ser definidos usando variáveis de ambiente.

Para obter mais informações, consulte [Trabalhando com vários ambientes](xref:fundamentals/environments).

## <a name="net-core-vs-net-framework-runtime"></a>Tempo de execução do .NET Core vs do .NET Framework

Um aplicativo do ASP.NET Core pode atingir o tempo de execução do .NET Core ou do .NET Framework. Para obter mais informações, consulte [Escolhendo entre o .NET Core e .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="additional-information"></a>Informações adicionais

Consulte também os seguintes tópicos:

- [Tratamento de erro](xref:fundamentals/error-handling)
- [Provedores de arquivo](xref:fundamentals/file-providers)
- [Globalização e localização](xref:fundamentals/localization)
- [Registro em log](xref:fundamentals/logging)
- [Gerenciando o estado do aplicativo](xref:fundamentals/app-state)