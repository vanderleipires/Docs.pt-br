---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animação dependendo de uma condição (c#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: c28f4583e6f0d1bb5c1438322980a44aa53fbd89
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391357"
---
<a name="animation-depending-on-a-condition-c"></a><span data-ttu-id="28a4f-104">Animação dependendo de uma condição (c#)</span><span class="sxs-lookup"><span data-stu-id="28a4f-104">Animation Depending On a Condition (C#)</span></span>
====================
<span data-ttu-id="28a4f-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="28a4f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="28a4f-106">[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="28a4f-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span></span>

> <span data-ttu-id="28a4f-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="28a4f-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="28a4f-108">Se uma animação é executada ou não também pode depender uma condição na forma de um código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="28a4f-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="28a4f-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="28a4f-109">Overview</span></span>

<span data-ttu-id="28a4f-110">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="28a4f-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="28a4f-111">Se uma animação é executada ou não também pode depender uma condição na forma de um código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="28a4f-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="28a4f-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="28a4f-112">Steps</span></span>

<span data-ttu-id="28a4f-113">Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:</span><span class="sxs-lookup"><span data-stu-id="28a4f-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

<span data-ttu-id="28a4f-114">A animação será aplicada a um painel de texto que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="28a4f-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

<span data-ttu-id="28a4f-115">Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="28a4f-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

<span data-ttu-id="28a4f-116">Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatório `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="28a4f-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

<span data-ttu-id="28a4f-117">Dentro de `<Animations>` nó, use `<OnLoad>` para as animações são executadas uma vez que a página foi totalmente carregada.</span><span class="sxs-lookup"><span data-stu-id="28a4f-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="28a4f-118">Em vez de uma das animações regulares, o `<Condition>` elemento entra em cena.</span><span class="sxs-lookup"><span data-stu-id="28a4f-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="28a4f-119">O código JavaScript fornecido como o valor da `ConditionScript` atributo é executado em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="28a4f-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="28a4f-120">Se for avaliada como true, a animação é executada, caso contrário, não.</span><span class="sxs-lookup"><span data-stu-id="28a4f-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="28a4f-121">A marcação a seguir fornece duas animações, cada um deles sendo executado em 50% dos casos após aleatório.</span><span class="sxs-lookup"><span data-stu-id="28a4f-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="28a4f-122">Já que pode haver apenas uma animação dentro `<OnLoad>`, os dois `<Condition>` animações são unidas usando o `<Sequence>` elemento:</span><span class="sxs-lookup"><span data-stu-id="28a4f-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

<span data-ttu-id="28a4f-123">Observe que o sinal de menor que (`<`) na `ConditionScript` atributo deve ser (escape).</span><span class="sxs-lookup"><span data-stu-id="28a4f-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="28a4f-124">Quando você executar esse script, nenhuma animação será executado, ou uma das duas delas, ou ambos fazem.</span><span class="sxs-lookup"><span data-stu-id="28a4f-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="28a4f-125">[![O painel está desaparecendo sem redimensionamento, portanto, o segunda animação é executado, o primeiro deles não](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="28a4f-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span></span>

<span data-ttu-id="28a4f-126">O painel está desaparecendo sem redimensionamento, portanto, o segunda animação é executado, o primeiro deles não ([clique para exibir a imagem em tamanho normal](animation-depending-on-a-condition-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="28a4f-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="28a4f-127">[Anterior](executing-several-animations-after-each-other-cs.md)
> [Próximo](picking-one-animation-out-of-a-list-cs.md)</span><span class="sxs-lookup"><span data-stu-id="28a4f-127">[Previous](executing-several-animations-after-each-other-cs.md)
[Next](picking-one-animation-out-of-a-list-cs.md)</span></span>
