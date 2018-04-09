---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Adicionar um novo campo para o modelo de filme e a tabela | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples de seguir e demonstração...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d8a42e9acdce687ab6e9742071dd2949f244622f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="85612-104">Adicionar um novo campo para o modelo de filme e tabela</span><span class="sxs-lookup"><span data-stu-id="85612-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="85612-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="85612-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="85612-106">Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="85612-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="85612-107">É muito mais simples a seguir, mais segura e demonstra mais recursos.</span><span class="sxs-lookup"><span data-stu-id="85612-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="85612-108">Nesta seção, você usará migrações do Entity Framework Code First para migrar algumas alterações para as classes de modelo para que a alteração será aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="85612-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="85612-109">Por padrão, ao usar o Entity Framework Code First para criar automaticamente um banco de dados, como você fez anteriormente neste tutorial, Code First adiciona uma tabela no banco de dados para ajudar a controlar se o esquema do banco de dados está em sincronizado com classes de modelo do que qual ela foi gerada.</span><span class="sxs-lookup"><span data-stu-id="85612-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="85612-110">Se não estiverem sincronizados, o Entity Framework gerará um erro.</span><span class="sxs-lookup"><span data-stu-id="85612-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="85612-111">Isso torna mais fácil rastrear os problemas em tempo de desenvolvimento que você pode caso contrário, apenas encontrar (por erros obscuros) em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="85612-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="85612-112">Configuração de migrações do Code First para alterações de modelo</span><span class="sxs-lookup"><span data-stu-id="85612-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="85612-113">Se você estiver usando o Visual Studio 2012, clique duas vezes o *Movies.mdf* arquivo no Gerenciador de soluções para abrir a ferramenta de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="85612-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="85612-114">Visual Studio Express para Web mostrará Pesquisador de objetos de banco de dados, o Visual Studio 2012 mostrará o Gerenciador de servidores.</span><span class="sxs-lookup"><span data-stu-id="85612-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="85612-115">Se você estiver usando o Visual Studio 2010, use o Pesquisador de objetos do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="85612-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="85612-116">Na ferramenta de banco de dados (Pesquisador de objetos de banco de dados, Gerenciador de servidores ou Pesquisador de objetos do SQL Server), clique com botão direito em `MovieDBContext` e selecione **excluir** para descartar o banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="85612-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="85612-117">Navegue de volta para o Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="85612-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="85612-118">Clique com o botão direito no *Movies.mdf* de arquivo e selecione **excluir** para remover o banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="85612-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="85612-119">O aplicativo para verificar se que não existem erros de compilação.</span><span class="sxs-lookup"><span data-stu-id="85612-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="85612-120">Do **ferramentas** menu, clique em **Gerenciador de biblioteca de pacote** e **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="85612-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Adicionar pacote Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="85612-122">No **Package Manager Console** janela o `PM>` prompt insira "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="85612-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="85612-123">O **Enable-Migrations** comando (mostrado acima) cria um *Configuration.cs* arquivo em uma nova *migrações* pasta.</span><span class="sxs-lookup"><span data-stu-id="85612-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="85612-124">O Visual Studio abrirá o *Configuration.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="85612-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="85612-125">Substitua o `Seed` método o *Configuration.cs* arquivo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="85612-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="85612-126">Clique com o botão direito na linha curvada vermelha sob `Movie` e selecione **resolver** , em seguida, **usando** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="85612-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="85612-127">Isso adiciona a seguinte instrução using:</span><span class="sxs-lookup"><span data-stu-id="85612-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="85612-128">Code First Migrations chamadas a `Seed` método após a migração de todos (ou seja, chamando **Atualizar banco de dados** no Console do Gerenciador de pacotes), e este método atualiza linhas que já foram inseridas ou insere-los se eles não existem ainda.</span><span class="sxs-lookup"><span data-stu-id="85612-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="85612-129">**Pressione CTRL-SHIFT-B para compilar o projeto.** (As etapas a seguir falhará se o não criar neste momento.)</span><span class="sxs-lookup"><span data-stu-id="85612-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="85612-130">A próxima etapa é criar um `DbMigration` classe para a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="85612-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="85612-131">A migração para cria um novo banco de dados, que é por isso que você excluiu o *movie.mdf* arquivo em uma etapa anterior.</span><span class="sxs-lookup"><span data-stu-id="85612-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="85612-132">No **Package Manager Console** janela, digite o comando "Adicionar-migração inicial" para criar a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="85612-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="85612-133">O nome "Inicial" é arbitrário e é usado para nomear o arquivo de migração criado.</span><span class="sxs-lookup"><span data-stu-id="85612-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="85612-134">Migrações do Code First cria outro arquivo de classe no *migrações* pasta (com o nome *{DateStamp}\_Initial.cs* ), e essa classe contém o código que cria o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="85612-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="85612-135">O nome do arquivo de migração previamente é fixo com um carimbo de hora para ajudar com a ordenação.</span><span class="sxs-lookup"><span data-stu-id="85612-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="85612-136">Examine o *{DateStamp}\_Initial.cs* arquivo, ele contém as instruções para criar a tabela de filmes para o banco de dados do filme.</span><span class="sxs-lookup"><span data-stu-id="85612-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="85612-137">Quando você atualiza o banco de dados nas instruções a seguir, isso *{DateStamp}\_Initial.cs* arquivo será executado e criar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="85612-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="85612-138">Em seguida, o **semente** método serão executadas para popular o banco de dados com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="85612-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="85612-139">No **Package Manager Console**, digite o comando "update-database" para criar o banco de dados e executar o **semente** método.</span><span class="sxs-lookup"><span data-stu-id="85612-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="85612-140">Se você receber um erro que indica uma tabela já existe e não pode ser criada, é provável que o aplicativo é executado depois que você excluir o banco de dados e antes da execução `update-database`.</span><span class="sxs-lookup"><span data-stu-id="85612-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="85612-141">Nesse caso, exclua o *Movies.mdf* novamente e repita a `update-database` comando.</span><span class="sxs-lookup"><span data-stu-id="85612-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="85612-142">Se você ainda receber um erro, exclua a pasta migrations e o conteúdo e começar com as instruções na parte superior desta página (que é excluir o *Movies.mdf* de arquivo e, depois, para habilitar migrações).</span><span class="sxs-lookup"><span data-stu-id="85612-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="85612-143">Execute o aplicativo e navegue até o */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="85612-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="85612-144">Os dados de semente são exibidos.</span><span class="sxs-lookup"><span data-stu-id="85612-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="85612-145">Adicionando uma propriedade de classificação ao modelo de filme</span><span class="sxs-lookup"><span data-stu-id="85612-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="85612-146">Comece adicionando um novo `Rating` propriedade existente `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="85612-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="85612-147">Abra o *Models\Movie.cs* e adicione o `Rating` propriedade como esta:</span><span class="sxs-lookup"><span data-stu-id="85612-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="85612-148">Completo `Movie` classe agora parece com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="85612-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="85612-149">Compile o aplicativo usando o **criar** &gt; **criar filme** menu de comando ou pressionando CTRL-SHIFT-B.</span><span class="sxs-lookup"><span data-stu-id="85612-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="85612-150">Agora que você atualizou o `Model` classe, você também precisa atualizar o *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* exibir modelos para exibir o novo `Rating`propriedade na exibição do navegador.</span><span class="sxs-lookup"><span data-stu-id="85612-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="85612-151">Abra o<em>\Views\Movies\Index.cshtml</em> e adicione um `<th>Rating</th>` cabeçalho da coluna logo após o <strong>preço</strong> coluna.</span><span class="sxs-lookup"><span data-stu-id="85612-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="85612-152">Em seguida, adicione um `<td>` coluna próximo ao final do modelo para renderizar o `@item.Rating` valor.</span><span class="sxs-lookup"><span data-stu-id="85612-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="85612-153">Abaixo está o que a atualização <em>cshtml</em> aparência do modelo de exibição:</span><span class="sxs-lookup"><span data-stu-id="85612-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="85612-154">Em seguida, abra o *\Views\Movies\Create.cshtml* de arquivo e adicione a seguinte marcação próximo ao final do formulário.</span><span class="sxs-lookup"><span data-stu-id="85612-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="85612-155">Isso apresenta uma caixa de texto para que você pode especificar uma classificação quando um filme novo é criado.</span><span class="sxs-lookup"><span data-stu-id="85612-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="85612-156">Agora que você atualizou o código do aplicativo para dar suporte aos novos `Rating` propriedade.</span><span class="sxs-lookup"><span data-stu-id="85612-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="85612-157">Agora execute o aplicativo e navegue até o */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="85612-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="85612-158">Quando você fizer isso, no entanto, você verá um dos seguintes erros:</span><span class="sxs-lookup"><span data-stu-id="85612-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="85612-159">Você está vendo este erro, porque a atualização `Movie` classe de modelo no aplicativo agora é diferente do esquema do `Movie` tabela do banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="85612-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="85612-160">(Não há nenhuma coluna `Rating` na tabela de banco de dados.)</span><span class="sxs-lookup"><span data-stu-id="85612-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="85612-161">Existem algumas abordagens para resolver o erro:</span><span class="sxs-lookup"><span data-stu-id="85612-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="85612-162">Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="85612-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="85612-163">Essa abordagem é muito conveniente ao fazer o desenvolvimento ativo em um banco de dados de teste; Ele permite que você rapidamente desenvolver o esquema de banco de dados e modelo juntos.</span><span class="sxs-lookup"><span data-stu-id="85612-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="85612-164">No entanto, a desvantagem é que você perca os dados existentes no banco de dados — para que você *não* para usar essa abordagem em um banco de dados de produção!</span><span class="sxs-lookup"><span data-stu-id="85612-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="85612-165">Usando um inicializador para um banco de dados com dados de teste de propagação automaticamente geralmente é uma maneira produtiva ao desenvolver um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="85612-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="85612-166">Para obter mais informações sobre os inicializadores de banco de dados do Entity Framework, consulte de Tom Dykstra [tutorial do ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="85612-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="85612-167">Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="85612-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="85612-168">A vantagem dessa abordagem é que você mantém os dados.</span><span class="sxs-lookup"><span data-stu-id="85612-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="85612-169">Faça essa alteração manualmente ou criando um script de alteração de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="85612-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="85612-170">Use as Migrações do Code First para atualizar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="85612-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="85612-171">Para este tutorial, usaremos as Migrações do Code First.</span><span class="sxs-lookup"><span data-stu-id="85612-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="85612-172">Atualize o método de propagação para que ele forneça um valor para a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="85612-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="85612-173">Abra o arquivo Migrations\Configuration.cs e adicionar um campo de classificação para cada objeto de filme.</span><span class="sxs-lookup"><span data-stu-id="85612-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="85612-174">Compile a solução e, em seguida, abra o **Package Manager Console** janela e digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="85612-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="85612-175">O `add-migration` comando informa o framework de migração para examinar o modelo de filme atual com o esquema de banco de dados do filme atual e criar o código necessário para migrar o banco de dados para o novo modelo.</span><span class="sxs-lookup"><span data-stu-id="85612-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="85612-176">O AddRatingMig é arbitrário e é usado para nomear o arquivo de migração.</span><span class="sxs-lookup"><span data-stu-id="85612-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="85612-177">É útil usar um nome significativo para a etapa da migração.</span><span class="sxs-lookup"><span data-stu-id="85612-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="85612-178">Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define o novo `DbMIgration` derivado da classe e no `Up` método, você pode ver o código que cria a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="85612-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="85612-179">Compile a solução e, em seguida, digite o comando "update-database" o **Package Manager Console** janela.</span><span class="sxs-lookup"><span data-stu-id="85612-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="85612-180">A imagem a seguir mostra a saída de **Package Manager Console** janela (o carimbo de data acrescentando AddRatingMig será diferente.)</span><span class="sxs-lookup"><span data-stu-id="85612-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="85612-181">Execute novamente o aplicativo e navegue até a URL de /Movies.</span><span class="sxs-lookup"><span data-stu-id="85612-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="85612-182">Você pode ver o novo campo de classificação.</span><span class="sxs-lookup"><span data-stu-id="85612-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="85612-183">Clique o **criar novo** link para adicionar um novo filme.</span><span class="sxs-lookup"><span data-stu-id="85612-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="85612-184">Observe que você pode adicionar uma classificação.</span><span class="sxs-lookup"><span data-stu-id="85612-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="85612-186">Clique em **Criar**.</span><span class="sxs-lookup"><span data-stu-id="85612-186">Click **Create**.</span></span> <span data-ttu-id="85612-187">Novo filme, incluindo classificação, agora é exibido nos filmes listando:</span><span class="sxs-lookup"><span data-stu-id="85612-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="85612-189">Você também deve adicionar o `Rating` campo para a edição, detalhes e SearchIndex modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="85612-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="85612-190">Você poderia inserir o comando "update-database" o **Package Manager Console** janela novamente e nenhuma alteração teriam sido feitas, porque o esquema corresponde ao modelo.</span><span class="sxs-lookup"><span data-stu-id="85612-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="85612-191">Nesta seção, você viu como você pode modificar objetos de modelo e manter o banco de dados em sincronia com as alterações.</span><span class="sxs-lookup"><span data-stu-id="85612-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="85612-192">Você também aprendeu uma maneira para popular um banco de dados recém-criado com dados de exemplo para que você pode experimentar os cenários.</span><span class="sxs-lookup"><span data-stu-id="85612-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="85612-193">Em seguida, vamos dar uma olhada em como você pode adicionar lógica de validação mais rica para as classes de modelo e habilitar algumas regras de negócio a serem aplicadas.</span><span class="sxs-lookup"><span data-stu-id="85612-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="85612-194">[Anterior](examining-the-edit-methods-and-edit-view.md)
> [Próximo](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="85612-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
