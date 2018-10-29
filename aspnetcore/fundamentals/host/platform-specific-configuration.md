---
title: Aprimorar um aplicativo por meio de um assembly externo no ASP.NET Core com IHostingStartup
author: guardrex
description: Descubra como aprimorar um aplicativo ASP.NET Core por meio de um assembly externo usando uma implementação de IHostingStartup.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: a06c2da04c1631f5811a535c891ca5190b0d8864
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207531"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="0576b-103">Aprimorar um aplicativo por meio de um assembly externo no ASP.NET Core com IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="0576b-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="0576b-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0576b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0576b-105">Uma implementação [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (inicialização de hospedagem) adiciona melhorias a um aplicativo durante a inicialização de um assembly externo.</span><span class="sxs-lookup"><span data-stu-id="0576b-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="0576b-106">Por exemplo, uma biblioteca externa pode usar uma implementação de inicialização de hospedagem para fornecer serviços ou provedores de configuração adicionais a um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0576b-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="0576b-107">`IHostingStartup` *está disponível no ASP.NET Core 2.0 ou posterior.*</span><span class="sxs-lookup"><span data-stu-id="0576b-107">`IHostingStartup` *is available in ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="0576b-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([como baixar](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0576b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="0576b-109">Atributo HostingStartup</span><span class="sxs-lookup"><span data-stu-id="0576b-109">HostingStartup attribute</span></span>

<span data-ttu-id="0576b-110">Um atributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) indica a presença de um assembly de inicialização de hospedagem para ativar em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="0576b-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="0576b-111">O assembly de entrada ou o assembly que contém a classe `Startup` é automaticamente examinado para o atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="0576b-111">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="0576b-112">A lista de assemblies a ser pesquisada para os atributos `HostingStartup` é carregada no tempo de execução da configuração em [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="0576b-112">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="0576b-113">A lista de assemblies para excluir da descoberta é carregada de [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="0576b-113">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="0576b-114">Para obter mais informações, consulte [Host da Web: assemblies de inicialização de hospedagem](xref:fundamentals/host/web-host#hosting-startup-assemblies) e [Host da Web: assemblies de exclusão da inicialização de hospedagem](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="0576b-114">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="0576b-115">No exemplo a seguir, o namespace do assembly de inicialização de hospedagem é `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="0576b-115">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="0576b-116">A classe que contém o código de inicialização de hospedagem é `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="0576b-116">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="0576b-117">O atributo `HostingStartup` normalmente está localizado no arquivo de classe de implementação `IHostingStartup` do assembly de inicialização de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="0576b-117">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="0576b-118">Descobrir assemblies de inicialização de hospedagem carregados</span><span class="sxs-lookup"><span data-stu-id="0576b-118">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="0576b-119">Para descobrir os assemblies de inicialização de hospedagem carregados, habilite o registro em log e verifique os logs do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0576b-119">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="0576b-120">Erros que ocorrem quando os assemblies carregados são registrados em log.</span><span class="sxs-lookup"><span data-stu-id="0576b-120">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="0576b-121">Os assemblies de inicialização de hospedagem carregados são registrados em log no nível Depuração e todos os erros são registrados.</span><span class="sxs-lookup"><span data-stu-id="0576b-121">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="0576b-122">Desabilitar o carregamento automático de assemblies de inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="0576b-122">Disable automatic loading of hosting startup assemblies</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0576b-123">Para desabilitar o carregamento automático de assemblies de inicialização de hospedagem, use uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="0576b-123">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="0576b-124">Para impedir o carregamento de todos os assemblies de inicialização de hospedagem, defina o seguinte para `true` ou `1`:</span><span class="sxs-lookup"><span data-stu-id="0576b-124">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="0576b-125">Configuração do host [Impedir inicialização de hospedagem](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="0576b-125">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="0576b-126">A variável de ambiente `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="0576b-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="0576b-127">Para evitar o carregamento de assemblies específicos de inicialização de hospedagem, defina uma das opções a seguir como uma cadeia de caracteres delimitada por ponto e vírgula de assemblies de inicialização de hospedagem para excluir na inicialização:</span><span class="sxs-lookup"><span data-stu-id="0576b-127">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="0576b-128">Configuração do host [Assemblies de exclusão de inicialização de hospedagem](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="0576b-128">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="0576b-129">A variável de ambiente `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="0576b-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0576b-130">Para desabilitar o carregamento automático de assemblies de inicialização de hospedagem, defina um dos seguintes como `true` ou `1`:</span><span class="sxs-lookup"><span data-stu-id="0576b-130">To disable automatic loading of hosting startup assemblies, set one of the following to `true` or `1`:</span></span>

* <span data-ttu-id="0576b-131">Configuração do host [Impedir inicialização de hospedagem](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="0576b-131">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="0576b-132">A variável de ambiente `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="0576b-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

::: moniker-end

<span data-ttu-id="0576b-133">Se a configuração do host e a variável de ambiente estiverem definidas, a configuração do host controlará o comportamento.</span><span class="sxs-lookup"><span data-stu-id="0576b-133">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="0576b-134">A desabilitação de assemblies de inicialização de hospedagem usando a configuração do host ou a variável de ambiente desabilita o assembly globalmente e poderá desabilitar várias características de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0576b-134">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="0576b-135">Projeto</span><span class="sxs-lookup"><span data-stu-id="0576b-135">Project</span></span>

<span data-ttu-id="0576b-136">Crie uma inicialização de hospedagem com qualquer um dos seguintes tipos de projeto:</span><span class="sxs-lookup"><span data-stu-id="0576b-136">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="0576b-137">Biblioteca de classes</span><span class="sxs-lookup"><span data-stu-id="0576b-137">Class library</span></span>](#class-library)
* [<span data-ttu-id="0576b-138">Aplicativo de console sem um ponto de entrada</span><span class="sxs-lookup"><span data-stu-id="0576b-138">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="0576b-139">Biblioteca de classes</span><span class="sxs-lookup"><span data-stu-id="0576b-139">Class library</span></span>

<span data-ttu-id="0576b-140">Uma melhoria da inicialização de hospedagem pode ser fornecida em uma biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="0576b-140">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="0576b-141">A biblioteca contém um atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="0576b-141">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="0576b-142">O [código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) inclui um aplicativo Razor Pages, *HostingStartupApp* e uma biblioteca de classes, *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="0576b-142">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="0576b-143">A biblioteca de classes:</span><span class="sxs-lookup"><span data-stu-id="0576b-143">The class library:</span></span>

* <span data-ttu-id="0576b-144">Contém uma classe de inicialização de hospedagem, `ServiceKeyInjection`, que implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="0576b-144">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="0576b-145">`ServiceKeyInjection` adiciona um par de cadeias de caracteres de serviço à configuração do aplicativo usando o provedor de configuração na memória ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="0576b-145">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="0576b-146">Inclui um atributo `HostingStartup` que identifica o namespace e a classe de inicialização de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="0576b-146">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="0576b-147">O método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) da classe `ServiceKeyInjection` usa um [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para adicionar melhorias a um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0576b-147">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="0576b-148">`IHostingStartup.Configure` no assembly de inicialização de hospedagem é chamado pelo tempo de execução antes de `Startup.Configure` no código do usuário, o que permite que o código de usuário substitua qualquer configuração fornecida pelo assembly de inicialização de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="0576b-148">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

<span data-ttu-id="0576b-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="0576b-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="0576b-150">A página de índice do aplicativo lê e renderiza os valores de configuração para as duas chaves definidas pelo assembly de inicialização de hospedagem da biblioteca de classes:</span><span class="sxs-lookup"><span data-stu-id="0576b-150">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="0576b-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="0576b-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="0576b-152">O [código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) também inclui um projeto de pacote do NuGet que fornece uma inicialização de hospedagem separada, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="0576b-152">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="0576b-153">O pacote tem as mesmas características da biblioteca de classes descrita anteriormente.</span><span class="sxs-lookup"><span data-stu-id="0576b-153">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="0576b-154">O pacote:</span><span class="sxs-lookup"><span data-stu-id="0576b-154">The package:</span></span>

* <span data-ttu-id="0576b-155">Contém uma classe de inicialização de hospedagem, `ServiceKeyInjection`, que implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="0576b-155">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="0576b-156">`ServiceKeyInjection` adiciona um par de cadeias de caracteres de serviço para a configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0576b-156">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="0576b-157">Inclui um atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="0576b-157">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="0576b-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="0576b-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="0576b-159">A página de índice do aplicativo lê e renderiza os valores de configuração para as duas chaves definidas pelo assembly de inicialização de hospedagem do pacote:</span><span class="sxs-lookup"><span data-stu-id="0576b-159">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="0576b-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="0576b-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="0576b-161">Aplicativo de console sem um ponto de entrada</span><span class="sxs-lookup"><span data-stu-id="0576b-161">Console app without an entry point</span></span>

<span data-ttu-id="0576b-162">*Essa abordagem só está disponível para aplicativos .NET Core, não para .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="0576b-162">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="0576b-163">Uma melhoria de inicialização de hospedagem dinâmica que não requer uma referência de tempo de compilação para a ativação pode ser fornecida em um aplicativo de console sem um ponto de entrada.</span><span class="sxs-lookup"><span data-stu-id="0576b-163">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point.</span></span> <span data-ttu-id="0576b-164">O aplicativo contém um atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="0576b-164">The app contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="0576b-165">Para criar uma inicialização de hospedagem dinâmica:</span><span class="sxs-lookup"><span data-stu-id="0576b-165">To create a dynamic hosting startup:</span></span>

1. <span data-ttu-id="0576b-166">Uma biblioteca de implementação é criada com base na classe que contém a implementação `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="0576b-166">An implementation library is created from the class that contains the `IHostingStartup` implementation.</span></span> <span data-ttu-id="0576b-167">A biblioteca de implementação é tratada como um pacote normal.</span><span class="sxs-lookup"><span data-stu-id="0576b-167">The implementation library is treated as a normal package.</span></span>
1. <span data-ttu-id="0576b-168">Um aplicativo de console sem um ponto de entrada referencia o pacote de biblioteca da implementação.</span><span class="sxs-lookup"><span data-stu-id="0576b-168">A console app without an entry point references the implementation library package.</span></span> <span data-ttu-id="0576b-169">Um aplicativo de console é usado, pois:</span><span class="sxs-lookup"><span data-stu-id="0576b-169">A console app is used because:</span></span>
   * <span data-ttu-id="0576b-170">Um arquivo de dependências é um ativo de aplicativo executável, então uma biblioteca não pode fornecer um arquivo de dependências.</span><span class="sxs-lookup"><span data-stu-id="0576b-170">A dependencies file is a runnable app asset, so a library can't furnish a dependencies file.</span></span>
   * <span data-ttu-id="0576b-171">Uma biblioteca não pode ser adicionada diretamente ao [repositório de pacotes de tempo de execução](/dotnet/core/deploying/runtime-store), o que exige um projeto executável que tem como alvo o tempo de execução compartilhado.</span><span class="sxs-lookup"><span data-stu-id="0576b-171">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>
1. <span data-ttu-id="0576b-172">O aplicativo de console é publicado para obter as dependências da inicialização de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="0576b-172">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="0576b-173">Uma consequência de publicar o aplicativo de console é que as dependências não utilizadas são cortadas do arquivo de dependências.</span><span class="sxs-lookup"><span data-stu-id="0576b-173">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
1. <span data-ttu-id="0576b-174">O aplicativo e seu arquivo de dependências é colocado no repositório do pacote de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="0576b-174">The app and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="0576b-175">Para descobrir o assembly de inicialização de hospedagem e seu arquivo de dependências, eles são referenciados em um par de variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="0576b-175">To discover the hosting startup assembly and its dependencies file, they're referenced in a pair of environment variables.</span></span>

<span data-ttu-id="0576b-176">O assembly de aplicativo do console referencia o pacote [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="0576b-176">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="0576b-177">Um atributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifica uma classe como uma implementação de `IHostingStartup` para o carregamento e a execução durante a criação do [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="0576b-177">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="0576b-178">No seguinte exemplo, o namespace é `StartupEnhancement` e a classe é `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="0576b-178">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="0576b-179">Uma classe implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="0576b-179">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="0576b-180">O método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) da classe usa um [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para adicionar melhorias a um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0576b-180">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="0576b-181">`IHostingStartup.Configure` no assembly de inicialização de hospedagem é chamado pelo tempo de execução antes de `Startup.Configure` no código do usuário, o que permite que o código de usuário substitua qualquer configuração fornecida pelo assembly de inicialização de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="0576b-181">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="0576b-182">Ao criar um projeto `IHostingStartup`, o arquivo de dependências (*\*.deps.json*) define o local `runtime` do assembly como a pasta *bin*:</span><span class="sxs-lookup"><span data-stu-id="0576b-182">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="0576b-183">Apenas uma parte do arquivo é mostrada.</span><span class="sxs-lookup"><span data-stu-id="0576b-183">Only part of the file is shown.</span></span> <span data-ttu-id="0576b-184">O nome do assembly no exemplo é `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="0576b-184">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="0576b-185">Especificar o assembly de inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="0576b-185">Specify the hosting startup assembly</span></span>

<span data-ttu-id="0576b-186">Para uma biblioteca de classes ou inicialização de hospedagem fornecida pelo aplicativo de console, especifique o nome do assembly de inicialização de hospedagem na variável de ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="0576b-186">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="0576b-187">A variável de ambiente é uma lista de assemblies delimitada por ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="0576b-187">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="0576b-188">Apenas assemblies de inicialização de hospedagem são examinados quanto ao atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="0576b-188">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="0576b-189">Para o aplicativo de exemplo, *HostingStartupApp*, para descobrir as inicializações de hospedagem descritas anteriormente, a variável de ambiente é definida como o seguinte valor:</span><span class="sxs-lookup"><span data-stu-id="0576b-189">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="0576b-190">Um assembly de inicialização de hospedagem também pode ser definido usando a configuração do host [Assemblies de inicialização de hospedagem](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="0576b-190">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="0576b-191">Quando há vários assemblies de inicialização de hospedagem, os métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) são executados na ordem em que os assemblies são listados.</span><span class="sxs-lookup"><span data-stu-id="0576b-191">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="0576b-192">Ativação</span><span class="sxs-lookup"><span data-stu-id="0576b-192">Activation</span></span>

<span data-ttu-id="0576b-193">As opções para ativação da inicialização de hospedagem são:</span><span class="sxs-lookup"><span data-stu-id="0576b-193">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="0576b-194">[Repositório de tempo de execução](#runtime-store) &ndash; A ativação não requer uma referência de tempo de compilação para a ativação.</span><span class="sxs-lookup"><span data-stu-id="0576b-194">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="0576b-195">O aplicativo de exemplo coloca os arquivos de dependências e o assembly de inicialização de hospedagem em uma pasta, *implantação*, para facilitar a implantação da inicialização de hospedagem em um ambiente multicomputador.</span><span class="sxs-lookup"><span data-stu-id="0576b-195">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="0576b-196">A pasta *implantação* também inclui um script do PowerShell que cria ou modifica variáveis de ambiente no sistema de implantação para habilitar a inicialização de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="0576b-196">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="0576b-197">Referência de tempo de compilação necessária para a ativação</span><span class="sxs-lookup"><span data-stu-id="0576b-197">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="0576b-198">Pacote do NuGet</span><span class="sxs-lookup"><span data-stu-id="0576b-198">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="0576b-199">Pasta Lixeira do projeto</span><span class="sxs-lookup"><span data-stu-id="0576b-199">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="0576b-200">Repositório de tempo de execução</span><span class="sxs-lookup"><span data-stu-id="0576b-200">Runtime store</span></span>

<span data-ttu-id="0576b-201">A implementação de inicialização de hospedagem é colocada no [repositório de tempo de execução](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="0576b-201">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="0576b-202">Uma referência de tempo de compilação para o assembly não é exigida pelo aplicativo aprimorado.</span><span class="sxs-lookup"><span data-stu-id="0576b-202">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="0576b-203">Depois que a inicialização de hospedagem é compilada, o arquivo de projeto de inicialização de hospedagem serve como o arquivo de manifesto para o comando [dotnet store](/dotnet/core/tools/dotnet-store).</span><span class="sxs-lookup"><span data-stu-id="0576b-203">After the hosting startup is built, the hosting startup's project file serves as the manifest file for the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

<span data-ttu-id="0576b-204">Esse comando coloca o assembly de inicialização de hospedagem e outras dependências que não fazem parte da estrutura compartilhada no repositório de tempo de execução do perfil do usuário em:</span><span class="sxs-lookup"><span data-stu-id="0576b-204">This command places the hosting startup assembly and other dependencies that aren't part of the shared framework in the user profile's runtime store at:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0576b-205">Windows</span><span class="sxs-lookup"><span data-stu-id="0576b-205">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="0576b-206">macOS</span><span class="sxs-lookup"><span data-stu-id="0576b-206">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="0576b-207">Linux</span><span class="sxs-lookup"><span data-stu-id="0576b-207">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="0576b-208">Se você desejar colocar o assembly e as dependências para uso global, adicione a opção `-o|--output` ao comando `dotnet store` com o seguinte caminho:</span><span class="sxs-lookup"><span data-stu-id="0576b-208">If you desire to place the assembly and dependencies for global use, add the `-o|--output` option to the `dotnet store` command with the following path:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0576b-209">Windows</span><span class="sxs-lookup"><span data-stu-id="0576b-209">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="0576b-210">macOS</span><span class="sxs-lookup"><span data-stu-id="0576b-210">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="0576b-211">Linux</span><span class="sxs-lookup"><span data-stu-id="0576b-211">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="0576b-212">**Modificar e colocar o arquivo de dependências da inicialização de hospedagem**</span><span class="sxs-lookup"><span data-stu-id="0576b-212">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="0576b-213">O local do tempo de execução é especificado no arquivo *\*.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="0576b-213">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="0576b-214">Para ativar a melhoria, o elemento `runtime` deve especificar o local do assembly de tempo de execução da melhoria.</span><span class="sxs-lookup"><span data-stu-id="0576b-214">To activate the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="0576b-215">Preceda o local `runtime` com `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span><span class="sxs-lookup"><span data-stu-id="0576b-215">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="0576b-216">No código de exemplo (projeto *StartupDiagnostics*), a modificação do arquivo *\*.deps.json* é executada por um script do [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="0576b-216">In the sample code (*StartupDiagnostics* project), modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="0576b-217">O script do PowerShell é disparado automaticamente por um destino de build no arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="0576b-217">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

<span data-ttu-id="0576b-218">O arquivo *\*.deps.json* da implementação deve estar em um local acessível.</span><span class="sxs-lookup"><span data-stu-id="0576b-218">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="0576b-219">Para uso por usuário, coloque o arquivo na pasta *additonalDeps* das configurações `.dotnet` do perfil do usuário:</span><span class="sxs-lookup"><span data-stu-id="0576b-219">For per-user use, place the file in the *additonalDeps* folder of the user profile's `.dotnet` settings:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0576b-220">Windows</span><span class="sxs-lookup"><span data-stu-id="0576b-220">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="0576b-221">macOS</span><span class="sxs-lookup"><span data-stu-id="0576b-221">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="0576b-222">Linux</span><span class="sxs-lookup"><span data-stu-id="0576b-222">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="0576b-223">Para uso global, coloque o arquivo na pasta *additonalDeps* da instalação do .NET Core:</span><span class="sxs-lookup"><span data-stu-id="0576b-223">For global use, place the file in the *additonalDeps* folder of the .NET Core installation:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0576b-224">Windows</span><span class="sxs-lookup"><span data-stu-id="0576b-224">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="0576b-225">macOS</span><span class="sxs-lookup"><span data-stu-id="0576b-225">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="0576b-226">Linux</span><span class="sxs-lookup"><span data-stu-id="0576b-226">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="0576b-227">Observe que a versão da estrutura compartilhada reflete a versão do tempo de execução compartilhado usado pelo aplicativo de destino.</span><span class="sxs-lookup"><span data-stu-id="0576b-227">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="0576b-228">O tempo de execução compartilhado é mostrado no arquivo *\*.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="0576b-228">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="0576b-229">No aplicativo de exemplo (*HostingStartupApp*), o tempo de execução compartilhado é especificado no arquivo *HostingStartupApp.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="0576b-229">In the sample app (*HostingStartupApp*), the shared runtime is specified in the *HostingStartupApp.runtimeconfig.json* file.</span></span>

<span data-ttu-id="0576b-230">**Listar o arquivo de dependências da inicialização de hospedagem**</span><span class="sxs-lookup"><span data-stu-id="0576b-230">**List the hosting startup's dependencies file**</span></span>

<span data-ttu-id="0576b-231">O local do arquivo *\*.deps.json* da implementação é listado na variável de ambiente `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="0576b-231">The location of the implementation's *\*.deps.json* file is listed in the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="0576b-232">Se o arquivo for colocado na pasta *.dotnet* do perfil do usuário, defina o valor da variável de ambiente para:</span><span class="sxs-lookup"><span data-stu-id="0576b-232">If the file is placed in the user profile's *.dotnet* folder, set the environment variable's value to:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0576b-233">Windows</span><span class="sxs-lookup"><span data-stu-id="0576b-233">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="0576b-234">macOS</span><span class="sxs-lookup"><span data-stu-id="0576b-234">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="0576b-235">Linux</span><span class="sxs-lookup"><span data-stu-id="0576b-235">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

<span data-ttu-id="0576b-236">Se o arquivo for colocado na instalação do .NET Core para uso global, forneça o caminho completo para o arquivo:</span><span class="sxs-lookup"><span data-stu-id="0576b-236">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0576b-237">Windows</span><span class="sxs-lookup"><span data-stu-id="0576b-237">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[<span data-ttu-id="0576b-238">macOS</span><span class="sxs-lookup"><span data-stu-id="0576b-238">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="0576b-239">Linux</span><span class="sxs-lookup"><span data-stu-id="0576b-239">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

<span data-ttu-id="0576b-240">Para o aplicativo de exemplo (*HostingStartupApp*) localizar o arquivo de dependências (*HostingStartupApp.runtimeconfig.json*), o arquivo de dependências é colocado no perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="0576b-240">For the sample app (*HostingStartupApp*) to find the dependencies file (*HostingStartupApp.runtimeconfig.json*), the dependencies file is placed in the user's profile.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0576b-241">Windows</span><span class="sxs-lookup"><span data-stu-id="0576b-241">Windows</span></span>](#tab/windows)

<span data-ttu-id="0576b-242">Use a variável de ambiente `DOTNET_ADDITIONAL_DEPS` para o valor a seguir:</span><span class="sxs-lookup"><span data-stu-id="0576b-242">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="0576b-243">macOS</span><span class="sxs-lookup"><span data-stu-id="0576b-243">macOS</span></span>](#tab/macos)

<span data-ttu-id="0576b-244">Use a variável de ambiente `DOTNET_ADDITIONAL_DEPS` para o valor a seguir:</span><span class="sxs-lookup"><span data-stu-id="0576b-244">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="0576b-245">Linux</span><span class="sxs-lookup"><span data-stu-id="0576b-245">Linux</span></span>](#tab/linux)

<span data-ttu-id="0576b-246">Use a variável de ambiente `DOTNET_ADDITIONAL_DEPS` para o valor a seguir:</span><span class="sxs-lookup"><span data-stu-id="0576b-246">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

<span data-ttu-id="0576b-247">Para obter exemplos de como definir variáveis de ambiente para vários sistemas operacionais, confira [Usar vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="0576b-247">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="0576b-248">**Implantação**</span><span class="sxs-lookup"><span data-stu-id="0576b-248">**Deployment**</span></span>

<span data-ttu-id="0576b-249">Para facilitar a implantação de uma inicialização de hospedagem em um ambiente multicomputador, o aplicativo de exemplo cria uma pasta *implantação* na saída publicada que contém:</span><span class="sxs-lookup"><span data-stu-id="0576b-249">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="0576b-250">O assembly de inicialização de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="0576b-250">The hosting startup assembly.</span></span>
* <span data-ttu-id="0576b-251">O arquivo de dependências de inicialização de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="0576b-251">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="0576b-252">Um script do PowerShell que cria ou modifica `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` e `DOTNET_ADDITIONAL_DEPS` para dar suporte à ativação da inicialização de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="0576b-252">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="0576b-253">Execute o script de um prompt de comando do PowerShell administrativo no sistema de implantação.</span><span class="sxs-lookup"><span data-stu-id="0576b-253">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="0576b-254">Pacote NuGet</span><span class="sxs-lookup"><span data-stu-id="0576b-254">NuGet package</span></span>

<span data-ttu-id="0576b-255">Uma melhoria da inicialização de hospedagem pode ser fornecida em pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="0576b-255">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="0576b-256">O pacote tem um atributo `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="0576b-256">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="0576b-257">Os tipos de inicialização de hospedagem fornecidos pelo pacote são disponibilizados para o aplicativo usando qualquer uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="0576b-257">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="0576b-258">O arquivo de projeto do aplicativo aprimorado faz uma referência de pacote para a inicialização de hospedagem no arquivo de projeto do aplicativo (uma referência de tempo de compilação).</span><span class="sxs-lookup"><span data-stu-id="0576b-258">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="0576b-259">Com a referência de tempo de compilação em vigor, o assembly de inicialização de hospedagem e todas as suas dependências são incorporados ao arquivo de dependência do aplicativo (*\*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="0576b-259">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="0576b-260">Essa abordagem se aplica a um pacote de assembly de inicialização de hospedagem publicado para [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="0576b-260">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="0576b-261">O arquivo de dependências da inicialização de hospedagem fica disponível para o aplicativo avançado, conforme descrito na seção [Repositório de tempo de execução](#runtime-store) (sem uma referência de tempo de compilação).</span><span class="sxs-lookup"><span data-stu-id="0576b-261">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="0576b-262">Para obter mais informações sobre pacotes do NuGet e o repositório de tempo de execução, consulte os tópicos a seguir:</span><span class="sxs-lookup"><span data-stu-id="0576b-262">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="0576b-263">Como criar um pacote do NuGet com ferramentas de plataforma cruzada</span><span class="sxs-lookup"><span data-stu-id="0576b-263">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="0576b-264">Publicando pacotes</span><span class="sxs-lookup"><span data-stu-id="0576b-264">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="0576b-265">Repositório de pacote de tempo de execução</span><span class="sxs-lookup"><span data-stu-id="0576b-265">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="0576b-266">Pasta Lixeira do projeto</span><span class="sxs-lookup"><span data-stu-id="0576b-266">Project bin folder</span></span>

<span data-ttu-id="0576b-267">Uma melhoria da inicialização de hospedagem pode ser fornecida por um assemby implantado na *lixeira* no aplicativo aprimorado.</span><span class="sxs-lookup"><span data-stu-id="0576b-267">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="0576b-268">Os tipos de inicialização de hospedagem fornecidos pelo assembly são disponibilizados para o aplicativo usando uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="0576b-268">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="0576b-269">O arquivo de projeto do aplicativo aprimorado faz uma referência de assembly para a inicialização de hospedagem (uma referência de tempo de compilação).</span><span class="sxs-lookup"><span data-stu-id="0576b-269">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="0576b-270">Com a referência de tempo de compilação em vigor, o assembly de inicialização de hospedagem e todas as suas dependências são incorporados ao arquivo de dependência do aplicativo (*\*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="0576b-270">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="0576b-271">Essa abordagem é aplicável quando o cenário de implantação faz chamadas para mover o assembly compilado da biblioteca de inicialização de hospedagem (arquivo DLL) para o projeto consumidor ou para um local acessível pelo projeto consumidor e uma referência de tempo de compilação é feita para o assembly de inicialização de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="0576b-271">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="0576b-272">O arquivo de dependências da inicialização de hospedagem fica disponível para o aplicativo avançado, conforme descrito na seção [Repositório de tempo de execução](#runtime-store) (sem uma referência de tempo de compilação).</span><span class="sxs-lookup"><span data-stu-id="0576b-272">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="0576b-273">Código de exemplo</span><span class="sxs-lookup"><span data-stu-id="0576b-273">Sample code</span></span>

<span data-ttu-id="0576b-274">O [código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([como baixar](xref:index#how-to-download-a-sample)) demonstra cenários de implementação de inicialização de hospedagem:</span><span class="sxs-lookup"><span data-stu-id="0576b-274">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="0576b-275">Dois assemblies de inicialização de hospedagem (bibliotecas de classes) definem um par chave-valor de configuração na memória cada:</span><span class="sxs-lookup"><span data-stu-id="0576b-275">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="0576b-276">Pacote do NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="0576b-276">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="0576b-277">Biblioteca de classes (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="0576b-277">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="0576b-278">Uma inicialização de hospedagem é ativada de um assembly implantado pelo repositório de tempo de execução (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="0576b-278">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="0576b-279">O assembly adiciona dois middlewares ao aplicativo na inicialização que fornecem informações de diagnóstico sobre:</span><span class="sxs-lookup"><span data-stu-id="0576b-279">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="0576b-280">Serviços registrados</span><span class="sxs-lookup"><span data-stu-id="0576b-280">Registered services</span></span>
  * <span data-ttu-id="0576b-281">Endereço (esquema, host, base do caminho, caminho, cadeia de caracteres de consulta)</span><span class="sxs-lookup"><span data-stu-id="0576b-281">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="0576b-282">Conexão (IP remoto, porta remota, IP local, porta local, certificado do cliente)</span><span class="sxs-lookup"><span data-stu-id="0576b-282">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="0576b-283">Cabeçalhos de solicitação</span><span class="sxs-lookup"><span data-stu-id="0576b-283">Request headers</span></span>
  * <span data-ttu-id="0576b-284">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="0576b-284">Environment variables</span></span>

<span data-ttu-id="0576b-285">Para executar a amostra:</span><span class="sxs-lookup"><span data-stu-id="0576b-285">To run the sample:</span></span>

<span data-ttu-id="0576b-286">**Ativação de um pacote do NuGet**</span><span class="sxs-lookup"><span data-stu-id="0576b-286">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="0576b-287">Compile o pacote *HostingStartupPackage* com o comando [dotnet pack](/dotnet/core/tools/dotnet-pack).</span><span class="sxs-lookup"><span data-stu-id="0576b-287">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="0576b-288">Adicione o nome do assembly do pacote do *HostingStartupPackage* para a variável de ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="0576b-288">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="0576b-289">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0576b-289">Compile and run the app.</span></span> <span data-ttu-id="0576b-290">Uma referência de pacote está presente no aplicativo aprimorado (uma referência de tempo de compilação).</span><span class="sxs-lookup"><span data-stu-id="0576b-290">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="0576b-291">Um `<PropertyGroup>` no arquivo de projeto do aplicativo especifica a saída do projeto de pacote (*../HostingStartupPackage/bin/Debug*) como uma origem de pacote.</span><span class="sxs-lookup"><span data-stu-id="0576b-291">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="0576b-292">Isso permite que o aplicativo use o pacote sem carregar o pacote para [nuget.org](https://www.nuget.org/). Para mais informações, consulte as notas no arquivo de projeto do HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="0576b-292">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. <span data-ttu-id="0576b-293">Observe que os valores de chave de configuração do serviço renderizados pela página de índice correspondem aos valores definidos pelo método `ServiceKeyInjection.Configure` do pacote.</span><span class="sxs-lookup"><span data-stu-id="0576b-293">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="0576b-294">Se você fizer alterações no projeto *HostingStartupPackage* e recompilá-lo, limpe os caches de pacote do NuGet locais para garantir que o *HostingStartupApp* receba o pacote atualizado e não um pacote obsoleto do cache local.</span><span class="sxs-lookup"><span data-stu-id="0576b-294">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="0576b-295">Para limpar os caches locais do NuGet, execute o seguinte comando [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals):</span><span class="sxs-lookup"><span data-stu-id="0576b-295">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="0576b-296">**Ativação de uma biblioteca de classes**</span><span class="sxs-lookup"><span data-stu-id="0576b-296">**Activation from a class library**</span></span>

1. <span data-ttu-id="0576b-297">Compile a biblioteca de classes *HostingStartupLibrary* com o comando [dotnet build](/dotnet/core/tools/dotnet-build).</span><span class="sxs-lookup"><span data-stu-id="0576b-297">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="0576b-298">Adicione nome do assembly da biblioteca de classes do *HostingStartupLibrary* à variável de ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="0576b-298">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="0576b-299">A pasta *lixeira* implanta o assembly da biblioteca de classes para o aplicativo ao copiar o arquivo *HostingStartupLibrary.dll* da saída compilada da biblioteca de classes para a pasta *lixeira/Depurar* do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0576b-299">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="0576b-300">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0576b-300">Compile and run the app.</span></span> <span data-ttu-id="0576b-301">Um `<ItemGroup>` do arquivo de projeto do aplicativo referencia o assembly da biblioteca de classes (*.\lixeira\Depurar\netcoreapp2.1\HostingStartupLibrary.dll*) (uma referência de tempo de compilação).</span><span class="sxs-lookup"><span data-stu-id="0576b-301">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="0576b-302">Para mais informações, consulte as notas no arquivo de projeto do HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="0576b-302">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. <span data-ttu-id="0576b-303">Observe que os valores de chave de configuração do serviço renderizados pela página de índice correspondem aos valores definidos pelo método `ServiceKeyInjection.Configure` da biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="0576b-303">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="0576b-304">**Ativação de um assembly implantado pelo repositório de tempo de execução**</span><span class="sxs-lookup"><span data-stu-id="0576b-304">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="0576b-305">O projeto *StartupDiagnostics* usa o [PowerShell](/powershell/scripting/powershell-scripting) para modificar seu arquivo *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="0576b-305">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="0576b-306">O PowerShell é instalado por padrão em um sistema operacional Windows começando no Windows 7 SP1 e no Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="0576b-306">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="0576b-307">Para obter o PowerShell em outras plataformas, confira [Instalando o Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="0576b-307">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="0576b-308">Compilar o projeto *StartupDiagnostics*.</span><span class="sxs-lookup"><span data-stu-id="0576b-308">Build the *StartupDiagnostics* project.</span></span> <span data-ttu-id="0576b-309">Depois que o projeto é compilado, um destino de compilação no arquivo de projeto automaticamente:</span><span class="sxs-lookup"><span data-stu-id="0576b-309">After the project is built, a build target in the project file automatically:</span></span>
   * <span data-ttu-id="0576b-310">Dispara o script do PowerShell para modificar o arquivo *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="0576b-310">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="0576b-311">Move o arquivo *StartupDiagnostics.deps.json* para a pasta *additionalDeps* do perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="0576b-311">Moves the *StartupDiagnostics.deps.json* file to the user profile's *additionalDeps* folder.</span></span>
1. <span data-ttu-id="0576b-312">Execute o comando `dotnet store` em um prompt de comando no diretório de inicialização de hospedagem para armazenar o assembly e suas dependências no repositório de tempo de execução do perfil do usuário:</span><span class="sxs-lookup"><span data-stu-id="0576b-312">Execute the `dotnet store` command at a command prompt in the hosting startup's directory to store the assembly and its dependencies in the user profile's runtime store:</span></span>

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   <span data-ttu-id="0576b-313">Para Windows, o comando usa o `win7-x64` [RID (identificador de tempo de execução)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="0576b-313">For Windows, the command uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="0576b-314">Ao fornecer a inicialização de hospedagem para um tempo de execução diferente, substitua o RID correto.</span><span class="sxs-lookup"><span data-stu-id="0576b-314">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="0576b-315">Defina as variáveis de ambiente:</span><span class="sxs-lookup"><span data-stu-id="0576b-315">Set the environment variables:</span></span>
   * <span data-ttu-id="0576b-316">Adicione o nome do assembly de *StartupDiagnostics* para a variável de ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="0576b-316">Add the assembly name of *StartupDiagnostics* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="0576b-317">No Windows, defina a variável de ambiente `DOTNET_ADDITIONAL_DEPS` como `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span><span class="sxs-lookup"><span data-stu-id="0576b-317">On Windows, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span></span> <span data-ttu-id="0576b-318">No macOS/Linux, defina a variável de ambiente `DOTNET_ADDITIONAL_DEPS` como `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, em que `<USER>` é o perfil do usuário que contém a inicialização de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="0576b-318">On macOS/Linux, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, where `<USER>` is the user profile that contains the hosting startup.</span></span>
1. <span data-ttu-id="0576b-319">Execute o aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="0576b-319">Run the sample app.</span></span>
1. <span data-ttu-id="0576b-320">Solicite o ponto de extremidade `/services` para ver os serviços registrados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0576b-320">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="0576b-321">Solicite o ponto de extremidade `/diag` para ver as informações de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="0576b-321">Request the `/diag` endpoint to see the diagnostic information.</span></span>
