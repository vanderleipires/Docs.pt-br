---
title: "Adicionando um modelo para um aplicativo de Páginas do Razor com o Visual Studio para Mac"
author: rick-anderson
description: "Adicionando um modelo para um aplicativo de Páginas do Razor no ASP.NET Core usando o Visual Studio para Mac"
keywords: "ASP.NET Core, Páginas do Razor, Razor, MVC, modelo"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 39b069f8c81ca9460abc33b32b0bcc27118939cb
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="b1c19-104">Adicionando um modelo para um aplicativo de Páginas do Razor no ASP.NET Core com o Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="b1c19-104">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="b1c19-105">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="b1c19-105">Add a data model</span></span>

* <span data-ttu-id="b1c19-106">Adicione uma pasta denominada *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="b1c19-106">Add a folder named *Models*.</span></span>
* <span data-ttu-id="b1c19-107">Adicionar uma classe denominada *Movie.cs* à pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="b1c19-107">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="b1c19-108">Adicione o código a seguir ao arquivo *Models/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="b1c19-108">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

<span data-ttu-id="b1c19-109">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="b1c19-109">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span></span>

<span data-ttu-id="b1c19-110">Crie o projeto para verificar se você não tem nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="b1c19-110">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="b1c19-111">Pacotes NuGet do Entity Framework Core para migrações</span><span class="sxs-lookup"><span data-stu-id="b1c19-111">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="b1c19-112">As ferramentas do EF para a CLI (interface de linha de comando) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="b1c19-112">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="b1c19-113">Para instalar esse pacote, adicione-o à coleção `DotNetCliToolReference` no arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="b1c19-113">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="b1c19-114">**Observação:** é necessário instalar este pacote editando o arquivo *.csproj*; não é possível usar o comando `install-package` ou a GUI do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="b1c19-114">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="b1c19-115">Edite o arquivo *RazorPagesMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="b1c19-115">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="b1c19-116">Selecione **Arquivo > Abrir Arquivo** e, em seguida, selecione o arquivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="b1c19-116">Select **File > Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="b1c19-117">Adicione `<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />` conforme destacado no código a seguir:</span><span class="sxs-lookup"><span data-stu-id="b1c19-117">Add `<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />` as highlighted in the following code:</span></span>

<span data-ttu-id="b1c19-118">[!code-xml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)] [!INCLUDE[model 3](../../includes/RP/model3.md)]</span><span class="sxs-lookup"><span data-stu-id="b1c19-118">[!code-xml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)] [!INCLUDE[model 3](../../includes/RP/model3.md)]</span></span>

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="b1c19-119">Fazer scaffolding do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="b1c19-119">Scaffold the Movie model</span></span>

* <span data-ttu-id="b1c19-120">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="b1c19-120">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="b1c19-121">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="b1c19-121">Run the following command:</span></span>

<span data-ttu-id="b1c19-122">**Observação: execute o comando a seguir no Windows. Para MacOS e Linux, consulte o próximo comando**</span><span class="sxs-lookup"><span data-stu-id="b1c19-122">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="b1c19-123">No MacOS e Linux, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="b1c19-123">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="b1c19-124">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="b1c19-124">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="b1c19-125">Saia do Visual Studio e execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="b1c19-125">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE[model 4](../../includes/RP/model4.md)]<span data-ttu-id="b1c19-126"> O tutorial a seguir explica os arquivos criados por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="b1c19-126"> The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b1c19-127">[Anterior: Introdução](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Próximo: Páginas do Razor geradas por scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="b1c19-127">[Previous: Getting Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
