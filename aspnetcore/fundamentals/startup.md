---
title: Inicialização do aplicativo no ASP.NET Core
author: ardalis
description: Saiba como a classe Startup no ASP.NET Core configura serviços e o pipeline de solicitação do aplicativo.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/startup
ms.openlocfilehash: 58ced0ae11f462bc309526b7db7bda7897c33009
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2018
ms.locfileid: "34233164"
---
# <a name="application-startup-in-aspnet-core"></a>Inicialização do aplicativo no ASP.NET Core

Por [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra) e [Luke Latham](https://github.com/guardrex)

A classe `Startup` configura serviços e o pipeline de solicitação do aplicativo.

## <a name="the-startup-class"></a>A classe Startup

Os aplicativos do ASP.NET Core usam uma classe `Startup`, que é chamada de `Startup` por convenção. A classe `Startup`:

* Também é possível incluir um método [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) para configurar os serviços do aplicativo.
* Deve incluir um método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) para criar o pipeline de processamento de solicitações do aplicativo.

`ConfigureServices` e `Configure` são chamados pelo tempo de execução quando o aplicativo é iniciado:

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

Especifique a classe `Startup` com o método [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_):

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

O construtor de classe `Startup` aceita dependências definidas pelo host. Um uso comum da [injeção de dependência](xref:fundamentals/dependency-injection) na classe `Startup` é injetar:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) para configurar serviços pelo ambiente.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para configurar o aplicativo durante a inicialização.

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

Uma alternativa a injetar `IHostingEnvironment` é usar uma abordagem baseada em convenções. O aplicativo pode definir classes `Startup` separadas para ambientes diferentes (por exemplo, `StartupDevelopment`) e a classe de inicialização adequada é selecionada em tempo de execução. A classe cujo sufixo do nome corresponde ao ambiente atual é priorizada. Se o aplicativo for executado no ambiente de desenvolvimento e incluir uma classe `Startup` e uma classe `StartupDevelopment`, a classe `StartupDevelopment` será usada. Para obter mais informações, veja [Usar vários ambientes](xref:fundamentals/environments#startup-conventions).

Para saber mais sobre `WebHostBuilder`, consulte tópico sobre [Hospedagem](xref:fundamentals/host/index). Para obter informações sobre como tratar erros durante a inicialização, consulte [Tratamento de exceção na inicialização](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>O método ConfigureServices

O método [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) é:

* Opcional
* Chamado pelo host da Web antes do `Configure` método para configurar os serviços do aplicativo.
* Quando as [opções de configuração](xref:fundamentals/configuration/index) são definidas por convenção.

Adicionar serviços ao contêiner de serviços os torna disponíveis dentro do aplicativo e no método `Configure`. Os serviços são resolvidos via [injeção de dependência](xref:fundamentals/dependency-injection) ou [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

O host da Web pode configurar alguns serviços antes que métodos `Startup` sejam chamados. Detalhes estão disponíveis no tópico [Host no ASP.NET Core](xref:fundamentals/host/index).

Para recursos que exigem uma configuração significativa, há métodos de extensão `Add[Service]` em [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Um aplicativo Web típico registra serviços para o Entity Framework, Identity e MVC:

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

::: moniker range=">= aspnetcore-2.1"

<a name="setcompatibilityversion"></a>

### <a name="setcompatibilityversion-for-aspnet-core-mvc"></a>SetCompatibilityVersion para ASP.NET Core MVC 

O método `SetCompatibilityVersion` permite que um aplicativo aceite ou recuse as possíveis alterações da falha de comportamento introduzidas no ASP.NET MVC Core 2.1+. Essas possíveis alterações da falha de comportamento geralmente afetam como o subsistema MVC se comporta e como **o código do usuário** é chamado pelo tempo de execução. Ao aceitar, você obtém o comportamento mais recente e o comportamento de longo prazo do ASP.NET Core.

O código a seguir define o modo de compatibilidade para o ASP.NET Core 2.1:

[!code-csharp[Main](startup/sampleCompatibility/Startup.cs?name=snippet1)]

É recomendável que você teste seu aplicativo usando a versão mais recente (`CompatibilityVersion.Version_2_1`). Estimamos que a maioria dos aplicativos não terão alterações da falha de comportamento usando a versão mais recente. 

Os aplicativos que chamam `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` são protegidos contra as possíveis alterações da falha de comportamento apresentadas no ASP.NET Core 2.1 MVC e nas versões 2.x posteriores. Essa proteção:

* Não se aplica a todas as alterações da 2.1 e posteriores, ela é direcionada às possíveis alterações da falha de comportamento do tempo de execução do ASP.NET Core no subsistema de MVC.
* Não se estende à próxima versão principal.

A compatibilidade de padrão para aplicativos ASP.NET Core 2.1 e 2.x posteriores que **não** é chamada é a `SetCompatibilityVersion` compatibilidade 2.0. Ou seja, não chamar `SetCompatibilityVersion` é o mesmo que chamar `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

O código a seguir define o modo de compatibilidade para o ASP.NET Core 2.1, exceto para os seguintes comportamentos:

* [AllowCombiningAuthorizeFilters](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [InputFormatterExceptionPolicy](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](startup/sampleCompatibility/Startup2.cs?name=snippet1)]

Para aplicativos que encontram alterações da falha de comportamento, o uso das opções de compatibilidade apropriadas:

* Permite que você use a versão mais recente e recuse alterações da falha de comportamento específicas.
* Tenha tempo para atualizar seu aplicativo para que ele funcione com as alterações mais recentes.

Os comentários do código-fonte da classe [MvcOptions](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) têm uma boa explicação sobre o que mudou e por que as alterações são uma melhoria para a maioria dos usuários.

Futuramente, haverá uma [versão 3.0 do ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap). Os comportamentos antigos compatíveis por meio de opções de compatibilidade serão removidos na versão 3.0. Consideramos que essas são alterações positivas que beneficiam quase todos os usuários. Com a introdução dessas alterações, a maioria dos aplicativos poderá se beneficiar agora e os demais terão tempo para serem atualizados.

::: moniker-end

## <a name="services-available-in-startup"></a>Não disponíveis na Inicialização

O host da Web fornece alguns serviços que estão disponíveis para o construtor de classe `Startup`. O aplicativo adiciona serviços adicionais por meio de `ConfigureServices`. Os Serviços de Aplicativos e o host ficam, então, disponíveis em `Configure` e em todo o aplicativo.

## <a name="the-configure-method"></a>O método Configure

O método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) é usado para especificar como o aplicativo responde as solicitações HTTP. O pipeline de solicitação é configurado adicionando componentes de [middleware](xref:fundamentals/middleware/index) a uma instância de [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder` está disponível para o método `Configure`, mas não é registrado no contêiner de serviço. A hospedagem cria um `IApplicationBuilder` e o passa diretamente para `Configure` ([fonte de referência](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

Os [modelos do ASP.NET Core](/dotnet/core/tools/dotnet-new) configuram o pipeline com suporte para uma página de exceção de desenvolvedor, [BrowserLink](http://vswebessentials.com/features/browserlink), páginas de erro, arquivos estáticos e o ASP.NET MVC:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Cada método de extensão `Use` adiciona um componente de middleware ao pipeline de solicitação. Por exemplo, o método de extensão `UseMvc` adiciona o [Middleware de roteamento](xref:fundamentals/routing) ao pipeline de solicitação e configura o [MVC](xref:mvc/overview) como o manipulador padrão.

Cada componente de middleware no pipeline de solicitação é responsável por invocar o próximo componente no pipeline ou causar um curto-circuito da cadeia, se apropriado. Se o curto-circuito não ocorrer ao longo da cadeia de middleware, cada middleware terá uma segunda chance de processar a solicitação antes que ela seja enviada ao cliente.

Serviços adicionais, como `IHostingEnvironment` e `ILoggerFactory`, também podem ser especificados na assinatura do método. Quando especificados, serviços adicionais são injetados quando estão disponíveis.

Para obter mais informações de como usar o `IApplicationBuilder` e a ordem de processamento de middleware, confira [Middleware](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Métodos de conveniência

Os métodos de conveniência [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) e [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) podem ser usados em vez de especificar uma classe `Startup`. Diversas chamadas para `ConfigureServices` são acrescentadas umas às outras. Diversas chamadas para `Configure` usam a última chamada de método.

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Filtros de inicialização

Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) para configurar o middleware no início ou no final do pipeline do middleware [Configure](#the-configure-method) de um aplicativo. `IStartupFilter` é útil para garantir que um middleware seja executado antes ou depois do middleware adicionado pelas bibliotecas no início ou no final do pipeline de processamento de solicitação do aplicativo.

`IStartupFilter` implementa um único método, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), que recebe e retorna um `Action<IApplicationBuilder>`. Um [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) define uma classe para configurar o pipeline de solicitação do aplicativo. Para obter mais informações, confira [Criar um pipeline de middleware com o IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder).

Cada `IStartupFilter` implementa uma ou mais middlewares no pipeline de solicitação. Os filtros são invocados na ordem em que foram adicionados ao contêiner de serviço. Filtros podem adicionar middleware antes ou depois de passar o controle para o filtro seguinte, de modo que eles são acrescentados ao início ou no final do pipeline de aplicativo.

O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) demonstra como registrar um middleware com `IStartupFilter`. O aplicativo de exemplo inclui um middleware que define um valor de opção de um parâmetro de cadeia de caracteres de consulta:

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

O `RequestSetOptionsMiddleware` é configurado na classe `RequestSetOptionsStartupFilter`:

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

O `IStartupFilter` é registrado no contêiner de serviço em `ConfigureServices`:

[!code-csharp[](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Quando um parâmetro de cadeia de caracteres de consulta para `option` é fornecido, o middleware processa a atribuição de valor antes que o middleware do MVC renderize a resposta:

![Janela do navegador mostrando a página de Índice renderizada. O valor de Option é renderizado como "From Middleware" com base na solicitação da página com o parâmetro da cadeia de caracteres de consulta e o valor da opção definido como "From Middleware".](startup/_static/index.png)

A ordem de execução do middleware é definida pela ordem dos registros `IStartupFilter`:

* Várias implementações de `IStartupFilter` podem interagir com os mesmos objetos. Se a ordem for importante, ordene seus registros de serviço `IStartupFilter` para corresponder à ordem em que os middlewares devem ser executados.
* Bibliotecas podem adicionar middleware com uma ou mais implementações de `IStartupFilter` que são executadas antes ou depois de outro middleware de aplicativo registrado com `IStartupFilter`. Para invocar um middleware `IStartupFilter` antes de um middleware adicionado pelo `IStartupFilter` de uma biblioteca, posicione o registro do serviço antes da biblioteca ser adicionada ao contêiner de serviço. Para invocá-lo posteriormente, posicione o registro do serviço após a biblioteca ser adicionada.

## <a name="adding-configuration-at-startup-from-an-external-assembly"></a>Adicionando a configuração na inicialização por meio de um assembly externo

Uma implementação [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) permite adicionar melhorias a um aplicativo durante a inicialização de um assembly externo fora da classe `Startup` do aplicativo. Para obter mais informações, veja [Aprimorar um aplicativo de um assembly externo](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="additional-resources"></a>Recursos adicionais

* [Hospedagem](xref:fundamentals/host/index)
* [Usar vários ambientes](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware/index)
* [Registro em log](xref:fundamentals/logging/index)
* [Configuração](xref:fundamentals/configuration/index)
* [Classe StartupLoader: método FindStartupType (fonte de referência)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
