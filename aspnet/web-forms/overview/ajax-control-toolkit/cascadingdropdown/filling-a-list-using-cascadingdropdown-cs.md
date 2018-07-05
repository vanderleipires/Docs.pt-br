---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Preenchendo uma lista usando o CascadingDropDown (c#) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associado valores em anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 12a4271b2697df8e24fca5f7ff30797b1e4e077a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385415"
---
<a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="9a54b-103">Preenchendo uma lista usando o CascadingDropDown (c#)</span><span class="sxs-lookup"><span data-stu-id="9a54b-103">Filling a List Using CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="9a54b-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9a54b-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9a54b-105">[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="9a54b-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="9a54b-106">O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="9a54b-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="9a54b-107">(Por exemplo, uma lista fornece uma lista de estados dos EUA e a lista seguinte, em seguida, é preenchida com principais cidades nesse estado.) O primeiro desafio para resolver é, na verdade, preencher uma lista suspensa usando esse controle.</span><span class="sxs-lookup"><span data-stu-id="9a54b-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="9a54b-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="9a54b-108">Overview</span></span>

<span data-ttu-id="9a54b-109">O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="9a54b-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="9a54b-110">(Por exemplo, uma lista fornece uma lista de estados dos EUA e a lista seguinte, em seguida, é preenchida com principais cidades nesse estado.) O primeiro desafio para resolver é, na verdade, preencher uma lista suspensa usando esse controle.</span><span class="sxs-lookup"><span data-stu-id="9a54b-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="9a54b-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="9a54b-111">Steps</span></span>

<span data-ttu-id="9a54b-112">Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="9a54b-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="9a54b-113">Em seguida, um controle DropDownList é necessário:</span><span class="sxs-lookup"><span data-stu-id="9a54b-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="9a54b-114">Para obter essa lista, um extensor de CascadingDropDown é adicionado.</span><span class="sxs-lookup"><span data-stu-id="9a54b-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="9a54b-115">Ele envia uma solicitação assíncrona para um serviço web que, em seguida, retornará uma lista de entradas a ser exibido na lista.</span><span class="sxs-lookup"><span data-stu-id="9a54b-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="9a54b-116">Para que isso funcione, os seguintes atributos de CascadingDropDown precisam ser definidas:</span><span class="sxs-lookup"><span data-stu-id="9a54b-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="9a54b-117">`ServicePath`: A URL de um serviço web fornecendo as entradas da lista</span><span class="sxs-lookup"><span data-stu-id="9a54b-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="9a54b-118">`ServiceMethod`: Método web fornecer as entradas da lista</span><span class="sxs-lookup"><span data-stu-id="9a54b-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="9a54b-119">`TargetControlID`: A ID da lista suspensa</span><span class="sxs-lookup"><span data-stu-id="9a54b-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="9a54b-120">`Category`: Informações de categoria que são enviadas para o método da web quando chamado</span><span class="sxs-lookup"><span data-stu-id="9a54b-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="9a54b-121">`PromptText`: Texto exibido quando assincronamente Carregando dados da lista do servidor</span><span class="sxs-lookup"><span data-stu-id="9a54b-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="9a54b-122">Aqui está a marcação para o `CascadingDropDown` elemento.</span><span class="sxs-lookup"><span data-stu-id="9a54b-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="9a54b-123">A única diferença entre c# e VB é o nome do serviço web associado:</span><span class="sxs-lookup"><span data-stu-id="9a54b-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="9a54b-124">O código JavaScript provenientes de `CascadingDropDown` extensor chama um método de serviço web com a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="9a54b-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="9a54b-125">Portanto, o aspecto importante é que o método deve retornar uma matriz do tipo `CascadingDropDownNameValue` (definido pelo ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="9a54b-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="9a54b-126">No `CascadingDropDownNameValue` construtor, primeiro texto da entrada de lista e, em seguida, seu valor devem ser fornecidas, assim como `<option value="VALUE">NAME</option>` faria em HTML.</span><span class="sxs-lookup"><span data-stu-id="9a54b-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="9a54b-127">Aqui estão alguns dados de exemplo:</span><span class="sxs-lookup"><span data-stu-id="9a54b-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="9a54b-128">Carregamento da página no navegador disparará a lista a ser preenchido com três fornecedores.</span><span class="sxs-lookup"><span data-stu-id="9a54b-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="9a54b-129">[![A lista é preenchida automaticamente](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9a54b-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="9a54b-130">A lista é preenchida automaticamente ([clique para exibir a imagem em tamanho normal](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9a54b-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9a54b-131">Avançar</span><span class="sxs-lookup"><span data-stu-id="9a54b-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
