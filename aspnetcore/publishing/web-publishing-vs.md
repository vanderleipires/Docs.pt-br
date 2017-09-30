---
title: "Criar perfis de publicação para o Visual Studio e o MSBuild"
author: rick-anderson
description: "Explica a publicação na Web no Visual Studio."
keywords: "ASP.NET Core, publicação na Web, publicação, msbuild, implantação da Web, dotnet publicar, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: 665c98b5ac16bb9739af4ac204fca59a55dbb812
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a><span data-ttu-id="4b871-104">Criar perfis de publicação para o Visual Studio e o MSBuild para implantar aplicativos ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4b871-104">Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps</span></span>

<span data-ttu-id="4b871-105">Por [Sayed Hashimi de Ibrahim](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4b871-105">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4b871-106">Este artigo se concentra no uso do Visual Studio 2017 para criar perfis de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-106">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="4b871-107">Os perfis de publicação criados com o Visual Studio podem ser executados do MSBuild e do Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="4b871-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span>

<span data-ttu-id="4b871-108">O arquivo *.csproj* a seguir foi criado com o comando `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="4b871-108">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4b871-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4b871-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4b871-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4b871-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="4b871-111">O atributo `Sdk` no elemento `<Project>` (na primeira linha) da marcação acima faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="4b871-111">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="4b871-112">Importa o arquivo `props` *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* no início.</span><span class="sxs-lookup"><span data-stu-id="4b871-112">Imports the `props` file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="4b871-113">Importa o arquivo de destino de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* no fim.</span><span class="sxs-lookup"><span data-stu-id="4b871-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="4b871-114">O local padrão para `MSBuildSDKsPath` (com o Visual Studio Enterprise 2017) é a pasta *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="4b871-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="4b871-115">`Microsoft.NET.Sdk.Web` depende de:</span><span class="sxs-lookup"><span data-stu-id="4b871-115">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="4b871-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="4b871-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="4b871-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="4b871-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="4b871-118">Que faz com que os seguintes `props` e `targets` sejam importados:</span><span class="sxs-lookup"><span data-stu-id="4b871-118">Which causes the following `props` and `targets` to be imported:</span></span>

* <span data-ttu-id="4b871-119">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="4b871-119">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="4b871-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="4b871-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="4b871-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="4b871-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="4b871-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="4b871-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="4b871-123">Destinos de publicação importarão novamente o conjunto certo de destinos com base no método de publicação usado.</span><span class="sxs-lookup"><span data-stu-id="4b871-123">Publish targets will again import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="4b871-124">Quando o MSBuild ou o Visual Studio carrega um projeto, as seguintes ações de nível alto são executadas:</span><span class="sxs-lookup"><span data-stu-id="4b871-124">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="4b871-125">Compilar projeto</span><span class="sxs-lookup"><span data-stu-id="4b871-125">Build project</span></span>
* <span data-ttu-id="4b871-126">Computar arquivos a publicar</span><span class="sxs-lookup"><span data-stu-id="4b871-126">Compute files to publish</span></span>
* <span data-ttu-id="4b871-127">Publicar arquivos para o destino</span><span class="sxs-lookup"><span data-stu-id="4b871-127">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="4b871-128">Itens de projeto de computação</span><span class="sxs-lookup"><span data-stu-id="4b871-128">Compute project items</span></span>

<span data-ttu-id="4b871-129">Quando o projeto é carregado, os itens de projeto (arquivos) são computados.</span><span class="sxs-lookup"><span data-stu-id="4b871-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="4b871-130">O atributo `item type` determina como o arquivo é processado.</span><span class="sxs-lookup"><span data-stu-id="4b871-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="4b871-131">Por padrão, os arquivos *.cs* são incluídos na lista de itens `Compile`.</span><span class="sxs-lookup"><span data-stu-id="4b871-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="4b871-132">Os arquivos na lista de itens `Compile` são compilados.</span><span class="sxs-lookup"><span data-stu-id="4b871-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="4b871-133">A lista de itens `Content` contém arquivos que serão publicados juntamente com as saídas de build.</span><span class="sxs-lookup"><span data-stu-id="4b871-133">The `Content` item list contains files that will be published in addition to the build outputs.</span></span> <span data-ttu-id="4b871-134">Por padrão, os arquivos correspondendo ao padrão wwwroot/** serão incluídos no item `Content`.</span><span class="sxs-lookup"><span data-stu-id="4b871-134">By default, files matching the pattern wwwroot/** will be included in the `Content` item.</span></span> <span data-ttu-id="4b871-135">[wwwroot/** é um padrão de recurso de curinga](https://gruntjs.com/configuring-tasks#globbing-patterns) que especifica todos os arquivos na pasta *wwwroot* **e** subpastas.</span><span class="sxs-lookup"><span data-stu-id="4b871-135">[wwwroot/** is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="4b871-136">Se você precisar adicionar explicitamente um arquivo à lista de publicação, você poderá adicionar o arquivo diretamente no arquivo *.csproj* conforme mostrado em [Incluindo Arquivos](#including-files).</span><span class="sxs-lookup"><span data-stu-id="4b871-136">If you need to explicitly add a file to the publish list you can add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="4b871-137">Quando você seleciona o botão **Publicar** no Visual Studio ou quando você publica da linha de comando:</span><span class="sxs-lookup"><span data-stu-id="4b871-137">When you select the **Publish** button in Visual Studio or when you publish from command line:</span></span>

- <span data-ttu-id="4b871-138">Os itens/propriedades são calculados (os arquivos necessários para compilar).</span><span class="sxs-lookup"><span data-stu-id="4b871-138">The properties/items are computed (the files that are needed to build).</span></span>
- <span data-ttu-id="4b871-139">Somente Visual Studio: pacotes NuGet são restaurados.</span><span class="sxs-lookup"><span data-stu-id="4b871-139">Visual Studio only: NuGet packages are restored.</span></span>  <span data-ttu-id="4b871-140">(A restauração precisa ser explícita pelo usuário na CLI.)</span><span class="sxs-lookup"><span data-stu-id="4b871-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
- <span data-ttu-id="4b871-141">O projeto é compilado.</span><span class="sxs-lookup"><span data-stu-id="4b871-141">The project builds.</span></span>
- <span data-ttu-id="4b871-142">Os itens de publicação são computados (os arquivos necessários para a publicação).</span><span class="sxs-lookup"><span data-stu-id="4b871-142">The publish items are computed (the files that are needed to publish).</span></span>
- <span data-ttu-id="4b871-143">O projeto é publicado.</span><span class="sxs-lookup"><span data-stu-id="4b871-143">The project is published.</span></span> <span data-ttu-id="4b871-144">(Os arquivos computados são copiados para o destino de publicação.)</span><span class="sxs-lookup"><span data-stu-id="4b871-144">(The computed files are copied to the publish destination.)</span></span>

## <a name="simple-command-line-publishing"></a><span data-ttu-id="4b871-145">Publicação de linha de comando simples</span><span class="sxs-lookup"><span data-stu-id="4b871-145">Simple command line publishing</span></span>

<span data-ttu-id="4b871-146">Esta seção funciona em todas as plataformas com suporte do .NET Core e não requer o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b871-146">This section works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="4b871-147">Nas amostras abaixo, o comando `dotnet publish` é executado no diretório do projeto (que contém o arquivo *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="4b871-147">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="4b871-148">Se você não estiver na pasta do projeto, você poderá passar explicitamente no caminho do arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="4b871-148">If you're not in the project folder, you can explicitly pass in the project file path.</span></span> <span data-ttu-id="4b871-149">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4b871-149">For example:</span></span>

```console
dotnet publish  c:/webs/web1
```

<span data-ttu-id="4b871-150">Execute os comandos a seguir para criar e publicar um aplicativo Web:</span><span class="sxs-lookup"><span data-stu-id="4b871-150">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet restore
dotnet publish
```

<span data-ttu-id="4b871-151">O `dotnet publish` produz uma saída semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="4b871-151">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.1.548.43366
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp1.1\Web1.dll
```

<span data-ttu-id="4b871-152">A pasta de publicação padrão é `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="4b871-152">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="4b871-153">O padrão de `$(Configuration)` é Depurar.</span><span class="sxs-lookup"><span data-stu-id="4b871-153">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="4b871-154">Na amostra acima, o `<TargetFramework>` é `netcoreapp1.1`.</span><span class="sxs-lookup"><span data-stu-id="4b871-154">In the sample above, the `<TargetFramework>` is `netcoreapp1.1`.</span></span> <span data-ttu-id="4b871-155">O caminho real na amostra acima é *bin\Debug\netcoreapp1.1\publish*.</span><span class="sxs-lookup"><span data-stu-id="4b871-155">The actual path in the sample above is *bin\Debug\netcoreapp1.1\publish*.</span></span>

<span data-ttu-id="4b871-156">`dotnet publish -h` exibe informações de ajuda para publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-156">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="4b871-157">O comando a seguir especifica um build de `Release` e o diretório de publicação:</span><span class="sxs-lookup"><span data-stu-id="4b871-157">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="4b871-158">O comando `dotnet publish` chama `MSBuild`, que invoca o destino `Publish`.</span><span class="sxs-lookup"><span data-stu-id="4b871-158">The `dotnet publish` command calls `MSBuild` which invokes the `Publish` target.</span></span> <span data-ttu-id="4b871-159">Quaisquer parâmetros passados para `dotnet publish` são passados para `MSBuild`.</span><span class="sxs-lookup"><span data-stu-id="4b871-159">Any parameters passed to `dotnet publish` are passed to `MSBuild`.</span></span> <span data-ttu-id="4b871-160">O parâmetro `-c` é mapeado para a propriedade do MSBuild `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="4b871-160">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="4b871-161">O parâmetro `-o` é mapeado para `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="4b871-161">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="4b871-162">Você pode passar propriedades do MSBuild usando qualquer um dos seguintes formatos:</span><span class="sxs-lookup"><span data-stu-id="4b871-162">You can pass MSBuild properties using either of the following formats:</span></span>

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

<span data-ttu-id="4b871-163">O comando a seguir publica um build de `Release` para um compartilhamento de rede:</span><span class="sxs-lookup"><span data-stu-id="4b871-163">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="4b871-164">O compartilhamento de rede é especificado com barras "/" (*//r8/*) e funciona em todas as plataformas com suporte do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4b871-164">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="4b871-165">Confirme que o aplicativo publicado para implantação não está em execução.</span><span class="sxs-lookup"><span data-stu-id="4b871-165">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="4b871-166">Os arquivos da pasta *publish* são bloqueados quando o aplicativo está em execução.</span><span class="sxs-lookup"><span data-stu-id="4b871-166">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="4b871-167">A implantação não pode ocorrer porque arquivos bloqueados não podem ser copiados.</span><span class="sxs-lookup"><span data-stu-id="4b871-167">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="4b871-168">Perfis de publicação</span><span class="sxs-lookup"><span data-stu-id="4b871-168">Publish profiles</span></span>

<span data-ttu-id="4b871-169">Esta seção usa o Visual Studio 2017 e superior para criar perfis de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-169">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="4b871-170">Uma vez criados, você pode publicar do Visual Studio ou da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="4b871-170">Once created, you can publish from Visual Studio or the command line.</span></span>

<span data-ttu-id="4b871-171">Perfis de publicação podem simplificar o processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-171">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="4b871-172">Você pode ter vários perfis de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-172">You can have multiple publish profiles.</span></span> <span data-ttu-id="4b871-173">Para criar um perfil de publicação no Visual Studio, clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="4b871-173">To create a publish profile in Visual Studio, right click on the project in Solution Explore and select **Publish**.</span></span> <span data-ttu-id="4b871-174">Como alternativa, você pode selecionar **Publicar     \<nome do projeto>** no menu do build.</span><span class="sxs-lookup"><span data-stu-id="4b871-174">Alternatively, you can select **Publish     \<project name>** from the build menu.</span></span> <span data-ttu-id="4b871-175">A guia **Publicar** da página de capacidades do aplicativo é exibida.</span><span class="sxs-lookup"><span data-stu-id="4b871-175">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="4b871-176">Se o projeto não contiver um perfil de publicação, a página a seguir será exibida:</span><span class="sxs-lookup"><span data-stu-id="4b871-176">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![A guia **Publicar** da página de capacidades de aplicativo mostrando Azure, IIS, FTB e Pasta, com a opção Azure selecionada.](web-publishing-vs/_static/az.png)

<span data-ttu-id="4b871-179">Quando a opção **Pasta** é selecionada, o botão **Publicar** cria um perfil de publicação de pasta e publica.</span><span class="sxs-lookup"><span data-stu-id="4b871-179">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![A guia **Publicar** da página de capacidades de aplicativo mostrando Azure, IIS, FTB e Pasta](web-publishing-vs/_static/pub1.png)

<span data-ttu-id="4b871-181">Depois que um perfil de publicação é criado, a guia **Publicar** é alterada e você seleciona **Criar novo perfil** para criar um novo perfil.</span><span class="sxs-lookup"><span data-stu-id="4b871-181">Once a publish profile is created, the **Publish** tab changes, and you select **Create new profile** to create a new profile.</span></span>

![A guia **Publicar** da página de recursos de aplicativo mostrando FolderProfile -created na última etapa o link Criar novo perfil](web-publishing-vs/_static/create_new.png)

<span data-ttu-id="4b871-183">O Assistente de publicação dá suporte aos destinos de publicação a seguir:</span><span class="sxs-lookup"><span data-stu-id="4b871-183">The Publish wizard supports the following publish targets:</span></span>

- <span data-ttu-id="4b871-184">Serviço de Aplicativo do Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="4b871-184">Microsoft Azure App Service</span></span>
- <span data-ttu-id="4b871-185">IIS, FTP, etc. (para qualquer servidor Web)</span><span class="sxs-lookup"><span data-stu-id="4b871-185">IIS, FTP, etc (for any web server)</span></span>
- <span data-ttu-id="4b871-186">Pasta</span><span class="sxs-lookup"><span data-stu-id="4b871-186">Folder</span></span>
- <span data-ttu-id="4b871-187">Importar perfil (permite que você importe um perfil).</span><span class="sxs-lookup"><span data-stu-id="4b871-187">Import profile (allows you to import a profile).</span></span>
- <span data-ttu-id="4b871-188">Máquinas Virtuais do Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="4b871-188">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="4b871-189">Para obter mais informações, consulte [Quais opções de publicação são adequadas para mim?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="4b871-189">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="4b871-190">Quando você cria um perfil de publicação com o Visual Studio, um arquivo do MSBuild *Properties/PublishProfiles/\<nome da publicação>.pubxml* é criado.</span><span class="sxs-lookup"><span data-stu-id="4b871-190">When you create a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="4b871-191">Esse arquivo *.pubxml* é um arquivo do MSBuild e contém definições de configuração de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-191">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="4b871-192">Você pode alterar esse arquivo para personalizar o build e o processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-192">You can change this file to customize the build and publish process.</span></span> <span data-ttu-id="4b871-193">Esse arquivo é lido pelo processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-193">This file is read by the publishing process.</span></span> <span data-ttu-id="4b871-194">`<LastUsedBuildConfiguration>` é especial porque é uma propriedade global e não deverá estar em nenhum arquivo que será importado no build.</span><span class="sxs-lookup"><span data-stu-id="4b871-194">`<LastUsedBuildConfiguration>` is special because it’s a global property and shouldn’t be in any file that’s imported in the build.</span></span> <span data-ttu-id="4b871-195">Para obter mais informações, consulte [MSBuild: como definir a propriedade de configuração](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).</span><span class="sxs-lookup"><span data-stu-id="4b871-195">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="4b871-196">O check-in do arquivo *.pubxml* não deve ser feito no controle do código-fonte porque ele depende do arquivo *.user*.</span><span class="sxs-lookup"><span data-stu-id="4b871-196">The *.pubxml* file should not be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="4b871-197">O check-in do arquivo *.user* nunca deve ser feito no controle do código-fonte porque ele pode conter informações confidenciais e só é válido para um usuário e computador.</span><span class="sxs-lookup"><span data-stu-id="4b871-197">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="4b871-198">Informações confidenciais (como a senha de publicação) são criptografadas em um nível por usuário/computador e armazenadas no arquivo *Properties/PublishProfiles/\<nome da publicação>.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="4b871-198">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="4b871-199">Como esse arquivo pode conter informações confidenciais, o check-in dele **não** deve ser realizado no controle do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="4b871-199">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="4b871-200">Para obter uma visão geral de como publicar um aplicativo Web do ASP.NET Core, consulte [Publicação e implantação](index.md).</span><span class="sxs-lookup"><span data-stu-id="4b871-200">For an overview of how to publish a web app on ASP.NET Core see [Publishing and Deployment](index.md).</span></span> <span data-ttu-id="4b871-201">[Publicação e implantação](index.md) é um projeto de software livre em https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="4b871-201">[Publishing and Deployment](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="4b871-202">No momento, `dotnet publish` não tem a capacidade de usar perfis de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-202">Currently `dotnet publish` doesn’t have the ability to use publish profiles.</span></span> <span data-ttu-id="4b871-203">Para usar perfis de publicação, use `dotnet build`.</span><span class="sxs-lookup"><span data-stu-id="4b871-203">To use publish profiles, use `dotnet build`.</span></span> <span data-ttu-id="4b871-204">`dotnet build` invoca o MSBuild no projeto.</span><span class="sxs-lookup"><span data-stu-id="4b871-204">`dotnet build` invokes MSBuild on the project.</span></span> <span data-ttu-id="4b871-205">Alternativamente, chame `msbuild` diretamente.</span><span class="sxs-lookup"><span data-stu-id="4b871-205">Alternatively, call `msbuild` directly.</span></span>

<span data-ttu-id="4b871-206">Defina as seguintes propriedades de MSBuild ao usar um perfil de publicação:</span><span class="sxs-lookup"><span data-stu-id="4b871-206">Set the following MSBuild properties when using a publish profile:</span></span>

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

<span data-ttu-id="4b871-207">Por exemplo, ao publicar com um perfil chamado *FolderProfile*, você pode executar qualquer um dos comandos abaixo.</span><span class="sxs-lookup"><span data-stu-id="4b871-207">For example, when publishing with a profile named *FolderProfile* you can execute either of the commands below.</span></span>

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="4b871-208">Quando você invoca `dotnet build`, ele chama `msbuild` para executar o build e o processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-208">When you invoke `dotnet build` it will call `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="4b871-209">Quando você passa um perfil de pasta, chamar `dotnet build` ou `msbuild` é essencialmente equivalente.</span><span class="sxs-lookup"><span data-stu-id="4b871-209">Calling `dotnet build` or `msbuild` is essentially equivalent when you pass in a folder profile.</span></span> <span data-ttu-id="4b871-210">Ao chamar o MSBuild diretamente no Windows, você obterá a versão do .NET Framework do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="4b871-210">When calling MSBuild directly on Windows you get the .NET Framework version of MSBuild.</span></span>  <span data-ttu-id="4b871-211">O MSDeploy é atualmente limitado a computadores Windows para a publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-211">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="4b871-212">Chamar `dotnet build` em um perfil não de pasta invoca o MSBuild que, por sua vez, usa MSDeploy em perfis não de pasta.</span><span class="sxs-lookup"><span data-stu-id="4b871-212">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="4b871-213">Chamar `dotnet build` em um perfil não de pasta invoca o MSBuild (usando MSDeploy) e resulta em uma falha (mesmo quando em execução em uma plataforma do Windows).</span><span class="sxs-lookup"><span data-stu-id="4b871-213">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="4b871-214">Para publicar com um perfil não de pasta, chame o MSBuild diretamente.</span><span class="sxs-lookup"><span data-stu-id="4b871-214">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="4b871-215">A pasta de perfil de publicação a seguir foi criada com o Visual Studio e publica em um compartilhamento de rede:</span><span class="sxs-lookup"><span data-stu-id="4b871-215">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

<span data-ttu-id="4b871-216">Observe que `<LastUsedBuildConfiguration>` é definido como `Release`.</span><span class="sxs-lookup"><span data-stu-id="4b871-216">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span>  <span data-ttu-id="4b871-217">Ao publicar no Visual Studio, o valor da propriedade de configuração `<LastUsedBuildConfiguration>` é definido usando o valor quando o processo de publicação é iniciado.</span><span class="sxs-lookup"><span data-stu-id="4b871-217">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="4b871-218">A propriedade de configuração `<LastUsedBuildConfiguration>` é especial e não deve ser substituída em um arquivo do MSBuild importado.</span><span class="sxs-lookup"><span data-stu-id="4b871-218">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn’t be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="4b871-219">Você pode substituir essa propriedade da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="4b871-219">You can override this property from the command line.</span></span> <span data-ttu-id="4b871-220">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4b871-220">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="4b871-221">Usando o MSBuild:</span><span class="sxs-lookup"><span data-stu-id="4b871-221">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="4b871-222">Publicar em um ponto de extremidade do MSDeploy da linha de comando</span><span class="sxs-lookup"><span data-stu-id="4b871-222">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="4b871-223">Como mencionado anteriormente, você pode publicar usando o comando `dotnet publish` ou `msbuild`.</span><span class="sxs-lookup"><span data-stu-id="4b871-223">As previously mentioned, you can publish using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="4b871-224">`dotnet publish` é executado no contexto do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4b871-224">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="4b871-225">`msbuild` requer o .NET framework e, portanto, é limitado a ambientes Windows.</span><span class="sxs-lookup"><span data-stu-id="4b871-225">`msbuild` requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="4b871-226">A maneira mais fácil publicar com MSDeploy é primeiro criar um perfil de publicação no Visual Studio de 2017 e usar o perfil da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="4b871-226">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="4b871-227">Na amostra a seguir, criei um aplicativo Web do ASP.NET Core (usando `dotnet new mvc`) e adicionei um perfil de publicação do Azure com o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b871-227">In the following sample, I created an ASP.NET Core web app ( using `dotnet new mvc`) and added an Azure publish profile with Visual Studio.</span></span>

<span data-ttu-id="4b871-228">Você executa `msbuild` de um **Prompt de Comando do Desenvolvedor para VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="4b871-228">You run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="4b871-229">O Prompt de comando do desenvolvedor terá o *msbuild.exe* correto em seu caminho e definirá algumas variáveis do MSBuild.</span><span class="sxs-lookup"><span data-stu-id="4b871-229">The Developer Command Prompt will have the correct *msbuild.exe* in its path and set some MSBuild variables.</span></span>

<span data-ttu-id="4b871-230">O MSBuild usa a sintaxe a seguir:</span><span class="sxs-lookup"><span data-stu-id="4b871-230">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="4b871-231">Você pode obter o `Password` do arquivo *\<Nome da publicação>.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="4b871-231">You can get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="4b871-232">Você pode baixar o arquivo *.PublishSettings*:</span><span class="sxs-lookup"><span data-stu-id="4b871-232">You can download the *.PublishSettings* file from:</span></span>

- <span data-ttu-id="4b871-233">Gerenciador de Soluções: clique com o botão direito do mouse no aplicativo Web e selecione **Baixar Perfil de Publicação**.</span><span class="sxs-lookup"><span data-stu-id="4b871-233">Solution Explorer: Right click on the Web App and select **Download Publish Profile**.</span></span>
- <span data-ttu-id="4b871-234">O Portal de Gerenciamento do Azure: selecione **Obter perfil de publicação** na folha Aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="4b871-234">The Azure Management Portal: Select **Get publish profile** from the  Web App blade.</span></span>

<span data-ttu-id="4b871-235">`Username` pode ser encontrado no perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-235">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="4b871-236">A amostra a seguir usa o perfil de publicação "Web11112 – implantação da Web":</span><span class="sxs-lookup"><span data-stu-id="4b871-236">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="4b871-237">Excluindo arquivos</span><span class="sxs-lookup"><span data-stu-id="4b871-237">Excluding files</span></span>

<span data-ttu-id="4b871-238">Ao publicar aplicativos Web do ASP.NET Core, os artefatos de build e o conteúdo da pasta *wwwroot* são incluídos.</span><span class="sxs-lookup"><span data-stu-id="4b871-238">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="4b871-239">`msbuild` dá suporte a [padrões de caractere curinga](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="4b871-239">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="4b871-240">Por exemplo, a marcação de elemento `<Content>` a seguir excluirá todos os arquivos de texto (*.txt*) da pasta *wwwroot/content* e de todas as suas subpastas.</span><span class="sxs-lookup"><span data-stu-id="4b871-240">For example, the following `<Content>` element markup will exclude all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="4b871-241">A marcação acima pode ser adicionada a um perfil de publicação ou ao arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4b871-241">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="4b871-242">Quando adicionada ao arquivo *.csproj*, a regra será adicionada a todos os perfis de publicação no projeto.</span><span class="sxs-lookup"><span data-stu-id="4b871-242">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="4b871-243">A marcação do elemento `<MsDeploySkipRules>` a seguir exclui todos os arquivos da pasta *wwwroot/content*:</span><span class="sxs-lookup"><span data-stu-id="4b871-243">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="4b871-244">`<MsDeploySkipRules>` não excluirá os destinos de *omissão* do site de implantação.</span><span class="sxs-lookup"><span data-stu-id="4b871-244">`<MsDeploySkipRules>`  will not delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="4b871-245">Pastas e arquivos de destino de `<Content>` serão excluídos do site de implantação.</span><span class="sxs-lookup"><span data-stu-id="4b871-245">`<Content>` targeted files and folders will be deleted from the deployment site.</span></span> <span data-ttu-id="4b871-246">Por exemplo, suponha que você implantou um aplicativo Web com os seguintes arquivos:</span><span class="sxs-lookup"><span data-stu-id="4b871-246">For example, suppose you had deployed a web app with the following files:</span></span>

- <span data-ttu-id="4b871-247">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4b871-247">*Views/Home/About1.cshtml*</span></span>
- <span data-ttu-id="4b871-248">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4b871-248">*Views/Home/About2.cshtml*</span></span>
- <span data-ttu-id="4b871-249">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4b871-249">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="4b871-250">Se você tivesse adicionado a marcação `<MsDeploySkipRules>` a seguir, esses arquivos não seriam excluídos no site de implantação.</span><span class="sxs-lookup"><span data-stu-id="4b871-250">If you added the following `<MsDeploySkipRules>` markup, those files would not be deleted on the deployment site.</span></span>

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

<span data-ttu-id="4b871-251">A marcação `<MsDeploySkipRules>` mostrada acima impede que os arquivos *ignorados* sejam implantados, mas não exclui os arquivos depois de eles serem implantados.</span><span class="sxs-lookup"><span data-stu-id="4b871-251">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed, but will not delete those files once they are deployed.</span></span>

<span data-ttu-id="4b871-252">A marcação `<Content>` a seguir excluiria os arquivos de destino no local de implantação:</span><span class="sxs-lookup"><span data-stu-id="4b871-252">The following `<Content>` markup would delete the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="4b871-253">Usar a implantação de linha de comando com a marcação `<Content>` acima resultaria em uma saída semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="4b871-253">Using command line deployment with the `<Content>` markup above would result in output similar to the following:</span></span>

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

## <a name="including-files"></a><span data-ttu-id="4b871-254">Incluindo arquivos</span><span class="sxs-lookup"><span data-stu-id="4b871-254">Including files</span></span>

<span data-ttu-id="4b871-255">A marcação a seguir pode ser usada para incluir uma pasta *images* fora do diretório do projeto para a pasta *wwwroot/images* do site de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-255">The following markup can be used to include an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="4b871-256">A marcação pode ser adicionada ao arquivo *.csproj* ou ao perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-256">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="4b871-257">Se ela for adicionada ao arquivo *.csproj*, ela será incluída em cada perfil de publicação no projeto.</span><span class="sxs-lookup"><span data-stu-id="4b871-257">If it's added to the *.csproj* file, it will be included in each publish profile in the project.</span></span>

<span data-ttu-id="4b871-258">A marcação realçada a seguir mostra como:</span><span class="sxs-lookup"><span data-stu-id="4b871-258">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="4b871-259">Copiar um arquivo de fora do projeto para a pasta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="4b871-259">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="4b871-260">Excluir a pasta *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="4b871-260">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="4b871-261">Excluir *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4b871-261">Exclude *Views\Home\About2.cshtml*.</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

<span data-ttu-id="4b871-262">Consulte o [Leiame do WebSDK](https://github.com/aspnet/websdk) para obter mais amostras de implantação.</span><span class="sxs-lookup"><span data-stu-id="4b871-262">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="4b871-263">Executar um destino antes ou depois da publicação</span><span class="sxs-lookup"><span data-stu-id="4b871-263">Run a target before or after publishing</span></span>

<span data-ttu-id="4b871-264">Os destinos `BeforePublish` e `AfterPublish` internos podem ser usados para executar um destino antes ou após o destino de publicação.</span><span class="sxs-lookup"><span data-stu-id="4b871-264">The builtin `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="4b871-265">A marcação a seguir pode ser adicionada ao perfil de publicação para registrar mensagens de saída do console antes e após a publicação:</span><span class="sxs-lookup"><span data-stu-id="4b871-265">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="4b871-266">O serviço Kudu</span><span class="sxs-lookup"><span data-stu-id="4b871-266">The Kudu service</span></span>

<span data-ttu-id="4b871-267">Para exibir os arquivos em seu aplicativo Web do Azure, use o [serviço kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="4b871-267">To view the files on your Azure Web App, use the [kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="4b871-268">Acrescente o token `scm` ao nome ou ao seu aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="4b871-268">Append the `scm` token to the name or your Web App.</span></span> <span data-ttu-id="4b871-269">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4b871-269">For example:</span></span>

| <span data-ttu-id="4b871-270">URL</span><span class="sxs-lookup"><span data-stu-id="4b871-270">URL</span></span>               | <span data-ttu-id="4b871-271">Resultado</span><span class="sxs-lookup"><span data-stu-id="4b871-271">Result</span></span>|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | <span data-ttu-id="4b871-272">Aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="4b871-272">Web App</span></span> |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="4b871-273">Serviço kudu</span><span class="sxs-lookup"><span data-stu-id="4b871-273">Kudu sevice</span></span> |

<span data-ttu-id="4b871-274">Selecione o item de menu [Console de Depuração](https://github.com/projectkudu/kudu/wiki/Kudu-console) para exibir/editar/excluir/adicionar arquivos.</span><span class="sxs-lookup"><span data-stu-id="4b871-274">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4b871-275">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="4b871-275">Additional resources</span></span>

- <span data-ttu-id="4b871-276">A [Implantação da Web](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) simplifica a implantação de aplicativos Web e sites da Web em servidores IIS.</span><span class="sxs-lookup"><span data-stu-id="4b871-276">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy)  (msdeploy) simplifies deployment of Web applications and Web sites to IIS servers.</span></span>

- <span data-ttu-id="4b871-277">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemas de arquivos e recursos de solicitação para implantação.</span><span class="sxs-lookup"><span data-stu-id="4b871-277">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
