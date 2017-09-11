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
# <a name="aspnet-core-fundamentals-overview"></a><span data-ttu-id="d06f0-104">Visão geral de conceitos básicos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d06f0-104">ASP.NET Core fundamentals overview</span></span>

<span data-ttu-id="d06f0-105">Um aplicativo ASP.NET Core é um aplicativo de console que cria um servidor Web em seu método `Main`:</span><span class="sxs-lookup"><span data-stu-id="d06f0-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d06f0-106">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d06f0-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d06f0-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span><span class="sxs-lookup"><span data-stu-id="d06f0-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span></span>

<span data-ttu-id="d06f0-108">O método `Main` invoca `WebHost.CreateDefaultBuilder`, que segue o padrão de construtor para criar um host de aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="d06f0-108">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="d06f0-109">O construtor tem métodos que definem o servidor Web (por exemplo, `UseKestrel`) e a classe de inicialização (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="d06f0-109">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="d06f0-110">No exemplo anterior, um servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é alocado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="d06f0-110">In the preceding example, a [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="d06f0-111">O host Web do ASP.NET Core tentará executar no IIS, se ele estiver disponível.</span><span class="sxs-lookup"><span data-stu-id="d06f0-111">ASP.NET Core's web host will attempt to run on IIS, if it is available.</span></span> <span data-ttu-id="d06f0-112">Outros servidores Web como [HTTP.sys](xref:fundamentals/servers/httpsys) podem ser usados ao chamar o método de extensão apropriado.</span><span class="sxs-lookup"><span data-stu-id="d06f0-112">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="d06f0-113">`UseStartup` é explicado em mais detalhes na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="d06f0-113">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="d06f0-114">`IWebHostBuilder`, o tipo de retorno da invocação de `WebHost.CreateDefaultBuilder`, fornece muitos métodos opcionais.</span><span class="sxs-lookup"><span data-stu-id="d06f0-114">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="d06f0-115">Alguns desses métodos incluem `UseHttpSys` para hospedar o aplicativo em HTTP.sys e `UseContentRoot` para especificar o diretório de conteúdo raiz.</span><span class="sxs-lookup"><span data-stu-id="d06f0-115">Some of these methods include `UseHttpSys` for hosting the application in HTTP.sys, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="d06f0-116">Os métodos `Build` e `Run` compilam o objeto `IWebHost` que hospedará o aplicativo e começam a escutar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="d06f0-116">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d06f0-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d06f0-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d06f0-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="d06f0-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span></span>

<span data-ttu-id="d06f0-119">O método `Main` usa `WebHostBuilder`, que segue o padrão de construtor para criar um host de aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="d06f0-119">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="d06f0-120">O construtor tem métodos que definem o servidor Web (por exemplo, `UseKestrel`) e a classe de inicialização (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="d06f0-120">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="d06f0-121">No exemplo anterior, o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é usado.</span><span class="sxs-lookup"><span data-stu-id="d06f0-121">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="d06f0-122">Outros servidores Web como [WebListener](xref:fundamentals/servers/weblistener) podem ser usados ao chamar o método de extensão apropriado.</span><span class="sxs-lookup"><span data-stu-id="d06f0-122">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="d06f0-123">`UseStartup` é explicado em mais detalhes na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="d06f0-123">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="d06f0-124">`WebHostBuilder` fornece muitos métodos opcionais, incluindo `UseIISIntegration`, para hospedagem no IIS e no IIS Express, e `UseContentRoot` para especificar o diretório do conteúdo raiz.</span><span class="sxs-lookup"><span data-stu-id="d06f0-124">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="d06f0-125">Os métodos `Build` e `Run` compilam o objeto `IWebHost` que hospedará o aplicativo e começam a escutar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="d06f0-125">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="d06f0-126">Inicialização</span><span class="sxs-lookup"><span data-stu-id="d06f0-126">Startup</span></span>

<span data-ttu-id="d06f0-127">O método `UseStartup` em `WebHostBuilder` especifica a classe `Startup` para seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="d06f0-127">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d06f0-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d06f0-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d06f0-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="d06f0-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d06f0-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d06f0-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d06f0-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="d06f0-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span></span>

---

<span data-ttu-id="d06f0-132">É na classe `Startup` que você define o pipeline de tratamento de solicitação e é nela que todos os serviços necessitados pelo aplicativo estão configurados.</span><span class="sxs-lookup"><span data-stu-id="d06f0-132">The `Startup` class is where you define the request handling pipeline and where any services needed by the application are configured.</span></span> <span data-ttu-id="d06f0-133">A classe `Startup` deve ser pública e conter os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="d06f0-133">The `Startup` class must be public and contain the following methods:</span></span>

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

* <span data-ttu-id="d06f0-134">`ConfigureServices` define os [serviços](#services) usados pelo seu aplicativo (como o ASP.NET Core MVC, Entity Framework Core, Identity, etc.).</span><span class="sxs-lookup"><span data-stu-id="d06f0-134">`ConfigureServices` defines the [Services](#services) used by your application (such as ASP.NET Core MVC, Entity Framework Core, Identity, etc.).</span></span>

* <span data-ttu-id="d06f0-135">`Configure` define o [middleware](xref:fundamentals/middleware) no pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="d06f0-135">`Configure` defines the [middleware](xref:fundamentals/middleware) in the request pipeline.</span></span>

<span data-ttu-id="d06f0-136">Para obter mais informações, veja [Inicialização do aplicativo](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="d06f0-136">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="services"></a><span data-ttu-id="d06f0-137">Serviços</span><span class="sxs-lookup"><span data-stu-id="d06f0-137">Services</span></span>

<span data-ttu-id="d06f0-138">Um serviço é um componente que é destinado ao consumo comum em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d06f0-138">A service is a component that is intended for common consumption in an application.</span></span> <span data-ttu-id="d06f0-139">Os serviços são disponibilizados por meio de DI ([injeção de dependência](xref:fundamentals/dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="d06f0-139">Services are made available through [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="d06f0-140">O ASP.NET Core inclui um contêiner nativo de IoC (inversão de controle) que dá suporte a [injeção de construtor](xref:mvc/controllers/dependency-injection#constructor-injection) por padrão.</span><span class="sxs-lookup"><span data-stu-id="d06f0-140">ASP.NET Core includes a native inversion of control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="d06f0-141">O contêiner nativo pode ser substituído pelo contêiner de sua escolha.</span><span class="sxs-lookup"><span data-stu-id="d06f0-141">The native container can be replaced with your container of choice.</span></span> <span data-ttu-id="d06f0-142">Além do benefício de seu acoplamento flexível, a DI disponibiliza serviços por todo o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d06f0-142">In addition to its loose coupling benefit, DI makes services available throughout your application.</span></span> <span data-ttu-id="d06f0-143">Por exemplo, o [registro em log](xref:fundamentals/logging) está disponível em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d06f0-143">For example, [logging](xref:fundamentals/logging) is available throughout your application.</span></span>

<span data-ttu-id="d06f0-144">Para obter mais informações, consulte [Injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d06f0-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="d06f0-145">Middleware</span><span class="sxs-lookup"><span data-stu-id="d06f0-145">Middleware</span></span>

<span data-ttu-id="d06f0-146">No ASP.NET Core, você compõe o pipeline de solicitação usando [Middleware](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="d06f0-146">In ASP.NET Core, you compose your request pipeline using [Middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="d06f0-147">O middleware do ASP.NET Core executa lógica assíncrona em um `HttpContext` e então invoca o próximo middleware na sequência ou encerra a solicitação diretamente.</span><span class="sxs-lookup"><span data-stu-id="d06f0-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="d06f0-148">Um componente de middleware chamado "XYZ" é adicionado invocando-se um método de extensão `UseXYZ` no método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d06f0-148">A middleware component called "XYZ" is added by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="d06f0-149">O ASP.NET Core vem com um conjunto avançado de middleware interno:</span><span class="sxs-lookup"><span data-stu-id="d06f0-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="d06f0-150">Arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="d06f0-150">Static files</span></span>](xref:fundamentals/static-files)

* [<span data-ttu-id="d06f0-151">Roteamento</span><span class="sxs-lookup"><span data-stu-id="d06f0-151">Routing</span></span>](xref:fundamentals/routing)

* [<span data-ttu-id="d06f0-152">Autenticação</span><span class="sxs-lookup"><span data-stu-id="d06f0-152">Authentication</span></span>](xref:security/authentication/index)

<span data-ttu-id="d06f0-153">Você pode usar qualquer middleware com base em [OWIN](http://owin.org) com o ASP.NET Core e você pode escrever seu próprio middleware personalizado.</span><span class="sxs-lookup"><span data-stu-id="d06f0-153">You can use any [OWIN](http://owin.org)-based middleware with ASP.NET Core, and you can write your own custom middleware.</span></span>

<span data-ttu-id="d06f0-154">Para obter mais informações, consulte [Middleware](xref:fundamentals/middleware) e [OWIN (Interface da Web Aberta para .NET)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="d06f0-154">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="servers"></a><span data-ttu-id="d06f0-155">Servidores</span><span class="sxs-lookup"><span data-stu-id="d06f0-155">Servers</span></span>

<span data-ttu-id="d06f0-156">O modelo de hospedagem do ASP.NET Core não escuta solicitações diretamente; em vez disso, ele se baseia em uma implementação do servidor HTTP para encaminhar a solicitação para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d06f0-156">The ASP.NET Core hosting model does not directly listen for requests; rather, it relies on an HTTP server implementation to forward the request to the application.</span></span> <span data-ttu-id="d06f0-157">A solicitação encaminhada é empacotada como um conjunto de objetos de recurso que você pode acessar por meio de interfaces.</span><span class="sxs-lookup"><span data-stu-id="d06f0-157">The forwarded request is wrapped as a set of feature objects that you can access through interfaces.</span></span> <span data-ttu-id="d06f0-158">O aplicativo compõe esse conjunto em um `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="d06f0-158">The application composes this set into an `HttpContext`.</span></span> <span data-ttu-id="d06f0-159">O ASP.NET Core inclui um servidor Web gerenciado e de plataforma cruzada chamado [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d06f0-159">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="d06f0-160">O Kestrel normalmente é executado atrás de um servidor Web de produção como [IIS](https://iis.net) ou [nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="d06f0-160">Kestrel is typically run behind a production web server like [IIS](https://iis.net) or [nginx](http://nginx.org).</span></span>

<span data-ttu-id="d06f0-161">Para obter mais informações, consulte [Servidores](xref:fundamentals/servers/index) e [Hospedagem](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="d06f0-161">For more information, see [Servers](xref:fundamentals/servers/index) and [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="content-root"></a><span data-ttu-id="d06f0-162">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="d06f0-162">Content root</span></span>

<span data-ttu-id="d06f0-163">A raiz do conteúdo é o caminho base para qualquer conteúdo usado pelo aplicativo, tal como exibições, [Páginas do Razor](xref:mvc/razor-pages/index) e ativos estáticos.</span><span class="sxs-lookup"><span data-stu-id="d06f0-163">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="d06f0-164">Por padrão, a conteúdo raiz é o mesmo caminho base do aplicativo para o executável que hospeda o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d06f0-164">By default, the content root is the same as application base path for the executable hosting the application.</span></span> <span data-ttu-id="d06f0-165">Um local alternativo para a raiz do conteúdo é especificado com `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d06f0-165">An alternative location for content root is specified with `WebHostBuilder`.</span></span>

## <a name="web-root"></a><span data-ttu-id="d06f0-166">Raiz da Web</span><span class="sxs-lookup"><span data-stu-id="d06f0-166">Web root</span></span>

<span data-ttu-id="d06f0-167">A raiz de um aplicativo Web é o diretório do projeto que contém recursos públicos e estáticos como CSS, JavaScript e arquivos de imagem.</span><span class="sxs-lookup"><span data-stu-id="d06f0-167">The web root of an application is the directory in the project containing public, static resources like CSS, JavaScript, and image files.</span></span> <span data-ttu-id="d06f0-168">Por padrão, o middleware de arquivos estáticos servirá apenas os arquivos do diretório raiz da Web e seus subdiretórios.</span><span class="sxs-lookup"><span data-stu-id="d06f0-168">By default, the static files middleware will only serve files from the web root directory and its sub-directories.</span></span> <span data-ttu-id="d06f0-169">Para obter mais informações, consulte [trabalhando com arquivos estáticos](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="d06f0-169">See [working with static files](xref:fundamentals/static-files) for more info.</span></span> <span data-ttu-id="d06f0-170">O caminho da raiz da Web assume o valor */wwwroot* por padrão, mas você pode especificar um local diferente usando o `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d06f0-170">The web root path defaults to */wwwroot*, but you can specify a different location using the `WebHostBuilder`.</span></span>

## <a name="configuration"></a><span data-ttu-id="d06f0-171">Configuração</span><span class="sxs-lookup"><span data-stu-id="d06f0-171">Configuration</span></span>

<span data-ttu-id="d06f0-172">O ASP.NET Core usa um novo modelo de configuração para lidar com pares de nome-valor simples.</span><span class="sxs-lookup"><span data-stu-id="d06f0-172">ASP.NET Core uses a new configuration model for handling simple name-value pairs.</span></span> <span data-ttu-id="d06f0-173">O novo modelo de configuração não se baseia em `System.Configuration` ou *web.config*; em vez disso, ele efetua pull de um conjunto ordenado de provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="d06f0-173">The new configuration model is not based on `System.Configuration` or *web.config*; rather, it pulls from an ordered set of configuration providers.</span></span> <span data-ttu-id="d06f0-174">Os provedores internos de configuração dão suporte a uma variedade de formatos de arquivo (XML, JSON, INI) e variáveis de ambiente para habilitar a configuração do ambiente.</span><span class="sxs-lookup"><span data-stu-id="d06f0-174">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="d06f0-175">Você também pode escrever seus próprios provedores de configuração personalizados.</span><span class="sxs-lookup"><span data-stu-id="d06f0-175">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="d06f0-176">Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="d06f0-176">For more information, see [Configuration](xref:fundamentals/configuration).</span></span>

## <a name="environments"></a><span data-ttu-id="d06f0-177">Ambientes</span><span class="sxs-lookup"><span data-stu-id="d06f0-177">Environments</span></span>

<span data-ttu-id="d06f0-178">Ambientes como "Desenvolvimento" e "Produção" são uma noção de primeira classe no ASP.NET Core e podem ser definidos usando variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="d06f0-178">Environments, like "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="d06f0-179">Para obter mais informações, consulte [Trabalhando com vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d06f0-179">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="d06f0-180">Tempo de execução do .NET Core vs do .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d06f0-180">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="d06f0-181">Um aplicativo do ASP.NET Core pode atingir o tempo de execução do .NET Core ou do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d06f0-181">An ASP.NET Core application can target the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="d06f0-182">Para obter mais informações, consulte [Escolhendo entre o .NET Core e .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="d06f0-182">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="additional-information"></a><span data-ttu-id="d06f0-183">Informações adicionais</span><span class="sxs-lookup"><span data-stu-id="d06f0-183">Additional information</span></span>

<span data-ttu-id="d06f0-184">Consulte também os seguintes tópicos:</span><span class="sxs-lookup"><span data-stu-id="d06f0-184">See also the following topics:</span></span>

- [<span data-ttu-id="d06f0-185">Tratamento de erro</span><span class="sxs-lookup"><span data-stu-id="d06f0-185">Error Handling</span></span>](xref:fundamentals/error-handling)
- [<span data-ttu-id="d06f0-186">Provedores de arquivo</span><span class="sxs-lookup"><span data-stu-id="d06f0-186">File Providers</span></span>](xref:fundamentals/file-providers)
- [<span data-ttu-id="d06f0-187">Globalização e localização</span><span class="sxs-lookup"><span data-stu-id="d06f0-187">Globalization and localization</span></span>](xref:fundamentals/localization)
- [<span data-ttu-id="d06f0-188">Registro em log</span><span class="sxs-lookup"><span data-stu-id="d06f0-188">Logging</span></span>](xref:fundamentals/logging)
- [<span data-ttu-id="d06f0-189">Gerenciando o estado do aplicativo</span><span class="sxs-lookup"><span data-stu-id="d06f0-189">Managing Application State</span></span>](xref:fundamentals/app-state)