---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: "Suporte a relações de entidade no OData v3 com Web API 2 | Microsoft Docs"
author: MikeWasson
description: "A maioria dos conjuntos de dados definem relações entre entidades: os clientes tiverem pedidos; os livros têm autores; os produtos têm fornecedores. Usando o OData, os clientes podem navegar por..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="7e75c-104">Suporte a relações de entidade no OData v3 com Web API 2</span><span class="sxs-lookup"><span data-stu-id="7e75c-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="7e75c-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7e75c-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="7e75c-106">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="7e75c-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="7e75c-107">A maioria dos conjuntos de dados definem relações entre entidades: os clientes tiverem pedidos; os livros têm autores; os produtos têm fornecedores.</span><span class="sxs-lookup"><span data-stu-id="7e75c-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="7e75c-108">Usando o OData, os clientes podem navegar em relações de entidade.</span><span class="sxs-lookup"><span data-stu-id="7e75c-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="7e75c-109">Um produto, você pode encontrar o fornecedor.</span><span class="sxs-lookup"><span data-stu-id="7e75c-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="7e75c-110">Você também pode criar ou remover relações.</span><span class="sxs-lookup"><span data-stu-id="7e75c-110">You can also create or remove relationships.</span></span> <span data-ttu-id="7e75c-111">Por exemplo, você pode definir o fornecedor para um produto.</span><span class="sxs-lookup"><span data-stu-id="7e75c-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="7e75c-112">Este tutorial mostra como dar suporte a essas operações na API da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7e75c-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="7e75c-113">O tutorial se baseia o tutorial [criando um ponto de extremidade OData v3 com a API Web 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="7e75c-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7e75c-114">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="7e75c-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7e75c-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="7e75c-115">Web API 2</span></span>
> - <span data-ttu-id="7e75c-116">OData versão 3</span><span class="sxs-lookup"><span data-stu-id="7e75c-116">OData Version 3</span></span>
> - <span data-ttu-id="7e75c-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="7e75c-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="7e75c-118">Adicionar uma entidade do fornecedor</span><span class="sxs-lookup"><span data-stu-id="7e75c-118">Add a Supplier Entity</span></span>

<span data-ttu-id="7e75c-119">Primeiro, precisamos adicionar um novo tipo de entidade para nosso feed OData.</span><span class="sxs-lookup"><span data-stu-id="7e75c-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="7e75c-120">Vamos adicionar um `Supplier` classe.</span><span class="sxs-lookup"><span data-stu-id="7e75c-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="7e75c-121">Essa classe usa uma cadeia de caracteres para a chave de entidade.</span><span class="sxs-lookup"><span data-stu-id="7e75c-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="7e75c-122">Na prática, o que pode ser menos comuns que usando uma chave de inteiro.</span><span class="sxs-lookup"><span data-stu-id="7e75c-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="7e75c-123">Mas vale a pena vendo como OData lida com outros tipos de chave além de inteiros.</span><span class="sxs-lookup"><span data-stu-id="7e75c-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="7e75c-124">Em seguida, vamos criar uma relação com a adição de um `Supplier` propriedade para o `Product` classe:</span><span class="sxs-lookup"><span data-stu-id="7e75c-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="7e75c-125">Adicionar um novo **DbSet** para o `ProductServiceContext` de classe, para que o Entity Framework incluirá o `Supplier` tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7e75c-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="7e75c-126">Em WebApiConfig.cs, adicione uma entidade de "Fornecedores" para o modelo EDM:</span><span class="sxs-lookup"><span data-stu-id="7e75c-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="7e75c-127">Propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="7e75c-127">Navigation Properties</span></span>

<span data-ttu-id="7e75c-128">Para obter o fornecedor para um produto, o cliente envia uma solicitação GET:</span><span class="sxs-lookup"><span data-stu-id="7e75c-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="7e75c-129">Aqui "Fornecedor" é uma propriedade de navegação no `Product` tipo.</span><span class="sxs-lookup"><span data-stu-id="7e75c-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="7e75c-130">Nesse caso, `Supplier` se refere a um único item, mas uma navegação propriedade também pode retornar uma coleção (relação um-para-muitos ou muitos-para-muitos).</span><span class="sxs-lookup"><span data-stu-id="7e75c-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="7e75c-131">Para dar suporte a essa solicitação, adicione o seguinte método para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="7e75c-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="7e75c-132">O *chave* parâmetro é a chave do produto.</span><span class="sxs-lookup"><span data-stu-id="7e75c-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="7e75c-133">O método retorna a entidade relacionada &#8212;nesse caso, um `Supplier` instância.</span><span class="sxs-lookup"><span data-stu-id="7e75c-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="7e75c-134">O nome do método e o nome de parâmetro são importantes.</span><span class="sxs-lookup"><span data-stu-id="7e75c-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="7e75c-135">Em geral, se a propriedade de navegação é denominada "X", você precisa adicionar um método chamado "GetX".</span><span class="sxs-lookup"><span data-stu-id="7e75c-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="7e75c-136">O método deve ter um parâmetro denominado "*chave*" que corresponde ao tipo de dados da chave do pai.</span><span class="sxs-lookup"><span data-stu-id="7e75c-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="7e75c-137">Também é importante incluir a **[FromOdataUri]** atributo o *chave* parâmetro.</span><span class="sxs-lookup"><span data-stu-id="7e75c-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="7e75c-138">Esse atributo diz API da Web para usar regras de sintaxe do OData quando ele analisa a chave do URI da solicitação.</span><span class="sxs-lookup"><span data-stu-id="7e75c-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="7e75c-139">Criação e exclusão de Links</span><span class="sxs-lookup"><span data-stu-id="7e75c-139">Creating and Deleting Links</span></span>

<span data-ttu-id="7e75c-140">OData oferece suporte ao criar ou remover relações entre duas entidades.</span><span class="sxs-lookup"><span data-stu-id="7e75c-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="7e75c-141">Na terminologia de OData, a relação é um "link".</span><span class="sxs-lookup"><span data-stu-id="7e75c-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="7e75c-142">Cada link tem um URI com o formulário *entidade*/$links /*entidade*.</span><span class="sxs-lookup"><span data-stu-id="7e75c-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="7e75c-143">Por exemplo, o link do produto para fornecedor tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="7e75c-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="7e75c-144">Para criar um novo link, o cliente envia uma solicitação POST para o URI do link.</span><span class="sxs-lookup"><span data-stu-id="7e75c-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="7e75c-145">O corpo da solicitação é o URI da entidade de destino.</span><span class="sxs-lookup"><span data-stu-id="7e75c-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="7e75c-146">Por exemplo, suponha que há um fornecedor com a chave "CTSO".</span><span class="sxs-lookup"><span data-stu-id="7e75c-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="7e75c-147">Para criar um link de "Product(1)" para "Supplier('CTSO')", o cliente envia uma solicitação semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="7e75c-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="7e75c-148">Para excluir um link, o cliente envia uma solicitação de exclusão para o URI do link.</span><span class="sxs-lookup"><span data-stu-id="7e75c-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="7e75c-149">**Criação de Links**</span><span class="sxs-lookup"><span data-stu-id="7e75c-149">**Creating Links**</span></span>

<span data-ttu-id="7e75c-150">Para habilitar um cliente criar links do fornecedor do produto, adicione o seguinte código para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="7e75c-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="7e75c-151">Este método usa três parâmetros:</span><span class="sxs-lookup"><span data-stu-id="7e75c-151">This method takes three parameters:</span></span>

- <span data-ttu-id="7e75c-152">*chave*: A chave para a entidade pai (produto)</span><span class="sxs-lookup"><span data-stu-id="7e75c-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="7e75c-153">*navigationProperty*: O nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="7e75c-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="7e75c-154">Neste exemplo, a propriedade de navegação só é válida é "Fornecedor".</span><span class="sxs-lookup"><span data-stu-id="7e75c-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="7e75c-155">*link*: O URI do OData da entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="7e75c-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="7e75c-156">Esse valor é obtido do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="7e75c-156">This value is taken from the request body.</span></span> <span data-ttu-id="7e75c-157">Por exemplo, o link URI pode ser "`http://localhost/odata/Suppliers('CTSO')`, que significa que o fornecedor com ID = 'CTSO'.</span><span class="sxs-lookup"><span data-stu-id="7e75c-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="7e75c-158">O método usa o link para consultar o fornecedor.</span><span class="sxs-lookup"><span data-stu-id="7e75c-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="7e75c-159">Se o fornecedor de correspondência for encontrado, o método define o `Product.Supplier` propriedade e salva o resultado para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7e75c-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="7e75c-160">A parte mais difícil é analisar o URI do link.</span><span class="sxs-lookup"><span data-stu-id="7e75c-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="7e75c-161">Basicamente, você precisa simular o resultado de enviar uma solicitação GET para esse URI.</span><span class="sxs-lookup"><span data-stu-id="7e75c-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="7e75c-162">O método auxiliar a seguir mostra como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="7e75c-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="7e75c-163">O método invoca o processo de roteamento de API da Web e obtém de volta um **ODataPath** instância que representa o caminho de OData analisado.</span><span class="sxs-lookup"><span data-stu-id="7e75c-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="7e75c-164">Para um link de URI, um dos segmentos deve ser a chave de entidade.</span><span class="sxs-lookup"><span data-stu-id="7e75c-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="7e75c-165">(Caso contrário, o cliente enviou um URI inválido.)</span><span class="sxs-lookup"><span data-stu-id="7e75c-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="7e75c-166">**Excluir Links**</span><span class="sxs-lookup"><span data-stu-id="7e75c-166">**Deleting Links**</span></span>

<span data-ttu-id="7e75c-167">Para excluir um link, adicione o seguinte código para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="7e75c-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="7e75c-168">Neste exemplo, a propriedade de navegação é um único `Supplier` entidade.</span><span class="sxs-lookup"><span data-stu-id="7e75c-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="7e75c-169">Se a propriedade de navegação é uma coleção, o URI para excluir um link deve incluir uma chave para a entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="7e75c-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="7e75c-170">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e75c-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="7e75c-171">Essa solicitação remove a ordem 1 cliente 1.</span><span class="sxs-lookup"><span data-stu-id="7e75c-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="7e75c-172">Nesse caso, o método DeleteLink tem a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="7e75c-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="7e75c-173">O *relatedKey* parâmetro fornece a chave para a entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="7e75c-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="7e75c-174">Assim, na sua `DeleteLink` método, procure a entidade principal pelo *chave* parâmetro, localizar a entidade relacionada, a *relatedKey* parâmetro e, em seguida, remova a associação.</span><span class="sxs-lookup"><span data-stu-id="7e75c-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="7e75c-175">Dependendo de seu modelo de dados, você talvez precise implementar ambas as versões do `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="7e75c-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="7e75c-176">API da Web se chamará a versão correta com base no URI da solicitação.</span><span class="sxs-lookup"><span data-stu-id="7e75c-176">Web API will call the correct version based on the request URI.</span></span>
