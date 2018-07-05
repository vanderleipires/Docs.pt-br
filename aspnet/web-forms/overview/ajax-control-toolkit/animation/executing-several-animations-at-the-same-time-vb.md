---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Executando várias animações ao mesmo tempo (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele permite para executar severa...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: b748f7f3f8344ff3c87230e26887c761737d4489
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814681"
---
<a name="executing-several-animations-at-the-same-time-vb"></a>Executando várias animações ao mesmo tempo (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele permite para executar várias animações de forma paralela.


## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele permite para executar várias animações de forma paralela.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto que tem esta aparência:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatoriamente `runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

Dentro de `<Animations>` nó, use `<OnLoad>` para as animações são executadas uma vez que a página foi totalmente carregada. Em geral, `<OnLoad>` aceita apenas uma animação. A estrutura de animação permite unir várias animações em um, utilizando o `<Parallel>` elemento. Todas as animações em `<Parallel>` são executadas ao mesmo tempo.

Aqui está a uma marcação possíveis para o `AnimationExtender` controle, o desaparecimento e redimensionar o painel ao mesmo tempo:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

E realmente: quando você executa esse script, o painel é exibido e, em seguida, redimensiona (mais do que threefolding sua largura e halfing sua altura) e fade out ao mesmo tempo.


[![O painel é desaparecimento e redimensionamento (incluindo seu conteúdo, graças ao mecanismo de renderização do navegador)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)

O painel é desaparecimento e redimensionamento (incluindo seu conteúdo, graças ao mecanismo de renderização do navegador) ([clique para exibir a imagem em tamanho normal](executing-several-animations-at-the-same-time-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](adding-animation-to-a-control-vb.md)
> [Próximo](executing-several-animations-after-each-other-vb.md)
