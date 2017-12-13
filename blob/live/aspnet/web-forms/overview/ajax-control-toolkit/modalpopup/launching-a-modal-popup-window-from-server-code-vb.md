---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: "Abrindo uma janela pop-up Modal do código do servidor (VB) | Microsoft Docs"
author: wenz
description: "O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente. No entanto, alguns cenários exigem que o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c4bcf3e32b3aa91bb73e01296bc1fc1a2e064711
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="17b5a-104">Abrindo uma janela pop-up Modal do código do servidor (VB)</span><span class="sxs-lookup"><span data-stu-id="17b5a-104">Launching a Modal Popup Window from Server Code (VB)</span></span>
====================
<span data-ttu-id="17b5a-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="17b5a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="17b5a-106">[Baixar o código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="17b5a-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="17b5a-107">O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="17b5a-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="17b5a-108">No entanto, alguns cenários exigem que a abertura do popup modal é acionada no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="17b5a-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="17b5a-109">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="17b5a-109">Overview</span></span>

<span data-ttu-id="17b5a-110">O controle ModalPopup no Kit de ferramentas de controle AJAX oferece uma maneira simples para criar um pop-up modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="17b5a-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="17b5a-111">No entanto, alguns cenários exigem que a abertura do popup modal é acionada no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="17b5a-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="17b5a-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="17b5a-112">Steps</span></span>

<span data-ttu-id="17b5a-113">Em primeiro lugar, um controle de botão do ASP.NET web é necessário para demonstrar como funciona o controle ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="17b5a-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="17b5a-114">Adicionar um botão desse tipo dentro de &lt;formulário&gt; elemento em uma nova página:</span><span class="sxs-lookup"><span data-stu-id="17b5a-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="17b5a-115">Em seguida, você precisa a marcação para o pop-up que você deseja criar.</span><span class="sxs-lookup"><span data-stu-id="17b5a-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="17b5a-116">Defina-o como um `<asp:Panel>` de controle e certifique-se de que ele inclui um controle de botão.</span><span class="sxs-lookup"><span data-stu-id="17b5a-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="17b5a-117">O controle ModalPopup oferece a funcionalidade para tornar este botão fechar o janela pop-up; Caso contrário, não há nenhuma maneira fácil de deixá-lo desaparecer.</span><span class="sxs-lookup"><span data-stu-id="17b5a-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="17b5a-118">Em seguida adicione o controle ModalPopup do Kit de ferramentas do ASP.NET AJAX para a página.</span><span class="sxs-lookup"><span data-stu-id="17b5a-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="17b5a-119">Definir propriedades para o botão que carrega o controle, o botão que torna desaparecem e a ID de pop-up do real.</span><span class="sxs-lookup"><span data-stu-id="17b5a-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="17b5a-120">Assim como acontece com todas as páginas da web com base no ASP.NET AJAX; o Gerenciador de Script é necessário para carregar as bibliotecas JavaScript necessárias para os navegadores de destino diferente:</span><span class="sxs-lookup"><span data-stu-id="17b5a-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="17b5a-121">Execute o exemplo no navegador.</span><span class="sxs-lookup"><span data-stu-id="17b5a-121">Run the example in the browser.</span></span> <span data-ttu-id="17b5a-122">Quando você clica no botão, aparece o pop-up modal.</span><span class="sxs-lookup"><span data-stu-id="17b5a-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="17b5a-123">Para obter o mesmo efeito que usar o código do lado do servidor, é necessário um novo botão:</span><span class="sxs-lookup"><span data-stu-id="17b5a-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="17b5a-124">Como você pode ver, um clique no botão gera um postback e executa o `ServerButton_Click()` método no servidor.</span><span class="sxs-lookup"><span data-stu-id="17b5a-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="17b5a-125">Esse método, uma função JavaScript chamada `launchModal()` é executado para ser exata, a função JavaScript será executada depois que a página foi carregada:</span><span class="sxs-lookup"><span data-stu-id="17b5a-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="17b5a-126">O trabalho de `launchModal()` é exibir o ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="17b5a-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="17b5a-127">O `launchModal()` função é executada depois que a página HTML completa foi carregada.</span><span class="sxs-lookup"><span data-stu-id="17b5a-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="17b5a-128">No momento, no entanto, a estrutura do ASP.NET AJAX não foi totalmente carregada ainda.</span><span class="sxs-lookup"><span data-stu-id="17b5a-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="17b5a-129">Portanto, o `launchModal()` função define apenas uma variável que o controle ModalPopup deve ser mostrado posteriormente:</span><span class="sxs-lookup"><span data-stu-id="17b5a-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="17b5a-130">O `pageLoad()` JavaScript é uma função especial que é executada depois que o ASP.NET AJAX foi totalmente carregado.</span><span class="sxs-lookup"><span data-stu-id="17b5a-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="17b5a-131">Portanto, adicione código para essa função para mostrar o controle ModalPopup, mas somente se `launchModal()` foi chamado antes de:</span><span class="sxs-lookup"><span data-stu-id="17b5a-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="17b5a-132">O `$find()` está procurando por um elemento nomeado na página de função e espera que a ID do servidor como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="17b5a-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="17b5a-133">Portanto, `$find("mpe")` retorna a representação de cliente do controle ModalPopup; seu `show()` método permite que o pop-up aparecer.</span><span class="sxs-lookup"><span data-stu-id="17b5a-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="17b5a-134">[![O pop-up modal aparece quando qualquer um dos botões é clicado](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="17b5a-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="17b5a-135">O pop-up modal aparece quando qualquer um dos botões é clicado ([clique para exibir a imagem em tamanho normal](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="17b5a-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="17b5a-136">[Anterior](positioning-a-modalpopup-cs.md)
[Próximo](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="17b5a-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
