---
title: Hospedagem no ASP.NET Core
author: guardrex
description: Saiba mais sobre o host da Web no ASP.NET Core, que é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 344bf5f0917f4c33d67eeb14176ff2aae3ae75da
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="acc3a-103">Hospedagem no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="acc3a-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="acc3a-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="acc3a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="acc3a-105">Aplicativos ASP.NET Core configuram e inicializam um *host*.</span><span class="sxs-lookup"><span data-stu-id="acc3a-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="acc3a-106">O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="acc3a-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="acc3a-107">No mínimo, o host configura um servidor e um pipeline de processamento de solicitações.</span><span class="sxs-lookup"><span data-stu-id="acc3a-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="acc3a-108">Configurando um host</span><span class="sxs-lookup"><span data-stu-id="acc3a-108">Setting up a host</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="acc3a-110">Crie um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="acc3a-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="acc3a-111">Normalmente, isso é feito no ponto de entrada do aplicativo, o método `Main`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="acc3a-112">Em modelos de projeto, `Main` está localizado em *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="acc3a-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="acc3a-113">Um *Program.cs* típico chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para começar a configurar um host:</span><span class="sxs-lookup"><span data-stu-id="acc3a-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="acc3a-114">`CreateDefaultBuilder` executa as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="acc3a-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="acc3a-115">Configura o [Kestrel](servers/kestrel.md) como o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="acc3a-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="acc3a-116">Para conhecer as opções padrão do Kestrel, consulte [Implementação do servidor Web Kestrel no ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="acc3a-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="acc3a-117">Define a raiz do conteúdo como o caminho retornado por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="acc3a-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="acc3a-118">Carrega configurações opcionais de:</span><span class="sxs-lookup"><span data-stu-id="acc3a-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="acc3a-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="acc3a-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="acc3a-120">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="acc3a-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="acc3a-121">[Segredos do usuário](xref:security/app-secrets) quando o aplicativo é executado no ambiente `Development`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="acc3a-122">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="acc3a-122">Environment variables.</span></span>
  * <span data-ttu-id="acc3a-123">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="acc3a-123">Command-line arguments.</span></span>
* <span data-ttu-id="acc3a-124">Configura o [registro em log](xref:fundamentals/logging/index) para a saída do console e de depuração.</span><span class="sxs-lookup"><span data-stu-id="acc3a-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="acc3a-125">O registro em log inclui regras de [filtragem de log](xref:fundamentals/logging/index#log-filtering) especificadas em uma seção de configuração de registro em log de um arquivo *appsettings.json* ou *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="acc3a-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="acc3a-126">Quando executado por trás do IIS, permite a [integração de IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="acc3a-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="acc3a-127">Configura o caminho base e a porta que o servidor escuta ao usar o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="acc3a-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="acc3a-128">O módulo cria um proxy reverso entre o IIS e o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="acc3a-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="acc3a-129">Também configura o aplicativo para [capturar erros de inicialização](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="acc3a-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="acc3a-130">Para conhecer as opções padrão do IIS, consulte [a seção sobre as opções do IIS para Hospedar o ASP.NET Core no Windows com o IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="acc3a-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="acc3a-131">Definirá [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) como `true` se o ambiente do aplicativo for de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="acc3a-131">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="acc3a-132">Para obter mais informações, confira [Validação de escopo](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="acc3a-132">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="acc3a-133">A *raiz do conteúdo* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="acc3a-133">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="acc3a-134">Quando o aplicativo é iniciado na pasta raiz do projeto, essa pasta é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="acc3a-134">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="acc3a-135">Esse é o padrão usado no [Visual Studio](https://www.visualstudio.com/) e nos [novos modelos dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="acc3a-135">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="acc3a-136">Para obter mais informações sobre a configuração de aplicativos, consulte [Configuração no ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="acc3a-136">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="acc3a-137">Como uma alternativa ao uso do método `CreateDefaultBuilder` estático, criar um host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) é uma abordagem compatível com o ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="acc3a-137">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="acc3a-138">Para obter mais informações, consulte a guia do ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="acc3a-138">For more information, see the ASP.NET Core 1.x tab.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="acc3a-140">Crie um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="acc3a-140">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="acc3a-141">A criação de um host normalmente é feita no ponto de entrada do aplicativo, o método `Main`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-141">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="acc3a-142">Em modelos de projeto, `Main` está localizado em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="acc3a-142">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="acc3a-143">`WebHostBuilder` requer um [servidor que implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="acc3a-143">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="acc3a-144">Os servidores internos são [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (antes do lançamento do ASP.NET Core 2.0, HTTP.sys era chamado de [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="acc3a-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="acc3a-145">Neste exemplo, o [método de extensão UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica o servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="acc3a-145">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="acc3a-146">A *raiz do conteúdo* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="acc3a-146">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="acc3a-147">A raiz de conteúdo padrão é obtida para `UseContentRoot` por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="acc3a-147">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="acc3a-148">Quando o aplicativo é iniciado na pasta raiz do projeto, essa pasta é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="acc3a-148">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="acc3a-149">Esse é o padrão usado no [Visual Studio](https://www.visualstudio.com/) e nos [novos modelos dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="acc3a-149">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="acc3a-150">Para usar o IIS como um proxy reverso, chame [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte da compilação do host.</span><span class="sxs-lookup"><span data-stu-id="acc3a-150">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="acc3a-151">`UseIISIntegration` não configura um *servidor* como [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) o faz.</span><span class="sxs-lookup"><span data-stu-id="acc3a-151">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="acc3a-152">`UseIISIntegration` configura o caminho base e a porta que o servidor escuta ao usar o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para criar um proxy reverso entre o Kestrel e o IIS.</span><span class="sxs-lookup"><span data-stu-id="acc3a-152">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="acc3a-153">Para usar o IIS com o ASP.NET Core, tanto `UseKestrel` quanto `UseIISIntegration` precisam ser especificados.</span><span class="sxs-lookup"><span data-stu-id="acc3a-153">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="acc3a-154">`UseIISIntegration` é ativado somente quando é executado por trás do IIS ou do IIS Express.</span><span class="sxs-lookup"><span data-stu-id="acc3a-154">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="acc3a-155">Para obter mais informações, consulte [Referência de configuração do módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e [Introdução ao Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="acc3a-155">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="acc3a-156">Uma implementação mínima que configura um host (e um aplicativo ASP.NET Core) inclui a especificação de um servidor e a configuração do pipeline de solicitações do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="acc3a-156">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

* * *
<span data-ttu-id="acc3a-157">Ao configurar um host, os métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) podem ser fornecidos.</span><span class="sxs-lookup"><span data-stu-id="acc3a-157">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="acc3a-158">Se uma classe `Startup` for especificada, ela deverá definir um método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-158">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="acc3a-159">Para obter mais informações, consulte [Inicialização do aplicativo no ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="acc3a-159">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="acc3a-160">Diversas chamadas para `ConfigureServices` são acrescentadas umas às outras.</span><span class="sxs-lookup"><span data-stu-id="acc3a-160">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="acc3a-161">Diversas chamadas para `Configure` ou `UseStartup` no `WebHostBuilder` substituem configurações anteriores.</span><span class="sxs-lookup"><span data-stu-id="acc3a-161">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="acc3a-162">Valores de configuração do host</span><span class="sxs-lookup"><span data-stu-id="acc3a-162">Host configuration values</span></span>

<span data-ttu-id="acc3a-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) conta com as seguintes abordagens para definir os valores de configuração do host:</span><span class="sxs-lookup"><span data-stu-id="acc3a-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="acc3a-164">Configuração do construtor do host, que inclui variáveis de ambiente com o formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-164">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="acc3a-165">Por exemplo, `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-165">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="acc3a-166">Métodos explícitos, como `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-166">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="acc3a-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e a chave associada.</span><span class="sxs-lookup"><span data-stu-id="acc3a-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="acc3a-168">Ao definir um valor com `UseSetting`, o valor é definido como uma cadeia de caracteres, independentemente do tipo.</span><span class="sxs-lookup"><span data-stu-id="acc3a-168">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="acc3a-169">O host usa a opção que define um valor por último.</span><span class="sxs-lookup"><span data-stu-id="acc3a-169">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="acc3a-170">Para obter mais informações, consulte [Substituindo a configuração](#overriding-configuration) na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="acc3a-170">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="acc3a-171">Capturar erros de inicialização</span><span class="sxs-lookup"><span data-stu-id="acc3a-171">Capture Startup Errors</span></span>

<span data-ttu-id="acc3a-172">Esta configuração controla a captura de erros de inicialização.</span><span class="sxs-lookup"><span data-stu-id="acc3a-172">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="acc3a-173">**Chave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="acc3a-173">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="acc3a-174">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="acc3a-174">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="acc3a-175">**Padrão**: o padrão é `false`, a menos que o aplicativo seja executado com o Kestrel por trás do IIS, em que o padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-175">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="acc3a-176">**Definido usando**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="acc3a-176">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="acc3a-177">**Variável de ambiente**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="acc3a-177">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="acc3a-178">Quando `false`, erros durante a inicialização resultam no encerramento do host.</span><span class="sxs-lookup"><span data-stu-id="acc3a-178">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="acc3a-179">Quando `true`, o host captura exceções durante a inicialização e tenta iniciar o servidor.</span><span class="sxs-lookup"><span data-stu-id="acc3a-179">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="acc3a-182">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="acc3a-182">Content Root</span></span>

<span data-ttu-id="acc3a-183">Essa configuração determina onde o ASP.NET Core começa a procurar por arquivos de conteúdo, como exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="acc3a-183">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="acc3a-184">**Chave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="acc3a-184">**Key**: contentRoot</span></span>  
<span data-ttu-id="acc3a-185">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="acc3a-185">**Type**: *string*</span></span>  
<span data-ttu-id="acc3a-186">**Padrão**: o padrão é a pasta em que o assembly do aplicativo reside.</span><span class="sxs-lookup"><span data-stu-id="acc3a-186">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="acc3a-187">**Definido usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="acc3a-187">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="acc3a-188">**Variável de ambiente**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="acc3a-188">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="acc3a-189">A raiz do conteúdo também é usada como o caminho base para a [Configuração da raiz da Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="acc3a-189">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="acc3a-190">Se o caminho não existir, o host não será iniciado.</span><span class="sxs-lookup"><span data-stu-id="acc3a-190">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="acc3a-193">Erros detalhados</span><span class="sxs-lookup"><span data-stu-id="acc3a-193">Detailed Errors</span></span>

<span data-ttu-id="acc3a-194">Determina se erros detalhados devem ser capturados.</span><span class="sxs-lookup"><span data-stu-id="acc3a-194">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="acc3a-195">**Chave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="acc3a-195">**Key**: detailedErrors</span></span>  
<span data-ttu-id="acc3a-196">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="acc3a-196">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="acc3a-197">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="acc3a-197">**Default**: false</span></span>  
<span data-ttu-id="acc3a-198">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="acc3a-198">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="acc3a-199">**Variável de ambiente**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="acc3a-199">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="acc3a-200">Quando habilitado (ou quando o <a href="#environment">Ambiente</a> é definido como `Development`), o aplicativo captura exceções detalhadas.</span><span class="sxs-lookup"><span data-stu-id="acc3a-200">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="acc3a-203">Ambiente</span><span class="sxs-lookup"><span data-stu-id="acc3a-203">Environment</span></span>

<span data-ttu-id="acc3a-204">Define o ambiente do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="acc3a-204">Sets the app's environment.</span></span>

<span data-ttu-id="acc3a-205">**Chave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="acc3a-205">**Key**: environment</span></span>  
<span data-ttu-id="acc3a-206">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="acc3a-206">**Type**: *string*</span></span>  
<span data-ttu-id="acc3a-207">**Padrão**: Production</span><span class="sxs-lookup"><span data-stu-id="acc3a-207">**Default**: Production</span></span>  
<span data-ttu-id="acc3a-208">**Definido usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="acc3a-208">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="acc3a-209">**Variável de ambiente**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="acc3a-209">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="acc3a-210">O ambiente pode ser definido como qualquer valor.</span><span class="sxs-lookup"><span data-stu-id="acc3a-210">The environment can be set to any value.</span></span> <span data-ttu-id="acc3a-211">Os valores definidos pela estrutura incluem `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-211">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="acc3a-212">Os valores não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="acc3a-212">Values aren't case sensitive.</span></span> <span data-ttu-id="acc3a-213">Por padrão, o *Ambiente* é lido da variável de ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-213">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="acc3a-214">Ao usar o [Visual Studio](https://www.visualstudio.com/), variáveis de ambiente podem ser definidas no arquivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="acc3a-214">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="acc3a-215">Para obter mais informações, veja [Trabalhar com vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="acc3a-215">For more information, see [Work with multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="acc3a-218">Hospedando assemblies de inicialização</span><span class="sxs-lookup"><span data-stu-id="acc3a-218">Hosting Startup Assemblies</span></span>

<span data-ttu-id="acc3a-219">Define os assemblies de inicialização de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="acc3a-219">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="acc3a-220">**Chave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="acc3a-220">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="acc3a-221">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="acc3a-221">**Type**: *string*</span></span>  
<span data-ttu-id="acc3a-222">**Padrão**: cadeia de caracteres vazia</span><span class="sxs-lookup"><span data-stu-id="acc3a-222">**Default**: Empty string</span></span>  
<span data-ttu-id="acc3a-223">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="acc3a-223">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="acc3a-224">**Variável de ambiente**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="acc3a-224">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="acc3a-225">Uma cadeia de caracteres delimitada por ponto e vírgula de assemblies de inicialização de hospedagem para carregamento na inicialização.</span><span class="sxs-lookup"><span data-stu-id="acc3a-225">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="acc3a-226">Este recurso é novo no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="acc3a-226">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="acc3a-227">Embora o valor padrão da configuração seja uma cadeia de caracteres vazia, os assemblies de inicialização de hospedagem sempre incluem o assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="acc3a-227">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="acc3a-228">Quando assemblies de inicialização de hospedagem são fornecidos, eles são adicionados ao assembly do aplicativo para carregamento quando o aplicativo compilar seus serviços comuns durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="acc3a-228">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-229">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-229">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-230">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-230">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="acc3a-231">Este recurso não está disponível no ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="acc3a-231">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="acc3a-232">Preferir URLs de hospedagem</span><span class="sxs-lookup"><span data-stu-id="acc3a-232">Prefer Hosting URLs</span></span>

<span data-ttu-id="acc3a-233">Indica se o host deve escutar as URLs configuradas com o `WebHostBuilder` em vez daquelas configuradas com a implementação `IServer`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-233">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="acc3a-234">**Chave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="acc3a-234">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="acc3a-235">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="acc3a-235">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="acc3a-236">**Padrão**: true</span><span class="sxs-lookup"><span data-stu-id="acc3a-236">**Default**: true</span></span>  
<span data-ttu-id="acc3a-237">**Definido usando**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="acc3a-237">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="acc3a-238">**Variável de ambiente**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="acc3a-238">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="acc3a-239">Este recurso é novo no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="acc3a-239">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-240">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-240">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="acc3a-242">Este recurso não está disponível no ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="acc3a-242">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="acc3a-243">Impedir inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="acc3a-243">Prevent Hosting Startup</span></span>

<span data-ttu-id="acc3a-244">Impede o carregamento automático de assemblies de inicialização de hospedagem, incluindo assemblies de inicialização de hospedagem configurados pelo assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="acc3a-244">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="acc3a-245">Confira [Adicionar recursos do aplicativo usando uma configuração específica da plataforma](xref:host-and-deploy/platform-specific-configuration) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="acc3a-245">See [Add app features using a platform-specific configuration](xref:host-and-deploy/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="acc3a-246">**Chave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="acc3a-246">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="acc3a-247">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="acc3a-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="acc3a-248">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="acc3a-248">**Default**: false</span></span>  
<span data-ttu-id="acc3a-249">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="acc3a-249">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="acc3a-250">**Variável de ambiente**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="acc3a-250">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="acc3a-251">Este recurso é novo no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="acc3a-251">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="acc3a-254">Este recurso não está disponível no ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="acc3a-254">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="acc3a-255">URLs de servidor</span><span class="sxs-lookup"><span data-stu-id="acc3a-255">Server URLs</span></span>

<span data-ttu-id="acc3a-256">Indica os endereços IP ou endereços de host com portas e protocolos que o servidor deve escutar para solicitações.</span><span class="sxs-lookup"><span data-stu-id="acc3a-256">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="acc3a-257">**Chave**: urls</span><span class="sxs-lookup"><span data-stu-id="acc3a-257">**Key**: urls</span></span>  
<span data-ttu-id="acc3a-258">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="acc3a-258">**Type**: *string*</span></span>  
<span data-ttu-id="acc3a-259">**Padrão**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="acc3a-259">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="acc3a-260">**Definido usando**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="acc3a-260">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="acc3a-261">**Variável de ambiente**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="acc3a-261">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="acc3a-262">Defina como uma lista separada por ponto e vírgula (;) de prefixos de URL aos quais o servidor deve responder.</span><span class="sxs-lookup"><span data-stu-id="acc3a-262">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="acc3a-263">Por exemplo, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-263">For example, `http://localhost:123`.</span></span> <span data-ttu-id="acc3a-264">Use "\*" para indicar que o servidor deve escutar solicitações em qualquer endereço IP ou nome do host usando a porta e o protocolo especificados (por exemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="acc3a-264">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="acc3a-265">O protocolo (`http://` ou `https://`) deve ser incluído com cada URL.</span><span class="sxs-lookup"><span data-stu-id="acc3a-265">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="acc3a-266">Os formatos compatíveis variam dependendo dos servidores.</span><span class="sxs-lookup"><span data-stu-id="acc3a-266">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-267">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-267">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="acc3a-268">O Kestrel tem sua própria API de configuração de ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="acc3a-268">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="acc3a-269">Para obter mais informações, consulte [Implementação do servidor Web Kestrel no ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="acc3a-269">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-270">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-270">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="acc3a-271">Tempo limite de desligamento</span><span class="sxs-lookup"><span data-stu-id="acc3a-271">Shutdown Timeout</span></span>

<span data-ttu-id="acc3a-272">Especifica o tempo de espera para o desligamento do host da Web.</span><span class="sxs-lookup"><span data-stu-id="acc3a-272">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="acc3a-273">**Chave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="acc3a-273">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="acc3a-274">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="acc3a-274">**Type**: *int*</span></span>  
<span data-ttu-id="acc3a-275">**Padrão**: 5</span><span class="sxs-lookup"><span data-stu-id="acc3a-275">**Default**: 5</span></span>  
<span data-ttu-id="acc3a-276">**Definido usando**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="acc3a-276">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="acc3a-277">**Variável de ambiente**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="acc3a-277">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="acc3a-278">Embora a chave aceite um *int* com `UseSetting` (por exemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), o método de extensão [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) usa um [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="acc3a-278">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="acc3a-279">Este recurso é novo no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="acc3a-279">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="acc3a-280">Durante o período de tempo limite, a hospedagem:</span><span class="sxs-lookup"><span data-stu-id="acc3a-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="acc3a-281">Dispara [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="acc3a-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="acc3a-282">Tenta parar os serviços hospedados, registrando em log os erros dos serviços que falham ao parar.</span><span class="sxs-lookup"><span data-stu-id="acc3a-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="acc3a-283">Se o período de tempo limite expirar antes que todos os serviços hospedados parem, os serviços ativos restantes serão parados quando o aplicativo for desligado.</span><span class="sxs-lookup"><span data-stu-id="acc3a-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="acc3a-284">Os serviços serão parados mesmo se ainda não tiverem concluído o processamento.</span><span class="sxs-lookup"><span data-stu-id="acc3a-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="acc3a-285">Se os serviços exigirem mais tempo para parar, aumente o tempo limite.</span><span class="sxs-lookup"><span data-stu-id="acc3a-285">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-286">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-286">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-287">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-287">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="acc3a-288">Este recurso não está disponível no ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="acc3a-288">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="acc3a-289">Assembly de inicialização</span><span class="sxs-lookup"><span data-stu-id="acc3a-289">Startup Assembly</span></span>

<span data-ttu-id="acc3a-290">Determina o assembly para pesquisar pela classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-290">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="acc3a-291">**Chave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="acc3a-291">**Key**: startupAssembly</span></span>  
<span data-ttu-id="acc3a-292">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="acc3a-292">**Type**: *string*</span></span>  
<span data-ttu-id="acc3a-293">**Padrão**: o assembly do aplicativo</span><span class="sxs-lookup"><span data-stu-id="acc3a-293">**Default**: The app's assembly</span></span>  
<span data-ttu-id="acc3a-294">**Definido usando**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="acc3a-294">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="acc3a-295">**Variável de ambiente**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="acc3a-295">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="acc3a-296">O assembly por nome (`string`) ou por tipo (`TStartup`) pode ser referenciado.</span><span class="sxs-lookup"><span data-stu-id="acc3a-296">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="acc3a-297">Se vários métodos `UseStartup` forem chamados, o último terá precedência.</span><span class="sxs-lookup"><span data-stu-id="acc3a-297">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-299">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-299">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="acc3a-300">Raiz da Web</span><span class="sxs-lookup"><span data-stu-id="acc3a-300">Web Root</span></span>

<span data-ttu-id="acc3a-301">Define o caminho relativo para os ativos estáticos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="acc3a-301">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="acc3a-302">**Chave**: webroot</span><span class="sxs-lookup"><span data-stu-id="acc3a-302">**Key**: webroot</span></span>  
<span data-ttu-id="acc3a-303">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="acc3a-303">**Type**: *string*</span></span>  
<span data-ttu-id="acc3a-304">**Padrão**: se não for especificado, o padrão será "(Raiz do conteúdo)/wwwroot", se o caminho existir.</span><span class="sxs-lookup"><span data-stu-id="acc3a-304">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="acc3a-305">Se o caminho não existir, um provedor de arquivo não operacional será usado.</span><span class="sxs-lookup"><span data-stu-id="acc3a-305">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="acc3a-306">**Definido usando**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="acc3a-306">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="acc3a-307">**Variável de ambiente**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="acc3a-307">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-309">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-309">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="acc3a-310">Substituindo a configuração</span><span class="sxs-lookup"><span data-stu-id="acc3a-310">Overriding configuration</span></span>

<span data-ttu-id="acc3a-311">Use [Configuração](xref:fundamentals/configuration/index) para configurar o host.</span><span class="sxs-lookup"><span data-stu-id="acc3a-311">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="acc3a-312">No exemplo a seguir, a configuração do host é especificada, opcionalmente, em um arquivo *hosting.json*.</span><span class="sxs-lookup"><span data-stu-id="acc3a-312">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="acc3a-313">Qualquer configuração carregada do arquivo *hosting.json* pode ser substituída por argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="acc3a-313">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="acc3a-314">A configuração interna (no `config`) é usada para configurar o host com `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-314">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-315">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-315">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="acc3a-316">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="acc3a-316">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="acc3a-317">Substituição da configuração fornecida por `UseUrls` pela configuração de *hosting.json* primeiro e pela configuração de argumento da linha de comando depois:</span><span class="sxs-lookup"><span data-stu-id="acc3a-317">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="acc3a-319">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="acc3a-319">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="acc3a-320">Substituição da configuração fornecida por `UseUrls` pela configuração de *hosting.json* primeiro e pela configuração de argumento da linha de comando depois:</span><span class="sxs-lookup"><span data-stu-id="acc3a-320">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="acc3a-321">Atualmente, o método de extensão [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) não é capaz de analisar uma seção de configuração retornada por `GetSection` (por exemplo, `.UseConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="acc3a-321">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="acc3a-322">O método `GetSection` filtra as chaves de configuração da seção solicitada, mas deixa o nome da seção nas chaves (por exemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="acc3a-322">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="acc3a-323">O método `UseConfiguration` espera que as chaves correspondam às chaves `WebHostBuilder` (por exemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="acc3a-323">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="acc3a-324">A presença do nome da seção nas chaves impede que os valores da seção configurem o host.</span><span class="sxs-lookup"><span data-stu-id="acc3a-324">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="acc3a-325">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="acc3a-325">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="acc3a-326">Para obter mais informações e soluções alternativas, consulte [Passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="acc3a-326">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="acc3a-327">Para especificar o host executado em uma URL específica, o valor desejado pode ser passado em um prompt de comando ao executar [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="acc3a-327">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="acc3a-328">O argumento de linha de comando substitui o valor `urls` do arquivo *hosting.json* e o servidor escuta na porta 8080:</span><span class="sxs-lookup"><span data-stu-id="acc3a-328">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="acc3a-329">Iniciando o host</span><span class="sxs-lookup"><span data-stu-id="acc3a-329">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="acc3a-330">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-330">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="acc3a-331">**Executar**</span><span class="sxs-lookup"><span data-stu-id="acc3a-331">**Run**</span></span>

<span data-ttu-id="acc3a-332">O método `Run` inicia o aplicativo Web e bloqueia o thread de chamada até que o host seja desligado:</span><span class="sxs-lookup"><span data-stu-id="acc3a-332">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="acc3a-333">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="acc3a-333">**Start**</span></span>

<span data-ttu-id="acc3a-334">Execute o host sem bloqueio, chamando seu método `Start`:</span><span class="sxs-lookup"><span data-stu-id="acc3a-334">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="acc3a-335">Se uma lista de URLs for passada para o método `Start`, ele escutará nas URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="acc3a-335">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="acc3a-336">O aplicativo pode inicializar e iniciar um novo host usando os padrões pré-configurados de `CreateDefaultBuilder`, usando um método estático conveniente.</span><span class="sxs-lookup"><span data-stu-id="acc3a-336">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="acc3a-337">Esses métodos iniciam o servidor sem uma saída do console e com [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) e aguardam uma quebra (Ctrl-C/SIGINT ou SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="acc3a-337">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="acc3a-338">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="acc3a-338">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="acc3a-339">Inicie com um `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="acc3a-339">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="acc3a-340">Faça uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Olá, Mundo!"</span><span class="sxs-lookup"><span data-stu-id="acc3a-340">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="acc3a-341">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="acc3a-341">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="acc3a-342">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="acc3a-342">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="acc3a-343">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="acc3a-343">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="acc3a-344">Inicie com uma URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="acc3a-344">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="acc3a-345">Produz o mesmo resultado que **Start(RequestDelegate app)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-345">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="acc3a-346">**Start(Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="acc3a-346">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="acc3a-347">Use uma instância de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar o middleware de roteamento:</span><span class="sxs-lookup"><span data-stu-id="acc3a-347">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="acc3a-348">Use as seguintes solicitações de navegador com o exemplo:</span><span class="sxs-lookup"><span data-stu-id="acc3a-348">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="acc3a-349">Solicitação</span><span class="sxs-lookup"><span data-stu-id="acc3a-349">Request</span></span>                                    | <span data-ttu-id="acc3a-350">Resposta</span><span class="sxs-lookup"><span data-stu-id="acc3a-350">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="acc3a-351">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="acc3a-351">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="acc3a-352">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="acc3a-352">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="acc3a-353">Gera uma exceção com a cadeia de caracteres "ooops!"</span><span class="sxs-lookup"><span data-stu-id="acc3a-353">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="acc3a-354">Gera uma exceção com a cadeia de caracteres "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="acc3a-354">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="acc3a-355">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="acc3a-355">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="acc3a-356">Olá, Mundo!</span><span class="sxs-lookup"><span data-stu-id="acc3a-356">Hello World!</span></span>                             |

<span data-ttu-id="acc3a-357">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="acc3a-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="acc3a-358">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="acc3a-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="acc3a-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="acc3a-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="acc3a-360">Use uma URL e uma instância de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="acc3a-360">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="acc3a-361">Produz o mesmo resultado que **Start(Action<IRouteBuilder> routeBuilder)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-361">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="acc3a-362">**StartWith(Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="acc3a-362">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="acc3a-363">Forneça um delegado para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="acc3a-363">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="acc3a-364">Faça uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Olá, Mundo!"</span><span class="sxs-lookup"><span data-stu-id="acc3a-364">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="acc3a-365">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="acc3a-365">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="acc3a-366">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="acc3a-366">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="acc3a-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="acc3a-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="acc3a-368">Forneça um delegado e uma URL para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="acc3a-368">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="acc3a-369">Produz o mesmo resultado que **StartWith(Action<IApplicationBuilder> app)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-369">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="acc3a-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="acc3a-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="acc3a-371">**Executar**</span><span class="sxs-lookup"><span data-stu-id="acc3a-371">**Run**</span></span>

<span data-ttu-id="acc3a-372">O método `Run` inicia o aplicativo Web e bloqueia o thread de chamada até que o host seja desligado:</span><span class="sxs-lookup"><span data-stu-id="acc3a-372">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="acc3a-373">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="acc3a-373">**Start**</span></span>

<span data-ttu-id="acc3a-374">Execute o host sem bloqueio, chamando seu método `Start`:</span><span class="sxs-lookup"><span data-stu-id="acc3a-374">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="acc3a-375">Se uma lista de URLs for passada para o método `Start`, ele escutará nas URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="acc3a-375">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="acc3a-376">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="acc3a-376">IHostingEnvironment interface</span></span>

<span data-ttu-id="acc3a-377">A [interface IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornece informações sobre o ambiente de hospedagem na Web do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="acc3a-377">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="acc3a-378">Use a [injeção de construtor](xref:fundamentals/dependency-injection) para obter o `IHostingEnvironment` para usar suas propriedades e métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="acc3a-378">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="acc3a-379">Uma [abordagem baseada em convenção](xref:fundamentals/environments#startup-conventions) pode ser usada para configurar o aplicativo na inicialização com base no ambiente.</span><span class="sxs-lookup"><span data-stu-id="acc3a-379">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="acc3a-380">Como alternativa, injete o `IHostingEnvironment` no construtor `Startup` para uso em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="acc3a-380">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="acc3a-381">Além do método de extensão `IsDevelopment`, `IHostingEnvironment` oferece os métodos `IsStaging`, `IsProduction` e `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-381">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="acc3a-382">Confira [trabalhar com vários ambientes](xref:fundamentals/environments) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="acc3a-382">See [Work with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="acc3a-383">O serviço `IHostingEnvironment` também pode ser injetado diretamente no método `Configure` para configurar o pipeline de processamento:</span><span class="sxs-lookup"><span data-stu-id="acc3a-383">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="acc3a-384">`IHostingEnvironment` pode ser injetado no método `Invoke` ao criar um [middleware](xref:fundamentals/middleware/index#writing-middleware) personalizado:</span><span class="sxs-lookup"><span data-stu-id="acc3a-384">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="acc3a-385">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="acc3a-385">IApplicationLifetime interface</span></span>

<span data-ttu-id="acc3a-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite atividades pós-inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="acc3a-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="acc3a-387">Três propriedades na interface são tokens de cancelamento usados para registrar métodos `Action` que definem eventos de inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="acc3a-387">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="acc3a-388">Token de cancelamento</span><span class="sxs-lookup"><span data-stu-id="acc3a-388">Cancellation Token</span></span>    | <span data-ttu-id="acc3a-389">Acionado quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="acc3a-389">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="acc3a-390">O host foi iniciado totalmente.</span><span class="sxs-lookup"><span data-stu-id="acc3a-390">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="acc3a-391">O host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="acc3a-391">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="acc3a-392">Solicitações ainda podem estar sendo processadas.</span><span class="sxs-lookup"><span data-stu-id="acc3a-392">Requests may still be processing.</span></span> <span data-ttu-id="acc3a-393">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="acc3a-393">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="acc3a-394">O host está concluindo um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="acc3a-394">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="acc3a-395">Todas as solicitações devem ser processadas.</span><span class="sxs-lookup"><span data-stu-id="acc3a-395">All requests should be processed.</span></span> <span data-ttu-id="acc3a-396">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="acc3a-396">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="acc3a-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) solicita o término do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="acc3a-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="acc3a-398">A classe a seguir usa `StopApplication` para desligar normalmente um aplicativo quando o método `Shutdown` da classe é chamado:</span><span class="sxs-lookup"><span data-stu-id="acc3a-398">The following class uses `StopApplication` to gracefully shutdown an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a><span data-ttu-id="acc3a-399">Validação de escopo</span><span class="sxs-lookup"><span data-stu-id="acc3a-399">Scope validation</span></span>

<span data-ttu-id="acc3a-400">No ASP.NET Core 2.0 ou posterior, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) define [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) como `true` quando o ambiente do aplicativo é de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="acc3a-400">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="acc3a-401">Quando `ValidateScopes` está definido como `true`, o provedor de serviço padrão executa verificações para saber se:</span><span class="sxs-lookup"><span data-stu-id="acc3a-401">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="acc3a-402">Os serviços com escopo não são resolvidos direta ou indiretamente pelo provedor de serviço raiz.</span><span class="sxs-lookup"><span data-stu-id="acc3a-402">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="acc3a-403">Os serviços com escopo não são injetados direta ou indiretamente em singletons.</span><span class="sxs-lookup"><span data-stu-id="acc3a-403">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="acc3a-404">O provedor de serviços raiz é criado quando [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) é chamado.</span><span class="sxs-lookup"><span data-stu-id="acc3a-404">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="acc3a-405">O tempo de vida do provedor de serviço raiz corresponde ao tempo de vida do aplicativo/servidor quando o provedor começa com o aplicativo e é descartado quando o aplicativo é desligado.</span><span class="sxs-lookup"><span data-stu-id="acc3a-405">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="acc3a-406">Os serviços com escopo são descartados pelo contêiner que os criou.</span><span class="sxs-lookup"><span data-stu-id="acc3a-406">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="acc3a-407">Se um serviço com escopo é criado no contêiner raiz, o tempo de vida do serviço é promovido efetivamente para singleton, porque ele só é descartado pelo contêiner raiz quando o aplicativo/servidor é desligado.</span><span class="sxs-lookup"><span data-stu-id="acc3a-407">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="acc3a-408">A validação dos escopos de serviço detecta essas situações quando `BuildServiceProvider` é chamado.</span><span class="sxs-lookup"><span data-stu-id="acc3a-408">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="acc3a-409">Para que os escopos sempre sejam validados, incluindo no ambiente de produção, configure [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) com [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) no construtor do host:</span><span class="sxs-lookup"><span data-stu-id="acc3a-409">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="acc3a-410">Solução de problemas de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="acc3a-410">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="acc3a-411">**Aplica-se somente ao ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="acc3a-411">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="acc3a-412">Um host pode ser compilado injetando `IStartup` diretamente no contêiner de injeção de dependência em vez de chamar `UseStartup` ou `Configure`:</span><span class="sxs-lookup"><span data-stu-id="acc3a-412">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="acc3a-413">Se o host for compilado dessa maneira, o seguinte erro poderá ocorrer:</span><span class="sxs-lookup"><span data-stu-id="acc3a-413">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="acc3a-414">Isso ocorre porque o [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (do assembly atual) é necessário para verificar se há `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-414">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="acc3a-415">Se o aplicativo injetar manualmente `IStartup` no contêiner de injeção de dependência, adicione a seguinte chamada para `WebHostBuilder` com o nome do assembly especificado:</span><span class="sxs-lookup"><span data-stu-id="acc3a-415">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="acc3a-416">Como alternativa, adicione um `Configure` fictício ao `WebHostBuilder`, que define o `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="acc3a-416">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="acc3a-417">**Observação**: isso só é necessário com a versão do ASP.NET Core 2.0 e somente quando o aplicativo não chama `UseStartup` ou `Configure`.</span><span class="sxs-lookup"><span data-stu-id="acc3a-417">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="acc3a-418">Para obter mais informações, consulte [Comunicado: Microsoft.Extensions.PlatformAbstractions foi removido (comentário)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e o [Exemplo de StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="acc3a-418">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="acc3a-419">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="acc3a-419">Additional resources</span></span>

* [<span data-ttu-id="acc3a-420">Hospedar no Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="acc3a-420">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="acc3a-421">Hospedar em Linux com o Nginx</span><span class="sxs-lookup"><span data-stu-id="acc3a-421">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="acc3a-422">Hospedar em Linux com o Apache</span><span class="sxs-lookup"><span data-stu-id="acc3a-422">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="acc3a-423">Hospedar em um serviço Windows</span><span class="sxs-lookup"><span data-stu-id="acc3a-423">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
