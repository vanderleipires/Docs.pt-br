---
title: "Formato de dados de resposta no ASP.NET MVC de núcleo"
author: ardalis
description: Saiba como formatar os dados de resposta MVC do ASP.NET Core.
keywords: "Núcleo do ASP.NET, dados de resposta, IOutputFormatter, IActionResult"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a7fd1ecb61c7b1559f8bd304c30f7201864bd448
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="52a95-104">Introdução à formatação de dados de resposta no ASP.NET MVC de núcleo</span><span class="sxs-lookup"><span data-stu-id="52a95-104">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="52a95-105">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="52a95-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="52a95-106">Núcleo do ASP.NET MVC tem suporte interno para formatação de dados de resposta, usando formatos fixos ou em resposta às especificações do cliente.</span><span class="sxs-lookup"><span data-stu-id="52a95-106">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="52a95-107">[Exibir ou baixar o exemplo do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span><span class="sxs-lookup"><span data-stu-id="52a95-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="52a95-108">Resultados de ação específica de formato</span><span class="sxs-lookup"><span data-stu-id="52a95-108">Format-Specific Action Results</span></span>

<span data-ttu-id="52a95-109">Alguns tipos de resultado de ação são específicos para um determinado formato, como `JsonResult` e `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="52a95-109">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="52a95-110">Ações podem retornar resultados específicos que são sempre formatados de maneira específica.</span><span class="sxs-lookup"><span data-stu-id="52a95-110">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="52a95-111">Por exemplo, retornando um `JsonResult` retornará dados formatados em JSON, independentemente de preferências do cliente.</span><span class="sxs-lookup"><span data-stu-id="52a95-111">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="52a95-112">Da mesma forma, retornando um `ContentResult` retornará dados de cadeia de caracteres formatada para texto sem formatação (assim como simplesmente retornar uma cadeia de caracteres).</span><span class="sxs-lookup"><span data-stu-id="52a95-112">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="52a95-113">Uma ação não é necessário para retornar qualquer tipo determinado; MVC oferece suporte a qualquer valor de retorno do objeto.</span><span class="sxs-lookup"><span data-stu-id="52a95-113">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="52a95-114">Se uma ação retorna um `IActionResult` herda de implementação e o controlador de `Controller`, os desenvolvedores têm muitos métodos auxiliares correspondente a muitas das opções.</span><span class="sxs-lookup"><span data-stu-id="52a95-114">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="52a95-115">Resultados de ações que retornam objetos que não são `IActionResult` tipos serão serializados usando apropriada `IOutputFormatter` implementação.</span><span class="sxs-lookup"><span data-stu-id="52a95-115">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="52a95-116">Para retornar dados em um formato específico de um controlador que herda o `Controller` classe base, use o método auxiliar interno `Json` para retornar JSON e `Content` para texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="52a95-116">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="52a95-117">O método de ação deve retornar o tipo de resultado específico (por exemplo, `JsonResult`) ou `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="52a95-117">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="52a95-118">Retornando dados formatados em JSON:</span><span class="sxs-lookup"><span data-stu-id="52a95-118">Returning JSON-formatted data:</span></span>

<span data-ttu-id="52a95-119">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]</span><span class="sxs-lookup"><span data-stu-id="52a95-119">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]</span></span>

<span data-ttu-id="52a95-120">Exemplo de resposta desta ação:</span><span class="sxs-lookup"><span data-stu-id="52a95-120">Sample response from this action:</span></span>

![Guia de rede das ferramentas de desenvolvedor no Microsoft Edge mostrando o tipo de conteúdo da resposta é application/json](formatting/_static/json-response.png)

<span data-ttu-id="52a95-122">Observe que o tipo de conteúdo da resposta é `application/json`, conforme mostrado na lista de solicitações de rede e na seção de cabeçalhos de resposta.</span><span class="sxs-lookup"><span data-stu-id="52a95-122">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="52a95-123">Além disso, observe a lista de opções apresentadas pelo navegador (nesse caso, Microsoft Edge) no cabeçalho Accept na seção de cabeçalhos de solicitação.</span><span class="sxs-lookup"><span data-stu-id="52a95-123">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="52a95-124">A técnica atual está ignorando esse cabeçalho. obedecer a ele é discutido abaixo.</span><span class="sxs-lookup"><span data-stu-id="52a95-124">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="52a95-125">Para retornar dados de texto simples formatado, use `ContentResult` e `Content` auxiliar:</span><span class="sxs-lookup"><span data-stu-id="52a95-125">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

<span data-ttu-id="52a95-126">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]</span><span class="sxs-lookup"><span data-stu-id="52a95-126">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]</span></span>

<span data-ttu-id="52a95-127">Uma resposta desta ação:</span><span class="sxs-lookup"><span data-stu-id="52a95-127">A response from this action:</span></span>

![Guia de rede das ferramentas de desenvolvedor no Microsoft Edge mostrando o tipo de conteúdo da resposta é texto/sem formatação](formatting/_static/text-response.png)

<span data-ttu-id="52a95-129">Observe neste caso o `Content-Type` retornado é `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="52a95-129">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="52a95-130">Você também pode obter esse mesmo comportamento usando apenas um tipo de resposta de cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="52a95-130">You can also achieve this same behavior using just a string response type:</span></span>

<span data-ttu-id="52a95-131">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]</span><span class="sxs-lookup"><span data-stu-id="52a95-131">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]</span></span>

>[!TIP]
> <span data-ttu-id="52a95-132">Para ações não trivial com várias opções (por exemplo, códigos de status HTTP diferentes com base no resultado de operações executadas) ou tipos de retorno, prefira `IActionResult` como o tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="52a95-132">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="52a95-133">Negociação de conteúdo</span><span class="sxs-lookup"><span data-stu-id="52a95-133">Content Negotiation</span></span>

<span data-ttu-id="52a95-134">Negociação de conteúdo (*conneg* abreviada) ocorre quando o cliente especifica uma [cabeçalho Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="52a95-134">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="52a95-135">O formato padrão usado pelo ASP.NET MVC de núcleo é JSON.</span><span class="sxs-lookup"><span data-stu-id="52a95-135">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="52a95-136">Negociação de conteúdo é implementada pelo `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="52a95-136">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="52a95-137">Ele também é compilado em código de status retornados de resultados de ação específica dos métodos auxiliares (que se baseiam em `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="52a95-137">It is also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="52a95-138">Você também pode retornar um tipo de modelo (uma classe que você definiu como seu tipo de transferência de dados) e o framework quebrará automaticamente em um `ObjectResult` para você.</span><span class="sxs-lookup"><span data-stu-id="52a95-138">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="52a95-139">O método de ação a seguir usa o `Ok` e `NotFound` métodos auxiliares:</span><span class="sxs-lookup"><span data-stu-id="52a95-139">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

<span data-ttu-id="52a95-140">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]</span><span class="sxs-lookup"><span data-stu-id="52a95-140">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]</span></span>

<span data-ttu-id="52a95-141">Retornará uma resposta formatada em JSON, a menos que outro formato foi solicitado e o servidor pode retornar o formato solicitado.</span><span class="sxs-lookup"><span data-stu-id="52a95-141">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="52a95-142">Você pode usar uma ferramenta como [Fiddler](http://www.telerik.com/fiddler) para criar uma solicitação que inclui um cabeçalho Accept e especifique outro formato.</span><span class="sxs-lookup"><span data-stu-id="52a95-142">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="52a95-143">Nesse caso, se o servidor tiver um *formatador* que pode produzir uma resposta no formato solicitado, o resultado será retornado no formato preferencial de cliente.</span><span class="sxs-lookup"><span data-stu-id="52a95-143">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Console do Fiddler mostrando um criados manualmente obter solicitação com um valor de cabeçalho Accept de application/xml](formatting/_static/fiddler-composer.png)

<span data-ttu-id="52a95-145">A captura de tela acima, o criador do Fiddler foi usado para gerar uma solicitação, especificando `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="52a95-145">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="52a95-146">Por padrão, MVC do ASP.NET Core só dá suporte a JSON, portanto, mesmo quando outro formato é especificado, o resultado retornado é ainda formatada em JSON.</span><span class="sxs-lookup"><span data-stu-id="52a95-146">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="52a95-147">Você verá como adicionar formatadores adicionais na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="52a95-147">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="52a95-148">Ações do controlador podem retornar POCOs (Plain Old CLR Objects), caso em que o ASP.NET MVC criará automaticamente um `ObjectResult` para você que encapsula o objeto.</span><span class="sxs-lookup"><span data-stu-id="52a95-148">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="52a95-149">O cliente receberá o objeto serializado formatado (formato JSON é o padrão; você pode configurar outros formatos ou XML).</span><span class="sxs-lookup"><span data-stu-id="52a95-149">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="52a95-150">Se o objeto que está sendo retornado é `null`, a estrutura será retornado um `204 No Content` resposta.</span><span class="sxs-lookup"><span data-stu-id="52a95-150">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="52a95-151">Retornando um tipo de objeto:</span><span class="sxs-lookup"><span data-stu-id="52a95-151">Returning an object type:</span></span>

<span data-ttu-id="52a95-152">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]</span><span class="sxs-lookup"><span data-stu-id="52a95-152">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]</span></span>

<span data-ttu-id="52a95-153">No exemplo, uma solicitação para um alias válido autor receberá uma resposta 200 Okey com dados do autor.</span><span class="sxs-lookup"><span data-stu-id="52a95-153">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="52a95-154">Uma solicitação para um alias inválido receberá uma resposta de 204 sem conteúdo.</span><span class="sxs-lookup"><span data-stu-id="52a95-154">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="52a95-155">Capturas de tela mostrando a resposta nos formatos XML e JSON são mostradas abaixo.</span><span class="sxs-lookup"><span data-stu-id="52a95-155">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="52a95-156">Processo de negociação de conteúdo</span><span class="sxs-lookup"><span data-stu-id="52a95-156">Content Negotiation Process</span></span>

<span data-ttu-id="52a95-157">Conteúdo *negociação* só ocorre se um `Accept` cabeçalho aparece na solicitação.</span><span class="sxs-lookup"><span data-stu-id="52a95-157">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="52a95-158">Quando uma solicitação contém um cabeçalho accept, o framework vai enumerar os tipos de mídia no cabeçalho accept na ordem de preferência e tentará encontrar um formatador que pode produzir uma resposta em um dos formatos especificados pelo cabeçalho accept.</span><span class="sxs-lookup"><span data-stu-id="52a95-158">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="52a95-159">No caso de nenhum formatador for encontrado que pode satisfazer a solicitação do cliente, a estrutura tentará localizar o primeiro formatador que pode produzir uma resposta (a menos que o desenvolvedor tenha configurado a opção em `MvcOptions` para retornar 406 não aceitável em vez disso).</span><span class="sxs-lookup"><span data-stu-id="52a95-159">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="52a95-160">Se a solicitação especifica XML, mas o formatador XML não tiver sido configurado, será usado o formatador JSON.</span><span class="sxs-lookup"><span data-stu-id="52a95-160">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="52a95-161">Geralmente, se nenhum formatador for configurado que pode fornecer o formato solicitado e o primeiro formatador que pode formatar o objeto é usado.</span><span class="sxs-lookup"><span data-stu-id="52a95-161">More generally, if no formatter is configured that can provide the requested format, then the first formatter than can format the object is used.</span></span> <span data-ttu-id="52a95-162">Se nenhum cabeçalho for especificado, o primeiro formatador que pode lidar com o objeto a ser retornado será usado para serializar a resposta.</span><span class="sxs-lookup"><span data-stu-id="52a95-162">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="52a95-163">Nesse caso, não há nenhuma negociação ocorra - o servidor é determinar em qual formato será usado.</span><span class="sxs-lookup"><span data-stu-id="52a95-163">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="52a95-164">Se o cabeçalho Accept contém `*/*`, o cabeçalho será ignorado, a menos que `RespectBrowserAcceptHeader` é definido como true no `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="52a95-164">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="52a95-165">Navegadores e negociação de conteúdo</span><span class="sxs-lookup"><span data-stu-id="52a95-165">Browsers and Content Negotiation</span></span>

<span data-ttu-id="52a95-166">Diferentemente dos clientes de API típica, os navegadores da web tendem a fornecer `Accept` cabeçalhos que incluem uma ampla variedade de formatos, incluindo caracteres curinga.</span><span class="sxs-lookup"><span data-stu-id="52a95-166">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="52a95-167">Por padrão, quando o framework detecta que a solicitação é proveniente de um navegador, ele ignorará o `Accept` cabeçalho e retorno em vez disso, o conteúdo do aplicativo configurada do formato padrão (JSON, a menos que o contrário configurada).</span><span class="sxs-lookup"><span data-stu-id="52a95-167">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="52a95-168">Isso fornece uma experiência mais consistente ao usar navegadores diferentes para consumir APIs.</span><span class="sxs-lookup"><span data-stu-id="52a95-168">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="52a95-169">Se você preferir cabeçalhos de aceitação do seu navegador de honrar do aplicativo, você pode configurar isso como parte da configuração do MVC definindo `RespectBrowserAcceptHeader` para `true` no `ConfigureServices` método *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="52a95-169">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
}
```

## <a name="configuring-formatters"></a><span data-ttu-id="52a95-170">Configurando formatadores</span><span class="sxs-lookup"><span data-stu-id="52a95-170">Configuring Formatters</span></span>

<span data-ttu-id="52a95-171">Se seu aplicativo precisa para dar suporte a formatos adicionais além do padrão de JSON, você pode adicionar pacotes do NuGet e configurar MVC para dar suporte a elas.</span><span class="sxs-lookup"><span data-stu-id="52a95-171">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="52a95-172">Há formatadores separados para entrada e saída.</span><span class="sxs-lookup"><span data-stu-id="52a95-172">There are separate formatters for input and output.</span></span> <span data-ttu-id="52a95-173">Formatadores de entrada são usados por [modelo associação](model-binding.md); formatadores de saída são usados para formatar as respostas.</span><span class="sxs-lookup"><span data-stu-id="52a95-173">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="52a95-174">Você também pode configurar [personalizado formatadores](../advanced/custom-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="52a95-174">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="52a95-175">Adicionando suporte para o formato XML</span><span class="sxs-lookup"><span data-stu-id="52a95-175">Adding XML Format Support</span></span>

<span data-ttu-id="52a95-176">Para adicionar suporte para formatação XML, instale o `Microsoft.AspNetCore.Mvc.Formatters.Xml` pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="52a95-176">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="52a95-177">Adicionar o XmlSerializerFormatters a configuração do MVC no *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="52a95-177">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

<span data-ttu-id="52a95-178">[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="52a95-178">[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]</span></span>

<span data-ttu-id="52a95-179">Como alternativa, você pode adicionar apenas o formatador de saída:</span><span class="sxs-lookup"><span data-stu-id="52a95-179">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="52a95-180">Essas duas abordagens serializa os resultados usando `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="52a95-180">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="52a95-181">Se preferir, você pode usar o `System.Runtime.Serialization.DataContractSerializer` adicionando o formatador associado:</span><span class="sxs-lookup"><span data-stu-id="52a95-181">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="52a95-182">Depois de adicionar suporte para formatação XML, seus métodos de controlador devem retornar o formato apropriado com base na solicitação de `Accept` cabeçalho, como este exemplo demonstra o Fiddler:</span><span class="sxs-lookup"><span data-stu-id="52a95-182">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Console do Fiddler: guia o bruto para a solicitação mostra o valor do cabeçalho Accept é aplicativo/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="52a95-185">Você pode ver na guia inspetores que a solicitação GET bruto foi feita com um `Accept: application/xml` conjunto de cabeçalhos.</span><span class="sxs-lookup"><span data-stu-id="52a95-185">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="52a95-186">O painel de resposta mostra o `Content-Type: application/xml` cabeçalho e o `Author` objeto tem foi serializado como XML.</span><span class="sxs-lookup"><span data-stu-id="52a95-186">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="52a95-187">Use a guia criador para modificar a solicitação para especificar `application/json` no `Accept` cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="52a95-187">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="52a95-188">Executar a solicitação e a resposta será formatada como JSON:</span><span class="sxs-lookup"><span data-stu-id="52a95-188">Execute the request, and the response will be formatted as JSON:</span></span>

![Console do Fiddler: guia o bruto para a solicitação mostra o valor do cabeçalho Accept é application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="52a95-191">Nessa tela, você pode ver a solicitação define um cabeçalho de `Accept: application/json` e a resposta especifica o mesmo que seu `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="52a95-191">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="52a95-192">O `Author` objeto é mostrado no corpo da resposta no formato JSON.</span><span class="sxs-lookup"><span data-stu-id="52a95-192">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="52a95-193">Forçar um formato específico</span><span class="sxs-lookup"><span data-stu-id="52a95-193">Forcing a Particular Format</span></span>

<span data-ttu-id="52a95-194">Se você quiser restringir os formatos de resposta para uma ação específica é possível, você pode aplicar o `[Produces]` filtro.</span><span class="sxs-lookup"><span data-stu-id="52a95-194">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="52a95-195">O `[Produces]` filtro especifica os formatos de resposta para uma ação específica (ou controlador).</span><span class="sxs-lookup"><span data-stu-id="52a95-195">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="52a95-196">Como a maioria [filtros](../controllers/filters.md), isso pode ser aplicado no escopo global, controlador ou ação.</span><span class="sxs-lookup"><span data-stu-id="52a95-196">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="52a95-197">O `[Produces]` filtro força todas as ações dentro a `AuthorsController` para retornar respostas formatadas em JSON, mesmo se outros formatadores foram configurados para o aplicativo e o cliente fornecida um `Accept` cabeçalho de solicitação diferente, disponível formato.</span><span class="sxs-lookup"><span data-stu-id="52a95-197">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="52a95-198">Consulte [filtros](../controllers/filters.md) para saber mais, incluindo a aplicação de filtros globais.</span><span class="sxs-lookup"><span data-stu-id="52a95-198">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="52a95-199">Formatadores casos especiais</span><span class="sxs-lookup"><span data-stu-id="52a95-199">Special Case Formatters</span></span>

<span data-ttu-id="52a95-200">Alguns casos especiais são implementados usando formatadores internos.</span><span class="sxs-lookup"><span data-stu-id="52a95-200">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="52a95-201">Por padrão, `string` tipos de retorno serão formatados como *texto/simples* (*texto/html* se solicitado `Accept` cabeçalho).</span><span class="sxs-lookup"><span data-stu-id="52a95-201">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="52a95-202">Esse comportamento pode ser removido, removendo o `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="52a95-202">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="52a95-203">Remover formatadores no `Configure` método *Startup.cs* (mostrada abaixo).</span><span class="sxs-lookup"><span data-stu-id="52a95-203">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="52a95-204">Ações com um objeto de modelo retornam tipo retornará um 204 sem conteúdo resposta ao retornar `null`.</span><span class="sxs-lookup"><span data-stu-id="52a95-204">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="52a95-205">Esse comportamento pode ser removido, removendo o `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="52a95-205">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="52a95-206">O código a seguir remove o `TextOutputFormatter` e `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="52a95-206">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="52a95-207">Sem o `TextOutputFormatter`, `string` retornar tipos de retorno 406 não aceitável, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="52a95-207">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="52a95-208">Observe que se um formatador XML existir, ele será formato `string` tipos de retorno se a `TextOutputFormatter` é removido.</span><span class="sxs-lookup"><span data-stu-id="52a95-208">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="52a95-209">Sem o `HttpNoContentOutputFormatter`, objetos nulos são formatados usando o formatador configurado.</span><span class="sxs-lookup"><span data-stu-id="52a95-209">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="52a95-210">Por exemplo, o formatador JSON simplesmente retornará uma resposta com um corpo de `null`, enquanto o formatador XML retorna um elemento XML vazio com o atributo `xsi:nil="true"` definido.</span><span class="sxs-lookup"><span data-stu-id="52a95-210">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="52a95-211">Mapeamentos de URL do formato de resposta</span><span class="sxs-lookup"><span data-stu-id="52a95-211">Response Format URL Mappings</span></span>

<span data-ttu-id="52a95-212">Os clientes podem solicitar um formato específico como parte da URL, como na cadeia de caracteres de consulta ou parte do caminho, ou usando uma extensão de arquivo de formato específico como. XML ou. JSON.</span><span class="sxs-lookup"><span data-stu-id="52a95-212">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="52a95-213">O mapeamento de caminho da solicitação deve ser especificado na rota que está usando a API.</span><span class="sxs-lookup"><span data-stu-id="52a95-213">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="52a95-214">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="52a95-214">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="52a95-215">Essa rota permitiria que o formato solicitado ser especificado como uma extensão de arquivo opcional.</span><span class="sxs-lookup"><span data-stu-id="52a95-215">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="52a95-216">O `[FormatFilter]` atributo verifica a existência do valor no formato de `RouteData` e mapeará o formato da resposta para o formatador adequado quando a resposta é criada.</span><span class="sxs-lookup"><span data-stu-id="52a95-216">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="52a95-217">Rota</span><span class="sxs-lookup"><span data-stu-id="52a95-217">Route</span></span>                      | <span data-ttu-id="52a95-218">Formatador</span><span class="sxs-lookup"><span data-stu-id="52a95-218">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="52a95-219">O formatador de saída padrão</span><span class="sxs-lookup"><span data-stu-id="52a95-219">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="52a95-220">O formatador JSON (se configurado)</span><span class="sxs-lookup"><span data-stu-id="52a95-220">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="52a95-221">O formatador XML (se configurado)</span><span class="sxs-lookup"><span data-stu-id="52a95-221">The XML formatter (if configured)</span></span>  |
