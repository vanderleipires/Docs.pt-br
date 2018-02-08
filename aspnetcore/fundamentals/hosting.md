---
title: Hospedagem no ASP.NET Core
author: guardrex
description: "Saiba mais sobre o host da Web no ASP.NET Core, que é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo."
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 78209c8d34fa1a2a164ae333d625feca1e145e89
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="16cc3-103">Hospedagem no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="16cc3-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="16cc3-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="16cc3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="16cc3-105">Aplicativos ASP.NET Core configuram e inicializam um *host*.</span><span class="sxs-lookup"><span data-stu-id="16cc3-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="16cc3-106">O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="16cc3-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="16cc3-107">No mínimo, o host configura um servidor e um pipeline de processamento de solicitações.</span><span class="sxs-lookup"><span data-stu-id="16cc3-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="16cc3-108">Configurando um host</span><span class="sxs-lookup"><span data-stu-id="16cc3-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="16cc3-110">Crie um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="16cc3-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="16cc3-111">Normalmente, isso é feito no ponto de entrada do aplicativo, o método `Main`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="16cc3-112">Em modelos de projeto, `Main` está localizado em *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="16cc3-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="16cc3-113">Um *Program.cs* típico chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para começar a configurar um host:</span><span class="sxs-lookup"><span data-stu-id="16cc3-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="16cc3-114">`CreateDefaultBuilder` executa as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="16cc3-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="16cc3-115">Configura o [Kestrel](servers/kestrel.md) como o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="16cc3-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="16cc3-116">Para conhecer as opções padrão do Kestrel, consulte [Implementação do servidor Web Kestrel no ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="16cc3-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="16cc3-117">Define a raiz do conteúdo como o caminho retornado por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="16cc3-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="16cc3-118">Carrega configurações opcionais de:</span><span class="sxs-lookup"><span data-stu-id="16cc3-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="16cc3-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="16cc3-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="16cc3-120">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="16cc3-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="16cc3-121">[Segredos do usuário](xref:security/app-secrets) quando o aplicativo é executado no ambiente `Development`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="16cc3-122">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="16cc3-122">Environment variables.</span></span>
  * <span data-ttu-id="16cc3-123">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="16cc3-123">Command-line arguments.</span></span>
* <span data-ttu-id="16cc3-124">Configura o [registro em log](xref:fundamentals/logging/index) para a saída do console e de depuração.</span><span class="sxs-lookup"><span data-stu-id="16cc3-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="16cc3-125">O registro em log inclui regras de [filtragem de log](xref:fundamentals/logging/index#log-filtering) especificadas em uma seção de configuração de registro em log de um arquivo *appsettings.json* ou *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="16cc3-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="16cc3-126">Quando executado por trás do IIS, permite a [integração de IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="16cc3-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="16cc3-127">Configura o caminho base e a porta que o servidor escuta ao usar o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="16cc3-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="16cc3-128">O módulo cria um proxy reverso entre o IIS e o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="16cc3-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="16cc3-129">Também configura o aplicativo para [capturar erros de inicialização](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="16cc3-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="16cc3-130">Para conhecer as opções padrão do IIS, consulte [a seção sobre as opções do IIS para Hospedar o ASP.NET Core no Windows com o IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="16cc3-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="16cc3-131">A *raiz do conteúdo* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="16cc3-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="16cc3-132">Quando o aplicativo é iniciado na pasta raiz do projeto, essa pasta é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="16cc3-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="16cc3-133">Esse é o padrão usado no [Visual Studio](https://www.visualstudio.com/) e nos [novos modelos dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="16cc3-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="16cc3-134">Para obter mais informações sobre a configuração de aplicativos, consulte [Configuração no ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="16cc3-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="16cc3-135">Como uma alternativa ao uso do método `CreateDefaultBuilder` estático, criar um host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) é uma abordagem compatível com o ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="16cc3-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="16cc3-136">Para obter mais informações, consulte a guia do ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="16cc3-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="16cc3-138">Crie um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="16cc3-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="16cc3-139">A criação de um host normalmente é feita no ponto de entrada do aplicativo, o método `Main`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="16cc3-140">Em modelos de projeto, `Main` está localizado em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="16cc3-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="16cc3-141">`WebHostBuilder` requer um [servidor que implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="16cc3-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="16cc3-142">Os servidores internos são [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (antes do lançamento do ASP.NET Core 2.0, HTTP.sys era chamado de [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="16cc3-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="16cc3-143">Neste exemplo, o [método de extensão UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica o servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="16cc3-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="16cc3-144">A *raiz do conteúdo* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="16cc3-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="16cc3-145">A raiz de conteúdo padrão é obtida para `UseContentRoot` por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="16cc3-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="16cc3-146">Quando o aplicativo é iniciado na pasta raiz do projeto, essa pasta é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="16cc3-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="16cc3-147">Esse é o padrão usado no [Visual Studio](https://www.visualstudio.com/) e nos [novos modelos dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="16cc3-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="16cc3-148">Para usar o IIS como um proxy reverso, chame [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte da compilação do host.</span><span class="sxs-lookup"><span data-stu-id="16cc3-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="16cc3-149">`UseIISIntegration` não configura um *servidor* como [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) o faz.</span><span class="sxs-lookup"><span data-stu-id="16cc3-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="16cc3-150">`UseIISIntegration` configura o caminho base e a porta que o servidor escuta ao usar o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para criar um proxy reverso entre o Kestrel e o IIS.</span><span class="sxs-lookup"><span data-stu-id="16cc3-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="16cc3-151">Para usar o IIS com o ASP.NET Core, tanto `UseKestrel` quanto `UseIISIntegration` precisam ser especificados.</span><span class="sxs-lookup"><span data-stu-id="16cc3-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="16cc3-152">`UseIISIntegration` é ativado somente quando é executado por trás do IIS ou do IIS Express.</span><span class="sxs-lookup"><span data-stu-id="16cc3-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="16cc3-153">Para obter mais informações, consulte [Referência de configuração do módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e [Introdução ao Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="16cc3-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="16cc3-154">Uma implementação mínima que configura um host (e um aplicativo ASP.NET Core) inclui a especificação de um servidor e a configuração do pipeline de solicitações do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="16cc3-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="16cc3-155">Ao configurar um host, os métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) podem ser fornecidos.</span><span class="sxs-lookup"><span data-stu-id="16cc3-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="16cc3-156">Se uma classe `Startup` for especificada, ela deverá definir um método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="16cc3-157">Para obter mais informações, consulte [Inicialização do aplicativo no ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="16cc3-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="16cc3-158">Diversas chamadas para `ConfigureServices` são acrescentadas umas às outras.</span><span class="sxs-lookup"><span data-stu-id="16cc3-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="16cc3-159">Diversas chamadas para `Configure` ou `UseStartup` no `WebHostBuilder` substituem configurações anteriores.</span><span class="sxs-lookup"><span data-stu-id="16cc3-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="16cc3-160">Valores de configuração do host</span><span class="sxs-lookup"><span data-stu-id="16cc3-160">Host configuration values</span></span>

<span data-ttu-id="16cc3-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) conta com as seguintes abordagens para definir os valores de configuração do host:</span><span class="sxs-lookup"><span data-stu-id="16cc3-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="16cc3-162">Configuração do construtor do host, que inclui variáveis de ambiente com o formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="16cc3-163">Por exemplo, `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="16cc3-164">Métodos explícitos, como `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="16cc3-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e a chave associada.</span><span class="sxs-lookup"><span data-stu-id="16cc3-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="16cc3-166">Ao definir um valor com `UseSetting`, o valor é definido como uma cadeia de caracteres, independentemente do tipo.</span><span class="sxs-lookup"><span data-stu-id="16cc3-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="16cc3-167">O host usa a opção que define um valor por último.</span><span class="sxs-lookup"><span data-stu-id="16cc3-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="16cc3-168">Para obter mais informações, consulte [Substituindo a configuração](#overriding-configuration) na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="16cc3-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="16cc3-169">Capturar erros de inicialização</span><span class="sxs-lookup"><span data-stu-id="16cc3-169">Capture Startup Errors</span></span>

<span data-ttu-id="16cc3-170">Esta configuração controla a captura de erros de inicialização.</span><span class="sxs-lookup"><span data-stu-id="16cc3-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="16cc3-171">**Chave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="16cc3-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="16cc3-172">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="16cc3-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="16cc3-173">**Padrão**: o padrão é `false`, a menos que o aplicativo seja executado com o Kestrel por trás do IIS, em que o padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="16cc3-174">**Definido usando**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="16cc3-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="16cc3-175">**Variável de ambiente**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="16cc3-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="16cc3-176">Quando `false`, erros durante a inicialização resultam no encerramento do host.</span><span class="sxs-lookup"><span data-stu-id="16cc3-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="16cc3-177">Quando `true`, o host captura exceções durante a inicialização e tenta iniciar o servidor.</span><span class="sxs-lookup"><span data-stu-id="16cc3-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="16cc3-180">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="16cc3-180">Content Root</span></span>

<span data-ttu-id="16cc3-181">Essa configuração determina onde o ASP.NET Core começa a procurar por arquivos de conteúdo, como exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="16cc3-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="16cc3-182">**Chave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="16cc3-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="16cc3-183">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="16cc3-183">**Type**: *string*</span></span>  
<span data-ttu-id="16cc3-184">**Padrão**: o padrão é a pasta em que o assembly do aplicativo reside.</span><span class="sxs-lookup"><span data-stu-id="16cc3-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="16cc3-185">**Definido usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="16cc3-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="16cc3-186">**Variável de ambiente**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="16cc3-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="16cc3-187">A raiz do conteúdo também é usada como o caminho base para a [Configuração da raiz da Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="16cc3-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="16cc3-188">Se o caminho não existir, o host não será iniciado.</span><span class="sxs-lookup"><span data-stu-id="16cc3-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="16cc3-191">Erros detalhados</span><span class="sxs-lookup"><span data-stu-id="16cc3-191">Detailed Errors</span></span>

<span data-ttu-id="16cc3-192">Determina se erros detalhados devem ser capturados.</span><span class="sxs-lookup"><span data-stu-id="16cc3-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="16cc3-193">**Chave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="16cc3-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="16cc3-194">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="16cc3-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="16cc3-195">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="16cc3-195">**Default**: false</span></span>  
<span data-ttu-id="16cc3-196">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="16cc3-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="16cc3-197">**Variável de ambiente**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="16cc3-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="16cc3-198">Quando habilitado (ou quando o <a href="#environment">Ambiente</a> é definido como `Development`), o aplicativo captura exceções detalhadas.</span><span class="sxs-lookup"><span data-stu-id="16cc3-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="16cc3-201">Ambiente</span><span class="sxs-lookup"><span data-stu-id="16cc3-201">Environment</span></span>

<span data-ttu-id="16cc3-202">Define o ambiente do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="16cc3-202">Sets the app's environment.</span></span>

<span data-ttu-id="16cc3-203">**Chave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="16cc3-203">**Key**: environment</span></span>  
<span data-ttu-id="16cc3-204">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="16cc3-204">**Type**: *string*</span></span>  
<span data-ttu-id="16cc3-205">**Padrão**: Production</span><span class="sxs-lookup"><span data-stu-id="16cc3-205">**Default**: Production</span></span>  
<span data-ttu-id="16cc3-206">**Definido usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="16cc3-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="16cc3-207">**Variável de ambiente**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="16cc3-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="16cc3-208">O ambiente pode ser definido como qualquer valor.</span><span class="sxs-lookup"><span data-stu-id="16cc3-208">The environment can be set to any value.</span></span> <span data-ttu-id="16cc3-209">Os valores definidos pela estrutura incluem `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="16cc3-210">Os valores não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="16cc3-210">Values aren't case sensitive.</span></span> <span data-ttu-id="16cc3-211">Por padrão, o *Ambiente* é lido da variável de ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="16cc3-212">Ao usar o [Visual Studio](https://www.visualstudio.com/), variáveis de ambiente podem ser definidas no arquivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="16cc3-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="16cc3-213">Para obter mais informações, consulte [Trabalhando com vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="16cc3-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="16cc3-216">Hospedando assemblies de inicialização</span><span class="sxs-lookup"><span data-stu-id="16cc3-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="16cc3-217">Define os assemblies de inicialização de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="16cc3-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="16cc3-218">**Chave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="16cc3-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="16cc3-219">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="16cc3-219">**Type**: *string*</span></span>  
<span data-ttu-id="16cc3-220">**Padrão**: cadeia de caracteres vazia</span><span class="sxs-lookup"><span data-stu-id="16cc3-220">**Default**: Empty string</span></span>  
<span data-ttu-id="16cc3-221">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="16cc3-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="16cc3-222">**Variável de ambiente**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="16cc3-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="16cc3-223">Uma cadeia de caracteres delimitada por ponto e vírgula de assemblies de inicialização de hospedagem para carregamento na inicialização.</span><span class="sxs-lookup"><span data-stu-id="16cc3-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="16cc3-224">Este recurso é novo no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="16cc3-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="16cc3-225">Embora o valor padrão da configuração seja uma cadeia de caracteres vazia, os assemblies de inicialização de hospedagem sempre incluem o assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="16cc3-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="16cc3-226">Quando assemblies de inicialização de hospedagem são fornecidos, eles são adicionados ao assembly do aplicativo para carregamento quando o aplicativo compilar seus serviços comuns durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="16cc3-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="16cc3-229">Este recurso não está disponível no ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="16cc3-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="16cc3-230">Preferir URLs de hospedagem</span><span class="sxs-lookup"><span data-stu-id="16cc3-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="16cc3-231">Indica se o host deve escutar as URLs configuradas com o `WebHostBuilder` em vez daquelas configuradas com a implementação `IServer`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="16cc3-232">**Chave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="16cc3-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="16cc3-233">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="16cc3-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="16cc3-234">**Padrão**: true</span><span class="sxs-lookup"><span data-stu-id="16cc3-234">**Default**: true</span></span>  
<span data-ttu-id="16cc3-235">**Definido usando**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="16cc3-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="16cc3-236">**Variável de ambiente**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="16cc3-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="16cc3-237">Este recurso é novo no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="16cc3-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="16cc3-240">Este recurso não está disponível no ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="16cc3-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="16cc3-241">Impedir inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="16cc3-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="16cc3-242">Impede o carregamento automático de assemblies de inicialização de hospedagem, incluindo assemblies de inicialização de hospedagem configurados pelo assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="16cc3-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="16cc3-243">Consulte [Adicionar recursos de aplicativo de um assembly externo usando IHostingStartup](xref:host-and-deploy/ihostingstartup) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="16cc3-243">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="16cc3-244">**Chave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="16cc3-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="16cc3-245">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="16cc3-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="16cc3-246">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="16cc3-246">**Default**: false</span></span>  
<span data-ttu-id="16cc3-247">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="16cc3-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="16cc3-248">**Variável de ambiente**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="16cc3-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="16cc3-249">Este recurso é novo no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="16cc3-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="16cc3-252">Este recurso não está disponível no ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="16cc3-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="16cc3-253">URLs de servidor</span><span class="sxs-lookup"><span data-stu-id="16cc3-253">Server URLs</span></span>

<span data-ttu-id="16cc3-254">Indica os endereços IP ou endereços de host com portas e protocolos que o servidor deve escutar para solicitações.</span><span class="sxs-lookup"><span data-stu-id="16cc3-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="16cc3-255">**Chave**: urls</span><span class="sxs-lookup"><span data-stu-id="16cc3-255">**Key**: urls</span></span>  
<span data-ttu-id="16cc3-256">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="16cc3-256">**Type**: *string*</span></span>  
<span data-ttu-id="16cc3-257">**Padrão**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="16cc3-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="16cc3-258">**Definido usando**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="16cc3-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="16cc3-259">**Variável de ambiente**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="16cc3-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="16cc3-260">Defina como uma lista separada por ponto e vírgula (;) de prefixos de URL aos quais o servidor deve responder.</span><span class="sxs-lookup"><span data-stu-id="16cc3-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="16cc3-261">Por exemplo, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="16cc3-262">Use "\*" para indicar que o servidor deve escutar solicitações em qualquer endereço IP ou nome do host usando a porta e o protocolo especificados (por exemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="16cc3-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="16cc3-263">O protocolo (`http://` ou `https://`) deve ser incluído com cada URL.</span><span class="sxs-lookup"><span data-stu-id="16cc3-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="16cc3-264">Os formatos compatíveis variam dependendo dos servidores.</span><span class="sxs-lookup"><span data-stu-id="16cc3-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="16cc3-266">O Kestrel tem sua própria API de configuração de ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="16cc3-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="16cc3-267">Para obter mais informações, consulte [Implementação do servidor Web Kestrel no ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="16cc3-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="16cc3-269">Tempo limite de desligamento</span><span class="sxs-lookup"><span data-stu-id="16cc3-269">Shutdown Timeout</span></span>

<span data-ttu-id="16cc3-270">Especifica o tempo de espera para o desligamento do host da Web.</span><span class="sxs-lookup"><span data-stu-id="16cc3-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="16cc3-271">**Chave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="16cc3-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="16cc3-272">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="16cc3-272">**Type**: *int*</span></span>  
<span data-ttu-id="16cc3-273">**Padrão**: 5</span><span class="sxs-lookup"><span data-stu-id="16cc3-273">**Default**: 5</span></span>  
<span data-ttu-id="16cc3-274">**Definido usando**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="16cc3-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="16cc3-275">**Variável de ambiente**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="16cc3-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="16cc3-276">Embora a chave aceite um *int* com `UseSetting` (por exemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), o `UseShutdownTimeout` método de extensão usa um `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="16cc3-277">Este recurso é novo no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="16cc3-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="16cc3-280">Este recurso não está disponível no ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="16cc3-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="16cc3-281">Assembly de inicialização</span><span class="sxs-lookup"><span data-stu-id="16cc3-281">Startup Assembly</span></span>

<span data-ttu-id="16cc3-282">Determina o assembly para pesquisar pela classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="16cc3-283">**Chave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="16cc3-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="16cc3-284">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="16cc3-284">**Type**: *string*</span></span>  
<span data-ttu-id="16cc3-285">**Padrão**: o assembly do aplicativo</span><span class="sxs-lookup"><span data-stu-id="16cc3-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="16cc3-286">**Definido usando**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="16cc3-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="16cc3-287">**Variável de ambiente**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="16cc3-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="16cc3-288">O assembly por nome (`string`) ou por tipo (`TStartup`) pode ser referenciado.</span><span class="sxs-lookup"><span data-stu-id="16cc3-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="16cc3-289">Se vários métodos `UseStartup` forem chamados, o último terá precedência.</span><span class="sxs-lookup"><span data-stu-id="16cc3-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="16cc3-292">Raiz da Web</span><span class="sxs-lookup"><span data-stu-id="16cc3-292">Web Root</span></span>

<span data-ttu-id="16cc3-293">Define o caminho relativo para os ativos estáticos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="16cc3-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="16cc3-294">**Chave**: webroot</span><span class="sxs-lookup"><span data-stu-id="16cc3-294">**Key**: webroot</span></span>  
<span data-ttu-id="16cc3-295">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="16cc3-295">**Type**: *string*</span></span>  
<span data-ttu-id="16cc3-296">**Padrão**: se não for especificado, o padrão será "(Raiz do conteúdo)/wwwroot", se o caminho existir.</span><span class="sxs-lookup"><span data-stu-id="16cc3-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="16cc3-297">Se o caminho não existir, um provedor de arquivo não operacional será usado.</span><span class="sxs-lookup"><span data-stu-id="16cc3-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="16cc3-298">**Definido usando**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="16cc3-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="16cc3-299">**Variável de ambiente**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="16cc3-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="16cc3-302">Substituindo a configuração</span><span class="sxs-lookup"><span data-stu-id="16cc3-302">Overriding configuration</span></span>

<span data-ttu-id="16cc3-303">Use [Configuração](xref:fundamentals/configuration/index) para configurar o host.</span><span class="sxs-lookup"><span data-stu-id="16cc3-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="16cc3-304">No exemplo a seguir, a configuração do host é especificada, opcionalmente, em um arquivo *hosting.json*.</span><span class="sxs-lookup"><span data-stu-id="16cc3-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="16cc3-305">Qualquer configuração carregada do arquivo *hosting.json* pode ser substituída por argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="16cc3-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="16cc3-306">A configuração interna (no `config`) é usada para configurar o host com `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="16cc3-308">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="16cc3-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="16cc3-309">Substituição da configuração fornecida por `UseUrls` pela configuração de *hosting.json* primeiro e pela configuração de argumento da linha de comando depois:</span><span class="sxs-lookup"><span data-stu-id="16cc3-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="16cc3-311">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="16cc3-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="16cc3-312">Substituição da configuração fornecida por `UseUrls` pela configuração de *hosting.json* primeiro e pela configuração de argumento da linha de comando depois:</span><span class="sxs-lookup"><span data-stu-id="16cc3-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="16cc3-313">Atualmente, o método de extensão [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) não é capaz de analisar uma seção de configuração retornada por `GetSection` (por exemplo, `.UseConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="16cc3-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="16cc3-314">O método `GetSection` filtra as chaves de configuração da seção solicitada, mas deixa o nome da seção nas chaves (por exemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="16cc3-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="16cc3-315">O método `UseConfiguration` espera que as chaves correspondam às chaves `WebHostBuilder` (por exemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="16cc3-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="16cc3-316">A presença do nome da seção nas chaves impede que os valores da seção configurem o host.</span><span class="sxs-lookup"><span data-stu-id="16cc3-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="16cc3-317">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="16cc3-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="16cc3-318">Para obter mais informações e soluções alternativas, consulte [Passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="16cc3-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="16cc3-319">Para especificar o host executado em uma URL em particular, o valor desejado pode ser passado de um prompt de comando ao executar `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="16cc3-320">O argumento de linha de comando substitui o valor `urls` do arquivo *hosting.json* e o servidor escuta na porta 8080:</span><span class="sxs-lookup"><span data-stu-id="16cc3-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="16cc3-321">Iniciando o host</span><span class="sxs-lookup"><span data-stu-id="16cc3-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="16cc3-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="16cc3-323">**Executar**</span><span class="sxs-lookup"><span data-stu-id="16cc3-323">**Run**</span></span>

<span data-ttu-id="16cc3-324">O método `Run` inicia o aplicativo Web e bloqueia o thread de chamada até que o host seja desligado:</span><span class="sxs-lookup"><span data-stu-id="16cc3-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="16cc3-325">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="16cc3-325">**Start**</span></span>

<span data-ttu-id="16cc3-326">Execute o host sem bloqueio, chamando seu método `Start`:</span><span class="sxs-lookup"><span data-stu-id="16cc3-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="16cc3-327">Se uma lista de URLs for passada para o método `Start`, ele escutará nas URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="16cc3-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="16cc3-328">O aplicativo pode inicializar e iniciar um novo host usando os padrões pré-configurados de `CreateDefaultBuilder`, usando um método estático conveniente.</span><span class="sxs-lookup"><span data-stu-id="16cc3-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="16cc3-329">Esses métodos iniciam o servidor sem uma saída do console e com [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) e aguardam uma quebra (Ctrl-C/SIGINT ou SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="16cc3-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="16cc3-330">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="16cc3-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="16cc3-331">Inicie com um `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="16cc3-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="16cc3-332">Faça uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Olá, Mundo!"</span><span class="sxs-lookup"><span data-stu-id="16cc3-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="16cc3-333">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="16cc3-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="16cc3-334">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="16cc3-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="16cc3-335">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="16cc3-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="16cc3-336">Inicie com uma URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="16cc3-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="16cc3-337">Produz o mesmo resultado que **Start(RequestDelegate app)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="16cc3-338">**Start(Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="16cc3-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="16cc3-339">Use uma instância de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar o middleware de roteamento:</span><span class="sxs-lookup"><span data-stu-id="16cc3-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="16cc3-340">Use as seguintes solicitações de navegador com o exemplo:</span><span class="sxs-lookup"><span data-stu-id="16cc3-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="16cc3-341">Solicitação</span><span class="sxs-lookup"><span data-stu-id="16cc3-341">Request</span></span>                                    | <span data-ttu-id="16cc3-342">Resposta</span><span class="sxs-lookup"><span data-stu-id="16cc3-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="16cc3-343">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="16cc3-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="16cc3-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="16cc3-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="16cc3-345">Gera uma exceção com a cadeia de caracteres "ooops!"</span><span class="sxs-lookup"><span data-stu-id="16cc3-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="16cc3-346">Gera uma exceção com a cadeia de caracteres "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="16cc3-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="16cc3-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="16cc3-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="16cc3-348">Olá, Mundo!</span><span class="sxs-lookup"><span data-stu-id="16cc3-348">Hello World!</span></span>                             |

<span data-ttu-id="16cc3-349">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="16cc3-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="16cc3-350">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="16cc3-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="16cc3-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="16cc3-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="16cc3-352">Use uma URL e uma instância de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="16cc3-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="16cc3-353">Produz o mesmo resultado que **Start(Action<IRouteBuilder> routeBuilder)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="16cc3-354">**StartWith(Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="16cc3-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="16cc3-355">Forneça um delegado para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="16cc3-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="16cc3-356">Faça uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Olá, Mundo!"</span><span class="sxs-lookup"><span data-stu-id="16cc3-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="16cc3-357">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="16cc3-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="16cc3-358">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="16cc3-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="16cc3-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="16cc3-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="16cc3-360">Forneça um delegado e uma URL para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="16cc3-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="16cc3-361">Produz o mesmo resultado que **StartWith(Action<IApplicationBuilder> app)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="16cc3-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="16cc3-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="16cc3-363">**Executar**</span><span class="sxs-lookup"><span data-stu-id="16cc3-363">**Run**</span></span>

<span data-ttu-id="16cc3-364">O método `Run` inicia o aplicativo Web e bloqueia o thread de chamada até que o host seja desligado:</span><span class="sxs-lookup"><span data-stu-id="16cc3-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="16cc3-365">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="16cc3-365">**Start**</span></span>

<span data-ttu-id="16cc3-366">Execute o host sem bloqueio, chamando seu método `Start`:</span><span class="sxs-lookup"><span data-stu-id="16cc3-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="16cc3-367">Se uma lista de URLs for passada para o método `Start`, ele escutará nas URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="16cc3-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="16cc3-368">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="16cc3-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="16cc3-369">A [interface IHostingEnvironment](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornece informações sobre o ambiente de hospedagem na Web do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="16cc3-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="16cc3-370">Use a [injeção de construtor](xref:fundamentals/dependency-injection) para obter o `IHostingEnvironment` para usar suas propriedades e métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="16cc3-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="16cc3-371">Uma [abordagem baseada em convenção](xref:fundamentals/environments#startup-conventions) pode ser usada para configurar o aplicativo na inicialização com base no ambiente.</span><span class="sxs-lookup"><span data-stu-id="16cc3-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="16cc3-372">Como alternativa, injete o `IHostingEnvironment` no construtor `Startup` para uso em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="16cc3-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="16cc3-373">Além do método de extensão `IsDevelopment`, `IHostingEnvironment` oferece os métodos `IsStaging`, `IsProduction` e `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="16cc3-374">Consulte [Trabalhando com vários ambientes](xref:fundamentals/environments) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="16cc3-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="16cc3-375">O serviço `IHostingEnvironment` também pode ser injetado diretamente no método `Configure` para configurar o pipeline de processamento:</span><span class="sxs-lookup"><span data-stu-id="16cc3-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="16cc3-376">`IHostingEnvironment` pode ser injetado no método `Invoke` ao criar um [middleware](xref:fundamentals/middleware#writing-middleware) personalizado:</span><span class="sxs-lookup"><span data-stu-id="16cc3-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="16cc3-377">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="16cc3-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="16cc3-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite atividades pós-inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="16cc3-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="16cc3-379">Três propriedades na interface são tokens de cancelamento usados para registrar métodos `Action` que definem eventos de inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="16cc3-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="16cc3-380">Também há um método `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-380">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="16cc3-381">Token de cancelamento</span><span class="sxs-lookup"><span data-stu-id="16cc3-381">Cancellation Token</span></span>    | <span data-ttu-id="16cc3-382">Acionado quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="16cc3-382">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="16cc3-383">O host foi iniciado totalmente.</span><span class="sxs-lookup"><span data-stu-id="16cc3-383">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="16cc3-384">O host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="16cc3-384">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="16cc3-385">Solicitações ainda podem estar sendo processadas.</span><span class="sxs-lookup"><span data-stu-id="16cc3-385">Requests may still be processing.</span></span> <span data-ttu-id="16cc3-386">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="16cc3-386">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="16cc3-387">O host está concluindo um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="16cc3-387">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="16cc3-388">Todas as solicitações devem ser processadas.</span><span class="sxs-lookup"><span data-stu-id="16cc3-388">All requests should be processed.</span></span> <span data-ttu-id="16cc3-389">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="16cc3-389">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="16cc3-390">Método</span><span class="sxs-lookup"><span data-stu-id="16cc3-390">Method</span></span>            | <span data-ttu-id="16cc3-391">Ação</span><span class="sxs-lookup"><span data-stu-id="16cc3-391">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="16cc3-392">Solicita o encerramento do aplicativo atual.</span><span class="sxs-lookup"><span data-stu-id="16cc3-392">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="16cc3-393">Solução de problemas de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="16cc3-393">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="16cc3-394">**Aplica-se somente ao ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="16cc3-394">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="16cc3-395">Um host pode ser compilado injetando `IStartup` diretamente no contêiner de injeção de dependência em vez de chamar `UseStartup` ou `Configure`:</span><span class="sxs-lookup"><span data-stu-id="16cc3-395">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="16cc3-396">Se o host for compilado dessa maneira, o seguinte erro poderá ocorrer:</span><span class="sxs-lookup"><span data-stu-id="16cc3-396">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="16cc3-397">Isso ocorre porque o [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (do assembly atual) é necessário para verificar se há `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="16cc3-398">Se o aplicativo injetar manualmente `IStartup` no contêiner de injeção de dependência, adicione a seguinte chamada para `WebHostBuilder` com o nome do assembly especificado:</span><span class="sxs-lookup"><span data-stu-id="16cc3-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="16cc3-399">Como alternativa, adicione um `Configure` fictício ao `WebHostBuilder`, que define o `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="16cc3-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="16cc3-400">**Observação**: isso só é necessário com a versão do ASP.NET Core 2.0 e somente quando o aplicativo não chama `UseStartup` ou `Configure`.</span><span class="sxs-lookup"><span data-stu-id="16cc3-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="16cc3-401">Para obter mais informações, consulte [Comunicado: Microsoft.Extensions.PlatformAbstractions foi removido (comentário)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e o [Exemplo de StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="16cc3-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16cc3-402">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="16cc3-402">Additional resources</span></span>

* [<span data-ttu-id="16cc3-403">Hospedar no Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="16cc3-403">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="16cc3-404">Hospedar em Linux com o Nginx</span><span class="sxs-lookup"><span data-stu-id="16cc3-404">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="16cc3-405">Hospedar em Linux com o Apache</span><span class="sxs-lookup"><span data-stu-id="16cc3-405">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="16cc3-406">Hospedar em um serviço Windows</span><span class="sxs-lookup"><span data-stu-id="16cc3-406">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
