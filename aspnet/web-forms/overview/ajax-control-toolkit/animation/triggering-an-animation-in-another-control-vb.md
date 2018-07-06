---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Disparar uma animação em outro controle (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Em geral, iniciar um...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d9ef7d35e34bd4f2b1433f7fb02d0c48834b2357
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804886"
---
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="15184-104">Disparar uma animação em outro controle (VB)</span><span class="sxs-lookup"><span data-stu-id="15184-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="15184-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="15184-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="15184-106">[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="15184-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="15184-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="15184-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="15184-108">Em geral, iniciar uma animação é disparada pela interação do usuário com o mesmo controle.</span><span class="sxs-lookup"><span data-stu-id="15184-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="15184-109">No entanto, também é possível interagir com um controle e, em seguida, a animação de outro controle.</span><span class="sxs-lookup"><span data-stu-id="15184-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="15184-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="15184-110">Overview</span></span>

<span data-ttu-id="15184-111">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="15184-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="15184-112">Em geral, iniciar uma animação é disparada pela interação do usuário com o mesmo controle.</span><span class="sxs-lookup"><span data-stu-id="15184-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="15184-113">No entanto, também é possível interagir com um controle e, em seguida, a animação de outro controle.</span><span class="sxs-lookup"><span data-stu-id="15184-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="15184-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="15184-114">Steps</span></span>

<span data-ttu-id="15184-115">Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:</span><span class="sxs-lookup"><span data-stu-id="15184-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="15184-116">A animação será aplicada a um painel de texto que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="15184-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="15184-117">Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="15184-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="15184-118">Para iniciar a animação o painel, um botão HTML é usado.</span><span class="sxs-lookup"><span data-stu-id="15184-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="15184-119">Observe que `<input type="button" />` é favoured sobre `<asp:Button />` , pois não queremos que um postback quando o usuário clica no botão.</span><span class="sxs-lookup"><span data-stu-id="15184-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="15184-120">Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatoriamente `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="15184-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="15184-121">É importante definir `TargetControlID` para a ID do botão (o elemento acionar a animação), não para a ID do painel (o elemento que está sendo animado)</span><span class="sxs-lookup"><span data-stu-id="15184-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="15184-122">Dentro de `<Animations>` nó, as animações de lugar como de costume.</span><span class="sxs-lookup"><span data-stu-id="15184-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="15184-123">Para torná-los a alterar o painel, não é o botão, defina a `AnimationTarget` atributo para cada elemento de animação em `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="15184-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="15184-124">O valor para `AnimationTarget` é a ID do painel, é claro.</span><span class="sxs-lookup"><span data-stu-id="15184-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="15184-125">Dessa forma, as animações acontecem com o painel, não com o botão de disparo.</span><span class="sxs-lookup"><span data-stu-id="15184-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="15184-126">Aqui está o `AnimationExtender` marcação para este cenário:</span><span class="sxs-lookup"><span data-stu-id="15184-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="15184-127">Observe a ordem especial em que as animações individuais aparecem.</span><span class="sxs-lookup"><span data-stu-id="15184-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="15184-128">Em primeiro lugar, o botão é desativado depois que a animação é executada.</span><span class="sxs-lookup"><span data-stu-id="15184-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="15184-129">Como não há nenhuma `AnimationTarget` de atributo no `<EnableAction>` elemento, essa animação é aplicado ao controle de origem: o botão.</span><span class="sxs-lookup"><span data-stu-id="15184-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="15184-130">As etapas de duas próximas animação deverão ser executadas parallelly (`<Parallel>` elemento).</span><span class="sxs-lookup"><span data-stu-id="15184-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="15184-131">Ambos têm seus `AnimationTarget` os atributos definidos para `"Panel1"`, animação, portanto, o painel, não o botão.</span><span class="sxs-lookup"><span data-stu-id="15184-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="15184-132">[![Um clique do mouse no botão inicia a animação de painel](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="15184-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="15184-133">Um clique do mouse no botão inicia a animação de painel ([clique para exibir a imagem em tamanho normal](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="15184-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="15184-134">[Anterior](disabling-actions-during-animation-vb.md)
> [Próximo](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="15184-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
