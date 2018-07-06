---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Uso de Postbacks com ReorderList (VB) | Microsoft Docs
author: wenz
description: O controle ReorderList no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar. Sempre que a lista for reordenada, uma OC...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 20214db1c038f417944bd374bdaa8ce55d6d44aa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838993"
---
<a name="using-postbacks-with-reorderlist-vb"></a><span data-ttu-id="1d79e-104">Uso de Postbacks com ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="1d79e-104">Using Postbacks with ReorderList (VB)</span></span>
====================
<span data-ttu-id="1d79e-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1d79e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1d79e-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1d79e-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span></span>

> <span data-ttu-id="1d79e-107">O controle ReorderList no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar.</span><span class="sxs-lookup"><span data-stu-id="1d79e-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="1d79e-108">Sempre que a lista for reordenada, deverá informar o servidor da alteração de um postback.</span><span class="sxs-lookup"><span data-stu-id="1d79e-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="1d79e-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="1d79e-109">Overview</span></span>

<span data-ttu-id="1d79e-110">O `ReorderList` controle no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar.</span><span class="sxs-lookup"><span data-stu-id="1d79e-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="1d79e-111">Sempre que a lista for reordenada, deverá informar o servidor da alteração de um postback.</span><span class="sxs-lookup"><span data-stu-id="1d79e-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="1d79e-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="1d79e-112">Steps</span></span>

<span data-ttu-id="1d79e-113">Há várias fontes de dados possíveis para o `ReorderList` controle.</span><span class="sxs-lookup"><span data-stu-id="1d79e-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="1d79e-114">Uma é usar um `XmlDataSource` controle:</span><span class="sxs-lookup"><span data-stu-id="1d79e-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="1d79e-115">Para associar esse XML para um `ReorderList` postbacks de controle e ativar os seguintes atributos devem ser definidas:</span><span class="sxs-lookup"><span data-stu-id="1d79e-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="1d79e-116">`DataSourceID`: A ID da fonte de dados</span><span class="sxs-lookup"><span data-stu-id="1d79e-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="1d79e-117">`SortOrderField`: A propriedade de classificação</span><span class="sxs-lookup"><span data-stu-id="1d79e-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="1d79e-118">`AllowReorder`: Se desejar permitir que o usuário reordenar os elementos de lista</span><span class="sxs-lookup"><span data-stu-id="1d79e-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="1d79e-119">`PostBackOnReorder`: Se desejar criar uma nova postagem sempre que a lista é reorganizada</span><span class="sxs-lookup"><span data-stu-id="1d79e-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="1d79e-120">Aqui está a marcação apropriada para o controle:</span><span class="sxs-lookup"><span data-stu-id="1d79e-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="1d79e-121">Dentro de `ReorderList` controle, dados específicos da fonte de dados pode ser associado usando o `Eval()` método:</span><span class="sxs-lookup"><span data-stu-id="1d79e-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

<span data-ttu-id="1d79e-122">Em uma posição arbitrária na página, um rótulo manterá as informações quando ocorreu a última reordenação:</span><span class="sxs-lookup"><span data-stu-id="1d79e-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="1d79e-123">Esse rótulo é preenchido com o texto no código do lado do servidor, manipular o postback:</span><span class="sxs-lookup"><span data-stu-id="1d79e-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

<span data-ttu-id="1d79e-124">Por fim, para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada na página:</span><span class="sxs-lookup"><span data-stu-id="1d79e-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


<span data-ttu-id="1d79e-125">[![Cada reordenação dispara um postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1d79e-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="1d79e-126">Cada reordenação dispara um postback ([clique para exibir a imagem em tamanho normal](using-postbacks-with-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1d79e-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1d79e-127">[Anterior](drag-and-drop-via-reorderlist-cs.md)
> [Próximo](drag-and-drop-via-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1d79e-127">[Previous](drag-and-drop-via-reorderlist-cs.md)
[Next](drag-and-drop-via-reorderlist-vb.md)</span></span>
