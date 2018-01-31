---
title: "Adicionando um Novo Campo a uma página Razor"
author: rick-anderson
description: "Mostra como adicionar um novo campo a uma página Razor com o Entity Framework Core"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 36412e9d1f3143f0d1999d0e754e6627f0984ad5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="bf9ef-103">Adicionando um novo campo a uma página Razor</span><span class="sxs-lookup"><span data-stu-id="bf9ef-103">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="bf9ef-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bf9ef-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bf9ef-105">Nesta seção, você usará as Migrações do [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First para adicionar um novo campo ao modelo e migrar essa alteração ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="bf9ef-106">Ao usar o EF Code First para criar um banco de dados automaticamente, o Code First adiciona uma tabela ao banco de dados para ajudar a acompanhar se o esquema do banco de dados está sincronizado com as classes de modelo com base nas quais ele foi gerado.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="bf9ef-107">Se ele não estiver sincronizado, o EF gerará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="bf9ef-108">Isso facilita a descoberta de problemas de código/banco de dados inconsistente.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="bf9ef-109">Adicionando uma propriedade de classificação ao modelo de filme</span><span class="sxs-lookup"><span data-stu-id="bf9ef-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="bf9ef-110">Abra o arquivo *Models/Movie.cs* e adicione uma propriedade `Rating`:</span><span class="sxs-lookup"><span data-stu-id="bf9ef-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="bf9ef-111">Compile o aplicativo (Ctrl+Shift+B).</span><span class="sxs-lookup"><span data-stu-id="bf9ef-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="bf9ef-112">Edite *Pages/Movies/Index.cshtml* e adicione um campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="bf9ef-112">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="bf9ef-113">Adicione o campo `Rating` às páginas Excluir e Detalhes.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-113">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="bf9ef-114">Atualize *Create.cshtml* com um campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-114">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="bf9ef-115">Copie/cole o elemento `<div>` anterior e permita que o IntelliSense ajude você a atualizar os campos.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-115">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="bf9ef-116">O IntelliSense funciona com os [Auxiliares de Marcação](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="bf9ef-116">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![O desenvolvedor digitou a letra R para o valor do atributo asp-for no segundo elemento de rótulo da exibição.](new-field/_static/cr.png)

<span data-ttu-id="bf9ef-120">O seguinte código mostra *Create.cshtml* com um campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="bf9ef-120">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="bf9ef-121">Adicione o campo `Rating` à página Editar.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-121">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="bf9ef-122">O aplicativo não funcionará até que o BD seja atualizado para incluir o novo campo.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-122">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="bf9ef-123">Se for executado agora, o aplicativo gerará uma `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="bf9ef-123">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="bf9ef-124">Esse erro é causado devido à classe de modelo Movie atualizada ser diferente do esquema da tabela Movie do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-124">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="bf9ef-125">(Não há nenhuma coluna `Rating` na tabela de banco de dados.)</span><span class="sxs-lookup"><span data-stu-id="bf9ef-125">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="bf9ef-126">Existem algumas abordagens para resolver o erro:</span><span class="sxs-lookup"><span data-stu-id="bf9ef-126">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="bf9ef-127">Faça com que o Entity Framework remova automaticamente e recrie o banco de dados usando o novo esquema de classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-127">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="bf9ef-128">Essa abordagem é conveniente no início do ciclo de desenvolvimento; ela permite que você desenvolva rapidamente o modelo e o esquema de banco de dados juntos.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-128">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="bf9ef-129">A desvantagem é que você perde os dados existentes no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-129">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="bf9ef-130">Você não deseja usar essa abordagem em um banco de dados de produção!</span><span class="sxs-lookup"><span data-stu-id="bf9ef-130">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="bf9ef-131">A remoção do BD em alterações de esquema e o uso de um inicializador para propagar automaticamente o banco de dados com os dados de teste é muitas vezes uma maneira produtiva de desenvolver um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-131">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="bf9ef-132">Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-132">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="bf9ef-133">A vantagem dessa abordagem é que você mantém os dados.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-133">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="bf9ef-134">Faça essa alteração manualmente ou criando um script de alteração de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-134">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="bf9ef-135">Use as Migrações do Code First para atualizar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-135">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="bf9ef-136">Para este tutorial, use as Migrações do Code First.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-136">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="bf9ef-137">Atualize a classe `SeedData` para que ela forneça um valor para a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-137">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="bf9ef-138">Uma alteração de amostra é mostrada abaixo, mas é recomendável fazer essa alteração em cada bloco `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-138">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="bf9ef-139">Consulte o [arquivo SeedData.cs concluído](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="bf9ef-139">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="bf9ef-140">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-140">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="bf9ef-141">No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="bf9ef-142">No PMC, insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="bf9ef-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="bf9ef-143">O comando `Add-Migration` informa à estrutura:</span><span class="sxs-lookup"><span data-stu-id="bf9ef-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="bf9ef-144">Compare o modelo `Movie` com o esquema de BD `Movie`.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="bf9ef-145">Crie um código para migrar o esquema de BD para o novo modelo.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="bf9ef-146">O nome “Classificação” é arbitrário e é usado para nomear o arquivo de migração.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="bf9ef-147">É útil usar um nome significativo para o arquivo de migração.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-147">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="bf9ef-148">Se você excluir todos os registros do BD, o inicializador propagará o BD e incluirá o campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-148">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="bf9ef-149">Faça isso com os links Excluir no navegador ou no [SSOX](xref:tutorials/razor-pages/sql#ssox) (Pesquisador de Objetos do SQL Server).</span><span class="sxs-lookup"><span data-stu-id="bf9ef-149">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="bf9ef-150">Para excluir o banco de dados do SSOX:</span><span class="sxs-lookup"><span data-stu-id="bf9ef-150">To delete the database from SSOX:</span></span>

* <span data-ttu-id="bf9ef-151">Selecione o banco de dados no SSOX.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-151">Select the database in SSOX.</span></span>
* <span data-ttu-id="bf9ef-152">Clique com o botão direito do mouse no banco de dados e selecione *Excluir*.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-152">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="bf9ef-153">Marque **Fechar conexões existentes**.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-153">Check **Close existing connections**.</span></span>
* <span data-ttu-id="bf9ef-154">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-154">Select **OK**.</span></span>
* <span data-ttu-id="bf9ef-155">No [PMC](xref:tutorials/razor-pages/new-field#pmc), atualize o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="bf9ef-155">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="bf9ef-156">Execute o aplicativo e verifique se você pode criar/editar/exibir filmes com um campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-156">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="bf9ef-157">Se o banco de dados não for propagado, pare o IIS Express e, em seguida, execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bf9ef-157">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="bf9ef-158">[Anterior: Adicionando uma pesquisa](xref:tutorials/razor-pages/search)
[Próximo: Adicionando Validação](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="bf9ef-158">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
