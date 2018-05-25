---
title: Aprimorar um aplicativo por meio de um assembly externo no ASP.NET Core com IHostingStartup
author: guardrex
description: Descubra como aprimorar um aplicativo ASP.NET Core por meio de um assembly externo usando uma implementação de IHostingStartup.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 793169b491596cd7326d747a3f19d7fdaf7e2b65
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="cb04b-103">Aprimorar um aplicativo por meio de um assembly externo no ASP.NET Core com IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="cb04b-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="cb04b-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cb04b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="cb04b-105">Uma implementação [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) permite adicionar melhorias a um aplicativo durante a inicialização de um assembly externo fora da classe `Startup` do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cb04b-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="cb04b-106">Por exemplo, uma biblioteca de ferramentas externas pode usar uma implementação de `IHostingStartup` para fornecer serviços ou provedores de configuração adicionais a um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cb04b-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="cb04b-107">`IHostingStartup` *está disponível no ASP.NET Core 2.0 e posterior.*</span><span class="sxs-lookup"><span data-stu-id="cb04b-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="cb04b-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cb04b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="cb04b-109">Descobrir assemblies de inicialização de hospedagem carregados</span><span class="sxs-lookup"><span data-stu-id="cb04b-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="cb04b-110">Para descobrir os assemblies de inicialização de hospedagem carregados pelo aplicativo ou por bibliotecas, habilite o log e verifique os logs do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cb04b-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="cb04b-111">Erros que ocorrem quando os assemblies carregados são registrados em log.</span><span class="sxs-lookup"><span data-stu-id="cb04b-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="cb04b-112">Os assemblies de inicialização de hospedagem carregados são registrados em log no nível Depuração e todos os erros são registrados.</span><span class="sxs-lookup"><span data-stu-id="cb04b-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="cb04b-113">O aplicativo de exemplo lê a [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) em uma matriz `string` e exibe o resultado na página Índice do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="cb04b-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="cb04b-114">Desabilitar o carregamento automático de assemblies de inicialização de hospedagem</span><span class="sxs-lookup"><span data-stu-id="cb04b-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="cb04b-115">Há duas maneiras para desabilitar o carregamento automático de assemblies de inicialização de hospedagem:</span><span class="sxs-lookup"><span data-stu-id="cb04b-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="cb04b-116">Definir a configuração do host [Impedir Inicialização de Hospedagem](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="cb04b-116">Set the [Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="cb04b-117">Definir a variável de ambiente `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="cb04b-117">Set the `ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

<span data-ttu-id="cb04b-118">Quando a configuração do host ou a variável de ambiente é definida como `true` ou `1`, os assemblies de inicialização de hospedagem não são carregados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="cb04b-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="cb04b-119">Se ambas estiverem definidas, a configuração do host controlará o comportamento.</span><span class="sxs-lookup"><span data-stu-id="cb04b-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="cb04b-120">A desabilitação de assemblies de inicialização de hospedagem usando a configuração do host ou a variável de ambiente desabilita-os globalmente e poderá desabilitar várias características de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cb04b-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several characteristics of an app.</span></span> <span data-ttu-id="cb04b-121">No momento, não é possível desabilitar seletivamente um assembly de inicialização de hospedagem adicionado por uma biblioteca, a menos que a biblioteca ofereça sua própria opção de configuração.</span><span class="sxs-lookup"><span data-stu-id="cb04b-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="cb04b-122">Uma versão futura oferecerá a capacidade de desabilitar seletivamente os assemblies de inicialização de hospedagem (confira [Problema do GitHub aspnet/Hospedagem nº 1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="cb04b-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup"></a><span data-ttu-id="cb04b-123">Implementar IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="cb04b-123">Implement IHostingStartup</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="cb04b-124">Criar o assembly</span><span class="sxs-lookup"><span data-stu-id="cb04b-124">Create the assembly</span></span>

<span data-ttu-id="cb04b-125">Uma melhoria de `IHostingStartup` é implantada como um assembly com base em um aplicativo de console sem um ponto de entrada.</span><span class="sxs-lookup"><span data-stu-id="cb04b-125">An `IHostingStartup` enhancement is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="cb04b-126">O assembly referencia o pacote [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="cb04b-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

<span data-ttu-id="cb04b-127">Um atributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifica uma classe como uma implementação de `IHostingStartup` para o carregamento e a execução durante a criação do [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="cb04b-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="cb04b-128">No seguinte exemplo, o namespace é `StartupEnhancement` e a classe é `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="cb04b-128">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="cb04b-129">Uma classe implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="cb04b-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="cb04b-130">O método [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) da classe usa um [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para adicionar melhorias a um aplicativo:</span><span class="sxs-lookup"><span data-stu-id="cb04b-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="cb04b-131">Ao criar um projeto `IHostingStartup`, o arquivo de dependências (*\*.deps.json*) define o local `runtime` do assembly como a pasta *bin*:</span><span class="sxs-lookup"><span data-stu-id="cb04b-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="cb04b-132">Apenas uma parte do arquivo é mostrada.</span><span class="sxs-lookup"><span data-stu-id="cb04b-132">Only part of the file is shown.</span></span> <span data-ttu-id="cb04b-133">O nome do assembly no exemplo é `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="cb04b-133">The assembly name in the example is `StartupEnhancement`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="cb04b-134">Atualizar o arquivo de dependências</span><span class="sxs-lookup"><span data-stu-id="cb04b-134">Update the dependencies file</span></span>

<span data-ttu-id="cb04b-135">O local do tempo de execução é especificado no arquivo *\*.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="cb04b-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="cb04b-136">Para ativar a melhoria, o elemento `runtime` deve especificar o local do assembly de tempo de execução da melhoria.</span><span class="sxs-lookup"><span data-stu-id="cb04b-136">To active the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="cb04b-137">Preceda o local `runtime` com `lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="cb04b-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="cb04b-138">No aplicativo de exemplo, a modificação do arquivo *\*.deps.json* é executada por um script do [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="cb04b-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="cb04b-139">O script do PowerShell é disparado automaticamente por um destino de build no arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="cb04b-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="enhancement-activation"></a><span data-ttu-id="cb04b-140">Ativação da melhoria</span><span class="sxs-lookup"><span data-stu-id="cb04b-140">Enhancement activation</span></span>

<span data-ttu-id="cb04b-141">**Colocar o arquivo do assembly**</span><span class="sxs-lookup"><span data-stu-id="cb04b-141">**Place the assembly file**</span></span>

<span data-ttu-id="cb04b-142">O arquivo do assembly da implementação de `IHostingStartup` deve estar implantado em *bin* no aplicativo ou colocado no [repositório de tempo de execução](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="cb04b-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="cb04b-143">Para uso por usuário, coloque o assembly no repositório de tempo de execução do perfil do usuário em:</span><span class="sxs-lookup"><span data-stu-id="cb04b-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="cb04b-144">Para uso global, coloque o assembly no repositório de tempo de execução da instalação do .NET Core:</span><span class="sxs-lookup"><span data-stu-id="cb04b-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="cb04b-145">Ao implantar o assembly no repositório de tempo de execução, o arquivo de símbolos pode ser implantado também, mas ele não é necessário para que a extensão funcione.</span><span class="sxs-lookup"><span data-stu-id="cb04b-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the enhancement to work.</span></span>

<span data-ttu-id="cb04b-146">**Colocar o arquivo de dependências**</span><span class="sxs-lookup"><span data-stu-id="cb04b-146">**Place the dependencies file**</span></span>

<span data-ttu-id="cb04b-147">O arquivo *\*.deps.json* da implementação deve estar em um local acessível.</span><span class="sxs-lookup"><span data-stu-id="cb04b-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="cb04b-148">Para uso por usuário, coloque o arquivo na pasta `additonalDeps` das configurações `.dotnet` do perfil do usuário:</span><span class="sxs-lookup"><span data-stu-id="cb04b-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="cb04b-149">Para uso global, coloque o arquivo na pasta `additonalDeps` da instalação do .NET Core:</span><span class="sxs-lookup"><span data-stu-id="cb04b-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="cb04b-150">Observe que a versão, `2.0.0`, reflete a versão do tempo de execução compartilhado usada pelo aplicativo de destino.</span><span class="sxs-lookup"><span data-stu-id="cb04b-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="cb04b-151">O tempo de execução compartilhado é mostrado no arquivo *\*.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="cb04b-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="cb04b-152">No aplicativo de exemplo, o tempo de execução compartilhado é especificado no arquivo *HostingStartupSample.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="cb04b-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="cb04b-153">**Definir variáveis de ambiente**</span><span class="sxs-lookup"><span data-stu-id="cb04b-153">**Set environment variables**</span></span>

<span data-ttu-id="cb04b-154">Defina as variáveis de ambiente a seguir no contexto do aplicativo que usa a melhoria.</span><span class="sxs-lookup"><span data-stu-id="cb04b-154">Set the following environment variables in the context of the app that uses the enhancement.</span></span>

<span data-ttu-id="cb04b-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="cb04b-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="cb04b-156">Apenas assemblies de inicialização de hospedagem são verificados quanto ao `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="cb04b-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="cb04b-157">O nome do assembly da implementação é fornecido nessa variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="cb04b-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="cb04b-158">O aplicativo de exemplo define esse valor como `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="cb04b-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="cb04b-159">O valor também pode ser definido usando a configuração do host [Assemblies de Inicialização de Hospedagem](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="cb04b-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="cb04b-160">DOTNET\_ADDITIONAL\_DEPS</span><span class="sxs-lookup"><span data-stu-id="cb04b-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="cb04b-161">O local do arquivo *\*.deps.json* da implementação.</span><span class="sxs-lookup"><span data-stu-id="cb04b-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="cb04b-162">Se o arquivo for colocado na pasta *.dotnet* do perfil do usuário para uso por usuário:</span><span class="sxs-lookup"><span data-stu-id="cb04b-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="cb04b-163">Se o arquivo for colocado na instalação do .NET Core para uso global, forneça o caminho completo para o arquivo:</span><span class="sxs-lookup"><span data-stu-id="cb04b-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="cb04b-164">O aplicativo de exemplo define esse valor como:</span><span class="sxs-lookup"><span data-stu-id="cb04b-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="cb04b-165">Para obter exemplos de como definir variáveis de ambiente para vários sistemas operacionais, confira [Usar vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="cb04b-165">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="cb04b-166">Aplicativo de exemplo</span><span class="sxs-lookup"><span data-stu-id="cb04b-166">Sample app</span></span>

<span data-ttu-id="cb04b-167">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) usa `IHostingStartup` para criar uma ferramenta de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="cb04b-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="cb04b-168">A ferramenta adiciona dois middlewares ao aplicativo na inicialização que fornecem informações de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="cb04b-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="cb04b-169">Serviços registrados</span><span class="sxs-lookup"><span data-stu-id="cb04b-169">Registered services</span></span>
* <span data-ttu-id="cb04b-170">Endereço: esquema, host, base do caminho, caminho, cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="cb04b-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="cb04b-171">Conexão: IP remoto, porta remota, IP local, porta local, certificado do cliente</span><span class="sxs-lookup"><span data-stu-id="cb04b-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="cb04b-172">Cabeçalhos de solicitação</span><span class="sxs-lookup"><span data-stu-id="cb04b-172">Request headers</span></span>
* <span data-ttu-id="cb04b-173">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="cb04b-173">Environment variables</span></span>

<span data-ttu-id="cb04b-174">Para executar a amostra:</span><span class="sxs-lookup"><span data-stu-id="cb04b-174">To run the sample:</span></span>

1. <span data-ttu-id="cb04b-175">O projeto de Diagnóstico de Inicialização usa o [PowerShell](/powershell/scripting/powershell-scripting) para modificar seu arquivo *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="cb04b-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="cb04b-176">O PowerShell é instalado por padrão em um sistema operacional Windows começando no Windows 7 SP1 e no Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="cb04b-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="cb04b-177">Para obter o PowerShell em outras plataformas, confira [Instalando o Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="cb04b-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="cb04b-178">Compile o projeto de Diagnóstico de Inicialização.</span><span class="sxs-lookup"><span data-stu-id="cb04b-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="cb04b-179">Um destino de build no arquivo de projeto:</span><span class="sxs-lookup"><span data-stu-id="cb04b-179">A build target in the project file:</span></span>
   * <span data-ttu-id="cb04b-180">Move o assembly e os arquivos de símbolos para o repositório de tempo de execução do perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="cb04b-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="cb04b-181">Dispara o script do PowerShell para modificar o arquivo *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="cb04b-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="cb04b-182">Move o arquivo *StartupDiagnostics.deps.json* para a pasta `additionalDeps` do perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="cb04b-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="cb04b-183">Defina as variáveis de ambiente:</span><span class="sxs-lookup"><span data-stu-id="cb04b-183">Set the environment variables:</span></span>
    * <span data-ttu-id="cb04b-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="cb04b-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="cb04b-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="cb04b-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="cb04b-186">Execute o aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="cb04b-186">Run the sample app.</span></span>
5. <span data-ttu-id="cb04b-187">Solicite o ponto de extremidade `/services` para ver os serviços registrados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cb04b-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="cb04b-188">Solicite o ponto de extremidade `/diag` para ver as informações de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="cb04b-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
