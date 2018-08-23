---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Desabilitar ações durante a animação (c#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte à ação...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 4315f06ea1599bacb93a23a3759610e19754cfba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825384"
---
<a name="disabling-actions-during-animation-c"></a>Desabilitar ações durante a animação (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte a ações, como cliques do mouse. No entanto, quando um clique do mouse inicia uma animação, é desejável para desabilitar os cliques do mouse durante a animação.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte a ações, como cliques do mouse. No entanto, quando um clique do mouse inicia uma animação, é desejável para desabilitar os cliques do mouse durante a animação.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

A animação será aplicada a um botão HTML como este:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Observe que um controle HTML é usado em vez de um controle da Web, pois não queremos que o botão para criar um postback; Ele apenas deverão iniciar a animação do lado do cliente para nós.

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatoriamente `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

Dentro de `<Animations>` nó, `<OnClick>` é o elemento certo a lidar com o clique do mouse. No entanto, o botão foi clicado durante a animação. O `<EnableAction>` elemento pode cuidar disso. Definindo `Enabled="false"` desabilita o botão como parte da animação. Como estamos usando várias animações individuais (como desabilitar o botão e as animações reais), o `<Parallel>` elemento é necessário para colar as animações únicas juntos em um. Aqui está a marcação completa para `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Também seria possível habilitar novamente o botão após a animação, usando o seguinte elemento XML no final da lista:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

No entanto em determinado cenário seria inútil desde o botão fade out e não fica visível no final da animação.


[![O botão estiver desabilitado, assim que a animação é executada](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

O botão estiver desabilitado, assim que a animação é executada ([clique para exibir a imagem em tamanho normal](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animating-in-response-to-user-interaction-cs.md)
> [Próximo](triggering-an-animation-in-another-control-cs.md)
