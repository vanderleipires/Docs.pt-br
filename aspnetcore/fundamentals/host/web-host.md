---
title: Host da Web do ASP.NET Core
author: guardrex
description: Saiba mais sobre o host da Web no ASP.NET Core, que é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 5af09ad715768d51ce8ef2c8425cc51ebada6859
ms.sourcegitcommit: 1d6ab43eed9cb3df6211c22b97bb3a9351ec4419
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51597817"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="8a1d7-103">Host da Web do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a1d7-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="8a1d7-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8a1d7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="8a1d7-105">Para obter a versão 1.1 deste tópico, baixe [Host da Web do ASP.NET Core (versão 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-105">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="8a1d7-106">Aplicativos ASP.NET Core configuram e inicializam um *host*.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="8a1d7-107">O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="8a1d7-108">No mínimo, o host configura um servidor e um pipeline de processamento de solicitações.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-108">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="8a1d7-109">Este tópico aborda o Host da Web ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), que é útil para hospedagem de aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-109">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="8a1d7-110">Para cobertura do Host Genérico .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), veja <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-110">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="8a1d7-111">Configurar um host</span><span class="sxs-lookup"><span data-stu-id="8a1d7-111">Set up a host</span></span>

<span data-ttu-id="8a1d7-112">Crie um host usando uma instância do [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="8a1d7-113">Normalmente, isso é feito no ponto de entrada do aplicativo, o método `Main`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="8a1d7-114">Em modelos de projeto, `Main` está localizado em *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="8a1d7-115">Um *Program.cs* típico chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para começar a configurar um host:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="8a1d7-116">`CreateDefaultBuilder` executa as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="8a1d7-117">Configura o [Kestrel](xref:fundamentals/servers/kestrel) como o servidor Web e configura o servidor usando provedores de configuração de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="8a1d7-118">Para as opções padrão do Kestrel, veja <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-118">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="8a1d7-119">Define a raiz do conteúdo como o caminho retornado por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="8a1d7-120">Carrega a [configuração do host](#host-configuration-values) de:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-120">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="8a1d7-121">Variáveis de ambiente prefixadas com `ASPNETCORE_` (por exemplo, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-121">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="8a1d7-122">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-122">Command-line arguments.</span></span>
* <span data-ttu-id="8a1d7-123">Carrega a configuração do aplicativo na seguinte ordem de:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-123">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="8a1d7-124">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-124">*appsettings.json*.</span></span>
  * <span data-ttu-id="8a1d7-125">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-125">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="8a1d7-126">[Gerenciador de Segredo](xref:security/app-secrets) quando o aplicativo é executado no ambiente `Development` usando o assembly de entrada.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-126">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="8a1d7-127">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-127">Environment variables.</span></span>
  * <span data-ttu-id="8a1d7-128">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-128">Command-line arguments.</span></span>
* <span data-ttu-id="8a1d7-129">Configura o [registro em log](xref:fundamentals/logging/index) para a saída do console e de depuração.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-129">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="8a1d7-130">O registro em log inclui regras de [filtragem de log](xref:fundamentals/logging/index#log-filtering) especificadas em uma seção de configuração de registro em log de um arquivo *appsettings.json* ou *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-130">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="8a1d7-131">Quando executado por trás do IIS, permite a [integração de IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-131">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="8a1d7-132">Configura o caminho base e a porta que o servidor escuta ao usar o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-132">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="8a1d7-133">O módulo cria um proxy reverso entre o IIS e o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-133">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="8a1d7-134">Também configura o aplicativo para [capturar erros de inicialização](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-134">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="8a1d7-135">Para as opções padrão do IIS, veja <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-135">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="8a1d7-136">Definirá [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) como `true` se o ambiente do aplicativo for de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-136">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="8a1d7-137">Para obter mais informações, confira [Validação de escopo](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-137">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="8a1d7-138">A configuração definida por `CreateDefaultBuilder` pode ser substituída e aumentada por [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) e outros métodos, bem como os métodos de extensão de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-138">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="8a1d7-139">Veja a seguir alguns exemplos:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-139">A few examples follow:</span></span>

* <span data-ttu-id="8a1d7-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) é usado para especificar `IConfiguration` adicionais para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="8a1d7-141">A seguinte chamada de `ConfigureAppConfiguration` adiciona um delegado para incluir a configuração do aplicativo no arquivo *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-141">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="8a1d7-142">`ConfigureAppConfiguration` pode ser chamado várias vezes.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-142">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="8a1d7-143">Observe que essa configuração não se aplica ao host (por exemplo, URLs de servidor ou de ambiente).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-143">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="8a1d7-144">Consulte a seção [Valores de configuração de Host](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-144">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="8a1d7-145">A seguinte chamada de `ConfigureLogging` adiciona um delegado para configurar o nível de log mínimo ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) como [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-145">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="8a1d7-146">Essa configuração substitui as configurações em *appsettings.Development.json* (`LogLevel.Debug`) e *appsettings.Production.json* (`LogLevel.Error`) configuradas por `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-146">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8a1d7-147">`ConfigureLogging` pode ser chamado várias vezes.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-147">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="8a1d7-148">A seguinte chamada para `ConfigureKestrel` substitui o padrão [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) de 30 milhões de bytes, estabelecido quando o Kestrel foi configurado pelo `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-148">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="8a1d7-149">A seguinte chamada a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) substitui o padrão [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) de 30.000.000 bytes estabelecido quando Kestrel foi configurado por `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-149">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="8a1d7-150">A *raiz do conteúdo* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-150">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="8a1d7-151">Quando o aplicativo é iniciado na pasta raiz do projeto, essa pasta é usada como a raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-151">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="8a1d7-152">Esse é o padrão usado no [Visual Studio](https://www.visualstudio.com/) e nos [novos modelos dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-152">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="8a1d7-153">Para obter mais informações sobre a configuração de aplicativo, veja <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-153">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="8a1d7-154">Como uma alternativa ao uso do método `CreateDefaultBuilder` estático, criar um host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) é uma abordagem compatível com o ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-154">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="8a1d7-155">Para obter mais informações, consulte a guia do ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-155">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="8a1d7-156">Ao configurar um host, os métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) podem ser fornecidos.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-156">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="8a1d7-157">Se uma classe `Startup` for especificada, ela deverá definir um método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-157">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="8a1d7-158">Para obter mais informações, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-158">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="8a1d7-159">Diversas chamadas para `ConfigureServices` são acrescentadas umas às outras.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-159">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="8a1d7-160">Diversas chamadas para `Configure` ou `UseStartup` no `WebHostBuilder` substituem configurações anteriores.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-160">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="8a1d7-161">Valores de configuração do host</span><span class="sxs-lookup"><span data-stu-id="8a1d7-161">Host configuration values</span></span>

<span data-ttu-id="8a1d7-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) conta com as seguintes abordagens para definir os valores de configuração do host:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="8a1d7-163">Configuração do construtor do host, que inclui variáveis de ambiente com o formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-163">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="8a1d7-164">Por exemplo, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-164">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="8a1d7-165">Extensões como [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) e [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (consulte a seção [Configuração de substituição](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-165">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="8a1d7-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e a chave associada.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="8a1d7-167">Ao definir um valor com `UseSetting`, o valor é definido como uma cadeia de caracteres, independentemente do tipo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-167">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="8a1d7-168">O host usa a opção que define um valor por último.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-168">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="8a1d7-169">Para obter mais informações, veja [Substituir configuração](#override-configuration) na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-169">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="8a1d7-170">Chave do Aplicativo (Nome)</span><span class="sxs-lookup"><span data-stu-id="8a1d7-170">Application Key (Name)</span></span>

<span data-ttu-id="8a1d7-171">A propriedade [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) é definida automaticamente quando [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) ou [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) é chamado durante a construção do host.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-171">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="8a1d7-172">O valor é definido para o nome do assembly que contém o ponto de entrada do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-172">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="8a1d7-173">Para definir o valor explicitamente, use o [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="8a1d7-173">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="8a1d7-174">**Chave**: applicationName</span><span class="sxs-lookup"><span data-stu-id="8a1d7-174">**Key**: applicationName</span></span>  
<span data-ttu-id="8a1d7-175">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="8a1d7-175">**Type**: *string*</span></span>  
<span data-ttu-id="8a1d7-176">**Padrão**: o nome do assembly que contém o ponto de entrada do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-176">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="8a1d7-177">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-177">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="8a1d7-178">**Variável de ambiente**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-178">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="8a1d7-179">Capturar erros de inicialização</span><span class="sxs-lookup"><span data-stu-id="8a1d7-179">Capture Startup Errors</span></span>

<span data-ttu-id="8a1d7-180">Esta configuração controla a captura de erros de inicialização.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-180">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="8a1d7-181">**Chave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="8a1d7-181">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="8a1d7-182">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="8a1d7-182">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="8a1d7-183">**Padrão**: o padrão é `false`, a menos que o aplicativo seja executado com o Kestrel por trás do IIS, em que o padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-183">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="8a1d7-184">**Definido usando**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-184">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="8a1d7-185">**Variável de ambiente**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-185">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="8a1d7-186">Quando `false`, erros durante a inicialização resultam no encerramento do host.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-186">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="8a1d7-187">Quando `true`, o host captura exceções durante a inicialização e tenta iniciar o servidor.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-187">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="8a1d7-188">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="8a1d7-188">Content Root</span></span>

<span data-ttu-id="8a1d7-189">Essa configuração determina onde o ASP.NET Core começa a procurar por arquivos de conteúdo, como exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-189">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="8a1d7-190">**Chave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="8a1d7-190">**Key**: contentRoot</span></span>  
<span data-ttu-id="8a1d7-191">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="8a1d7-191">**Type**: *string*</span></span>  
<span data-ttu-id="8a1d7-192">**Padrão**: o padrão é a pasta em que o assembly do aplicativo reside.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-192">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="8a1d7-193">**Definido usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-193">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="8a1d7-194">**Variável de ambiente**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-194">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="8a1d7-195">A raiz do conteúdo também é usada como o caminho base para a [Configuração da raiz da Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-195">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="8a1d7-196">Se o caminho não existir, o host não será iniciado.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-196">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="8a1d7-197">Erros detalhados</span><span class="sxs-lookup"><span data-stu-id="8a1d7-197">Detailed Errors</span></span>

<span data-ttu-id="8a1d7-198">Determina se erros detalhados devem ser capturados.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-198">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="8a1d7-199">**Chave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="8a1d7-199">**Key**: detailedErrors</span></span>  
<span data-ttu-id="8a1d7-200">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="8a1d7-200">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="8a1d7-201">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="8a1d7-201">**Default**: false</span></span>  
<span data-ttu-id="8a1d7-202">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-202">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="8a1d7-203">**Variável de ambiente**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-203">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="8a1d7-204">Quando habilitado (ou quando o <a href="#environment">Ambiente</a> é definido como `Development`), o aplicativo captura exceções detalhadas.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-204">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="8a1d7-205">Ambiente</span><span class="sxs-lookup"><span data-stu-id="8a1d7-205">Environment</span></span>

<span data-ttu-id="8a1d7-206">Define o ambiente do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-206">Sets the app's environment.</span></span>

<span data-ttu-id="8a1d7-207">**Chave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="8a1d7-207">**Key**: environment</span></span>  
<span data-ttu-id="8a1d7-208">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="8a1d7-208">**Type**: *string*</span></span>  
<span data-ttu-id="8a1d7-209">**Padrão**: Production</span><span class="sxs-lookup"><span data-stu-id="8a1d7-209">**Default**: Production</span></span>  
<span data-ttu-id="8a1d7-210">**Definido usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-210">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="8a1d7-211">**Variável de ambiente**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-211">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="8a1d7-212">O ambiente pode ser definido como qualquer valor.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-212">The environment can be set to any value.</span></span> <span data-ttu-id="8a1d7-213">Os valores definidos pela estrutura incluem `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-213">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="8a1d7-214">Os valores não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-214">Values aren't case sensitive.</span></span> <span data-ttu-id="8a1d7-215">Por padrão, o *Ambiente* é lido da variável de ambiente `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-215">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="8a1d7-216">Ao usar o [Visual Studio](https://www.visualstudio.com/), variáveis de ambiente podem ser definidas no arquivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-216">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="8a1d7-217">Para obter mais informações, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-217">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="8a1d7-218">Hospedando assemblies de inicialização</span><span class="sxs-lookup"><span data-stu-id="8a1d7-218">Hosting Startup Assemblies</span></span>

<span data-ttu-id="8a1d7-219">Define os assemblies de inicialização de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-219">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="8a1d7-220">**Chave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="8a1d7-220">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="8a1d7-221">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="8a1d7-221">**Type**: *string*</span></span>  
<span data-ttu-id="8a1d7-222">**Padrão**: cadeia de caracteres vazia</span><span class="sxs-lookup"><span data-stu-id="8a1d7-222">**Default**: Empty string</span></span>  
<span data-ttu-id="8a1d7-223">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-223">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="8a1d7-224">**Variável de ambiente**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-224">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="8a1d7-225">Uma cadeia de caracteres delimitada por ponto e vírgula de assemblies de inicialização de hospedagem para carregamento na inicialização.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-225">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="8a1d7-226">Embora o valor padrão da configuração seja uma cadeia de caracteres vazia, os assemblies de inicialização de hospedagem sempre incluem o assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-226">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="8a1d7-227">Quando assemblies de inicialização de hospedagem são fornecidos, eles são adicionados ao assembly do aplicativo para carregamento quando o aplicativo compilar seus serviços comuns durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-227">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="8a1d7-228">Porta HTTPS</span><span class="sxs-lookup"><span data-stu-id="8a1d7-228">HTTPS Port</span></span>

<span data-ttu-id="8a1d7-229">Defina a porta de redirecionamento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-229">Set the HTTPS redirect port.</span></span> <span data-ttu-id="8a1d7-230">Uso em [aplicação de HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-230">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="8a1d7-231">**Chave**: https_port **Tipo**: *string*
**Padrão**: Um valor padrão não está definido.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-231">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="8a1d7-232">**Definido usando**: `UseSetting`
**Variável de ambiente**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-232">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="8a1d7-233">Hospedando assemblies de exclusão de inicialização</span><span class="sxs-lookup"><span data-stu-id="8a1d7-233">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="8a1d7-234">Uma cadeia de caracteres delimitada por ponto e vírgula de assemblies de inicialização de hospedagem para exclusão na inicialização.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-234">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="8a1d7-235">**Chave**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="8a1d7-235">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="8a1d7-236">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="8a1d7-236">**Type**: *string*</span></span>  
<span data-ttu-id="8a1d7-237">**Padrão**: cadeia de caracteres vazia</span><span class="sxs-lookup"><span data-stu-id="8a1d7-237">**Default**: Empty string</span></span>  
<span data-ttu-id="8a1d7-238">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-238">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="8a1d7-239">**Variável de ambiente**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-239">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="8a1d7-240">Preferir URLs de hospedagem</span><span class="sxs-lookup"><span data-stu-id="8a1d7-240">Prefer Hosting URLs</span></span>

<span data-ttu-id="8a1d7-241">Indica se o host deve escutar as URLs configuradas com o `WebHostBuilder` em vez daquelas configuradas com a implementação `IServer`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-241">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="8a1d7-242">**Chave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="8a1d7-242">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="8a1d7-243">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="8a1d7-243">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="8a1d7-244">**Padrão**: true</span><span class="sxs-lookup"><span data-stu-id="8a1d7-244">**Default**: true</span></span>  
<span data-ttu-id="8a1d7-245">**Definido usando**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-245">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="8a1d7-246">**Variável de ambiente**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-246">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="8a1d7-247">Impedir inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="8a1d7-247">Prevent Hosting Startup</span></span>

<span data-ttu-id="8a1d7-248">Impede o carregamento automático de assemblies de inicialização de hospedagem, incluindo assemblies de inicialização de hospedagem configurados pelo assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-248">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="8a1d7-249">Para obter mais informações, consulte <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-249">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="8a1d7-250">**Chave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="8a1d7-250">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="8a1d7-251">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="8a1d7-251">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="8a1d7-252">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="8a1d7-252">**Default**: false</span></span>  
<span data-ttu-id="8a1d7-253">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-253">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="8a1d7-254">**Variável de ambiente**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-254">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="8a1d7-255">URLs de servidor</span><span class="sxs-lookup"><span data-stu-id="8a1d7-255">Server URLs</span></span>

<span data-ttu-id="8a1d7-256">Indica os endereços IP ou endereços de host com portas e protocolos que o servidor deve escutar para solicitações.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-256">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="8a1d7-257">**Chave**: urls</span><span class="sxs-lookup"><span data-stu-id="8a1d7-257">**Key**: urls</span></span>  
<span data-ttu-id="8a1d7-258">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="8a1d7-258">**Type**: *string*</span></span>  
<span data-ttu-id="8a1d7-259">**Padrão**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="8a1d7-259">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="8a1d7-260">**Definido usando**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-260">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="8a1d7-261">**Variável de ambiente**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-261">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="8a1d7-262">Defina como uma lista separada por ponto e vírgula (;) de prefixos de URL aos quais o servidor deve responder.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-262">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="8a1d7-263">Por exemplo, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-263">For example, `http://localhost:123`.</span></span> <span data-ttu-id="8a1d7-264">Use "\*" para indicar que o servidor deve escutar solicitações em qualquer endereço IP ou nome do host usando a porta e o protocolo especificados (por exemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-264">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="8a1d7-265">O protocolo (`http://` ou `https://`) deve ser incluído com cada URL.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-265">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="8a1d7-266">Os formatos compatíveis variam dependendo dos servidores.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-266">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="8a1d7-267">O Kestrel tem sua própria API de configuração de ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-267">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="8a1d7-268">Para obter mais informações, consulte <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-268">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="8a1d7-269">Tempo limite de desligamento</span><span class="sxs-lookup"><span data-stu-id="8a1d7-269">Shutdown Timeout</span></span>

<span data-ttu-id="8a1d7-270">Especifica o tempo de espera para o desligamento do host da Web.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-270">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="8a1d7-271">**Chave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="8a1d7-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="8a1d7-272">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="8a1d7-272">**Type**: *int*</span></span>  
<span data-ttu-id="8a1d7-273">**Padrão**: 5</span><span class="sxs-lookup"><span data-stu-id="8a1d7-273">**Default**: 5</span></span>  
<span data-ttu-id="8a1d7-274">**Definido usando**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="8a1d7-275">**Variável de ambiente**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="8a1d7-276">Embora a chave aceite um *int* com `UseSetting` (por exemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), o método de extensão [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) usa um [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="8a1d7-277">Durante o período de tempo limite, a hospedagem:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-277">During the timeout period, hosting:</span></span>

* <span data-ttu-id="8a1d7-278">Dispara [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-278">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="8a1d7-279">Tenta parar os serviços hospedados, registrando em log os erros dos serviços que falham ao parar.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-279">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="8a1d7-280">Se o período de tempo limite expirar antes que todos os serviços hospedados parem, os serviços ativos restantes serão parados quando o aplicativo for desligado.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-280">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="8a1d7-281">Os serviços serão parados mesmo se ainda não tiverem concluído o processamento.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-281">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="8a1d7-282">Se os serviços exigirem mais tempo para parar, aumente o tempo limite.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-282">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="8a1d7-283">Assembly de inicialização</span><span class="sxs-lookup"><span data-stu-id="8a1d7-283">Startup Assembly</span></span>

<span data-ttu-id="8a1d7-284">Determina o assembly para pesquisar pela classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-284">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="8a1d7-285">**Chave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="8a1d7-285">**Key**: startupAssembly</span></span>  
<span data-ttu-id="8a1d7-286">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="8a1d7-286">**Type**: *string*</span></span>  
<span data-ttu-id="8a1d7-287">**Padrão**: o assembly do aplicativo</span><span class="sxs-lookup"><span data-stu-id="8a1d7-287">**Default**: The app's assembly</span></span>  
<span data-ttu-id="8a1d7-288">**Definido usando**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-288">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="8a1d7-289">**Variável de ambiente**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-289">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="8a1d7-290">O assembly por nome (`string`) ou por tipo (`TStartup`) pode ser referenciado.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-290">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="8a1d7-291">Se vários métodos `UseStartup` forem chamados, o último terá precedência.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-291">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="8a1d7-292">Raiz da Web</span><span class="sxs-lookup"><span data-stu-id="8a1d7-292">Web Root</span></span>

<span data-ttu-id="8a1d7-293">Define o caminho relativo para os ativos estáticos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="8a1d7-294">**Chave**: webroot</span><span class="sxs-lookup"><span data-stu-id="8a1d7-294">**Key**: webroot</span></span>  
<span data-ttu-id="8a1d7-295">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="8a1d7-295">**Type**: *string*</span></span>  
<span data-ttu-id="8a1d7-296">**Padrão**: se não for especificado, o padrão será "(Raiz do conteúdo)/wwwroot", se o caminho existir.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="8a1d7-297">Se o caminho não existir, um provedor de arquivo não operacional será usado.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="8a1d7-298">**Definido usando**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="8a1d7-299">**Variável de ambiente**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="8a1d7-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="8a1d7-300">Substituir configuração</span><span class="sxs-lookup"><span data-stu-id="8a1d7-300">Override configuration</span></span>

<span data-ttu-id="8a1d7-301">Use [Configuração](xref:fundamentals/configuration/index) para configurar o host Web.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-301">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="8a1d7-302">No exemplo a seguir, a configuração do host é especificada, opcionalmente, em um arquivo *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-302">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="8a1d7-303">Qualquer configuração carregada do arquivo *hostsettings.json* pode ser substituída por argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-303">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="8a1d7-304">A configuração de build (no `config`) é usada para configurar o host com [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-304">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="8a1d7-305">A configuração `IWebHostBuilder` é adicionada à configuração do aplicativo, mas o contrário não é verdade&mdash;`ConfigureAppConfiguration` não afeta a configuração `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-305">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="8a1d7-306">Substituição da configuração fornecida por `UseUrls` pela configuração de *hostsettings.json* primeiro, e pela configuração de argumento da linha de comando depois:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-306">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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
            });
    }
}
```

<span data-ttu-id="8a1d7-307">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-307">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="8a1d7-308">Atualmente, o método de extensão [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) não é capaz de analisar uma seção de configuração retornada por `GetSection` (por exemplo, `.UseConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-308">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="8a1d7-309">O método `GetSection` filtra as chaves de configuração da seção solicitada, mas deixa o nome da seção nas chaves (por exemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-309">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="8a1d7-310">O método `UseConfiguration` espera que as chaves correspondam às chaves `WebHostBuilder` (por exemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-310">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="8a1d7-311">A presença do nome da seção nas chaves impede que os valores da seção configurem o host.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-311">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="8a1d7-312">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-312">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="8a1d7-313">Para obter mais informações e soluções alternativas, consulte [Passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-313">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="8a1d7-314">`UseConfiguration` somente copia as chaves do `IConfiguration` fornecido para a configuração do construtor de host.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-314">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="8a1d7-315">Portanto, definir `reloadOnChange: true` para arquivos de configuração JSON, INI e XML não tem nenhum efeito.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-315">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="8a1d7-316">Para especificar o host executado em uma URL específica, o valor desejado pode ser passado em um prompt de comando ao executar [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="8a1d7-316">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="8a1d7-317">O argumento de linha de comando substitui o valor `urls` do arquivo *hostsettings.json* e o servidor escuta na porta 8080:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-317">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="8a1d7-318">Gerenciar o host</span><span class="sxs-lookup"><span data-stu-id="8a1d7-318">Manage the host</span></span>

<span data-ttu-id="8a1d7-319">**Executar**</span><span class="sxs-lookup"><span data-stu-id="8a1d7-319">**Run**</span></span>

<span data-ttu-id="8a1d7-320">O método `Run` inicia o aplicativo Web e bloqueia o thread de chamada até que o host seja desligado:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-320">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="8a1d7-321">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="8a1d7-321">**Start**</span></span>

<span data-ttu-id="8a1d7-322">Execute o host sem bloqueio, chamando seu método `Start`:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-322">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="8a1d7-323">Se uma lista de URLs for passada para o método `Start`, ele escutará nas URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-323">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="8a1d7-324">O aplicativo pode inicializar e iniciar um novo host usando os padrões pré-configurados de `CreateDefaultBuilder`, usando um método estático conveniente.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-324">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="8a1d7-325">Esses métodos iniciam o servidor sem uma saída do console e com [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) e aguardam uma quebra (Ctrl-C/SIGINT ou SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="8a1d7-325">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="8a1d7-326">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="8a1d7-326">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="8a1d7-327">Inicie com um `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-327">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="8a1d7-328">Faça uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Olá, Mundo!"</span><span class="sxs-lookup"><span data-stu-id="8a1d7-328">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="8a1d7-329">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-329">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="8a1d7-330">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-330">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="8a1d7-331">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="8a1d7-331">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="8a1d7-332">Inicie com uma URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-332">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="8a1d7-333">Produz o mesmo resultado que **Start(RequestDelegate app)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-333">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="8a1d7-334">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="8a1d7-334">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="8a1d7-335">Use uma instância de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar o middleware de roteamento:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-335">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="8a1d7-336">Use as seguintes solicitações de navegador com o exemplo:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-336">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="8a1d7-337">Solicitação</span><span class="sxs-lookup"><span data-stu-id="8a1d7-337">Request</span></span>                                    | <span data-ttu-id="8a1d7-338">Resposta</span><span class="sxs-lookup"><span data-stu-id="8a1d7-338">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="8a1d7-339">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="8a1d7-339">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="8a1d7-340">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="8a1d7-340">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="8a1d7-341">Gera uma exceção com a cadeia de caracteres "ooops!"</span><span class="sxs-lookup"><span data-stu-id="8a1d7-341">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="8a1d7-342">Gera uma exceção com a cadeia de caracteres "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="8a1d7-342">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="8a1d7-343">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="8a1d7-343">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="8a1d7-344">Olá, Mundo!</span><span class="sxs-lookup"><span data-stu-id="8a1d7-344">Hello World!</span></span>                             |

<span data-ttu-id="8a1d7-345">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-345">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="8a1d7-346">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-346">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="8a1d7-347">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="8a1d7-347">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="8a1d7-348">Use uma URL e uma instância de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-348">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="8a1d7-349">Produz o mesmo resultado que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-349">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="8a1d7-350">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="8a1d7-350">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="8a1d7-351">Forneça um delegado para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-351">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="8a1d7-352">Faça uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Olá, Mundo!"</span><span class="sxs-lookup"><span data-stu-id="8a1d7-352">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="8a1d7-353">`WaitForShutdown` bloqueia até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-353">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="8a1d7-354">O aplicativo exibe a mensagem `Console.WriteLine` e aguarda um pressionamento de tecla para ser encerrado.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-354">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="8a1d7-355">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="8a1d7-355">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="8a1d7-356">Forneça um delegado e uma URL para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-356">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="8a1d7-357">Produz o mesmo resultado que **StartWith(Action&lt;IApplicationBuilder&gt; app)**, mas o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-357">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="8a1d7-358">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="8a1d7-358">IHostingEnvironment interface</span></span>

<span data-ttu-id="8a1d7-359">A [interface IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornece informações sobre o ambiente de hospedagem na Web do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-359">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="8a1d7-360">Use a [injeção de construtor](xref:fundamentals/dependency-injection) para obter o `IHostingEnvironment` para usar suas propriedades e métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-360">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="8a1d7-361">Uma [abordagem baseada em convenção](xref:fundamentals/environments#environment-based-startup-class-and-methods) pode ser usada para configurar o aplicativo na inicialização com base no ambiente.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-361">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="8a1d7-362">Como alternativa, injete o `IHostingEnvironment` no construtor `Startup` para uso em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-362">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="8a1d7-363">Além do método de extensão `IsDevelopment`, `IHostingEnvironment` oferece os métodos `IsStaging`, `IsProduction` e `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-363">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="8a1d7-364">Para obter mais informações, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-364">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="8a1d7-365">O serviço `IHostingEnvironment` também pode ser injetado diretamente no método `Configure` para configurar o pipeline de processamento:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-365">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="8a1d7-366">`IHostingEnvironment` pode ser injetado no método `Invoke` ao criar um [middleware](xref:fundamentals/middleware/index#write-middleware) personalizado:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-366">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="8a1d7-367">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="8a1d7-367">IApplicationLifetime interface</span></span>

<span data-ttu-id="8a1d7-368">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite atividades pós-inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-368">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="8a1d7-369">Três propriedades na interface são tokens de cancelamento usados para registrar métodos `Action` que definem eventos de inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-369">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="8a1d7-370">Token de cancelamento</span><span class="sxs-lookup"><span data-stu-id="8a1d7-370">Cancellation Token</span></span>    | <span data-ttu-id="8a1d7-371">Acionado quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="8a1d7-371">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="8a1d7-372">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="8a1d7-372">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="8a1d7-373">O host foi iniciado totalmente.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-373">The host has fully started.</span></span> |
| [<span data-ttu-id="8a1d7-374">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="8a1d7-374">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="8a1d7-375">O host está concluindo um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-375">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="8a1d7-376">Todas as solicitações devem ser processadas.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-376">All requests should be processed.</span></span> <span data-ttu-id="8a1d7-377">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-377">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="8a1d7-378">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="8a1d7-378">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="8a1d7-379">O host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-379">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="8a1d7-380">Solicitações ainda podem estar sendo processadas.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-380">Requests may still be processing.</span></span> <span data-ttu-id="8a1d7-381">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-381">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="8a1d7-382">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) solicita o término do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-382">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="8a1d7-383">A classe a seguir usa `StopApplication` para desligar normalmente um aplicativo quando o método `Shutdown` da classe é chamado:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-383">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="8a1d7-384">Validação de escopo</span><span class="sxs-lookup"><span data-stu-id="8a1d7-384">Scope validation</span></span>

<span data-ttu-id="8a1d7-385">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) define [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) como `true` quando o ambiente do aplicativo é de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-385">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="8a1d7-386">Quando `ValidateScopes` está definido como `true`, o provedor de serviço padrão executa verificações para saber se:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-386">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="8a1d7-387">Os serviços com escopo não são resolvidos direta ou indiretamente pelo provedor de serviço raiz.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-387">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="8a1d7-388">Os serviços com escopo não são injetados direta ou indiretamente em singletons.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-388">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="8a1d7-389">O provedor de serviços raiz é criado quando [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) é chamado.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-389">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="8a1d7-390">O tempo de vida do provedor de serviço raiz corresponde ao tempo de vida do aplicativo/servidor quando o provedor começa com o aplicativo e é descartado quando o aplicativo é desligado.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-390">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="8a1d7-391">Os serviços com escopo são descartados pelo contêiner que os criou.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-391">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="8a1d7-392">Se um serviço com escopo é criado no contêiner raiz, o tempo de vida do serviço é promovido efetivamente para singleton, porque ele só é descartado pelo contêiner raiz quando o aplicativo/servidor é desligado.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-392">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="8a1d7-393">A validação dos escopos de serviço detecta essas situações quando `BuildServiceProvider` é chamado.</span><span class="sxs-lookup"><span data-stu-id="8a1d7-393">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="8a1d7-394">Para que os escopos sempre sejam validados, incluindo no ambiente de produção, configure [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) com [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) no construtor do host:</span><span class="sxs-lookup"><span data-stu-id="8a1d7-394">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="8a1d7-395">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="8a1d7-395">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
