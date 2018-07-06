---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animação dependendo de uma condição (c#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: cb08c330d6fbc86035a2f21ad382cc009411bcd6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808418"
---
<a name="animation-depending-on-a-condition-c"></a>Animação dependendo de uma condição (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é executada ou não também pode depender uma condição na forma de um código JavaScript.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é executada ou não também pode depender uma condição na forma de um código JavaScript.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatório `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

Dentro de `<Animations>` nó, use `<OnLoad>` para as animações são executadas uma vez que a página foi totalmente carregada. Em vez de uma das animações regulares, o `<Condition>` elemento entra em cena. O código JavaScript fornecido como o valor da `ConditionScript` atributo é executado em tempo de execução. Se for avaliada como true, a animação é executada, caso contrário, não. A marcação a seguir fornece duas animações, cada um deles sendo executado em 50% dos casos após aleatório. Já que pode haver apenas uma animação dentro `<OnLoad>`, os dois `<Condition>` animações são unidas usando o `<Sequence>` elemento:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Observe que o sinal de menor que (`<`) na `ConditionScript` atributo deve ser (escape). Quando você executar esse script, nenhuma animação será executado, ou uma das duas delas, ou ambos fazem.


[![O painel está desaparecendo sem redimensionamento, portanto, o segunda animação é executado, o primeiro deles não](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

O painel está desaparecendo sem redimensionamento, portanto, o segunda animação é executado, o primeiro deles não ([clique para exibir a imagem em tamanho normal](animation-depending-on-a-condition-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-several-animations-after-each-other-cs.md)
> [Próximo](picking-one-animation-out-of-a-list-cs.md)
