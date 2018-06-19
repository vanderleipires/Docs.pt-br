---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Tratamento de postagens de um controle pop-up com um UpdatePanel (c#) | Microsoft Docs
author: wenz
description: O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Tenha cuidado especial a ser executada...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: abedb5247f710b02752651a7bfb011ab63d32844
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879628"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="ad0ef-104">Tratamento de postagens de um controle pop-up com um UpdatePanel (c#)</span><span class="sxs-lookup"><span data-stu-id="ad0ef-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="ad0ef-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ad0ef-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ad0ef-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ad0ef-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="ad0ef-107">O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado.</span><span class="sxs-lookup"><span data-stu-id="ad0ef-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="ad0ef-108">Cuidado especial precisa ser executada quando ocorre um postback dentro de tal um pop-up.</span><span class="sxs-lookup"><span data-stu-id="ad0ef-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="ad0ef-109">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="ad0ef-109">Overview</span></span>

<span data-ttu-id="ad0ef-110">O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado.</span><span class="sxs-lookup"><span data-stu-id="ad0ef-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="ad0ef-111">Cuidado especial precisa ser executada quando ocorre um postback dentro de tal um pop-up.</span><span class="sxs-lookup"><span data-stu-id="ad0ef-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="ad0ef-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="ad0ef-112">Steps</span></span>

<span data-ttu-id="ad0ef-113">Ao usar um `PopupControl` com um postback, um `UpdatePanel` pode impedir que a atualização de página causada por postback.</span><span class="sxs-lookup"><span data-stu-id="ad0ef-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="ad0ef-114">A marcação a seguir define alguns dos elementos importantes:</span><span class="sxs-lookup"><span data-stu-id="ad0ef-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="ad0ef-115">Um `ScriptManager` controlar de forma que o ASP.NET AJAX Control Toolkit funciona</span><span class="sxs-lookup"><span data-stu-id="ad0ef-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="ad0ef-116">Dois `TextBox` controla quais ambos dispararão um pop-up</span><span class="sxs-lookup"><span data-stu-id="ad0ef-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="ad0ef-117">Um `Panel` controle que servirá como o pop-up</span><span class="sxs-lookup"><span data-stu-id="ad0ef-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="ad0ef-118">No painel de um `Calendar` controle é inserido em um `UpdatePanel` controle</span><span class="sxs-lookup"><span data-stu-id="ad0ef-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="ad0ef-119">Dois `PopupControlExtender` controles que atribui o painel para as caixas de texto</span><span class="sxs-lookup"><span data-stu-id="ad0ef-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="ad0ef-120">Observe que o `OnSelectionChanged` atributo o `Calendar` controle está definido.</span><span class="sxs-lookup"><span data-stu-id="ad0ef-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="ad0ef-121">Portanto, quando o usuário seleciona uma data no calendário, ocorre um postback e o método do lado do servidor `c1_SelectionChanged()` é executado.</span><span class="sxs-lookup"><span data-stu-id="ad0ef-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="ad0ef-122">Dentro desse método, a data atual deve ser recuperada e gravada de volta para a caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="ad0ef-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="ad0ef-123">A sintaxe para o que é o seguinte: primeiro, um proxy do objeto para o `PopupControlExtender` na página deve ser gerado.</span><span class="sxs-lookup"><span data-stu-id="ad0ef-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="ad0ef-124">O Kit de ferramentas de controle do ASP.NET AJAX oferece o `GetProxyForCurrentPopup()` método.</span><span class="sxs-lookup"><span data-stu-id="ad0ef-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="ad0ef-125">O objeto retorna este método oferece suporte a `Commit()` método que envia um valor para o controle que disparou o pop-up (não o controle que disparou a chamada do método!).</span><span class="sxs-lookup"><span data-stu-id="ad0ef-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="ad0ef-126">O código a seguir fornece a data selecionada como o argumento para o `Commit()` método, fazendo com que o código para gravar as datas selecionadas de volta à caixa de texto:</span><span class="sxs-lookup"><span data-stu-id="ad0ef-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="ad0ef-127">Agora sempre que você clicar em uma data do calendário, a data selecionada aparece na caixa de texto associada, criando um controle de seletor de data que é utilizada em vários sites.</span><span class="sxs-lookup"><span data-stu-id="ad0ef-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="ad0ef-128">[![O calendário será exibido quando o usuário clica na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ad0ef-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="ad0ef-129">O calendário será exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ad0ef-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="ad0ef-130">[![Clicar em uma data coloca na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="ad0ef-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="ad0ef-131">Clicar em uma data coloca na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ad0ef-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ad0ef-132">[Anterior](using-multiple-popup-controls-cs.md)
> [Próximo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ad0ef-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
