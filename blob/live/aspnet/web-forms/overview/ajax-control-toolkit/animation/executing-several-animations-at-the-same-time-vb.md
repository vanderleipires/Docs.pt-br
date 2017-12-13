---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: "Executando várias animações ao mesmo tempo (VB) | Microsoft Docs"
author: wenz
description: "O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele permite para executar severa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: 8461f5ea303a9e1166f694d4039d4c1aedd1caa8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-at-the-same-time-vb"></a><span data-ttu-id="30a72-104">Executando várias animações ao mesmo tempo (VB)</span><span class="sxs-lookup"><span data-stu-id="30a72-104">Executing Several Animations at The Same Time (VB)</span></span>
====================
<span data-ttu-id="30a72-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="30a72-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="30a72-106">[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="30a72-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span></span>

> <span data-ttu-id="30a72-107">O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="30a72-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="30a72-108">Ele permite para executar várias animações de forma paralela.</span><span class="sxs-lookup"><span data-stu-id="30a72-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="30a72-109">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="30a72-109">Overview</span></span>

<span data-ttu-id="30a72-110">O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="30a72-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="30a72-111">Ele permite para executar várias animações de forma paralela.</span><span class="sxs-lookup"><span data-stu-id="30a72-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="30a72-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="30a72-112">Steps</span></span>

<span data-ttu-id="30a72-113">Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="30a72-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

<span data-ttu-id="30a72-114">A animação será aplicada a um painel de texto que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="30a72-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

<span data-ttu-id="30a72-115">Na classe CSS associada para o painel, definir uma cor de plano de fundo adequado e também uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="30a72-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

<span data-ttu-id="30a72-116">Em seguida, adicione o `AnimationExtender` para a página, fornecendo um `ID`, o `TargetControlID` atributo e o obrigatórias `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="30a72-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

<span data-ttu-id="30a72-117">Dentro de `<Animations>` nó, use `<OnLoad>` para executar as animações depois que a página foi totalmente carregada.</span><span class="sxs-lookup"><span data-stu-id="30a72-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="30a72-118">Em geral, `<OnLoad>` aceita apenas uma animação.</span><span class="sxs-lookup"><span data-stu-id="30a72-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="30a72-119">A estrutura de animação permite unir várias animações em um, utilizando o `<Parallel>` elemento.</span><span class="sxs-lookup"><span data-stu-id="30a72-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="30a72-120">Todas as animações em `<Parallel>` são executadas ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="30a72-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="30a72-121">Aqui está a uma marcação possíveis para o `AnimationExtender` controle, desaparecimento e redimensionar o painel ao mesmo tempo:</span><span class="sxs-lookup"><span data-stu-id="30a72-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

<span data-ttu-id="30a72-122">E realmente: quando você executar esse script, o painel é exibido e redimensiona (mais de threefolding largura e halfing sua altura) e desaparece ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="30a72-122">And indeed: when you run this script, the panel is displayed, then resizes (more than threefolding its width and halfing its height) and fades out at the same time.</span></span>


<span data-ttu-id="30a72-123">[![O painel é desaparecimento e redimensionamento (incluindo seu conteúdo, graças ao mecanismo de renderização do navegador)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="30a72-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span></span>

<span data-ttu-id="30a72-124">O painel é desaparecimento e redimensionamento (incluindo seu conteúdo, graças ao mecanismo de renderização do navegador) ([clique para exibir a imagem em tamanho normal](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="30a72-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="30a72-125">[Anterior](adding-animation-to-a-control-vb.md)
[Próximo](executing-several-animations-after-each-other-vb.md)</span><span class="sxs-lookup"><span data-stu-id="30a72-125">[Previous](adding-animation-to-a-control-vb.md)
[Next](executing-several-animations-after-each-other-vb.md)</span></span>
