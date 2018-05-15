---
title: Auxiliar de marca parcial no ASP.NET Core
author: scottaddie
description: Descubra o auxiliar de marca parcial do ASP.NET Core e a função de cada um de seus atributos na renderização de uma exibição parcial.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 670663b963f4207da793afff44d55b85ba58b7f8
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2018
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="4cde1-103">Auxiliar de marca parcial no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cde1-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="4cde1-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="4cde1-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="4cde1-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4cde1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="4cde1-106">Visão geral</span><span class="sxs-lookup"><span data-stu-id="4cde1-106">Overview</span></span>

<span data-ttu-id="4cde1-107">O auxiliar de marca parcial é usado para renderizar uma [exibição parcial](xref:mvc/views/partial) em Páginas Razor e em aplicativos MVC.</span><span class="sxs-lookup"><span data-stu-id="4cde1-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="4cde1-108">Considere que ele:</span><span class="sxs-lookup"><span data-stu-id="4cde1-108">Consider that it:</span></span>

* <span data-ttu-id="4cde1-109">Exige o ASP.NET Core 2.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="4cde1-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="4cde1-110">É uma alternativa à [sintaxe do HTML Helper](xref:mvc/views/partial#referencing-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="4cde1-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#referencing-a-partial-view).</span></span>
* <span data-ttu-id="4cde1-111">Renderiza a exibição parcial de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="4cde1-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="4cde1-112">As opções do HTML Helper para renderizar uma exibição parcial incluem:</span><span class="sxs-lookup"><span data-stu-id="4cde1-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="4cde1-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="4cde1-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="4cde1-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="4cde1-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="4cde1-115">O modelo *Product* é usado nos exemplos ao longo deste documento:</span><span class="sxs-lookup"><span data-stu-id="4cde1-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="4cde1-116">Segue um inventário dos atributos do auxiliar de marca parcial.</span><span class="sxs-lookup"><span data-stu-id="4cde1-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="4cde1-117">name</span><span class="sxs-lookup"><span data-stu-id="4cde1-117">name</span></span>

<span data-ttu-id="4cde1-118">O atributo `name` é necessário.</span><span class="sxs-lookup"><span data-stu-id="4cde1-118">The `name` attribute is required.</span></span> <span data-ttu-id="4cde1-119">Indica o nome ou o caminho da exibição parcial a ser renderizada.</span><span class="sxs-lookup"><span data-stu-id="4cde1-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="4cde1-120">Quando é fornecido um nome de exibição parcial, o processo [descoberta de exibição](xref:mvc/views/overview#view-discovery) é iniciado.</span><span class="sxs-lookup"><span data-stu-id="4cde1-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="4cde1-121">Esse processo é ignorado quando um caminho explícito é fornecido.</span><span class="sxs-lookup"><span data-stu-id="4cde1-121">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="4cde1-122">A marcação a seguir usa um caminho explícito, indicando que *_ProductPartial.cshtml* deve ser carregado da pasta *Compartilhado*.</span><span class="sxs-lookup"><span data-stu-id="4cde1-122">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="4cde1-123">Usando o atributo [for](#for), um modelo é passado para a exibição parcial para associação.</span><span class="sxs-lookup"><span data-stu-id="4cde1-123">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="4cde1-124">for</span><span class="sxs-lookup"><span data-stu-id="4cde1-124">for</span></span>

<span data-ttu-id="4cde1-125">O atributo `for` atribui uma [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) a ser avaliada em relação ao modelo atual.</span><span class="sxs-lookup"><span data-stu-id="4cde1-125">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="4cde1-126">Uma `ModelExpression` infere a sintaxe `@Model.`.</span><span class="sxs-lookup"><span data-stu-id="4cde1-126">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="4cde1-127">Por exemplo, `for="Product"` pode ser usado em vez de `for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="4cde1-127">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="4cde1-128">Esse comportamento de inferência padrão é substituído usando o símbolo `@` para definir uma expressão embutida.</span><span class="sxs-lookup"><span data-stu-id="4cde1-128">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="4cde1-129">O `for` atributo não pode ser usado com o atributo [modelo](#model).</span><span class="sxs-lookup"><span data-stu-id="4cde1-129">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="4cde1-130">A seguinte marcação carrega *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4cde1-130">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="4cde1-131">A exibição parcial é associada à propriedade `Product` do modelo de página associado:</span><span class="sxs-lookup"><span data-stu-id="4cde1-131">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="4cde1-132">modelo</span><span class="sxs-lookup"><span data-stu-id="4cde1-132">model</span></span>

<span data-ttu-id="4cde1-133">O atributo `model` atribui uma instância de modelo a ser passada para a exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="4cde1-133">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="4cde1-134">O atributo `model` não pode ser usado com o atributo [for](#for).</span><span class="sxs-lookup"><span data-stu-id="4cde1-134">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="4cde1-135">Na marcação a seguir, é criada uma nova instância do objeto `Product` que é passada ao atributo `model` para associação:</span><span class="sxs-lookup"><span data-stu-id="4cde1-135">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="4cde1-136">view-data</span><span class="sxs-lookup"><span data-stu-id="4cde1-136">view-data</span></span>

<span data-ttu-id="4cde1-137">O atributo `view-data` atribui um [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) a ser passado para a exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="4cde1-137">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="4cde1-138">A seguinte marcação faz com que toda a coleção ViewData fique acessível para a exibição parcial:</span><span class="sxs-lookup"><span data-stu-id="4cde1-138">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="4cde1-139">No código anterior, o valor da chave `IsNumberReadOnly` é definido como `true` e adicionado à coleção ViewData.</span><span class="sxs-lookup"><span data-stu-id="4cde1-139">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="4cde1-140">Consequentemente, `ViewData["IsNumberReadOnly"]` se torna acessível dentro da exibição parcial a seguir:</span><span class="sxs-lookup"><span data-stu-id="4cde1-140">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="4cde1-141">Neste exemplo, o valor de `ViewData["IsNumberReadOnly"]` determina se o campo *Número* é exibido como somente leitura.</span><span class="sxs-lookup"><span data-stu-id="4cde1-141">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4cde1-142">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="4cde1-142">Additional resources</span></span>

* [<span data-ttu-id="4cde1-143">Exibições parciais</span><span class="sxs-lookup"><span data-stu-id="4cde1-143">Partial views</span></span>](xref:mvc/views/partial)
* [<span data-ttu-id="4cde1-144">Dados fracamente tipados (ViewData e ViewBag)</span><span class="sxs-lookup"><span data-stu-id="4cde1-144">Weakly typed data (ViewData and ViewBag)</span></span>](xref:mvc/views/overview#weakly-typed-data-viewdata-and-viewbag)
