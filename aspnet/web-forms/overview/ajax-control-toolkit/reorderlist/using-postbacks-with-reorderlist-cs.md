---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Com Postbacks ReorderList (c#) | Microsoft Docs
author: wenz
description: O controle ReorderList no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar. Sempre que a lista é reordenada, uma ordem de compra...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: ed01c30c0721c8f1cd8ccb3fea0735ea8fa4f0a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-postbacks-with-reorderlist-c"></a>Usando Postbacks com ReorderList (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> O controle ReorderList no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar. Sempre que a lista é reordenada, um postback deve informar ao servidor da alteração.


## <a name="overview"></a>Visão geral

O `ReorderList` controle no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar. Sempre que a lista é reordenada, um postback deve informar ao servidor da alteração.

## <a name="steps"></a>Etapas

Há várias fontes de dados possíveis para o `ReorderList` controle. Uma é usar um `XmlDataSource` controle:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

Para associar esse XML para um `ReorderList` postbacks de controle e ativar os seguintes atributos devem ser definidos:

- `DataSourceID`: A ID da fonte de dados
- `SortOrderField`: A propriedade classificar por
- `AllowReorder`: Se deseja permitir que o usuário reordenar os elementos de lista
- `PostBackOnReorder`: Se deseja criar uma nova postagem sempre que a lista é reorganizada

Aqui está a marcação apropriada para o controle:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

Dentro de `ReorderList` controle, os dados específicos da fonte de dados pode ser vinculado usando o `Eval()` método:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

Em uma posição arbitrária na página, um rótulo armazenará as informações quando ocorreu a última reordenação:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

Este rótulo é preenchido com o texto no código do lado do servidor, a postagem de tratamento:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

Por fim, para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado na página:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![Cada reordenação dispara um postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

Cada reordenação dispara um postback ([clique para exibir a imagem em tamanho normal](using-postbacks-with-reorderlist-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Avançar](drag-and-drop-via-reorderlist-cs.md)
