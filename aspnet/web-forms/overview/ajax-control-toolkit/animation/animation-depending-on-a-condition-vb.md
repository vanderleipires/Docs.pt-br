---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animação dependendo de uma condição (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 160b53519382c215db039fd6297bb907688b81c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368551"
---
<a name="animation-depending-on-a-condition-vb"></a>Animação dependendo de uma condição (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é executada ou não também pode depender uma condição na forma de um código JavaScript.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é executada ou não também pode depender uma condição na forma de um código JavaScript.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatório `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

Dentro de `<Animations>` nó, use `<OnLoad>` para as animações são executadas uma vez que a página foi totalmente carregada. Em vez de uma das animações regulares, o `<Condition>` elemento entra em cena. O código JavaScript fornecido como o valor da `ConditionScript` atributo é executado em tempo de execução. Se for avaliada como true, a animação é executada, caso contrário, não. A marcação a seguir fornece duas animações, cada um deles sendo executado em 50% dos casos após aleatório. Já que pode haver apenas uma animação dentro `<OnLoad>`, os dois `<Condition>` animações são unidas usando o `<Sequence>` elemento:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Observe que o sinal de menor que (`<`) na `ConditionScript` atributo deve ser (escape). Quando você executar esse script, nenhuma animação será executado, ou uma das duas delas, ou ambos fazem.


[![O painel está desaparecendo sem redimensionamento, portanto, o segunda animação é executado, o primeiro deles não](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

O painel está desaparecendo sem redimensionamento, portanto, o segunda animação é executado, o primeiro deles não ([clique para exibir a imagem em tamanho normal](animation-depending-on-a-condition-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-several-animations-after-each-other-vb.md)
> [Próximo](picking-one-animation-out-of-a-list-vb.md)
