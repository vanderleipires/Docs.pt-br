---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Criar uma restrição de rota personalizada (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther demonstra como você pode criar uma restrição de rota personalizados. Implementamos um simples restrição personalizada que impede que uma rota que está sendo correspondido w...
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: e21e7e027cf66f390fc37ec08a07ae007e8242c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831807"
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="76e6b-104">Criar uma restrição de rota personalizada (c#)</span><span class="sxs-lookup"><span data-stu-id="76e6b-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="76e6b-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="76e6b-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="76e6b-106">Stephen Walther demonstra como você pode criar uma restrição de rota personalizados.</span><span class="sxs-lookup"><span data-stu-id="76e6b-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="76e6b-107">Implementamos uma restrição personalizada simple que impede que uma rota que está sendo correspondido quando é feita uma solicitação de navegador de um computador remoto.</span><span class="sxs-lookup"><span data-stu-id="76e6b-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="76e6b-108">O objetivo deste tutorial é demonstrar como você pode criar uma restrição de rota personalizados.</span><span class="sxs-lookup"><span data-stu-id="76e6b-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="76e6b-109">Uma restrição de rota personalizada permite impedir que uma rota que está sendo correspondido a menos que alguma condição personalizada é correspondida.</span><span class="sxs-lookup"><span data-stu-id="76e6b-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="76e6b-110">Neste tutorial, podemos criar uma restrição de rota do Localhost.</span><span class="sxs-lookup"><span data-stu-id="76e6b-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="76e6b-111">A restrição de rota Localhost corresponde apenas a solicitações feitas do computador local.</span><span class="sxs-lookup"><span data-stu-id="76e6b-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="76e6b-112">Não correspondem às solicitações remotas pela Internet.</span><span class="sxs-lookup"><span data-stu-id="76e6b-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="76e6b-113">Você pode implementar uma restrição de rota personalizada, Implementando a interface IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="76e6b-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="76e6b-114">Esta é uma interface muito simple que descreve um único método:</span><span class="sxs-lookup"><span data-stu-id="76e6b-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="76e6b-115">O método retorna um valor booliano.</span><span class="sxs-lookup"><span data-stu-id="76e6b-115">The method returns a Boolean value.</span></span> <span data-ttu-id="76e6b-116">Se você retornar false, a rota associada com a restrição não corresponde a solicitação do navegador.</span><span class="sxs-lookup"><span data-stu-id="76e6b-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="76e6b-117">A restrição de Localhost está contida na listagem 1.</span><span class="sxs-lookup"><span data-stu-id="76e6b-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="76e6b-118">**Listagem 1 - LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="76e6b-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="76e6b-119">A restrição na listagem 1 tira proveito da propriedade IsLocal exposto pela classe HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="76e6b-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="76e6b-120">Essa propriedade retorna true quando o endereço IP da solicitação é qualquer 127.0.0.1 ou quando o IP da solicitação é o mesmo endereço IP do servidor.</span><span class="sxs-lookup"><span data-stu-id="76e6b-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="76e6b-121">Você usar uma restrição personalizada dentro de uma rota definida no arquivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="76e6b-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="76e6b-122">O arquivo global asax na listagem 2 usa a restrição de Localhost para evitar que qualquer pessoa que solicite uma página de administração, a menos que eles fazem a solicitação do servidor local.</span><span class="sxs-lookup"><span data-stu-id="76e6b-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="76e6b-123">Por exemplo, uma solicitação para /Admin/DeleteAll falhará quando feita de um servidor remoto.</span><span class="sxs-lookup"><span data-stu-id="76e6b-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="76e6b-124">**Listagem 2 - global. asax**</span><span class="sxs-lookup"><span data-stu-id="76e6b-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="76e6b-125">A restrição de Localhost é usada na definição de rota de administrador.</span><span class="sxs-lookup"><span data-stu-id="76e6b-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="76e6b-126">Essa rota não ser correspondida por uma solicitação remota do navegador.</span><span class="sxs-lookup"><span data-stu-id="76e6b-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="76e6b-127">No entanto, observe que outras rotas definidas em global. asax podem corresponder a mesma solicitação.</span><span class="sxs-lookup"><span data-stu-id="76e6b-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="76e6b-128">É importante entender que uma restrição impede que uma rota específica de correspondência de uma solicitação e não todas as rotas definidas no arquivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="76e6b-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="76e6b-129">Observe que a rota padrão foi comentada do arquivo global. asax na listagem 2.</span><span class="sxs-lookup"><span data-stu-id="76e6b-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="76e6b-130">Se você incluir a rota padrão, a rota padrão corresponderia solicitações para o controlador de administrador.</span><span class="sxs-lookup"><span data-stu-id="76e6b-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="76e6b-131">Nesse caso, os usuários remotos ainda pode invocar ações do controlador de administrador, mesmo que suas solicitações não correspondem à rota Admin.</span><span class="sxs-lookup"><span data-stu-id="76e6b-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="76e6b-132">[Anterior](creating-a-route-constraint-cs.md)
> [Próximo](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="76e6b-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
