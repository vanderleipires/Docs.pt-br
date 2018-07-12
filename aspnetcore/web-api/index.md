---
title: Criar APIs Web com o ASP.NET Core
author: scottaddie
description: Saiba mais sobre os recursos disponíveis para a criação de uma API Web no ASP.NET Core e quando é apropriado usar cada recurso.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: 0ff0bbc629930666d46247d6c1257fac8bfaf7c2
ms.sourcegitcommit: 260abb706ed17f07a53288d8a0c3e69fc13e7468
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38966803"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="6fdcd-103">Criar APIs Web com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6fdcd-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="6fdcd-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="6fdcd-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="6fdcd-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6fdcd-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6fdcd-106">Este documento explica como criar uma API Web no ASP.NET Core e quando é mais adequado usar cada recurso.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="6fdcd-107">Derivar a classe por meio do ControllerBase</span><span class="sxs-lookup"><span data-stu-id="6fdcd-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="6fdcd-108">Herde da classe [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) em um controlador destinado a servir como uma API Web.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="6fdcd-109">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="6fdcd-110">A classe `ControllerBase` fornece acesso a várias propriedades e métodos.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="6fdcd-111">No exemplo anterior, alguns desses métodos incluem [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) e [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="6fdcd-111">In the preceding code, examples include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="6fdcd-112">Esses métodos são chamados em métodos de ação para retornar os códigos de status HTTP 400 e 201, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="6fdcd-113">A propriedade [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), também fornecida pelo `ControllerBase`, é acessada para executar a validação do modelo de solicitação.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="6fdcd-114">Anotar classe com o ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="6fdcd-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="6fdcd-115">O ASP.NET Core 2.1 apresenta o atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) para denotar uma classe do controlador API Web.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="6fdcd-116">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="6fdcd-117">Uma versão de compatibilidade do 2.1 ou posteriores, definida por meio de [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), é necessária para usar esse atributo.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-117">A compatibility version of 2.1 or later, set via [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), is required to use this attribute.</span></span> <span data-ttu-id="6fdcd-118">Por exemplo, o código realçado em *Startup.ConfigureServices* define o sinalizador de compatibilidade 2.1:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="6fdcd-119">Geralmente, o atributo `[ApiController]` é associado ao `ControllerBase` para habilitar o comportamento específico de REST para controladores.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-119">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="6fdcd-120">`ControllerBase` fornece acesso aos métodos como [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) e [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="6fdcd-120">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="6fdcd-121">Outra abordagem é criar uma classe de base de controlador personalizada anotada com o atributo `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-121">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="6fdcd-122">As seções a seguir descrevem os recursos de conveniência adicionados pelo atributo.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-122">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="6fdcd-123">Respostas automáticas do HTTP 400</span><span class="sxs-lookup"><span data-stu-id="6fdcd-123">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="6fdcd-124">Os erros de validação disparam uma resposta HTTP 400 automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-124">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="6fdcd-125">O código a seguir se torna desnecessário em suas ações:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-125">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="6fdcd-126">O comportamento padrão é desabilitado quando a propriedade [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) estiver definida como `true`.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-126">The default behavior is disabled when the [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) property is set to `true`.</span></span> <span data-ttu-id="6fdcd-127">Adicione o seguinte código em *Startup.ConfigureServices* depois de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-127">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="6fdcd-128">Inferência de parâmetro de origem de associação</span><span class="sxs-lookup"><span data-stu-id="6fdcd-128">Binding source parameter inference</span></span>

<span data-ttu-id="6fdcd-129">Um atributo de origem de associação define o local no qual o valor do parâmetro de uma ação é encontrado.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-129">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="6fdcd-130">Os seguintes atributos da origem da associação existem:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-130">The following binding source attributes exist:</span></span>

|<span data-ttu-id="6fdcd-131">Atributo</span><span class="sxs-lookup"><span data-stu-id="6fdcd-131">Attribute</span></span>|<span data-ttu-id="6fdcd-132">Fonte de associação</span><span class="sxs-lookup"><span data-stu-id="6fdcd-132">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="6fdcd-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="6fdcd-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="6fdcd-134">Corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="6fdcd-134">Request body</span></span> |
|<span data-ttu-id="6fdcd-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="6fdcd-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="6fdcd-136">Dados do formulário no corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="6fdcd-136">Form data in the request body</span></span> |
|<span data-ttu-id="6fdcd-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="6fdcd-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="6fdcd-138">Cabeçalho da solicitação</span><span class="sxs-lookup"><span data-stu-id="6fdcd-138">Request header</span></span> |
|<span data-ttu-id="6fdcd-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="6fdcd-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="6fdcd-140">Parâmetro de cadeia de caracteres de consulta de solicitação</span><span class="sxs-lookup"><span data-stu-id="6fdcd-140">Request query string parameter</span></span> |
|<span data-ttu-id="6fdcd-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="6fdcd-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="6fdcd-142">Dados de rota da solicitação atual</span><span class="sxs-lookup"><span data-stu-id="6fdcd-142">Route data from the current request</span></span> |
|<span data-ttu-id="6fdcd-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="6fdcd-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="6fdcd-144">O serviço de solicitação inserido como um parâmetro de ação</span><span class="sxs-lookup"><span data-stu-id="6fdcd-144">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="6fdcd-145">Não use `[FromRoute]` quando os valores puderem conter `%2f` (ou seja, `/`).</span><span class="sxs-lookup"><span data-stu-id="6fdcd-145">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="6fdcd-146">`%2f` não ficará sem escape para `/`.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-146">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="6fdcd-147">Use `[FromQuery]`, se o valor puder conter `%2f`.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-147">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="6fdcd-148">Sem o atributo `[ApiController]`, os atributos da origem da associação são definidos explicitamente.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-148">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="6fdcd-149">No exemplo a seguir, o atributo `[FromQuery]` indica que o valor do parâmetro `discontinuedOnly` é fornecido na cadeia de caracteres de consulta da URL de solicitação:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-149">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="6fdcd-150">Regras de inferência são aplicadas para as fontes de dados padrão dos parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-150">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="6fdcd-151">Essas regras configuram as origens da associação que você provavelmente aplicaria manualmente aos parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-151">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="6fdcd-152">Os atributos da origem da associação se comportam da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-152">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="6fdcd-153">**[FromBody]**  é inferido para parâmetros de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-153">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="6fdcd-154">Uma exceção a essa regra é qualquer tipo complexo, interno com um significado especial, como [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) e [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="6fdcd-154">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="6fdcd-155">O código de inferência da origem da associação ignora esses tipos especiais.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-155">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="6fdcd-156">Quando uma ação tiver mais de um parâmetro especificado explicitamente (via `[FromBody]`) ou inferido como limite do corpo da solicitação, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-156">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="6fdcd-157">Por exemplo, as seguintes assinaturas de ação causam uma exceção:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-157">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="6fdcd-158">**[FromForm]**  é inferido para os parâmetros de tipo de ação [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) e [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="6fdcd-158">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="6fdcd-159">Ele não é inferido para qualquer tipo simples ou definido pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-159">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="6fdcd-160">**[FromRoute]**  é inferido para qualquer nome de parâmetro de ação correspondente a um parâmetro no modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-160">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="6fdcd-161">Quando mais de uma rota correspondem a um parâmetro de ação, qualquer valor de rota é considerado `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-161">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="6fdcd-162">**[FromQuery]**  é inferido para todos os outros parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-162">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="6fdcd-163">As regras de inferência padrão são desabilitadas quando a propriedade [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) estiver definida como `true`.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-163">The default inference rules are disabled when the [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) property is set to `true`.</span></span> <span data-ttu-id="6fdcd-164">Adicione o seguinte código em *Startup.ConfigureServices* depois de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-164">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="6fdcd-165">Inferência de solicitação de várias partes/dados de formulário</span><span class="sxs-lookup"><span data-stu-id="6fdcd-165">Multipart/form-data request inference</span></span>

<span data-ttu-id="6fdcd-166">Quando um parâmetro de ação é anotado com o atributo [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), o tipo de conteúdo de solicitação `multipart/form-data` é inferido.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-166">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="6fdcd-167">O comportamento padrão é desabilitado quando a propriedade [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) estiver definida como `true`.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-167">The default behavior is disabled when the [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) property is set to `true`.</span></span> <span data-ttu-id="6fdcd-168">Adicione o seguinte código em *Startup.ConfigureServices* depois de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-168">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="6fdcd-169">Requisito de roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="6fdcd-169">Attribute routing requirement</span></span>

<span data-ttu-id="6fdcd-170">O roteamento de atributo se torna um requisito.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-170">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="6fdcd-171">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="6fdcd-171">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="6fdcd-172">As ações são inacessíveis por meio de [rotas convencionais](xref:mvc/controllers/routing#conventional-routing) definidas em [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) ou [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) em *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="6fdcd-172">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6fdcd-173">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="6fdcd-173">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
