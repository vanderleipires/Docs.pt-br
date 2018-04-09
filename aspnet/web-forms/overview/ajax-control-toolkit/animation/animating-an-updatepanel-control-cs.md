---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animar um controle UpdatePanel (c#) | Microsoft Docs
author: wenz
description: O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 5d8d5b9c3f15b39045b5e01b455bdddfc9443a24
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="animating-an-updatepanel-control-c"></a>Animar um controle UpdatePanel (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)

> O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo do UpdatePanel, existe um extensor especial que baseia-se na estrutura da animação: UpdatePanelAnimation. Este tutorial mostra como configurar esse uma animação para UpdatePanel.


## <a name="overview"></a>Visão Geral

O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um `UpdatePanel`, existe um extensor especial baseia-se na estrutura da animação: `UpdatePanelAnimation`. Este tutorial mostra como configurar esse uma animação para um `UpdatePanel`.

## <a name="steps"></a>Etapas

Normalmente, a primeira etapa é incluir o `ScriptManager` na página de forma que a biblioteca ASP.NET AJAX foi carregada e o Kit de ferramentas de controle pode ser usado:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

A animação neste cenário será aplicada a um ASP.NET `Wizard` controle da web que reside em um `UpdatePanel`. Três etapas (arbitrárias) fornecem opções suficiente para disparar postbacks:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

A marcação necessária para o `UpdatePanelAnimationExtender` controle é bastante semelhante à marcação usada para o `AnimationExtender`. No `TargetControlID` atributo fornecemos o `ID` do `UpdatePanel` para animar; dentro a `UpdatePanelAnimationExtender` controle, o `<Animations>` elemento contém marcação XML para a animação. No entanto, há uma diferença: A quantidade de eventos e manipuladores de eventos é limitada em comparação aos `AnimationExtender`. Para `UpdatePanels`, apenas duas delas existir:

- `<OnUpdated>` Quando o UpdatePanel foi atualizado
- `<OnUpdating>` Quando o UpdatePanel inicia a atualização

Nesse cenário, o novo conteúdo do `UpdatePanel` (após o postback) deverá desaparecer. Esta é a marcação necessário para que:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

Agora, sempre que um postback ocorre dentro do UpdatePanel, o novo conteúdo do painel desaparecer sem problemas.


[![A próxima etapa do assistente é fade in](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)

A próxima etapa do assistente é fade in ([clique para exibir a imagem em tamanho normal](animating-an-updatepanel-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](changing-an-animation-using-client-side-code-cs.md)
> [Próximo](dynamically-controlling-updatepanel-animations-cs.md)
