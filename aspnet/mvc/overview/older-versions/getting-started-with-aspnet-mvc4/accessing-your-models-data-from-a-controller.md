---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Acessando dados do seu modelo de um controlador | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. Ele é mais seguro e muito mais simples a seguir e demonstração...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf1d27088c1e65d55a6820825eebe63f7fdcb515
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804931"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="917db-104">Acessando dados do seu modelo de um controlador</span><span class="sxs-lookup"><span data-stu-id="917db-104">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="917db-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="917db-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="917db-106">Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="917db-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="917db-107">É mais seguro e muito mais simples a seguir e apresenta mais recursos.</span><span class="sxs-lookup"><span data-stu-id="917db-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="917db-108">Nesta seção, você criará um novo `MoviesController` de classe e escrever um código que recupera os dados do filme e o exibe no navegador usando um modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="917db-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="917db-109">**Compilar o aplicativo** antes de ir para a próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="917db-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="917db-110">Clique com botão direito do *controladores* pasta e crie um novo `MoviesController` controlador.</span><span class="sxs-lookup"><span data-stu-id="917db-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="917db-111">As opções a seguir não aparecerá até que você criar seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="917db-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="917db-112">Selecione as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="917db-112">Select the following options:</span></span>

- <span data-ttu-id="917db-113">Nome do controlador: **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="917db-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="917db-114">(Esse é o padrão.</span><span class="sxs-lookup"><span data-stu-id="917db-114">(This is the default.</span></span> <span data-ttu-id="917db-115">)</span><span class="sxs-lookup"><span data-stu-id="917db-115">)</span></span>
- <span data-ttu-id="917db-116">Modelo: **controlador MVC com ações de leitura/gravação e modos de exibição, usando o Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="917db-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="917db-117">Classe de modelo: **Movie (mvcmovie. Models)**.</span><span class="sxs-lookup"><span data-stu-id="917db-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="917db-118">Classe de contexto de dados: **MovieDBContext (mvcmovie. Models)**.</span><span class="sxs-lookup"><span data-stu-id="917db-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="917db-119">Modos de exibição: **Razor (CSHTML)**.</span><span class="sxs-lookup"><span data-stu-id="917db-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="917db-120">(O padrão).</span><span class="sxs-lookup"><span data-stu-id="917db-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="917db-122">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="917db-122">Click **Add**.</span></span> <span data-ttu-id="917db-123">Visual Studio Express cria os seguintes arquivos e pastas:</span><span class="sxs-lookup"><span data-stu-id="917db-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="917db-124">*Um MoviesController.cs* arquivo do projeto *controladores* pasta.</span><span class="sxs-lookup"><span data-stu-id="917db-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="917db-125">Um *filmes* pasta do projeto *modos de exibição* pasta.</span><span class="sxs-lookup"><span data-stu-id="917db-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="917db-126">*Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*, e *index. cshtml* no novo *exibições \ filmes* pasta.</span><span class="sxs-lookup"><span data-stu-id="917db-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="917db-127">ASP.NET MVC 4 criado automaticamente o CRUD (criar, ler, atualizar e excluir) os métodos de ação e modos de exibição para você (a criação automática de métodos de ação CRUD e exibições é conhecida como scaffolding).</span><span class="sxs-lookup"><span data-stu-id="917db-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="917db-128">Agora você tem um aplicativo web totalmente funcional que permite que você criar, listar, editar e excluir entradas de filme.</span><span class="sxs-lookup"><span data-stu-id="917db-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="917db-129">Execute o aplicativo e navegue até a `Movies` controlador por meio do acréscimo */Movies* para a URL na barra de endereços do navegador.</span><span class="sxs-lookup"><span data-stu-id="917db-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="917db-130">Porque o aplicativo é contar com o roteamento padrão (definido na *global. asax* arquivo), a solicitação do navegador `http://localhost:xxxxx/Movies` é roteado para o padrão `Index` método de ação do `Movies` controlador.</span><span class="sxs-lookup"><span data-stu-id="917db-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="917db-131">Em outras palavras, a solicitação do navegador `http://localhost:xxxxx/Movies` é praticamente o mesmo que a solicitação do navegador `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="917db-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="917db-132">O resultado é uma lista vazia de filmes, porque você não adicionou nenhum.</span><span class="sxs-lookup"><span data-stu-id="917db-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="917db-133">Criação de um filme</span><span class="sxs-lookup"><span data-stu-id="917db-133">Creating a Movie</span></span>

<span data-ttu-id="917db-134">Selecione o link **Criar Novo**.</span><span class="sxs-lookup"><span data-stu-id="917db-134">Select the **Create New** link.</span></span> <span data-ttu-id="917db-135">Insira alguns detalhes sobre um filme e, em seguida, clique no **criar** botão.</span><span class="sxs-lookup"><span data-stu-id="917db-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="917db-136">Clicar a **criar** botão faz com que o formulário seja enviado ao servidor, onde as informações do filme é salvo no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="917db-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="917db-137">Em seguida, você será redirecionado para o */Movies* URL, onde você pode ver o filme recém-criado na lista.</span><span class="sxs-lookup"><span data-stu-id="917db-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="917db-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="917db-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="917db-139">Crie duas mais entradas de filme adicionais.</span><span class="sxs-lookup"><span data-stu-id="917db-139">Create a couple more movie entries.</span></span> <span data-ttu-id="917db-140">Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.</span><span class="sxs-lookup"><span data-stu-id="917db-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="917db-141">Examinando o código gerado</span><span class="sxs-lookup"><span data-stu-id="917db-141">Examining the Generated Code</span></span>

<span data-ttu-id="917db-142">Abra o *Controllers\MoviesController.cs* do arquivo e examine o gerado `Index` método.</span><span class="sxs-lookup"><span data-stu-id="917db-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="917db-143">Uma parte do controlador de filmes com o `Index` método é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="917db-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="917db-144">A seguinte linha do `MoviesController` classe cria uma instância de um contexto de banco de dados de filme, conforme descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="917db-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="917db-145">Você pode usar o contexto de banco de dados de filme para consultar, editar e excluir filmes.</span><span class="sxs-lookup"><span data-stu-id="917db-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="917db-146">Uma solicitação para o `Movies` controlador retorna todas as entradas na `Movies` tabela do banco de dados do filme e, em seguida, passa os resultados para o `Index` modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="917db-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="917db-147">Modelos fortemente tipados e a @model palavra-chave</span><span class="sxs-lookup"><span data-stu-id="917db-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="917db-148">Neste tutorial, você viu como um controlador pode passar dados ou objetos para um modelo de exibição usando o `ViewBag` objeto.</span><span class="sxs-lookup"><span data-stu-id="917db-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="917db-149">O `ViewBag` é um objeto dinâmico que fornece uma maneira conveniente de associação tardia para passar informações para um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="917db-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="917db-150">ASP.NET MVC também fornece a capacidade de passar fortemente tipados dados ou objetos para um modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="917db-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="917db-151">Isso fortemente tipado a abordagem permite um melhor tempo de compilação de seu código e o IntelliSense mais sofisticado no editor do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="917db-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="917db-152">O mecanismo de scaffolding no Visual Studio usou essa abordagem com o `MoviesController` modelos de classe e o modo de exibição quando ele criou os métodos e os modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="917db-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="917db-153">No *Controllers\MoviesController.cs* arquivo examinar gerado `Details` método.</span><span class="sxs-lookup"><span data-stu-id="917db-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="917db-154">Uma parte do controlador de filmes com o `Details` método é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="917db-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="917db-155">Se um `Movie` for encontrado, uma instância da `Movie` modelo é passado para o modo de exibição de detalhes.</span><span class="sxs-lookup"><span data-stu-id="917db-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="917db-156">Examinar o conteúdo do *Views\Movies\Details.cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="917db-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="917db-157">Ao incluir um `@model` instrução na parte superior do arquivo de modelo de exibição, você pode especificar o tipo de objeto que espera que o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="917db-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="917db-158">Quando você criou o controlador de filmes, o Visual Studio incluiu automaticamente a seguinte instrução `@model` na parte superior do arquivo *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="917db-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="917db-159">Esta diretiva `@model` permite acessar o filme que o controlador passou para a exibição usando um objeto `Model` fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="917db-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="917db-160">Por exemplo, nos *details. cshtml* modelo, o código passa cada campo de filme para o `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) auxiliares HTML com rigidez de tipos `Model` objeto.</span><span class="sxs-lookup"><span data-stu-id="917db-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="917db-161">Os modelos de exibição e criar e editar métodos também passam um objeto de modelo de filme.</span><span class="sxs-lookup"><span data-stu-id="917db-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="917db-162">Examine os *index. cshtml* modelo de exibição e o `Index` método no *MoviesController.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="917db-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="917db-163">Observe como o código cria uma [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto quando ele chama o `View` método auxiliar no `Index` método de ação.</span><span class="sxs-lookup"><span data-stu-id="917db-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="917db-164">Em seguida, o código passa esta `Movies` lista do controlador para o modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="917db-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="917db-165">Quando você criou o controlador movie, Visual Studio Express incluiu automaticamente a seguinte `@model` instrução na parte superior de *index. cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="917db-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="917db-166">Isso `@model` diretiva permite que você acesse a lista de filmes que o controlador passou para o modo de exibição, usando um `Model` objeto fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="917db-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="917db-167">Por exemplo, nos *index. cshtml* modelo, o código percorre os filmes fazendo uma `foreach` instrução fortemente tipado `Model` objeto:</span><span class="sxs-lookup"><span data-stu-id="917db-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="917db-168">Porque o `Model` objeto é fortemente tipado (como uma `IEnumerable<Movie>` objeto), cada `item` objeto no loop é tipado como `Movie`.</span><span class="sxs-lookup"><span data-stu-id="917db-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="917db-169">Entre outros benefícios, isso significa que você obtenha a verificação de tempo de compilação do código e total suporte IntelliSense no editor de código:</span><span class="sxs-lookup"><span data-stu-id="917db-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="917db-171">Trabalhando com o SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="917db-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="917db-172">Entity Framework Code First detectou que a cadeia de caracteres de conexão de banco de dados que foi fornecida apontado para um `Movies` banco de dados que ainda não existia, portanto, o código primeiro criou o banco de dados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="917db-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="917db-173">Você pode verificar se ele é foi criado examinando os *App\_dados* pasta.</span><span class="sxs-lookup"><span data-stu-id="917db-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="917db-174">Se você não vir as *Movies.mdf* arquivo, clique no **Mostrar todos os arquivos** botão no **Gerenciador de soluções** barra de ferramentas, clique no **atualizar** botão e, em seguida, expanda o *App\_dados* pasta.</span><span class="sxs-lookup"><span data-stu-id="917db-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="917db-175">Clique duas vezes em *Movies.mdf* para abrir **DATABASE EXPLORER**, em seguida, expanda o **tabelas** pasta para ver a tabela de filmes.</span><span class="sxs-lookup"><span data-stu-id="917db-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="917db-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="917db-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="917db-177">Se o Gerenciador de banco de dados não aparecer, do **ferramentas** menu, selecione **conectar-se ao banco de dados**, cancelar, em seguida, o **Escolher fonte de dados** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="917db-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="917db-178">Isso forçará abrir o Gerenciador de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="917db-178">This will force open the database explorer.</span></span>


> [!NOTE]
> <span data-ttu-id="917db-179">Se você estiver usando VWD ou o Visual Studio 2010 e receber um erro semelhante a qualquer um dos seguintes procedimentos:</span><span class="sxs-lookup"><span data-stu-id="917db-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="917db-180">O banco de dados ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. Arquivos MDF' não pode ser aberto porque sua versão 706 é.</span><span class="sxs-lookup"><span data-stu-id="917db-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="917db-181">Este servidor suporta a versão 655 e versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="917db-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="917db-182">Não há suporte para um caminho de downgrade.</span><span class="sxs-lookup"><span data-stu-id="917db-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="917db-183">&quot;Exceção InvalidOperation não foi tratada pelo código do usuário&quot; o SqlConnection fornecido não especifica um catálogo inicial.</span><span class="sxs-lookup"><span data-stu-id="917db-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="917db-184">Você precisa instalar o [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) e [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span><span class="sxs-lookup"><span data-stu-id="917db-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="917db-185">Verifique se o `MovieDBContext` cadeia de caracteres de conexão especificada na página anterior.</span><span class="sxs-lookup"><span data-stu-id="917db-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>


<span data-ttu-id="917db-186">Clique com botão direito do `Movies` de tabela e selecione **Mostrar dados da tabela** para ver os dados que você criou.</span><span class="sxs-lookup"><span data-stu-id="917db-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="917db-187">Clique com botão direito do `Movies` de tabela e selecione **abrir definição de tabela** para ver a tabela de estrutura que o Entity Framework Code First criado para você.</span><span class="sxs-lookup"><span data-stu-id="917db-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

<span data-ttu-id="917db-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span><span class="sxs-lookup"><span data-stu-id="917db-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span></span>

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="917db-189">Observe como o esquema do `Movies` tabela mapeia para o `Movie` classe que você criou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="917db-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="917db-190">Entity Framework Code First criado automaticamente esse esquema para você com base em seu `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="917db-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="917db-191">Quando tiver terminado, feche a conexão com o botão direito clicando *MovieDBContext* e selecionando **fechar Conexão**.</span><span class="sxs-lookup"><span data-stu-id="917db-191">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="917db-192">(Se você não fechar a conexão, você poderá receber um erro na próxima vez que você executar o projeto).</span><span class="sxs-lookup"><span data-stu-id="917db-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="917db-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="917db-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span></span>

<span data-ttu-id="917db-194">Agora você tem o banco de dados e uma página de lista simples para exibir o conteúdo dele.</span><span class="sxs-lookup"><span data-stu-id="917db-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="917db-195">No próximo tutorial, vamos examinar o restante do código gerado por scaffolding e adicionar um `SearchIndex` método e um `SearchIndex` modo de exibição que permite pesquisar filmes neste banco de dados.</span><span class="sxs-lookup"><span data-stu-id="917db-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="917db-196">[Anterior](adding-a-model.md)
> [Próximo](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="917db-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
