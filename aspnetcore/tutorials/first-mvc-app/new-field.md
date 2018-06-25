---
title: Adicionar um novo campo a um aplicativo ASP.NET Core
author: rick-anderson
description: Saiba como usar as Migrações do Entity Framework Code First para adicionar um novo campo a um modelo e migrar essa alteração para um banco de dados.
ms.author: riande
ms.date: 10/14/2016
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 0077205e0f10037c9b24eab80337cb76f027e688
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278772"
---
# <a name="add-a-new-field-to-an-aspnet-core-app"></a><span data-ttu-id="9c955-103">Adicionar um novo campo a um aplicativo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9c955-103">Add a new field to an ASP.NET Core app</span></span>

<span data-ttu-id="9c955-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9c955-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9c955-105">Nesta seção, você usará as Migrações do [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First para adicionar um novo campo ao modelo e migrar essa alteração ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9c955-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="9c955-106">Ao usar o EF Code First para criar um banco de dados automaticamente, o Code First adiciona uma tabela ao banco de dados para ajudar a acompanhar se o esquema do banco de dados está sincronizado com as classes de modelo com base nas quais ele foi gerado.</span><span class="sxs-lookup"><span data-stu-id="9c955-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="9c955-107">Se ele não estiver sincronizado, o EF gerará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="9c955-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="9c955-108">Isso facilita a descoberta de problemas de código/banco de dados inconsistente.</span><span class="sxs-lookup"><span data-stu-id="9c955-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="9c955-109">Adicionando uma propriedade de classificação ao modelo de filme</span><span class="sxs-lookup"><span data-stu-id="9c955-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="9c955-110">Abra o arquivo *Models/Movie.cs* e adicione uma propriedade `Rating`:</span><span class="sxs-lookup"><span data-stu-id="9c955-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="9c955-111">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="9c955-111">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="9c955-112">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="9c955-112">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>
::: moniker-end

<span data-ttu-id="9c955-113">Compile o aplicativo (Ctrl+Shift+B).</span><span class="sxs-lookup"><span data-stu-id="9c955-113">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="9c955-114">Como você adicionou um novo campo à classe `Movie`, você também precisa atualizar a lista de permissões de associação para que essa nova propriedade seja incluída.</span><span class="sxs-lookup"><span data-stu-id="9c955-114">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="9c955-115">Em *MoviesController.cs*, atualize o atributo `[Bind]` dos métodos de ação `Create` e `Edit` para incluir a propriedade `Rating`:</span><span class="sxs-lookup"><span data-stu-id="9c955-115">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="9c955-116">Você também precisa atualizar os modelos de exibição para exibir, criar e editar a nova propriedade `Rating` na exibição do navegador.</span><span class="sxs-lookup"><span data-stu-id="9c955-116">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="9c955-117">Edite o arquivo */Views/Movies/Index.cshtml* e adicione um campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="9c955-117">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="9c955-118">Atualize */Views/Movies/Create.cshtml* com um campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="9c955-118">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="9c955-119">Copie/cole o “grupo de formulário” anterior e permita que o IntelliSense ajude você a atualizar os campos.</span><span class="sxs-lookup"><span data-stu-id="9c955-119">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="9c955-120">O IntelliSense funciona com os [Auxiliares de Marcação](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="9c955-120">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="9c955-121">Observação: na versão RTM do Visual Studio 2017, você precisa instalar o [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) para o IntelliSense do Razor.</span><span class="sxs-lookup"><span data-stu-id="9c955-121">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="9c955-122">Isso será corrigido na próxima versão.</span><span class="sxs-lookup"><span data-stu-id="9c955-122">This will be fixed in the next release.</span></span>

![O desenvolvedor digitou a letra R para o valor do atributo asp-for no segundo elemento de rótulo da exibição.](new-field/_static/cr.png)

<span data-ttu-id="9c955-126">O aplicativo não funcionará até que atualizemos o BD para incluir o novo campo.</span><span class="sxs-lookup"><span data-stu-id="9c955-126">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="9c955-127">Se você executá-lo agora, obterá o seguinte `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="9c955-127">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="9c955-128">Você está vendo este erro porque a classe de modelo Movie atualizada é diferente do esquema da tabela Movie do banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="9c955-128">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="9c955-129">(Não há nenhuma coluna Classificação na tabela de banco de dados.)</span><span class="sxs-lookup"><span data-stu-id="9c955-129">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="9c955-130">Existem algumas abordagens para resolver o erro:</span><span class="sxs-lookup"><span data-stu-id="9c955-130">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="9c955-131">Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="9c955-131">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="9c955-132">Essa abordagem é muito conveniente no início do ciclo de desenvolvimento, quando você está fazendo o desenvolvimento ativo em um banco de dados de teste; ela permite que você desenvolva rapidamente o modelo e o esquema de banco de dados juntos.</span><span class="sxs-lookup"><span data-stu-id="9c955-132">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="9c955-133">No entanto, a desvantagem é que você perde os dados existentes no banco de dados – portanto, você não deseja usar essa abordagem em um banco de dados de produção!</span><span class="sxs-lookup"><span data-stu-id="9c955-133">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="9c955-134">Muitas vezes, o uso de um inicializador para propagar um banco de dados com os dados de teste automaticamente é uma maneira produtiva de desenvolver um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9c955-134">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="9c955-135">Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="9c955-135">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="9c955-136">A vantagem dessa abordagem é que você mantém os dados.</span><span class="sxs-lookup"><span data-stu-id="9c955-136">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="9c955-137">Faça essa alteração manualmente ou criando um script de alteração de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9c955-137">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="9c955-138">Use as Migrações do Code First para atualizar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9c955-138">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="9c955-139">Para este tutorial, usaremos as Migrações do Code First.</span><span class="sxs-lookup"><span data-stu-id="9c955-139">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="9c955-140">Atualize a classe `SeedData` para que ela forneça um valor para a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="9c955-140">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="9c955-141">Uma alteração de amostra é mostrada abaixo, mas é recomendável fazer essa alteração em cada `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="9c955-141">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="9c955-142">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="9c955-142">Build the solution.</span></span>

<span data-ttu-id="9c955-143">No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="9c955-143">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Menu do PMC](adding-model/_static/pmc.png)

<span data-ttu-id="9c955-145">No PMC, insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="9c955-145">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="9c955-146">O comando `Add-Migration` informa a estrutura de migração para examinar o atual modelo `Movie` com o atual esquema de BD `Movie` e criar o código necessário para migrar o BD para o novo modelo.</span><span class="sxs-lookup"><span data-stu-id="9c955-146">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="9c955-147">O nome “Classificação” é arbitrário e é usado para nomear o arquivo de migração.</span><span class="sxs-lookup"><span data-stu-id="9c955-147">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="9c955-148">É útil usar um nome significativo para o arquivo de migração.</span><span class="sxs-lookup"><span data-stu-id="9c955-148">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="9c955-149">Se você excluir todos os registros do BD, o inicializador propagará o BD e incluirá o campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="9c955-149">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="9c955-150">Faça isso com os links Excluir no navegador ou no SSOX.</span><span class="sxs-lookup"><span data-stu-id="9c955-150">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="9c955-151">Execute o aplicativo e verifique se você pode criar/editar/exibir filmes com um campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="9c955-151">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="9c955-152">Você também deve adicionar o campo `Rating` aos modelos de exibição `Edit`, `Details` e `Delete`.</span><span class="sxs-lookup"><span data-stu-id="9c955-152">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9c955-153">[Anterior](search.md)
> [Próximo](validation.md)</span><span class="sxs-lookup"><span data-stu-id="9c955-153">[Previous](search.md)
[Next](validation.md)</span></span>  
