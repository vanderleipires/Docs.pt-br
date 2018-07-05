---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Tratamento de Postbacks de um controle pop-up com um UpdatePanel (VB) | Microsoft Docs
author: wenz
description: O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um pop-up quando qualquer outro controle é ativado. Tenha cuidado especial a ser executada...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: cf17025219fbf24e749c172f2ddb47ec20980cdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395896"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="726f7-104">Tratamento de Postbacks de um controle pop-up com um UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="726f7-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="726f7-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="726f7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="726f7-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="726f7-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="726f7-107">O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um pop-up quando qualquer outro controle é ativado.</span><span class="sxs-lookup"><span data-stu-id="726f7-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="726f7-108">Tenha cuidado especial a ser executada quando ocorre um postback dentro de tal um pop-up.</span><span class="sxs-lookup"><span data-stu-id="726f7-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="726f7-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="726f7-109">Overview</span></span>

<span data-ttu-id="726f7-110">O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um pop-up quando qualquer outro controle é ativado.</span><span class="sxs-lookup"><span data-stu-id="726f7-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="726f7-111">Tenha cuidado especial a ser executada quando ocorre um postback dentro de tal um pop-up.</span><span class="sxs-lookup"><span data-stu-id="726f7-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="726f7-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="726f7-112">Steps</span></span>

<span data-ttu-id="726f7-113">Ao usar um `PopupControl` com um postback, um `UpdatePanel` pode evitar a atualização de página causada por postback.</span><span class="sxs-lookup"><span data-stu-id="726f7-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="726f7-114">A marcação a seguir define dois elementos importantes:</span><span class="sxs-lookup"><span data-stu-id="726f7-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="726f7-115">Um `ScriptManager` controlar de forma que o ASP.NET AJAX Control Toolkit funciona</span><span class="sxs-lookup"><span data-stu-id="726f7-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="726f7-116">Dois `TextBox` controla quais ambas disparará um pop-up</span><span class="sxs-lookup"><span data-stu-id="726f7-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="726f7-117">Um `Panel` controle que servirá como o pop-up</span><span class="sxs-lookup"><span data-stu-id="726f7-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="726f7-118">Dentro do painel, um `Calendar` controle é inserido em um `UpdatePanel` controle</span><span class="sxs-lookup"><span data-stu-id="726f7-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="726f7-119">Dois `PopupControlExtender` controles que atribui o painel para as caixas de texto</span><span class="sxs-lookup"><span data-stu-id="726f7-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="726f7-120">Observe que o `OnSelectionChanged` atributo do `Calendar` controle está definido.</span><span class="sxs-lookup"><span data-stu-id="726f7-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="726f7-121">Portanto, quando o usuário seleciona uma data no calendário, ocorre um postback e o método do lado do servidor `c1_SelectionChanged()` é executado.</span><span class="sxs-lookup"><span data-stu-id="726f7-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="726f7-122">Dentro desse método, a data atual deve ser recuperada e Write-back para a caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="726f7-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="726f7-123">A sintaxe para o que é da seguinte maneira: em primeiro lugar, um proxy do objeto para o `PopupControlExtender` na página deve ser gerado.</span><span class="sxs-lookup"><span data-stu-id="726f7-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="726f7-124">O ASP.NET AJAX Control Toolkit oferece o `GetProxyForCurrentPopup()` método.</span><span class="sxs-lookup"><span data-stu-id="726f7-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="726f7-125">O objeto retorna este método dá suporte a `Commit()` método que envia um valor de volta para o controle que disparou o pop-up (não o controle que disparou a chamada de método!).</span><span class="sxs-lookup"><span data-stu-id="726f7-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="726f7-126">O código a seguir fornece a data selecionada como o argumento para o `Commit()` método, fazendo com que o código gravar a data selecionada de volta à caixa de texto:</span><span class="sxs-lookup"><span data-stu-id="726f7-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="726f7-127">Agora sempre que você clica em uma data de calendário, a data selecionada aparece na caixa de texto associada, criando um controle de seletor de data que é utilizada em vários sites.</span><span class="sxs-lookup"><span data-stu-id="726f7-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="726f7-128">[![O calendário é exibido quando o usuário clica na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="726f7-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="726f7-129">O calendário é exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="726f7-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="726f7-130">[![Clicar em uma data coloca na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="726f7-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="726f7-131">Clicar em uma data coloca na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="726f7-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="726f7-132">[Anterior](using-multiple-popup-controls-vb.md)
> [Próximo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="726f7-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
