---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Ações e funções no OData v4 usando ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: Em OData, ações e funções são uma maneira de adicionar os comportamentos do servidor que não são facilmente definidos como operações CRUD nas entidades. Este tutorial mostra como...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508225"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="95186-104">Ações e funções no OData v4 usando ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="95186-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="95186-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="95186-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="95186-106">Em OData, ações e funções são uma maneira de adicionar os comportamentos do servidor que não são facilmente definidos como operações CRUD nas entidades.</span><span class="sxs-lookup"><span data-stu-id="95186-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="95186-107">Este tutorial mostra como adicionar funções e ações para um ponto de extremidade de v4 do OData, usando 2.2 de API da Web.</span><span class="sxs-lookup"><span data-stu-id="95186-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="95186-108">O tutorial se baseia o tutorial [criar uma instalação do OData v4 ponto de extremidade usando ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="95186-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="95186-109">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="95186-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="95186-110">2.2 da API da Web</span><span class="sxs-lookup"><span data-stu-id="95186-110">Web API 2.2</span></span>
> - <span data-ttu-id="95186-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="95186-111">OData v4</span></span>
> - [<span data-ttu-id="95186-112">Visual Studio 2013 Atualização 2</span><span class="sxs-lookup"><span data-stu-id="95186-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="95186-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="95186-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="95186-114">Versões do tutoriais</span><span class="sxs-lookup"><span data-stu-id="95186-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="95186-115">Para OData versão 3, consulte [ações de OData no ASP.NET Web API 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="95186-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="95186-116">A diferença entre *ações* e *funções* que ações podem ter efeitos colaterais e não funções.</span><span class="sxs-lookup"><span data-stu-id="95186-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="95186-117">Ações e funções podem retornar dados.</span><span class="sxs-lookup"><span data-stu-id="95186-117">Both actions and functions can return data.</span></span> <span data-ttu-id="95186-118">Alguns usos para ações incluem:</span><span class="sxs-lookup"><span data-stu-id="95186-118">Some uses for actions include:</span></span>

- <span data-ttu-id="95186-119">Transações complexas.</span><span class="sxs-lookup"><span data-stu-id="95186-119">Complex transactions.</span></span>
- <span data-ttu-id="95186-120">Manipulando várias entidades de uma vez.</span><span class="sxs-lookup"><span data-stu-id="95186-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="95186-121">Permitir atualizações apenas para determinadas propriedades de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="95186-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="95186-122">Envio de dados que não é uma entidade.</span><span class="sxs-lookup"><span data-stu-id="95186-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="95186-123">Funções são úteis para retornar informações que não correspondem diretamente a uma entidade ou coleção.</span><span class="sxs-lookup"><span data-stu-id="95186-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="95186-124">Uma ação (ou função) pode direcionar uma única entidade ou uma coleção.</span><span class="sxs-lookup"><span data-stu-id="95186-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="95186-125">Na terminologia do OData, este é o *associação*.</span><span class="sxs-lookup"><span data-stu-id="95186-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="95186-126">Você também pode ter &quot;desassociado&quot; ações/funções, que são chamadas como estáticas operações no serviço.</span><span class="sxs-lookup"><span data-stu-id="95186-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="95186-127">Exemplo: Adicionar uma ação</span><span class="sxs-lookup"><span data-stu-id="95186-127">Example: Adding an Action</span></span>

<span data-ttu-id="95186-128">Vamos definir uma ação para avaliar um produto.</span><span class="sxs-lookup"><span data-stu-id="95186-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="95186-129">Este tutorial baseia-se no tutorial [criar uma instalação do OData v4 ponto de extremidade usando ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="95186-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="95186-130">Primeiro, adicione um `ProductRating` modelo para representar as classificações.</span><span class="sxs-lookup"><span data-stu-id="95186-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="95186-131">Também adicionar uma **DbSet** para o `ProductsContext` de classe, para que o EF criará uma tabela de classificação no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="95186-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="95186-132">Adicione a ação de EDM</span><span class="sxs-lookup"><span data-stu-id="95186-132">Add the Action to the EDM</span></span>

<span data-ttu-id="95186-133">Em WebApiConfig.cs, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="95186-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="95186-134">O **EntityTypeConfiguration.Action** método adiciona uma ação para o modelo de dados de entidade (EDM).</span><span class="sxs-lookup"><span data-stu-id="95186-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="95186-135">O **parâmetro** método Especifica um parâmetro de tipo para a ação.</span><span class="sxs-lookup"><span data-stu-id="95186-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="95186-136">Esse código também define o namespace do EDM.</span><span class="sxs-lookup"><span data-stu-id="95186-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="95186-137">O namespace é importante porque o URI para a ação inclui o nome totalmente qualificado de ação:</span><span class="sxs-lookup"><span data-stu-id="95186-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="95186-138">Em uma configuração típica do IIS, o ponto nessa URL fará com que o IIS retornar o erro 404.</span><span class="sxs-lookup"><span data-stu-id="95186-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="95186-139">Você pode resolver esse problema adicionando a seguinte seção ao seu arquivo Web. config:</span><span class="sxs-lookup"><span data-stu-id="95186-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="95186-140">Adicionar um método de controlador para a ação</span><span class="sxs-lookup"><span data-stu-id="95186-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="95186-141">Para habilitar o &quot;taxa&quot; ação, adicione o seguinte método para `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="95186-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="95186-142">Observe que o nome do método corresponde ao nome de ação.</span><span class="sxs-lookup"><span data-stu-id="95186-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="95186-143">O **[HttpPost]** atributo especifica o método é um método HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="95186-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="95186-144">Para chamar a ação, o cliente envia uma solicitação HTTP POST com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="95186-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="95186-145">O &quot;taxa&quot; ação está associada às instâncias de produto, portanto, o URI para a ação é o nome da ação totalmente qualificado acrescentado à entidade URI.</span><span class="sxs-lookup"><span data-stu-id="95186-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="95186-146">(Lembre-se de que definimos o namespace EDM como &quot;ProductService&quot;, portanto, é o nome totalmente qualificado ação &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="95186-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="95186-147">O corpo da solicitação contém os parâmetros de ação como uma carga JSON.</span><span class="sxs-lookup"><span data-stu-id="95186-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="95186-148">API da Web converte automaticamente a carga de JSON para um **ODataActionParameters** objeto, que é apenas um dicionário de valores de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="95186-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="95186-149">Use este dicionário para acessar os parâmetros em seu método de controlador.</span><span class="sxs-lookup"><span data-stu-id="95186-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="95186-150">Se o cliente envia os parâmetros de ação em errado Formatar, o valor de **ModelState** é false.</span><span class="sxs-lookup"><span data-stu-id="95186-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="95186-151">Verificar esse sinalizador em seu método de controlador e retornará um erro se **IsValid** é false.</span><span class="sxs-lookup"><span data-stu-id="95186-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="95186-152">Exemplo: Adicionar uma função</span><span class="sxs-lookup"><span data-stu-id="95186-152">Example: Adding a Function</span></span>

<span data-ttu-id="95186-153">Agora vamos adicionar uma função OData que retorna o produto mais caro.</span><span class="sxs-lookup"><span data-stu-id="95186-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="95186-154">Como antes, a primeira etapa é adicionar a função de EDM.</span><span class="sxs-lookup"><span data-stu-id="95186-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="95186-155">No WebApiConfig.cs, adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="95186-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="95186-156">Nesse caso, a função está associada a coleção de produtos, em vez de instâncias individuais do produto.</span><span class="sxs-lookup"><span data-stu-id="95186-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="95186-157">Os clientes invocam a função enviando uma solicitação GET:</span><span class="sxs-lookup"><span data-stu-id="95186-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="95186-158">Aqui está o método do controlador para esta função:</span><span class="sxs-lookup"><span data-stu-id="95186-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="95186-159">Observe que o nome do método corresponde ao nome de função.</span><span class="sxs-lookup"><span data-stu-id="95186-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="95186-160">O **[HttpGet]** atributo especifica o método é um método HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="95186-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="95186-161">Aqui está a resposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="95186-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="95186-162">Exemplo: Adicionar uma função não associada</span><span class="sxs-lookup"><span data-stu-id="95186-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="95186-163">O exemplo anterior era uma função associada a uma coleção.</span><span class="sxs-lookup"><span data-stu-id="95186-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="95186-164">No exemplo a seguir, vamos criar um *desassociado* função.</span><span class="sxs-lookup"><span data-stu-id="95186-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="95186-165">Funções não associadas são chamadas como estáticas operações no serviço.</span><span class="sxs-lookup"><span data-stu-id="95186-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="95186-166">A função neste exemplo retorna o imposto sobre vendas para um determinado código postal.</span><span class="sxs-lookup"><span data-stu-id="95186-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="95186-167">No arquivo WebApiConfig, adicione a função de EDM:</span><span class="sxs-lookup"><span data-stu-id="95186-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="95186-168">Observe que estamos ligando **função** diretamente no **ODataModelBuilder**, em vez do tipo de entidade ou coleção.</span><span class="sxs-lookup"><span data-stu-id="95186-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="95186-169">Isso informa ao construtor de modelo que a função é desassociada.</span><span class="sxs-lookup"><span data-stu-id="95186-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="95186-170">Aqui está o método do controlador que implementa a função:</span><span class="sxs-lookup"><span data-stu-id="95186-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="95186-171">Não importa qual controlador de API da Web que você colocar este método em.</span><span class="sxs-lookup"><span data-stu-id="95186-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="95186-172">Você pode colocá-lo `ProductsController`, ou definir um controlador separado.</span><span class="sxs-lookup"><span data-stu-id="95186-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="95186-173">O **[ODataRoute]** atributo define o modelo de URI para a função.</span><span class="sxs-lookup"><span data-stu-id="95186-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="95186-174">Aqui está um exemplo de solicitação de cliente:</span><span class="sxs-lookup"><span data-stu-id="95186-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="95186-175">A resposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="95186-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
