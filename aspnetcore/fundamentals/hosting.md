---
title: "Hospedagem no núcleo do ASP.NET"
author: guardrex
description: "Saiba mais sobre o host da web em ASP.NET Core, que é responsável pelo gerenciamento de inicialização e o tempo de vida do aplicativo."
keywords: Host, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime da web do ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/10/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1a789ff1bc6b3e3af99419e7d74d3fb46bb2345
ms.sourcegitcommit: 368aabde4de3728a8e5a8c016a2ec61f9c0854bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="69034-104">Hospedagem no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="69034-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="69034-105">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="69034-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="69034-106">Aplicativos ASP.NET Core configurar e iniciar um *host*, que é responsável pelo gerenciamento de inicialização e o tempo de vida do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69034-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="69034-107">No mínimo, o host configura um servidor e um pipeline de processamento de solicitação.</span><span class="sxs-lookup"><span data-stu-id="69034-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="69034-108">Configurando um host</span><span class="sxs-lookup"><span data-stu-id="69034-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="69034-110">Criar um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="69034-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="69034-111">Isso normalmente é executado no ponto de entrada do aplicativo, o `Main` método.</span><span class="sxs-lookup"><span data-stu-id="69034-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="69034-112">Nos modelos de projeto, `Main` está localizado em *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="69034-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="69034-113">Um típico *Program.cs* chamadas [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para começar a configurar um host:</span><span class="sxs-lookup"><span data-stu-id="69034-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="69034-114">`CreateDefaultBuilder`executa as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="69034-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="69034-115">Configura [Kestrel](servers/kestrel.md) como o servidor web.</span><span class="sxs-lookup"><span data-stu-id="69034-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span>
* <span data-ttu-id="69034-116">Define a raiz de conteúdo [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="69034-116">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="69034-117">Configuração opcional de cargas de:</span><span class="sxs-lookup"><span data-stu-id="69034-117">Loads optional configuration from:</span></span>
  * <span data-ttu-id="69034-118">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="69034-118">*appsettings.json*.</span></span>
  * <span data-ttu-id="69034-119">*appSettings. . JSON de {ambiente}*.</span><span class="sxs-lookup"><span data-stu-id="69034-119">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="69034-120">[Segredos do usuário](xref:security/app-secrets) quando o aplicativo é executado no `Development` ambiente.</span><span class="sxs-lookup"><span data-stu-id="69034-120">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="69034-121">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="69034-121">Environment variables.</span></span>
  * <span data-ttu-id="69034-122">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="69034-122">Command-line arguments.</span></span>
* <span data-ttu-id="69034-123">Configura [log](xref:fundamentals/logging) para a saída do console e de depuração com [filtragem de log](xref:fundamentals/logging#log-filtering) regras especificadas em uma seção de configuração de log de um *appSettings. JSON* ou *appsettings. . JSON de {ambiente}* arquivo.</span><span class="sxs-lookup"><span data-stu-id="69034-123">Configures [logging](xref:fundamentals/logging) for console and debug output with [log filtering](xref:fundamentals/logging#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="69034-124">Quando em execução por trás do IIS, permite a integração do IIS, configurando o caminho base e a porta que o servidor deve escutar ao usar o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="69034-124">When running behind IIS, enables IIS integration by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="69034-125">O módulo cria um proxy reverso entre Kestrel e o IIS.</span><span class="sxs-lookup"><span data-stu-id="69034-125">The module creates a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="69034-126">Também configura o aplicativo [capturar erros de inicialização](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="69034-126">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span>

<span data-ttu-id="69034-127">O *conteúdo raiz* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="69034-127">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="69034-128">A raiz de conteúdo padrão é [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="69034-128">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="69034-129">Isso resulta em usando a pasta raiz do projeto da web como a raiz de conteúdo quando o aplicativo é iniciado na pasta raiz (por exemplo, chamar [dotnet executar](/dotnet/core/tools/dotnet-run) da pasta de projeto).</span><span class="sxs-lookup"><span data-stu-id="69034-129">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="69034-130">Esse é o padrão usado em [Visual Studio](https://www.visualstudio.com/) e [dotnet novos modelos](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="69034-130">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="69034-131">Consulte [configuração no ASP.NET Core](xref:fundamentals/configuration) para obter mais informações sobre a configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69034-131">See [Configuration in ASP.NET Core](xref:fundamentals/configuration) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="69034-132">Como uma alternativa ao uso estático `CreateDefaultBuilder` método, criando um host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) é uma abordagem com suporte com o ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="69034-132">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="69034-133">Consulte a guia de 1. x do ASP.NET Core para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="69034-133">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-134">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-134">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69034-135">Criar um host usando uma instância de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="69034-135">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="69034-136">Isso normalmente é executado no ponto de entrada do aplicativo, o `Main` método.</span><span class="sxs-lookup"><span data-stu-id="69034-136">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="69034-137">Nos modelos de projeto, `Main` está localizado em *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="69034-137">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="69034-138">O seguinte *Program.cs* demonstra como usar `WebHostBuilder` para criar o host:</span><span class="sxs-lookup"><span data-stu-id="69034-138">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="69034-139">`WebHostBuilder`requer um [server que implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="69034-139">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="69034-140">Os servidores internos são [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (antes da versão do ASP.NET Core 2.0, o HTTP.sys foi chamado [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="69034-140">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="69034-141">Neste exemplo, o [o método de extensão UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Especifica o servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="69034-141">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="69034-142">O *conteúdo raiz* determina onde o host procura por arquivos de conteúdo, como arquivos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="69034-142">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="69034-143">A raiz de conteúdo padrão fornecido a `UseContentRoot` é [GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="69034-143">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="69034-144">Isso resulta em usando a pasta raiz do projeto da web como a raiz de conteúdo quando o aplicativo é iniciado na pasta raiz (por exemplo, chamar [dotnet executar](/dotnet/core/tools/dotnet-run) da pasta de projeto).</span><span class="sxs-lookup"><span data-stu-id="69034-144">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="69034-145">Esse é o padrão usado em [Visual Studio](https://www.visualstudio.com/) e [dotnet novos modelos](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="69034-145">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="69034-146">Para usar o IIS como um proxy reverso, chame [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte da construção do host.</span><span class="sxs-lookup"><span data-stu-id="69034-146">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="69034-147">`UseIISIntegration`não configure um *servidor*, como [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="69034-147">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="69034-148">`UseIISIntegration`configura o caminho base e a porta que o servidor deve escutar ao usar o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) para criar um proxy reverso entre Kestrel e o IIS.</span><span class="sxs-lookup"><span data-stu-id="69034-148">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="69034-149">Para usar o IIS com o ASP.NET Core, você deve especificar `UseKestrel` e `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="69034-149">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="69034-150">`UseIISIntegration`ativa somente quando em execução por trás do IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="69034-150">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="69034-151">Para obter mais informações, consulte [Introdução ao ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) e [referência de configuração do módulo do ASP.NET Core](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="69034-151">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="69034-152">Uma implementação mínima que configura um host (e um aplicativo ASP.NET Core) inclui a especificação de um servidor e a configuração do canal de solicitação do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="69034-152">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="69034-153">Ao configurar um host, você pode fornecer [configurar](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) métodos.</span><span class="sxs-lookup"><span data-stu-id="69034-153">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="69034-154">Se você especificar um `Startup` classe, deve definir um `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="69034-154">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="69034-155">Para obter mais informações, consulte [inicialização do aplicativo no ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="69034-155">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="69034-156">Diversas chamadas para `ConfigureServices` acrescentar um ao outro.</span><span class="sxs-lookup"><span data-stu-id="69034-156">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="69034-157">Diversas chamadas para `Configure` ou `UseStartup` no `WebHostBuilder` substituir as configurações anteriores.</span><span class="sxs-lookup"><span data-stu-id="69034-157">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="69034-158">Valores de configuração do host</span><span class="sxs-lookup"><span data-stu-id="69034-158">Host configuration values</span></span>

<span data-ttu-id="69034-159">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) fornece métodos para definir a maioria dos valores de configuração disponíveis para o host, que também pode ser definida diretamente com [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e a chave associada.</span><span class="sxs-lookup"><span data-stu-id="69034-159">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="69034-160">Ao definir um valor com `UseSetting`, o valor é definido como uma cadeia de caracteres (entre aspas), independentemente do tipo.</span><span class="sxs-lookup"><span data-stu-id="69034-160">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="69034-161">Capturar erros de inicialização</span><span class="sxs-lookup"><span data-stu-id="69034-161">Capture Startup Errors</span></span>

<span data-ttu-id="69034-162">Essa configuração controla a captura de erros de inicialização.</span><span class="sxs-lookup"><span data-stu-id="69034-162">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="69034-163">**Chave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="69034-163">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="69034-164">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="69034-164">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="69034-165">**Padrão**: assume como padrão `false` , a menos que o aplicativo é executado com Kestrel por trás do IIS, em que o padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="69034-165">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="69034-166">**Definido usando**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="69034-166">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="69034-167">Quando `false`, erros durante o resultado de inicialização no host sair.</span><span class="sxs-lookup"><span data-stu-id="69034-167">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="69034-168">Quando `true`, o host captura exceções durante a inicialização e tenta iniciar o servidor.</span><span class="sxs-lookup"><span data-stu-id="69034-168">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="69034-171">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="69034-171">Content Root</span></span>

<span data-ttu-id="69034-172">Essa configuração determina onde o ASP.NET Core começa a procurar por arquivos de conteúdo, como exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="69034-172">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="69034-173">**Chave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="69034-173">**Key**: contentRoot</span></span>  
<span data-ttu-id="69034-174">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="69034-174">**Type**: *string*</span></span>  
<span data-ttu-id="69034-175">**Padrão**: padrão é a pasta em que reside o assembly de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69034-175">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="69034-176">**Definido usando**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="69034-176">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="69034-177">A conteúdo raiz também é usada como o caminho base para o [configuração Web raiz](#web-root).</span><span class="sxs-lookup"><span data-stu-id="69034-177">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="69034-178">Se o caminho não existir, o host não será iniciado.</span><span class="sxs-lookup"><span data-stu-id="69034-178">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="69034-181">Erros detalhados</span><span class="sxs-lookup"><span data-stu-id="69034-181">Detailed Errors</span></span>

<span data-ttu-id="69034-182">Determina se detalhado erros devem ser capturados.</span><span class="sxs-lookup"><span data-stu-id="69034-182">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="69034-183">**Chave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="69034-183">**Key**: detailedErrors</span></span>  
<span data-ttu-id="69034-184">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="69034-184">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="69034-185">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="69034-185">**Default**: false</span></span>  
<span data-ttu-id="69034-186">**Definido usando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="69034-186">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="69034-187">Quando habilitado (ou quando o <a href="#environment">ambiente</a> é definido como `Development`), o aplicativo captura exceções detalhadas.</span><span class="sxs-lookup"><span data-stu-id="69034-187">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-189">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-189">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="69034-190">Ambiente</span><span class="sxs-lookup"><span data-stu-id="69034-190">Environment</span></span>

<span data-ttu-id="69034-191">Define o ambiente do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69034-191">Sets the app's environment.</span></span>

<span data-ttu-id="69034-192">**Chave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="69034-192">**Key**: environment</span></span>  
<span data-ttu-id="69034-193">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="69034-193">**Type**: *string*</span></span>  
<span data-ttu-id="69034-194">**Padrão**: produção</span><span class="sxs-lookup"><span data-stu-id="69034-194">**Default**: Production</span></span>  
<span data-ttu-id="69034-195">**Definido usando**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="69034-195">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="69034-196">Você pode definir o *ambiente* para qualquer valor.</span><span class="sxs-lookup"><span data-stu-id="69034-196">You can set the *Environment* to any value.</span></span> <span data-ttu-id="69034-197">Incluem valores definidos pelo Framework `Development`, `Staging`, e `Production`.</span><span class="sxs-lookup"><span data-stu-id="69034-197">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="69034-198">Valores não diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="69034-198">Values aren't case sensitive.</span></span> <span data-ttu-id="69034-199">Por padrão, o *ambiente* são lidos a partir de `ASPNETCORE_ENVIRONMENT` variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="69034-199">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="69034-200">Ao usar [Visual Studio](https://www.visualstudio.com/), variáveis de ambiente podem ser definidas no *launchSettings.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="69034-200">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="69034-201">Para obter mais informações, consulte [Trabalhando com vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="69034-201">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-202">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-202">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-203">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-203">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="69034-204">Assemblies de inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="69034-204">Hosting Startup Assemblies</span></span>

<span data-ttu-id="69034-205">Define a hospedagem assemblies de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69034-205">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="69034-206">**Chave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="69034-206">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="69034-207">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="69034-207">**Type**: *string*</span></span>  
<span data-ttu-id="69034-208">**Padrão**: cadeia de caracteres vazia</span><span class="sxs-lookup"><span data-stu-id="69034-208">**Default**: Empty string</span></span>  
<span data-ttu-id="69034-209">**Definido usando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="69034-209">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="69034-210">Uma cadeia delimitada por ponto e vírgula de hospedagem assemblies de inicialização para carregar na inicialização.</span><span class="sxs-lookup"><span data-stu-id="69034-210">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="69034-211">Esse recurso é novo no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="69034-211">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="69034-212">Embora o valor de configuração padrão é uma cadeia de caracteres vazia, os assemblies de inicialização hospedagem sempre incluem o assembly do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69034-212">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="69034-213">Quando você fornece hospedagem assemblies de inicialização, elas são adicionadas ao assembly do aplicativo de carregamento quando o aplicativo cria seus serviços comuns durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="69034-213">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69034-216">Este recurso está indisponível no núcleo do ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="69034-216">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="69034-217">Preferir URLs de hospedagem</span><span class="sxs-lookup"><span data-stu-id="69034-217">Prefer Hosting URLs</span></span>

<span data-ttu-id="69034-218">Indica se o host deve escutar nas URLs configuradas com o `WebHostBuilder` em vez das configuradas com o `IServer` implementação.</span><span class="sxs-lookup"><span data-stu-id="69034-218">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="69034-219">**Chave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="69034-219">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="69034-220">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="69034-220">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="69034-221">**Padrão**: true</span><span class="sxs-lookup"><span data-stu-id="69034-221">**Default**: true</span></span>  
<span data-ttu-id="69034-222">**Definido usando**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="69034-222">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="69034-223">Esse recurso é novo no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="69034-223">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-224">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-224">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-225">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-225">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69034-226">Este recurso está indisponível no núcleo do ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="69034-226">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="69034-227">Impedir a inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="69034-227">Prevent Hosting Startup</span></span>

<span data-ttu-id="69034-228">Impede o carregamento automático de assemblies de inicialização, incluindo o assembly do aplicativo de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="69034-228">Prevents the automatic loading of hosting startup assemblies, including the app's assembly.</span></span>

<span data-ttu-id="69034-229">**Chave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="69034-229">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="69034-230">**Tipo**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="69034-230">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="69034-231">**Padrão**: falso</span><span class="sxs-lookup"><span data-stu-id="69034-231">**Default**: false</span></span>  
<span data-ttu-id="69034-232">**Definido usando**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="69034-232">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="69034-233">Esse recurso é novo no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="69034-233">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-234">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-234">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-235">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-235">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69034-236">Este recurso está indisponível no núcleo do ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="69034-236">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="69034-237">URLs de servidor</span><span class="sxs-lookup"><span data-stu-id="69034-237">Server URLs</span></span>

<span data-ttu-id="69034-238">Indica os endereços IP ou endereços de host com as portas e protocolos que o servidor deve ouvir para solicitações.</span><span class="sxs-lookup"><span data-stu-id="69034-238">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="69034-239">**Chave**: urls</span><span class="sxs-lookup"><span data-stu-id="69034-239">**Key**: urls</span></span>  
<span data-ttu-id="69034-240">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="69034-240">**Type**: *string*</span></span>  
<span data-ttu-id="69034-241">**Padrão**: http://localhost:5000/</span><span class="sxs-lookup"><span data-stu-id="69034-241">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="69034-242">**Definido usando**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="69034-242">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="69034-243">Definir um separados por ponto e vírgula (;) prefixos de lista de URL para o servidor deve responder.</span><span class="sxs-lookup"><span data-stu-id="69034-243">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="69034-244">Por exemplo, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="69034-244">For example, `http://localhost:123`.</span></span> <span data-ttu-id="69034-245">Use "\*" para indicar que o servidor deve escutar solicitações em qualquer endereço IP ou nome do host usando o protocolo e porta especificada (por exemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="69034-245">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="69034-246">O protocolo (`http://` ou `https://`) devem ser incluídas com cada URL.</span><span class="sxs-lookup"><span data-stu-id="69034-246">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="69034-247">Os formatos com suporte variam entre servidores.</span><span class="sxs-lookup"><span data-stu-id="69034-247">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-248">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-248">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="69034-249">Kestrel tem sua própria API de configuração do ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="69034-249">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="69034-250">Para obter mais informações, consulte [Implementação do servidor Web Kestrel no ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="69034-250">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="69034-252">Tempo limite de desligamento</span><span class="sxs-lookup"><span data-stu-id="69034-252">Shutdown Timeout</span></span>

<span data-ttu-id="69034-253">Especifica a quantidade de tempo de espera para o host da web para o desligamento.</span><span class="sxs-lookup"><span data-stu-id="69034-253">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="69034-254">**Chave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="69034-254">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="69034-255">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="69034-255">**Type**: *int*</span></span>  
<span data-ttu-id="69034-256">**Padrão**: 5</span><span class="sxs-lookup"><span data-stu-id="69034-256">**Default**: 5</span></span>  
<span data-ttu-id="69034-257">**Definido usando**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="69034-257">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="69034-258">Embora a chave aceita um *int* com `UseSetting` (por exemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), o `UseShutdownTimeout` método de extensão usa um `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="69034-258">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="69034-259">Esse recurso é novo no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="69034-259">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-260">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-260">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-261">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-261">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69034-262">Este recurso está indisponível no núcleo do ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="69034-262">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="69034-263">Assembly de inicialização</span><span class="sxs-lookup"><span data-stu-id="69034-263">Startup Assembly</span></span>

<span data-ttu-id="69034-264">Determina o assembly para pesquisar o `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="69034-264">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="69034-265">**Chave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="69034-265">**Key**: startupAssembly</span></span>  
<span data-ttu-id="69034-266">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="69034-266">**Type**: *string*</span></span>  
<span data-ttu-id="69034-267">**Padrão**: assembly do aplicativo</span><span class="sxs-lookup"><span data-stu-id="69034-267">**Default**: The app's assembly</span></span>  
<span data-ttu-id="69034-268">**Definido usando**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="69034-268">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="69034-269">Você pode fazer referência ao assembly por nome (`string`) ou tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="69034-269">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="69034-270">Se vários `UseStartup` métodos são chamados, último terá precedência.</span><span class="sxs-lookup"><span data-stu-id="69034-270">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-271">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-271">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-272">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-272">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="69034-273">Raiz da Web</span><span class="sxs-lookup"><span data-stu-id="69034-273">Web Root</span></span>

<span data-ttu-id="69034-274">Define o caminho relativo para ativos estático do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69034-274">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="69034-275">**Chave**: webroot</span><span class="sxs-lookup"><span data-stu-id="69034-275">**Key**: webroot</span></span>  
<span data-ttu-id="69034-276">**Tipo**: *cadeia de caracteres*</span><span class="sxs-lookup"><span data-stu-id="69034-276">**Type**: *string*</span></span>  
<span data-ttu-id="69034-277">**Padrão**: se não for especificado, o padrão é "(Content Root)/wwwroot", se o caminho existe.</span><span class="sxs-lookup"><span data-stu-id="69034-277">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="69034-278">Se o caminho não existir, é usado um provedor de arquivo não-operacional.</span><span class="sxs-lookup"><span data-stu-id="69034-278">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="69034-279">**Definido usando**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="69034-279">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-280">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-280">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-281">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-281">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="69034-282">Configuração de substituição</span><span class="sxs-lookup"><span data-stu-id="69034-282">Overriding configuration</span></span>

<span data-ttu-id="69034-283">Use [configuração](configuration.md) para configurar o host.</span><span class="sxs-lookup"><span data-stu-id="69034-283">Use [Configuration](configuration.md) to configure the host.</span></span> <span data-ttu-id="69034-284">No exemplo a seguir, a configuração do host é especificada, opcionalmente, em um *hosting.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="69034-284">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="69034-285">Qualquer configuração carregados a partir de *hosting.json* arquivo pode ser substituído pelos argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="69034-285">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="69034-286">A configuração interna (no `config`) é usado para configurar o host com `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="69034-286">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-287">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-287">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="69034-288">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="69034-288">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="69034-289">Substituindo a configuração fornecida pelo `UseUrls` com *hosting.json* config primeiro argumento de linha de comando config segundo:</span><span class="sxs-lookup"><span data-stu-id="69034-289">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-290">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-290">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69034-291">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="69034-291">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="69034-292">Substituindo a configuração fornecida pelo `UseUrls` com *hosting.json* config primeiro argumento de linha de comando config segundo:</span><span class="sxs-lookup"><span data-stu-id="69034-292">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="69034-293">O `UseConfiguration` método de extensão não está atualmente capaz de analisar uma seção de configuração retornada por `GetSection` (por exemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="69034-293">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="69034-294">O `GetSection` método filtra as chaves de configuração para a seção solicitada, mas deixa o nome da seção sobre as chaves (por exemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="69034-294">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="69034-295">O `UseConfiguration` método espera que as chaves para corresponder a `WebHostBuilder` chaves (por exemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="69034-295">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="69034-296">A presença do nome da seção nas chaves impede que os valores da seção Configurando o host.</span><span class="sxs-lookup"><span data-stu-id="69034-296">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="69034-297">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="69034-297">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="69034-298">Para obter mais informações e soluções alternativas, consulte [passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="69034-298">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="69034-299">Para especificar o host executado em uma URL específica, você pode transmitir o valor desejado de um prompt de comando ao executar `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="69034-299">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="69034-300">O argumento de linha de comando substitui o `urls` valor o *hosting.json* arquivo e o servidor escuta na porta 8080:</span><span class="sxs-lookup"><span data-stu-id="69034-300">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="69034-301">Ordem de importância</span><span class="sxs-lookup"><span data-stu-id="69034-301">Ordering importance</span></span>

<span data-ttu-id="69034-302">Alguns do `WebHostBuilder` configurações primeiro são lidos a partir de variáveis de ambiente, se definido.</span><span class="sxs-lookup"><span data-stu-id="69034-302">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="69034-303">Essas variáveis de ambiente usam o formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="69034-303">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="69034-304">Para definir as URLs que o servidor escuta em por padrão, você deve definir `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="69034-304">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="69034-305">Você pode substituir qualquer um desses valores de variável de ambiente com a especificação de configuração (usando `UseConfiguration`) ou definindo o valor explicitamente (usando `UseSetting` ou um dos métodos de extensão explícito, como `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="69034-305">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="69034-306">O host usa qualquer opção define o valor da última.</span><span class="sxs-lookup"><span data-stu-id="69034-306">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="69034-307">Se você deseja definir a URL padrão para um valor programaticamente, mas permitir que ele seja substituído com a configuração, você pode usar a configuração de linha de comando depois de definir a URL.</span><span class="sxs-lookup"><span data-stu-id="69034-307">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="69034-308">Consulte [configuração substituindo](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="69034-308">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="69034-309">Iniciando o host</span><span class="sxs-lookup"><span data-stu-id="69034-309">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69034-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69034-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="69034-311">**Executar**</span><span class="sxs-lookup"><span data-stu-id="69034-311">**Run**</span></span>

<span data-ttu-id="69034-312">O `Run` método inicia o aplicativo web e bloqueia o thread de chamada até que o host é desligado:</span><span class="sxs-lookup"><span data-stu-id="69034-312">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="69034-313">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="69034-313">**Start**</span></span>

<span data-ttu-id="69034-314">Você pode executar o host de maneira sem bloqueio, chamando seu `Start` método:</span><span class="sxs-lookup"><span data-stu-id="69034-314">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="69034-315">Se você passar uma lista de URLs para o `Start` método, ele escuta em URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="69034-315">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="69034-316">Você pode inicializar e iniciar um novo host usando os padrões pré-configurado de `CreateDefaultBuilder` usando um método estático de conveniência.</span><span class="sxs-lookup"><span data-stu-id="69034-316">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="69034-317">Esses métodos de iniciar o servidor sem saída de console e [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) aguardar uma quebra (Ctrl-C/SIGINT ou SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="69034-317">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="69034-318">**Início (RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="69034-318">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="69034-319">Iniciar com um `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="69034-319">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="69034-320">Fazer uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="69034-320">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="69034-321">`WaitForShutdown`blocos até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="69034-321">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="69034-322">O aplicativo é exibido o `Console.WriteLine` mensagem e aguarda um pressionamento de tecla sair.</span><span class="sxs-lookup"><span data-stu-id="69034-322">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="69034-323">**Iniciar (cadeia de caracteres de url, RequestDelegate do aplicativo)**</span><span class="sxs-lookup"><span data-stu-id="69034-323">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="69034-324">Iniciar com uma URL e `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="69034-324">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="69034-325">Produz o mesmo resultado que **inicial (aplicativo RequestDelegate)**, exceto o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="69034-325">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="69034-326">**Iniciar (ação<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="69034-326">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="69034-327">Usar uma instância de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar o middleware de roteamento:</span><span class="sxs-lookup"><span data-stu-id="69034-327">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="69034-328">Use as seguintes solicitações de navegador com o exemplo:</span><span class="sxs-lookup"><span data-stu-id="69034-328">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="69034-329">Solicitação</span><span class="sxs-lookup"><span data-stu-id="69034-329">Request</span></span>                                    | <span data-ttu-id="69034-330">Resposta</span><span class="sxs-lookup"><span data-stu-id="69034-330">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="69034-331">Olá, Martin!</span><span class="sxs-lookup"><span data-stu-id="69034-331">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="69034-332">Dias de Buenos, Catrina!</span><span class="sxs-lookup"><span data-stu-id="69034-332">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="69034-333">Gera uma exceção com a cadeia de caracteres "ooops!"</span><span class="sxs-lookup"><span data-stu-id="69034-333">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="69034-334">Gera uma exceção com a cadeia de caracteres "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="69034-334">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="69034-335">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="69034-335">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="69034-336">Olá, mundo!</span><span class="sxs-lookup"><span data-stu-id="69034-336">Hello World!</span></span>                             |

<span data-ttu-id="69034-337">`WaitForShutdown`blocos até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="69034-337">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="69034-338">O aplicativo é exibido o `Console.WriteLine` mensagem e aguarda um pressionamento de tecla sair.</span><span class="sxs-lookup"><span data-stu-id="69034-338">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="69034-339">**Iniciar (url, a ação de cadeia de caracteres<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="69034-339">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="69034-340">Use uma URL e uma instância do `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="69034-340">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="69034-341">Produz o mesmo resultado que **iniciar (ação<IRouteBuilder> routeBuilder)**, exceto o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="69034-341">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="69034-342">**StartWith (ação<IApplicationBuilder> aplicativo)**</span><span class="sxs-lookup"><span data-stu-id="69034-342">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="69034-343">Forneça um delegado para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="69034-343">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="69034-344">Fazer uma solicitação no navegador para `http://localhost:5000` para receber a resposta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="69034-344">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="69034-345">`WaitForShutdown`blocos até que uma quebra (Ctrl-C/SIGINT ou SIGTERM) seja emitida.</span><span class="sxs-lookup"><span data-stu-id="69034-345">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="69034-346">O aplicativo é exibido o `Console.WriteLine` mensagem e aguarda um pressionamento de tecla sair.</span><span class="sxs-lookup"><span data-stu-id="69034-346">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="69034-347">**StartWith (url, a ação de cadeia de caracteres<IApplicationBuilder> aplicativo)**</span><span class="sxs-lookup"><span data-stu-id="69034-347">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="69034-348">Forneça uma URL e um delegado para configurar um `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="69034-348">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="69034-349">Produz o mesmo resultado que **StartWith (ação<IApplicationBuilder> aplicativo)**, exceto o aplicativo responde em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="69034-349">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69034-350">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69034-350">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69034-351">**Executar**</span><span class="sxs-lookup"><span data-stu-id="69034-351">**Run**</span></span>

<span data-ttu-id="69034-352">O `Run` método inicia o aplicativo web e bloqueia o thread de chamada até que o host é desligado:</span><span class="sxs-lookup"><span data-stu-id="69034-352">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="69034-353">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="69034-353">**Start**</span></span>

<span data-ttu-id="69034-354">Você pode executar o host de maneira sem bloqueio, chamando seu `Start` método:</span><span class="sxs-lookup"><span data-stu-id="69034-354">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="69034-355">Se você passar uma lista de URLs para o `Start` método, ele escuta em URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="69034-355">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="69034-356">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="69034-356">IHostingEnvironment interface</span></span>

<span data-ttu-id="69034-357">O [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornece informações sobre o ambiente de hospedagem na web do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69034-357">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="69034-358">Você pode usar [injeção de construtor](xref:fundamentals/dependency-injection) para obter o `IHostingEnvironment` para usar suas propriedades e métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="69034-358">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="69034-359">Você pode usar um [abordagem baseado em convenção](xref:fundamentals/environments#startup-conventions) para configurar seu aplicativo na inicialização com base no ambiente.</span><span class="sxs-lookup"><span data-stu-id="69034-359">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="69034-360">Como alternativa, você pode injetar o `IHostingEnvironment` para o `Startup` construtor para uso em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="69034-360">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="69034-361">Além de `IsDevelopment` método de extensão, `IHostingEnvironment` oferece `IsStaging`, `IsProduction`, e `IsEnvironment(string environmentName)` métodos.</span><span class="sxs-lookup"><span data-stu-id="69034-361">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="69034-362">Consulte [trabalhando com vários ambientes](xref:fundamentals/environments) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="69034-362">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="69034-363">O `IHostingEnvironment` serviço também pode ser inserido diretamente para o `Configure` método para configurar o pipeline de processamento:</span><span class="sxs-lookup"><span data-stu-id="69034-363">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="69034-364">Você pode injetar `IHostingEnvironment` para o `Invoke` método ao criar personalizado [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="69034-364">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="69034-365">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="69034-365">IApplicationLifetime interface</span></span>

<span data-ttu-id="69034-366">O [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite executar atividades de pós-inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="69034-366">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="69034-367">Três propriedades na interface são tokens de cancelamento que você pode registrar com `Action` métodos para definir os eventos de inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="69034-367">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="69034-368">Também há um `StopApplication` método.</span><span class="sxs-lookup"><span data-stu-id="69034-368">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="69034-369">Token de cancelamento</span><span class="sxs-lookup"><span data-stu-id="69034-369">Cancellation Token</span></span>    | <span data-ttu-id="69034-370">Acionado quando &#8230;</span><span class="sxs-lookup"><span data-stu-id="69034-370">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="69034-371">O host foi totalmente iniciado.</span><span class="sxs-lookup"><span data-stu-id="69034-371">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="69034-372">O host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="69034-372">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="69034-373">Ainda podem estar processando solicitações.</span><span class="sxs-lookup"><span data-stu-id="69034-373">Requests may still be processing.</span></span> <span data-ttu-id="69034-374">Blocos de desligamento até que esse evento seja concluída.</span><span class="sxs-lookup"><span data-stu-id="69034-374">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="69034-375">O host está concluindo um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="69034-375">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="69034-376">Todas as solicitações devem ser processadas completamente.</span><span class="sxs-lookup"><span data-stu-id="69034-376">All requests should be completely processed.</span></span> <span data-ttu-id="69034-377">Blocos de desligamento até que esse evento seja concluída.</span><span class="sxs-lookup"><span data-stu-id="69034-377">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="69034-378">Método</span><span class="sxs-lookup"><span data-stu-id="69034-378">Method</span></span>            | <span data-ttu-id="69034-379">Ação</span><span class="sxs-lookup"><span data-stu-id="69034-379">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="69034-380">Encerramento de solicitações do aplicativo atual.</span><span class="sxs-lookup"><span data-stu-id="69034-380">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="69034-381">Solucionando problemas de System. ArgumentException</span><span class="sxs-lookup"><span data-stu-id="69034-381">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="69034-382">**Aplica-se ao ASP.NET Core 2.0 apenas**</span><span class="sxs-lookup"><span data-stu-id="69034-382">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="69034-383">Se você compilar o host injetando `IStartup` diretamente para o contêiner de injeção de dependência em vez de chamar `UseStartup` ou `Configure`, você pode encontrar o seguinte erro: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="69034-383">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="69034-384">Isso ocorre porque o [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (do assembly atual) é necessário para verificar se há `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="69034-384">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="69034-385">Se você inserir manualmente `IStartup` para o contêiner de injeção de dependência, adicione a seguinte chamada a sua `WebHostBuilder` com o nome do assembly especificado:</span><span class="sxs-lookup"><span data-stu-id="69034-385">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="69034-386">Como alternativa, adicione uma cópia `Configure` para sua `WebHostBuilder`, que define o `applicationName`(`ApplicationKey`) automaticamente:</span><span class="sxs-lookup"><span data-stu-id="69034-386">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="69034-387">**Observação**: isso só é necessária com a versão 2.0 do ASP.NET Core e somente quando você não chama `UseStartup` ou `Configure`.</span><span class="sxs-lookup"><span data-stu-id="69034-387">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="69034-388">Para obter mais informações, consulte [anúncios: Microsoft.Extensions.PlatformAbstractions foi removido (comentário)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) e [StartupInjection exemplo](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="69034-388">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69034-389">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="69034-389">Additional resources</span></span>

* [<span data-ttu-id="69034-390">Publicar no Windows usando o IIS</span><span class="sxs-lookup"><span data-stu-id="69034-390">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="69034-391">Publicar no Linux usando Nginx</span><span class="sxs-lookup"><span data-stu-id="69034-391">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="69034-392">Publicar no Linux usando o Apache</span><span class="sxs-lookup"><span data-stu-id="69034-392">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="69034-393">Host em um serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="69034-393">Host in a Windows Service</span></span>](xref:hosting/windows-service)
