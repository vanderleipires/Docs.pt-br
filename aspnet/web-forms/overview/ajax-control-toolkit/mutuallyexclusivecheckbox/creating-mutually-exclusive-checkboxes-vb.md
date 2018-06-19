---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Criando caixas de seleção mutuamente (VB) | Microsoft Docs
author: wenz
description: 'Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção são normalmente usadas. Há uma desvantagem, embora: quando um botão de opção em um grupo é selecionada,...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: cdb93a080fb62cfdc7e3ff0604a1447e2bb99071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874246"
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a>Criando caixas de seleção mutuamente (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção são normalmente usadas. Há uma desvantagem, embora: quando um botão de opção em um grupo é selecionada, não é possível desmarcar todos os botões de opção. Caixas de seleção pode ser desmarcadas a qualquer momento, porém não são mutuamente exclusivas. Este tutorial fornece o melhor de ambas as abordagens: caixas de seleção são mutuamente exclusivas.


## <a name="overview"></a>Visão geral

Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção são normalmente usadas. Há uma desvantagem, embora: quando um botão de opção em um grupo é selecionada, não é possível desmarcar todos os botões de opção. Caixas de seleção pode ser desmarcadas a qualquer momento, porém não são mutuamente exclusivas. Este tutorial fornece o melhor de ambas as abordagens: caixas de seleção são mutuamente exclusivas.

## <a name="steps"></a>Etapas

O Kit de ferramentas de controle do ASP.NET AJAX contém o extensor MutuallyExclusiveCheckBox. Isso permite aos programadores atribuir qualquer caixa de seleção para um nome de grupo (`Key` atributo). Todas as caixas de seleção dentro do mesmo grupo, somente um pode ser selecionado por vez.

Vamos começar com colocar duas caixas de seleção em uma nova página ASP.NET. Pode haver mais, mas duas delas é suficiente para demonstrar o princípio:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Para ambas as caixas de seleção, um controle MutuallyExclusiveCheckBoxExtender deve ser colocado na página. Ambos os atributos de chave precisam ter o mesmo valor, assim como o valor de atributos de elementos de botão de opção HTML devem ser idênticos para indicar o grupo que pertencem a. A propriedade TargetControlID do extensor aponta para a ID da caixa de seleção.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Por fim, incluem o ASP.NET AJAX `ScriptManager` que é necessário para todos os elementos do Kit de ferramentas de controle AJAX ASP.NET:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Salve e execute a página: você pode verificar e desmarque as duas caixas de seleção, porém em nenhum momento pode ambas as caixas de seleção ser verificadas.


[![Apenas uma caixa de seleção pode ser verificada por vez](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Apenas uma caixa de seleção pode ser verificada por vez ([clique para exibir a imagem em tamanho normal](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](creating-mutually-exclusive-checkboxes-cs.md)
