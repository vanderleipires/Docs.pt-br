---
title: Tipos de retorno de ação do controlador na API Web ASP.NET Core
author: scottaddie
description: Aprenda a usar os vários tipos de retorno do método de ação do controlador em uma API Web ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: web-api/action-return-types
ms.openlocfilehash: 84300eae4271c3ee4387be022c3576dc83e144eb
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207518"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="a9b93-103">Tipos de retorno de ação do controlador na API Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9b93-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="a9b93-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="a9b93-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="a9b93-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([como baixar](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9b93-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a9b93-106">O ASP.NET Core oferece as seguintes opções para os tipos de retorno da ação do controlador da API Web:</span><span class="sxs-lookup"><span data-stu-id="a9b93-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="a9b93-107">Tipo específico</span><span class="sxs-lookup"><span data-stu-id="a9b93-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="a9b93-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="a9b93-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="a9b93-109">ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="a9b93-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="a9b93-110">Tipo específico</span><span class="sxs-lookup"><span data-stu-id="a9b93-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="a9b93-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="a9b93-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="a9b93-112">Este documento explica quando é mais adequado usar cada tipo de retorno.</span><span class="sxs-lookup"><span data-stu-id="a9b93-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="a9b93-113">Tipo específico</span><span class="sxs-lookup"><span data-stu-id="a9b93-113">Specific type</span></span>

<span data-ttu-id="a9b93-114">A ação mais simples retorna um tipo de dados complexo ou primitivo (por exemplo, `string` ou um tipo de objeto personalizado).</span><span class="sxs-lookup"><span data-stu-id="a9b93-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="a9b93-115">Considere a seguinte ação, que retorna uma coleção de objetos `Product` personalizados:</span><span class="sxs-lookup"><span data-stu-id="a9b93-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="a9b93-116">Sem condições conhecidas contra as quais se proteger durante a execução da ação, retornar um tipo específico pode ser suficiente.</span><span class="sxs-lookup"><span data-stu-id="a9b93-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="a9b93-117">A ação anterior não aceita parâmetros, assim, validação de restrições de parâmetro não é necessária.</span><span class="sxs-lookup"><span data-stu-id="a9b93-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="a9b93-118">Quando condições conhecidas precisarem ser incluídas em uma ação, vários caminhos de retorno serão introduzidos.</span><span class="sxs-lookup"><span data-stu-id="a9b93-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="a9b93-119">Nesse caso, é comum combinar um tipo de retorno [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) com o tipo de retorno primitivo ou complexo.</span><span class="sxs-lookup"><span data-stu-id="a9b93-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="a9b93-120">[IActionResult](#iactionresult-type) ou [ActionResult\<T >](#actionresultt-type) é necessário para acomodar esse tipo de ação.</span><span class="sxs-lookup"><span data-stu-id="a9b93-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="a9b93-121">Tipo IActionResult</span><span class="sxs-lookup"><span data-stu-id="a9b93-121">IActionResult type</span></span>

<span data-ttu-id="a9b93-122">O tipo de retorno [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) é apropriado quando vários tipos de retorno [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) são possíveis em uma ação.</span><span class="sxs-lookup"><span data-stu-id="a9b93-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="a9b93-123">Os tipos `ActionResult` representam vários códigos de status HTTP.</span><span class="sxs-lookup"><span data-stu-id="a9b93-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="a9b93-124">Alguns tipos de retorno comuns que se enquadram nessa categoria são [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400) [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) e [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span><span class="sxs-lookup"><span data-stu-id="a9b93-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="a9b93-125">Porque há vários tipos de retorno e caminhos na ação, o uso liberal do atributo [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) é necessário.</span><span class="sxs-lookup"><span data-stu-id="a9b93-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="a9b93-126">Esse atributo produz detalhes de resposta mais descritivos para páginas de ajuda da API geradas por ferramentas como [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="a9b93-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="a9b93-127">`[ProducesResponseType]` indica os tipos conhecidos e os códigos de status HTTP a serem retornados pela ação.</span><span class="sxs-lookup"><span data-stu-id="a9b93-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="a9b93-128">Ação síncrona</span><span class="sxs-lookup"><span data-stu-id="a9b93-128">Synchronous action</span></span>

<span data-ttu-id="a9b93-129">Considere a seguinte ação síncrona em que há dois tipos de retorno possíveis:</span><span class="sxs-lookup"><span data-stu-id="a9b93-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="a9b93-130">Na ação anterior, um código de status 404 é retornado quando o produto representado por `id` não existe no armazenamento de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="a9b93-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="a9b93-131">O método auxiliar [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) é invocado como um atalho para `return new NotFoundResult();`.</span><span class="sxs-lookup"><span data-stu-id="a9b93-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="a9b93-132">Se o produto existir, um objeto `Product` que representa o conteúdo será retornado com um código de status 200.</span><span class="sxs-lookup"><span data-stu-id="a9b93-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="a9b93-133">O método auxiliar [OK](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) é invocado como a forma abreviada de `return new OkObjectResult(product);`.</span><span class="sxs-lookup"><span data-stu-id="a9b93-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="a9b93-134">Ação assíncrona</span><span class="sxs-lookup"><span data-stu-id="a9b93-134">Asynchronous action</span></span>

<span data-ttu-id="a9b93-135">Considere a seguinte ação assíncrona em que há dois tipos de retorno possíveis:</span><span class="sxs-lookup"><span data-stu-id="a9b93-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="a9b93-136">Na ação anterior, um código de status 400 é retornado quando há falha na validação do modelo e o método auxiliar [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) é invocado.</span><span class="sxs-lookup"><span data-stu-id="a9b93-136">In the preceding action, a 400 status code is returned when model validation fails and the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) helper method is invoked.</span></span> <span data-ttu-id="a9b93-137">Por exemplo, o modelo a seguir indica que as solicitações devem fornecer a propriedade `Name` e um valor.</span><span class="sxs-lookup"><span data-stu-id="a9b93-137">For example, the following model indicates that requests must provide the `Name` property and a value.</span></span> <span data-ttu-id="a9b93-138">Portanto, a falha em fornecer um `Name` adequado na solicitação causa falha na validação de modelo.</span><span class="sxs-lookup"><span data-stu-id="a9b93-138">Therefore, failure to provide a proper `Name` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

<span data-ttu-id="a9b93-139">O outro código de retorno conhecido da ação anterior é 201, que é gerado pelo método auxiliar [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="a9b93-139">The preceding action's other known return code is a 201, which is generated by the [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) helper method.</span></span> <span data-ttu-id="a9b93-140">Nesse caminho, o objeto `Product` é retornado.</span><span class="sxs-lookup"><span data-stu-id="a9b93-140">In this path, the `Product` object is returned.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="actionresultt-type"></a><span data-ttu-id="a9b93-141">Tipo ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="a9b93-141">ActionResult\<T> type</span></span>

<span data-ttu-id="a9b93-142">O ASP.NET Core 2.1 apresenta o tipo de retorno [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) para ações do controlador de API Web.</span><span class="sxs-lookup"><span data-stu-id="a9b93-142">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="a9b93-143">Permite que você retorne um tipo derivado de [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) ou retorne um [tipo específico](#specific-type).</span><span class="sxs-lookup"><span data-stu-id="a9b93-143">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="a9b93-144">`ActionResult<T>` oferece os seguintes benefícios em relação ao [tipo IActionResult](#iactionresult-type):</span><span class="sxs-lookup"><span data-stu-id="a9b93-144">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="a9b93-145">O atributo [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) pode ter sua propriedade `Type` excluída.</span><span class="sxs-lookup"><span data-stu-id="a9b93-145">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="a9b93-146">Por exemplo, `[ProducesResponseType(200, Type = typeof(Product))]` é simplificado para `[ProducesResponseType(200)]`.</span><span class="sxs-lookup"><span data-stu-id="a9b93-146">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="a9b93-147">O tipo de retorno esperado da ação é inferido do `T` em `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="a9b93-147">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="a9b93-148">[Operadores de conversão implícita](/dotnet/csharp/language-reference/keywords/implicit) são compatíveis com a conversão de `T` e `ActionResult` em `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="a9b93-148">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="a9b93-149">`T` converte em [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), o que significa que `return new ObjectResult(T);` é simplificado para `return T;`.</span><span class="sxs-lookup"><span data-stu-id="a9b93-149">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="a9b93-150">C# não dá suporte a operadores de conversão implícita em interfaces.</span><span class="sxs-lookup"><span data-stu-id="a9b93-150">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="a9b93-151">Consequentemente, a conversão da interface para um tipo concreto é necessário para usar `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="a9b93-151">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="a9b93-152">Por exemplo, o uso de `IEnumerable` no exemplo a seguir não funciona:</span><span class="sxs-lookup"><span data-stu-id="a9b93-152">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

<span data-ttu-id="a9b93-153">Uma opção para corrigir o código anterior é retornar a `_repository.GetProducts().ToList();`.</span><span class="sxs-lookup"><span data-stu-id="a9b93-153">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="a9b93-154">A maioria das ações tem um tipo de retorno específico.</span><span class="sxs-lookup"><span data-stu-id="a9b93-154">Most actions have a specific return type.</span></span> <span data-ttu-id="a9b93-155">Condições inesperadas podem ocorrer durante a execução da ação, caso em que o tipo específico não é retornado.</span><span class="sxs-lookup"><span data-stu-id="a9b93-155">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="a9b93-156">Por exemplo, o parâmetro de entrada de uma ação pode falhar na validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="a9b93-156">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="a9b93-157">Nesse caso, é comum retornar o tipo `ActionResult` adequado, em vez do tipo específico.</span><span class="sxs-lookup"><span data-stu-id="a9b93-157">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="a9b93-158">Ação síncrona</span><span class="sxs-lookup"><span data-stu-id="a9b93-158">Synchronous action</span></span>

<span data-ttu-id="a9b93-159">Considere uma ação síncrona em que há dois tipos de retorno possíveis:</span><span class="sxs-lookup"><span data-stu-id="a9b93-159">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="a9b93-160">No código anterior, um código de status 404 é retornado quando o produto não existe no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a9b93-160">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="a9b93-161">Se o produto existir, o objeto `Product` correspondente será retornado.</span><span class="sxs-lookup"><span data-stu-id="a9b93-161">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="a9b93-162">Antes do ASP.NET Core 2.1, a linha `return product;` teria sido `return Ok(product);`.</span><span class="sxs-lookup"><span data-stu-id="a9b93-162">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="a9b93-163">Do ASP.NET Core 2.1 em diante, a inferência de origem de associação de parâmetro de ação é habilitada quando uma classe de controlador é decorada com o atributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="a9b93-163">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="a9b93-164">Um nome de parâmetro correspondente a um nome do modelo de rota é associado automaticamente usando os dados de rota de solicitação.</span><span class="sxs-lookup"><span data-stu-id="a9b93-164">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="a9b93-165">Consequentemente, o parâmetro `id` da ação anterior não é explicitamente anotado com o atributo [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute).</span><span class="sxs-lookup"><span data-stu-id="a9b93-165">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="a9b93-166">Ação assíncrona</span><span class="sxs-lookup"><span data-stu-id="a9b93-166">Asynchronous action</span></span>

<span data-ttu-id="a9b93-167">Considere uma ação assíncrona em que há dois tipos de retorno possíveis:</span><span class="sxs-lookup"><span data-stu-id="a9b93-167">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="a9b93-168">Se a validação do modelo falhar, o método [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) será chamado para retornar um código de status 400.</span><span class="sxs-lookup"><span data-stu-id="a9b93-168">If model validation fails, the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) method is invoked to return a 400 status code.</span></span> <span data-ttu-id="a9b93-169">A propriedade [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) que contém os erros de validação específicos que são passados para ela.</span><span class="sxs-lookup"><span data-stu-id="a9b93-169">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property containing the specific validation errors is passed to it.</span></span> <span data-ttu-id="a9b93-170">Se a validação do modelo for bem-sucedida, o produto será criado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a9b93-170">If model validation succeeds, the product is created in the database.</span></span> <span data-ttu-id="a9b93-171">Um código de status 201 é retornado.</span><span class="sxs-lookup"><span data-stu-id="a9b93-171">A 201 status code is returned.</span></span>

> [!TIP]
> <span data-ttu-id="a9b93-172">Do ASP.NET Core 2.1 em diante, a inferência de origem de associação de parâmetro de ação é habilitada quando uma classe de controlador é decorada com o atributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="a9b93-172">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="a9b93-173">Parâmetros de tipo complexo são vinculados automaticamente usando o corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="a9b93-173">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="a9b93-174">Consequentemente, o parâmetro `product` da ação anterior não é explicitamente anotado com o atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute).</span><span class="sxs-lookup"><span data-stu-id="a9b93-174">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a9b93-175">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="a9b93-175">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
