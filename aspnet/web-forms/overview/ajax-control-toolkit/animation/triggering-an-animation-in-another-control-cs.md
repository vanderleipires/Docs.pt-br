---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: "Disparar uma animação em outro controle (c#) | Microsoft Docs"
author: wenz
description: "O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar um..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d243eebc42b66f1e86b38a1b7531e527144ea7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="c58c9-104">Disparar uma animação em outro controle (c#)</span><span class="sxs-lookup"><span data-stu-id="c58c9-104">Triggering an Animation in another Control (C#)</span></span>
====================
<span data-ttu-id="c58c9-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c58c9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c58c9-106">[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c58c9-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="c58c9-107">O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="c58c9-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c58c9-108">Em geral, iniciar uma animação é disparada pela interação do usuário com o mesmo controle.</span><span class="sxs-lookup"><span data-stu-id="c58c9-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="c58c9-109">No entanto, também é possível interagir com um controle e, em seguida, animação outro controle.</span><span class="sxs-lookup"><span data-stu-id="c58c9-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="c58c9-110">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="c58c9-110">Overview</span></span>

<span data-ttu-id="c58c9-111">O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="c58c9-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c58c9-112">Em geral, iniciar uma animação é disparada pela interação do usuário com o mesmo controle.</span><span class="sxs-lookup"><span data-stu-id="c58c9-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="c58c9-113">No entanto, também é possível interagir com um controle e, em seguida, animação outro controle.</span><span class="sxs-lookup"><span data-stu-id="c58c9-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="c58c9-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="c58c9-114">Steps</span></span>

<span data-ttu-id="c58c9-115">Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="c58c9-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="c58c9-116">A animação será aplicada a um painel de texto que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="c58c9-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="c58c9-117">Na classe CSS associada para o painel, definir uma cor de plano de fundo adequado e também uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="c58c9-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="c58c9-118">Para iniciar o painel de animação, um botão HTML é usado.</span><span class="sxs-lookup"><span data-stu-id="c58c9-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="c58c9-119">Observe que `<input type="button" />` é favoured pela `<asp:Button />` porque não queremos um postback quando o usuário clica no botão.</span><span class="sxs-lookup"><span data-stu-id="c58c9-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="c58c9-120">Em seguida, adicione o `AnimationExtender` para a página, fornecendo um `ID`, o `TargetControlID` atributo e o obrigatórias `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="c58c9-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="c58c9-121">É importante definir `TargetControlID` para a ID do botão (o elemento acionar a animação), não para a ID do painel (o elemento que está sendo animado)</span><span class="sxs-lookup"><span data-stu-id="c58c9-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="c58c9-122">Dentro de `<Animations>` nó, animações local como de costume.</span><span class="sxs-lookup"><span data-stu-id="c58c9-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="c58c9-123">Para torná-los a alterar o painel, não o botão Definir o `AnimationTarget` atributo para cada elemento de animação em `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="c58c9-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="c58c9-124">O valor de `AnimationTarget` é a ID do painel, é claro.</span><span class="sxs-lookup"><span data-stu-id="c58c9-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="c58c9-125">Dessa forma, as animações acontecem com o painel, não com o botão de disparo.</span><span class="sxs-lookup"><span data-stu-id="c58c9-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="c58c9-126">Aqui está o `AnimationExtender` marcação para este cenário:</span><span class="sxs-lookup"><span data-stu-id="c58c9-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="c58c9-127">Observe a ordem especial em que as animações individuais aparecem.</span><span class="sxs-lookup"><span data-stu-id="c58c9-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="c58c9-128">Em primeiro lugar, o botão é desativado quando a animação é executada.</span><span class="sxs-lookup"><span data-stu-id="c58c9-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="c58c9-129">Porque não há nenhum `AnimationTarget` atributo o `<EnableAction>` elemento, a animação é aplicado ao controle de origem: o botão.</span><span class="sxs-lookup"><span data-stu-id="c58c9-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="c58c9-130">As etapas dois animação devem ser executadas parallelly (`<Parallel>` elemento).</span><span class="sxs-lookup"><span data-stu-id="c58c9-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="c58c9-131">Ambos têm seus `AnimationTarget` atributos definidos como `"Panel1"`, animação, portanto, o painel, não o botão.</span><span class="sxs-lookup"><span data-stu-id="c58c9-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="c58c9-132">[![Um clique do mouse no botão inicia a animação de painel](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c58c9-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="c58c9-133">Um clique do mouse no botão inicia a animação de painel ([clique para exibir a imagem em tamanho normal](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c58c9-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c58c9-134">[Anterior](disabling-actions-during-animation-cs.md)
[Próximo](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c58c9-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
