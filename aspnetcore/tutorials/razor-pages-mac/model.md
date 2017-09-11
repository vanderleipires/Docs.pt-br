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
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: b234eb93fbca1f4c83712990712b86e9941968fd
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="7c90d-104">Adicionando um modelo para um aplicativo de Páginas do Razor no ASP.NET Core com o Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="7c90d-104">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="7c90d-105">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="7c90d-105">Add a data model</span></span>

* <span data-ttu-id="7c90d-106">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **RazorPagesMovie** e então selecione **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="7c90d-106">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="7c90d-107">Nomeie a pasta como *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="7c90d-107">Name the folder *Models*.</span></span>
* <span data-ttu-id="7c90d-108">Clique com o botão direito do mouse na pasta *Modelos* e, em seguida, selecione **Adicionar** > **Novo Arquivo**.</span><span class="sxs-lookup"><span data-stu-id="7c90d-108">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="7c90d-109">Na caixa de diálogo **Novo Arquivo**:</span><span class="sxs-lookup"><span data-stu-id="7c90d-109">In the **New File** dialog:</span></span>

  * <span data-ttu-id="7c90d-110">Selecione **Geral** no painel esquerdo.</span><span class="sxs-lookup"><span data-stu-id="7c90d-110">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="7c90d-111">Selecione **Classe Vazia** no painel central.</span><span class="sxs-lookup"><span data-stu-id="7c90d-111">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="7c90d-112">Nomeie a classe **Movie** e selecione **Novo**.</span><span class="sxs-lookup"><span data-stu-id="7c90d-112">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

<span data-ttu-id="7c90d-113">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="7c90d-113">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span></span>

<span data-ttu-id="7c90d-114">Clique com o botão direito do mouse em uma linha curvada vermelha, por exemplo, `MovieContext` na linha `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="7c90d-114">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="7c90d-115">Selecione **Correção Rápida > using RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="7c90d-115">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="7c90d-116">O Visual studio adiciona a instrução using.</span><span class="sxs-lookup"><span data-stu-id="7c90d-116">Visual studio adds the using statement.</span></span>

<span data-ttu-id="7c90d-117">Crie o projeto para verificar se você não tem nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="7c90d-117">Build the project to verify you don't have any errors.</span></span>

![Criar página](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="7c90d-119">Pacotes NuGet do Entity Framework Core para migrações</span><span class="sxs-lookup"><span data-stu-id="7c90d-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="7c90d-120">As ferramentas do EF para a CLI (interface de linha de comando) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="7c90d-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="7c90d-121">Para instalar esse pacote, adicione-o à coleção `DotNetCliToolReference` no arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="7c90d-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="7c90d-122">**Observação:** é necessário instalar este pacote editando o arquivo *.csproj*; não é possível usar o comando `install-package` ou a GUI do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="7c90d-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="7c90d-123">Para editar um arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="7c90d-123">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="7c90d-124">Selecione **Arquivo > Abrir** e, em seguida, selecione o arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="7c90d-124">Select **File > Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="7c90d-125">Selecione **Opções**.</span><span class="sxs-lookup"><span data-stu-id="7c90d-125">Select **Options**.</span></span>
* <span data-ttu-id="7c90d-126">Alterar **Abrir com** para **Editor de código-fonte**.</span><span class="sxs-lookup"><span data-stu-id="7c90d-126">Change **Open with** to **Source Code Editor**.</span></span>

![Editar o arquivo csproj](model/csproj.png)

<span data-ttu-id="7c90d-128">O código a seguir mostra o arquivo *csproj* atualizado.</span><span class="sxs-lookup"><span data-stu-id="7c90d-128">The following code shows the updated *csproj* file.</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="7c90d-129">Adicionar os arquivos de páginas/filmes ao projeto</span><span class="sxs-lookup"><span data-stu-id="7c90d-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="7c90d-130">No Visual Studio, clique com o botão direito do mouse na pasta *Páginas* e selecione **Adicionar > Adicionar Pasta Existente**.</span><span class="sxs-lookup"><span data-stu-id="7c90d-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="7c90d-131">Selecione a pasta *Filmes*.</span><span class="sxs-lookup"><span data-stu-id="7c90d-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="7c90d-132">Na caixa de diálogo *Selecionar arquivos para incluir no projeto*, selecione **Incluir Todos**.</span><span class="sxs-lookup"><span data-stu-id="7c90d-132">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="7c90d-133">O tutorial a seguir explica os arquivos criados por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="7c90d-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7c90d-134">[Anterior: Introdução](xref:tutorials/razor-pages-mac/razor-pages-start)
[Próximo: Páginas do Razor geradas por scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="7c90d-134">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
