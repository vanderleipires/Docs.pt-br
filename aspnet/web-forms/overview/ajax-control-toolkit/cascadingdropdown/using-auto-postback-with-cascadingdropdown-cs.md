---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Uso de Postback automático com CascadingDropDown (c#) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associado valores em anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: bece4f78b46c44db988e04e0e987450d94c37aab
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819372"
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a><span data-ttu-id="05621-103">Uso de Postback automático com CascadingDropDown (c#)</span><span class="sxs-lookup"><span data-stu-id="05621-103">Using Auto-Postback with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="05621-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="05621-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="05621-105">[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="05621-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span></span>

> <span data-ttu-id="05621-106">O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="05621-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="05621-107">No entanto ao usar o controle CascadingDropDown, ASP. Recurso de AutoPostBack do controle de DropDownList da rede não funciona, pois assincronamente Carregando dados na lista gera um postback (desnecessário) em si.</span><span class="sxs-lookup"><span data-stu-id="05621-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="05621-108">Com um código JavaScript, esse efeito pode ser evitado.</span><span class="sxs-lookup"><span data-stu-id="05621-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="05621-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="05621-109">Overview</span></span>

<span data-ttu-id="05621-110">O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="05621-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="05621-111">(Por exemplo, uma lista fornece uma lista de estados dos EUA e a lista seguinte, em seguida, é preenchida com principais cidades nesse estado.) No entanto ao usar o controle CascadingDropDown, ASP. Recurso de AutoPostBack do controle de DropDownList da rede não funciona, pois assincronamente Carregando dados na lista gera um postback (desnecessário) em si.</span><span class="sxs-lookup"><span data-stu-id="05621-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="05621-112">Com um código JavaScript, esse efeito pode ser evitado.</span><span class="sxs-lookup"><span data-stu-id="05621-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="05621-113">Etapas</span><span class="sxs-lookup"><span data-stu-id="05621-113">Steps</span></span>

<span data-ttu-id="05621-114">Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de &lt; `form` &gt; elemento):</span><span class="sxs-lookup"><span data-stu-id="05621-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="05621-115">Em seguida, um controle DropDownList é necessário:</span><span class="sxs-lookup"><span data-stu-id="05621-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="05621-116">Para obter essa lista, um extensor de CascadingDropDown é adicionado, fornecendo as informações de URL e o método de serviço web:</span><span class="sxs-lookup"><span data-stu-id="05621-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="05621-117">O extensor CascadingDropDown chama assincronamente um serviço web com a assinatura de método a seguir:</span><span class="sxs-lookup"><span data-stu-id="05621-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="05621-118">O método retorna uma matriz do tipo valor CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="05621-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="05621-119">O construtor do tipo primeiro espera legenda da entrada de lista e, em seguida, o valor (HTML `value` atributo).</span><span class="sxs-lookup"><span data-stu-id="05621-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="05621-120">Carregamento da página no navegador preencherá a lista suspensa com três fornecedores, o segundo é que está sendo pré-selecionado.</span><span class="sxs-lookup"><span data-stu-id="05621-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="05621-121">Além disso, o ASP.NET define o `__doPostBack()` método JavaScript.</span><span class="sxs-lookup"><span data-stu-id="05621-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="05621-122">Depois que a página foi carregada, essa chamada de JavaScript é adicionada à lista suspensa, mas somente se houver elementos nele.</span><span class="sxs-lookup"><span data-stu-id="05621-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="05621-123">Se não houver nenhum elemento na lista, o Kit de ferramentas de controle é carregá-los, no momento, para que o código JavaScript usa um tempo limite e tenta novamente em meio segundo.</span><span class="sxs-lookup"><span data-stu-id="05621-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

<span data-ttu-id="05621-124">Dessa forma, um postback é executado apenas quando há, na verdade, os elementos na lista e o usuário seleciona uma entrada.</span><span class="sxs-lookup"><span data-stu-id="05621-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="05621-125">[![Selecionar um elemento de lista faz com que um postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="05621-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="05621-126">Selecionar um elemento de lista faz com que um postback ([clique para exibir a imagem em tamanho normal](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="05621-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="05621-127">[Anterior](presetting-list-entries-with-cascadingdropdown-cs.md)
> [Próximo](filling-a-list-using-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="05621-127">[Previous](presetting-list-entries-with-cascadingdropdown-cs.md)
[Next](filling-a-list-using-cascadingdropdown-vb.md)</span></span>
