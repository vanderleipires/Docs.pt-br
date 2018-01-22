---
title: "Adicionando um modelo para um aplicativo de Páginas do Razor com o Visual Studio para Mac"
author: rick-anderson
description: "Adicionando um modelo para um aplicativo de Páginas do Razor no ASP.NET Core usando o Visual Studio para Mac"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 7b1b2d54e9c68b0a6f2b1355726d0d1cb484f69e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="65649-103">Adicionando um modelo para um aplicativo de Páginas do Razor no ASP.NET Core com o Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="65649-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="65649-104">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="65649-104">Add a data model</span></span>

* <span data-ttu-id="65649-105">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **RazorPagesMovie** e então selecione **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="65649-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="65649-106">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="65649-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="65649-107">Clique com o botão direito do mouse na pasta *Modelos* e, em seguida, selecione **Adicionar** > **Novo Arquivo**.</span><span class="sxs-lookup"><span data-stu-id="65649-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="65649-108">Na caixa de diálogo **Novo Arquivo**:</span><span class="sxs-lookup"><span data-stu-id="65649-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="65649-109">Selecione **Geral** no painel esquerdo.</span><span class="sxs-lookup"><span data-stu-id="65649-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="65649-110">Selecione **Classe Vazia** no painel central.</span><span class="sxs-lookup"><span data-stu-id="65649-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="65649-111">Nomeie a classe **Movie** e selecione **Novo**.</span><span class="sxs-lookup"><span data-stu-id="65649-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="65649-112">Clique com o botão direito do mouse em uma linha curvada vermelha, por exemplo, `MovieContext` na linha `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="65649-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="65649-113">Selecione **Correção Rápida > using RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="65649-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="65649-114">O Visual studio adiciona a instrução using.</span><span class="sxs-lookup"><span data-stu-id="65649-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="65649-115">Compile o projeto para verificar se não há erros.</span><span class="sxs-lookup"><span data-stu-id="65649-115">Build the project to verify you don't have any errors.</span></span>

![Criar página](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="65649-117">Pacotes NuGet do Entity Framework Core para migrações</span><span class="sxs-lookup"><span data-stu-id="65649-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="65649-118">As ferramentas do EF para a CLI (interface de linha de comando) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="65649-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="65649-119">Para instalar esse pacote, adicione-o à coleção `DotNetCliToolReference` no arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="65649-119">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="65649-120">**Observação:** é necessário instalar este pacote editando o arquivo *.csproj*; não é possível usar o comando `install-package` ou a GUI do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="65649-120">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="65649-121">Para editar um arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="65649-121">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="65649-122">Selecione **Arquivo** > **Abrir**, e, em seguida, selecione o arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="65649-122">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="65649-123">Selecione **Opções**.</span><span class="sxs-lookup"><span data-stu-id="65649-123">Select **Options**.</span></span>
* <span data-ttu-id="65649-124">Alterar **Abrir com** para **Editor de código-fonte**.</span><span class="sxs-lookup"><span data-stu-id="65649-124">Change **Open with** to **Source Code Editor**.</span></span>

![Editar o arquivo csproj](model/csproj.png)

<span data-ttu-id="65649-126">Adicione a referência da ferramenta `Microsoft.EntityFrameworkCore.Tools.DotNet` para o segundo **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="65649-126">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="65649-127">Adicionar os arquivos de páginas/filmes ao projeto</span><span class="sxs-lookup"><span data-stu-id="65649-127">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="65649-128">No Visual Studio, clique com o botão direito do mouse na pasta *Páginas* e selecione **Adicionar > Adicionar Pasta Existente**.</span><span class="sxs-lookup"><span data-stu-id="65649-128">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="65649-129">Selecione a pasta *Filmes*.</span><span class="sxs-lookup"><span data-stu-id="65649-129">Select the *Movies* folder.</span></span>
* <span data-ttu-id="65649-130">Na caixa de diálogo *Selecionar arquivos para incluir no projeto*, selecione **Incluir Todos**.</span><span class="sxs-lookup"><span data-stu-id="65649-130">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="65649-131">O tutorial a seguir explica os arquivos criados por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="65649-131">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="65649-132">[Anterior: Introdução](xref:tutorials/razor-pages-mac/razor-pages-start)
[Próximo: Páginas do Razor geradas por scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="65649-132">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
