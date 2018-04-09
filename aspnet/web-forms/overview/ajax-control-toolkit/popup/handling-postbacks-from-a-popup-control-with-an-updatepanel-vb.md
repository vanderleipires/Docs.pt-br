---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Tratamento de postagens de um controle pop-up com um UpdatePanel (VB) | Microsoft Docs
author: wenz
description: O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Tenha cuidado especial a ser executada...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 312927e01911ea705d0614eac462cd034820c7b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="bc890-104">Tratamento de postagens de um controle pop-up com um UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="bc890-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="bc890-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bc890-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bc890-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="bc890-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="bc890-107">O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado.</span><span class="sxs-lookup"><span data-stu-id="bc890-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="bc890-108">Cuidado especial precisa ser executada quando ocorre um postback dentro de tal um pop-up.</span><span class="sxs-lookup"><span data-stu-id="bc890-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="bc890-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="bc890-109">Overview</span></span>

<span data-ttu-id="bc890-110">O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado.</span><span class="sxs-lookup"><span data-stu-id="bc890-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="bc890-111">Cuidado especial precisa ser executada quando ocorre um postback dentro de tal um pop-up.</span><span class="sxs-lookup"><span data-stu-id="bc890-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="bc890-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="bc890-112">Steps</span></span>

<span data-ttu-id="bc890-113">Ao usar um `PopupControl` com um postback, um `UpdatePanel` pode impedir que a atualização de página causada por postback.</span><span class="sxs-lookup"><span data-stu-id="bc890-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="bc890-114">A marcação a seguir define alguns dos elementos importantes:</span><span class="sxs-lookup"><span data-stu-id="bc890-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="bc890-115">Um `ScriptManager` controlar de forma que o ASP.NET AJAX Control Toolkit funciona</span><span class="sxs-lookup"><span data-stu-id="bc890-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="bc890-116">Dois `TextBox` controla quais ambos dispararão um pop-up</span><span class="sxs-lookup"><span data-stu-id="bc890-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="bc890-117">Um `Panel` controle que servirá como o pop-up</span><span class="sxs-lookup"><span data-stu-id="bc890-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="bc890-118">No painel de um `Calendar` controle é inserido em um `UpdatePanel` controle</span><span class="sxs-lookup"><span data-stu-id="bc890-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="bc890-119">Dois `PopupControlExtender` controles que atribui o painel para as caixas de texto</span><span class="sxs-lookup"><span data-stu-id="bc890-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="bc890-120">Observe que o `OnSelectionChanged` atributo o `Calendar` controle está definido.</span><span class="sxs-lookup"><span data-stu-id="bc890-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="bc890-121">Portanto, quando o usuário seleciona uma data no calendário, ocorre um postback e o método do lado do servidor `c1_SelectionChanged()` é executado.</span><span class="sxs-lookup"><span data-stu-id="bc890-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="bc890-122">Dentro desse método, a data atual deve ser recuperada e gravada de volta para a caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="bc890-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="bc890-123">A sintaxe para o que é o seguinte: primeiro, um proxy do objeto para o `PopupControlExtender` na página deve ser gerado.</span><span class="sxs-lookup"><span data-stu-id="bc890-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="bc890-124">O Kit de ferramentas de controle do ASP.NET AJAX oferece o `GetProxyForCurrentPopup()` método.</span><span class="sxs-lookup"><span data-stu-id="bc890-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="bc890-125">O objeto retorna este método oferece suporte a `Commit()` método que envia um valor para o controle que disparou o pop-up (não o controle que disparou a chamada do método!).</span><span class="sxs-lookup"><span data-stu-id="bc890-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="bc890-126">O código a seguir fornece a data selecionada como o argumento para o `Commit()` método, fazendo com que o código para gravar as datas selecionadas de volta à caixa de texto:</span><span class="sxs-lookup"><span data-stu-id="bc890-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="bc890-127">Agora sempre que você clicar em uma data do calendário, a data selecionada aparece na caixa de texto associada, criando um controle de seletor de data que é utilizada em vários sites.</span><span class="sxs-lookup"><span data-stu-id="bc890-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="bc890-128">[![O calendário será exibido quando o usuário clica na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bc890-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="bc890-129">O calendário será exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bc890-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="bc890-130">[![Clicar em uma data coloca na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="bc890-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="bc890-131">Clicar em uma data coloca na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="bc890-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bc890-132">[Anterior](using-multiple-popup-controls-vb.md)
> [Próximo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="bc890-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
