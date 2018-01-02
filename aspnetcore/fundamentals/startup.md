---
title: "Inicialização do aplicativo no núcleo do ASP.NET"
author: ardalis
description: "Descobrir como a classe de inicialização no ASP.NET Core configura serviços e pipeline de solicitação do aplicativo."
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: dd2eb3d3996bc0bf277c8d5e772c8568ef9f147e
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/19/2017
---
# <a name="application-startup-in-aspnet-core"></a>Inicialização do aplicativo no núcleo do ASP.NET

Por [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), e [Luke Latham](https://github.com/guardrex)

O `Startup` classe configura os serviços e pipeline de solicitação do aplicativo.

## <a name="the-startup-class"></a>A classe de inicialização

Uso de aplicativos do ASP.NET Core um `Startup` classe, que é chamado `Startup` por convenção. O `Startup` classe:

* Opcionalmente, pode incluir um [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) método para configurar os serviços do aplicativo.
* Deve incluir um [configurar](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) método para criar o pipeline de processamento de solicitação do aplicativo.

`ConfigureServices`e `Configure` são chamados pelo tempo de execução quando o aplicativo for iniciado:

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

Especifique o `Startup` classe com o [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) método:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

O `Startup` construtor de classe aceita dependências definidas pelo host. Um uso comum de [injeção de dependência](xref:fundamentals/dependency-injection) para o `Startup` classe é injetar [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) para configurar serviços pelo ambiente:

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

Uma alternativa para injetar `IHostingStartup` é usar uma abordagem baseada em convenções. O aplicativo pode definir separada `Startup` classes para ambientes diferentes (por exemplo, `StartupDevelopment`), e a classe de inicialização adequado é selecionada em tempo de execução. A classe sufixo cujo nome corresponde ao ambiente atual é priorizada. Se o aplicativo é executado no ambiente de desenvolvimento e inclui tanto um `Startup` classe e um `StartupDevelopment` classe, o `StartupDevelopment` classe é usada. Para obter mais informações, consulte [trabalhando com vários ambientes](xref:fundamentals/environments#startup-conventions).

Para saber mais sobre `WebHostBuilder`, consulte o [hospedagem](xref:fundamentals/hosting) tópico. Para obter informações sobre como manipular erros durante a inicialização, consulte [tratamento de exceção de inicialização](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>O método ConfigureServices

O [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) método é:

* Opcional.
* Chamado pelo host da web antes do `Configure` método para configurar os serviços do aplicativo.
* Onde [opções de configuração](xref:fundamentals/configuration/index) são definidos por convenção.

Adicionar serviços ao contêiner de serviço torna-os disponíveis dentro do aplicativo e no `Configure` método. Os serviços são resolvidos por meio de [injeção de dependência](xref:fundamentals/dependency-injection) ou [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

O host da web pode configurar alguns serviços antes `Startup` métodos são chamados. Detalhes estão disponíveis no [hospedagem](xref:fundamentals/hosting) tópico. 

Para recursos que exigem configuração significativa, há `Add[Service]` métodos de extensão em [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Um aplicativo web típico registra os serviços para Entity Framework, identidade e MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>Serviços disponíveis na inicialização

O host da web fornece alguns serviços que estão disponíveis para o `Startup` construtor de classe. O aplicativo adiciona serviços adicionais por meio de `ConfigureServices`. O host e o aplicativo de serviços, em seguida, estão disponíveis em `Configure` e em todo o aplicativo.

## <a name="the-configure-method"></a>O método de configurar

O [configurar](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) método é usado para especificar como o aplicativo responde às solicitações HTTP. O pipeline de solicitação está configurado com a adição de [middleware](xref:fundamentals/middleware) componentes para uma [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instância. `IApplicationBuilder`está disponível para o `Configure` método, mas ele não está registrado no contêiner de serviço. Hospedagem cria uma `IApplicationBuilder` e passá-lo diretamente para `Configure` ([fonte de referência](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

O [modelos ASP.NET Core](/dotnet/core/tools/dotnet-new) configurar o pipeline com suporte para uma página de exceção de desenvolvedor, [BrowserLink](http://vswebessentials.com/features/browserlink), ASP.NET MVC, arquivos estáticos e páginas de erro:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Cada `Use` método de extensão adiciona um componente de middleware no pipeline de solicitação. Por exemplo, o `UseMvc` método de extensão adiciona o [roteamento middleware](xref:fundamentals/routing) para o pipeline de solicitação e configura [MVC](xref:mvc/overview) como o manipulador padrão.

Serviços adicionais, como `IHostingEnvironment` e `ILoggerFactory`, também pode ser especificado na assinatura do método. Quando especificado, os serviços adicionais podem ser adicionados se elas estão disponíveis.

Para obter mais informações sobre como usar `IApplicationBuilder`, consulte [Middleware](xref:fundamentals/middleware).

## <a name="convenience-methods"></a>Métodos de conveniência

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) e [configurar](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) métodos de conveniência podem ser usados em vez de especificar um `Startup` classe. Diversas chamadas para `ConfigureServices` acrescentar um ao outro. Diversas chamadas para `Configure` usar a última chamada de método.

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=16,20)]

## <a name="startup-filters"></a>Filtros de inicialização

Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) para configurar o middleware no início ou no final de um aplicativo [configurar](#the-configure-method) pipeline de middleware. `IStartupFilter`é útil para garantir que um middleware seja executado antes ou depois de adicionadas pelas bibliotecas no início ou no final do pipeline de processamento de solicitação do aplicativo de middleware.

`IStartupFilter`implementa um método único, [configurar](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), que recebe e retorna um `Action<IApplicationBuilder>`. Um [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) define uma classe para configurar o pipeline de solicitação do aplicativo. Para obter mais informações, consulte [criando um pipeline de middleware com IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).

Cada `IStartupFilter` implementa uma ou mais middlewares no pipeline de solicitação. Os filtros são invocados na ordem em que eles foram adicionados ao contêiner de serviço. Filtros podem adicionar middleware antes ou depois de passar o controle para o próximo filtro, assim eles acrescentam ao início ou no final do pipeline de aplicativo.

O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) demonstra como registrar um middleware com `IStartupFilter`. O aplicativo de exemplo inclui um middleware que define um valor de opção de um parâmetro de cadeia de caracteres de consulta:

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

O `RequestSetOptionsMiddleware` é configurado no `RequestSetOptionsStartupFilter` classe:

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

O `IStartupFilter` está registrado no contêiner de serviço no `ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Quando um parâmetro de cadeia de caracteres de consulta para `option` for fornecido, o middleware processa a atribuição de valor antes do middleware MVC processa a resposta:

![Janela do navegador mostrando a página renderizada. O valor de opção é renderizado como 'De Middleware' com base na solicitação de página com o parâmetro de cadeia de caracteres de consulta e o valor de opção definida como 'De Middleware'.](startup/_static/index.png)

Ordem de execução de middleware é definida pela ordem de `IStartupFilter` registros:

* Vários `IStartupFilter` implementações podem interagir com os mesmos objetos. Se a ordem é importante, ordene suas `IStartupFilter` registros para corresponder à ordem em que devem ser executados por seus middlewares de serviço.
* Bibliotecas podem adicionar middleware com um ou mais `IStartupFilter` implementações executam antes ou depois de outro middleware aplicativo registrado com `IStartupFilter`. Para invocar um `IStartupFilter` middleware antes de um middleware adicionado por uma biblioteca `IStartupFilter`, posicione o registro do serviço antes da biblioteca é adicionado ao contêiner de serviço. Para invocar posteriormente, posicione o registro do serviço depois que a biblioteca é adicionado.

## <a name="additional-resources"></a>Recursos adicionais

* [Hospedagem](xref:fundamentals/hosting)
* [Trabalhando com vários ambientes](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware)
* [Registro em log](xref:fundamentals/logging/index)
* [Configuração](xref:fundamentals/configuration/index)
* [Classe StartupLoader: método FindStartupType (fonte de referência)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
