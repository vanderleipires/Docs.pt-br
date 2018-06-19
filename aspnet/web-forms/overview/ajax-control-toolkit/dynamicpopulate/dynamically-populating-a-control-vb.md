---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Preencher dinamicamente um controle (VB) | Microsoft Docs
author: wenz
description: O controle DynamicPopulate no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e2031a80be71a406e632955583d83920dd0f3ef7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879732"
---
<a name="dynamically-populating-a-control-vb"></a><span data-ttu-id="0f4f0-103">Preencher dinamicamente um controle (VB)</span><span class="sxs-lookup"><span data-stu-id="0f4f0-103">Dynamically Populating a Control (VB)</span></span>
====================
<span data-ttu-id="0f4f0-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0f4f0-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0f4f0-105">[Baixar o código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0f4f0-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span></span>

> <span data-ttu-id="0f4f0-106">O controle DynamicPopulate no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="0f4f0-107">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="0f4f0-107">Overview</span></span>

<span data-ttu-id="0f4f0-108">O `DynamicPopulate` controle no Kit de ferramentas de controle AJAX ASP.NET chama um serviço da web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="0f4f0-109">Este tutorial mostra como configurar isso.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="0f4f0-110">Etapas</span><span class="sxs-lookup"><span data-stu-id="0f4f0-110">Steps</span></span>

<span data-ttu-id="0f4f0-111">Em primeiro lugar, você precisa de um serviço Web do ASP.NET que implementa o método a ser chamado `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="0f4f0-112">A classe de serviço da web requer o `ScriptService` atributo que é definido em `Microsoft.Web.Script.Services`; caso contrário, o ASP.NET AJAX não é possível criar proxy JavaScript do lado do cliente para o serviço web, que por sua vez, é necessário por `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="0f4f0-113">O método da web deve esperar que um argumento do tipo cadeia de caracteres chamado `contextKey`, pois o `DynamicPopulate` controle envia um pedaço de informações de contexto com cada chamada de serviço web.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="0f4f0-114">O serviço da web a seguir retorna a data atual em um formato representado pelo `contextKey` argumento:</span><span class="sxs-lookup"><span data-stu-id="0f4f0-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="0f4f0-115">O serviço da web, em seguida, é salvo como `DynamicPopulate.vb.asmx`.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-115">The web service is then saved as `DynamicPopulate.vb.asmx`.</span></span> <span data-ttu-id="0f4f0-116">Como alternativa, você pode implementar o `getDate()` método como um método de página dentro da página real do ASP.NET com o `DynamicPopulate` controle.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="0f4f0-117">Na próxima etapa, crie um novo arquivo do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="0f4f0-118">Como sempre, a primeira etapa é incluir o `ScriptManager` na página atual para carregar a biblioteca ASP.NET AJAX e fazer o trabalho do Kit de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="0f4f0-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="0f4f0-119">Em seguida, adicione um controle de rótulo (por exemplo, usando o controle HTML de mesmo nome, ou o &lt; `asp:Label`  / &gt; controle web) que mostrará o resultado da chamada de serviço web mais tarde.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

<span data-ttu-id="0f4f0-120">Um botão HTML (como um controle HTML, desde que não exigimos um postback para o servidor) será usado para disparar a população dinâmica:</span><span class="sxs-lookup"><span data-stu-id="0f4f0-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="0f4f0-121">Por fim, é necessário o `DynamicPopulateExtender` controle as coisas durante a transmissão.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="0f4f0-122">Os seguintes atributos serão definidos (além das óbvias, `ID` e `runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="0f4f0-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="0f4f0-123">`TargetControlID` onde colocar o resultado da chamada de serviço web</span><span class="sxs-lookup"><span data-stu-id="0f4f0-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="0f4f0-124">`ServicePath` caminho para o serviço da web (omitir se você quiser usar um método de página)</span><span class="sxs-lookup"><span data-stu-id="0f4f0-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="0f4f0-125">`ServiceMethod` nome do método da web ou método de página</span><span class="sxs-lookup"><span data-stu-id="0f4f0-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="0f4f0-126">`ContextKey` informações de contexto a ser enviado ao serviço web</span><span class="sxs-lookup"><span data-stu-id="0f4f0-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="0f4f0-127">`PopulateTriggerControlID` elemento que dispara a chamada de serviço web</span><span class="sxs-lookup"><span data-stu-id="0f4f0-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="0f4f0-128">`ClearContentsDuringUpdate` Se deseja esvaziar o elemento de destino durante a chamada de serviço web</span><span class="sxs-lookup"><span data-stu-id="0f4f0-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="0f4f0-129">Como você pode ver, o controle requer algumas informações mas colocar tudo em vigor é bastante direta.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="0f4f0-130">Aqui está a marcação para o `DynamicPopulateExtender` controle no cenário atual:</span><span class="sxs-lookup"><span data-stu-id="0f4f0-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="0f4f0-131">Execute a página ASP.NET no navegador e clique no botão; Você receberá a data atual no formato mês / dia / ano.</span><span class="sxs-lookup"><span data-stu-id="0f4f0-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="0f4f0-132">[![Um clique no botão recupera a data do servidor](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0f4f0-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="0f4f0-133">Um clique no botão recupera a data do servidor ([clique para exibir a imagem em tamanho normal](dynamically-populating-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0f4f0-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0f4f0-134">[Anterior](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Próximo](dynamically-populating-a-control-using-javascript-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0f4f0-134">[Previous](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
[Next](dynamically-populating-a-control-using-javascript-code-vb.md)</span></span>
