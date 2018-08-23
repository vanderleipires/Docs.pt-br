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
<a name="media-formatters-in-aspnet-web-api-2"></a>Formatadores de mídia na API Web ASP.NET 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tutorial mostra como dar suporte a formatos de mídia adicionais na API Web ASP.NET.

## <a name="internet-media-types"></a>Tipos de mídia da Internet

Um tipo de mídia, também chamado de um tipo MIME, identifica o formato de uma parte dos dados. Em HTTP, os tipos de mídia descrevem o formato do corpo da mensagem. Um tipo de mídia consiste em duas cadeias de caracteres, um tipo e um subtipo. Por exemplo:

- texto/html
- imagem/png
- application/json

Quando uma mensagem HTTP contém um corpo de entidade, o cabeçalho Content-Type especifica o formato do corpo da mensagem. Isso informa ao destinatário como analisar o conteúdo do corpo da mensagem.

Por exemplo, se uma resposta HTTP contém uma imagem PNG, a resposta pode ter os seguintes cabeçalhos.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Quando o cliente envia uma mensagem de solicitação, ele pode incluir um cabeçalho Accept. O cabeçalho Accept informa que deseja que o servidor no qual mídia tipo (s) o cliente do servidor. Por exemplo:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Esse cabeçalho informa ao servidor que o cliente deseja HTML, XHTML e XML.

O tipo de mídia determina como a API da Web serializa e desserializa o corpo da mensagem HTTP. API da Web tem suporte interno para XML, JSON, BSON e dados de formulário urlencoded, e você pode dar suporte a tipos de mídia adicionais escrevendo uma *formatador de mídia*.

Para criar um formatador de mídia, derive de uma dessas classes:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Esta leitura assíncrona de usos de classe e métodos de gravação.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Essa classe deriva **MediaTypeFormatter** mas utiliza métodos de leitura/gravação síncrono.

Derivando de **BufferedMediaTypeFormatter** é mais simples, porque não há nenhum código assíncrono, mas isso também significa que o thread de chamada pode bloquear durante e/s.

## <a name="example-creating-a-csv-media-formatter"></a>Exemplo: Criando um formatador de mídia do CSV

O exemplo a seguir mostra um formatador de tipo de mídia que pode serializar um objeto de produto para um formato de valores separados por vírgulas (CSV). Este exemplo usa o tipo de produto definido no tutorial [criando uma API da Web que dá suporte a operações de CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Aqui está a definição do objeto Product:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Para implementar um formatador CSV, defina uma classe que deriva de **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

No construtor, adicione os tipos de mídia que o formatador suporta. Neste exemplo, o formatador dá suporte a um único tipo de mídia, &quot;texto/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Substituir a **CanWriteType** método para indicar quais tipos o formatador pode serializar:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

Neste exemplo, o formatador pode serializar única `Product` objetos, bem como coleções de `Product` objetos.

Substituir da mesma forma, o **CanReadType** método para indicar quais tipos o formatador pode desserializar. Neste exemplo, o formatador não oferece suporte a desserialização, então o método simplesmente retorna **falsos**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Por fim, substitua os **WriteToStream** método. Esse método serializa um tipo por meio da gravação em um fluxo. Se o formatador dá suporte à desserialização, também substituir as **ReadFromStream** método.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Adicionando um formatador de mídia ao Pipeline da API da Web

Para adicionar um tipo de mídia formatador para o pipeline da API da Web, use o **formatadores** propriedade sobre o **HttpConfiguration** objeto.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Codificações de caracteres

Opcionalmente, um formatador de mídia pode dar suporte a várias codificações de caracteres, como UTF-8 ou ISO 8859-1.

No construtor, adicione um ou mais [Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) tipos para o **SupportedEncodings** coleção. Coloque a primeira de codificação padrão.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

No **WriteToStream** e **ReadFromStream** métodos, chame [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para selecionar a codificação de caractere preferencial. Esse método corresponde os cabeçalhos de solicitação em relação à lista de codificações com suporte. Usar retornado **Encoding** quando você ler ou gravar no fluxo:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
