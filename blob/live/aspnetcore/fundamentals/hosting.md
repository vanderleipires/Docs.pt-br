---
title: "Hospedagem no núcleo do ASP.NET"
author: guardrex
description: "Saiba mais sobre o host da web em ASP.NET Core, que é responsável pelo gerenciamento de inicialização e o tempo de vida do aplicativo."
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 7f6712073002b73ca4ddd7586718c81e62cacbc2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="4e44d-103">Hospedagem no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4e44d-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="4e44d-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4e44d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4e44d-105">Aplicativos ASP.NET Core configurar e iniciar um *host*.</span><span class="sxs-lookup"><span data-stu-id="4e44d-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="4e44d-106">O host é responsável pelo gerenciamento de inicialização e o tempo de vida do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="4e44d-107">No mínimo, o host configura um servidor e um pipeline de processamento de solicitação.</span><span class="sxs-lookup"><span data-stu-id="4e44d-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="4e44d-108">Configurando um host</span><span class="sxs-lookup"><span data-stu-id="4e44d-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4e44d-110">Criar um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4e44d-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="4e44d-111">Isso normalmente é executado no ponto de entrada do aplicativo, o `Main` método.</span><span class="sxs-lookup"><span data-stu-id="4e44d-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="4e44d-112">Nos modelos de projeto, `Main` está localizado em *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="4e44d-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="4e44d-113">Um típico *Program.cs* chamadas [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para começar a configurar um host:</span><span class="sxs-lookup"><span data-stu-id="4e44d-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="4e44d-114">`CreateDefaultBuilder`executa as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="4e44d-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="4e44d-115">Configura [Kestrel](servers/kestrel.md) como o servidor web.</span><span class="sxs-lookup"><span data-stu-id="4e44d-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="4e44d-116">Para as opções padrão Kestrel, consulte [o Kestrel opções de seção de implementação do servidor web Kestrel no ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="4e44d-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="4e44d-117">Define a raiz de conteúdo para o caminho retornado por [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="4e44d-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="4e44d-118">Configuração opcional de cargas de:</span><span class="sxs-lookup"><span data-stu-id="4e44d-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="4e44d-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="4e44d-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="4e44d-120">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="4e44d-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="4e44d-121">[Segredos do usuário](xref:security/app-secrets) quando o aplicativo é executado no `Development` ambiente.</span><span class="sxs-lookup"><span data-stu-id="4e44d-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="4e44d-122">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="4e44d-122">Environment variables.</span></span>
  * <span data-ttu-id="4e44d-123">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="4e44d-123">Command-line arguments.</span></span>
* <span data-ttu-id="4e44d-124">Configura [log](xref:fundamentals/logging/index) para a saída do console e de depuração.</span><span class="sxs-lookup"><span data-stu-id="4e44d-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="4e44d-125">O log inclui [filtragem de log](xref:fundamentals/logging/index#log-filtering) regras especificadas em uma seção de configuração de log de um *appSettings. JSON* ou *appsettings. { . JSON de ambiente}* arquivo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="4e44d-126">Quando em execução por trás do IIS, permite [integração IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="4e44d-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="4e44d-127">Configura o caminho base e a porta que o servidor escuta ao usar o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="4e44d-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="4e44d-128">O módulo cria um proxy reverso entre o IIS e Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4e44d-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="4e44d-129">Também configura o aplicativo [capturar erros de inicialização](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="4e44d-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="4e44d-130">Para as opções de padrão do IIS, consulte [o IIS opções seção de Host ASP.NET Core no Windows com o IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="4e44d-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="4e44d-131">O *conteúdo raiz* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="4e44d-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="4e44d-132">Quando o aplicativo é iniciado na pasta raiz do projeto, a pasta raiz do projeto é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="4e44d-133">Esse é o padrão usado em [Visual Studio](https://www.visualstudio.com/) e [dotnet novos modelos](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="4e44d-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="4e44d-134">Para obter mais informações sobre a configuração do aplicativo, consulte [configuração no ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="4e44d-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="4e44d-135">Como uma alternativa ao uso estático `CreateDefaultBuilder` método, criando um host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) é uma abordagem com suporte com o ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="4e44d-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="4e44d-136">Para obter mais informações, consulte a guia de 1. x do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4e44d-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e44d-138">Criar um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4e44d-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="4e44d-139">Criando um host normalmente é executada no ponto de entrada do aplicativo, o `Main` método.</span><span class="sxs-lookup"><span data-stu-id="4e44d-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="4e44d-140">Nos modelos de projeto, `Main` está localizado em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="4e44d-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="4e44d-141">`WebHostBuilder`requer um [server que implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="4e44d-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="4e44d-142">Os servidores internos são [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (antes da versão do ASP.NET Core 2.0, o HTTP.sys foi chamado [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="4e44d-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="4e44d-143">Neste exemplo, o [o método de extensão UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Especifica o servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4e44d-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="4e44d-144">O *conteúdo raiz* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="4e44d-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="4e44d-145">A raiz de conteúdo padrão é obtida para `UseContentRoot` por [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="4e44d-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="4e44d-146">Quando o aplicativo é iniciado na pasta raiz do projeto, a pasta raiz do projeto é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="4e44d-147">Esse é o padrão usado em [Visual Studio](https://www.visualstudio.com/) e [dotnet novos modelos](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="4e44d-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="4e44d-148">Para usar o IIS como um proxy reverso, chame [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte da construção do host.</span><span class="sxs-lookup"><span data-stu-id="4e44d-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="4e44d-149">`UseIISIntegration`não configure um *servidor*, como [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="4e44d-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="4e44d-150">`UseIISIntegration`configura o caminho base e a porta que o servidor escuta ao usar o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) para criar um proxy reverso entre Kestrel e o IIS.</span><span class="sxs-lookup"><span data-stu-id="4e44d-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="4e44d-151">Para usar o IIS com o ASP.NET Core `UseKestrel` e `UseIISIntegration` devem ser especificados.</span><span class="sxs-lookup"><span data-stu-id="4e44d-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="4e44d-152">`UseIISIntegration`ativa somente quando em execução por trás do IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4e44d-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="4e44d-153">Para obter mais informações, consulte [Introdução ao ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) e [referência de configuração do módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="4e44d-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="4e44d-154">Uma implementação mínima que configura um host (e um aplicativo ASP.NET Core) inclui a especificação de um servidor e a configuração do canal de solicitação do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="4e44d-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="4e44d-155">Ao configurar um host, [configurar](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) métodos podem ser fornecidos.</span><span class="sxs-lookup"><span data-stu-id="4e44d-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="4e44d-156">Se um `Startup` classe for especificada, ele deve definir um `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="4e44d-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="4e44d-157">Para obter mais informações, consulte [inicialização do aplicativo no ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="4e44d-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="4e44d-158">Diversas chamadas para `ConfigureServices` acrescentar um ao outro.</span><span class="sxs-lookup"><span data-stu-id="4e44d-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="4e44d-159">Diversas chamadas para `Configure` ou `UseStartup` no `WebHostBuilder` substituir as configurações anteriores.</span><span class="sxs-lookup"><span data-stu-id="4e44d-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="4e44d-160">Valores de configuração do host</span><span class="sxs-lookup"><span data-stu-id="4e44d-160">Host configuration values</span></span>

<span data-ttu-id="4e44d-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) conta com as seguintes abordagens para definir valores de configuração de host:</span><span class="sxs-lookup"><span data-stu-id="4e44d-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="4e44d-162">Configuração de construtor do host, que inclui variáveis de ambiente com o formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="4e44d-163">Por exemplo, `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="4e44d-164">Métodos explícitos, como `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="4e44d-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e a chave associada.</span><span class="sxs-lookup"><span data-stu-id="4e44d-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="4e44d-166">Ao definir um valor com `UseSetting`, o valor é definido como uma cadeia de caracteres, independentemente do tipo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="4e44d-167">O host usa qualquer opção define um valor de última.</span><span class="sxs-lookup"><span data-stu-id="4e44d-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="4e44d-168">Para obter mais informações, consulte [substituindo configuração](#overriding-configuration) na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="4e44d-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="4e44d-169">Capturar erros de inicialização</span><span class="sxs-lookup"><span data-stu-id="4e44d-169">Capture Startup Errors</span></span>

<span data-ttu-id="4e44d-170">Essa configuração controla a captura de erros de inicialização.</span><span class="sxs-lookup"><span data-stu-id="4e44d-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="4e44d-171">**Chave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="4e44d-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="4e44d-172">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="4e44d-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4e44d-173">**Padrão**: assume como padrão `false` , a menos que o aplicativo é executado com Kestrel por trás do IIS, em que o padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="4e44d-174">**Definido usando**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="4e44d-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="4e44d-175">**Variável de ambiente**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="4e44d-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="4e44d-176">Quando `false`, erros durante o resultado de inicialização no host sair.</span><span class="sxs-lookup"><span data-stu-id="4e44d-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="4e44d-177">Quando `true`, o host captura exceções durante a inicialização e tenta iniciar o servidor.</span><span class="sxs-lookup"><span data-stu-id="4e44d-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="4e44d-180">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="4e44d-180">Content Root</span></span>

<span data-ttu-id="4e44d-181">Essa configuração determina onde o ASP.NET Core começa a procurar por arquivos de conteúdo, como exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="4e44d-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="4e44d-182">**Chave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="4e44d-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="4e44d-183">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="4e44d-183">**Type**: *string*</span></span>  
<span data-ttu-id="4e44d-184">**Padrão**: padrão é a pasta em que reside o assembly de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="4e44d-185">**Definido usando**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="4e44d-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="4e44d-186">**Variável de ambiente**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="4e44d-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="4e44d-187">A conteúdo raiz também é usada como o caminho base para o [configuração Web raiz](#web-root).</span><span class="sxs-lookup"><span data-stu-id="4e44d-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="4e44d-188">Se o caminho não existir, o host não será iniciado.</span><span class="sxs-lookup"><span data-stu-id="4e44d-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="4e44d-191">Erros detalhados</span><span class="sxs-lookup"><span data-stu-id="4e44d-191">Detailed Errors</span></span>

<span data-ttu-id="4e44d-192">Determina se detalhado erros devem ser capturados.</span><span class="sxs-lookup"><span data-stu-id="4e44d-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="4e44d-193">**Chave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="4e44d-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="4e44d-194">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="4e44d-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4e44d-195">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="4e44d-195">**Default**: false</span></span>  
<span data-ttu-id="4e44d-196">**Definido usando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4e44d-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="4e44d-197">**Variável de ambiente**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="4e44d-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="4e44d-198">Quando habilitado (ou quando o <a href="#environment">ambiente</a> é definido como `Development`), o aplicativo captura exceções detalhadas.</span><span class="sxs-lookup"><span data-stu-id="4e44d-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="4e44d-201">Ambiente</span><span class="sxs-lookup"><span data-stu-id="4e44d-201">Environment</span></span>

<span data-ttu-id="4e44d-202">Define o ambiente do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-202">Sets the app's environment.</span></span>

<span data-ttu-id="4e44d-203">**Chave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="4e44d-203">**Key**: environment</span></span>  
<span data-ttu-id="4e44d-204">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="4e44d-204">**Type**: *string*</span></span>  
<span data-ttu-id="4e44d-205">**Padrão**: produção</span><span class="sxs-lookup"><span data-stu-id="4e44d-205">**Default**: Production</span></span>  
<span data-ttu-id="4e44d-206">**Definido usando**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="4e44d-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="4e44d-207">**Variável de ambiente**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="4e44d-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="4e44d-208">O ambiente pode ser definido como qualquer valor.</span><span class="sxs-lookup"><span data-stu-id="4e44d-208">The environment can be set to any value.</span></span> <span data-ttu-id="4e44d-209">Incluem valores definidos pelo Framework `Development`, `Staging`, e `Production`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="4e44d-210">Valores não diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="4e44d-210">Values aren't case sensitive.</span></span> <span data-ttu-id="4e44d-211">Por padrão, o *ambiente* são lidos a partir de `ASPNETCORE_ENVIRONMENT` variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="4e44d-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="4e44d-212">Ao usar [Visual Studio](https://www.visualstudio.com/), variáveis de ambiente podem ser definidas no *launchSettings.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="4e44d-213">Para obter mais informações, consulte [Trabalhando com vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="4e44d-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="4e44d-216">Assemblies de inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="4e44d-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="4e44d-217">Define a hospedagem assemblies de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="4e44d-218">**Chave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="4e44d-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="4e44d-219">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="4e44d-219">**Type**: *string*</span></span>  
<span data-ttu-id="4e44d-220">**Padrão**: cadeia de caracteres vazia</span><span class="sxs-lookup"><span data-stu-id="4e44d-220">**Default**: Empty string</span></span>  
<span data-ttu-id="4e44d-221">**Definido usando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4e44d-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="4e44d-222">**Variável de ambiente**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="4e44d-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="4e44d-223">Uma cadeia delimitada por ponto e vírgula de hospedagem assemblies de inicialização para carregar na inicialização.</span><span class="sxs-lookup"><span data-stu-id="4e44d-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="4e44d-224">Esse recurso é novo no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="4e44d-225">Embora o valor de configuração padrão é uma cadeia de caracteres vazia, os assemblies de inicialização hospedagem sempre incluem o assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="4e44d-226">Quando os assemblies de inicialização de hospedagem são fornecidos, elas são adicionadas ao assembly do aplicativo de carregamento quando o aplicativo cria seus serviços comuns durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="4e44d-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e44d-229">Este recurso está indisponível no núcleo do ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="4e44d-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="4e44d-230">Preferir URLs de hospedagem</span><span class="sxs-lookup"><span data-stu-id="4e44d-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="4e44d-231">Indica se o host deve escutar nas URLs configuradas com o `WebHostBuilder` em vez das configuradas com o `IServer` implementação.</span><span class="sxs-lookup"><span data-stu-id="4e44d-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="4e44d-232">**Key**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="4e44d-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="4e44d-233">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="4e44d-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4e44d-234">**Padrão**: true</span><span class="sxs-lookup"><span data-stu-id="4e44d-234">**Default**: true</span></span>  
<span data-ttu-id="4e44d-235">**Definido usando**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="4e44d-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="4e44d-236">**Variável de ambiente**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="4e44d-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="4e44d-237">Esse recurso é novo no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e44d-240">Este recurso está indisponível no núcleo do ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="4e44d-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="4e44d-241">Impedir a inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="4e44d-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="4e44d-242">Impede o carregamento automático de hospedagem assemblies de inicialização, incluindo os assemblies de inicialização configurados pelo assembly do aplicativo de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="4e44d-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="4e44d-243">Consulte [adicionar recursos de aplicativo de um assembly externo usando IHostingStartup](xref:host-and-deploy/ihostingstartup) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="4e44d-243">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="4e44d-244">**Key**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="4e44d-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="4e44d-245">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="4e44d-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4e44d-246">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="4e44d-246">**Default**: false</span></span>  
<span data-ttu-id="4e44d-247">**Definido usando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4e44d-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="4e44d-248">**Variável de ambiente**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="4e44d-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="4e44d-249">Esse recurso é novo no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e44d-252">Este recurso está indisponível no núcleo do ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="4e44d-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="4e44d-253">URLs de servidor</span><span class="sxs-lookup"><span data-stu-id="4e44d-253">Server URLs</span></span>

<span data-ttu-id="4e44d-254">Indica os endereços IP ou endereços de host com as portas e protocolos que o servidor deve ouvir para solicitações.</span><span class="sxs-lookup"><span data-stu-id="4e44d-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="4e44d-255">**Chave**: urls</span><span class="sxs-lookup"><span data-stu-id="4e44d-255">**Key**: urls</span></span>  
<span data-ttu-id="4e44d-256">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="4e44d-256">**Type**: *string*</span></span>  
<span data-ttu-id="4e44d-257">**Default**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="4e44d-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="4e44d-258">**Definido usando**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="4e44d-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="4e44d-259">**Variável de ambiente**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="4e44d-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="4e44d-260">Definir um separados por ponto e vírgula (;) prefixos de lista de URL para o servidor deve responder.</span><span class="sxs-lookup"><span data-stu-id="4e44d-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="4e44d-261">Por exemplo, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="4e44d-262">Use "\*" para indicar que o servidor deve escutar solicitações em qualquer endereço IP ou nome do host usando o protocolo e porta especificada (por exemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="4e44d-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="4e44d-263">O protocolo (`http://` ou `https://`) devem ser incluídas com cada URL.</span><span class="sxs-lookup"><span data-stu-id="4e44d-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="4e44d-264">Os formatos com suporte variam entre servidores.</span><span class="sxs-lookup"><span data-stu-id="4e44d-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="4e44d-266">Kestrel tem sua própria API de configuração do ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="4e44d-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="4e44d-267">Para obter mais informações, consulte [Implementação do servidor Web Kestrel no ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="4e44d-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="4e44d-269">Tempo limite de desligamento</span><span class="sxs-lookup"><span data-stu-id="4e44d-269">Shutdown Timeout</span></span>

<span data-ttu-id="4e44d-270">Especifica a quantidade de tempo de espera para o host da web para o desligamento.</span><span class="sxs-lookup"><span data-stu-id="4e44d-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="4e44d-271">**Chave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="4e44d-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="4e44d-272">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="4e44d-272">**Type**: *int*</span></span>  
<span data-ttu-id="4e44d-273">**Padrão**: 5</span><span class="sxs-lookup"><span data-stu-id="4e44d-273">**Default**: 5</span></span>  
<span data-ttu-id="4e44d-274">**Definido usando**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="4e44d-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="4e44d-275">**Variável de ambiente**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="4e44d-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="4e44d-276">Embora a chave aceita um *int* com `UseSetting` (por exemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), o `UseShutdownTimeout` método de extensão usa um `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="4e44d-277">Esse recurso é novo no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e44d-280">Este recurso está indisponível no núcleo do ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="4e44d-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="4e44d-281">Assembly de inicialização</span><span class="sxs-lookup"><span data-stu-id="4e44d-281">Startup Assembly</span></span>

<span data-ttu-id="4e44d-282">Determina o assembly para pesquisar o `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="4e44d-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="4e44d-283">**Chave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="4e44d-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="4e44d-284">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="4e44d-284">**Type**: *string*</span></span>  
<span data-ttu-id="4e44d-285">**Padrão**: assembly do aplicativo</span><span class="sxs-lookup"><span data-stu-id="4e44d-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="4e44d-286">**Definido usando**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="4e44d-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="4e44d-287">**Variável de ambiente**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="4e44d-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="4e44d-288">O assembly por nome (`string`) ou tipo (`TStartup`) pode ser referenciado.</span><span class="sxs-lookup"><span data-stu-id="4e44d-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="4e44d-289">Se vários `UseStartup` métodos são chamados, último terá precedência.</span><span class="sxs-lookup"><span data-stu-id="4e44d-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="4e44d-292">Raiz da Web</span><span class="sxs-lookup"><span data-stu-id="4e44d-292">Web Root</span></span>

<span data-ttu-id="4e44d-293">Define o caminho relativo para ativos estático do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="4e44d-294">**Chave**: webroot</span><span class="sxs-lookup"><span data-stu-id="4e44d-294">**Key**: webroot</span></span>  
<span data-ttu-id="4e44d-295">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="4e44d-295">**Type**: *string*</span></span>  
<span data-ttu-id="4e44d-296">**Padrão**: se não for especificado, o padrão é "(Content Root)/wwwroot", se o caminho existe.</span><span class="sxs-lookup"><span data-stu-id="4e44d-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="4e44d-297">Se o caminho não existir, é usado um provedor de arquivo não-operacional.</span><span class="sxs-lookup"><span data-stu-id="4e44d-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="4e44d-298">**Definido usando**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="4e44d-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="4e44d-299">**Variável de ambiente**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="4e44d-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="4e44d-302">Configuração de substituição</span><span class="sxs-lookup"><span data-stu-id="4e44d-302">Overriding configuration</span></span>

<span data-ttu-id="4e44d-303">Use [configuração](xref:fundamentals/configuration/index) para configurar o host.</span><span class="sxs-lookup"><span data-stu-id="4e44d-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="4e44d-304">No exemplo a seguir, a configuração do host é especificada, opcionalmente, em um *hosting.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="4e44d-305">Qualquer configuração carregados a partir de *hosting.json* arquivo pode ser substituído pelos argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="4e44d-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="4e44d-306">A configuração interna (no `config`) é usado para configurar o host com `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4e44d-308">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="4e44d-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="4e44d-309">Substituindo a configuração fornecida pelo `UseUrls` com *hosting.json* config primeiro argumento de linha de comando config segundo:</span><span class="sxs-lookup"><span data-stu-id="4e44d-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e44d-311">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="4e44d-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="4e44d-312">Substituindo a configuração fornecida pelo `UseUrls` com *hosting.json* config primeiro argumento de linha de comando config segundo:</span><span class="sxs-lookup"><span data-stu-id="4e44d-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="4e44d-313">O [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) método de extensão não está atualmente capaz de analisar uma seção de configuração retornada por `GetSection` (por exemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="4e44d-314">O `GetSection` método filtra as chaves de configuração para a seção solicitada, mas deixa o nome da seção sobre as chaves (por exemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="4e44d-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="4e44d-315">O `UseConfiguration` método espera que as chaves para corresponder a `WebHostBuilder` chaves (por exemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="4e44d-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="4e44d-316">A presença do nome da seção nas chaves impede que os valores da seção Configurando o host.</span><span class="sxs-lookup"><span data-stu-id="4e44d-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="4e44d-317">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="4e44d-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="4e44d-318">Para obter mais informações e soluções alternativas, consulte [passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="4e44d-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="4e44d-319">Para especificar o host executado em uma URL em particular, o valor desejado pode ser passado em um prompt de comando ao executar `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="4e44d-320">O argumento de linha de comando substitui o `urls` valor o *hosting.json* arquivo e o servidor escuta na porta 8080:</span><span class="sxs-lookup"><span data-stu-id="4e44d-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="4e44d-321">Iniciando o host</span><span class="sxs-lookup"><span data-stu-id="4e44d-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4e44d-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4e44d-323">**Executar**</span><span class="sxs-lookup"><span data-stu-id="4e44d-323">**Run**</span></span>

<span data-ttu-id="4e44d-324">O `Run` método inicia o aplicativo web e bloqueia o thread de chamada até que o host é desligado:</span><span class="sxs-lookup"><span data-stu-id="4e44d-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="4e44d-325">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="4e44d-325">**Start**</span></span>

<span data-ttu-id="4e44d-326">Executar o host de maneira sem bloqueio, chamando seu `Start` método:</span><span class="sxs-lookup"><span data-stu-id="4e44d-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="4e44d-327">Se uma lista de URLs é passada para o `Start` método, ele escuta em URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="4e44d-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="4e44d-328">O aplicativo pode inicializar e iniciar um novo host usando os padrões pré-configurado de `CreateDefaultBuilder` usando um método estático de conveniência.</span><span class="sxs-lookup"><span data-stu-id="4e44d-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="4e44d-329">Esses métodos de iniciar o servidor sem saída de console e [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) aguardar uma quebra (Ctrl-C/SIGINT ou SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="4e44d-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="4e44d-330">**Início (RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="4e44d-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="4e44d-331">Iniciar com um `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="4e44d-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="4e44d-332">Fazer uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="4e44d-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="4e44d-333">`WaitForShutdown`blocos até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="4e44d-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="4e44d-334">O aplicativo é exibido o `Console.WriteLine` mensagem e aguarda um pressionamento de tecla sair.</span><span class="sxs-lookup"><span data-stu-id="4e44d-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="4e44d-335">**Iniciar (cadeia de caracteres de url, RequestDelegate do aplicativo)**</span><span class="sxs-lookup"><span data-stu-id="4e44d-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="4e44d-336">Iniciar com uma URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="4e44d-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="4e44d-337">Produz o mesmo resultado que **inicial (aplicativo RequestDelegate)**, exceto o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="4e44d-338">**Iniciar (ação<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="4e44d-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="4e44d-339">Usar uma instância de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar o middleware de roteamento:</span><span class="sxs-lookup"><span data-stu-id="4e44d-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="4e44d-340">Use as seguintes solicitações de navegador com o exemplo:</span><span class="sxs-lookup"><span data-stu-id="4e44d-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="4e44d-341">Solicitação</span><span class="sxs-lookup"><span data-stu-id="4e44d-341">Request</span></span>                                    | <span data-ttu-id="4e44d-342">Resposta</span><span class="sxs-lookup"><span data-stu-id="4e44d-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="4e44d-343">Olá, Martin!</span><span class="sxs-lookup"><span data-stu-id="4e44d-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="4e44d-344">Dias de Buenos, Catrina!</span><span class="sxs-lookup"><span data-stu-id="4e44d-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="4e44d-345">Gera uma exceção com a cadeia de caracteres "ooops!"</span><span class="sxs-lookup"><span data-stu-id="4e44d-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="4e44d-346">Gera uma exceção com a cadeia de caracteres "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="4e44d-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="4e44d-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="4e44d-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="4e44d-348">Olá, mundo!</span><span class="sxs-lookup"><span data-stu-id="4e44d-348">Hello World!</span></span>                             |

<span data-ttu-id="4e44d-349">`WaitForShutdown`blocos até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="4e44d-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="4e44d-350">O aplicativo é exibido o `Console.WriteLine` mensagem e aguarda um pressionamento de tecla sair.</span><span class="sxs-lookup"><span data-stu-id="4e44d-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="4e44d-351">**Iniciar (url, a ação de cadeia de caracteres<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="4e44d-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="4e44d-352">Use uma URL e uma instância do `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4e44d-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="4e44d-353">Produz o mesmo resultado que **iniciar (ação<IRouteBuilder> routeBuilder)**, exceto o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="4e44d-354">**StartWith (ação<IApplicationBuilder> aplicativo)**</span><span class="sxs-lookup"><span data-stu-id="4e44d-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="4e44d-355">Forneça um delegado para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4e44d-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="4e44d-356">Fazer uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="4e44d-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="4e44d-357">`WaitForShutdown`blocos até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="4e44d-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="4e44d-358">O aplicativo é exibido o `Console.WriteLine` mensagem e aguarda um pressionamento de tecla sair.</span><span class="sxs-lookup"><span data-stu-id="4e44d-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="4e44d-359">**StartWith (url, a ação de cadeia de caracteres<IApplicationBuilder> aplicativo)**</span><span class="sxs-lookup"><span data-stu-id="4e44d-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="4e44d-360">Forneça uma URL e um delegado para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4e44d-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="4e44d-361">Produz o mesmo resultado que **StartWith (ação<IApplicationBuilder> aplicativo)**, exceto o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4e44d-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4e44d-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4e44d-363">**Executar**</span><span class="sxs-lookup"><span data-stu-id="4e44d-363">**Run**</span></span>

<span data-ttu-id="4e44d-364">O `Run` método inicia o aplicativo web e bloqueia o thread de chamada até que o host for desligado:</span><span class="sxs-lookup"><span data-stu-id="4e44d-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="4e44d-365">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="4e44d-365">**Start**</span></span>

<span data-ttu-id="4e44d-366">Executar o host de maneira sem bloqueio, chamando seu `Start` método:</span><span class="sxs-lookup"><span data-stu-id="4e44d-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="4e44d-367">Se uma lista de URLs é passada para o `Start` método, ele escuta em URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="4e44d-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="4e44d-368">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="4e44d-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="4e44d-369">O [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornece informações sobre o ambiente de hospedagem na web do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4e44d-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="4e44d-370">Use [injeção de construtor](xref:fundamentals/dependency-injection) para obter o `IHostingEnvironment` para usar suas propriedades e métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="4e44d-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="4e44d-371">Um [abordagem baseado em convenção](xref:fundamentals/environments#startup-conventions) pode ser usado para configurar o aplicativo na inicialização com base no ambiente.</span><span class="sxs-lookup"><span data-stu-id="4e44d-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="4e44d-372">Como alternativa, injetar o `IHostingEnvironment` para o `Startup` construtor para uso em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4e44d-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="4e44d-373">Além de `IsDevelopment` método de extensão, `IHostingEnvironment` oferece `IsStaging`, `IsProduction`, e `IsEnvironment(string environmentName)` métodos.</span><span class="sxs-lookup"><span data-stu-id="4e44d-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="4e44d-374">Consulte [trabalhando com vários ambientes](xref:fundamentals/environments) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="4e44d-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="4e44d-375">O `IHostingEnvironment` serviço também pode ser inserido diretamente para o `Configure` método para configurar o pipeline de processamento:</span><span class="sxs-lookup"><span data-stu-id="4e44d-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="4e44d-376">`IHostingEnvironment`pode ser inserida no `Invoke` método ao criar personalizado [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="4e44d-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="4e44d-377">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="4e44d-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="4e44d-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite para atividades de pós-inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="4e44d-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="4e44d-379">Três propriedades na interface são tokens de cancelamento usados para registrar `Action` métodos que definem os eventos de inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="4e44d-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="4e44d-380">Também há um `StopApplication` método.</span><span class="sxs-lookup"><span data-stu-id="4e44d-380">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="4e44d-381">Token de cancelamento</span><span class="sxs-lookup"><span data-stu-id="4e44d-381">Cancellation Token</span></span>    | <span data-ttu-id="4e44d-382">Acionado quando &#8230;</span><span class="sxs-lookup"><span data-stu-id="4e44d-382">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="4e44d-383">O host foi totalmente iniciado.</span><span class="sxs-lookup"><span data-stu-id="4e44d-383">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="4e44d-384">O host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="4e44d-384">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="4e44d-385">Ainda podem estar processando solicitações.</span><span class="sxs-lookup"><span data-stu-id="4e44d-385">Requests may still be processing.</span></span> <span data-ttu-id="4e44d-386">Blocos de desligamento até que esse evento seja concluída.</span><span class="sxs-lookup"><span data-stu-id="4e44d-386">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="4e44d-387">O host está concluindo um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="4e44d-387">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="4e44d-388">Todas as solicitações devem ser processadas.</span><span class="sxs-lookup"><span data-stu-id="4e44d-388">All requests should be processed.</span></span> <span data-ttu-id="4e44d-389">Blocos de desligamento até que esse evento seja concluída.</span><span class="sxs-lookup"><span data-stu-id="4e44d-389">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="4e44d-390">Método</span><span class="sxs-lookup"><span data-stu-id="4e44d-390">Method</span></span>            | <span data-ttu-id="4e44d-391">Ação</span><span class="sxs-lookup"><span data-stu-id="4e44d-391">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="4e44d-392">Encerramento de solicitações do aplicativo atual.</span><span class="sxs-lookup"><span data-stu-id="4e44d-392">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="4e44d-393">Solucionando problemas de System. ArgumentException</span><span class="sxs-lookup"><span data-stu-id="4e44d-393">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="4e44d-394">**Aplica-se ao ASP.NET Core 2.0 apenas**</span><span class="sxs-lookup"><span data-stu-id="4e44d-394">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="4e44d-395">Um host pode ser criado injetando `IStartup` diretamente para o contêiner de injeção de dependência em vez de chamar `UseStartup` ou `Configure`:</span><span class="sxs-lookup"><span data-stu-id="4e44d-395">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="4e44d-396">Se o host for criado dessa maneira, o seguinte erro pode ocorrer:</span><span class="sxs-lookup"><span data-stu-id="4e44d-396">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="4e44d-397">Isso ocorre porque o [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (do assembly atual) é necessário para verificar se há `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="4e44d-398">Se o aplicativo manualmente injeta `IStartup` para o contêiner de injeção de dependência, adicione a seguinte chamada a `WebHostBuilder` com o nome do assembly especificado:</span><span class="sxs-lookup"><span data-stu-id="4e44d-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="4e44d-399">Como alternativa, adicione uma cópia `Configure` para o `WebHostBuilder`, que define o `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="4e44d-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="4e44d-400">**Observação**: isso só é necessária com a versão 2.0 do ASP.NET Core e somente quando o aplicativo não chama `UseStartup` ou `Configure`.</span><span class="sxs-lookup"><span data-stu-id="4e44d-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="4e44d-401">Para obter mais informações, consulte [anúncios: Microsoft.Extensions.PlatformAbstractions foi removido (comentário)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e [StartupInjection exemplo](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="4e44d-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e44d-402">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="4e44d-402">Additional resources</span></span>

* [<span data-ttu-id="4e44d-403">Hospedar no Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="4e44d-403">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="4e44d-404">Hospedar em Linux com o Nginx</span><span class="sxs-lookup"><span data-stu-id="4e44d-404">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="4e44d-405">Hospedar em Linux com o Apache</span><span class="sxs-lookup"><span data-stu-id="4e44d-405">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="4e44d-406">Host em um serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="4e44d-406">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
