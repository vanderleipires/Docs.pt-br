---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Disparar uma animação em outro controle (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar um...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 132f9f85eccabc890308984b9e78ed1d2212c57a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834079"
---
<a name="triggering-an-animation-in-another-control-vb"></a>Disparar uma animação em outro controle (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar uma animação é disparada pela interação do usuário com o mesmo controle. No entanto, também é possível interagir com um controle e, em seguida, a animação de outro controle.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar uma animação é disparada pela interação do usuário com o mesmo controle. No entanto, também é possível interagir com um controle e, em seguida, a animação de outro controle.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

Para iniciar a animação o painel, um botão HTML é usado. Observe que `<input type="button" />` é favoured sobre `<asp:Button />` , pois não queremos que um postback quando o usuário clica no botão.

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatoriamente `runat="server"`. É importante definir `TargetControlID` para a ID do botão (o elemento acionar a animação), não para a ID do painel (o elemento que está sendo animado)

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

Dentro de `<Animations>` nó, as animações de lugar como de costume. Para torná-los a alterar o painel, não é o botão, defina a `AnimationTarget` atributo para cada elemento de animação em `AnimationExtender`. O valor para `AnimationTarget` é a ID do painel, é claro. Dessa forma, as animações acontecem com o painel, não com o botão de disparo. Aqui está o `AnimationExtender` marcação para este cenário:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

Observe a ordem especial em que as animações individuais aparecem. Em primeiro lugar, o botão é desativado depois que a animação é executada. Como não há nenhuma `AnimationTarget` de atributo no `<EnableAction>` elemento, essa animação é aplicado ao controle de origem: o botão. As etapas de duas próximas animação deverão ser executadas parallelly (`<Parallel>` elemento). Ambos têm seus `AnimationTarget` os atributos definidos para `"Panel1"`, animação, portanto, o painel, não o botão.


[![Um clique do mouse no botão inicia a animação de painel](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

Um clique do mouse no botão inicia a animação de painel ([clique para exibir a imagem em tamanho normal](triggering-an-animation-in-another-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](disabling-actions-during-animation-vb.md)
> [Próximo](modifying-animations-from-the-server-side-vb.md)
