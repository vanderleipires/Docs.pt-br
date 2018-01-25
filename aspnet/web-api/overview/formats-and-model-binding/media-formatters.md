---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: "Formatadores de mídia no ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 9103574597df126a22e21a2f51815f608e46f47f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="c2c6c-102">Formatadores de mídia no ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c2c6c-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="c2c6c-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c2c6c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c2c6c-104">Este tutorial mostra como suporte a formatos de mídia adicionais na API da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-104">This tutorial shows how support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="c2c6c-105">Tipos de mídia da Internet</span><span class="sxs-lookup"><span data-stu-id="c2c6c-105">Internet Media Types</span></span>

<span data-ttu-id="c2c6c-106">Um tipo de mídia, também chamado de um tipo MIME, identifica o formato de uma parte dos dados.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="c2c6c-107">Tipos de mídia HTTP, descrevem o formato do corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="c2c6c-108">Um tipo de mídia consiste em duas cadeias de caracteres, um tipo e um subtipo.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="c2c6c-109">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c2c6c-109">For example:</span></span>

- <span data-ttu-id="c2c6c-110">text/html</span><span class="sxs-lookup"><span data-stu-id="c2c6c-110">text/html</span></span>
- <span data-ttu-id="c2c6c-111">image/png</span><span class="sxs-lookup"><span data-stu-id="c2c6c-111">image/png</span></span>
- <span data-ttu-id="c2c6c-112">application/json</span><span class="sxs-lookup"><span data-stu-id="c2c6c-112">application/json</span></span>

<span data-ttu-id="c2c6c-113">Quando uma mensagem HTTP contém um corpo de entidade, o cabeçalho Content-Type especifica o formato do corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="c2c6c-114">Isso informa o receptor como analisar o conteúdo do corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="c2c6c-115">Por exemplo, se uma resposta HTTP contém uma imagem PNG, a resposta pode ter os seguintes cabeçalhos.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="c2c6c-116">Quando o cliente envia uma mensagem de solicitação, ele pode incluir um cabeçalho Accept.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="c2c6c-117">O cabeçalho Accept informa que deseja que o servidor que mídia tipo (s) de cliente do servidor.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="c2c6c-118">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c2c6c-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="c2c6c-119">Esse cabeçalho informa ao servidor que o cliente deseja HTML, XHTML e XML.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="c2c6c-120">O tipo de mídia determina como a API da Web serializa e desserializa o corpo da mensagem HTTP.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="c2c6c-121">API da Web tem suporte interno para XML, JSON, BSON e dados de formulário urlencoded, e você pode dar suporte a tipos de mídia adicionais, escrevendo um *formatador de mídia*.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="c2c6c-122">Para criar um formatador de mídia, derivam de uma dessas classes:</span><span class="sxs-lookup"><span data-stu-id="c2c6c-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="c2c6c-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="c2c6c-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="c2c6c-124">Essa classe usa assíncrona leitura e métodos de gravação.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="c2c6c-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="c2c6c-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="c2c6c-126">Essa classe é derivada de **MediaTypeFormatter** , mas usa métodos de leitura/gravação síncrono.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="c2c6c-127">Derivando de **BufferedMediaTypeFormatter** é mais simples, porque não há nenhum código assíncrono, mas isso também significa que o thread de chamada pode bloquear durante e/s.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="c2c6c-128">Exemplo: Criando um formatador de mídia do CSV</span><span class="sxs-lookup"><span data-stu-id="c2c6c-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="c2c6c-129">O exemplo a seguir mostra um formatador de tipo de mídia que pode serializar um objeto de produto para um formato de valores separados por vírgulas (CSV).</span><span class="sxs-lookup"><span data-stu-id="c2c6c-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="c2c6c-130">Este exemplo usa o tipo de produto definido no tutorial [criando uma API da Web que dá suporte a operações de CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="c2c6c-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="c2c6c-131">Esta é a definição do objeto de produto:</span><span class="sxs-lookup"><span data-stu-id="c2c6c-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="c2c6c-132">Para implementar um formatador CSV, definir uma classe que deriva de **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="c2c6c-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="c2c6c-133">No construtor, adicione os tipos de mídia que o formatador suporta.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="c2c6c-134">Neste exemplo, o formatador dá suporte a um único tipo de mídia, &quot;texto/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="c2c6c-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="c2c6c-135">Substituir o **CanWriteType** método para indicar quais tipos o formatador pode serializar:</span><span class="sxs-lookup"><span data-stu-id="c2c6c-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="c2c6c-136">Neste exemplo, o formatador pode serializar único `Product` objetos, bem como coleções de `Product` objetos.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="c2c6c-137">Da mesma forma, substituir o **CanReadType** pode desserializar o método para indicar quais tipos de formatador.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="c2c6c-138">Neste exemplo, o formatador não oferece suporte a desserialização, então o método simplesmente retorna **false**.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="c2c6c-139">Por fim, substituir o **WriteToStream** método.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="c2c6c-140">Esse método serializa um tipo por meio da gravação em um fluxo.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="c2c6c-141">Se o formatador suporta desserialização, também substituir o **ReadFromStream** método.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="c2c6c-142">Adicionando um formatador de mídia para o Pipeline de API da Web</span><span class="sxs-lookup"><span data-stu-id="c2c6c-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="c2c6c-143">Para adicionar um tipo de mídia formatador para o pipeline de API da Web, use o **formatadores** propriedade o **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="c2c6c-144">Codificações de caracteres</span><span class="sxs-lookup"><span data-stu-id="c2c6c-144">Character Encodings</span></span>

<span data-ttu-id="c2c6c-145">Opcionalmente, um formatador de mídia pode dar suporte a várias codificações de caracteres, como UTF-8 ou ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="c2c6c-146">No construtor, adicione um ou mais [Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) tipos para o **SupportedEncodings** coleção.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="c2c6c-147">Coloque a primeira de codificação padrão.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="c2c6c-148">No **WriteToStream** e **ReadFromStream** chamar métodos, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para selecionar a codificação de caracteres preferencial.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="c2c6c-149">Esse método corresponde os cabeçalhos de solicitação com a lista de codificações com suporte.</span><span class="sxs-lookup"><span data-stu-id="c2c6c-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="c2c6c-150">Use retornado **codificação** ao ler ou gravar no fluxo:</span><span class="sxs-lookup"><span data-stu-id="c2c6c-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
