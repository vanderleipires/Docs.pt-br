---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Enviar dados de formulário HTML na API Web ASP.NET: dados de formulário urlencoded | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 24410c92df828d4aaaa3b91dd3e9fa14575fd300
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399864"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Enviar dados de formulário HTML na API Web ASP.NET: dados de formulário urlencoded
====================
por [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Parte 1: Dados de formulário urlencoded

Este artigo mostra como publicar dados de formulário urlencoded para um controlador de API da Web.

- [Visão geral de formulários HTML](#overview_of_html_forms)
- [Enviando tipos complexos](#sending_complex_types)
- [Enviar dados de formulário por meio do AJAX](#sending_form_data_via_ajax)
- [Enviando tipos simples](#sending_simple_types)

> [!NOTE]
> [Baixe o projeto concluído](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Visão geral de formulários HTML

Uso de formulários HTML GET ou POST para enviar dados para o servidor. O **método** atributo da **formulário** elemento fornece o método HTTP:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

O método padrão é GET. Se o formulário usa GET, o formulário de dados são codificados no URI como uma cadeia de caracteres de consulta. Se o formulário usa o POST, os dados do formulário serão colocados no corpo da solicitação. Para dados lançado, o **enctype** atributo especifica o formato do corpo da solicitação:

| enctype | Descrição |
| --- | --- |
| application/x-www-form-urlencoded | Dados de formulário são codificados como pares de nome/valor, semelhantes a uma cadeia de caracteres de consulta do URI. Isso é o formato padrão para POST. |
| multipart/form-data | Dados de formulário são codificados como uma mensagem MIME de várias partes. Use este formato, se você estiver carregando um arquivo para o servidor. |

Parte 1 deste artigo examina o formato x-www-form-urlencoded. [Parte 2](sending-html-form-data-part-2.md) descreve com diversas partes MIME.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Enviando tipos complexos

Normalmente, você enviará um tipo complexo, composto de valores extraídos de vários controles de formulário. Considere o seguinte modelo que representa uma atualização de status:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Aqui está um controlador de API da Web que aceita um `Update` objeto por meio do POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Este controlador usa [roteamento com base na ação](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), portanto, é o modelo de rota &quot;api / {controller} / {action} / {id}&quot;. O cliente irá postar os dados a serem &quot;/api/updates/complex&quot;.


Agora vamos escrever um formulário HTML para os usuários enviem uma atualização de status.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Observe que o **ação** atributo no formulário é o URI do nosso ação do controlador. Aqui está o formulário com alguns valores inseridos no:

![](sending-html-form-data-part-1/_static/image1.png)

Quando o usuário clica em Submit, o navegador envia uma solicitação HTTP semelhante ao seguinte:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Observe que o corpo da solicitação contém os dados do formulário, formatados como pares nome/valor. API da Web converte automaticamente os pares nome/valor em uma instância da `Update` classe.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Enviar dados de formulário por meio do AJAX

Quando um usuário envia um formulário, o navegador navega para fora da página atual e renderiza o corpo da mensagem de resposta. Isso é Okey quando a resposta é uma página HTML. Com uma API da web, no entanto, o corpo da resposta é geralmente vazia ou contiver dados estruturados, como JSON. Nesse caso, ele faz mais sentido para enviar os dados de formulário usando um AJAX solicitação, para que a página possa processar a resposta.

O código a seguir mostra como enviar dados de formulário usando o jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

O jQuery **enviar** função substitui a ação de formulário com uma nova função. Isso substitui o comportamento padrão do botão Enviar. O **serializar** função serializa os dados do formulário em pares nome/valor. Para enviar os dados de formulário para o servidor, chame `$.post()`.

Quando a solicitação for concluída, o `.success()` ou `.error()` manipulador exibe uma mensagem apropriada para o usuário.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Enviando tipos simples

Nas seções anteriores, enviamos um tipo complexo, que a API Web desserializada para uma instância de uma classe de modelo. Você também pode enviar tipos simples, como uma cadeia de caracteres.

> [!NOTE]
> Antes de enviar um tipo simples, considere a possibilidade de encapsular o valor em um tipo complexo em vez disso. Isso lhe oferece os benefícios da validação do modelo no lado do servidor e torna mais fácil de estender o modelo, se necessário.


As etapas básicas para enviar um tipo simples são os mesmos, mas há duas diferenças sutis. Primeiro, no controlador, é necessário decorar o nome do parâmetro com o **FromBody** atributo.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Por padrão, a API da Web tenta obter tipos simples do URI da solicitação. O **FromBody** atributo instrui a API da Web para ler o valor do corpo da solicitação.

> [!NOTE]
> API da Web lê o corpo da resposta no máximo uma vez, apenas um parâmetro de uma ação pode vir de corpo da solicitação. Se você precisar obter vários valores do corpo da solicitação, defina um tipo complexo.


Em segundo lugar, o cliente precisa enviar o valor com o seguinte formato:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

Especificamente, a parte do nome do par nome/valor deve estar vazia para um tipo simples. Nem todos os navegadores dão suporte a isso para os formulários HTML, mas você cria esse formato no script da seguinte maneira:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Aqui está um formulário de exemplo:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

E aqui está o script para enviar o valor do formulário. A única diferença do script anterior é o argumento passado para o **postar** função.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Você pode usar a mesma abordagem para enviar uma matriz de tipos simples:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Recursos adicionais

[Parte 2: Upload do arquivo e MIME com diversas partes](sending-html-form-data-part-2.md)
