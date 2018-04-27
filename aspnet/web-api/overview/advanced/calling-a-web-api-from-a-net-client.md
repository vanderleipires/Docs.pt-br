---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Chamar uma API da Web de um cliente .NET (c#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: fdb74b0eb74ce7f387f49a0b25ceebd3fc389da9
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="0a1eb-102">Chamar uma API da Web de um cliente .NET (c#)</span><span class="sxs-lookup"><span data-stu-id="0a1eb-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="0a1eb-103">por [Mike Wasson](https://github.com/MikeWasson) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0a1eb-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0a1eb-104">[Baixe o projeto concluído](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="0a1eb-104">[Download Completed Project](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="0a1eb-105">[Instruções de download](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="0a1eb-105">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="0a1eb-106">Este tutorial mostra como chamar uma API da web de um aplicativo .NET, usando [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="0a1eb-106">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="0a1eb-107">Neste tutorial, um aplicativo cliente é gravado que consome a API da web seguinte:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-107">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="0a1eb-108">Ação</span><span class="sxs-lookup"><span data-stu-id="0a1eb-108">Action</span></span> | <span data-ttu-id="0a1eb-109">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="0a1eb-109">HTTP method</span></span> | <span data-ttu-id="0a1eb-110">URI relativo</span><span class="sxs-lookup"><span data-stu-id="0a1eb-110">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0a1eb-111">Obter um produto por ID</span><span class="sxs-lookup"><span data-stu-id="0a1eb-111">Get a product by ID</span></span> | <span data-ttu-id="0a1eb-112">OBTER</span><span class="sxs-lookup"><span data-stu-id="0a1eb-112">GET</span></span> | <span data-ttu-id="0a1eb-113">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="0a1eb-113">/api/products/*id*</span></span> |
| <span data-ttu-id="0a1eb-114">Criar um novo produto</span><span class="sxs-lookup"><span data-stu-id="0a1eb-114">Create a new product</span></span> | <span data-ttu-id="0a1eb-115">POSTAR</span><span class="sxs-lookup"><span data-stu-id="0a1eb-115">POST</span></span> | <span data-ttu-id="0a1eb-116">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="0a1eb-116">/api/products</span></span> |
| <span data-ttu-id="0a1eb-117">Atualização de um produto</span><span class="sxs-lookup"><span data-stu-id="0a1eb-117">Update a product</span></span> | <span data-ttu-id="0a1eb-118">PUT</span><span class="sxs-lookup"><span data-stu-id="0a1eb-118">PUT</span></span> | <span data-ttu-id="0a1eb-119">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="0a1eb-119">/api/products/*id*</span></span> |
| <span data-ttu-id="0a1eb-120">Excluir um produto</span><span class="sxs-lookup"><span data-stu-id="0a1eb-120">Delete a product</span></span> | <span data-ttu-id="0a1eb-121">DELETE</span><span class="sxs-lookup"><span data-stu-id="0a1eb-121">DELETE</span></span> | <span data-ttu-id="0a1eb-122">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="0a1eb-122">/api/products/*id*</span></span> |

<span data-ttu-id="0a1eb-123">Para saber como implementar essa API com a API da Web ASP.NET, consulte [criando uma API da Web que dá suporte a operações de CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="0a1eb-123">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="0a1eb-124">Para simplificar, o aplicativo de cliente neste tutorial é um aplicativo de console do Windows.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-124">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="0a1eb-125">**HttpClient** também tem suporte para aplicativos do Windows Phone e da Windows Store.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-125">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="0a1eb-126">Para obter mais informações, consulte [escrevendo código de cliente de API da Web para várias plataformas usando bibliotecas portáteis](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="0a1eb-126">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="0a1eb-127">Criar o aplicativo de Console</span><span class="sxs-lookup"><span data-stu-id="0a1eb-127">Create the Console Application</span></span>

<span data-ttu-id="0a1eb-128">No Visual Studio, crie um novo aplicativo de console do Windows chamado **HttpClientSample** e cole o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-128">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="0a1eb-129">O código anterior é o aplicativo cliente completa.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-129">The preceding code is the complete client app.</span></span>

<span data-ttu-id="0a1eb-130">`RunAsync` é executado e bloqueia até que ela seja concluída.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-130">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="0a1eb-131">A maioria dos **HttpClient** métodos são assíncronas, porque eles realizam e/s de rede.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-131">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="0a1eb-132">Todas as tarefas assíncronas são feitas em `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-132">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="0a1eb-133">Normalmente um aplicativo não bloqueia o thread principal, mas este aplicativo não permite que qualquer interação.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-133">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="0a1eb-134">Instalar as bibliotecas de cliente de API da Web</span><span class="sxs-lookup"><span data-stu-id="0a1eb-134">Install the Web API Client Libraries</span></span>

<span data-ttu-id="0a1eb-135">Use o NuGet Package Manager para instalar o pacote de bibliotecas de cliente de API da Web.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-135">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="0a1eb-136">No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-136">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="0a1eb-137">No pacote Manager Console (PMC), digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-137">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="0a1eb-138">O comando anterior adiciona os seguintes pacotes do NuGet ao projeto:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-138">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="0a1eb-139">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="0a1eb-139">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="0a1eb-140">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="0a1eb-140">Newtonsoft.Json</span></span>

<span data-ttu-id="0a1eb-141">Json.NET é uma estrutura JSON de alto desempenho popular para .NET.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-141">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="0a1eb-142">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="0a1eb-142">Add a Model Class</span></span>

<span data-ttu-id="0a1eb-143">Examine o `Product` classe:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-143">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="0a1eb-144">Essa classe coincide com o modelo de dados usado pela API da web.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-144">This class matches the data model used by the web API.</span></span> <span data-ttu-id="0a1eb-145">Um aplicativo pode usar **HttpClient** para ler um `Product` instância de uma resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-145">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="0a1eb-146">O aplicativo não precisa escrever nenhum código de desserialização.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-146">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="0a1eb-147">Criar e inicializar HttpClient</span><span class="sxs-lookup"><span data-stu-id="0a1eb-147">Create and Initialize HttpClient</span></span>

<span data-ttu-id="0a1eb-148">Examine estático **HttpClient** propriedade:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-148">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="0a1eb-149">**HttpClient** é destinado a ser instanciado uma vez e reutilizados durante a vida útil de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-149">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="0a1eb-150">As condições a seguir podem resultar em **SocketException** erros:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-150">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="0a1eb-151">Criando um novo **HttpClient** instância por solicitação.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-151">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="0a1eb-152">Servidor sob carga pesada.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-152">Server under heavy load.</span></span>

<span data-ttu-id="0a1eb-153">Criando um novo **HttpClient** instância por solicitação pode esgotar os soquetes disponíveis.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-153">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="0a1eb-154">O código a seguir inicializa o **HttpClient** instância:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-154">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="0a1eb-155">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-155">The preceding code:</span></span>

* <span data-ttu-id="0a1eb-156">Define o URI de base para solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-156">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="0a1eb-157">Altere o número da porta para a porta usada para o aplicativo de servidor.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-157">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="0a1eb-158">O aplicativo não funcionará a menos que a porta para o aplicativo de servidor é usada.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-158">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="0a1eb-159">Define o cabeçalho Accept, como "application/json".</span><span class="sxs-lookup"><span data-stu-id="0a1eb-159">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="0a1eb-160">Definir esse cabeçalho informa ao servidor para enviar dados em formato JSON.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-160">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="0a1eb-161">Enviar uma solicitação GET para recuperar um recurso</span><span class="sxs-lookup"><span data-stu-id="0a1eb-161">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="0a1eb-162">O código a seguir envia uma solicitação GET para um produto:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-162">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="0a1eb-163">O **GetAsync** método envia a solicitação HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-163">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="0a1eb-164">Quando o método é concluído, ele retorna um **HttpResponseMessage** que contém a resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-164">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="0a1eb-165">Se o código de status na resposta é um código de êxito, o corpo da resposta contém a representação JSON de um produto.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-165">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="0a1eb-166">Chamar **ReadAsAsync** para desserializar a carga de JSON para um `Product` instância.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-166">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="0a1eb-167">O **ReadAsAsync** método é assíncrono, como o corpo da resposta pode ser arbitrariamente grande.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-167">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="0a1eb-168">**HttpClient** não lança uma exceção quando a resposta HTTP contém um código de erro.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-168">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="0a1eb-169">Em vez disso, o **IsSuccessStatusCode** é de propriedade **false** se o status é um código de erro.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-169">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="0a1eb-170">Se você preferir tratar os códigos de erro HTTP como exceções, chame [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) no objeto de resposta.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-170">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="0a1eb-171">`EnsureSuccessStatusCode` gera uma exceção se o código de status está fora do intervalo de 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-171">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="0a1eb-172">Observe que **HttpClient** pode lançar exceções para outros motivos &mdash; por exemplo, se a solicitação expire.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-172">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="0a1eb-173">Formatadores de tipo de mídia a ser desserializado</span><span class="sxs-lookup"><span data-stu-id="0a1eb-173">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="0a1eb-174">Quando **ReadAsAsync** é chamado sem parâmetros, ele usa um conjunto padrão de *formatadores de mídia* para ler o corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-174">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="0a1eb-175">Os formatadores padrão oferecem suporte a JSON, XML e dados codificados de url do formulário.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-175">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="0a1eb-176">Em vez de usar os formatadores padrão, você pode fornecer uma lista dos formatadores para o **ReadAsAsync** método.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-176">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="0a1eb-177">Usando uma lista de formatadores é útil se você tiver um formatador personalizado do tipo de mídia:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-177">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="0a1eb-178">Para obter mais informações, consulte [formatadores de mídia no ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="0a1eb-178">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="0a1eb-179">Enviando uma solicitação POST para criar um recurso</span><span class="sxs-lookup"><span data-stu-id="0a1eb-179">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="0a1eb-180">O código a seguir envia uma solicitação POST que contém um `Product` instância no formato JSON:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-180">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="0a1eb-181">O **PostAsJsonAsync** método:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-181">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="0a1eb-182">Serializa um objeto em JSON.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-182">Serializes an object to JSON.</span></span>
* <span data-ttu-id="0a1eb-183">Envia a carga de JSON em uma solicitação POST.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-183">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="0a1eb-184">Se a solicitação tiver êxito:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-184">If the request succeeds:</span></span>

* <span data-ttu-id="0a1eb-185">Ele deverá retornar uma resposta de 201 (criado).</span><span class="sxs-lookup"><span data-stu-id="0a1eb-185">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="0a1eb-186">A resposta deve incluir a URL dos recursos criados no cabeçalho de localização.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-186">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="0a1eb-187">Enviar uma solicitação PUT para atualizar um recurso</span><span class="sxs-lookup"><span data-stu-id="0a1eb-187">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="0a1eb-188">O código a seguir envia uma solicitação PUT para atualizar um produto:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-188">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="0a1eb-189">O **PutAsJsonAsync** método funciona como **PostAsJsonAsync**, exceto que ele envia uma solicitação PUT, em vez de POSTAGEM.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-189">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="0a1eb-190">Enviar uma solicitação de exclusão para excluir um recurso</span><span class="sxs-lookup"><span data-stu-id="0a1eb-190">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="0a1eb-191">O código a seguir envia uma solicitação de exclusão para excluir um produto:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-191">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="0a1eb-192">Como obter, uma solicitação de exclusão não tem um corpo de solicitação.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-192">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="0a1eb-193">Você não precisa especificar o formato JSON ou XML com a exclusão.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-193">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="0a1eb-194">Testar o exemplo</span><span class="sxs-lookup"><span data-stu-id="0a1eb-194">Test the sample</span></span>

<span data-ttu-id="0a1eb-195">Para testar o aplicativo cliente:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-195">To test the client app:</span></span>

1. <span data-ttu-id="0a1eb-196">[Baixar](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) e executar o aplicativo de servidor.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-196">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="0a1eb-197">[Instruções de download](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="0a1eb-197">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="0a1eb-198">Verifique se que o aplicativo de servidor está funcionando.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-198">Verify the server app is working.</span></span> <span data-ttu-id="0a1eb-199">Para exaxmple, `http://localhost:64195/api/products` deve retornar uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-199">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="0a1eb-200">Defina o URI de base para solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-200">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="0a1eb-201">Altere o número da porta para a porta usada para o aplicativo de servidor.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-201">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="0a1eb-202">Execute o aplicativo cliente.</span><span class="sxs-lookup"><span data-stu-id="0a1eb-202">Run the client app.</span></span> <span data-ttu-id="0a1eb-203">A saída a seguir será produzida:</span><span class="sxs-lookup"><span data-stu-id="0a1eb-203">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
