---
title: "Hospedagem no núcleo do ASP.NET"
author: guardrex
description: "Saiba mais sobre o host da web em ASP.NET Core, que é responsável pelo gerenciamento de inicialização e o tempo de vida do aplicativo."
keywords: Host, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime da web do ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 0ee8827ad3d5464e1645a40d453054b9e23641cf
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/03/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="5a467-104">Hospedagem no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5a467-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="5a467-105">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5a467-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5a467-106">Aplicativos ASP.NET Core configurar e iniciar um *host*.</span><span class="sxs-lookup"><span data-stu-id="5a467-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="5a467-107">O host é responsável pelo gerenciamento de inicialização e o tempo de vida do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a467-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="5a467-108">No mínimo, o host configura um servidor e um pipeline de processamento de solicitação.</span><span class="sxs-lookup"><span data-stu-id="5a467-108">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="5a467-109">Configurando um host</span><span class="sxs-lookup"><span data-stu-id="5a467-109">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5a467-111">Criar um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="5a467-111">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="5a467-112">Isso normalmente é executado no ponto de entrada do aplicativo, o `Main` método.</span><span class="sxs-lookup"><span data-stu-id="5a467-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="5a467-113">Nos modelos de projeto, `Main` está localizado em *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="5a467-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="5a467-114">Um típico *Program.cs* chamadas [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para começar a configurar um host:</span><span class="sxs-lookup"><span data-stu-id="5a467-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="5a467-115">`CreateDefaultBuilder`executa as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="5a467-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="5a467-116">Configura [Kestrel](servers/kestrel.md) como o servidor web.</span><span class="sxs-lookup"><span data-stu-id="5a467-116">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="5a467-117">Para as opções padrão Kestrel, consulte [o Kestrel opções de seção de implementação do servidor web Kestrel no ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="5a467-117">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="5a467-118">Define a raiz de conteúdo para o caminho retornado por [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="5a467-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="5a467-119">Configuração opcional de cargas de:</span><span class="sxs-lookup"><span data-stu-id="5a467-119">Loads optional configuration from:</span></span>
  * <span data-ttu-id="5a467-120">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="5a467-120">*appsettings.json*.</span></span>
  * <span data-ttu-id="5a467-121">*appSettings. . JSON de {ambiente}*.</span><span class="sxs-lookup"><span data-stu-id="5a467-121">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="5a467-122">[Segredos do usuário](xref:security/app-secrets) quando o aplicativo é executado no `Development` ambiente.</span><span class="sxs-lookup"><span data-stu-id="5a467-122">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="5a467-123">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="5a467-123">Environment variables.</span></span>
  * <span data-ttu-id="5a467-124">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="5a467-124">Command-line arguments.</span></span>
* <span data-ttu-id="5a467-125">Configura [log](xref:fundamentals/logging/index) para a saída do console e de depuração.</span><span class="sxs-lookup"><span data-stu-id="5a467-125">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="5a467-126">O log inclui [filtragem de log](xref:fundamentals/logging/index#log-filtering) regras especificadas em uma seção de configuração de log de um *appSettings. JSON* ou *appsettings. { . JSON de ambiente}* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5a467-126">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="5a467-127">Quando em execução por trás do IIS, permite [integração IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="5a467-127">When running behind IIS, enables [IIS integration](xref:publishing/iis).</span></span> <span data-ttu-id="5a467-128">Configura o caminho base e a porta que o servidor deve escutar ao usar o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="5a467-128">Configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="5a467-129">O módulo cria um proxy reverso entre o IIS e Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5a467-129">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="5a467-130">Também configura o aplicativo [capturar erros de inicialização](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="5a467-130">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="5a467-131">Para as opções de padrão do IIS, consulte [o IIS opções seção de Host ASP.NET Core no Windows com o IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="5a467-131">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="5a467-132">O *conteúdo raiz* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="5a467-132">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="5a467-133">Quando o aplicativo é iniciado na pasta raiz do projeto, a pasta raiz do projeto é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="5a467-133">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="5a467-134">Esse é o padrão usado em [Visual Studio](https://www.visualstudio.com/) e [dotnet novos modelos](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="5a467-134">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="5a467-135">Para obter mais informações sobre a configuração do aplicativo, consulte [configuração no ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="5a467-135">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="5a467-136">Como uma alternativa ao uso estático `CreateDefaultBuilder` método, criando um host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) é uma abordagem com suporte com o ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="5a467-136">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="5a467-137">Para obter mais informações, consulte a guia de 1. x do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5a467-137">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5a467-139">Criar um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="5a467-139">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="5a467-140">Criando um host normalmente é executada no ponto de entrada do aplicativo, o `Main` método.</span><span class="sxs-lookup"><span data-stu-id="5a467-140">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="5a467-141">Nos modelos de projeto, `Main` está localizado em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="5a467-141">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="5a467-142">`WebHostBuilder`requer um [server que implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="5a467-142">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="5a467-143">Os servidores internos são [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (antes da versão do ASP.NET Core 2.0, o HTTP.sys foi chamado [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="5a467-143">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="5a467-144">Neste exemplo, o [o método de extensão UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Especifica o servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5a467-144">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="5a467-145">O *conteúdo raiz* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="5a467-145">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="5a467-146">A raiz de conteúdo padrão é obtida para `UseContentRoot` por [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="5a467-146">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="5a467-147">Quando o aplicativo é iniciado na pasta raiz do projeto, a pasta raiz do projeto é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="5a467-147">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="5a467-148">Esse é o padrão usado em [Visual Studio](https://www.visualstudio.com/) e [dotnet novos modelos](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="5a467-148">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="5a467-149">Para usar o IIS como um proxy reverso, chame [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte da construção do host.</span><span class="sxs-lookup"><span data-stu-id="5a467-149">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="5a467-150">`UseIISIntegration`não configure um *servidor*, como [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="5a467-150">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="5a467-151">`UseIISIntegration`configura o caminho base e a porta que o servidor deve escutar ao usar o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) para criar um proxy reverso entre Kestrel e o IIS.</span><span class="sxs-lookup"><span data-stu-id="5a467-151">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="5a467-152">Para usar o IIS com o ASP.NET Core `UseKestrel` e `UseIISIntegration` devem ser especificados.</span><span class="sxs-lookup"><span data-stu-id="5a467-152">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="5a467-153">`UseIISIntegration`ativa somente quando em execução por trás do IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="5a467-153">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="5a467-154">Para obter mais informações, consulte [Introdução ao ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) e [referência de configuração do módulo do ASP.NET Core](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="5a467-154">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="5a467-155">Uma implementação mínima que configura um host (e um aplicativo ASP.NET Core) inclui a especificação de um servidor e a configuração do canal de solicitação do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="5a467-155">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

<span data-ttu-id="5a467-156">Ao configurar um host, [configurar](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) métodos podem ser fornecidos.</span><span class="sxs-lookup"><span data-stu-id="5a467-156">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="5a467-157">Se um `Startup` classe for especificada, ele deve definir um `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="5a467-157">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="5a467-158">Para obter mais informações, consulte [inicialização do aplicativo no ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="5a467-158">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="5a467-159">Diversas chamadas para `ConfigureServices` acrescentar um ao outro.</span><span class="sxs-lookup"><span data-stu-id="5a467-159">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="5a467-160">Diversas chamadas para `Configure` ou `UseStartup` no `WebHostBuilder` substituir as configurações anteriores.</span><span class="sxs-lookup"><span data-stu-id="5a467-160">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="5a467-161">Valores de configuração do host</span><span class="sxs-lookup"><span data-stu-id="5a467-161">Host configuration values</span></span>

<span data-ttu-id="5a467-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) conta com as seguintes abordagens para definir valores de configuração de host:</span><span class="sxs-lookup"><span data-stu-id="5a467-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="5a467-163">Configuração de construtor do host, que inclui variáveis de ambiente com o formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="5a467-163">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="5a467-164">Por exemplo, `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="5a467-164">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="5a467-165">Métodos explícitos, como `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="5a467-165">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="5a467-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e a chave associada.</span><span class="sxs-lookup"><span data-stu-id="5a467-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="5a467-167">Ao definir um valor com `UseSetting`, o valor é definido como uma cadeia de caracteres, independentemente do tipo.</span><span class="sxs-lookup"><span data-stu-id="5a467-167">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="5a467-168">O host usa qualquer opção define um valor de última.</span><span class="sxs-lookup"><span data-stu-id="5a467-168">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="5a467-169">Para obter mais informações, consulte [substituindo configuração](#overriding-configuration) na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="5a467-169">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="5a467-170">Capturar erros de inicialização</span><span class="sxs-lookup"><span data-stu-id="5a467-170">Capture Startup Errors</span></span>

<span data-ttu-id="5a467-171">Essa configuração controla a captura de erros de inicialização.</span><span class="sxs-lookup"><span data-stu-id="5a467-171">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="5a467-172">**Chave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="5a467-172">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="5a467-173">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="5a467-173">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="5a467-174">**Padrão**: assume como padrão `false` , a menos que o aplicativo é executado com Kestrel por trás do IIS, em que o padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="5a467-174">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="5a467-175">**Definido usando**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="5a467-175">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="5a467-176">**Variável de ambiente**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="5a467-176">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="5a467-177">Quando `false`, erros durante o resultado de inicialização no host sair.</span><span class="sxs-lookup"><span data-stu-id="5a467-177">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="5a467-178">Quando `true`, o host captura exceções durante a inicialização e tenta iniciar o servidor.</span><span class="sxs-lookup"><span data-stu-id="5a467-178">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="5a467-181">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="5a467-181">Content Root</span></span>

<span data-ttu-id="5a467-182">Essa configuração determina onde o ASP.NET Core começa a procurar por arquivos de conteúdo, como exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="5a467-182">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="5a467-183">**Chave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="5a467-183">**Key**: contentRoot</span></span>  
<span data-ttu-id="5a467-184">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="5a467-184">**Type**: *string*</span></span>  
<span data-ttu-id="5a467-185">**Padrão**: padrão é a pasta em que reside o assembly de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a467-185">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="5a467-186">**Definido usando**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="5a467-186">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="5a467-187">**Variável de ambiente**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="5a467-187">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="5a467-188">A conteúdo raiz também é usada como o caminho base para o [configuração Web raiz](#web-root).</span><span class="sxs-lookup"><span data-stu-id="5a467-188">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="5a467-189">Se o caminho não existir, o host não será iniciado.</span><span class="sxs-lookup"><span data-stu-id="5a467-189">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="5a467-192">Erros detalhados</span><span class="sxs-lookup"><span data-stu-id="5a467-192">Detailed Errors</span></span>

<span data-ttu-id="5a467-193">Determina se detalhado erros devem ser capturados.</span><span class="sxs-lookup"><span data-stu-id="5a467-193">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="5a467-194">**Chave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="5a467-194">**Key**: detailedErrors</span></span>  
<span data-ttu-id="5a467-195">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="5a467-195">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="5a467-196">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="5a467-196">**Default**: false</span></span>  
<span data-ttu-id="5a467-197">**Definido usando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="5a467-197">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="5a467-198">**Variável de ambiente**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="5a467-198">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="5a467-199">Quando habilitado (ou quando o <a href="#environment">ambiente</a> é definido como `Development`), o aplicativo captura exceções detalhadas.</span><span class="sxs-lookup"><span data-stu-id="5a467-199">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-200">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-200">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="5a467-202">Ambiente</span><span class="sxs-lookup"><span data-stu-id="5a467-202">Environment</span></span>

<span data-ttu-id="5a467-203">Define o ambiente do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a467-203">Sets the app's environment.</span></span>

<span data-ttu-id="5a467-204">**Chave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="5a467-204">**Key**: environment</span></span>  
<span data-ttu-id="5a467-205">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="5a467-205">**Type**: *string*</span></span>  
<span data-ttu-id="5a467-206">**Padrão**: produção</span><span class="sxs-lookup"><span data-stu-id="5a467-206">**Default**: Production</span></span>  
<span data-ttu-id="5a467-207">**Definido usando**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="5a467-207">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="5a467-208">**Variável de ambiente**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="5a467-208">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="5a467-209">O ambiente pode ser definido como qualquer valor.</span><span class="sxs-lookup"><span data-stu-id="5a467-209">The environment can be set to any value.</span></span> <span data-ttu-id="5a467-210">Incluem valores definidos pelo Framework `Development`, `Staging`, e `Production`.</span><span class="sxs-lookup"><span data-stu-id="5a467-210">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="5a467-211">Valores não diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="5a467-211">Values aren't case sensitive.</span></span> <span data-ttu-id="5a467-212">Por padrão, o *ambiente* são lidos a partir de `ASPNETCORE_ENVIRONMENT` variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="5a467-212">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="5a467-213">Ao usar [Visual Studio](https://www.visualstudio.com/), variáveis de ambiente podem ser definidas no *launchSettings.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5a467-213">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="5a467-214">Para obter mais informações, consulte [Trabalhando com vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="5a467-214">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-215">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-215">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-216">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-216">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="5a467-217">Assemblies de inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="5a467-217">Hosting Startup Assemblies</span></span>

<span data-ttu-id="5a467-218">Define a hospedagem assemblies de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a467-218">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="5a467-219">**Chave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="5a467-219">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="5a467-220">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="5a467-220">**Type**: *string*</span></span>  
<span data-ttu-id="5a467-221">**Padrão**: cadeia de caracteres vazia</span><span class="sxs-lookup"><span data-stu-id="5a467-221">**Default**: Empty string</span></span>  
<span data-ttu-id="5a467-222">**Definido usando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="5a467-222">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="5a467-223">**Variável de ambiente**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="5a467-223">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="5a467-224">Uma cadeia delimitada por ponto e vírgula de hospedagem assemblies de inicialização para carregar na inicialização.</span><span class="sxs-lookup"><span data-stu-id="5a467-224">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="5a467-225">Esse recurso é novo no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="5a467-225">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="5a467-226">Embora o valor de configuração padrão é uma cadeia de caracteres vazia, os assemblies de inicialização hospedagem sempre incluem o assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a467-226">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="5a467-227">Quando os assemblies de inicialização de hospedagem são fornecidos, elas são adicionadas ao assembly do aplicativo de carregamento quando o aplicativo cria seus serviços comuns durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="5a467-227">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-229">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-229">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5a467-230">Este recurso está indisponível no núcleo do ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="5a467-230">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="5a467-231">Preferir URLs de hospedagem</span><span class="sxs-lookup"><span data-stu-id="5a467-231">Prefer Hosting URLs</span></span>

<span data-ttu-id="5a467-232">Indica se o host deve escutar nas URLs configuradas com o `WebHostBuilder` em vez das configuradas com o `IServer` implementação.</span><span class="sxs-lookup"><span data-stu-id="5a467-232">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="5a467-233">**Chave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="5a467-233">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="5a467-234">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="5a467-234">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="5a467-235">**Padrão**: true</span><span class="sxs-lookup"><span data-stu-id="5a467-235">**Default**: true</span></span>  
<span data-ttu-id="5a467-236">**Definido usando**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="5a467-236">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="5a467-237">**Variável de ambiente**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="5a467-237">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="5a467-238">Esse recurso é novo no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="5a467-238">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-239">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-239">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-240">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-240">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5a467-241">Este recurso está indisponível no núcleo do ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="5a467-241">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="5a467-242">Impedir a inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="5a467-242">Prevent Hosting Startup</span></span>

<span data-ttu-id="5a467-243">Impede o carregamento automático de hospedagem assemblies de inicialização, incluindo os assemblies de inicialização configurados pelo assembly do aplicativo de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="5a467-243">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="5a467-244">Consulte [adicionar recursos de aplicativo de um assembly externo usando IHostingStartup](xref:hosting/ihostingstartup) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="5a467-244">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="5a467-245">**Chave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="5a467-245">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="5a467-246">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="5a467-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="5a467-247">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="5a467-247">**Default**: false</span></span>  
<span data-ttu-id="5a467-248">**Definido usando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="5a467-248">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="5a467-249">**Variável de ambiente**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="5a467-249">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="5a467-250">Esse recurso é novo no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="5a467-250">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-252">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-252">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5a467-253">Este recurso está indisponível no núcleo do ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="5a467-253">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="5a467-254">URLs de servidor</span><span class="sxs-lookup"><span data-stu-id="5a467-254">Server URLs</span></span>

<span data-ttu-id="5a467-255">Indica os endereços IP ou endereços de host com as portas e protocolos que o servidor deve ouvir para solicitações.</span><span class="sxs-lookup"><span data-stu-id="5a467-255">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="5a467-256">**Chave**: urls</span><span class="sxs-lookup"><span data-stu-id="5a467-256">**Key**: urls</span></span>  
<span data-ttu-id="5a467-257">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="5a467-257">**Type**: *string*</span></span>  
<span data-ttu-id="5a467-258">**Padrão**: http://localhost:5000/</span><span class="sxs-lookup"><span data-stu-id="5a467-258">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="5a467-259">**Definido usando**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="5a467-259">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="5a467-260">**Variável de ambiente**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="5a467-260">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="5a467-261">Definir um separados por ponto e vírgula (;) prefixos de lista de URL para o servidor deve responder.</span><span class="sxs-lookup"><span data-stu-id="5a467-261">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="5a467-262">Por exemplo, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="5a467-262">For example, `http://localhost:123`.</span></span> <span data-ttu-id="5a467-263">Use "\*" para indicar que o servidor deve escutar solicitações em qualquer endereço IP ou nome do host usando o protocolo e porta especificada (por exemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="5a467-263">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="5a467-264">O protocolo (`http://` ou `https://`) devem ser incluídas com cada URL.</span><span class="sxs-lookup"><span data-stu-id="5a467-264">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="5a467-265">Os formatos com suporte variam entre servidores.</span><span class="sxs-lookup"><span data-stu-id="5a467-265">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-266">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-266">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="5a467-267">Kestrel tem sua própria API de configuração do ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="5a467-267">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="5a467-268">Para obter mais informações, consulte [Implementação do servidor Web Kestrel no ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="5a467-268">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-269">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="5a467-270">Tempo limite de desligamento</span><span class="sxs-lookup"><span data-stu-id="5a467-270">Shutdown Timeout</span></span>

<span data-ttu-id="5a467-271">Especifica a quantidade de tempo de espera para o host da web para o desligamento.</span><span class="sxs-lookup"><span data-stu-id="5a467-271">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="5a467-272">**Chave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="5a467-272">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="5a467-273">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="5a467-273">**Type**: *int*</span></span>  
<span data-ttu-id="5a467-274">**Padrão**: 5</span><span class="sxs-lookup"><span data-stu-id="5a467-274">**Default**: 5</span></span>  
<span data-ttu-id="5a467-275">**Definido usando**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="5a467-275">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="5a467-276">**Variável de ambiente**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="5a467-276">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="5a467-277">Embora a chave aceita um *int* com `UseSetting` (por exemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), o `UseShutdownTimeout` método de extensão usa um `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="5a467-277">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="5a467-278">Esse recurso é novo no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="5a467-278">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-279">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-279">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-280">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-280">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5a467-281">Este recurso está indisponível no núcleo do ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="5a467-281">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="5a467-282">Assembly de inicialização</span><span class="sxs-lookup"><span data-stu-id="5a467-282">Startup Assembly</span></span>

<span data-ttu-id="5a467-283">Determina o assembly para pesquisar o `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="5a467-283">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="5a467-284">**Chave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="5a467-284">**Key**: startupAssembly</span></span>  
<span data-ttu-id="5a467-285">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="5a467-285">**Type**: *string*</span></span>  
<span data-ttu-id="5a467-286">**Padrão**: assembly do aplicativo</span><span class="sxs-lookup"><span data-stu-id="5a467-286">**Default**: The app's assembly</span></span>  
<span data-ttu-id="5a467-287">**Definido usando**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="5a467-287">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="5a467-288">**Variável de ambiente**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="5a467-288">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="5a467-289">O assembly por nome (`string`) ou tipo (`TStartup`) pode ser referenciado.</span><span class="sxs-lookup"><span data-stu-id="5a467-289">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="5a467-290">Se vários `UseStartup` métodos são chamados, último terá precedência.</span><span class="sxs-lookup"><span data-stu-id="5a467-290">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-291">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-291">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-292">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-292">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="5a467-293">Raiz da Web</span><span class="sxs-lookup"><span data-stu-id="5a467-293">Web Root</span></span>

<span data-ttu-id="5a467-294">Define o caminho relativo para ativos estático do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a467-294">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="5a467-295">**Chave**: webroot</span><span class="sxs-lookup"><span data-stu-id="5a467-295">**Key**: webroot</span></span>  
<span data-ttu-id="5a467-296">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="5a467-296">**Type**: *string*</span></span>  
<span data-ttu-id="5a467-297">**Padrão**: se não for especificado, o padrão é "(Content Root)/wwwroot", se o caminho existe.</span><span class="sxs-lookup"><span data-stu-id="5a467-297">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="5a467-298">Se o caminho não existir, é usado um provedor de arquivo não-operacional.</span><span class="sxs-lookup"><span data-stu-id="5a467-298">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="5a467-299">**Definido usando**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="5a467-299">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="5a467-300">**Variável de ambiente**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="5a467-300">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-301">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-301">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-302">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-302">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="5a467-303">Configuração de substituição</span><span class="sxs-lookup"><span data-stu-id="5a467-303">Overriding configuration</span></span>

<span data-ttu-id="5a467-304">Use [configuração](xref:fundamentals/configuration/index) para configurar o host.</span><span class="sxs-lookup"><span data-stu-id="5a467-304">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="5a467-305">No exemplo a seguir, a configuração do host é especificada, opcionalmente, em um *hosting.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5a467-305">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="5a467-306">Qualquer configuração carregados a partir de *hosting.json* arquivo pode ser substituído pelos argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="5a467-306">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="5a467-307">A configuração interna (no `config`) é usado para configurar o host com `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="5a467-307">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5a467-309">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="5a467-309">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="5a467-310">Substituindo a configuração fornecida pelo `UseUrls` com *hosting.json* config primeiro argumento de linha de comando config segundo:</span><span class="sxs-lookup"><span data-stu-id="5a467-310">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5a467-312">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="5a467-312">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="5a467-313">Substituindo a configuração fornecida pelo `UseUrls` com *hosting.json* config primeiro argumento de linha de comando config segundo:</span><span class="sxs-lookup"><span data-stu-id="5a467-313">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> <span data-ttu-id="5a467-314">O [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) método de extensão não está atualmente capaz de analisar uma seção de configuração retornada por `GetSection` (por exemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="5a467-314">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="5a467-315">O `GetSection` método filtra as chaves de configuração para a seção solicitada, mas deixa o nome da seção sobre as chaves (por exemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="5a467-315">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="5a467-316">O `UseConfiguration` método espera que as chaves para corresponder a `WebHostBuilder` chaves (por exemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="5a467-316">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="5a467-317">A presença do nome da seção nas chaves impede que os valores da seção Configurando o host.</span><span class="sxs-lookup"><span data-stu-id="5a467-317">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="5a467-318">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="5a467-318">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="5a467-319">Para obter mais informações e soluções alternativas, consulte [passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="5a467-319">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="5a467-320">Para especificar o host executado em uma URL em particular, o valor desejado pode ser passado em um prompt de comando ao executar `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="5a467-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="5a467-321">O argumento de linha de comando substitui o `urls` valor o *hosting.json* arquivo e o servidor escuta na porta 8080:</span><span class="sxs-lookup"><span data-stu-id="5a467-321">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="5a467-322">Iniciando o host</span><span class="sxs-lookup"><span data-stu-id="5a467-322">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a467-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a467-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5a467-324">**Executar**</span><span class="sxs-lookup"><span data-stu-id="5a467-324">**Run**</span></span>

<span data-ttu-id="5a467-325">O `Run` método inicia o aplicativo web e bloqueia o thread de chamada até que o host é desligado:</span><span class="sxs-lookup"><span data-stu-id="5a467-325">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="5a467-326">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="5a467-326">**Start**</span></span>

<span data-ttu-id="5a467-327">Executar o host de maneira sem bloqueio, chamando seu `Start` método:</span><span class="sxs-lookup"><span data-stu-id="5a467-327">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="5a467-328">Se uma lista de URLs é passada para o `Start` método, ele escuta em URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="5a467-328">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="5a467-329">O aplicativo pode inicializar e iniciar um novo host usando os padrões pré-configurado de `CreateDefaultBuilder` usando um método estático de conveniência.</span><span class="sxs-lookup"><span data-stu-id="5a467-329">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="5a467-330">Esses métodos de iniciar o servidor sem saída de console e [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) aguardar uma quebra (Ctrl-C/SIGINT ou SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="5a467-330">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="5a467-331">**Início (RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="5a467-331">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="5a467-332">Iniciar com um `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="5a467-332">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="5a467-333">Fazer uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="5a467-333">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="5a467-334">`WaitForShutdown`blocos até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="5a467-334">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="5a467-335">O aplicativo é exibido o `Console.WriteLine` mensagem e aguarda um pressionamento de tecla sair.</span><span class="sxs-lookup"><span data-stu-id="5a467-335">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="5a467-336">**Iniciar (cadeia de caracteres de url, RequestDelegate do aplicativo)**</span><span class="sxs-lookup"><span data-stu-id="5a467-336">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="5a467-337">Iniciar com uma URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="5a467-337">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="5a467-338">Produz o mesmo resultado que **inicial (aplicativo RequestDelegate)**, exceto o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="5a467-338">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="5a467-339">**Iniciar (ação<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="5a467-339">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="5a467-340">Usar uma instância de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar o middleware de roteamento:</span><span class="sxs-lookup"><span data-stu-id="5a467-340">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="5a467-341">Use as seguintes solicitações de navegador com o exemplo:</span><span class="sxs-lookup"><span data-stu-id="5a467-341">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="5a467-342">Solicitação</span><span class="sxs-lookup"><span data-stu-id="5a467-342">Request</span></span>                                    | <span data-ttu-id="5a467-343">Resposta</span><span class="sxs-lookup"><span data-stu-id="5a467-343">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="5a467-344">Olá, Martin!</span><span class="sxs-lookup"><span data-stu-id="5a467-344">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="5a467-345">Dias de Buenos, Catrina!</span><span class="sxs-lookup"><span data-stu-id="5a467-345">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="5a467-346">Gera uma exceção com a cadeia de caracteres "ooops!"</span><span class="sxs-lookup"><span data-stu-id="5a467-346">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="5a467-347">Gera uma exceção com a cadeia de caracteres "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="5a467-347">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="5a467-348">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="5a467-348">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="5a467-349">Olá, mundo!</span><span class="sxs-lookup"><span data-stu-id="5a467-349">Hello World!</span></span>                             |

<span data-ttu-id="5a467-350">`WaitForShutdown`blocos até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="5a467-350">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="5a467-351">O aplicativo é exibido o `Console.WriteLine` mensagem e aguarda um pressionamento de tecla sair.</span><span class="sxs-lookup"><span data-stu-id="5a467-351">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="5a467-352">**Iniciar (url, a ação de cadeia de caracteres<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="5a467-352">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="5a467-353">Use uma URL e uma instância do `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="5a467-353">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="5a467-354">Produz o mesmo resultado que **iniciar (ação<IRouteBuilder> routeBuilder)**, exceto o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="5a467-354">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="5a467-355">**StartWith (ação<IApplicationBuilder> aplicativo)**</span><span class="sxs-lookup"><span data-stu-id="5a467-355">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="5a467-356">Forneça um delegado para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="5a467-356">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="5a467-357">Fazer uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="5a467-357">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="5a467-358">`WaitForShutdown`blocos até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="5a467-358">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="5a467-359">O aplicativo é exibido o `Console.WriteLine` mensagem e aguarda um pressionamento de tecla sair.</span><span class="sxs-lookup"><span data-stu-id="5a467-359">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="5a467-360">**StartWith (url, a ação de cadeia de caracteres<IApplicationBuilder> aplicativo)**</span><span class="sxs-lookup"><span data-stu-id="5a467-360">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="5a467-361">Forneça uma URL e um delegado para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="5a467-361">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="5a467-362">Produz o mesmo resultado que **StartWith (ação<IApplicationBuilder> aplicativo)**, exceto o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="5a467-362">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a467-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a467-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5a467-364">**Executar**</span><span class="sxs-lookup"><span data-stu-id="5a467-364">**Run**</span></span>

<span data-ttu-id="5a467-365">O `Run` método inicia o aplicativo web e bloqueia o thread de chamada até que o host for desligado:</span><span class="sxs-lookup"><span data-stu-id="5a467-365">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="5a467-366">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="5a467-366">**Start**</span></span>

<span data-ttu-id="5a467-367">Executar o host de maneira sem bloqueio, chamando seu `Start` método:</span><span class="sxs-lookup"><span data-stu-id="5a467-367">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="5a467-368">Se uma lista de URLs é passada para o `Start` método, ele escuta em URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="5a467-368">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="5a467-369">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="5a467-369">IHostingEnvironment interface</span></span>

<span data-ttu-id="5a467-370">O [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornece informações sobre o ambiente de hospedagem na web do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a467-370">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="5a467-371">Use [injeção de construtor](xref:fundamentals/dependency-injection) para obter o `IHostingEnvironment` para usar suas propriedades e métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="5a467-371">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="5a467-372">Um [abordagem baseado em convenção](xref:fundamentals/environments#startup-conventions) pode ser usado para configurar o aplicativo na inicialização com base no ambiente.</span><span class="sxs-lookup"><span data-stu-id="5a467-372">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="5a467-373">Como alternativa, injetar o `IHostingEnvironment` para o `Startup` construtor para uso em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5a467-373">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="5a467-374">Além de `IsDevelopment` método de extensão, `IHostingEnvironment` oferece `IsStaging`, `IsProduction`, e `IsEnvironment(string environmentName)` métodos.</span><span class="sxs-lookup"><span data-stu-id="5a467-374">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="5a467-375">Consulte [trabalhando com vários ambientes](xref:fundamentals/environments) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="5a467-375">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="5a467-376">O `IHostingEnvironment` serviço também pode ser inserido diretamente para o `Configure` método para configurar o pipeline de processamento:</span><span class="sxs-lookup"><span data-stu-id="5a467-376">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="5a467-377">`IHostingEnvironment`pode ser inserida no `Invoke` método ao criar personalizado [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="5a467-377">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="5a467-378">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="5a467-378">IApplicationLifetime interface</span></span>

<span data-ttu-id="5a467-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite para atividades de pós-inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="5a467-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="5a467-380">Três propriedades na interface são tokens de cancelamento usados para registrar `Action` métodos que definem os eventos de inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="5a467-380">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="5a467-381">Também há um `StopApplication` método.</span><span class="sxs-lookup"><span data-stu-id="5a467-381">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="5a467-382">Token de cancelamento</span><span class="sxs-lookup"><span data-stu-id="5a467-382">Cancellation Token</span></span>    | <span data-ttu-id="5a467-383">Acionado quando &#8230;</span><span class="sxs-lookup"><span data-stu-id="5a467-383">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="5a467-384">O host foi totalmente iniciado.</span><span class="sxs-lookup"><span data-stu-id="5a467-384">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="5a467-385">O host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="5a467-385">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="5a467-386">Ainda podem estar processando solicitações.</span><span class="sxs-lookup"><span data-stu-id="5a467-386">Requests may still be processing.</span></span> <span data-ttu-id="5a467-387">Blocos de desligamento até que esse evento seja concluída.</span><span class="sxs-lookup"><span data-stu-id="5a467-387">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="5a467-388">O host está concluindo um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="5a467-388">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="5a467-389">Todas as solicitações devem ser processadas.</span><span class="sxs-lookup"><span data-stu-id="5a467-389">All requests should be processed.</span></span> <span data-ttu-id="5a467-390">Blocos de desligamento até que esse evento seja concluída.</span><span class="sxs-lookup"><span data-stu-id="5a467-390">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="5a467-391">Método</span><span class="sxs-lookup"><span data-stu-id="5a467-391">Method</span></span>            | <span data-ttu-id="5a467-392">Ação</span><span class="sxs-lookup"><span data-stu-id="5a467-392">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="5a467-393">Encerramento de solicitações do aplicativo atual.</span><span class="sxs-lookup"><span data-stu-id="5a467-393">Requests termination of the current application.</span></span> |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="5a467-394">Solucionando problemas de System. ArgumentException</span><span class="sxs-lookup"><span data-stu-id="5a467-394">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="5a467-395">**Aplica-se ao ASP.NET Core 2.0 apenas**</span><span class="sxs-lookup"><span data-stu-id="5a467-395">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="5a467-396">Se o host for construído injetando `IStartup` diretamente para o contêiner de injeção de dependência em vez de chamar `UseStartup` ou `Configure`, pode ocorrer o seguinte erro: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="5a467-396">If the host is built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, the following error may occur: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="5a467-397">Isso ocorre porque o [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (do assembly atual) é necessário para verificar se há `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="5a467-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="5a467-398">Se o aplicativo manualmente injeta `IStartup` para o contêiner de injeção de dependência, adicione a seguinte chamada a `WebHostBuilder` com o nome do assembly especificado:</span><span class="sxs-lookup"><span data-stu-id="5a467-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="5a467-399">Como alternativa, adicione uma cópia `Configure` para o `WebHostBuilder`, que define o `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="5a467-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="5a467-400">**Observação**: isso só é necessária com a versão 2.0 do ASP.NET Core e somente quando o aplicativo não chama `UseStartup` ou `Configure`.</span><span class="sxs-lookup"><span data-stu-id="5a467-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="5a467-401">Para obter mais informações, consulte [anúncios: Microsoft.Extensions.PlatformAbstractions foi removido (comentário)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e [StartupInjection exemplo](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="5a467-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5a467-402">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="5a467-402">Additional resources</span></span>

* [<span data-ttu-id="5a467-403">Publicar no Windows usando o IIS</span><span class="sxs-lookup"><span data-stu-id="5a467-403">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="5a467-404">Publicar no Linux usando Nginx</span><span class="sxs-lookup"><span data-stu-id="5a467-404">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="5a467-405">Publicar no Linux usando o Apache</span><span class="sxs-lookup"><span data-stu-id="5a467-405">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="5a467-406">Host em um serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="5a467-406">Host in a Windows Service</span></span>](xref:hosting/windows-service)
