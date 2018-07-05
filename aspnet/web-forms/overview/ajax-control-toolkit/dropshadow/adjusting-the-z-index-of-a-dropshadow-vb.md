---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Ajuste do índice Z de um DropShadow (VB) | Microsoft Docs
author: wenz
description: O controle de DropShadow no AJAX Control Toolkit, um painel com uma sombra. No entanto esse sombra às vezes, está em conflito com outros controles, para insta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: afc51c6e52a08f46ffc44cc462bdf9a9d5c8ef43
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397536"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a>Ajuste do índice Z de um DropShadow (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)

> O controle de DropShadow no AJAX Control Toolkit, um painel com uma sombra. No entanto esse sombra às vezes, está em conflito com outros controles, por exemplo, o controle de Menu do ASP.NET. Quando uma entrada de menu é exibido, ele aparece por trás da sombra.


## <a name="overview"></a>Visão geral

O controle de DropShadow no AJAX Control Toolkit, um painel com uma sombra. No entanto esse sombra às vezes, está em conflito com outros controles, por exemplo, o controle de Menu do ASP.NET. Quando uma entrada de menu é exibido, ele aparece por trás da sombra.

## <a name="steps"></a>Etapas

O código começa com o próprio painel, que contém o texto suficiente para que o painel contém texto suficiente para o efeito para ser visível:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

Outro painel é colocado diretamente antes do `panelShadow` painel. Ele contém um menu com a orientação horizontal para que as entradas de menu aparecem ao longo (ou melhor: sob) o `dropShadow` painel):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

Em seguida, o `DropShadowExtender` é adicionado ao estender o `panelShadow` painel com um efeito de sombra:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

Por fim, o ASP.NET AJAX `ScriptManager` controle permite que o Kit de ferramentas trabalhar:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

Quando você executa esse script, as entradas de menu aparecem sob o painel. No entanto, o menu usa a classe CSS `panel` em que você só precisa definir duas coisas para fazer com que elementos são exibidos na frente de outro painel:

- Posicionamento relativo
- Um índice z positivo

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

Em seguida, o `DropShadowExtender` controle não está em conflito mais com o controle de Menu.


[![Antes: A entrada de menu não estiver visível](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)

Antes: A entrada de menu não é visível ([clique para exibir a imagem em tamanho normal](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))


[![Depois: A entrada de menu é exibido](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)

Depois: A entrada de menu é exibido ([clique para exibir a imagem em tamanho normal](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Próximo](manipulating-dropshadow-properties-from-client-code-vb.md)
