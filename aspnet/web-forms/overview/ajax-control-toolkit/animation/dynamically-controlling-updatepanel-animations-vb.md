---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Controlando dinamicamente animações UpdatePanel (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 2a4c01ffdee20f2c7970d999b34bf1374088d4c5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830622"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>Controlando dinamicamente animações UpdatePanel (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um UpdatePanel, existe um extensor especial que depende muito do framework de animação: UpdatePanelAnimation. Ele também pode trabalhar junto com os gatilhos UpdatePanel.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um `UpdatePanel`, um extensor especial existe que depende muito do framework de animação: `UpdatePanelAnimation`. Ele também pode trabalhar em conjunto com `UpdatePanel` gatilhos.

## <a name="steps"></a>Etapas

Como de costume, a primeira etapa é incluir o `ScriptManager` na página de modo que a biblioteca do AJAX ASP.NET é carregada e o Kit de ferramentas de controle pode ser usado:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

A animação neste cenário será aplicada a uma exibição de tempo atual. Essas informações podem ser gravadas em um rótulo usando o `Page_Load()` método, ou (para fins de simplicidade) o seguinte código embutido é usado:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

Além disso, é criado um botão para disparar a atualização de tempo:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

Esse código é colocado na `<ContentTemplate>` seção de um `UpdatePanel` elemento. O painel `UpdateMode` atributo deve ser definido como `"Conditional"`, já que apenas os gatilhos podem atualizar o conteúdo do painel. No `<Triggers>` seção o `UpdatePanel`, um gatilho de postback assíncrono é criado e vinculado ao `Click` evento do botão. Portanto, se o usuário clica no botão, o `UpdatePanel` é atualizado. Aqui está a marcação para o `UpdatePanel` controle:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Por fim, o `UpdatePanelAnimationExtender` deve ser configurado: definir o `TargetControlID` para a ID do painel de atributo e definir uma animação dentro do extensor. Fade in faz sentido, o que cria uma ótima visual enfatiza a hora da atualização. Sua marcação de extensor pode, em seguida, esta aparência:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Execute o arquivo no navegador. Sempre que você clica no botão, a hora atual é mostrada no painel, sempre fade in para a duração de um segundo.


[![A hora atual está desaparecendo](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

A hora atual está desaparecendo ([clique para exibir a imagem em tamanho normal](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animating-an-updatepanel-control-vb.md)
