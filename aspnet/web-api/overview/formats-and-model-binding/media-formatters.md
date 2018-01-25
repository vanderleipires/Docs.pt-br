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
<a name="media-formatters-in-aspnet-web-api-2"></a>Formatadores de mídia no ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tutorial mostra como suporte a formatos de mídia adicionais na API da Web do ASP.NET.

## <a name="internet-media-types"></a>Tipos de mídia da Internet

Um tipo de mídia, também chamado de um tipo MIME, identifica o formato de uma parte dos dados. Tipos de mídia HTTP, descrevem o formato do corpo da mensagem. Um tipo de mídia consiste em duas cadeias de caracteres, um tipo e um subtipo. Por exemplo:

- text/html
- image/png
- application/json

Quando uma mensagem HTTP contém um corpo de entidade, o cabeçalho Content-Type especifica o formato do corpo da mensagem. Isso informa o receptor como analisar o conteúdo do corpo da mensagem.

Por exemplo, se uma resposta HTTP contém uma imagem PNG, a resposta pode ter os seguintes cabeçalhos.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Quando o cliente envia uma mensagem de solicitação, ele pode incluir um cabeçalho Accept. O cabeçalho Accept informa que deseja que o servidor que mídia tipo (s) de cliente do servidor. Por exemplo:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Esse cabeçalho informa ao servidor que o cliente deseja HTML, XHTML e XML.

O tipo de mídia determina como a API da Web serializa e desserializa o corpo da mensagem HTTP. API da Web tem suporte interno para XML, JSON, BSON e dados de formulário urlencoded, e você pode dar suporte a tipos de mídia adicionais, escrevendo um *formatador de mídia*.

Para criar um formatador de mídia, derivam de uma dessas classes:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Essa classe usa assíncrona leitura e métodos de gravação.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Essa classe é derivada de **MediaTypeFormatter** , mas usa métodos de leitura/gravação síncrono.

Derivando de **BufferedMediaTypeFormatter** é mais simples, porque não há nenhum código assíncrono, mas isso também significa que o thread de chamada pode bloquear durante e/s.

## <a name="example-creating-a-csv-media-formatter"></a>Exemplo: Criando um formatador de mídia do CSV

O exemplo a seguir mostra um formatador de tipo de mídia que pode serializar um objeto de produto para um formato de valores separados por vírgulas (CSV). Este exemplo usa o tipo de produto definido no tutorial [criando uma API da Web que dá suporte a operações de CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Esta é a definição do objeto de produto:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Para implementar um formatador CSV, definir uma classe que deriva de **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

No construtor, adicione os tipos de mídia que o formatador suporta. Neste exemplo, o formatador dá suporte a um único tipo de mídia, &quot;texto/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Substituir o **CanWriteType** método para indicar quais tipos o formatador pode serializar:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

Neste exemplo, o formatador pode serializar único `Product` objetos, bem como coleções de `Product` objetos.

Da mesma forma, substituir o **CanReadType** pode desserializar o método para indicar quais tipos de formatador. Neste exemplo, o formatador não oferece suporte a desserialização, então o método simplesmente retorna **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Por fim, substituir o **WriteToStream** método. Esse método serializa um tipo por meio da gravação em um fluxo. Se o formatador suporta desserialização, também substituir o **ReadFromStream** método.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Adicionando um formatador de mídia para o Pipeline de API da Web

Para adicionar um tipo de mídia formatador para o pipeline de API da Web, use o **formatadores** propriedade o **HttpConfiguration** objeto.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Codificações de caracteres

Opcionalmente, um formatador de mídia pode dar suporte a várias codificações de caracteres, como UTF-8 ou ISO 8859-1.

No construtor, adicione um ou mais [Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) tipos para o **SupportedEncodings** coleção. Coloque a primeira de codificação padrão.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

No **WriteToStream** e **ReadFromStream** chamar métodos, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para selecionar a codificação de caracteres preferencial. Esse método corresponde os cabeçalhos de solicitação com a lista de codificações com suporte. Use retornado **codificação** ao ler ou gravar no fluxo:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
