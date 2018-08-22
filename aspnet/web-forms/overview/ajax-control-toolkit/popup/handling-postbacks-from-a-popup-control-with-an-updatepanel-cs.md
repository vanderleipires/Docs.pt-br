---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Tratamento de Postbacks de um controle pop-up com um UpdatePanel (c#) | Microsoft Docs
author: wenz
description: O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um pop-up quando qualquer outro controle é ativado. Tenha cuidado especial a ser executada...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 0f886ba0f3e79bc6d5daf193eaedfd627bc9b937
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830413"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="40fbf-104">Tratamento de Postbacks de um controle pop-up com um UpdatePanel (c#)</span><span class="sxs-lookup"><span data-stu-id="40fbf-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="40fbf-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="40fbf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="40fbf-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="40fbf-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="40fbf-107">O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um pop-up quando qualquer outro controle é ativado.</span><span class="sxs-lookup"><span data-stu-id="40fbf-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="40fbf-108">Tenha cuidado especial a ser executada quando ocorre um postback dentro de tal um pop-up.</span><span class="sxs-lookup"><span data-stu-id="40fbf-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="40fbf-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="40fbf-109">Overview</span></span>

<span data-ttu-id="40fbf-110">O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um pop-up quando qualquer outro controle é ativado.</span><span class="sxs-lookup"><span data-stu-id="40fbf-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="40fbf-111">Tenha cuidado especial a ser executada quando ocorre um postback dentro de tal um pop-up.</span><span class="sxs-lookup"><span data-stu-id="40fbf-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="40fbf-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="40fbf-112">Steps</span></span>

<span data-ttu-id="40fbf-113">Ao usar um `PopupControl` com um postback, um `UpdatePanel` pode evitar a atualização de página causada por postback.</span><span class="sxs-lookup"><span data-stu-id="40fbf-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="40fbf-114">A marcação a seguir define dois elementos importantes:</span><span class="sxs-lookup"><span data-stu-id="40fbf-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="40fbf-115">Um `ScriptManager` controlar de forma que o ASP.NET AJAX Control Toolkit funciona</span><span class="sxs-lookup"><span data-stu-id="40fbf-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="40fbf-116">Dois `TextBox` controla quais ambas disparará um pop-up</span><span class="sxs-lookup"><span data-stu-id="40fbf-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="40fbf-117">Um `Panel` controle que servirá como o pop-up</span><span class="sxs-lookup"><span data-stu-id="40fbf-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="40fbf-118">Dentro do painel, um `Calendar` controle é inserido em um `UpdatePanel` controle</span><span class="sxs-lookup"><span data-stu-id="40fbf-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="40fbf-119">Dois `PopupControlExtender` controles que atribui o painel para as caixas de texto</span><span class="sxs-lookup"><span data-stu-id="40fbf-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="40fbf-120">Observe que o `OnSelectionChanged` atributo do `Calendar` controle está definido.</span><span class="sxs-lookup"><span data-stu-id="40fbf-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="40fbf-121">Portanto, quando o usuário seleciona uma data no calendário, ocorre um postback e o método do lado do servidor `c1_SelectionChanged()` é executado.</span><span class="sxs-lookup"><span data-stu-id="40fbf-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="40fbf-122">Dentro desse método, a data atual deve ser recuperada e Write-back para a caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="40fbf-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="40fbf-123">A sintaxe para o que é da seguinte maneira: em primeiro lugar, um proxy do objeto para o `PopupControlExtender` na página deve ser gerado.</span><span class="sxs-lookup"><span data-stu-id="40fbf-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="40fbf-124">O ASP.NET AJAX Control Toolkit oferece o `GetProxyForCurrentPopup()` método.</span><span class="sxs-lookup"><span data-stu-id="40fbf-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="40fbf-125">O objeto retorna este método dá suporte a `Commit()` método que envia um valor de volta para o controle que disparou o pop-up (não o controle que disparou a chamada de método!).</span><span class="sxs-lookup"><span data-stu-id="40fbf-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="40fbf-126">O código a seguir fornece a data selecionada como o argumento para o `Commit()` método, fazendo com que o código gravar a data selecionada de volta à caixa de texto:</span><span class="sxs-lookup"><span data-stu-id="40fbf-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="40fbf-127">Agora sempre que você clica em uma data de calendário, a data selecionada aparece na caixa de texto associada, criando um controle de seletor de data que é utilizada em vários sites.</span><span class="sxs-lookup"><span data-stu-id="40fbf-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="40fbf-128">[![O calendário é exibido quando o usuário clica na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="40fbf-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="40fbf-129">O calendário é exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="40fbf-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="40fbf-130">[![Clicar em uma data coloca na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="40fbf-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="40fbf-131">Clicar em uma data coloca na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="40fbf-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="40fbf-132">[Anterior](using-multiple-popup-controls-cs.md)
> [Próximo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="40fbf-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
