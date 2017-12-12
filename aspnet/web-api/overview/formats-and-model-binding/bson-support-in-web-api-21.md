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
ms.openlocfilehash: 08ef1564b2f8f11294c3bb1ec0ff9a3d063895b6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="bson-support-in-aspnet-web-api-21"></a>Suporte a BSON no ASP.NET Web API 2.1
====================
por [Mike Wasson](https://github.com/MikeWasson)

Web API 2.1 introduz suporte para BSON. Este tópico mostra como usar o BSON em seu controlador de API da Web (do lado do servidor) em um aplicativo cliente .NET.

## <a name="what-is-bson"></a>O que é BSON?

[BSON](http://bsonspec.org/) é um formato de serialização binária. "BSON" significa "Binary JSON", mas BSON e JSON são serializadas de forma muito diferente. BSON é "JSON-like", porque os objetos são representados como pares de nome-valor, semelhantes ao JSON. Ao contrário de JSON, tipos de dados numéricos são armazenados como bytes, não cadeias de caracteres

BSON foi projetado para ser simples, fáceis de examinar e rápido para codificar/decodificar.

- BSON é comparável no tamanho em JSON. Dependendo dos dados, uma carga BSON pode ser menor ou maior do que uma carga JSON. Para serializar os dados binários, como um arquivo de imagem, BSON é menor do que o JSON, porque os dados binários não não está codificada em base64.
- Documentos BSON são fáceis de examinar como elementos são prefixados com um campo de comprimento, para que um analisador pode ignorar elementos sem decodificação-los.
- Codificação e decodificação são eficientes, como tipos de dados numéricos são armazenados como números, não cadeias de caracteres.

Clientes nativos, como aplicativos de cliente .NET, podem se beneficiar do uso BSON no lugar de formatos baseados em texto, como JSON ou XML. Para clientes de navegador, provavelmente vai preferir ficar com JSON, pois JavaScript pode converter diretamente a carga de JSON.

Felizmente, a API da Web usa [negociação de conteúdo](content-negotiation.md), portanto, sua API pode oferecer suporte a ambos os formatos e permitir que o cliente escolher.

## <a name="enabling-bson-on-the-server"></a>Habilitando BSON no servidor

Em sua configuração de API da Web, adicione o **BsonMediaTypeFormatter** à coleção de formatadores.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Agora, se o cliente solicita "application/bson", API da Web usará o formatador BSON.

Para associar BSON com outros tipos de mídia, adicioná-los à coleção SupportedMediaTypes. O código a seguir adiciona "application/vnd.contoso" para os tipos de mídia com suporte:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Sessão HTTP de exemplo

Neste exemplo, vamos usar a seguinte classe de modelo mais de um controlador Web API simple:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Um cliente pode enviar a solicitação HTTP a seguir:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Aqui está a resposta:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Aqui, substituir os dados binários com &quot;.&quot; caracteres. A seguinte captura de tela de mostra Fiddler valores hexadecimais brutos.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Usando o BSON com HttpClient

Aplicativos de clientes .NET podem usar o formatador BSON com **HttpClient**. Para obter mais informações sobre **HttpClient**, consulte [chamar uma Web API de um cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).

O código a seguir envia uma solicitação GET que aceita BSON e, em seguida, desserializa a carga BSON na resposta.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Para solicitar BSON do servidor, defina o cabeçalho Accept para "application/bson":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Para desserializar o corpo da resposta, use o **BsonMediaTypeFormatter**. Esse formatador não está na coleção de formatadores do padrão, você precisa especificá-la ao ler o corpo da resposta:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

O exemplo a seguir mostra como enviar uma solicitação POST que contém o BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Grande parte do código é o mesmo que o exemplo anterior. Mas no **PostAsync** método, especifique **BsonMediaTypeFormatter** como o formatador:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serialização de tipos primitivos de nível superior

Cada documento BSON é uma lista de pares chave/valor. A especificação de BSON não define uma sintaxe para serializar um único valor bruto, como um inteiro ou uma cadeia de caracteres.

Para contornar essa limitação, o **BsonMediaTypeFormatter** trata tipos primitivos como um caso especial. Antes de serializar, ele converte o valor em um par chave/valor com a chave "Valor". Por exemplo, suponha que seu controlador API retorna um inteiro:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Antes de serializar, o formatador BSON converte isso para o par chave/valor a seguir:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Ao tentar desserializar, o formatador converte os dados de volta para o valor original. No entanto, os clientes que usam um analisador BSON diferente precisará lidar com isso, se sua API da web retorna os valores brutos. Em geral, você deve considerar retornando dados estruturados, em vez de valores brutos.

## <a name="additional-resources"></a>Recursos adicionais

[Exemplo de API BSON Web](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Formatadores de mídia.](media-formatters.md)
