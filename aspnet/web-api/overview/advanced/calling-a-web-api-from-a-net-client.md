---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Chamar uma API da Web de um cliente .NET (c#) | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 8fcc5e7c6bc39f961931589128a7a5863482aa4e
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2017
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="95f04-102">Chamar uma API da Web de um cliente .NET (c#)</span><span class="sxs-lookup"><span data-stu-id="95f04-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="95f04-103">por [Mike Wasson](https://github.com/MikeWasson) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="95f04-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="95f04-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="95f04-104">Download Completed Project</span></span>](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

<span data-ttu-id="95f04-105">Este tutorial mostra como chamar uma API da web de um aplicativo .NET, usando [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="95f04-105">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="95f04-106">Neste tutorial, um aplicativo cliente é gravado que consome a API da web seguinte:</span><span class="sxs-lookup"><span data-stu-id="95f04-106">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="95f04-107">Ação</span><span class="sxs-lookup"><span data-stu-id="95f04-107">Action</span></span> | <span data-ttu-id="95f04-108">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="95f04-108">HTTP method</span></span> | <span data-ttu-id="95f04-109">URI relativo</span><span class="sxs-lookup"><span data-stu-id="95f04-109">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="95f04-110">Obter um produto por ID</span><span class="sxs-lookup"><span data-stu-id="95f04-110">Get a product by ID</span></span> | <span data-ttu-id="95f04-111">OBTER</span><span class="sxs-lookup"><span data-stu-id="95f04-111">GET</span></span> | <span data-ttu-id="95f04-112">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="95f04-112">/api/products/*id*</span></span> |
| <span data-ttu-id="95f04-113">Criar um novo produto</span><span class="sxs-lookup"><span data-stu-id="95f04-113">Create a new product</span></span> | <span data-ttu-id="95f04-114">POSTAR</span><span class="sxs-lookup"><span data-stu-id="95f04-114">POST</span></span> | <span data-ttu-id="95f04-115">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="95f04-115">/api/products</span></span> |
| <span data-ttu-id="95f04-116">Atualização de um produto</span><span class="sxs-lookup"><span data-stu-id="95f04-116">Update a product</span></span> | <span data-ttu-id="95f04-117">PUT</span><span class="sxs-lookup"><span data-stu-id="95f04-117">PUT</span></span> | <span data-ttu-id="95f04-118">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="95f04-118">/api/products/*id*</span></span> |
| <span data-ttu-id="95f04-119">Excluir um produto</span><span class="sxs-lookup"><span data-stu-id="95f04-119">Delete a product</span></span> | <span data-ttu-id="95f04-120">DELETE</span><span class="sxs-lookup"><span data-stu-id="95f04-120">DELETE</span></span> | <span data-ttu-id="95f04-121">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="95f04-121">/api/products/*id*</span></span> |

<span data-ttu-id="95f04-122">Para saber como implementar essa API com a API da Web ASP.NET, consulte [criando uma API da Web que dá suporte a operações de CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="95f04-122">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="95f04-123">Para simplificar, o aplicativo de cliente neste tutorial é um aplicativo de console do Windows.</span><span class="sxs-lookup"><span data-stu-id="95f04-123">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="95f04-124">**HttpClient** também tem suporte para aplicativos do Windows Phone e da Windows Store.</span><span class="sxs-lookup"><span data-stu-id="95f04-124">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="95f04-125">Para obter mais informações, consulte [escrevendo código de cliente de API da Web para várias plataformas usando bibliotecas portáteis](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="95f04-125">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="95f04-126">Criar o aplicativo de Console</span><span class="sxs-lookup"><span data-stu-id="95f04-126">Create the Console Application</span></span>

<span data-ttu-id="95f04-127">No Visual Studio, crie um novo aplicativo de console do Windows chamado **HttpClientSample** e cole o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="95f04-127">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="95f04-128">O código anterior é o aplicativo cliente completa.</span><span class="sxs-lookup"><span data-stu-id="95f04-128">The preceding code is the complete client app.</span></span>

<span data-ttu-id="95f04-129">`RunAsync`é executado e bloqueia até que ela seja concluída.</span><span class="sxs-lookup"><span data-stu-id="95f04-129">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="95f04-130">A maioria dos **HttpClient** métodos são assíncronas, porque eles realizam e/s de rede.</span><span class="sxs-lookup"><span data-stu-id="95f04-130">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="95f04-131">Todas as tarefas assíncronas são feitas em `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="95f04-131">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="95f04-132">Normalmente um aplicativo não bloqueia o thread principal, mas este aplicativo não permite que qualquer interação.</span><span class="sxs-lookup"><span data-stu-id="95f04-132">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="95f04-133">Instalar as bibliotecas de cliente de API da Web</span><span class="sxs-lookup"><span data-stu-id="95f04-133">Install the Web API Client Libraries</span></span>

<span data-ttu-id="95f04-134">Use o NuGet Package Manager para instalar o pacote de bibliotecas de cliente de API da Web.</span><span class="sxs-lookup"><span data-stu-id="95f04-134">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="95f04-135">No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="95f04-135">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="95f04-136">No pacote Manager Console (PMC), digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="95f04-136">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="95f04-137">O comando anterior adiciona os seguintes pacotes do NuGet ao projeto:</span><span class="sxs-lookup"><span data-stu-id="95f04-137">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="95f04-138">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="95f04-138">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="95f04-139">Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="95f04-139">Newtonsoft.Json</span></span>

<span data-ttu-id="95f04-140">Json.NET é uma estrutura JSON de alto desempenho popular para .NET.</span><span class="sxs-lookup"><span data-stu-id="95f04-140">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="95f04-141">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="95f04-141">Add a Model Class</span></span>

<span data-ttu-id="95f04-142">Examine o `Product` classe:</span><span class="sxs-lookup"><span data-stu-id="95f04-142">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="95f04-143">Essa classe coincide com o modelo de dados usado pela API da web.</span><span class="sxs-lookup"><span data-stu-id="95f04-143">This class matches the data model used by the web API.</span></span> <span data-ttu-id="95f04-144">Um aplicativo pode usar **HttpClient** para ler um `Product` instância de uma resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="95f04-144">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="95f04-145">O aplicativo não precisa escrever nenhum código de desserialização.</span><span class="sxs-lookup"><span data-stu-id="95f04-145">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="95f04-146">Criar e inicializar HttpClient</span><span class="sxs-lookup"><span data-stu-id="95f04-146">Create and Initialize HttpClient</span></span>

<span data-ttu-id="95f04-147">Examine estático **HttpClient** propriedade:</span><span class="sxs-lookup"><span data-stu-id="95f04-147">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="95f04-148">**HttpClient** é destinado a ser instanciado uma vez e reutilizados durante a vida útil de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="95f04-148">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="95f04-149">As condições a seguir podem resultar em **SocketException** erros:</span><span class="sxs-lookup"><span data-stu-id="95f04-149">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="95f04-150">Criando um novo **HttpClient** instância por solicitação.</span><span class="sxs-lookup"><span data-stu-id="95f04-150">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="95f04-151">Servidor sob carga pesada.</span><span class="sxs-lookup"><span data-stu-id="95f04-151">Server under heavy load.</span></span>

<span data-ttu-id="95f04-152">Criando um novo **HttpClient** instância por solicitação pode esgotar os soquetes disponíveis.</span><span class="sxs-lookup"><span data-stu-id="95f04-152">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="95f04-153">O código a seguir inicializa o **HttpClient** instância:</span><span class="sxs-lookup"><span data-stu-id="95f04-153">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="95f04-154">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="95f04-154">The preceding code:</span></span>

* <span data-ttu-id="95f04-155">Define o URI de base para solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="95f04-155">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="95f04-156">Altere o número da porta para a porta usada para o aplicativo de servidor.</span><span class="sxs-lookup"><span data-stu-id="95f04-156">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="95f04-157">O aplicativo não funcionará a menos que a porta para o aplicativo de servidor é usada.</span><span class="sxs-lookup"><span data-stu-id="95f04-157">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="95f04-158">Define o cabeçalho Accept, como "application/json".</span><span class="sxs-lookup"><span data-stu-id="95f04-158">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="95f04-159">Definir esse cabeçalho informa ao servidor para enviar dados em formato JSON.</span><span class="sxs-lookup"><span data-stu-id="95f04-159">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="95f04-160">Enviar uma solicitação GET para recuperar um recurso</span><span class="sxs-lookup"><span data-stu-id="95f04-160">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="95f04-161">O código a seguir envia uma solicitação GET para um produto:</span><span class="sxs-lookup"><span data-stu-id="95f04-161">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="95f04-162">O **GetAsync** método envia a solicitação HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="95f04-162">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="95f04-163">Quando o método é concluído, ele retorna um **HttpResponseMessage** que contém a resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="95f04-163">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="95f04-164">Se o código de status na resposta é um código de êxito, o corpo da resposta contém a representação JSON de um produto.</span><span class="sxs-lookup"><span data-stu-id="95f04-164">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="95f04-165">Chamar **ReadAsAsync** para desserializar a carga de JSON para um `Product` instância.</span><span class="sxs-lookup"><span data-stu-id="95f04-165">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="95f04-166">O **ReadAsAsync** método é assíncrono, como o corpo da resposta pode ser arbitrariamente grande.</span><span class="sxs-lookup"><span data-stu-id="95f04-166">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="95f04-167">**HttpClient** não lança uma exceção quando a resposta HTTP contém um código de erro.</span><span class="sxs-lookup"><span data-stu-id="95f04-167">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="95f04-168">Em vez disso, o **IsSuccessStatusCode** é de propriedade **false** se o status é um código de erro.</span><span class="sxs-lookup"><span data-stu-id="95f04-168">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="95f04-169">Se você preferir tratar os códigos de erro HTTP como exceções, chame [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) no objeto de resposta.</span><span class="sxs-lookup"><span data-stu-id="95f04-169">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="95f04-170">`EnsureSuccessStatusCode`gera uma exceção se o código de status está fora do intervalo de 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="95f04-170">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="95f04-171">Observe que **HttpClient** pode lançar exceções para outros motivos &mdash; por exemplo, se a solicitação expire.</span><span class="sxs-lookup"><span data-stu-id="95f04-171">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="95f04-172">Formatadores de tipo de mídia a ser desserializado</span><span class="sxs-lookup"><span data-stu-id="95f04-172">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="95f04-173">Quando **ReadAsAsync** é chamado sem parâmetros, ele usa um conjunto padrão de *formatadores de mídia* para ler o corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="95f04-173">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="95f04-174">Os formatadores padrão oferecem suporte a JSON, XML e dados codificados de url do formulário.</span><span class="sxs-lookup"><span data-stu-id="95f04-174">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="95f04-175">Em vez de usar os formatadores padrão, você pode fornecer uma lista dos formatadores para o **ReadAsAsync** método.</span><span class="sxs-lookup"><span data-stu-id="95f04-175">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="95f04-176">Usando um uma lista dos formatadores é útil se você tiver um formatador personalizado do tipo de mídia:</span><span class="sxs-lookup"><span data-stu-id="95f04-176">Using a a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="95f04-177">Para obter mais informações, consulte [formatadores de mídia no ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="95f04-177">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="95f04-178">Enviando uma solicitação POST para criar um recurso</span><span class="sxs-lookup"><span data-stu-id="95f04-178">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="95f04-179">O código a seguir envia uma solicitação POST que contém um `Product` instância no formato JSON:</span><span class="sxs-lookup"><span data-stu-id="95f04-179">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="95f04-180">O **PostAsJsonAsync** método:</span><span class="sxs-lookup"><span data-stu-id="95f04-180">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="95f04-181">Serializa um objeto em JSON.</span><span class="sxs-lookup"><span data-stu-id="95f04-181">Serializes an object to JSON.</span></span>
* <span data-ttu-id="95f04-182">Envia a carga de JSON em uma solicitação POST.</span><span class="sxs-lookup"><span data-stu-id="95f04-182">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="95f04-183">Se a solicitação tiver êxito:</span><span class="sxs-lookup"><span data-stu-id="95f04-183">If the request succeeds:</span></span>

* <span data-ttu-id="95f04-184">Ele deverá retornar uma resposta de 201 (criado).</span><span class="sxs-lookup"><span data-stu-id="95f04-184">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="95f04-185">A resposta deve incluir a URL dos recursos criados no cabeçalho de localização.</span><span class="sxs-lookup"><span data-stu-id="95f04-185">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="95f04-186">Enviar uma solicitação PUT para atualizar um recurso</span><span class="sxs-lookup"><span data-stu-id="95f04-186">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="95f04-187">O código a seguir envia uma solicitação PUT para atualizar um produto:</span><span class="sxs-lookup"><span data-stu-id="95f04-187">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="95f04-188">O **PutAsJsonAsync** método funciona como **PostAsJsonAsync**, exceto que ele envia uma solicitação PUT, em vez de POSTAGEM.</span><span class="sxs-lookup"><span data-stu-id="95f04-188">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="95f04-189">Enviar uma solicitação de exclusão para excluir um recurso</span><span class="sxs-lookup"><span data-stu-id="95f04-189">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="95f04-190">O código a seguir envia uma solicitação de exclusão para excluir um produto:</span><span class="sxs-lookup"><span data-stu-id="95f04-190">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="95f04-191">Como obter, uma solicitação de exclusão não tem um corpo de solicitação.</span><span class="sxs-lookup"><span data-stu-id="95f04-191">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="95f04-192">Você não precisa especificar o formato JSON ou XML com a exclusão.</span><span class="sxs-lookup"><span data-stu-id="95f04-192">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="95f04-193">Testar o exemplo</span><span class="sxs-lookup"><span data-stu-id="95f04-193">Test the sample</span></span>

<span data-ttu-id="95f04-194">Para testar o aplicativo cliente:</span><span class="sxs-lookup"><span data-stu-id="95f04-194">To test the client app:</span></span>

1. <span data-ttu-id="95f04-195">[Baixar](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/samples/server) e executar o aplicativo de servidor.</span><span class="sxs-lookup"><span data-stu-id="95f04-195">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/samples/server) and run the server app.</span></span> <span data-ttu-id="95f04-196">[Instruções de download](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="95f04-196">[Download instructions](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="95f04-197">Verifique se que o aplicativo de servidor está funcionando.</span><span class="sxs-lookup"><span data-stu-id="95f04-197">Verify the server app is working.</span></span> <span data-ttu-id="95f04-198">Para exaxmple, `http://localhost:64195/api/products` deve retornar uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="95f04-198">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="95f04-199">Defina o URI de base para solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="95f04-199">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="95f04-200">Altere o número da porta para a porta usada para o aplicativo de servidor.</span><span class="sxs-lookup"><span data-stu-id="95f04-200">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="95f04-201">Execute o aplicativo cliente.</span><span class="sxs-lookup"><span data-stu-id="95f04-201">Run the client app.</span></span> <span data-ttu-id="95f04-202">A saída a seguir será produzida:</span><span class="sxs-lookup"><span data-stu-id="95f04-202">The following output is produced:</span></span>

 ```console
 Created at http://localhost:64195/api/products/4
Name: Gizmo     Price: 100.0    Category: Widgets
Updating price...
Name: Gizmo     Price: 80.0     Category: Widgets
Deleted (HTTP Status = 204)
```
