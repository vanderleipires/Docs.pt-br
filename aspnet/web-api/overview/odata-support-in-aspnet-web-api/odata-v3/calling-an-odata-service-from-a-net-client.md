---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Chamar um serviço OData de um cliente .NET (c#) | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como chamar um serviço OData de um aplicativo cliente c#. Versões de software usadas no tutorial do Visual Studio 2013 (funciona com Visual s....
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: d987e7fbe737055b3e2b690ef3e8de5ca7e2b937
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369777"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="0e4f0-104">Chamar um serviço OData de um cliente .NET (c#)</span><span class="sxs-lookup"><span data-stu-id="0e4f0-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="0e4f0-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0e4f0-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0e4f0-106">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="0e4f0-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="0e4f0-107">Este tutorial mostra como chamar um serviço OData de um aplicativo cliente c#.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0e4f0-108">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="0e4f0-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="0e4f0-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funciona com o Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="0e4f0-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="0e4f0-110">[WCF Data Services Client Library](https://msdn.microsoft.com/library/cc668772.aspx) (Biblioteca de clientes do WCF Data Services)</span><span class="sxs-lookup"><span data-stu-id="0e4f0-110">[WCF Data Services Client Library](https://msdn.microsoft.com/library/cc668772.aspx)</span></span>
> - <span data-ttu-id="0e4f0-111">API Web 2.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-111">Web API 2.</span></span> <span data-ttu-id="0e4f0-112">(O serviço OData de exemplo é criado usando a API Web 2, mas o aplicativo cliente não depende do API da Web).</span><span class="sxs-lookup"><span data-stu-id="0e4f0-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="0e4f0-113">Neste tutorial, eu explicarei como criar um aplicativo cliente que chama um serviço OData.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="0e4f0-114">O serviço OData expõe as seguintes entidades:</span><span class="sxs-lookup"><span data-stu-id="0e4f0-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="0e4f0-115">Os artigos a seguir descrevem como implementar o serviço OData na API da Web.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="0e4f0-116">(Você não precisará lê-los para entender neste tutorial, no entanto.)</span><span class="sxs-lookup"><span data-stu-id="0e4f0-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="0e4f0-117">Criando um ponto de extremidade OData na API Web 2</span><span class="sxs-lookup"><span data-stu-id="0e4f0-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="0e4f0-118">Relações de entidade do OData na API Web 2</span><span class="sxs-lookup"><span data-stu-id="0e4f0-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="0e4f0-119">Ações de OData na API Web 2</span><span class="sxs-lookup"><span data-stu-id="0e4f0-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="0e4f0-120">Gerar o Proxy de serviço</span><span class="sxs-lookup"><span data-stu-id="0e4f0-120">Generate the Service Proxy</span></span>

<span data-ttu-id="0e4f0-121">A primeira etapa é gerar um proxy de serviço.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="0e4f0-122">O proxy de serviço é uma classe .NET que define métodos para acessar o serviço OData.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="0e4f0-123">O proxy converte as chamadas de método em solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="0e4f0-124">Comece abrindo o projeto do serviço OData no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="0e4f0-125">Pressione CTRL + F5 para executar o serviço localmente no IIS Express.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="0e4f0-126">Anote o endereço local, incluindo o número da porta que o Visual Studio atribui.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="0e4f0-127">Quando você cria o proxy, você precisará desse endereço.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="0e4f0-128">Em seguida, abra outra instância do Visual Studio e crie um projeto de aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="0e4f0-129">O aplicativo de console será nosso aplicativo de cliente do OData.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-129">The console application will be our OData client application.</span></span> <span data-ttu-id="0e4f0-130">(Você também pode adicionar o projeto para a mesma solução que o serviço.)</span><span class="sxs-lookup"><span data-stu-id="0e4f0-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="0e4f0-131">As etapas restantes consulte o projeto de console.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="0e4f0-132">No Gerenciador de soluções, clique com botão direito **referências** e selecione **Add Service Reference**.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="0e4f0-133">No **adicionar referência de serviço** caixa de diálogo, digite o endereço do serviço OData:</span><span class="sxs-lookup"><span data-stu-id="0e4f0-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="0e4f0-134">em que *porta* é o número da porta.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="0e4f0-135">Para **Namespace**, digite "ProductService".</span><span class="sxs-lookup"><span data-stu-id="0e4f0-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="0e4f0-136">Essa opção define o namespace da classe de proxy.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="0e4f0-137">Clique em **acesse**.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-137">Click **Go**.</span></span> <span data-ttu-id="0e4f0-138">Visual Studio lê o documento de metadados para descobrir as entidades no serviço do OData.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="0e4f0-139">Clique em **Okey** para adicionar a classe de proxy ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="0e4f0-140">Criar uma instância da classe de Proxy de serviço</span><span class="sxs-lookup"><span data-stu-id="0e4f0-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="0e4f0-141">Dentro de seu `Main` método, crie uma nova instância da classe de proxy, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0e4f0-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="0e4f0-142">Novamente, use o número de porta real em que o seu serviço está em execução.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="0e4f0-143">Quando você implanta seu serviço, você usará o URI do serviço ativo.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="0e4f0-144">Você não precisa atualizar o proxy.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="0e4f0-145">O código a seguir adiciona um manipulador de eventos que imprime os URIs de solicitação para a janela do console.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="0e4f0-146">Essa etapa não é necessária, mas é interessante ver os URIs para cada consulta.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="0e4f0-147">O serviço de consulta</span><span class="sxs-lookup"><span data-stu-id="0e4f0-147">Query the Service</span></span>

<span data-ttu-id="0e4f0-148">O código a seguir obtém a lista de produtos do serviço OData.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="0e4f0-149">Observe que você não precisa escrever nenhum código para enviar a solicitação HTTP ou analisar a resposta.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="0e4f0-150">A classe de proxy faz isso automaticamente quando você enumerar os `Container.Products` coleta na **foreach** loop.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="0e4f0-151">Quando você executa o aplicativo, a saída deve ser semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="0e4f0-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="0e4f0-152">Para obter uma entidade por ID, use um `where` cláusula.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="0e4f0-153">Para o restante deste tópico, não vou mostrar todo o `Main` funcionar, apenas o código necessário para chamar o serviço.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="0e4f0-154">Aplicar as opções de consulta</span><span class="sxs-lookup"><span data-stu-id="0e4f0-154">Apply Query Options</span></span>

<span data-ttu-id="0e4f0-155">OData define [opções de consulta](../supporting-odata-query-options.md) que pode ser usado para filtrar, classificar, dados da página e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="0e4f0-156">No proxy de serviço, você pode aplicar essas opções usando várias expressões de LINQ.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="0e4f0-157">Nesta seção, vou mostrar breves exemplos.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="0e4f0-158">Para obter mais detalhes, consulte o tópico [considerações sobre o LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) no MSDN.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="0e4f0-159">Filtragem ($filter)</span><span class="sxs-lookup"><span data-stu-id="0e4f0-159">Filtering ($filter)</span></span>

<span data-ttu-id="0e4f0-160">Para filtrar, use um `where` cláusula.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="0e4f0-161">A exemplo a seguir filtra por categoria de produto.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="0e4f0-162">Esse código corresponde à consulta de OData a seguir.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="0e4f0-163">Observe que o proxy converte o `where` cláusula em um OData `$filter` expressão.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="0e4f0-164">Classificação ($orderby)</span><span class="sxs-lookup"><span data-stu-id="0e4f0-164">Sorting ($orderby)</span></span>

<span data-ttu-id="0e4f0-165">Para classificar, use um `orderby` cláusula.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="0e4f0-166">O exemplo a seguir classifica por preço, em ordem decrescente.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="0e4f0-167">Aqui está a solicitação correspondente do OData.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="0e4f0-168">Paginação do cliente ($skip Skip e $top)</span><span class="sxs-lookup"><span data-stu-id="0e4f0-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="0e4f0-169">Para grandes conjuntos de entidade, o cliente talvez queira limitar o número de resultados.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="0e4f0-170">Por exemplo, um cliente pode mostrar 10 entradas por vez.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="0e4f0-171">Isso é chamado *paginação do lado do cliente*.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-171">This is called *client-side paging*.</span></span> <span data-ttu-id="0e4f0-172">(Também há [paginação de servidor](../supporting-odata-query-options.md#server-paging), em que o servidor limita o número de resultados.) Para realizar a paginação do lado do cliente, use o LINQ **Skip** e **levar** métodos.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="0e4f0-173">O exemplo a seguir ignora os primeiros 40 resultados e usa os 10.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="0e4f0-174">Aqui está a solicitação correspondente do OData:</span><span class="sxs-lookup"><span data-stu-id="0e4f0-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="0e4f0-175">Selecione ($select) e Expand ($expand)</span><span class="sxs-lookup"><span data-stu-id="0e4f0-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="0e4f0-176">Para incluir as entidades relacionadas, use o `DataServiceQuery<t>.Expand` método.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="0e4f0-177">Por exemplo, para incluir a `Supplier` para cada `Product`:</span><span class="sxs-lookup"><span data-stu-id="0e4f0-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="0e4f0-178">Aqui está a solicitação correspondente do OData:</span><span class="sxs-lookup"><span data-stu-id="0e4f0-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="0e4f0-179">Para alterar a forma da resposta, use o LINQ **selecionar** cláusula.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="0e4f0-180">O exemplo a seguir obtém apenas o nome de cada produto, com nenhuma outra propriedade.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="0e4f0-181">Aqui está a solicitação correspondente do OData:</span><span class="sxs-lookup"><span data-stu-id="0e4f0-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="0e4f0-182">Uma cláusula select pode incluir entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-182">A select clause can include related entities.</span></span> <span data-ttu-id="0e4f0-183">Nesse caso, não chame **expandir**; o proxy automaticamente inclui a expansão neste caso.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="0e4f0-184">O exemplo a seguir obtém o nome e o fornecedor de cada produto.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="0e4f0-185">Aqui está a solicitação correspondente do OData.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="0e4f0-186">Observe que ele inclui o **$expand** opção.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="0e4f0-187">Para obter mais informações sobre $select e $expandir, consulte [usando $select, $expand e $value na API Web 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="0e4f0-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="0e4f0-188">Adicionar uma nova entidade</span><span class="sxs-lookup"><span data-stu-id="0e4f0-188">Add a New Entity</span></span>

<span data-ttu-id="0e4f0-189">Para adicionar uma nova entidade para um conjunto de entidades, chame `AddToEntitySet`, onde *EntitySet* é o nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="0e4f0-190">Por exemplo, `AddToProducts` adiciona um novo `Product` para o `Products` conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="0e4f0-191">Quando você gerar o proxy, WCF Data Services automaticamente cria esses fortemente tipadas **AdicionarPara** métodos.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="0e4f0-192">Para adicionar um link entre duas entidades, use o **AddLink** e **SetLink** métodos.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="0e4f0-193">O código a seguir adiciona um novo fornecedor e um novo produto e, em seguida, cria links entre eles.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="0e4f0-194">Use **AddLink** quando a propriedade de navegação é uma coleção.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="0e4f0-195">Neste exemplo, estamos adicionando um produto para o `Products` coleta sobre o fornecedor.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="0e4f0-196">Use **SetLink** quando a propriedade de navegação é uma única entidade.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="0e4f0-197">Neste exemplo, definimos o `Supplier` propriedade no produto.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="0e4f0-198">Atualizar / Aplicar Patch</span><span class="sxs-lookup"><span data-stu-id="0e4f0-198">Update / Patch</span></span>

<span data-ttu-id="0e4f0-199">Para atualizar uma entidade, chame o **UpdateObject** método.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="0e4f0-200">A atualização é executada quando você chama **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="0e4f0-201">Por padrão, o WCF envia uma solicitação HTTP MERGE.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="0e4f0-202">O **PatchOnUpdate** opção informa ao WCF para enviar uma HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="0e4f0-203">Por que o PATCH versus mesclagem?</span><span class="sxs-lookup"><span data-stu-id="0e4f0-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="0e4f0-204">A especificação HTTP 1.1 original ([RCF 2616](http://tools.ietf.org/html/rfc2616)) não definiu qualquer método HTTP com a semântica de "atualização parcial".</span><span class="sxs-lookup"><span data-stu-id="0e4f0-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="0e4f0-205">Para dar suporte a atualizações parciais, a especificação de OData definido o método de mesclagem.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="0e4f0-206">Em 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) definido o método de PATCH para atualizações parciais.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="0e4f0-207">Você pode ler algumas do histórico desta [postagem de blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) no Blog do WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="0e4f0-208">Hoje, PATCH é preferível direta.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="0e4f0-209">O controlador de OData criado pelo scaffolding de API da Web dá suporte a ambos os métodos.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="0e4f0-210">Se você deseja substituir a entidade inteira (semântica PUT), especifique o **ReplaceOnUpdate** opção.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="0e4f0-211">Isso faz com que o WCF enviar uma solicitação HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="0e4f0-212">Excluir uma entidade</span><span class="sxs-lookup"><span data-stu-id="0e4f0-212">Delete an Entity</span></span>

<span data-ttu-id="0e4f0-213">Para excluir uma entidade, chame **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="0e4f0-214">Invocar uma ação do OData</span><span class="sxs-lookup"><span data-stu-id="0e4f0-214">Invoke an OData Action</span></span>

<span data-ttu-id="0e4f0-215">Em OData, [ações](odata-actions.md) são uma maneira de adicionar comportamentos do lado do servidor que não são facilmente definidos como operações CRUD nas entidades.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="0e4f0-216">Embora o documento de metadados OData descreve as ações, a classe de proxy não cria todos os métodos fortemente tipados para eles.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="0e4f0-217">Você ainda pode invocar uma ação OData usando o genérico **Execute** método.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="0e4f0-218">No entanto, você precisará saber os tipos de dados dos parâmetros e o valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="0e4f0-219">Por exemplo, o `RateProduct` ação leva um parâmetro denominado "Classificação" do tipo `Int32` e retorna um `double`.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="0e4f0-220">O código a seguir mostra como invocar essa ação.</span><span class="sxs-lookup"><span data-stu-id="0e4f0-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="0e4f0-221">Para obter mais informações, consulte[chamar operações de serviço e ações](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="0e4f0-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="0e4f0-222">Uma opção é estender a **contêiner** classe para fornecer um método com rigidez de tipos que invoca a ação:</span><span class="sxs-lookup"><span data-stu-id="0e4f0-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
