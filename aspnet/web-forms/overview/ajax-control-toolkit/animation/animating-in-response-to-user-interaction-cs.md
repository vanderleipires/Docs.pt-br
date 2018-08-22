---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animação em resposta à interação do usuário (c#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem estrelas...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 9de99b3194a5517047e40922d89c28e341ba8e20
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830436"
---
<a name="animating-in-response-to-user-interaction-c"></a>Animação em resposta à interação do usuário (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem iniciar automaticamente ou podem ser disparadas pela interação do usuário, por exemplo, clicando com o mouse.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem iniciar automaticamente ou podem ser disparadas pela interação do usuário, por exemplo, clicando com o mouse.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatoriamente `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

Dentro de `<Animations>` nó, há cinco maneiras de iniciar a animação por meio de interação do usuário (o elemento ausente é `<OnLoad>` que é executado depois que a página inteira tenha sido totalmente carregada):

- `<OnClick>` (clique do mouse no controle)
- `<OnHoverOut>` (o mouse deixa o controle)
- `<OnHoverOver>` (mouse passa sobre um controle, interrompendo o `<OnHoverOut>` animação)
- `<OnMouseOut>` (o mouse sai de um controle)
- `<OnMouseOver>` (mouse passa sobre um controle, não parar a `<OnMouseOut>` animação)

Nesse cenário, `<OnClick>` é usado. Quando o usuário clica no painel, ele é redimensionado e fade out ao mesmo tempo.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


[![Um clique do mouse inicia a animação](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Um clique do mouse inicia a animação ([clique para exibir a imagem em tamanho normal](animating-in-response-to-user-interaction-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](picking-one-animation-out-of-a-list-cs.md)
> [Próximo](disabling-actions-during-animation-cs.md)
