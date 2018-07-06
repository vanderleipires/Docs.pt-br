---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animação em resposta à interação do usuário (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem estrelas...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b774a058e715e4a98e767daf92886f24e627822
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815519"
---
<a name="animating-in-response-to-user-interaction-vb"></a>Animação em resposta à interação do usuário (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem iniciar automaticamente ou podem ser disparadas pela interação do usuário, por exemplo, clicando com o mouse.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem iniciar automaticamente ou podem ser disparadas pela interação do usuário, por exemplo, clicando com o mouse.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatoriamente `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

Dentro de `<Animations>` nó, há cinco maneiras de iniciar a animação por meio de interação do usuário (o elemento ausente é `<OnLoad>` que é executado depois que a página inteira tenha sido totalmente carregada):

- `<OnClick>` (clique do mouse no controle)
- `<OnHoverOut>` (o mouse deixa o controle)
- `<OnHoverOver>` (mouse passa sobre um controle, interrompendo o `<OnHoverOut>` animação)
- `<OnMouseOut>` (o mouse sai de um controle)
- `<OnMouseOver>` (mouse passa sobre um controle, não parar a `<OnMouseOut>` animação)

Nesse cenário, `<OnClick>` é usado. Quando o usuário clica no painel, ele é redimensionado e fade out ao mesmo tempo.

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


[![Um clique do mouse inicia a animação](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

Um clique do mouse inicia a animação ([clique para exibir a imagem em tamanho normal](animating-in-response-to-user-interaction-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](picking-one-animation-out-of-a-list-vb.md)
> [Próximo](disabling-actions-during-animation-vb.md)
