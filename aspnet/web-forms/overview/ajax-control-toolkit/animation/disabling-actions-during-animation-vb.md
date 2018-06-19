---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Desabilitar ações durante a animação (VB) | Microsoft Docs
author: wenz
description: O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte à ação...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 9e2a0517800e90788bb67c1d75482a3d9340674b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868640"
---
<a name="disabling-actions-during-animation-vb"></a><span data-ttu-id="54d81-104">Desabilitar ações durante a animação (VB)</span><span class="sxs-lookup"><span data-stu-id="54d81-104">Disabling Actions during Animation (VB)</span></span>
====================
<span data-ttu-id="54d81-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="54d81-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="54d81-106">[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="54d81-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span></span>

> <span data-ttu-id="54d81-107">O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="54d81-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="54d81-108">Ele também dá suporte a ações, como cliques do mouse.</span><span class="sxs-lookup"><span data-stu-id="54d81-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="54d81-109">No entanto quando uma animação é iniciado um clique do mouse, é aconselhável desabilitar cliques do mouse durante a animação.</span><span class="sxs-lookup"><span data-stu-id="54d81-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="54d81-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="54d81-110">Overview</span></span>

<span data-ttu-id="54d81-111">O controle de animação no Kit de ferramentas de controle AJAX ASP.NET não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="54d81-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="54d81-112">Ele também dá suporte a ações, como cliques do mouse.</span><span class="sxs-lookup"><span data-stu-id="54d81-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="54d81-113">No entanto quando uma animação é iniciado um clique do mouse, é aconselhável desabilitar cliques do mouse durante a animação.</span><span class="sxs-lookup"><span data-stu-id="54d81-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="54d81-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="54d81-114">Steps</span></span>

<span data-ttu-id="54d81-115">Em primeiro lugar, incluem o `ScriptManager` na página; em seguida, a biblioteca ASP.NET AJAX é carregada, tornando possível usar o Kit de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="54d81-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

<span data-ttu-id="54d81-116">A animação será aplicada a um botão HTML como este:</span><span class="sxs-lookup"><span data-stu-id="54d81-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

<span data-ttu-id="54d81-117">Observe que um controle HTML é usado em vez de um controle da Web, pois não queremos que o botão para criar uma nova postagem; Ele apenas deve iniciar a animação do lado do cliente para nós.</span><span class="sxs-lookup"><span data-stu-id="54d81-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="54d81-118">Em seguida, adicione o `AnimationExtender` para a página, fornecendo um `ID`, o `TargetControlID` atributo e o obrigatórias `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="54d81-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

<span data-ttu-id="54d81-119">Dentro de `<Animations>` nó `<OnClick>` é o elemento à direita para lidar com o clique do mouse.</span><span class="sxs-lookup"><span data-stu-id="54d81-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="54d81-120">No entanto, o botão foi clicado durante a animação.</span><span class="sxs-lookup"><span data-stu-id="54d81-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="54d81-121">O `<EnableAction>` elemento cuidaremos disso.</span><span class="sxs-lookup"><span data-stu-id="54d81-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="54d81-122">Configuração `Enabled="false"` desabilita o botão como parte da animação.</span><span class="sxs-lookup"><span data-stu-id="54d81-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="54d81-123">Como estamos usando diversas animações individuais (desabilitar o botão e as animações reais), o `<Parallel>` elemento é necessário para colar animações juntas em um único.</span><span class="sxs-lookup"><span data-stu-id="54d81-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="54d81-124">Aqui está a marcação concluída para `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="54d81-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

<span data-ttu-id="54d81-125">Também seria possível reabilitar a botão após a animação, usando o seguinte elemento XML no final da lista:</span><span class="sxs-lookup"><span data-stu-id="54d81-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

<span data-ttu-id="54d81-126">No entanto em determinado cenário seria inútil desde o botão desaparece e não fica visível no final da animação.</span><span class="sxs-lookup"><span data-stu-id="54d81-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="54d81-127">[![O botão estiver desabilitado, assim como a animação é executada](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="54d81-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span></span>

<span data-ttu-id="54d81-128">O botão estiver desabilitado, assim como a animação é executada ([clique para exibir a imagem em tamanho normal](disabling-actions-during-animation-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="54d81-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="54d81-129">[Anterior](animating-in-response-to-user-interaction-vb.md)
> [Próximo](triggering-an-animation-in-another-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="54d81-129">[Previous](animating-in-response-to-user-interaction-vb.md)
[Next](triggering-an-animation-in-another-control-vb.md)</span></span>
