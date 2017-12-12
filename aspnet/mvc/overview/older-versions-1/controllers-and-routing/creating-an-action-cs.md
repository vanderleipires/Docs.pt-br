---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: "Criando uma ação (c#) | Microsoft Docs"
author: microsoft
description: "Saiba como adicionar uma nova ação a um controlador MVC do ASP.NET. Saiba mais sobre os requisitos para um método de uma ação."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b751dc7e34951be33e7c27a3429c383a3e1e1c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-action-c"></a><span data-ttu-id="4ffbc-104">Criando uma ação (c#)</span><span class="sxs-lookup"><span data-stu-id="4ffbc-104">Creating an Action (C#)</span></span>
====================
<span data-ttu-id="4ffbc-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4ffbc-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="4ffbc-106">Saiba como adicionar uma nova ação a um controlador MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="4ffbc-107">Saiba mais sobre os requisitos para um método de uma ação.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="4ffbc-108">O objetivo deste tutorial é explicar como você pode criar uma nova ação de controlador.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="4ffbc-109">Você aprenderá sobre os requisitos de um método de ação.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="4ffbc-110">Você também aprenderá como impedir que um método que está sendo exposto como uma ação.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="4ffbc-111">Adicionar uma ação a um controlador</span><span class="sxs-lookup"><span data-stu-id="4ffbc-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="4ffbc-112">Adicionar uma nova ação a um controlador, adicionando um novo método para o controlador.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="4ffbc-113">Por exemplo, o controlador na listagem 1 contém uma ação chamada index () e uma ação chamada sayHello ().</span><span class="sxs-lookup"><span data-stu-id="4ffbc-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="4ffbc-114">Ambos os métodos são expostos como ações.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="4ffbc-115">**Listando 1 - Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="4ffbc-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="4ffbc-116">Para ser exposto ao universo como uma ação, um método deve atender a certos requisitos:</span><span class="sxs-lookup"><span data-stu-id="4ffbc-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="4ffbc-117">O método deve ser público.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-117">The method must be public.</span></span>
- <span data-ttu-id="4ffbc-118">O método não pode ser um método estático.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="4ffbc-119">O método não pode ser um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="4ffbc-120">O método não pode ser um construtor, getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="4ffbc-121">O método não pode ter abertos tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="4ffbc-122">O método não é um método da classe base do controlador.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="4ffbc-123">O método não pode conter **ref** ou **out** parâmetros.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="4ffbc-124">Observe que não existem restrições no tipo de retorno de uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="4ffbc-125">Uma ação do controlador pode retornar uma cadeia de caracteres, uma data e hora, uma instância da classe Random, nulo ou vazio.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="4ffbc-126">A estrutura ASP.NET MVC converterá qualquer tipo de retorno não é o resultado de uma ação em uma cadeia de caracteres e processar a cadeia de caracteres para o navegador.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="4ffbc-127">Quando você adicionar qualquer método que não viole a esses requisitos para um controlador, o método é exposto como uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="4ffbc-128">Tenha cuidado aqui.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-128">Be careful here.</span></span> <span data-ttu-id="4ffbc-129">Uma ação do controlador pode ser chamada por qualquer usuário conectado à Internet.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="4ffbc-130">Não, por exemplo, crie uma ação do controlador DeleteMyWebsite().</span><span class="sxs-lookup"><span data-stu-id="4ffbc-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="4ffbc-131">Impedindo que um método público que está sendo invocado</span><span class="sxs-lookup"><span data-stu-id="4ffbc-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="4ffbc-132">Se você precisa criar um método público em uma classe de controlador e você não deseja expor o método como uma ação do controlador, em seguida, você pode impedir que o método que está sendo invocado usando o atributo [NonAction].</span><span class="sxs-lookup"><span data-stu-id="4ffbc-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="4ffbc-133">Por exemplo, o controlador na listagem 2 contém um método público chamado CompanySecrets() decorada com o atributo [NonAction].</span><span class="sxs-lookup"><span data-stu-id="4ffbc-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="4ffbc-134">**A listagem 2 - Controllers\WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="4ffbc-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="4ffbc-135">Se você tentar chamar a ação de controlador CompanySecrets() digitando /Work/CompanySecrets na barra de endereços do navegador, em seguida, você receberá a mensagem de erro na Figura 1.</span><span class="sxs-lookup"><span data-stu-id="4ffbc-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="4ffbc-136">[![Invocando um método NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4ffbc-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="4ffbc-137">**Figura 01**: invocando um método NonAction ([clique para exibir a imagem em tamanho normal](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4ffbc-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4ffbc-138">[Anterior](creating-a-controller-cs.md)
[Próximo](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4ffbc-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
