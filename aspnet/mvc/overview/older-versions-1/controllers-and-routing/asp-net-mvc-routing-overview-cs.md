---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Visão geral o roteamento do ASP.NET MVC (c#) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther mostra como o ASP.NET MVC framework mapeia as solicitações do navegador para ações do controlador.
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b43c3dca1587d79a60c5a89a451dda989403d56
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819141"
---
<a name="aspnet-mvc-routing-overview-c"></a><span data-ttu-id="8edd0-103">Visão geral o roteamento do ASP.NET MVC (c#)</span><span class="sxs-lookup"><span data-stu-id="8edd0-103">ASP.NET MVC Routing Overview (C#)</span></span>
====================
<span data-ttu-id="8edd0-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="8edd0-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="8edd0-105">Neste tutorial, Stephen Walther mostra como o ASP.NET MVC framework mapeia as solicitações do navegador para ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="8edd0-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="8edd0-106">Neste tutorial, você será apresentado a um recurso importante de todos os aplicativos ASP.NET MVC chamado *roteamento do ASP.NET*.</span><span class="sxs-lookup"><span data-stu-id="8edd0-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="8edd0-107">O módulo de roteamento do ASP.NET é responsável por mapear solicitações recebidas do navegador para determinadas ações do controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="8edd0-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="8edd0-108">No final deste tutorial, você entenderá como a tabela de rota padrão mapeia solicitações para ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="8edd0-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="8edd0-109">Usando a tabela de rota padrão</span><span class="sxs-lookup"><span data-stu-id="8edd0-109">Using the Default Route Table</span></span>

<span data-ttu-id="8edd0-110">Quando você cria um novo aplicativo ASP.NET MVC, o aplicativo já está configurado para usar o roteamento do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8edd0-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="8edd0-111">O roteamento do ASP.NET está configurado em dois locais.</span><span class="sxs-lookup"><span data-stu-id="8edd0-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="8edd0-112">Primeiro, o roteamento do ASP.NET está habilitado no arquivo de configuração do seu aplicativo Web (arquivo Web. config).</span><span class="sxs-lookup"><span data-stu-id="8edd0-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="8edd0-113">Há quatro seções no arquivo de configuração que são relevantes para roteamento: seção system.web.httpModules, a seção system.web.httpHandlers, a seção system.webserver.modules e a seção system.webserver.handlers.</span><span class="sxs-lookup"><span data-stu-id="8edd0-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="8edd0-114">Tenha cuidado para não excluir essas seções, porque sem essas seções roteamento deixarão de funcionar.</span><span class="sxs-lookup"><span data-stu-id="8edd0-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="8edd0-115">Em segundo lugar e mais importante, uma tabela de rotas é criada no arquivo global asax do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8edd0-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="8edd0-116">O arquivo global asax é um arquivo especial que contém os manipuladores de eventos para eventos de ciclo de vida do aplicativo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8edd0-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="8edd0-117">A tabela de rotas é criada durante o evento de início do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8edd0-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="8edd0-118">O arquivo na listagem 1 contém o arquivo global asax padrão para um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8edd0-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="8edd0-119">**Listagem 1 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="8edd0-119">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

<span data-ttu-id="8edd0-120">Quando um aplicativo MVC é iniciado, o aplicativo\_método Start () é chamado.</span><span class="sxs-lookup"><span data-stu-id="8edd0-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="8edd0-121">Esse método, por sua vez, chama o método de RegisterRoutes().</span><span class="sxs-lookup"><span data-stu-id="8edd0-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="8edd0-122">O método RegisterRoutes() cria a tabela de rotas.</span><span class="sxs-lookup"><span data-stu-id="8edd0-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="8edd0-123">A tabela de rotas padrão contém uma única rota (denominada Default).</span><span class="sxs-lookup"><span data-stu-id="8edd0-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="8edd0-124">A rota padrão mapeia o primeiro segmento de URL para um nome de controlador, o segundo segmento de URL para uma ação do controlador e o terceiro segmento para um parâmetro denominado **id**.</span><span class="sxs-lookup"><span data-stu-id="8edd0-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="8edd0-125">Imagine que você insira a seguinte URL na barra de endereços do navegador da web:</span><span class="sxs-lookup"><span data-stu-id="8edd0-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="8edd0-126">Página inicial/3/índice</span><span class="sxs-lookup"><span data-stu-id="8edd0-126">/Home/Index/3</span></span>

<span data-ttu-id="8edd0-127">A rota padrão mapeia essa URL para os seguintes parâmetros:</span><span class="sxs-lookup"><span data-stu-id="8edd0-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="8edd0-128">controlador = início</span><span class="sxs-lookup"><span data-stu-id="8edd0-128">controller = Home</span></span>

- <span data-ttu-id="8edd0-129">ação = índice</span><span class="sxs-lookup"><span data-stu-id="8edd0-129">action = Index</span></span>

- <span data-ttu-id="8edd0-130">id = 3</span><span class="sxs-lookup"><span data-stu-id="8edd0-130">id = 3</span></span>

<span data-ttu-id="8edd0-131">Quando você solicita 3/índice/URL /Home, o código a seguir é executado:</span><span class="sxs-lookup"><span data-stu-id="8edd0-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="8edd0-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="8edd0-132">HomeController.Index(3)</span></span>

<span data-ttu-id="8edd0-133">A rota padrão inclui os padrões para todos os três parâmetros.</span><span class="sxs-lookup"><span data-stu-id="8edd0-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="8edd0-134">Se você não fornecer um controlador, em seguida, o parâmetro de controlador padrão para o valor **Home**.</span><span class="sxs-lookup"><span data-stu-id="8edd0-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="8edd0-135">Se você não fornecer uma ação, o parâmetro de ação padrão para o valor **índice**.</span><span class="sxs-lookup"><span data-stu-id="8edd0-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="8edd0-136">Por fim, se você não fornecer uma id, o parâmetro id padrão é uma cadeia de caracteres vazia.</span><span class="sxs-lookup"><span data-stu-id="8edd0-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="8edd0-137">Vamos examinar alguns exemplos de como a rota padrão mapeia as URLs para ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="8edd0-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="8edd0-138">Imagine que você insira a seguinte URL na barra de endereços do navegador:</span><span class="sxs-lookup"><span data-stu-id="8edd0-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="8edd0-139">/ Home</span><span class="sxs-lookup"><span data-stu-id="8edd0-139">/Home</span></span>

<span data-ttu-id="8edd0-140">Por causa dos padrões de parâmetro de rota padrão, inserir esta URL fará com que o método Index () da classe HomeController na listagem 2 a ser chamado.</span><span class="sxs-lookup"><span data-stu-id="8edd0-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="8edd0-141">**Listagem 2 - HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="8edd0-141">**Listing 2 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

<span data-ttu-id="8edd0-142">Na listagem 2, a classe HomeController inclui um método chamado Index () que aceita um parâmetro único chamado ID. A URL /Home faz com que o método Index () seja chamado com uma cadeia de caracteres vazia como o valor do parâmetro Id.</span><span class="sxs-lookup"><span data-stu-id="8edd0-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with an empty string as the value of the Id parameter.</span></span>

<span data-ttu-id="8edd0-143">Por causa da maneira que a estrutura do MVC invoca as ações do controlador, /Home a URL também corresponde o método Index () da classe HomeController na listagem 3.</span><span class="sxs-lookup"><span data-stu-id="8edd0-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="8edd0-144">**Listagem 3 - HomeController.cs (ação de índice sem nenhum parâmetro)**</span><span class="sxs-lookup"><span data-stu-id="8edd0-144">**Listing 3 - HomeController.cs (Index action with no parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

<span data-ttu-id="8edd0-145">O método Index () na listagem 3 não aceita parâmetros.</span><span class="sxs-lookup"><span data-stu-id="8edd0-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="8edd0-146">O /Home URL fará com que esse método Index () seja chamado.</span><span class="sxs-lookup"><span data-stu-id="8edd0-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="8edd0-147">3/índice/URL /Home também invoca esse método (a Id é ignorada).</span><span class="sxs-lookup"><span data-stu-id="8edd0-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="8edd0-148">O /Home URL também corresponde o método Index () da classe HomeController na listagem 4.</span><span class="sxs-lookup"><span data-stu-id="8edd0-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="8edd0-149">**Listagem 4 - HomeController.cs (ação de índice com o parâmetro anulável)**</span><span class="sxs-lookup"><span data-stu-id="8edd0-149">**Listing 4 - HomeController.cs (Index action with nullable parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

<span data-ttu-id="8edd0-150">Na listagem 4, o método Index () tem um parâmetro de número inteiro.</span><span class="sxs-lookup"><span data-stu-id="8edd0-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="8edd0-151">Como o parâmetro é um parâmetro anulável (pode ter o valor Null), o Index () pode ser chamado sem gerar um erro.</span><span class="sxs-lookup"><span data-stu-id="8edd0-151">Because the parameter is a nullable parameter (can have the value Null), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="8edd0-152">Por fim, invocando o método Index () na listagem 5 com a URL /Home faz com que uma exceção desde o parâmetro de Id *não é* um parâmetro que permite valor nulo.</span><span class="sxs-lookup"><span data-stu-id="8edd0-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="8edd0-153">Se você tentar invocar o método Index (), em seguida, você obterá o erro exibido na Figura 1.</span><span class="sxs-lookup"><span data-stu-id="8edd0-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="8edd0-154">**Listagem 5 - HomeController.cs (ação de índice com o parâmetro de Id)**</span><span class="sxs-lookup"><span data-stu-id="8edd0-154">**Listing 5 - HomeController.cs (Index action with Id parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


<span data-ttu-id="8edd0-155">[![Invocar uma ação do controlador que espera um valor de parâmetro](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8edd0-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="8edd0-156">**Figura 01**: invocar uma ação do controlador que espera um valor de parâmetro ([clique para exibir a imagem em tamanho normal](asp-net-mvc-routing-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="8edd0-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="8edd0-157">O URL /Home/índice/3, por outro lado, funciona muito bem com a ação de controlador de índice na listagem 5.</span><span class="sxs-lookup"><span data-stu-id="8edd0-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="8edd0-158">A solicitação /Home/Index/3 faz com que o método Index () seja chamado com um parâmetro de Id que tem o valor 3.</span><span class="sxs-lookup"><span data-stu-id="8edd0-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="8edd0-159">Resumo</span><span class="sxs-lookup"><span data-stu-id="8edd0-159">Summary</span></span>

<span data-ttu-id="8edd0-160">O objetivo deste tutorial era fornecer uma breve introdução ao roteamento ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8edd0-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="8edd0-161">Examinamos a tabela de rotas padrão que você obtém com um novo aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8edd0-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="8edd0-162">Você aprendeu como a rota padrão mapeia as URLs para ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="8edd0-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8edd0-163">Avançar</span><span class="sxs-lookup"><span data-stu-id="8edd0-163">Next</span></span>](understanding-action-filters-cs.md)
