---
title: Adicionar um modelo a um aplicativo Páginas Razor do ASP.NET Core com o Visual Studio Code
author: rick-anderson
description: Saiba como adicionar um modelo para um aplicativo Páginas Razor no ASP.NET Core usando o Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244717"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="cd7df-103">Adicionar um modelo a um aplicativo Páginas Razor do ASP.NET Core com o Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cd7df-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="cd7df-104">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="cd7df-104">Add a data model</span></span>

* <span data-ttu-id="cd7df-105">Adicione uma pasta denominada *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="cd7df-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="cd7df-106">Adicionar uma classe denominada *Movie.cs* à pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="cd7df-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="cd7df-107">Adicione o código a seguir ao arquivo *Models/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd7df-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a><span data-ttu-id="cd7df-108">Pacotes do NuGet do Entity Framework Core para SQLite</span><span class="sxs-lookup"><span data-stu-id="cd7df-108">Entity Framework Core NuGet package for SQLite</span></span>

<span data-ttu-id="cd7df-109">Na linha de comando, execute o seguinte comando de CLI do .NET Core:</span><span class="sxs-lookup"><span data-stu-id="cd7df-109">From the command line, run the following .NET Core CLI command:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="cd7df-110">Registrar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="cd7df-110">Register the database context</span></span>

<span data-ttu-id="cd7df-111">Registre o contexto do banco de dados com o contêiner de [injeção de dependência](xref:fundamentals/dependency-injection) no arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="cd7df-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

<span data-ttu-id="cd7df-112">Adicione os demonstrativos do `using` a seguir à parte superior do *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd7df-112">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="cd7df-113">Compile o projeto para verificar se não há erros.</span><span class="sxs-lookup"><span data-stu-id="cd7df-113">Build the project to verify you don't have any errors.</span></span>

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a><span data-ttu-id="cd7df-114">Fazer scaffolding do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="cd7df-114">Scaffold the Movie model</span></span>

* <span data-ttu-id="cd7df-115">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="cd7df-115">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="cd7df-116">**No Windows**, execute o comando a seguir:</span><span class="sxs-lookup"><span data-stu-id="cd7df-116">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="cd7df-117">**No macOS e Linux**, execute o comando a seguir:</span><span class="sxs-lookup"><span data-stu-id="cd7df-117">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="cd7df-118">O tutorial a seguir explica os arquivos criados por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="cd7df-118">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cd7df-119">[Anterior: Introdução](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Próximo: Páginas Razor geradas por scaffolding](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="cd7df-119">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
