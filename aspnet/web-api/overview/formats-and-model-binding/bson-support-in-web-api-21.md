---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Suporte a BSON na API Web ASP.NET 2.1 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 709fb0266c0725176358a1bd0d08b3e07fa6e2a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824412"
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="66e0d-102">Suporte a BSON na API Web ASP.NET 2.1</span><span class="sxs-lookup"><span data-stu-id="66e0d-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="66e0d-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="66e0d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="66e0d-104">API Web 2.1 apresenta suporte para BSON.</span><span class="sxs-lookup"><span data-stu-id="66e0d-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="66e0d-105">Este tópico mostra como usar o BSON em seu controlador de API da Web (lado do servidor) em um aplicativo cliente .NET.</span><span class="sxs-lookup"><span data-stu-id="66e0d-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="66e0d-106">O que é o BSON?</span><span class="sxs-lookup"><span data-stu-id="66e0d-106">What is BSON?</span></span>

<span data-ttu-id="66e0d-107">[BSON](http://bsonspec.org/) é um formato de serialização binária.</span><span class="sxs-lookup"><span data-stu-id="66e0d-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="66e0d-108">"BSON" significa "Binary JSON", mas BSON e JSON são serializados de forma muito diferente.</span><span class="sxs-lookup"><span data-stu-id="66e0d-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="66e0d-109">BSON é "Como JSON", porque os objetos são representados como pares nome-valor, semelhantes ao JSON.</span><span class="sxs-lookup"><span data-stu-id="66e0d-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="66e0d-110">Ao contrário de JSON, os tipos de dados numéricos são armazenados como bytes, não cadeias de caracteres</span><span class="sxs-lookup"><span data-stu-id="66e0d-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="66e0d-111">BSON foi projetado para ser leve, fácil de examinar e rápido para codificação/decodificação.</span><span class="sxs-lookup"><span data-stu-id="66e0d-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="66e0d-112">BSON é comparável em tamanho ao JSON.</span><span class="sxs-lookup"><span data-stu-id="66e0d-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="66e0d-113">Dependendo dos dados, uma carga BSON pode ser menor ou maior que uma carga JSON.</span><span class="sxs-lookup"><span data-stu-id="66e0d-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="66e0d-114">Para serializar os dados binários, como um arquivo de imagem BSON é menor do que o JSON, porque os dados binários não são codificada em base64.</span><span class="sxs-lookup"><span data-stu-id="66e0d-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="66e0d-115">Documentos BSON são fáceis de examinar como os elementos são prefixados com um campo de comprimento, portanto, um analisador pode ignorar elementos sem decodificação-los.</span><span class="sxs-lookup"><span data-stu-id="66e0d-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="66e0d-116">Codificação e decodificação são eficientes, como tipos de dados numéricos são armazenados como números, não cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="66e0d-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="66e0d-117">Clientes nativos, como aplicativos de cliente .NET, podem se beneficiar do uso BSON no lugar de formatos baseados em texto, como JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="66e0d-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="66e0d-118">Para clientes de navegador, você provavelmente desejará fique com JSON, porque o JavaScript pode converter diretamente o conteúdo JSON.</span><span class="sxs-lookup"><span data-stu-id="66e0d-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="66e0d-119">Felizmente, a API Web usa [negociação de conteúdo](content-negotiation.md), portanto, sua API pode oferecer suporte a ambos os formatos e permitir que o cliente escolher.</span><span class="sxs-lookup"><span data-stu-id="66e0d-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="66e0d-120">Habilitando o BSON no servidor</span><span class="sxs-lookup"><span data-stu-id="66e0d-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="66e0d-121">Em sua configuração de API da Web, adicione a **BsonMediaTypeFormatter** à coleção de formatadores.</span><span class="sxs-lookup"><span data-stu-id="66e0d-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="66e0d-122">Agora, se o cliente solicita "application/bson", API da Web usará o formatador BSON.</span><span class="sxs-lookup"><span data-stu-id="66e0d-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="66e0d-123">Para associar o BSON com outros tipos de mídia, adicioná-los à coleção SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="66e0d-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="66e0d-124">O código a seguir adiciona "application/vnd.contoso" para os tipos de mídia com suporte:</span><span class="sxs-lookup"><span data-stu-id="66e0d-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="66e0d-125">Sessão HTTP de exemplo</span><span class="sxs-lookup"><span data-stu-id="66e0d-125">Example HTTP Session</span></span>

<span data-ttu-id="66e0d-126">Neste exemplo, vamos usar a seguinte classe de modelo mais de um controlador de API Web simples:</span><span class="sxs-lookup"><span data-stu-id="66e0d-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="66e0d-127">Um cliente pode enviar a solicitação HTTP a seguir:</span><span class="sxs-lookup"><span data-stu-id="66e0d-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="66e0d-128">Aqui está a resposta:</span><span class="sxs-lookup"><span data-stu-id="66e0d-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="66e0d-129">Aqui posso substituir os dados binários com &quot;.&quot; caracteres.</span><span class="sxs-lookup"><span data-stu-id="66e0d-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="66e0d-130">A seguinte captura de tela do mostra Fiddler os valores de hexadecimais brutos.</span><span class="sxs-lookup"><span data-stu-id="66e0d-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="66e0d-131">Usando o BSON com HttpClient</span><span class="sxs-lookup"><span data-stu-id="66e0d-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="66e0d-132">Aplicativos de clientes .NET podem usar com o formatador de BSON **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="66e0d-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="66e0d-133">Para obter mais informações sobre **HttpClient**, consulte [chamar um Web API de um cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="66e0d-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="66e0d-134">O código a seguir envia uma solicitação GET que aceita BSON e, em seguida, desserializa o conteúdo BSON na resposta.</span><span class="sxs-lookup"><span data-stu-id="66e0d-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="66e0d-135">Para solicitar o BSON do servidor, defina o cabeçalho Accept para "application/bson":</span><span class="sxs-lookup"><span data-stu-id="66e0d-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="66e0d-136">Para desserializar o corpo da resposta, use o **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="66e0d-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="66e0d-137">Esse formatador não está na coleção de formatadores do padrão, portanto, você precisa especificá-lo quando você ler o corpo da resposta:</span><span class="sxs-lookup"><span data-stu-id="66e0d-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="66e0d-138">O exemplo a seguir mostra como enviar uma solicitação POST que contém o BSON.</span><span class="sxs-lookup"><span data-stu-id="66e0d-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="66e0d-139">Grande parte desse código é o mesmo do exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="66e0d-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="66e0d-140">Mas no **PostAsync** método, especifique **BsonMediaTypeFormatter** como o formatador:</span><span class="sxs-lookup"><span data-stu-id="66e0d-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="66e0d-141">Serialização de tipos primitivos de nível superior</span><span class="sxs-lookup"><span data-stu-id="66e0d-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="66e0d-142">Todos os documentos BSON é uma lista de pares chave/valor. A especificação de BSON não define uma sintaxe para serializar um único valor bruto, como um inteiro ou cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="66e0d-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="66e0d-143">Para contornar essa limitação, o **BsonMediaTypeFormatter** trata tipos primitivos como um caso especial.</span><span class="sxs-lookup"><span data-stu-id="66e0d-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="66e0d-144">Antes de serializar, ele converte o valor em um par chave/valor com a chave "Value".</span><span class="sxs-lookup"><span data-stu-id="66e0d-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="66e0d-145">Por exemplo, suponha que seu controlador de API retorna um inteiro:</span><span class="sxs-lookup"><span data-stu-id="66e0d-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="66e0d-146">Antes de serialização, o formatador BSON converte esse valor para o par chave/valor a seguir:</span><span class="sxs-lookup"><span data-stu-id="66e0d-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="66e0d-147">Quando você desserializa, o formatador converte os dados para o valor original.</span><span class="sxs-lookup"><span data-stu-id="66e0d-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="66e0d-148">No entanto, os clientes que usam um analisador BSON diferente precisará lidar com isso, se sua API web retornar valores brutos.</span><span class="sxs-lookup"><span data-stu-id="66e0d-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="66e0d-149">Em geral, você deve considerar retornando dados estruturados, em vez de valores brutos.</span><span class="sxs-lookup"><span data-stu-id="66e0d-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="66e0d-150">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="66e0d-150">Additional Resources</span></span>

[<span data-ttu-id="66e0d-151">Exemplo de API BSON Web</span><span class="sxs-lookup"><span data-stu-id="66e0d-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="66e0d-152">Formatadores de mídia</span><span class="sxs-lookup"><span data-stu-id="66e0d-152">Media Formatters</span></span>](media-formatters.md)
