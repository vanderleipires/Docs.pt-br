---
title: Adicionar um novo campo em uma página Razor no ASP.NET Core
author: rick-anderson
description: Mostra como adicionar um novo campo a uma página Razor com o Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 6c23e5ab21dbb94c69ba50200a1d76647e22410a
ms.sourcegitcommit: 4afaa55918262c8dcbd3efa9584959a731b47681
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45613447"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="c1c27-103">Adicionar um novo campo em uma página Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1c27-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="c1c27-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c1c27-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c1c27-105">Nesta seção, você usará as Migrações do Code First do [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) para adicionar um novo campo ao modelo e migrar essa alteração ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c1c27-105">In this section you use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="c1c27-106">Ao usar o Code First do EF para criar automaticamente um banco de dados, o Code First:</span><span class="sxs-lookup"><span data-stu-id="c1c27-106">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="c1c27-107">Adiciona uma tabela ao banco de dados para acompanhar se o esquema do banco de dados está sincronizado com as classes de modelo das quais ele foi gerado.</span><span class="sxs-lookup"><span data-stu-id="c1c27-107">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="c1c27-108">Se as classes de modelo não estiverem em sincronia com o banco de dados, o EF gerará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="c1c27-108">If the model classes aren't in sync with the DB, EF throws an exception.</span></span> 

<span data-ttu-id="c1c27-109">Verificação automática de esquema/modelo em sincronia torna mais fácil encontrar problemas de código/banco de dados inconsistente.</span><span class="sxs-lookup"><span data-stu-id="c1c27-109">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="c1c27-110">Adicionando uma propriedade de classificação ao modelo de filme</span><span class="sxs-lookup"><span data-stu-id="c1c27-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="c1c27-111">Abra o arquivo *Models/Movie.cs* e adicione uma propriedade `Rating`:</span><span class="sxs-lookup"><span data-stu-id="c1c27-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

<span data-ttu-id="c1c27-112">Compile o aplicativo (Ctrl+Shift+B).</span><span class="sxs-lookup"><span data-stu-id="c1c27-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="c1c27-113">Edite *Pages/Movies/Index.cshtml* e adicione um campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="c1c27-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="c1c27-114">Adicione o campo `Rating` às páginas Excluir e Detalhes.</span><span class="sxs-lookup"><span data-stu-id="c1c27-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="c1c27-115">Atualize *Create.cshtml* com um campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c1c27-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="c1c27-116">Copie/cole o elemento `<div>` anterior e permita que o IntelliSense ajude você a atualizar os campos.</span><span class="sxs-lookup"><span data-stu-id="c1c27-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="c1c27-117">O IntelliSense funciona com os [Auxiliares de Marcação](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c1c27-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![O desenvolvedor digitou a letra R para o valor do atributo asp-for no segundo elemento de rótulo da exibição.](new-field/_static/cr.png)

<span data-ttu-id="c1c27-121">O seguinte código mostra *Create.cshtml* com um campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="c1c27-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="c1c27-122">Adicione o campo `Rating` à página Editar.</span><span class="sxs-lookup"><span data-stu-id="c1c27-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="c1c27-123">O aplicativo não funcionará até que o BD seja atualizado para incluir o novo campo.</span><span class="sxs-lookup"><span data-stu-id="c1c27-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="c1c27-124">Se for executado agora, o aplicativo gerará uma `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="c1c27-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="c1c27-125">Esse erro é causado devido à classe de modelo Movie atualizada ser diferente do esquema da tabela Movie do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c1c27-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="c1c27-126">(Não há nenhuma coluna `Rating` na tabela de banco de dados.)</span><span class="sxs-lookup"><span data-stu-id="c1c27-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="c1c27-127">Existem algumas abordagens para resolver o erro:</span><span class="sxs-lookup"><span data-stu-id="c1c27-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="c1c27-128">Faça com que o Entity Framework remova automaticamente e recrie o banco de dados usando o novo esquema de classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="c1c27-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="c1c27-129">Essa abordagem é conveniente no início do ciclo de desenvolvimento; ela permite que você desenvolva rapidamente o modelo e o esquema de banco de dados juntos.</span><span class="sxs-lookup"><span data-stu-id="c1c27-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="c1c27-130">A desvantagem é que você perde os dados existentes no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c1c27-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="c1c27-131">Você não deseja usar essa abordagem em um banco de dados de produção!</span><span class="sxs-lookup"><span data-stu-id="c1c27-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="c1c27-132">A remoção do BD em alterações de esquema e o uso de um inicializador para propagar automaticamente o banco de dados com os dados de teste é muitas vezes uma maneira produtiva de desenvolver um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c1c27-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="c1c27-133">Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="c1c27-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="c1c27-134">A vantagem dessa abordagem é que você mantém os dados.</span><span class="sxs-lookup"><span data-stu-id="c1c27-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="c1c27-135">Faça essa alteração manualmente ou criando um script de alteração de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c1c27-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="c1c27-136">Use as Migrações do Code First para atualizar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c1c27-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="c1c27-137">Para este tutorial, use as Migrações do Code First.</span><span class="sxs-lookup"><span data-stu-id="c1c27-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="c1c27-138">Atualize a classe `SeedData` para que ela forneça um valor para a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="c1c27-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="c1c27-139">Uma alteração de amostra é mostrada abaixo, mas é recomendável fazer essa alteração em cada bloco `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="c1c27-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="c1c27-140">Consulte o [arquivo SeedData.cs concluído](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="c1c27-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="c1c27-141">Consulte o [arquivo SeedData.cs concluído](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="c1c27-141">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span></span>
::: moniker-end

<span data-ttu-id="c1c27-142">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="c1c27-142">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="c1c27-143">No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="c1c27-143">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="c1c27-144">No PMC, insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="c1c27-144">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="c1c27-145">O comando `Add-Migration` informa à estrutura:</span><span class="sxs-lookup"><span data-stu-id="c1c27-145">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="c1c27-146">Compare o modelo `Movie` com o esquema de BD `Movie`.</span><span class="sxs-lookup"><span data-stu-id="c1c27-146">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="c1c27-147">Crie um código para migrar o esquema de BD para o novo modelo.</span><span class="sxs-lookup"><span data-stu-id="c1c27-147">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="c1c27-148">O nome “Classificação” é arbitrário e é usado para nomear o arquivo de migração.</span><span class="sxs-lookup"><span data-stu-id="c1c27-148">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="c1c27-149">É útil usar um nome significativo para o arquivo de migração.</span><span class="sxs-lookup"><span data-stu-id="c1c27-149">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="c1c27-150">Se você excluir todos os registros do BD, o inicializador propagará o BD e incluirá o campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c1c27-150">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="c1c27-151">Faça isso com os links Excluir no navegador ou no [SSOX](xref:tutorials/razor-pages/sql#ssox) (Pesquisador de Objetos do SQL Server).</span><span class="sxs-lookup"><span data-stu-id="c1c27-151">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="c1c27-152">Para excluir o banco de dados do SSOX:</span><span class="sxs-lookup"><span data-stu-id="c1c27-152">To delete the database from SSOX:</span></span>

* <span data-ttu-id="c1c27-153">Selecione o banco de dados no SSOX.</span><span class="sxs-lookup"><span data-stu-id="c1c27-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="c1c27-154">Clique com o botão direito do mouse no banco de dados e selecione *Excluir*.</span><span class="sxs-lookup"><span data-stu-id="c1c27-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="c1c27-155">Marque **Fechar conexões existentes**.</span><span class="sxs-lookup"><span data-stu-id="c1c27-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="c1c27-156">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1c27-156">Select **OK**.</span></span>
* <span data-ttu-id="c1c27-157">No [PMC](xref:tutorials/razor-pages/new-field#pmc), atualize o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="c1c27-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="c1c27-158">Execute o aplicativo e verifique se você pode criar/editar/exibir filmes com um campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c1c27-158">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="c1c27-159">Se o banco de dados não for propagado, pare o IIS Express e, em seguida, execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c1c27-159">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c1c27-160">[Anterior: Adicionando uma pesquisa](xref:tutorials/razor-pages/search)
> [Próximo: Adicionando Validação](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="c1c27-160">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
