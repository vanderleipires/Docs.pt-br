---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: "Executar animações usando o código do lado do cliente (VB) | Microsoft Docs"
author: wenz
description: "O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A execução de animação..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c97ce87addd566ed1ba63ccdb81206c6449f49a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="4704a-104">Executar animações usando o código do lado do cliente (VB)</span><span class="sxs-lookup"><span data-stu-id="4704a-104">Executing Animations Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="4704a-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4704a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4704a-106">[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4704a-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="4704a-107">O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="4704a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4704a-108">A execução da animação também pode ser disparada usando código personalizado de JavaScript do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="4704a-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="4704a-109">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="4704a-109">Overview</span></span>

<span data-ttu-id="4704a-110">O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="4704a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4704a-111">A execução da animação também pode ser disparada usando código personalizado de JavaScript do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="4704a-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="4704a-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="4704a-112">Steps</span></span>

<span data-ttu-id="4704a-113">Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="4704a-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="4704a-114">A animação será aplicada a um painel de texto que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="4704a-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="4704a-115">Na classe CSS associada para o painel, definir uma cor de plano de fundo adequado e também uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="4704a-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="4704a-116">Em seguida, adicione o `AnimationExtender` para a página, fornecendo um `ID`, o `TargetControlID` atributo e o obrigatórias `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="4704a-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="4704a-117">Dentro de `<Animations>` nó, use `<OnClick>` executar as animações uma vez, o usuário clica no painel.</span><span class="sxs-lookup"><span data-stu-id="4704a-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="4704a-118">Adicione duas animações a serem executados parallelly:</span><span class="sxs-lookup"><span data-stu-id="4704a-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="4704a-119">Para fins de demonstração, essa animação (e qualquer outra animação criado usando o Kit de ferramentas de controle) são executados usando o código JavaScript, depois que a página é executada.</span><span class="sxs-lookup"><span data-stu-id="4704a-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="4704a-120">Em primeiro lugar, precisamos acessem o `AnimationExtender` controle.</span><span class="sxs-lookup"><span data-stu-id="4704a-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="4704a-121">A biblioteca ASP.NET AJAX fornece o `$find()` função para esta tarefa:</span><span class="sxs-lookup"><span data-stu-id="4704a-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="4704a-122">O `AnimationExtender` controle expõe uma API avançada, incluindo métodos com nomes idênticos aos manipuladores de eventos usados na marcação XML: `OnClick()`, `OnLoad()`e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="4704a-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="4704a-123">Por exemplo, uma chamada do `OnClick()` método executa a animação dentro a `<OnClick>` elemento do `AnimationExtender` controle:</span><span class="sxs-lookup"><span data-stu-id="4704a-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="4704a-124">Aqui está o código completo de JavaScript do lado do cliente que emula o clique no painel depois que a página foi totalmente carregada Observe que o `pageLoad()` nome da função é usado que é chamado pelo ASP.NET AJAX uma vez a página e todos os incluídos JavaScript bibliotecas foram carregado.</span><span class="sxs-lookup"><span data-stu-id="4704a-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


<span data-ttu-id="4704a-125">[![A animação é executada imediatamente, sem um clique do mouse](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4704a-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="4704a-126">A animação é executada imediatamente, sem um clique do mouse ([clique para exibir a imagem em tamanho normal](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4704a-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4704a-127">[Anterior](modifying-animations-from-the-server-side-vb.md)
[Próximo](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4704a-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
