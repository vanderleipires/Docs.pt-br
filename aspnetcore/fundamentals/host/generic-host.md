---
title: Host Genérico .NET
author: guardrex
description: Saiba mais sobre o Host Genérico no .NET, que é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 33e5829ce4a09e132743b4174a588cf232a44775
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276247"
---
# <a name="net-generic-host"></a><span data-ttu-id="bfa6c-103">Host Genérico .NET</span><span class="sxs-lookup"><span data-stu-id="bfa6c-103">.NET Generic Host</span></span>

<span data-ttu-id="bfa6c-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bfa6c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bfa6c-105">Aplicativos ASP.NET Core configuram e inicializam um *host*.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="bfa6c-106">O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="bfa6c-107">Este tópico aborda o Host Genérico do ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), que é útil para a hospedagem de aplicativos que não processam solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="bfa6c-108">Para cobertura do host da Web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), veja o tópico [Host da Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="bfa6c-109">O objetivo do Host Genérico é separar o pipeline HTTP da API de host da Web para permitir maior gama de cenários de host.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="bfa6c-110">Sistema de mensagens, tarefas em segundo plano e outras cargas de trabalho não HTTP com base no benefício do Host Genérico de recursos abrangentes, como configuração, DI (injeção de dependência) e log.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="bfa6c-111">O Host Genérico é novo no ASP.NET Core 2.1 e não é adequado para cenários de hospedagem na Web.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="bfa6c-112">Para cenários de hospedagem na Web, use o [host da Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="bfa6c-113">O Host Genérico está em desenvolvimento para substituir o host da Web em uma versão futura e atuar como API do host principal em cenários HTTP e não HTTP.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="bfa6c-114">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bfa6c-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="bfa6c-115">Ao executar o aplicativo de exemplo no [Visual Studio Code](https://code.visualstudio.com/), use um *terminal externo ou integrado*.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="bfa6c-116">Não execute o exemplo em um `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="bfa6c-117">Para definir o console no Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="bfa6c-118">Abra o arquivo *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="bfa6c-119">Na configuração **Inicialização do .NET Core (console)**, localize a entrada **console**.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="bfa6c-120">Defina o valor como `externalTerminal` ou `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="bfa6c-121">Introdução</span><span class="sxs-lookup"><span data-stu-id="bfa6c-121">Introduction</span></span>

<span data-ttu-id="bfa6c-122">A biblioteca do Host Genérico está disponível no [namespace Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) e é fornecida pelo [pacote Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="bfa6c-123">O pacote `Microsoft.Extensions.Hosting` está incluído no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou posterior).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="bfa6c-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) é o ponto de entrada para a execução de código.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="bfa6c-125">Cada implementação do `IHostedService` é executada na ordem do [registro de serviço em ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="bfa6c-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) é chamado em cada `IHostedService` quando o host é iniciado, e [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) é chamado na ordem inversa de registro quando o host é desligado normalmente.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="bfa6c-127">Configurar um host</span><span class="sxs-lookup"><span data-stu-id="bfa6c-127">Set up a host</span></span>

<span data-ttu-id="bfa6c-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) é o principal componente usado por aplicativos e bibliotecas para inicializar, compilar e executar o host:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="bfa6c-129">Configuração do host</span><span class="sxs-lookup"><span data-stu-id="bfa6c-129">Host configuration</span></span>

<span data-ttu-id="bfa6c-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) conta com as seguintes abordagens para definir os valores de configuração do host:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="bfa6c-131">Construtor de configuração</span><span class="sxs-lookup"><span data-stu-id="bfa6c-131">Configuration builder</span></span>
* <span data-ttu-id="bfa6c-132">Configuração do método de extensão</span><span class="sxs-lookup"><span data-stu-id="bfa6c-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="bfa6c-133">Construtor de configuração</span><span class="sxs-lookup"><span data-stu-id="bfa6c-133">Configuration builder</span></span>

<span data-ttu-id="bfa6c-134">A configuração do construtor de host é criada chamando [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) na implementação [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="bfa6c-135">`ConfigureHostConfiguration` usa um [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) para criar um [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para o host.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="bfa6c-136">O construtor de configuração inicializa o [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) para uso no processo de compilação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="bfa6c-137">A configuração de variável de ambiente não é adicionada por padrão.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-137">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="bfa6c-138">Chame [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) no construtor de host para configurar o host com base em variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-138">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="bfa6c-139">`AddEnvironmentVariables` aceita um prefixo opcional definido pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-139">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="bfa6c-140">O aplicativo de exemplo usa um prefixo igual a `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-140">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="bfa6c-141">O prefixo é removido quando as variáveis de ambiente são lidas.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-141">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="bfa6c-142">Quando o host do aplicativo de exemplo é configurado, o valor da variável de ambiente de `PREFIX_ENVIRONMENT` torna-se o valor de configuração de host para a chave `environment`.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-142">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="bfa6c-143">Durante o desenvolvimento, ao usar o [Visual Studio](https://www.visualstudio.com/) ou executar um aplicativo com `dotnet run`, as variáveis de ambiente poderão ser definidas no arquivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-143">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="bfa6c-144">No [Visual Studio Code](https://code.visualstudio.com/), as variáveis de ambiente podem ser definidas no arquivo *.vscode/launch.json* durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-144">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="bfa6c-145">Para obter mais informações, veja [Usar vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-145">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="bfa6c-146">`ConfigureHostConfiguration` pode ser chamado várias vezes com resultados aditivos.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-146">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="bfa6c-147">O host usa a opção que define um valor por último.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-147">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="bfa6c-148">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-148">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="bfa6c-149">Exemplo da configuração `HostBuilder` usando `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-149">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="bfa6c-150">Atualmente, o método de extensão [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) não é capaz de analisar uma seção de configuração retornada por [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (por exemplo, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-150">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="bfa6c-151">O método `GetSection` filtra as chaves de configuração da seção solicitada, mas deixa o nome da seção nas chaves (por exemplo, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-151">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="bfa6c-152">O método `AddConfiguration` espera que as chaves correspondam às chaves `HostBuilder` (por exemplo, `environment`).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-152">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="bfa6c-153">A presença do nome da seção nas chaves impede que os valores da seção configurem o host.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-153">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="bfa6c-154">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-154">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="bfa6c-155">Para obter mais informações e soluções alternativas, consulte [Passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-155">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="bfa6c-156">Configuração do método de extensão</span><span class="sxs-lookup"><span data-stu-id="bfa6c-156">Extension method configuration</span></span>

<span data-ttu-id="bfa6c-157">Métodos de extensão são chamados na implementação `IHostBuilder` para configurar a raiz do conteúdo e o ambiente.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-157">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="bfa6c-158">Raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="bfa6c-158">Content Root</span></span>

<span data-ttu-id="bfa6c-159">Essa configuração determina onde o host começa a procurar por arquivos de conteúdo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-159">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="bfa6c-160">**Chave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="bfa6c-160">**Key**: contentRoot</span></span>  
<span data-ttu-id="bfa6c-161">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="bfa6c-161">**Type**: *string*</span></span>  
<span data-ttu-id="bfa6c-162">**Padrão**: o padrão é a pasta em que o assembly do aplicativo reside.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-162">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="bfa6c-163">**Definido usando**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="bfa6c-163">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="bfa6c-164">**Variável de ambiente**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` é [opcional e definida pelo usuário](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="bfa6c-164">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="bfa6c-165">Se o caminho não existir, o host não será iniciado.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-165">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="bfa6c-166">Ambiente</span><span class="sxs-lookup"><span data-stu-id="bfa6c-166">Environment</span></span>

<span data-ttu-id="bfa6c-167">Define o [ambiente](xref:fundamentals/environments) do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-167">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="bfa6c-168">**Chave**: ambiente</span><span class="sxs-lookup"><span data-stu-id="bfa6c-168">**Key**: environment</span></span>  
<span data-ttu-id="bfa6c-169">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="bfa6c-169">**Type**: *string*</span></span>  
<span data-ttu-id="bfa6c-170">**Padrão**: Production</span><span class="sxs-lookup"><span data-stu-id="bfa6c-170">**Default**: Production</span></span>  
<span data-ttu-id="bfa6c-171">**Definido usando**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="bfa6c-171">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="bfa6c-172">**Variável de ambiente**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` é [opcional e definida pelo usuário](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="bfa6c-172">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="bfa6c-173">O ambiente pode ser definido como qualquer valor.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-173">The environment can be set to any value.</span></span> <span data-ttu-id="bfa6c-174">Os valores definidos pela estrutura incluem `Development`, `Staging` e `Production`.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-174">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="bfa6c-175">Os valores não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-175">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="bfa6c-176">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="bfa6c-176">ConfigureAppConfiguration</span></span>

<span data-ttu-id="bfa6c-177">A configuração do construtor de aplicativos é criada chamando [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) na implementação [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-177">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="bfa6c-178">`ConfigureAppConfiguration` usa um [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) para criar um [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-178">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="bfa6c-179">`ConfigureAppConfiguration` pode ser chamado várias vezes com resultados aditivos.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-179">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="bfa6c-180">O aplicativo usa a opção que define um valor por último.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-180">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="bfa6c-181">A configuração criada pelo `ConfigureAppConfiguration` está disponível em [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) para operações seguintes e em [Serviços](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-181">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="bfa6c-182">Exemplo da configuração de aplicativo usando `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-182">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="bfa6c-183">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-183">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="bfa6c-184">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-184">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="bfa6c-185">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-185">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="bfa6c-186">Atualmente, o método de extensão [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) não é capaz de analisar uma seção de configuração retornada por [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (por exemplo, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-186">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="bfa6c-187">O método `GetSection` filtra as chaves de configuração da seção solicitada, mas deixa o nome da seção nas chaves (por exemplo, `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-187">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="bfa6c-188">O método `AddConfiguration` espera uma correspondência exata para chaves de configuração (por exemplo, `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-188">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="bfa6c-189">A presença do nome da seção nas chaves impede que os valores da seção configurem o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-189">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="bfa6c-190">Esse problema será corrigido em uma próxima versão.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-190">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="bfa6c-191">Para obter mais informações e soluções alternativas, consulte [Passar a seção de configuração para WebHostBuilder.UseConfiguration usa chaves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-191">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

## <a name="configureservices"></a><span data-ttu-id="bfa6c-192">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="bfa6c-192">ConfigureServices</span></span>

<span data-ttu-id="bfa6c-193">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adiciona serviços ao contêiner [injeção de dependência](xref:fundamentals/dependency-injection) do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-193">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="bfa6c-194">`ConfigureServices` pode ser chamado várias vezes com resultados aditivos.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-194">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="bfa6c-195">Um serviço hospedado é uma classe com lógica de tarefa em segundo plano que implementa a interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-195">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="bfa6c-196">Saiba mais no tópico [Tarefas em segundo plano com serviços hospedados](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-196">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="bfa6c-197">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa o método de extensão `AddHostedService` para adicionar um serviço para eventos de tempo de vida, `LifetimeEventsHostedService`, e uma tarefa em segundo plano programada, `TimedHostedService`, para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-197">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="bfa6c-198">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="bfa6c-198">ConfigureLogging</span></span>

<span data-ttu-id="bfa6c-199">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adiciona um delegado para configurar o [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) fornecido.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-199">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="bfa6c-200">`ConfigureLogging` pode ser chamado várias vezes com resultados aditivos.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-200">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="bfa6c-201">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="bfa6c-201">UseConsoleLifetime</span></span>

<span data-ttu-id="bfa6c-202">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) escuta `Ctrl+C`/SIGINT ou SIGTERM e chama [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) para iniciar o processo de desligamento.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-202">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="bfa6c-203">`UseConsoleLifetime` desbloqueia extensões como [RunAsync](#runasync) e [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-203">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="bfa6c-204">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) é previamente registrado como a implementação de tempo de vida padrão.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-204">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="bfa6c-205">O último tempo de vida registrado é usado.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-205">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="bfa6c-206">Configuração do contêiner</span><span class="sxs-lookup"><span data-stu-id="bfa6c-206">Container configuration</span></span>

<span data-ttu-id="bfa6c-207">Para dar suporte à conexão de outros contêineres, o host pode aceitar um [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-207">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="bfa6c-208">O fornecimento de um alocador não faz parte do registro de contêiner de injeção de dependência, mas, em vez disso, um host intrínseco é usado para criar o contêiner de DI concreto.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-208">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="bfa6c-209">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) substitui o alocador padrão usado para criar o provedor de serviços do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-209">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="bfa6c-210">A configuração de contêiner personalizado é gerenciada pelo método [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-210">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="bfa6c-211">`ConfigureContainer` fornece uma experiência fortemente tipada para configurar o contêiner sobre a API do host subjacente.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-211">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="bfa6c-212">`ConfigureContainer` pode ser chamado várias vezes com resultados aditivos.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-212">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="bfa6c-213">Crie um contêiner de serviço para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-213">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="bfa6c-214">Forneça um alocador de contêiner de serviço:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-214">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="bfa6c-215">Use o alocador e configure o contêiner de serviço personalizado para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-215">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="bfa6c-216">Extensibilidade</span><span class="sxs-lookup"><span data-stu-id="bfa6c-216">Extensibility</span></span>

<span data-ttu-id="bfa6c-217">Extensibilidade de host é executada com métodos de extensão em `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-217">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="bfa6c-218">O exemplo a seguir mostra como um método de extensão estende uma implementação `IHostBuilder` com [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-218">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="bfa6c-219">O método de extensão (em outro lugar no aplicativo) registra um RabbitMQ `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-219">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="bfa6c-220">Gerenciar o host</span><span class="sxs-lookup"><span data-stu-id="bfa6c-220">Manage the host</span></span>

<span data-ttu-id="bfa6c-221">A implementação [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) é responsável por iniciar e parar as implementações `IHostedService` que estão registradas no contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-221">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="bfa6c-222">Executar</span><span class="sxs-lookup"><span data-stu-id="bfa6c-222">Run</span></span>

<span data-ttu-id="bfa6c-223">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) executa o aplicativo e bloqueia o thread de chamada até que o host seja desligado:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-223">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="bfa6c-224">RunAsync</span><span class="sxs-lookup"><span data-stu-id="bfa6c-224">RunAsync</span></span>

<span data-ttu-id="bfa6c-225">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) executa o aplicativo e retorna um `Task` que é concluído quando o token de cancelamento ou o desligamento é disparado:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-225">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="bfa6c-226">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="bfa6c-226">RunConsoleAsync</span></span>

<span data-ttu-id="bfa6c-227">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) habilita o suporte do console, compila e inicia o host e aguarda `Ctrl+C`/SIGINT ou SIGTERM desligar.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-227">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="bfa6c-228">Start e StopAsync</span><span class="sxs-lookup"><span data-stu-id="bfa6c-228">Start and StopAsync</span></span>

<span data-ttu-id="bfa6c-229">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) inicia o host de forma síncrona.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-229">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="bfa6c-230">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) tenta parar o host dentro do tempo limite oferecido.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-230">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="bfa6c-231">StartAsync e StopAsync</span><span class="sxs-lookup"><span data-stu-id="bfa6c-231">StartAsync and StopAsync</span></span>

<span data-ttu-id="bfa6c-232">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) inicia o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-232">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="bfa6c-233">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-233">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="bfa6c-234">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="bfa6c-234">WaitForShutdown</span></span>

<span data-ttu-id="bfa6c-235">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) é disparado por meio de [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), como [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (escuta `Ctrl+C`/SIGINT ou SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-235">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="bfa6c-236">`WaitForShutdown` chama [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-236">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="bfa6c-237">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="bfa6c-237">WaitForShutdownAsync</span></span>

<span data-ttu-id="bfa6c-238">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) retorna um `Task` que é concluído quando o desligamento é disparado por meio do token fornecido e chama [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-238">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="bfa6c-239">Controle externo</span><span class="sxs-lookup"><span data-stu-id="bfa6c-239">External control</span></span>

<span data-ttu-id="bfa6c-240">O controle externo do host pode ser obtido usando os métodos que podem ser chamados externamente:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-240">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="bfa6c-241">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) é chamado no início de [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), que aguarda até que ele seja concluído antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-241">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="bfa6c-242">Isso pode ser usado para atrasar a inicialização até que seja sinalizado por um evento externo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-242">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="bfa6c-243">Interface IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="bfa6c-243">IHostingEnvironment interface</span></span>

<span data-ttu-id="bfa6c-244">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) fornece informações sobre o ambiente de hospedagem do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-244">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="bfa6c-245">Use a [injeção de construtor](xref:fundamentals/dependency-injection) para obter o `IHostingEnvironment` para usar suas propriedades e métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-245">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="bfa6c-246">Para obter mais informações, veja [Usar vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="bfa6c-246">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="bfa6c-247">Interface IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="bfa6c-247">IApplicationLifetime interface</span></span>

<span data-ttu-id="bfa6c-248">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) permite atividades pós-inicialização e desligamento, inclusive solicitações de desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-248">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="bfa6c-249">Três propriedades na interface são tokens de cancelamento usados para registrar métodos `Action` que definem eventos de inicialização e desligamento.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-249">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="bfa6c-250">Token de cancelamento</span><span class="sxs-lookup"><span data-stu-id="bfa6c-250">Cancellation Token</span></span> | <span data-ttu-id="bfa6c-251">Acionado quando&#8230;</span><span class="sxs-lookup"><span data-stu-id="bfa6c-251">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="bfa6c-252">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="bfa6c-252">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="bfa6c-253">O host foi iniciado totalmente.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-253">The host has fully started.</span></span> |
| [<span data-ttu-id="bfa6c-254">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="bfa6c-254">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="bfa6c-255">O host está concluindo um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-255">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="bfa6c-256">Todas as solicitações devem ser processadas.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-256">All requests should be processed.</span></span> <span data-ttu-id="bfa6c-257">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-257">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="bfa6c-258">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="bfa6c-258">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="bfa6c-259">O host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-259">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="bfa6c-260">Solicitações ainda podem estar sendo processadas.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-260">Requests may still be processing.</span></span> <span data-ttu-id="bfa6c-261">O desligamento é bloqueado até que esse evento seja concluído.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-261">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="bfa6c-262">O construtor injeta o serviço `IApplicationLifetime` em qualquer classe.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-262">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="bfa6c-263">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa injeção de construtor em uma classe `LifetimeEventsHostedService` (uma implementação `IHostedService`) para registrar os eventos.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-263">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses contructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="bfa6c-264">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-264">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="bfa6c-265">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) solicita o término do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bfa6c-265">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="bfa6c-266">A classe a seguir usa `StopApplication` para desligar normalmente um aplicativo quando o método `Shutdown` da classe é chamado:</span><span class="sxs-lookup"><span data-stu-id="bfa6c-266">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="bfa6c-267">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="bfa6c-267">Additional resources</span></span>

* [<span data-ttu-id="bfa6c-268">Tarefas em segundo plano com serviços hospedados</span><span class="sxs-lookup"><span data-stu-id="bfa6c-268">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="bfa6c-269">Hospedagem de exemplos do repositório no GitHub</span><span class="sxs-lookup"><span data-stu-id="bfa6c-269">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
