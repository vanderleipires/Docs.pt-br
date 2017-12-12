---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Acessando dados do modelo de um controlador | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b60913cef4b62745cf167e6074834bf7d0c228d1
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2017
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="fb352-102">Acessando dados do modelo de um controlador</span><span class="sxs-lookup"><span data-stu-id="fb352-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="fb352-103">Por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="fb352-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="fb352-104">Nesta seção, você criará um novo `MoviesController` classe e gravar o código que recupera os dados do filme e o exibe no navegador usando um modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fb352-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="fb352-105">**Compile o aplicativo** antes de ir para a próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="fb352-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="fb352-106">Se você não criar o aplicativo, você obterá um erro ao adicionar um controlador.</span><span class="sxs-lookup"><span data-stu-id="fb352-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="fb352-107">No Gerenciador de soluções, clique o *controladores* pasta e clique **adicionar**, em seguida, **controlador**.</span><span class="sxs-lookup"><span data-stu-id="fb352-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="fb352-108">No **adicionar Scaffold** caixa de diálogo, clique em **controlador MVC 5 com modos de exibição usando o Entity Framework**e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="fb352-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="fb352-109">Selecione **filme (MvcMovie.Models)** para a classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="fb352-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="fb352-110">Selecione **MovieDBContext (MvcMovie.Models)** para a classe de contexto de dados.</span><span class="sxs-lookup"><span data-stu-id="fb352-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="fb352-111">Para o nome do controlador, digite **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="fb352-111">For the Controller name enter **MoviesController**.</span></span>

 <span data-ttu-id="fb352-112">A imagem abaixo mostra a caixa de diálogo concluída.</span><span class="sxs-lookup"><span data-stu-id="fb352-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="fb352-113">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="fb352-113">Click **Add**.</span></span> <span data-ttu-id="fb352-114">(Se você receber um erro, você provavelmente não compilar o aplicativo antes de iniciar a adição do controlador.) O Visual Studio cria os seguintes arquivos e pastas:</span><span class="sxs-lookup"><span data-stu-id="fb352-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="fb352-115">*Um MoviesController.cs* arquivo o *controladores* pasta.</span><span class="sxs-lookup"><span data-stu-id="fb352-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="fb352-116">Um *Views\Movies* pasta.</span><span class="sxs-lookup"><span data-stu-id="fb352-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="fb352-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, e *cshtml* no novo *Views\Movies* pasta.</span><span class="sxs-lookup"><span data-stu-id="fb352-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="fb352-118">Visual Studio criado automaticamente o [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (criar, ler, atualizar e excluir) métodos de ação e o modo de exibição (a criação automática de métodos de ação CRUD e modos de exibição é conhecida como scaffolding).</span><span class="sxs-lookup"><span data-stu-id="fb352-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="fb352-119">Agora você tem um aplicativo web totalmente funcional que lhe permite criar, listar, editar e excluir entradas do filme.</span><span class="sxs-lookup"><span data-stu-id="fb352-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="fb352-120">Execute o aplicativo e clique no **MVC filme** link (ou navegue até o `Movies` controlador acrescentando */Movies* para a URL na barra de endereços do navegador).</span><span class="sxs-lookup"><span data-stu-id="fb352-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="fb352-121">Porque o aplicativo depender de roteamento padrão (definido no *aplicativo\_Start\RouteConfig.cs* arquivo), a solicitação do navegador `http://localhost:xxxxx/Movies` é roteada para o padrão `Index` método de ação do `Movies` controlador.</span><span class="sxs-lookup"><span data-stu-id="fb352-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="fb352-122">Em outras palavras, a solicitação do navegador `http://localhost:xxxxx/Movies` é praticamente o mesmo que a solicitação do navegador `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="fb352-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="fb352-123">O resultado é uma lista vazia de filmes, porque você não adicionou nenhum.</span><span class="sxs-lookup"><span data-stu-id="fb352-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="fb352-124">Criando um filme</span><span class="sxs-lookup"><span data-stu-id="fb352-124">Creating a Movie</span></span>

<span data-ttu-id="fb352-125">Selecione o link **Criar Novo**.</span><span class="sxs-lookup"><span data-stu-id="fb352-125">Select the **Create New** link.</span></span> <span data-ttu-id="fb352-126">Insira alguns detalhes sobre um filme e, em seguida, clique no **criar** botão.</span><span class="sxs-lookup"><span data-stu-id="fb352-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="fb352-127">Você não poderá inserir pontos decimais ou vírgulas no campo preço.</span><span class="sxs-lookup"><span data-stu-id="fb352-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="fb352-128">Para dar suporte a validação jQuery para idiomas diferentes do inglês que usam uma vírgula (&quot;,&quot;) para um ponto decimal e formatos de data diferente do inglês dos EUA, você deve incluir *globalize.js* e específicos  *cultures/globalize.cultures.js* arquivo (de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript para usar `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="fb352-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="fb352-129">Mostrarei como fazer isso no tutorial Avançar.</span><span class="sxs-lookup"><span data-stu-id="fb352-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="fb352-130">Por enquanto, insira apenas números inteiros como 10.</span><span class="sxs-lookup"><span data-stu-id="fb352-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="fb352-131">Clique o **criar** botão faz com que o formulário seja enviado para o servidor, onde as informações de filme é salvo no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fb352-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="fb352-132">Em seguida, você será redirecionado para a */Movies* URL, onde você pode ver o filme recém-criado na lista.</span><span class="sxs-lookup"><span data-stu-id="fb352-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="fb352-133">Crie duas mais entradas de filme adicionais.</span><span class="sxs-lookup"><span data-stu-id="fb352-133">Create a couple more movie entries.</span></span> <span data-ttu-id="fb352-134">Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.</span><span class="sxs-lookup"><span data-stu-id="fb352-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="fb352-135">Examinar o código gerado</span><span class="sxs-lookup"><span data-stu-id="fb352-135">Examining the Generated Code</span></span>

<span data-ttu-id="fb352-136">Abra o *Controllers\MoviesController.cs* de arquivo e examine o gerado `Index` método.</span><span class="sxs-lookup"><span data-stu-id="fb352-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="fb352-137">Uma parte do controlador de filme com o `Index` método é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="fb352-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="fb352-138">Uma solicitação para o `Movies` retorna todas as entradas de `Movies` de tabela e, em seguida, passa os resultados para o `Index` exibição.</span><span class="sxs-lookup"><span data-stu-id="fb352-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="fb352-139">A seguinte linha da `MoviesController` classe instancia um contexto de banco de dados do filme, conforme descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="fb352-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="fb352-140">Você pode usar o contexto de banco de dados do filme para consultar, editar e excluir filmes.</span><span class="sxs-lookup"><span data-stu-id="fb352-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="fb352-141">Fortemente tipado modelos e o @model palavra-chave</span><span class="sxs-lookup"><span data-stu-id="fb352-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="fb352-142">Anteriormente neste tutorial, você viu como um controlador pode passar dados ou objetos para um modelo de exibição usando o `ViewBag` objeto.</span><span class="sxs-lookup"><span data-stu-id="fb352-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="fb352-143">O `ViewBag` é um objeto dinâmico que fornece uma maneira conveniente de associação tardia para passar informações para um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fb352-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="fb352-144">MVC também fornece a capacidade de passar *fortemente* digitado objetos para um modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fb352-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="fb352-145">Essa abordagem com rigidez de tipos permite melhor tempo de compilação mais rico e verificação do seu código [IntelliSense](https://msdn.microsoft.com/en-us/library/hcw1s69b(v=vs.120).aspx) no editor do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fb352-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/en-us/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="fb352-146">O mecanismo de scaffolding no Visual Studio usado essa abordagem (ou seja, passando um *fortemente* com tipo de modelo) com o `MoviesController` modelos de classe e o modo de exibição quando criado os métodos e as exibições.</span><span class="sxs-lookup"><span data-stu-id="fb352-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="fb352-147">No *Controllers\MoviesController.cs* arquivo examinar gerado `Details` método.</span><span class="sxs-lookup"><span data-stu-id="fb352-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="fb352-148">O `Details` método é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="fb352-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="fb352-149">O `id` parâmetro geralmente é passado como dados de rota, por exemplo `http://localhost:1234/movies/details/1` definirá o controlador para o controlador de filme, a ação a ser `details` e `id` como 1.</span><span class="sxs-lookup"><span data-stu-id="fb352-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="fb352-150">Você também pode transmitir a identificação com uma cadeia de caracteres de consulta da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="fb352-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="fb352-151">Se um `Movie` for encontrado, uma instância do `Movie` modelo é passado para o `Details` exibição:</span><span class="sxs-lookup"><span data-stu-id="fb352-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="fb352-152">Examine o conteúdo do *Views\Movies\Details.cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="fb352-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="fb352-153">Incluindo um `@model` instrução na parte superior do arquivo do modelo de exibição, você pode especificar o tipo de objeto que espera que o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fb352-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="fb352-154">Quando você criou o controlador de filmes, o Visual Studio incluiu automaticamente a seguinte instrução `@model` na parte superior do arquivo *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fb352-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="fb352-155">Esta diretiva `@model` permite acessar o filme que o controlador passou para a exibição usando um objeto `Model` fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="fb352-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="fb352-156">Por exemplo, o *Details.cshtml* modelo, o código passa cada campo de filme o `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) auxiliares HTML com rigidez de tipos `Model` objeto.</span><span class="sxs-lookup"><span data-stu-id="fb352-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="fb352-157">O `Create` e `Edit` métodos e modelos de exibição também passam um objeto de modelo do filme.</span><span class="sxs-lookup"><span data-stu-id="fb352-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="fb352-158">Examine o *cshtml* modelo de exibição e o `Index` método o *MoviesController.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="fb352-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="fb352-159">Observe como o código cria um [ `List` ](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) objeto quando ele chama o `View` método auxiliar a `Index` método de ação.</span><span class="sxs-lookup"><span data-stu-id="fb352-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="fb352-160">O código, em seguida, passa essa `Movies` lista da `Index` método de ação para o modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="fb352-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="fb352-161">Quando você criou o controlador de filme, o Visual Studio incluídos automaticamente o seguinte `@model` instrução na parte superior de *cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="fb352-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="fb352-162">Isso `@model` diretiva permite que você acesse a lista de filmes que o controlador passado para o modo de exibição usando uma `Model` objeto que é fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="fb352-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="fb352-163">Por exemplo, no *cshtml* modelo, o código percorre os filmes fazendo uma `foreach` instrução sobre fortemente tipada `Model` objeto:</span><span class="sxs-lookup"><span data-stu-id="fb352-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="fb352-164">Porque o `Model` objeto é fortemente tipado (como um `IEnumerable<Movie>` objeto), cada `item` objeto no loop é digitado como `Movie`.</span><span class="sxs-lookup"><span data-stu-id="fb352-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="fb352-165">Entre outros benefícios, isso significa que você obtenha a verificação de tempo de compilação do código e total suporte IntelliSense no editor de código:</span><span class="sxs-lookup"><span data-stu-id="fb352-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="fb352-167">Trabalhando com o SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="fb352-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="fb352-168">Entity Framework Code First detectou que a cadeia de caracteres de conexão de banco de dados que foi fornecida apontado para um `Movies` banco de dados que não existe, portanto Code First criada a banco de dados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="fb352-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="fb352-169">Você pode verificar se ela é criada examinando o *aplicativo\_dados* pasta.</span><span class="sxs-lookup"><span data-stu-id="fb352-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="fb352-170">Se você não vir o *Movies.mdf* de arquivo, clique no **Mostrar todos os arquivos** no botão de **Gerenciador de soluções** barra de ferramentas, clique no **atualizar** botão e, em seguida, expanda o *aplicativo\_dados* pasta.</span><span class="sxs-lookup"><span data-stu-id="fb352-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="fb352-171">Clique duas vezes em *Movies.mdf* abrir **SERVER EXPLORER**, em seguida, expanda o **tabelas** pasta para ver a tabela de filmes.</span><span class="sxs-lookup"><span data-stu-id="fb352-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="fb352-172">Observe o ícone de chave ao lado de ID.</span><span class="sxs-lookup"><span data-stu-id="fb352-172">Note the key icon next to ID.</span></span> <span data-ttu-id="fb352-173">Por padrão, o EF fará uma propriedade chamada ID, a chave primária.</span><span class="sxs-lookup"><span data-stu-id="fb352-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="fb352-174">Para obter mais informações sobre o EF e MVC, consulte o tutorial excelente de Tom Dykstra em [MVC e EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="fb352-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="fb352-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="fb352-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="fb352-176">Clique com botão direito do `Movies` de tabela e selecione **Mostrar dados da tabela** para ver os dados que você criou.</span><span class="sxs-lookup"><span data-stu-id="fb352-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="fb352-177">Clique com botão direito do `Movies` de tabela e selecione **abrir definição de tabela** para ver a tabela de estrutura que Entity Framework Code First criado para você.</span><span class="sxs-lookup"><span data-stu-id="fb352-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="fb352-178">Observe como o esquema do `Movies` tabela mapeia para o `Movie` classe que você criou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="fb352-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="fb352-179">Entity Framework Code First criado automaticamente esse esquema para você com base na sua `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="fb352-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="fb352-180">Quando tiver terminado, feche a conexão com um clique direito *MovieDBContext* e selecionando **fechar Conexão**.</span><span class="sxs-lookup"><span data-stu-id="fb352-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="fb352-181">(Se você não fechar a conexão, você poderá receber um erro na próxima vez que você executar o projeto).</span><span class="sxs-lookup"><span data-stu-id="fb352-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="fb352-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="fb352-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="fb352-183">Agora você tem um banco de dados e páginas para exibir, editar, atualizar e excluir dados.</span><span class="sxs-lookup"><span data-stu-id="fb352-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="fb352-184">O seguinte tutorial, vamos examinar o restante do código scaffolding e adicionar um `SearchIndex` método e uma `SearchIndex` modo de exibição que permite que você pesquise filmes neste banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fb352-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="fb352-185">Para obter mais informações sobre como usar o Entity Framework com MVC, consulte [criando um modelo de dados do Entity Framework para um aplicativo ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="fb352-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fb352-186">[Anterior](creating-a-connection-string.md)
[Próximo](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="fb352-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
