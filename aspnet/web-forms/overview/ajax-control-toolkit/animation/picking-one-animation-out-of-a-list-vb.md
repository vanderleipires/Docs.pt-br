---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Escolhendo uma animação fora de uma lista (VB) | Microsoft Docs
author: wenz
description: O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A estrutura também Perm...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: f2bd1b3cc72595da7e8901786ea8415d7c1c524a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="picking-one-animation-out-of-a-list-vb"></a>Escolhendo uma animação fora de uma lista (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A estrutura também permite que o programador escolher uma animação fora de uma lista de animações, dependendo da avaliação de um código JavaScript.


## <a name="overview"></a>Visão geral

O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A estrutura também permite que o programador escolher uma animação fora de uma lista de animações, dependendo da avaliação de um código JavaScript.

## <a name="steps"></a>Etapas

Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

Na classe CSS associada para o painel, definir uma cor de plano de fundo adequado e também uma largura fixa para o painel:

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

Em seguida, adicione o `AnimationExtender` para a página, fornecendo um `ID`, o `TargetControlID` atributo e o obrigatórias `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

Dentro de `<Animations>` nó, use `<OnLoad>` para executar as animações depois que a página foi totalmente carregada. Em vez de uma das animações regulares, o `<Case>` elemento entra em cena. O valor de seu atributo SelectScript é avaliada; o valor de retorno deve ser numérico. Dependendo desse número, um dos subanimations em &lt;caso&gt; é executado. Por exemplo, se SelectScript for avaliada como 2, o Kit de ferramentas de controle executará a terceira animação em &lt;caso&gt; (contando começa em 0).

A seguinte marcação define três subanimations: redimensionando a largura, a altura de redimensionamento e desaparecimento. O código JavaScript (`Math.floor(3 * Math.random())`), em seguida, escolhe um número entre 0 e 2, para que uma das três animações é executada:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![Uma das três animações possíveis: O painel se estende](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

Uma das três animações possíveis: O painel obtém maior ([clique para exibir a imagem em tamanho normal](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animation-depending-on-a-condition-vb.md)
> [Próximo](animating-in-response-to-user-interaction-vb.md)
