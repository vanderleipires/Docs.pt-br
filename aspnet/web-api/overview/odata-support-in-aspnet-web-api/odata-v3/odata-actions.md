---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Suporte a ações de OData na API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: 'Em OData, as ações são uma maneira de adicionar comportamentos do lado do servidor que não são facilmente definidos como operações CRUD nas entidades. Alguns usos para ações incluem: implemente...'
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: e02ab21b864e328fe6892a00e5d5aca3f88eb9a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837766"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="69947-104">Suporte a ações de OData na API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="69947-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="69947-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="69947-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="69947-106">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="69947-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="69947-107">Em OData, *ações* são uma maneira de adicionar comportamentos do lado do servidor que não são facilmente definidos como operações CRUD nas entidades.</span><span class="sxs-lookup"><span data-stu-id="69947-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="69947-108">Alguns usos para ações incluem:</span><span class="sxs-lookup"><span data-stu-id="69947-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="69947-109">Implementando transações complexas.</span><span class="sxs-lookup"><span data-stu-id="69947-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="69947-110">Manipulando várias entidades de uma só vez.</span><span class="sxs-lookup"><span data-stu-id="69947-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="69947-111">Permitindo atualizações apenas em determinadas propriedades de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="69947-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="69947-112">Enviando informações para o servidor que não está definido em uma entidade.</span><span class="sxs-lookup"><span data-stu-id="69947-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="69947-113">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="69947-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="69947-114">API Web 2</span><span class="sxs-lookup"><span data-stu-id="69947-114">Web API 2</span></span>
> - <span data-ttu-id="69947-115">OData versão 3</span><span class="sxs-lookup"><span data-stu-id="69947-115">OData Version 3</span></span>
> - <span data-ttu-id="69947-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="69947-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="69947-117">Exemplo: Classificação de um produto</span><span class="sxs-lookup"><span data-stu-id="69947-117">Example: Rating a Product</span></span>

<span data-ttu-id="69947-118">Neste exemplo, queremos que permitem aos usuários classificar os produtos e, em seguida, expor as classificações de média para cada produto.</span><span class="sxs-lookup"><span data-stu-id="69947-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="69947-119">Banco de dados, armazenaremos uma lista de classificações, inseridos em produtos.</span><span class="sxs-lookup"><span data-stu-id="69947-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="69947-120">Aqui está o modelo que podemos usar para representar as classificações no Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="69947-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="69947-121">Mas não queremos que os clientes para POSTAGEM em um `ProductRating` objeto a uma coleção de "Classificações".</span><span class="sxs-lookup"><span data-stu-id="69947-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="69947-122">Intuitivamente, a classificação é associada com a coleção de produtos, e o cliente só será necessário lançar o valor de classificação.</span><span class="sxs-lookup"><span data-stu-id="69947-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="69947-123">Portanto, em vez de usar as operações CRUD normais, podemos definir uma ação que um cliente pode chamar em um produto.</span><span class="sxs-lookup"><span data-stu-id="69947-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="69947-124">Na terminologia do OData, a ação é *vinculado* para entidades do produto.</span><span class="sxs-lookup"><span data-stu-id="69947-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="69947-125">Ações têm efeitos colaterais no servidor.</span><span class="sxs-lookup"><span data-stu-id="69947-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="69947-126">Por esse motivo, eles são invocados usando solicitações HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="69947-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="69947-127">As ações podem ter parâmetros e tipos de retorno, que são descritos nos metadados de serviço.</span><span class="sxs-lookup"><span data-stu-id="69947-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="69947-128">O cliente envia os parâmetros no corpo da solicitação e o servidor envia o valor de retorno no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="69947-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="69947-129">Para invocar a ação de "Taxa de produto", o cliente envia um POST para um URI semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="69947-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="69947-130">Os dados na solicitação POST são simplesmente a classificação de produto:</span><span class="sxs-lookup"><span data-stu-id="69947-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="69947-131">Declarar a ação no modelo de dados de entidade</span><span class="sxs-lookup"><span data-stu-id="69947-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="69947-132">Em sua configuração de API da Web, adicione a ação para o modelo de dados de entidade (EDM):</span><span class="sxs-lookup"><span data-stu-id="69947-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="69947-133">Esse código define "RateProduct" como uma ação que pode ser executada em entidades de produto.</span><span class="sxs-lookup"><span data-stu-id="69947-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="69947-134">Ele também declara que a ação leva um **int** parâmetro denominado "Classificação" e retorna um **int** valor.</span><span class="sxs-lookup"><span data-stu-id="69947-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="69947-135">Adicionar a ação para o controlador</span><span class="sxs-lookup"><span data-stu-id="69947-135">Add the Action to the Controller</span></span>

<span data-ttu-id="69947-136">A ação "RateProduct" está associada às entidades de produto.</span><span class="sxs-lookup"><span data-stu-id="69947-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="69947-137">Para implementar a ação, adicione um método chamado `RateProduct` para o controlador de produtos:</span><span class="sxs-lookup"><span data-stu-id="69947-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="69947-138">Observe que o nome do método corresponde ao nome da ação no EDM.</span><span class="sxs-lookup"><span data-stu-id="69947-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="69947-139">O método tem dois parâmetros:</span><span class="sxs-lookup"><span data-stu-id="69947-139">The method has two parameters:</span></span>

- <span data-ttu-id="69947-140">*chave*: A chave do produto para a taxa.</span><span class="sxs-lookup"><span data-stu-id="69947-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="69947-141">*parâmetros*: um dicionário de valores de parâmetro de ação.</span><span class="sxs-lookup"><span data-stu-id="69947-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="69947-142">Se você estiver usando as convenções de roteamento padrão, o parâmetro de chave deve ser nomeado "chave".</span><span class="sxs-lookup"><span data-stu-id="69947-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="69947-143">Também é importante incluir a **[FromOdataUri]** de atributo, conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="69947-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="69947-144">Esse atributo instrui a API da Web para usar regras de sintaxe do OData, quando ele analisa a chave do URI da solicitação.</span><span class="sxs-lookup"><span data-stu-id="69947-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="69947-145">Use o *parâmetros* dicionário para obter os parâmetros de ação:</span><span class="sxs-lookup"><span data-stu-id="69947-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="69947-146">Se o cliente envia os parâmetros de ação no item correto de formato, o valor de **ModelState** é verdadeiro.</span><span class="sxs-lookup"><span data-stu-id="69947-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="69947-147">Nesse caso, você pode usar o **ODataActionParameters** dicionário para obter os valores de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="69947-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="69947-148">Neste exemplo, o `RateProduct` ação leva um parâmetro único chamado "Classificação".</span><span class="sxs-lookup"><span data-stu-id="69947-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="69947-149">Metadados de ação</span><span class="sxs-lookup"><span data-stu-id="69947-149">Action Metadata</span></span>

<span data-ttu-id="69947-150">Para exibir os metadados de serviço, envie uma solicitação GET para /odata/$ metadata.</span><span class="sxs-lookup"><span data-stu-id="69947-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="69947-151">Aqui está a parte dos metadados que declaram o `RateProduct` ação:</span><span class="sxs-lookup"><span data-stu-id="69947-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="69947-152">O **FunctionImport** elemento declara que a ação.</span><span class="sxs-lookup"><span data-stu-id="69947-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="69947-153">A maioria dos campos são auto-explicativos, mas o dois valem a pena observar:</span><span class="sxs-lookup"><span data-stu-id="69947-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="69947-154">**For IsBindable** significa que a ação pode ser invocada na entidade de destino, pelo menos parte do tempo.</span><span class="sxs-lookup"><span data-stu-id="69947-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="69947-155">**IsAlwaysBindable** significa que a ação sempre pode ser invocada na entidade de destino.</span><span class="sxs-lookup"><span data-stu-id="69947-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="69947-156">A diferença é que algumas ações estão sempre disponíveis para clientes, mas outras ações podem depender do estado da entidade.</span><span class="sxs-lookup"><span data-stu-id="69947-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="69947-157">Por exemplo, suponha que você definir uma ação de "Comprar".</span><span class="sxs-lookup"><span data-stu-id="69947-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="69947-158">Você só poderá comprar um item que está no estoque.</span><span class="sxs-lookup"><span data-stu-id="69947-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="69947-159">Se o item está fora de estoque, um cliente não é possível invocar essa ação.</span><span class="sxs-lookup"><span data-stu-id="69947-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="69947-160">Quando você define o EDM, o **ação** método cria uma ação sempre associável:</span><span class="sxs-lookup"><span data-stu-id="69947-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="69947-161">Vou falar sobre as ações de não-sempre-associável (também chamado de *transitório* ações) mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="69947-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="69947-162">Invocar a ação</span><span class="sxs-lookup"><span data-stu-id="69947-162">Invoking the Action</span></span>

<span data-ttu-id="69947-163">Agora vamos ver como um cliente seria invocar essa ação.</span><span class="sxs-lookup"><span data-stu-id="69947-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="69947-164">Suponha que o cliente quiser dar uma classificação de 2 para o produto com ID = 4.</span><span class="sxs-lookup"><span data-stu-id="69947-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="69947-165">Aqui está uma mensagem de solicitação de exemplo, usando o formato JSON para o corpo da solicitação:</span><span class="sxs-lookup"><span data-stu-id="69947-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="69947-166">Aqui está a mensagem de resposta:</span><span class="sxs-lookup"><span data-stu-id="69947-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="69947-167">Associando uma ação a um conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="69947-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="69947-168">No exemplo anterior, a ação está associada a uma única entidade: O cliente com as tarifas de um único produto.</span><span class="sxs-lookup"><span data-stu-id="69947-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="69947-169">Você também pode vincular uma ação para uma coleção de entidades.</span><span class="sxs-lookup"><span data-stu-id="69947-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="69947-170">Basta fazer as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="69947-170">Just make the following changes:</span></span>

<span data-ttu-id="69947-171">No EDM, adicione a ação para a entidade **coleção** propriedade.</span><span class="sxs-lookup"><span data-stu-id="69947-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="69947-172">No método do controlador, omita a *chave* parâmetro.</span><span class="sxs-lookup"><span data-stu-id="69947-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="69947-173">Agora o cliente invoca a ação no conjunto de entidades de produtos:</span><span class="sxs-lookup"><span data-stu-id="69947-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="69947-174">Ações com parâmetros de coleta</span><span class="sxs-lookup"><span data-stu-id="69947-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="69947-175">As ações podem ter parâmetros que usam uma coleção de valores.</span><span class="sxs-lookup"><span data-stu-id="69947-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="69947-176">No EDM, use **CollectionParameter&lt;T&gt;**  para declarar o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="69947-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="69947-177">Isso declara um parâmetro denominado "Classificações" que usa uma coleção de **int** valores.</span><span class="sxs-lookup"><span data-stu-id="69947-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="69947-178">No método do controlador, você ainda receber o valor do parâmetro de **ODataActionParameters** objeto, mas agora o valor é um **ICollection&lt;int&gt;**  valor:</span><span class="sxs-lookup"><span data-stu-id="69947-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="69947-179">Ações transitórias</span><span class="sxs-lookup"><span data-stu-id="69947-179">Transient Actions</span></span>

<span data-ttu-id="69947-180">No exemplo "RateProduct", os usuários sempre podem classificar um produto para a ação está sempre disponível.</span><span class="sxs-lookup"><span data-stu-id="69947-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="69947-181">Mas, algumas ações dependem do estado da entidade.</span><span class="sxs-lookup"><span data-stu-id="69947-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="69947-182">Por exemplo, em um serviço de aluguel de vídeo, a ação "Check-out" não está sempre disponível.</span><span class="sxs-lookup"><span data-stu-id="69947-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="69947-183">(Isso depende se uma cópia do que o vídeo está disponível.) Esse tipo de ação é chamado uma *transitório* ação.</span><span class="sxs-lookup"><span data-stu-id="69947-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="69947-184">Nos metadados de serviço, tem uma ação transitória **IsAlwaysBindable** igual a false.</span><span class="sxs-lookup"><span data-stu-id="69947-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="69947-185">Isso é, na verdade, o valor padrão, portanto, os metadados terá esta aparência:</span><span class="sxs-lookup"><span data-stu-id="69947-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="69947-186">Veja por que isso é importante: se uma ação for transitória, o servidor precisa dizer ao cliente quando a ação está disponível.</span><span class="sxs-lookup"><span data-stu-id="69947-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="69947-187">Ele faz isso, incluindo um link para a ação na entidade.</span><span class="sxs-lookup"><span data-stu-id="69947-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="69947-188">Aqui está um exemplo de uma entidade de filme:</span><span class="sxs-lookup"><span data-stu-id="69947-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="69947-189">A propriedade "#CheckOut" contém um link para a ação de check-out.</span><span class="sxs-lookup"><span data-stu-id="69947-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="69947-190">Se a ação não estiver disponível, o servidor omite o link.</span><span class="sxs-lookup"><span data-stu-id="69947-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="69947-191">Para declarar uma ação transitória no EDM, chame o **TransientAction** método:</span><span class="sxs-lookup"><span data-stu-id="69947-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="69947-192">Além disso, você deve fornecer uma função que retorna um link de ação para uma determinada entidade.</span><span class="sxs-lookup"><span data-stu-id="69947-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="69947-193">Definir essa função chamando **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="69947-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="69947-194">Você pode escrever a função como uma expressão lambda:</span><span class="sxs-lookup"><span data-stu-id="69947-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="69947-195">Se a ação está disponível, a expressão lambda retorna um link para a ação.</span><span class="sxs-lookup"><span data-stu-id="69947-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="69947-196">O serializador do OData inclui esse link quando ele serializa a entidade.</span><span class="sxs-lookup"><span data-stu-id="69947-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="69947-197">Quando a ação não estiver disponível, a função retornará `null`.</span><span class="sxs-lookup"><span data-stu-id="69947-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69947-198">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="69947-198">Additional Resources</span></span>

[<span data-ttu-id="69947-199">Exemplo de ações de OData</span><span class="sxs-lookup"><span data-stu-id="69947-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
