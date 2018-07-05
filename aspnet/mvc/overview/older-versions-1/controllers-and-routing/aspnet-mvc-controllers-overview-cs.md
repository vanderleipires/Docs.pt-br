---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Visão geral do controlador ASP.NET MVC (c#) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther apresenta controladores do ASP.NET MVC. Você aprenderá a criar novos controladores e retornar tipos diferentes de res de ação...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: e1834498d5a0f5d84f650faf32bb6352f2ad3e70
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370081"
---
<a name="aspnet-mvc-controller-overview-c"></a><span data-ttu-id="35da7-104">Visão geral do controlador ASP.NET MVC (c#)</span><span class="sxs-lookup"><span data-stu-id="35da7-104">ASP.NET MVC Controller Overview (C#)</span></span>
====================
<span data-ttu-id="35da7-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="35da7-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="35da7-106">Neste tutorial, Stephen Walther apresenta controladores do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="35da7-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="35da7-107">Você aprenderá a criar novos controladores e retornar tipos diferentes de resultados de ação.</span><span class="sxs-lookup"><span data-stu-id="35da7-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="35da7-108">Este tutorial explora o tópico de controladores, ações do controlador e resultados de ação do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="35da7-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="35da7-109">Depois de concluir este tutorial, você entenderá como os controladores são usados para controlar a maneira como um visitante interage com um site ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="35da7-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="35da7-110">Noções básicas sobre controladores</span><span class="sxs-lookup"><span data-stu-id="35da7-110">Understanding Controllers</span></span>

<span data-ttu-id="35da7-111">Controladores MVC serão responsáveis por responder a solicitações feitas em relação a um site ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="35da7-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="35da7-112">Cada solicitação do navegador é mapeada para um controlador específico.</span><span class="sxs-lookup"><span data-stu-id="35da7-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="35da7-113">Por exemplo, imagine que você insira a seguinte URL na barra de endereços do navegador:</span><span class="sxs-lookup"><span data-stu-id="35da7-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="35da7-114">Nesse caso, um controlador chamado ProductController é invocado.</span><span class="sxs-lookup"><span data-stu-id="35da7-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="35da7-115">ProductController é responsável por gerar a resposta para a solicitação do navegador.</span><span class="sxs-lookup"><span data-stu-id="35da7-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="35da7-116">Por exemplo, o controlador pode retornar uma exibição específica de volta para o navegador ou o controlador pode redirecionar o usuário para outro controlador.</span><span class="sxs-lookup"><span data-stu-id="35da7-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="35da7-117">Listagem 1 contém um controlador simples chamado ProductController.</span><span class="sxs-lookup"><span data-stu-id="35da7-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="35da7-118">**Listing1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="35da7-118">**Listing1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

<span data-ttu-id="35da7-119">Como você pode ver na listagem 1, um controlador é apenas uma classe (uma classe Visual Basic .NET ou c#).</span><span class="sxs-lookup"><span data-stu-id="35da7-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="35da7-120">Um controlador é uma classe que deriva da classe Controller base.</span><span class="sxs-lookup"><span data-stu-id="35da7-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="35da7-121">Como um controlador herda dessa classe base, um controlador herda os vários métodos úteis gratuitamente (abordaremos esses métodos em instantes).</span><span class="sxs-lookup"><span data-stu-id="35da7-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="35da7-122">Noções básicas sobre ações do controlador</span><span class="sxs-lookup"><span data-stu-id="35da7-122">Understanding Controller Actions</span></span>

<span data-ttu-id="35da7-123">Um controlador expõe as ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="35da7-123">A controller exposes controller actions.</span></span> <span data-ttu-id="35da7-124">Uma ação é um método em um controlador que é chamado quando você inserir uma URL específica na barra de endereços do navegador.</span><span class="sxs-lookup"><span data-stu-id="35da7-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="35da7-125">Por exemplo, imagine que você faça uma solicitação para a URL a seguir:</span><span class="sxs-lookup"><span data-stu-id="35da7-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="35da7-126">Nesse caso, o método Index () é chamado na classe ProductController.</span><span class="sxs-lookup"><span data-stu-id="35da7-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="35da7-127">O método Index () é um exemplo de uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="35da7-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="35da7-128">Uma ação do controlador deve ser um método público de uma classe de controlador.</span><span class="sxs-lookup"><span data-stu-id="35da7-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="35da7-129">Métodos c#, por padrão, são métodos privados.</span><span class="sxs-lookup"><span data-stu-id="35da7-129">C# methods, by default, are private methods.</span></span> <span data-ttu-id="35da7-130">Perceba que qualquer método público que você adicionar a uma classe de controlador é exposto como uma ação do controlador automaticamente (você deve ter cuidado isso vez que uma ação do controlador pode ser invocada por qualquer pessoa no universo simplesmente digitando a URL correta na barra de endereços do navegador).</span><span class="sxs-lookup"><span data-stu-id="35da7-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="35da7-131">Há alguns requisitos adicionais que devem ser atendidos por uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="35da7-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="35da7-132">Um método usado como uma ação de controlador não pode ser sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="35da7-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="35da7-133">Além disso, uma ação de controlador não pode ser um método estático.</span><span class="sxs-lookup"><span data-stu-id="35da7-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="35da7-134">Além disso, você pode usar qualquer método como uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="35da7-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="35da7-135">Noções básicas sobre resultados de ação</span><span class="sxs-lookup"><span data-stu-id="35da7-135">Understanding Action Results</span></span>

<span data-ttu-id="35da7-136">Uma ação do controlador retorna algo chamado de um *resultado da ação*.</span><span class="sxs-lookup"><span data-stu-id="35da7-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="35da7-137">Resultado de uma ação é o que uma ação do controlador retorna em resposta a uma solicitação do navegador.</span><span class="sxs-lookup"><span data-stu-id="35da7-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="35da7-138">O ASP.NET MVC framework oferece suporte a vários tipos de resultados de ação, incluindo:</span><span class="sxs-lookup"><span data-stu-id="35da7-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="35da7-139">ViewResult - representa HTML e marcação.</span><span class="sxs-lookup"><span data-stu-id="35da7-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="35da7-140">EmptyResult - não representa nenhum resultado.</span><span class="sxs-lookup"><span data-stu-id="35da7-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="35da7-141">RedirectResult - representa um redirecionamento para uma nova URL.</span><span class="sxs-lookup"><span data-stu-id="35da7-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="35da7-142">JsonResult - representa um resultado de JavaScript Object Notation que pode ser usado em um aplicativo AJAX.</span><span class="sxs-lookup"><span data-stu-id="35da7-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="35da7-143">JavaScriptResult - representa um script do JavaScript.</span><span class="sxs-lookup"><span data-stu-id="35da7-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="35da7-144">ContentResult - representa um resultado de texto.</span><span class="sxs-lookup"><span data-stu-id="35da7-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="35da7-145">FileContentResult - representa um arquivo para download (com o conteúdo binário).</span><span class="sxs-lookup"><span data-stu-id="35da7-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="35da7-146">FilePathResult - representa um arquivo para download (com um caminho).</span><span class="sxs-lookup"><span data-stu-id="35da7-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="35da7-147">FileStreamResult - representa um arquivo para download (com um fluxo de arquivos).</span><span class="sxs-lookup"><span data-stu-id="35da7-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="35da7-148">Todos esses resultados de ação herdam da classe ActionResult base.</span><span class="sxs-lookup"><span data-stu-id="35da7-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="35da7-149">Na maioria dos casos, uma ação do controlador retorna um ViewResult.</span><span class="sxs-lookup"><span data-stu-id="35da7-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="35da7-150">Por exemplo, a ação do controlador de índice na listagem 2 retorna um ViewResult.</span><span class="sxs-lookup"><span data-stu-id="35da7-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="35da7-151">**Listagem 2 - Controllers\BookController.cs**</span><span class="sxs-lookup"><span data-stu-id="35da7-151">**Listing 2 - Controllers\BookController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

<span data-ttu-id="35da7-152">Quando uma ação retorna um ViewResult, HTML é retornado ao navegador.</span><span class="sxs-lookup"><span data-stu-id="35da7-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="35da7-153">O método Index () na listagem 2 retorna uma exibição nomeada de índice para o navegador.</span><span class="sxs-lookup"><span data-stu-id="35da7-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="35da7-154">Observe que a ação Index () na listagem 2 não retorna um ViewResult().</span><span class="sxs-lookup"><span data-stu-id="35da7-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="35da7-155">Em vez disso, o método View() da classe Controller base é chamado.</span><span class="sxs-lookup"><span data-stu-id="35da7-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="35da7-156">Normalmente, você não retornar um resultado de ação diretamente.</span><span class="sxs-lookup"><span data-stu-id="35da7-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="35da7-157">Em vez disso, você chama um dos seguintes métodos da classe base do controlador:</span><span class="sxs-lookup"><span data-stu-id="35da7-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="35da7-158">Exibição - retorna um resultado de ação do ViewResult.</span><span class="sxs-lookup"><span data-stu-id="35da7-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="35da7-159">Redirecionar - retorna um resultado de ação RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="35da7-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="35da7-160">RedirectToAction - retorna um resultado de ação RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="35da7-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="35da7-161">RedirectToRoute - retorna um resultado de ação RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="35da7-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="35da7-162">JSON - retorna um resultado de ação JsonResult.</span><span class="sxs-lookup"><span data-stu-id="35da7-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="35da7-163">JavaScriptResult - retorna um JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="35da7-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="35da7-164">Conteúdo - retorna um resultado de ação ContentResult.</span><span class="sxs-lookup"><span data-stu-id="35da7-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="35da7-165">Arquivo - retorna um FileContentResult, FilePathResult ou FileStreamResult dependendo dos parâmetros passado para o método.</span><span class="sxs-lookup"><span data-stu-id="35da7-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="35da7-166">Portanto, se você quiser retornar uma exibição para o navegador, você chamar o método de View().</span><span class="sxs-lookup"><span data-stu-id="35da7-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="35da7-167">Se você deseja redirecionar o usuário da ação de um controlador para outro, você pode chamar o método de RedirectToAction().</span><span class="sxs-lookup"><span data-stu-id="35da7-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="35da7-168">Por exemplo, a ação de Details() na listagem 3 mostra uma exibição tanto redireciona o usuário para a ação Index (), dependendo se o parâmetro de Id tem um valor.</span><span class="sxs-lookup"><span data-stu-id="35da7-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="35da7-169">**Listagem 3 - CustomerController.cs**</span><span class="sxs-lookup"><span data-stu-id="35da7-169">**Listing 3 - CustomerController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

<span data-ttu-id="35da7-170">O resultado da ação ContentResult é especial.</span><span class="sxs-lookup"><span data-stu-id="35da7-170">The ContentResult action result is special.</span></span> <span data-ttu-id="35da7-171">Você pode usar o resultado da ação ContentResult para retornar o resultado de uma ação como texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="35da7-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="35da7-172">Por exemplo, o método Index () na listagem 4 retorna uma mensagem como texto sem formatação e não como HTML.</span><span class="sxs-lookup"><span data-stu-id="35da7-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="35da7-173">**Listagem 4 - Controllers\StatusController.cs**</span><span class="sxs-lookup"><span data-stu-id="35da7-173">**Listing 4 - Controllers\StatusController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

<span data-ttu-id="35da7-174">Quando a ação de StatusController.Index() é chamada, um modo de exibição não é retornado.</span><span class="sxs-lookup"><span data-stu-id="35da7-174">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="35da7-175">Em vez disso, o texto não processado "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="35da7-175">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="35da7-176">é retornado ao navegador.</span><span class="sxs-lookup"><span data-stu-id="35da7-176">is returned to the browser.</span></span>

<span data-ttu-id="35da7-177">Se uma ação do controlador retorna um resultado que é não um resultado de ação - por exemplo, uma data ou um inteiro -, em seguida, o resultado é encapsulado em um ContentResult automaticamente.</span><span class="sxs-lookup"><span data-stu-id="35da7-177">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="35da7-178">Por exemplo, quando a ação Index () do WorkController na listagem 5 é invocada, a data é retornada como um ContentResult automaticamente.</span><span class="sxs-lookup"><span data-stu-id="35da7-178">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="35da7-179">**Listagem 5 - WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="35da7-179">**Listing 5 - WorkController.cs**</span></span>

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

<span data-ttu-id="35da7-180">A ação Index () na listagem 5 retorna um objeto DateTime.</span><span class="sxs-lookup"><span data-stu-id="35da7-180">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="35da7-181">O ASP.NET MVC framework converte o objeto DateTime em uma cadeia de caracteres e encapsula o valor de data e hora em um ContentResult automaticamente.</span><span class="sxs-lookup"><span data-stu-id="35da7-181">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="35da7-182">O navegador recebe a data e hora como texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="35da7-182">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="35da7-183">Resumo</span><span class="sxs-lookup"><span data-stu-id="35da7-183">Summary</span></span>

<span data-ttu-id="35da7-184">A finalidade deste tutorial era apresentar a você os conceitos de controladores, ações do controlador e resultados de ação do controlador MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="35da7-184">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="35da7-185">Na primeira seção, você aprendeu a adicionar novos controladores para um projeto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="35da7-185">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="35da7-186">Em seguida, você aprendeu como públicos métodos de um controlador são expostos para o universo como ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="35da7-186">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="35da7-187">Por fim, discutimos os diferentes tipos de resultados de ações que podem ser retornados de uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="35da7-187">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="35da7-188">Em particular, discutimos como retornar um ViewResult, RedirectToActionResult e ContentResult de uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="35da7-188">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="35da7-189">[Anterior](creating-an-action-vb.md)
> [Próximo](creating-custom-routes-cs.md)</span><span class="sxs-lookup"><span data-stu-id="35da7-189">[Previous](creating-an-action-vb.md)
[Next](creating-custom-routes-cs.md)</span></span>
