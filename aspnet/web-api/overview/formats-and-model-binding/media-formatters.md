---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formatadores de mídia na API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 7b7ba2fb3f1bba0447e700c84a017266cba305e6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831657"
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="686d9-102">Formatadores de mídia na API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="686d9-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="686d9-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="686d9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="686d9-104">Este tutorial mostra como dar suporte a formatos de mídia adicionais na API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="686d9-104">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="686d9-105">Tipos de mídia da Internet</span><span class="sxs-lookup"><span data-stu-id="686d9-105">Internet Media Types</span></span>

<span data-ttu-id="686d9-106">Um tipo de mídia, também chamado de um tipo MIME, identifica o formato de uma parte dos dados.</span><span class="sxs-lookup"><span data-stu-id="686d9-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="686d9-107">Em HTTP, os tipos de mídia descrevem o formato do corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="686d9-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="686d9-108">Um tipo de mídia consiste em duas cadeias de caracteres, um tipo e um subtipo.</span><span class="sxs-lookup"><span data-stu-id="686d9-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="686d9-109">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="686d9-109">For example:</span></span>

- <span data-ttu-id="686d9-110">texto/html</span><span class="sxs-lookup"><span data-stu-id="686d9-110">text/html</span></span>
- <span data-ttu-id="686d9-111">imagem/png</span><span class="sxs-lookup"><span data-stu-id="686d9-111">image/png</span></span>
- <span data-ttu-id="686d9-112">application/json</span><span class="sxs-lookup"><span data-stu-id="686d9-112">application/json</span></span>

<span data-ttu-id="686d9-113">Quando uma mensagem HTTP contém um corpo de entidade, o cabeçalho Content-Type especifica o formato do corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="686d9-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="686d9-114">Isso informa ao destinatário como analisar o conteúdo do corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="686d9-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="686d9-115">Por exemplo, se uma resposta HTTP contém uma imagem PNG, a resposta pode ter os seguintes cabeçalhos.</span><span class="sxs-lookup"><span data-stu-id="686d9-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="686d9-116">Quando o cliente envia uma mensagem de solicitação, ele pode incluir um cabeçalho Accept.</span><span class="sxs-lookup"><span data-stu-id="686d9-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="686d9-117">O cabeçalho Accept informa que deseja que o servidor no qual mídia tipo (s) o cliente do servidor.</span><span class="sxs-lookup"><span data-stu-id="686d9-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="686d9-118">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="686d9-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="686d9-119">Esse cabeçalho informa ao servidor que o cliente deseja HTML, XHTML e XML.</span><span class="sxs-lookup"><span data-stu-id="686d9-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="686d9-120">O tipo de mídia determina como a API da Web serializa e desserializa o corpo da mensagem HTTP.</span><span class="sxs-lookup"><span data-stu-id="686d9-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="686d9-121">API da Web tem suporte interno para XML, JSON, BSON e dados de formulário urlencoded, e você pode dar suporte a tipos de mídia adicionais escrevendo uma *formatador de mídia*.</span><span class="sxs-lookup"><span data-stu-id="686d9-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="686d9-122">Para criar um formatador de mídia, derive de uma dessas classes:</span><span class="sxs-lookup"><span data-stu-id="686d9-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="686d9-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="686d9-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="686d9-124">Esta leitura assíncrona de usos de classe e métodos de gravação.</span><span class="sxs-lookup"><span data-stu-id="686d9-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="686d9-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="686d9-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="686d9-126">Essa classe deriva **MediaTypeFormatter** mas utiliza métodos de leitura/gravação síncrono.</span><span class="sxs-lookup"><span data-stu-id="686d9-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="686d9-127">Derivando de **BufferedMediaTypeFormatter** é mais simples, porque não há nenhum código assíncrono, mas isso também significa que o thread de chamada pode bloquear durante e/s.</span><span class="sxs-lookup"><span data-stu-id="686d9-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="686d9-128">Exemplo: Criando um formatador de mídia do CSV</span><span class="sxs-lookup"><span data-stu-id="686d9-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="686d9-129">O exemplo a seguir mostra um formatador de tipo de mídia que pode serializar um objeto de produto para um formato de valores separados por vírgulas (CSV).</span><span class="sxs-lookup"><span data-stu-id="686d9-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="686d9-130">Este exemplo usa o tipo de produto definido no tutorial [criando uma API da Web que dá suporte a operações de CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="686d9-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="686d9-131">Aqui está a definição do objeto Product:</span><span class="sxs-lookup"><span data-stu-id="686d9-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="686d9-132">Para implementar um formatador CSV, defina uma classe que deriva de **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="686d9-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="686d9-133">No construtor, adicione os tipos de mídia que o formatador suporta.</span><span class="sxs-lookup"><span data-stu-id="686d9-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="686d9-134">Neste exemplo, o formatador dá suporte a um único tipo de mídia, &quot;texto/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="686d9-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="686d9-135">Substituir a **CanWriteType** método para indicar quais tipos o formatador pode serializar:</span><span class="sxs-lookup"><span data-stu-id="686d9-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="686d9-136">Neste exemplo, o formatador pode serializar única `Product` objetos, bem como coleções de `Product` objetos.</span><span class="sxs-lookup"><span data-stu-id="686d9-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="686d9-137">Substituir da mesma forma, o **CanReadType** método para indicar quais tipos o formatador pode desserializar.</span><span class="sxs-lookup"><span data-stu-id="686d9-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="686d9-138">Neste exemplo, o formatador não oferece suporte a desserialização, então o método simplesmente retorna **falsos**.</span><span class="sxs-lookup"><span data-stu-id="686d9-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="686d9-139">Por fim, substitua os **WriteToStream** método.</span><span class="sxs-lookup"><span data-stu-id="686d9-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="686d9-140">Esse método serializa um tipo por meio da gravação em um fluxo.</span><span class="sxs-lookup"><span data-stu-id="686d9-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="686d9-141">Se o formatador dá suporte à desserialização, também substituir as **ReadFromStream** método.</span><span class="sxs-lookup"><span data-stu-id="686d9-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="686d9-142">Adicionando um formatador de mídia ao Pipeline da API da Web</span><span class="sxs-lookup"><span data-stu-id="686d9-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="686d9-143">Para adicionar um tipo de mídia formatador para o pipeline da API da Web, use o **formatadores** propriedade sobre o **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="686d9-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="686d9-144">Codificações de caracteres</span><span class="sxs-lookup"><span data-stu-id="686d9-144">Character Encodings</span></span>

<span data-ttu-id="686d9-145">Opcionalmente, um formatador de mídia pode dar suporte a várias codificações de caracteres, como UTF-8 ou ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="686d9-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="686d9-146">No construtor, adicione um ou mais [Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) tipos para o **SupportedEncodings** coleção.</span><span class="sxs-lookup"><span data-stu-id="686d9-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="686d9-147">Coloque a primeira de codificação padrão.</span><span class="sxs-lookup"><span data-stu-id="686d9-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="686d9-148">No **WriteToStream** e **ReadFromStream** métodos, chame [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para selecionar a codificação de caractere preferencial.</span><span class="sxs-lookup"><span data-stu-id="686d9-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="686d9-149">Esse método corresponde os cabeçalhos de solicitação em relação à lista de codificações com suporte.</span><span class="sxs-lookup"><span data-stu-id="686d9-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="686d9-150">Usar retornado **Encoding** quando você ler ou gravar no fluxo:</span><span class="sxs-lookup"><span data-stu-id="686d9-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
