---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animação dependendo de uma condição (VB) | Microsoft Docs
author: wenz
description: O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: d3a648ff8299c9720e9f34522f271595ab1b9bc9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="animation-depending-on-a-condition-vb"></a>Animação dependendo de uma condição (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é executada ou não pode também dependem de uma condição na forma de um código JavaScript.


## <a name="overview"></a>Visão geral

O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é executada ou não pode também dependem de uma condição na forma de um código JavaScript.

## <a name="steps"></a>Etapas

Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

Na classe CSS associada para o painel, definir uma cor de plano de fundo adequado e também uma largura fixa para o painel:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

Em seguida, adicione o `AnimationExtender` para a página, fornecendo um `ID`, o `TargetControlID` atributo e o obrigatórias `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

Dentro de `<Animations>` nó, use `<OnLoad>` para executar as animações depois que a página foi totalmente carregada. Em vez de uma das animações regulares, o `<Condition>` elemento entra em cena. O código JavaScript fornecido como o valor da `ConditionScript` atributo é executado no tempo de execução. Se ele for avaliada como true, a animação é executada, caso contrário, não. A seguinte marcação fornece duas animações, cada uma delas sendo executadas em 50% dos casos após aleatório. Como só pode haver uma animação em `<OnLoad>`, os dois `<Condition>` animações são unidas usando o `<Sequence>` elemento:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Observe que o sinal de menor que (`<`) no `ConditionScript` atributo deve ser () com caracteres de escape. Quando você executar esse script, ou nenhuma animação é executado, ou um dos dois não ou fazem.


[![O painel é desaparecimento sem redimensioná-la, para que a segunda animação é executado, a primeira delas não](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

O painel é desaparecimento sem redimensioná-la, para que a segunda animação é executado, a primeira delas não ([clique para exibir a imagem em tamanho normal](animation-depending-on-a-condition-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-several-animations-after-each-other-vb.md)
> [Próximo](picking-one-animation-out-of-a-list-vb.md)
