---
title: Criar APIs Web com o ASP.NET Core
author: scottaddie
description: Saiba mais sobre os recursos disponíveis para a criação de uma API Web no ASP.NET Core e quando é apropriado usar cada recurso.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: 950f4e8afa13bf297ea8658ef1c1bea0c9b62936
ms.sourcegitcommit: 2ef32676c16f76282f7c23154d13affce8c8bf35
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234586"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="e851d-103">Criar APIs Web com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e851d-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="e851d-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="e851d-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="e851d-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([como baixar](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e851d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e851d-106">Este documento explica como criar uma API Web no ASP.NET Core e quando é mais adequado usar cada recurso.</span><span class="sxs-lookup"><span data-stu-id="e851d-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="e851d-107">Derivar a classe por meio do ControllerBase</span><span class="sxs-lookup"><span data-stu-id="e851d-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="e851d-108">Herde da classe <xref:Microsoft.AspNetCore.Mvc.ControllerBase> em um controlador destinado a funcionar como uma API Web.</span><span class="sxs-lookup"><span data-stu-id="e851d-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="e851d-109">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e851d-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="e851d-110">A classe `ControllerBase` fornece acesso a várias propriedades e métodos.</span><span class="sxs-lookup"><span data-stu-id="e851d-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="e851d-111">No código anterior, os exemplos incluem <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> e <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="e851d-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="e851d-112">Esses métodos são chamados em métodos de ação para retornar os códigos de status HTTP 400 e 201, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="e851d-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="e851d-113">A propriedade <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, também fornecida pelo `ControllerBase`, é acessada para executar a validação do modelo de solicitação.</span><span class="sxs-lookup"><span data-stu-id="e851d-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="e851d-114">Anotar classe com o ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="e851d-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="e851d-115">O ASP.NET Core 2.1 apresenta o atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) para denotar uma classe do controlador API Web.</span><span class="sxs-lookup"><span data-stu-id="e851d-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="e851d-116">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e851d-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="e851d-117">Uma versão de compatibilidade do 2.1 ou posteriores, definida por meio de <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, é necessária para usar esse atributo.</span><span class="sxs-lookup"><span data-stu-id="e851d-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="e851d-118">Por exemplo, o código realçado em *Startup.ConfigureServices* define o sinalizador de compatibilidade 2.2:</span><span class="sxs-lookup"><span data-stu-id="e851d-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.2 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="e851d-119">Para obter mais informações, consulte <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="e851d-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="e851d-120">Geralmente, o atributo `[ApiController]` é associado ao `ControllerBase` para habilitar o comportamento específico de REST para controladores.</span><span class="sxs-lookup"><span data-stu-id="e851d-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="e851d-121">`ControllerBase` fornece acesso a métodos como <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> e <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="e851d-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="e851d-122">Outra abordagem é criar uma classe de base de controlador personalizada anotada com o atributo `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="e851d-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="e851d-123">As seções a seguir descrevem os recursos de conveniência adicionados pelo atributo.</span><span class="sxs-lookup"><span data-stu-id="e851d-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="e851d-124">Respostas de detalhes de problema para códigos de status de erro</span><span class="sxs-lookup"><span data-stu-id="e851d-124">Problem details responses for error status codes</span></span>

<span data-ttu-id="e851d-125">O ASP.NET Core 2.1 e posterior inclui [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), um tipo baseado na [especificação RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="e851d-125">ASP.NET Core 2.1 and later includes [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), a type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="e851d-126">O tipo `ProblemDetails` fornece um formato padronizado para transmitir detalhes legíveis por computador de erros em uma resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="e851d-126">The `ProblemDetails` type provides a standardized format for conveying machine readable details of errors in a HTTP response.</span></span>

<span data-ttu-id="e851d-127">No ASP.NET Core 2.2 e posterior, o MVC transforma os resultados do código de status de erro (código de status 400 e posterior) em um resultado com `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="e851d-127">In ASP.NET Core 2.2 and later, MVC transforms error status code results (status code 400 and higher) to a result with `ProblemDetails`.</span></span> <span data-ttu-id="e851d-128">Considere o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="e851d-128">Consider the following code:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

<span data-ttu-id="e851d-129">A resposta HTTP para o resultado `NotFound` tem um código de status 404 com um corpo `ProblemDetails` semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="e851d-129">The HTTP response for the `NotFound` result has a 404 status code with a `ProblemDetails` body similar to the following:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="e851d-130">O recurso de detalhes do problema requer um sinalizador de compatibilidade de 2.2 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="e851d-130">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="e851d-131">O comportamento padrão é desabilitado quando a propriedade [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> está definida como `true`.</span><span class="sxs-lookup"><span data-stu-id="e851d-131">The default behavior is disabled when the [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> property is set to `true`.</span></span> <span data-ttu-id="e851d-132">O seguinte código realçado de `Startup.ConfigureServices` desabilita os detalhes do problema:</span><span class="sxs-lookup"><span data-stu-id="e851d-132">The following highlighted code from `Startup.ConfigureServices` disables problem details:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

<span data-ttu-id="e851d-133">Use a propriedade [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> para configurar o conteúdo da resposta `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="e851d-133">Use the [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="e851d-134">Por exemplo, o código a seguir atualiza a propriedade `type` para respostas 404:</span><span class="sxs-lookup"><span data-stu-id="e851d-134">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a><span data-ttu-id="e851d-135">Respostas automáticas do HTTP 400</span><span class="sxs-lookup"><span data-stu-id="e851d-135">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="e851d-136">Os erros de validação disparam uma resposta HTTP 400 automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e851d-136">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="e851d-137">O código a seguir se torna desnecessário em suas ações:</span><span class="sxs-lookup"><span data-stu-id="e851d-137">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="e851d-138">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> para personalizar a saída da resposta resultante.</span><span class="sxs-lookup"><span data-stu-id="e851d-138">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the the resulting response.</span></span>

<span data-ttu-id="e851d-139">O comportamento padrão é desabilitado quando a propriedade <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> está definida como `true`.</span><span class="sxs-lookup"><span data-stu-id="e851d-139">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="e851d-140">Adicione o seguinte código em *Startup.ConfigureServices* depois de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="e851d-140">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

<span data-ttu-id="e851d-141">Com um sinalizador de compatibilidade de 2.2 ou posterior, o tipo de resposta padrão retornado para respostas 400 é um <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="e851d-141">With a compatibility flag of 2.2 or later, the default response type returned for 400 responses is a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="e851d-142">Use a propriedade [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> para usar o formato de erro do ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="e851d-142">Use the [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> property to use the ASP.NET Core 2.1 error format.</span></span>

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="e851d-143">Inferência de parâmetro de origem de associação</span><span class="sxs-lookup"><span data-stu-id="e851d-143">Binding source parameter inference</span></span>

<span data-ttu-id="e851d-144">Um atributo de origem de associação define o local no qual o valor do parâmetro de uma ação é encontrado.</span><span class="sxs-lookup"><span data-stu-id="e851d-144">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="e851d-145">Os seguintes atributos da origem da associação existem:</span><span class="sxs-lookup"><span data-stu-id="e851d-145">The following binding source attributes exist:</span></span>

|<span data-ttu-id="e851d-146">Atributo</span><span class="sxs-lookup"><span data-stu-id="e851d-146">Attribute</span></span>|<span data-ttu-id="e851d-147">Fonte de associação</span><span class="sxs-lookup"><span data-stu-id="e851d-147">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="e851d-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="e851d-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="e851d-149">Corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="e851d-149">Request body</span></span> |
|<span data-ttu-id="e851d-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="e851d-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="e851d-151">Dados do formulário no corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="e851d-151">Form data in the request body</span></span> |
|<span data-ttu-id="e851d-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="e851d-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="e851d-153">Cabeçalho da solicitação</span><span class="sxs-lookup"><span data-stu-id="e851d-153">Request header</span></span> |
|<span data-ttu-id="e851d-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="e851d-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="e851d-155">Parâmetro de cadeia de caracteres de consulta de solicitação</span><span class="sxs-lookup"><span data-stu-id="e851d-155">Request query string parameter</span></span> |
|<span data-ttu-id="e851d-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="e851d-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="e851d-157">Dados de rota da solicitação atual</span><span class="sxs-lookup"><span data-stu-id="e851d-157">Route data from the current request</span></span> |
|<span data-ttu-id="e851d-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="e851d-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="e851d-159">O serviço de solicitação inserido como um parâmetro de ação</span><span class="sxs-lookup"><span data-stu-id="e851d-159">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="e851d-160">Não use `[FromRoute]` quando os valores puderem conter `%2f` (ou seja, `/`).</span><span class="sxs-lookup"><span data-stu-id="e851d-160">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="e851d-161">`%2f` não ficará sem escape para `/`.</span><span class="sxs-lookup"><span data-stu-id="e851d-161">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="e851d-162">Use `[FromQuery]`, se o valor puder conter `%2f`.</span><span class="sxs-lookup"><span data-stu-id="e851d-162">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="e851d-163">Sem o atributo `[ApiController]`, os atributos da origem da associação são definidos explicitamente.</span><span class="sxs-lookup"><span data-stu-id="e851d-163">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="e851d-164">No exemplo a seguir, o atributo `[FromQuery]` indica que o valor do parâmetro `discontinuedOnly` é fornecido na cadeia de caracteres de consulta da URL de solicitação:</span><span class="sxs-lookup"><span data-stu-id="e851d-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="e851d-165">Regras de inferência são aplicadas para as fontes de dados padrão dos parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="e851d-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="e851d-166">Essas regras configuram as origens da associação que você provavelmente aplicaria manualmente aos parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="e851d-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="e851d-167">Os atributos da origem da associação se comportam da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="e851d-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="e851d-168">**[FromBody]**  é inferido para parâmetros de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="e851d-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="e851d-169">Uma exceção a essa regra é qualquer tipo interno complexo com um significado especial, como <xref:Microsoft.AspNetCore.Http.IFormCollection> e <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="e851d-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="e851d-170">O código de inferência da origem da associação ignora esses tipos especiais.</span><span class="sxs-lookup"><span data-stu-id="e851d-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="e851d-171">`[FromBody]` não é inferido para tipos simples, como `string` ou `int`.</span><span class="sxs-lookup"><span data-stu-id="e851d-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="e851d-172">Portanto, o atributo `[FromBody]` deve ser usado para tipos simples quando essa funcionalidade é desejada.</span><span class="sxs-lookup"><span data-stu-id="e851d-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="e851d-173">Quando uma ação tiver mais de um parâmetro especificado explicitamente (via `[FromBody]`) ou inferido como limite do corpo da solicitação, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="e851d-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="e851d-174">Por exemplo, as seguintes assinaturas de ação causam uma exceção:</span><span class="sxs-lookup"><span data-stu-id="e851d-174">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="e851d-175">**[FromForm]** é inferido para parâmetros de ação do tipo <xref:Microsoft.AspNetCore.Http.IFormFile> e <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="e851d-175">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="e851d-176">Ele não é inferido para qualquer tipo simples ou definido pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="e851d-176">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="e851d-177">**[FromRoute]**  é inferido para qualquer nome de parâmetro de ação correspondente a um parâmetro no modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="e851d-177">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="e851d-178">Quando mais de uma rota correspondem a um parâmetro de ação, qualquer valor de rota é considerado `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="e851d-178">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="e851d-179">**[FromQuery]**  é inferido para todos os outros parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="e851d-179">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="e851d-180">As regras de inferência padrão são desabilitadas quando a propriedade <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> está definida como `true`.</span><span class="sxs-lookup"><span data-stu-id="e851d-180">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="e851d-181">Adicione o seguinte código em *Startup.ConfigureServices* depois de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="e851d-181">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="e851d-182">Inferência de solicitação de várias partes/dados de formulário</span><span class="sxs-lookup"><span data-stu-id="e851d-182">Multipart/form-data request inference</span></span>

<span data-ttu-id="e851d-183">Quando um parâmetro de ação é anotado com o atributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), o tipo de conteúdo de solicitação `multipart/form-data` é inferido.</span><span class="sxs-lookup"><span data-stu-id="e851d-183">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="e851d-184">O comportamento padrão é desabilitado quando a propriedade <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> está definida como `true`.</span><span class="sxs-lookup"><span data-stu-id="e851d-184">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="e851d-185">Adicione o seguinte código em *Startup.ConfigureServices* depois de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="e851d-185">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="e851d-186">Requisito de roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="e851d-186">Attribute routing requirement</span></span>

<span data-ttu-id="e851d-187">O roteamento de atributo se torna um requisito.</span><span class="sxs-lookup"><span data-stu-id="e851d-187">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="e851d-188">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e851d-188">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="e851d-189">As ações são inacessíveis por meio de [rotas convencionais](xref:mvc/controllers/routing#conventional-routing) definidas em <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> ou por <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> em *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="e851d-189">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="e851d-190">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="e851d-190">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
