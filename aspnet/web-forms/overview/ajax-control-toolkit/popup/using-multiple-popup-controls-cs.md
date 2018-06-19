---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Usando vários controles de pop-up (c#) | Microsoft Docs
author: wenz
description: O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Também é possível usar m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 7acd1b53e1b3e3e0d09d248b68941b166da3e81e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869680"
---
<a name="using-multiple-popup-controls-c"></a><span data-ttu-id="223be-104">Usando vários controles de pop-up (c#)</span><span class="sxs-lookup"><span data-stu-id="223be-104">Using Multiple Popup Controls (C#)</span></span>
====================
<span data-ttu-id="223be-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="223be-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="223be-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="223be-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="223be-107">O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado.</span><span class="sxs-lookup"><span data-stu-id="223be-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="223be-108">Também é possível usar mais de um controle de pop-up em uma única página.</span><span class="sxs-lookup"><span data-stu-id="223be-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="223be-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="223be-109">Overview</span></span>

<span data-ttu-id="223be-110">O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado.</span><span class="sxs-lookup"><span data-stu-id="223be-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="223be-111">Também é possível usar mais de um controle de pop-up em uma única página.</span><span class="sxs-lookup"><span data-stu-id="223be-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="223be-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="223be-112">Steps</span></span>

<span data-ttu-id="223be-113">Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="223be-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="223be-114">Em seguida, adicione um painel que serve como o pop-up.</span><span class="sxs-lookup"><span data-stu-id="223be-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="223be-115">No cenário atual, o painel contém um `Calendar` controle.</span><span class="sxs-lookup"><span data-stu-id="223be-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="223be-116">Para evitar as atualizações de página causadas por postagens do calendário, o painel é colocado dentro de um `UpdatePanel` controle:</span><span class="sxs-lookup"><span data-stu-id="223be-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="223be-117">A página também contém duas caixas de texto.</span><span class="sxs-lookup"><span data-stu-id="223be-117">The page also contains two text boxes.</span></span> <span data-ttu-id="223be-118">Para cada caixa de texto, pop-up do calendário deve aparecer depois que a caixa de texto está ativada.</span><span class="sxs-lookup"><span data-stu-id="223be-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="223be-119">Agora estender cada uma das duas caixas de texto com um `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="223be-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="223be-120">O `TargetControlID` atributo fornece a ID do controle associado para o extensor.</span><span class="sxs-lookup"><span data-stu-id="223be-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="223be-121">O `PopupControlID` atributo contém a ID do painel pop-up.</span><span class="sxs-lookup"><span data-stu-id="223be-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="223be-122">Nesse caso, ambos os extensores mostram o mesmo painel, mas diferentes painéis são possíveis, também.</span><span class="sxs-lookup"><span data-stu-id="223be-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="223be-123">Agora sempre que você clicar em um campo de texto, um calendário aparece abaixo do campo, permitindo que você selecione uma data.</span><span class="sxs-lookup"><span data-stu-id="223be-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="223be-124">(Voltar a data selecionada nas caixas de texto será abordado em um tutorial diferente.)</span><span class="sxs-lookup"><span data-stu-id="223be-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="223be-125">[![O calendário será exibido quando o usuário clica na caixa de texto](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="223be-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="223be-126">O calendário será exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="223be-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="223be-127">Avançar</span><span class="sxs-lookup"><span data-stu-id="223be-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
