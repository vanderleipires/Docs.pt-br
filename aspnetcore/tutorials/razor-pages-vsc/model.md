---
title: "Adicionando um modelo para um aplicativo de Páginas do Razor com o Visual Studio para Mac"
author: rick-anderson
description: "Adicionando um modelo para um aplicativo de Páginas do Razor no ASP.NET Core usando o Visual Studio para Mac"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 9600392b47fb8b1dded06faefaff1bf87d67af4e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="25376-103">Adicionando um modelo para um aplicativo de Páginas do Razor no ASP.NET Core com o Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="25376-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio Code</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="25376-104">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="25376-104">Add a data model</span></span>

* <span data-ttu-id="25376-105">Adicione uma pasta denominada *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="25376-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="25376-106">Adicionar uma classe denominada *Movie.cs* à pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="25376-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="25376-107">Adicione o código a seguir ao arquivo *Models/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="25376-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="25376-108">Crie o projeto para verificar se você não tem nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="25376-108">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="25376-109">Pacotes NuGet do Entity Framework Core para migrações</span><span class="sxs-lookup"><span data-stu-id="25376-109">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="25376-110">As ferramentas do EF para a CLI (interface de linha de comando) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="25376-110">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="25376-111">Para instalar esse pacote, adicione-o à coleção `DotNetCliToolReference` no arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="25376-111">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="25376-112">**Observação:** é necessário instalar este pacote editando o arquivo *.csproj*; não é possível usar o comando `install-package` ou a GUI do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="25376-112">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="25376-113">Edite o arquivo *RazorPagesMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="25376-113">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="25376-114">Selecione **Arquivo** > **Abrir Arquivo**, e, em seguida, selecione o arquivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="25376-114">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="25376-115">Adicione a referência da ferramenta para o `Microsoft.EntityFrameworkCore.Tools.DotNet` ao segundo **\<ItemGroup>**:</span><span class="sxs-lookup"><span data-stu-id="25376-115">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE[model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="25376-116">Fazer scaffolding do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="25376-116">Scaffold the Movie model</span></span>

* <span data-ttu-id="25376-117">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="25376-117">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="25376-118">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="25376-118">Run the following command:</span></span>

<span data-ttu-id="25376-119">**Observação: execute o comando a seguir no Windows. Para MacOS e Linux, consulte o próximo comando**</span><span class="sxs-lookup"><span data-stu-id="25376-119">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="25376-120">No MacOS e Linux, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="25376-120">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="25376-121">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="25376-121">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="25376-122">Saia do Visual Studio e execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="25376-122">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE[model 4](../../includes/RP/model4.md)]<span data-ttu-id="25376-123"> O tutorial a seguir explica os arquivos criados por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="25376-123"> The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="25376-124">[Anterior: Introdução](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Próximo: Páginas do Razor geradas por scaffolding](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="25376-124">[Previous: Getting Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
