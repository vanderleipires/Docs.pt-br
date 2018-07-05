---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Escolhendo uma animação em uma lista (c#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A estrutura também mitir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 4b0193f47697e072605ed8c01a37fbfa128eb0e0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380740"
---
<a name="picking-one-animation-out-of-a-list-c"></a>Escolhendo uma animação em uma lista (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. O framework também permite que o programador escolher uma animação em uma lista de animações, dependendo da avaliação de algum código JavaScript.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. O framework também permite que o programador escolher uma animação em uma lista de animações, dependendo da avaliação de algum código JavaScript.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatório `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

Dentro de `<Animations>` nó, use `<OnLoad>` para as animações são executadas uma vez que a página foi totalmente carregada. Em vez de uma das animações regulares, o `<Case>` elemento entra em cena. O valor do seu atributo SelectScript é avaliado; o valor de retorno deve ser numérico. Dependendo desse número, uma das subanimations dentro &lt;caso&gt; é executado. Por exemplo, se SelectScript for avaliada como 2, o Kit de ferramentas de controle é executado a terceira animação dentro &lt;caso&gt; (contando começa em 0).

A marcação a seguir define três subanimations: redimensionar a largura, redimensionar a altura e desaparecimento. O código JavaScript (`Math.floor(3 * Math.random())`), em seguida, escolhe um número entre 0 e 2, para que um dos três animações é executado:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


[![Uma das três animações possíveis: O painel obtém maior](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)

Uma das três animações possíveis: O painel obtém mais amplo ([clique para exibir a imagem em tamanho normal](picking-one-animation-out-of-a-list-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animation-depending-on-a-condition-cs.md)
> [Próximo](animating-in-response-to-user-interaction-cs.md)
