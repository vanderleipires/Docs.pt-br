---
title: Criar APIs Web com o ASP.NET Core
author: scottaddie
description: Saiba mais sobre os recursos disponíveis para a criação de uma API Web no ASP.NET Core e quando é apropriado usar cada recurso.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: d410f28ff7fda3bf33f73c06b3e626dfd4ee7dd8
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41822134"
---
# <a name="build-web-apis-with-aspnet-core"></a>Criar APIs Web com o ASP.NET Core

Por [Scott Addie](https://github.com/scottaddie)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

Este documento explica como criar uma API Web no ASP.NET Core e quando é mais adequado usar cada recurso.

## <a name="derive-class-from-controllerbase"></a>Derivar a classe por meio do ControllerBase

Herde da classe <xref:Microsoft.AspNetCore.Mvc.ControllerBase> em um controlador destinado a funcionar como uma API Web. Por exemplo:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

A classe `ControllerBase` fornece acesso a várias propriedades e métodos. No código anterior, os exemplos incluem <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> e <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>. Esses métodos são chamados em métodos de ação para retornar os códigos de status HTTP 400 e 201, respectivamente. A propriedade <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, também fornecida pelo `ControllerBase`, é acessada para executar a validação do modelo de solicitação.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Anotar classe com o ApiControllerAttribute

O ASP.NET Core 2.1 apresenta o atributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) para denotar uma classe do controlador API Web. Por exemplo:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Uma versão de compatibilidade do 2.1 ou posteriores, definida por meio de <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, é necessária para usar esse atributo. Por exemplo, o código realçado em *Startup.ConfigureServices* define o sinalizador de compatibilidade 2.1:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Para obter mais informações, consulte <xref:mvc/compatibility-version>.

Geralmente, o atributo `[ApiController]` é associado ao `ControllerBase` para habilitar o comportamento específico de REST para controladores. `ControllerBase` fornece acesso a métodos como <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> e <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.

Outra abordagem é criar uma classe de base de controlador personalizada anotada com o atributo `[ApiController]`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

As seções a seguir descrevem os recursos de conveniência adicionados pelo atributo.

### <a name="automatic-http-400-responses"></a>Respostas automáticas do HTTP 400

Os erros de validação disparam uma resposta HTTP 400 automaticamente. O código a seguir se torna desnecessário em suas ações:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

O comportamento padrão é desabilitado quando a propriedade <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> está definida como `true`. Adicione o seguinte código em *Startup.ConfigureServices* depois de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Inferência de parâmetro de origem de associação

Um atributo de origem de associação define o local no qual o valor do parâmetro de uma ação é encontrado. Os seguintes atributos da origem da associação existem:

|Atributo|Fonte de associação |
|---------|---------|
|**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**     | Corpo da solicitação |
|**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**     | Dados do formulário no corpo da solicitação |
|**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)** | Cabeçalho da solicitação |
|**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**   | Parâmetro de cadeia de caracteres de consulta de solicitação |
|**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**   | Dados de rota da solicitação atual |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | O serviço de solicitação inserido como um parâmetro de ação |

> [!WARNING]
> Não use `[FromRoute]` quando os valores puderem conter `%2f` (ou seja, `/`). `%2f` não ficará sem escape para `/`. Use `[FromQuery]`, se o valor puder conter `%2f`.

Sem o atributo `[ApiController]`, os atributos da origem da associação são definidos explicitamente. No exemplo a seguir, o atributo `[FromQuery]` indica que o valor do parâmetro `discontinuedOnly` é fornecido na cadeia de caracteres de consulta da URL de solicitação:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Regras de inferência são aplicadas para as fontes de dados padrão dos parâmetros de ação. Essas regras configuram as origens da associação que você provavelmente aplicaria manualmente aos parâmetros de ação. Os atributos da origem da associação se comportam da seguinte maneira:

* **[FromBody]**  é inferido para parâmetros de tipo complexo. Uma exceção a essa regra é qualquer tipo interno complexo com um significado especial, como <xref:Microsoft.AspNetCore.Http.IFormCollection> e <xref:System.Threading.CancellationToken>. O código de inferência da origem da associação ignora esses tipos especiais. `[FromBody]` não é inferido para tipos simples, como `string` ou `int`. Portanto, o atributo `[FromBody]` deve ser usado para tipos simples quando essa funcionalidade é desejada. Quando uma ação tiver mais de um parâmetro especificado explicitamente (via `[FromBody]`) ou inferido como limite do corpo da solicitação, uma exceção será lançada. Por exemplo, as seguintes assinaturas de ação causam uma exceção:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** é inferido para parâmetros de ação do tipo <xref:Microsoft.AspNetCore.Http.IFormFile> e <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. Ele não é inferido para qualquer tipo simples ou definido pelo usuário.
* **[FromRoute]**  é inferido para qualquer nome de parâmetro de ação correspondente a um parâmetro no modelo de rota. Quando mais de uma rota correspondem a um parâmetro de ação, qualquer valor de rota é considerado `[FromRoute]`.
* **[FromQuery]**  é inferido para todos os outros parâmetros de ação.

As regras de inferência padrão são desabilitadas quando a propriedade <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> está definida como `true`. Adicione o seguinte código em *Startup.ConfigureServices* depois de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Inferência de solicitação de várias partes/dados de formulário

Quando um parâmetro de ação é anotado com o atributo [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), o tipo de conteúdo de solicitação `multipart/form-data` é inferido.

O comportamento padrão é desabilitado quando a propriedade <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> está definida como `true`. Adicione o seguinte código em *Startup.ConfigureServices* depois de `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Requisito de roteamento de atributo

O roteamento de atributo se torna um requisito. Por exemplo:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

As ações são inacessíveis por meio de [rotas convencionais](xref:mvc/controllers/routing#conventional-routing) definidas em <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> ou por <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> em *Startup.Configure*.

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionais

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
