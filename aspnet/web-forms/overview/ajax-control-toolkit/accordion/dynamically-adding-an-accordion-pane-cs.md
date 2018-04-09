---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Adicionar dinamicamente um painel Acordeão (c#) | Microsoft Docs
author: wenz
description: O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite ao usuário exibir um por vez. Painéis são normalmente declaradas w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: ad2fc6ea3d527215c0226f3f594d781163d538b5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-adding-an-accordion-pane-c"></a>Adicionar dinamicamente um painel Acordeão (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite ao usuário exibir um por vez. Painéis são geralmente declarados dentro da página em si, mas o código do lado do servidor pode ser usado para alcançar o mesmo resultado.


## <a name="overview"></a>Visão Geral

O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite ao usuário exibir um por vez. Painéis são geralmente declarados dentro da página em si, mas o código do lado do servidor pode ser usado para alcançar o mesmo resultado.

## <a name="steps"></a>Etapas

O controle Accordion expõe todas as propriedades importantes para o código do lado do servidor. Entre outras coisas, o `Panes` propriedade concede acesso à coleção de painéis que compõem o Accordion. Cada painel há do tipo `AccordionPane`. Portanto, é comum para criar um painel tal:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

O `HeaderContainer` propriedade de `AccordionPane` fornece acesso aos controles ASP.NET dentro da seção de cabeçalho do painel; o `ContentContainer` propriedade `AccordionPane` faz o mesmo para a seção de conteúdo do painel. Isso permite que o código do ASP.NET adicionar conteúdo a painéis:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Por fim, os painéis devem ser adicionados para o `Panes` coleção do Accordion:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

Aqui está um código do lado do servidor completo que adiciona dois painéis a um controle Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

O único elemento ausente é Accordion em si, o que depende da presença do ASP.NET `ScriptManager` controle:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Para concluir o exemplo, duas classes CSS referenciadas no controle Accordion fornecem informações de estilo para o navegador:

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![Os dados de accordion dinamicamente foi adicionados pelo código do lado do servidor](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Os dados de accordion dinamicamente foi adicionados pelo código do lado do servidor ([clique para exibir a imagem em tamanho normal](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](databinding-to-an-accordion-cs.md)
> [Próximo](databinding-to-an-accordion-vb.md)
