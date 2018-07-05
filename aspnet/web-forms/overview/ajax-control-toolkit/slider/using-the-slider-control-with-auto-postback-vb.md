---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Usando o controle deslizante com Postback automático (VB) | Microsoft Docs
author: wenz
description: O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível fazer a lançar automaticamente slider...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ddc5b119a7f58b4d289f11e1789cf193870ae4e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374033"
---
<a name="using-the-slider-control-with-auto-postback-vb"></a>Usando o controle deslizante com Postback automático (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível fazer o controle deslizante autopostback uma vez, seu valor for alterado.


## <a name="overview"></a>Visão geral

O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível fazer o controle deslizante autopostback uma vez, seu valor for alterado.

## <a name="steps"></a>Etapas

Para fazer o controle deslizante com postback automaticamente após uma alteração, ambas as caixas de texto precisam do atributo `AutoPostBack="true"`: caixa de texto que se tornará o controle deslizante em si e a caixa de texto que contém a posição do controle deslizante. Aqui está a marcação necessária para fazer isso:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

O `SliderExtender` controle do ASP.NET AJAX Control Toolkit atribui a funcionalidade de controle deslizante para duas caixas de texto:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Um elemento de rótulo adicional será usado mais tarde para informar o usuário de um postback:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Por fim, o `ScriptManager` controle do ASP.NET AJAX carrega o JavaScript necessário para o Kit de ferramentas trabalhar:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

Agora o controle deslizante está postando novamente; no lado do servidor, esse evento pode ser detectado e tratado:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![Movendo o controle deslizante dispara um postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Movendo o controle deslizante dispara um postback ([clique para exibir a imagem em tamanho normal](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![Depois disso, a data desta alteração é gravada no rótulo](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Depois disso, a data desta alteração é gravada no rótulo ([clique para exibir a imagem em tamanho normal](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](databinding-the-slider-control-cs.md)
> [Próximo](databinding-the-slider-control-vb.md)
