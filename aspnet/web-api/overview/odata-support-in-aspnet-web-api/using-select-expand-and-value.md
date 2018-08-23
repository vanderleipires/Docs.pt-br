---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Usando $select, $expand e $value no ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/11/2013
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: d198ecf40155cba36204bc0810f4735aae6b100b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834504"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="6f05b-102">Usando $select, $expand e $value no ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="6f05b-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="6f05b-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6f05b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6f05b-104">API Web 2 adiciona suporte para o $expand, $select e opções de $value no OData.</span><span class="sxs-lookup"><span data-stu-id="6f05b-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="6f05b-105">Essas opções permitem que um cliente controlar a representação de que ele obtém de volta do servidor.</span><span class="sxs-lookup"><span data-stu-id="6f05b-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="6f05b-106">**$expand** faz com que as entidades relacionadas ao ser incluídas embutidas na resposta.</span><span class="sxs-lookup"><span data-stu-id="6f05b-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="6f05b-107">**$select** seleciona um subconjunto de propriedades para incluir na resposta.</span><span class="sxs-lookup"><span data-stu-id="6f05b-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="6f05b-108">**$value** obtém o valor bruto de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="6f05b-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="6f05b-109">Esquema de exemplo</span><span class="sxs-lookup"><span data-stu-id="6f05b-109">Example Schema</span></span>

<span data-ttu-id="6f05b-110">Neste artigo, vou usar um serviço OData que define três entidades: produto, fornecedor e categoria.</span><span class="sxs-lookup"><span data-stu-id="6f05b-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="6f05b-111">Cada produto tem uma categoria e um fornecedor.</span><span class="sxs-lookup"><span data-stu-id="6f05b-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="6f05b-112">Aqui estão as classes do c# que definem os modelos de entidade:</span><span class="sxs-lookup"><span data-stu-id="6f05b-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="6f05b-113">Observe que o `Product` classe define propriedades de navegação para o `Supplier` e `Category`.</span><span class="sxs-lookup"><span data-stu-id="6f05b-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="6f05b-114">O `Category` classe define uma propriedade de navegação para os produtos em cada categoria.</span><span class="sxs-lookup"><span data-stu-id="6f05b-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="6f05b-115">Para criar um ponto de extremidade OData para este esquema, use o scaffolding do Visual Studio 2013, conforme descrito em [criando um ponto de extremidade OData na API Web ASP.NET](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="6f05b-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="6f05b-116">Adicione controladores separados para fornecedor, categoria e produto.</span><span class="sxs-lookup"><span data-stu-id="6f05b-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="6f05b-117">Habilitando $expandir e $select</span><span class="sxs-lookup"><span data-stu-id="6f05b-117">Enabling $expand and $select</span></span>

<span data-ttu-id="6f05b-118">No Visual Studio 2013, o scaffolding de Web API OData cria um controlador que automaticamente dá suporte a $expand e $select.</span><span class="sxs-lookup"><span data-stu-id="6f05b-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="6f05b-119">Para referência, aqui estão os requisitos para dar suporte a $expandem e $select em um controlador.</span><span class="sxs-lookup"><span data-stu-id="6f05b-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="6f05b-120">Para coleções, o controlador `Get` método deve retornar um **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="6f05b-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="6f05b-121">Para entidades únicas, retornar um **SingleResult&lt;T&gt;**, onde T é um **IQueryable** que contém zero ou uma entidade.</span><span class="sxs-lookup"><span data-stu-id="6f05b-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="6f05b-122">Além disso, decorar seu `Get` métodos com o **[Queryable]** de atributo, conforme mostrado nos trechos de código anterior.</span><span class="sxs-lookup"><span data-stu-id="6f05b-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="6f05b-123">Como alternativa, chame **EnableQuerySupport** sobre o **HttpConfiguration** objeto na inicialização.</span><span class="sxs-lookup"><span data-stu-id="6f05b-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="6f05b-124">(Para obter mais informações, consulte [habilitando opções de consulta OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="6f05b-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="6f05b-125">Usando $expand</span><span class="sxs-lookup"><span data-stu-id="6f05b-125">Using $expand</span></span>

<span data-ttu-id="6f05b-126">Quando você consulta a uma entidade OData ou coleção, a resposta padrão não inclui entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="6f05b-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="6f05b-127">Por exemplo, aqui está a resposta padrão para o conjunto de entidades de categorias:</span><span class="sxs-lookup"><span data-stu-id="6f05b-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="6f05b-128">Como você pode ver, a resposta não inclui quaisquer produtos, mesmo que a entidade da categoria tem um link de navegação de produtos.</span><span class="sxs-lookup"><span data-stu-id="6f05b-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="6f05b-129">No entanto, o cliente pode usar $expandir para obter a lista de produtos para cada categoria.</span><span class="sxs-lookup"><span data-stu-id="6f05b-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="6f05b-130">A opção $expand vai para a cadeia de caracteres de consulta da solicitação:</span><span class="sxs-lookup"><span data-stu-id="6f05b-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="6f05b-131">Agora o servidor incluirá os produtos para cada categoria, embutido com as categorias.</span><span class="sxs-lookup"><span data-stu-id="6f05b-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="6f05b-132">Aqui está a carga de resposta:</span><span class="sxs-lookup"><span data-stu-id="6f05b-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="6f05b-133">Observe que cada entrada na matriz "value" contém uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="6f05b-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="6f05b-134">O $expand opção leva uma separada por vírgulas lista de propriedades de navegação a expandir.</span><span class="sxs-lookup"><span data-stu-id="6f05b-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="6f05b-135">A solicitação a seguir expande a categoria e o fornecedor para um produto.</span><span class="sxs-lookup"><span data-stu-id="6f05b-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="6f05b-136">Aqui está o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="6f05b-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="6f05b-137">Você pode expandir mais de um nível de propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="6f05b-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="6f05b-138">O exemplo a seguir inclui todos os produtos para uma categoria e também o fornecedor para cada produto.</span><span class="sxs-lookup"><span data-stu-id="6f05b-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="6f05b-139">Aqui está o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="6f05b-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="6f05b-140">Por padrão, a API da Web limita a profundidade máxima de expansão para 2.</span><span class="sxs-lookup"><span data-stu-id="6f05b-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="6f05b-141">Que evita que o cliente envie solicitações complexas, como `$expand=Orders/OrderDetails/Product/Supplier/Region`, que pode ser ineficiente para consultar e criar respostas grandes.</span><span class="sxs-lookup"><span data-stu-id="6f05b-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="6f05b-142">Para substituir o padrão, defina as **MaxExpansionDepth** propriedade sobre o **[Queryable]** atributo.</span><span class="sxs-lookup"><span data-stu-id="6f05b-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="6f05b-143">Para obter mais informações sobre a $expand opção, consulte [opção de consulta de sistema expanda ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) na documentação oficial do OData.</span><span class="sxs-lookup"><span data-stu-id="6f05b-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="6f05b-144">Usando $select</span><span class="sxs-lookup"><span data-stu-id="6f05b-144">Using $select</span></span>

<span data-ttu-id="6f05b-145">A opção de $select Especifica um subconjunto de propriedades a serem incluídas no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="6f05b-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="6f05b-146">Por exemplo, para que somente o nome e o preço de cada produto, use a seguinte consulta:</span><span class="sxs-lookup"><span data-stu-id="6f05b-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="6f05b-147">Aqui está o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="6f05b-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="6f05b-148">Você pode combinar $select e $expand na mesma consulta.</span><span class="sxs-lookup"><span data-stu-id="6f05b-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="6f05b-149">Certifique-se de incluir a propriedade expandida na opção $select.</span><span class="sxs-lookup"><span data-stu-id="6f05b-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="6f05b-150">Por exemplo, a solicitação a seguir obtém o nome do produto e o fornecedor.</span><span class="sxs-lookup"><span data-stu-id="6f05b-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="6f05b-151">Aqui está o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="6f05b-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="6f05b-152">Você também pode selecionar as propriedades dentro de uma propriedade expandida.</span><span class="sxs-lookup"><span data-stu-id="6f05b-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="6f05b-153">A solicitação a seguir expande os produtos e seleciona o nome de categoria e produto.</span><span class="sxs-lookup"><span data-stu-id="6f05b-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="6f05b-154">Aqui está o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="6f05b-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="6f05b-155">Para obter mais informações sobre a opção $select, consulte [Selecionar opção de consulta do sistema ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) na documentação oficial do OData.</span><span class="sxs-lookup"><span data-stu-id="6f05b-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="6f05b-156">Obtendo propriedades individuais de uma entidade ($value)</span><span class="sxs-lookup"><span data-stu-id="6f05b-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="6f05b-157">Há duas maneiras para um cliente OData obter uma propriedade individual de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="6f05b-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="6f05b-158">O cliente pode obter o valor no formato OData ou obter o valor bruto da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6f05b-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="6f05b-159">A solicitação a seguir obtém uma propriedade no formato OData.</span><span class="sxs-lookup"><span data-stu-id="6f05b-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="6f05b-160">Aqui está um exemplo de resposta no formato JSON:</span><span class="sxs-lookup"><span data-stu-id="6f05b-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="6f05b-161">Para obter o valor bruto da propriedade, acrescente $value para o URI:</span><span class="sxs-lookup"><span data-stu-id="6f05b-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="6f05b-162">Aqui está a resposta.</span><span class="sxs-lookup"><span data-stu-id="6f05b-162">Here is the response.</span></span> <span data-ttu-id="6f05b-163">Observe que o tipo de conteúdo "text/plain", não é JSON.</span><span class="sxs-lookup"><span data-stu-id="6f05b-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="6f05b-164">Para dar suporte a essas consultas no seu controlador de OData, adicione um método chamado `GetProperty`, onde `Property` é o nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="6f05b-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="6f05b-165">Por exemplo, o método para obter a propriedade de nome seria nomeado `GetName`.</span><span class="sxs-lookup"><span data-stu-id="6f05b-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="6f05b-166">O método deve retornar o valor dessa propriedade:</span><span class="sxs-lookup"><span data-stu-id="6f05b-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
