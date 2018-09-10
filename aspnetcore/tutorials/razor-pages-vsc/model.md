---
title: Adicionar um modelo a um aplicativo Páginas Razor do ASP.NET Core com o Visual Studio Code
author: rick-anderson
description: Saiba como adicionar um modelo para um aplicativo Páginas Razor no ASP.NET Core usando o Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: b891b921baf1fe6d167c7bfb8b4c5278ce9fe9f5
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055858"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="00be8-103">Adicionar um modelo a um aplicativo Páginas Razor do ASP.NET Core com o Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="00be8-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="00be8-104">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="00be8-104">Add a data model</span></span>

* <span data-ttu-id="00be8-105">Adicione uma pasta denominada *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="00be8-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="00be8-106">Adicionar uma classe denominada *Movie.cs* à pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="00be8-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="00be8-107">Adicione o código a seguir ao arquivo *Models/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="00be8-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a><span data-ttu-id="00be8-108">Pacotes do NuGet do Entity Framework Core para SQLite</span><span class="sxs-lookup"><span data-stu-id="00be8-108">Entity Framework Core NuGet package for SQLite</span></span>

<span data-ttu-id="00be8-109">Na linha de comando, execute o seguinte comando de CLI do .NET Core:</span><span class="sxs-lookup"><span data-stu-id="00be8-109">From the command line, run the following .NET Core CLI command:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

<span data-ttu-id="00be8-110">Adicione os demonstrativos do `using` a seguir à parte superior do *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="00be8-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="00be8-111">Compile o projeto para verificar se não há erros.</span><span class="sxs-lookup"><span data-stu-id="00be8-111">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="00be8-112">Pacotes NuGet do Entity Framework Core para migrações</span><span class="sxs-lookup"><span data-stu-id="00be8-112">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="00be8-113">As ferramentas do EF para a CLI (interface de linha de comando) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="00be8-113">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="00be8-114">Para instalar esse pacote, adicione-o à coleção `DotNetCliToolReference` no arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="00be8-114">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="00be8-115">**Observação:** é necessário instalar este pacote editando o arquivo *.csproj*; não é possível usar o comando `install-package` ou a GUI do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="00be8-115">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="00be8-116">Edite o arquivo *RazorPagesMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="00be8-116">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="00be8-117">Selecione **Arquivo** > **Abrir Arquivo**, e, em seguida, selecione o arquivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="00be8-117">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="00be8-118">Adicione a referência da ferramenta para o `Microsoft.EntityFrameworkCore.Tools.DotNet` ao segundo **\<ItemGroup>**:</span><span class="sxs-lookup"><span data-stu-id="00be8-118">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="00be8-119">Fazer scaffolding do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="00be8-119">Scaffold the Movie model</span></span>

* <span data-ttu-id="00be8-120">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="00be8-120">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="00be8-121">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="00be8-121">Run the following command:</span></span>

<span data-ttu-id="00be8-122">**Observação: execute o comando a seguir no Windows. Para MacOS e Linux, consulte o próximo comando**</span><span class="sxs-lookup"><span data-stu-id="00be8-122">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="00be8-123">No MacOS e Linux, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="00be8-123">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="00be8-124">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="00be8-124">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="00be8-125">Saia do Visual Studio e execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="00be8-125">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="00be8-126">O tutorial a seguir explica os arquivos criados por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="00be8-126">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="00be8-127">[Anterior: Introdução](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Próximo: Páginas Razor geradas por scaffolding](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="00be8-127">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
