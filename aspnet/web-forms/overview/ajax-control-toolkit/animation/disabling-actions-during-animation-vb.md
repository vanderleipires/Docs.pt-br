---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Desabilitar ações durante a animação (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte à ação...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 824eb8f0a219df08044bc62d7ad020c53e1ee792
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399568"
---
<a name="disabling-actions-during-animation-vb"></a>Desabilitar ações durante a animação (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte a ações, como cliques do mouse. No entanto, quando um clique do mouse inicia uma animação, é desejável para desabilitar os cliques do mouse durante a animação.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte a ações, como cliques do mouse. No entanto, quando um clique do mouse inicia uma animação, é desejável para desabilitar os cliques do mouse durante a animação.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

A animação será aplicada a um botão HTML como este:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Observe que um controle HTML é usado em vez de um controle da Web, pois não queremos que o botão para criar um postback; Ele apenas deverão iniciar a animação do lado do cliente para nós.

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatoriamente `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

Dentro de `<Animations>` nó, `<OnClick>` é o elemento certo a lidar com o clique do mouse. No entanto, o botão foi clicado durante a animação. O `<EnableAction>` elemento pode cuidar disso. Definindo `Enabled="false"` desabilita o botão como parte da animação. Como estamos usando várias animações individuais (como desabilitar o botão e as animações reais), o `<Parallel>` elemento é necessário para colar as animações únicas juntos em um. Aqui está a marcação completa para `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

Também seria possível habilitar novamente o botão após a animação, usando o seguinte elemento XML no final da lista:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

No entanto em determinado cenário seria inútil desde o botão fade out e não fica visível no final da animação.


[![O botão estiver desabilitado, assim que a animação é executada](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

O botão estiver desabilitado, assim que a animação é executada ([clique para exibir a imagem em tamanho normal](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animating-in-response-to-user-interaction-vb.md)
> [Próximo](triggering-an-animation-in-another-control-vb.md)
