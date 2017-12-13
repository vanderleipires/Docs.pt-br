---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: "Validando com uma camada de serviço (c#) | Microsoft Docs"
author: StephenWalther
description: "Saiba como mover a lógica de validação de suas ações do controlador e em uma camada de serviço separado. Neste tutorial, Stephen Walther explica como você..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: f36301aef4377c6c00cb4fc33dbc5c57b1c426a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-a-service-layer-c"></a><span data-ttu-id="d674d-104">Validando com uma camada de serviço (c#)</span><span class="sxs-lookup"><span data-stu-id="d674d-104">Validating with a Service Layer (C#)</span></span>
====================
<span data-ttu-id="d674d-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="d674d-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="d674d-106">Saiba como mover a lógica de validação de suas ações do controlador e em uma camada de serviço separado.</span><span class="sxs-lookup"><span data-stu-id="d674d-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="d674d-107">Neste tutorial, Stephen Walther explica como você pode manter uma curva separação de preocupações isolando sua camada de serviço de sua camada de controlador.</span><span class="sxs-lookup"><span data-stu-id="d674d-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="d674d-108">O objetivo deste tutorial é descrever um método de executar a validação em um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d674d-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="d674d-109">Neste tutorial, você aprenderá a mover sua lógica de validação de seus controladores e em uma camada de serviço separado.</span><span class="sxs-lookup"><span data-stu-id="d674d-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="d674d-110">Separação de preocupações</span><span class="sxs-lookup"><span data-stu-id="d674d-110">Separating Concerns</span></span>

<span data-ttu-id="d674d-111">Quando você compila um aplicativo ASP.NET MVC, você não deve colocar a lógica de banco de dados dentro de suas ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="d674d-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="d674d-112">Combinação de sua lógica de banco de dados e o controlador torna o seu aplicativo mais difíceis de manter ao longo do tempo.</span><span class="sxs-lookup"><span data-stu-id="d674d-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="d674d-113">A recomendação é que você coloque toda sua lógica de banco de dados em uma camada de repositório separado.</span><span class="sxs-lookup"><span data-stu-id="d674d-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="d674d-114">Por exemplo, a listagem 1 contém um repositório simple chamado o ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="d674d-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="d674d-115">O repositório de produto contém todo o código de acesso de dados para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d674d-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="d674d-116">A lista também inclui a interface IProductRepository que implementa o repositório de produto.</span><span class="sxs-lookup"><span data-stu-id="d674d-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="d674d-117">**Listando 1--Models\ProductRepository.cs**</span><span class="sxs-lookup"><span data-stu-id="d674d-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="d674d-118">O controlador na listagem 2 usa a camada de repositório em ações sua Index () e Create ().</span><span class="sxs-lookup"><span data-stu-id="d674d-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="d674d-119">Observe que esse controlador não contêm nenhuma lógica de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d674d-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="d674d-120">Criando uma camada de repositório permite que você mantenha uma separação limpa de preocupações.</span><span class="sxs-lookup"><span data-stu-id="d674d-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="d674d-121">Os controladores são responsáveis pela lógica de controle de fluxo do aplicativo e o repositório é responsável pela lógica de acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="d674d-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="d674d-122">**A listagem 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="d674d-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="d674d-123">Criando uma camada de serviço</span><span class="sxs-lookup"><span data-stu-id="d674d-123">Creating a Service Layer</span></span>

<span data-ttu-id="d674d-124">Portanto, lógica de controle de fluxo do aplicativo pertence a um controlador e lógica de acesso a dados pertence em um repositório.</span><span class="sxs-lookup"><span data-stu-id="d674d-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="d674d-125">Nesse caso, onde são inseridos a lógica de validação?</span><span class="sxs-lookup"><span data-stu-id="d674d-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="d674d-126">Uma opção é colocar sua lógica de validação em um *camada de serviço*.</span><span class="sxs-lookup"><span data-stu-id="d674d-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="d674d-127">Uma camada de serviço é uma camada adicional em um aplicativo ASP.NET MVC que atua como mediador comunicação entre um controlador e a camada de repositório.</span><span class="sxs-lookup"><span data-stu-id="d674d-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="d674d-128">A camada de serviço contém a lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="d674d-128">The service layer contains business logic.</span></span> <span data-ttu-id="d674d-129">Em particular, ele contém a lógica de validação.</span><span class="sxs-lookup"><span data-stu-id="d674d-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="d674d-130">Por exemplo, a camada de serviço do produto na listagem 3 tem um método CreateProduct().</span><span class="sxs-lookup"><span data-stu-id="d674d-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="d674d-131">O método CreateProduct() chama o método de ValidateProduct() para validar um novo produto antes de passar o produto para o repositório de produto.</span><span class="sxs-lookup"><span data-stu-id="d674d-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="d674d-132">**A listagem 3 - Models\ProductService.cs**</span><span class="sxs-lookup"><span data-stu-id="d674d-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="d674d-133">O controlador de produto foi atualizado na listagem 4 para usar a camada de serviço em vez de camada de repositório.</span><span class="sxs-lookup"><span data-stu-id="d674d-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="d674d-134">A camada de controlador se comunica com a camada de serviço.</span><span class="sxs-lookup"><span data-stu-id="d674d-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="d674d-135">A camada de serviço se comunica com a camada de repositório.</span><span class="sxs-lookup"><span data-stu-id="d674d-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="d674d-136">Cada camada tem a responsabilidade separada.</span><span class="sxs-lookup"><span data-stu-id="d674d-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="d674d-137">**A listagem 4 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="d674d-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="d674d-138">Observe que o serviço de produto é criado no construtor do controlador de produto.</span><span class="sxs-lookup"><span data-stu-id="d674d-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="d674d-139">Quando o serviço de produto é criado, o dicionário de estado de modelo é passado para o serviço.</span><span class="sxs-lookup"><span data-stu-id="d674d-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="d674d-140">O serviço de produto usa o estado de modelo para passar mensagens de erro de validação o controlador.</span><span class="sxs-lookup"><span data-stu-id="d674d-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="d674d-141">Separando a camada de serviço</span><span class="sxs-lookup"><span data-stu-id="d674d-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="d674d-142">Conseguimos isolar o controlador e as camadas de serviço em um aspecto.</span><span class="sxs-lookup"><span data-stu-id="d674d-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="d674d-143">O controlador e as camadas de serviço se comunicam através de estado do modelo.</span><span class="sxs-lookup"><span data-stu-id="d674d-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="d674d-144">Em outras palavras, a camada de serviço tem uma dependência em um recurso específico da estrutura ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d674d-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="d674d-145">Queremos isolar a camada de serviço da nossa camada controlador tanto quanto possível.</span><span class="sxs-lookup"><span data-stu-id="d674d-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="d674d-146">Em teoria, podemos deve ser capazes de usar a camada de serviço com qualquer tipo de aplicativo e não apenas um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d674d-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="d674d-147">Por exemplo, no futuro, podemos talvez queira criar um front-end para nosso aplicativo do WPF.</span><span class="sxs-lookup"><span data-stu-id="d674d-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="d674d-148">Deve encontrar uma maneira de remover a dependência do ASP.NET MVC estado do modelo da camada de nosso serviço.</span><span class="sxs-lookup"><span data-stu-id="d674d-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="d674d-149">Na listagem 5, a camada de serviço foi atualizada para que ele não usa o estado do modelo.</span><span class="sxs-lookup"><span data-stu-id="d674d-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="d674d-150">Em vez disso, ele usa qualquer classe que implementa a interface IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="d674d-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="d674d-151">**Listagem 5 - Models\ProductService.cs (desacoplado)**</span><span class="sxs-lookup"><span data-stu-id="d674d-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="d674d-152">A interface IValidationDictionary é definida na listagem 6.</span><span class="sxs-lookup"><span data-stu-id="d674d-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="d674d-153">Essa interface simple tem um único método e uma única propriedade.</span><span class="sxs-lookup"><span data-stu-id="d674d-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="d674d-154">**Listando 6 - Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="d674d-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="d674d-155">A classe na listagem 7, nomeada classe ModelStateWrapper, implemente a interface IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="d674d-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="d674d-156">Você pode instanciar a classe ModelStateWrapper passando um dicionário de estado de modelo para o construtor.</span><span class="sxs-lookup"><span data-stu-id="d674d-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="d674d-157">**Listando 7 - Models\ModelStateWrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="d674d-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="d674d-158">Por fim, o controlador atualizado na listagem 8 usa o ModelStateWrapper ao criar a camada de serviço no seu construtor.</span><span class="sxs-lookup"><span data-stu-id="d674d-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="d674d-159">**Listando 8 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="d674d-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="d674d-160">Usando o IValidationDictionary interface e a classe ModelStateWrapper nos permite isolar completamente nossa camada de serviço de camada nosso controlador.</span><span class="sxs-lookup"><span data-stu-id="d674d-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="d674d-161">A camada de serviço não é dependente de estado do modelo.</span><span class="sxs-lookup"><span data-stu-id="d674d-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="d674d-162">Você pode passar qualquer classe que implementa a interface IValidationDictionary para a camada de serviço.</span><span class="sxs-lookup"><span data-stu-id="d674d-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="d674d-163">Por exemplo, um aplicativo WPF pode implementar a interface IValidationDictionary com uma classe de coleção simples.</span><span class="sxs-lookup"><span data-stu-id="d674d-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="d674d-164">Resumo</span><span class="sxs-lookup"><span data-stu-id="d674d-164">Summary</span></span>

<span data-ttu-id="d674d-165">O objetivo deste tutorial era discutir uma abordagem para executar a validação em um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d674d-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="d674d-166">Neste tutorial, você aprendeu como mover todos os de sua lógica de validação de seus controladores e em uma camada de serviço separado.</span><span class="sxs-lookup"><span data-stu-id="d674d-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="d674d-167">Você também aprendeu como isolar sua camada de serviço de sua camada de controlador, criando uma classe ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="d674d-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d674d-168">[Anterior](validating-with-the-idataerrorinfo-interface-cs.md)
[Próximo](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="d674d-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
