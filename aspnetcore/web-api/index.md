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
# <a name="build-web-apis-with-aspnet-core"></a>Criar APIs Web com o ASP.NET Core

Por [Scott Addie](https://github.com/scottaddie)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

Este documento explica como criar uma API Web no ASP.NET Core e quando é mais adequado usar cada recurso.

## <a name="derive-class-from-controllerbase"></a>Derivar a classe por meio do ControllerBase

Herde da classe [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) em um controlador destinado a servir como uma API Web. Por exemplo:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

A classe `ControllerBase` fornece acesso a várias propriedades e métodos. No exemplo anterior, alguns desses métodos incluem [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) e [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Esses métodos são chamados em métodos de ação para retornar os códigos de status HTTP 400 e 201, respectivamente. A propriedade [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), também fornecida pelo `ControllerBase`, é acessada para executar a validação do modelo de solicitação.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Anotar classe com o ApiControllerAttribute

O ASP.NET Core 2.1 apresenta o atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) para denotar uma classe do controlador API Web. Por exemplo:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Uma versão de compatibilidade do 2.1 ou posteriores, definida por meio de [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), é necessária para usar esse atributo. Por exemplo, o código realçado em *Startup.ConfigureServices* define o sinalizador de compatibilidade 2.1:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Geralmente, o atributo `[ApiController]` é associado ao `ControllerBase` para habilitar o comportamento específico de REST para controladores. `ControllerBase` fornece acesso aos métodos como [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) e [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).

Outra abordagem é criar uma classe de base de controlador personalizada anotada com o atributo `[ApiController]`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

As seções a seguir descrevem os recursos de conveniência adicionados pelo atributo.

### <a name="automatic-http-400-responses"></a>Respostas automáticas do HTTP 400

Os erros de validação disparam uma resposta HTTP 400 automaticamente. O código a seguir se torna desnecessário em suas ações:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

O comportamento padrão é desabilitado quando a propriedade [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) estiver definida como `true`. Adicione o seguinte código em *Startup.ConfigureServices* depois de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Inferência de parâmetro de origem de associação

Um atributo de origem de associação define o local no qual o valor do parâmetro de uma ação é encontrado. Os seguintes atributos da origem da associação existem:

|Atributo|Fonte de associação |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | Corpo da solicitação |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | Dados do formulário no corpo da solicitação |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | Cabeçalho da solicitação |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | Parâmetro de cadeia de caracteres de consulta de solicitação |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | Dados de rota da solicitação atual |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | O serviço de solicitação inserido como um parâmetro de ação |

> [!WARNING]
> Não use `[FromRoute]` quando os valores puderem conter `%2f` (ou seja, `/`). `%2f` não ficará sem escape para `/`. Use `[FromQuery]`, se o valor puder conter `%2f`.

Sem o atributo `[ApiController]`, os atributos da origem da associação são definidos explicitamente. No exemplo a seguir, o atributo `[FromQuery]` indica que o valor do parâmetro `discontinuedOnly` é fornecido na cadeia de caracteres de consulta da URL de solicitação:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Regras de inferência são aplicadas para as fontes de dados padrão dos parâmetros de ação. Essas regras configuram as origens da associação que você provavelmente aplicaria manualmente aos parâmetros de ação. Os atributos da origem da associação se comportam da seguinte maneira:

* **[FromBody]**  é inferido para parâmetros de tipo complexo. Uma exceção a essa regra é qualquer tipo complexo, interno com um significado especial, como [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) e [CancellationToken](/dotnet/api/system.threading.cancellationtoken). O código de inferência da origem da associação ignora esses tipos especiais. Quando uma ação tiver mais de um parâmetro especificado explicitamente (via `[FromBody]`) ou inferido como limite do corpo da solicitação, uma exceção será lançada. Por exemplo, as seguintes assinaturas de ação causam uma exceção:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]**  é inferido para os parâmetros de tipo de ação [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) e [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection). Ele não é inferido para qualquer tipo simples ou definido pelo usuário.
* **[FromRoute]**  é inferido para qualquer nome de parâmetro de ação correspondente a um parâmetro no modelo de rota. Quando mais de uma rota correspondem a um parâmetro de ação, qualquer valor de rota é considerado `[FromRoute]`.
* **[FromQuery]**  é inferido para todos os outros parâmetros de ação.

As regras de inferência padrão são desabilitadas quando a propriedade [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) estiver definida como `true`. Adicione o seguinte código em *Startup.ConfigureServices* depois de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Inferência de solicitação de várias partes/dados de formulário

Quando um parâmetro de ação é anotado com o atributo [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), o tipo de conteúdo de solicitação `multipart/form-data` é inferido.

O comportamento padrão é desabilitado quando a propriedade [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) estiver definida como `true`. Adicione o seguinte código em *Startup.ConfigureServices* depois de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Requisito de roteamento de atributo

O roteamento de atributo se torna um requisito. Por exemplo:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

As ações são inacessíveis por meio de [rotas convencionais](xref:mvc/controllers/routing#conventional-routing) definidas em [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) ou [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) em *Startup.Configure*.

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionais

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
