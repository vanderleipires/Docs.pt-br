---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Uso de DynamicPopulate com um controle de usuário e o JavaScript (VB) | Microsoft Docs
author: wenz
description: O controle de DynamicPopulate no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: a275eed17552d26b63f98762c6c870bd53dd455d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824402"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a><span data-ttu-id="074d1-103">Uso de DynamicPopulate com um controle de usuário e o JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="074d1-103">Using DynamicPopulate with a User Control And JavaScript (VB)</span></span>
====================
<span data-ttu-id="074d1-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="074d1-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="074d1-105">[Baixar o código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="074d1-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span></span>

> <span data-ttu-id="074d1-106">O controle de DynamicPopulate no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="074d1-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="074d1-107">Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="074d1-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="074d1-108">No entanto, um cuidado especial tem a ser tomada quando o extensor reside em um controle de usuário.</span><span class="sxs-lookup"><span data-stu-id="074d1-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="074d1-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="074d1-109">Overview</span></span>

<span data-ttu-id="074d1-110">O `DynamicPopulate` controle no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="074d1-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="074d1-111">Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="074d1-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="074d1-112">No entanto, um cuidado especial tem a ser tomada quando o extensor reside em um controle de usuário.</span><span class="sxs-lookup"><span data-stu-id="074d1-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="074d1-113">Etapas</span><span class="sxs-lookup"><span data-stu-id="074d1-113">Steps</span></span>

<span data-ttu-id="074d1-114">Em primeiro lugar, você precisa de um serviço Web do ASP.NET que implementa o método a ser chamado pelo `DynamicPopulateExtender` controle.</span><span class="sxs-lookup"><span data-stu-id="074d1-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="074d1-115">O serviço da web implementa o método `getDate()` que espera um argumento de cadeia de caracteres de tipo, chamado `contextKey`, uma vez que o `DynamicPopulate` controle envia uma parte das informações de contexto com cada chamada de serviço web.</span><span class="sxs-lookup"><span data-stu-id="074d1-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="074d1-116">Aqui está o código (arquivos `DynamicPopulate.vb.asmx`) que recupera a data atual em um dos três formatos:</span><span class="sxs-lookup"><span data-stu-id="074d1-116">Here is the code (files `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="074d1-117">Na próxima etapa, crie um novo controle de usuário (`.ascx` arquivo), indicado pela seguinte declaração em sua primeira linha:</span><span class="sxs-lookup"><span data-stu-id="074d1-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="074d1-118">Um &lt; `label` &gt; elemento será usado para exibir os dados provenientes do servidor.</span><span class="sxs-lookup"><span data-stu-id="074d1-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

<span data-ttu-id="074d1-119">Também no arquivo de controle de usuário, usaremos três botões de opção, cada um representando um dos três formatos de data possível compatíveis com o serviço web.</span><span class="sxs-lookup"><span data-stu-id="074d1-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="074d1-120">Quando o usuário clica em um dos botões de opção, o navegador executará o código JavaScript que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="074d1-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

<span data-ttu-id="074d1-121">Esse código acessa a `DynamicPopulateExtender` (não se preocupe sobre a ID estranha ainda, isso será abordado mais adiante) e dispara o preenchimento dinâmico com os dados.</span><span class="sxs-lookup"><span data-stu-id="074d1-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="074d1-122">No contexto do botão de opção atual `this.value` refere-se ao seu valor que é `format1`, `format2` ou `format3` exatamente o que o método web espera.</span><span class="sxs-lookup"><span data-stu-id="074d1-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="074d1-123">A única coisa faltando no controle de usuário ainda é o `DynamicPopulateExtender` controle que vincula os botões de opção para o serviço web.</span><span class="sxs-lookup"><span data-stu-id="074d1-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="074d1-124">Novamente, você pode observar a ID estranha usada no controle: `mcd1$myDate` em vez de `myDate`.</span><span class="sxs-lookup"><span data-stu-id="074d1-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="074d1-125">Anteriormente, o código JavaScript usado `mcd1_dpe1` para acessar o `DynamicPopulateExtender` em vez de `dpe1`. Essa estratégia de nomenclatura é uma necessidade especial ao usar `DynamicPopulateExtender` dentro de um controle de usuário.</span><span class="sxs-lookup"><span data-stu-id="074d1-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="074d1-126">Além disso, você precisa inserir o controle de usuário de uma maneira específica para fazer tudo funcionar.</span><span class="sxs-lookup"><span data-stu-id="074d1-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="074d1-127">Criar uma nova página ASP.NET e registrar um prefixo de marca para o controle de usuário que você acabou de implementar:</span><span class="sxs-lookup"><span data-stu-id="074d1-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

<span data-ttu-id="074d1-128">Em seguida, inclua o AJAX ASP.NET `ScriptManager` controle na nova página:</span><span class="sxs-lookup"><span data-stu-id="074d1-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

<span data-ttu-id="074d1-129">Por fim, adicione o controle de usuário para a página.</span><span class="sxs-lookup"><span data-stu-id="074d1-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="074d1-130">Você deve definir seus `ID` atributo (e `runat="server"`, é claro), mas você também precisa defini-lo como um nome específico: `mcd1` como esse é o prefixo usado dentro do controle de usuário para acessá-los usando JavaScript.</span><span class="sxs-lookup"><span data-stu-id="074d1-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

<span data-ttu-id="074d1-131">E pronto.</span><span class="sxs-lookup"><span data-stu-id="074d1-131">And that's it!</span></span> <span data-ttu-id="074d1-132">A página se comporta conforme o esperado: um usuário clica em um dos botões de opção, o controle no Kit de ferramentas chama o serviço web e exibe a data atual no formato desejado.</span><span class="sxs-lookup"><span data-stu-id="074d1-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="074d1-133">[![Os botões de opção residem em um controle de usuário](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="074d1-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="074d1-134">Os botões de opção residem em um controle de usuário ([clique para exibir a imagem em tamanho normal](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="074d1-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="074d1-135">Anterior</span><span class="sxs-lookup"><span data-stu-id="074d1-135">Previous</span></span>](dynamically-populating-a-control-using-javascript-code-vb.md)
