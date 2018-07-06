---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animar um controle UpdatePanel (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c508d549c7740079b02da323c655c9f789cfd3b9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805295"
---
<a name="animating-an-updatepanel-control-vb"></a>Animar um controle UpdatePanel (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um UpdatePanel, existe um extensor especial que depende muito do framework de animação: UpdatePanelAnimation. Este tutorial mostra como configurar uma animação desse tipo para um UpdatePanel.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um `UpdatePanel`, um extensor especial existe que depende muito do framework de animação: `UpdatePanelAnimation`. Este tutorial mostra como configurar uma animação desse tipo para um `UpdatePanel`.

## <a name="steps"></a>Etapas

Como de costume, a primeira etapa é incluir o `ScriptManager` na página de modo que a biblioteca do AJAX ASP.NET é carregada e o Kit de ferramentas de controle pode ser usado:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

A animação neste cenário será aplicada a um ASP.NET `Wizard` controle de web que reside em um `UpdatePanel`. Três etapas (arbitrárias) fornecem opções suficiente para disparar postbacks:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

A marcação necessária para o `UpdatePanelAnimationExtender` controle é bastante semelhante à marcação usada para o `AnimationExtender`. No `TargetControlID` atributo fornecemos a `ID` da `UpdatePanel` animar; dentro a `UpdatePanelAnimationExtender` controle, o `<Animations>` elemento contém a marcação XML para as animações. No entanto, há uma diferença: A quantidade de eventos e manipuladores de eventos é limitada em comparação com `AnimationExtender`. Para `UpdatePanels`, apenas duas delas existir:

- `<OnUpdated>` Quando o UpdatePanel foi atualizado
- `<OnUpdating>` Quando o UpdatePanel inicia a atualização

Nesse cenário, o novo conteúdo do `UpdatePanel` (após o postback) deverá aplicar fade-in. Isso é a marcação necessária para fazer isso:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Agora, sempre que um postback ocorre dentro do UpdatePanel, o novo conteúdo do painel aplicar fade-in sem problemas.


[![A próxima etapa do assistente está desaparecendo](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

A próxima etapa do assistente está desaparecendo ([clique para exibir a imagem em tamanho normal](animating-an-updatepanel-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](changing-an-animation-using-client-side-code-vb.md)
> [Próximo](dynamically-controlling-updatepanel-animations-vb.md)
