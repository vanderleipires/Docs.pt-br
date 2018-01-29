---
title: "Perfis para a implantação de aplicativo do ASP.NET Core de publicação do Visual Studio"
author: rick-anderson
description: "Descubra como criar perfis de aplicativos do ASP.NET Core de publicação no Visual Studio."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 1f403447c39db4ebfe3dafda591602f0dc9db8c3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="155ca-103">Perfis para a implantação de aplicativo do ASP.NET Core de publicação do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="155ca-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="155ca-104">Por [Sayed Hashimi de Ibrahim](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="155ca-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="155ca-105">Este artigo se concentra no uso do Visual Studio 2017 para criar perfis de publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-105">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="155ca-106">Os perfis de publicação criados com o Visual Studio podem ser executados do MSBuild e do Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="155ca-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="155ca-107">O artigo fornece detalhes do processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-107">The article provides details of the publishing process.</span></span> <span data-ttu-id="155ca-108">Consulte [Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obter instruções sobre a publicação no Azure.</span><span class="sxs-lookup"><span data-stu-id="155ca-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="155ca-109">O arquivo *.csproj* a seguir foi criado com o comando `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="155ca-109">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="155ca-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="155ca-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="155ca-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="155ca-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="155ca-112">O atributo `Sdk` no elemento `<Project>` (na primeira linha) da marcação acima faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="155ca-112">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="155ca-113">Importa o arquivo de propriedades de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* no início.</span><span class="sxs-lookup"><span data-stu-id="155ca-113">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="155ca-114">Importa o arquivo de destino de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* no fim.</span><span class="sxs-lookup"><span data-stu-id="155ca-114">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="155ca-115">O local padrão para `MSBuildSDKsPath` (com o Visual Studio Enterprise 2017) é a pasta *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="155ca-115">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="155ca-116">`Microsoft.NET.Sdk.Web` depende de:</span><span class="sxs-lookup"><span data-stu-id="155ca-116">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="155ca-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="155ca-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="155ca-118">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="155ca-118">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="155ca-119">Que faz com que as propriedades e os destinos a serem importados a seguir:</span><span class="sxs-lookup"><span data-stu-id="155ca-119">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="155ca-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="155ca-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="155ca-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="155ca-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="155ca-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="155ca-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="155ca-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="155ca-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="155ca-124">Publica o direito de conjunto de destinos com base no método de publicação usado de importação de destinos.</span><span class="sxs-lookup"><span data-stu-id="155ca-124">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="155ca-125">Quando o MSBuild ou o Visual Studio carrega um projeto, as seguintes ações de nível alto são executadas:</span><span class="sxs-lookup"><span data-stu-id="155ca-125">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="155ca-126">Compilar projeto</span><span class="sxs-lookup"><span data-stu-id="155ca-126">Build project</span></span>
* <span data-ttu-id="155ca-127">Computar arquivos a publicar</span><span class="sxs-lookup"><span data-stu-id="155ca-127">Compute files to publish</span></span>
* <span data-ttu-id="155ca-128">Publicar arquivos para o destino</span><span class="sxs-lookup"><span data-stu-id="155ca-128">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="155ca-129">Itens de projeto de computação</span><span class="sxs-lookup"><span data-stu-id="155ca-129">Compute project items</span></span>

<span data-ttu-id="155ca-130">Quando o projeto é carregado, os itens de projeto (arquivos) são computados.</span><span class="sxs-lookup"><span data-stu-id="155ca-130">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="155ca-131">O atributo `item type` determina como o arquivo é processado.</span><span class="sxs-lookup"><span data-stu-id="155ca-131">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="155ca-132">Por padrão, os arquivos *.cs* são incluídos na lista de itens `Compile`.</span><span class="sxs-lookup"><span data-stu-id="155ca-132">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="155ca-133">Os arquivos na lista de itens `Compile` são compilados.</span><span class="sxs-lookup"><span data-stu-id="155ca-133">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="155ca-134">O `Content` à lista de itens contém arquivos que são publicados além das saídas de compilação.</span><span class="sxs-lookup"><span data-stu-id="155ca-134">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="155ca-135">Por padrão, arquivos correspondentes ao padrão `wwwroot/**` são incluídos no `Content` item.</span><span class="sxs-lookup"><span data-stu-id="155ca-135">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="155ca-136">[wwwroot /\* \* é um padrão de globalização](https://gruntjs.com/configuring-tasks#globbing-patterns) que especifica todos os arquivos de *wwwroot* pasta **e** subpastas.</span><span class="sxs-lookup"><span data-stu-id="155ca-136">[wwwroot/\*\* is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="155ca-137">Para adicionar explicitamente um arquivo à lista de publicação, adicione o arquivo diretamente no arquivo *.csproj* conforme mostrado em [Incluindo arquivos](#including-files).</span><span class="sxs-lookup"><span data-stu-id="155ca-137">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="155ca-138">Ao selecionar o **publicar** botão no Visual Studio ou a publicação da linha de comando:</span><span class="sxs-lookup"><span data-stu-id="155ca-138">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="155ca-139">Os itens/propriedades são calculados (os arquivos necessários para compilar).</span><span class="sxs-lookup"><span data-stu-id="155ca-139">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="155ca-140">Somente Visual Studio: pacotes NuGet são restaurados.</span><span class="sxs-lookup"><span data-stu-id="155ca-140">Visual Studio only: NuGet packages are restored.</span></span> <span data-ttu-id="155ca-141">(A restauração precisa ser explícita pelo usuário na CLI.)</span><span class="sxs-lookup"><span data-stu-id="155ca-141">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="155ca-142">O projeto é compilado.</span><span class="sxs-lookup"><span data-stu-id="155ca-142">The project builds.</span></span>
* <span data-ttu-id="155ca-143">Os itens de publicação são computados (os arquivos necessários para a publicação).</span><span class="sxs-lookup"><span data-stu-id="155ca-143">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="155ca-144">O projeto é publicado.</span><span class="sxs-lookup"><span data-stu-id="155ca-144">The project is published.</span></span> <span data-ttu-id="155ca-145">(Os arquivos computados são copiados para o destino de publicação.)</span><span class="sxs-lookup"><span data-stu-id="155ca-145">(The computed files are copied to the publish destination.)</span></span>

<span data-ttu-id="155ca-146">Quando faz referência a um projeto do ASP.NET Core `Microsoft.NET.Sdk.Web` no arquivo de projeto, um *app_offline.htm* arquivo é colocado na raiz do diretório de aplicativo da web.</span><span class="sxs-lookup"><span data-stu-id="155ca-146">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="155ca-147">Quando o arquivo estiver presente, o módulo do ASP.NET Core apenas desligará o aplicativo e servirá o arquivo *app_offline.htm* durante a implantação.</span><span class="sxs-lookup"><span data-stu-id="155ca-147">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="155ca-148">Para obter mais informações, consulte [Referência de configuração do módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="155ca-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="155ca-149">Publicação de linha de comando básica</span><span class="sxs-lookup"><span data-stu-id="155ca-149">Basic command-line publishing</span></span>

<span data-ttu-id="155ca-150">Publicação de linha de comando funciona em todas as plataformas com suporte do .NET Core e não requer o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="155ca-150">Command-line publishing works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="155ca-151">Nas amostras abaixo, o comando `dotnet publish` é executado no diretório do projeto (que contém o arquivo *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="155ca-151">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="155ca-152">Se não estiver na pasta do projeto, passar explicitamente no caminho do arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="155ca-152">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="155ca-153">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="155ca-153">For example:</span></span>

```console
dotnet publish c:/webs/web1
```

<span data-ttu-id="155ca-154">Execute os comandos a seguir para criar e publicar um aplicativo Web:</span><span class="sxs-lookup"><span data-stu-id="155ca-154">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="155ca-155">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="155ca-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="155ca-156">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="155ca-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="155ca-157">O `dotnet publish` produz uma saída semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="155ca-157">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="155ca-158">A pasta de publicação padrão é `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="155ca-158">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="155ca-159">O padrão de `$(Configuration)` é Depurar.</span><span class="sxs-lookup"><span data-stu-id="155ca-159">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="155ca-160">Na amostra acima, o `<TargetFramework>` é `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="155ca-160">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="155ca-161">`dotnet publish -h` exibe informações de ajuda para publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-161">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="155ca-162">O comando a seguir especifica um build de `Release` e o diretório de publicação:</span><span class="sxs-lookup"><span data-stu-id="155ca-162">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="155ca-163">O `dotnet publish` comando chama o MSBuild que invoca o `Publish` destino.</span><span class="sxs-lookup"><span data-stu-id="155ca-163">The `dotnet publish` command calls MSBuild which invokes the `Publish` target.</span></span> <span data-ttu-id="155ca-164">Todos os parâmetros passados para `dotnet publish` são passados para o MSBuild.</span><span class="sxs-lookup"><span data-stu-id="155ca-164">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="155ca-165">O parâmetro `-c` é mapeado para a propriedade do MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="155ca-165">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="155ca-166">O parâmetro `-o` é mapeado para `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="155ca-166">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="155ca-167">Propriedades MSBuild podem ser passadas usando qualquer um dos seguintes formatos:</span><span class="sxs-lookup"><span data-stu-id="155ca-167">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="155ca-168">O comando a seguir publica um build de `Release` para um compartilhamento de rede:</span><span class="sxs-lookup"><span data-stu-id="155ca-168">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="155ca-169">O compartilhamento de rede é especificado com barras "/" (*//r8/*) e funciona em todas as plataformas com suporte do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="155ca-169">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="155ca-170">Confirme que o aplicativo publicado para implantação não está em execução.</span><span class="sxs-lookup"><span data-stu-id="155ca-170">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="155ca-171">Os arquivos da pasta *publish* são bloqueados quando o aplicativo está em execução.</span><span class="sxs-lookup"><span data-stu-id="155ca-171">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="155ca-172">A implantação não pode ocorrer porque arquivos bloqueados não podem ser copiados.</span><span class="sxs-lookup"><span data-stu-id="155ca-172">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="155ca-173">Perfis de publicação</span><span class="sxs-lookup"><span data-stu-id="155ca-173">Publish profiles</span></span>

<span data-ttu-id="155ca-174">Esta seção usa o Visual Studio 2017 e superior para criar perfis de publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-174">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="155ca-175">Depois de criado, a publicação do Visual Studio ou a linha de comando está disponível.</span><span class="sxs-lookup"><span data-stu-id="155ca-175">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="155ca-176">Perfis de publicação podem simplificar o processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-176">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="155ca-177">Vários perfis de publicação pode existir.</span><span class="sxs-lookup"><span data-stu-id="155ca-177">Multiple publish profiles can exist.</span></span> <span data-ttu-id="155ca-178">Para criar um perfil de publicação no Visual Studio, clique com botão direito no projeto no Gerenciador de soluções e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="155ca-178">To create a publish profile in Visual Studio, right-click on the project in Solution Explorer and select **Publish**.</span></span> <span data-ttu-id="155ca-179">Como alternativa, selecione **publicar \<nome do projeto >** no menu build.</span><span class="sxs-lookup"><span data-stu-id="155ca-179">Alternatively, select **Publish \<project name>** from the build menu.</span></span> <span data-ttu-id="155ca-180">A guia **Publicar** da página de capacidades do aplicativo é exibida.</span><span class="sxs-lookup"><span data-stu-id="155ca-180">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="155ca-181">Se o projeto não contiver um perfil de publicação, a página a seguir será exibida:</span><span class="sxs-lookup"><span data-stu-id="155ca-181">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Guia Publicar da página de recursos do aplicativo mostrando o Azure, IIS, FTB, pasta com o Azure selecionada.](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="155ca-184">Quando a opção **Pasta** é selecionada, o botão **Publicar** cria um perfil de publicação de pasta e publica.</span><span class="sxs-lookup"><span data-stu-id="155ca-184">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![A guia **Publicar** da página de capacidades de aplicativo mostrando Azure, IIS, FTB e Pasta](visual-studio-publish-profiles/_static/pub1.png)

<span data-ttu-id="155ca-186">Depois de criar um perfil de publicação, o **publicar** alterações e selecione **criar novo perfil** para criar um novo perfil.</span><span class="sxs-lookup"><span data-stu-id="155ca-186">Once a publish profile is created, the **Publish** tab changes, and select **Create new profile** to create a new profile.</span></span>

![A guia **Publicar** da página de recursos de aplicativo mostrando FolderProfile -created na última etapa o link Criar novo perfil](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="155ca-188">O Assistente de publicação dá suporte aos destinos de publicação a seguir:</span><span class="sxs-lookup"><span data-stu-id="155ca-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="155ca-189">Serviço de Aplicativo do Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="155ca-189">Microsoft Azure App Service</span></span>
* <span data-ttu-id="155ca-190">IIS, FTP, etc. (para qualquer servidor Web)</span><span class="sxs-lookup"><span data-stu-id="155ca-190">IIS, FTP, etc (for any web server)</span></span>
* <span data-ttu-id="155ca-191">Pasta</span><span class="sxs-lookup"><span data-stu-id="155ca-191">Folder</span></span>
* <span data-ttu-id="155ca-192">Importe perfil (permite a importação de perfil).</span><span class="sxs-lookup"><span data-stu-id="155ca-192">Import profile (allows profile import).</span></span>
* <span data-ttu-id="155ca-193">Máquinas Virtuais do Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="155ca-193">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="155ca-194">Para obter mais informações, consulte [Quais opções de publicação são adequadas para mim?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="155ca-194">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="155ca-195">Ao criar um perfil de publicação com o Visual Studio, um *propriedades/PublishProfiles/\<nome da publicação >. pubxml* arquivo MSBuild é criado.</span><span class="sxs-lookup"><span data-stu-id="155ca-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="155ca-196">Esse arquivo *.pubxml* é um arquivo do MSBuild e contém definições de configuração de publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-196">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="155ca-197">Esse arquivo pode ser alterado para personalizar a compilação e o processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="155ca-198">Esse arquivo é lido pelo processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-198">This file is read by the publishing process.</span></span> <span data-ttu-id="155ca-199">`<LastUsedBuildConfiguration>`é especial porque é uma propriedade global e não deve estar em qualquer arquivo que será importado no build.</span><span class="sxs-lookup"><span data-stu-id="155ca-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="155ca-200">Para obter mais informações, consulte [MSBuild: como definir a propriedade de configuração](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).</span><span class="sxs-lookup"><span data-stu-id="155ca-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="155ca-201">O *. pubxml* arquivo não deve ser verificado no controle de origem porque ele depende do *. User* arquivo.</span><span class="sxs-lookup"><span data-stu-id="155ca-201">The *.pubxml* file shouldn't be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="155ca-202">O check-in do arquivo *.user* nunca deve ser feito no controle do código-fonte porque ele pode conter informações confidenciais e só é válido para um usuário e computador.</span><span class="sxs-lookup"><span data-stu-id="155ca-202">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="155ca-203">Informações confidenciais (como a senha de publicação) são criptografadas em um nível por usuário/computador e armazenadas no arquivo *Properties/PublishProfiles/\<nome da publicação>.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="155ca-203">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="155ca-204">Como esse arquivo pode conter informações confidenciais, o check-in dele **não** deve ser realizado no controle do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="155ca-204">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="155ca-205">Para obter uma visão geral de como publicar um aplicativo web do ASP.NET Core consulte [Host e implantar](index.md).</span><span class="sxs-lookup"><span data-stu-id="155ca-205">For an overview of how to publish a web app on ASP.NET Core see [Host and deploy](index.md).</span></span> <span data-ttu-id="155ca-206">[Hospedar e implantar](index.md) é um projeto de código-fonte aberto no https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="155ca-206">[Host and deploy](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="155ca-207">`dotnet publish`pode usar a pasta, MSDeploy, e [KUDU](https://github.com/projectkudu/kudu/wiki) perfis de publicação:</span><span class="sxs-lookup"><span data-stu-id="155ca-207">`dotnet publish` can use folder, MSDeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="155ca-208">Pasta (funciona entre plataformas):`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="155ca-208">Folder (works cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="155ca-209">MSDeploy (atualmente isso só funciona no windows como MSDeploy não plataforma cruzada):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="155ca-209">MSDeploy (currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="155ca-210">Pacote MSDeploy (atualmente isso só funciona no windows como MSDeploy não plataforma cruzada):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="155ca-210">MSDeploy package(currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="155ca-211">Nos exemplos anteriores, **não** passar `deployonbuild` para `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="155ca-211">In the preceeding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="155ca-212">Para obter mais informações, consulte [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="155ca-212">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="155ca-213">O `dotnet publish` oferece suporte a APIs do KUDU para publicar no Azure de qualquer plataforma.</span><span class="sxs-lookup"><span data-stu-id="155ca-213">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="155ca-214">O Visual Studio publique suporte a APIs KUDU mas há suporte para websdk cruzada em várias plataformas de publicação no Azure.</span><span class="sxs-lookup"><span data-stu-id="155ca-214">Visual Studio publish does support the KUDU APIs but it's supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="155ca-215">Adicionar um perfil de publicação à pasta *Propriedades/PerfisDePublicação* com o seguinte conteúdo:</span><span class="sxs-lookup"><span data-stu-id="155ca-215">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="155ca-216">Executando o seguinte comando Fechar backup do conteúdo de publicar e publicá-lo no Azure usando as APIs de KUDU:</span><span class="sxs-lookup"><span data-stu-id="155ca-216">Running the following command zips up the publish contents and publish it to Azure using the KUDU APIs:</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="155ca-217">Defina as seguintes propriedades de MSBuild ao usar um perfil de publicação:</span><span class="sxs-lookup"><span data-stu-id="155ca-217">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="155ca-218">Durante a publicação com um perfil chamado *FolderProfile*, qualquer um dos comandos a seguir podem ser executado:</span><span class="sxs-lookup"><span data-stu-id="155ca-218">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="155ca-219">Ao invocar `dotnet build`, ele chama `msbuild` para executar a compilação e o processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-219">When invoking `dotnet build`, it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="155ca-220">Chamando `dotnet build` ou `msbuild` é essencialmente equivalente ao passar em um perfil de pasta.</span><span class="sxs-lookup"><span data-stu-id="155ca-220">Calling `dotnet build` or `msbuild` is essentially equivalent when passing in a folder profile.</span></span> <span data-ttu-id="155ca-221">Ao chamar o MSBuild diretamente no Windows, a versão do .NET Framework do MSBuild é usada.</span><span class="sxs-lookup"><span data-stu-id="155ca-221">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="155ca-222">O MSDeploy é atualmente limitado a computadores Windows para a publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-222">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="155ca-223">Chamar `dotnet build` em um perfil não de pasta invoca o MSBuild que, por sua vez, usa MSDeploy em perfis não de pasta.</span><span class="sxs-lookup"><span data-stu-id="155ca-223">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="155ca-224">Chamar `dotnet build` em um perfil não de pasta invoca o MSBuild (usando MSDeploy) e resulta em uma falha (mesmo quando em execução em uma plataforma do Windows).</span><span class="sxs-lookup"><span data-stu-id="155ca-224">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="155ca-225">Para publicar com um perfil não de pasta, chame o MSBuild diretamente.</span><span class="sxs-lookup"><span data-stu-id="155ca-225">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="155ca-226">A pasta de perfil de publicação a seguir foi criada com o Visual Studio e publica em um compartilhamento de rede:</span><span class="sxs-lookup"><span data-stu-id="155ca-226">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="155ca-227">Observe que `<LastUsedBuildConfiguration>` é definido como `Release`.</span><span class="sxs-lookup"><span data-stu-id="155ca-227">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="155ca-228">Ao publicar no Visual Studio, o valor da propriedade de configuração `<LastUsedBuildConfiguration>` é definido usando o valor quando o processo de publicação é iniciado.</span><span class="sxs-lookup"><span data-stu-id="155ca-228">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="155ca-229">O `<LastUsedBuildConfiguration>` propriedade de configuração é especial e não deve ser substituída em um arquivo importado do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="155ca-229">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="155ca-230">Essa propriedade pode ser substituída na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="155ca-230">This property can be overridden from the command line.</span></span> <span data-ttu-id="155ca-231">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="155ca-231">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="155ca-232">Usando o MSBuild:</span><span class="sxs-lookup"><span data-stu-id="155ca-232">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="155ca-233">Publicar em um ponto de extremidade do MSDeploy da linha de comando</span><span class="sxs-lookup"><span data-stu-id="155ca-233">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="155ca-234">Como mencionado anteriormente, a publicação pode ser realizada usando `dotnet publish` ou `msbuild` comando.</span><span class="sxs-lookup"><span data-stu-id="155ca-234">As previously mentioned, publishing can be accomplished using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="155ca-235">`dotnet publish` é executado no contexto do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="155ca-235">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="155ca-236">O `msbuild` comando requer o .NET framework e, portanto, é limitado a ambientes do Windows.</span><span class="sxs-lookup"><span data-stu-id="155ca-236">The `msbuild` command requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="155ca-237">A maneira mais fácil publicar com MSDeploy é primeiro criar um perfil de publicação no Visual Studio de 2017 e usar o perfil da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="155ca-237">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="155ca-238">No exemplo a seguir, é criado um aplicativo web do ASP.NET Core (usando `dotnet new mvc`), e um perfil de publicação do Azure é adicionado com o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="155ca-238">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="155ca-239">Executar `msbuild` de um **Prompt de comando do desenvolvedor para VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="155ca-239">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="155ca-240">O Prompt de comando do desenvolvedor tem corretas *msbuild.exe* em seu caminho com um conjunto de variáveis do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="155ca-240">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="155ca-241">O MSBuild usa a sintaxe a seguir:</span><span class="sxs-lookup"><span data-stu-id="155ca-241">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="155ca-242">Obter o `Password` do  *\<nome da publicação >. PublishSettings* arquivo.</span><span class="sxs-lookup"><span data-stu-id="155ca-242">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="155ca-243">Baixe o *. PublishSettings* arquivo do:</span><span class="sxs-lookup"><span data-stu-id="155ca-243">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="155ca-244">Gerenciador de soluções: Clique no aplicativo Web e selecione **baixar perfil de publicação**.</span><span class="sxs-lookup"><span data-stu-id="155ca-244">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="155ca-245">O Portal de gerenciamento do Azure: Selecione **obter perfil de publicação** na folha de aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="155ca-245">The Azure Management Portal: Select **Get publish profile** from the Web App blade.</span></span>

<span data-ttu-id="155ca-246">`Username` pode ser encontrado no perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-246">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="155ca-247">A amostra a seguir usa o perfil de publicação "Web11112 – implantação da Web":</span><span class="sxs-lookup"><span data-stu-id="155ca-247">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="155ca-248">Excluindo arquivos</span><span class="sxs-lookup"><span data-stu-id="155ca-248">Excluding files</span></span>

<span data-ttu-id="155ca-249">Ao publicar aplicativos Web do ASP.NET Core, os artefatos de build e o conteúdo da pasta *wwwroot* são incluídos.</span><span class="sxs-lookup"><span data-stu-id="155ca-249">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="155ca-250">`msbuild` dá suporte a [padrões de caractere curinga](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="155ca-250">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="155ca-251">Por exemplo, a seguinte `<Content>` marcação de elemento exclui todo o texto (*. txt*) de arquivos do *wwwroot/content* pasta e todas as suas subpastas.</span><span class="sxs-lookup"><span data-stu-id="155ca-251">For example, the following `<Content>` element markup excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="155ca-252">A marcação acima pode ser adicionada a um perfil de publicação ou ao arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="155ca-252">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="155ca-253">Quando adicionada ao arquivo *.csproj*, a regra será adicionada a todos os perfis de publicação no projeto.</span><span class="sxs-lookup"><span data-stu-id="155ca-253">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="155ca-254">A marcação do elemento `<MsDeploySkipRules>` a seguir exclui todos os arquivos da pasta *wwwroot/content*:</span><span class="sxs-lookup"><span data-stu-id="155ca-254">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="155ca-255">`<MsDeploySkipRules>`Não exclua o *ignorar* destinos do site de implantação.</span><span class="sxs-lookup"><span data-stu-id="155ca-255">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="155ca-256">`<Content>`pastas e arquivos de destino são excluídas do site de implantação.</span><span class="sxs-lookup"><span data-stu-id="155ca-256">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="155ca-257">Por exemplo, suponha que um aplicativo da web implantados tinha os seguintes arquivos:</span><span class="sxs-lookup"><span data-stu-id="155ca-257">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="155ca-258">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="155ca-258">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="155ca-259">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="155ca-259">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="155ca-260">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="155ca-260">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="155ca-261">Se o seguinte `<MsDeploySkipRules>` marcação é adicionada, esses arquivos não serão excluídos no site de implantação.</span><span class="sxs-lookup"><span data-stu-id="155ca-261">If the following `<MsDeploySkipRules>` markup is added, those files wouldn't be deleted on the deployment site.</span></span>

``` xml
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

<span data-ttu-id="155ca-262">O `<MsDeploySkipRules>` marcação mostrada acima impede que o *ignorada* arquivos sejam depoyed, mas não excluir esses arquivos depois que são implantados.</span><span class="sxs-lookup"><span data-stu-id="155ca-262">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed but won't delete those files once they're deployed.</span></span>

<span data-ttu-id="155ca-263">O seguinte `<Content>` marcação exclui os arquivos de destino no local de implantação:</span><span class="sxs-lookup"><span data-stu-id="155ca-263">The following `<Content>` markup deletes the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="155ca-264">Usando a implantação de linha de comando com o `<Content>` marcação acima resulta em uma saída semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="155ca-264">Using command-line deployment with the `<Content>` markup above results in output similar to the following:</span></span>

``` console
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

## <a name="including-files"></a><span data-ttu-id="155ca-265">Incluindo arquivos</span><span class="sxs-lookup"><span data-stu-id="155ca-265">Including files</span></span>

<span data-ttu-id="155ca-266">A seguinte marcação inclui um *imagens* pasta fora do diretório do projeto para o *wwwroot/imagens* pasta do site de publicação:</span><span class="sxs-lookup"><span data-stu-id="155ca-266">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="155ca-267">A marcação pode ser adicionada ao arquivo *.csproj* ou ao perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-267">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="155ca-268">Se ela for adicionada para o *. csproj* arquivo, ele está incluído em cada perfil de publicação no projeto.</span><span class="sxs-lookup"><span data-stu-id="155ca-268">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="155ca-269">A marcação realçada a seguir mostra como:</span><span class="sxs-lookup"><span data-stu-id="155ca-269">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="155ca-270">Copiar um arquivo de fora do projeto para a pasta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="155ca-270">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="155ca-271">Excluir a pasta *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="155ca-271">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="155ca-272">Excluir *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="155ca-272">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="155ca-273">Consulte o [Leiame do WebSDK](https://github.com/aspnet/websdk) para obter mais amostras de implantação.</span><span class="sxs-lookup"><span data-stu-id="155ca-273">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="155ca-274">Executar um destino antes ou depois da publicação</span><span class="sxs-lookup"><span data-stu-id="155ca-274">Run a target before or after publishing</span></span>

<span data-ttu-id="155ca-275">O interno `BeforePublish` e `AfterPublish` destinos podem ser usados para executar um destino antes ou após o destino de publicação.</span><span class="sxs-lookup"><span data-stu-id="155ca-275">The built-in `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="155ca-276">A marcação a seguir pode ser adicionada ao perfil de publicação para registrar mensagens de saída do console antes e após a publicação:</span><span class="sxs-lookup"><span data-stu-id="155ca-276">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="155ca-277">O serviço Kudu</span><span class="sxs-lookup"><span data-stu-id="155ca-277">The Kudu service</span></span>

<span data-ttu-id="155ca-278">Para exibir os arquivos de uma implantação de aplicativo de web do serviço de aplicativos do Azure, use o [serviço Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="155ca-278">To view the files in the an Azure Apps Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="155ca-279">Acrescente a `scm` token para o nome do aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="155ca-279">Append the `scm` token to the name of the web app.</span></span> <span data-ttu-id="155ca-280">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="155ca-280">For example:</span></span>

| <span data-ttu-id="155ca-281">URL</span><span class="sxs-lookup"><span data-stu-id="155ca-281">URL</span></span>                                    | <span data-ttu-id="155ca-282">Resultado</span><span class="sxs-lookup"><span data-stu-id="155ca-282">Result</span></span>      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="155ca-283">Aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="155ca-283">Web App</span></span>     |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="155ca-284">Serviço kudu</span><span class="sxs-lookup"><span data-stu-id="155ca-284">Kudu sevice</span></span> |

<span data-ttu-id="155ca-285">Selecione o item de menu [Console de Depuração](https://github.com/projectkudu/kudu/wiki/Kudu-console) para exibir/editar/excluir/adicionar arquivos.</span><span class="sxs-lookup"><span data-stu-id="155ca-285">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="155ca-286">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="155ca-286">Additional resources</span></span>

* <span data-ttu-id="155ca-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifica a implantação de aplicativos web e sites para servidores IIS.</span><span class="sxs-lookup"><span data-stu-id="155ca-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="155ca-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemas de arquivos e recursos de solicitação para implantação.</span><span class="sxs-lookup"><span data-stu-id="155ca-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
