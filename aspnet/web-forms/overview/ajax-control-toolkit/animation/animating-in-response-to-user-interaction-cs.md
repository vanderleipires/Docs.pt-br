---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animação em resposta à interação do usuário (c#) | Microsoft Docs
author: wenz
description: O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem estrelas...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 783563f4e33087e99a071cf829ca6bab246ba3b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="animating-in-response-to-user-interaction-c"></a>Animação em resposta à interação do usuário (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem iniciar automaticamente, ou podem ser disparadas pela interação do usuário, por exemplo, clicando com o mouse.


## <a name="overview"></a>Visão geral

O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem iniciar automaticamente, ou podem ser disparadas pela interação do usuário, por exemplo, clicando com o mouse.

## <a name="steps"></a>Etapas

Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

Na classe CSS associada para o painel, definir uma cor de plano de fundo adequado e também uma largura fixa para o painel:

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Em seguida, adicione o `AnimationExtender` para a página, fornecendo um `ID`, o `TargetControlID` atributo e o obrigatórias `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

Dentro de `<Animations>` nó, há cinco maneiras de iniciar a animação por meio da interação do usuário (o elemento ausente é `<OnLoad>` que é executado depois que a página inteira foi totalmente carregada):

- `<OnClick>` (clique do mouse no controle)
- `<OnHoverOut>` (o mouse sai do controle)
- `<OnHoverOver>` (mouse passa sobre um controle, interrompendo o `<OnHoverOut>` animação)
- `<OnMouseOut>` (o mouse sai de um controle)
- `<OnMouseOver>` (mouse passa sobre um controle não parar a `<OnMouseOut>` animação)

Nesse cenário, `<OnClick>` é usado. Quando o usuário clica no painel, ele é redimensionado e desaparece ao mesmo tempo.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


[![Um clique do mouse inicia a animação](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Um clique do mouse inicia a animação ([clique para exibir a imagem em tamanho normal](animating-in-response-to-user-interaction-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](picking-one-animation-out-of-a-list-cs.md)
> [Próximo](disabling-actions-during-animation-cs.md)
