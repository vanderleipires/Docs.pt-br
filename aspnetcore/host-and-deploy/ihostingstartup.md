---
title: "Adicionar recursos de aplicativo de um assembly externo usando IHostingStartup no núcleo do ASP.NET"
author: guardrex
description: "Saiba como adicionar recursos a um aplicativo ASP.NET Core de um assembly externo usando uma implementação de IHostingStartup."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/ihostingstartup
ms.openlocfilehash: bd2446d6133e0c06dc14509271c2d17be4c95b63
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="add-app-features-from-an-external-assembly-using-ihostingstartup-in-aspnet-core"></a><span data-ttu-id="6542c-103">Adicionar recursos de aplicativo de um assembly externo usando IHostingStartup no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6542c-103">Add app features from an external assembly using IHostingStartup in ASP.NET Core</span></span>

<span data-ttu-id="6542c-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6542c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6542c-105">Um [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementação permite adicionar recursos a um aplicativo durante a inicialização de fora do aplicativo `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="6542c-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding features to an app at startup from outside of the app's `Startup` class.</span></span> <span data-ttu-id="6542c-106">Por exemplo, uma biblioteca de ferramentas externas pode usar um `IHostingStartup` implementação para fornecer serviços a um aplicativo ou provedores de configuração adicionais.</span><span class="sxs-lookup"><span data-stu-id="6542c-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="6542c-107">`IHostingStartup`*está disponível no ASP.NET Core 2.0 e posterior.*</span><span class="sxs-lookup"><span data-stu-id="6542c-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="6542c-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6542c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="6542c-109">Descobrir assemblies carregados de hospedagem inicialização</span><span class="sxs-lookup"><span data-stu-id="6542c-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="6542c-110">Para descobrir a hospedagem assemblies de inicialização carregados pelo aplicativo ou bibliotecas, habilite o log e verifique os logs de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6542c-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="6542c-111">Erros que ocorrem durante o carregamento de assemblies são registrados.</span><span class="sxs-lookup"><span data-stu-id="6542c-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="6542c-112">Assemblies de inicialização de hospedagem carregados são registrados no nível de depuração e todos os erros são registrados.</span><span class="sxs-lookup"><span data-stu-id="6542c-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="6542c-113">As leituras de aplicativo de exemplo da [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) em um `string` de matriz e exibe o resultado na página de índice do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="6542c-113">The sample app reads the the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[Main](ihostingstartup/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="6542c-114">Desabilitar o carregamento automático de hospedagem assemblies de inicialização</span><span class="sxs-lookup"><span data-stu-id="6542c-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="6542c-115">Há duas maneiras para desabilitar o carregamento automático de hospedagem assemblies de inicialização:</span><span class="sxs-lookup"><span data-stu-id="6542c-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="6542c-116">Definir o [impedir a inicialização de hospedagem](xref:fundamentals/hosting#prevent-hosting-startup) configuração de host.</span><span class="sxs-lookup"><span data-stu-id="6542c-116">Set the [Prevent Hosting Startup](xref:fundamentals/hosting#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="6542c-117">Definir o `ASPNETCORE_preventHostingStartup` variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="6542c-117">Set the `ASPNETCORE_preventHostingStartup` environment variable.</span></span>

<span data-ttu-id="6542c-118">Quando a configuração do host ou a variável de ambiente é definida como `true` ou `1`, hospedagem assemblies de inicialização não são carregados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6542c-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="6542c-119">Se ambas estiverem definidas, a configuração do host controla o comportamento.</span><span class="sxs-lookup"><span data-stu-id="6542c-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="6542c-120">Desabilitando hospedagem assemblies de inicialização usando a variável de ambiente ou configuração de host desabilita globalmente e poderá desabilitar os vários recursos de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6542c-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several features of an app.</span></span> <span data-ttu-id="6542c-121">Não é possível desabilitar seletivamente um assembly de inicialização de hospedagem adicionado por uma biblioteca, a menos que a biblioteca oferece sua própria opção de configuração no momento.</span><span class="sxs-lookup"><span data-stu-id="6542c-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="6542c-122">Uma versão futura oferecem a capacidade de desabilitar seletivamente a hospedagem assemblies de inicialização (consulte [GitHub emitir aspnet/hospedagem #1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="6542c-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup-features"></a><span data-ttu-id="6542c-123">Implementar recursos de IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="6542c-123">Implement IHostingStartup features</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="6542c-124">Criar o assembly</span><span class="sxs-lookup"><span data-stu-id="6542c-124">Create the assembly</span></span>

<span data-ttu-id="6542c-125">Um `IHostingStartup` recurso é implantado como um assembly com base em um aplicativo de console sem um ponto de entrada.</span><span class="sxs-lookup"><span data-stu-id="6542c-125">An `IHostingStartup` feature is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="6542c-126">As referências de assembly a [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) pacote:</span><span class="sxs-lookup"><span data-stu-id="6542c-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[Main](ihostingstartup/snapshot_sample/StartupFeature.csproj)]

<span data-ttu-id="6542c-127">Um [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atributo identifica uma classe como uma implementação de `IHostingStartup` para carregamento e execução ao compilar o [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="6542c-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="6542c-128">O exemplo a seguir, o namespace é `StartupFeature`, e a classe é `StartupFeatureHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="6542c-128">In the following example, the namespace is `StartupFeature`, and the class is `StartupFeatureHostingStartup`:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet1)]

<span data-ttu-id="6542c-129">Uma classe implementa `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="6542c-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="6542c-130">A classe [configurar](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) método usa um [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para adicionar recursos a um aplicativo:</span><span class="sxs-lookup"><span data-stu-id="6542c-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add features to an app:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="6542c-131">Ao criar um `IHostingStartup` do projeto, o arquivo de dependências (*\*. deps.json*) define o `runtime` localização do assembly para o *bin* pasta:</span><span class="sxs-lookup"><span data-stu-id="6542c-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="6542c-132">Apenas uma parte do arquivo é mostrada.</span><span class="sxs-lookup"><span data-stu-id="6542c-132">Only part of the file is shown.</span></span> <span data-ttu-id="6542c-133">É o nome do assembly no exemplo `StartupFeature`.</span><span class="sxs-lookup"><span data-stu-id="6542c-133">The assembly name in the example is `StartupFeature`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="6542c-134">Atualizar o arquivo de dependências</span><span class="sxs-lookup"><span data-stu-id="6542c-134">Update the dependencies file</span></span>

<span data-ttu-id="6542c-135">O local de tempo de execução é especificado no  *\*. deps.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="6542c-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="6542c-136">Ativa o recurso de `runtime` elemento deve especificar o local do assembly de tempo de execução do recurso.</span><span class="sxs-lookup"><span data-stu-id="6542c-136">To active the feature, the `runtime` element must specify the location of the feature's runtime assembly.</span></span> <span data-ttu-id="6542c-137">Prefixo de `runtime` local com `lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="6542c-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="6542c-138">No aplicativo de exemplo, a modificação do  *\*. deps.json* arquivo é executado por um [PowerShell](/powershell/scripting/powershell-scripting) script.</span><span class="sxs-lookup"><span data-stu-id="6542c-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="6542c-139">O script do PowerShell é acionado automaticamente por um destino de compilação no arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="6542c-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="feature-activation"></a><span data-ttu-id="6542c-140">Ativação de recurso</span><span class="sxs-lookup"><span data-stu-id="6542c-140">Feature activation</span></span>

<span data-ttu-id="6542c-141">**Coloque o arquivo de assembly**</span><span class="sxs-lookup"><span data-stu-id="6542c-141">**Place the assembly file**</span></span>

<span data-ttu-id="6542c-142">O `IHostingStartup` arquivo de assembly de implementação deve ser *bin*-implantada no aplicativo ou colocadas no [repositório tempo de execução](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="6542c-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="6542c-143">Para uso por usuário, coloque o assembly no repositório de tempo de execução do perfil do usuário em:</span><span class="sxs-lookup"><span data-stu-id="6542c-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="6542c-144">Para uso global, coloque o assembly no repositório de tempo de execução da instalação de núcleo do .NET:</span><span class="sxs-lookup"><span data-stu-id="6542c-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="6542c-145">Ao implantar o assembly no armazenamento de tempo de execução, o arquivo de símbolos pode ser implantado também, mas não é necessário para o recurso funcione.</span><span class="sxs-lookup"><span data-stu-id="6542c-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the feature to work.</span></span>

<span data-ttu-id="6542c-146">**Coloque o arquivo de dependências**</span><span class="sxs-lookup"><span data-stu-id="6542c-146">**Place the dependencies file**</span></span>

<span data-ttu-id="6542c-147">A implementação  *\*. deps.json* arquivo deve estar em um local acessível.</span><span class="sxs-lookup"><span data-stu-id="6542c-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="6542c-148">Para uso por usuário, coloque o arquivo no `additonalDeps` pasta do perfil do usuário `.dotnet` configurações:</span><span class="sxs-lookup"><span data-stu-id="6542c-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="6542c-149">Para uso global, coloque o arquivo no `additonalDeps` pasta de instalação do núcleo do .NET:</span><span class="sxs-lookup"><span data-stu-id="6542c-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="6542c-150">Observe a versão `2.0.0`, reflete a versão de tempo de execução compartilhada que usa o aplicativo de destino.</span><span class="sxs-lookup"><span data-stu-id="6542c-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="6542c-151">O tempo de execução compartilhado é mostrado no  *\*. runtimeconfig.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="6542c-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="6542c-152">O aplicativo de exemplo, o tempo de execução compartilhado é especificado no *HostingStartupSample.runtimeconfig.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="6542c-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="6542c-153">**Conjunto de variáveis de ambiente**</span><span class="sxs-lookup"><span data-stu-id="6542c-153">**Set environment variables**</span></span>

<span data-ttu-id="6542c-154">Defina as seguintes variáveis de ambiente no contexto do aplicativo que usa o recurso.</span><span class="sxs-lookup"><span data-stu-id="6542c-154">Set the following environment variables in the context of the app that uses the feature.</span></span>

<span data-ttu-id="6542c-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="6542c-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="6542c-156">Apenas assemblies de inicialização de hospedagem são verificados para o `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="6542c-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="6542c-157">O nome do assembly da implementação é fornecido nessa variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="6542c-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="6542c-158">O aplicativo de exemplo define esse valor como `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="6542c-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="6542c-159">O valor também pode ser definido usando o [Assemblies de inicialização de hospedagem](xref:fundamentals/hosting#hosting-startup-assemblies) configuração de host.</span><span class="sxs-lookup"><span data-stu-id="6542c-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/hosting#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="6542c-160">DOTNET\_ADICIONAIS\_DEPS</span><span class="sxs-lookup"><span data-stu-id="6542c-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="6542c-161">O local da implementação do  *\*. deps.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="6542c-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="6542c-162">Se o arquivo será colocado no perfil do usuário *.dotnet* pasta para uso por usuário:</span><span class="sxs-lookup"><span data-stu-id="6542c-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="6542c-163">Se o arquivo é colocado na instalação do núcleo do .NET para uso global, forneça o caminho completo para o arquivo:</span><span class="sxs-lookup"><span data-stu-id="6542c-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="6542c-164">O aplicativo de exemplo define esse valor como:</span><span class="sxs-lookup"><span data-stu-id="6542c-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="6542c-165">Para obter exemplos de como definir variáveis de ambiente para vários sistemas operacionais, consulte [trabalhando com vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="6542c-165">For examples of how to set environment variables for various operating systems, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="6542c-166">Aplicativo de exemplo</span><span class="sxs-lookup"><span data-stu-id="6542c-166">Sample app</span></span>

<span data-ttu-id="6542c-167">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) usa `IHostingStartup` para criar uma ferramenta de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="6542c-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="6542c-168">A ferramenta adiciona dois middlewares para o aplicativo na inicialização que fornecem informações de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="6542c-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="6542c-169">Serviços registrados</span><span class="sxs-lookup"><span data-stu-id="6542c-169">Registered services</span></span>
* <span data-ttu-id="6542c-170">Endereço: esquema, host, caminho de base, caminho, cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="6542c-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="6542c-171">Conexão: IP remoto, a porta remota, IP local, porta local, o certificado de cliente</span><span class="sxs-lookup"><span data-stu-id="6542c-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="6542c-172">Cabeçalhos de solicitação</span><span class="sxs-lookup"><span data-stu-id="6542c-172">Request headers</span></span>
* <span data-ttu-id="6542c-173">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="6542c-173">Environment variables</span></span>

<span data-ttu-id="6542c-174">Para executar o exemplo:</span><span class="sxs-lookup"><span data-stu-id="6542c-174">To run the sample:</span></span>

1. <span data-ttu-id="6542c-175">O projeto de inicialização de diagnóstico usa [PowerShell](/powershell/scripting/powershell-scripting) para modificar seu *StartupDiagnostics.deps.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="6542c-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="6542c-176">PowerShell é instalado por padrão em um sistema operacional do Windows a partir do Windows 7 SP1 e Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="6542c-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="6542c-177">Para obter o PowerShell em outras plataformas, consulte [instalando o Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="6542c-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="6542c-178">Compile o projeto de inicialização de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="6542c-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="6542c-179">Um destino de compilação no arquivo de projeto:</span><span class="sxs-lookup"><span data-stu-id="6542c-179">A build target in the project file:</span></span>
   * <span data-ttu-id="6542c-180">Move o assembly e símbolos de arquivos para o repositório de tempo de execução do perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="6542c-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="6542c-181">Aciona o script do PowerShell para modificar o *StartupDiagnostics.deps.json* arquivo.</span><span class="sxs-lookup"><span data-stu-id="6542c-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="6542c-182">Move o *StartupDiagnostics.deps.json* arquivo para o perfil de usuário `additionalDeps` pasta.</span><span class="sxs-lookup"><span data-stu-id="6542c-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="6542c-183">Defina as variáveis de ambiente:</span><span class="sxs-lookup"><span data-stu-id="6542c-183">Set the environment variables:</span></span>
    * <span data-ttu-id="6542c-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="6542c-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="6542c-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="6542c-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="6542c-186">Execute o aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="6542c-186">Run the sample app.</span></span>
5. <span data-ttu-id="6542c-187">Solicitar o `/services` ponto de extremidade para ver o aplicativo registrado serviços.</span><span class="sxs-lookup"><span data-stu-id="6542c-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="6542c-188">Solicitar o `/diag` ponto de extremidade para ver as informações de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="6542c-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
