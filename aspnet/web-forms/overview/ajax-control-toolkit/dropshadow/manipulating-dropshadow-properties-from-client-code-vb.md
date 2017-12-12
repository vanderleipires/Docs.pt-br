---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: "Manipulação de propriedades de sombra no código do cliente (VB) | Microsoft Docs"
author: wenz
description: "O controle de sombra no AJAX Control Toolkit estende um painel com uma sombra. Propriedades desse extensor também podem ser alteradas usando o cliente JavaScrip..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 706ccb5a95873aad4c1b9e0942ab06cf4162710a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a>Manipulação de propriedades de sombra no código do cliente (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> O controle de sombra no AJAX Control Toolkit estende um painel com uma sombra. Propriedades desse extensor também podem ser alteradas usando o código de JavaScript do cliente.


## <a name="overview"></a>Visão Geral

O controle de sombra no AJAX Control Toolkit estende um painel com uma sombra. Propriedades desse extensor também podem ser alteradas usando o código de JavaScript do cliente.

## <a name="steps"></a>Etapas

O código começa com um painel que contém algumas linhas de texto:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

A classe CSS associada fornece o painel de uma cor de plano de fundo adequado:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

O `DropShadowExtender` é adicionado ao estender o painel com um efeito de sombra, definido como 50% de opacidade:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Em seguida, o ASP.NET AJAX `ScriptManager` controle permite que o Kit de ferramentas de controle funcionar:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Outro painel contém dois links de JavaScript para definir a opacidade da sombra: o link menos diminui a opacidade da sombra, o link mais aumenta.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

A função JavaScript `changeOpacity()` , em seguida, deve saber o `DropShadowExtender` controle na página. ASP.NET AJAX define o `$find()` método exatamente dessa tarefa. Em seguida, o `get_Opacity()` método recupera a opacidade atual, o `set_Opacity()` método define. O código JavaScript coloca o valor de opacidade atual no `<label>` elemento:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


[![A opacidade é alterada no lado do cliente](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

A opacidade é alterada no lado do cliente ([clique para exibir a imagem em tamanho normal](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](adjusting-the-z-index-of-a-dropshadow-vb.md)
