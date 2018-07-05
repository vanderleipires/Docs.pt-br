---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Permitir somente determinados caracteres em uma caixa de texto (c#) | Microsoft Docs
author: wenz
description: Controles de validação ASP.NET podem garantir que somente determinados caracteres são permitidos em entrada do usuário. No entanto isso ainda não impede que os usuários digitem inválidos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: dd7203d7f367f275d2d80c86119edc9645c9d24c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400724"
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="0d9f3-104">Permitir somente determinados caracteres em uma caixa de texto (c#)</span><span class="sxs-lookup"><span data-stu-id="0d9f3-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>
====================
<span data-ttu-id="0d9f3-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0d9f3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0d9f3-106">[Baixar o código](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0d9f3-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="0d9f3-107">Controles de validação ASP.NET podem garantir que somente determinados caracteres são permitidos em entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="0d9f3-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="0d9f3-108">No entanto isso ainda não impedir que os usuários digitando caracteres inválidos e tentar enviar o formulário.</span><span class="sxs-lookup"><span data-stu-id="0d9f3-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="0d9f3-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="0d9f3-109">Overview</span></span>

<span data-ttu-id="0d9f3-110">Controles de validação ASP.NET podem garantir que somente determinados caracteres são permitidos em entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="0d9f3-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="0d9f3-111">No entanto isso ainda não impedir que os usuários digitando caracteres inválidos e tentar enviar o formulário.</span><span class="sxs-lookup"><span data-stu-id="0d9f3-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="0d9f3-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="0d9f3-112">Steps</span></span>

<span data-ttu-id="0d9f3-113">O ASP.NET AJAX Control Toolkit contém o `FilteredTextBox` controle que se estende de uma caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="0d9f3-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="0d9f3-114">Uma vez ativado, somente um determinado conjunto de caracteres pode ser inserido no campo.</span><span class="sxs-lookup"><span data-stu-id="0d9f3-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="0d9f3-115">Para isso funcionar, primeiro precisamos como de costume ASP.NET AJAX `ScriptManager` que carrega as bibliotecas JavaScript que também são usadas pelo ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="0d9f3-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="0d9f3-116">Em seguida, precisamos de uma caixa de texto:</span><span class="sxs-lookup"><span data-stu-id="0d9f3-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="0d9f3-117">Por fim, o `FilteredTextBoxExtender` controle se encarrega de restringir os caracteres que o usuário tem permissão para o tipo.</span><span class="sxs-lookup"><span data-stu-id="0d9f3-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="0d9f3-118">Primeiro, defina as `TargetControlID` de atributo para o `ID` da `TextBox` controle.</span><span class="sxs-lookup"><span data-stu-id="0d9f3-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="0d9f3-119">Em seguida, escolha uma das disponíveis `FilterType` valores:</span><span class="sxs-lookup"><span data-stu-id="0d9f3-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="0d9f3-120">`Custom` padrão; Você precisa fornecer uma lista de caracteres válidas</span><span class="sxs-lookup"><span data-stu-id="0d9f3-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="0d9f3-121">`LowercaseLetters` apenas letras minúsculas</span><span class="sxs-lookup"><span data-stu-id="0d9f3-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="0d9f3-122">`Numbers` somente dígitos</span><span class="sxs-lookup"><span data-stu-id="0d9f3-122">`Numbers` digits only</span></span>
- <span data-ttu-id="0d9f3-123">`UppercaseLetters` somente as letras maiusculas</span><span class="sxs-lookup"><span data-stu-id="0d9f3-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="0d9f3-124">Se o `Custom FilterType` for usado, o `ValidChars` propriedade deve ser definido e fornecer uma lista de caracteres que podem ser digitados.</span><span class="sxs-lookup"><span data-stu-id="0d9f3-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="0d9f3-125">A propósito: se você tentar colar o texto na caixa de texto, todos os caracteres inválidos são removidos.</span><span class="sxs-lookup"><span data-stu-id="0d9f3-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="0d9f3-126">Aqui está a marcação para o `FilteredTextBoxExtender` controle que permite somente dígitos (algo que também seria possível com `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="0d9f3-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="0d9f3-127">Executar a página e tente digitar uma letra, se o JavaScript estiver habilitado, ele não funcionará; No entanto, dígitos exibidos na página.</span><span class="sxs-lookup"><span data-stu-id="0d9f3-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="0d9f3-128">No entanto, observe que a proteção `FilteredTextBox` fornece não é à prova de marcador: JavaScript se estiver habilitada, todos os dados podem ser inseridos na caixa de texto, portanto, você precisa usar meios de validação adicional, ou seja, o ASP. Controles de validação da rede.</span><span class="sxs-lookup"><span data-stu-id="0d9f3-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="0d9f3-129">[![Somente dígitos podem ser inseridos.](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0d9f3-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="0d9f3-130">Somente dígitos podem ser inseridos ([clique para exibir a imagem em tamanho normal](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0d9f3-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0d9f3-131">Avançar</span><span class="sxs-lookup"><span data-stu-id="0d9f3-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
