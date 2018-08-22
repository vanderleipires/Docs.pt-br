---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Adição dinâmica de um painel Accordion (VB) | Microsoft Docs
author: wenz
description: O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite que o usuário exibir um por vez. Painéis são normalmente declaradas w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: b0ac0919e64fb6494bd9c7c5f00a5f69274799ed
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823508"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Adição dinâmica de um painel Accordion (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite que o usuário exibir um por vez. Painéis geralmente são declaradas dentro da página em si, mas o código do lado do servidor pode ser usado para alcançar o mesmo resultado.


## <a name="overview"></a>Visão geral

O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite que o usuário exibir um por vez. Painéis geralmente são declaradas dentro da página em si, mas o código do lado do servidor pode ser usado para alcançar o mesmo resultado.

## <a name="steps"></a>Etapas

O controle Accordion expõe todas as propriedades importantes para o código do lado do servidor. Entre outras coisas, o `Panes` propriedade concede acesso à coleção de painéis que compõem o Accordion. Cada painel lá é do tipo `AccordionPane`. Portanto, é trivial criar tal um painel:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

O `HeaderContainer` propriedade de `AccordionPane` fornece acesso aos controles ASP.NET dentro da seção de cabeçalho do painel; a `ContentContainer` propriedade do `AccordionPane` faz o mesmo para a seção de conteúdo do painel. Isso permite que o código ASP.NET para adicionar conteúdo para os painéis:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Por fim, os painéis devem ser adicionados para o `Panes` coleção de Acordeão:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Aqui está um código do lado do servidor completo que adiciona dois painéis a um controle Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

O único elemento ausente é Accordion em si, que depende da presença do ASP.NET `ScriptManager` controle:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Para concluir o exemplo, as duas classes CSS referenciadas no controle Accordion fornecem informações de estilo para o navegador:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Os dados em um acordeão dinamicamente foi adicionados pelo código do lado do servidor](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Os dados em um acordeão dinamicamente foi adicionados pelo código do lado do servidor ([clique para exibir a imagem em tamanho normal](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](databinding-to-an-accordion-vb.md)
