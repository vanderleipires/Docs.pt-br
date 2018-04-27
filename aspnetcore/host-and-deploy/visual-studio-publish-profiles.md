---
title: Perfis para a implantação de aplicativo do ASP.NET Core de publicação do Visual Studio
author: rick-anderson
description: Saiba como criar perfis de publicação no Visual Studio e usá-los para gerenciar implantações de aplicativo do ASP.NET Core para vários destinos.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="dd9ef-103">Perfis para a implantação de aplicativo do ASP.NET Core de publicação do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd9ef-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="dd9ef-104">Por [Sayed Hashimi de Ibrahim](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dd9ef-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dd9ef-105">Este documento se concentra no uso de 2017 do Visual Studio para criar e usar perfis de publicação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="dd9ef-106">Os perfis de publicação criados com o Visual Studio podem ser executados do MSBuild e do Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="dd9ef-107">Consulte [Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obter instruções sobre a publicação no Azure.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="dd9ef-108">O seguinte arquivo de projeto foi criado com o comando `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dd9ef-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dd9ef-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dd9ef-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dd9ef-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="dd9ef-111">O `<Project>` do elemento `Sdk` atributo realiza as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="dd9ef-112">Importa o arquivo de propriedades de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* no início.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="dd9ef-113">Importa o arquivo de destino de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* no fim.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="dd9ef-114">O local padrão para `MSBuildSDKsPath` (com o Visual Studio Enterprise 2017) é a pasta *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="dd9ef-115">O `Microsoft.NET.Sdk.Web` depende do SDK:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="dd9ef-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="dd9ef-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="dd9ef-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="dd9ef-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="dd9ef-118">Que faz com que as propriedades e os destinos a serem importados a seguir:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="dd9ef-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="dd9ef-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="dd9ef-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="dd9ef-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="dd9ef-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="dd9ef-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="dd9ef-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="dd9ef-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="dd9ef-123">Publica o direito de conjunto de destinos com base no método de publicação usado de importação de destinos.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="dd9ef-124">Quando o MSBuild ou o Visual Studio carrega um projeto, ocorrem as seguintes ações de alto nível:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="dd9ef-125">Compilar projeto</span><span class="sxs-lookup"><span data-stu-id="dd9ef-125">Build project</span></span>
* <span data-ttu-id="dd9ef-126">Computar arquivos a publicar</span><span class="sxs-lookup"><span data-stu-id="dd9ef-126">Compute files to publish</span></span>
* <span data-ttu-id="dd9ef-127">Publicar arquivos para o destino</span><span class="sxs-lookup"><span data-stu-id="dd9ef-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="dd9ef-128">Itens de projeto de computação</span><span class="sxs-lookup"><span data-stu-id="dd9ef-128">Compute project items</span></span>

<span data-ttu-id="dd9ef-129">Quando o projeto é carregado, os itens de projeto (arquivos) são computados.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="dd9ef-130">O atributo `item type` determina como o arquivo é processado.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="dd9ef-131">Por padrão, os arquivos *.cs* são incluídos na lista de itens `Compile`.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="dd9ef-132">Os arquivos na lista de itens `Compile` são compilados.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="dd9ef-133">O `Content` à lista de itens contém arquivos que são publicados além das saídas de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="dd9ef-134">Por padrão, arquivos correspondentes ao padrão `wwwroot/**` são incluídos no `Content` item.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="dd9ef-135">O `wwwroot/\*\*` [padrão de globalização](https://gruntjs.com/configuring-tasks#globbing-patterns) corresponde a todos os arquivos de *wwwroot* pasta **e** subpastas.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="dd9ef-136">Para adicionar explicitamente um arquivo à lista de publicação, adicione o arquivo diretamente no *. csproj* arquivo conforme mostrado na [arquivos de inclusão](#include-files).</span><span class="sxs-lookup"><span data-stu-id="dd9ef-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="dd9ef-137">Ao selecionar o **publicar** botão no Visual Studio ou a publicação da linha de comando:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="dd9ef-138">Os itens/propriedades são calculados (os arquivos necessários para compilar).</span><span class="sxs-lookup"><span data-stu-id="dd9ef-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="dd9ef-139">**O Visual Studio só**: pacotes do NuGet serão restaurados.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="dd9ef-140">(A restauração precisa ser explícita pelo usuário na CLI.)</span><span class="sxs-lookup"><span data-stu-id="dd9ef-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="dd9ef-141">O projeto é compilado.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-141">The project builds.</span></span>
* <span data-ttu-id="dd9ef-142">Os itens de publicação são computados (os arquivos necessários para a publicação).</span><span class="sxs-lookup"><span data-stu-id="dd9ef-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="dd9ef-143">O projeto é publicado (computados arquivos são copiados para o destino de publicação).</span><span class="sxs-lookup"><span data-stu-id="dd9ef-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="dd9ef-144">Quando faz referência a um projeto do ASP.NET Core `Microsoft.NET.Sdk.Web` no arquivo de projeto, um *app_offline.htm* arquivo é colocado na raiz do diretório de aplicativo da web.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="dd9ef-145">Quando o arquivo estiver presente, o módulo do ASP.NET Core apenas desligará o aplicativo e servirá o arquivo *app_offline.htm* durante a implantação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="dd9ef-146">Para obter mais informações, consulte [Referência de configuração do módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="dd9ef-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="dd9ef-147">Publicação de linha de comando básica</span><span class="sxs-lookup"><span data-stu-id="dd9ef-147">Basic command-line publishing</span></span>

<span data-ttu-id="dd9ef-148">Publicação de linha de comando funciona em todas as plataformas com suporte ao .NET Core e não requer o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="dd9ef-149">No exemplo abaixo, o [dotnet publicar](/dotnet/core/tools/dotnet-publish) comando é executado no diretório do projeto (que contém o *. csproj* arquivo).</span><span class="sxs-lookup"><span data-stu-id="dd9ef-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="dd9ef-150">Se não estiver na pasta do projeto, passar explicitamente no caminho do arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="dd9ef-151">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="dd9ef-152">Execute os comandos a seguir para criar e publicar um aplicativo Web:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dd9ef-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dd9ef-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dd9ef-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dd9ef-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="dd9ef-155">O [dotnet publicar](/dotnet/core/tools/dotnet-publish) comando produz saída semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="dd9ef-156">A pasta de publicação padrão é `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="dd9ef-157">O padrão para `$(Configuration)` é *depurar*.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="dd9ef-158">No exemplo anterior, o `<TargetFramework>` é `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="dd9ef-159">`dotnet publish -h` exibe informações de ajuda para publicação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="dd9ef-160">O comando a seguir especifica um build de `Release` e o diretório de publicação:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="dd9ef-161">O [dotnet publicar](/dotnet/core/tools/dotnet-publish) comando chamadas MSBuild, que chama o `Publish` destino.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="dd9ef-162">Todos os parâmetros passados para `dotnet publish` são passados para o MSBuild.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="dd9ef-163">O parâmetro `-c` é mapeado para a propriedade do MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="dd9ef-164">O parâmetro `-o` é mapeado para `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="dd9ef-165">Propriedades MSBuild podem ser passadas usando qualquer um dos seguintes formatos:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="dd9ef-166">O comando a seguir publica um build de `Release` para um compartilhamento de rede:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="dd9ef-167">O compartilhamento de rede é especificado com barras "/" (*//r8/*) e funciona em todas as plataformas com suporte do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="dd9ef-168">Confirme que o aplicativo publicado para implantação não está em execução.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="dd9ef-169">Os arquivos da pasta *publish* são bloqueados quando o aplicativo está em execução.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="dd9ef-170">A implantação não pode ocorrer porque arquivos bloqueados não podem ser copiados.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="dd9ef-171">Perfis de publicação</span><span class="sxs-lookup"><span data-stu-id="dd9ef-171">Publish profiles</span></span>

<span data-ttu-id="dd9ef-172">Esta seção usa 2017 do Visual Studio para criar um perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="dd9ef-173">Depois de criado, a publicação do Visual Studio ou a linha de comando está disponível.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="dd9ef-174">Publicar perfis podem simplificar o processo de publicação, e podem existir vários perfis.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="dd9ef-175">Crie um perfil de publicação no Visual Studio escolhendo um dos seguintes caminhos:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="dd9ef-176">Clique com botão direito no projeto no Gerenciador de soluções e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="dd9ef-177">Selecione **publicar &lt;project_name&gt;**  do **criar** menu.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="dd9ef-178">O **publicar** guia da página de recursos do aplicativo é exibida.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="dd9ef-179">Se o projeto não tiver um perfil de publicação, a página seguinte é exibida:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![Guia Publicar da página de recursos do aplicativo](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="dd9ef-181">Quando **pasta** é selecionada, especifique um caminho de pasta para armazenar os recursos publicados.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="dd9ef-182">A pasta padrão é *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="dd9ef-183">Clique o **criar perfil** botão para concluir.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="dd9ef-184">Depois de criar um perfil de publicação, o **publicar** guia alterações.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="dd9ef-185">O perfil criado recentemente é exibida em uma lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="dd9ef-186">Clique em **criar novo perfil** para criar outro novo perfil.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-186">Click **Create new profile** to create another new profile.</span></span>

![Guia Publicar da página de recursos de aplicativo mostrando FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="dd9ef-188">O Assistente de publicação dá suporte aos destinos de publicação a seguir:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="dd9ef-189">Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="dd9ef-189">Azure App Service</span></span>
* <span data-ttu-id="dd9ef-190">Máquinas Virtuais do Azure</span><span class="sxs-lookup"><span data-stu-id="dd9ef-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="dd9ef-191">IIS, FTP, etc. (para qualquer servidor web)</span><span class="sxs-lookup"><span data-stu-id="dd9ef-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="dd9ef-192">Pasta</span><span class="sxs-lookup"><span data-stu-id="dd9ef-192">Folder</span></span>
* <span data-ttu-id="dd9ef-193">Importar perfil</span><span class="sxs-lookup"><span data-stu-id="dd9ef-193">Import Profile</span></span>

<span data-ttu-id="dd9ef-194">Para obter mais informações, consulte [quais opções de publicação são certos para mim](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="dd9ef-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="dd9ef-195">Ao criar um perfil de publicação com o Visual Studio, um *propriedades/PublishProfiles/&lt;profile_name&gt;. pubxml* arquivo MSBuild é criado.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="dd9ef-196">O *. pubxml* arquivo é um arquivo MSBuild e contém definições de configuração de publicação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="dd9ef-197">Esse arquivo pode ser alterado para personalizar a compilação e o processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="dd9ef-198">Esse arquivo é lido pelo processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-198">This file is read by the publishing process.</span></span> <span data-ttu-id="dd9ef-199">`<LastUsedBuildConfiguration>` é especial porque é uma propriedade global e não deve estar em qualquer arquivo que será importado no build.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="dd9ef-200">Consulte [MSBuild: como definir a propriedade de configuração](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="dd9ef-201">Ao publicar em um destino do Azure, o *. pubxml* arquivo contém o identificador de assinatura do Azure.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="dd9ef-202">Com esse tipo de destino, adicionar esse arquivo ao controle de origem não é recomendado.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="dd9ef-203">Ao publicar em um destino não Azure, é seguro fazer check-in a *. pubxml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="dd9ef-204">Informações confidenciais (como a senha de publicação) são criptografadas em um nível de usuário/computador.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="dd9ef-205">Ele é armazenado na *propriedades/PublishProfiles/&lt;profile_name&gt;. pubxml.user* arquivo.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="dd9ef-206">Como esse arquivo pode armazenar informações confidenciais, não deve ser verificado no controle de origem.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="dd9ef-207">Para obter uma visão geral de como publicar um aplicativo web do ASP.NET Core, consulte [Host e implantar](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="dd9ef-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="dd9ef-208">As tarefas de MSBuild e os destinos necessários para publicar um aplicativo do ASP.NET Core são o código-fonte aberto no https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="dd9ef-209">`dotnet publish` pode usar a pasta, MSDeploy, e [Kudu](https://github.com/projectkudu/kudu/wiki) perfis de publicação:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="dd9ef-210">Pasta (funciona entre plataformas):</span><span class="sxs-lookup"><span data-stu-id="dd9ef-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="dd9ef-211">MSDeploy (atualmente isso só funciona no Windows como MSDeploy não plataforma cruzada):</span><span class="sxs-lookup"><span data-stu-id="dd9ef-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="dd9ef-212">Pacote MSDeploy (atualmente isso só funciona no Windows como MSDeploy não plataforma cruzada):</span><span class="sxs-lookup"><span data-stu-id="dd9ef-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="dd9ef-213">Nos exemplos anteriores, **não** passar `deployonbuild` para `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="dd9ef-214">Para obter mais informações, consulte [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="dd9ef-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="dd9ef-215">`dotnet publish` oferece suporte a APIs do Kudu para publicar no Azure de qualquer plataforma.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="dd9ef-216">O Visual Studio publique dá suporte a APIs do Kudu, mas ele é suportado por WebSDK para várias plataformas publicar no Azure.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="dd9ef-217">Adicionar um perfil de publicação para o *propriedades/PublishProfiles* pasta com o seguinte conteúdo:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="dd9ef-218">Execute o seguinte comando para compactar o conteúdo de publicar e publicá-lo no Azure usando as APIs de Kudu:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="dd9ef-219">Defina as seguintes propriedades de MSBuild ao usar um perfil de publicação:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="dd9ef-220">Durante a publicação com um perfil chamado *FolderProfile*, qualquer um dos comandos a seguir podem ser executado:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="dd9ef-221">Ao invocar [dotnet compilação](/dotnet/core/tools/dotnet-build), ele chama `msbuild` para executar a compilação e o processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="dd9ef-222">Chamar `dotnet build` ou `msbuild` é equivalente ao passar em um perfil de pasta.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="dd9ef-223">Ao chamar o MSBuild diretamente no Windows, a versão do .NET Framework do MSBuild é usada.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="dd9ef-224">O MSDeploy é atualmente limitado a computadores Windows para a publicação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="dd9ef-225">Chamar `dotnet build` em um perfil não de pasta invoca o MSBuild que, por sua vez, usa MSDeploy em perfis não de pasta.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="dd9ef-226">Chamar `dotnet build` em um perfil não de pasta invoca o MSBuild (usando MSDeploy) e resulta em uma falha (mesmo quando em execução em uma plataforma do Windows).</span><span class="sxs-lookup"><span data-stu-id="dd9ef-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="dd9ef-227">Para publicar com um perfil não de pasta, chame o MSBuild diretamente.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="dd9ef-228">A pasta de perfil de publicação a seguir foi criada com o Visual Studio e publica em um compartilhamento de rede:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="dd9ef-229">Observe que `<LastUsedBuildConfiguration>` é definido como `Release`.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="dd9ef-230">Ao publicar no Visual Studio, o valor da propriedade de configuração `<LastUsedBuildConfiguration>` é definido usando o valor quando o processo de publicação é iniciado.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="dd9ef-231">O `<LastUsedBuildConfiguration>` propriedade de configuração é especial e não deve ser substituída em um arquivo importado do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="dd9ef-232">Essa propriedade pode ser substituída na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="dd9ef-233">Usando o .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="dd9ef-234">Usando o MSBuild:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="dd9ef-235">Publicar em um ponto de extremidade do MSDeploy da linha de comando</span><span class="sxs-lookup"><span data-stu-id="dd9ef-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="dd9ef-236">A publicação pode ser feita usando o .NET Core CLI ou o MSBuild.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="dd9ef-237">`dotnet publish` é executado no contexto do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="dd9ef-238">O `msbuild` comando requer o .NET Framework, que limita a ambientes de Windows.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="dd9ef-239">A maneira mais fácil publicar com MSDeploy é primeiro criar um perfil de publicação no Visual Studio de 2017 e usar o perfil da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="dd9ef-240">No exemplo a seguir, é criado um aplicativo web do ASP.NET Core (usando `dotnet new mvc`), e um perfil de publicação do Azure é adicionado com o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="dd9ef-241">Executar `msbuild` de um **Prompt de comando do desenvolvedor para VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="dd9ef-242">O Prompt de comando do desenvolvedor tem corretas *msbuild.exe* em seu caminho com um conjunto de variáveis do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="dd9ef-243">O MSBuild usa a sintaxe a seguir:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="dd9ef-244">Obter o `Password` do  *\<nome da publicação >. PublishSettings* arquivo.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="dd9ef-245">Baixe o *. PublishSettings* arquivo do:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="dd9ef-246">Gerenciador de soluções: Clique no aplicativo Web e selecione **baixar perfil de publicação**.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="dd9ef-247">Portal do Azure: clique em **obter perfil de publicação** em seu aplicativo Web **visão geral** painel.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="dd9ef-248">`Username` pode ser encontrado no perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="dd9ef-249">O exemplo a seguir usa o *Web11112 - implantação da Web* perfil de publicação:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="dd9ef-250">Excluir arquivos</span><span class="sxs-lookup"><span data-stu-id="dd9ef-250">Exclude files</span></span>

<span data-ttu-id="dd9ef-251">Ao publicar aplicativos Web do ASP.NET Core, os artefatos de build e o conteúdo da pasta *wwwroot* são incluídos.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="dd9ef-252">`msbuild` dá suporte a [padrões de caractere curinga](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="dd9ef-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="dd9ef-253">Por exemplo, a seguinte `<Content>` elemento exclui todo o texto (*. txt*) de arquivos do *wwwroot/content* pasta e todas as suas subpastas.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="dd9ef-254">A marcação anterior pode ser adicionada a um perfil de publicação ou o *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="dd9ef-255">Quando adicionada ao arquivo *.csproj*, a regra será adicionada a todos os perfis de publicação no projeto.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="dd9ef-256">O seguinte `<MsDeploySkipRules>` elemento exclui todos os arquivos da *wwwroot/content* pasta:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="dd9ef-257">`<MsDeploySkipRules>` Não exclua o *ignorar* destinos do site de implantação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="dd9ef-258">`<Content>` pastas e arquivos de destino são excluídas do site de implantação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="dd9ef-259">Por exemplo, suponha que um aplicativo da web implantados tinha os seguintes arquivos:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="dd9ef-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="dd9ef-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="dd9ef-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="dd9ef-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="dd9ef-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="dd9ef-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="dd9ef-263">Se o seguinte `<MsDeploySkipRules>` elementos são adicionados, esses arquivos não serão excluídos no site de implantação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="dd9ef-264">Anterior `<MsDeploySkipRules>` elementos impedir o *ignorada* arquivos que está sendo implantado.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="dd9ef-265">Ele não excluirá os arquivos depois que são implantados.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="dd9ef-266">O seguinte `<Content>` elemento exclui os arquivos de destino no local de implantação:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="dd9ef-267">Com a implantação de linha de comando anterior `<Content>` elemento produz o seguinte resultado:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a><span data-ttu-id="dd9ef-268">Incluir arquivos</span><span class="sxs-lookup"><span data-stu-id="dd9ef-268">Include files</span></span>

<span data-ttu-id="dd9ef-269">A seguinte marcação inclui um *imagens* pasta fora do diretório do projeto para o *wwwroot/imagens* pasta do site de publicação:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="dd9ef-270">A marcação pode ser adicionada ao arquivo *.csproj* ou ao perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="dd9ef-271">Se ela for adicionada para o *. csproj* arquivo, ele está incluído em cada perfil de publicação no projeto.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="dd9ef-272">A marcação realçada a seguir mostra como:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="dd9ef-273">Copiar um arquivo de fora do projeto para a pasta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="dd9ef-274">Excluir a pasta *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="dd9ef-275">Excluir *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-275">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="dd9ef-276">Consulte o [Leiame do WebSDK](https://github.com/aspnet/websdk) para obter mais amostras de implantação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="dd9ef-277">Executar um destino antes ou depois da publicação</span><span class="sxs-lookup"><span data-stu-id="dd9ef-277">Run a target before or after publishing</span></span>

<span data-ttu-id="dd9ef-278">O interno `BeforePublish` e `AfterPublish` destinos executam um destino antes ou após o destino de publicação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="dd9ef-279">Adicione os seguintes elementos para o perfil de publicação para registrar em log mensagens de console antes e após a publicação:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="dd9ef-280">Publicar em um servidor usando um certificado não confiável</span><span class="sxs-lookup"><span data-stu-id="dd9ef-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="dd9ef-281">Adicionar o `<AllowUntrustedCertificate>` propriedade com um valor de `True` para o perfil de publicação:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="dd9ef-282">O serviço Kudu</span><span class="sxs-lookup"><span data-stu-id="dd9ef-282">The Kudu service</span></span>

<span data-ttu-id="dd9ef-283">Para exibir os arquivos em uma implantação de aplicativo web do serviço de aplicativo do Azure, use o [service Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="dd9ef-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="dd9ef-284">Acrescente a `scm` token para o nome do aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="dd9ef-285">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dd9ef-285">For example:</span></span>

| <span data-ttu-id="dd9ef-286">URL</span><span class="sxs-lookup"><span data-stu-id="dd9ef-286">URL</span></span>                                    | <span data-ttu-id="dd9ef-287">Resultado</span><span class="sxs-lookup"><span data-stu-id="dd9ef-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="dd9ef-288">Aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="dd9ef-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="dd9ef-289">Serviço kudu</span><span class="sxs-lookup"><span data-stu-id="dd9ef-289">Kudu service</span></span> |

<span data-ttu-id="dd9ef-290">Selecione o [Console depuração](https://github.com/projectkudu/kudu/wiki/Kudu-console) item de menu para exibir, editar, excluir ou adicionar arquivos.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dd9ef-291">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="dd9ef-291">Additional resources</span></span>

* <span data-ttu-id="dd9ef-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifica a implantação de aplicativos web e sites para servidores IIS.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="dd9ef-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Problemas de arquivos e recursos para implantação de solicitação.</span><span class="sxs-lookup"><span data-stu-id="dd9ef-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
