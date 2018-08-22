---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Criando caixas de seleção mutuamente exclusivas (VB) | Microsoft Docs
author: wenz
description: 'Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção são geralmente usados. Há uma desvantagem, no entanto: quando um botão de opção em um grupo for selecionado,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: fd338b622779b64dd59f9cf6f3e2365ef5cb3ffb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833062"
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a>Criando caixas de seleção mutuamente exclusivas (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção são geralmente usados. Há uma desvantagem, no entanto: quando um botão de opção em um grupo for selecionado, não é possível desmarcar todos os botões de opção. Caixas de seleção pode ser desmarcadas a qualquer momento, porém não são mutuamente exclusivas. Este tutorial fornece o melhor de ambas as abordagens: caixas de seleção que são mutuamente exclusivas.


## <a name="overview"></a>Visão geral

Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção são geralmente usados. Há uma desvantagem, no entanto: quando um botão de opção em um grupo for selecionado, não é possível desmarcar todos os botões de opção. Caixas de seleção pode ser desmarcadas a qualquer momento, porém não são mutuamente exclusivas. Este tutorial fornece o melhor de ambas as abordagens: caixas de seleção que são mutuamente exclusivas.

## <a name="steps"></a>Etapas

O ASP.NET AJAX Control Toolkit contém o extensor MutuallyExclusiveCheckBox. Isso permite que os programadores a atribuir qualquer caixa de seleção a um nome de grupo (`Key` atributo). De todas as caixas de seleção dentro do mesmo grupo, somente um pode ser selecionado ao mesmo tempo.

Vamos começar com a colocação de duas caixas de seleção em uma nova página ASP.NET. Pode haver mais, mas dois deles é suficiente para demonstrar o princípio:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Para ambas as caixas de seleção, um controle MutuallyExclusiveCheckBoxExtender deve ser colocado na página. Ambos os atributos de chave precisam ter o mesmo valor, assim como o valor de atributos de elementos de botão de opção HTML devem ser idênticos para indicar o grupo pertencem. A propriedade TargetControlID do extensor aponta para a ID da caixa de seleção.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Finalmente, inclua o AJAX ASP.NET `ScriptManager` que é necessária para todos os elementos do ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Salve e execute a página: você pode verificar e desmarque as duas caixas de seleção, no entanto em nenhum momento pode ambas as caixas de seleção estar marcadas.


[![Apenas uma caixa de seleção pode ser verificada por vez](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Apenas uma caixa de seleção pode ser verificada por vez ([clique para exibir a imagem em tamanho normal](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](creating-mutually-exclusive-checkboxes-cs.md)
