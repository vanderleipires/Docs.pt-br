---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Tratamento de Postbacks de um ModalPopup (VB) | Microsoft Docs
author: wenz
description: O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um pop-up modal usando meios do lado do cliente. Cuidado especial deve ser executado quando um pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f81a2bf1866d026046fdba815fdbae1b162c1dd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825214"
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="af27c-104">Tratamento de Postbacks de um ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="af27c-104">Handling Postbacks from a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="af27c-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="af27c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="af27c-106">[Baixar o código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="af27c-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="af27c-107">O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um pop-up modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="af27c-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="af27c-108">Cuidado especial deve ser executado quando um postback é criado a partir do pop-up.</span><span class="sxs-lookup"><span data-stu-id="af27c-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="af27c-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="af27c-109">Overview</span></span>

<span data-ttu-id="af27c-110">O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um pop-up modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="af27c-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="af27c-111">Cuidado especial deve ser executado quando um postback é criado a partir do pop-up.</span><span class="sxs-lookup"><span data-stu-id="af27c-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="af27c-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="af27c-112">Steps</span></span>

<span data-ttu-id="af27c-113">Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="af27c-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="af27c-114">Em seguida, adicione um painel que serve como o popup modal.</span><span class="sxs-lookup"><span data-stu-id="af27c-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="af27c-115">Lá, o usuário pode inserir um nome e um endereço de email.</span><span class="sxs-lookup"><span data-stu-id="af27c-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="af27c-116">Um botão é usado para fechar o janela pop-up e salvar as informações.</span><span class="sxs-lookup"><span data-stu-id="af27c-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="af27c-117">Observe que o `OnClick` atributo é definido para que um postback ocorre quando este botão é clicado:</span><span class="sxs-lookup"><span data-stu-id="af27c-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="af27c-118">A página propriamente dita consiste em dois rótulos para exatamente as mesmas informações: nome e endereço de email.</span><span class="sxs-lookup"><span data-stu-id="af27c-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="af27c-119">Um botão é usado para disparar o popup modal:</span><span class="sxs-lookup"><span data-stu-id="af27c-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="af27c-120">Para fazer com que o pop-up aparecer, adicione o `ModalPopupExtender` controle.</span><span class="sxs-lookup"><span data-stu-id="af27c-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="af27c-121">Defina as `PopupControlID` atributo à ID do painel e `TargetControlID` à ID do botão:</span><span class="sxs-lookup"><span data-stu-id="af27c-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="af27c-122">Agora sempre que o `Save` botão dentro do popup modal é clicado, o servidor `SaveData()` método é executado.</span><span class="sxs-lookup"><span data-stu-id="af27c-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="af27c-123">Lá, você pode salvar os dados inseridos em um repositório de dados.</span><span class="sxs-lookup"><span data-stu-id="af27c-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="af27c-124">Para simplificar, os novos dados é apenas de saída no rótulo:</span><span class="sxs-lookup"><span data-stu-id="af27c-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="af27c-125">Além disso, os controles de caixa de texto dentro do popup modal devem ser preenchidos com o nome atual e o email.</span><span class="sxs-lookup"><span data-stu-id="af27c-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="af27c-126">No entanto isso só é necessário quando nenhum postback ocorre.</span><span class="sxs-lookup"><span data-stu-id="af27c-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="af27c-127">Se houver um postback, o recurso de viewstate do ASP.NET preencherá automaticamente as caixas de texto com os valores apropriados.</span><span class="sxs-lookup"><span data-stu-id="af27c-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


<span data-ttu-id="af27c-128">[![O popup modal faz com que um postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="af27c-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="af27c-129">O popup modal faz com que um postback ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="af27c-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="af27c-130">[Anterior](using-modalpopup-with-a-repeater-control-vb.md)
> [Próximo](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="af27c-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
