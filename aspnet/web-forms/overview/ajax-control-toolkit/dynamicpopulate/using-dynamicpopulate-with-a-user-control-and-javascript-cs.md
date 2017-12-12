---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: "Usando DynamicPopulate com um controle de usuário e o JavaScript (c#) | Microsoft Docs"
author: wenz
description: "O controle DynamicPopulate no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino em t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 3f78da3898d44cc2cf1db6623ebcaf6a73ebbf3e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a><span data-ttu-id="f7d8b-103">Usando DynamicPopulate com um controle de usuário e o JavaScript (c#)</span><span class="sxs-lookup"><span data-stu-id="f7d8b-103">Using DynamicPopulate with a User Control And JavaScript (C#)</span></span>
====================
<span data-ttu-id="f7d8b-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f7d8b-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f7d8b-105">[Baixar o código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f7d8b-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span></span>

> <span data-ttu-id="f7d8b-106">O controle DynamicPopulate no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="f7d8b-107">Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="f7d8b-108">No entanto, um cuidado especial tem a ser tomada quando o extensor reside em um controle de usuário.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="f7d8b-109">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="f7d8b-109">Overview</span></span>

<span data-ttu-id="f7d8b-110">O `DynamicPopulate` controle no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="f7d8b-111">Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="f7d8b-112">No entanto, um cuidado especial tem a ser tomada quando o extensor reside em um controle de usuário.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="f7d8b-113">Etapas</span><span class="sxs-lookup"><span data-stu-id="f7d8b-113">Steps</span></span>

<span data-ttu-id="f7d8b-114">Em primeiro lugar, você precisa de um serviço Web do ASP.NET que implementa o método a ser chamado `DynamicPopulateExtender` controle.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="f7d8b-115">O serviço web implementa o método `getDate()` que espera um argumento do tipo cadeia de caracteres chamado `contextKey`, pois o `DynamicPopulate` controle envia um pedaço de informações de contexto com cada chamada de serviço web.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="f7d8b-116">Aqui está o código (arquivo `DynamicPopulate.cs.asmx`) que recupera a data atual em um dos três formatos:</span><span class="sxs-lookup"><span data-stu-id="f7d8b-116">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="f7d8b-117">Na próxima etapa, crie um novo controle de usuário (`.ascx` arquivo), indicado pela seguinte declaração em sua primeira linha:</span><span class="sxs-lookup"><span data-stu-id="f7d8b-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="f7d8b-118">Um &lt; `label` &gt; elemento será usado para exibir os dados provenientes do servidor.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

<span data-ttu-id="f7d8b-119">Também no arquivo de controle de usuário, usaremos três botões de opção, cada um representando um dos três formatos de data possível suportados pelo serviço da web.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="f7d8b-120">Quando o usuário clica em um dos botões de opção, o navegador executará o código JavaScript que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="f7d8b-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

<span data-ttu-id="f7d8b-121">Este código acessa o `DynamicPopulateExtender` (não se preocupe a ID estranha ainda, isso será abordado mais adiante) e dispara a população dinâmica com dados.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="f7d8b-122">No contexto do botão de opção atual, `this.value` refere-se ao seu valor que é `format1`, `format2` ou `format3` exatamente o que o método web espera.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="f7d8b-123">A única coisa ausente no controle de usuário ainda é o `DynamicPopulateExtender` controle que vincula os botões de opção para o serviço web.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="f7d8b-124">Novamente, você pode observar o estranho ID usada no controle: `mcd1$myDate` em vez de `myDate`.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="f7d8b-125">Anteriormente, o código JavaScript usado `mcd1_dpe1` para acessar o `DynamicPopulateExtender` em vez de `dpe1`. Essa estratégia de nomenclatura é um requisito especial ao usar `DynamicPopulateExtender` dentro de um controle de usuário.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="f7d8b-126">Além disso, você precisa inserir o controle de usuário de uma maneira específica para que tudo funcione.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="f7d8b-127">Crie uma nova página ASP.NET e registre um prefixo de marca para o controle de usuário que você implementou:</span><span class="sxs-lookup"><span data-stu-id="f7d8b-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

<span data-ttu-id="f7d8b-128">Em seguida, inclua o ASP.NET AJAX `ScriptManager` controle na nova página:</span><span class="sxs-lookup"><span data-stu-id="f7d8b-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

<span data-ttu-id="f7d8b-129">Finalmente, adicione o controle de usuário para a página.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="f7d8b-130">Você só precisa definir seu `ID` atributo (e `runat="server"`, é claro), mas você também tem defini-lo como um nome específico: `mcd1` já que este é o prefixo usado dentro do controle de usuário para acessá-lo usando JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

<span data-ttu-id="f7d8b-131">E pronto.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-131">And that's it!</span></span> <span data-ttu-id="f7d8b-132">A página se comporta como esperado: um usuário clica em dos botões de opção, o controle no Kit de ferramentas chama o serviço web e exibe a data atual no formato desejado.</span><span class="sxs-lookup"><span data-stu-id="f7d8b-132">The page behaves as expected: A user clicks on on of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="f7d8b-133">[![Os botões de opção residem em um controle de usuário](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f7d8b-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="f7d8b-134">Os botões de opção residem em um controle de usuário ([clique para exibir a imagem em tamanho normal](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f7d8b-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f7d8b-135">[Anterior](dynamically-populating-a-control-using-javascript-code-cs.md)
[Próximo](dynamically-populating-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f7d8b-135">[Previous](dynamically-populating-a-control-using-javascript-code-cs.md)
[Next](dynamically-populating-a-control-vb.md)</span></span>
