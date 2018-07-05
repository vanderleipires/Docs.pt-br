---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Adicionando animação a um controle (c#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Este tutorial mostra como...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 80c982e041af2d0b9ee789665613ced0311dfcc9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396566"
---
<a name="adding-animation-to-a-control-c"></a>Adicionando animação a um controle (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Este tutorial mostra como configurar uma animação desse tipo.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Este tutorial mostra como configurar uma animação desse tipo.

## <a name="steps"></a>Etapas

Como de costume, a primeira etapa é incluir o `ScriptManager` na página de modo que a biblioteca do AJAX ASP.NET é carregada e o Kit de ferramentas de controle pode ser usado:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

A animação neste cenário será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

A classe CSS associada para o painel define uma cor de fundo e uma largura:

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Em seguida, é necessário o `AnimationExtender`. Depois de fornecer um `ID` e o usual `runat="server"`, o `TargetControlID` atributo deve ser definido para o controle para animar em nosso caso, o painel:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

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

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Quando você executa esse script, o painel é exibido e fade out em um e meio segundos.


[![O painel está desaparecendo](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

O painel está desaparecendo ([clique para exibir a imagem em tamanho normal](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Avançar](executing-several-animations-at-the-same-time-cs.md)
