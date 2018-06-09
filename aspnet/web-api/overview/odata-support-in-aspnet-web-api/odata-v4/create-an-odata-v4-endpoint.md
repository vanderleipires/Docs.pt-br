---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Criar um ponto de extremidade OData v4 usando ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: O Open Data Protocol (OData) é um protocolo de acesso de dados para a web. O OData fornece uma maneira uniforme para consultar e manipular os conjuntos de dados por meio de operações CRUD...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508045"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="6416a-104">Criar um ponto de extremidade OData v4 usando ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="6416a-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="6416a-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6416a-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="6416a-106">O Open Data Protocol (OData) é um protocolo de acesso de dados para a web.</span><span class="sxs-lookup"><span data-stu-id="6416a-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="6416a-107">O OData fornece uma maneira uniforme para consultar e manipular os conjuntos de dados por meio de operações CRUD (criar, ler, atualizar e excluir).</span><span class="sxs-lookup"><span data-stu-id="6416a-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
> 
> <span data-ttu-id="6416a-108">Dá suporte a ASP.NET Web API v3 e v4 do protocolo.</span><span class="sxs-lookup"><span data-stu-id="6416a-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="6416a-109">Você pode ter um ponto de extremidade v4 executa lado a lado com um ponto de extremidade v3.</span><span class="sxs-lookup"><span data-stu-id="6416a-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
> 
> <span data-ttu-id="6416a-110">Este tutorial mostra como criar um ponto de extremidade de v4 do OData que suporta operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="6416a-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6416a-111">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="6416a-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6416a-112">2.2 da API da Web</span><span class="sxs-lookup"><span data-stu-id="6416a-112">Web API 2.2</span></span>
> - <span data-ttu-id="6416a-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="6416a-113">OData v4</span></span>
> - [<span data-ttu-id="6416a-114">Visual Studio 2013 Atualização 2</span><span class="sxs-lookup"><span data-stu-id="6416a-114">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="6416a-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6416a-115">Entity Framework 6</span></span>
> - <span data-ttu-id="6416a-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6416a-116">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="6416a-117">Versões do tutoriais</span><span class="sxs-lookup"><span data-stu-id="6416a-117">Tutorial versions</span></span>
> 
> <span data-ttu-id="6416a-118">Para o OData versão 3, consulte [criar um ponto de extremidade OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="6416a-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="6416a-119">Criar o projeto do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6416a-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="6416a-120">No Visual Studio, do **arquivo** menu, selecione **novo** &gt; **projeto**.</span><span class="sxs-lookup"><span data-stu-id="6416a-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="6416a-121">Expanda **instalado** &gt; **modelos** &gt; **Visual C#** &gt; **Web**e selecione o  **Aplicativo Web ASP.NET** modelo.</span><span class="sxs-lookup"><span data-stu-id="6416a-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="6416a-122">Nomeie o projeto &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="6416a-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="6416a-123">No **novo projeto** caixa de diálogo, selecione o **vazio** modelo.</span><span class="sxs-lookup"><span data-stu-id="6416a-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="6416a-124">Em &quot;adicionar pastas e referências de núcleo... &quot;, clique em **API da Web**.</span><span class="sxs-lookup"><span data-stu-id="6416a-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="6416a-125">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="6416a-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="6416a-126">Instalar os pacotes de OData</span><span class="sxs-lookup"><span data-stu-id="6416a-126">Install the OData Packages</span></span>

<span data-ttu-id="6416a-127">Do **ferramentas** menu, selecione **NuGet Package Manager** &gt; **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="6416a-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="6416a-128">Na janela do Console do Gerenciador de pacote, digite:</span><span class="sxs-lookup"><span data-stu-id="6416a-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="6416a-129">Esse comando instala os pacotes de OData NuGet mais recentes.</span><span class="sxs-lookup"><span data-stu-id="6416a-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="6416a-130">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="6416a-130">Add a Model Class</span></span>

<span data-ttu-id="6416a-131">Um *modelo* é um objeto que representa uma entidade de dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6416a-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="6416a-132">No Gerenciador de Soluções, clique com o botão direito na pasta de modelos (Models).</span><span class="sxs-lookup"><span data-stu-id="6416a-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="6416a-133">No menu de contexto, selecione **adicionar** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="6416a-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="6416a-134">Por convenção, as classes de modelo são colocadas na pasta de modelos, mas você não precisa seguir essa convenção em seus próprios projetos.</span><span class="sxs-lookup"><span data-stu-id="6416a-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="6416a-135">Nomeie a classe `Product`.</span><span class="sxs-lookup"><span data-stu-id="6416a-135">Name the class `Product`.</span></span> <span data-ttu-id="6416a-136">No arquivo Product.cs, substitua o código clichê com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="6416a-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="6416a-137">O `Id` propriedade é a chave de entidade.</span><span class="sxs-lookup"><span data-stu-id="6416a-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="6416a-138">Os clientes podem consultar entidades por chave.</span><span class="sxs-lookup"><span data-stu-id="6416a-138">Clients can query entities by key.</span></span> <span data-ttu-id="6416a-139">Por exemplo, para obter o produto com ID 5, o URI é `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="6416a-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="6416a-140">O `Id` propriedade também será a chave primária no banco de dados back-end.</span><span class="sxs-lookup"><span data-stu-id="6416a-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="6416a-141">Habilitar o Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6416a-141">Enable Entity Framework</span></span>

<span data-ttu-id="6416a-142">Para este tutorial, usaremos Entity Framework (EF) Code First para criar o banco de dados de back-end.</span><span class="sxs-lookup"><span data-stu-id="6416a-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="6416a-143">OData da API da Web não requerem EF.</span><span class="sxs-lookup"><span data-stu-id="6416a-143">Web API OData does not require EF.</span></span> <span data-ttu-id="6416a-144">Use qualquer camada de acesso a dados que pode ser traduzidos entidades de banco de dados em modelos.</span><span class="sxs-lookup"><span data-stu-id="6416a-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="6416a-145">Primeiro, instale o pacote NuGet para EF.</span><span class="sxs-lookup"><span data-stu-id="6416a-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="6416a-146">Do **ferramentas** menu, selecione **NuGet Package Manager** &gt; **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="6416a-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="6416a-147">Na janela do Console do Gerenciador de pacote, digite:</span><span class="sxs-lookup"><span data-stu-id="6416a-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="6416a-148">Abra o arquivo Web. config e adicione a seguinte seção dentro de **configuração** elemento, após o **configSections** elemento.</span><span class="sxs-lookup"><span data-stu-id="6416a-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="6416a-149">Essa configuração adiciona uma cadeia de caracteres de conexão para um banco de dados LocalDB.</span><span class="sxs-lookup"><span data-stu-id="6416a-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="6416a-150">Este banco de dados será usado quando você executar o aplicativo localmente.</span><span class="sxs-lookup"><span data-stu-id="6416a-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="6416a-151">Em seguida, adicione uma classe denominada `ProductsContext` para a pasta de modelos:</span><span class="sxs-lookup"><span data-stu-id="6416a-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="6416a-152">No construtor, `"name=ProductsContext"` fornece o nome da cadeia de caracteres de conexão.</span><span class="sxs-lookup"><span data-stu-id="6416a-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="6416a-153">Configurar o ponto de extremidade OData</span><span class="sxs-lookup"><span data-stu-id="6416a-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="6416a-154">Abra o arquivo de aplicativo\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="6416a-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="6416a-155">Adicione o seguinte **usando** instruções:</span><span class="sxs-lookup"><span data-stu-id="6416a-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="6416a-156">Em seguida, adicione o seguinte código para o **registrar** método:</span><span class="sxs-lookup"><span data-stu-id="6416a-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="6416a-157">Este código faz duas coisas:</span><span class="sxs-lookup"><span data-stu-id="6416a-157">This code does two things:</span></span>

- <span data-ttu-id="6416a-158">Cria um modelo de dados de entidade (EDM).</span><span class="sxs-lookup"><span data-stu-id="6416a-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="6416a-159">Adiciona uma rota.</span><span class="sxs-lookup"><span data-stu-id="6416a-159">Adds a route.</span></span>

<span data-ttu-id="6416a-160">Um EDM é um modelo abstrato dos dados.</span><span class="sxs-lookup"><span data-stu-id="6416a-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="6416a-161">EDM é usado para criar o documento de metadados do serviço.</span><span class="sxs-lookup"><span data-stu-id="6416a-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="6416a-162">O **ODataConventionModelBuilder** classe cria um EDM usando convenções de nomenclatura padrão.</span><span class="sxs-lookup"><span data-stu-id="6416a-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="6416a-163">Essa abordagem exige o mínimo de código.</span><span class="sxs-lookup"><span data-stu-id="6416a-163">This approach requires the least code.</span></span> <span data-ttu-id="6416a-164">Se você quiser mais controle sobre o EDM, você pode usar o **ODataModelBuilder** classe para criar o EDM Adicionando propriedades, chaves e propriedades de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="6416a-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="6416a-165">Um *rota* informa como rotear solicitações HTTP para o ponto de extremidade de API da Web.</span><span class="sxs-lookup"><span data-stu-id="6416a-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="6416a-166">Para criar uma rota do OData v4, chame o **MapODataServiceRoute** método de extensão.</span><span class="sxs-lookup"><span data-stu-id="6416a-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="6416a-167">Se seu aplicativo tiver vários pontos de extremidade do OData, crie uma rota separada para cada um.</span><span class="sxs-lookup"><span data-stu-id="6416a-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="6416a-168">Dê a cada rota de um nome de rota exclusivo e um prefixo.</span><span class="sxs-lookup"><span data-stu-id="6416a-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="6416a-169">Adicionar o controlador de OData</span><span class="sxs-lookup"><span data-stu-id="6416a-169">Add the OData Controller</span></span>

<span data-ttu-id="6416a-170">Um *controlador* é uma classe que trata as solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="6416a-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="6416a-171">Você criar um controlador separado para cada entidade definida no seu serviço OData.</span><span class="sxs-lookup"><span data-stu-id="6416a-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="6416a-172">Neste tutorial, você criará um controlador, para o `Product` entidade.</span><span class="sxs-lookup"><span data-stu-id="6416a-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="6416a-173">No Gerenciador de soluções, clique na pasta controladores e selecione **adicionar** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="6416a-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="6416a-174">Nomeie a classe `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="6416a-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="6416a-175">A versão deste tutorial para OData v3 usa o **Adicionar controlador** scaffolding.</span><span class="sxs-lookup"><span data-stu-id="6416a-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="6416a-176">Atualmente, não há nenhum scaffolding para OData v4.</span><span class="sxs-lookup"><span data-stu-id="6416a-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="6416a-177">Substitua o código de texto clichê em ProductsController.cs com o seguinte.</span><span class="sxs-lookup"><span data-stu-id="6416a-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="6416a-178">O controlador usa o `ProductsContext` classe para acessar o banco de dados usando EF.</span><span class="sxs-lookup"><span data-stu-id="6416a-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="6416a-179">Observe que o controlador substitui o **Dispose** método para descartar o **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="6416a-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="6416a-180">Este é o ponto de partida para o controlador.</span><span class="sxs-lookup"><span data-stu-id="6416a-180">This is the starting point for the controller.</span></span> <span data-ttu-id="6416a-181">Em seguida, vamos adicionar métodos para todas as operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="6416a-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="6416a-182">Consultando o conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="6416a-182">Querying the Entity Set</span></span>

<span data-ttu-id="6416a-183">Adicione os seguintes métodos para `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="6416a-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="6416a-184">A versão para o `Get` método retorna toda a coleção de produtos.</span><span class="sxs-lookup"><span data-stu-id="6416a-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="6416a-185">O `Get` método com um *chave* parâmetro procura um produto por sua chave (nesse caso, o `Id` propriedade).</span><span class="sxs-lookup"><span data-stu-id="6416a-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="6416a-186">O **[EnableQuery]** atributo permite que os clientes modificar a consulta, usando as opções de consulta, como $filter, $sort e $page.</span><span class="sxs-lookup"><span data-stu-id="6416a-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="6416a-187">Para obter mais informações, consulte [dando suporte a opções de consulta OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="6416a-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="6416a-188">Adicionando uma entidade para o conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="6416a-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="6416a-189">Para habilitar clientes para adicionar um novo produto no banco de dados, adicione o seguinte método para `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="6416a-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="6416a-190">Atualizando uma entidade</span><span class="sxs-lookup"><span data-stu-id="6416a-190">Updating an Entity</span></span>

<span data-ttu-id="6416a-191">OData oferece suporte a dois semânticas diferentes para atualizar uma entidade, PATCH e PUT.</span><span class="sxs-lookup"><span data-stu-id="6416a-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="6416a-192">PATCH executa uma atualização parcial.</span><span class="sxs-lookup"><span data-stu-id="6416a-192">PATCH performs a partial update.</span></span> <span data-ttu-id="6416a-193">O cliente especifica apenas as propriedades para atualizar.</span><span class="sxs-lookup"><span data-stu-id="6416a-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="6416a-194">PUT substitui a entidade inteira.</span><span class="sxs-lookup"><span data-stu-id="6416a-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="6416a-195">A desvantagem de PUT é que o cliente deve enviar valores para todas as propriedades da entidade, incluindo os valores que não estão sendo alteradas.</span><span class="sxs-lookup"><span data-stu-id="6416a-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="6416a-196">O [especificação OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) afirma que o PATCH é preferencial.</span><span class="sxs-lookup"><span data-stu-id="6416a-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="6416a-197">Em qualquer caso, aqui está o código para os métodos de PATCH e PUT:</span><span class="sxs-lookup"><span data-stu-id="6416a-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="6416a-198">No caso de PATCH, o controlador usará o **Delta&lt;T&gt;**  tipo para controlar as alterações.</span><span class="sxs-lookup"><span data-stu-id="6416a-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="6416a-199">Excluindo uma entidade</span><span class="sxs-lookup"><span data-stu-id="6416a-199">Deleting an Entity</span></span>

<span data-ttu-id="6416a-200">Para habilitar clientes para excluir um produto do banco de dados, adicione o seguinte método para `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="6416a-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
