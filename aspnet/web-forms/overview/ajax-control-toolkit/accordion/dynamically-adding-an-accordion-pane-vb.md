---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Adição dinâmica de um painel Accordion (VB) | Microsoft Docs
author: wenz
description: O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite que o usuário exibir um por vez. Painéis são normalmente declaradas w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: b0ac0919e64fb6494bd9c7c5f00a5f69274799ed
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823508"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="95bd1-104">Adição dinâmica de um painel Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="95bd1-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="95bd1-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="95bd1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="95bd1-106">[Baixar o código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="95bd1-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="95bd1-107">O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite que o usuário exibir um por vez.</span><span class="sxs-lookup"><span data-stu-id="95bd1-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="95bd1-108">Painéis geralmente são declaradas dentro da página em si, mas o código do lado do servidor pode ser usado para alcançar o mesmo resultado.</span><span class="sxs-lookup"><span data-stu-id="95bd1-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="95bd1-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="95bd1-109">Overview</span></span>

<span data-ttu-id="95bd1-110">O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite que o usuário exibir um por vez.</span><span class="sxs-lookup"><span data-stu-id="95bd1-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="95bd1-111">Painéis geralmente são declaradas dentro da página em si, mas o código do lado do servidor pode ser usado para alcançar o mesmo resultado.</span><span class="sxs-lookup"><span data-stu-id="95bd1-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="95bd1-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="95bd1-112">Steps</span></span>

<span data-ttu-id="95bd1-113">O controle Accordion expõe todas as propriedades importantes para o código do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="95bd1-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="95bd1-114">Entre outras coisas, o `Panes` propriedade concede acesso à coleção de painéis que compõem o Accordion.</span><span class="sxs-lookup"><span data-stu-id="95bd1-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="95bd1-115">Cada painel lá é do tipo `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="95bd1-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="95bd1-116">Portanto, é trivial criar tal um painel:</span><span class="sxs-lookup"><span data-stu-id="95bd1-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="95bd1-117">O `HeaderContainer` propriedade de `AccordionPane` fornece acesso aos controles ASP.NET dentro da seção de cabeçalho do painel; a `ContentContainer` propriedade do `AccordionPane` faz o mesmo para a seção de conteúdo do painel.</span><span class="sxs-lookup"><span data-stu-id="95bd1-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="95bd1-118">Isso permite que o código ASP.NET para adicionar conteúdo para os painéis:</span><span class="sxs-lookup"><span data-stu-id="95bd1-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="95bd1-119">Por fim, os painéis devem ser adicionados para o `Panes` coleção de Acordeão:</span><span class="sxs-lookup"><span data-stu-id="95bd1-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="95bd1-120">Aqui está um código do lado do servidor completo que adiciona dois painéis a um controle Accordion:</span><span class="sxs-lookup"><span data-stu-id="95bd1-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="95bd1-121">O único elemento ausente é Accordion em si, que depende da presença do ASP.NET `ScriptManager` controle:</span><span class="sxs-lookup"><span data-stu-id="95bd1-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="95bd1-122">Para concluir o exemplo, as duas classes CSS referenciadas no controle Accordion fornecem informações de estilo para o navegador:</span><span class="sxs-lookup"><span data-stu-id="95bd1-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="95bd1-123">[![Os dados em um acordeão dinamicamente foi adicionados pelo código do lado do servidor](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="95bd1-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="95bd1-124">Os dados em um acordeão dinamicamente foi adicionados pelo código do lado do servidor ([clique para exibir a imagem em tamanho normal](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="95bd1-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="95bd1-125">Anterior</span><span class="sxs-lookup"><span data-stu-id="95bd1-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
