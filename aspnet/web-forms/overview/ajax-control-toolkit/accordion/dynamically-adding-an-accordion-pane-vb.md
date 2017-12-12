---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: "Adicionar dinamicamente um painel Acordeão (VB) | Microsoft Docs"
author: wenz
description: "O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite ao usuário exibir um por vez. Painéis são normalmente declaradas w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 6adc0b7985e5ba3da1684b645ae1b71b5112594a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Adicionar dinamicamente um painel Acordeão (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite ao usuário exibir um por vez. Painéis são geralmente declarados dentro da página em si, mas o código do lado do servidor pode ser usado para alcançar o mesmo resultado.


## <a name="overview"></a>Visão Geral

O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite ao usuário exibir um por vez. Painéis são geralmente declarados dentro da página em si, mas o código do lado do servidor pode ser usado para alcançar o mesmo resultado.

## <a name="steps"></a>Etapas

O controle Accordion expõe todas as propriedades importantes para o código do lado do servidor. Entre outras coisas, o `Panes` propriedade concede acesso à coleção de painéis que compõem o Accordion. Cada painel há do tipo `AccordionPane`. Portanto, é comum para criar um painel tal:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

O `HeaderContainer` propriedade de `AccordionPane` fornece acesso aos controles ASP.NET dentro da seção de cabeçalho do painel; o `ContentContainer` propriedade `AccordionPane` faz o mesmo para a seção de conteúdo do painel. Isso permite que o código do ASP.NET adicionar conteúdo a painéis:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Por fim, os painéis devem ser adicionados para o `Panes` coleção do Accordion:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Aqui está um código do lado do servidor completo que adiciona dois painéis a um controle Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

O único elemento ausente é Accordion em si, o que depende da presença do ASP.NET `ScriptManager` controle:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Para concluir o exemplo, duas classes CSS referenciadas no controle Accordion fornecem informações de estilo para o navegador:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Os dados de accordion dinamicamente foi adicionados pelo código do lado do servidor](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Os dados de accordion dinamicamente foi adicionados pelo código do lado do servidor ([clique para exibir a imagem em tamanho normal](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](databinding-to-an-accordion-vb.md)
