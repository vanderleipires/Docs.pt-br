---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Criando caixas de seleção mutuamente exclusivas (c#) | Microsoft Docs
author: wenz
description: 'Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção são geralmente usados. Há uma desvantagem, no entanto: quando um botão de opção em um grupo for selecionado,...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: b5850c2e8d053d61e6230d3a24ddbbfe215bc531
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396112"
---
<a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="7eb4b-104">Criando caixas de seleção mutuamente exclusivas (c#)</span><span class="sxs-lookup"><span data-stu-id="7eb4b-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>
====================
<span data-ttu-id="7eb4b-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7eb4b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7eb4b-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7eb4b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="7eb4b-107">Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção são geralmente usados.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="7eb4b-108">Há uma desvantagem, no entanto: quando um botão de opção em um grupo for selecionado, não é possível desmarcar todos os botões de opção.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="7eb4b-109">Caixas de seleção pode ser desmarcadas a qualquer momento, porém não são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="7eb4b-110">Este tutorial fornece o melhor de ambas as abordagens: caixas de seleção que são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="7eb4b-111">Visão geral</span><span class="sxs-lookup"><span data-stu-id="7eb4b-111">Overview</span></span>

<span data-ttu-id="7eb4b-112">Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção são geralmente usados.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="7eb4b-113">Há uma desvantagem, no entanto: quando um botão de opção em um grupo for selecionado, não é possível desmarcar todos os botões de opção.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="7eb4b-114">Caixas de seleção pode ser desmarcadas a qualquer momento, porém não são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="7eb4b-115">Este tutorial fornece o melhor de ambas as abordagens: caixas de seleção que são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="7eb4b-116">Etapas</span><span class="sxs-lookup"><span data-stu-id="7eb4b-116">Steps</span></span>

<span data-ttu-id="7eb4b-117">O ASP.NET AJAX Control Toolkit contém o extensor MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="7eb4b-118">Isso permite que os programadores a atribuir qualquer caixa de seleção a um nome de grupo (`Key` atributo).</span><span class="sxs-lookup"><span data-stu-id="7eb4b-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="7eb4b-119">De todas as caixas de seleção dentro do mesmo grupo, somente um pode ser selecionado ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="7eb4b-120">Vamos começar com a colocação de duas caixas de seleção em uma nova página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="7eb4b-121">Pode haver mais, mas dois deles é suficiente para demonstrar o princípio:</span><span class="sxs-lookup"><span data-stu-id="7eb4b-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="7eb4b-122">Para ambas as caixas de seleção, um controle MutuallyExclusiveCheckBoxExtender deve ser colocado na página.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="7eb4b-123">Ambos os atributos de chave precisam ter o mesmo valor, assim como o valor de atributos de elementos de botão de opção HTML devem ser idênticos para indicar o grupo pertencem.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="7eb4b-124">A propriedade TargetControlID do extensor aponta para a ID da caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="7eb4b-125">Finalmente, inclua o AJAX ASP.NET `ScriptManager` que é necessária para todos os elementos do ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="7eb4b-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="7eb4b-126">Salve e execute a página: você pode verificar e desmarque as duas caixas de seleção, no entanto em nenhum momento pode ambas as caixas de seleção estar marcadas.</span><span class="sxs-lookup"><span data-stu-id="7eb4b-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="7eb4b-127">[![Apenas uma caixa de seleção pode ser verificada por vez](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7eb4b-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="7eb4b-128">Apenas uma caixa de seleção pode ser verificada por vez ([clique para exibir a imagem em tamanho normal](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7eb4b-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7eb4b-129">Avançar</span><span class="sxs-lookup"><span data-stu-id="7eb4b-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
