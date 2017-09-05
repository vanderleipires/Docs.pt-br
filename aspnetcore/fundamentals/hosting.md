---
title: Hospedando em ASP.NET Core | Microsoft Docs
author: ardalis
description: "Introdução aos hosts da web em ASP.NET Core."
keywords: ASP.NET Core, host da web, IWebHost
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a><span data-ttu-id="da8ee-104">Introdução ao ASP.NET Core de hospedagem</span><span class="sxs-lookup"><span data-stu-id="da8ee-104">Introduction to hosting in ASP.NET Core</span></span>

<span data-ttu-id="da8ee-105">Por [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="da8ee-105">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="da8ee-106">Para executar um aplicativo ASP.NET Core, é necessário configurar e iniciar um host usando `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-106">To run an ASP.NET Core app, you need to configure and launch a host using `WebHostBuilder`.</span></span>

## <a name="what-is-a-host"></a><span data-ttu-id="da8ee-107">O que é um Host?</span><span class="sxs-lookup"><span data-stu-id="da8ee-107">What is a Host?</span></span>

<span data-ttu-id="da8ee-108">Aplicativos do ASP.NET Core exigem um *host* no qual executar.</span><span class="sxs-lookup"><span data-stu-id="da8ee-108">ASP.NET Core apps require a *host* in which to execute.</span></span> <span data-ttu-id="da8ee-109">Um host deve implementar o `IWebHost` interface, que expõe a coleções de recursos e serviços, e um `Start` método.</span><span class="sxs-lookup"><span data-stu-id="da8ee-109">A host must implement the `IWebHost` interface, which exposes collections of features and services, and a `Start` method.</span></span> <span data-ttu-id="da8ee-110">O host é normalmente criado usando uma instância de um `WebHostBuilder`, que cria e retorna um `WebHost` instância.</span><span class="sxs-lookup"><span data-stu-id="da8ee-110">The host is typically created using an instance of a `WebHostBuilder`, which builds and returns a  `WebHost` instance.</span></span> <span data-ttu-id="da8ee-111">O `WebHost` referencia o servidor que manipulará as solicitações.</span><span class="sxs-lookup"><span data-stu-id="da8ee-111">The `WebHost` references the server that will handle requests.</span></span> <span data-ttu-id="da8ee-112">Saiba mais sobre [servidores](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="da8ee-112">Learn more about [servers](servers/index.md).</span></span>

### <a name="what-is-the-difference-between-a-host-and-a-server"></a><span data-ttu-id="da8ee-113">Qual é a diferença entre um host e um servidor?</span><span class="sxs-lookup"><span data-stu-id="da8ee-113">What is the difference between a host and a server?</span></span>

<span data-ttu-id="da8ee-114">O host é responsável pelo gerenciamento de inicialização e o tempo de vida do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="da8ee-114">The host is responsible for application startup and lifetime management.</span></span> <span data-ttu-id="da8ee-115">O servidor é responsável para aceitar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="da8ee-115">The server is responsible for accepting HTTP requests.</span></span> <span data-ttu-id="da8ee-116">Parte da responsabilidade do host inclui garantir a que serviços do aplicativo e o servidor estiverem disponível e adequadamente configurado.</span><span class="sxs-lookup"><span data-stu-id="da8ee-116">Part of the host's responsibility includes ensuring the application's services and the server are available and properly configured.</span></span> <span data-ttu-id="da8ee-117">Você pode pensar o host como um wrapper em torno do servidor.</span><span class="sxs-lookup"><span data-stu-id="da8ee-117">You can think of the host as being a wrapper around the server.</span></span> <span data-ttu-id="da8ee-118">O host está configurado para usar um servidor específico. o servidor não está ciente do seu host.</span><span class="sxs-lookup"><span data-stu-id="da8ee-118">The host is configured to use a particular server; the server is unaware of its host.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="da8ee-119">Configurando um Host</span><span class="sxs-lookup"><span data-stu-id="da8ee-119">Setting up a Host</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="da8ee-120">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="da8ee-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="da8ee-121">Criar um host usando uma instância de `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-121">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="da8ee-122">Isso geralmente é feito no ponto de entrada do aplicativo: `public static void Main` (que nos modelos de projeto está localizado em um *Program.cs* arquivo).</span><span class="sxs-lookup"><span data-stu-id="da8ee-122">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="da8ee-123">Um típico *Program.cs*, conforme mostrado abaixo, demonstra como usar um `WebHostBuilder` para criar um host.</span><span class="sxs-lookup"><span data-stu-id="da8ee-123">A typical *Program.cs*, shown below, demonstrates how to use a `WebHostBuilder` to build a host.</span></span>

<span data-ttu-id="da8ee-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span><span class="sxs-lookup"><span data-stu-id="da8ee-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span></span>

<span data-ttu-id="da8ee-125">O `WebHostBuilder` é responsável por criar o host que serão inicializar o servidor para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="da8ee-125">The `WebHostBuilder` is responsible for creating the host that will bootstrap the server for the app.</span></span> <span data-ttu-id="da8ee-126">`WebHostBuilder`requer que você forneça um servidor que implementa `IServer` (`UseKestrel` no código acima).</span><span class="sxs-lookup"><span data-stu-id="da8ee-126">`WebHostBuilder` requires you provide a server that implements `IServer` (`UseKestrel` in the code above).</span></span> <span data-ttu-id="da8ee-127">`UseKestrel`Especifica que o servidor de Kestrel será usado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="da8ee-127">`UseKestrel` specifies the Kestrel server will be used by the app.</span></span>

<span data-ttu-id="da8ee-128">O servidor *conteúdo raiz* determina onde ele procura por arquivos de conteúdo, como modo de exibição MVC.</span><span class="sxs-lookup"><span data-stu-id="da8ee-128">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="da8ee-129">A raiz de conteúdo padrão é a pasta na qual o aplicativo é executado.</span><span class="sxs-lookup"><span data-stu-id="da8ee-129">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="da8ee-130">Especificando `Directory.GetCurrentDirectory` como a raiz de conteúdo usarão pasta raiz do projeto da web como raiz de conteúdo do aplicativo quando o aplicativo for iniciado a partir desta pasta (por exemplo, chamar `dotnet run` da pasta de projeto da web).</span><span class="sxs-lookup"><span data-stu-id="da8ee-130">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="da8ee-131">Esse é o padrão usado no Visual Studio e `dotnet new` modelos.</span><span class="sxs-lookup"><span data-stu-id="da8ee-131">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="da8ee-132">Para usar o IIS como um proxy reverso, chame `UseIISIntegration` como parte da construção do host.</span><span class="sxs-lookup"><span data-stu-id="da8ee-132">To use IIS as a reverse proxy, call `UseIISIntegration` as part of building the host.</span></span> 

<span data-ttu-id="da8ee-133">Observe que `UseIISIntegration` não configurar uma *servidor*, como `UseKestrel` does.</span><span class="sxs-lookup"><span data-stu-id="da8ee-133">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="da8ee-134">Para usar o IIS com o ASP.NET Core, você deve especificar `UseKestrel` e `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-134">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="da8ee-135">`UseKestrel`cria o servidor web e hospeda o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="da8ee-135">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="da8ee-136">`UseIISIntegration`examina as variáveis de ambiente usadas pelo IIS/IISExpress e define as configurações como a porta a ser escutada e os cabeçalhos para usar.</span><span class="sxs-lookup"><span data-stu-id="da8ee-136">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="da8ee-137">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="da8ee-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="da8ee-138">Criar um host usando uma instância de `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-138">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="da8ee-139">Isso geralmente é feito no ponto de entrada do aplicativo: `public static void Main` (que nos modelos de projeto está localizado em um *Program.cs* arquivo).</span><span class="sxs-lookup"><span data-stu-id="da8ee-139">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="da8ee-140">Um típico *Program.cs*, conforme mostrado abaixo, chamadas `CreateDefaultbuilder` para criar um host:</span><span class="sxs-lookup"><span data-stu-id="da8ee-140">A typical *Program.cs*, shown below, calls `CreateDefaultbuilder` to build a host:</span></span>

<span data-ttu-id="da8ee-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="da8ee-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span></span>

<span data-ttu-id="da8ee-142">`CreateDefaultbuilder`cria uma instância de `WebHostBuilder` para criar o host que inicializa o servidor para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="da8ee-142">`CreateDefaultbuilder` creates an instance of `WebHostBuilder` to build the host that bootstraps the server for the app.</span></span> <span data-ttu-id="da8ee-143">O host requer uma [server que implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="da8ee-143">The host requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="da8ee-144">Os servidores internos são [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` usam Kestrel por padrão.</span><span class="sxs-lookup"><span data-stu-id="da8ee-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` use Kestrel by default.</span></span>

<span data-ttu-id="da8ee-145">`CreateDefaultbuilder`executa tarefas de configuração, além de configurar Kestrel como o servidor web:</span><span class="sxs-lookup"><span data-stu-id="da8ee-145">`CreateDefaultbuilder` performs set-up tasks in addition to configuring Kestrel as the web server:</span></span>

* <span data-ttu-id="da8ee-146">Define a raiz de conteúdo `Directory.GetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-146">Sets the content root to `Directory.GetCurrentDirectory`.</span></span>
* <span data-ttu-id="da8ee-147">Configuração de cargas de:</span><span class="sxs-lookup"><span data-stu-id="da8ee-147">Loads configuration from:</span></span>
  * <span data-ttu-id="da8ee-148">*appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="da8ee-148">*appsettings.json*</span></span>
  * <span data-ttu-id="da8ee-149">*appSettings. \<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="da8ee-149">*appsettings.\<EnvironmentName>.json*.</span></span>
  * <span data-ttu-id="da8ee-150">segredos do usuário quando o aplicativo é executado no ambiente de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="da8ee-150">user secrets when the app runs in the Development environment</span></span>
  * <span data-ttu-id="da8ee-151">variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="da8ee-151">environment variables</span></span>
  * <span data-ttu-id="da8ee-152">argumentos de linha de comando fornecidos</span><span class="sxs-lookup"><span data-stu-id="da8ee-152">supplied command line args</span></span>
* <span data-ttu-id="da8ee-153">Configura o registro para a saída do console e de depuração, com as regras especificadas em uma seção de configuração de log de filtragem.</span><span class="sxs-lookup"><span data-stu-id="da8ee-153">Configures logging for console and debug output, with filtering rules specified in a Logging configuration section.</span></span>
* <span data-ttu-id="da8ee-154">Permite a integração do IIS.</span><span class="sxs-lookup"><span data-stu-id="da8ee-154">Enables IIS integration.</span></span>
* <span data-ttu-id="da8ee-155">Adiciona a página de exceção do desenvolvedor quando o aplicativo é executado no ambiente de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="da8ee-155">Adds the developer exception page when the app runs in the Development environment.</span></span>

<span data-ttu-id="da8ee-156">O servidor *conteúdo raiz* determina onde ele procura por arquivos de conteúdo, como modo de exibição MVC.</span><span class="sxs-lookup"><span data-stu-id="da8ee-156">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="da8ee-157">A raiz de conteúdo padrão é a pasta na qual o aplicativo é executado.</span><span class="sxs-lookup"><span data-stu-id="da8ee-157">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="da8ee-158">Especificando `Directory.GetCurrentDirectory` como a raiz de conteúdo usarão pasta raiz do projeto da web como raiz de conteúdo do aplicativo quando o aplicativo for iniciado a partir desta pasta (por exemplo, chamar `dotnet run` da pasta de projeto da web).</span><span class="sxs-lookup"><span data-stu-id="da8ee-158">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="da8ee-159">Esse é o padrão usado no Visual Studio e `dotnet new` modelos.</span><span class="sxs-lookup"><span data-stu-id="da8ee-159">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="da8ee-160">Quando você usar o IIS como um proxy reverso, ASP.NET Core automaticamente chama `UseIISIntegration` como parte da construção do host.</span><span class="sxs-lookup"><span data-stu-id="da8ee-160">When you use IIS as a reverse proxy, ASP.NET Core automatically calls `UseIISIntegration` as part of building the host.</span></span> <span data-ttu-id="da8ee-161">Para obter mais informações, consulte [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="da8ee-161">For more information, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="da8ee-162">Observe que `UseIISIntegration` não configurar uma *servidor*, como `UseKestrel` does.</span><span class="sxs-lookup"><span data-stu-id="da8ee-162">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="da8ee-163">`UseKestrel`cria o servidor web e hospeda o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="da8ee-163">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="da8ee-164">`UseIISIntegration`examina as variáveis de ambiente usadas pelo IIS/IISExpress e define as configurações como a porta a ser escutada e os cabeçalhos para usar.</span><span class="sxs-lookup"><span data-stu-id="da8ee-164">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

---

<span data-ttu-id="da8ee-165">Uma implementação mínima de configuração de um host (e um aplicativo ASP.NET Core) inclui apenas um servidor e a configuração do canal de solicitação do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="da8ee-165">A minimal implementation of configuring a host (and an ASP.NET Core app) would include just a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> <span data-ttu-id="da8ee-166">Ao configurar um host, você pode fornecer `Configure` e `ConfigureServices` métodos, em vez de ou além de especificar um `Startup` classe (que também deve definir esses métodos - consulte [inicialização do aplicativo](startup.md)).</span><span class="sxs-lookup"><span data-stu-id="da8ee-166">When setting up a host, you can provide `Configure` and `ConfigureServices` methods, instead of or in addition to specifying a `Startup` class (which must also define these methods - see [Application Startup](startup.md)).</span></span> <span data-ttu-id="da8ee-167">Diversas chamadas para `ConfigureServices` acrescentará um ao outro; chamadas para `Configure` ou `UseStartup` substituirá as configurações anteriores.</span><span class="sxs-lookup"><span data-stu-id="da8ee-167">Multiple calls to `ConfigureServices` will append to one another; calls to `Configure` or `UseStartup` will replace previous settings.</span></span>

## <a name="configuring-a-host"></a><span data-ttu-id="da8ee-168">Configurando um Host</span><span class="sxs-lookup"><span data-stu-id="da8ee-168">Configuring a Host</span></span>

<span data-ttu-id="da8ee-169">O `WebHostBuilder` fornece métodos para definir a maioria dos valores de configuração disponíveis para o host, que também pode ser definido diretamente usando `UseSetting` e a chave associada.</span><span class="sxs-lookup"><span data-stu-id="da8ee-169">The `WebHostBuilder` provides methods for setting most of the available configuration values for the host, which can also be set directly using `UseSetting` and associated key.</span></span>

### <a name="host-configuration-values"></a><span data-ttu-id="da8ee-170">Valores de configuração do host</span><span class="sxs-lookup"><span data-stu-id="da8ee-170">Host Configuration Values</span></span>

<span data-ttu-id="da8ee-171">**Capturar erros de inicialização**`bool`</span><span class="sxs-lookup"><span data-stu-id="da8ee-171">**Capture Startup Errors** `bool`</span></span>

<span data-ttu-id="da8ee-172">Chave: `captureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-172">Key: `captureStartupErrors`.</span></span> <span data-ttu-id="da8ee-173">Assume o padrão de `false`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-173">Defaults to `false`.</span></span> <span data-ttu-id="da8ee-174">Quando `false`, erros durante o resultado de inicialização no host sair.</span><span class="sxs-lookup"><span data-stu-id="da8ee-174">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="da8ee-175">Quando `true`, o host irá capturar todas as exceções do `Startup` classe e tente iniciar o servidor.</span><span class="sxs-lookup"><span data-stu-id="da8ee-175">When `true`, the host will capture any exceptions from the `Startup` class and attempt to start the server.</span></span> <span data-ttu-id="da8ee-176">Ele exibirá uma página de erro (genérico ou detalhados, com base na configuração de erros detalhados, abaixo) para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="da8ee-176">It will display an error page (generic, or detailed, based on the Detailed Errors setting, below) for every request.</span></span> <span data-ttu-id="da8ee-177">Definido usando o `CaptureStartupErrors` método.</span><span class="sxs-lookup"><span data-stu-id="da8ee-177">Set using the `CaptureStartupErrors` method.</span></span>

<span data-ttu-id="da8ee-178">Observação: Quando seu aplicativo é executado com Kestrel e o IIS, o comportamento padrão é capturar erros de inicialização.</span><span class="sxs-lookup"><span data-stu-id="da8ee-178">Note: When your app runs with Kestrel and IIS, the default behavior is to capture startup errors.</span></span> 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

<span data-ttu-id="da8ee-179">**Raiz de conteúdo**`string`</span><span class="sxs-lookup"><span data-stu-id="da8ee-179">**Content Root** `string`</span></span>

<span data-ttu-id="da8ee-180">Chave: `contentRoot`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-180">Key: `contentRoot`.</span></span> <span data-ttu-id="da8ee-181">O padrão é a pasta onde o assembly de aplicativo reside (para Kestrel; IIS usará a raiz do projeto web por padrão).</span><span class="sxs-lookup"><span data-stu-id="da8ee-181">Defaults to the folder where the application assembly resides (for Kestrel; IIS will use the web project root by default).</span></span> <span data-ttu-id="da8ee-182">Essa configuração determina onde ASP.NET Core começará a procurar pelos arquivos de conteúdo, como exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="da8ee-182">This setting determines where ASP.NET Core will begin searching for content files, such as MVC Views.</span></span> <span data-ttu-id="da8ee-183">Também é usado como o caminho base para o [Web raiz configuração](#web-root-setting).</span><span class="sxs-lookup"><span data-stu-id="da8ee-183">Also used as the base path for the [Web Root Setting](#web-root-setting).</span></span> <span data-ttu-id="da8ee-184">Definido usando o `UseContentRoot` método.</span><span class="sxs-lookup"><span data-stu-id="da8ee-184">Set using the `UseContentRoot` method.</span></span> <span data-ttu-id="da8ee-185">Caminho deve existir ou o host não será iniciado.</span><span class="sxs-lookup"><span data-stu-id="da8ee-185">Path must exist, or host will fail to start.</span></span>

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

<span data-ttu-id="da8ee-186">**Erros detalhados**`bool`</span><span class="sxs-lookup"><span data-stu-id="da8ee-186">**Detailed Errors** `bool`</span></span>

<span data-ttu-id="da8ee-187">Chave: `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-187">Key: `detailedErrors`.</span></span> <span data-ttu-id="da8ee-188">Assume o padrão de `false`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-188">Defaults to `false`.</span></span> <span data-ttu-id="da8ee-189">Quando `true` (ou quando o ambiente está definido como "Desenvolvimento"), o aplicativo exibirá detalhes de exceções de inicialização, em vez de apenas uma página de erro genérica.</span><span class="sxs-lookup"><span data-stu-id="da8ee-189">When `true` (or when Environment is set to "Development"), the app will display details of startup exceptions, instead of just a generic error page.</span></span> <span data-ttu-id="da8ee-190">Definido usando `UseSetting`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-190">Set using `UseSetting`.</span></span>

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

<span data-ttu-id="da8ee-191">Quando erros detalhados está definido como `false` e capturar erros de inicialização é `true`, uma página de erro genérica é exibida em resposta a cada solicitação ao servidor.</span><span class="sxs-lookup"><span data-stu-id="da8ee-191">When Detailed Errors is set to `false` and Capture Startup Errors is `true`, a generic error page is displayed in response to every request to the server.</span></span>

![Página de erro genérica](hosting/_static/generic-error-page.png)

<span data-ttu-id="da8ee-193">Quando erros detalhados está definido como `true` e capturar erros de inicialização é `true`, uma página de erro detalhada é exibida em resposta a cada solicitação ao servidor.</span><span class="sxs-lookup"><span data-stu-id="da8ee-193">When Detailed Errors is set to `true` and Capture Startup Errors is `true`, a detailed error page is displayed in response to every request to the server.</span></span>

![Página de erro detalhadas](hosting/_static/detailed-error-page.png)

<span data-ttu-id="da8ee-195">**Ambiente**`string`</span><span class="sxs-lookup"><span data-stu-id="da8ee-195">**Environment** `string`</span></span>

<span data-ttu-id="da8ee-196">Chave: `environment`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-196">Key: `environment`.</span></span> <span data-ttu-id="da8ee-197">O padrão é "Produção".</span><span class="sxs-lookup"><span data-stu-id="da8ee-197">Defaults to "Production".</span></span> <span data-ttu-id="da8ee-198">Pode ser definida como qualquer valor.</span><span class="sxs-lookup"><span data-stu-id="da8ee-198">May be set to any value.</span></span> <span data-ttu-id="da8ee-199">Valores definidos pelo Framework incluem "Desenvolvimento", "Preparação" e "Produção".</span><span class="sxs-lookup"><span data-stu-id="da8ee-199">Framework-defined values include "Development", "Staging", and "Production".</span></span> <span data-ttu-id="da8ee-200">Valores não diferenciam maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="da8ee-200">Values are not case sensitive.</span></span> <span data-ttu-id="da8ee-201">Consulte [trabalhando com vários ambientes](environments.md).</span><span class="sxs-lookup"><span data-stu-id="da8ee-201">See [Working with Multiple Environments](environments.md).</span></span> <span data-ttu-id="da8ee-202">Definido usando o `UseEnvironment` método.</span><span class="sxs-lookup"><span data-stu-id="da8ee-202">Set using the `UseEnvironment` method.</span></span>

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> <span data-ttu-id="da8ee-203">Por padrão, o ambiente é lido a partir de `ASPNETCORE_ENVIRONMENT` variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="da8ee-203">By default, the environment is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="da8ee-204">Ao usar o Visual Studio, as variáveis de ambiente podem ser definidas no *launchSettings.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="da8ee-204">When using Visual Studio, environment variables may be set in the *launchSettings.json* file.</span></span>

<a id="server-urls"></a>

<span data-ttu-id="da8ee-205">**URLs de servidor**`string`</span><span class="sxs-lookup"><span data-stu-id="da8ee-205">**Server URLs** `string`</span></span>

<span data-ttu-id="da8ee-206">Chave: `urls`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-206">Key: `urls`.</span></span> <span data-ttu-id="da8ee-207">Definido como um ponto e vírgula (;) separados de prefixos de lista de URL para o servidor deve responder.</span><span class="sxs-lookup"><span data-stu-id="da8ee-207">Set to a semicolon (;) separated list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="da8ee-208">Por exemplo, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-208">For example, `http://localhost:123`.</span></span> <span data-ttu-id="da8ee-209">O nome do domínio/host pode ser substituído por "\*" para indicar que o servidor deve escutar solicitações em qualquer endereço IP ou usando a porta especificada e o protocolo de host (por exemplo, `http://*:5000` ou `https://*:5001`).</span><span class="sxs-lookup"><span data-stu-id="da8ee-209">The domain/host name can be replaced with "\*" to indicate the server should listen to requests on any IP address or host using the specified port and protocol (for example, `http://*:5000` or `https://*:5001`).</span></span> <span data-ttu-id="da8ee-210">O protocolo (`http://` ou `https://`) devem ser incluídas com cada URL.</span><span class="sxs-lookup"><span data-stu-id="da8ee-210">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="da8ee-211">Os prefixos são interpretados pelo servidor configurado; os formatos com suporte variam entre servidores.</span><span class="sxs-lookup"><span data-stu-id="da8ee-211">The prefixes are interpreted by the configured server; supported formats will vary between servers.</span></span>

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="da8ee-212">No ASP.NET 2.0 de núcleo, Kestrel tem sua própria API de configuração do ponto de extremidade e não oferece suporte a `https://` no `urls` cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="da8ee-212">In ASP.NET Core 2.0, Kestrel has its own endpoint configuration API and does not support `https://` in the `urls` string.</span></span> <span data-ttu-id="da8ee-213">Para obter mais informações, consulte [Introdução ao Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="da8ee-213">For more information, see [Introduction to Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

<span data-ttu-id="da8ee-214">**Assembly de inicialização**`string`</span><span class="sxs-lookup"><span data-stu-id="da8ee-214">**Startup Assembly** `string`</span></span>

<span data-ttu-id="da8ee-215">Chave: `startupAssembly`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-215">Key: `startupAssembly`.</span></span> <span data-ttu-id="da8ee-216">Determina o assembly para pesquisar o `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="da8ee-216">Determines the assembly to search for the `Startup` class.</span></span> <span data-ttu-id="da8ee-217">Definido usando o `UseStartup` método.</span><span class="sxs-lookup"><span data-stu-id="da8ee-217">Set using the `UseStartup` method.</span></span> <span data-ttu-id="da8ee-218">Em vez disso, podem fazer referência tipo específico usando `WebHostBuilder.UseStartup<StartupType>`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-218">May instead reference specific type using `WebHostBuilder.UseStartup<StartupType>`.</span></span> <span data-ttu-id="da8ee-219">Se vários `UseStartup` métodos são chamados, último terá precedência.</span><span class="sxs-lookup"><span data-stu-id="da8ee-219">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

<span data-ttu-id="da8ee-220">**Web raiz**`string`</span><span class="sxs-lookup"><span data-stu-id="da8ee-220">**Web Root** `string`</span></span>

<span data-ttu-id="da8ee-221">Chave: `webroot`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-221">Key: `webroot`.</span></span> <span data-ttu-id="da8ee-222">Se não especificado, o padrão é `(Content Root Path)\wwwroot`, se ele existir.</span><span class="sxs-lookup"><span data-stu-id="da8ee-222">If not specified the default is `(Content Root Path)\wwwroot`, if it exists.</span></span> <span data-ttu-id="da8ee-223">Se esse caminho não existir, é usado um provedor de arquivo não-operacional.</span><span class="sxs-lookup"><span data-stu-id="da8ee-223">If this path doesn't exist, then a no-op file provider is used.</span></span> <span data-ttu-id="da8ee-224">Definido usando `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-224">Set using `UseWebRoot`.</span></span>

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a><span data-ttu-id="da8ee-225">Substituindo a configuração</span><span class="sxs-lookup"><span data-stu-id="da8ee-225">Overriding Configuration</span></span>

<span data-ttu-id="da8ee-226">Use [configuração](configuration.md) para definir valores de configuração a ser usado pelo host.</span><span class="sxs-lookup"><span data-stu-id="da8ee-226">Use [Configuration](configuration.md) to set configuration values to be used by the host.</span></span> <span data-ttu-id="da8ee-227">Esses valores podem ser substituídos posteriormente.</span><span class="sxs-lookup"><span data-stu-id="da8ee-227">These values may be subsequently overridden.</span></span> <span data-ttu-id="da8ee-228">Isso é especificado usando `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-228">This is specified using `UseConfiguration`.</span></span>

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

<span data-ttu-id="da8ee-229">No exemplo acima, os argumentos de linha de comando podem ser passados para configurar o host ou definições de configuração, opcionalmente, podem ser especificadas em uma *hosting.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="da8ee-229">In the example above, command-line arguments may be passed in to configure the host, or configuration settings may optionally be specified in a *hosting.json* file.</span></span> <span data-ttu-id="da8ee-230">Para especificar o host executado em uma URL específica, você pode transmitir o valor desejado de um prompt de comando:</span><span class="sxs-lookup"><span data-stu-id="da8ee-230">To specify the host run on a particular URL, you could pass in the desired value from a command prompt:</span></span>

```console
dotnet run --urls "http://*:5000"
```

<span data-ttu-id="da8ee-231">O `Run` método inicia o aplicativo web e bloqueia o thread de chamada até que o host é desligado.</span><span class="sxs-lookup"><span data-stu-id="da8ee-231">The `Run` method starts the web app and blocks the calling thread until the host is shutdown.</span></span>

```csharp
host.Run();
```

<span data-ttu-id="da8ee-232">Você pode executar o host de maneira sem bloqueio, chamando seu `Start` método:</span><span class="sxs-lookup"><span data-stu-id="da8ee-232">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="da8ee-233">Passar uma lista de URLs para o `Start` método e ele escutará em URLs especificadas:</span><span class="sxs-lookup"><span data-stu-id="da8ee-233">Pass a list of URLs to the `Start` method and it will listen on the URLs specified:</span></span>

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

<span data-ttu-id="da8ee-234">Os formatos de URL que são válidos aqui dependem do servidor que você está usando.</span><span class="sxs-lookup"><span data-stu-id="da8ee-234">The URL formats that are valid here depend on the server you're using.</span></span> <span data-ttu-id="da8ee-235">Para obter mais informações, consulte [URLs de servidor](#server-urls) anteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="da8ee-235">For more information, see [Server URLs](#server-urls) earlier in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="da8ee-236">O `UseConfiguration` método de extensão não está atualmente capaz de analisar uma seção de configuração retornada por `GetSection` (por exemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-236">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="da8ee-237">O `GetSection` método filtra as chaves de configuração para a seção solicitada, mas deixa o nome da seção sobre as chaves (por exemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="da8ee-237">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="da8ee-238">O `UseConfiguration` método espera que as chaves para corresponder a `WebHostBuilder` chaves (por exemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="da8ee-238">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="da8ee-239">A presença do nome da seção nas chaves impede que os valores da seção Configurando o host.</span><span class="sxs-lookup"><span data-stu-id="da8ee-239">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="da8ee-240">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="da8ee-240">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="da8ee-241">Para obter mais informações e soluções alternativas, consulte [passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="da8ee-241">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="ordering-importance"></a><span data-ttu-id="da8ee-242">Ordem de importância</span><span class="sxs-lookup"><span data-stu-id="da8ee-242">Ordering Importance</span></span>

<span data-ttu-id="da8ee-243">`WebHostBuilder`as configurações são lidas primeiro de determinadas variáveis de ambiente, se definido.</span><span class="sxs-lookup"><span data-stu-id="da8ee-243">`WebHostBuilder` settings are first read from certain environment variables, if set.</span></span> <span data-ttu-id="da8ee-244">Essas variáveis de ambiente devem usar o formato `ASPNETCORE_{configurationKey}`, portanto, por exemplo para definir as URLs de servidor escutará em por padrão, você definiria `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="da8ee-244">These environment variables must use the format `ASPNETCORE_{configurationKey}`, so for example to set the URLs the server will listen on by default, you would set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="da8ee-245">Você pode substituir qualquer um desses valores de variável de ambiente com a especificação de configuração (usando `UseConfiguration`) ou definindo o valor explicitamente (usando `UseUrls` para a instância).</span><span class="sxs-lookup"><span data-stu-id="da8ee-245">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseUrls` for instance).</span></span> <span data-ttu-id="da8ee-246">O host usará qualquer opção define o valor da última.</span><span class="sxs-lookup"><span data-stu-id="da8ee-246">The host will use whichever option sets the value last.</span></span> <span data-ttu-id="da8ee-247">Por esse motivo, `UseIISIntegration` devem aparecer após `UseUrls`, porque ele substitui o URL com um dinamicamente fornecida pelo IIS.</span><span class="sxs-lookup"><span data-stu-id="da8ee-247">For this reason, `UseIISIntegration` must appear after `UseUrls`, because it replaces the URL with one dynamically provided by IIS.</span></span> <span data-ttu-id="da8ee-248">Se você quiser programaticamente definir a URL padrão para um valor, mas permitir que ele seja substituído com a configuração, você poderá configurar o host da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="da8ee-248">If you want to programmatically set the default URL to one value, but allow it to be overridden with configuration, you could configure the host as follows:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="da8ee-249">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="da8ee-249">Additional resources</span></span>

* [<span data-ttu-id="da8ee-250">Publicar no Windows usando o IIS</span><span class="sxs-lookup"><span data-stu-id="da8ee-250">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="da8ee-251">Publicar no Linux usando Nginx</span><span class="sxs-lookup"><span data-stu-id="da8ee-251">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="da8ee-252">Publicar no Linux usando o Apache</span><span class="sxs-lookup"><span data-stu-id="da8ee-252">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="da8ee-253">Host em um serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="da8ee-253">Host in a Windows Service</span></span>](xref:hosting/windows-service)

