---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Usando $select, $expand e $value no ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508055"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="8b93c-102">Usando $select, $expand e $value no ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="8b93c-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="8b93c-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8b93c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8b93c-104">Web API 2 adiciona suporte para o $expand, $select e $value opções no OData.</span><span class="sxs-lookup"><span data-stu-id="8b93c-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="8b93c-105">Essas opções permitem que um cliente controlar a representação que ele obtém de volta do servidor.</span><span class="sxs-lookup"><span data-stu-id="8b93c-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="8b93c-106">**$expand** faz com que as entidades relacionadas ao ser embutido incluído na resposta.</span><span class="sxs-lookup"><span data-stu-id="8b93c-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="8b93c-107">**$select** seleciona um subconjunto de propriedades a serem incluídas na resposta.</span><span class="sxs-lookup"><span data-stu-id="8b93c-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="8b93c-108">**$value** obtém o valor bruto de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="8b93c-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="8b93c-109">Esquema de exemplo</span><span class="sxs-lookup"><span data-stu-id="8b93c-109">Example Schema</span></span>

<span data-ttu-id="8b93c-110">Neste artigo, vou usar um serviço OData que define três entidades: produto, fornecedor e categoria.</span><span class="sxs-lookup"><span data-stu-id="8b93c-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="8b93c-111">Cada produto tem uma categoria e um fornecedor.</span><span class="sxs-lookup"><span data-stu-id="8b93c-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="8b93c-112">Aqui estão as classes do c# que definem os modelos de entidade:</span><span class="sxs-lookup"><span data-stu-id="8b93c-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="8b93c-113">Observe que o `Product` classe define propriedades de navegação para o `Supplier` e `Category`.</span><span class="sxs-lookup"><span data-stu-id="8b93c-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="8b93c-114">O `Category` classe define uma propriedade de navegação para os produtos em cada categoria.</span><span class="sxs-lookup"><span data-stu-id="8b93c-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="8b93c-115">Para criar um ponto de extremidade OData para este esquema, use a estrutura do Visual Studio 2013, conforme descrito em [criar um ponto de extremidade OData na API da Web ASP.NET](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="8b93c-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="8b93c-116">Adicione controladores separados para fornecedor, categoria e produto.</span><span class="sxs-lookup"><span data-stu-id="8b93c-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="8b93c-117">Habilitando $expandir e $select</span><span class="sxs-lookup"><span data-stu-id="8b93c-117">Enabling $expand and $select</span></span>

<span data-ttu-id="8b93c-118">No Visual Studio 2013, o scaffolding OData da API Web cria um controlador que automaticamente dá suporte a $expand e $select.</span><span class="sxs-lookup"><span data-stu-id="8b93c-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="8b93c-119">Para referência, aqui estão os requisitos para dar suporte a $expandem e $select em um controlador.</span><span class="sxs-lookup"><span data-stu-id="8b93c-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="8b93c-120">Para coleções, o controlador `Get` método deve retornar um **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="8b93c-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="8b93c-121">Para entidades únicas, retornar um **SingleResult&lt;T&gt;**, onde T é um **IQueryable** que contém zero ou uma entidade.</span><span class="sxs-lookup"><span data-stu-id="8b93c-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="8b93c-122">Além disso, decorar seu `Get` métodos com o **[Queryable]** atributo, conforme os trechos de código anterior.</span><span class="sxs-lookup"><span data-stu-id="8b93c-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="8b93c-123">Como alternativa, chame **EnableQuerySupport** no **HttpConfiguration** objeto durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="8b93c-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="8b93c-124">(Para obter mais informações, consulte [habilitando opções de consulta OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="8b93c-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="8b93c-125">Usando $expand</span><span class="sxs-lookup"><span data-stu-id="8b93c-125">Using $expand</span></span>

<span data-ttu-id="8b93c-126">Quando você consulta uma entidade OData ou coleção, a resposta padrão não inclui entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="8b93c-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="8b93c-127">Por exemplo, aqui está a resposta padrão para o conjunto de entidades de categorias:</span><span class="sxs-lookup"><span data-stu-id="8b93c-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="8b93c-128">Como você pode ver, a resposta não inclui todos os produtos, mesmo que a entidade da categoria tem um link de navegação de produtos.</span><span class="sxs-lookup"><span data-stu-id="8b93c-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="8b93c-129">No entanto, o cliente pode usar $expandir para obter a lista de produtos para cada categoria.</span><span class="sxs-lookup"><span data-stu-id="8b93c-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="8b93c-130">A opção $expand vai para a cadeia de caracteres de consulta da solicitação:</span><span class="sxs-lookup"><span data-stu-id="8b93c-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="8b93c-131">Agora o servidor incluirá os produtos para cada categoria, embutido com as categorias.</span><span class="sxs-lookup"><span data-stu-id="8b93c-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="8b93c-132">Aqui está a carga de resposta:</span><span class="sxs-lookup"><span data-stu-id="8b93c-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="8b93c-133">Observe que cada entrada na matriz de "valor" contém uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="8b93c-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="8b93c-134">O $expand opção leva um separados por vírgula lista de propriedades de navegação a expandir.</span><span class="sxs-lookup"><span data-stu-id="8b93c-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="8b93c-135">A solicitação a seguir expande a categoria e o fornecedor para um produto.</span><span class="sxs-lookup"><span data-stu-id="8b93c-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="8b93c-136">Aqui está o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="8b93c-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="8b93c-137">Você pode expandir um nível mais de uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="8b93c-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="8b93c-138">O exemplo a seguir inclui todos os produtos para uma categoria e também o fornecedor para cada produto.</span><span class="sxs-lookup"><span data-stu-id="8b93c-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="8b93c-139">Aqui está o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="8b93c-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="8b93c-140">Por padrão, a API da Web limita a profundidade máxima de expansão para 2.</span><span class="sxs-lookup"><span data-stu-id="8b93c-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="8b93c-141">Que evita que o cliente envie solicitações complexas, como `$expand=Orders/OrderDetails/Product/Supplier/Region`, que pode ser ineficiente para consultar e criar respostas grandes.</span><span class="sxs-lookup"><span data-stu-id="8b93c-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="8b93c-142">Para substituir o padrão, defina o **MaxExpansionDepth** propriedade o **[Queryable]** atributo.</span><span class="sxs-lookup"><span data-stu-id="8b93c-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="8b93c-143">Para obter mais informações sobre a $expand opção, consulte [opção de consulta do sistema expandir ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) na documentação oficial do OData.</span><span class="sxs-lookup"><span data-stu-id="8b93c-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="8b93c-144">Usando $select</span><span class="sxs-lookup"><span data-stu-id="8b93c-144">Using $select</span></span>

<span data-ttu-id="8b93c-145">A opção $select Especifica um subconjunto de propriedades a serem incluídas no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="8b93c-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="8b93c-146">Por exemplo, para obter apenas o nome e o preço de cada produto, use a seguinte consulta:</span><span class="sxs-lookup"><span data-stu-id="8b93c-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="8b93c-147">Aqui está o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="8b93c-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="8b93c-148">Você pode combinar $select e $expand na mesma consulta.</span><span class="sxs-lookup"><span data-stu-id="8b93c-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="8b93c-149">Certifique-se de incluir a propriedade expandida na opção $select.</span><span class="sxs-lookup"><span data-stu-id="8b93c-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="8b93c-150">Por exemplo, a solicitação a seguir obtém o nome do produto e fornecedor.</span><span class="sxs-lookup"><span data-stu-id="8b93c-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="8b93c-151">Aqui está o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="8b93c-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="8b93c-152">Você também pode selecionar as propriedades dentro de uma propriedade expandida.</span><span class="sxs-lookup"><span data-stu-id="8b93c-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="8b93c-153">A solicitação a seguir expande produtos e seleciona o nome de categoria e produto.</span><span class="sxs-lookup"><span data-stu-id="8b93c-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="8b93c-154">Aqui está o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="8b93c-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="8b93c-155">Para obter mais informações sobre a opção $select, consulte [Selecionar opção de consulta do sistema ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) na documentação oficial do OData.</span><span class="sxs-lookup"><span data-stu-id="8b93c-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="8b93c-156">Obtendo as propriedades individuais de uma entidade ($value)</span><span class="sxs-lookup"><span data-stu-id="8b93c-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="8b93c-157">Há duas maneiras de um cliente OData obter uma propriedade individual de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="8b93c-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="8b93c-158">O cliente pode obter o valor no formato OData ou obter o valor bruto da propriedade.</span><span class="sxs-lookup"><span data-stu-id="8b93c-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="8b93c-159">A solicitação a seguir obtém uma propriedade no formato OData.</span><span class="sxs-lookup"><span data-stu-id="8b93c-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="8b93c-160">Aqui está um exemplo de resposta no formato JSON:</span><span class="sxs-lookup"><span data-stu-id="8b93c-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="8b93c-161">Para obter o valor bruto da propriedade, acrescente $value para o URI:</span><span class="sxs-lookup"><span data-stu-id="8b93c-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="8b93c-162">Aqui está a resposta.</span><span class="sxs-lookup"><span data-stu-id="8b93c-162">Here is the response.</span></span> <span data-ttu-id="8b93c-163">Observe que o tipo de conteúdo "texto/simples", não é JSON.</span><span class="sxs-lookup"><span data-stu-id="8b93c-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="8b93c-164">Para oferecer suporte a essas consultas em seu controlador de OData, adicione um método chamado `GetProperty`, onde `Property` é o nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="8b93c-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="8b93c-165">Por exemplo, o método para obter a propriedade de nome será nomeado `GetName`.</span><span class="sxs-lookup"><span data-stu-id="8b93c-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="8b93c-166">O método deve retornar o valor da propriedade:</span><span class="sxs-lookup"><span data-stu-id="8b93c-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
