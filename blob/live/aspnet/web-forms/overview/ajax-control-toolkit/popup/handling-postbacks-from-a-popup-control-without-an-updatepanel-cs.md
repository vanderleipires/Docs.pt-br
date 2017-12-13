---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Tratamento de postagens de um controle Popup sem um UpdatePanel (c#) | Microsoft Docs
author: wenz
description: "O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado. Quando ocorre um postback no su..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: df43052950b6186908fe1baf04808f40cb926f69
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="54ac2-104">Tratamento de postagens de um controle Popup sem um UpdatePanel (c#)</span><span class="sxs-lookup"><span data-stu-id="54ac2-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="54ac2-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="54ac2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="54ac2-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="54ac2-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="54ac2-107">O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado.</span><span class="sxs-lookup"><span data-stu-id="54ac2-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="54ac2-108">Quando ocorre um postback em tal um painel e há vários painéis na página é difícil determinar qual painel foi clicado.</span><span class="sxs-lookup"><span data-stu-id="54ac2-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="54ac2-109">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="54ac2-109">Overview</span></span>

<span data-ttu-id="54ac2-110">O extensor PopupControl no Kit de ferramentas de controle AJAX oferece uma maneira fácil para disparar um pop-up quando qualquer outro controle está ativado.</span><span class="sxs-lookup"><span data-stu-id="54ac2-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="54ac2-111">Quando ocorre um postback em tal um painel e há vários painéis na página é difícil determinar qual painel foi clicado.</span><span class="sxs-lookup"><span data-stu-id="54ac2-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="54ac2-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="54ac2-112">Steps</span></span>

<span data-ttu-id="54ac2-113">Ao usar um `PopupControl` com um postback, mas sem ter um `UpdatePanel` na página, o Kit de ferramentas de controle não oferece uma maneira de determinar qual elemento cliente acionou o pop-up que por sua vez causou o postback.</span><span class="sxs-lookup"><span data-stu-id="54ac2-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="54ac2-114">No entanto, um pequeno truque fornece uma solução alternativa para esse cenário.</span><span class="sxs-lookup"><span data-stu-id="54ac2-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="54ac2-115">Em primeiro lugar, aqui está a configuração básica: duas caixas de texto que disparam o mesmo pop-up, um calendário.</span><span class="sxs-lookup"><span data-stu-id="54ac2-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="54ac2-116">Dois `PopupControlExtenders` reunir caixas de texto e pop-up.</span><span class="sxs-lookup"><span data-stu-id="54ac2-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="54ac2-117">A ideia básica é adicionar um campo de formulário oculto o &lt; `form` &gt; elemento que contém a caixa de texto que iniciou o pop-up:</span><span class="sxs-lookup"><span data-stu-id="54ac2-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="54ac2-118">Quando a página for carregada, o código JavaScript adiciona um manipulador de eventos para ambas as caixas de texto: sempre que o usuário clica em uma caixa de texto, seu nome será gravado no campo oculto do formulário:</span><span class="sxs-lookup"><span data-stu-id="54ac2-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="54ac2-119">No código do lado do servidor, o valor do campo oculto deve ser lidos.</span><span class="sxs-lookup"><span data-stu-id="54ac2-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="54ac2-120">Como os campos ocultos do formulário são triviais para manipular, uma abordagem de lista branca para validar o valor de hidden é necessária.</span><span class="sxs-lookup"><span data-stu-id="54ac2-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="54ac2-121">Depois que a caixa de texto correto foi identificada, a data do calendário é gravada nele.</span><span class="sxs-lookup"><span data-stu-id="54ac2-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


<span data-ttu-id="54ac2-122">[![O calendário será exibido quando o usuário clica na caixa de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="54ac2-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="54ac2-123">O calendário será exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="54ac2-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="54ac2-124">[![Clicar em uma data coloca na caixa de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="54ac2-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="54ac2-125">Clicar em uma data coloca na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="54ac2-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="54ac2-126">[Anterior](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Próximo](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="54ac2-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
