---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "Enviar dados de formulário HTML na API da Web do ASP.NET: dados de formulário urlencoded | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="d11b1-102">Enviar dados de formulário HTML na API da Web do ASP.NET: dados de formulário urlencoded</span><span class="sxs-lookup"><span data-stu-id="d11b1-102">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>
====================
<span data-ttu-id="d11b1-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d11b1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="d11b1-104">Parte 1: Dados de formulário urlencoded</span><span class="sxs-lookup"><span data-stu-id="d11b1-104">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="d11b1-105">Este artigo mostra como enviar dados de formulário urlencoded para um controlador Web API.</span><span class="sxs-lookup"><span data-stu-id="d11b1-105">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="d11b1-106">Visão geral de formulários HTML</span><span class="sxs-lookup"><span data-stu-id="d11b1-106">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="d11b1-107">Enviar tipos complexos</span><span class="sxs-lookup"><span data-stu-id="d11b1-107">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="d11b1-108">Enviar dados de formulário por meio de AJAX</span><span class="sxs-lookup"><span data-stu-id="d11b1-108">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="d11b1-109">Enviar tipos simples</span><span class="sxs-lookup"><span data-stu-id="d11b1-109">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="d11b1-110">[Baixe o projeto concluído](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="d11b1-110">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="d11b1-111">Visão geral de formulários HTML</span><span class="sxs-lookup"><span data-stu-id="d11b1-111">Overview of HTML Forms</span></span>

<span data-ttu-id="d11b1-112">Uso de formulários HTML GET ou POST para enviar dados para o servidor.</span><span class="sxs-lookup"><span data-stu-id="d11b1-112">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="d11b1-113">O **método** atributo o **formulário** elemento fornece o método HTTP:</span><span class="sxs-lookup"><span data-stu-id="d11b1-113">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="d11b1-114">O método padrão é GET.</span><span class="sxs-lookup"><span data-stu-id="d11b1-114">The default method is GET.</span></span> <span data-ttu-id="d11b1-115">Se o formulário usa GET, o formulário de dados são codificados no URI como uma cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="d11b1-115">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="d11b1-116">Se o formulário usa POST, os dados do formulário são colocados no corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="d11b1-116">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="d11b1-117">Para dados de postagem, o **enctype** atributo especifica o formato do corpo da solicitação:</span><span class="sxs-lookup"><span data-stu-id="d11b1-117">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="d11b1-118">enctype</span><span class="sxs-lookup"><span data-stu-id="d11b1-118">enctype</span></span> | <span data-ttu-id="d11b1-119">Descrição</span><span class="sxs-lookup"><span data-stu-id="d11b1-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d11b1-120">Application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="d11b1-120">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="d11b1-121">Dados de formulário são codificados como pares de nome/valor, semelhantes a uma cadeia de caracteres de consulta URI.</span><span class="sxs-lookup"><span data-stu-id="d11b1-121">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="d11b1-122">Este é o formato padrão para POST.</span><span class="sxs-lookup"><span data-stu-id="d11b1-122">This is the default format for POST.</span></span> |
| <span data-ttu-id="d11b1-123">com diversas partes/dados de formulário</span><span class="sxs-lookup"><span data-stu-id="d11b1-123">multipart/form-data</span></span> | <span data-ttu-id="d11b1-124">Dados de formulário são codificados como uma mensagem MIME de várias partes.</span><span class="sxs-lookup"><span data-stu-id="d11b1-124">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="d11b1-125">Use este formato se você estiver carregando um arquivo para o servidor.</span><span class="sxs-lookup"><span data-stu-id="d11b1-125">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="d11b1-126">Parte 1 deste artigo examina o formato x-www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="d11b1-126">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="d11b1-127">[Parte 2](sending-html-form-data-part-2.md) descreve MIME de várias partes.</span><span class="sxs-lookup"><span data-stu-id="d11b1-127">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="d11b1-128">Enviar tipos complexos</span><span class="sxs-lookup"><span data-stu-id="d11b1-128">Sending Complex Types</span></span>

<span data-ttu-id="d11b1-129">Normalmente, você enviará um tipo complexo, composto de valores extraídos de vários controles de formulário.</span><span class="sxs-lookup"><span data-stu-id="d11b1-129">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="d11b1-130">Considere o seguinte modelo que representa uma atualização de status:</span><span class="sxs-lookup"><span data-stu-id="d11b1-130">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="d11b1-131">Aqui está um controlador Web API que aceita um `Update` objeto por meio de POST.</span><span class="sxs-lookup"><span data-stu-id="d11b1-131">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="d11b1-132">Usa esse controlador [roteamento baseado em ação](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), portanto, o modelo de rota é &quot;api / {controller} / {action} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="d11b1-132">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="d11b1-133">O cliente publicará os dados a serem &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="d11b1-133">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>


<span data-ttu-id="d11b1-134">Agora vamos escrever um formulário HTML para os usuários enviem uma atualização de status.</span><span class="sxs-lookup"><span data-stu-id="d11b1-134">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="d11b1-135">Observe que o **ação** atributo no formulário é o URI da nossa ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="d11b1-135">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="d11b1-136">Aqui está o formulário com alguns valores inseridos em:</span><span class="sxs-lookup"><span data-stu-id="d11b1-136">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="d11b1-137">Quando o usuário clicar em enviar, o navegador envia uma solicitação HTTP semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="d11b1-137">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="d11b1-138">Observe que o corpo da solicitação contém os dados do formulário, formatados como pares nome/valor.</span><span class="sxs-lookup"><span data-stu-id="d11b1-138">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="d11b1-139">API da Web converte automaticamente os pares nome/valor em uma instância de `Update` classe.</span><span class="sxs-lookup"><span data-stu-id="d11b1-139">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="d11b1-140">Enviar dados de formulário por meio de AJAX</span><span class="sxs-lookup"><span data-stu-id="d11b1-140">Sending Form Data via AJAX</span></span>

<span data-ttu-id="d11b1-141">Quando um usuário envia um formulário, o navegador navega para fora da página atual e renderiza o corpo da mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="d11b1-141">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="d11b1-142">Isso é Okey quando a resposta é uma página HTML.</span><span class="sxs-lookup"><span data-stu-id="d11b1-142">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="d11b1-143">Com uma API da web, no entanto, o corpo da resposta é geralmente vazio ou contém dados estruturados, como JSON.</span><span class="sxs-lookup"><span data-stu-id="d11b1-143">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="d11b1-144">Nesse caso, faz mais sentido para enviar a solicitação de dados do formulário usando um AJAX, para que a página pode processar a resposta.</span><span class="sxs-lookup"><span data-stu-id="d11b1-144">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="d11b1-145">O código a seguir mostra como enviar dados de formulário usando jQuery.</span><span class="sxs-lookup"><span data-stu-id="d11b1-145">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="d11b1-146">O jQuery **enviar** função substitui a ação de formulário com uma nova função.</span><span class="sxs-lookup"><span data-stu-id="d11b1-146">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="d11b1-147">Isso substitui o comportamento padrão do botão Enviar.</span><span class="sxs-lookup"><span data-stu-id="d11b1-147">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="d11b1-148">O **serializar** função serializa os dados do formulário em pares de nome/valor.</span><span class="sxs-lookup"><span data-stu-id="d11b1-148">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="d11b1-149">Para enviar os dados do formulário para o servidor, chame `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="d11b1-149">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="d11b1-150">Quando a solicitação é concluída, o `.success()` ou `.error()` manipulador exibe uma mensagem apropriada para o usuário.</span><span class="sxs-lookup"><span data-stu-id="d11b1-150">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="d11b1-151">Enviar tipos simples</span><span class="sxs-lookup"><span data-stu-id="d11b1-151">Sending Simple Types</span></span>

<span data-ttu-id="d11b1-152">Nas seções anteriores, enviamos um tipo complexo, API da Web desserializada para uma instância de uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="d11b1-152">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="d11b1-153">Você também pode enviar tipos simples, como uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d11b1-153">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="d11b1-154">Antes de enviar um tipo simples, considere o valor de disposição em um tipo complexo em vez disso.</span><span class="sxs-lookup"><span data-stu-id="d11b1-154">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="d11b1-155">Isso traz os benefícios de validação de modelo no lado do servidor e torna mais fácil estender o modelo, se necessário.</span><span class="sxs-lookup"><span data-stu-id="d11b1-155">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>


<span data-ttu-id="d11b1-156">As etapas básicas para enviar um tipo simples são os mesmos, mas há duas diferenças sutis.</span><span class="sxs-lookup"><span data-stu-id="d11b1-156">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="d11b1-157">Primeiro, no controlador, você deve decorar o nome do parâmetro com o **FromBody** atributo.</span><span class="sxs-lookup"><span data-stu-id="d11b1-157">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="d11b1-158">Por padrão, a API da Web tenta obter tipos simples do URI da solicitação.</span><span class="sxs-lookup"><span data-stu-id="d11b1-158">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="d11b1-159">O **FromBody** atributo informa a API da Web para ler o valor do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="d11b1-159">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="d11b1-160">API da Web lê o corpo da resposta no máximo uma vez, apenas um parâmetro de uma ação pode vir do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="d11b1-160">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="d11b1-161">Se você precisar obter vários valores do corpo da solicitação, defina um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="d11b1-161">If you need to get multiple values from the request body, define a complex type.</span></span>


<span data-ttu-id="d11b1-162">Segundo, o cliente precisa enviar o valor com o seguinte formato:</span><span class="sxs-lookup"><span data-stu-id="d11b1-162">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="d11b1-163">Especificamente, a parte do nome do par nome/valor deve estar vazia para um tipo simples.</span><span class="sxs-lookup"><span data-stu-id="d11b1-163">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="d11b1-164">Nem todos os navegadores dão suporte a isso para formulários HTML, mas você criar este formato no script da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="d11b1-164">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="d11b1-165">Aqui está um formulário de exemplo:</span><span class="sxs-lookup"><span data-stu-id="d11b1-165">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="d11b1-166">E aqui está o script para enviar o valor de formulário.</span><span class="sxs-lookup"><span data-stu-id="d11b1-166">And here is the script to submit the form value.</span></span> <span data-ttu-id="d11b1-167">A única diferença do script anterior é o argumento passado para o **lançar** função.</span><span class="sxs-lookup"><span data-stu-id="d11b1-167">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="d11b1-168">Você pode usar a mesma abordagem para enviar uma matriz de tipos simples:</span><span class="sxs-lookup"><span data-stu-id="d11b1-168">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="d11b1-169">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="d11b1-169">Additional Resources</span></span>

[<span data-ttu-id="d11b1-170">Parte 2: Carregamento de arquivo e com diversas partes MIME</span><span class="sxs-lookup"><span data-stu-id="d11b1-170">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
