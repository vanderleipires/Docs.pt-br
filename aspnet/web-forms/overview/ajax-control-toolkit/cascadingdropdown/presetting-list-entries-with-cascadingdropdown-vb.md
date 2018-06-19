---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Predefinição de entradas da lista com CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f74e6ac80b756240870d9406a03db11c610093aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869888"
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="3345a-103">Predefinição de entradas da lista com CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="3345a-103">Presetting List Entries with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="3345a-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3345a-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3345a-105">[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3345a-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="3345a-106">O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="3345a-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="3345a-107">Com um pouco de código, é possível que um elemento de lista é pré-selecionado depois que os dados forem carregados dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="3345a-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="3345a-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="3345a-108">Overview</span></span>

<span data-ttu-id="3345a-109">O controle CascadingDropDown AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma carga de DropDownList associados valores em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="3345a-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="3345a-110">(Por exemplo, uma lista fornece uma lista de nós estados e lista seguinte é então preenchida com cidades nesse estado.) Com um pouco de código, é possível que um elemento de lista é pré-selecionado depois que os dados forem carregados dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="3345a-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="3345a-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="3345a-111">Steps</span></span>

<span data-ttu-id="3345a-112">Para ativar a funcionalidade do ASP.NET AJAX e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocado em qualquer lugar na página (mas dentro do `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="3345a-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="3345a-113">Em seguida, um controle DropDownList é necessário:</span><span class="sxs-lookup"><span data-stu-id="3345a-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="3345a-114">Para esta lista é adicionado um extensor CascadingDropDown, fornecendo as informações de URL e o método de serviço web:</span><span class="sxs-lookup"><span data-stu-id="3345a-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="3345a-115">O extensor CascadingDropDown chama assincronamente um serviço web com a assinatura de método a seguir:</span><span class="sxs-lookup"><span data-stu-id="3345a-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="3345a-116">O método retorna uma matriz de tipo de valor de CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="3345a-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="3345a-117">O construtor do tipo primeiro espera legenda da entrada da lista e, em seguida, o valor (HTML `value` atributo).</span><span class="sxs-lookup"><span data-stu-id="3345a-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="3345a-118">Se o terceiro argumento é definido como true, a lista de elemento é selecionado automaticamente no navegador.</span><span class="sxs-lookup"><span data-stu-id="3345a-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="3345a-119">Ao carregar a página no navegador preencher a lista suspensa com três fornecedores, o outro sendo pré-selecionados.</span><span class="sxs-lookup"><span data-stu-id="3345a-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="3345a-120">[![A lista é preenchida e pré-selecionados automaticamente](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3345a-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="3345a-121">A lista é preenchida e automaticamente pré-selecionados ([clique para exibir a imagem em tamanho normal](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3345a-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3345a-122">[Anterior](using-cascadingdropdown-with-a-database-vb.md)
> [Próximo](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3345a-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>
