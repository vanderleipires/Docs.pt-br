---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Usando TextBoxWatermark com controles de validação (VB) | Microsoft Docs
author: wenz
description: O controle TextBoxWatermark AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa, ele,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ca1a4af62af1d65525e59d0b7bc47245dd01476
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a><span data-ttu-id="7d27b-104">Usando TextBoxWatermark com controles de validação (VB)</span><span class="sxs-lookup"><span data-stu-id="7d27b-104">Using TextBoxWatermark With Validation Controls (VB)</span></span>
====================
<span data-ttu-id="7d27b-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7d27b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7d27b-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7d27b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span></span>

> <span data-ttu-id="7d27b-107">O controle TextBoxWatermark AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa.</span><span class="sxs-lookup"><span data-stu-id="7d27b-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="7d27b-108">Quando um usuário clica na caixa, é esvaziado.</span><span class="sxs-lookup"><span data-stu-id="7d27b-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="7d27b-109">Se o usuário deixar a caixa sem inserir texto, o texto pré-preenchidos reaparecerá.</span><span class="sxs-lookup"><span data-stu-id="7d27b-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="7d27b-110">Isso pode entrar em conflito com os controles de validação ASP.NET na mesma página, mas podem ser solucionar esses problemas.</span><span class="sxs-lookup"><span data-stu-id="7d27b-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="7d27b-111">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="7d27b-111">Overview</span></span>

<span data-ttu-id="7d27b-112">O `TextBoxWatermark` controle AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa.</span><span class="sxs-lookup"><span data-stu-id="7d27b-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="7d27b-113">Quando um usuário clica na caixa, é esvaziado.</span><span class="sxs-lookup"><span data-stu-id="7d27b-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="7d27b-114">Se o usuário deixar a caixa sem inserir texto, o texto pré-preenchidos reaparecerá.</span><span class="sxs-lookup"><span data-stu-id="7d27b-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="7d27b-115">Isso pode entrar em conflito com os controles de validação ASP.NET na mesma página, mas podem ser solucionar esses problemas.</span><span class="sxs-lookup"><span data-stu-id="7d27b-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="7d27b-116">Etapas</span><span class="sxs-lookup"><span data-stu-id="7d27b-116">Steps</span></span>

<span data-ttu-id="7d27b-117">A configuração básica do exemplo é o seguinte: uma `TextBox` controle é marca d'água usando um `TextBoxWatermarkExtender` controle.</span><span class="sxs-lookup"><span data-stu-id="7d27b-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="7d27b-118">Um botão aciona um postback e será posteriormente usado para disparar os controles de validação na página.</span><span class="sxs-lookup"><span data-stu-id="7d27b-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="7d27b-119">Além disso, um `ScriptManager` controle é necessário para inicializar o ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="7d27b-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="7d27b-120">Agora, adicione um `RequiredFieldValidator` controle que verifica se há texto no campo quando o formulário é enviado.</span><span class="sxs-lookup"><span data-stu-id="7d27b-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="7d27b-121">O `InitialValue` propriedade do validador deve ser definida com o mesmo valor que é usado no `TextBoxWatermarkExtender` controle: quando o formulário é enviado, o valor de uma caixa de texto inalterado é o valor de marca d'água nele:</span><span class="sxs-lookup"><span data-stu-id="7d27b-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="7d27b-122">No entanto, há um problema com essa abordagem: se o cliente desativa o JavaScript, o campo de texto não é previamente preenchido com o texto de marca d'água, portanto o `RequiredFieldValidator` não aciona uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="7d27b-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="7d27b-123">Portanto, um segundo `RequiredFieldValidator` controle é necessário que verifica se há uma caixa de texto (omitindo o `InitialValue` atributo).</span><span class="sxs-lookup"><span data-stu-id="7d27b-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="7d27b-124">Como usam ambos os validadores `Display` = `"Dynamic"`, o usuário final não pode distinguir a aparência visual que os dois validadores foi acionado; em vez disso, parece que houve apenas um deles.</span><span class="sxs-lookup"><span data-stu-id="7d27b-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="7d27b-125">Finalmente, adicione o código do lado do servidor para o texto no campo de saída, se nenhum validador emitida uma mensagem de erro:</span><span class="sxs-lookup"><span data-stu-id="7d27b-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


<span data-ttu-id="7d27b-126">[![O validador reclama que não há nenhum texto no campo](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7d27b-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="7d27b-127">O validador reclama que não há nenhum texto no campo ([clique para exibir a imagem em tamanho normal](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7d27b-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7d27b-128">Anterior</span><span class="sxs-lookup"><span data-stu-id="7d27b-128">Previous</span></span>](using-textboxwatermark-in-a-formview-vb.md)
