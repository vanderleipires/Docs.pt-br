---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulação de propriedades de sombra no código do cliente (c#) | Microsoft Docs
author: wenz
description: Personalizando a Interface de edição do DataList.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 37a7784e1d42477e31938e1d15495993ac86fc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870330"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="0ae88-103">Manipulação de propriedades de sombra no código do cliente (c#)</span><span class="sxs-lookup"><span data-stu-id="0ae88-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="0ae88-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0ae88-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0ae88-105">[Baixar o código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0ae88-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="0ae88-106">O controle de sombra no AJAX Control Toolkit estende um painel com uma sombra.</span><span class="sxs-lookup"><span data-stu-id="0ae88-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="0ae88-107">Propriedades desse extensor também podem ser alteradas usando o código de JavaScript do cliente.</span><span class="sxs-lookup"><span data-stu-id="0ae88-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="0ae88-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="0ae88-108">Overview</span></span>

<span data-ttu-id="0ae88-109">O controle de sombra no AJAX Control Toolkit estende um painel com uma sombra.</span><span class="sxs-lookup"><span data-stu-id="0ae88-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="0ae88-110">Propriedades desse extensor também podem ser alteradas usando o código de JavaScript do cliente.</span><span class="sxs-lookup"><span data-stu-id="0ae88-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="0ae88-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="0ae88-111">Steps</span></span>

<span data-ttu-id="0ae88-112">O código começa com um painel que contém algumas linhas de texto:</span><span class="sxs-lookup"><span data-stu-id="0ae88-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="0ae88-113">A classe CSS associada fornece o painel de uma cor de plano de fundo adequado:</span><span class="sxs-lookup"><span data-stu-id="0ae88-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="0ae88-114">O `DropShadowExtender` é adicionado ao estender o painel com um efeito de sombra, definido como 50% de opacidade:</span><span class="sxs-lookup"><span data-stu-id="0ae88-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="0ae88-115">Em seguida, o ASP.NET AJAX `ScriptManager` controle permite que o Kit de ferramentas de controle funcionar:</span><span class="sxs-lookup"><span data-stu-id="0ae88-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="0ae88-116">Outro painel contém dois links de JavaScript para definir a opacidade da sombra: o link menos diminui a opacidade da sombra, o link mais aumenta.</span><span class="sxs-lookup"><span data-stu-id="0ae88-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="0ae88-117">A função JavaScript `changeOpacity()` , em seguida, deve saber o `DropShadowExtender` controle na página.</span><span class="sxs-lookup"><span data-stu-id="0ae88-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="0ae88-118">ASP.NET AJAX define o `$find()` método exatamente dessa tarefa.</span><span class="sxs-lookup"><span data-stu-id="0ae88-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="0ae88-119">Em seguida, o `get_Opacity()` método recupera a opacidade atual, o `set_Opacity()` método define.</span><span class="sxs-lookup"><span data-stu-id="0ae88-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="0ae88-120">O código JavaScript coloca o valor de opacidade atual no `<label>` elemento:</span><span class="sxs-lookup"><span data-stu-id="0ae88-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="0ae88-121">[![A opacidade é alterada no lado do cliente](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0ae88-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="0ae88-122">A opacidade é alterada no lado do cliente ([clique para exibir a imagem em tamanho normal](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0ae88-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0ae88-123">[Anterior](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Próximo](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0ae88-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
