---
title: Criar APIs Web com o ASP.NET Core
author: scottaddie
description: Saiba mais sobre os recursos disponíveis para a criação de uma API Web no ASP.NET Core e quando é apropriado usar cada recurso.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/04/2018
ms.locfileid: "36274960"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="dfc78-103">Criar APIs Web com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dfc78-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="dfc78-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="dfc78-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="dfc78-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dfc78-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="dfc78-106">Este documento explica como criar uma API Web no ASP.NET Core e quando é mais adequado usar cada recurso.</span><span class="sxs-lookup"><span data-stu-id="dfc78-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="dfc78-107">Derivar a classe por meio do ControllerBase</span><span class="sxs-lookup"><span data-stu-id="dfc78-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="dfc78-108">Herde da classe [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) em um controlador destinado a servir como uma API Web.</span><span class="sxs-lookup"><span data-stu-id="dfc78-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="dfc78-109">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dfc78-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

<span data-ttu-id="dfc78-110">A classe `ControllerBase` fornece acesso a várias propriedades e métodos.</span><span class="sxs-lookup"><span data-stu-id="dfc78-110">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="dfc78-111">No exemplo anterior, alguns desses métodos incluem [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) e [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="dfc78-111">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="dfc78-112">Esses métodos são invocados em métodos de ação para retornar os códigos de status HTTP 400 e 201, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="dfc78-112">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="dfc78-113">A propriedade [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), também fornecida pelo `ControllerBase`, é acessada para executar a validação do modelo de solicitação.</span><span class="sxs-lookup"><span data-stu-id="dfc78-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="dfc78-114">Anotar classe com o ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="dfc78-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="dfc78-115">O ASP.NET Core 2.1 apresenta o atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) para denotar uma classe do controlador API Web.</span><span class="sxs-lookup"><span data-stu-id="dfc78-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="dfc78-116">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dfc78-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="dfc78-117">Esse atributo é geralmente associado ao `ControllerBase` para obter acesso a propriedades e métodos úteis.</span><span class="sxs-lookup"><span data-stu-id="dfc78-117">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="dfc78-118">`ControllerBase` fornece acesso aos métodos como [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) e [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="dfc78-118">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="dfc78-119">Outra abordagem é criar uma classe de base de controlador personalizada anotada com o atributo `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="dfc78-119">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="dfc78-120">As seções a seguir descrevem os recursos de conveniência adicionados pelo atributo.</span><span class="sxs-lookup"><span data-stu-id="dfc78-120">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="dfc78-121">Respostas automáticas do HTTP 400</span><span class="sxs-lookup"><span data-stu-id="dfc78-121">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="dfc78-122">Os erros de validação disparam uma resposta HTTP 400 automaticamente.</span><span class="sxs-lookup"><span data-stu-id="dfc78-122">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="dfc78-123">O código a seguir se torna desnecessário em suas ações:</span><span class="sxs-lookup"><span data-stu-id="dfc78-123">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="dfc78-124">Esse comportamento padrão é desabilitado com o código a seguir no *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="dfc78-124">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="dfc78-125">Inferência de parâmetro de origem de associação</span><span class="sxs-lookup"><span data-stu-id="dfc78-125">Binding source parameter inference</span></span>

<span data-ttu-id="dfc78-126">Um atributo de origem de associação define o local no qual o valor do parâmetro de uma ação é encontrado.</span><span class="sxs-lookup"><span data-stu-id="dfc78-126">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="dfc78-127">Os seguintes atributos da origem da associação existem:</span><span class="sxs-lookup"><span data-stu-id="dfc78-127">The following binding source attributes exist:</span></span>

|<span data-ttu-id="dfc78-128">Atributo</span><span class="sxs-lookup"><span data-stu-id="dfc78-128">Attribute</span></span>|<span data-ttu-id="dfc78-129">Fonte de associação</span><span class="sxs-lookup"><span data-stu-id="dfc78-129">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="dfc78-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="dfc78-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="dfc78-131">Corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="dfc78-131">Request body</span></span> |
|<span data-ttu-id="dfc78-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="dfc78-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="dfc78-133">Dados do formulário no corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="dfc78-133">Form data in the request body</span></span> |
|<span data-ttu-id="dfc78-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="dfc78-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="dfc78-135">Cabeçalho da solicitação</span><span class="sxs-lookup"><span data-stu-id="dfc78-135">Request header</span></span> |
|<span data-ttu-id="dfc78-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="dfc78-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="dfc78-137">Parâmetro de cadeia de caracteres de consulta de solicitação</span><span class="sxs-lookup"><span data-stu-id="dfc78-137">Request query string parameter</span></span> |
|<span data-ttu-id="dfc78-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="dfc78-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="dfc78-139">Dados de rota da solicitação atual</span><span class="sxs-lookup"><span data-stu-id="dfc78-139">Route data from the current request</span></span> |
|<span data-ttu-id="dfc78-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="dfc78-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="dfc78-141">O serviço de solicitação inserido como um parâmetro de ação</span><span class="sxs-lookup"><span data-stu-id="dfc78-141">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="dfc78-142">**Não** use `[FromRoute]`, se os valores puderem conter `%2f` (que é `/`), porque `%2f` não ficará sem escape para `/`.</span><span class="sxs-lookup"><span data-stu-id="dfc78-142">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="dfc78-143">Use `[FromQuery]`, se o valor puder conter `%2f`.</span><span class="sxs-lookup"><span data-stu-id="dfc78-143">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="dfc78-144">Sem o atributo `[ApiController]`, os atributos da origem da associação são definidos explicitamente.</span><span class="sxs-lookup"><span data-stu-id="dfc78-144">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="dfc78-145">No exemplo a seguir, o atributo `[FromQuery]` indica que o valor do parâmetro `discontinuedOnly` é fornecido na cadeia de caracteres de consulta da URL de solicitação:</span><span class="sxs-lookup"><span data-stu-id="dfc78-145">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="dfc78-146">Regras de inferência são aplicadas para as fontes de dados padrão dos parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="dfc78-146">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="dfc78-147">Essas regras configuram as origens da associação que você provavelmente aplicaria manualmente aos parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="dfc78-147">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="dfc78-148">Os atributos da origem da associação se comportam da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dfc78-148">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="dfc78-149">**[FromBody]**  é inferido para parâmetros de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="dfc78-149">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="dfc78-150">Uma exceção a essa regra é qualquer tipo complexo, interno com um significado especial, como [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) e [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="dfc78-150">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="dfc78-151">O código de inferência da origem da associação ignora esses tipos especiais.</span><span class="sxs-lookup"><span data-stu-id="dfc78-151">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="dfc78-152">Quando uma ação tiver mais de um parâmetro especificado explicitamente (via `[FromBody]`) ou inferido como limite do corpo da solicitação, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="dfc78-152">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="dfc78-153">Por exemplo, as seguintes assinaturas de ação causam uma exceção:</span><span class="sxs-lookup"><span data-stu-id="dfc78-153">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="dfc78-154">**[FromForm]**  é inferido para os parâmetros de tipo de ação [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) e [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="dfc78-154">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="dfc78-155">Ele não é inferido para qualquer tipo simples ou definido pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="dfc78-155">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="dfc78-156">**[FromRoute]**  é inferido para qualquer nome de parâmetro de ação correspondente a um parâmetro no modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="dfc78-156">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="dfc78-157">Quando várias rotas correspondem a um parâmetro de ação, qualquer valor de rota é considerado `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="dfc78-157">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="dfc78-158">**[FromQuery]**  é inferido para todos os outros parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="dfc78-158">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="dfc78-159">As regras de inferência padrão são desabilitadas com o código a seguir em *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="dfc78-159">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="dfc78-160">Inferência de solicitação de várias partes/dados de formulário</span><span class="sxs-lookup"><span data-stu-id="dfc78-160">Multipart/form-data request inference</span></span>

<span data-ttu-id="dfc78-161">Quando um parâmetro de ação é anotado com o atributo [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), o tipo de conteúdo de solicitação `multipart/form-data` é inferido.</span><span class="sxs-lookup"><span data-stu-id="dfc78-161">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="dfc78-162">O comportamento padrão é desabilitado com o código a seguir em *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="dfc78-162">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="dfc78-163">Requisito de roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="dfc78-163">Attribute routing requirement</span></span>

<span data-ttu-id="dfc78-164">O roteamento de atributo se torna um requisito.</span><span class="sxs-lookup"><span data-stu-id="dfc78-164">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="dfc78-165">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dfc78-165">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="dfc78-166">As ações são inacessíveis por meio de [rotas convencionais](xref:mvc/controllers/routing#conventional-routing) definidas em [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) ou [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) em *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="dfc78-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="dfc78-167">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="dfc78-167">Additional resources</span></span>

* [<span data-ttu-id="dfc78-168">Tipos de retorno de ação do controlador</span><span class="sxs-lookup"><span data-stu-id="dfc78-168">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="dfc78-169">Formatadores personalizados</span><span class="sxs-lookup"><span data-stu-id="dfc78-169">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="dfc78-170">Formatar dados de resposta</span><span class="sxs-lookup"><span data-stu-id="dfc78-170">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="dfc78-171">Páginas de Ajuda usando o Swagger</span><span class="sxs-lookup"><span data-stu-id="dfc78-171">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="dfc78-172">Roteamento para ações do controlador</span><span class="sxs-lookup"><span data-stu-id="dfc78-172">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
