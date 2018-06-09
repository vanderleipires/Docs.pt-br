---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Negociação na API da Web ASP.NET de conteúdo | Microsoft Docs
author: MikeWasson
description: Descreve como o ASP.NET Web API implementa negociação de conteúdo HTTP.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26507015"
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="81ea5-103">Negociação de conteúdo na API da Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="81ea5-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="81ea5-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="81ea5-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="81ea5-105">Este artigo descreve como o ASP.NET Web API implementa negociação de conteúdo.</span><span class="sxs-lookup"><span data-stu-id="81ea5-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="81ea5-106">A especificação de HTTP (RFC 2616) define a negociação de conteúdo como "o processo de selecionar a melhor representação para uma determinada resposta quando há várias representações."</span><span class="sxs-lookup"><span data-stu-id="81ea5-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="81ea5-107">O mecanismo primário de negociação de conteúdo em HTTP são esses cabeçalhos de solicitação:</span><span class="sxs-lookup"><span data-stu-id="81ea5-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="81ea5-108">**Aceitar:** quais tipos de mídia são aceitáveis para a resposta, como "application/json", "application/xml" ou um tipo de mídia personalizado, como &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="81ea5-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="81ea5-109">**Accept-Charset:** quais conjuntos de caracteres são aceitáveis, como UTF-8 ou ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="81ea5-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="81ea5-110">**Codificação aceita:** quais codificações de conteúdo são aceitáveis, como gzip.</span><span class="sxs-lookup"><span data-stu-id="81ea5-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="81ea5-111">**Aceite-idioma:** o idioma natural, como "en-us".</span><span class="sxs-lookup"><span data-stu-id="81ea5-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="81ea5-112">O servidor também pode examinar outras partes da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="81ea5-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="81ea5-113">Por exemplo, se a solicitação contém um cabeçalho X-Requested-With, indicando que uma solicitação AJAX, o servidor pode padrão JSON se não houver nenhum cabeçalho Accept.</span><span class="sxs-lookup"><span data-stu-id="81ea5-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="81ea5-114">Neste artigo, vamos examinar como a API da Web usa cabeçalhos de aceitação e Accept-Charset.</span><span class="sxs-lookup"><span data-stu-id="81ea5-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="81ea5-115">(No momento, não há nenhum suporte interno para Accept-Encoding ou Accept-Language.)</span><span class="sxs-lookup"><span data-stu-id="81ea5-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="81ea5-116">Serialização</span><span class="sxs-lookup"><span data-stu-id="81ea5-116">Serialization</span></span>

<span data-ttu-id="81ea5-117">Se um controlador Web API retorna um recurso como tipo CLR, o pipeline serializa o valor de retorno e grava-o no corpo da resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="81ea5-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="81ea5-118">Por exemplo, considere a seguinte ação do controlador:</span><span class="sxs-lookup"><span data-stu-id="81ea5-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="81ea5-119">Um cliente pode enviar esta solicitação HTTP:</span><span class="sxs-lookup"><span data-stu-id="81ea5-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="81ea5-120">Em resposta, o servidor pode enviar:</span><span class="sxs-lookup"><span data-stu-id="81ea5-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="81ea5-121">Neste exemplo, o cliente solicitou JSON, Javascript ou "qualquer" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="81ea5-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="81ea5-122">O servidor respondido com uma representação JSON do `Product` objeto.</span><span class="sxs-lookup"><span data-stu-id="81ea5-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="81ea5-123">Observe que o cabeçalho Content-Type na resposta é definido como &quot;aplicativo/json&quot;.</span><span class="sxs-lookup"><span data-stu-id="81ea5-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="81ea5-124">Um controlador também pode retornar um **HttpResponseMessage** objeto.</span><span class="sxs-lookup"><span data-stu-id="81ea5-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="81ea5-125">Para especificar um objeto CLR para o corpo da resposta, chame o **CreateResponse** método de extensão:</span><span class="sxs-lookup"><span data-stu-id="81ea5-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="81ea5-126">Essa opção oferece mais controle sobre os detalhes da resposta.</span><span class="sxs-lookup"><span data-stu-id="81ea5-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="81ea5-127">Você pode definir o código de status, adicionar cabeçalhos HTTP e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="81ea5-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="81ea5-128">O objeto que serializa o recurso é chamado um *formatador de mídia*.</span><span class="sxs-lookup"><span data-stu-id="81ea5-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="81ea5-129">Formatadores de mídia derivam o **MediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="81ea5-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="81ea5-130">API da Web fornece formatadores de mídia para XML e JSON, e você pode criar formatadores personalizados para dar suporte a outros tipos de mídia.</span><span class="sxs-lookup"><span data-stu-id="81ea5-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="81ea5-131">Para obter informações sobre como escrever um formatador personalizado, consulte [formatadores de mídia](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="81ea5-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="81ea5-132">Funciona como conteúdo de negociação</span><span class="sxs-lookup"><span data-stu-id="81ea5-132">How Content Negotiation Works</span></span>

<span data-ttu-id="81ea5-133">Primeiro, o pipeline obtém o **IContentNegotiator** serviço o **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="81ea5-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="81ea5-134">Ele também obtém a lista de formatadores de mídia a partir de **HttpConfiguration.Formatters** coleção.</span><span class="sxs-lookup"><span data-stu-id="81ea5-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="81ea5-135">Em seguida, chama o pipeline **IContentNegotiatior.Negotiate**, passando:</span><span class="sxs-lookup"><span data-stu-id="81ea5-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="81ea5-136">O tipo de objeto a ser serializado</span><span class="sxs-lookup"><span data-stu-id="81ea5-136">The type of object to serialize</span></span>
- <span data-ttu-id="81ea5-137">A coleção de formatadores de mídia.</span><span class="sxs-lookup"><span data-stu-id="81ea5-137">The collection of media formatters</span></span>
- <span data-ttu-id="81ea5-138">A solicitação HTTP</span><span class="sxs-lookup"><span data-stu-id="81ea5-138">The HTTP request</span></span>

<span data-ttu-id="81ea5-139">O **Negotiate** método retorna dois tipos de informações:</span><span class="sxs-lookup"><span data-stu-id="81ea5-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="81ea5-140">O formatador a ser usado</span><span class="sxs-lookup"><span data-stu-id="81ea5-140">Which formatter to use</span></span>
- <span data-ttu-id="81ea5-141">O tipo de mídia para a resposta</span><span class="sxs-lookup"><span data-stu-id="81ea5-141">The media type for the response</span></span>

<span data-ttu-id="81ea5-142">Se nenhum formatador for encontrado, o **Negotiate** método **nulo**e o erro do cliente recebe HTTP 406 (não aceitável).</span><span class="sxs-lookup"><span data-stu-id="81ea5-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="81ea5-143">O código a seguir mostra como um controlador pode invocar diretamente negociação de conteúdo:</span><span class="sxs-lookup"><span data-stu-id="81ea5-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="81ea5-144">Esse código é equivalente para o que o pipeline não automaticamente.</span><span class="sxs-lookup"><span data-stu-id="81ea5-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="81ea5-145">Negociador de conteúdo padrão</span><span class="sxs-lookup"><span data-stu-id="81ea5-145">Default Content Negotiator</span></span>

<span data-ttu-id="81ea5-146">O **DefaultContentNegotiator** classe fornece a implementação padrão de **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="81ea5-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="81ea5-147">Ele usa vários critérios para selecionar um formatador.</span><span class="sxs-lookup"><span data-stu-id="81ea5-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="81ea5-148">Primeiro, o formatador deve ser capaz de serializar o tipo.</span><span class="sxs-lookup"><span data-stu-id="81ea5-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="81ea5-149">Isso é verificado chamando **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="81ea5-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="81ea5-150">Em seguida, o Negociador de conteúdo examina cada formatador e avalia como ele corresponde a solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="81ea5-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="81ea5-151">Para avaliar a correspondência, o Negociador de conteúdo analisa duas coisas sobre o formatador:</span><span class="sxs-lookup"><span data-stu-id="81ea5-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="81ea5-152">O **SupportedMediaTypes** coleção que contém uma lista de tipos de mídia com suporte.</span><span class="sxs-lookup"><span data-stu-id="81ea5-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="81ea5-153">O Negociador de conteúdo tenta corresponder a essa lista em relação o cabeçalho de solicitação aceitar.</span><span class="sxs-lookup"><span data-stu-id="81ea5-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="81ea5-154">Observe que o cabeçalho Accept pode incluir intervalos.</span><span class="sxs-lookup"><span data-stu-id="81ea5-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="81ea5-155">Por exemplo, "texto/simples" é uma correspondência de texto /\* ou \* / \*.</span><span class="sxs-lookup"><span data-stu-id="81ea5-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="81ea5-156">O **MediaTypeMappings** coleção que contém uma lista de **MediaTypeMapping** objetos.</span><span class="sxs-lookup"><span data-stu-id="81ea5-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="81ea5-157">O **MediaTypeMapping** classe fornece um modo genérico correspondem às solicitações HTTP com tipos de mídia.</span><span class="sxs-lookup"><span data-stu-id="81ea5-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="81ea5-158">Por exemplo, ele pode mapear um cabeçalho HTTP personalizado para um determinado tipo de mídia.</span><span class="sxs-lookup"><span data-stu-id="81ea5-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="81ea5-159">Se houver várias corresponder, a correspondência com o wins de fator mais alta qualidade.</span><span class="sxs-lookup"><span data-stu-id="81ea5-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="81ea5-160">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="81ea5-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="81ea5-161">Neste exemplo, application/json tem um fator de qualidade implícita de 1.0, portanto, é preferencial em aplicativo/xml.</span><span class="sxs-lookup"><span data-stu-id="81ea5-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="81ea5-162">Se nenhuma correspondência for encontrada, o Negociador de conteúdo tenta corresponder o tipo de mídia do corpo da solicitação, se houver.</span><span class="sxs-lookup"><span data-stu-id="81ea5-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="81ea5-163">Por exemplo, se a solicitação contém dados JSON, o Negociador de conteúdo procurará um formatador JSON.</span><span class="sxs-lookup"><span data-stu-id="81ea5-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="81ea5-164">Se ainda não houver nenhuma correspondência, o Negociador de conteúdo simplesmente escolhe o primeiro formatador que puder serializar o tipo.</span><span class="sxs-lookup"><span data-stu-id="81ea5-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="81ea5-165">Selecionar uma codificação de caracteres</span><span class="sxs-lookup"><span data-stu-id="81ea5-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="81ea5-166">Depois que um formatador for selecionado, o Negociador de conteúdo escolhe a melhor codificação de caractere, observando o **SupportedEncodings** propriedade o formatador e a correspondência com o cabeçalho Accept-Charset na solicitação (se houver).</span><span class="sxs-lookup"><span data-stu-id="81ea5-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
