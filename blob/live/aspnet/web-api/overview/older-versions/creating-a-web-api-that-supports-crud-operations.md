---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: "Habilitar operações CRUD de ASP.NET Web API 1 | Microsoft Docs"
author: MikeWasson
description: "Este tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando ASP.NET Web API. Versões de software usadas no tutorial Visual Studio 2012 Web ponto de acesso..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a91bf065c9ce0fc5bd9b7115340edabea975a7e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="144af-104">Habilitar operações CRUD de ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="144af-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="144af-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="144af-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="144af-106">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="144af-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="144af-107">Este tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="144af-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="144af-108">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="144af-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="144af-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="144af-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="144af-110">API Web 1 (também funciona com a API Web 2)</span><span class="sxs-lookup"><span data-stu-id="144af-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="144af-111">CRUD significa &quot;criar, ler, atualizar e excluir,&quot; quais são as quatro operações de banco de dados básico.</span><span class="sxs-lookup"><span data-stu-id="144af-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="144af-112">Muitos serviços HTTP também modelo operações CRUD por meio de REST ou APIs de REST.</span><span class="sxs-lookup"><span data-stu-id="144af-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="144af-113">Neste tutorial, você criará uma API para gerenciar uma lista de produtos de web muito simple.</span><span class="sxs-lookup"><span data-stu-id="144af-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="144af-114">Cada produto contém um nome, o preço e a categoria (como &quot;toys&quot; ou &quot;hardware&quot;), além de uma ID de produto.</span><span class="sxs-lookup"><span data-stu-id="144af-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="144af-115">Os produtos de API expõe métodos a seguir.</span><span class="sxs-lookup"><span data-stu-id="144af-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="144af-116">Ação</span><span class="sxs-lookup"><span data-stu-id="144af-116">Action</span></span> | <span data-ttu-id="144af-117">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="144af-117">HTTP method</span></span> | <span data-ttu-id="144af-118">URI relativo</span><span class="sxs-lookup"><span data-stu-id="144af-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="144af-119">Obter uma lista de todos os produtos</span><span class="sxs-lookup"><span data-stu-id="144af-119">Get a list of all products</span></span> | <span data-ttu-id="144af-120">OBTER</span><span class="sxs-lookup"><span data-stu-id="144af-120">GET</span></span> | <span data-ttu-id="144af-121">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="144af-121">/api/products</span></span> |
| <span data-ttu-id="144af-122">Obter um produto por ID</span><span class="sxs-lookup"><span data-stu-id="144af-122">Get a product by ID</span></span> | <span data-ttu-id="144af-123">OBTER</span><span class="sxs-lookup"><span data-stu-id="144af-123">GET</span></span> | <span data-ttu-id="144af-124">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="144af-124">/api/products/*id*</span></span> |
| <span data-ttu-id="144af-125">Obter um produto por categoria</span><span class="sxs-lookup"><span data-stu-id="144af-125">Get a product by category</span></span> | <span data-ttu-id="144af-126">OBTER</span><span class="sxs-lookup"><span data-stu-id="144af-126">GET</span></span> | <span data-ttu-id="144af-127">produtos/api /? categoria =*categoria*</span><span class="sxs-lookup"><span data-stu-id="144af-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="144af-128">Criar um novo produto</span><span class="sxs-lookup"><span data-stu-id="144af-128">Create a new product</span></span> | <span data-ttu-id="144af-129">POSTAR</span><span class="sxs-lookup"><span data-stu-id="144af-129">POST</span></span> | <span data-ttu-id="144af-130">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="144af-130">/api/products</span></span> |
| <span data-ttu-id="144af-131">Atualização de um produto</span><span class="sxs-lookup"><span data-stu-id="144af-131">Update a product</span></span> | <span data-ttu-id="144af-132">PUT</span><span class="sxs-lookup"><span data-stu-id="144af-132">PUT</span></span> | <span data-ttu-id="144af-133">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="144af-133">/api/products/*id*</span></span> |
| <span data-ttu-id="144af-134">Excluir um produto</span><span class="sxs-lookup"><span data-stu-id="144af-134">Delete a product</span></span> | <span data-ttu-id="144af-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="144af-135">DELETE</span></span> | <span data-ttu-id="144af-136">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="144af-136">/api/products/*id*</span></span> |

<span data-ttu-id="144af-137">Observe que alguns dos URIs incluem a identificação do produto no caminho.</span><span class="sxs-lookup"><span data-stu-id="144af-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="144af-138">Por exemplo, para obter o produto cuja ID é 28, o cliente envia uma solicitação GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="144af-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="144af-139">Recursos</span><span class="sxs-lookup"><span data-stu-id="144af-139">Resources</span></span>

<span data-ttu-id="144af-140">Os produtos API define URIs para os dois tipos de recursos:</span><span class="sxs-lookup"><span data-stu-id="144af-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="144af-141">Recurso</span><span class="sxs-lookup"><span data-stu-id="144af-141">Resource</span></span> | <span data-ttu-id="144af-142">URI</span><span class="sxs-lookup"><span data-stu-id="144af-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="144af-143">A lista de todos os produtos.</span><span class="sxs-lookup"><span data-stu-id="144af-143">The list of all the products.</span></span> | <span data-ttu-id="144af-144">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="144af-144">/api/products</span></span> |
| <span data-ttu-id="144af-145">Um produto individual.</span><span class="sxs-lookup"><span data-stu-id="144af-145">An individual product.</span></span> | <span data-ttu-id="144af-146">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="144af-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="144af-147">Métodos</span><span class="sxs-lookup"><span data-stu-id="144af-147">Methods</span></span>

<span data-ttu-id="144af-148">Os quatro principais métodos HTTP (GET, PUT, POST e DELETE) podem ser mapeados para as operações CRUD da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="144af-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="144af-149">GET recupera a representação do recurso em um URI especificado.</span><span class="sxs-lookup"><span data-stu-id="144af-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="144af-150">GET deve não têm efeitos colaterais no servidor.</span><span class="sxs-lookup"><span data-stu-id="144af-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="144af-151">PUT atualiza um recurso em um URI especificado.</span><span class="sxs-lookup"><span data-stu-id="144af-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="144af-152">PUT também pode ser usado para criar um novo recurso em um URI especificado, se o servidor permite que os clientes especificar os URIs de novo.</span><span class="sxs-lookup"><span data-stu-id="144af-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="144af-153">Para este tutorial, a API não dará suporte a criação por meio de PUT.</span><span class="sxs-lookup"><span data-stu-id="144af-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="144af-154">POST cria um novo recurso.</span><span class="sxs-lookup"><span data-stu-id="144af-154">POST creates a new resource.</span></span> <span data-ttu-id="144af-155">O servidor atribui o URI para o novo objeto e retorna esse URI como parte da mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="144af-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="144af-156">DELETE Exclui um recurso em um URI especificado.</span><span class="sxs-lookup"><span data-stu-id="144af-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="144af-157">Observação: O método PUT substitui a entidade de produtos inteiro.</span><span class="sxs-lookup"><span data-stu-id="144af-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="144af-158">Ou seja, o cliente deve enviar uma representação completa do produto atualizado.</span><span class="sxs-lookup"><span data-stu-id="144af-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="144af-159">Se você desejar oferecer suporte a atualizações parciais, o método de PATCH é preferencial.</span><span class="sxs-lookup"><span data-stu-id="144af-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="144af-160">Este tutorial não implementa o PATCH.</span><span class="sxs-lookup"><span data-stu-id="144af-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="144af-161">Criar um novo projeto de API da Web</span><span class="sxs-lookup"><span data-stu-id="144af-161">Create a New Web API Project</span></span>

<span data-ttu-id="144af-162">Comece executando o Visual Studio e selecione **novo projeto** do **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="144af-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="144af-163">Ou, do **arquivo** menu, selecione **novo** e **projeto**.</span><span class="sxs-lookup"><span data-stu-id="144af-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="144af-164">No **modelos** painel, selecione **modelos instalados** e expanda o **Visual C#** nó.</span><span class="sxs-lookup"><span data-stu-id="144af-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="144af-165">Em **Visual C#**, selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="144af-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="144af-166">Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="144af-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="144af-167">Nomeie o projeto &quot;ProductStore&quot; e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="144af-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="144af-168">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione **API da Web** e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="144af-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="144af-169">Adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="144af-169">Adding a Model</span></span>

<span data-ttu-id="144af-170">Um *modelo* é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="144af-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="144af-171">Na API da Web do ASP.NET, fortemente tipados objetos CLR podem ser usados como modelos e ele serão automaticamente serializados como XML ou JSON para o cliente.</span><span class="sxs-lookup"><span data-stu-id="144af-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="144af-172">Para a API ProductStore, nossos dados consistem em produtos, portanto, vamos criar uma nova classe chamada `Product`.</span><span class="sxs-lookup"><span data-stu-id="144af-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="144af-173">Se o Gerenciador de soluções não estiver visível, clique no **exibição** menu e selecione **Gerenciador de soluções**.</span><span class="sxs-lookup"><span data-stu-id="144af-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="144af-174">No Gerenciador de soluções, clique com botão direito do **modelos** pasta.</span><span class="sxs-lookup"><span data-stu-id="144af-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="144af-175">Meny contexto, selecione **adicionar**, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="144af-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="144af-176">Nomeie a classe &quot;produto&quot;.</span><span class="sxs-lookup"><span data-stu-id="144af-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="144af-177">Adicione as seguintes propriedades para o `Product` classe.</span><span class="sxs-lookup"><span data-stu-id="144af-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="144af-178">Adicionar um repositório</span><span class="sxs-lookup"><span data-stu-id="144af-178">Adding a Repository</span></span>

<span data-ttu-id="144af-179">É necessário armazenar um conjunto de produtos.</span><span class="sxs-lookup"><span data-stu-id="144af-179">We need to store a collection of products.</span></span> <span data-ttu-id="144af-180">É uma boa ideia para separar a coleção de nossa implementação de serviço.</span><span class="sxs-lookup"><span data-stu-id="144af-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="144af-181">Dessa forma, podemos alterar o repositório de backup sem reescrever a classe de serviço.</span><span class="sxs-lookup"><span data-stu-id="144af-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="144af-182">Esse tipo de design é chamado de *repositório* padrão.</span><span class="sxs-lookup"><span data-stu-id="144af-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="144af-183">Começar definindo uma interface genérica para o repositório.</span><span class="sxs-lookup"><span data-stu-id="144af-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="144af-184">No Gerenciador de soluções, clique com botão direito do **modelos** pasta.</span><span class="sxs-lookup"><span data-stu-id="144af-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="144af-185">Selecione **adicionar**, em seguida, selecione **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="144af-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="144af-186">No **modelos** painel, selecione **modelos instalados** e expanda o nó C#.</span><span class="sxs-lookup"><span data-stu-id="144af-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="144af-187">Em c#, selecione **código**.</span><span class="sxs-lookup"><span data-stu-id="144af-187">Under C#, select **Code**.</span></span> <span data-ttu-id="144af-188">Na lista de modelos de código, selecione **Interface**.</span><span class="sxs-lookup"><span data-stu-id="144af-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="144af-189">Nome da interface &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="144af-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="144af-190">Adicione a implementação a seguir:</span><span class="sxs-lookup"><span data-stu-id="144af-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="144af-191">Agora adicione outra classe para a pasta de modelos, denominada &quot;ProductRepository.&quot; Essa classe implementará a interface `IProductRespository`.</span><span class="sxs-lookup"><span data-stu-id="144af-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="144af-192">Adicione a implementação a seguir:</span><span class="sxs-lookup"><span data-stu-id="144af-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="144af-193">O repositório mantém a lista na memória local.</span><span class="sxs-lookup"><span data-stu-id="144af-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="144af-194">Isso é Okey para obter um tutorial, mas em um aplicativo real, você deve armazenar os dados externamente, um banco de dados ou no armazenamento em nuvem.</span><span class="sxs-lookup"><span data-stu-id="144af-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="144af-195">O padrão de repositório tornará mais fácil de alterar a implementação mais tarde.</span><span class="sxs-lookup"><span data-stu-id="144af-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="144af-196">Adicionar um controlador de API da Web</span><span class="sxs-lookup"><span data-stu-id="144af-196">Adding a Web API Controller</span></span>

<span data-ttu-id="144af-197">Se você trabalhou com o ASP.NET MVC, em seguida, você já está familiarizado com controladores.</span><span class="sxs-lookup"><span data-stu-id="144af-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="144af-198">Na API da Web do ASP.NET, um *controlador* é uma classe que trata as solicitações HTTP do cliente.</span><span class="sxs-lookup"><span data-stu-id="144af-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="144af-199">O Assistente de novo projeto criado dois controladores quando ele criou o projeto.</span><span class="sxs-lookup"><span data-stu-id="144af-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="144af-200">Para vê-los, expanda a pasta de controladores no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="144af-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="144af-201">HomeController é um controlador MVC do ASP.NET tradicional.</span><span class="sxs-lookup"><span data-stu-id="144af-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="144af-202">Ele é responsável pelo fornecimento de páginas HTML para o site e não está diretamente relacionado ao nosso API da web.</span><span class="sxs-lookup"><span data-stu-id="144af-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="144af-203">ValuesController é um controlador de WebAPI de exemplo.</span><span class="sxs-lookup"><span data-stu-id="144af-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="144af-204">Vá em frente e excluir ValuesController, clicando duas vezes o arquivo no Gerenciador de soluções e selecionando **excluir.**</span><span class="sxs-lookup"><span data-stu-id="144af-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="144af-205">Agora adicione um novo controlador, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="144af-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="144af-206">Em **Solution Explorer**, clique a pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="144af-206">In **Solution Explorer**, right-click the the Controllers folder.</span></span> <span data-ttu-id="144af-207">Selecione **adicionar** e, em seguida, selecione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="144af-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="144af-208">No **Adicionar controlador** assistente, o nome do controlador &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="144af-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="144af-209">No **modelo** lista suspensa, selecione **controlador API vazio**.</span><span class="sxs-lookup"><span data-stu-id="144af-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="144af-210">Em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="144af-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="144af-211">Não é necessário colocar seus controladores em uma pasta chamada controladores.</span><span class="sxs-lookup"><span data-stu-id="144af-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="144af-212">O nome da pasta não é importante; ele é simplesmente uma maneira conveniente de organizar seus arquivos de origem.</span><span class="sxs-lookup"><span data-stu-id="144af-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="144af-213">O **Adicionar controlador** assistente criará um arquivo chamado ProductsController.cs na pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="144af-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="144af-214">Se esse arquivo não estiver aberto, clique duas vezes no arquivo para abri-lo.</span><span class="sxs-lookup"><span data-stu-id="144af-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="144af-215">Adicione o seguinte **usando** instrução:</span><span class="sxs-lookup"><span data-stu-id="144af-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="144af-216">Adicionar um campo que contém um **IProductRepository** instância.</span><span class="sxs-lookup"><span data-stu-id="144af-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="144af-217">Chamando `new ProductRepository()` no controlador não é o melhor design, porque ela unirá o controlador a uma implementação específica de `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="144af-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="144af-218">Uma abordagem melhor, consulte [usando o resolvedor de dependência de API da Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="144af-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="144af-219">Obtendo um recurso</span><span class="sxs-lookup"><span data-stu-id="144af-219">Getting a Resource</span></span>

<span data-ttu-id="144af-220">A API ProductStore irá expor vários &quot;ler&quot; ações como métodos HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="144af-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="144af-221">Cada ação corresponderá a um método em de `ProductsController` classe.</span><span class="sxs-lookup"><span data-stu-id="144af-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="144af-222">Ação</span><span class="sxs-lookup"><span data-stu-id="144af-222">Action</span></span> | <span data-ttu-id="144af-223">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="144af-223">HTTP method</span></span> | <span data-ttu-id="144af-224">URI relativo</span><span class="sxs-lookup"><span data-stu-id="144af-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="144af-225">Obter uma lista de todos os produtos</span><span class="sxs-lookup"><span data-stu-id="144af-225">Get a list of all products</span></span> | <span data-ttu-id="144af-226">OBTER</span><span class="sxs-lookup"><span data-stu-id="144af-226">GET</span></span> | <span data-ttu-id="144af-227">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="144af-227">/api/products</span></span> |
| <span data-ttu-id="144af-228">Obter um produto por ID</span><span class="sxs-lookup"><span data-stu-id="144af-228">Get a product by ID</span></span> | <span data-ttu-id="144af-229">OBTER</span><span class="sxs-lookup"><span data-stu-id="144af-229">GET</span></span> | <span data-ttu-id="144af-230">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="144af-230">/api/products/*id*</span></span> |
| <span data-ttu-id="144af-231">Obter um produto por categoria</span><span class="sxs-lookup"><span data-stu-id="144af-231">Get a product by category</span></span> | <span data-ttu-id="144af-232">OBTER</span><span class="sxs-lookup"><span data-stu-id="144af-232">GET</span></span> | <span data-ttu-id="144af-233">produtos/api /? categoria =*categoria*</span><span class="sxs-lookup"><span data-stu-id="144af-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="144af-234">Para obter a lista de todos os produtos, adicione este método para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="144af-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="144af-235">O nome do método começa com &quot;obter&quot;, portanto, por convenção, ele será mapeado para solicitações GET.</span><span class="sxs-lookup"><span data-stu-id="144af-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="144af-236">Além disso, porque o método não tem parâmetros, ele será mapeado para um URI que não contém um  *&quot;id&quot;*  segmento do caminho.</span><span class="sxs-lookup"><span data-stu-id="144af-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="144af-237">Para obter um produto por ID, adicione este método para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="144af-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="144af-238">Esse nome de método também inicia com &quot;obter&quot;, mas o método tem um parâmetro denominado *id*. Esse parâmetro é mapeado para o &quot;id&quot; segmento do caminho URI.</span><span class="sxs-lookup"><span data-stu-id="144af-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="144af-239">A estrutura do ASP.NET Web API converte automaticamente a ID para o tipo de dados correto (**int**) para o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="144af-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="144af-240">O método GetProduct lança uma exceção do tipo **HttpResponseException** se *id* não é válido.</span><span class="sxs-lookup"><span data-stu-id="144af-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="144af-241">Essa exceção será convertida pela estrutura em um erro 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="144af-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="144af-242">Finalmente, adicione um método para localizar produtos por categoria:</span><span class="sxs-lookup"><span data-stu-id="144af-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="144af-243">Se o URI da solicitação tem uma cadeia de caracteres de consulta, API da Web tenta corresponder aos parâmetros de consulta para parâmetros de método do controlador.</span><span class="sxs-lookup"><span data-stu-id="144af-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="144af-244">Portanto, um URI do formulário "api/produtos? categoria =*categoria*" será mapeado para esse método.</span><span class="sxs-lookup"><span data-stu-id="144af-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="144af-245">Criar um recurso</span><span class="sxs-lookup"><span data-stu-id="144af-245">Creating a Resource</span></span>

<span data-ttu-id="144af-246">Em seguida, vamos adicionar um método para o `ProductsController` classe para criar um novo produto.</span><span class="sxs-lookup"><span data-stu-id="144af-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="144af-247">Aqui está uma simple implementação do método:</span><span class="sxs-lookup"><span data-stu-id="144af-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="144af-248">Observe que duas coisas sobre este método:</span><span class="sxs-lookup"><span data-stu-id="144af-248">Note two things about this method:</span></span>

- <span data-ttu-id="144af-249">O nome do método começa com &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="144af-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="144af-250">Para criar um novo produto, o cliente envia uma solicitação HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="144af-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="144af-251">O método usa um parâmetro de tipo de produto.</span><span class="sxs-lookup"><span data-stu-id="144af-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="144af-252">Na API da Web, os parâmetros com tipos complexos são desserializar o corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="144af-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="144af-253">Portanto, esperamos que o cliente envie uma representação serializada de um objeto de produto, em formato XML ou JSON.</span><span class="sxs-lookup"><span data-stu-id="144af-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="144af-254">Essa implementação funcionará, mas não está completo.</span><span class="sxs-lookup"><span data-stu-id="144af-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="144af-255">Idealmente, gostaríamos de receber a resposta HTTP para incluem o seguinte:</span><span class="sxs-lookup"><span data-stu-id="144af-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="144af-256">**Código de resposta:** por padrão, a estrutura da API Web define o código de status de resposta para 200 (Okey).</span><span class="sxs-lookup"><span data-stu-id="144af-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="144af-257">Mas de acordo com o protocolo HTTP/1.1, quando uma solicitação POST resulta na criação de um recurso, o servidor deve responder com status 201 (criado).</span><span class="sxs-lookup"><span data-stu-id="144af-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="144af-258">**Local:** quando o servidor cria um recurso, ele deve incluir o URI do novo recurso no cabeçalho de local da resposta.</span><span class="sxs-lookup"><span data-stu-id="144af-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="144af-259">ASP.NET Web API facilita manipular a mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="144af-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="144af-260">Esta é a implementação aprimorada:</span><span class="sxs-lookup"><span data-stu-id="144af-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="144af-261">Observe que o tipo de retorno do método agora é **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="144af-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="144af-262">Retornando um **HttpResponseMessage** em vez de um produto, é possível controlar os detalhes da mensagem de resposta HTTP, incluindo o código de status e o cabeçalho de local.</span><span class="sxs-lookup"><span data-stu-id="144af-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="144af-263">O **CreateResponse** método cria um **HttpResponseMessage** e grava uma representação serializada do objeto Product automaticamente para o corpo fo a mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="144af-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="144af-264">Este exemplo não valida o `Product`.</span><span class="sxs-lookup"><span data-stu-id="144af-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="144af-265">Para obter informações sobre a validação de modelo, consulte [validação de modelo no ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="144af-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="144af-266">Atualizando um recurso</span><span class="sxs-lookup"><span data-stu-id="144af-266">Updating a Resource</span></span>

<span data-ttu-id="144af-267">Atualizar um produto com PUT é simples:</span><span class="sxs-lookup"><span data-stu-id="144af-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="144af-268">O nome do método começa com &quot;colocar... &quot;, portanto, API da Web corresponde a ele para solicitações PUT.</span><span class="sxs-lookup"><span data-stu-id="144af-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="144af-269">O método aceita dois parâmetros, a ID do produto e o produto atualizado.</span><span class="sxs-lookup"><span data-stu-id="144af-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="144af-270">O *id* parâmetro é obtido do caminho de URI e o *produto* parâmetro é desserializado do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="144af-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="144af-271">Por padrão, a estrutura do ASP.NET Web API usa os tipos de parâmetro simples do roteiro e tipos complexos do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="144af-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="144af-272">Excluir um recurso</span><span class="sxs-lookup"><span data-stu-id="144af-272">Deleting a Resource</span></span>

<span data-ttu-id="144af-273">Para excluir um resourse, defina um método "Excluir...".</span><span class="sxs-lookup"><span data-stu-id="144af-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="144af-274">Se uma solicitação de exclusão for bem-sucedida, ela pode retornar o status 200 (Okey) com um corpo de entidade que descreve o status; status 202 (aceito) se a exclusão ainda pendentes; ou status 204 (sem conteúdo) sem corpo de entidade.</span><span class="sxs-lookup"><span data-stu-id="144af-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="144af-275">Nesse caso, o `DeleteProduct` método tem um `void` tipo de retorno, para a API da Web ASP.NET converte isso automaticamente em status 204 (sem conteúdo) de código.</span><span class="sxs-lookup"><span data-stu-id="144af-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
