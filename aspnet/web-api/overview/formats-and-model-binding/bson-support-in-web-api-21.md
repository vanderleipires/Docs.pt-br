---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Suporte a BSON no ASP.NET Web API 2.1 | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2018
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="d0d64-102">Suporte a BSON no ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="d0d64-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="d0d64-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d0d64-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d0d64-104">Web API 2.1 introduz suporte para BSON.</span><span class="sxs-lookup"><span data-stu-id="d0d64-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="d0d64-105">Este tópico mostra como usar o BSON em seu controlador de API da Web (do lado do servidor) em um aplicativo cliente .NET.</span><span class="sxs-lookup"><span data-stu-id="d0d64-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="d0d64-106">O que é BSON?</span><span class="sxs-lookup"><span data-stu-id="d0d64-106">What is BSON?</span></span>

<span data-ttu-id="d0d64-107">[BSON](http://bsonspec.org/) é um formato de serialização binária.</span><span class="sxs-lookup"><span data-stu-id="d0d64-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="d0d64-108">"BSON" significa "Binary JSON", mas BSON e JSON são serializadas de forma muito diferente.</span><span class="sxs-lookup"><span data-stu-id="d0d64-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="d0d64-109">BSON é "JSON-like", porque os objetos são representados como pares de nome-valor, semelhantes ao JSON.</span><span class="sxs-lookup"><span data-stu-id="d0d64-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="d0d64-110">Ao contrário de JSON, tipos de dados numéricos são armazenados como bytes, não cadeias de caracteres</span><span class="sxs-lookup"><span data-stu-id="d0d64-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="d0d64-111">BSON foi projetado para ser simples, fáceis de examinar e rápido para codificar/decodificar.</span><span class="sxs-lookup"><span data-stu-id="d0d64-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="d0d64-112">BSON é comparável no tamanho em JSON.</span><span class="sxs-lookup"><span data-stu-id="d0d64-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="d0d64-113">Dependendo dos dados, uma carga BSON pode ser menor ou maior do que uma carga JSON.</span><span class="sxs-lookup"><span data-stu-id="d0d64-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="d0d64-114">Para serializar os dados binários, como um arquivo de imagem, BSON é menor do que o JSON, porque os dados binários não estão codificada em base64.</span><span class="sxs-lookup"><span data-stu-id="d0d64-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="d0d64-115">Documentos BSON são fáceis de examinar como elementos são prefixados com um campo de comprimento, para que um analisador pode ignorar elementos sem decodificação-los.</span><span class="sxs-lookup"><span data-stu-id="d0d64-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="d0d64-116">Codificação e decodificação são eficientes, como tipos de dados numéricos são armazenados como números, não cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d0d64-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="d0d64-117">Clientes nativos, como aplicativos de cliente .NET, podem se beneficiar do uso BSON no lugar de formatos baseados em texto, como JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="d0d64-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="d0d64-118">Para clientes de navegador, provavelmente vai preferir ficar com JSON, pois JavaScript pode converter diretamente a carga de JSON.</span><span class="sxs-lookup"><span data-stu-id="d0d64-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="d0d64-119">Felizmente, a API da Web usa [negociação de conteúdo](content-negotiation.md), portanto, sua API pode oferecer suporte a ambos os formatos e permitir que o cliente escolher.</span><span class="sxs-lookup"><span data-stu-id="d0d64-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="d0d64-120">Habilitando BSON no servidor</span><span class="sxs-lookup"><span data-stu-id="d0d64-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="d0d64-121">Em sua configuração de API da Web, adicione o **BsonMediaTypeFormatter** à coleção de formatadores.</span><span class="sxs-lookup"><span data-stu-id="d0d64-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="d0d64-122">Agora, se o cliente solicita "application/bson", API da Web usará o formatador BSON.</span><span class="sxs-lookup"><span data-stu-id="d0d64-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="d0d64-123">Para associar BSON com outros tipos de mídia, adicioná-los à coleção SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="d0d64-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="d0d64-124">O código a seguir adiciona "application/vnd.contoso" para os tipos de mídia com suporte:</span><span class="sxs-lookup"><span data-stu-id="d0d64-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="d0d64-125">Sessão HTTP de exemplo</span><span class="sxs-lookup"><span data-stu-id="d0d64-125">Example HTTP Session</span></span>

<span data-ttu-id="d0d64-126">Neste exemplo, vamos usar a seguinte classe de modelo mais de um controlador Web API simple:</span><span class="sxs-lookup"><span data-stu-id="d0d64-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="d0d64-127">Um cliente pode enviar a solicitação HTTP a seguir:</span><span class="sxs-lookup"><span data-stu-id="d0d64-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="d0d64-128">Aqui está a resposta:</span><span class="sxs-lookup"><span data-stu-id="d0d64-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="d0d64-129">Aqui, substituir os dados binários com &quot;.&quot; caracteres.</span><span class="sxs-lookup"><span data-stu-id="d0d64-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="d0d64-130">A seguinte captura de tela de mostra Fiddler valores hexadecimais brutos.</span><span class="sxs-lookup"><span data-stu-id="d0d64-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="d0d64-131">Usando o BSON com HttpClient</span><span class="sxs-lookup"><span data-stu-id="d0d64-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="d0d64-132">Aplicativos de clientes .NET podem usar o formatador BSON com **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="d0d64-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="d0d64-133">Para obter mais informações sobre **HttpClient**, consulte [chamar uma Web API de um cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="d0d64-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="d0d64-134">O código a seguir envia uma solicitação GET que aceita BSON e, em seguida, desserializa a carga BSON na resposta.</span><span class="sxs-lookup"><span data-stu-id="d0d64-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="d0d64-135">Para solicitar BSON do servidor, defina o cabeçalho Accept para "application/bson":</span><span class="sxs-lookup"><span data-stu-id="d0d64-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="d0d64-136">Para desserializar o corpo da resposta, use o **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="d0d64-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="d0d64-137">Esse formatador não está na coleção de formatadores do padrão, você precisa especificá-la ao ler o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="d0d64-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="d0d64-138">O exemplo a seguir mostra como enviar uma solicitação POST que contém o BSON.</span><span class="sxs-lookup"><span data-stu-id="d0d64-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="d0d64-139">Grande parte do código é o mesmo que o exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="d0d64-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="d0d64-140">Mas no **PostAsync** método, especifique **BsonMediaTypeFormatter** como o formatador:</span><span class="sxs-lookup"><span data-stu-id="d0d64-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="d0d64-141">Serialização de tipos primitivos de nível superior</span><span class="sxs-lookup"><span data-stu-id="d0d64-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="d0d64-142">Cada documento BSON é uma lista de pares chave/valor. A especificação de BSON não define uma sintaxe para serializar um único valor bruto, como um inteiro ou uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d0d64-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="d0d64-143">Para contornar essa limitação, o **BsonMediaTypeFormatter** trata tipos primitivos como um caso especial.</span><span class="sxs-lookup"><span data-stu-id="d0d64-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="d0d64-144">Antes de serializar, ele converte o valor em um par chave/valor com a chave "Valor".</span><span class="sxs-lookup"><span data-stu-id="d0d64-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="d0d64-145">Por exemplo, suponha que seu controlador API retorna um inteiro:</span><span class="sxs-lookup"><span data-stu-id="d0d64-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="d0d64-146">Antes de serializar, o formatador BSON converte isso para o par chave/valor a seguir:</span><span class="sxs-lookup"><span data-stu-id="d0d64-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="d0d64-147">Ao tentar desserializar, o formatador converte os dados de volta para o valor original.</span><span class="sxs-lookup"><span data-stu-id="d0d64-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="d0d64-148">No entanto, os clientes que usam um analisador BSON diferente precisará lidar com isso, se sua API da web retorna os valores brutos.</span><span class="sxs-lookup"><span data-stu-id="d0d64-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="d0d64-149">Em geral, você deve considerar retornando dados estruturados, em vez de valores brutos.</span><span class="sxs-lookup"><span data-stu-id="d0d64-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d0d64-150">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="d0d64-150">Additional Resources</span></span>

[<span data-ttu-id="d0d64-151">Exemplo de API BSON Web</span><span class="sxs-lookup"><span data-stu-id="d0d64-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="d0d64-152">Formatadores de mídia.</span><span class="sxs-lookup"><span data-stu-id="d0d64-152">Media Formatters</span></span>](media-formatters.md)
