---
title: Adicionar um modelo a um aplicativo Páginas Razor do ASP.NET Core com o Visual Studio para Mac
author: rick-anderson
description: Saiba como adicionar um modelo para um aplicativo Páginas Razor no ASP.NET Core usando o Visual Studio para Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 3ca6c9b9988b8335116b7248c6c4a89997d02b14
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186752"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a><span data-ttu-id="d774c-103">Adicionar um modelo a um aplicativo Páginas Razor do ASP.NET Core com o Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="d774c-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio for Mac</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="d774c-104">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="d774c-104">Add a data model</span></span>

* <span data-ttu-id="d774c-105">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **RazorPagesMovie** e então selecione **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="d774c-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="d774c-106">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="d774c-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="d774c-107">Clique com o botão direito do mouse na pasta *Modelos* e, em seguida, selecione **Adicionar** > **Novo Arquivo**.</span><span class="sxs-lookup"><span data-stu-id="d774c-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="d774c-108">Na caixa de diálogo **Novo Arquivo**:</span><span class="sxs-lookup"><span data-stu-id="d774c-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="d774c-109">Selecione **Geral** no painel esquerdo.</span><span class="sxs-lookup"><span data-stu-id="d774c-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="d774c-110">Selecione **Classe Vazia** no painel central.</span><span class="sxs-lookup"><span data-stu-id="d774c-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="d774c-111">Nomeie a classe **Movie** e selecione **Novo**.</span><span class="sxs-lookup"><span data-stu-id="d774c-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="d774c-112">Clique com o botão direito do mouse em uma linha curvada vermelha, por exemplo, `MovieContext` na linha `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="d774c-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="d774c-113">Selecione **Correção Rápida > using RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="d774c-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="d774c-114">O Visual studio adiciona a instrução using.</span><span class="sxs-lookup"><span data-stu-id="d774c-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="d774c-115">Compile o projeto para verificar se não há erros.</span><span class="sxs-lookup"><span data-stu-id="d774c-115">Build the project to verify you don't have any errors.</span></span>

![Criar página](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="d774c-117">Pacotes NuGet do Entity Framework Core para migrações</span><span class="sxs-lookup"><span data-stu-id="d774c-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="d774c-118">As ferramentas do EF para a CLI (interface de linha de comando) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="d774c-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="d774c-119">Clique no link [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) para obter o número de versão a ser usado.</span><span class="sxs-lookup"><span data-stu-id="d774c-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="d774c-120">Para instalar esse pacote, adicione-o à coleção `DotNetCliToolReference` no arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="d774c-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="d774c-121">**Observação:** é necessário instalar este pacote editando o arquivo *.csproj*; não é possível usar o comando `install-package` ou a GUI do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="d774c-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="d774c-122">Para editar um arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="d774c-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="d774c-123">Selecione **Arquivo** > **Abrir**, e, em seguida, selecione o arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="d774c-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="d774c-124">Selecione **Opções**.</span><span class="sxs-lookup"><span data-stu-id="d774c-124">Select **Options**.</span></span>
* <span data-ttu-id="d774c-125">Alterar **Abrir com** para **Editor de código-fonte**.</span><span class="sxs-lookup"><span data-stu-id="d774c-125">Change **Open with** to **Source Code Editor**.</span></span>

![Editar o arquivo csproj](model/csproj.png)

<span data-ttu-id="d774c-127">Adicione a referência da ferramenta `Microsoft.EntityFrameworkCore.Tools.DotNet` para o segundo **\<ItemGroup>**.:</span><span class="sxs-lookup"><span data-stu-id="d774c-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="d774c-128">Os números de versão mostrados no código a seguir estavam corretos no momento da gravação.</span><span class="sxs-lookup"><span data-stu-id="d774c-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="d774c-129">Adicionar os arquivos de páginas/filmes ao projeto</span><span class="sxs-lookup"><span data-stu-id="d774c-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="d774c-130">No Visual Studio, clique com o botão direito do mouse na pasta *Páginas* e selecione **Adicionar > Adicionar Pasta Existente**.</span><span class="sxs-lookup"><span data-stu-id="d774c-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="d774c-131">Selecione a pasta *Filmes*.</span><span class="sxs-lookup"><span data-stu-id="d774c-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="d774c-132">Na caixa de diálogo *Escolher arquivos para incluir no projeto*, selecione **Incluir Todos**.</span><span class="sxs-lookup"><span data-stu-id="d774c-132">In the *Choose files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="d774c-133">O tutorial a seguir explica os arquivos criados por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="d774c-133">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d774c-134">[Anterior: Introdução](xref:tutorials/razor-pages-mac/razor-pages-start)
> [Próximo: Páginas Razor geradas por scaffolding](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="d774c-134">[Previous: Get Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
