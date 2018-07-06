---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Disparar uma animação em outro controle (c#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar um...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a7eeb522ae982cd5a84aa9c8e4228871c8c78440
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810796"
---
<a name="triggering-an-animation-in-another-control-c"></a>Disparar uma animação em outro controle (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar uma animação é disparada pela interação do usuário com o mesmo controle. No entanto, também é possível interagir com um controle e, em seguida, a animação de outro controle.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar uma animação é disparada pela interação do usuário com o mesmo controle. No entanto, também é possível interagir com um controle e, em seguida, a animação de outro controle.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Para iniciar a animação o painel, um botão HTML é usado. Observe que `<input type="button" />` é favoured sobre `<asp:Button />` , pois não queremos que um postback quando o usuário clica no botão.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatoriamente `runat="server"`. É importante definir `TargetControlID` para a ID do botão (o elemento acionar a animação), não para a ID do painel (o elemento que está sendo animado)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

Dentro de `<Animations>` nó, as animações de lugar como de costume. Para torná-los a alterar o painel, não é o botão, defina a `AnimationTarget` atributo para cada elemento de animação em `AnimationExtender`. O valor para `AnimationTarget` é a ID do painel, é claro. Dessa forma, as animações acontecem com o painel, não com o botão de disparo. Aqui está o `AnimationExtender` marcação para este cenário:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Observe a ordem especial em que as animações individuais aparecem. Em primeiro lugar, o botão é desativado depois que a animação é executada. Como não há nenhuma `AnimationTarget` de atributo no `<EnableAction>` elemento, essa animação é aplicado ao controle de origem: o botão. As etapas de duas próximas animação deverão ser executadas parallelly (`<Parallel>` elemento). Ambos têm seus `AnimationTarget` os atributos definidos para `"Panel1"`, animação, portanto, o painel, não o botão.


[![Um clique do mouse no botão inicia a animação de painel](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Um clique do mouse no botão inicia a animação de painel ([clique para exibir a imagem em tamanho normal](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](disabling-actions-during-animation-cs.md)
> [Próximo](modifying-animations-from-the-server-side-cs.md)
