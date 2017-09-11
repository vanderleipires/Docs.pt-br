---
title: Adicionando um modelo a um aplicativo ASP.NET Core MVC
author: rick-anderson
description: Adicione um modelo para um aplicativo simples do ASP.NET Core.
keywords: ASP.NET Core, MVC, scaffold, scaffolding
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-1638-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 4a158802a19011cbb45da1b3ca43d67706efe4cd
ms.sourcegitcommit: d9e2c99c837078fcac0e315025f8fbfbd45ea6e8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="0608b-104">Clique com o botão direito do mouse na pasta *Modelos* e, em seguida, selecione **Adicionar** > **Novo Arquivo**.</span><span class="sxs-lookup"><span data-stu-id="0608b-104">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="0608b-105">Na caixa de diálogo **Novo Arquivo**:</span><span class="sxs-lookup"><span data-stu-id="0608b-105">In the **New File** dialog:</span></span>

  * <span data-ttu-id="0608b-106">Selecione **Geral** no painel esquerdo.</span><span class="sxs-lookup"><span data-stu-id="0608b-106">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="0608b-107">Selecione **Classe Vazia** no painel central.</span><span class="sxs-lookup"><span data-stu-id="0608b-107">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="0608b-108">Nomeie a classe **Movie** e selecione **Novo**.</span><span class="sxs-lookup"><span data-stu-id="0608b-108">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="0608b-109">Adicione as seguintes propriedades à classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="0608b-109">Add the following properties to the `Movie` class:</span></span>

<span data-ttu-id="0608b-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="0608b-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span></span>

<span data-ttu-id="0608b-111">O campo `ID` é necessário para o banco de dados para a chave primária.</span><span class="sxs-lookup"><span data-stu-id="0608b-111">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="0608b-112">Crie o projeto para verificar se você não tem nenhum erro.</span><span class="sxs-lookup"><span data-stu-id="0608b-112">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="0608b-113">Agora você tem um **M**odelo no seu aplicativo **M**VC.</span><span class="sxs-lookup"><span data-stu-id="0608b-113">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="0608b-114">Preparar o projeto para scaffolding</span><span class="sxs-lookup"><span data-stu-id="0608b-114">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="0608b-115">Clique com o botão direito do mouse no arquivo de projeto e, em seguida, selecione **Ferramentas > Editar Arquivo**.</span><span class="sxs-lookup"><span data-stu-id="0608b-115">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![exibição da etapa acima](adding-model/_static/1.png)

- <span data-ttu-id="0608b-117">Adicione os pacotes NuGet realçados a seguir ao arquivo *MvcMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="0608b-117">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  <span data-ttu-id="0608b-118">[!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]</span><span class="sxs-lookup"><span data-stu-id="0608b-118">[!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]</span></span>

- <span data-ttu-id="0608b-119">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="0608b-119">Save the file.</span></span>

- <span data-ttu-id="0608b-120">Crie um arquivo *Models/MvcMovieContext.cs* e adicione a seguinte classe `MvcMovieContext`: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="0608b-120">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="0608b-121">Abra o arquivo *Startup.cs* e adicione dois usings: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="0608b-121">Open the *Startup.cs* file and add two usings:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="0608b-122">Adicione o contexto do banco de dados para o arquivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0608b-122">Add the database context to the *Startup.cs* file:</span></span>

   <span data-ttu-id="0608b-123">[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="0608b-123">[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]</span></span>

  <span data-ttu-id="0608b-124">Isso informa ao Entity Framework quais classes de modelo estão incluídas no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="0608b-124">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="0608b-125">Você está definindo um *conjunto de entidades* de objetos Movie, que serão representados no banco de dados como uma tabela Movie.</span><span class="sxs-lookup"><span data-stu-id="0608b-125">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="0608b-126">Crie o projeto para verificar se não existem erros.</span><span class="sxs-lookup"><span data-stu-id="0608b-126">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="0608b-127">Faça o scaffolding do MovieController</span><span class="sxs-lookup"><span data-stu-id="0608b-127">Scaffold the MovieController</span></span>

<span data-ttu-id="0608b-128">Abra uma janela de terminal na pasta do projeto e execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="0608b-128">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="0608b-129">Se você obtiver o erro `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span><span class="sxs-lookup"><span data-stu-id="0608b-129">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="0608b-130">Você está no diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="0608b-130">You are in the project directory.</span></span> <span data-ttu-id="0608b-131">O diretório do projeto tem os arquivos *Program.cs*, *Startup.cs* e *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0608b-131">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="0608b-132">Sua versão do dotnet é 1.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="0608b-132">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="0608b-133">Execute `dotnet` para obter a versão.</span><span class="sxs-lookup"><span data-stu-id="0608b-133">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="0608b-134">Você adicionou o elemento `<DotNetCliToolReference>` ao [arquivo MvcMovie.csproj](#prepare-the-project-for-scaffolding).</span><span class="sxs-lookup"><span data-stu-id="0608b-134">You have added the `<DotNetCliToolReference>` elment to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="0608b-135">O mecanismo de scaffolding cria o seguinte:</span><span class="sxs-lookup"><span data-stu-id="0608b-135">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="0608b-136">Um controlador de filmes (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="0608b-136">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="0608b-137">Arquivos de exibição do Razor para as páginas Criar, Excluir, Detalhes, Editar e Índice (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="0608b-137">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="0608b-138">A criação automática das exibições e métodos de ação [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (criar, ler, atualizar e excluir) é conhecida como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="0608b-138">The automatic creation of [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="0608b-139">Logo você terá um aplicativo Web totalmente funcional que permitirá que você gerencie um banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="0608b-139">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="0608b-140">Adicionar os arquivos ao Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0608b-140">Add the files to Visual Studio</span></span>

* <span data-ttu-id="0608b-141">Adicione o arquivo *MovieController.cs* ao projeto do Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="0608b-141">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="0608b-142">Clique com o botão direito do mouse na pasta *Controladores* e selecione **Adicionar > Adicionar Arquivos**.</span><span class="sxs-lookup"><span data-stu-id="0608b-142">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="0608b-143">Selecione o arquivo *MovieController.cs*.</span><span class="sxs-lookup"><span data-stu-id="0608b-143">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="0608b-144">Adicione a pasta *Filmes* e as exibições:</span><span class="sxs-lookup"><span data-stu-id="0608b-144">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="0608b-145">Clique com o botão direito do mouse na pasta *Exibições* e selecione **Adicionar > Adicionar Pasta Existente**.</span><span class="sxs-lookup"><span data-stu-id="0608b-145">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="0608b-146">Navegue até a pasta *Exibições*, selecione *Exibições\Filmes* e, em seguida, selecione **Abrir**.</span><span class="sxs-lookup"><span data-stu-id="0608b-146">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="0608b-147">Na caixa de diálogo **Selecionar arquivos para adicionar de Filmes**, selecione **Incluir Todos** e, em seguida, **OK**.</span><span class="sxs-lookup"><span data-stu-id="0608b-147">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="0608b-148">Agora você tem um banco de dados e páginas para exibir, editar, atualizar e excluir dados.</span><span class="sxs-lookup"><span data-stu-id="0608b-148">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="0608b-149">No próximo tutorial, trabalharemos com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0608b-149">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0608b-150">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="0608b-150">Additional resources</span></span>

* [<span data-ttu-id="0608b-151">Auxiliares de Marcas</span><span class="sxs-lookup"><span data-stu-id="0608b-151">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="0608b-152">Globalização e localização</span><span class="sxs-lookup"><span data-stu-id="0608b-152">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="0608b-153">[Anterior – adicionar uma exibição](adding-view.md)
[Próximo – trabalhando com SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="0608b-153">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
