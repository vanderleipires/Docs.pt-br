---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Ajustando o índice Z de uma sombra (VB) | Microsoft Docs
author: wenz
description: O controle de sombra no AJAX Control Toolkit estende um painel com uma sombra. No entanto essa sombra às vezes, está em conflito com outros controles, para insta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: b484dc6bfa6f67bd6b70f7c36c2eb2ec7143edaf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871292"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="b7754-104">Ajustando o índice Z de uma sombra (VB)</span><span class="sxs-lookup"><span data-stu-id="b7754-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="b7754-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b7754-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b7754-106">[Baixar o código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b7754-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="b7754-107">O controle de sombra no AJAX Control Toolkit estende um painel com uma sombra.</span><span class="sxs-lookup"><span data-stu-id="b7754-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="b7754-108">No entanto essa sombra às vezes, está em conflito com outros controles, por exemplo, o controle de Menu do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b7754-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="b7754-109">Quando uma entrada de menu é exibido, ele aparece atrás a sombra projetada.</span><span class="sxs-lookup"><span data-stu-id="b7754-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="b7754-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="b7754-110">Overview</span></span>

<span data-ttu-id="b7754-111">O controle de sombra no AJAX Control Toolkit estende um painel com uma sombra.</span><span class="sxs-lookup"><span data-stu-id="b7754-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="b7754-112">No entanto essa sombra às vezes, está em conflito com outros controles, por exemplo, o controle de Menu do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b7754-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="b7754-113">Quando uma entrada de menu é exibido, ele aparece atrás a sombra projetada.</span><span class="sxs-lookup"><span data-stu-id="b7754-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="b7754-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="b7754-114">Steps</span></span>

<span data-ttu-id="b7754-115">O código começa com o próprio painel, que contém texto suficiente para que o painel contém texto suficiente para que o efeito seja visível:</span><span class="sxs-lookup"><span data-stu-id="b7754-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="b7754-116">Outro painel é colocado diretamente antes do `panelShadow` painel.</span><span class="sxs-lookup"><span data-stu-id="b7754-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="b7754-117">Ele contém um menu com orientação horizontal para que as entradas do menu exibido acima (ou ainda: sob) o `dropShadow` painel):</span><span class="sxs-lookup"><span data-stu-id="b7754-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="b7754-118">Em seguida, o `DropShadowExtender` é adicionado ao estender o `panelShadow` painel com um efeito de sombra:</span><span class="sxs-lookup"><span data-stu-id="b7754-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="b7754-119">Por fim, o ASP.NET AJAX `ScriptManager` controle permite que o Kit de ferramentas de controle funcionar:</span><span class="sxs-lookup"><span data-stu-id="b7754-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="b7754-120">Quando você executar esse script, as entradas do menu aparecem sob o painel.</span><span class="sxs-lookup"><span data-stu-id="b7754-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="b7754-121">No entanto, o menu usa a classe CSS `panel` onde você só precisa definir duas coisas para fazer com que elementos exibidos na frente do painel de:</span><span class="sxs-lookup"><span data-stu-id="b7754-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="b7754-122">Posicionamento relativo</span><span class="sxs-lookup"><span data-stu-id="b7754-122">Relative positioning</span></span>
- <span data-ttu-id="b7754-123">Um índice z positivo</span><span class="sxs-lookup"><span data-stu-id="b7754-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="b7754-124">Em seguida, o `DropShadowExtender` controle não está em conflito mais com o controle de Menu.</span><span class="sxs-lookup"><span data-stu-id="b7754-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="b7754-125">[![Antes: A entrada de menu não está visível](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b7754-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="b7754-126">Antes: A entrada de menu não estiver visível ([clique para exibir a imagem em tamanho normal](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b7754-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="b7754-127">[![Depois: A entrada de menu é exibido](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="b7754-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="b7754-128">Depois: A entrada de menu é exibido ([clique para exibir a imagem em tamanho normal](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b7754-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b7754-129">[Anterior](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Próximo](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b7754-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
