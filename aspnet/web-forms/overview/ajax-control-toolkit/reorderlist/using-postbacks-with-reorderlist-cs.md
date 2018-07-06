---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Uso de Postbacks com ReorderList (c#) | Microsoft Docs
author: wenz
description: O controle ReorderList no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar. Sempre que a lista for reordenada, uma OC...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 4510adc4ecf6928863b035d0afe8d008968d25b0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820098"
---
<a name="using-postbacks-with-reorderlist-c"></a>Uso de Postbacks com ReorderList (c#)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> O controle ReorderList no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar. Sempre que a lista for reordenada, deverá informar o servidor da alteração de um postback.


## <a name="overview"></a>Visão geral

O `ReorderList` controle no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar. Sempre que a lista for reordenada, deverá informar o servidor da alteração de um postback.

## <a name="steps"></a>Etapas

Há várias fontes de dados possíveis para o `ReorderList` controle. Uma é usar um `XmlDataSource` controle:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

Para associar esse XML para um `ReorderList` postbacks de controle e ativar os seguintes atributos devem ser definidas:

- `DataSourceID`: A ID da fonte de dados
- `SortOrderField`: A propriedade de classificação
- `AllowReorder`: Se desejar permitir que o usuário reordenar os elementos de lista
- `PostBackOnReorder`: Se desejar criar uma nova postagem sempre que a lista é reorganizada

Aqui está a marcação apropriada para o controle:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

Dentro de `ReorderList` controle, dados específicos da fonte de dados pode ser associado usando o `Eval()` método:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

Em uma posição arbitrária na página, um rótulo manterá as informações quando ocorreu a última reordenação:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

Esse rótulo é preenchido com o texto no código do lado do servidor, manipular o postback:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

Por fim, para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada na página:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![Cada reordenação dispara um postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

Cada reordenação dispara um postback ([clique para exibir a imagem em tamanho normal](using-postbacks-with-reorderlist-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Avançar](drag-and-drop-via-reorderlist-cs.md)
