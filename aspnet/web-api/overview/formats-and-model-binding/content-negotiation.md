---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: "Negociação na API da Web ASP.NET de conteúdo | Microsoft Docs"
author: MikeWasson
description: "Descreve como o ASP.NET Web API implementa negociação de conteúdo HTTP."
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
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="content-negotiation-in-aspnet-web-api"></a>Negociação de conteúdo na API da Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este artigo descreve como o ASP.NET Web API implementa negociação de conteúdo.

A especificação de HTTP (RFC 2616) define a negociação de conteúdo como "o processo de selecionar a melhor representação para uma determinada resposta quando há várias representações." O mecanismo primário de negociação de conteúdo em HTTP são esses cabeçalhos de solicitação:

- **Aceitar:** quais tipos de mídia são aceitáveis para a resposta, como "application/json", "application/xml" ou um tipo de mídia personalizado, como &quot;application/vnd.example+xml&quot;
- **Accept-Charset:** quais conjuntos de caracteres são aceitáveis, como UTF-8 ou ISO 8859-1.
- **Codificação aceita:** quais codificações de conteúdo são aceitáveis, como gzip.
- **Aceite-idioma:** o idioma natural, como "en-us".

O servidor também pode examinar outras partes da solicitação HTTP. Por exemplo, se a solicitação contém um cabeçalho X-Requested-With, indicando que uma solicitação AJAX, o servidor pode padrão JSON se não houver nenhum cabeçalho Accept.

Neste artigo, vamos examinar como a API da Web usa cabeçalhos de aceitação e Accept-Charset. (No momento, não há nenhum suporte interno para Accept-Encoding ou Accept-Language.)

## <a name="serialization"></a>Serialização

Se um controlador Web API retorna um recurso como tipo CLR, o pipeline serializa o valor de retorno e grava-o no corpo da resposta HTTP.

Por exemplo, considere a seguinte ação do controlador:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Um cliente pode enviar esta solicitação HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

Em resposta, o servidor pode enviar:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

Neste exemplo, o cliente solicitou JSON, Javascript ou "qualquer" (\*/\*). O servidor respondido com uma representação JSON do `Product` objeto. Observe que o cabeçalho Content-Type na resposta é definido como &quot;aplicativo/json&quot;.

Um controlador também pode retornar um **HttpResponseMessage** objeto. Para especificar um objeto CLR para o corpo da resposta, chame o **CreateResponse** método de extensão:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Essa opção oferece mais controle sobre os detalhes da resposta. Você pode definir o código de status, adicionar cabeçalhos HTTP e assim por diante.

O objeto que serializa o recurso é chamado um *formatador de mídia*. Formatadores de mídia derivam o **MediaTypeFormatter** classe. API da Web fornece formatadores de mídia para XML e JSON, e você pode criar formatadores personalizados para dar suporte a outros tipos de mídia. Para obter informações sobre como escrever um formatador personalizado, consulte [formatadores de mídia](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Funciona como conteúdo de negociação

Primeiro, o pipeline obtém o **IContentNegotiator** serviço o **HttpConfiguration** objeto. Ele também obtém a lista de formatadores de mídia a partir de **HttpConfiguration.Formatters** coleção.

Em seguida, chama o pipeline **IContentNegotiatior.Negotiate**, passando:

- O tipo de objeto a ser serializado
- A coleção de formatadores de mídia.
- A solicitação HTTP

O **Negotiate** método retorna dois tipos de informações:

- O formatador a ser usado
- O tipo de mídia para a resposta

Se nenhum formatador for encontrado, o **Negotiate** método **nulo**e o erro do cliente recebe HTTP 406 (não aceitável).

O código a seguir mostra como um controlador pode invocar diretamente negociação de conteúdo:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Esse código é equivalente para o que o pipeline não automaticamente.

## <a name="default-content-negotiator"></a>Negociador de conteúdo padrão

O **DefaultContentNegotiator** classe fornece a implementação padrão de **IContentNegotiator**. Ele usa vários critérios para selecionar um formatador.

Primeiro, o formatador deve ser capaz de serializar o tipo. Isso é verificado chamando **MediaTypeFormatter.CanWriteType**.

Em seguida, o Negociador de conteúdo examina cada formatador e avalia como ele corresponde a solicitação HTTP. Para avaliar a correspondência, o Negociador de conteúdo analisa duas coisas sobre o formatador:

- O **SupportedMediaTypes** coleção que contém uma lista de tipos de mídia com suporte. O Negociador de conteúdo tenta corresponder a essa lista em relação o cabeçalho de solicitação aceitar. Observe que o cabeçalho Accept pode incluir intervalos. Por exemplo, "texto/simples" é uma correspondência de texto /\* ou \* / \*.
- O **MediaTypeMappings** coleção que contém uma lista de **MediaTypeMapping** objetos. O **MediaTypeMapping** classe fornece um modo genérico correspondem às solicitações HTTP com tipos de mídia. Por exemplo, ele pode mapear um cabeçalho HTTP personalizado para um determinado tipo de mídia.

Se houver várias corresponder, a correspondência com o wins de fator mais alta qualidade. Por exemplo:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

Neste exemplo, application/json tem um fator de qualidade implícita de 1.0, portanto, é preferencial em aplicativo/xml.

Se nenhuma correspondência for encontrada, o Negociador de conteúdo tenta corresponder o tipo de mídia do corpo da solicitação, se houver. Por exemplo, se a solicitação contém dados JSON, o Negociador de conteúdo procurará um formatador JSON.

Se ainda não houver nenhuma correspondência, o Negociador de conteúdo simplesmente escolhe o primeiro formatador que puder serializar o tipo.

## <a name="selecting-a-character-encoding"></a>Selecionar uma codificação de caracteres

Depois que um formatador for selecionado, o Negociador de conteúdo escolhe a melhor codificação de caractere, observando o **SupportedEncodings** propriedade o formatador e a correspondência com o cabeçalho Accept-Charset na solicitação (se houver).
