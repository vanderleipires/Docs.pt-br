---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Escolhendo uma animação em uma lista (c#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A estrutura também mitir...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 9cbbfaefca43a2fb25eec30115bcc4b5112754a9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834075"
---
<a name="picking-one-animation-out-of-a-list-c"></a><span data-ttu-id="9fa0d-104">Escolhendo uma animação em uma lista (c#)</span><span class="sxs-lookup"><span data-stu-id="9fa0d-104">Picking One Animation Out Of a List (C#)</span></span>
====================
<span data-ttu-id="9fa0d-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9fa0d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9fa0d-106">[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="9fa0d-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span></span>

> <span data-ttu-id="9fa0d-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="9fa0d-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9fa0d-108">O framework também permite que o programador escolher uma animação em uma lista de animações, dependendo da avaliação de algum código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9fa0d-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="9fa0d-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="9fa0d-109">Overview</span></span>

<span data-ttu-id="9fa0d-110">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="9fa0d-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9fa0d-111">O framework também permite que o programador escolher uma animação em uma lista de animações, dependendo da avaliação de algum código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9fa0d-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="9fa0d-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="9fa0d-112">Steps</span></span>

<span data-ttu-id="9fa0d-113">Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:</span><span class="sxs-lookup"><span data-stu-id="9fa0d-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

<span data-ttu-id="9fa0d-114">A animação será aplicada a um painel de texto que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="9fa0d-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

<span data-ttu-id="9fa0d-115">Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="9fa0d-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

<span data-ttu-id="9fa0d-116">Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatório `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="9fa0d-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

<span data-ttu-id="9fa0d-117">Dentro de `<Animations>` nó, use `<OnLoad>` para as animações são executadas uma vez que a página foi totalmente carregada.</span><span class="sxs-lookup"><span data-stu-id="9fa0d-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="9fa0d-118">Em vez de uma das animações regulares, o `<Case>` elemento entra em cena.</span><span class="sxs-lookup"><span data-stu-id="9fa0d-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="9fa0d-119">O valor do seu atributo SelectScript é avaliado; o valor de retorno deve ser numérico.</span><span class="sxs-lookup"><span data-stu-id="9fa0d-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="9fa0d-120">Dependendo desse número, uma das subanimations dentro &lt;caso&gt; é executado.</span><span class="sxs-lookup"><span data-stu-id="9fa0d-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="9fa0d-121">Por exemplo, se SelectScript for avaliada como 2, o Kit de ferramentas de controle é executado a terceira animação dentro &lt;caso&gt; (contando começa em 0).</span><span class="sxs-lookup"><span data-stu-id="9fa0d-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="9fa0d-122">A marcação a seguir define três subanimations: redimensionar a largura, redimensionar a altura e desaparecimento. O código JavaScript (`Math.floor(3 * Math.random())`), em seguida, escolhe um número entre 0 e 2, para que um dos três animações é executado:</span><span class="sxs-lookup"><span data-stu-id="9fa0d-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


<span data-ttu-id="9fa0d-123">[![Uma das três animações possíveis: O painel obtém maior](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9fa0d-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span></span>

<span data-ttu-id="9fa0d-124">Uma das três animações possíveis: O painel obtém mais amplo ([clique para exibir a imagem em tamanho normal](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9fa0d-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9fa0d-125">[Anterior](animation-depending-on-a-condition-cs.md)
> [Próximo](animating-in-response-to-user-interaction-cs.md)</span><span class="sxs-lookup"><span data-stu-id="9fa0d-125">[Previous](animation-depending-on-a-condition-cs.md)
[Next](animating-in-response-to-user-interaction-cs.md)</span></span>
