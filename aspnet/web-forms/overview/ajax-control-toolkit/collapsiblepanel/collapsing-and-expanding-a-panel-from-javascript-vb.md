---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Recolher e expandir um painel de JavaScript (VB) | Microsoft Docs
author: wenz
description: O controle de CollapsiblePanel no ASP.NET AJAX Control Toolkit estende um painel e fornece a capacidade de seu conteúdo de recolher e expandi-lo um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 5cf61cd0d8204a5405ba62cd3884d66ccb21968b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873112"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a><span data-ttu-id="2693c-103">Recolher e expandir um painel de JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="2693c-103">Collapsing and Expanding a Panel from JavaScript (VB)</span></span>
====================
<span data-ttu-id="2693c-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2693c-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2693c-105">[Baixar o código](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2693c-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span></span>

> <span data-ttu-id="2693c-106">O controle CollapsiblePanel no ASP.NET AJAX Control Toolkit estende um painel e fornece a capacidade de seu conteúdo de recolher e expandi-lo novamente.</span><span class="sxs-lookup"><span data-stu-id="2693c-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="2693c-107">Essas duas ações também podem ser disparadas de código JavaScript personalizado.</span><span class="sxs-lookup"><span data-stu-id="2693c-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="2693c-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="2693c-108">Overview</span></span>

<span data-ttu-id="2693c-109">O controle CollapsiblePanel no ASP.NET AJAX Control Toolkit estende um painel e fornece a capacidade de seu conteúdo de recolher e expandi-lo novamente.</span><span class="sxs-lookup"><span data-stu-id="2693c-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="2693c-110">Essas duas ações também podem ser disparadas de código JavaScript personalizado.</span><span class="sxs-lookup"><span data-stu-id="2693c-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="2693c-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="2693c-111">Steps</span></span>

<span data-ttu-id="2693c-112">Primeiro, crie uma nova página ASP.NET e incluem o `ScriptManager` dentro de um `<form>` elemento.</span><span class="sxs-lookup"><span data-stu-id="2693c-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="2693c-113">Isso carrega a biblioteca ASP.NET AJAX, que é necessária para o Kit de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="2693c-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="2693c-114">Em seguida, crie um painel com algum texto para que o efeito de recolher/expandir pode ser visto:</span><span class="sxs-lookup"><span data-stu-id="2693c-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="2693c-115">Como você pode ver, o painel faz referência a uma classe CSS que é mostrada aqui (e, basicamente, define uma cor de plano de fundo e a largura do painel):</span><span class="sxs-lookup"><span data-stu-id="2693c-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

<span data-ttu-id="2693c-116">O `CollapsiblePanelExtender` controle requer o `TargetControlID` atributo para que o Kit de ferramentas saiba qual painel para recolher ou expandir mediante solicitação:</span><span class="sxs-lookup"><span data-stu-id="2693c-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

<span data-ttu-id="2693c-117">Infelizmente, o extensor atualmente não expõe uma API específica para recolher ou expandir o painel, mas farão alguns métodos não documentados.</span><span class="sxs-lookup"><span data-stu-id="2693c-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="2693c-118">Em primeiro lugar, adicione três botões HTML para a página que, em seguida, irá disparar o JavaScript do lado do cliente para recolher ou expandir o conteúdo do painel:</span><span class="sxs-lookup"><span data-stu-id="2693c-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="2693c-119">No código JavaScript do lado do cliente (Introdução ao `<script type="text/javascript">`), o `$find()` método precisa ser usada para acessar o `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="2693c-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="2693c-120">`$find("cpe")` Retorna uma referência a ele.</span><span class="sxs-lookup"><span data-stu-id="2693c-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="2693c-121">A partir daí, os métodos específicos resolverá a tarefa em questão.</span><span class="sxs-lookup"><span data-stu-id="2693c-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="2693c-122">O método de abertura (expandir) o painel é chamado `_doOpen()`; o código a seguir implementa a `doOpen()` função é chamada quando o primeiro botão é clicado:</span><span class="sxs-lookup"><span data-stu-id="2693c-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

<span data-ttu-id="2693c-123">Para fechar ou recolher o painel, o `_doClose()` método precisa ser executado.</span><span class="sxs-lookup"><span data-stu-id="2693c-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="2693c-124">Quando o usuário clica no segundo botão, o seguinte código JavaScript é chamado:</span><span class="sxs-lookup"><span data-stu-id="2693c-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

<span data-ttu-id="2693c-125">O terceiro botão alterna o estado do painel: de recolhido expandida e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="2693c-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="2693c-126">O `CollapsiblePanelExtender` expõe o `toggle()` método que faz exatamente isso: inverte o estado do painel.</span><span class="sxs-lookup"><span data-stu-id="2693c-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="2693c-127">No entanto também há outra abordagem (que é usado internamente pelo `toggle()` método): O `get_Collapsed()` método do `CollapsiblePanelExtender()` informa o painel é recolhido.</span><span class="sxs-lookup"><span data-stu-id="2693c-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="2693c-128">Dependendo do valor de retorno dessa função, o painel é, em seguida, seja expandido (`_doOpen()` método) ou recolhido (`_doClose()`) método:</span><span class="sxs-lookup"><span data-stu-id="2693c-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


<span data-ttu-id="2693c-129">[![O terceiro botão altera o estado do painel: de recolhido expandido e](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2693c-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="2693c-130">O terceiro botão altera o estado do painel: de recolhido expandido e ([clique para exibir a imagem em tamanho normal](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2693c-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2693c-131">Anterior</span><span class="sxs-lookup"><span data-stu-id="2693c-131">Previous</span></span>](collapsing-and-expanding-a-panel-from-javascript-cs.md)
