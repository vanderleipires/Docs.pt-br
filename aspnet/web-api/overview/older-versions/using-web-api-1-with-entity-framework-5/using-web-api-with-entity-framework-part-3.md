---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: Criar um controlador de Admin | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="96eeb-102">Parte 3: Criar um controlador de Admin</span><span class="sxs-lookup"><span data-stu-id="96eeb-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="96eeb-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="96eeb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="96eeb-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="96eeb-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="96eeb-105">Adicionar um controlador de Admin</span><span class="sxs-lookup"><span data-stu-id="96eeb-105">Add an Admin Controller</span></span>

<span data-ttu-id="96eeb-106">Nesta seção, vamos adicionar um controlador de API da Web que oferece suporte a CRUD (criar, ler, atualizar e excluir) operações em produtos.</span><span class="sxs-lookup"><span data-stu-id="96eeb-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="96eeb-107">O controlador usará o Entity Framework para se comunicar com a camada de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="96eeb-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="96eeb-108">Somente os administradores poderão usar esse controlador.</span><span class="sxs-lookup"><span data-stu-id="96eeb-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="96eeb-109">Os clientes acessarão os produtos por meio de outro controlador.</span><span class="sxs-lookup"><span data-stu-id="96eeb-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="96eeb-110">No Gerenciador de soluções, clique na pasta de controladores.</span><span class="sxs-lookup"><span data-stu-id="96eeb-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="96eeb-111">Selecione **adicionar** e **controlador**.</span><span class="sxs-lookup"><span data-stu-id="96eeb-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="96eeb-112">No **Adicionar controlador** caixa de diálogo, o nome do controlador `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="96eeb-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="96eeb-113">Em **modelo**, selecione &quot;controlador API com ações de leitura/gravação, usando o Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="96eeb-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="96eeb-114">Em **classe modelo**, selecione "Product (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="96eeb-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="96eeb-115">Em **contexto de dados**, selecione "&lt;novo contexto de dados&gt;".</span><span class="sxs-lookup"><span data-stu-id="96eeb-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="96eeb-116">Se o **classe modelo** suspensa não mostra as classes de modelo, verifique se você compilou o projeto.</span><span class="sxs-lookup"><span data-stu-id="96eeb-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="96eeb-117">Entity Framework usa reflexão, então ele precisa do assembly compilado.</span><span class="sxs-lookup"><span data-stu-id="96eeb-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="96eeb-118">Selecionando "&lt;novo contexto de dados&gt;" abrirá o **novo contexto de dados** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="96eeb-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="96eeb-119">Nomeie o contexto de dados `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="96eeb-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="96eeb-120">Clique em **Okey** para ignorar o **novo contexto de dados** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="96eeb-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="96eeb-121">No **Adicionar controlador** caixa de diálogo, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="96eeb-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="96eeb-122">Aqui está o que foi adicionado ao projeto:</span><span class="sxs-lookup"><span data-stu-id="96eeb-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="96eeb-123">Uma classe denominada `OrdersContext` que deriva de **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="96eeb-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="96eeb-124">Essa classe fornece a cola entre os modelos POCO e o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="96eeb-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="96eeb-125">Um controlador de API da Web chamado `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="96eeb-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="96eeb-126">Esse controlador suporta operações CRUD em `Product` instâncias.</span><span class="sxs-lookup"><span data-stu-id="96eeb-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="96eeb-127">Ele usa o `OrdersContext` classe para se comunicar com o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="96eeb-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="96eeb-128">Uma nova cadeia de conexão de banco de dados no arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="96eeb-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="96eeb-129">Abra o arquivo OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="96eeb-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="96eeb-130">Observe que o construtor Especifica o nome da cadeia de caracteres de conexão de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="96eeb-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="96eeb-131">Esse nome refere-se à cadeia de conexão que foi adicionada ao Web. config.</span><span class="sxs-lookup"><span data-stu-id="96eeb-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="96eeb-132">Adicione as seguintes propriedades à classe `OrdersContext`:</span><span class="sxs-lookup"><span data-stu-id="96eeb-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="96eeb-133">Um **DbSet** representa um conjunto de entidades que podem ser consultados.</span><span class="sxs-lookup"><span data-stu-id="96eeb-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="96eeb-134">Aqui está a lista completa de `OrdersContext` classe:</span><span class="sxs-lookup"><span data-stu-id="96eeb-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="96eeb-135">O `AdminController` classe define cinco métodos que implementam a funcionalidade básica de CRUD.</span><span class="sxs-lookup"><span data-stu-id="96eeb-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="96eeb-136">Cada método corresponde a um URI que o cliente pode invocar:</span><span class="sxs-lookup"><span data-stu-id="96eeb-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="96eeb-137">Método do controlador</span><span class="sxs-lookup"><span data-stu-id="96eeb-137">Controller Method</span></span> | <span data-ttu-id="96eeb-138">Descrição</span><span class="sxs-lookup"><span data-stu-id="96eeb-138">Description</span></span> | <span data-ttu-id="96eeb-139">URI</span><span class="sxs-lookup"><span data-stu-id="96eeb-139">URI</span></span> | <span data-ttu-id="96eeb-140">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="96eeb-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="96eeb-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="96eeb-141">GetProducts</span></span> | <span data-ttu-id="96eeb-142">Obtém todos os produtos.</span><span class="sxs-lookup"><span data-stu-id="96eeb-142">Gets all products.</span></span> | <span data-ttu-id="96eeb-143">API e produtos</span><span class="sxs-lookup"><span data-stu-id="96eeb-143">api/products</span></span> | <span data-ttu-id="96eeb-144">OBTER</span><span class="sxs-lookup"><span data-stu-id="96eeb-144">GET</span></span> |
| <span data-ttu-id="96eeb-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="96eeb-145">GetProduct</span></span> | <span data-ttu-id="96eeb-146">Localiza um produto por ID.</span><span class="sxs-lookup"><span data-stu-id="96eeb-146">Finds a product by ID.</span></span> | <span data-ttu-id="96eeb-147">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="96eeb-147">api/products/*id*</span></span> | <span data-ttu-id="96eeb-148">OBTER</span><span class="sxs-lookup"><span data-stu-id="96eeb-148">GET</span></span> |
| <span data-ttu-id="96eeb-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="96eeb-149">PutProduct</span></span> | <span data-ttu-id="96eeb-150">Atualiza um produto.</span><span class="sxs-lookup"><span data-stu-id="96eeb-150">Updates a product.</span></span> | <span data-ttu-id="96eeb-151">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="96eeb-151">api/products/*id*</span></span> | <span data-ttu-id="96eeb-152">PUT</span><span class="sxs-lookup"><span data-stu-id="96eeb-152">PUT</span></span> |
| <span data-ttu-id="96eeb-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="96eeb-153">PostProduct</span></span> | <span data-ttu-id="96eeb-154">Cria um novo produto.</span><span class="sxs-lookup"><span data-stu-id="96eeb-154">Creates a new product.</span></span> | <span data-ttu-id="96eeb-155">API e produtos</span><span class="sxs-lookup"><span data-stu-id="96eeb-155">api/products</span></span> | <span data-ttu-id="96eeb-156">POSTAR</span><span class="sxs-lookup"><span data-stu-id="96eeb-156">POST</span></span> |
| <span data-ttu-id="96eeb-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="96eeb-157">DeleteProduct</span></span> | <span data-ttu-id="96eeb-158">Exclui um produto.</span><span class="sxs-lookup"><span data-stu-id="96eeb-158">Deletes a product.</span></span> | <span data-ttu-id="96eeb-159">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="96eeb-159">api/products/*id*</span></span> | <span data-ttu-id="96eeb-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="96eeb-160">DELETE</span></span> |

<span data-ttu-id="96eeb-161">Cada método chama `OrdersContext` para consultar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="96eeb-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="96eeb-162">Chamam os métodos que modificam a coleção (PUT, POST e DELETE) `db.SaveChanges` para manter as alterações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="96eeb-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="96eeb-163">Os controladores são criados por solicitação HTTP e, em seguida, descartados, portanto, é necessário manter as alterações antes de retorna de um método.</span><span class="sxs-lookup"><span data-stu-id="96eeb-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="96eeb-164">Adicionar um inicializador de banco de dados</span><span class="sxs-lookup"><span data-stu-id="96eeb-164">Add a Database Initializer</span></span>

<span data-ttu-id="96eeb-165">Entity Framework tem um recurso interessante que permite que você preencher o banco de dados na inicialização e recriar automaticamente o banco de dados sempre que alterar os modelos.</span><span class="sxs-lookup"><span data-stu-id="96eeb-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="96eeb-166">Esse recurso é útil durante o desenvolvimento, porque você sempre ter alguns dados de teste, mesmo se você alterar os modelos.</span><span class="sxs-lookup"><span data-stu-id="96eeb-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="96eeb-167">No Gerenciador de soluções, clique com botão direito na pasta modelos e criar uma nova classe chamada `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="96eeb-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="96eeb-168">Cole a seguinte implementação:</span><span class="sxs-lookup"><span data-stu-id="96eeb-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="96eeb-169">Herdando do **DropCreateDatabaseIfModelChanges** classe, dizemos Entity Framework para descartar o banco de dados sempre que é modificar as classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="96eeb-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="96eeb-170">Quando o Entity Framework cria (ou recria) o banco de dados, ele chama o **semente** para popular as tabelas.</span><span class="sxs-lookup"><span data-stu-id="96eeb-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="96eeb-171">Usamos o **semente** método para adicionar alguns produtos de exemplo e uma ordem de exemplo.</span><span class="sxs-lookup"><span data-stu-id="96eeb-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="96eeb-172">Esse recurso é ótimo para teste, mas não use o **DropCreateDatabaseIfModelChanges** classe em produção, pois você pode perder os dados se alguém altera uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="96eeb-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="96eeb-173">Em seguida, abra global. asax e adicione o seguinte código para o **aplicativo\_iniciar** método:</span><span class="sxs-lookup"><span data-stu-id="96eeb-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="96eeb-174">Enviar uma solicitação para o controlador</span><span class="sxs-lookup"><span data-stu-id="96eeb-174">Send a Request to the Controller</span></span>

<span data-ttu-id="96eeb-175">Neste momento, não tenhamos escrito qualquer código de cliente, mas você pode chamar web API usando um navegador da web ou uma depuração de HTTP, como ferramenta [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="96eeb-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="96eeb-176">No Visual Studio, pressione F5 para iniciar a depuração.</span><span class="sxs-lookup"><span data-stu-id="96eeb-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="96eeb-177">Seu navegador da web será aberto para `http://localhost:*portnum*/`, onde *portnum* é um número de porta.</span><span class="sxs-lookup"><span data-stu-id="96eeb-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="96eeb-178">Enviar uma solicitação HTTP para "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="96eeb-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="96eeb-179">A primeira solicitação pode ser lenta para ser concluída, porque a estrutura Entify precisa para criar e propagar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="96eeb-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="96eeb-180">A resposta deve algo semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="96eeb-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="96eeb-181">[Anterior](using-web-api-with-entity-framework-part-2.md)
> [Próximo](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="96eeb-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
