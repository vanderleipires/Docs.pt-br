---
title: Host da Web do ASP.NET Core
author: guardrex
description: Saiba mais sobre o host da Web no ASP.NET Core, que é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.
ms.author: riande
ms.custom: mvc
ms.date: 09/01/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 7440ab26534840b190a346614f645860fc2b7d78
ms.sourcegitcommit: 7211ae2dd702f67d36365831c490d6178c9a46c8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44089893"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="d943a-103">Host da Web do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d943a-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="d943a-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d943a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d943a-105">Aplicativos ASP.NET Core configuram e inicializam um *host*.</span><span class="sxs-lookup"><span data-stu-id="d943a-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="d943a-106">O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d943a-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d943a-107">No mínimo, o host configura um servidor e um pipeline de processamento de solicitações.</span><span class="sxs-lookup"><span data-stu-id="d943a-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="d943a-108">Este tópico aborda o Host da Web ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), que é útil para hospedagem de aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="d943a-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="d943a-109">Para cobertura do Host Genérico .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), veja <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="d943a-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="d943a-110">Configurar um host</span><span class="sxs-lookup"><span data-stu-id="d943a-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d943a-111">Crie um host usando uma instância do [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d943a-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="d943a-112">Normalmente, isso é feito no ponto de entrada do aplicativo, o método `Main`.</span><span class="sxs-lookup"><span data-stu-id="d943a-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d943a-113">Em modelos de projeto, `Main` está localizado em *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="d943a-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="d943a-114">Um *Program.cs* típico chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para começar a configurar um host:</span><span class="sxs-lookup"><span data-stu-id="d943a-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="d943a-115">`CreateDefaultBuilder` executa as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="d943a-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="d943a-116">Configura o [Kestrel](xref:fundamentals/servers/kestrel) como o servidor Web e configura o servidor usando provedores de configuração de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d943a-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="d943a-117">Para as opções padrão do Kestrel, veja <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="d943a-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="d943a-118">Define a raiz do conteúdo como o caminho retornado por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="d943a-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="d943a-119">Carrega a [configuração do host](#host-configuration-values) de:</span><span class="sxs-lookup"><span data-stu-id="d943a-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="d943a-120">Variáveis de ambiente prefixadas com `ASPNETCORE_` (por exemplo, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="d943a-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="d943a-121">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d943a-121">Command-line arguments.</span></span>
* <span data-ttu-id="d943a-122">Carrega a configuração do aplicativo de:</span><span class="sxs-lookup"><span data-stu-id="d943a-122">Loads app configuration from:</span></span>
  * <span data-ttu-id="d943a-123">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d943a-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="d943a-124">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d943a-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="d943a-125">[Segredos do usuário](xref:security/app-secrets) quando o aplicativo é executado no ambiente `Development` usando o assembly de entrada.</span><span class="sxs-lookup"><span data-stu-id="d943a-125">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="d943a-126">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="d943a-126">Environment variables.</span></span>
  * <span data-ttu-id="d943a-127">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d943a-127">Command-line arguments.</span></span>
* <span data-ttu-id="d943a-128">Configura o [registro em log](xref:fundamentals/logging/index) para a saída do console e de depuração.</span><span class="sxs-lookup"><span data-stu-id="d943a-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="d943a-129">O registro em log inclui regras de [filtragem de log](xref:fundamentals/logging/index#log-filtering) especificadas em uma seção de configuração de registro em log de um arquivo *appsettings.json* ou *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d943a-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="d943a-130">Quando executado por trás do IIS, permite a [integração de IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="d943a-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="d943a-131">Configura o caminho base e a porta que o servidor escuta ao usar o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d943a-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="d943a-132">O módulo cria um proxy reverso entre o IIS e o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d943a-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="d943a-133">Também configura o aplicativo para [capturar erros de inicialização](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="d943a-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="d943a-134">Para as opções padrão do IIS, veja <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="d943a-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="d943a-135">Definirá [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) como `true` se o ambiente do aplicativo for de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="d943a-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="d943a-136">Para obter mais informações, confira [Validação de escopo](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="d943a-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="d943a-137">A configuração definida por `CreateDefaultBuilder` pode ser substituída e aumentada por [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) e outros métodos, bem como os métodos de extensão de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d943a-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="d943a-138">Veja a seguir alguns exemplos:</span><span class="sxs-lookup"><span data-stu-id="d943a-138">A few examples follow:</span></span>

* <span data-ttu-id="d943a-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) é usado para especificar `IConfiguration` adicionais para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d943a-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="d943a-140">A seguinte chamada de `ConfigureAppConfiguration` adiciona um delegado para incluir a configuração do aplicativo no arquivo *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="d943a-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="d943a-141">`ConfigureAppConfiguration` pode ser chamado várias vezes.</span><span class="sxs-lookup"><span data-stu-id="d943a-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="d943a-142">Observe que essa configuração não se aplica ao host (por exemplo, URLs de servidor ou de ambiente).</span><span class="sxs-lookup"><span data-stu-id="d943a-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="d943a-143">Consulte a seção [Valores de configuração de Host](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="d943a-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="d943a-144">A seguinte chamada de `ConfigureLogging` adiciona um delegado para configurar o nível de log mínimo ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) como [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="d943a-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="d943a-145">Essa configuração substitui as configurações em *appsettings.Development.json* (`LogLevel.Debug`) e *appsettings.Production.json* (`LogLevel.Error`) configuradas por `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d943a-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d943a-146">`ConfigureLogging` pode ser chamado várias vezes.</span><span class="sxs-lookup"><span data-stu-id="d943a-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d943a-147">A seguinte chamada para `ConfigureKestrel` substitui o padrão [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) de 30 milhões de bytes, estabelecido quando o Kestrel foi configurado pelo `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d943a-147">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

* <span data-ttu-id="d943a-148">A seguinte chamada a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) substitui o padrão [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) de 30.000.000 bytes estabelecido quando Kestrel foi configurado por `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d943a-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d943a-149">A *raiz do conteúdo* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="d943a-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d943a-150">Quando o aplicativo é iniciado na pasta raiz do projeto, essa pasta é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="d943a-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d943a-151">Esse é o padrão usado no [Visual Studio](https://www.visualstudio.com/) e nos [novos modelos dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d943a-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d943a-152">Para obter mais informações sobre a configuração de aplicativo, veja <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d943a-152">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="d943a-153">Como uma alternativa ao uso do método `CreateDefaultBuilder` estático, criar um host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) é uma abordagem compatível com o ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="d943a-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="d943a-154">Para obter mais informações, consulte a guia do ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d943a-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d943a-155">Crie um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d943a-155">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="d943a-156">A criação de um host normalmente é feita no ponto de entrada do aplicativo, o método `Main`.</span><span class="sxs-lookup"><span data-stu-id="d943a-156">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d943a-157">Em modelos de projeto, `Main` está localizado em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d943a-157">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="d943a-158">`WebHostBuilder` requer um [servidor que implementa IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="d943a-158">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="d943a-159">Os servidores internos são [Kestrel](xref:fundamentals/servers/kestrel) e [HTTP.sys](xref:fundamentals/servers/httpsys) (antes do lançamento do ASP.NET Core 2.0, HTTP.sys era chamado de [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="d943a-159">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="d943a-160">Neste exemplo, o [método de extensão UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica o servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d943a-160">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="d943a-161">A *raiz do conteúdo* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="d943a-161">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d943a-162">A raiz de conteúdo padrão é obtida para `UseContentRoot` por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="d943a-162">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="d943a-163">Quando o aplicativo é iniciado na pasta raiz do projeto, essa pasta é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="d943a-163">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d943a-164">Esse é o padrão usado no [Visual Studio](https://www.visualstudio.com/) e nos [novos modelos dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d943a-164">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d943a-165">Para usar o IIS como um proxy reverso, chame [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte da compilação do host.</span><span class="sxs-lookup"><span data-stu-id="d943a-165">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="d943a-166">`UseIISIntegration` não configura um *servidor* como [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) o faz.</span><span class="sxs-lookup"><span data-stu-id="d943a-166">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="d943a-167">`UseIISIntegration` configura o caminho base e a porta que o servidor escuta ao usar o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para criar um proxy reverso entre o Kestrel e o IIS.</span><span class="sxs-lookup"><span data-stu-id="d943a-167">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="d943a-168">Para usar o IIS com o ASP.NET Core, tanto `UseKestrel` quanto `UseIISIntegration` precisam ser especificados.</span><span class="sxs-lookup"><span data-stu-id="d943a-168">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="d943a-169">`UseIISIntegration` é ativado somente quando é executado por trás do IIS ou do IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d943a-169">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="d943a-170">Para obter mais informações, consulte <xref:fundamentals/servers/aspnet-core-module> e <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="d943a-170">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="d943a-171">Uma implementação mínima que configura um host (e um aplicativo ASP.NET Core) inclui a especificação de um servidor e a configuração do pipeline de solicitações do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="d943a-171">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="d943a-172">Ao configurar um host, os métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) podem ser fornecidos.</span><span class="sxs-lookup"><span data-stu-id="d943a-172">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="d943a-173">Se uma classe `Startup` for especificada, ela deverá definir um método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d943a-173">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="d943a-174">Para obter mais informações, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d943a-174">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="d943a-175">Diversas chamadas para `ConfigureServices` são acrescentadas umas às outras.</span><span class="sxs-lookup"><span data-stu-id="d943a-175">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="d943a-176">Diversas chamadas para `Configure` ou `UseStartup` no `WebHostBuilder` substituem configurações anteriores.</span><span class="sxs-lookup"><span data-stu-id="d943a-176">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d943a-177">Valores de configuração do host</span><span class="sxs-lookup"><span data-stu-id="d943a-177">Host configuration values</span></span>

<span data-ttu-id="d943a-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) conta com as seguintes abordagens para definir os valores de configuração do host:</span><span class="sxs-lookup"><span data-stu-id="d943a-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="d943a-179">Configuração do construtor do host, que inclui variáveis de ambiente com o formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="d943a-179">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="d943a-180">Por exemplo, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="d943a-180">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="d943a-181">Extensões como [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) e [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (consulte a seção [Configuração de substituição](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="d943a-181">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="d943a-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e a chave associada.</span><span class="sxs-lookup"><span data-stu-id="d943a-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="d943a-183">Ao definir um valor com `UseSetting`, o valor é definido como uma cadeia de caracteres, independentemente do tipo.</span><span class="sxs-lookup"><span data-stu-id="d943a-183">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="d943a-184">O host usa a opção que define um valor por último.</span><span class="sxs-lookup"><span data-stu-id="d943a-184">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="d943a-185">Para obter mais informações, veja [Substituir configuração](#override-configuration) na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="d943a-185">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="d943a-186">Chave do Aplicativo (Nome)</span><span class="sxs-lookup"><span data-stu-id="d943a-186">Application Key (Name)</span></span>

<span data-ttu-id="d943a-187">A propriedade [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) é definida automaticamente quando [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) ou [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) é chamado durante a construção do host.</span><span class="sxs-lookup"><span data-stu-id="d943a-187">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="d943a-188">O valor é definido para o nome do assembly que contém o ponto de entrada do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d943a-188">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="d943a-189">Para definir o valor explicitamente, use o [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="d943a-189">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="d943a-190">**Chave**: applicationName</span><span class="sxs-lookup"><span data-stu-id="d943a-190">**Key**: applicationName</span></span>  
<span data-ttu-id="d943a-191">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d943a-191">**Type**: *string*</span></span>  
<span data-ttu-id="d943a-192">**Padrão**: o nome do assembly que contém o ponto de entrada do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d943a-192">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="d943a-193">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d943a-193">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d943a-194">**Variável de ambiente**: `ASPNETCORE_APPLICATIONKEY`</span><span class="sxs-lookup"><span data-stu-id="d943a-194">**Environment variable**: `ASPNETCORE_APPLICATIONKEY`</span></span>

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

### <a name="capture-startup-errors"></a><span data-ttu-id="d943a-195">Capturar erros de inicialização</span><span class="sxs-lookup"><span data-stu-id="d943a-195">Capture Startup Errors</span></span>

<span data-ttu-id="d943a-196">Esta configuração controla a captura de erros de inicialização.</span><span class="sxs-lookup"><span data-stu-id="d943a-196">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="d943a-197">**Chave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="d943a-197">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="d943a-198">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="d943a-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d943a-199">**Padrão**: o padrão é `false`, a menos que o aplicativo seja executado com o Kestrel por trás do IIS, em que o padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="d943a-199">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="d943a-200">**Definido usando**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="d943a-200">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="d943a-201">**Variável de ambiente**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="d943a-201">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="d943a-202">Quando `false`, erros durante a inicialização resultam no encerramento do host.</span><span class="sxs-lookup"><span data-stu-id="d943a-202">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="d943a-203">Quando `true`, o host captura exceções durante a inicialização e tenta iniciar o servidor.</span><span class="sxs-lookup"><span data-stu-id="d943a-203">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

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

### <a name="content-root"></a><span data-ttu-id="d943a-204">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="d943a-204">Content Root</span></span>

<span data-ttu-id="d943a-205">Essa configuração determina onde o ASP.NET Core começa a procurar por arquivos de conteúdo, como exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="d943a-205">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="d943a-206">**Chave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="d943a-206">**Key**: contentRoot</span></span>  
<span data-ttu-id="d943a-207">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d943a-207">**Type**: *string*</span></span>  
<span data-ttu-id="d943a-208">**Padrão**: o padrão é a pasta em que o assembly do aplicativo reside.</span><span class="sxs-lookup"><span data-stu-id="d943a-208">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="d943a-209">**Definido usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="d943a-209">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="d943a-210">**Variável de ambiente**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="d943a-210">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="d943a-211">A raiz do conteúdo também é usada como o caminho base para a [Configuração da raiz da Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="d943a-211">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="d943a-212">Se o caminho não existir, o host não será iniciado.</span><span class="sxs-lookup"><span data-stu-id="d943a-212">If the path doesn't exist, the host fails to start.</span></span>

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

### <a name="detailed-errors"></a><span data-ttu-id="d943a-213">Erros detalhados</span><span class="sxs-lookup"><span data-stu-id="d943a-213">Detailed Errors</span></span>

<span data-ttu-id="d943a-214">Determina se erros detalhados devem ser capturados.</span><span class="sxs-lookup"><span data-stu-id="d943a-214">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="d943a-215">**Chave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="d943a-215">**Key**: detailedErrors</span></span>  
<span data-ttu-id="d943a-216">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="d943a-216">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d943a-217">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="d943a-217">**Default**: false</span></span>  
<span data-ttu-id="d943a-218">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d943a-218">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d943a-219">**Variável de ambiente**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="d943a-219">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="d943a-220">Quando habilitado (ou quando o <a href="#environment">Ambiente</a> é definido como `Development`), o aplicativo captura exceções detalhadas.</span><span class="sxs-lookup"><span data-stu-id="d943a-220">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

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

### <a name="environment"></a><span data-ttu-id="d943a-221">Ambiente</span><span class="sxs-lookup"><span data-stu-id="d943a-221">Environment</span></span>

<span data-ttu-id="d943a-222">Define o ambiente do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d943a-222">Sets the app's environment.</span></span>

<span data-ttu-id="d943a-223">**Chave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="d943a-223">**Key**: environment</span></span>  
<span data-ttu-id="d943a-224">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d943a-224">**Type**: *string*</span></span>  
<span data-ttu-id="d943a-225">**Padrão**: Production</span><span class="sxs-lookup"><span data-stu-id="d943a-225">**Default**: Production</span></span>  
<span data-ttu-id="d943a-226">**Definido usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="d943a-226">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="d943a-227">**Variável de ambiente**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="d943a-227">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="d943a-228">O ambiente pode ser definido como qualquer valor.</span><span class="sxs-lookup"><span data-stu-id="d943a-228">The environment can be set to any value.</span></span> <span data-ttu-id="d943a-229">Os valores definidos pela estrutura incluem `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="d943a-229">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="d943a-230">Os valores não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d943a-230">Values aren't case sensitive.</span></span> <span data-ttu-id="d943a-231">Por padrão, o *Ambiente* é lido da variável de ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="d943a-231">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d943a-232">Ao usar o [Visual Studio](https://www.visualstudio.com/), variáveis de ambiente podem ser definidas no arquivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d943a-232">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="d943a-233">Para obter mais informações, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d943a-233">For more information, see <xref:fundamentals/environments>.</span></span>

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

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="d943a-234">Hospedando assemblies de inicialização</span><span class="sxs-lookup"><span data-stu-id="d943a-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="d943a-235">Define os assemblies de inicialização de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d943a-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="d943a-236">**Chave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="d943a-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="d943a-237">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d943a-237">**Type**: *string*</span></span>  
<span data-ttu-id="d943a-238">**Padrão**: cadeia de caracteres vazia</span><span class="sxs-lookup"><span data-stu-id="d943a-238">**Default**: Empty string</span></span>  
<span data-ttu-id="d943a-239">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d943a-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d943a-240">**Variável de ambiente**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d943a-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="d943a-241">Uma cadeia de caracteres delimitada por ponto e vírgula de assemblies de inicialização de hospedagem para carregamento na inicialização.</span><span class="sxs-lookup"><span data-stu-id="d943a-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="d943a-242">Embora o valor padrão da configuração seja uma cadeia de caracteres vazia, os assemblies de inicialização de hospedagem sempre incluem o assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d943a-242">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="d943a-243">Quando assemblies de inicialização de hospedagem são fornecidos, eles são adicionados ao assembly do aplicativo para carregamento quando o aplicativo compilar seus serviços comuns durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="d943a-243">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a><span data-ttu-id="d943a-244">Porta HTTPS</span><span class="sxs-lookup"><span data-stu-id="d943a-244">HTTPS Port</span></span>

<span data-ttu-id="d943a-245">Defina a porta de redirecionamento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d943a-245">Set the HTTPS redirect port.</span></span> <span data-ttu-id="d943a-246">Uso em [aplicação de HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="d943a-246">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="d943a-247">**Chave**: https_port **Tipo**: *string*
**Padrão**: Um valor padrão não está definido.</span><span class="sxs-lookup"><span data-stu-id="d943a-247">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="d943a-248">**Definido usando**: `UseSetting`
**Variável de ambiente**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="d943a-248">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="d943a-249">Hospedando assemblies de exclusão de inicialização</span><span class="sxs-lookup"><span data-stu-id="d943a-249">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="d943a-250">DESCRIÇÃO</span><span class="sxs-lookup"><span data-stu-id="d943a-250">DESCRIPTION</span></span>

<span data-ttu-id="d943a-251">**Chave**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="d943a-251">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="d943a-252">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d943a-252">**Type**: *string*</span></span>  
<span data-ttu-id="d943a-253">**Padrão**: cadeia de caracteres vazia</span><span class="sxs-lookup"><span data-stu-id="d943a-253">**Default**: Empty string</span></span>  
<span data-ttu-id="d943a-254">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d943a-254">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d943a-255">**Variável de ambiente**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d943a-255">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="d943a-256">Uma cadeia de caracteres delimitada por ponto e vírgula de assemblies de inicialização de hospedagem para exclusão na inicialização.</span><span class="sxs-lookup"><span data-stu-id="d943a-256">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="d943a-257">Preferir URLs de hospedagem</span><span class="sxs-lookup"><span data-stu-id="d943a-257">Prefer Hosting URLs</span></span>

<span data-ttu-id="d943a-258">Indica se o host deve escutar as URLs configuradas com o `WebHostBuilder` em vez daquelas configuradas com a implementação `IServer`.</span><span class="sxs-lookup"><span data-stu-id="d943a-258">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="d943a-259">**Chave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="d943a-259">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="d943a-260">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="d943a-260">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d943a-261">**Padrão**: true</span><span class="sxs-lookup"><span data-stu-id="d943a-261">**Default**: true</span></span>  
<span data-ttu-id="d943a-262">**Definido usando**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="d943a-262">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="d943a-263">**Variável de ambiente**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="d943a-263">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="d943a-264">Impedir inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="d943a-264">Prevent Hosting Startup</span></span>

<span data-ttu-id="d943a-265">Impede o carregamento automático de assemblies de inicialização de hospedagem, incluindo assemblies de inicialização de hospedagem configurados pelo assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d943a-265">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="d943a-266">Para obter mais informações, consulte <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d943a-266">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="d943a-267">**Chave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="d943a-267">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="d943a-268">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="d943a-268">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d943a-269">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="d943a-269">**Default**: false</span></span>  
<span data-ttu-id="d943a-270">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d943a-270">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d943a-271">**Variável de ambiente**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="d943a-271">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="d943a-272">URLs de servidor</span><span class="sxs-lookup"><span data-stu-id="d943a-272">Server URLs</span></span>

<span data-ttu-id="d943a-273">Indica os endereços IP ou endereços de host com portas e protocolos que o servidor deve escutar para solicitações.</span><span class="sxs-lookup"><span data-stu-id="d943a-273">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="d943a-274">**Chave**: urls</span><span class="sxs-lookup"><span data-stu-id="d943a-274">**Key**: urls</span></span>  
<span data-ttu-id="d943a-275">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d943a-275">**Type**: *string*</span></span>  
<span data-ttu-id="d943a-276">**Padrão**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="d943a-276">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="d943a-277">**Definido usando**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="d943a-277">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="d943a-278">**Variável de ambiente**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="d943a-278">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="d943a-279">Defina como uma lista separada por ponto e vírgula (;) de prefixos de URL aos quais o servidor deve responder.</span><span class="sxs-lookup"><span data-stu-id="d943a-279">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="d943a-280">Por exemplo, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="d943a-280">For example, `http://localhost:123`.</span></span> <span data-ttu-id="d943a-281">Use "\*" para indicar que o servidor deve escutar solicitações em qualquer endereço IP ou nome do host usando a porta e o protocolo especificados (por exemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="d943a-281">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="d943a-282">O protocolo (`http://` ou `https://`) deve ser incluído com cada URL.</span><span class="sxs-lookup"><span data-stu-id="d943a-282">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="d943a-283">Os formatos compatíveis variam dependendo dos servidores.</span><span class="sxs-lookup"><span data-stu-id="d943a-283">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="d943a-284">O Kestrel tem sua própria API de configuração de ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="d943a-284">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="d943a-285">Para obter mais informações, consulte <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d943a-285">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="d943a-286">Tempo limite de desligamento</span><span class="sxs-lookup"><span data-stu-id="d943a-286">Shutdown Timeout</span></span>

<span data-ttu-id="d943a-287">Especifica o tempo de espera para o desligamento do host da Web.</span><span class="sxs-lookup"><span data-stu-id="d943a-287">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="d943a-288">**Chave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="d943a-288">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="d943a-289">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="d943a-289">**Type**: *int*</span></span>  
<span data-ttu-id="d943a-290">**Padrão**: 5</span><span class="sxs-lookup"><span data-stu-id="d943a-290">**Default**: 5</span></span>  
<span data-ttu-id="d943a-291">**Definido usando**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="d943a-291">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="d943a-292">**Variável de ambiente**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="d943a-292">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="d943a-293">Embora a chave aceite um *int* com `UseSetting` (por exemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), o método de extensão [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) usa um [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="d943a-293">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="d943a-294">Durante o período de tempo limite, a hospedagem:</span><span class="sxs-lookup"><span data-stu-id="d943a-294">During the timeout period, hosting:</span></span>

* <span data-ttu-id="d943a-295">Dispara [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="d943a-295">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="d943a-296">Tenta parar os serviços hospedados, registrando em log os erros dos serviços que falham ao parar.</span><span class="sxs-lookup"><span data-stu-id="d943a-296">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="d943a-297">Se o período de tempo limite expirar antes que todos os serviços hospedados parem, os serviços ativos restantes serão parados quando o aplicativo for desligado.</span><span class="sxs-lookup"><span data-stu-id="d943a-297">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="d943a-298">Os serviços serão parados mesmo se ainda não tiverem concluído o processamento.</span><span class="sxs-lookup"><span data-stu-id="d943a-298">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="d943a-299">Se os serviços exigirem mais tempo para parar, aumente o tempo limite.</span><span class="sxs-lookup"><span data-stu-id="d943a-299">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="d943a-300">Assembly de inicialização</span><span class="sxs-lookup"><span data-stu-id="d943a-300">Startup Assembly</span></span>

<span data-ttu-id="d943a-301">Determina o assembly para pesquisar pela classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="d943a-301">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="d943a-302">**Chave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="d943a-302">**Key**: startupAssembly</span></span>  
<span data-ttu-id="d943a-303">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d943a-303">**Type**: *string*</span></span>  
<span data-ttu-id="d943a-304">**Padrão**: o assembly do aplicativo</span><span class="sxs-lookup"><span data-stu-id="d943a-304">**Default**: The app's assembly</span></span>  
<span data-ttu-id="d943a-305">**Definido usando**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="d943a-305">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="d943a-306">**Variável de ambiente**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="d943a-306">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="d943a-307">O assembly por nome (`string`) ou por tipo (`TStartup`) pode ser referenciado.</span><span class="sxs-lookup"><span data-stu-id="d943a-307">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="d943a-308">Se vários métodos `UseStartup` forem chamados, o último terá precedência.</span><span class="sxs-lookup"><span data-stu-id="d943a-308">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

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

### <a name="web-root"></a><span data-ttu-id="d943a-309">Raiz da Web</span><span class="sxs-lookup"><span data-stu-id="d943a-309">Web Root</span></span>

<span data-ttu-id="d943a-310">Define o caminho relativo para os ativos estáticos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d943a-310">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="d943a-311">**Chave**: webroot</span><span class="sxs-lookup"><span data-stu-id="d943a-311">**Key**: webroot</span></span>  
<span data-ttu-id="d943a-312">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="d943a-312">**Type**: *string*</span></span>  
<span data-ttu-id="d943a-313">**Padrão**: se não for especificado, o padrão será "(Raiz do conteúdo)/wwwroot", se o caminho existir.</span><span class="sxs-lookup"><span data-stu-id="d943a-313">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="d943a-314">Se o caminho não existir, um provedor de arquivo não operacional será usado.</span><span class="sxs-lookup"><span data-stu-id="d943a-314">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="d943a-315">**Definido usando**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="d943a-315">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="d943a-316">**Variável de ambiente**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="d943a-316">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

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

## <a name="override-configuration"></a><span data-ttu-id="d943a-317">Substituir configuração</span><span class="sxs-lookup"><span data-stu-id="d943a-317">Override configuration</span></span>

<span data-ttu-id="d943a-318">Use [Configuração](xref:fundamentals/configuration/index) para configurar o host Web.</span><span class="sxs-lookup"><span data-stu-id="d943a-318">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="d943a-319">No exemplo a seguir, a configuração do host é especificada, opcionalmente, em um arquivo *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d943a-319">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="d943a-320">Qualquer configuração carregada do arquivo *hostsettings.json* pode ser substituída por argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d943a-320">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="d943a-321">A configuração de build (no `config`) é usada para configurar o host com [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="d943a-321">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="d943a-322">A configuração `IWebHostBuilder` é adicionada à configuração do aplicativo, mas o contrário não é verdade&mdash;`ConfigureAppConfiguration` não afeta a configuração `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d943a-322">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d943a-323">Substituição da configuração fornecida por `UseUrls` pela configuração de *hostsettings.json* primeiro, e pela configuração de argumento da linha de comando depois:</span><span class="sxs-lookup"><span data-stu-id="d943a-323">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="d943a-324">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d943a-324">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d943a-325">Substituição da configuração fornecida por `UseUrls` pela configuração de *hostsettings.json* primeiro, e pela configuração de argumento da linha de comando depois:</span><span class="sxs-lookup"><span data-stu-id="d943a-325">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="d943a-326">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d943a-326">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="d943a-327">Atualmente, o método de extensão [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) não é capaz de analisar uma seção de configuração retornada por `GetSection` (por exemplo, `.UseConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="d943a-327">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d943a-328">O método `GetSection` filtra as chaves de configuração da seção solicitada, mas deixa o nome da seção nas chaves (por exemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="d943a-328">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="d943a-329">O método `UseConfiguration` espera que as chaves correspondam às chaves `WebHostBuilder` (por exemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="d943a-329">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="d943a-330">A presença do nome da seção nas chaves impede que os valores da seção configurem o host.</span><span class="sxs-lookup"><span data-stu-id="d943a-330">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="d943a-331">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="d943a-331">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d943a-332">Para obter mais informações e soluções alternativas, consulte [Passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="d943a-332">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="d943a-333">`UseConfiguration` somente copia as chaves do `IConfiguration` fornecido para a configuração do construtor de host.</span><span class="sxs-lookup"><span data-stu-id="d943a-333">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="d943a-334">Portanto, definir `reloadOnChange: true` para arquivos de configuração JSON, INI e XML não tem nenhum efeito.</span><span class="sxs-lookup"><span data-stu-id="d943a-334">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="d943a-335">Para especificar o host executado em uma URL específica, o valor desejado pode ser passado em um prompt de comando ao executar [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="d943a-335">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="d943a-336">O argumento de linha de comando substitui o valor `urls` do arquivo *hostsettings.json* e o servidor escuta na porta 8080:</span><span class="sxs-lookup"><span data-stu-id="d943a-336">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="d943a-337">Gerenciar o host</span><span class="sxs-lookup"><span data-stu-id="d943a-337">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d943a-338">**Executar**</span><span class="sxs-lookup"><span data-stu-id="d943a-338">**Run**</span></span>

<span data-ttu-id="d943a-339">O método `Run` inicia o aplicativo Web e bloqueia o thread de chamada até que o host seja desligado:</span><span class="sxs-lookup"><span data-stu-id="d943a-339">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d943a-340">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="d943a-340">**Start**</span></span>

<span data-ttu-id="d943a-341">Execute o host sem bloqueio, chamando seu método `Start`:</span><span class="sxs-lookup"><span data-stu-id="d943a-341">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d943a-342">Se uma lista de URLs for passada para o método `Start`, ele escutará nas URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="d943a-342">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="d943a-343">O aplicativo pode inicializar e iniciar um novo host usando os padrões pré-configurados de `CreateDefaultBuilder`, usando um método estático conveniente.</span><span class="sxs-lookup"><span data-stu-id="d943a-343">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="d943a-344">Esses métodos iniciam o servidor sem uma saída do console e com [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) e aguardam uma quebra (Ctrl-C/SIGINT ou SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="d943a-344">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="d943a-345">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="d943a-345">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="d943a-346">Inicie com um `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d943a-346">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d943a-347">Faça uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Olá, Mundo!"</span><span class="sxs-lookup"><span data-stu-id="d943a-347">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d943a-348">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="d943a-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d943a-349">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="d943a-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d943a-350">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="d943a-350">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="d943a-351">Inicie com uma URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d943a-351">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d943a-352">Produz o mesmo resultado que **Start(RequestDelegate app)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d943a-352">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="d943a-353">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d943a-353">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="d943a-354">Use uma instância de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar o middleware de roteamento:</span><span class="sxs-lookup"><span data-stu-id="d943a-354">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="d943a-355">Use as seguintes solicitações de navegador com o exemplo:</span><span class="sxs-lookup"><span data-stu-id="d943a-355">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="d943a-356">Solicitação</span><span class="sxs-lookup"><span data-stu-id="d943a-356">Request</span></span>                                    | <span data-ttu-id="d943a-357">Resposta</span><span class="sxs-lookup"><span data-stu-id="d943a-357">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="d943a-358">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="d943a-358">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="d943a-359">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="d943a-359">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="d943a-360">Gera uma exceção com a cadeia de caracteres "ooops!"</span><span class="sxs-lookup"><span data-stu-id="d943a-360">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="d943a-361">Gera uma exceção com a cadeia de caracteres "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="d943a-361">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="d943a-362">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="d943a-362">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="d943a-363">Olá, Mundo!</span><span class="sxs-lookup"><span data-stu-id="d943a-363">Hello World!</span></span>                             |

<span data-ttu-id="d943a-364">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="d943a-364">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d943a-365">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="d943a-365">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d943a-366">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d943a-366">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="d943a-367">Use uma URL e uma instância de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d943a-367">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="d943a-368">Produz o mesmo resultado que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d943a-368">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="d943a-369">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="d943a-369">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="d943a-370">Forneça um delegado para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d943a-370">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d943a-371">Faça uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Olá, Mundo!"</span><span class="sxs-lookup"><span data-stu-id="d943a-371">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d943a-372">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="d943a-372">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d943a-373">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="d943a-373">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d943a-374">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="d943a-374">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="d943a-375">Forneça um delegado e uma URL para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d943a-375">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d943a-376">Produz o mesmo resultado que **StartWith(Action&lt;IApplicationBuilder&gt; app)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d943a-376">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d943a-377">**Executar**</span><span class="sxs-lookup"><span data-stu-id="d943a-377">**Run**</span></span>

<span data-ttu-id="d943a-378">O método `Run` inicia o aplicativo Web e bloqueia o thread de chamada até que o host seja desligado:</span><span class="sxs-lookup"><span data-stu-id="d943a-378">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d943a-379">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="d943a-379">**Start**</span></span>

<span data-ttu-id="d943a-380">Execute o host sem bloqueio, chamando seu método `Start`:</span><span class="sxs-lookup"><span data-stu-id="d943a-380">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d943a-381">Se uma lista de URLs for passada para o método `Start`, ele escutará nas URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="d943a-381">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="d943a-382">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="d943a-382">IHostingEnvironment interface</span></span>

<span data-ttu-id="d943a-383">A [interface IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornece informações sobre o ambiente de hospedagem na Web do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d943a-383">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="d943a-384">Use a [injeção de construtor](xref:fundamentals/dependency-injection) para obter o `IHostingEnvironment` para usar suas propriedades e métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="d943a-384">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="d943a-385">Uma [abordagem baseada em convenção](xref:fundamentals/environments#environment-based-startup-class-and-methods) pode ser usada para configurar o aplicativo na inicialização com base no ambiente.</span><span class="sxs-lookup"><span data-stu-id="d943a-385">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="d943a-386">Como alternativa, injete o `IHostingEnvironment` no construtor `Startup` para uso em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d943a-386">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="d943a-387">Além do método de extensão `IsDevelopment`, `IHostingEnvironment` oferece os métodos `IsStaging`, `IsProduction` e `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="d943a-387">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="d943a-388">Para obter mais informações, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d943a-388">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="d943a-389">O serviço `IHostingEnvironment` também pode ser injetado diretamente no método `Configure` para configurar o pipeline de processamento:</span><span class="sxs-lookup"><span data-stu-id="d943a-389">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="d943a-390">`IHostingEnvironment` pode ser injetado no método `Invoke` ao criar um [middleware](xref:fundamentals/middleware/index#write-middleware) personalizado:</span><span class="sxs-lookup"><span data-stu-id="d943a-390">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="d943a-391">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="d943a-391">IApplicationLifetime interface</span></span>

<span data-ttu-id="d943a-392">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite atividades pós-inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="d943a-392">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="d943a-393">Três propriedades na interface são tokens de cancelamento usados para registrar métodos `Action` que definem eventos de inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="d943a-393">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="d943a-394">Token de cancelamento</span><span class="sxs-lookup"><span data-stu-id="d943a-394">Cancellation Token</span></span>    | <span data-ttu-id="d943a-395">Acionado quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="d943a-395">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="d943a-396">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="d943a-396">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="d943a-397">O host foi iniciado totalmente.</span><span class="sxs-lookup"><span data-stu-id="d943a-397">The host has fully started.</span></span> |
| [<span data-ttu-id="d943a-398">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="d943a-398">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="d943a-399">O host está concluindo um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="d943a-399">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d943a-400">Todas as solicitações devem ser processadas.</span><span class="sxs-lookup"><span data-stu-id="d943a-400">All requests should be processed.</span></span> <span data-ttu-id="d943a-401">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="d943a-401">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="d943a-402">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="d943a-402">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="d943a-403">O host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="d943a-403">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d943a-404">Solicitações ainda podem estar sendo processadas.</span><span class="sxs-lookup"><span data-stu-id="d943a-404">Requests may still be processing.</span></span> <span data-ttu-id="d943a-405">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="d943a-405">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="d943a-406">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) solicita o término do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d943a-406">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="d943a-407">A classe a seguir usa `StopApplication` para desligar normalmente um aplicativo quando o método `Shutdown` da classe é chamado:</span><span class="sxs-lookup"><span data-stu-id="d943a-407">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="d943a-408">Validação de escopo</span><span class="sxs-lookup"><span data-stu-id="d943a-408">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d943a-409">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) define [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) como `true` quando o ambiente do aplicativo é de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="d943a-409">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="d943a-410">Quando `ValidateScopes` está definido como `true`, o provedor de serviço padrão executa verificações para saber se:</span><span class="sxs-lookup"><span data-stu-id="d943a-410">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="d943a-411">Os serviços com escopo não são resolvidos direta ou indiretamente pelo provedor de serviço raiz.</span><span class="sxs-lookup"><span data-stu-id="d943a-411">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="d943a-412">Os serviços com escopo não são injetados direta ou indiretamente em singletons.</span><span class="sxs-lookup"><span data-stu-id="d943a-412">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="d943a-413">O provedor de serviços raiz é criado quando [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) é chamado.</span><span class="sxs-lookup"><span data-stu-id="d943a-413">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="d943a-414">O tempo de vida do provedor de serviço raiz corresponde ao tempo de vida do aplicativo/servidor quando o provedor começa com o aplicativo e é descartado quando o aplicativo é desligado.</span><span class="sxs-lookup"><span data-stu-id="d943a-414">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="d943a-415">Os serviços com escopo são descartados pelo contêiner que os criou.</span><span class="sxs-lookup"><span data-stu-id="d943a-415">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="d943a-416">Se um serviço com escopo é criado no contêiner raiz, o tempo de vida do serviço é promovido efetivamente para singleton, porque ele só é descartado pelo contêiner raiz quando o aplicativo/servidor é desligado.</span><span class="sxs-lookup"><span data-stu-id="d943a-416">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="d943a-417">A validação dos escopos de serviço detecta essas situações quando `BuildServiceProvider` é chamado.</span><span class="sxs-lookup"><span data-stu-id="d943a-417">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="d943a-418">Para que os escopos sempre sejam validados, incluindo no ambiente de produção, configure [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) com [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) no construtor do host:</span><span class="sxs-lookup"><span data-stu-id="d943a-418">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="d943a-419">Solução de problemas de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="d943a-419">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="d943a-420">**O seguinte se aplica somente aos aplicativos do ASP.NET Core 2.0 quando o aplicativo não chama `UseStartup` ou `Configure`.**</span><span class="sxs-lookup"><span data-stu-id="d943a-420">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="d943a-421">Um host pode ser compilado injetando `IStartup` diretamente no contêiner de injeção de dependência em vez de chamar `UseStartup` ou `Configure`:</span><span class="sxs-lookup"><span data-stu-id="d943a-421">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="d943a-422">Se o host for compilado dessa maneira, o seguinte erro poderá ocorrer:</span><span class="sxs-lookup"><span data-stu-id="d943a-422">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="d943a-423">Isso ocorre porque o nome do aplicativo (o nome do assembly atual) é necessário para verificar se há `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="d943a-423">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="d943a-424">Se o aplicativo injetar manualmente `IStartup` no contêiner de injeção de dependência, adicione a seguinte chamada para `WebHostBuilder` com o nome do assembly especificado:</span><span class="sxs-lookup"><span data-stu-id="d943a-424">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="d943a-425">Como alternativa, adicione um `Configure` fictício ao `WebHostBuilder`, que define o nome do aplicativo automaticamente:</span><span class="sxs-lookup"><span data-stu-id="d943a-425">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="d943a-426">Para obter mais informações, consulte [Comunicado: Microsoft.Extensions.PlatformAbstractions foi removido (comentário)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e o [Exemplo de StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="d943a-426">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d943a-427">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="d943a-427">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
