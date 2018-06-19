---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relações de entidade no OData v4 usando ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: 'A maioria dos conjuntos de dados definem relações entre entidades: os clientes tiverem pedidos; os livros têm autores; os produtos têm fornecedores. Usando o OData, os clientes podem navegar por...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508095"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="6b625-104">Relações de entidade no OData v4 usando ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="6b625-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="6b625-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6b625-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="6b625-106">A maioria dos conjuntos de dados definem relações entre entidades: os clientes tiverem pedidos; os livros têm autores; os produtos têm fornecedores.</span><span class="sxs-lookup"><span data-stu-id="6b625-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="6b625-107">Usando o OData, os clientes podem navegar em relações de entidade.</span><span class="sxs-lookup"><span data-stu-id="6b625-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="6b625-108">Um produto, você pode encontrar o fornecedor.</span><span class="sxs-lookup"><span data-stu-id="6b625-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="6b625-109">Você também pode criar ou remover relações.</span><span class="sxs-lookup"><span data-stu-id="6b625-109">You can also create or remove relationships.</span></span> <span data-ttu-id="6b625-110">Por exemplo, você pode definir o fornecedor para um produto.</span><span class="sxs-lookup"><span data-stu-id="6b625-110">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="6b625-111">Este tutorial mostra como dar suporte a essas operações em OData v4 usando a API da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6b625-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="6b625-112">O tutorial se baseia o tutorial [criar uma instalação do OData v4 ponto de extremidade usando ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="6b625-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6b625-113">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="6b625-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6b625-114">2.1 da API da Web</span><span class="sxs-lookup"><span data-stu-id="6b625-114">Web API 2.1</span></span>
> - <span data-ttu-id="6b625-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="6b625-115">OData v4</span></span>
> - [<span data-ttu-id="6b625-116">Visual Studio 2013 Atualização 2</span><span class="sxs-lookup"><span data-stu-id="6b625-116">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="6b625-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6b625-117">Entity Framework 6</span></span>
> - <span data-ttu-id="6b625-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6b625-118">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="6b625-119">Versões do tutoriais</span><span class="sxs-lookup"><span data-stu-id="6b625-119">Tutorial versions</span></span>
> 
> <span data-ttu-id="6b625-120">Para o OData versão 3, consulte [dando suporte a relações de entidade no OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="6b625-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="6b625-121">Adicionar uma entidade do fornecedor</span><span class="sxs-lookup"><span data-stu-id="6b625-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="6b625-122">O tutorial se baseia o tutorial [criar uma instalação do OData v4 ponto de extremidade usando ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="6b625-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>


<span data-ttu-id="6b625-123">Primeiro, precisamos de uma entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="6b625-123">First, we need a related entity.</span></span> <span data-ttu-id="6b625-124">Adicione uma classe denominada `Supplier` na pasta de modelos.</span><span class="sxs-lookup"><span data-stu-id="6b625-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="6b625-125">Adicionar uma propriedade de navegação para o `Product` classe:</span><span class="sxs-lookup"><span data-stu-id="6b625-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="6b625-126">Adicionar um novo **DbSet** para o `ProductsContext` de classe, para que o Entity Framework incluirá a tabela Fornecedores no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6b625-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="6b625-127">Em WebApiConfig.cs, adicione um &quot;fornecedores&quot; do conjunto de entidades para o modelo de dados de entidade:</span><span class="sxs-lookup"><span data-stu-id="6b625-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="6b625-128">Adicionar um controlador de fornecedores</span><span class="sxs-lookup"><span data-stu-id="6b625-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="6b625-129">Adicionar um `SuppliersController` classe para a pasta de controladores.</span><span class="sxs-lookup"><span data-stu-id="6b625-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="6b625-130">Não mostrar como adicionar operações CRUD para esse controlador.</span><span class="sxs-lookup"><span data-stu-id="6b625-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="6b625-131">As etapas são as mesmas para o controlador de produtos (consulte [criar um ponto de extremidade OData v4](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="6b625-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="6b625-132">Obter entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="6b625-132">Getting Related Entities</span></span>

<span data-ttu-id="6b625-133">Para obter o fornecedor para um produto, o cliente envia uma solicitação GET:</span><span class="sxs-lookup"><span data-stu-id="6b625-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="6b625-134">Para dar suporte a essa solicitação, adicione o seguinte método para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="6b625-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="6b625-135">Esse método usa uma convenção de nomenclatura padrão</span><span class="sxs-lookup"><span data-stu-id="6b625-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="6b625-136">Nome do método: GetX, onde X é a propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="6b625-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="6b625-137">Nome do parâmetro: *chave*</span><span class="sxs-lookup"><span data-stu-id="6b625-137">Parameter name: *key*</span></span>

<span data-ttu-id="6b625-138">Se você seguir a convenção de nomenclatura, API da Web mapeia automaticamente a solicitação HTTP para o método do controlador.</span><span class="sxs-lookup"><span data-stu-id="6b625-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="6b625-139">Solicitação de exemplo, HTTP:</span><span class="sxs-lookup"><span data-stu-id="6b625-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="6b625-140">Resposta de exemplo, HTTP:</span><span class="sxs-lookup"><span data-stu-id="6b625-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="6b625-141">Obter uma coleção relacionada</span><span class="sxs-lookup"><span data-stu-id="6b625-141">Getting a related collection</span></span>

<span data-ttu-id="6b625-142">No exemplo anterior, um produto tem um fornecedor.</span><span class="sxs-lookup"><span data-stu-id="6b625-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="6b625-143">Uma propriedade de navegação também pode retornar uma coleção.</span><span class="sxs-lookup"><span data-stu-id="6b625-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="6b625-144">O código a seguir obtém os produtos para um fornecedor:</span><span class="sxs-lookup"><span data-stu-id="6b625-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="6b625-145">Nesse caso, o método retorna um **IQueryable** em vez de um **SingleResult&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="6b625-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="6b625-146">Solicitação de exemplo, HTTP:</span><span class="sxs-lookup"><span data-stu-id="6b625-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="6b625-147">Resposta de exemplo, HTTP:</span><span class="sxs-lookup"><span data-stu-id="6b625-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="6b625-148">Criar uma relação entre entidades</span><span class="sxs-lookup"><span data-stu-id="6b625-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="6b625-149">OData oferece suporte ao criar ou remover relações entre duas entidades existentes.</span><span class="sxs-lookup"><span data-stu-id="6b625-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="6b625-150">Na terminologia de v4 do OData, a relação é uma &quot;referência&quot;.</span><span class="sxs-lookup"><span data-stu-id="6b625-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="6b625-151">(No OData v3, a relação foi chamada um *link*.</span><span class="sxs-lookup"><span data-stu-id="6b625-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="6b625-152">As diferenças de protocolo não importam para este tutorial.)</span><span class="sxs-lookup"><span data-stu-id="6b625-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="6b625-153">Uma referência tem seu próprio URI, com o formulário `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="6b625-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="6b625-154">Por exemplo, aqui está o URI para resolver a referência entre um produto e seus fornecedores:</span><span class="sxs-lookup"><span data-stu-id="6b625-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="6b625-155">Para adicionar uma relação, o cliente envia uma solicitação POST ou PUT para esse endereço.</span><span class="sxs-lookup"><span data-stu-id="6b625-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="6b625-156">Se a propriedade de navegação é uma entidade única, como `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="6b625-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="6b625-157">Lançar se a propriedade de navegação é uma coleção, como `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="6b625-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="6b625-158">O corpo da solicitação contém o URI da entidade na relação.</span><span class="sxs-lookup"><span data-stu-id="6b625-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="6b625-159">Aqui está um exemplo de solicitação:</span><span class="sxs-lookup"><span data-stu-id="6b625-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="6b625-160">Neste exemplo, o cliente envia uma solicitação PUT para `/Products(6)/Supplier/$ref`, que é o URI de $ref para o `Supplier` do produto com ID = 6.</span><span class="sxs-lookup"><span data-stu-id="6b625-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="6b625-161">Se a solicitação for bem-sucedida, o servidor envia uma resposta de 204 (sem conteúdo):</span><span class="sxs-lookup"><span data-stu-id="6b625-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="6b625-162">Aqui está o método do controlador para adicionar uma relação a um `Product`:</span><span class="sxs-lookup"><span data-stu-id="6b625-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="6b625-163">O *navigationProperty* parâmetro especifica qual relacionamento que definir.</span><span class="sxs-lookup"><span data-stu-id="6b625-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="6b625-164">(Se houver mais de uma propriedade de navegação na entidade, você pode adicionar mais `case` instruções.)</span><span class="sxs-lookup"><span data-stu-id="6b625-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="6b625-165">O *link* parâmetro contém o URI do fornecedor.</span><span class="sxs-lookup"><span data-stu-id="6b625-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="6b625-166">API da Web automaticamente analisa o corpo da solicitação para obter o valor para esse parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6b625-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="6b625-167">Para consultar o fornecedor, é necessário o ID (ou chave), que faz parte do *link* parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6b625-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="6b625-168">Para fazer isso, use o método auxiliar a seguir:</span><span class="sxs-lookup"><span data-stu-id="6b625-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="6b625-169">Basicamente, este método usa a biblioteca OData para dividir o caminho URI em segmentos, localize o segmento que contém a chave e converter a chave do tipo correto.</span><span class="sxs-lookup"><span data-stu-id="6b625-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="6b625-170">Excluir uma relação entre entidades</span><span class="sxs-lookup"><span data-stu-id="6b625-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="6b625-171">Para excluir uma relação, o cliente envia uma solicitação HTTP DELETE para o URI $ref:</span><span class="sxs-lookup"><span data-stu-id="6b625-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="6b625-172">Aqui está o método do controlador para excluir a relação entre um produto e um fornecedor:</span><span class="sxs-lookup"><span data-stu-id="6b625-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="6b625-173">Nesse caso, `Product.Supplier` é o &quot;1&quot; final de uma relação de 1-para-muitos, para que você pode remover a relação apenas definindo `Product.Supplier` para `null`.</span><span class="sxs-lookup"><span data-stu-id="6b625-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="6b625-174">No &quot;muitos&quot; final de uma relação, o cliente deve especificar qual relacionados de entidades para remover.</span><span class="sxs-lookup"><span data-stu-id="6b625-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="6b625-175">Para fazer isso, o cliente envia o URI da entidade relacionada na cadeia de caracteres de consulta da solicitação.</span><span class="sxs-lookup"><span data-stu-id="6b625-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="6b625-176">Por exemplo, para remover "Produto 1" de "fornecedor 1":</span><span class="sxs-lookup"><span data-stu-id="6b625-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="6b625-177">Para tanto na API da Web, é preciso incluir um parâmetro extra no `DeleteRef` método.</span><span class="sxs-lookup"><span data-stu-id="6b625-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="6b625-178">Aqui está o método do controlador para remover um produto de `Supplier.Products` relação.</span><span class="sxs-lookup"><span data-stu-id="6b625-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="6b625-179">O *chave* parâmetro é a chave para o fornecedor e o *relatedKey* parâmetro é a chave do produto remover do `Products` relação.</span><span class="sxs-lookup"><span data-stu-id="6b625-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="6b625-180">Observe que o API da Web automaticamente obtém a chave da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="6b625-180">Note that Web API automatically gets the key from the query string.</span></span>
