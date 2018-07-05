---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Uso de TextBoxWatermark com controles de validação (c#) | Microsoft Docs
author: wenz
description: O controle TextBoxWatermark do AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa de-eu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: fe28f169e5e714ba07d8a71bb33d0f62608d688b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380685"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="fa5a1-104">Uso de TextBoxWatermark com controles de validação (c#)</span><span class="sxs-lookup"><span data-stu-id="fa5a1-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>
====================
<span data-ttu-id="fa5a1-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fa5a1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fa5a1-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fa5a1-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="fa5a1-107">O controle TextBoxWatermark do AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="fa5a1-108">Quando um usuário clica na caixa, é esvaziado.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="fa5a1-109">Se o usuário deixa a caixa sem digitar texto, o texto previamente preenchido reaparece.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="fa5a1-110">Isso entrem em conflito com os controles de validação ASP.NET na mesma página, mas esses problemas podem ser superados.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="fa5a1-111">Visão geral</span><span class="sxs-lookup"><span data-stu-id="fa5a1-111">Overview</span></span>

<span data-ttu-id="fa5a1-112">O `TextBoxWatermark` o controle no AJAX Control Toolkit, uma caixa de texto para que um texto seja exibido dentro da caixa.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="fa5a1-113">Quando um usuário clica na caixa, é esvaziado.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="fa5a1-114">Se o usuário deixa a caixa sem digitar texto, o texto previamente preenchido reaparece.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="fa5a1-115">Isso entrem em conflito com os controles de validação ASP.NET na mesma página, mas esses problemas podem ser superados.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="fa5a1-116">Etapas</span><span class="sxs-lookup"><span data-stu-id="fa5a1-116">Steps</span></span>

<span data-ttu-id="fa5a1-117">A configuração básica do exemplo é o seguinte: uma `TextBox` controle é com marca d'água usando um `TextBoxWatermarkExtender` controle.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="fa5a1-118">Um botão dispara um postback e será posteriormente usado para disparar os controles de validação na página.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="fa5a1-119">Além disso, um `ScriptManager` controle é necessário para inicializar o ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="fa5a1-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="fa5a1-120">Agora, adicione um `RequiredFieldValidator` controle que verifica se há texto no campo quando o formulário é enviado.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="fa5a1-121">O `InitialValue` propriedade do validador deve ser definida com o mesmo valor que é usado no `TextBoxWatermarkExtender` controle: quando o formulário for enviado, o valor de uma caixa de texto inalterado é o valor de marca d'água dentro dele:</span><span class="sxs-lookup"><span data-stu-id="fa5a1-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="fa5a1-122">No entanto, há um problema com essa abordagem: se o cliente desativa o JavaScript, o campo de texto não é preenchido com o texto de marca d'água, portanto o `RequiredFieldValidator` não dispara uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="fa5a1-123">Portanto, um segundo `RequiredFieldValidator` é necessário o controle que verifica se há uma caixa de texto vazia (omitindo o `InitialValue` atributo).</span><span class="sxs-lookup"><span data-stu-id="fa5a1-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="fa5a1-124">Uma vez que utilizar ambos os validadores `Display` = `"Dynamic"`, o usuário final não é possível distinguir da aparência visual que foi acionada dos dois validadores; em vez disso, parece que houve apenas um deles.</span><span class="sxs-lookup"><span data-stu-id="fa5a1-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="fa5a1-125">Por fim, adicione algum código do lado do servidor para o texto no campo de saída se nenhum validador emitida uma mensagem de erro:</span><span class="sxs-lookup"><span data-stu-id="fa5a1-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


<span data-ttu-id="fa5a1-126">[![O validador reclama que não há nenhum texto no campo](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fa5a1-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="fa5a1-127">O validador reclama que não há nenhum texto no campo ([clique para exibir a imagem em tamanho normal](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fa5a1-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fa5a1-128">[Anterior](using-textboxwatermark-in-a-formview-cs.md)
> [Próximo](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fa5a1-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
