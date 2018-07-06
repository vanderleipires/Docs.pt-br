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
<a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="c167a-104">Uso de Postbacks com ReorderList (c#)</span><span class="sxs-lookup"><span data-stu-id="c167a-104">Using Postbacks with ReorderList (C#)</span></span>
====================
<span data-ttu-id="c167a-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c167a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c167a-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c167a-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="c167a-107">O controle ReorderList no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar.</span><span class="sxs-lookup"><span data-stu-id="c167a-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="c167a-108">Sempre que a lista for reordenada, deverá informar o servidor da alteração de um postback.</span><span class="sxs-lookup"><span data-stu-id="c167a-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="c167a-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="c167a-109">Overview</span></span>

<span data-ttu-id="c167a-110">O `ReorderList` controle no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar.</span><span class="sxs-lookup"><span data-stu-id="c167a-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="c167a-111">Sempre que a lista for reordenada, deverá informar o servidor da alteração de um postback.</span><span class="sxs-lookup"><span data-stu-id="c167a-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="c167a-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="c167a-112">Steps</span></span>

<span data-ttu-id="c167a-113">Há várias fontes de dados possíveis para o `ReorderList` controle.</span><span class="sxs-lookup"><span data-stu-id="c167a-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="c167a-114">Uma é usar um `XmlDataSource` controle:</span><span class="sxs-lookup"><span data-stu-id="c167a-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="c167a-115">Para associar esse XML para um `ReorderList` postbacks de controle e ativar os seguintes atributos devem ser definidas:</span><span class="sxs-lookup"><span data-stu-id="c167a-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="c167a-116">`DataSourceID`: A ID da fonte de dados</span><span class="sxs-lookup"><span data-stu-id="c167a-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="c167a-117">`SortOrderField`: A propriedade de classificação</span><span class="sxs-lookup"><span data-stu-id="c167a-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="c167a-118">`AllowReorder`: Se desejar permitir que o usuário reordenar os elementos de lista</span><span class="sxs-lookup"><span data-stu-id="c167a-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="c167a-119">`PostBackOnReorder`: Se desejar criar uma nova postagem sempre que a lista é reorganizada</span><span class="sxs-lookup"><span data-stu-id="c167a-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="c167a-120">Aqui está a marcação apropriada para o controle:</span><span class="sxs-lookup"><span data-stu-id="c167a-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="c167a-121">Dentro de `ReorderList` controle, dados específicos da fonte de dados pode ser associado usando o `Eval()` método:</span><span class="sxs-lookup"><span data-stu-id="c167a-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="c167a-122">Em uma posição arbitrária na página, um rótulo manterá as informações quando ocorreu a última reordenação:</span><span class="sxs-lookup"><span data-stu-id="c167a-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="c167a-123">Esse rótulo é preenchido com o texto no código do lado do servidor, manipular o postback:</span><span class="sxs-lookup"><span data-stu-id="c167a-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="c167a-124">Por fim, para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada na página:</span><span class="sxs-lookup"><span data-stu-id="c167a-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


<span data-ttu-id="c167a-125">[![Cada reordenação dispara um postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c167a-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="c167a-126">Cada reordenação dispara um postback ([clique para exibir a imagem em tamanho normal](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c167a-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c167a-127">Avançar</span><span class="sxs-lookup"><span data-stu-id="c167a-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
