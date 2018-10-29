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
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="5e369-103">Conceitos básicos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e369-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="5e369-104">Um aplicativo ASP.NET Core é um aplicativo de console que cria um servidor Web em seu método `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="5e369-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="5e369-105">O método `Main` é o *ponto de entrada gerenciado* do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="5e369-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="5e369-106">O host do .NET Core:</span><span class="sxs-lookup"><span data-stu-id="5e369-106">The .NET Core Host:</span></span>

* <span data-ttu-id="5e369-107">Carrega o [tempo de execução do .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="5e369-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="5e369-108">Usa o primeiro argumento de linha de comando como o caminho para o binário gerenciado que contém o ponto de entrada (`Main`) e inicia a execução do código.</span><span class="sxs-lookup"><span data-stu-id="5e369-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="5e369-109">O método `Main` invoca [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), que segue o [padrão de construtor](https://wikipedia.org/wiki/Builder_pattern) para criar um host da Web.</span><span class="sxs-lookup"><span data-stu-id="5e369-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="5e369-110">O construtor tem métodos que definem o servidor Web (por exemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) e a classe de inicialização (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="5e369-110">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="5e369-111">No exemplo anterior, o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é alocado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="5e369-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="5e369-112">O host Web do ASP.NET Core tenta executar no IIS, se disponível.</span><span class="sxs-lookup"><span data-stu-id="5e369-112">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="5e369-113">Outros servidores Web como [HTTP.sys](xref:fundamentals/servers/httpsys) podem ser usados ao chamar o método de extensão apropriado.</span><span class="sxs-lookup"><span data-stu-id="5e369-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="5e369-114">`UseStartup` é explicado em mais detalhes na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="5e369-114">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="5e369-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, o tipo de retorno da invocação de `WebHost.CreateDefaultBuilder`, fornece muitos métodos opcionais.</span><span class="sxs-lookup"><span data-stu-id="5e369-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="5e369-116">Alguns desses métodos incluem `UseHttpSys` para hospedar o aplicativo em HTTP.sys e <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar o diretório de conteúdo raiz.</span><span class="sxs-lookup"><span data-stu-id="5e369-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="5e369-117">Os métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> e <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> compilam o objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda o aplicativo e começa a escutar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e369-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="5e369-118">O host do .NET Core:</span><span class="sxs-lookup"><span data-stu-id="5e369-118">The .NET Core Host:</span></span>

* <span data-ttu-id="5e369-119">Carrega o [tempo de execução do .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="5e369-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="5e369-120">Usa o primeiro argumento de linha de comando como o caminho para o binário gerenciado que contém o ponto de entrada (`Main`) e inicia a execução do código.</span><span class="sxs-lookup"><span data-stu-id="5e369-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="5e369-121">O método `Main` usa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, que segue o [padrão de construtor](https://wikipedia.org/wiki/Builder_pattern) para criar um host de aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="5e369-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="5e369-122">O construtor tem métodos que definem o servidor Web (por exemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) e a classe de inicialização (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="5e369-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="5e369-123">No exemplo anterior, o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é usado.</span><span class="sxs-lookup"><span data-stu-id="5e369-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="5e369-124">Outros servidores Web como [WebListener](xref:fundamentals/servers/weblistener) podem ser usados ao chamar o método de extensão apropriado.</span><span class="sxs-lookup"><span data-stu-id="5e369-124">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="5e369-125">`UseStartup` é explicado em mais detalhes na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="5e369-125">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="5e369-126">O `WebHostBuilder` fornece muitos métodos opcionais, incluindo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>, para hospedagem no IIS e no IIS Express, e <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>, para especificar o diretório do conteúdo raiz.</span><span class="sxs-lookup"><span data-stu-id="5e369-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="5e369-127">Os métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> e <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> compilam o objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda o aplicativo e começa a escutar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e369-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="5e369-128">Inicialização</span><span class="sxs-lookup"><span data-stu-id="5e369-128">Startup</span></span>

<span data-ttu-id="5e369-129">O método `UseStartup` em `WebHostBuilder` especifica a classe `Startup` para seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="5e369-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="5e369-130">É na classe `Startup` que você define o pipeline de tratamento de solicitação e é nela que todos os serviços que o aplicativo precisa estão configurados.</span><span class="sxs-lookup"><span data-stu-id="5e369-130">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="5e369-131">A classe `Startup` deve ser pública e conter os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="5e369-131">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="5e369-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> define os [Serviços](#dependency-injection-services) usados pelo seu aplicativo (por exemplo, ASP.NET Core MVC, Entity Framework Core, Identity etc.).</span><span class="sxs-lookup"><span data-stu-id="5e369-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="5e369-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> define o [middleware](xref:fundamentals/middleware/index) chamado no pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="5e369-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="5e369-134">Para obter mais informações, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="5e369-134">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="5e369-135">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="5e369-135">Content root</span></span>

<span data-ttu-id="5e369-136">A raiz do conteúdo é o caminho base para qualquer conteúdo usado pelo aplicativo, tal como [Razor Pages](xref:razor-pages/index), exibições do MVC e ativos estáticos.</span><span class="sxs-lookup"><span data-stu-id="5e369-136">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="5e369-137">Por padrão, a raiz do conteúdo é a mesma localização que o caminho base do aplicativo para o executável que hospeda o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5e369-137">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="5e369-138">Diretório base (webroot)</span><span class="sxs-lookup"><span data-stu-id="5e369-138">Web root (webroot)</span></span>

<span data-ttu-id="5e369-139">O webroot de um aplicativo é o diretório do projeto que contém recursos públicos e estáticos como CSS, JavaScript e arquivos de imagem.</span><span class="sxs-lookup"><span data-stu-id="5e369-139">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="5e369-140">Por padrão, *wwwroot* é o webroot.</span><span class="sxs-lookup"><span data-stu-id="5e369-140">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="5e369-141">Para arquivos (*.cshtml*) do Razor, o til-barra `~/` aponta para o webroot.</span><span class="sxs-lookup"><span data-stu-id="5e369-141">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="5e369-142">Caminhos que começam com `~/` são denominados caminhos virtuais.</span><span class="sxs-lookup"><span data-stu-id="5e369-142">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="5e369-143">Injeção de dependência (serviços)</span><span class="sxs-lookup"><span data-stu-id="5e369-143">Dependency injection (services)</span></span>

<span data-ttu-id="5e369-144">Um *serviço* é um componente que é destinado ao consumo comum em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5e369-144">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="5e369-145">Os serviços são disponibilizados por meio de DI ([injeção de dependência](xref:fundamentals/dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="5e369-145">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="5e369-146">O ASP.NET Core inclui um contêiner nativo de IoC (Inversão de Controle) que dá suporte à [injeção de construtor](xref:mvc/controllers/dependency-injection#constructor-injection) por padrão.</span><span class="sxs-lookup"><span data-stu-id="5e369-146">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="5e369-147">É possível substituir o contêiner padrão, se desejado.</span><span class="sxs-lookup"><span data-stu-id="5e369-147">You can replace the default container if you wish.</span></span> <span data-ttu-id="5e369-148">Além do [benefício de seu acoplamento flexível](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), a DI disponibiliza serviços, tais como [registro em log](xref:fundamentals/logging/index), por todo o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5e369-148">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="5e369-149">Para obter mais informações, consulte <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="5e369-149">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="5e369-150">Middleware</span><span class="sxs-lookup"><span data-stu-id="5e369-150">Middleware</span></span>

<span data-ttu-id="5e369-151">No ASP.NET Core, você compõe o pipeline de solicitação usando o [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="5e369-151">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="5e369-152">O middleware do ASP.NET Core executa operações assíncronas em um `HttpContext` e então invoca o próximo middleware no pipeline ou encerra a solicitação.</span><span class="sxs-lookup"><span data-stu-id="5e369-152">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="5e369-153">Por convenção, um componente de middleware chamado "XYZ" é adicionado ao pipeline invocando-se um método de extensão `UseXYZ` no método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="5e369-153">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="5e369-154">O ASP.NET Core inclui um conjunto de middleware interno, e você pode escrever seu próprio middleware personalizado.</span><span class="sxs-lookup"><span data-stu-id="5e369-154">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="5e369-155">A [OWIN (Open Web Interface para .NET)](xref:fundamentals/owin), que permite que aplicativos Web sejam desacoplados de servidores Web, é compatível com aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e369-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="5e369-156">Para obter mais informações, consulte <xref:fundamentals/middleware/index> e <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="5e369-156">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="5e369-157">Iniciar solicitações HTTP</span><span class="sxs-lookup"><span data-stu-id="5e369-157">Initiate HTTP requests</span></span>

<span data-ttu-id="5e369-158">O <xref:System.Net.Http.IHttpClientFactory> está disponível para acessar instâncias de <xref:System.Net.Http.HttpClient> para fazer solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e369-158"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="5e369-159">Para obter mais informações, consulte <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="5e369-159">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="5e369-160">Ambientes</span><span class="sxs-lookup"><span data-stu-id="5e369-160">Environments</span></span>

<span data-ttu-id="5e369-161">Os ambientes, como *Desenvolvimento* e *Produção*, são uma noção de primeira classe no ASP.NET Core e podem ser definidos usando uma variável de ambiente, um arquivo de configurações e um argumento de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="5e369-161">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="5e369-162">Para obter mais informações, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="5e369-162">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="5e369-163">Hospedagem</span><span class="sxs-lookup"><span data-stu-id="5e369-163">Hosting</span></span>

<span data-ttu-id="5e369-164">Os aplicativos ASP.NET Core configuram e iniciam um *host*, que é responsável pelo gerenciamento de inicialização e de tempo de vida do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5e369-164">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="5e369-165">Para obter mais informações, consulte <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="5e369-165">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="5e369-166">Servidores</span><span class="sxs-lookup"><span data-stu-id="5e369-166">Servers</span></span>

<span data-ttu-id="5e369-167">O modelo de hospedagem do ASP.NET Core não escuta diretamente as solicitações.</span><span class="sxs-lookup"><span data-stu-id="5e369-167">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="5e369-168">O modelo de host se baseia em uma implementação do servidor HTTP para encaminhar a solicitação ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5e369-168">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="5e369-169">A solicitação encaminhada é empacotada como um conjunto de objetos de recurso que podem ser acessados por meio de interfaces.</span><span class="sxs-lookup"><span data-stu-id="5e369-169">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="5e369-170">O ASP.NET Core inclui um servidor Web gerenciado e de plataforma cruzada chamado [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="5e369-170">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="5e369-171">O Kestrel normalmente é executado por trás de um servidor Web de produção, assim como [IIS](https://www.iis.net/) ou [Nginx](http://nginx.org) em uma configuração de proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="5e369-171">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="5e369-172">O Kestrel também pode ser executado como um servidor de borda voltado para o público exposto diretamente à Internet no ASP.NET Core 2.0 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="5e369-172">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="5e369-173">Para obter mais informações, consulte <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="5e369-173">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="5e369-174">Configuração</span><span class="sxs-lookup"><span data-stu-id="5e369-174">Configuration</span></span>

<span data-ttu-id="5e369-175">O ASP.NET Core usa um modelo de configuração com base nos pares de nome-valor.</span><span class="sxs-lookup"><span data-stu-id="5e369-175">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="5e369-176">O modelo de configuração não baseado em <xref:System.Configuration> ou em *web.config*. A configuração obtém as definições de um conjunto ordenado de provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="5e369-176">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="5e369-177">Os provedores internos de configuração dão suporte a uma variedade de formatos de arquivo (XML, JSON, INI), variáveis de ambiente e argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="5e369-177">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="5e369-178">Você também pode escrever seus próprios provedores de configuração personalizados.</span><span class="sxs-lookup"><span data-stu-id="5e369-178">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="5e369-179">Para obter mais informações, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="5e369-179">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="5e369-180">Registrando em log</span><span class="sxs-lookup"><span data-stu-id="5e369-180">Logging</span></span>

<span data-ttu-id="5e369-181">O ASP.NET Core dá suporte a uma API de registro em log que funciona com uma variedade de provedores de logs.</span><span class="sxs-lookup"><span data-stu-id="5e369-181">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="5e369-182">Os provedores internos dão suporte ao envio de logs para um ou mais destinos.</span><span class="sxs-lookup"><span data-stu-id="5e369-182">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="5e369-183">As estruturas de registro em log de terceiros podem ser usadas.</span><span class="sxs-lookup"><span data-stu-id="5e369-183">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="5e369-184">Para obter mais informações, consulte <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="5e369-184">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="5e369-185">Tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="5e369-185">Error handling</span></span>

<span data-ttu-id="5e369-186">O ASP.NET Core tem cenários internos para tratamento de erros em aplicativos, incluindo uma página de exceção de desenvolvedor, páginas de erro personalizadas, páginas de código de status estático e tratamento de exceções de inicialização.</span><span class="sxs-lookup"><span data-stu-id="5e369-186">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="5e369-187">Para obter mais informações, consulte <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="5e369-187">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="5e369-188">Roteamento</span><span class="sxs-lookup"><span data-stu-id="5e369-188">Routing</span></span>

<span data-ttu-id="5e369-189">O ASP.NET Core oferece cenários para roteamento de solicitações de aplicativo para manipuladores de rotas.</span><span class="sxs-lookup"><span data-stu-id="5e369-189">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="5e369-190">Para obter mais informações, consulte <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="5e369-190">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="file-providers"></a><span data-ttu-id="5e369-191">Provedores de arquivo</span><span class="sxs-lookup"><span data-stu-id="5e369-191">File Providers</span></span>

<span data-ttu-id="5e369-192">O ASP.NET Core resume o acesso de sistema de arquivos por meio do uso de Provedores de Arquivo, que oferece uma interface comum para trabalhar com arquivos entre plataformas.</span><span class="sxs-lookup"><span data-stu-id="5e369-192">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="5e369-193">Para obter mais informações, consulte <xref:fundamentals/file-providers>.</span><span class="sxs-lookup"><span data-stu-id="5e369-193">For more information, see <xref:fundamentals/file-providers>.</span></span>

## <a name="static-files"></a><span data-ttu-id="5e369-194">Arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="5e369-194">Static files</span></span>

<span data-ttu-id="5e369-195">O middleware de arquivos estáticos fornece arquivos estáticos, tais como arquivos HTML, CSS, de imagem e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5e369-195">Static Files Middleware serves static files, such as HTML, CSS, image, and JavaScript files.</span></span>

<span data-ttu-id="5e369-196">Para obter mais informações, consulte <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="5e369-196">For more information, see <xref:fundamentals/static-files>.</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="5e369-197">Estado de sessão e de aplicativo</span><span class="sxs-lookup"><span data-stu-id="5e369-197">Session and app state</span></span>

<span data-ttu-id="5e369-198">O ASP.NET Core oferece várias abordagens para preservar o estado de sessão e de aplicativo enquanto um usuário procura um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="5e369-198">ASP.NET Core offers several approaches to preserve session and app state while a user browses a web app.</span></span>

<span data-ttu-id="5e369-199">Para obter mais informações, consulte <xref:fundamentals/app-state>.</span><span class="sxs-lookup"><span data-stu-id="5e369-199">For more information, see <xref:fundamentals/app-state>.</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="5e369-200">Globalização e localização</span><span class="sxs-lookup"><span data-stu-id="5e369-200">Globalization and localization</span></span>

<span data-ttu-id="5e369-201">Criar um site multilíngue com o ASP.NET Core permite que seu site alcance um público maior.</span><span class="sxs-lookup"><span data-stu-id="5e369-201">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="5e369-202">O ASP.NET Core fornece serviços e middleware para localização de conteúdo em diferentes idiomas e culturas.</span><span class="sxs-lookup"><span data-stu-id="5e369-202">ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>

<span data-ttu-id="5e369-203">Para obter mais informações, consulte <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="5e369-203">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="request-features"></a><span data-ttu-id="5e369-204">Recursos de solicitação</span><span class="sxs-lookup"><span data-stu-id="5e369-204">Request features</span></span>

<span data-ttu-id="5e369-205">Os detalhes de implementação de servidor Web relacionados a solicitações HTTP e respostas são definidos em interfaces.</span><span class="sxs-lookup"><span data-stu-id="5e369-205">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="5e369-206">Essas interfaces são usadas pelas implementações de servidor e pelo middleware para criar e modificar o pipeline de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5e369-206">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="5e369-207">Para obter mais informações, consulte <xref:fundamentals/request-features>.</span><span class="sxs-lookup"><span data-stu-id="5e369-207">For more information, see <xref:fundamentals/request-features>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="5e369-208">Tarefas em segundo plano</span><span class="sxs-lookup"><span data-stu-id="5e369-208">Background tasks</span></span>

<span data-ttu-id="5e369-209">As tarefas em segundo plano são implementadas como *serviços hospedados*.</span><span class="sxs-lookup"><span data-stu-id="5e369-209">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="5e369-210">Um serviço hospedado é uma classe com lógica de tarefa em segundo plano que implementa a interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="5e369-210">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="5e369-211">Para obter mais informações, consulte <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="5e369-211">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="5e369-212">Acessar o HttpContext</span><span class="sxs-lookup"><span data-stu-id="5e369-212">Access HttpContext</span></span>

<span data-ttu-id="5e369-213">`HttpContext` está disponível automaticamente durante o processamento de solicitações com Razor Pages e com o MVC.</span><span class="sxs-lookup"><span data-stu-id="5e369-213">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="5e369-214">Em circunstâncias em que `HttpContext` não está prontamente disponível, você pode acessar o `HttpContext` por meio da interface <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> e sua implementação padrão, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="5e369-214">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="5e369-215">Para obter mais informações, consulte <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="5e369-215">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="websockets"></a><span data-ttu-id="5e369-216">WebSockets</span><span class="sxs-lookup"><span data-stu-id="5e369-216">WebSockets</span></span>

<span data-ttu-id="5e369-217">O [WebSocket](https://wikipedia.org/wiki/WebSocket) é um protocolo que permite canais de comunicação persistentes bidirecionais em conexões TCP.</span><span class="sxs-lookup"><span data-stu-id="5e369-217">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="5e369-218">Ele é usado em aplicativos como chat, cotações de ações, jogos e em qualquer lugar que desejar funcionalidade em tempo real em um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="5e369-218">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="5e369-219">O ASP.NET Core dá suporte a cenários de soquete da Web.</span><span class="sxs-lookup"><span data-stu-id="5e369-219">ASP.NET Core supports web socket scenarios.</span></span>

<span data-ttu-id="5e369-220">Para obter mais informações, consulte <xref:fundamentals/websockets>.</span><span class="sxs-lookup"><span data-stu-id="5e369-220">For more information, see <xref:fundamentals/websockets>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="5e369-221">Metapacote Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="5e369-221">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="5e369-222">O metapacote [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) simplifica o gerenciamento de pacotes.</span><span class="sxs-lookup"><span data-stu-id="5e369-222">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span>

<span data-ttu-id="5e369-223">Para obter mais informações, consulte <xref:fundamentals/metapackage-app>.</span><span class="sxs-lookup"><span data-stu-id="5e369-223">For more information, see <xref:fundamentals/metapackage-app>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="5e369-224">Metapacote Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="5e369-224">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="5e369-225">O [Metapacote do Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core inclui:</span><span class="sxs-lookup"><span data-stu-id="5e369-225">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="5e369-226">Todos os pacotes com suporte da equipe do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e369-226">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="5e369-227">Todos os pacotes compatíveis com o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5e369-227">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="5e369-228">Dependências internas e de terceiros usadas por ASP.NET Core e pelo Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5e369-228">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="5e369-229">Para obter mais informações, consulte <xref:fundamentals/metapackage>.</span><span class="sxs-lookup"><span data-stu-id="5e369-229">For more information, see <xref:fundamentals/metapackage>.</span></span>

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="5e369-230">Tempo de execução do .NET Core vs do .NET Framework</span><span class="sxs-lookup"><span data-stu-id="5e369-230">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="5e369-231">Um aplicativo do ASP.NET Core pode atingir o tempo de execução do .NET Core ou do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5e369-231">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="5e369-232">Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="5e369-232">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="5e369-233">Escolher entre o ASP.NET Core e o ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5e369-233">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="5e369-234">Para obter mais informações sobre como escolher entre ASP.NET Core e ASP.NET, consulte <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span><span class="sxs-lookup"><span data-stu-id="5e369-234">For more information on choosing between ASP.NET Core and ASP.NET, see <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span></span>
