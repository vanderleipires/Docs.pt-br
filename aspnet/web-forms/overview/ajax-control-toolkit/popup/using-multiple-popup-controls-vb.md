---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Usando vários controles de pop-up (VB) | Microsoft Docs
author: wenz
description: O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Também é possível usar m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c57aab3ecf2c02a8488b5ea4e3e0ed33ac5e7fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869316"
---
<a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="61e54-104">Usando vários controles de pop-up (VB)</span><span class="sxs-lookup"><span data-stu-id="61e54-104">Using Multiple Popup Controls (VB)</span></span>
====================
<span data-ttu-id="61e54-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="61e54-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="61e54-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="61e54-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="61e54-107">O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado.</span><span class="sxs-lookup"><span data-stu-id="61e54-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="61e54-108">Também é possível usar mais de um controle de pop-up em uma única página.</span><span class="sxs-lookup"><span data-stu-id="61e54-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="61e54-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="61e54-109">Overview</span></span>

<span data-ttu-id="61e54-110">O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado.</span><span class="sxs-lookup"><span data-stu-id="61e54-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="61e54-111">Também é possível usar mais de um controle de pop-up em uma única página.</span><span class="sxs-lookup"><span data-stu-id="61e54-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="61e54-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="61e54-112">Steps</span></span>

<span data-ttu-id="61e54-113">Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="61e54-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="61e54-114">Em seguida, adicione um painel que serve como o pop-up.</span><span class="sxs-lookup"><span data-stu-id="61e54-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="61e54-115">No cenário atual, o painel contém um `Calendar` controle.</span><span class="sxs-lookup"><span data-stu-id="61e54-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="61e54-116">Para evitar as atualizações de página causadas por postagens do calendário, o painel é colocado dentro de um `UpdatePanel` controle:</span><span class="sxs-lookup"><span data-stu-id="61e54-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="61e54-117">A página também contém duas caixas de texto.</span><span class="sxs-lookup"><span data-stu-id="61e54-117">The page also contains two text boxes.</span></span> <span data-ttu-id="61e54-118">Para cada caixa de texto, pop-up do calendário deve aparecer depois que a caixa de texto está ativada.</span><span class="sxs-lookup"><span data-stu-id="61e54-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="61e54-119">Agora estender cada uma das duas caixas de texto com um `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="61e54-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="61e54-120">O `TargetControlID` atributo fornece a ID do controle associado para o extensor.</span><span class="sxs-lookup"><span data-stu-id="61e54-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="61e54-121">O `PopupControlID` atributo contém a ID do painel pop-up.</span><span class="sxs-lookup"><span data-stu-id="61e54-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="61e54-122">Nesse caso, ambos os extensores mostram o mesmo painel, mas diferentes painéis são possíveis, também.</span><span class="sxs-lookup"><span data-stu-id="61e54-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="61e54-123">Agora sempre que você clicar em um campo de texto, um calendário aparece abaixo do campo, permitindo que você selecione uma data.</span><span class="sxs-lookup"><span data-stu-id="61e54-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="61e54-124">(Voltar a data selecionada nas caixas de texto será abordado em um tutorial diferente.)</span><span class="sxs-lookup"><span data-stu-id="61e54-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="61e54-125">[![O calendário será exibido quando o usuário clica na caixa de texto](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="61e54-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="61e54-126">O calendário será exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="61e54-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="61e54-127">[Anterior](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Próximo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="61e54-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
