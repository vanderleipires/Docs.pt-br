---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Criando caixas de seleção mutuamente (VB) | Microsoft Docs
author: wenz
description: 'Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção são normalmente usadas. Há uma desvantagem, embora: quando um botão de opção em um grupo é selecionada,...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: cdb93a080fb62cfdc7e3ff0604a1447e2bb99071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="0bc28-104">Criando caixas de seleção mutuamente (VB)</span><span class="sxs-lookup"><span data-stu-id="0bc28-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>
====================
<span data-ttu-id="0bc28-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0bc28-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0bc28-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0bc28-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="0bc28-107">Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção são normalmente usadas.</span><span class="sxs-lookup"><span data-stu-id="0bc28-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="0bc28-108">Há uma desvantagem, embora: quando um botão de opção em um grupo é selecionada, não é possível desmarcar todos os botões de opção.</span><span class="sxs-lookup"><span data-stu-id="0bc28-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="0bc28-109">Caixas de seleção pode ser desmarcadas a qualquer momento, porém não são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="0bc28-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="0bc28-110">Este tutorial fornece o melhor de ambas as abordagens: caixas de seleção são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="0bc28-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="0bc28-111">Visão geral</span><span class="sxs-lookup"><span data-stu-id="0bc28-111">Overview</span></span>

<span data-ttu-id="0bc28-112">Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção são normalmente usadas.</span><span class="sxs-lookup"><span data-stu-id="0bc28-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="0bc28-113">Há uma desvantagem, embora: quando um botão de opção em um grupo é selecionada, não é possível desmarcar todos os botões de opção.</span><span class="sxs-lookup"><span data-stu-id="0bc28-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="0bc28-114">Caixas de seleção pode ser desmarcadas a qualquer momento, porém não são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="0bc28-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="0bc28-115">Este tutorial fornece o melhor de ambas as abordagens: caixas de seleção são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="0bc28-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="0bc28-116">Etapas</span><span class="sxs-lookup"><span data-stu-id="0bc28-116">Steps</span></span>

<span data-ttu-id="0bc28-117">O Kit de ferramentas de controle do ASP.NET AJAX contém o extensor MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="0bc28-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="0bc28-118">Isso permite aos programadores atribuir qualquer caixa de seleção para um nome de grupo (`Key` atributo).</span><span class="sxs-lookup"><span data-stu-id="0bc28-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="0bc28-119">Todas as caixas de seleção dentro do mesmo grupo, somente um pode ser selecionado por vez.</span><span class="sxs-lookup"><span data-stu-id="0bc28-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="0bc28-120">Vamos começar com colocar duas caixas de seleção em uma nova página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0bc28-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="0bc28-121">Pode haver mais, mas duas delas é suficiente para demonstrar o princípio:</span><span class="sxs-lookup"><span data-stu-id="0bc28-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="0bc28-122">Para ambas as caixas de seleção, um controle MutuallyExclusiveCheckBoxExtender deve ser colocado na página.</span><span class="sxs-lookup"><span data-stu-id="0bc28-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="0bc28-123">Ambos os atributos de chave precisam ter o mesmo valor, assim como o valor de atributos de elementos de botão de opção HTML devem ser idênticos para indicar o grupo que pertencem a.</span><span class="sxs-lookup"><span data-stu-id="0bc28-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="0bc28-124">A propriedade TargetControlID do extensor aponta para a ID da caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="0bc28-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="0bc28-125">Por fim, incluem o ASP.NET AJAX `ScriptManager` que é necessário para todos os elementos do Kit de ferramentas de controle AJAX ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="0bc28-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="0bc28-126">Salve e execute a página: você pode verificar e desmarque as duas caixas de seleção, porém em nenhum momento pode ambas as caixas de seleção ser verificadas.</span><span class="sxs-lookup"><span data-stu-id="0bc28-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="0bc28-127">[![Apenas uma caixa de seleção pode ser verificada por vez](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0bc28-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="0bc28-128">Apenas uma caixa de seleção pode ser verificada por vez ([clique para exibir a imagem em tamanho normal](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0bc28-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0bc28-129">Anterior</span><span class="sxs-lookup"><span data-stu-id="0bc28-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
