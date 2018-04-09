---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Criar uma restrição de rota (VB) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther demonstra como você pode controlar como o navegador solicita rotas de correspondência, criando restrições da rota com expressões regulares.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f50b371ac679218b06c4848e6d33516d29d3a82
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-route-constraint-vb"></a><span data-ttu-id="0b7a0-103">Criar uma restrição de rota (VB)</span><span class="sxs-lookup"><span data-stu-id="0b7a0-103">Creating a Route Constraint (VB)</span></span>
====================
<span data-ttu-id="0b7a0-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="0b7a0-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="0b7a0-105">Neste tutorial, Stephen Walther demonstra como você pode controlar como o navegador solicita rotas de correspondência, criando restrições da rota com expressões regulares.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="0b7a0-106">Você pode usar restrições da rota para restringir as solicitações do navegador que correspondam a uma rota específica.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="0b7a0-107">Você pode usar uma expressão regular para especificar uma restrição de rota.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="0b7a0-108">Por exemplo, imagine que você definiu a rota na listagem 1 no arquivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="0b7a0-109">**Listando 1 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="0b7a0-109">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="0b7a0-110">Listar 1 contém uma rota denominada produto.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="0b7a0-111">Você pode usar a rota de produto para mapear solicitações do navegador para o ProductController contido na listagem 2.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="0b7a0-112">**A listagem 2 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="0b7a0-112">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="0b7a0-113">Observe que a ação de Details() exposta pelo controlador de produto aceita um único parâmetro nomeado productId.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="0b7a0-114">Esse parâmetro é um parâmetro de número inteiro.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="0b7a0-115">A rota definida na listagem 1 corresponderá a qualquer uma das seguintes URLs:</span><span class="sxs-lookup"><span data-stu-id="0b7a0-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="0b7a0-116">/ Produto/23</span><span class="sxs-lookup"><span data-stu-id="0b7a0-116">/Product/23</span></span>
- <span data-ttu-id="0b7a0-117">/Product/7</span><span class="sxs-lookup"><span data-stu-id="0b7a0-117">/Product/7</span></span>

<span data-ttu-id="0b7a0-118">Infelizmente, a rota também serão compatíveis com as seguintes URLs:</span><span class="sxs-lookup"><span data-stu-id="0b7a0-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="0b7a0-119">/ Produtos/blah</span><span class="sxs-lookup"><span data-stu-id="0b7a0-119">/Product/blah</span></span>
- <span data-ttu-id="0b7a0-120">/ Produtos/apple</span><span class="sxs-lookup"><span data-stu-id="0b7a0-120">/Product/apple</span></span>

<span data-ttu-id="0b7a0-121">Porque a ação Details() espera um parâmetro de número inteiro, fazer uma solicitação que contém algo diferente de um valor inteiro causará um erro.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="0b7a0-122">Por exemplo, se você digitar a URL /Product/apple em seu navegador, em seguida, você obterá a página de erro na Figura 1.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="0b7a0-123">[![A caixa de diálogo Novo projeto](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0b7a0-123">[![The New Project dialog box](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span></span>

<span data-ttu-id="0b7a0-124">**Figura 01**: vendo uma página Detalhar ([clique para exibir a imagem em tamanho normal](creating-a-route-constraint-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="0b7a0-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>


<span data-ttu-id="0b7a0-125">O que você realmente deseja fazer é corresponder somente URLs que contêm um número inteiro apropriado productId.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="0b7a0-126">Você pode usar uma restrição ao definir uma rota para restringir as URLs que correspondem à rota.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="0b7a0-127">A rota de produto modificada na listagem 3 contém uma restrição de expressão regular que corresponde apenas números inteiros.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="0b7a0-128">**Listing 3 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="0b7a0-128">**Listing 3 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="0b7a0-129">\D+ a expressão regular corresponde a um ou mais inteiros.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="0b7a0-130">Essa restrição faz com que a rota de produto coincidir com as seguintes URLs:</span><span class="sxs-lookup"><span data-stu-id="0b7a0-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="0b7a0-131">/ Produto/3</span><span class="sxs-lookup"><span data-stu-id="0b7a0-131">/Product/3</span></span>
- <span data-ttu-id="0b7a0-132">/Product/8999</span><span class="sxs-lookup"><span data-stu-id="0b7a0-132">/Product/8999</span></span>

<span data-ttu-id="0b7a0-133">Mas não as seguintes URLs:</span><span class="sxs-lookup"><span data-stu-id="0b7a0-133">But not the following URLs:</span></span>

- <span data-ttu-id="0b7a0-134">/ Produtos/apple</span><span class="sxs-lookup"><span data-stu-id="0b7a0-134">/Product/apple</span></span>
- <span data-ttu-id="0b7a0-135">/ Produto</span><span class="sxs-lookup"><span data-stu-id="0b7a0-135">/Product</span></span>

<span data-ttu-id="0b7a0-136">Essas solicitações do navegador serão tratadas por outra rota ou, se não houver nenhuma rota correspondente, um *não foi possível encontrar o recurso* erro será retornado.</span><span class="sxs-lookup"><span data-stu-id="0b7a0-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0b7a0-137">[Anterior](creating-custom-routes-vb.md)
> [Próximo](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0b7a0-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>
