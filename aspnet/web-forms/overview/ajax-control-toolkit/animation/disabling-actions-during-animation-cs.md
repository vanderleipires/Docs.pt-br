---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Desabilitar ações durante a animação (c#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte à ação...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 396411332735503d1781e171bead8ab6aaed6efa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828189"
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="2db3b-104">Desabilitar ações durante a animação (c#)</span><span class="sxs-lookup"><span data-stu-id="2db3b-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="2db3b-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2db3b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2db3b-106">[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2db3b-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="2db3b-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="2db3b-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2db3b-108">Ele também dá suporte a ações, como cliques do mouse.</span><span class="sxs-lookup"><span data-stu-id="2db3b-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="2db3b-109">No entanto, quando um clique do mouse inicia uma animação, é desejável para desabilitar os cliques do mouse durante a animação.</span><span class="sxs-lookup"><span data-stu-id="2db3b-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="2db3b-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="2db3b-110">Overview</span></span>

<span data-ttu-id="2db3b-111">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="2db3b-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2db3b-112">Ele também dá suporte a ações, como cliques do mouse.</span><span class="sxs-lookup"><span data-stu-id="2db3b-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="2db3b-113">No entanto, quando um clique do mouse inicia uma animação, é desejável para desabilitar os cliques do mouse durante a animação.</span><span class="sxs-lookup"><span data-stu-id="2db3b-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="2db3b-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="2db3b-114">Steps</span></span>

<span data-ttu-id="2db3b-115">Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:</span><span class="sxs-lookup"><span data-stu-id="2db3b-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="2db3b-116">A animação será aplicada a um botão HTML como este:</span><span class="sxs-lookup"><span data-stu-id="2db3b-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="2db3b-117">Observe que um controle HTML é usado em vez de um controle da Web, pois não queremos que o botão para criar um postback; Ele apenas deverão iniciar a animação do lado do cliente para nós.</span><span class="sxs-lookup"><span data-stu-id="2db3b-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="2db3b-118">Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatoriamente `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="2db3b-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="2db3b-119">Dentro de `<Animations>` nó, `<OnClick>` é o elemento certo a lidar com o clique do mouse.</span><span class="sxs-lookup"><span data-stu-id="2db3b-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="2db3b-120">No entanto, o botão foi clicado durante a animação.</span><span class="sxs-lookup"><span data-stu-id="2db3b-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="2db3b-121">O `<EnableAction>` elemento pode cuidar disso.</span><span class="sxs-lookup"><span data-stu-id="2db3b-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="2db3b-122">Definindo `Enabled="false"` desabilita o botão como parte da animação.</span><span class="sxs-lookup"><span data-stu-id="2db3b-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="2db3b-123">Como estamos usando várias animações individuais (como desabilitar o botão e as animações reais), o `<Parallel>` elemento é necessário para colar as animações únicas juntos em um.</span><span class="sxs-lookup"><span data-stu-id="2db3b-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="2db3b-124">Aqui está a marcação completa para `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="2db3b-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="2db3b-125">Também seria possível habilitar novamente o botão após a animação, usando o seguinte elemento XML no final da lista:</span><span class="sxs-lookup"><span data-stu-id="2db3b-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="2db3b-126">No entanto em determinado cenário seria inútil desde o botão fade out e não fica visível no final da animação.</span><span class="sxs-lookup"><span data-stu-id="2db3b-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="2db3b-127">[![O botão estiver desabilitado, assim que a animação é executada](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2db3b-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="2db3b-128">O botão estiver desabilitado, assim que a animação é executada ([clique para exibir a imagem em tamanho normal](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2db3b-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2db3b-129">[Anterior](animating-in-response-to-user-interaction-cs.md)
> [Próximo](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2db3b-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
