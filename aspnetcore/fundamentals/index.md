---
title: Conceitos básicos do ASP.NET Core
author: rick-anderson
description: Descubra os conceitos fundamentais para a criação de aplicativos do ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 07/02/2018
uid: fundamentals/index
ms.openlocfilehash: 30c456685ce26522faff9b58fbd2977ad2f2869a
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202621"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="53ea7-103">Conceitos básicos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53ea7-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="53ea7-104">Um aplicativo ASP.NET Core é um aplicativo de console que cria um servidor Web em seu método `Main`:</span><span class="sxs-lookup"><span data-stu-id="53ea7-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="53ea7-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="53ea7-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="53ea7-106">O método `Main` invoca `WebHost.CreateDefaultBuilder`, que segue o padrão de construtor para criar um host de aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="53ea7-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="53ea7-107">O construtor tem métodos que definem o servidor Web (por exemplo, `UseKestrel`) e a classe de inicialização (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="53ea7-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="53ea7-108">No exemplo anterior, o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é alocado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="53ea7-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="53ea7-109">O host Web do ASP.NET Core tenta executar no IIS, se disponível.</span><span class="sxs-lookup"><span data-stu-id="53ea7-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="53ea7-110">Outros servidores Web como [HTTP.sys](xref:fundamentals/servers/httpsys) podem ser usados ao chamar o método de extensão apropriado.</span><span class="sxs-lookup"><span data-stu-id="53ea7-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="53ea7-111">`UseStartup` é explicado em mais detalhes na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="53ea7-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="53ea7-112">`IWebHostBuilder`, o tipo de retorno da invocação de `WebHost.CreateDefaultBuilder`, fornece muitos métodos opcionais.</span><span class="sxs-lookup"><span data-stu-id="53ea7-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="53ea7-113">Alguns desses métodos incluem `UseHttpSys` para hospedar o aplicativo em HTTP.sys e `UseContentRoot` para especificar o diretório de conteúdo raiz.</span><span class="sxs-lookup"><span data-stu-id="53ea7-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="53ea7-114">Os métodos `Build` e `Run` compilam o objeto `IWebHost` que hospeda o aplicativo e começa a escutar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="53ea7-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="53ea7-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="53ea7-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="53ea7-116">O método `Main` usa `WebHostBuilder`, que segue o padrão de construtor para criar um host de aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="53ea7-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="53ea7-117">O construtor tem métodos que definem o servidor Web (por exemplo, `UseKestrel`) e a classe de inicialização (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="53ea7-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="53ea7-118">No exemplo anterior, o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) é usado.</span><span class="sxs-lookup"><span data-stu-id="53ea7-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="53ea7-119">Outros servidores Web como [WebListener](xref:fundamentals/servers/weblistener) podem ser usados ao chamar o método de extensão apropriado.</span><span class="sxs-lookup"><span data-stu-id="53ea7-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="53ea7-120">`UseStartup` é explicado em mais detalhes na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="53ea7-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="53ea7-121">O `WebHostBuilder` fornece muitos métodos opcionais, incluindo `UseIISIntegration`, para hospedagem no IIS e no IIS Express, e `UseContentRoot`, para especificar o diretório do conteúdo raiz.</span><span class="sxs-lookup"><span data-stu-id="53ea7-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="53ea7-122">Os métodos `Build` e `Run` compilam o objeto `IWebHost` que hospeda o aplicativo e começa a escutar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="53ea7-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="53ea7-123">Inicialização</span><span class="sxs-lookup"><span data-stu-id="53ea7-123">Startup</span></span>

<span data-ttu-id="53ea7-124">O método `UseStartup` em `WebHostBuilder` especifica a classe `Startup` para seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="53ea7-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="53ea7-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="53ea7-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="53ea7-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="53ea7-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="53ea7-127">É na classe `Startup` que você define o pipeline de tratamento de solicitação e é nela que todos os serviços que o aplicativo precisa estão configurados.</span><span class="sxs-lookup"><span data-stu-id="53ea7-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="53ea7-128">A classe `Startup` deve ser pública e conter os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="53ea7-128">The `Startup` class must be public and contain the following methods:</span></span>

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

<span data-ttu-id="53ea7-129">`ConfigureServices` define os [Serviços](#dependency-injection-services) usados pelo seu aplicativo (por exemplo, ASP.NET Core MVC, Entity Framework Core, Identity etc.).</span><span class="sxs-lookup"><span data-stu-id="53ea7-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="53ea7-130">`Configure` define o [middleware](xref:fundamentals/middleware/index) para o pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="53ea7-130">`Configure` defines the [middleware](xref:fundamentals/middleware/index) for the request pipeline.</span></span>

<span data-ttu-id="53ea7-131">Para obter mais informações, veja [Inicialização do aplicativo](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="53ea7-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="53ea7-132">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="53ea7-132">Content root</span></span>

<span data-ttu-id="53ea7-133">A raiz do conteúdo é o caminho base para qualquer conteúdo usado pelo aplicativo, tal como exibições, [Páginas do Razor](xref:razor-pages/index) e ativos estáticos.</span><span class="sxs-lookup"><span data-stu-id="53ea7-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:razor-pages/index), and static assets.</span></span> <span data-ttu-id="53ea7-134">Por padrão, a raiz do conteúdo é o mesmo caminho base do aplicativo para o executável que hospeda o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="53ea7-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="53ea7-135">Raiz da Web</span><span class="sxs-lookup"><span data-stu-id="53ea7-135">Web root</span></span>

<span data-ttu-id="53ea7-136">A raiz Web de um aplicativo é o diretório do projeto que contém recursos públicos e estáticos como CSS, JavaScript e arquivos de imagem.</span><span class="sxs-lookup"><span data-stu-id="53ea7-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="53ea7-137">Injeção de dependência (serviços)</span><span class="sxs-lookup"><span data-stu-id="53ea7-137">Dependency injection (services)</span></span>

<span data-ttu-id="53ea7-138">Um serviço é um componente que é destinado ao consumo comum em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="53ea7-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="53ea7-139">Os serviços são disponibilizados por meio de DI ([injeção de dependência](xref:fundamentals/dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="53ea7-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="53ea7-140">O ASP.NET Core inclui um contêiner nativo de IoC (**I**nversão **d**e **C**ontrole) que dá suporte a injeção de [construtor](xref:mvc/controllers/dependency-injection#constructor-injection) por padrão.</span><span class="sxs-lookup"><span data-stu-id="53ea7-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="53ea7-141">É possível substituir o contêiner nativo padrão se desejar.</span><span class="sxs-lookup"><span data-stu-id="53ea7-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="53ea7-142">Além do benefício de seu acoplamento flexível, a DI disponibiliza serviços por todo o seu aplicativo (por exemplo, [registrar em log](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="53ea7-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="53ea7-143">Para obter mais informações, consulte [Injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="53ea7-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="53ea7-144">Middleware</span><span class="sxs-lookup"><span data-stu-id="53ea7-144">Middleware</span></span>

<span data-ttu-id="53ea7-145">No ASP.NET Core, você compõe o pipeline de solicitação usando o [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="53ea7-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="53ea7-146">O middleware do ASP.NET Core executa lógica assíncrona em um `HttpContext` e então invoca o próximo middleware na sequência ou encerra a solicitação diretamente.</span><span class="sxs-lookup"><span data-stu-id="53ea7-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="53ea7-147">Um componente de middleware chamado "XYZ" é adicionado ao invocar um `UseXYZ` método de extensão no método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="53ea7-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="53ea7-148">O ASP.NET Core inclui um conjunto avançado de middleware interno:</span><span class="sxs-lookup"><span data-stu-id="53ea7-148">ASP.NET Core includes a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="53ea7-149">Arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="53ea7-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="53ea7-150">Roteamento</span><span class="sxs-lookup"><span data-stu-id="53ea7-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="53ea7-151">Autenticação</span><span class="sxs-lookup"><span data-stu-id="53ea7-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="53ea7-152">Middleware de compactação de resposta</span><span class="sxs-lookup"><span data-stu-id="53ea7-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="53ea7-153">Middleware de regravação de URL</span><span class="sxs-lookup"><span data-stu-id="53ea7-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="53ea7-154">O middleware com base em [OWIN](http://owin.org) está disponível para aplicativos do ASP.NET Core e é possível escrever seu próprio middleware personalizado.</span><span class="sxs-lookup"><span data-stu-id="53ea7-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="53ea7-155">Para obter mais informações, consulte [Middleware](xref:fundamentals/middleware/index) e [OWIN (Interface da Web Aberta para .NET)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="53ea7-155">For more information, see [Middleware](xref:fundamentals/middleware/index) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="53ea7-156">Iniciar solicitações HTTP</span><span class="sxs-lookup"><span data-stu-id="53ea7-156">Initiate HTTP requests</span></span>

<span data-ttu-id="53ea7-157">Para saber mais sobre como usar o `IHttpClientFactory` para acessar instâncias do `HttpClient` a fim de fazer solicitações HTTP, confira [Iniciar solicitações HTTP](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="53ea7-157">For information about using `IHttpClientFactory` to access `HttpClient` instances to make HTTP requests, see [Initiate HTTP requests](xref:fundamentals/http-requests).</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="53ea7-158">Ambientes</span><span class="sxs-lookup"><span data-stu-id="53ea7-158">Environments</span></span>

<span data-ttu-id="53ea7-159">Os ambientes, como "Desenvolvimento" e "Produção", são uma noção de primeira classe no ASP.NET Core e podem ser definidos usando variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="53ea7-159">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="53ea7-160">Para obter mais informações, veja [Usar vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="53ea7-160">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="53ea7-161">Configuração</span><span class="sxs-lookup"><span data-stu-id="53ea7-161">Configuration</span></span>

<span data-ttu-id="53ea7-162">O ASP.NET Core usa um modelo de configuração com base nos pares de nome-valor.</span><span class="sxs-lookup"><span data-stu-id="53ea7-162">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="53ea7-163">O modelo de configuração não baseado em `System.Configuration` ou em *web.config*. A configuração obtém as definições de um conjunto ordenado de provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="53ea7-163">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="53ea7-164">Os provedores internos de configuração dão suporte a uma variedade de formatos de arquivo (XML, JSON, INI) e variáveis de ambiente para habilitar a configuração do ambiente.</span><span class="sxs-lookup"><span data-stu-id="53ea7-164">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="53ea7-165">Você também pode escrever seus próprios provedores de configuração personalizados.</span><span class="sxs-lookup"><span data-stu-id="53ea7-165">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="53ea7-166">Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="53ea7-166">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="53ea7-167">Registro em log</span><span class="sxs-lookup"><span data-stu-id="53ea7-167">Logging</span></span>

<span data-ttu-id="53ea7-168">O ASP.NET Core dá suporte a uma API de registro em log que funciona com uma variedade de provedores de logs.</span><span class="sxs-lookup"><span data-stu-id="53ea7-168">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="53ea7-169">Os provedores internos dão suporte ao envio de logs para um ou mais destinos.</span><span class="sxs-lookup"><span data-stu-id="53ea7-169">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="53ea7-170">As estruturas de registro em log de terceiros podem ser usadas.</span><span class="sxs-lookup"><span data-stu-id="53ea7-170">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="53ea7-171">Consulte [Log](xref:fundamentals/logging/index) para obter mais informações</span><span class="sxs-lookup"><span data-stu-id="53ea7-171">For more information, see [Logging](xref:fundamentals/logging/index)</span></span>

## <a name="error-handling"></a><span data-ttu-id="53ea7-172">Tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="53ea7-172">Error handling</span></span>

<span data-ttu-id="53ea7-173">O ASP.NET Core tem recursos internos para manipular erros em aplicativos, incluindo uma página de exceção de desenvolvedor, páginas de erro personalizadas, páginas de código de status estático e tratamento de exceções de inicialização.</span><span class="sxs-lookup"><span data-stu-id="53ea7-173">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="53ea7-174">Para obter mais informações, veja [Como tratar erros](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="53ea7-174">For more information, see [how to handle errors](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="53ea7-175">Roteamento</span><span class="sxs-lookup"><span data-stu-id="53ea7-175">Routing</span></span>

<span data-ttu-id="53ea7-176">O ASP.NET Core oferece recursos para roteamento de solicitações de aplicativo para manipuladores de rotas.</span><span class="sxs-lookup"><span data-stu-id="53ea7-176">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="53ea7-177">Para obter mais informações, consulte [Roteamento](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="53ea7-177">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="53ea7-178">Provedores de arquivo</span><span class="sxs-lookup"><span data-stu-id="53ea7-178">File providers</span></span>

<span data-ttu-id="53ea7-179">O ASP.NET Core resume o acesso de sistema de arquivos por meio do uso de Provedores de Arquivo, que oferece uma interface comum para trabalhar com arquivos entre plataformas.</span><span class="sxs-lookup"><span data-stu-id="53ea7-179">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="53ea7-180">Para obter mais informações, consulte [Provedores de Arquivos](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="53ea7-180">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="53ea7-181">Arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="53ea7-181">Static files</span></span>

<span data-ttu-id="53ea7-182">O middleware de arquivos estáticos fornece arquivos estáticos, como HTML, CSS, imagem e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="53ea7-182">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="53ea7-183">Para obter mais informações, veja [Arquivos estáticos](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="53ea7-183">For more information, see [Static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="53ea7-184">Hospedagem</span><span class="sxs-lookup"><span data-stu-id="53ea7-184">Hosting</span></span>

<span data-ttu-id="53ea7-185">Os aplicativos ASP.NET Core configuram e iniciam um *host*, que é responsável pelo gerenciamento de inicialização e de tempo de vida do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="53ea7-185">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="53ea7-186">Para saber mais, confira [Host no ASP.NET Core](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="53ea7-186">For more information, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="53ea7-187">Estado de sessão e de aplicativo</span><span class="sxs-lookup"><span data-stu-id="53ea7-187">Session and app state</span></span>

<span data-ttu-id="53ea7-188">O ASP.NET Core oferece várias abordagens para preservar o estado de sessão e de aplicativo enquanto o usuário procura um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="53ea7-188">ASP.NET Core offers several approaches to preserve session and app state while the user browses a web app.</span></span>

<span data-ttu-id="53ea7-189">Para obter mais informações, consulte [Estado de sessão e aplicativo](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="53ea7-189">For more information, see [Session and app state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="53ea7-190">Servidores</span><span class="sxs-lookup"><span data-stu-id="53ea7-190">Servers</span></span>

<span data-ttu-id="53ea7-191">O modelo de hospedagem do ASP.NET Core não escuta diretamente as solicitações.</span><span class="sxs-lookup"><span data-stu-id="53ea7-191">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="53ea7-192">O modelo de host se baseia em uma implementação do servidor HTTP para encaminhar a solicitação ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="53ea7-192">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="53ea7-193">A solicitação encaminhada é empacotada como um conjunto de objetos de recurso que podem ser acessados por meio de interfaces.</span><span class="sxs-lookup"><span data-stu-id="53ea7-193">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="53ea7-194">O ASP.NET Core inclui um servidor Web gerenciado e de plataforma cruzada chamado [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="53ea7-194">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="53ea7-195">O Kestrel normalmente é executado atrás de um servidor Web de produção, como [IIS](https://www.iis.net/) ou [Nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="53ea7-195">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="53ea7-196">O Kestrel pode ser executado como um servidor de borda.</span><span class="sxs-lookup"><span data-stu-id="53ea7-196">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="53ea7-197">Para obter mais informações, consulte [Servidores](xref:fundamentals/servers/index) e os seguintes tópicos:</span><span class="sxs-lookup"><span data-stu-id="53ea7-197">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="53ea7-198">Kestrel</span><span class="sxs-lookup"><span data-stu-id="53ea7-198">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="53ea7-199">Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53ea7-199">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="53ea7-200">[HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente chamado [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="53ea7-200">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="53ea7-201">Globalização e localização</span><span class="sxs-lookup"><span data-stu-id="53ea7-201">Globalization and localization</span></span>

<span data-ttu-id="53ea7-202">Criar um site multilíngue com o ASP.NET Core permite que seu site alcance um público maior.</span><span class="sxs-lookup"><span data-stu-id="53ea7-202">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="53ea7-203">O ASP.NET Core fornece serviços e middleware para localização em diferentes idiomas e culturas.</span><span class="sxs-lookup"><span data-stu-id="53ea7-203">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="53ea7-204">Para obter mais informações, consulte [Globalização e localização](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="53ea7-204">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="53ea7-205">Recursos de solicitação</span><span class="sxs-lookup"><span data-stu-id="53ea7-205">Request features</span></span>

<span data-ttu-id="53ea7-206">Os detalhes de implementação de servidor Web relacionados a solicitações HTTP e respostas são definidos em interfaces.</span><span class="sxs-lookup"><span data-stu-id="53ea7-206">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="53ea7-207">Essas interfaces são usadas pelas implementações de servidor e pelo middleware para criar e modificar o pipeline de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="53ea7-207">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="53ea7-208">Para obter mais informações, consulte [Solicitar Recursos](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="53ea7-208">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="background-tasks"></a><span data-ttu-id="53ea7-209">Tarefas em segundo plano</span><span class="sxs-lookup"><span data-stu-id="53ea7-209">Background tasks</span></span>

<span data-ttu-id="53ea7-210">As tarefas em segundo plano são implementadas como *serviços hospedados*.</span><span class="sxs-lookup"><span data-stu-id="53ea7-210">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="53ea7-211">Um serviço hospedado é uma classe com lógica de tarefa em segundo plano que implementa a interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="53ea7-211">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span>

<span data-ttu-id="53ea7-212">Saiba mais em [Tarefas em segundo plano com serviços hospedados](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="53ea7-212">For more information, see [Background tasks with hosted services](xref:fundamentals/host/hosted-services).</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="53ea7-213">Acessar o HttpContext</span><span class="sxs-lookup"><span data-stu-id="53ea7-213">Access HttpContext</span></span>

<span data-ttu-id="53ea7-214">Acesse o `HttpContext` por meio da interface [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) e da implementação padrão do [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="53ea7-214">Access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span>

<span data-ttu-id="53ea7-215">Para obter mais informações, consulte <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="53ea7-215">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="53ea7-216">OWIN (Open Web Interface para .NET)</span><span class="sxs-lookup"><span data-stu-id="53ea7-216">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="53ea7-217">O ASP.NET Core dá suporte para OWIN (Open Web Interface para .NET).</span><span class="sxs-lookup"><span data-stu-id="53ea7-217">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="53ea7-218">O OWIN permite que os aplicativos Web sejam separados dos servidores Web.</span><span class="sxs-lookup"><span data-stu-id="53ea7-218">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="53ea7-219">Para obter mais informações, consulte [OWIN (Interface da Web Aberta para .NET)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="53ea7-219">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="53ea7-220">WebSockets</span><span class="sxs-lookup"><span data-stu-id="53ea7-220">WebSockets</span></span>

<span data-ttu-id="53ea7-221">O [WebSocket](https://wikipedia.org/wiki/WebSocket) é um protocolo que permite canais de comunicação persistentes bidirecionais em conexões TCP.</span><span class="sxs-lookup"><span data-stu-id="53ea7-221">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="53ea7-222">Ele é usado em aplicativos como chat, cotações de ações, jogos e em qualquer lugar que desejar funcionalidade em tempo real em um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="53ea7-222">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="53ea7-223">O ASP.NET Core dá suporte a recursos de soquete da Web.</span><span class="sxs-lookup"><span data-stu-id="53ea7-223">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="53ea7-224">Para obter mais informações, consulte [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="53ea7-224">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="53ea7-225">Metapacote Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="53ea7-225">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="53ea7-226">O metapacote [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) simplifica o gerenciamento de pacotes.</span><span class="sxs-lookup"><span data-stu-id="53ea7-226">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span> <span data-ttu-id="53ea7-227">Para obter mais informações, veja [Metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="53ea7-227">For more information, see [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end
::: moniker range="= aspnetcore-2.0"
## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="53ea7-228">Metapacote Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="53ea7-228">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="53ea7-229">O [Metapacote do Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core inclui:</span><span class="sxs-lookup"><span data-stu-id="53ea7-229">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="53ea7-230">Todos os pacotes com suporte da equipe do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="53ea7-230">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="53ea7-231">Todos os pacotes compatíveis com o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="53ea7-231">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="53ea7-232">Dependências internas e de terceiros usadas por ASP.NET Core e pelo Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="53ea7-232">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="53ea7-233">Para obter mais informações, consulte [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="53ea7-233">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>
::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="53ea7-234">Tempo de execução do .NET Core vs do .NET Framework</span><span class="sxs-lookup"><span data-stu-id="53ea7-234">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="53ea7-235">Um aplicativo do ASP.NET Core pode atingir o tempo de execução do .NET Core ou do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="53ea7-235">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="53ea7-236">Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="53ea7-236">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="53ea7-237">Escolher entre o ASP.NET Core e o ASP.NET</span><span class="sxs-lookup"><span data-stu-id="53ea7-237">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="53ea7-238">Para obter mais informações sobre como escolher entre ASP.NET Core e ASP.NET, consulte [Escolha entre ASP.NET Core e ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="53ea7-238">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
