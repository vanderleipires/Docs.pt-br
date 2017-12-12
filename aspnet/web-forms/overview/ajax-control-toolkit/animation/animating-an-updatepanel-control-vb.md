---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animar um controle UpdatePanel (VB) | Microsoft Docs
author: wenz
description: "O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d1056fc798e22254e94e5cad54436576a297f7d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="2da44-104">Animar um controle UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="2da44-104">Animating an UpdatePanel Control (VB)</span></span>
====================
<span data-ttu-id="2da44-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2da44-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2da44-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2da44-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="2da44-107">O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="2da44-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2da44-108">Para o conteúdo do UpdatePanel, existe um extensor especial que baseia-se na estrutura da animação: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="2da44-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="2da44-109">Este tutorial mostra como configurar esse uma animação para UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="2da44-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="2da44-110">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="2da44-110">Overview</span></span>

<span data-ttu-id="2da44-111">O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="2da44-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2da44-112">Para o conteúdo de um `UpdatePanel`, existe um extensor especial baseia-se na estrutura da animação: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="2da44-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="2da44-113">Este tutorial mostra como configurar esse uma animação para um `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="2da44-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="2da44-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="2da44-114">Steps</span></span>

<span data-ttu-id="2da44-115">Normalmente, a primeira etapa é incluir o `ScriptManager` na página de forma que a biblioteca ASP.NET AJAX foi carregada e o Kit de ferramentas de controle pode ser usado:</span><span class="sxs-lookup"><span data-stu-id="2da44-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="2da44-116">A animação neste cenário será aplicada a um ASP.NET `Wizard` controle da web que reside em um `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="2da44-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="2da44-117">Três etapas (arbitrárias) fornecem opções suficiente para disparar postbacks:</span><span class="sxs-lookup"><span data-stu-id="2da44-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="2da44-118">A marcação necessária para o `UpdatePanelAnimationExtender` controle é bastante semelhante à marcação usada para o `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="2da44-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="2da44-119">No `TargetControlID` atributo fornecemos o `ID` do `UpdatePanel` para animar; dentro a `UpdatePanelAnimationExtender` controle, o `<Animations>` elemento contém marcação XML para a animação.</span><span class="sxs-lookup"><span data-stu-id="2da44-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="2da44-120">No entanto, há uma diferença: A quantidade de eventos e manipuladores de eventos é limitada em comparação aos `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="2da44-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="2da44-121">Para `UpdatePanels`, apenas duas delas existir:</span><span class="sxs-lookup"><span data-stu-id="2da44-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="2da44-122">`<OnUpdated>`Quando o UpdatePanel foi atualizado</span><span class="sxs-lookup"><span data-stu-id="2da44-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="2da44-123">`<OnUpdating>`Quando o UpdatePanel inicia a atualização</span><span class="sxs-lookup"><span data-stu-id="2da44-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="2da44-124">Nesse cenário, o novo conteúdo do `UpdatePanel` (após o postback) deverá desaparecer.</span><span class="sxs-lookup"><span data-stu-id="2da44-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="2da44-125">Esta é a marcação necessário para que:</span><span class="sxs-lookup"><span data-stu-id="2da44-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="2da44-126">Agora, sempre que um postback ocorre dentro do UpdatePanel, o novo conteúdo do painel desaparecer sem problemas.</span><span class="sxs-lookup"><span data-stu-id="2da44-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


<span data-ttu-id="2da44-127">[![A próxima etapa do assistente é fade in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2da44-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="2da44-128">A próxima etapa do assistente é fade in ([clique para exibir a imagem em tamanho normal](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2da44-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2da44-129">[Anterior](changing-an-animation-using-client-side-code-vb.md)
[Próximo](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2da44-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
