---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: Criando o produto e controladores de ordem | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6bd485d29821af12b9ebe31b2d04a2d9ab826731
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869381"
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="49520-102">Parte 6: Criação de produto e os controladores de ordem</span><span class="sxs-lookup"><span data-stu-id="49520-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="49520-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="49520-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="49520-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="49520-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="49520-105">Adicionar um controlador de produtos</span><span class="sxs-lookup"><span data-stu-id="49520-105">Add a Products Controller</span></span>

<span data-ttu-id="49520-106">O controlador de administrador é para usuários que têm privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="49520-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="49520-107">Os clientes, por outro lado, podem exibir produtos, mas não é possível criar, atualizar ou excluí-los.</span><span class="sxs-lookup"><span data-stu-id="49520-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="49520-108">Podemos facilmente pode restringir o acesso aos métodos Post, Put e Delete, deixando os métodos Get aberto.</span><span class="sxs-lookup"><span data-stu-id="49520-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="49520-109">Mas, veja os dados que são retornados de um produto:</span><span class="sxs-lookup"><span data-stu-id="49520-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="49520-110">O `ActualCost` propriedade não deve estar visível para os clientes!</span><span class="sxs-lookup"><span data-stu-id="49520-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="49520-111">A solução é definir um *o objeto de transferência de dados* (DTO) que inclui um subconjunto das propriedades que devem ser visíveis aos clientes.</span><span class="sxs-lookup"><span data-stu-id="49520-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="49520-112">Usaremos o LINQ para projeto `Product` instâncias `ProductDTO` instâncias.</span><span class="sxs-lookup"><span data-stu-id="49520-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="49520-113">Adicione uma classe denominada `ProductDTO` para a pasta de modelos.</span><span class="sxs-lookup"><span data-stu-id="49520-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="49520-114">Agora adicione o controlador.</span><span class="sxs-lookup"><span data-stu-id="49520-114">Now add the controller.</span></span> <span data-ttu-id="49520-115">No Gerenciador de soluções, clique na pasta de controladores.</span><span class="sxs-lookup"><span data-stu-id="49520-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="49520-116">Selecione **adicionar**, em seguida, selecione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="49520-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="49520-117">Na caixa de diálogo **Adicionar controlador**, nomeie o controlador como &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="49520-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="49520-118">Em **modelo**, selecione **controlador API vazio**.</span><span class="sxs-lookup"><span data-stu-id="49520-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="49520-119">Substitua tudo no arquivo de origem com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="49520-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="49520-120">O controlador ainda usa o `OrdersContext` para consultar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="49520-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="49520-121">Mas em vez de retornar `Product` instâncias diretamente, podemos chamar `MapProducts` ao projeto-os no `ProductDTO` instâncias:</span><span class="sxs-lookup"><span data-stu-id="49520-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="49520-122">O `MapProducts` método retorna um **IQueryable**, portanto, é possível compor o resultado com outros parâmetros de consulta.</span><span class="sxs-lookup"><span data-stu-id="49520-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="49520-123">Você pode ver isso no `GetProduct` método, que adiciona um **onde** cláusula para a consulta:</span><span class="sxs-lookup"><span data-stu-id="49520-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="49520-124">Adicionar um controlador de pedidos</span><span class="sxs-lookup"><span data-stu-id="49520-124">Add an Orders Controller</span></span>

<span data-ttu-id="49520-125">Em seguida, adicione um controlador que permite aos usuários criar e exibir ordens.</span><span class="sxs-lookup"><span data-stu-id="49520-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="49520-126">Vamos começar com outro DTO.</span><span class="sxs-lookup"><span data-stu-id="49520-126">We'll start with another DTO.</span></span> <span data-ttu-id="49520-127">No Gerenciador de soluções, clique com botão direito na pasta modelos e adicione uma classe denominada `OrderDTO` usar a implementação a seguir:</span><span class="sxs-lookup"><span data-stu-id="49520-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="49520-128">Agora adicione o controlador.</span><span class="sxs-lookup"><span data-stu-id="49520-128">Now add the controller.</span></span> <span data-ttu-id="49520-129">No Gerenciador de soluções, clique na pasta de controladores.</span><span class="sxs-lookup"><span data-stu-id="49520-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="49520-130">Selecione **adicionar**, em seguida, selecione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="49520-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="49520-131">No **Adicionar controlador** caixa de diálogo, defina as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="49520-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="49520-132">Em **nome do controlador**, digite "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="49520-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="49520-133">Em **modelo**, selecione "Controlador API com ações de leitura/gravação, usando o Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="49520-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="49520-134">Em **classe modelo**, selecione &quot;ordem (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="49520-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="49520-135">Em **classe de contexto de dados**, selecione &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="49520-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="49520-136">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="49520-136">Click **Add**.</span></span> <span data-ttu-id="49520-137">Isso adiciona um arquivo chamado OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="49520-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="49520-138">Em seguida, é preciso modificar a implementação padrão do controlador.</span><span class="sxs-lookup"><span data-stu-id="49520-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="49520-139">Primeiro, exclua o `PutOrder` e `DeleteOrder` métodos.</span><span class="sxs-lookup"><span data-stu-id="49520-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="49520-140">Para este exemplo, os clientes não podem modificar ou excluir ordens existentes.</span><span class="sxs-lookup"><span data-stu-id="49520-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="49520-141">Em um aplicativo real, você precisaria muita lógica de back-end para lidar com esses casos.</span><span class="sxs-lookup"><span data-stu-id="49520-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="49520-142">(Por exemplo, a ordem já vinha?)</span><span class="sxs-lookup"><span data-stu-id="49520-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="49520-143">Alterar o `GetOrders` método para retornar apenas os pedidos que pertencem ao usuário:</span><span class="sxs-lookup"><span data-stu-id="49520-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="49520-144">Alterar o `GetOrder` método da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="49520-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="49520-145">Aqui estão as alterações que fizemos para o método:</span><span class="sxs-lookup"><span data-stu-id="49520-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="49520-146">O valor de retorno é um `OrderDTO` instância, em vez de um `Order`.</span><span class="sxs-lookup"><span data-stu-id="49520-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="49520-147">Quando é consultar o banco de dados para a ordem, usamos o [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) método para buscar relacionado `OrderDetail` e `Product` entidades.</span><span class="sxs-lookup"><span data-stu-id="49520-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="49520-148">Podemos mesclar o resultados usando uma projeção.</span><span class="sxs-lookup"><span data-stu-id="49520-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="49520-149">A resposta HTTP conterá uma matriz de produtos com quantidades:</span><span class="sxs-lookup"><span data-stu-id="49520-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="49520-150">Esse formato é mais fácil para os clientes usarem que o gráfico de objeto original, que contém entidades aninhadas (ordem, os detalhes e produtos).</span><span class="sxs-lookup"><span data-stu-id="49520-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="49520-151">O último método considerá-la `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="49520-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="49520-152">No momento, esse método usa um `Order` instância.</span><span class="sxs-lookup"><span data-stu-id="49520-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="49520-153">Porém, considere o que acontece se um cliente envia um corpo de solicitação como este:</span><span class="sxs-lookup"><span data-stu-id="49520-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="49520-154">Esta é uma ordem bem estruturada e Felizmente irá inserir do Entity Framework para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="49520-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="49520-155">Mas ele contém uma entidade de produto que não existia anteriormente.</span><span class="sxs-lookup"><span data-stu-id="49520-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="49520-156">O cliente que acabou de criar um novo produto no nosso banco de dados!</span><span class="sxs-lookup"><span data-stu-id="49520-156">The client just created a new product in our database!</span></span> <span data-ttu-id="49520-157">Isso será uma surpresa para o departamento de preenchimento do pedido, quando eles veem uma ordem para koala ursos.</span><span class="sxs-lookup"><span data-stu-id="49520-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="49520-158">É a moral, tenha muito cuidado sobre os dados que você aceitar uma solicitação POST ou PUT.</span><span class="sxs-lookup"><span data-stu-id="49520-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="49520-159">Para evitar esse problema, altere o `PostOrder` método tenham uma `OrderDTO` instância.</span><span class="sxs-lookup"><span data-stu-id="49520-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="49520-160">Use o `OrderDTO` para criar o `Order`.</span><span class="sxs-lookup"><span data-stu-id="49520-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="49520-161">Observe que usamos o `ProductID` e `Quantity` propriedades e vamos ignorar quaisquer valores que o cliente enviou para o nome do produto ou no preço.</span><span class="sxs-lookup"><span data-stu-id="49520-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="49520-162">Se a ID do produto não é válida, ele violarão a restrição de chave estrangeira no banco de dados e a inserção falhará, como deveria.</span><span class="sxs-lookup"><span data-stu-id="49520-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="49520-163">Aqui está o completo `PostOrder` método:</span><span class="sxs-lookup"><span data-stu-id="49520-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="49520-164">Finalmente, adicione o **autorizar** de atributo para o controlador:</span><span class="sxs-lookup"><span data-stu-id="49520-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="49520-165">Agora, somente usuários registrados podem criar ou exibir ordens.</span><span class="sxs-lookup"><span data-stu-id="49520-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="49520-166">[Anterior](using-web-api-with-entity-framework-part-5.md)
> [Próximo](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="49520-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
