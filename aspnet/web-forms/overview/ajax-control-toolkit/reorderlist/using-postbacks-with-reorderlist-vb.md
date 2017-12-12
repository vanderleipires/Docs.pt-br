---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Com Postbacks ReorderList (VB) | Microsoft Docs
author: wenz
description: "O controle ReorderList no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar. Sempre que a lista é reordenada, uma ordem de compra..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 971d060f2ee69e82ec574392a308754e015b0fd0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-postbacks-with-reorderlist-vb"></a>Usando Postbacks com ReorderList (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> O controle ReorderList no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar. Sempre que a lista é reordenada, um postback deve informar ao servidor da alteração.


## <a name="overview"></a>Visão Geral

O `ReorderList` controle no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar. Sempre que a lista é reordenada, um postback deve informar ao servidor da alteração.

## <a name="steps"></a>Etapas

Há várias fontes de dados possíveis para o `ReorderList` controle. Uma é usar um `XmlDataSource` controle:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Para associar esse XML para um `ReorderList` postbacks de controle e ativar os seguintes atributos devem ser definidos:

- `DataSourceID`: A ID da fonte de dados
- `SortOrderField`: A propriedade classificar por
- `AllowReorder`: Se deseja permitir que o usuário reordenar os elementos de lista
- `PostBackOnReorder`: Se deseja criar uma nova postagem sempre que a lista é reorganizada

Aqui está a marcação apropriada para o controle:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

Dentro de `ReorderList` controle, os dados específicos da fonte de dados pode ser vinculado usando o `Eval()` método:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

Em uma posição arbitrária na página, um rótulo armazenará as informações quando ocorreu a última reordenação:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Este rótulo é preenchido com o texto no código do lado do servidor, a postagem de tratamento:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Por fim, para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado na página:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![Cada reordenação dispara um postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Cada reordenação dispara um postback ([clique para exibir a imagem em tamanho normal](using-postbacks-with-reorderlist-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](drag-and-drop-via-reorderlist-cs.md)
[Próximo](drag-and-drop-via-reorderlist-vb.md)
