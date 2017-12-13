---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: "Associação de dados o controle deslizante (c#) | Microsoft Docs"
author: wenz
description: "O controle deslizante no controle AJAX Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível associar o positio atual..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 2aa770bce5969a4ab57893d5ceecc127cf7f7872
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="databinding-the-slider-control-c"></a><span data-ttu-id="1b5e6-104">Associação de dados o controle deslizante (c#)</span><span class="sxs-lookup"><span data-stu-id="1b5e6-104">Databinding the Slider Control (C#)</span></span>
====================
<span data-ttu-id="1b5e6-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1b5e6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1b5e6-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1b5e6-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="1b5e6-107">O controle deslizante no controle AJAX Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse.</span><span class="sxs-lookup"><span data-stu-id="1b5e6-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="1b5e6-108">É possível associar a posição atual do controle deslizante para outro controle ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1b5e6-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="1b5e6-109">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="1b5e6-109">Overview</span></span>

<span data-ttu-id="1b5e6-110">O controle deslizante no controle AJAX Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse.</span><span class="sxs-lookup"><span data-stu-id="1b5e6-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="1b5e6-111">É possível associar a posição atual do controle deslizante para outro controle ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1b5e6-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="1b5e6-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="1b5e6-112">Steps</span></span>

<span data-ttu-id="1b5e6-113">Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="1b5e6-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="1b5e6-114">Em seguida, adicione dois `TextBox` controles para a página.</span><span class="sxs-lookup"><span data-stu-id="1b5e6-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="1b5e6-115">Um será transformado em um controle deslizante gráfico e outro conterá a posição do controle deslizante.</span><span class="sxs-lookup"><span data-stu-id="1b5e6-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="1b5e6-116">A próxima etapa já é a etapa final.</span><span class="sxs-lookup"><span data-stu-id="1b5e6-116">The next step is already the final step.</span></span> <span data-ttu-id="1b5e6-117">O `SliderExtender` faz com que um controle deslizante fora da primeira caixa de texto de controle do Kit de ferramentas de controle AJAX ASP.NET e atualiza automaticamente a segunda caixa de texto quando o controle deslizante posicionar as alterações.</span><span class="sxs-lookup"><span data-stu-id="1b5e6-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="1b5e6-118">Para que funcione, o `SliderExtender`do `TargetControlID` atributo deve ser definido para a ID da primeira caixa de texto; a `BoundControlID` atributo deve ser definido para a ID da segunda caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="1b5e6-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="1b5e6-119">Como você pode ver no navegador, a associação de dados funciona em ambas as direções: inserindo um novo valor na caixa de texto atualiza a posição do controle deslizante.</span><span class="sxs-lookup"><span data-stu-id="1b5e6-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="1b5e6-120">Se você fizer a segunda caixa de texto somente leitura, você pode adicionar uma proteção fraca para o campo de texto para que seja mais difícil para o usuário atualizar manualmente o valor de lá.</span><span class="sxs-lookup"><span data-stu-id="1b5e6-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="1b5e6-121">[![Controle deslizante e caixa de texto estão em sincronia](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1b5e6-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="1b5e6-122">Controle deslizante e caixa de texto estão em sincronia ([clique para exibir a imagem em tamanho normal](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1b5e6-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1b5e6-123">[Anterior](using-the-slider-control-with-auto-postback-cs.md)
[Próximo](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1b5e6-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
