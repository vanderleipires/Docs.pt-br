---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Adicionando animação a um controle (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Este tutorial mostra como...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 9392b1bab2289d886baf308d05644afbdc42a13a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832950"
---
<a name="adding-animation-to-a-control-vb"></a>Adicionando animação a um controle (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Este tutorial mostra como configurar uma animação desse tipo.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Este tutorial mostra como configurar uma animação desse tipo.

## <a name="steps"></a>Etapas

Como de costume, a primeira etapa é incluir o `ScriptManager` na página de modo que a biblioteca do AJAX ASP.NET é carregada e o Kit de ferramentas de controle pode ser usado:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

A animação neste cenário será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

A classe CSS associada para o painel define uma cor de fundo e uma largura:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

Em seguida, é necessário o `AnimationExtender`. Depois de fornecer um `ID` e o usual `runat="server"`, o `TargetControlID` atributo deve ser definido para o controle para animar em nosso caso, o painel:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

A animação completa é aplicada declarativamente, usando uma sintaxe XML, infelizmente atualmente não têm suportada completo pelo IntelliSense do Visual Studio. O nó raiz é `<Animations>;` dentro desse nó, vários eventos são permitidos que determinam quando as animações take(s) local:

- `OnClick` (clique do mouse)
- `OnHoverOut` (quando o mouse deixa um controle)
- `OnHoverOver` (quando o mouse passa sobre um controle, interrompendo o `OnHoverOut` animação)
- `OnLoad` (quando a página foi carregada)
- `OnMouseOut` (quando o mouse deixa um controle)
- `OnMouseOver` (quando o mouse passa sobre um controle, não parar a `OnMouseOut` animação)

A estrutura vem com um conjunto de animações, cada um representado por seu próprio elemento XML. Aqui está uma seleção:

- `<Color>` (uma cor de alteração)
- `<FadeIn>` (fade in)
- `<FadeOut>` (esmaecimento)
- `<Property>` (propriedade de um controle de alteração)
- `<Pulse>` (pulsating)
- `<Resize>` (o tamanho de alteração)
- `<Scale>` (proporcionalmente o tamanho de alteração)

Neste exemplo, o painel deve desaparecer. A animação deverão utilizar 1,5 segundos (`Duration` atributo), exibindo a 24 quadros (etapas de animação) por segundo (`Fps` attributs). Aqui está a marcação completa para o `AnimationExtender` controle:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Quando você executa esse script, o painel é exibido e fade out em um e meio segundos.


[![O painel está desaparecendo](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

O painel está desaparecendo ([clique para exibir a imagem em tamanho normal](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-controlling-updatepanel-animations-cs.md)
> [Próximo](executing-several-animations-at-the-same-time-vb.md)
