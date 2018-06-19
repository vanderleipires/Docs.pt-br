---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulação de propriedades de sombra no código do cliente (c#) | Microsoft Docs
author: wenz
description: Personalizando a Interface de edição do DataList.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 37a7784e1d42477e31938e1d15495993ac86fc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870330"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a>Manipulação de propriedades de sombra no código do cliente (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> O controle de sombra no AJAX Control Toolkit estende um painel com uma sombra. Propriedades desse extensor também podem ser alteradas usando o código de JavaScript do cliente.


## <a name="overview"></a>Visão geral

O controle de sombra no AJAX Control Toolkit estende um painel com uma sombra. Propriedades desse extensor também podem ser alteradas usando o código de JavaScript do cliente.

## <a name="steps"></a>Etapas

O código começa com um painel que contém algumas linhas de texto:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

A classe CSS associada fornece o painel de uma cor de plano de fundo adequado:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

O `DropShadowExtender` é adicionado ao estender o painel com um efeito de sombra, definido como 50% de opacidade:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Em seguida, o ASP.NET AJAX `ScriptManager` controle permite que o Kit de ferramentas de controle funcionar:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Outro painel contém dois links de JavaScript para definir a opacidade da sombra: o link menos diminui a opacidade da sombra, o link mais aumenta.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

A função JavaScript `changeOpacity()` , em seguida, deve saber o `DropShadowExtender` controle na página. ASP.NET AJAX define o `$find()` método exatamente dessa tarefa. Em seguida, o `get_Opacity()` método recupera a opacidade atual, o `set_Opacity()` método define. O código JavaScript coloca o valor de opacidade atual no `<label>` elemento:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![A opacidade é alterada no lado do cliente](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

A opacidade é alterada no lado do cliente ([clique para exibir a imagem em tamanho normal](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Próximo](adjusting-the-z-index-of-a-dropshadow-vb.md)
