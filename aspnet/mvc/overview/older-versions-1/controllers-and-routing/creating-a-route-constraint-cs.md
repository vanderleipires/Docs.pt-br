---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Criar uma restrição de rota (c#) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther demonstra como você pode controlar como o navegador solicita as rotas de correspondência, criando restrições de rota com expressões regulares.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 8c977df126ce79f6ca20bd3941009ae7295ae0a5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366526"
---
<a name="creating-a-route-constraint-c"></a><span data-ttu-id="e0815-103">Criar uma restrição de rota (c#)</span><span class="sxs-lookup"><span data-stu-id="e0815-103">Creating a Route Constraint (C#)</span></span>
====================
<span data-ttu-id="e0815-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="e0815-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="e0815-105">Neste tutorial, Stephen Walther demonstra como você pode controlar como o navegador solicita as rotas de correspondência, criando restrições de rota com expressões regulares.</span><span class="sxs-lookup"><span data-stu-id="e0815-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="e0815-106">Você pode usar restrições de rota para restringir as solicitações de navegador que correspondam a uma rota específica.</span><span class="sxs-lookup"><span data-stu-id="e0815-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="e0815-107">Você pode usar uma expressão regular para especificar uma restrição de rota.</span><span class="sxs-lookup"><span data-stu-id="e0815-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="e0815-108">Por exemplo, imagine que você definiu a rota na listagem 1 no arquivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="e0815-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="e0815-109">**Listagem 1 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="e0815-109">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="e0815-110">Listagem 1 contém uma rota chamada Product.</span><span class="sxs-lookup"><span data-stu-id="e0815-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="e0815-111">Você pode usar a rota de produto para mapear as solicitações do navegador para ProductController contida na listagem 2.</span><span class="sxs-lookup"><span data-stu-id="e0815-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="e0815-112">**Listagem 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="e0815-112">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="e0815-113">Observe que a ação de Details() exposta pelo controlador de produto aceita um parâmetro único chamado productId.</span><span class="sxs-lookup"><span data-stu-id="e0815-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="e0815-114">Esse parâmetro é um parâmetro de número inteiro.</span><span class="sxs-lookup"><span data-stu-id="e0815-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="e0815-115">A rota definida na listagem 1 corresponderá a qualquer uma das seguintes URLs:</span><span class="sxs-lookup"><span data-stu-id="e0815-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="e0815-116">/ Produto/23</span><span class="sxs-lookup"><span data-stu-id="e0815-116">/Product/23</span></span>
- <span data-ttu-id="e0815-117">/ Produto/7</span><span class="sxs-lookup"><span data-stu-id="e0815-117">/Product/7</span></span>

<span data-ttu-id="e0815-118">Infelizmente, a rota também serão compatíveis com as seguintes URLs:</span><span class="sxs-lookup"><span data-stu-id="e0815-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="e0815-119">/ Produto/blah</span><span class="sxs-lookup"><span data-stu-id="e0815-119">/Product/blah</span></span>
- <span data-ttu-id="e0815-120">/ Produto/apple</span><span class="sxs-lookup"><span data-stu-id="e0815-120">/Product/apple</span></span>

<span data-ttu-id="e0815-121">Porque a ação Details() espera um parâmetro de número inteiro, fazendo uma solicitação que contém algo diferente de um valor inteiro causará um erro.</span><span class="sxs-lookup"><span data-stu-id="e0815-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="e0815-122">Por exemplo, se você digitar a URL /Product/apple no seu navegador, em seguida, você obterá a página de erro na Figura 1.</span><span class="sxs-lookup"><span data-stu-id="e0815-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="e0815-123">[![A caixa de diálogo Novo projeto](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e0815-123">[![The New Project dialog box](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span></span>

<span data-ttu-id="e0815-124">**Figura 01**: vendo uma página explodir ([clique para exibir a imagem em tamanho normal](creating-a-route-constraint-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="e0815-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-cs/_static/image2.png))</span></span>


<span data-ttu-id="e0815-125">O que você realmente deseja fazer é correspondente somente a URLs que contêm um productId inteiro apropriado.</span><span class="sxs-lookup"><span data-stu-id="e0815-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="e0815-126">Você pode usar uma restrição ao definir uma rota para restringir as URLs que correspondem à rota.</span><span class="sxs-lookup"><span data-stu-id="e0815-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="e0815-127">A rota de produto modificada na listagem 3 contém uma restrição de expressão regular que corresponde apenas números inteiros.</span><span class="sxs-lookup"><span data-stu-id="e0815-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="e0815-128">**Listagem 3 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="e0815-128">**Listing 3 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="e0815-129">A expressão regular \d+ corresponde a um ou mais números inteiros.</span><span class="sxs-lookup"><span data-stu-id="e0815-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="e0815-130">Essa restrição faz com que a rota de produto coincidir com as seguintes URLs:</span><span class="sxs-lookup"><span data-stu-id="e0815-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="e0815-131">/ Produto/3</span><span class="sxs-lookup"><span data-stu-id="e0815-131">/Product/3</span></span>
- <span data-ttu-id="e0815-132">/ Produto/8999</span><span class="sxs-lookup"><span data-stu-id="e0815-132">/Product/8999</span></span>

<span data-ttu-id="e0815-133">Mas não as URLs a seguir:</span><span class="sxs-lookup"><span data-stu-id="e0815-133">But not the following URLs:</span></span>

- <span data-ttu-id="e0815-134">/ Produto/apple</span><span class="sxs-lookup"><span data-stu-id="e0815-134">/Product/apple</span></span>
- <span data-ttu-id="e0815-135">/ Produto</span><span class="sxs-lookup"><span data-stu-id="e0815-135">/Product</span></span>

- <span data-ttu-id="e0815-136">Essas solicitações do navegador serão tratadas por outra rota ou, se não houver rotas correspondentes, uma *não foi possível encontrar o recurso* erro será retornado.</span><span class="sxs-lookup"><span data-stu-id="e0815-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e0815-137">[Anterior](creating-custom-routes-cs.md)
> [Próximo](creating-a-custom-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e0815-137">[Previous](creating-custom-routes-cs.md)
[Next](creating-a-custom-route-constraint-cs.md)</span></span>
