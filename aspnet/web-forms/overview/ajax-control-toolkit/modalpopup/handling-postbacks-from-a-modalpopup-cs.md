---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Tratamento de postagens de um ModalPopup (c#) | Microsoft Docs
author: wenz
description: O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente. Deve ter especial cuidado quando um pos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 096c32d6004474d3d5952ce12d7349b9ebda6003
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="ae0ed-104">Postagens de tratamento de um ModalPopup (c#)</span><span class="sxs-lookup"><span data-stu-id="ae0ed-104">Handling Postbacks from a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="ae0ed-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ae0ed-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ae0ed-106">[Baixar o código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ae0ed-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="ae0ed-107">O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="ae0ed-108">Cuidado cuidado especial quando um postback é criado de dentro do popup.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="ae0ed-109">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="ae0ed-109">Overview</span></span>

<span data-ttu-id="ae0ed-110">O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="ae0ed-111">Cuidado cuidado especial quando um postback é criado de dentro do popup.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="ae0ed-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="ae0ed-112">Steps</span></span>

<span data-ttu-id="ae0ed-113">Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="ae0ed-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="ae0ed-114">Em seguida, adicione um painel que serve como o pop-up modal.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="ae0ed-115">Lá, o usuário pode inserir um nome e um endereço de email.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="ae0ed-116">Um botão é usado para fechar o janela pop-up e salvar as informações.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="ae0ed-117">Observe que o `OnClick` atributo está definido para que um postback ocorre quando este botão é clicado:</span><span class="sxs-lookup"><span data-stu-id="ae0ed-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="ae0ed-118">A página propriamente dita consiste em dois rótulos para exatamente as mesmas informações: nome e endereço de email.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="ae0ed-119">Um botão é usado para disparar o pop-up modal:</span><span class="sxs-lookup"><span data-stu-id="ae0ed-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="ae0ed-120">Para fazer com que o pop-up aparecer, adicione o `ModalPopupExtender` controle.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="ae0ed-121">Definir o `PopupControlID` a ID do painel atributo e `TargetControlID` a ID do botão:</span><span class="sxs-lookup"><span data-stu-id="ae0ed-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="ae0ed-122">Agora sempre que o `Save` botão dentro do popup modal é clicado, o lado do servidor `SaveData()` o método é executado.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="ae0ed-123">Lá, você pode salvar os dados inseridos em um repositório de dados.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="ae0ed-124">Para simplificar, os novos dados é apenas de saída no rótulo:</span><span class="sxs-lookup"><span data-stu-id="ae0ed-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="ae0ed-125">Além disso, os controles de caixa de texto dentro do popup modal devem ser preenchidos com o nome atual e o email.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="ae0ed-126">No entanto isso só é necessário quando nenhum postback ocorre.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="ae0ed-127">Se houver um postback, o recurso de viewstate do ASP.NET preencherá automaticamente as caixas de texto com os valores apropriados.</span><span class="sxs-lookup"><span data-stu-id="ae0ed-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


<span data-ttu-id="ae0ed-128">[![O pop-up modal causa um postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ae0ed-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="ae0ed-129">O pop-up modal causa um postback ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ae0ed-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ae0ed-130">[Anterior](using-modalpopup-with-a-repeater-control-cs.md)
[Próximo](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ae0ed-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
