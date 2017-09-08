---
title: "Inicialização do aplicativo no núcleo do ASP.NET"
author: ardalis
description: "Explica a classe de inicialização no núcleo do ASP.NET."
keywords: "ASP.NET Core, inicialização, configurar método, o método ConfigureServices"
ms.author: tdykstra
manager: wpickett
ms.date: 02/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 16969386c55ae2fd2ab574c1799a765e74f59278
ms.sourcegitcommit: 4147d2d29ea50e7e9b87879c572ac2a9fb51798c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2017
---
# <a name="application-startup-in-aspnet-core"></a>Inicialização do aplicativo no núcleo do ASP.NET

Por [Steve Smith](http://ardalis.com) e [Tom Dykstra](https://github.com/tdykstra/)

O `Startup` classe configura os serviços e pipeline de solicitação do aplicativo. 

## <a name="the-startup-class"></a>A classe de inicialização

Aplicativos do ASP.NET Core exigem um `Startup` classe. Por convenção, o `Startup` classe é chamada de "Inicialização". Especifique o nome da classe de inicialização no `Main` do programa [WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [ `UseStartup<TStartup>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) método. Consulte [hospedagem](xref:fundamentals/hosting) para saber mais sobre `WebHostBuilder`, que é executado antes de `Startup`.

Você pode definir separada `Startup` classes para diferentes ambientes e apropriada um será selecionado em tempo de execução. Se você especificar `startupAssembly` no [WebHost configuração](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host) ou opções de hospedagem carregará esse assembly de inicialização e procure um `Startup` ou `Startup[Environment]` tipo. A classe sufixo cujo nome corresponde ao que o ambiente atual será ser priorizado, portanto, se o aplicativo é executado no *desenvolvimento* ambiente e inclui tanto um `Startup` e um `StartupDevelopment` classe, o `StartupDevelopment` classe será usado. Consulte [FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs) na `StartupLoader` e [trabalhando com vários ambientes](environments.md#startup-conventions).

Como alternativa, você pode definir um fixa `Startup` classe que será usado, independentemente do ambiente chamando `UseStartup<TStartup>`. Essa é a abordagem recomendada.

O `Startup` construtor da classe pode aceitar as dependências que são fornecidas por meio de [injeção de dependência](xref:fundamentals/dependency-injection). Uma abordagem comum é usar `IHostingEnvironment` configurar [configuração](xref:fundamentals/configuration) fontes.

O `Startup` classe deve incluir um `Configure` método e, opcionalmente, pode incluir um `ConfigureServices` método, que são chamados quando o aplicativo for iniciado. A classe também pode incluir [versões específicas do ambiente desses métodos](xref:fundamentals/environments#startup-conventions). `ConfigureServices`, se presente, é chamado antes de `Configure`.

Saiba mais sobre [tratamento de exceções durante a inicialização do aplicativo](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>O método ConfigureServices

O [ConfigureServices](https://docs.microsoft.com/en-us/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_) é opcional; mas se usado, ele é chamado antes do `Configure` método pelo host da web. O host da web pode configurar alguns serviços antes de ``Startup`` métodos são chamados (consulte [hospedagem](xref:fundamentals/hosting)). Por convenção, [opções de configuração](xref:fundamentals/configuration) são definidas nesse método.

Para recursos que exigem configuração significativa há `Add[Service]` métodos de extensão em [IServiceCollection](https://docs.microsoft.com/en-us/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection). Este exemplo do modelo de site da web padrão configura o aplicativo para usar os serviços de Entity Framework, identidade e MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

Adicionando serviços para o contêiner de serviços fiquem disponíveis dentro de seu aplicativo por meio de [injeção de dependência](xref:fundamentals/dependency-injection).

## <a name="services-available-in-startup"></a>Serviços disponíveis na inicialização

Injeção de dependência do ASP.NET Core fornece serviços durante a inicialização do aplicativo. Você pode solicitar esses serviços, incluindo a interface adequada como um parâmetro em sua `Startup` construtor da classe ou seus `Configure` método. O `ConfigureServices` método só aceita um `IServiceCollection` parâmetro (mas qualquer pode serviço registrado ser recuperado desta coleção, então parâmetros adicionais não são necessários).

Abaixo estão alguns dos serviços normalmente solicitados por `Startup` métodos:

* No construtor: `IHostingEnvironment`,`ILogger<Startup>`
* Em `ConfigureServices`:`IServiceCollection`
* Em `Configure`: `IApplicationBuilder`, `IHostingEnvironment`,`ILoggerFactory`

Todos os serviços adicionados pelo ``WebHostBuilder`` ``ConfigureServices`` método pode ser solicitado pelo ``Startup`` construtor de classe ou seus ``Configure`` método. Use `WebHostBuilder` fornecer qualquer um dos serviços necessários durante `Startup` métodos.

## <a name="the-configure-method"></a>O método de configurar

O `Configure` método é usado para especificar como o aplicativo ASP.NET responde a solicitações HTTP. O pipeline de solicitação está configurado com a adição de [middleware](middleware.md) componentes para uma `IApplicationBuilder` instância fornecida pelo injeção de dependência.

No exemplo a seguir do modelo de site da web padrão, vários métodos de extensão são usados para configurar o pipeline com suporte para [BrowserLink](http://vswebessentials.com/features/browserlink), páginas de erro, arquivos estáticos, ASP.NET MVC e identidade.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Cada `Use` método de extensão adiciona um [middleware](xref:fundamentals/middleware) componente ao pipeline de solicitação. Por exemplo, o `UseMvc` método de extensão adiciona o [roteamento](routing.md) middleware no pipeline de solicitação e configura [MVC](xref:mvc/overview) como o manipulador padrão.

Para obter mais informações sobre como usar `IApplicationBuilder`, consulte [Middleware](xref:fundamentals/middleware).

Serviços adicionais, como `IHostingEnvironment` e `ILoggerFactory` também pode ser especificado na assinatura do método, caso em que esses serviços estarão [injetado](dependency-injection.md) se elas estiverem disponíveis. 

## <a name="additional-resources"></a>Recursos adicionais

* [Trabalhando com vários ambientes](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware)
* [Registro em log](xref:fundamentals/logging)
* [Configuração](xref:fundamentals/configuration)
