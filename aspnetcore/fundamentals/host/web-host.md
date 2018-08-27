---
title: Host da Web do ASP.NET Core
author: guardrex
description: Saiba mais sobre o host da Web no ASP.NET Core, que é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: dfef2bf21f325f11d147379f75a8d81a8bd05eec
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41886740"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="d4e93-103">Host da Web do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4e93-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="d4e93-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d4e93-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d4e93-105">Aplicativos ASP.NET Core configuram e inicializam um *host*.</span><span class="sxs-lookup"><span data-stu-id="d4e93-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="d4e93-106">O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d4e93-107">No mínimo, o host configura um servidor e um pipeline de processamento de solicitações.</span><span class="sxs-lookup"><span data-stu-id="d4e93-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="d4e93-108">Este tópico aborda o Host da Web ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), que é útil para hospedagem de aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="d4e93-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="d4e93-109">Para cobertura do Host Genérico .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), veja <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="d4e93-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="d4e93-110">Configurar um host</span><span class="sxs-lookup"><span data-stu-id="d4e93-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d4e93-111">Crie um host usando uma instância do [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d4e93-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="d4e93-112">Normalmente, isso é feito no ponto de entrada do aplicativo, o método `Main`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d4e93-113">Em modelos de projeto, `Main` está localizado em *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="d4e93-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="d4e93-114">Um *Program.cs* típico chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para começar a configurar um host:</span><span class="sxs-lookup"><span data-stu-id="d4e93-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="d4e93-115">`CreateDefaultBuilder` executa as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="d4e93-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="d4e93-116">Configura o [Kestrel](xref:fundamentals/servers/kestrel) como o servidor Web e configura o servidor usando provedores de configuração de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="d4e93-117">Para as opções padrão do Kestrel, veja <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="d4e93-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="d4e93-118">Define a raiz do conteúdo como o caminho retornado por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="d4e93-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="d4e93-119">Carrega a [configuração do host](#host-configuration-values) de:</span><span class="sxs-lookup"><span data-stu-id="d4e93-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="d4e93-120">Variáveis de ambiente prefixadas com `ASPNETCORE_` (por exemplo, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="d4e93-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="d4e93-121">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d4e93-121">Command-line arguments.</span></span>
* <span data-ttu-id="d4e93-122">Carrega a configuração do aplicativo de:</span><span class="sxs-lookup"><span data-stu-id="d4e93-122">Loads app configuration from:</span></span>
  * <span data-ttu-id="d4e93-123">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d4e93-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="d4e93-124">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d4e93-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="d4e93-125">[Segredos do usuário](xref:security/app-secrets) quando o aplicativo é executado no ambiente `Development` usando o assembly de entrada.</span><span class="sxs-lookup"><span data-stu-id="d4e93-125">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="d4e93-126">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="d4e93-126">Environment variables.</span></span>
  * <span data-ttu-id="d4e93-127">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d4e93-127">Command-line arguments.</span></span>
* <span data-ttu-id="d4e93-128">Configura o [registro em log](xref:fundamentals/logging/index) para a saída do console e de depuração.</span><span class="sxs-lookup"><span data-stu-id="d4e93-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="d4e93-129">O registro em log inclui regras de [filtragem de log](xref:fundamentals/logging/index#log-filtering) especificadas em uma seção de configuração de registro em log de um arquivo *appsettings.json* ou *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d4e93-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="d4e93-130">Quando executado por trás do IIS, permite a [integração de IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="d4e93-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="d4e93-131">Configura o caminho base e a porta que o servidor escuta ao usar o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d4e93-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="d4e93-132">O módulo cria um proxy reverso entre o IIS e o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4e93-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="d4e93-133">Também configura o aplicativo para [capturar erros de inicialização](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="d4e93-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="d4e93-134">Para as opções padrão do IIS, veja <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="d4e93-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="d4e93-135">Definirá [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) como `true` se o ambiente do aplicativo for de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="d4e93-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="d4e93-136">Para obter mais informações, confira [Validação de escopo](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="d4e93-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="d4e93-137">A configuração definida por `CreateDefaultBuilder` pode ser substituída e aumentada por [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) e outros métodos, bem como os métodos de extensão de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d4e93-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="d4e93-138">Veja a seguir alguns exemplos:</span><span class="sxs-lookup"><span data-stu-id="d4e93-138">A few examples follow:</span></span>

* <span data-ttu-id="d4e93-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) é usado para especificar `IConfiguration` adicionais para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="d4e93-140">A seguinte chamada de `ConfigureAppConfiguration` adiciona um delegado para incluir a configuração do aplicativo no arquivo *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="d4e93-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="d4e93-141">`ConfigureAppConfiguration` pode ser chamado várias vezes.</span><span class="sxs-lookup"><span data-stu-id="d4e93-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="d4e93-142">Observe que essa configuração não se aplica ao host (por exemplo, URLs de servidor ou de ambiente).</span><span class="sxs-lookup"><span data-stu-id="d4e93-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="d4e93-143">Consulte a seção [Valores de configuração de Host](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="d4e93-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="d4e93-144">A seguinte chamada de `ConfigureLogging` adiciona um delegado para configurar o nível de log mínimo ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) como [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="d4e93-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="d4e93-145">Essa configuração substitui as configurações em *appsettings.Development.json* (`LogLevel.Debug`) e *appsettings.Production.json* (`LogLevel.Error`) configuradas por `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d4e93-146">`ConfigureLogging` pode ser chamado várias vezes.</span><span class="sxs-lookup"><span data-stu-id="d4e93-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

* <span data-ttu-id="d4e93-147">A seguinte chamada a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) substitui o padrão [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) de 30.000.000 bytes estabelecido quando Kestrel foi configurado por `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d4e93-147">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

<span data-ttu-id="d4e93-148">A *raiz do conteúdo* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="d4e93-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d4e93-149">Quando o aplicativo é iniciado na pasta raiz do projeto, essa pasta é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-149">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d4e93-150">Esse é o padrão usado no [Visual Studio](https://www.visualstudio.com/) e nos [novos modelos dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d4e93-150">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d4e93-151">Para obter mais informações sobre a configuração de aplicativo, veja <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d4e93-151">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="d4e93-152">Como uma alternativa ao uso do método `CreateDefaultBuilder` estático, criar um host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) é uma abordagem compatível com o ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="d4e93-152">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="d4e93-153">Para obter mais informações, consulte a guia do ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d4e93-153">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d4e93-154">Crie um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d4e93-154">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="d4e93-155">A criação de um host normalmente é feita no ponto de entrada do aplicativo, o método `Main`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-155">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d4e93-156">Em modelos de projeto, `Main` está localizado em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d4e93-156">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="d4e93-157">`WebHostBuilder` requer um [servidor que implementa IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="d4e93-157">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="d4e93-158">Os servidores internos são [Kestrel](xref:fundamentals/servers/kestrel) e [HTTP.sys](xref:fundamentals/servers/httpsys) (antes do lançamento do ASP.NET Core 2.0, HTTP.sys era chamado de [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="d4e93-158">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="d4e93-159">Neste exemplo, o [método de extensão UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica o servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4e93-159">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="d4e93-160">A *raiz do conteúdo* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="d4e93-160">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d4e93-161">A raiz de conteúdo padrão é obtida para `UseContentRoot` por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="d4e93-161">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="d4e93-162">Quando o aplicativo é iniciado na pasta raiz do projeto, essa pasta é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-162">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d4e93-163">Esse é o padrão usado no [Visual Studio](https://www.visualstudio.com/) e nos [novos modelos dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d4e93-163">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d4e93-164">Para usar o IIS como um proxy reverso, chame [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte da compilação do host.</span><span class="sxs-lookup"><span data-stu-id="d4e93-164">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="d4e93-165">`UseIISIntegration` não configura um *servidor* como [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) o faz.</span><span class="sxs-lookup"><span data-stu-id="d4e93-165">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="d4e93-166">`UseIISIntegration` configura o caminho base e a porta que o servidor escuta ao usar o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para criar um proxy reverso entre o Kestrel e o IIS.</span><span class="sxs-lookup"><span data-stu-id="d4e93-166">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="d4e93-167">Para usar o IIS com o ASP.NET Core, tanto `UseKestrel` quanto `UseIISIntegration` precisam ser especificados.</span><span class="sxs-lookup"><span data-stu-id="d4e93-167">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="d4e93-168">`UseIISIntegration` é ativado somente quando é executado por trás do IIS ou do IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d4e93-168">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="d4e93-169">Para obter mais informações, consulte <xref:fundamentals/servers/aspnet-core-module> e <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="d4e93-169">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="d4e93-170">Uma implementação mínima que configura um host (e um aplicativo ASP.NET Core) inclui a especificação de um servidor e a configuração do pipeline de solicitações do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="d4e93-170">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

::: moniker-end

<span data-ttu-id="d4e93-171">Ao configurar um host, os métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) podem ser fornecidos.</span><span class="sxs-lookup"><span data-stu-id="d4e93-171">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="d4e93-172">Se uma classe `Startup` for especificada, ela deverá definir um método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-172">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="d4e93-173">Para obter mais informações, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d4e93-173">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="d4e93-174">Diversas chamadas para `ConfigureServices` são acrescentadas umas às outras.</span><span class="sxs-lookup"><span data-stu-id="d4e93-174">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="d4e93-175">Diversas chamadas para `Configure` ou `UseStartup` no `WebHostBuilder` substituem configurações anteriores.</span><span class="sxs-lookup"><span data-stu-id="d4e93-175">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d4e93-176">Valores de configuração do host</span><span class="sxs-lookup"><span data-stu-id="d4e93-176">Host configuration values</span></span>

<span data-ttu-id="d4e93-177">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) conta com as seguintes abordagens para definir os valores de configuração do host:</span><span class="sxs-lookup"><span data-stu-id="d4e93-177">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="d4e93-178">Configuração do construtor do host, que inclui variáveis de ambiente com o formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-178">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="d4e93-179">Por exemplo, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-179">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="d4e93-180">Extensões como [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) e [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (consulte a seção [Configuração de substituição](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="d4e93-180">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="d4e93-181">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e a chave associada.</span><span class="sxs-lookup"><span data-stu-id="d4e93-181">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="d4e93-182">Ao definir um valor com `UseSetting`, o valor é definido como uma cadeia de caracteres, independentemente do tipo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-182">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="d4e93-183">O host usa a opção que define um valor por último.</span><span class="sxs-lookup"><span data-stu-id="d4e93-183">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="d4e93-184">Para obter mais informações, veja [Substituir configuração](#override-configuration) na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="d4e93-184">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="d4e93-185">Chave do Aplicativo (Nome)</span><span class="sxs-lookup"><span data-stu-id="d4e93-185">Application Key (Name)</span></span>

<span data-ttu-id="d4e93-186">A propriedade [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) é definida automaticamente quando [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) ou [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) é chamado durante a construção do host.</span><span class="sxs-lookup"><span data-stu-id="d4e93-186">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="d4e93-187">O valor é definido para o nome do assembly que contém o ponto de entrada do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-187">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="d4e93-188">Para definir o valor explicitamente, use o [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="d4e93-188">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="d4e93-189">**Chave**: applicationName</span><span class="sxs-lookup"><span data-stu-id="d4e93-189">**Key**: applicationName</span></span>  
<span data-ttu-id="d4e93-190">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d4e93-190">**Type**: *string*</span></span>  
<span data-ttu-id="d4e93-191">**Padrão**: o nome do assembly que contém o ponto de entrada do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-191">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="d4e93-192">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d4e93-192">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d4e93-193">**Variável de ambiente**: `ASPNETCORE_APPLICATIONKEY`</span><span class="sxs-lookup"><span data-stu-id="d4e93-193">**Environment variable**: `ASPNETCORE_APPLICATIONKEY`</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a><span data-ttu-id="d4e93-194">Capturar erros de inicialização</span><span class="sxs-lookup"><span data-stu-id="d4e93-194">Capture Startup Errors</span></span>

<span data-ttu-id="d4e93-195">Esta configuração controla a captura de erros de inicialização.</span><span class="sxs-lookup"><span data-stu-id="d4e93-195">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="d4e93-196">**Chave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="d4e93-196">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="d4e93-197">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="d4e93-197">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d4e93-198">**Padrão**: o padrão é `false`, a menos que o aplicativo seja executado com o Kestrel por trás do IIS, em que o padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-198">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="d4e93-199">**Definido usando**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="d4e93-199">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="d4e93-200">**Variável de ambiente**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="d4e93-200">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="d4e93-201">Quando `false`, erros durante a inicialização resultam no encerramento do host.</span><span class="sxs-lookup"><span data-stu-id="d4e93-201">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="d4e93-202">Quando `true`, o host captura exceções durante a inicialização e tenta iniciar o servidor.</span><span class="sxs-lookup"><span data-stu-id="d4e93-202">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a><span data-ttu-id="d4e93-203">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="d4e93-203">Content Root</span></span>

<span data-ttu-id="d4e93-204">Essa configuração determina onde o ASP.NET Core começa a procurar por arquivos de conteúdo, como exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="d4e93-204">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="d4e93-205">**Chave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="d4e93-205">**Key**: contentRoot</span></span>  
<span data-ttu-id="d4e93-206">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d4e93-206">**Type**: *string*</span></span>  
<span data-ttu-id="d4e93-207">**Padrão**: o padrão é a pasta em que o assembly do aplicativo reside.</span><span class="sxs-lookup"><span data-stu-id="d4e93-207">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="d4e93-208">**Definido usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="d4e93-208">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="d4e93-209">**Variável de ambiente**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="d4e93-209">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="d4e93-210">A raiz do conteúdo também é usada como o caminho base para a [Configuração da raiz da Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="d4e93-210">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="d4e93-211">Se o caminho não existir, o host não será iniciado.</span><span class="sxs-lookup"><span data-stu-id="d4e93-211">If the path doesn't exist, the host fails to start.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a><span data-ttu-id="d4e93-212">Erros detalhados</span><span class="sxs-lookup"><span data-stu-id="d4e93-212">Detailed Errors</span></span>

<span data-ttu-id="d4e93-213">Determina se erros detalhados devem ser capturados.</span><span class="sxs-lookup"><span data-stu-id="d4e93-213">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="d4e93-214">**Chave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="d4e93-214">**Key**: detailedErrors</span></span>  
<span data-ttu-id="d4e93-215">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="d4e93-215">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d4e93-216">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="d4e93-216">**Default**: false</span></span>  
<span data-ttu-id="d4e93-217">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d4e93-217">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d4e93-218">**Variável de ambiente**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="d4e93-218">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="d4e93-219">Quando habilitado (ou quando o <a href="#environment">Ambiente</a> é definido como `Development`), o aplicativo captura exceções detalhadas.</span><span class="sxs-lookup"><span data-stu-id="d4e93-219">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a><span data-ttu-id="d4e93-220">Ambiente</span><span class="sxs-lookup"><span data-stu-id="d4e93-220">Environment</span></span>

<span data-ttu-id="d4e93-221">Define o ambiente do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-221">Sets the app's environment.</span></span>

<span data-ttu-id="d4e93-222">**Chave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="d4e93-222">**Key**: environment</span></span>  
<span data-ttu-id="d4e93-223">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d4e93-223">**Type**: *string*</span></span>  
<span data-ttu-id="d4e93-224">**Padrão**: Production</span><span class="sxs-lookup"><span data-stu-id="d4e93-224">**Default**: Production</span></span>  
<span data-ttu-id="d4e93-225">**Definido usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="d4e93-225">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="d4e93-226">**Variável de ambiente**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="d4e93-226">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="d4e93-227">O ambiente pode ser definido como qualquer valor.</span><span class="sxs-lookup"><span data-stu-id="d4e93-227">The environment can be set to any value.</span></span> <span data-ttu-id="d4e93-228">Os valores definidos pela estrutura incluem `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-228">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="d4e93-229">Os valores não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d4e93-229">Values aren't case sensitive.</span></span> <span data-ttu-id="d4e93-230">Por padrão, o *Ambiente* é lido da variável de ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-230">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d4e93-231">Ao usar o [Visual Studio](https://www.visualstudio.com/), variáveis de ambiente podem ser definidas no arquivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d4e93-231">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="d4e93-232">Para obter mais informações, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d4e93-232">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="d4e93-233">Hospedando assemblies de inicialização</span><span class="sxs-lookup"><span data-stu-id="d4e93-233">Hosting Startup Assemblies</span></span>

<span data-ttu-id="d4e93-234">Define os assemblies de inicialização de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-234">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="d4e93-235">**Chave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="d4e93-235">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="d4e93-236">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d4e93-236">**Type**: *string*</span></span>  
<span data-ttu-id="d4e93-237">**Padrão**: cadeia de caracteres vazia</span><span class="sxs-lookup"><span data-stu-id="d4e93-237">**Default**: Empty string</span></span>  
<span data-ttu-id="d4e93-238">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d4e93-238">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d4e93-239">**Variável de ambiente**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d4e93-239">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="d4e93-240">Uma cadeia de caracteres delimitada por ponto e vírgula de assemblies de inicialização de hospedagem para carregamento na inicialização.</span><span class="sxs-lookup"><span data-stu-id="d4e93-240">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="d4e93-241">Embora o valor padrão da configuração seja uma cadeia de caracteres vazia, os assemblies de inicialização de hospedagem sempre incluem o assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-241">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="d4e93-242">Quando assemblies de inicialização de hospedagem são fornecidos, eles são adicionados ao assembly do aplicativo para carregamento quando o aplicativo compilar seus serviços comuns durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="d4e93-242">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a><span data-ttu-id="d4e93-243">Porta HTTPS</span><span class="sxs-lookup"><span data-stu-id="d4e93-243">HTTPS Port</span></span>

<span data-ttu-id="d4e93-244">Defina a porta de redirecionamento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d4e93-244">Set the HTTPS redirect port.</span></span> <span data-ttu-id="d4e93-245">Uso em [aplicação de HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="d4e93-245">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="d4e93-246">**Chave**: https_port **Tipo**: *string*
**Padrão**: Um valor padrão não está definido.</span><span class="sxs-lookup"><span data-stu-id="d4e93-246">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="d4e93-247">**Definido usando**: `UseSetting`
**Variável de ambiente**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="d4e93-247">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="d4e93-248">Preferir URLs de hospedagem</span><span class="sxs-lookup"><span data-stu-id="d4e93-248">Prefer Hosting URLs</span></span>

<span data-ttu-id="d4e93-249">Indica se o host deve escutar as URLs configuradas com o `WebHostBuilder` em vez daquelas configuradas com a implementação `IServer`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-249">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="d4e93-250">**Chave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="d4e93-250">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="d4e93-251">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="d4e93-251">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d4e93-252">**Padrão**: true</span><span class="sxs-lookup"><span data-stu-id="d4e93-252">**Default**: true</span></span>  
<span data-ttu-id="d4e93-253">**Definido usando**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="d4e93-253">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="d4e93-254">**Variável de ambiente**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="d4e93-254">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="d4e93-255">Impedir inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="d4e93-255">Prevent Hosting Startup</span></span>

<span data-ttu-id="d4e93-256">Impede o carregamento automático de assemblies de inicialização de hospedagem, incluindo assemblies de inicialização de hospedagem configurados pelo assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-256">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="d4e93-257">Para obter mais informações, consulte <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d4e93-257">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="d4e93-258">**Chave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="d4e93-258">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="d4e93-259">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="d4e93-259">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d4e93-260">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="d4e93-260">**Default**: false</span></span>  
<span data-ttu-id="d4e93-261">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d4e93-261">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d4e93-262">**Variável de ambiente**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="d4e93-262">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="d4e93-263">URLs de servidor</span><span class="sxs-lookup"><span data-stu-id="d4e93-263">Server URLs</span></span>

<span data-ttu-id="d4e93-264">Indica os endereços IP ou endereços de host com portas e protocolos que o servidor deve escutar para solicitações.</span><span class="sxs-lookup"><span data-stu-id="d4e93-264">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="d4e93-265">**Chave**: urls</span><span class="sxs-lookup"><span data-stu-id="d4e93-265">**Key**: urls</span></span>  
<span data-ttu-id="d4e93-266">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d4e93-266">**Type**: *string*</span></span>  
<span data-ttu-id="d4e93-267">**Padrão**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="d4e93-267">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="d4e93-268">**Definido usando**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="d4e93-268">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="d4e93-269">**Variável de ambiente**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="d4e93-269">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="d4e93-270">Defina como uma lista separada por ponto e vírgula (;) de prefixos de URL aos quais o servidor deve responder.</span><span class="sxs-lookup"><span data-stu-id="d4e93-270">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="d4e93-271">Por exemplo, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-271">For example, `http://localhost:123`.</span></span> <span data-ttu-id="d4e93-272">Use "\*" para indicar que o servidor deve escutar solicitações em qualquer endereço IP ou nome do host usando a porta e o protocolo especificados (por exemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="d4e93-272">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="d4e93-273">O protocolo (`http://` ou `https://`) deve ser incluído com cada URL.</span><span class="sxs-lookup"><span data-stu-id="d4e93-273">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="d4e93-274">Os formatos compatíveis variam dependendo dos servidores.</span><span class="sxs-lookup"><span data-stu-id="d4e93-274">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="d4e93-275">O Kestrel tem sua própria API de configuração de ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="d4e93-275">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="d4e93-276">Para obter mais informações, consulte <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d4e93-276">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="d4e93-277">Tempo limite de desligamento</span><span class="sxs-lookup"><span data-stu-id="d4e93-277">Shutdown Timeout</span></span>

<span data-ttu-id="d4e93-278">Especifica o tempo de espera para o desligamento do host da Web.</span><span class="sxs-lookup"><span data-stu-id="d4e93-278">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="d4e93-279">**Chave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="d4e93-279">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="d4e93-280">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="d4e93-280">**Type**: *int*</span></span>  
<span data-ttu-id="d4e93-281">**Padrão**: 5</span><span class="sxs-lookup"><span data-stu-id="d4e93-281">**Default**: 5</span></span>  
<span data-ttu-id="d4e93-282">**Definido usando**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="d4e93-282">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="d4e93-283">**Variável de ambiente**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="d4e93-283">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="d4e93-284">Embora a chave aceite um *int* com `UseSetting` (por exemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), o método de extensão [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) usa um [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="d4e93-284">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="d4e93-285">Durante o período de tempo limite, a hospedagem:</span><span class="sxs-lookup"><span data-stu-id="d4e93-285">During the timeout period, hosting:</span></span>

* <span data-ttu-id="d4e93-286">Dispara [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="d4e93-286">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="d4e93-287">Tenta parar os serviços hospedados, registrando em log os erros dos serviços que falham ao parar.</span><span class="sxs-lookup"><span data-stu-id="d4e93-287">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="d4e93-288">Se o período de tempo limite expirar antes que todos os serviços hospedados parem, os serviços ativos restantes serão parados quando o aplicativo for desligado.</span><span class="sxs-lookup"><span data-stu-id="d4e93-288">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="d4e93-289">Os serviços serão parados mesmo se ainda não tiverem concluído o processamento.</span><span class="sxs-lookup"><span data-stu-id="d4e93-289">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="d4e93-290">Se os serviços exigirem mais tempo para parar, aumente o tempo limite.</span><span class="sxs-lookup"><span data-stu-id="d4e93-290">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="d4e93-291">Assembly de inicialização</span><span class="sxs-lookup"><span data-stu-id="d4e93-291">Startup Assembly</span></span>

<span data-ttu-id="d4e93-292">Determina o assembly para pesquisar pela classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-292">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="d4e93-293">**Chave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="d4e93-293">**Key**: startupAssembly</span></span>  
<span data-ttu-id="d4e93-294">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d4e93-294">**Type**: *string*</span></span>  
<span data-ttu-id="d4e93-295">**Padrão**: o assembly do aplicativo</span><span class="sxs-lookup"><span data-stu-id="d4e93-295">**Default**: The app's assembly</span></span>  
<span data-ttu-id="d4e93-296">**Definido usando**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="d4e93-296">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="d4e93-297">**Variável de ambiente**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="d4e93-297">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="d4e93-298">O assembly por nome (`string`) ou por tipo (`TStartup`) pode ser referenciado.</span><span class="sxs-lookup"><span data-stu-id="d4e93-298">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="d4e93-299">Se vários métodos `UseStartup` forem chamados, o último terá precedência.</span><span class="sxs-lookup"><span data-stu-id="d4e93-299">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a><span data-ttu-id="d4e93-300">Raiz da Web</span><span class="sxs-lookup"><span data-stu-id="d4e93-300">Web Root</span></span>

<span data-ttu-id="d4e93-301">Define o caminho relativo para os ativos estáticos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-301">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="d4e93-302">**Chave**: webroot</span><span class="sxs-lookup"><span data-stu-id="d4e93-302">**Key**: webroot</span></span>  
<span data-ttu-id="d4e93-303">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d4e93-303">**Type**: *string*</span></span>  
<span data-ttu-id="d4e93-304">**Padrão**: se não for especificado, o padrão será "(Raiz do conteúdo)/wwwroot", se o caminho existir.</span><span class="sxs-lookup"><span data-stu-id="d4e93-304">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="d4e93-305">Se o caminho não existir, um provedor de arquivo não operacional será usado.</span><span class="sxs-lookup"><span data-stu-id="d4e93-305">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="d4e93-306">**Definido usando**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="d4e93-306">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="d4e93-307">**Variável de ambiente**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="d4e93-307">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a><span data-ttu-id="d4e93-308">Substituir configuração</span><span class="sxs-lookup"><span data-stu-id="d4e93-308">Override configuration</span></span>

<span data-ttu-id="d4e93-309">Use [Configuração](xref:fundamentals/configuration/index) para configurar o host Web.</span><span class="sxs-lookup"><span data-stu-id="d4e93-309">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="d4e93-310">No exemplo a seguir, a configuração do host é especificada, opcionalmente, em um arquivo *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d4e93-310">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="d4e93-311">Qualquer configuração carregada do arquivo *hostsettings.json* pode ser substituída por argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d4e93-311">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="d4e93-312">A configuração de build (no `config`) é usada para configurar o host com [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="d4e93-312">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="d4e93-313">A configuração `IWebHostBuilder` é adicionada à configuração do aplicativo, mas o contrário não é verdade&mdash;`ConfigureAppConfiguration` não afeta a configuração `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-313">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d4e93-314">Substituição da configuração fornecida por `UseUrls` pela configuração de *hostsettings.json* primeiro, e pela configuração de argumento da linha de comando depois:</span><span class="sxs-lookup"><span data-stu-id="d4e93-314">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="d4e93-315">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d4e93-315">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d4e93-316">Substituição da configuração fornecida por `UseUrls` pela configuração de *hostsettings.json* primeiro, e pela configuração de argumento da linha de comando depois:</span><span class="sxs-lookup"><span data-stu-id="d4e93-316">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="d4e93-317">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d4e93-317">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="d4e93-318">Atualmente, o método de extensão [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) não é capaz de analisar uma seção de configuração retornada por `GetSection` (por exemplo, `.UseConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="d4e93-318">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d4e93-319">O método `GetSection` filtra as chaves de configuração da seção solicitada, mas deixa o nome da seção nas chaves (por exemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="d4e93-319">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="d4e93-320">O método `UseConfiguration` espera que as chaves correspondam às chaves `WebHostBuilder` (por exemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="d4e93-320">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="d4e93-321">A presença do nome da seção nas chaves impede que os valores da seção configurem o host.</span><span class="sxs-lookup"><span data-stu-id="d4e93-321">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="d4e93-322">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="d4e93-322">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d4e93-323">Para obter mais informações e soluções alternativas, consulte [Passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="d4e93-323">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="d4e93-324">`UseConfiguration` somente copia as chaves do `IConfiguration` fornecido para a configuração do construtor de host.</span><span class="sxs-lookup"><span data-stu-id="d4e93-324">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="d4e93-325">Portanto, definir `reloadOnChange: true` para arquivos de configuração JSON, INI e XML não tem nenhum efeito.</span><span class="sxs-lookup"><span data-stu-id="d4e93-325">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="d4e93-326">Para especificar o host executado em uma URL específica, o valor desejado pode ser passado em um prompt de comando ao executar [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="d4e93-326">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="d4e93-327">O argumento de linha de comando substitui o valor `urls` do arquivo *hostsettings.json* e o servidor escuta na porta 8080:</span><span class="sxs-lookup"><span data-stu-id="d4e93-327">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="d4e93-328">Gerenciar o host</span><span class="sxs-lookup"><span data-stu-id="d4e93-328">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d4e93-329">**Executar**</span><span class="sxs-lookup"><span data-stu-id="d4e93-329">**Run**</span></span>

<span data-ttu-id="d4e93-330">O método `Run` inicia o aplicativo Web e bloqueia o thread de chamada até que o host seja desligado:</span><span class="sxs-lookup"><span data-stu-id="d4e93-330">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d4e93-331">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="d4e93-331">**Start**</span></span>

<span data-ttu-id="d4e93-332">Execute o host sem bloqueio, chamando seu método `Start`:</span><span class="sxs-lookup"><span data-stu-id="d4e93-332">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d4e93-333">Se uma lista de URLs for passada para o método `Start`, ele escutará nas URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="d4e93-333">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="d4e93-334">O aplicativo pode inicializar e iniciar um novo host usando os padrões pré-configurados de `CreateDefaultBuilder`, usando um método estático conveniente.</span><span class="sxs-lookup"><span data-stu-id="d4e93-334">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="d4e93-335">Esses métodos iniciam o servidor sem uma saída do console e com [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) e aguardam uma quebra (Ctrl-C/SIGINT ou SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="d4e93-335">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="d4e93-336">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="d4e93-336">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="d4e93-337">Inicie com um `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d4e93-337">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d4e93-338">Faça uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Olá, Mundo!"</span><span class="sxs-lookup"><span data-stu-id="d4e93-338">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d4e93-339">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="d4e93-339">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d4e93-340">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="d4e93-340">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d4e93-341">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="d4e93-341">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="d4e93-342">Inicie com uma URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d4e93-342">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d4e93-343">Produz o mesmo resultado que **Start(RequestDelegate app)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-343">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="d4e93-344">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d4e93-344">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="d4e93-345">Use uma instância de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar o middleware de roteamento:</span><span class="sxs-lookup"><span data-stu-id="d4e93-345">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="d4e93-346">Use as seguintes solicitações de navegador com o exemplo:</span><span class="sxs-lookup"><span data-stu-id="d4e93-346">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="d4e93-347">Solicitação</span><span class="sxs-lookup"><span data-stu-id="d4e93-347">Request</span></span>                                    | <span data-ttu-id="d4e93-348">Resposta</span><span class="sxs-lookup"><span data-stu-id="d4e93-348">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="d4e93-349">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="d4e93-349">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="d4e93-350">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="d4e93-350">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="d4e93-351">Gera uma exceção com a cadeia de caracteres "ooops!"</span><span class="sxs-lookup"><span data-stu-id="d4e93-351">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="d4e93-352">Gera uma exceção com a cadeia de caracteres "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="d4e93-352">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="d4e93-353">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="d4e93-353">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="d4e93-354">Olá, Mundo!</span><span class="sxs-lookup"><span data-stu-id="d4e93-354">Hello World!</span></span>                             |

<span data-ttu-id="d4e93-355">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="d4e93-355">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d4e93-356">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="d4e93-356">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d4e93-357">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d4e93-357">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="d4e93-358">Use uma URL e uma instância de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d4e93-358">Use a URL and an instance of `IRouteBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d4e93-359">Produz o mesmo resultado que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-359">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="d4e93-360">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="d4e93-360">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="d4e93-361">Forneça um delegado para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d4e93-361">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d4e93-362">Faça uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Olá, Mundo!"</span><span class="sxs-lookup"><span data-stu-id="d4e93-362">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d4e93-363">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="d4e93-363">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d4e93-364">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="d4e93-364">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d4e93-365">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="d4e93-365">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="d4e93-366">Forneça um delegado e uma URL para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d4e93-366">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d4e93-367">Produz o mesmo resultado que **StartWith(Action&lt;IApplicationBuilder&gt; app)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-367">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d4e93-368">**Executar**</span><span class="sxs-lookup"><span data-stu-id="d4e93-368">**Run**</span></span>

<span data-ttu-id="d4e93-369">O método `Run` inicia o aplicativo Web e bloqueia o thread de chamada até que o host seja desligado:</span><span class="sxs-lookup"><span data-stu-id="d4e93-369">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d4e93-370">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="d4e93-370">**Start**</span></span>

<span data-ttu-id="d4e93-371">Execute o host sem bloqueio, chamando seu método `Start`:</span><span class="sxs-lookup"><span data-stu-id="d4e93-371">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d4e93-372">Se uma lista de URLs for passada para o método `Start`, ele escutará nas URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="d4e93-372">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

::: moniker-end

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="d4e93-373">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="d4e93-373">IHostingEnvironment interface</span></span>

<span data-ttu-id="d4e93-374">A [interface IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornece informações sobre o ambiente de hospedagem na Web do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-374">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="d4e93-375">Use a [injeção de construtor](xref:fundamentals/dependency-injection) para obter o `IHostingEnvironment` para usar suas propriedades e métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="d4e93-375">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="d4e93-376">Uma [abordagem baseada em convenção](xref:fundamentals/environments#environment-based-startup-class-and-methods) pode ser usada para configurar o aplicativo na inicialização com base no ambiente.</span><span class="sxs-lookup"><span data-stu-id="d4e93-376">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="d4e93-377">Como alternativa, injete o `IHostingEnvironment` no construtor `Startup` para uso em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d4e93-377">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="d4e93-378">Além do método de extensão `IsDevelopment`, `IHostingEnvironment` oferece os métodos `IsStaging`, `IsProduction` e `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-378">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="d4e93-379">Para obter mais informações, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d4e93-379">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="d4e93-380">O serviço `IHostingEnvironment` também pode ser injetado diretamente no método `Configure` para configurar o pipeline de processamento:</span><span class="sxs-lookup"><span data-stu-id="d4e93-380">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="d4e93-381">`IHostingEnvironment` pode ser injetado no método `Invoke` ao criar um [middleware](xref:fundamentals/middleware/index#write-middleware) personalizado:</span><span class="sxs-lookup"><span data-stu-id="d4e93-381">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="d4e93-382">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="d4e93-382">IApplicationLifetime interface</span></span>

<span data-ttu-id="d4e93-383">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite atividades pós-inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="d4e93-383">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="d4e93-384">Três propriedades na interface são tokens de cancelamento usados para registrar métodos `Action` que definem eventos de inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="d4e93-384">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="d4e93-385">Token de cancelamento</span><span class="sxs-lookup"><span data-stu-id="d4e93-385">Cancellation Token</span></span>    | <span data-ttu-id="d4e93-386">Acionado quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="d4e93-386">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="d4e93-387">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="d4e93-387">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="d4e93-388">O host foi iniciado totalmente.</span><span class="sxs-lookup"><span data-stu-id="d4e93-388">The host has fully started.</span></span> |
| [<span data-ttu-id="d4e93-389">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="d4e93-389">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="d4e93-390">O host está concluindo um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="d4e93-390">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d4e93-391">Todas as solicitações devem ser processadas.</span><span class="sxs-lookup"><span data-stu-id="d4e93-391">All requests should be processed.</span></span> <span data-ttu-id="d4e93-392">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="d4e93-392">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="d4e93-393">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="d4e93-393">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="d4e93-394">O host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="d4e93-394">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d4e93-395">Solicitações ainda podem estar sendo processadas.</span><span class="sxs-lookup"><span data-stu-id="d4e93-395">Requests may still be processing.</span></span> <span data-ttu-id="d4e93-396">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="d4e93-396">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="d4e93-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) solicita o término do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d4e93-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="d4e93-398">A classe a seguir usa `StopApplication` para desligar normalmente um aplicativo quando o método `Shutdown` da classe é chamado:</span><span class="sxs-lookup"><span data-stu-id="d4e93-398">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="d4e93-399">Validação de escopo</span><span class="sxs-lookup"><span data-stu-id="d4e93-399">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d4e93-400">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) define [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) como `true` quando o ambiente do aplicativo é de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="d4e93-400">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="d4e93-401">Quando `ValidateScopes` está definido como `true`, o provedor de serviço padrão executa verificações para saber se:</span><span class="sxs-lookup"><span data-stu-id="d4e93-401">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="d4e93-402">Os serviços com escopo não são resolvidos direta ou indiretamente pelo provedor de serviço raiz.</span><span class="sxs-lookup"><span data-stu-id="d4e93-402">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="d4e93-403">Os serviços com escopo não são injetados direta ou indiretamente em singletons.</span><span class="sxs-lookup"><span data-stu-id="d4e93-403">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="d4e93-404">O provedor de serviços raiz é criado quando [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) é chamado.</span><span class="sxs-lookup"><span data-stu-id="d4e93-404">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="d4e93-405">O tempo de vida do provedor de serviço raiz corresponde ao tempo de vida do aplicativo/servidor quando o provedor começa com o aplicativo e é descartado quando o aplicativo é desligado.</span><span class="sxs-lookup"><span data-stu-id="d4e93-405">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="d4e93-406">Os serviços com escopo são descartados pelo contêiner que os criou.</span><span class="sxs-lookup"><span data-stu-id="d4e93-406">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="d4e93-407">Se um serviço com escopo é criado no contêiner raiz, o tempo de vida do serviço é promovido efetivamente para singleton, porque ele só é descartado pelo contêiner raiz quando o aplicativo/servidor é desligado.</span><span class="sxs-lookup"><span data-stu-id="d4e93-407">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="d4e93-408">A validação dos escopos de serviço detecta essas situações quando `BuildServiceProvider` é chamado.</span><span class="sxs-lookup"><span data-stu-id="d4e93-408">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="d4e93-409">Para que os escopos sempre sejam validados, incluindo no ambiente de produção, configure [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) com [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) no construtor do host:</span><span class="sxs-lookup"><span data-stu-id="d4e93-409">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="d4e93-410">Solução de problemas de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="d4e93-410">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="d4e93-411">**O seguinte se aplica somente aos aplicativos do ASP.NET Core 2.0 quando o aplicativo não chama `UseStartup` ou `Configure`.**</span><span class="sxs-lookup"><span data-stu-id="d4e93-411">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="d4e93-412">Um host pode ser compilado injetando `IStartup` diretamente no contêiner de injeção de dependência em vez de chamar `UseStartup` ou `Configure`:</span><span class="sxs-lookup"><span data-stu-id="d4e93-412">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="d4e93-413">Se o host for compilado dessa maneira, o seguinte erro poderá ocorrer:</span><span class="sxs-lookup"><span data-stu-id="d4e93-413">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="d4e93-414">Isso ocorre porque o nome do aplicativo (o nome do assembly atual) é necessário para verificar se há `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="d4e93-414">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="d4e93-415">Se o aplicativo injetar manualmente `IStartup` no contêiner de injeção de dependência, adicione a seguinte chamada para `WebHostBuilder` com o nome do assembly especificado:</span><span class="sxs-lookup"><span data-stu-id="d4e93-415">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="d4e93-416">Como alternativa, adicione um `Configure` fictício ao `WebHostBuilder`, que define o nome do aplicativo automaticamente:</span><span class="sxs-lookup"><span data-stu-id="d4e93-416">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="d4e93-417">Para obter mais informações, consulte [Comunicado: Microsoft.Extensions.PlatformAbstractions foi removido (comentário)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e o [Exemplo de StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="d4e93-417">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d4e93-418">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="d4e93-418">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
