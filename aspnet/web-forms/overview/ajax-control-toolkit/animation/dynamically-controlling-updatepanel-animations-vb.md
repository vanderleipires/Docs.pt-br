---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Controlando dinamicamente UpdatePanel animações (VB) | Microsoft Docs
author: wenz
description: O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: ff2853b4457a83a7367b4d1072d21929c40a3ed2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871539"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>Controlando dinamicamente UpdatePanel animações (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo do UpdatePanel, existe um extensor especial que baseia-se na estrutura da animação: UpdatePanelAnimation. Ele também pode trabalhar com gatilhos UpdatePanel.


## <a name="overview"></a>Visão geral

O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um `UpdatePanel`, existe um extensor especial baseia-se na estrutura da animação: `UpdatePanelAnimation`. Ele também pode trabalhar em conjunto com `UpdatePanel` gatilhos.

## <a name="steps"></a>Etapas

Normalmente, a primeira etapa é incluir o `ScriptManager` na página de forma que a biblioteca ASP.NET AJAX foi carregada e o Kit de ferramentas de controle pode ser usado:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

A animação neste cenário será aplicada a uma exibição da hora atual. Essas informações podem ser gravadas em um rótulo que usa o `Page_Load()` método, ou (para simplificar) é usado o seguinte código embutido:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

Além disso, é criado um botão para disparar o tempo de atualização:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

Esse código é colocado no `<ContentTemplate>` seção um `UpdatePanel` elemento. O painel `UpdateMode` atributo deve ser definido como `"Conditional"`, uma vez que apenas os gatilhos podem atualizar o conteúdo do painel. No `<Triggers>` seção o `UpdatePanel`, um gatilho de postback assíncrono é criado e vinculado ao `Click` eventos do botão. Portanto, se o usuário clica no botão, o `UpdatePanel` é atualizado. Aqui está a marcação para o `UpdatePanel` controle:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Por fim, o `UpdatePanelAnimationExtender` deve ser configurado: definir o `TargetControlID` de atributo para a ID do painel e definir uma animação dentro do extensor. Fade in faz sentido, o que cria uma ênfase visual adequada na hora atualizada. A marcação de extensor, em seguida, pode se parecer com isso:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Execute o arquivo no navegador. Sempre que você clicar no botão, a hora atual é mostrada no painel, sempre fade in para a duração de um segundo.


[![A hora atual é fade in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

A hora atual é fade in ([clique para exibir a imagem em tamanho normal](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animating-an-updatepanel-control-vb.md)
