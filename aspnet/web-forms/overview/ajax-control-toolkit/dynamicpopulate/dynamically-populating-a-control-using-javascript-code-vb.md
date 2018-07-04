---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Preenchimento dinâmico de um controle usando código JavaScript (VB) | Microsoft Docs
author: wenz
description: O controle de DynamicPopulate no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 9df2a66bd49ecba52b0dd8b1d52a65b36c38a5dc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366841"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="cd96b-103">Preenchimento dinâmico de um controle usando código JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="cd96b-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>
====================
<span data-ttu-id="cd96b-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="cd96b-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="cd96b-105">[Baixar o código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="cd96b-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="cd96b-106">O controle de DynamicPopulate no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="cd96b-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="cd96b-107">Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="cd96b-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="cd96b-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="cd96b-108">Overview</span></span>

<span data-ttu-id="cd96b-109">O `DynamicPopulate` controle no ASP.NET AJAX Control Toolkit chama um serviço web (ou o método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="cd96b-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="cd96b-110">Também é possível disparar a população usando código personalizado de JavaScript do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="cd96b-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="cd96b-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="cd96b-111">Steps</span></span>

<span data-ttu-id="cd96b-112">Em primeiro lugar, você precisa de um serviço Web do ASP.NET que implementa o método a ser chamado pelo `DynamicPopulateExtender` controle.</span><span class="sxs-lookup"><span data-stu-id="cd96b-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="cd96b-113">O serviço da web implementa o método `getDate()` que espera um argumento de cadeia de caracteres de tipo, chamado `contextKey`, uma vez que o `DynamicPopulate` controle envia uma parte das informações de contexto com cada chamada de serviço web.</span><span class="sxs-lookup"><span data-stu-id="cd96b-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="cd96b-114">Aqui está o código (arquivo `DynamicPopulate.vb.asmx`) que recupera a data atual em um dos três formatos:</span><span class="sxs-lookup"><span data-stu-id="cd96b-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="cd96b-115">Na próxima etapa, crie um novo site do ASP.NET e iniciar com o controle ScriptManager do AJAX ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="cd96b-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="cd96b-116">Em seguida, adicione um controle de rótulo (por exemplo, usando o controle HTML de mesmo nome, ou o `<asp:Label />` controle da web) mais tarde, que mostrará o resultado da chamada de serviço web.</span><span class="sxs-lookup"><span data-stu-id="cd96b-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="cd96b-117">Em seguida, inclua um `DynamicPopulateExtender` controlar e fornecer informações do serviço web, o controle de destino, mas não o nome do controle que dispara a população que isso será feito no futuro, usando o JavaScript personalizado!</span><span class="sxs-lookup"><span data-stu-id="cd96b-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="cd96b-118">Agora à parte de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="cd96b-118">Now to the JavaScript part.</span></span> <span data-ttu-id="cd96b-119">O `$find()` função, definida pela biblioteca do AJAX ASP.NET, retorna uma referência a objetos do lado do servidor do ASP.NET AJAX Control Toolkit, como `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="cd96b-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="cd96b-120">No arquivo atual, `$find("dpe")` retorna uma referência para aquele `DynamicPopulateExtender` controle na página.</span><span class="sxs-lookup"><span data-stu-id="cd96b-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="cd96b-121">Ela expõe um método chamado `populate()` que dispara o processo de preenchimento dinâmico.</span><span class="sxs-lookup"><span data-stu-id="cd96b-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="cd96b-122">O `populate()` método requer um argumento: a chave de contexto que servirá como argumento para o `getDate()` método web.</span><span class="sxs-lookup"><span data-stu-id="cd96b-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="cd96b-123">Por exemplo, `$find("dpe").populate("format1")` preencheria o rótulo com a data atual no formato mês / dia / ano.</span><span class="sxs-lookup"><span data-stu-id="cd96b-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="cd96b-124">Para tornar o exemplo um pouco mais flexível, o usuário pode escolher entre vários formatos de data.</span><span class="sxs-lookup"><span data-stu-id="cd96b-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="cd96b-125">Para cada um deles, um botão de opção é exibido.</span><span class="sxs-lookup"><span data-stu-id="cd96b-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="cd96b-126">Uma vez o usuário clica em um botão de opção, o código JavaScript preenche dinamicamente o rótulo com o formato da data selecionada.</span><span class="sxs-lookup"><span data-stu-id="cd96b-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="cd96b-127">Aqui estão os botões de opção:</span><span class="sxs-lookup"><span data-stu-id="cd96b-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="cd96b-128">Observe que dentro do contexto de um botão de opção, a expressão JavaScript `this.value` refere-se ao valor do botão atual, o que vem a ser exatamente as mesmas informações de `getDate()` método pode trabalhar com.</span><span class="sxs-lookup"><span data-stu-id="cd96b-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="cd96b-129">[![Um clique no botão recupera a data do servidor, no formato especificado](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cd96b-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="cd96b-130">Um clique no botão recupera a data do servidor, no formato especificado ([clique para exibir a imagem em tamanho normal](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="cd96b-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cd96b-131">[Anterior](dynamically-populating-a-control-vb.md)
> [Próximo](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="cd96b-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>
