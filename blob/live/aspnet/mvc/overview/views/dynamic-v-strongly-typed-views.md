---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: "Dinâmico v. Fortemente tipado exibições | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="c22ef-103">Dinâmico v.</span><span class="sxs-lookup"><span data-stu-id="c22ef-103">Dynamic v.</span></span> <span data-ttu-id="c22ef-104">Modos de exibição fortemente tipados</span><span class="sxs-lookup"><span data-stu-id="c22ef-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="c22ef-105">Por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="c22ef-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="c22ef-106">Há três maneiras de transmitir informações de um controlador em uma exibição no ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="c22ef-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="c22ef-107">Como um objeto de modelo com rigidez de tipos.</span><span class="sxs-lookup"><span data-stu-id="c22ef-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="c22ef-108">Como um tipo dinâmico (usando @model dinâmico)</span><span class="sxs-lookup"><span data-stu-id="c22ef-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="c22ef-109">Usando a ViewBag</span><span class="sxs-lookup"><span data-stu-id="c22ef-109">Using the ViewBag</span></span>

<span data-ttu-id="c22ef-110">Eu escrevi um aplicativo MVC 3 superior Blog simples para comparar e contrastar exibições dinâmicas e fortemente tipadas.</span><span class="sxs-lookup"><span data-stu-id="c22ef-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="c22ef-111">O controlador começa com uma lista simple de blogs:</span><span class="sxs-lookup"><span data-stu-id="c22ef-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="c22ef-112">Clique com o botão direito no método IndexNotStonglyTyped() e adicionar um modo de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="c22ef-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="c22ef-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c22ef-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="c22ef-114">Verifique se o **criar uma exibição fortemente tipada** caixa não estiver marcada.</span><span class="sxs-lookup"><span data-stu-id="c22ef-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="c22ef-115">A exibição resultante não contêm muito:</span><span class="sxs-lookup"><span data-stu-id="c22ef-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="c22ef-116">Como estamos usando um dinâmico e não uma exibição fortemente tipada, o intellisense não ajude-nos.</span><span class="sxs-lookup"><span data-stu-id="c22ef-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="c22ef-117">O código completo é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="c22ef-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="c22ef-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c22ef-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="c22ef-119">Agora vamos adicionar uma exibição fortemente tipada.</span><span class="sxs-lookup"><span data-stu-id="c22ef-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="c22ef-120">Adicione o seguinte código para o controlador:</span><span class="sxs-lookup"><span data-stu-id="c22ef-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="c22ef-121">Observe que é exatamente é o View(topBlogs) retorno mesmo; chamar como o modo de exibição não fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="c22ef-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="c22ef-122">Clique com o botão direito dentro do *StonglyTypedIndex()* e selecione **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="c22ef-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="c22ef-123">Dessa vez, selecione o **Blog** classe de modelo e selecione **lista** como o modelo de Scaffold.</span><span class="sxs-lookup"><span data-stu-id="c22ef-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="c22ef-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c22ef-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="c22ef-125">Dentro do novo modelo de exibição, podemos obter suporte do intellisense.</span><span class="sxs-lookup"><span data-stu-id="c22ef-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="c22ef-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c22ef-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="c22ef-127">O projeto c# pode ser baixado [aqui](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="c22ef-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
