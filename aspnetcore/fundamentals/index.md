---
title: Conceitos básicos do ASP.NET Core
author: rick-anderson
description: Descubra os conceitos fundamentais para a criação de aplicativos do ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 68760f179c4d6e806510b727e2284f8c2c4a4ff6
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41748571"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="d2852-103">Conceitos básicos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d2852-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="d2852-104">Um aplicativo ASP.NET Core é um aplicativo de console que cria um servidor Web em seu método `Main`:</span><span class="sxs-lookup"><span data-stu-id="d2852-104">An ASP.NET Core app is a console app that creates a web server in its `Main` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="d2852-105">O método `Main` invoca [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), que segue o [padrão de construtor](https://wikipedia.org/wiki/Builder_pattern) para criar um host da Web.</span><span class="sxs-lookup"><span data-stu-id="d2852-105">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="d2852-106">O construtor tem métodos que definem o servidor Web (por exemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) e a classe de inicialização (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="d2852-106">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="d2852-107">No exemplo anterior, o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é alocado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="d2852-107">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="d2852-108">O host Web do ASP.NET Core tenta executar no IIS, se disponível.</span><span class="sxs-lookup"><span data-stu-id="d2852-108">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="d2852-109">Outros servidores Web como [HTTP.sys](xref:fundamentals/servers/httpsys) podem ser usados ao chamar o método de extensão apropriado.</span><span class="sxs-lookup"><span data-stu-id="d2852-109">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="d2852-110">`UseStartup` é explicado em mais detalhes na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="d2852-110">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="d2852-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, o tipo de retorno da invocação de `WebHost.CreateDefaultBuilder`, fornece muitos métodos opcionais.</span><span class="sxs-lookup"><span data-stu-id="d2852-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="d2852-112">Alguns desses métodos incluem `UseHttpSys` para hospedar o aplicativo em HTTP.sys e <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar o diretório de conteúdo raiz.</span><span class="sxs-lookup"><span data-stu-id="d2852-112">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="d2852-113">Os métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> e <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> compilam o objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda o aplicativo e começa a escutar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2852-113">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="d2852-114">O método `Main` usa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, que segue o [padrão de construtor](https://wikipedia.org/wiki/Builder_pattern) para criar um host de aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="d2852-114">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="d2852-115">O construtor tem métodos que definem o servidor Web (por exemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) e a classe de inicialização (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="d2852-115">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="d2852-116">No exemplo anterior, o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é usado.</span><span class="sxs-lookup"><span data-stu-id="d2852-116">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="d2852-117">Outros servidores Web como [WebListener](xref:fundamentals/servers/weblistener) podem ser usados ao chamar o método de extensão apropriado.</span><span class="sxs-lookup"><span data-stu-id="d2852-117">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="d2852-118">`UseStartup` é explicado em mais detalhes na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="d2852-118">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="d2852-119">O `WebHostBuilder` fornece muitos métodos opcionais, incluindo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>, para hospedagem no IIS e no IIS Express, e <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>, para especificar o diretório do conteúdo raiz.</span><span class="sxs-lookup"><span data-stu-id="d2852-119">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="d2852-120">Os métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> e <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> compilam o objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda o aplicativo e começa a escutar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2852-120">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="d2852-121">Inicialização</span><span class="sxs-lookup"><span data-stu-id="d2852-121">Startup</span></span>

<span data-ttu-id="d2852-122">O método `UseStartup` em `WebHostBuilder` especifica a classe `Startup` para seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="d2852-122">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="d2852-123">É na classe `Startup` que você define o pipeline de tratamento de solicitação e é nela que todos os serviços que o aplicativo precisa estão configurados.</span><span class="sxs-lookup"><span data-stu-id="d2852-123">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="d2852-124">A classe `Startup` deve ser pública e conter os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="d2852-124">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="d2852-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> define os [Serviços](#dependency-injection-services) usados pelo seu aplicativo (por exemplo, ASP.NET Core MVC, Entity Framework Core, Identity etc.).</span><span class="sxs-lookup"><span data-stu-id="d2852-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="d2852-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> define o [middleware](xref:fundamentals/middleware/index) chamado no pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="d2852-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="d2852-127">Para obter mais informações, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d2852-127">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="d2852-128">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="d2852-128">Content root</span></span>

<span data-ttu-id="d2852-129">A raiz do conteúdo é o caminho base para qualquer conteúdo usado pelo aplicativo, tal como [Razor Pages](xref:razor-pages/index), exibições do MVC e ativos estáticos.</span><span class="sxs-lookup"><span data-stu-id="d2852-129">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="d2852-130">Por padrão, a raiz do conteúdo é a mesma localização que o caminho base do aplicativo para o executável que hospeda o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d2852-130">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="d2852-131">Raiz da Web</span><span class="sxs-lookup"><span data-stu-id="d2852-131">Web root</span></span>

<span data-ttu-id="d2852-132">A raiz Web de um aplicativo é o diretório do projeto que contém recursos públicos e estáticos como CSS, JavaScript e arquivos de imagem.</span><span class="sxs-lookup"><span data-stu-id="d2852-132">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="d2852-133">Injeção de dependência (serviços)</span><span class="sxs-lookup"><span data-stu-id="d2852-133">Dependency injection (services)</span></span>

<span data-ttu-id="d2852-134">Um *serviço* é um componente que é destinado ao consumo comum em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d2852-134">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="d2852-135">Os serviços são disponibilizados por meio de DI ([injeção de dependência](xref:fundamentals/dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="d2852-135">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d2852-136">O ASP.NET Core inclui um contêiner nativo de IoC (Inversão de Controle) que dá suporte à [injeção de construtor](xref:mvc/controllers/dependency-injection#constructor-injection) por padrão.</span><span class="sxs-lookup"><span data-stu-id="d2852-136">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="d2852-137">É possível substituir o contêiner padrão, se desejado.</span><span class="sxs-lookup"><span data-stu-id="d2852-137">You can replace the default container if you wish.</span></span> <span data-ttu-id="d2852-138">Além do [benefício de seu acoplamento flexível](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), a DI disponibiliza serviços, tais como [registro em log](xref:fundamentals/logging/index), por todo o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d2852-138">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="d2852-139">Para obter mais informações, consulte <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="d2852-139">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="d2852-140">Middleware</span><span class="sxs-lookup"><span data-stu-id="d2852-140">Middleware</span></span>

<span data-ttu-id="d2852-141">No ASP.NET Core, você compõe o pipeline de solicitação usando o [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="d2852-141">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="d2852-142">O middleware do ASP.NET Core executa operações assíncronas em um `HttpContext` e então invoca o próximo middleware no pipeline ou encerra a solicitação.</span><span class="sxs-lookup"><span data-stu-id="d2852-142">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="d2852-143">Por convenção, um componente de middleware chamado "XYZ" é adicionado ao pipeline invocando-se um método de extensão `UseXYZ` no método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d2852-143">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="d2852-144">O ASP.NET Core inclui um conjunto de middleware interno, e você pode escrever seu próprio middleware personalizado.</span><span class="sxs-lookup"><span data-stu-id="d2852-144">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="d2852-145">A [OWIN (Open Web Interface para .NET)](xref:fundamentals/owin), que permite que aplicativos Web sejam desacoplados de servidores Web, é compatível com aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2852-145">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="d2852-146">Para obter mais informações, consulte <xref:fundamentals/middleware/index> e <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="d2852-146">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="d2852-147">Iniciar solicitações HTTP</span><span class="sxs-lookup"><span data-stu-id="d2852-147">Initiate HTTP requests</span></span>

<span data-ttu-id="d2852-148">O <xref:System.Net.Http.IHttpClientFactory> está disponível para acessar instâncias de <xref:System.Net.Http.HttpClient> para fazer solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="d2852-148"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="d2852-149">Para obter mais informações, consulte <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="d2852-149">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="d2852-150">Ambientes</span><span class="sxs-lookup"><span data-stu-id="d2852-150">Environments</span></span>

<span data-ttu-id="d2852-151">Os ambientes, como *Desenvolvimento* e *Produção*, são uma noção de primeira classe no ASP.NET Core e podem ser definidos usando uma variável de ambiente, um arquivo de configurações e um argumento de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d2852-151">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="d2852-152">Para obter mais informações, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d2852-152">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="d2852-153">Hospedagem</span><span class="sxs-lookup"><span data-stu-id="d2852-153">Hosting</span></span>

<span data-ttu-id="d2852-154">Os aplicativos ASP.NET Core configuram e iniciam um *host*, que é responsável pelo gerenciamento de inicialização e de tempo de vida do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d2852-154">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="d2852-155">Para obter mais informações, consulte <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="d2852-155">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="d2852-156">Servidores</span><span class="sxs-lookup"><span data-stu-id="d2852-156">Servers</span></span>

<span data-ttu-id="d2852-157">O modelo de hospedagem do ASP.NET Core não escuta diretamente as solicitações.</span><span class="sxs-lookup"><span data-stu-id="d2852-157">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="d2852-158">O modelo de host se baseia em uma implementação do servidor HTTP para encaminhar a solicitação ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d2852-158">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="d2852-159">A solicitação encaminhada é empacotada como um conjunto de objetos de recurso que podem ser acessados por meio de interfaces.</span><span class="sxs-lookup"><span data-stu-id="d2852-159">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="d2852-160">O ASP.NET Core inclui um servidor Web gerenciado e de plataforma cruzada chamado [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d2852-160">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="d2852-161">O Kestrel normalmente é executado por trás de um servidor Web de produção, assim como [IIS](https://www.iis.net/) ou [Nginx](http://nginx.org) em uma configuração de proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="d2852-161">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="d2852-162">O Kestrel também pode ser executado como um servidor de borda exposto diretamente à Internet no ASP.NET Core 2.0 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="d2852-162">Kestrel can also be run as an edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="d2852-163">Para obter mais informações, consulte <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="d2852-163">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="d2852-164">Configuração</span><span class="sxs-lookup"><span data-stu-id="d2852-164">Configuration</span></span>

<span data-ttu-id="d2852-165">O ASP.NET Core usa um modelo de configuração com base nos pares de nome-valor.</span><span class="sxs-lookup"><span data-stu-id="d2852-165">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="d2852-166">O modelo de configuração não baseado em <xref:System.Configuration> ou em *web.config*. A configuração obtém as definições de um conjunto ordenado de provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="d2852-166">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="d2852-167">Os provedores internos de configuração dão suporte a uma variedade de formatos de arquivo (XML, JSON, INI), variáveis de ambiente e argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d2852-167">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="d2852-168">Você também pode escrever seus próprios provedores de configuração personalizados.</span><span class="sxs-lookup"><span data-stu-id="d2852-168">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="d2852-169">Para obter mais informações, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d2852-169">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="d2852-170">Registrando em log</span><span class="sxs-lookup"><span data-stu-id="d2852-170">Logging</span></span>

<span data-ttu-id="d2852-171">O ASP.NET Core dá suporte a uma API de registro em log que funciona com uma variedade de provedores de logs.</span><span class="sxs-lookup"><span data-stu-id="d2852-171">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="d2852-172">Os provedores internos dão suporte ao envio de logs para um ou mais destinos.</span><span class="sxs-lookup"><span data-stu-id="d2852-172">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="d2852-173">As estruturas de registro em log de terceiros podem ser usadas.</span><span class="sxs-lookup"><span data-stu-id="d2852-173">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="d2852-174">Para obter mais informações, consulte <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="d2852-174">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="d2852-175">Tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="d2852-175">Error handling</span></span>

<span data-ttu-id="d2852-176">O ASP.NET Core tem cenários internos para tratamento de erros em aplicativos, incluindo uma página de exceção de desenvolvedor, páginas de erro personalizadas, páginas de código de status estático e tratamento de exceções de inicialização.</span><span class="sxs-lookup"><span data-stu-id="d2852-176">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="d2852-177">Para obter mais informações, consulte <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="d2852-177">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="d2852-178">Roteamento</span><span class="sxs-lookup"><span data-stu-id="d2852-178">Routing</span></span>

<span data-ttu-id="d2852-179">O ASP.NET Core oferece cenários para roteamento de solicitações de aplicativo para manipuladores de rotas.</span><span class="sxs-lookup"><span data-stu-id="d2852-179">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="d2852-180">Para obter mais informações, consulte <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="d2852-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="file-providers"></a><span data-ttu-id="d2852-181">Provedores de arquivo</span><span class="sxs-lookup"><span data-stu-id="d2852-181">File Providers</span></span>

<span data-ttu-id="d2852-182">O ASP.NET Core resume o acesso de sistema de arquivos por meio do uso de Provedores de Arquivo, que oferece uma interface comum para trabalhar com arquivos entre plataformas.</span><span class="sxs-lookup"><span data-stu-id="d2852-182">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="d2852-183">Para obter mais informações, consulte <xref:fundamentals/file-providers>.</span><span class="sxs-lookup"><span data-stu-id="d2852-183">For more information, see <xref:fundamentals/file-providers>.</span></span>

## <a name="static-files"></a><span data-ttu-id="d2852-184">Arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="d2852-184">Static files</span></span>

<span data-ttu-id="d2852-185">O middleware de arquivos estáticos fornece arquivos estáticos, tais como arquivos HTML, CSS, de imagem e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d2852-185">Static Files Middleware serves static files, such as HTML, CSS, image, and JavaScript files.</span></span>

<span data-ttu-id="d2852-186">Para obter mais informações, consulte <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="d2852-186">For more information, see <xref:fundamentals/static-files>.</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="d2852-187">Estado de sessão e de aplicativo</span><span class="sxs-lookup"><span data-stu-id="d2852-187">Session and app state</span></span>

<span data-ttu-id="d2852-188">O ASP.NET Core oferece várias abordagens para preservar o estado de sessão e de aplicativo enquanto um usuário procura um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="d2852-188">ASP.NET Core offers several approaches to preserve session and app state while a user browses a web app.</span></span>

<span data-ttu-id="d2852-189">Para obter mais informações, consulte <xref:fundamentals/app-state>.</span><span class="sxs-lookup"><span data-stu-id="d2852-189">For more information, see <xref:fundamentals/app-state>.</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="d2852-190">Globalização e localização</span><span class="sxs-lookup"><span data-stu-id="d2852-190">Globalization and localization</span></span>

<span data-ttu-id="d2852-191">Criar um site multilíngue com o ASP.NET Core permite que seu site alcance um público maior.</span><span class="sxs-lookup"><span data-stu-id="d2852-191">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="d2852-192">O ASP.NET Core fornece serviços e middleware para localização de conteúdo em diferentes idiomas e culturas.</span><span class="sxs-lookup"><span data-stu-id="d2852-192">ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>

<span data-ttu-id="d2852-193">Para obter mais informações, consulte <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="d2852-193">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="request-features"></a><span data-ttu-id="d2852-194">Recursos de solicitação</span><span class="sxs-lookup"><span data-stu-id="d2852-194">Request features</span></span>

<span data-ttu-id="d2852-195">Os detalhes de implementação de servidor Web relacionados a solicitações HTTP e respostas são definidos em interfaces.</span><span class="sxs-lookup"><span data-stu-id="d2852-195">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="d2852-196">Essas interfaces são usadas pelas implementações de servidor e pelo middleware para criar e modificar o pipeline de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d2852-196">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="d2852-197">Para obter mais informações, consulte <xref:fundamentals/request-features>.</span><span class="sxs-lookup"><span data-stu-id="d2852-197">For more information, see <xref:fundamentals/request-features>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="d2852-198">Tarefas em segundo plano</span><span class="sxs-lookup"><span data-stu-id="d2852-198">Background tasks</span></span>

<span data-ttu-id="d2852-199">As tarefas em segundo plano são implementadas como *serviços hospedados*.</span><span class="sxs-lookup"><span data-stu-id="d2852-199">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="d2852-200">Um serviço hospedado é uma classe com lógica de tarefa em segundo plano que implementa a interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="d2852-200">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="d2852-201">Para obter mais informações, consulte <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="d2852-201">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="d2852-202">Acessar o HttpContext</span><span class="sxs-lookup"><span data-stu-id="d2852-202">Access HttpContext</span></span>

<span data-ttu-id="d2852-203">`HttpContext` está disponível automaticamente durante o processamento de solicitações com Razor Pages e com o MVC.</span><span class="sxs-lookup"><span data-stu-id="d2852-203">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="d2852-204">Em circunstâncias em que `HttpContext` não está prontamente disponível, você pode acessar o `HttpContext` por meio da interface <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> e sua implementação padrão, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="d2852-204">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="d2852-205">Para obter mais informações, consulte <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="d2852-205">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="websockets"></a><span data-ttu-id="d2852-206">WebSockets</span><span class="sxs-lookup"><span data-stu-id="d2852-206">WebSockets</span></span>

<span data-ttu-id="d2852-207">O [WebSocket](https://wikipedia.org/wiki/WebSocket) é um protocolo que permite canais de comunicação persistentes bidirecionais em conexões TCP.</span><span class="sxs-lookup"><span data-stu-id="d2852-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="d2852-208">Ele é usado em aplicativos como chat, cotações de ações, jogos e em qualquer lugar que desejar funcionalidade em tempo real em um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="d2852-208">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="d2852-209">O ASP.NET Core dá suporte a cenários de soquete da Web.</span><span class="sxs-lookup"><span data-stu-id="d2852-209">ASP.NET Core supports web socket scenarios.</span></span>

<span data-ttu-id="d2852-210">Para obter mais informações, consulte <xref:fundamentals/websockets>.</span><span class="sxs-lookup"><span data-stu-id="d2852-210">For more information, see <xref:fundamentals/websockets>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="d2852-211">Metapacote Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="d2852-211">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="d2852-212">O metapacote [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) simplifica o gerenciamento de pacotes.</span><span class="sxs-lookup"><span data-stu-id="d2852-212">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span>

<span data-ttu-id="d2852-213">Para obter mais informações, consulte <xref:fundamentals/metapackage-app>.</span><span class="sxs-lookup"><span data-stu-id="d2852-213">For more information, see <xref:fundamentals/metapackage-app>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="d2852-214">Metapacote Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="d2852-214">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="d2852-215">O [Metapacote do Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core inclui:</span><span class="sxs-lookup"><span data-stu-id="d2852-215">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="d2852-216">Todos os pacotes com suporte da equipe do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2852-216">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="d2852-217">Todos os pacotes compatíveis com o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d2852-217">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="d2852-218">Dependências internas e de terceiros usadas por ASP.NET Core e pelo Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d2852-218">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="d2852-219">Para obter mais informações, consulte <xref:fundamentals/metapackage>.</span><span class="sxs-lookup"><span data-stu-id="d2852-219">For more information, see <xref:fundamentals/metapackage>.</span></span>

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="d2852-220">Tempo de execução do .NET Core vs do .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d2852-220">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="d2852-221">Um aplicativo do ASP.NET Core pode atingir o tempo de execução do .NET Core ou do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d2852-221">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="d2852-222">Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="d2852-222">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="d2852-223">Escolher entre o ASP.NET Core e o ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d2852-223">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="d2852-224">Para obter mais informações sobre como escolher entre ASP.NET Core e ASP.NET, consulte <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span><span class="sxs-lookup"><span data-stu-id="d2852-224">For more information on choosing between ASP.NET Core and ASP.NET, see <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span></span>
