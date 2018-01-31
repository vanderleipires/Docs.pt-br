---
title: Adicionando um modelo a um aplicativo ASP.NET Core MVC.
author: rick-anderson
description: Adicione um modelo a um aplicativo ASP.NET Core simples.
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 81511b05a3cc11a58b93452d3c6e5305e7ee4357
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="83fab-103">Adicionar uma classe denominada *Movie.cs* à pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="83fab-103">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="83fab-104">Adicione o código a seguir ao arquivo *Models/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="83fab-104">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="83fab-105">O campo `ID` é necessário para o banco de dados para a chave primária.</span><span class="sxs-lookup"><span data-stu-id="83fab-105">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="83fab-106">Compile o aplicativo para verificar se você não tem nenhum erro e, por fim, você adicionou um **M**odelo ao seu aplicativo **M**VC.</span><span class="sxs-lookup"><span data-stu-id="83fab-106">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="83fab-107">Preparar o projeto para scaffolding</span><span class="sxs-lookup"><span data-stu-id="83fab-107">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="83fab-108">Adicione os pacotes NuGet realçados a seguir ao arquivo *MvcMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="83fab-108">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="83fab-109">Salve o arquivo e selecione **Restaurar** para a mensagem **Informativa** "Há dependências não resolvidas".</span><span class="sxs-lookup"><span data-stu-id="83fab-109">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="83fab-110">Crie um arquivo *Models/MvcMovieContext.cs* e adicione a seguinte classe `MvcMovieContext`:</span><span class="sxs-lookup"><span data-stu-id="83fab-110">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="83fab-111">Abra o arquivo *Startup.cs* e adicione dois usings:</span><span class="sxs-lookup"><span data-stu-id="83fab-111">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="83fab-112">Adicione o contexto do banco de dados para o arquivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="83fab-112">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="83fab-113">Isso informa ao Entity Framework quais classes de modelo estão incluídas no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="83fab-113">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="83fab-114">Você está definindo um *conjunto de entidades* de objetos Movie, que serão representados no banco de dados como uma tabela Movie.</span><span class="sxs-lookup"><span data-stu-id="83fab-114">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="83fab-115">Crie o projeto para verificar se não existem erros.</span><span class="sxs-lookup"><span data-stu-id="83fab-115">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="83fab-116">Faça o scaffolding do MovieController</span><span class="sxs-lookup"><span data-stu-id="83fab-116">Scaffold the MovieController</span></span>

<span data-ttu-id="83fab-117">Abra uma janela de terminal na pasta do projeto e execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="83fab-117">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="83fab-118">O mecanismo de scaffolding cria o seguinte:</span><span class="sxs-lookup"><span data-stu-id="83fab-118">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="83fab-119">Um controlador de filmes (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="83fab-119">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="83fab-120">Arquivos de exibição do Razor para as páginas Criar, Excluir, Detalhes, Editar e Índice (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="83fab-120">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="83fab-121">A criação automática das exibições e métodos de ação [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (criar, ler, atualizar e excluir) é conhecida como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="83fab-121">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="83fab-122">Logo você terá um aplicativo Web totalmente funcional que permitirá que você gerencie um banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="83fab-122">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="83fab-123">Agora você tem um banco de dados e páginas para exibir, editar, atualizar e excluir dados.</span><span class="sxs-lookup"><span data-stu-id="83fab-123">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="83fab-124">No próximo tutorial, trabalharemos com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="83fab-124">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="83fab-125">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="83fab-125">Additional resources</span></span>

* [<span data-ttu-id="83fab-126">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="83fab-126">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="83fab-127">Globalização e localização</span><span class="sxs-lookup"><span data-stu-id="83fab-127">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="83fab-128">[Anterior – adicionar uma exibição](adding-view.md)
[Próximo – trabalhando com SQLite](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="83fab-128">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
