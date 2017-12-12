---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "Enviar dados de formulário HTML na API da Web do ASP.NET: dados de formulário urlencoded | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Enviar dados de formulário HTML na API da Web do ASP.NET: dados de formulário urlencoded
====================
por [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Parte 1: Dados de formulário urlencoded

Este artigo mostra como enviar dados de formulário urlencoded para um controlador Web API.

- [Visão geral de formulários HTML](#overview_of_html_forms)
- [Enviar tipos complexos](#sending_complex_types)
- [Enviar dados de formulário por meio de AJAX](#sending_form_data_via_ajax)
- [Enviar tipos simples](#sending_simple_types)

> [!NOTE]
> [Baixe o projeto concluído](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Visão geral de formulários HTML

Uso de formulários HTML GET ou POST para enviar dados para o servidor. O **método** atributo o **formulário** elemento fornece o método HTTP:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

O método padrão é GET. Se o formulário usa GET, o formulário de dados são codificados no URI como uma cadeia de caracteres de consulta. Se o formulário usa POST, os dados do formulário são colocados no corpo da solicitação. Para dados de postagem, o **enctype** atributo especifica o formato do corpo da solicitação:

| enctype | Descrição |
| --- | --- |
| Application/x-www-form-urlencoded | Dados de formulário são codificados como pares de nome/valor, semelhantes a uma cadeia de caracteres de consulta URI. Este é o formato padrão para POST. |
| com diversas partes/dados de formulário | Dados de formulário são codificados como uma mensagem MIME de várias partes. Use este formato se você estiver carregando um arquivo para o servidor. |

Parte 1 deste artigo examina o formato x-www-form-urlencoded. [Parte 2](sending-html-form-data-part-2.md) descreve MIME de várias partes.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Enviar tipos complexos

Normalmente, você enviará um tipo complexo, composto de valores extraídos de vários controles de formulário. Considere o seguinte modelo que representa uma atualização de status:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Aqui está um controlador Web API que aceita um `Update` objeto por meio de POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Usa esse controlador [roteamento baseado em ação](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), portanto, o modelo de rota é &quot;api / {controller} / {action} / {id}&quot;. O cliente publicará os dados a serem &quot;/api/updates/complex&quot;.


Agora vamos escrever um formulário HTML para os usuários enviem uma atualização de status.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Observe que o **ação** atributo no formulário é o URI da nossa ação do controlador. Aqui está o formulário com alguns valores inseridos em:

![](sending-html-form-data-part-1/_static/image1.png)

Quando o usuário clicar em enviar, o navegador envia uma solicitação HTTP semelhante à seguinte:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Observe que o corpo da solicitação contém os dados do formulário, formatados como pares nome/valor. API da Web converte automaticamente os pares nome/valor em uma instância de `Update` classe.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Enviar dados de formulário por meio de AJAX

Quando um usuário envia um formulário, o navegador navega para fora da página atual e renderiza o corpo da mensagem de resposta. Isso é Okey quando a resposta é uma página HTML. Com uma API da web, no entanto, o corpo da resposta é geralmente vazio ou contém dados estruturados, como JSON. Nesse caso, faz mais sentido para enviar a solicitação de dados do formulário usando um AJAX, para que a página pode processar a resposta.

O código a seguir mostra como enviar dados de formulário usando jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

O jQuery **enviar** função substitui a ação de formulário com uma nova função. Isso substitui o comportamento padrão do botão Enviar. O **serializar** função serializa os dados do formulário em pares de nome/valor. Para enviar os dados do formulário para o servidor, chame `$.post()`.

Quando a solicitação é concluída, o `.success()` ou `.error()` manipulador exibe uma mensagem apropriada para o usuário.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Enviar tipos simples

Nas seções anteriores, enviamos um tipo complexo, API da Web desserializada para uma instância de uma classe de modelo. Você também pode enviar tipos simples, como uma cadeia de caracteres.

> [!NOTE]
> Antes de enviar um tipo simples, considere o valor de disposição em um tipo complexo em vez disso. Isso traz os benefícios de validação de modelo no lado do servidor e torna mais fácil estender o modelo, se necessário.


As etapas básicas para enviar um tipo simples são os mesmos, mas há duas diferenças sutis. Primeiro, no controlador, você deve decorar o nome do parâmetro com o **FromBody** atributo.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Por padrão, a API da Web tenta obter tipos simples do URI da solicitação. O **FromBody** atributo informa a API da Web para ler o valor do corpo da solicitação.

> [!NOTE]
> API da Web lê o corpo da resposta no máximo uma vez, apenas um parâmetro de uma ação pode vir do corpo da solicitação. Se você precisar obter vários valores do corpo da solicitação, defina um tipo complexo.


Segundo, o cliente precisa enviar o valor com o seguinte formato:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

Especificamente, a parte do nome do par nome/valor deve estar vazia para um tipo simples. Nem todos os navegadores dão suporte a isso para formulários HTML, mas você criar este formato no script da seguinte maneira:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Aqui está um formulário de exemplo:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

E aqui está o script para enviar o valor de formulário. A única diferença do script anterior é o argumento passado para o **lançar** função.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Você pode usar a mesma abordagem para enviar uma matriz de tipos simples:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Recursos adicionais

[Parte 2: Carregamento de arquivo e com diversas partes MIME](sending-html-form-data-part-2.md)
