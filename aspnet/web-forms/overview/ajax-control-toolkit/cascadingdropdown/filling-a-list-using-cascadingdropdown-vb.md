---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Preenchendo uma lista usando o CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associado valores em anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: edd6c330d21a7875b8f90fcf9bbde75978b6d08d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389593"
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="86dba-103">Preenchendo uma lista usando o CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="86dba-103">Filling a List Using CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="86dba-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="86dba-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="86dba-105">[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="86dba-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="86dba-106">O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="86dba-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="86dba-107">(Por exemplo, uma lista fornece uma lista de estados dos EUA e a lista seguinte, em seguida, é preenchida com principais cidades nesse estado.) O primeiro desafio para resolver é, na verdade, preencher uma lista suspensa usando esse controle.</span><span class="sxs-lookup"><span data-stu-id="86dba-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="86dba-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="86dba-108">Overview</span></span>

<span data-ttu-id="86dba-109">O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="86dba-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="86dba-110">(Por exemplo, uma lista fornece uma lista de estados dos EUA e a lista seguinte, em seguida, é preenchida com principais cidades nesse estado.) O primeiro desafio para resolver é, na verdade, preencher uma lista suspensa usando esse controle.</span><span class="sxs-lookup"><span data-stu-id="86dba-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="86dba-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="86dba-111">Steps</span></span>

<span data-ttu-id="86dba-112">Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="86dba-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="86dba-113">Em seguida, um controle DropDownList é necessário:</span><span class="sxs-lookup"><span data-stu-id="86dba-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="86dba-114">Para obter essa lista, um extensor de CascadingDropDown é adicionado.</span><span class="sxs-lookup"><span data-stu-id="86dba-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="86dba-115">Ele envia uma solicitação assíncrona para um serviço web que, em seguida, retornará uma lista de entradas a ser exibido na lista.</span><span class="sxs-lookup"><span data-stu-id="86dba-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="86dba-116">Para que isso funcione, os seguintes atributos de CascadingDropDown precisam ser definidas:</span><span class="sxs-lookup"><span data-stu-id="86dba-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="86dba-117">`ServicePath`: A URL de um serviço web fornecendo as entradas da lista</span><span class="sxs-lookup"><span data-stu-id="86dba-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="86dba-118">`ServiceMethod`: Método web fornecer as entradas da lista</span><span class="sxs-lookup"><span data-stu-id="86dba-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="86dba-119">`TargetControlID`: A ID da lista suspensa</span><span class="sxs-lookup"><span data-stu-id="86dba-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="86dba-120">`Category`: Informações de categoria que são enviadas para o método da web quando chamado</span><span class="sxs-lookup"><span data-stu-id="86dba-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="86dba-121">`PromptText`: Texto exibido quando assincronamente Carregando dados da lista do servidor</span><span class="sxs-lookup"><span data-stu-id="86dba-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="86dba-122">Aqui está a marcação para o `CascadingDropDown` elemento.</span><span class="sxs-lookup"><span data-stu-id="86dba-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="86dba-123">A única diferença entre c# e VB é o nome do serviço web associado:</span><span class="sxs-lookup"><span data-stu-id="86dba-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="86dba-124">O código JavaScript provenientes de `CascadingDropDown` extensor chama um método de serviço web com a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="86dba-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="86dba-125">Portanto, o aspecto importante é que o método deve retornar uma matriz do tipo `CascadingDropDownNameValue` (definido pelo ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="86dba-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="86dba-126">No `CascadingDropDownNameValue` construtor, primeiro texto da entrada de lista e, em seguida, seu valor devem ser fornecidas, assim como `<option value="VALUE">NAME</option>` faria em HTML.</span><span class="sxs-lookup"><span data-stu-id="86dba-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="86dba-127">Aqui estão alguns dados de exemplo:</span><span class="sxs-lookup"><span data-stu-id="86dba-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="86dba-128">Carregamento da página no navegador disparará a lista a ser preenchido com três fornecedores.</span><span class="sxs-lookup"><span data-stu-id="86dba-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="86dba-129">[![A lista é preenchida automaticamente](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="86dba-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="86dba-130">A lista é preenchida automaticamente ([clique para exibir a imagem em tamanho normal](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="86dba-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="86dba-131">[Anterior](using-auto-postback-with-cascadingdropdown-cs.md)
> [Próximo](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="86dba-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
