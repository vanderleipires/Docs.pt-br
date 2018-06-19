---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Controlando dinamicamente UpdatePanel animações (VB) | Microsoft Docs
author: wenz
description: O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: ff2853b4457a83a7367b4d1072d21929c40a3ed2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871539"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="908b1-104">Controlando dinamicamente UpdatePanel animações (VB)</span><span class="sxs-lookup"><span data-stu-id="908b1-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>
====================
<span data-ttu-id="908b1-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="908b1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="908b1-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="908b1-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="908b1-107">O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="908b1-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="908b1-108">Para o conteúdo do UpdatePanel, existe um extensor especial que baseia-se na estrutura da animação: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="908b1-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="908b1-109">Ele também pode trabalhar com gatilhos UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="908b1-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="908b1-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="908b1-110">Overview</span></span>

<span data-ttu-id="908b1-111">O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="908b1-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="908b1-112">Para o conteúdo de um `UpdatePanel`, existe um extensor especial baseia-se na estrutura da animação: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="908b1-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="908b1-113">Ele também pode trabalhar em conjunto com `UpdatePanel` gatilhos.</span><span class="sxs-lookup"><span data-stu-id="908b1-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="908b1-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="908b1-114">Steps</span></span>

<span data-ttu-id="908b1-115">Normalmente, a primeira etapa é incluir o `ScriptManager` na página de forma que a biblioteca ASP.NET AJAX foi carregada e o Kit de ferramentas de controle pode ser usado:</span><span class="sxs-lookup"><span data-stu-id="908b1-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="908b1-116">A animação neste cenário será aplicada a uma exibição da hora atual.</span><span class="sxs-lookup"><span data-stu-id="908b1-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="908b1-117">Essas informações podem ser gravadas em um rótulo que usa o `Page_Load()` método, ou (para simplificar) é usado o seguinte código embutido:</span><span class="sxs-lookup"><span data-stu-id="908b1-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="908b1-118">Além disso, é criado um botão para disparar o tempo de atualização:</span><span class="sxs-lookup"><span data-stu-id="908b1-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="908b1-119">Esse código é colocado no `<ContentTemplate>` seção um `UpdatePanel` elemento.</span><span class="sxs-lookup"><span data-stu-id="908b1-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="908b1-120">O painel `UpdateMode` atributo deve ser definido como `"Conditional"`, uma vez que apenas os gatilhos podem atualizar o conteúdo do painel.</span><span class="sxs-lookup"><span data-stu-id="908b1-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="908b1-121">No `<Triggers>` seção o `UpdatePanel`, um gatilho de postback assíncrono é criado e vinculado ao `Click` eventos do botão.</span><span class="sxs-lookup"><span data-stu-id="908b1-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="908b1-122">Portanto, se o usuário clica no botão, o `UpdatePanel` é atualizado.</span><span class="sxs-lookup"><span data-stu-id="908b1-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="908b1-123">Aqui está a marcação para o `UpdatePanel` controle:</span><span class="sxs-lookup"><span data-stu-id="908b1-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="908b1-124">Por fim, o `UpdatePanelAnimationExtender` deve ser configurado: definir o `TargetControlID` de atributo para a ID do painel e definir uma animação dentro do extensor.</span><span class="sxs-lookup"><span data-stu-id="908b1-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="908b1-125">Fade in faz sentido, o que cria uma ênfase visual adequada na hora atualizada.</span><span class="sxs-lookup"><span data-stu-id="908b1-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="908b1-126">A marcação de extensor, em seguida, pode se parecer com isso:</span><span class="sxs-lookup"><span data-stu-id="908b1-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="908b1-127">Execute o arquivo no navegador.</span><span class="sxs-lookup"><span data-stu-id="908b1-127">Run the file in the browser.</span></span> <span data-ttu-id="908b1-128">Sempre que você clicar no botão, a hora atual é mostrada no painel, sempre fade in para a duração de um segundo.</span><span class="sxs-lookup"><span data-stu-id="908b1-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="908b1-129">[![A hora atual é fade in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="908b1-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="908b1-130">A hora atual é fade in ([clique para exibir a imagem em tamanho normal](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="908b1-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="908b1-131">Anterior</span><span class="sxs-lookup"><span data-stu-id="908b1-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
