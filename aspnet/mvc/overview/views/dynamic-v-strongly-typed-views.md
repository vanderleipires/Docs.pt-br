---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dinâmico v. Fortemente tipados exibições | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 7622ca8248374da27f4190075df5a6bfc32bb2e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389079"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="393f9-103">Dinâmico v.</span><span class="sxs-lookup"><span data-stu-id="393f9-103">Dynamic v.</span></span> <span data-ttu-id="393f9-104">Exibições fortemente tipadas</span><span class="sxs-lookup"><span data-stu-id="393f9-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="393f9-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="393f9-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="393f9-106">Há três maneiras de transmitir informações de um controlador para um modo de exibição no ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="393f9-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="393f9-107">Como um objeto de modelo fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="393f9-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="393f9-108">Como um tipo dinâmico (usando @model dinâmico)</span><span class="sxs-lookup"><span data-stu-id="393f9-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="393f9-109">Usando a ViewBag</span><span class="sxs-lookup"><span data-stu-id="393f9-109">Using the ViewBag</span></span>

<span data-ttu-id="393f9-110">Escrevi um aplicativo simples do Blog de parte superior do MVC 3 para comparar e contrastar os modos de exibição dinâmicos e com rigidez de tipos.</span><span class="sxs-lookup"><span data-stu-id="393f9-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="393f9-111">O controlador começa com uma simple lista de blogs:</span><span class="sxs-lookup"><span data-stu-id="393f9-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="393f9-112">Clique com botão direito no método IndexNotStonglyTyped() e adicione um modo de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="393f9-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="393f9-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="393f9-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="393f9-114">Verifique se o **criar uma exibição fortemente tipada** caixa não estiver marcada.</span><span class="sxs-lookup"><span data-stu-id="393f9-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="393f9-115">A exibição resultante não contêm muito:</span><span class="sxs-lookup"><span data-stu-id="393f9-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="393f9-116">Como estamos usando um dinâmico e não uma exibição fortemente tipada, o intellisense não ajude-nos.</span><span class="sxs-lookup"><span data-stu-id="393f9-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="393f9-117">O código completo é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="393f9-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="393f9-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="393f9-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="393f9-119">Agora vamos adicionar uma exibição fortemente tipada.</span><span class="sxs-lookup"><span data-stu-id="393f9-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="393f9-120">Adicione o seguinte código para o controlador:</span><span class="sxs-lookup"><span data-stu-id="393f9-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="393f9-121">Observe que é exatamente é o View(topBlogs) retorno mesmo; Chame como o modo de exibição não fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="393f9-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="393f9-122">Clique com botão direito dentro do *StonglyTypedIndex()* e selecione **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="393f9-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="393f9-123">Dessa vez, selecione o **Blog** classe de modelo e selecione **lista** como o modelo de Scaffold.</span><span class="sxs-lookup"><span data-stu-id="393f9-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="393f9-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="393f9-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="393f9-125">Dentro do novo modelo de exibição, podemos obter suporte do intellisense.</span><span class="sxs-lookup"><span data-stu-id="393f9-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="393f9-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="393f9-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="393f9-127">O projeto do c# pode ser baixado [aqui](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="393f9-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
