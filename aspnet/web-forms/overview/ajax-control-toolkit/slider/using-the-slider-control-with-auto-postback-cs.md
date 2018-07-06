---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Usando o controle deslizante com Postback automático (c#) | Microsoft Docs
author: wenz
description: O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível fazer a lançar automaticamente slider...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: b5cb3c041f2a8a499d27cbcbc2f8975eedcac12e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834145"
---
<a name="using-the-slider-control-with-auto-postback-c"></a>Usando o controle deslizante com Postback automático (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível fazer o controle deslizante autopostback uma vez, seu valor for alterado.


## <a name="overview"></a>Visão geral

O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível fazer o controle deslizante autopostback uma vez, seu valor for alterado.

## <a name="steps"></a>Etapas

Para fazer o controle deslizante com postback automaticamente após uma alteração, ambas as caixas de texto precisam do atributo `AutoPostBack="true"`: caixa de texto que se tornará o controle deslizante em si e a caixa de texto que contém a posição do controle deslizante. Aqui está a marcação necessária para fazer isso:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

O `SliderExtender` controle do ASP.NET AJAX Control Toolkit atribui a funcionalidade de controle deslizante para duas caixas de texto:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Um elemento de rótulo adicional será usado mais tarde para informar o usuário de um postback:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Por fim, o `ScriptManager` controle do ASP.NET AJAX carrega o JavaScript necessário para o Kit de ferramentas trabalhar:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Agora o controle deslizante está postando novamente; no lado do servidor, esse evento pode ser detectado e tratado:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![Movendo o controle deslizante dispara um postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Movendo o controle deslizante dispara um postback ([clique para exibir a imagem em tamanho normal](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![Depois disso, a data desta alteração é gravada no rótulo](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

Depois disso, a data desta alteração é gravada no rótulo ([clique para exibir a imagem em tamanho normal](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Avançar](databinding-the-slider-control-cs.md)
