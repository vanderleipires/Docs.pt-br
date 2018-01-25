---
title: "Injeção de dependência em exibições"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: a1258dbe2e659f6c5149d15b37451810ec7d6601
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="dependency-injection-into-views"></a><span data-ttu-id="76be4-102">Injeção de dependência em exibições</span><span class="sxs-lookup"><span data-stu-id="76be4-102">Dependency injection into views</span></span>

<span data-ttu-id="76be4-103">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="76be4-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="76be4-104">Dá suporte ao ASP.NET Core [injeção de dependência](xref:fundamentals/dependency-injection) em modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="76be4-104">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="76be4-105">Isso pode ser útil para serviços de exibição específicos, como localização ou os dados necessários apenas para o preenchimento de elementos de exibição.</span><span class="sxs-lookup"><span data-stu-id="76be4-105">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="76be4-106">Você deve tentar manter [separação de preocupações](http://deviq.com/separation-of-concerns/) entre seus controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="76be4-106">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="76be4-107">A maioria dos dados que exibem seus modos de exibição deve ser passada do controlador.</span><span class="sxs-lookup"><span data-stu-id="76be4-107">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="76be4-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="76be4-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="76be4-109">Um exemplo simples</span><span class="sxs-lookup"><span data-stu-id="76be4-109">A Simple Example</span></span>

<span data-ttu-id="76be4-110">Você pode injetar um serviço em uma exibição usando o `@inject` diretiva.</span><span class="sxs-lookup"><span data-stu-id="76be4-110">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="76be4-111">Você pode pensar `@inject` como adicionar uma propriedade ao modo de exibição e preenchendo a propriedade usando a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="76be4-111">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="76be4-112">A sintaxe para `@inject`:`@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="76be4-112">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="76be4-113">Um exemplo de `@inject` em ação:</span><span class="sxs-lookup"><span data-stu-id="76be4-113">An example of `@inject` in action:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="76be4-114">Este modo de exibição exibe uma lista de `ToDoItem` instâncias, junto com um resumo mostrando estatísticas gerais.</span><span class="sxs-lookup"><span data-stu-id="76be4-114">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="76be4-115">O resumo é preenchido do injetado `StatisticsService`.</span><span class="sxs-lookup"><span data-stu-id="76be4-115">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="76be4-116">Esse serviço é registrado para injeção de dependência em `ConfigureServices` na *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="76be4-116">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="76be4-117">O `StatisticsService` realiza alguns cálculos no conjunto de `ToDoItem` instâncias que ele acessa por meio de um repositório:</span><span class="sxs-lookup"><span data-stu-id="76be4-117">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

<span data-ttu-id="76be4-118">O repositório de exemplo usa uma coleção de memória.</span><span class="sxs-lookup"><span data-stu-id="76be4-118">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="76be4-119">A implementação mostrada acima (que opera em todos os dados na memória) não é recomendado para conjuntos de dados grandes e acessados remotamente.</span><span class="sxs-lookup"><span data-stu-id="76be4-119">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="76be4-120">O exemplo exibe dados de modelo associado à exibição e o serviço injetados no modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="76be4-120">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Para exibir a listagem de itens total, concluída itens, prioridade média e uma lista de tarefas com níveis de prioridade e valores boolean que indica a conclusão.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="76be4-122">Preenchimento de dados de pesquisa</span><span class="sxs-lookup"><span data-stu-id="76be4-122">Populating Lookup Data</span></span>

<span data-ttu-id="76be4-123">Injeção de exibição pode ser útil para preencher as opções em elementos de interface do usuário, como listas suspensas.</span><span class="sxs-lookup"><span data-stu-id="76be4-123">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="76be4-124">Considere a possibilidade de um formulário de perfil de usuário que inclui opções para especificar o sexo, estado e outras preferências.</span><span class="sxs-lookup"><span data-stu-id="76be4-124">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="76be4-125">Renderização desse formulário usando uma abordagem MVC padrão exigiria o controlador para solicitar serviços de acesso de dados para cada um desses conjuntos de opções e, em seguida, preencher um modelo ou `ViewBag` com cada conjunto de opções para ser associado.</span><span class="sxs-lookup"><span data-stu-id="76be4-125">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="76be4-126">Uma abordagem alternativa injeta services diretamente no modo de exibição para obter as opções.</span><span class="sxs-lookup"><span data-stu-id="76be4-126">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="76be4-127">Isso minimiza a quantidade de código necessário para o controlador, movendo essa lógica de construção do elemento de exibição para o modo de exibição em si.</span><span class="sxs-lookup"><span data-stu-id="76be4-127">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="76be4-128">A ação de controlador para exibir um formulário de edição de perfil só precisa passar o formulário a instância de perfil:</span><span class="sxs-lookup"><span data-stu-id="76be4-128">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="76be4-129">O formulário HTML usado para atualizar essas preferências inclui listas suspensas para três propriedades:</span><span class="sxs-lookup"><span data-stu-id="76be4-129">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Atualize o modo de perfil com um formulário que permite que a entrada de nome, sexo, estado e cor favorita.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="76be4-131">Essas listas são preenchidas por um serviço que tenha sido injetado no modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="76be4-131">These lists are populated by a service that has been injected into the view:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="76be4-132">O `ProfileOptionsService` é um serviço de nível de interface do usuário criado para fornecer apenas os dados necessários para este formulário:</span><span class="sxs-lookup"><span data-stu-id="76be4-132">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> <span data-ttu-id="76be4-133">Não se esqueça de registrar tipos que você vai solicitar por meio de injeção de dependência no `ConfigureServices` método *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="76be4-133">Don't forget to register types you will request through dependency injection in the  `ConfigureServices` method in *Startup.cs*.</span></span>

## <a name="overriding-services"></a><span data-ttu-id="76be4-134">Serviços de substituição</span><span class="sxs-lookup"><span data-stu-id="76be4-134">Overriding Services</span></span>

<span data-ttu-id="76be4-135">Além de injeção de novos serviços, essa técnica pode ser usada para substituir os serviços anteriormente injetados em uma página.</span><span class="sxs-lookup"><span data-stu-id="76be4-135">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="76be4-136">A figura a seguir mostra todos os campos disponíveis na página usada no primeiro exemplo:</span><span class="sxs-lookup"><span data-stu-id="76be4-136">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Menu contextual do IntelliSense em uma lista de campos de Html, componente, StatsService e Url do símbolo @](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="76be4-138">Como você pode ver, os campos padrão incluem `Html`, `Component`, e `Url` (bem como o `StatsService` que é injetado).</span><span class="sxs-lookup"><span data-stu-id="76be4-138">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="76be4-139">Se para a instância que você deseja substituir os auxiliares de HTML padrão com seu próprio, você pode facilmente fazer isso usando `@inject`:</span><span class="sxs-lookup"><span data-stu-id="76be4-139">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="76be4-140">Se você quiser estender os serviços existentes, você pode simplesmente usar essa técnica ao herdar de ou encapsulamento a implementação existente com seus próprios.</span><span class="sxs-lookup"><span data-stu-id="76be4-140">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="76be4-141">Consulte também</span><span class="sxs-lookup"><span data-stu-id="76be4-141">See Also</span></span>

* <span data-ttu-id="76be4-142">Blog do Simon Timms: [Obtendo dados de pesquisa em modo de exibição](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="76be4-142">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
