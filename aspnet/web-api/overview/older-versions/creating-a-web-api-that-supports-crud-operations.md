---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Habilitar operações CRUD na API 1 da Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando a API Web do ASP.NET. Versões de software usadas no tutorial Visual Studio 2012 Web ponto de acesso...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 1658e120225cd3c9425168238981133c96ff398a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402922"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="e0e43-104">Habilitar operações CRUD na API 1 da Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e0e43-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="e0e43-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e0e43-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e0e43-106">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="e0e43-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="e0e43-107">Este tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando a API Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e0e43-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e0e43-108">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="e0e43-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e0e43-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e0e43-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="e0e43-110">API Web 1 (também funciona com a API Web 2)</span><span class="sxs-lookup"><span data-stu-id="e0e43-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="e0e43-111">Representa o CRUD &quot;criação, leitura, atualização e exclusão,&quot; que são as quatro operações de banco de dados básico.</span><span class="sxs-lookup"><span data-stu-id="e0e43-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="e0e43-112">Muitos serviços HTTP do modelo também operações CRUD por meio do REST ou APIs de REST.</span><span class="sxs-lookup"><span data-stu-id="e0e43-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="e0e43-113">Neste tutorial, você criará uma API para gerenciar uma lista de produtos de web muito simple.</span><span class="sxs-lookup"><span data-stu-id="e0e43-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="e0e43-114">Cada produto contém um nome, preço e categoria (tal como &quot;toys&quot; ou &quot;hardware&quot;), além de uma ID de produto.</span><span class="sxs-lookup"><span data-stu-id="e0e43-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="e0e43-115">Os produtos de API irá expor métodos a seguir.</span><span class="sxs-lookup"><span data-stu-id="e0e43-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="e0e43-116">Ação</span><span class="sxs-lookup"><span data-stu-id="e0e43-116">Action</span></span> | <span data-ttu-id="e0e43-117">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="e0e43-117">HTTP method</span></span> | <span data-ttu-id="e0e43-118">URI relativo</span><span class="sxs-lookup"><span data-stu-id="e0e43-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e0e43-119">Obter uma lista de todos os produtos</span><span class="sxs-lookup"><span data-stu-id="e0e43-119">Get a list of all products</span></span> | <span data-ttu-id="e0e43-120">OBTER</span><span class="sxs-lookup"><span data-stu-id="e0e43-120">GET</span></span> | <span data-ttu-id="e0e43-121">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="e0e43-121">/api/products</span></span> |
| <span data-ttu-id="e0e43-122">Obter um produto por ID</span><span class="sxs-lookup"><span data-stu-id="e0e43-122">Get a product by ID</span></span> | <span data-ttu-id="e0e43-123">OBTER</span><span class="sxs-lookup"><span data-stu-id="e0e43-123">GET</span></span> | <span data-ttu-id="e0e43-124">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="e0e43-124">/api/products/*id*</span></span> |
| <span data-ttu-id="e0e43-125">Obter um produto por categoria</span><span class="sxs-lookup"><span data-stu-id="e0e43-125">Get a product by category</span></span> | <span data-ttu-id="e0e43-126">OBTER</span><span class="sxs-lookup"><span data-stu-id="e0e43-126">GET</span></span> | <span data-ttu-id="e0e43-127">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="e0e43-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="e0e43-128">Criar um novo produto</span><span class="sxs-lookup"><span data-stu-id="e0e43-128">Create a new product</span></span> | <span data-ttu-id="e0e43-129">POSTAR</span><span class="sxs-lookup"><span data-stu-id="e0e43-129">POST</span></span> | <span data-ttu-id="e0e43-130">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="e0e43-130">/api/products</span></span> |
| <span data-ttu-id="e0e43-131">Atualizar um produto</span><span class="sxs-lookup"><span data-stu-id="e0e43-131">Update a product</span></span> | <span data-ttu-id="e0e43-132">PUT</span><span class="sxs-lookup"><span data-stu-id="e0e43-132">PUT</span></span> | <span data-ttu-id="e0e43-133">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="e0e43-133">/api/products/*id*</span></span> |
| <span data-ttu-id="e0e43-134">Excluir um produto</span><span class="sxs-lookup"><span data-stu-id="e0e43-134">Delete a product</span></span> | <span data-ttu-id="e0e43-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="e0e43-135">DELETE</span></span> | <span data-ttu-id="e0e43-136">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="e0e43-136">/api/products/*id*</span></span> |

<span data-ttu-id="e0e43-137">Observe que alguns dos URIs incluem a ID do produto no caminho.</span><span class="sxs-lookup"><span data-stu-id="e0e43-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="e0e43-138">Por exemplo, para obter o produto cuja ID é 28, o cliente envia uma solicitação GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="e0e43-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="e0e43-139">Recursos</span><span class="sxs-lookup"><span data-stu-id="e0e43-139">Resources</span></span>

<span data-ttu-id="e0e43-140">Os produtos de API define os URIs para dois tipos de recursos:</span><span class="sxs-lookup"><span data-stu-id="e0e43-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="e0e43-141">Recurso</span><span class="sxs-lookup"><span data-stu-id="e0e43-141">Resource</span></span> | <span data-ttu-id="e0e43-142">URI</span><span class="sxs-lookup"><span data-stu-id="e0e43-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="e0e43-143">A lista de todos os produtos.</span><span class="sxs-lookup"><span data-stu-id="e0e43-143">The list of all the products.</span></span> | <span data-ttu-id="e0e43-144">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="e0e43-144">/api/products</span></span> |
| <span data-ttu-id="e0e43-145">Um produto individual.</span><span class="sxs-lookup"><span data-stu-id="e0e43-145">An individual product.</span></span> | <span data-ttu-id="e0e43-146">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="e0e43-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="e0e43-147">Métodos</span><span class="sxs-lookup"><span data-stu-id="e0e43-147">Methods</span></span>

<span data-ttu-id="e0e43-148">Os quatro principais métodos HTTP (GET, PUT, POST e DELETE) podem ser mapeados para operações CRUD da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="e0e43-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="e0e43-149">GET recupera a representação do recurso em um URI especificado.</span><span class="sxs-lookup"><span data-stu-id="e0e43-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="e0e43-150">GET não deve ter nenhum efeito colateral no servidor.</span><span class="sxs-lookup"><span data-stu-id="e0e43-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="e0e43-151">PUT atualiza um recurso em um URI especificado.</span><span class="sxs-lookup"><span data-stu-id="e0e43-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="e0e43-152">PUT também pode ser usado para criar um novo recurso em um URI especificado, se o servidor permite que os clientes especifiquem os novos URIs.</span><span class="sxs-lookup"><span data-stu-id="e0e43-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="e0e43-153">Para este tutorial, a API não dará suporte à criação por meio de PUT.</span><span class="sxs-lookup"><span data-stu-id="e0e43-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="e0e43-154">POST cria um novo recurso.</span><span class="sxs-lookup"><span data-stu-id="e0e43-154">POST creates a new resource.</span></span> <span data-ttu-id="e0e43-155">O servidor atribui o URI para o novo objeto e retorna esse URI como parte da mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="e0e43-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="e0e43-156">DELETE Exclui um recurso em um URI especificado.</span><span class="sxs-lookup"><span data-stu-id="e0e43-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="e0e43-157">Observação: O método PUT substitui a entidade product inteiro.</span><span class="sxs-lookup"><span data-stu-id="e0e43-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="e0e43-158">Ou seja, o cliente deve enviar uma representação completa do produto atualizado.</span><span class="sxs-lookup"><span data-stu-id="e0e43-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="e0e43-159">Se você quiser dar suporte a atualizações parciais, o método de PATCH é preferencial.</span><span class="sxs-lookup"><span data-stu-id="e0e43-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="e0e43-160">Este tutorial não implementa o PATCH.</span><span class="sxs-lookup"><span data-stu-id="e0e43-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="e0e43-161">Criar um novo projeto de API da Web</span><span class="sxs-lookup"><span data-stu-id="e0e43-161">Create a New Web API Project</span></span>

<span data-ttu-id="e0e43-162">Comece executando o Visual Studio e selecione **novo projeto** da **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="e0e43-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="e0e43-163">Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="e0e43-164">No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="e0e43-165">Em **Visual C#**, selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="e0e43-166">Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="e0e43-167">Nomeie o projeto &quot;ProductStore&quot; e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="e0e43-168">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione **API da Web** e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="e0e43-169">Adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="e0e43-169">Adding a Model</span></span>

<span data-ttu-id="e0e43-170">Um *modelo (model)* é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e0e43-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="e0e43-171">Na API Web ASP.NET, você pode usar objetos CLR fortemente tipados como modelos e eles serão automaticamente serializados para JSON ou XML para o cliente.</span><span class="sxs-lookup"><span data-stu-id="e0e43-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="e0e43-172">Para a API ProductStore, nossos dados consistem em produtos, portanto, vamos criar uma nova classe chamada `Product`.</span><span class="sxs-lookup"><span data-stu-id="e0e43-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="e0e43-173">Se o Gerenciador de Soluções não estiver visível, clique no menu **Exibir** e selecione **Gerenciador de Soluções**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="e0e43-174">No Gerenciador de soluções, clique com botão direito do **modelos** pasta.</span><span class="sxs-lookup"><span data-stu-id="e0e43-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="e0e43-175">A partir do meny contexto, selecione **Add**, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="e0e43-176">Nomeie a classe &quot;Produt&quot; (produto).</span><span class="sxs-lookup"><span data-stu-id="e0e43-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="e0e43-177">Adicione as seguintes propriedades para a classe `Product`.</span><span class="sxs-lookup"><span data-stu-id="e0e43-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="e0e43-178">Adicionar um repositório</span><span class="sxs-lookup"><span data-stu-id="e0e43-178">Adding a Repository</span></span>

<span data-ttu-id="e0e43-179">É necessário armazenar uma coleção de produtos.</span><span class="sxs-lookup"><span data-stu-id="e0e43-179">We need to store a collection of products.</span></span> <span data-ttu-id="e0e43-180">É uma boa ideia separar a coleção da nossa implementação de serviço.</span><span class="sxs-lookup"><span data-stu-id="e0e43-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="e0e43-181">Dessa forma, podemos alterar o repositório de backup sem reescrever a classe de serviço.</span><span class="sxs-lookup"><span data-stu-id="e0e43-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="e0e43-182">Esse tipo de design é chamado de *repositório* padrão.</span><span class="sxs-lookup"><span data-stu-id="e0e43-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="e0e43-183">Comece definindo uma interface genérica para o repositório.</span><span class="sxs-lookup"><span data-stu-id="e0e43-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="e0e43-184">No Gerenciador de soluções, clique com botão direito do **modelos** pasta.</span><span class="sxs-lookup"><span data-stu-id="e0e43-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="e0e43-185">Selecione **Add**, em seguida, selecione **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="e0e43-186">No **modelos** painel, selecione **modelos instalados** e expanda o nó do c#.</span><span class="sxs-lookup"><span data-stu-id="e0e43-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="e0e43-187">Em c#, selecione **código**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-187">Under C#, select **Code**.</span></span> <span data-ttu-id="e0e43-188">Na lista de modelos de código, selecione **Interface**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="e0e43-189">Nomeie a interface &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="e0e43-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="e0e43-190">Adicione a seguinte implementação:</span><span class="sxs-lookup"><span data-stu-id="e0e43-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="e0e43-191">Agora, adicione outra classe para a pasta de modelos, denominada &quot;ProductRepository.&quot; Essa classe implementará a interface `IProductRespository`.</span><span class="sxs-lookup"><span data-stu-id="e0e43-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="e0e43-192">Adicione a seguinte implementação:</span><span class="sxs-lookup"><span data-stu-id="e0e43-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="e0e43-193">O repositório mantém a lista na memória local.</span><span class="sxs-lookup"><span data-stu-id="e0e43-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="e0e43-194">Isso é Okey para obter um tutorial, mas em um aplicativo real, você poderia armazenar os dados externamente, um banco de dados ou no armazenamento em nuvem.</span><span class="sxs-lookup"><span data-stu-id="e0e43-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="e0e43-195">O padrão de repositório tornará mais fácil alterar a implementação mais tarde.</span><span class="sxs-lookup"><span data-stu-id="e0e43-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="e0e43-196">Adicionando um controlador de API da Web</span><span class="sxs-lookup"><span data-stu-id="e0e43-196">Adding a Web API Controller</span></span>

<span data-ttu-id="e0e43-197">Se você tiver trabalhado com o ASP.NET MVC, em seguida, você já está familiarizados com os controladores.</span><span class="sxs-lookup"><span data-stu-id="e0e43-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="e0e43-198">Na API Web ASP.NET, uma *controlador* é uma classe que manipula as solicitações HTTP do cliente.</span><span class="sxs-lookup"><span data-stu-id="e0e43-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="e0e43-199">O Assistente de novo projeto criado dois controladores para você quando ele criou o projeto.</span><span class="sxs-lookup"><span data-stu-id="e0e43-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="e0e43-200">Para vê-las, expanda a pasta de controladores no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="e0e43-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="e0e43-201">HomeController é um controlador de MVC do ASP.NET tradicional.</span><span class="sxs-lookup"><span data-stu-id="e0e43-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="e0e43-202">Ele é responsável por fornecer páginas HTML para o site e não está diretamente relacionado à nossa API da web.</span><span class="sxs-lookup"><span data-stu-id="e0e43-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="e0e43-203">ValuesController é um controlador de API da Web de exemplo.</span><span class="sxs-lookup"><span data-stu-id="e0e43-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="e0e43-204">Vá em frente e exclua ValuesController, clicando duas vezes no arquivo no Gerenciador de soluções e selecionando **excluir.**</span><span class="sxs-lookup"><span data-stu-id="e0e43-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="e0e43-205">Agora adicione um novo controlador, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="e0e43-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="e0e43-206">Em **Gerenciador de Soluções**, clique com o botão direito na pasta controllers (controladores). </span><span class="sxs-lookup"><span data-stu-id="e0e43-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="e0e43-207">Selecione **Adicionar** e, em seguida, selecione **Controlador**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="e0e43-208">No **Adicionar controlador** assistente, nomeie o controlador &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="e0e43-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="e0e43-209">No **modelo** lista suspensa, selecione **controlador API vazio**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="e0e43-210">Em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="e0e43-211">Não é necessário colocar seus controladores em uma pasta chamada controladores.</span><span class="sxs-lookup"><span data-stu-id="e0e43-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="e0e43-212">O nome da pasta não é importante; é simplesmente uma maneira conveniente de organizar seus arquivos de origem.</span><span class="sxs-lookup"><span data-stu-id="e0e43-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="e0e43-213">O **Adicionar controlador** assistente criará um arquivo chamado ProductsController.cs na pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="e0e43-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="e0e43-214">Se esse arquivo ainda não estiver aberto, clique duas vezes no arquivo para abri-lo.</span><span class="sxs-lookup"><span data-stu-id="e0e43-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="e0e43-215">Adicione o seguinte **usando** instrução:</span><span class="sxs-lookup"><span data-stu-id="e0e43-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="e0e43-216">Adicionar um campo que contém um **IProductRepository** instância.</span><span class="sxs-lookup"><span data-stu-id="e0e43-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="e0e43-217">Chamando `new ProductRepository()` no controlador não é o melhor design, porque ele liga o controlador para uma implementação específica de `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="e0e43-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="e0e43-218">Para uma abordagem melhor, consulte [usando o resolvedor de dependência do Web API](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="e0e43-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="e0e43-219">Introdução a um recurso</span><span class="sxs-lookup"><span data-stu-id="e0e43-219">Getting a Resource</span></span>

<span data-ttu-id="e0e43-220">A API ProductStore irá expor várias &quot;ler&quot; ações como métodos HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="e0e43-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="e0e43-221">Cada ação corresponderá a um método no `ProductsController` classe.</span><span class="sxs-lookup"><span data-stu-id="e0e43-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="e0e43-222">Ação</span><span class="sxs-lookup"><span data-stu-id="e0e43-222">Action</span></span> | <span data-ttu-id="e0e43-223">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="e0e43-223">HTTP method</span></span> | <span data-ttu-id="e0e43-224">URI relativo</span><span class="sxs-lookup"><span data-stu-id="e0e43-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e0e43-225">Obter uma lista de todos os produtos</span><span class="sxs-lookup"><span data-stu-id="e0e43-225">Get a list of all products</span></span> | <span data-ttu-id="e0e43-226">OBTER</span><span class="sxs-lookup"><span data-stu-id="e0e43-226">GET</span></span> | <span data-ttu-id="e0e43-227">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="e0e43-227">/api/products</span></span> |
| <span data-ttu-id="e0e43-228">Obter um produto por ID</span><span class="sxs-lookup"><span data-stu-id="e0e43-228">Get a product by ID</span></span> | <span data-ttu-id="e0e43-229">OBTER</span><span class="sxs-lookup"><span data-stu-id="e0e43-229">GET</span></span> | <span data-ttu-id="e0e43-230">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="e0e43-230">/api/products/*id*</span></span> |
| <span data-ttu-id="e0e43-231">Obter um produto por categoria</span><span class="sxs-lookup"><span data-stu-id="e0e43-231">Get a product by category</span></span> | <span data-ttu-id="e0e43-232">OBTER</span><span class="sxs-lookup"><span data-stu-id="e0e43-232">GET</span></span> | <span data-ttu-id="e0e43-233">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="e0e43-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="e0e43-234">Para obter a lista de todos os produtos, adicione este método para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="e0e43-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="e0e43-235">O nome do método começa com &quot;obter&quot;, portanto, por convenção, ele é mapeado para solicitações GET.</span><span class="sxs-lookup"><span data-stu-id="e0e43-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="e0e43-236">Além disso, porque o método não tiver parâmetros, ele é mapeado para um URI que não contém um *&quot;id&quot;* segmento no caminho.</span><span class="sxs-lookup"><span data-stu-id="e0e43-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="e0e43-237">Para obter um produto por ID, adicione este método para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="e0e43-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="e0e43-238">Esse nome de método também começa com &quot;Obtenha&quot;, mas o método tem um parâmetro denominado *id*. Esse parâmetro é mapeado para o &quot;id&quot; segmento do caminho do URI.</span><span class="sxs-lookup"><span data-stu-id="e0e43-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="e0e43-239">A estrutura da API Web ASP.NET converte automaticamente a ID para o tipo de dados correto (**int**) para o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="e0e43-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="e0e43-240">O método GetProduct lança uma exceção do tipo **HttpResponseException** se *id* não é válido.</span><span class="sxs-lookup"><span data-stu-id="e0e43-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="e0e43-241">Essa exceção será convertida pela estrutura em um erro 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="e0e43-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="e0e43-242">Por fim, adicione um método para localizar produtos por categoria:</span><span class="sxs-lookup"><span data-stu-id="e0e43-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="e0e43-243">Se o URI da solicitação tem uma cadeia de caracteres de consulta, API da Web tenta corresponder os parâmetros de consulta para parâmetros do método do controlador.</span><span class="sxs-lookup"><span data-stu-id="e0e43-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="e0e43-244">Portanto, um URI do formulário "api/produtos? categoria =*categoria*" será mapeado para esse método.</span><span class="sxs-lookup"><span data-stu-id="e0e43-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="e0e43-245">Criar um recurso</span><span class="sxs-lookup"><span data-stu-id="e0e43-245">Creating a Resource</span></span>

<span data-ttu-id="e0e43-246">Em seguida, adicionaremos um método para o `ProductsController` classe para criar um novo produto.</span><span class="sxs-lookup"><span data-stu-id="e0e43-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="e0e43-247">Aqui está uma implementação simples do método:</span><span class="sxs-lookup"><span data-stu-id="e0e43-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="e0e43-248">Observe duas coisas sobre esse método:</span><span class="sxs-lookup"><span data-stu-id="e0e43-248">Note two things about this method:</span></span>

- <span data-ttu-id="e0e43-249">O nome do método começa com &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="e0e43-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="e0e43-250">Para criar um novo produto, o cliente envia uma solicitação HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="e0e43-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="e0e43-251">O método utiliza um parâmetro de tipo de produto.</span><span class="sxs-lookup"><span data-stu-id="e0e43-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="e0e43-252">Na API da Web, os parâmetros com tipos complexos são desserializados do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="e0e43-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="e0e43-253">Portanto, esperamos que o cliente envie uma representação serializada de um objeto de produto, em formato XML ou JSON.</span><span class="sxs-lookup"><span data-stu-id="e0e43-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="e0e43-254">Essa implementação funcionará, mas não está completa.</span><span class="sxs-lookup"><span data-stu-id="e0e43-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="e0e43-255">Idealmente, gostaríamos de resposta HTTP para incluir o seguinte:</span><span class="sxs-lookup"><span data-stu-id="e0e43-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="e0e43-256">**Código de resposta:** por padrão, a estrutura da API Web define o código de status de resposta a 200 (Okey).</span><span class="sxs-lookup"><span data-stu-id="e0e43-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="e0e43-257">Mas de acordo com o protocolo HTTP/1.1, quando uma solicitação POST resulta na criação de um recurso, o servidor deverá responder com status 201 (criado).</span><span class="sxs-lookup"><span data-stu-id="e0e43-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="e0e43-258">**Local:** quando o servidor cria um recurso, ele deve incluir o URI do novo recurso no cabeçalho Location da resposta.</span><span class="sxs-lookup"><span data-stu-id="e0e43-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="e0e43-259">API Web ASP.NET torna fácil manipular a mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="e0e43-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="e0e43-260">Aqui está a implementação aprimorada:</span><span class="sxs-lookup"><span data-stu-id="e0e43-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="e0e43-261">Observe que o tipo de retorno do método agora é **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="e0e43-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="e0e43-262">Retornando um **HttpResponseMessage** em vez de um produto, podemos controlar os detalhes da mensagem de resposta HTTP, incluindo o código de status e o cabeçalho de localização.</span><span class="sxs-lookup"><span data-stu-id="e0e43-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="e0e43-263">O **CreateResponse** método cria um **HttpResponseMessage** e gravará automaticamente uma representação serializada do objeto Product no corpo da fo a mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="e0e43-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="e0e43-264">Este exemplo não valida o `Product`.</span><span class="sxs-lookup"><span data-stu-id="e0e43-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="e0e43-265">Para obter informações sobre a validação de modelo, consulte [validação do modelo na API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e0e43-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="e0e43-266">Atualizar um recurso</span><span class="sxs-lookup"><span data-stu-id="e0e43-266">Updating a Resource</span></span>

<span data-ttu-id="e0e43-267">Atualizar um produto com PUT é simples:</span><span class="sxs-lookup"><span data-stu-id="e0e43-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="e0e43-268">O nome do método começa com &quot;colocar... &quot;, portanto, a API da Web corresponde a ele para solicitações PUT.</span><span class="sxs-lookup"><span data-stu-id="e0e43-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="e0e43-269">O método utiliza dois parâmetros, a ID do produto e o produto atualizado.</span><span class="sxs-lookup"><span data-stu-id="e0e43-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="e0e43-270">O *identificação* parâmetro é obtido do caminho do URI e o *produto* parâmetro é desserializado do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="e0e43-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="e0e43-271">Por padrão, a estrutura da API Web do ASP.NET usa tipos de parâmetro simples da rota e tipos complexos do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="e0e43-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="e0e43-272">Excluir um recurso</span><span class="sxs-lookup"><span data-stu-id="e0e43-272">Deleting a Resource</span></span>

<span data-ttu-id="e0e43-273">Para excluir um resourse, defina um método de "Excluir...".</span><span class="sxs-lookup"><span data-stu-id="e0e43-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="e0e43-274">Se uma solicitação de exclusão for bem-sucedida, ela pode retornar o status 200 (Okey) com um corpo de entidade que descreve o status; status 202 (aceito) se a exclusão ainda estiver pendente; ou de status 204 (sem conteúdo) sem o corpo da entidade.</span><span class="sxs-lookup"><span data-stu-id="e0e43-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="e0e43-275">Nesse caso, o `DeleteProduct` método tem um `void` tipo de retorno, portanto, a API Web ASP.NET automaticamente converte isso em status 204 (sem conteúdo) de código.</span><span class="sxs-lookup"><span data-stu-id="e0e43-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
