---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Ação resultará na API Web 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/03/2014
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: b2b5ae5e5cef19e75a184aa28ac838a31e5ef1fd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825017"
---
<a name="action-results-in-web-api-2"></a><span data-ttu-id="f0148-102">Resultados de ação na API Web 2</span><span class="sxs-lookup"><span data-stu-id="f0148-102">Action Results in Web API 2</span></span>
====================
<span data-ttu-id="f0148-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f0148-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f0148-104">Este tópico descreve como a API Web ASP.NET converte o valor de retorno de uma ação do controlador em uma mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0148-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="f0148-105">Uma ação do controlador API Web pode retornar qualquer um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="f0148-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="f0148-106">void</span><span class="sxs-lookup"><span data-stu-id="f0148-106">void</span></span>
2. <span data-ttu-id="f0148-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="f0148-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="f0148-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="f0148-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="f0148-109">Algum outro tipo</span><span class="sxs-lookup"><span data-stu-id="f0148-109">Some other type</span></span>

<span data-ttu-id="f0148-110">Dependendo de qual deles é retornada, API Web usa um mecanismo diferente para criar a resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0148-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="f0148-111">Tipo de retorno</span><span class="sxs-lookup"><span data-stu-id="f0148-111">Return type</span></span> | <span data-ttu-id="f0148-112">Como a API da Web cria a resposta</span><span class="sxs-lookup"><span data-stu-id="f0148-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="f0148-113">void</span><span class="sxs-lookup"><span data-stu-id="f0148-113">void</span></span> | <span data-ttu-id="f0148-114">Retorna vazio 204 (sem conteúdo)</span><span class="sxs-lookup"><span data-stu-id="f0148-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="f0148-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="f0148-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="f0148-116">Converta diretamente em uma mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0148-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="f0148-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="f0148-117">**IHttpActionResult**</span></span> | <span data-ttu-id="f0148-118">Chame **ExecuteAsync** para criar um **HttpResponseMessage**, em seguida, converter em uma mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0148-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="f0148-119">Outro tipo</span><span class="sxs-lookup"><span data-stu-id="f0148-119">Other type</span></span> | <span data-ttu-id="f0148-120">Gravar o valor de retorno serializado no corpo da resposta; retorne 200 (Okey).</span><span class="sxs-lookup"><span data-stu-id="f0148-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="f0148-121">O restante deste tópico descreve cada opção em mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="f0148-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="f0148-122">void</span><span class="sxs-lookup"><span data-stu-id="f0148-122">void</span></span>

<span data-ttu-id="f0148-123">Se o tipo de retorno é `void`, API da Web simplesmente retorna uma resposta HTTP vazia com o código de status 204 (sem conteúdo).</span><span class="sxs-lookup"><span data-stu-id="f0148-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="f0148-124">Controlador de exemplo:</span><span class="sxs-lookup"><span data-stu-id="f0148-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="f0148-125">Resposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="f0148-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="f0148-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="f0148-126">HttpResponseMessage</span></span>

<span data-ttu-id="f0148-127">Se a ação retorna um [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), API Web converte o valor de retorno diretamente em uma mensagem de resposta HTTP, usando as propriedades da **HttpResponseMessage** objeto para popular o resposta.</span><span class="sxs-lookup"><span data-stu-id="f0148-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="f0148-128">Essa opção fornece muito controle sobre a mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="f0148-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="f0148-129">Por exemplo, a ação de controlador a seguir define o cabeçalho Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="f0148-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="f0148-130">Resposta:</span><span class="sxs-lookup"><span data-stu-id="f0148-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="f0148-131">Se você passar um modelo de domínio para o **CreateResponse** método, a API da Web usa um [formatador de mídia](../formats-and-model-binding/media-formatters.md) para gravar o modelo serializado no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="f0148-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="f0148-132">API da Web usa o cabeçalho Accept na solicitação para escolher o formatador.</span><span class="sxs-lookup"><span data-stu-id="f0148-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="f0148-133">Para obter mais informações, consulte [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="f0148-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="f0148-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="f0148-134">IHttpActionResult</span></span>

<span data-ttu-id="f0148-135">O **IHttpActionResult** interface foi introduzida na API Web 2.</span><span class="sxs-lookup"><span data-stu-id="f0148-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="f0148-136">Essencialmente, ele define uma **HttpResponseMessage** factory.</span><span class="sxs-lookup"><span data-stu-id="f0148-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="f0148-137">Aqui estão algumas vantagens de usar o **IHttpActionResult** interface:</span><span class="sxs-lookup"><span data-stu-id="f0148-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="f0148-138">Simplifica [testes de unidade](../testing-and-debugging/unit-testing-controllers-in-web-api.md) seus controladores.</span><span class="sxs-lookup"><span data-stu-id="f0148-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="f0148-139">Move a lógica comum para a criação de respostas HTTP em classes separadas.</span><span class="sxs-lookup"><span data-stu-id="f0148-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="f0148-140">Torna a intenção da ação de controlador mais clara, ocultando os detalhes de nível baixo de construir a resposta.</span><span class="sxs-lookup"><span data-stu-id="f0148-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="f0148-141">**IHttpActionResult** contém um único método, **ExecuteAsync**, que cria de maneira assíncrona uma **HttpResponseMessage** instância.</span><span class="sxs-lookup"><span data-stu-id="f0148-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="f0148-142">Se uma ação do controlador retorna um **IHttpActionResult**, chamadas de API da Web a **ExecuteAsync** método para criar um **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="f0148-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="f0148-143">Em seguida, ele converte o **HttpResponseMessage** em uma mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0148-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="f0148-144">Aqui está uma implementação simple da **IHttpActionResult** que cria uma resposta de texto sem formatação:</span><span class="sxs-lookup"><span data-stu-id="f0148-144">Here is a simple implementaton of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="f0148-145">Ação do controlador de exemplo:</span><span class="sxs-lookup"><span data-stu-id="f0148-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="f0148-146">Resposta:</span><span class="sxs-lookup"><span data-stu-id="f0148-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="f0148-147">Mais frequentemente, você usará o **IHttpActionResult** implementações definidas na **[Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span><span class="sxs-lookup"><span data-stu-id="f0148-147">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="f0148-148">O **ApiController** classe define os métodos auxiliares que retornam esses resultados de ação interna.</span><span class="sxs-lookup"><span data-stu-id="f0148-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="f0148-149">No exemplo a seguir, se a solicitação não corresponder a uma ID de produto existente, o controlador chama [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) para criar uma resposta 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="f0148-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="f0148-150">Caso contrário, o controlador chama [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), que cria uma resposta de 200 (Okey) que contém o produto.</span><span class="sxs-lookup"><span data-stu-id="f0148-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="f0148-151">Outros tipos de retorno</span><span class="sxs-lookup"><span data-stu-id="f0148-151">Other Return Types</span></span>

<span data-ttu-id="f0148-152">Para todos os outros tipos de retorno, a API Web usa um [formatador de mídia](../formats-and-model-binding/media-formatters.md) para serializar o valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="f0148-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="f0148-153">API da Web grava o valor serializado no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="f0148-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="f0148-154">O código de status de resposta é 200 (Okey).</span><span class="sxs-lookup"><span data-stu-id="f0148-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="f0148-155">Uma desvantagem dessa abordagem é que você não pode retornar diretamente um código de erro, como 404.</span><span class="sxs-lookup"><span data-stu-id="f0148-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="f0148-156">No entanto, você pode lançar uma **HttpResponseException** para códigos de erro.</span><span class="sxs-lookup"><span data-stu-id="f0148-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="f0148-157">Para obter mais informações, consulte [tratamento de exceções em API Web ASP.NET](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="f0148-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="f0148-158">API da Web usa o cabeçalho Accept na solicitação para escolher o formatador.</span><span class="sxs-lookup"><span data-stu-id="f0148-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="f0148-159">Para obter mais informações, consulte [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="f0148-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="f0148-160">Exemplo de solicitação</span><span class="sxs-lookup"><span data-stu-id="f0148-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="f0148-161">Resposta de exemplo:</span><span class="sxs-lookup"><span data-stu-id="f0148-161">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
