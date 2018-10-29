---
title: Host Genérico .NET
author: guardrex
description: Saiba mais sobre o Host Genérico no .NET, que é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: e5f91ed64b7f8402dfe938f0fa8a0d94755d15c6
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207713"
---
# <a name="net-generic-host"></a><span data-ttu-id="9175c-103">Host Genérico .NET</span><span class="sxs-lookup"><span data-stu-id="9175c-103">.NET Generic Host</span></span>

<span data-ttu-id="9175c-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9175c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9175c-105">Os aplicativos .NET Core configuram e iniciam um *host*.</span><span class="sxs-lookup"><span data-stu-id="9175c-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="9175c-106">O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9175c-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="9175c-107">Este tópico aborda o Host Genérico do ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), que é útil para a hospedagem de aplicativos que não processam solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="9175c-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="9175c-108">Para cobertura do host da Web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), veja <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="9175c-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="9175c-109">O objetivo do Host Genérico é separar o pipeline HTTP da API de host da Web para permitir maior gama de cenários de host.</span><span class="sxs-lookup"><span data-stu-id="9175c-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="9175c-110">Sistema de mensagens, tarefas em segundo plano e outras cargas de trabalho não HTTP com base no benefício do Host Genérico de recursos abrangentes, como configuração, DI (injeção de dependência) e log.</span><span class="sxs-lookup"><span data-stu-id="9175c-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="9175c-111">O Host Genérico é novo no ASP.NET Core 2.1 e não é adequado para cenários de hospedagem na Web.</span><span class="sxs-lookup"><span data-stu-id="9175c-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="9175c-112">Para cenários de hospedagem na Web, use o [host da Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="9175c-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="9175c-113">O Host Genérico está em desenvolvimento para substituir o host da Web em uma versão futura e atuar como API do host principal em cenários HTTP e não HTTP.</span><span class="sxs-lookup"><span data-stu-id="9175c-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="9175c-114">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([como baixar](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9175c-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9175c-115">Ao executar o aplicativo de exemplo no [Visual Studio Code](https://code.visualstudio.com/), use um *terminal externo ou integrado*.</span><span class="sxs-lookup"><span data-stu-id="9175c-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="9175c-116">Não execute o exemplo em um `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="9175c-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="9175c-117">Para definir o console no Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="9175c-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="9175c-118">Abra o arquivo *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="9175c-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="9175c-119">Na configuração **Inicialização do .NET Core (console)**, localize a entrada **console**.</span><span class="sxs-lookup"><span data-stu-id="9175c-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="9175c-120">Defina o valor como `externalTerminal` ou `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="9175c-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="9175c-121">Introdução</span><span class="sxs-lookup"><span data-stu-id="9175c-121">Introduction</span></span>

<span data-ttu-id="9175c-122">A biblioteca do Host Genérico está disponível no [namespace Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) e é fornecida pelo [pacote Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="9175c-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="9175c-123">O pacote `Microsoft.Extensions.Hosting` está incluído no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou posterior).</span><span class="sxs-lookup"><span data-stu-id="9175c-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="9175c-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) é o ponto de entrada para a execução de código.</span><span class="sxs-lookup"><span data-stu-id="9175c-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="9175c-125">Cada implementação do `IHostedService` é executada na ordem do [registro de serviço em ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="9175c-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="9175c-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) é chamado em cada `IHostedService` quando o host é iniciado, e [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) é chamado na ordem inversa de registro quando o host é desligado normalmente.</span><span class="sxs-lookup"><span data-stu-id="9175c-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="9175c-127">Configurar um host</span><span class="sxs-lookup"><span data-stu-id="9175c-127">Set up a host</span></span>

<span data-ttu-id="9175c-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) é o principal componente usado por aplicativos e bibliotecas para inicializar, compilar e executar o host:</span><span class="sxs-lookup"><span data-stu-id="9175c-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a><span data-ttu-id="9175c-129">Serviços padrão</span><span class="sxs-lookup"><span data-stu-id="9175c-129">Default services</span></span>

<span data-ttu-id="9175c-130">Os seguintes serviços são registrados durante a inicialização do host:</span><span class="sxs-lookup"><span data-stu-id="9175c-130">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="9175c-131">[Ambiente](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="9175c-131">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="9175c-132">[Configuração](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="9175c-132">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="9175c-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="9175c-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="9175c-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="9175c-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="9175c-135">[Opções](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="9175c-135">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="9175c-136">[Registro em log](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="9175c-136">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="9175c-137">Configuração do host</span><span class="sxs-lookup"><span data-stu-id="9175c-137">Host configuration</span></span>

<span data-ttu-id="9175c-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) conta com as seguintes abordagens para definir os valores de configuração do host:</span><span class="sxs-lookup"><span data-stu-id="9175c-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="9175c-139">Construtor de configuração</span><span class="sxs-lookup"><span data-stu-id="9175c-139">Configuration builder</span></span>
* <span data-ttu-id="9175c-140">Configuração do método de extensão</span><span class="sxs-lookup"><span data-stu-id="9175c-140">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="9175c-141">Construtor de configuração</span><span class="sxs-lookup"><span data-stu-id="9175c-141">Configuration builder</span></span>

<span data-ttu-id="9175c-142">A configuração do construtor de host é criada chamando [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) na implementação [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="9175c-142">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="9175c-143">`ConfigureHostConfiguration` usa um [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) para criar um [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para o host.</span><span class="sxs-lookup"><span data-stu-id="9175c-143">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="9175c-144">O construtor de configuração inicializa o [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) para uso no processo de compilação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9175c-144">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="9175c-145">A configuração de variável de ambiente não é adicionada por padrão.</span><span class="sxs-lookup"><span data-stu-id="9175c-145">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="9175c-146">Chame [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) no construtor de host para configurar o host com base em variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="9175c-146">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="9175c-147">`AddEnvironmentVariables` aceita um prefixo opcional definido pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="9175c-147">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="9175c-148">O aplicativo de exemplo usa um prefixo igual a `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="9175c-148">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="9175c-149">O prefixo é removido quando as variáveis de ambiente são lidas.</span><span class="sxs-lookup"><span data-stu-id="9175c-149">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="9175c-150">Quando o host do aplicativo de exemplo é configurado, o valor da variável de ambiente de `PREFIX_ENVIRONMENT` torna-se o valor de configuração de host para a chave `environment`.</span><span class="sxs-lookup"><span data-stu-id="9175c-150">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="9175c-151">Durante o desenvolvimento, ao usar o [Visual Studio](https://www.visualstudio.com/) ou executar um aplicativo com `dotnet run`, as variáveis de ambiente poderão ser definidas no arquivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9175c-151">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="9175c-152">No [Visual Studio Code](https://code.visualstudio.com/), as variáveis de ambiente podem ser definidas no arquivo *.vscode/launch.json* durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="9175c-152">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="9175c-153">Para obter mais informações, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="9175c-153">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="9175c-154">`ConfigureHostConfiguration` pode ser chamado várias vezes com resultados aditivos.</span><span class="sxs-lookup"><span data-stu-id="9175c-154">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="9175c-155">O host usa a opção que define um valor por último.</span><span class="sxs-lookup"><span data-stu-id="9175c-155">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="9175c-156">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="9175c-156">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="9175c-157">Exemplo da configuração `HostBuilder` usando `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="9175c-157">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="9175c-158">Atualmente, o método de extensão [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) não é capaz de analisar uma seção de configuração retornada por [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (por exemplo, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="9175c-158">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="9175c-159">O método `GetSection` filtra as chaves de configuração da seção solicitada, mas deixa o nome da seção nas chaves (por exemplo, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="9175c-159">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="9175c-160">O método `AddConfiguration` espera que as chaves correspondam às chaves `HostBuilder` (por exemplo, `environment`).</span><span class="sxs-lookup"><span data-stu-id="9175c-160">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="9175c-161">A presença do nome da seção nas chaves impede que os valores da seção configurem o host.</span><span class="sxs-lookup"><span data-stu-id="9175c-161">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="9175c-162">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="9175c-162">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="9175c-163">Para obter mais informações e soluções alternativas, consulte [Passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="9175c-163">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="9175c-164">Configuração do método de extensão</span><span class="sxs-lookup"><span data-stu-id="9175c-164">Extension method configuration</span></span>

<span data-ttu-id="9175c-165">Métodos de extensão são chamados na implementação `IHostBuilder` para configurar a raiz do conteúdo e o ambiente.</span><span class="sxs-lookup"><span data-stu-id="9175c-165">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="application-key-name"></a><span data-ttu-id="9175c-166">Chave do Aplicativo (Nome)</span><span class="sxs-lookup"><span data-stu-id="9175c-166">Application Key (Name)</span></span>

<span data-ttu-id="9175c-167">A propriedade [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) é definida na configuração do host durante a construção do host.</span><span class="sxs-lookup"><span data-stu-id="9175c-167">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is set from host configuration during host construction.</span></span> <span data-ttu-id="9175c-168">Para definir o valor explicitamente, use o [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="9175c-168">To set the value explicitly, use the [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span></span>

<span data-ttu-id="9175c-169">**Chave**: applicationName</span><span class="sxs-lookup"><span data-stu-id="9175c-169">**Key**: applicationName</span></span>  
<span data-ttu-id="9175c-170">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="9175c-170">**Type**: *string*</span></span>  
<span data-ttu-id="9175c-171">**Padrão**: o nome do assembly que contém o ponto de entrada do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9175c-171">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="9175c-172">**Definido usando**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="9175c-172">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="9175c-173">**Variável de ambiente**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` é [opcional e definida pelo usuário](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="9175c-173">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a><span data-ttu-id="9175c-174">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="9175c-174">Content Root</span></span>

<span data-ttu-id="9175c-175">Essa configuração determina onde o host começa a procurar por arquivos de conteúdo.</span><span class="sxs-lookup"><span data-stu-id="9175c-175">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="9175c-176">**Chave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="9175c-176">**Key**: contentRoot</span></span>  
<span data-ttu-id="9175c-177">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="9175c-177">**Type**: *string*</span></span>  
<span data-ttu-id="9175c-178">**Padrão**: o padrão é a pasta em que o assembly do aplicativo reside.</span><span class="sxs-lookup"><span data-stu-id="9175c-178">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="9175c-179">**Definido usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="9175c-179">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="9175c-180">**Variável de ambiente**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` é [opcional e definida pelo usuário](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="9175c-180">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="9175c-181">Se o caminho não existir, o host não será iniciado.</span><span class="sxs-lookup"><span data-stu-id="9175c-181">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="9175c-182">Ambiente</span><span class="sxs-lookup"><span data-stu-id="9175c-182">Environment</span></span>

<span data-ttu-id="9175c-183">Define o [ambiente](xref:fundamentals/environments) do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9175c-183">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="9175c-184">**Chave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="9175c-184">**Key**: environment</span></span>  
<span data-ttu-id="9175c-185">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="9175c-185">**Type**: *string*</span></span>  
<span data-ttu-id="9175c-186">**Padrão**: Production</span><span class="sxs-lookup"><span data-stu-id="9175c-186">**Default**: Production</span></span>  
<span data-ttu-id="9175c-187">**Definido usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="9175c-187">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="9175c-188">**Variável de ambiente**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` é [opcional e definida pelo usuário](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="9175c-188">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="9175c-189">O ambiente pode ser definido como qualquer valor.</span><span class="sxs-lookup"><span data-stu-id="9175c-189">The environment can be set to any value.</span></span> <span data-ttu-id="9175c-190">Os valores definidos pela estrutura incluem `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="9175c-190">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="9175c-191">Os valores não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="9175c-191">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="9175c-192">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="9175c-192">ConfigureAppConfiguration</span></span>

<span data-ttu-id="9175c-193">A configuração do construtor de aplicativos é criada chamando [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) na implementação [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="9175c-193">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="9175c-194">`ConfigureAppConfiguration` usa um [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) para criar um [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9175c-194">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="9175c-195">`ConfigureAppConfiguration` pode ser chamado várias vezes com resultados aditivos.</span><span class="sxs-lookup"><span data-stu-id="9175c-195">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="9175c-196">O aplicativo usa a opção que define um valor por último.</span><span class="sxs-lookup"><span data-stu-id="9175c-196">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="9175c-197">A configuração criada pelo `ConfigureAppConfiguration` está disponível em [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) para operações seguintes e em [Serviços](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="9175c-197">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="9175c-198">Exemplo da configuração de aplicativo usando `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="9175c-198">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="9175c-199">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="9175c-199">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="9175c-200">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="9175c-200">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="9175c-201">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="9175c-201">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="9175c-202">Atualmente, o método de extensão [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) não é capaz de analisar uma seção de configuração retornada por [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (por exemplo, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="9175c-202">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="9175c-203">O método `GetSection` filtra as chaves de configuração da seção solicitada, mas deixa o nome da seção nas chaves (por exemplo, `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="9175c-203">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="9175c-204">O método `AddConfiguration` espera uma correspondência exata para chaves de configuração (por exemplo, `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="9175c-204">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="9175c-205">A presença do nome da seção nas chaves impede que os valores da seção configurem o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9175c-205">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="9175c-206">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="9175c-206">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="9175c-207">Para obter mais informações e soluções alternativas, consulte [Passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="9175c-207">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="9175c-208">Para mover arquivos de configurações para o diretório de saída, especifique os arquivos de configurações como [itens de projeto do MSBuild](/visualstudio/msbuild/common-msbuild-project-items) no arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="9175c-208">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="9175c-209">O aplicativo de exemplo move seus arquivos de configurações do aplicativo JSON e *hostsettings.json* com o seguinte **&lt;conteúdo:&gt;** item:</span><span class="sxs-lookup"><span data-stu-id="9175c-209">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="9175c-210">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="9175c-210">ConfigureServices</span></span>

<span data-ttu-id="9175c-211">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adiciona serviços ao contêiner [injeção de dependência](xref:fundamentals/dependency-injection) do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9175c-211">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="9175c-212">`ConfigureServices` pode ser chamado várias vezes com resultados aditivos.</span><span class="sxs-lookup"><span data-stu-id="9175c-212">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="9175c-213">Um serviço hospedado é uma classe com lógica de tarefa em segundo plano que implementa a interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="9175c-213">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="9175c-214">Para obter mais informações, consulte <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="9175c-214">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="9175c-215">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa o método de extensão `AddHostedService` para adicionar um serviço para eventos de tempo de vida, `LifetimeEventsHostedService`, e uma tarefa em segundo plano programada, `TimedHostedService`, para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="9175c-215">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="9175c-216">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="9175c-216">ConfigureLogging</span></span>

<span data-ttu-id="9175c-217">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adiciona um delegado para configurar o [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) fornecido.</span><span class="sxs-lookup"><span data-stu-id="9175c-217">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="9175c-218">`ConfigureLogging` pode ser chamado várias vezes com resultados aditivos.</span><span class="sxs-lookup"><span data-stu-id="9175c-218">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="9175c-219">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="9175c-219">UseConsoleLifetime</span></span>

<span data-ttu-id="9175c-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) escuta `Ctrl+C`/SIGINT ou SIGTERM e chama [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) para iniciar o processo de desligamento.</span><span class="sxs-lookup"><span data-stu-id="9175c-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="9175c-221">`UseConsoleLifetime` desbloqueia extensões como [RunAsync](#runasync) e [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="9175c-221">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="9175c-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) é previamente registrado como a implementação de tempo de vida padrão.</span><span class="sxs-lookup"><span data-stu-id="9175c-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="9175c-223">O último tempo de vida registrado é usado.</span><span class="sxs-lookup"><span data-stu-id="9175c-223">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="9175c-224">Configuração do contêiner</span><span class="sxs-lookup"><span data-stu-id="9175c-224">Container configuration</span></span>

<span data-ttu-id="9175c-225">Para dar suporte à conexão de outros contêineres, o host pode aceitar um [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="9175c-225">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="9175c-226">O fornecimento de um alocador não faz parte do registro de contêiner de injeção de dependência, mas, em vez disso, um host intrínseco é usado para criar o contêiner de DI concreto.</span><span class="sxs-lookup"><span data-stu-id="9175c-226">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="9175c-227">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) substitui o alocador padrão usado para criar o provedor de serviços do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9175c-227">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="9175c-228">A configuração de contêiner personalizado é gerenciada pelo método [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer).</span><span class="sxs-lookup"><span data-stu-id="9175c-228">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="9175c-229">`ConfigureContainer` fornece uma experiência fortemente tipada para configurar o contêiner sobre a API do host subjacente.</span><span class="sxs-lookup"><span data-stu-id="9175c-229">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="9175c-230">`ConfigureContainer` pode ser chamado várias vezes com resultados aditivos.</span><span class="sxs-lookup"><span data-stu-id="9175c-230">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="9175c-231">Crie um contêiner de serviço para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="9175c-231">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="9175c-232">Forneça um alocador de contêiner de serviço:</span><span class="sxs-lookup"><span data-stu-id="9175c-232">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="9175c-233">Use o alocador e configure o contêiner de serviço personalizado para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="9175c-233">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="9175c-234">Extensibilidade</span><span class="sxs-lookup"><span data-stu-id="9175c-234">Extensibility</span></span>

<span data-ttu-id="9175c-235">Extensibilidade de host é executada com métodos de extensão em `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9175c-235">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="9175c-236">O exemplo a seguir mostra como um método de extensão estende uma implementação do `IHostBuilder` com o exemplo do [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks), demonstrado no <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="9175c-236">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="9175c-237">Um aplicativo estabelece o método de extensão `UseHostedService` para registrar o serviço hospedado passado no `T`:</span><span class="sxs-lookup"><span data-stu-id="9175c-237">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a><span data-ttu-id="9175c-238">Gerenciar o host</span><span class="sxs-lookup"><span data-stu-id="9175c-238">Manage the host</span></span>

<span data-ttu-id="9175c-239">A implementação [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) é responsável por iniciar e parar as implementações `IHostedService` que estão registradas no contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="9175c-239">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="9175c-240">Executar</span><span class="sxs-lookup"><span data-stu-id="9175c-240">Run</span></span>

<span data-ttu-id="9175c-241">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) executa o aplicativo e bloqueia o thread de chamada até que o host seja desligado:</span><span class="sxs-lookup"><span data-stu-id="9175c-241">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a><span data-ttu-id="9175c-242">RunAsync</span><span class="sxs-lookup"><span data-stu-id="9175c-242">RunAsync</span></span>

<span data-ttu-id="9175c-243">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) executa o aplicativo e retorna um `Task` que é concluído quando o token de cancelamento ou o desligamento é disparado:</span><span class="sxs-lookup"><span data-stu-id="9175c-243">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a><span data-ttu-id="9175c-244">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="9175c-244">RunConsoleAsync</span></span>

<span data-ttu-id="9175c-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) habilita o suporte do console, compila e inicia o host e aguarda `Ctrl+C`/SIGINT ou SIGTERM desligar.</span><span class="sxs-lookup"><span data-stu-id="9175c-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="9175c-246">Start e StopAsync</span><span class="sxs-lookup"><span data-stu-id="9175c-246">Start and StopAsync</span></span>

<span data-ttu-id="9175c-247">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) inicia o host de forma síncrona.</span><span class="sxs-lookup"><span data-stu-id="9175c-247">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="9175c-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) tenta parar o host dentro do tempo limite oferecido.</span><span class="sxs-lookup"><span data-stu-id="9175c-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a><span data-ttu-id="9175c-249">StartAsync e StopAsync</span><span class="sxs-lookup"><span data-stu-id="9175c-249">StartAsync and StopAsync</span></span>

<span data-ttu-id="9175c-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) inicia o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9175c-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="9175c-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9175c-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a><span data-ttu-id="9175c-252">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="9175c-252">WaitForShutdown</span></span>

<span data-ttu-id="9175c-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) é disparado por meio de [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), como [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (escuta `Ctrl+C`/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="9175c-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="9175c-254">`WaitForShutdown` chama [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="9175c-254">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a><span data-ttu-id="9175c-255">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="9175c-255">WaitForShutdownAsync</span></span>

<span data-ttu-id="9175c-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) retorna um `Task` que é concluído quando o desligamento é disparado por meio do token fornecido e chama [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="9175c-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a><span data-ttu-id="9175c-257">Controle externo</span><span class="sxs-lookup"><span data-stu-id="9175c-257">External control</span></span>

<span data-ttu-id="9175c-258">O controle externo do host pode ser obtido usando os métodos que podem ser chamados externamente:</span><span class="sxs-lookup"><span data-stu-id="9175c-258">External control of the host can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<span data-ttu-id="9175c-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) é chamado no início de [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), que aguarda até que ele seja concluído antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9175c-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="9175c-260">Isso pode ser usado para atrasar a inicialização até que seja sinalizado por um evento externo.</span><span class="sxs-lookup"><span data-stu-id="9175c-260">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="9175c-261">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="9175c-261">IHostingEnvironment interface</span></span>

<span data-ttu-id="9175c-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) fornece informações sobre o ambiente de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9175c-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="9175c-263">Use a [injeção de construtor](xref:fundamentals/dependency-injection) para obter o `IHostingEnvironment` para usar suas propriedades e métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="9175c-263">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

<span data-ttu-id="9175c-264">Para obter mais informações, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="9175c-264">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="9175c-265">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="9175c-265">IApplicationLifetime interface</span></span>

<span data-ttu-id="9175c-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) permite atividades pós-inicialização e desligamento, inclusive solicitações de desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="9175c-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="9175c-267">Três propriedades na interface são tokens de cancelamento usados para registrar métodos `Action` que definem eventos de inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="9175c-267">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="9175c-268">Token de cancelamento</span><span class="sxs-lookup"><span data-stu-id="9175c-268">Cancellation Token</span></span> | <span data-ttu-id="9175c-269">Acionado quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="9175c-269">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="9175c-270">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="9175c-270">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="9175c-271">O host foi iniciado totalmente.</span><span class="sxs-lookup"><span data-stu-id="9175c-271">The host has fully started.</span></span> |
| [<span data-ttu-id="9175c-272">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="9175c-272">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="9175c-273">O host está concluindo um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="9175c-273">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="9175c-274">Todas as solicitações devem ser processadas.</span><span class="sxs-lookup"><span data-stu-id="9175c-274">All requests should be processed.</span></span> <span data-ttu-id="9175c-275">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="9175c-275">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="9175c-276">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="9175c-276">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="9175c-277">O host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="9175c-277">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="9175c-278">Solicitações ainda podem estar sendo processadas.</span><span class="sxs-lookup"><span data-stu-id="9175c-278">Requests may still be processing.</span></span> <span data-ttu-id="9175c-279">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="9175c-279">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="9175c-280">O construtor injeta o serviço `IApplicationLifetime` em qualquer classe.</span><span class="sxs-lookup"><span data-stu-id="9175c-280">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="9175c-281">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa injeção de construtor em uma classe `LifetimeEventsHostedService` (uma implementação `IHostedService`) para registrar os eventos.</span><span class="sxs-lookup"><span data-stu-id="9175c-281">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="9175c-282">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="9175c-282">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="9175c-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) solicita o término do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9175c-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="9175c-284">A classe a seguir usa `StopApplication` para desligar normalmente um aplicativo quando o método `Shutdown` da classe é chamado:</span><span class="sxs-lookup"><span data-stu-id="9175c-284">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="9175c-285">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="9175c-285">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="9175c-286">Hospedagem de exemplos do repositório no GitHub</span><span class="sxs-lookup"><span data-stu-id="9175c-286">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
