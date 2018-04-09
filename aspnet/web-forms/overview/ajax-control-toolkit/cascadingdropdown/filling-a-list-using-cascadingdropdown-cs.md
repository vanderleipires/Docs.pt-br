---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Preenchendo uma lista usando CascadingDropDown (c#) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: c9e47f6484e49013004bf15084f98440ee67558e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="f1398-103">Preenchendo uma lista usando CascadingDropDown (c#)</span><span class="sxs-lookup"><span data-stu-id="f1398-103">Filling a List Using CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="f1398-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f1398-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f1398-105">[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f1398-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="f1398-106">O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="f1398-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="f1398-107">(Por exemplo, uma lista fornece uma lista de nós estados e lista seguinte é então preenchida com cidades nesse estado.) O primeiro desafio para resolver é realmente preencher uma lista suspensa usando o controle.</span><span class="sxs-lookup"><span data-stu-id="f1398-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="f1398-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="f1398-108">Overview</span></span>

<span data-ttu-id="f1398-109">O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="f1398-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="f1398-110">(Por exemplo, uma lista fornece uma lista de nós estados e lista seguinte é então preenchida com cidades nesse estado.) O primeiro desafio para resolver é realmente preencher uma lista suspensa usando o controle.</span><span class="sxs-lookup"><span data-stu-id="f1398-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="f1398-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="f1398-111">Steps</span></span>

<span data-ttu-id="f1398-112">Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="f1398-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="f1398-113">Em seguida, um controle DropDownList é necessário:</span><span class="sxs-lookup"><span data-stu-id="f1398-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="f1398-114">Para obter essa lista, um extensor de CascadingDropDown é adicionado.</span><span class="sxs-lookup"><span data-stu-id="f1398-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="f1398-115">Ele envia uma solicitação assíncrona para um serviço web que retornará uma lista de entradas a serem exibidos na lista.</span><span class="sxs-lookup"><span data-stu-id="f1398-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="f1398-116">Para que isso funcione, os seguintes atributos CascadingDropDown precisam ser definidos:</span><span class="sxs-lookup"><span data-stu-id="f1398-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="f1398-117">`ServicePath`: A URL de um serviço web fornecendo as entradas da lista</span><span class="sxs-lookup"><span data-stu-id="f1398-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="f1398-118">`ServiceMethod`: Método da web fornecendo as entradas da lista</span><span class="sxs-lookup"><span data-stu-id="f1398-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="f1398-119">`TargetControlID`: A ID da lista suspensa</span><span class="sxs-lookup"><span data-stu-id="f1398-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="f1398-120">`Category`: Informações de categoria que são enviadas ao método da web quando chamado</span><span class="sxs-lookup"><span data-stu-id="f1398-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="f1398-121">`PromptText`: Texto exibido quando assincronamente Carregando dados de lista do servidor</span><span class="sxs-lookup"><span data-stu-id="f1398-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="f1398-122">Aqui está a marcação para o `CascadingDropDown` elemento.</span><span class="sxs-lookup"><span data-stu-id="f1398-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="f1398-123">A única diferença entre c# e VB é o nome do serviço da web associada:</span><span class="sxs-lookup"><span data-stu-id="f1398-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="f1398-124">O código JavaScript provenientes de `CascadingDropDown` extensor chama um método de serviço web com a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="f1398-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="f1398-125">O aspecto importante é que o método deve retornar uma matriz do tipo `CascadingDropDownNameValue` (definido pelo Kit de ferramentas de controle AJAX ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="f1398-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="f1398-126">No `CascadingDropDownNameValue` construtor, primeiro texto da entrada da lista e, em seguida, seu valor devem ser fornecidos, assim como `<option value="VALUE">NAME</option>` faria em HTML.</span><span class="sxs-lookup"><span data-stu-id="f1398-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="f1398-127">Eis aqui alguns dados de exemplo:</span><span class="sxs-lookup"><span data-stu-id="f1398-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="f1398-128">Ao carregar a página no navegador disparará a lista a ser preenchida com três fornecedores.</span><span class="sxs-lookup"><span data-stu-id="f1398-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="f1398-129">[![A lista é preenchida automaticamente](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f1398-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="f1398-130">A lista é preenchida automaticamente ([clique para exibir a imagem em tamanho normal](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f1398-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f1398-131">Avançar</span><span class="sxs-lookup"><span data-stu-id="f1398-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
