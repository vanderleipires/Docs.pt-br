---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Criar um ponto de extremidade OData v3 com Web API 2 | Microsoft Docs
author: MikeWasson
description: "O Open Data Protocol (OData) é um protocolo de acesso de dados para a web. O OData fornece uma maneira uniforme de estrutura de dados, consultar os dados e manipular os dados..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 33fe4d764bf9bf64c852f1269255925b5cc42536
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2018
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="c4687-104">Criar um ponto de extremidade OData v3 com Web API 2</span><span class="sxs-lookup"><span data-stu-id="c4687-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="c4687-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c4687-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c4687-106">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="c4687-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="c4687-107">O [Open Data Protocol](http://www.odata.org/) (OData) é um protocolo de acesso de dados para a web.</span><span class="sxs-lookup"><span data-stu-id="c4687-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="c4687-108">O OData fornece uma maneira uniforme de estrutura de dados, consultar os dados e manipular o conjunto de dados por meio de operações CRUD (criar, ler, atualizar e excluir).</span><span class="sxs-lookup"><span data-stu-id="c4687-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="c4687-109">OData oferece suporte a formatos JSON e AtomPub (XML).</span><span class="sxs-lookup"><span data-stu-id="c4687-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="c4687-110">OData define também uma maneira para expor metadados sobre os dados.</span><span class="sxs-lookup"><span data-stu-id="c4687-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="c4687-111">Os clientes podem usar os metadados para descobrir as informações de tipo e as relações para o conjunto de dados.</span><span class="sxs-lookup"><span data-stu-id="c4687-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
> 
> <span data-ttu-id="c4687-112">ASP.NET Web API torna fácil criar um ponto de extremidade OData para um conjunto de dados.</span><span class="sxs-lookup"><span data-stu-id="c4687-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="c4687-113">Você pode controlar exatamente quais operações OData oferece suporte a ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="c4687-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="c4687-114">Você pode hospedar vários pontos de extremidade OData, juntamente com pontos de extremidade OData não.</span><span class="sxs-lookup"><span data-stu-id="c4687-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="c4687-115">Você tem controle total sobre o modelo de dados, a lógica de negócios de back-end e a camada de dados.</span><span class="sxs-lookup"><span data-stu-id="c4687-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c4687-116">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="c4687-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="c4687-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c4687-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="c4687-118">Web API 2</span><span class="sxs-lookup"><span data-stu-id="c4687-118">Web API 2</span></span>
> - <span data-ttu-id="c4687-119">OData versão 3</span><span class="sxs-lookup"><span data-stu-id="c4687-119">OData Version 3</span></span>
> - <span data-ttu-id="c4687-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c4687-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="c4687-121">O Fiddler Web Proxy (opcional) de depuração</span><span class="sxs-lookup"><span data-stu-id="c4687-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
> 
> <span data-ttu-id="c4687-122">Suporte de OData da API da Web foi adicionado na [ASP.NET e Web Tools 2012.2 atualização](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="c4687-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="c4687-123">No entanto, este tutorial usa scaffolding foi adicionado no Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c4687-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="c4687-124">Neste tutorial, você criará um ponto de extremidade OData simple que os clientes podem consultar.</span><span class="sxs-lookup"><span data-stu-id="c4687-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="c4687-125">Você também criará um cliente c# para o ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="c4687-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="c4687-126">Depois de concluir este tutorial, o próximo conjunto de tutoriais mostram como adicionar mais funcionalidade, incluindo as relações de entidade, ações, e selecione $Expandir / $.</span><span class="sxs-lookup"><span data-stu-id="c4687-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="c4687-127">Criar o projeto do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4687-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="c4687-128">Adicionar um modelo de entidade</span><span class="sxs-lookup"><span data-stu-id="c4687-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="c4687-129">Adicionar um controlador OData</span><span class="sxs-lookup"><span data-stu-id="c4687-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="c4687-130">Adicione o EDM e rota</span><span class="sxs-lookup"><span data-stu-id="c4687-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="c4687-131">Propagação do banco de dados (opcional)</span><span class="sxs-lookup"><span data-stu-id="c4687-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="c4687-132">Explorando o ponto de extremidade OData</span><span class="sxs-lookup"><span data-stu-id="c4687-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="c4687-133">Formatos de serialização do OData</span><span class="sxs-lookup"><span data-stu-id="c4687-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="c4687-134">Criar o projeto do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4687-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="c4687-135">Neste tutorial, você criará um ponto de extremidade OData que suporta operações CRUD básicas.</span><span class="sxs-lookup"><span data-stu-id="c4687-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="c4687-136">O ponto de extremidade irá expor um único recurso, uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="c4687-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="c4687-137">Tutoriais posteriores adicionará mais recursos.</span><span class="sxs-lookup"><span data-stu-id="c4687-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="c4687-138">Inicie o Visual Studio e selecione **novo projeto** na página de início.</span><span class="sxs-lookup"><span data-stu-id="c4687-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="c4687-139">Ou, do **arquivo** menu, selecione **novo** e **projeto**.</span><span class="sxs-lookup"><span data-stu-id="c4687-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="c4687-140">No **modelos** painel, selecione **modelos instalados** e expanda o nó do Visual c#.</span><span class="sxs-lookup"><span data-stu-id="c4687-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="c4687-141">Em **Visual C#**, selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="c4687-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c4687-142">Selecione **aplicativo Web ASP.NET** modelo.</span><span class="sxs-lookup"><span data-stu-id="c4687-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="c4687-143">No **novo projeto ASP.NET** caixa de diálogo, selecione o **vazio** modelo.</span><span class="sxs-lookup"><span data-stu-id="c4687-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="c4687-144">Em &quot;adicionar pastas e referências de núcleo... &quot;, verifique **API da Web**.</span><span class="sxs-lookup"><span data-stu-id="c4687-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="c4687-145">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4687-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="c4687-146">Adicionar um modelo de entidade</span><span class="sxs-lookup"><span data-stu-id="c4687-146">Add an Entity Model</span></span>

<span data-ttu-id="c4687-147">Um *modelo* é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c4687-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="c4687-148">Para este tutorial, precisamos de um modelo que representa um produto.</span><span class="sxs-lookup"><span data-stu-id="c4687-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="c4687-149">O modelo corresponde ao nosso tipo de entidade do OData.</span><span class="sxs-lookup"><span data-stu-id="c4687-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="c4687-150">No Gerenciador de soluções, clique na pasta de modelos.</span><span class="sxs-lookup"><span data-stu-id="c4687-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="c4687-151">No menu de contexto, selecione **adicionar** , em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="c4687-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="c4687-152">No **adicionar novo** Item de caixa de diálogo, nomeie a classe &quot;produto&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4687-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="c4687-153">Por convenção, as classes de modelo são colocados na pasta modelos.</span><span class="sxs-lookup"><span data-stu-id="c4687-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="c4687-154">Você não precisa seguir essa convenção em seus projetos, mas usaremos para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="c4687-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="c4687-155">No arquivo Product.cs, adicione a seguinte definição de classe:</span><span class="sxs-lookup"><span data-stu-id="c4687-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="c4687-156">A propriedade ID será a chave de entidade.</span><span class="sxs-lookup"><span data-stu-id="c4687-156">The ID property will be the entity key.</span></span> <span data-ttu-id="c4687-157">Os clientes podem consultar os produtos por ID.</span><span class="sxs-lookup"><span data-stu-id="c4687-157">Clients can query products by ID.</span></span> <span data-ttu-id="c4687-158">Este campo também seria a chave primária no banco de dados back-end.</span><span class="sxs-lookup"><span data-stu-id="c4687-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="c4687-159">Compile o projeto agora.</span><span class="sxs-lookup"><span data-stu-id="c4687-159">Build the project now.</span></span> <span data-ttu-id="c4687-160">Na próxima etapa, vamos usar alguns scaffolding do Visual Studio que usa reflexão para encontrar o tipo de produto.</span><span class="sxs-lookup"><span data-stu-id="c4687-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="c4687-161">Adicionar um controlador OData</span><span class="sxs-lookup"><span data-stu-id="c4687-161">Add an OData Controller</span></span>

<span data-ttu-id="c4687-162">Um *controlador* é uma classe que trata as solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4687-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="c4687-163">Você definir um controlador separado para cada entidade definida no serviço OData.</span><span class="sxs-lookup"><span data-stu-id="c4687-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="c4687-164">Neste tutorial, vamos criar um único controlador.</span><span class="sxs-lookup"><span data-stu-id="c4687-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="c4687-165">No Gerenciador de soluções, clique na pasta de controladores.</span><span class="sxs-lookup"><span data-stu-id="c4687-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="c4687-166">Selecione **adicionar** e, em seguida, selecione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="c4687-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="c4687-167">No **adicionar Scaffold** caixa de diálogo, selecione &quot;Web API 2 OData controlador com ações, usando o Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4687-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="c4687-168">No **Adicionar controlador** caixa de diálogo, o nome do controlador "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="c4687-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="c4687-169">Selecione o &quot;usar ações assíncronas do controlador&quot; caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="c4687-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="c4687-170">No **modelo** lista suspensa, selecione a classe do produto.</span><span class="sxs-lookup"><span data-stu-id="c4687-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="c4687-171">Clique o **novo contexto de dados...**  botão.</span><span class="sxs-lookup"><span data-stu-id="c4687-171">Click the **New data context...** button.</span></span> <span data-ttu-id="c4687-172">Deixe o nome padrão para o tipo de contexto de dados e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="c4687-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="c4687-173">Na caixa de diálogo Adicionar controlador para adicionar o controlador, clique em Adicionar.</span><span class="sxs-lookup"><span data-stu-id="c4687-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="c4687-174">Observação: Se você receber uma mensagem de erro que diz &quot;Ocorreu um erro ao obter o tipo... &quot;, certifique-se de que você criou o projeto do Visual Studio depois que você adicionou a classe do produto.</span><span class="sxs-lookup"><span data-stu-id="c4687-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="c4687-175">O scaffolding usa reflexão para localizar a classe.</span><span class="sxs-lookup"><span data-stu-id="c4687-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="c4687-176">O scaffolding adiciona dois arquivos de código ao projeto:</span><span class="sxs-lookup"><span data-stu-id="c4687-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="c4687-177">Products.cs define o controlador de API da Web que implementa o ponto de extremidade OData.</span><span class="sxs-lookup"><span data-stu-id="c4687-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="c4687-178">ProductServiceContext.cs fornece métodos para consultar o banco de dados subjacente, usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c4687-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="c4687-179">Adicione o EDM e rota</span><span class="sxs-lookup"><span data-stu-id="c4687-179">Add the EDM and Route</span></span>

<span data-ttu-id="c4687-180">No Gerenciador de soluções, expanda o aplicativo\_pasta e abra o arquivo chamado WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="c4687-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="c4687-181">Essa classe contém o código de configuração para a API da Web.</span><span class="sxs-lookup"><span data-stu-id="c4687-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="c4687-182">Substitua esse código pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="c4687-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="c4687-183">Este código faz duas coisas:</span><span class="sxs-lookup"><span data-stu-id="c4687-183">This code does two things:</span></span>

- <span data-ttu-id="c4687-184">Cria uma entidade de dados EDM (modelo) para o ponto de extremidade OData.</span><span class="sxs-lookup"><span data-stu-id="c4687-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="c4687-185">Adiciona uma rota para o ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="c4687-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="c4687-186">Um EDM é um modelo abstrato dos dados.</span><span class="sxs-lookup"><span data-stu-id="c4687-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="c4687-187">EDM é usado para criar o documento de metadados e definir os URIs para o serviço.</span><span class="sxs-lookup"><span data-stu-id="c4687-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="c4687-188">O **ODataConventionModelBuilder** cria um EDM usando um conjunto de convenções de nomenclatura padrão EDM.</span><span class="sxs-lookup"><span data-stu-id="c4687-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="c4687-189">Essa abordagem exige o mínimo de código.</span><span class="sxs-lookup"><span data-stu-id="c4687-189">This approach requires the least code.</span></span> <span data-ttu-id="c4687-190">Se você quiser mais controle sobre o EDM, você pode usar o **ODataModelBuilder** classe para criar o EDM Adicionando propriedades, chaves e propriedades de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="c4687-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="c4687-191">O **EntitySet** método adiciona uma entidade definida como EDM:</span><span class="sxs-lookup"><span data-stu-id="c4687-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="c4687-192">A cadeia de caracteres "Produtos" define o nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="c4687-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="c4687-193">O nome do controlador deve corresponder ao nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="c4687-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="c4687-194">Neste tutorial, o conjunto de entidades é denominado "Produtos", e o controlador `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="c4687-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="c4687-195">Se você nomeou o conjunto de entidades "ProductSet", você poderia nomear o controlador de `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="c4687-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="c4687-196">Observe que um ponto de extremidade pode ter vários conjuntos de entidades.</span><span class="sxs-lookup"><span data-stu-id="c4687-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="c4687-197">Chamar **EntitySet&lt;T&gt;**  para cada entidade definida e, em seguida, defina um controlador correspondente.</span><span class="sxs-lookup"><span data-stu-id="c4687-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="c4687-198">O **MapODataRoute** método adiciona uma rota para o ponto de extremidade OData.</span><span class="sxs-lookup"><span data-stu-id="c4687-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="c4687-199">O primeiro parâmetro é um nome amigável para a rota.</span><span class="sxs-lookup"><span data-stu-id="c4687-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="c4687-200">Os clientes do serviço não vir esse nome.</span><span class="sxs-lookup"><span data-stu-id="c4687-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="c4687-201">O segundo parâmetro é o prefixo do URI do ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="c4687-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="c4687-202">Devido a esse código, o URI para o conjunto de entidades de produtos é http://*hostname*  /odata/produtos.</span><span class="sxs-lookup"><span data-stu-id="c4687-202">Given this code, the URI for the Products entity set is http://*hostname*/odata/Products.</span></span> <span data-ttu-id="c4687-203">O aplicativo pode ter mais de um ponto de extremidade OData.</span><span class="sxs-lookup"><span data-stu-id="c4687-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="c4687-204">Para cada ponto de extremidade, chame **MapODataRoute** e forneça um nome de rota exclusivo e um prefixo URI exclusivo.</span><span class="sxs-lookup"><span data-stu-id="c4687-204">For each endpoint, call **MapODataRoute** and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="c4687-205">Propagação do banco de dados (opcional)</span><span class="sxs-lookup"><span data-stu-id="c4687-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="c4687-206">Nesta etapa, você usará o Entity Framework para propagar o banco de dados com alguns dados de teste.</span><span class="sxs-lookup"><span data-stu-id="c4687-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="c4687-207">Esta etapa é opcional, mas permite que você teste seu ponto de extremidade OData imediatamente.</span><span class="sxs-lookup"><span data-stu-id="c4687-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="c4687-208">Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="c4687-208">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c4687-209">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="c4687-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="c4687-210">Isso adiciona uma pasta chamada migrações e um arquivo de código chamado Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="c4687-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="c4687-211">Abra o arquivo e adicione o seguinte código para o `Configuration.Seed` método.</span><span class="sxs-lookup"><span data-stu-id="c4687-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="c4687-212">Na janela do Console do Gerenciador de pacotes, digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="c4687-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="c4687-213">Esses comandos geram código que cria o banco de dados e, em seguida, execute esse código.</span><span class="sxs-lookup"><span data-stu-id="c4687-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="c4687-214">Explorando o ponto de extremidade OData</span><span class="sxs-lookup"><span data-stu-id="c4687-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="c4687-215">Nesta seção, vamos usar o [Proxy de depuração da Web Fiddler](http://www.fiddler2.com) para enviar solicitações para o ponto de extremidade e examine as mensagens de resposta.</span><span class="sxs-lookup"><span data-stu-id="c4687-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="c4687-216">Isso ajudará você a entender os recursos de um ponto de extremidade OData.</span><span class="sxs-lookup"><span data-stu-id="c4687-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="c4687-217">No Visual Studio, pressione F5 para iniciar a depuração.</span><span class="sxs-lookup"><span data-stu-id="c4687-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="c4687-218">Por padrão, o Visual Studio abre seu navegador para `http://localhost:*port*`, onde *porta* é o número de porta configurado nas configurações do projeto.</span><span class="sxs-lookup"><span data-stu-id="c4687-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="c4687-219">Você pode alterar o número da porta nas configurações do projeto.</span><span class="sxs-lookup"><span data-stu-id="c4687-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="c4687-220">No Gerenciador de soluções, clique com o botão direito e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="c4687-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="c4687-221">Na janela Propriedades, selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="c4687-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="c4687-222">Insira o número da porta em **Url do projeto**.</span><span class="sxs-lookup"><span data-stu-id="c4687-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="c4687-223">Documento de serviço</span><span class="sxs-lookup"><span data-stu-id="c4687-223">Service Document</span></span>

<span data-ttu-id="c4687-224">O *documento de serviço* contém uma lista de conjuntos de entidades para o ponto de extremidade OData.</span><span class="sxs-lookup"><span data-stu-id="c4687-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="c4687-225">Para obter o documento de serviço, envie uma solicitação GET para a raiz do URI do serviço.</span><span class="sxs-lookup"><span data-stu-id="c4687-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="c4687-226">Usando o Fiddler, insira o seguinte URI no **criador** guia: `http://localhost:port/odata/`, onde *porta* é o número da porta.</span><span class="sxs-lookup"><span data-stu-id="c4687-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="c4687-227">Clique o **Execute** botão.</span><span class="sxs-lookup"><span data-stu-id="c4687-227">Click the **Execute** button.</span></span> <span data-ttu-id="c4687-228">Fiddler envia uma solicitação HTTP GET para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c4687-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="c4687-229">Você deve ver a resposta na lista de sessões da Web.</span><span class="sxs-lookup"><span data-stu-id="c4687-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="c4687-230">Se tudo está funcionando, o código de status será 200.</span><span class="sxs-lookup"><span data-stu-id="c4687-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="c4687-231">Clique duas vezes a resposta na lista de sessões da Web para ver os detalhes da mensagem de resposta, na guia inspetores.</span><span class="sxs-lookup"><span data-stu-id="c4687-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="c4687-232">A mensagem de resposta HTTP bruta deve ser semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="c4687-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="c4687-233">Por padrão, a API Web retorna o documento de serviço no formato de AtomPub.</span><span class="sxs-lookup"><span data-stu-id="c4687-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="c4687-234">Para solicitar o JSON, adicione o seguinte cabeçalho à solicitação HTTP:</span><span class="sxs-lookup"><span data-stu-id="c4687-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="c4687-235">Agora, a resposta HTTP contém uma carga JSON:</span><span class="sxs-lookup"><span data-stu-id="c4687-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="c4687-236">Documento de metadados de serviço</span><span class="sxs-lookup"><span data-stu-id="c4687-236">Service Metadata Document</span></span>

<span data-ttu-id="c4687-237">O *documento de metadados de serviço* descreve o modelo de dados do serviço, usando uma linguagem XML chamada linguagem de definição de esquema (CSDL) conceitual.</span><span class="sxs-lookup"><span data-stu-id="c4687-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="c4687-238">O documento de metadados mostra a estrutura dos dados no serviço e pode ser usado para gerar o código do cliente.</span><span class="sxs-lookup"><span data-stu-id="c4687-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="c4687-239">Para obter o documento de metadados, envie uma solicitação GET para `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="c4687-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="c4687-240">Eis os metadados para o ponto de extremidade mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="c4687-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="c4687-241">Conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="c4687-241">Entity Set</span></span>

<span data-ttu-id="c4687-242">Para obter o conjunto de entidades de produtos, envie uma solicitação GET para `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="c4687-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="c4687-243">Entidade</span><span class="sxs-lookup"><span data-stu-id="c4687-243">Entity</span></span>

<span data-ttu-id="c4687-244">Para obter um produto individual, envie uma solicitação GET para `http://localhost:port/odata/Products(1)`, onde "1" é a ID do produto.</span><span class="sxs-lookup"><span data-stu-id="c4687-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="c4687-245">Formatos de serialização do OData</span><span class="sxs-lookup"><span data-stu-id="c4687-245">OData Serialization Formats</span></span>

<span data-ttu-id="c4687-246">OData oferece suporte a vários formatos de serialização:</span><span class="sxs-lookup"><span data-stu-id="c4687-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="c4687-247">Publicação do Atom (XML)</span><span class="sxs-lookup"><span data-stu-id="c4687-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="c4687-248">JSON "claro" (introduzido no OData v3)</span><span class="sxs-lookup"><span data-stu-id="c4687-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="c4687-249">JSON "verbose" (OData v2)</span><span class="sxs-lookup"><span data-stu-id="c4687-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="c4687-250">Por padrão, a API da Web usa formato de "light" AtomPubJSON.</span><span class="sxs-lookup"><span data-stu-id="c4687-250">By default, Web API uses AtomPubJSON "light" format.</span></span> 

<span data-ttu-id="c4687-251">Para obter o formato de AtomPub, defina o cabeçalho Accept "application/atom + xml".</span><span class="sxs-lookup"><span data-stu-id="c4687-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="c4687-252">Aqui está um exemplo de corpo de resposta:</span><span class="sxs-lookup"><span data-stu-id="c4687-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="c4687-253">Você pode ver uma desvantagem óbvia do formato Atom: é muito mais detalhado do que o JSON leve.</span><span class="sxs-lookup"><span data-stu-id="c4687-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="c4687-254">No entanto, se você tiver um cliente que compreende AtomPub, o cliente talvez prefira formato JSON.</span><span class="sxs-lookup"><span data-stu-id="c4687-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="c4687-255">Aqui está a versão leve do JSON da mesma entidade:</span><span class="sxs-lookup"><span data-stu-id="c4687-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="c4687-256">O formato JSON light foi introduzido na versão 3 do protocolo OData.</span><span class="sxs-lookup"><span data-stu-id="c4687-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="c4687-257">Para compatibilidade com versões anteriores, o cliente pode solicitar o antigo formato JSON "detalhado".</span><span class="sxs-lookup"><span data-stu-id="c4687-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="c4687-258">Para solicitar JSON detalhado, defina o cabeçalho Accept `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="c4687-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="c4687-259">Aqui está a versão detalhada:</span><span class="sxs-lookup"><span data-stu-id="c4687-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="c4687-260">Esse formato transmite mais metadados no corpo da resposta, que pode adicionar uma sobrecarga considerável através de uma sessão inteira.</span><span class="sxs-lookup"><span data-stu-id="c4687-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="c4687-261">Além disso, ele adiciona um nível de erro encapsulando o objeto em uma propriedade chamada "d".</span><span class="sxs-lookup"><span data-stu-id="c4687-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4687-262">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="c4687-262">Next Steps</span></span>

- [<span data-ttu-id="c4687-263">Adicionar relações de entidade</span><span class="sxs-lookup"><span data-stu-id="c4687-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="c4687-264">Adicionar ações de OData</span><span class="sxs-lookup"><span data-stu-id="c4687-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="c4687-265">Chamar o serviço OData de um cliente .NET</span><span class="sxs-lookup"><span data-stu-id="c4687-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
