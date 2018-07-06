---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Criar uma API REST com roteamento de atributo na API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 892d353ded17136825fda05b671eab4195b44b07
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819053"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="1af71-102">Criar uma API REST com roteamento de atributo na API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="1af71-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="1af71-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1af71-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1af71-104">API Web 2 dá suporte a um novo tipo de roteamento, chamado *roteamento de atributo*.</span><span class="sxs-lookup"><span data-stu-id="1af71-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="1af71-105">Para obter uma visão geral de roteamento de atributo, consulte [roteamento de atributo na API Web 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="1af71-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="1af71-106">Neste tutorial, você usará o roteamento de atributo para criar uma API REST para uma coleção de livros.</span><span class="sxs-lookup"><span data-stu-id="1af71-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="1af71-107">A API é compatível com as seguintes ações:</span><span class="sxs-lookup"><span data-stu-id="1af71-107">The API will support the following actions:</span></span>

| <span data-ttu-id="1af71-108">Ação</span><span class="sxs-lookup"><span data-stu-id="1af71-108">Action</span></span> | <span data-ttu-id="1af71-109">Exemplo de URI</span><span class="sxs-lookup"><span data-stu-id="1af71-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="1af71-110">Obtenha uma lista de todos os livros.</span><span class="sxs-lookup"><span data-stu-id="1af71-110">Get a list of all books.</span></span> | <span data-ttu-id="1af71-111">/ api/manuais</span><span class="sxs-lookup"><span data-stu-id="1af71-111">/api/books</span></span> |
| <span data-ttu-id="1af71-112">Obtenha um livro por ID.</span><span class="sxs-lookup"><span data-stu-id="1af71-112">Get a book by ID.</span></span> | <span data-ttu-id="1af71-113">/API/Books/1</span><span class="sxs-lookup"><span data-stu-id="1af71-113">/api/books/1</span></span> |
| <span data-ttu-id="1af71-114">Obtenha os detalhes de um livro.</span><span class="sxs-lookup"><span data-stu-id="1af71-114">Get the details of a book.</span></span> | <span data-ttu-id="1af71-115">/API/Books/1/Details</span><span class="sxs-lookup"><span data-stu-id="1af71-115">/api/books/1/details</span></span> |
| <span data-ttu-id="1af71-116">Obtenha uma lista de livros por gênero.</span><span class="sxs-lookup"><span data-stu-id="1af71-116">Get a list of books by genre.</span></span> | <span data-ttu-id="1af71-117">/API/Books/fantasy</span><span class="sxs-lookup"><span data-stu-id="1af71-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="1af71-118">Obter uma lista de livros por data de publicação.</span><span class="sxs-lookup"><span data-stu-id="1af71-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="1af71-119">/API/Books/Date/2013-02-16 /api/books/date/2013/02/16 (formulário alternativo)</span><span class="sxs-lookup"><span data-stu-id="1af71-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="1af71-120">Obtenha uma lista de livros publicados por um autor específico.</span><span class="sxs-lookup"><span data-stu-id="1af71-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="1af71-121">/API/Authors/1/Books</span><span class="sxs-lookup"><span data-stu-id="1af71-121">/api/authors/1/books</span></span> |

<span data-ttu-id="1af71-122">Todos os métodos são somente leitura (solicitações HTTP GET).</span><span class="sxs-lookup"><span data-stu-id="1af71-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="1af71-123">Para a camada de dados, vamos usar Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1af71-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="1af71-124">Registros de livro terão os seguintes campos:</span><span class="sxs-lookup"><span data-stu-id="1af71-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="1af71-125">ID</span><span class="sxs-lookup"><span data-stu-id="1af71-125">ID</span></span>
- <span data-ttu-id="1af71-126">Título</span><span class="sxs-lookup"><span data-stu-id="1af71-126">Title</span></span>
- <span data-ttu-id="1af71-127">Gênero</span><span class="sxs-lookup"><span data-stu-id="1af71-127">Genre</span></span>
- <span data-ttu-id="1af71-128">Data de publicação</span><span class="sxs-lookup"><span data-stu-id="1af71-128">Publication date</span></span>
- <span data-ttu-id="1af71-129">Preço</span><span class="sxs-lookup"><span data-stu-id="1af71-129">Price</span></span>
- <span data-ttu-id="1af71-130">Descrição</span><span class="sxs-lookup"><span data-stu-id="1af71-130">Description</span></span>
- <span data-ttu-id="1af71-131">AuthorID (chave estrangeira em uma tabela de autores)</span><span class="sxs-lookup"><span data-stu-id="1af71-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="1af71-132">Para a maioria das solicitações, no entanto, a API retornará um subconjunto desses dados (título, autor e gênero).</span><span class="sxs-lookup"><span data-stu-id="1af71-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="1af71-133">Para obter o registro completo, o cliente solicitações `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="1af71-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1af71-134">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="1af71-134">Prerequisites</span></span>

<span data-ttu-id="1af71-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional ou Enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="1af71-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="1af71-136">Criar o projeto do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1af71-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="1af71-137">Comece executando o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1af71-137">Start by running Visual Studio.</span></span> <span data-ttu-id="1af71-138">Dos **arquivo** menu, selecione **New** e, em seguida, selecione **projeto**.</span><span class="sxs-lookup"><span data-stu-id="1af71-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="1af71-139">No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="1af71-139">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="1af71-140">Em **Visual C#**, selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="1af71-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="1af71-141">Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="1af71-141">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="1af71-142">Nomeie o projeto &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="1af71-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="1af71-143">Na caixa de diálogo **Novo Aplicativo Web ASP.NET**, selecione o modelo **Vazio**.</span><span class="sxs-lookup"><span data-stu-id="1af71-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="1af71-144">Em "Adicionar pastas e core references for", selecione a **API Web** caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="1af71-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="1af71-145">Clique em **criar projeto**.</span><span class="sxs-lookup"><span data-stu-id="1af71-145">Click **Create Project**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="1af71-146">Isso cria um projeto de esqueleto está configurado para a funcionalidade da API da Web.</span><span class="sxs-lookup"><span data-stu-id="1af71-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="1af71-147">Modelos de domínio</span><span class="sxs-lookup"><span data-stu-id="1af71-147">Domain Models</span></span>

<span data-ttu-id="1af71-148">Em seguida, adicione classes para modelos de domínio.</span><span class="sxs-lookup"><span data-stu-id="1af71-148">Next, add classes for domain models.</span></span> <span data-ttu-id="1af71-149">No Gerenciador de Soluções, clique com o botão direito na pasta de modelos (Models).</span><span class="sxs-lookup"><span data-stu-id="1af71-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="1af71-150">Selecione **Add**, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="1af71-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="1af71-151">Nomeie a classe `Author`.</span><span class="sxs-lookup"><span data-stu-id="1af71-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="1af71-152">Substitua o código no Author.cs com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1af71-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="1af71-153">Agora, adicione outra classe denominada `Book`.</span><span class="sxs-lookup"><span data-stu-id="1af71-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="1af71-154">Adicionar um controlador de API da Web</span><span class="sxs-lookup"><span data-stu-id="1af71-154">Add a Web API Controller</span></span>

<span data-ttu-id="1af71-155">Nesta etapa, vamos adicionar um controlador de API da Web que usa o Entity Framework como a camada de dados.</span><span class="sxs-lookup"><span data-stu-id="1af71-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="1af71-156">Pressione CTRL+SHIFT+B para criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="1af71-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="1af71-157">Entity Framework usa a reflexão para descobrir as propriedades dos modelos, portanto, ele exige um assembly compilado criar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1af71-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="1af71-158">No Gerenciador de soluções, clique com botão direito na pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="1af71-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="1af71-159">Selecione **Add**, em seguida, selecione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="1af71-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="1af71-160">No **adicionar Scaffold** caixa de diálogo, selecione "Web API 2 Controller com ações de leitura/gravação, usando o Entity Framework."</span><span class="sxs-lookup"><span data-stu-id="1af71-160">In the **Add Scaffold** dialog, select "Web API 2 Controller with read/write actions, using Entity Framework."</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="1af71-161">No **Adicionar controlador** caixa de diálogo, para **nome do controlador**, insira &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="1af71-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="1af71-162">Selecione o &quot;usar ações do controlador assíncrono&quot; caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="1af71-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="1af71-163">Para **classe de modelo**, selecione &quot;livro&quot;.</span><span class="sxs-lookup"><span data-stu-id="1af71-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="1af71-164">(Se você não vir o `Book` classe listada na lista suspensa, certifique-se de que você compilou o projeto.) Em seguida, clique no botão "+".</span><span class="sxs-lookup"><span data-stu-id="1af71-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="1af71-165">Clique em **Add** na **novo contexto de dados** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="1af71-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="1af71-166">Clique em **Add** na **Adicionar controlador** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="1af71-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="1af71-167">O scaffolding adiciona uma classe chamada `BooksController` que define o controlador da API.</span><span class="sxs-lookup"><span data-stu-id="1af71-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="1af71-168">Ele também adiciona uma classe chamada `BooksAPIContext` na pasta modelos, que define o contexto de dados para o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1af71-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="1af71-169">Propagar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="1af71-169">Seed the Database</span></span>

<span data-ttu-id="1af71-170">No menu Ferramentas, selecione **Gerenciador de pacotes de biblioteca**e, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="1af71-170">From the Tools menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="1af71-171">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="1af71-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="1af71-172">Este comando cria uma pasta migrações e adiciona um novo arquivo de código chamado Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="1af71-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="1af71-173">Abra esse arquivo e adicione o seguinte código para o `Configuration.Seed` método.</span><span class="sxs-lookup"><span data-stu-id="1af71-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="1af71-174">Na janela do Console do Gerenciador de pacotes, digite os comandos a seguir.</span><span class="sxs-lookup"><span data-stu-id="1af71-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="1af71-175">Esses comandos criam um banco de dados local e invocar o método de semente para preencher o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1af71-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="1af71-176">Adicionar Classes DTO</span><span class="sxs-lookup"><span data-stu-id="1af71-176">Add DTO Classes</span></span>

<span data-ttu-id="1af71-177">Se você executa o aplicativo agora e envia uma solicitação GET para /api/books/1, a resposta é semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="1af71-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="1af71-178">(Eu adicionei o recuo para facilitar a leitura).</span><span class="sxs-lookup"><span data-stu-id="1af71-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="1af71-179">Em vez disso, quero que esta solicitação para retornar um subconjunto dos campos.</span><span class="sxs-lookup"><span data-stu-id="1af71-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="1af71-180">Além disso, quero que ela retorne o nome do autor, em vez da ID do autor.</span><span class="sxs-lookup"><span data-stu-id="1af71-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="1af71-181">Para fazer isso, vamos modificar os métodos do controlador para retornar um *objeto de transferência de dados* (DTO) em vez do modelo do EF.</span><span class="sxs-lookup"><span data-stu-id="1af71-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="1af71-182">Um DTO é um objeto que foi criado apenas para transportar dados.</span><span class="sxs-lookup"><span data-stu-id="1af71-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="1af71-183">No Gerenciador de soluções, clique com botão direito no projeto e selecione **Add** | **nova pasta**.</span><span class="sxs-lookup"><span data-stu-id="1af71-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="1af71-184">Nomeie a pasta &quot;DTOs&quot;.</span><span class="sxs-lookup"><span data-stu-id="1af71-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="1af71-185">Adicione uma classe chamada `BookDto` para a pasta de DTOs, com a seguinte definição:</span><span class="sxs-lookup"><span data-stu-id="1af71-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="1af71-186">Adicione outra classe denominada `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="1af71-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="1af71-187">Em seguida, atualize o `BooksController` classe para retornar `BookDto` instâncias.</span><span class="sxs-lookup"><span data-stu-id="1af71-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="1af71-188">Vamos usar o [Queryable](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) método ao projeto `Book` para instâncias `BookDto` instâncias.</span><span class="sxs-lookup"><span data-stu-id="1af71-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="1af71-189">Aqui está o código atualizado para a classe de controlador.</span><span class="sxs-lookup"><span data-stu-id="1af71-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="1af71-190">Exclui o `PutBook`, `PostBook`, e `DeleteBook` métodos, porque eles não são necessários para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="1af71-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="1af71-191">Agora, se você executa o aplicativo e solicitar /api/books/1, o corpo da resposta deve ser assim:</span><span class="sxs-lookup"><span data-stu-id="1af71-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="1af71-192">Adicionar atributos de rota</span><span class="sxs-lookup"><span data-stu-id="1af71-192">Add Route Attributes</span></span>

<span data-ttu-id="1af71-193">Em seguida, converteremos o controlador para usar o roteamento de atributo.</span><span class="sxs-lookup"><span data-stu-id="1af71-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="1af71-194">Primeiro, adicione uma **RoutePrefix** de atributo para o controlador.</span><span class="sxs-lookup"><span data-stu-id="1af71-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="1af71-195">Este atributo define os segmentos URI inicias para todos os métodos neste controlador.</span><span class="sxs-lookup"><span data-stu-id="1af71-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="1af71-196">Em seguida, adicione **[Route]** atributos para as ações do controlador, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="1af71-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="1af71-197">O modelo de rota para cada método de controlador é o prefixo mais a cadeia de caracteres especificada na **rota** atributo.</span><span class="sxs-lookup"><span data-stu-id="1af71-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="1af71-198">Para o `GetBook` a cadeia de caracteres com parâmetros inclui o método, o modelo de rota &quot;{id: int}&quot;, que corresponde a se o segmento URI contém um valor inteiro.</span><span class="sxs-lookup"><span data-stu-id="1af71-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="1af71-199">Método</span><span class="sxs-lookup"><span data-stu-id="1af71-199">Method</span></span> | <span data-ttu-id="1af71-200">Modelo de rota</span><span class="sxs-lookup"><span data-stu-id="1af71-200">Route Template</span></span> | <span data-ttu-id="1af71-201">Exemplo de URI</span><span class="sxs-lookup"><span data-stu-id="1af71-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="1af71-202">"api/livros"</span><span class="sxs-lookup"><span data-stu-id="1af71-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="1af71-203">"api/manuais / {id: int}"</span><span class="sxs-lookup"><span data-stu-id="1af71-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="1af71-204">Obter os detalhes do livro</span><span class="sxs-lookup"><span data-stu-id="1af71-204">Get Book Details</span></span>

<span data-ttu-id="1af71-205">Para obter detalhes do livro, o cliente enviará uma solicitação GET para `/api/books/{id}/details`, onde *{id}* é a ID do livro.</span><span class="sxs-lookup"><span data-stu-id="1af71-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="1af71-206">Adicione o seguinte método à classe `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="1af71-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="1af71-207">Se você solicitar `/api/books/1/details`, a resposta tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="1af71-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="1af71-208">Obter livros por gênero</span><span class="sxs-lookup"><span data-stu-id="1af71-208">Get Books By Genre</span></span>

<span data-ttu-id="1af71-209">Para obter uma lista de livros em um gênero específico, o cliente enviará uma solicitação GET para `/api/books/genre`, onde *gênero* é o nome do gênero.</span><span class="sxs-lookup"><span data-stu-id="1af71-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="1af71-210">(Por exemplo, `/api/books/fantasy`.)</span><span class="sxs-lookup"><span data-stu-id="1af71-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="1af71-211">Adicione o seguinte método para `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="1af71-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="1af71-212">Aqui definimos uma rota que contém um parâmetro {gênero} no modelo de URI.</span><span class="sxs-lookup"><span data-stu-id="1af71-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="1af71-213">Observe que a API da Web é capaz de distinguir esses dois URIs e roteá-las para métodos diferentes:</span><span class="sxs-lookup"><span data-stu-id="1af71-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="1af71-214">Isso ocorre porque o `GetBook` método inclui uma restrição de que o segmento "id" deve ser um valor inteiro:</span><span class="sxs-lookup"><span data-stu-id="1af71-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="1af71-215">Se você solicitar /api/books/fantasy, a resposta terá esta aparência:</span><span class="sxs-lookup"><span data-stu-id="1af71-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="1af71-216">Obter livros por autor</span><span class="sxs-lookup"><span data-stu-id="1af71-216">Get Books By Author</span></span>

<span data-ttu-id="1af71-217">Para obter uma lista de livros de um para um autor específico, o cliente enviará uma solicitação GET para `/api/authors/id/books`, onde *id* é a ID do autor.</span><span class="sxs-lookup"><span data-stu-id="1af71-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="1af71-218">Adicione o seguinte método para `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="1af71-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="1af71-219">Este exemplo é interessante porque &quot;livros&quot; é tratado de um recurso filho de &quot;autores&quot;.</span><span class="sxs-lookup"><span data-stu-id="1af71-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="1af71-220">Esse padrão é muito comum nas APIs RESTful.</span><span class="sxs-lookup"><span data-stu-id="1af71-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="1af71-221">O til (~) no modelo de rota substitui o prefixo da rota na **RoutePrefix** atributo.</span><span class="sxs-lookup"><span data-stu-id="1af71-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="1af71-222">Obter livros por data de publicação</span><span class="sxs-lookup"><span data-stu-id="1af71-222">Get Books By Publication Date</span></span>

<span data-ttu-id="1af71-223">Para obter uma lista de livros por data de publicação, o cliente enviará uma solicitação GET para `/api/books/date/yyyy-mm-dd`, onde *aaaa-mm-dd* é a data.</span><span class="sxs-lookup"><span data-stu-id="1af71-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="1af71-224">Aqui está uma maneira de fazer isso:</span><span class="sxs-lookup"><span data-stu-id="1af71-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="1af71-225">O `{pubdate:datetime}` parâmetro é restrito para corresponder a um **DateTime** valor.</span><span class="sxs-lookup"><span data-stu-id="1af71-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="1af71-226">Isso funciona, mas é realmente mais permissivo do que gostaríamos.</span><span class="sxs-lookup"><span data-stu-id="1af71-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="1af71-227">Por exemplo, esses URIs também serão compatíveis com a rota:</span><span class="sxs-lookup"><span data-stu-id="1af71-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="1af71-228">Não há nada de errado com permitindo que esses URIs.</span><span class="sxs-lookup"><span data-stu-id="1af71-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="1af71-229">No entanto, você pode restringir a rota para um determinado formato, adicionando uma restrição de expressão regular para o modelo de rota:</span><span class="sxs-lookup"><span data-stu-id="1af71-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="1af71-230">Agora apenas as datas no formato &quot;aaaa-mm-dd&quot; fará a correspondência.</span><span class="sxs-lookup"><span data-stu-id="1af71-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="1af71-231">Observe que não usamos a expressão regular para validar que temos uma data real.</span><span class="sxs-lookup"><span data-stu-id="1af71-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="1af71-232">Isso é feito quando a API da Web tenta converter o segmento do URI em uma **DateTime** instância.</span><span class="sxs-lookup"><span data-stu-id="1af71-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="1af71-233">Uma data inválida, como ' 2012-47-99' falhará a ser convertido e o cliente receberá um erro 404.</span><span class="sxs-lookup"><span data-stu-id="1af71-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="1af71-234">Você também pode dar suporte a um separador de barra "/" (`/api/books/date/yyyy/mm/dd`), adicionando outro **[Route]** atributo com um regex diferente.</span><span class="sxs-lookup"><span data-stu-id="1af71-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="1af71-235">Há um detalhe sutil mas importante aqui.</span><span class="sxs-lookup"><span data-stu-id="1af71-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="1af71-236">O segundo modelo de rota tem um caractere curinga (\*) no início do parâmetro {pubdate}:</span><span class="sxs-lookup"><span data-stu-id="1af71-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="1af71-237">Isso informa ao mecanismo de roteamento que {pubdate} deve coincidir com o restante do URI.</span><span class="sxs-lookup"><span data-stu-id="1af71-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="1af71-238">Por padrão, um parâmetro de modelo corresponde a um único segmento do URI.</span><span class="sxs-lookup"><span data-stu-id="1af71-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="1af71-239">Nesse caso, queremos {pubdate} abrangem vários segmentos URI:</span><span class="sxs-lookup"><span data-stu-id="1af71-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="1af71-240">Código do controlador</span><span class="sxs-lookup"><span data-stu-id="1af71-240">Controller Code</span></span>

<span data-ttu-id="1af71-241">Aqui está o código completo para a classe BooksController.</span><span class="sxs-lookup"><span data-stu-id="1af71-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="1af71-242">Resumo</span><span class="sxs-lookup"><span data-stu-id="1af71-242">Summary</span></span>

<span data-ttu-id="1af71-243">Roteamento de atributo fornece mais controle e maior flexibilidade ao projetar os URIs para sua API.</span><span class="sxs-lookup"><span data-stu-id="1af71-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
