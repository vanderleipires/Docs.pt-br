---
title: Migrar de API da Web do ASP.NET para o ASP.NET Core
author: ardalis
description: Saiba como migrar uma implementação da API da Web do ASP.NET Web API ao MVC do ASP.NET Core.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 9385805d548bc87f4a50b87f2c06aa74abdaf8af
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272524"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="a6745-103">Migrar de API da Web do ASP.NET para o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6745-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="a6745-104">Por [Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="a6745-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="a6745-105">APIs da Web são serviços HTTP que alcançam uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="a6745-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="a6745-106">Núcleo do ASP.NET MVC inclui suporte para a criação de APIs da Web fornecendo uma maneira simples e consistente de criação de aplicativos web.</span><span class="sxs-lookup"><span data-stu-id="a6745-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="a6745-107">Neste artigo, vamos demonstrar as etapas necessárias para migrar uma implementação da API da Web do ASP.NET Web API para MVC do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6745-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="a6745-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a6745-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="a6745-109">Revisão ASP.NET Web API do projeto</span><span class="sxs-lookup"><span data-stu-id="a6745-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="a6745-110">Este artigo usa o projeto de exemplo, *ProductsApp*, criado no artigo [guia de Introdução ao ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) como ponto de partida.</span><span class="sxs-lookup"><span data-stu-id="a6745-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="a6745-111">No projeto, um projeto de API da Web ASP.NET simple é configurado da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="a6745-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="a6745-112">Em *Global.asax.cs*, é feita uma chamada para `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="a6745-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="a6745-113">`WebApiConfig` é definido em *App_Start*, e tem apenas uma estática `Register` método:</span><span class="sxs-lookup"><span data-stu-id="a6745-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="a6745-114">Essa classe configura [roteamento de atributo](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), embora na verdade não está sendo usado no projeto.</span><span class="sxs-lookup"><span data-stu-id="a6745-114">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="a6745-115">Ele também configura a tabela de roteamento, que é usada pelo ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="a6745-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="a6745-116">Nesse caso, o ASP.NET Web API esperará URLs para corresponder ao formato */api/ {controller} / {id}*, com *{id}* opcionais.</span><span class="sxs-lookup"><span data-stu-id="a6745-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="a6745-117">O *ProductsApp* projeto inclui apenas um controlador simple, que herda de `ApiController` e expõe dois métodos:</span><span class="sxs-lookup"><span data-stu-id="a6745-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="a6745-118">Por fim, o modelo *produto*, usada pelo *ProductsApp*, é uma classe simple:</span><span class="sxs-lookup"><span data-stu-id="a6745-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="a6745-119">Agora que temos um projeto simple da qual iniciar, podemos demonstrar como migrar este projeto de API da Web para ASP.NET MVC de núcleo.</span><span class="sxs-lookup"><span data-stu-id="a6745-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="a6745-120">Criar o projeto de destino</span><span class="sxs-lookup"><span data-stu-id="a6745-120">Create the Destination Project</span></span>

<span data-ttu-id="a6745-121">Usando o Visual Studio, crie uma solução nova e vazia e nomeie- *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="a6745-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="a6745-122">Adicionar existente *ProductsApp* projeto a ele, em seguida, adicionar um novo projeto de aplicativo de Web de núcleo ASP.NET à solução.</span><span class="sxs-lookup"><span data-stu-id="a6745-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="a6745-123">Nomeie o novo projeto *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="a6745-123">Name the new project *ProductsCore*.</span></span>

![Abrir uma caixa de diálogo Nova projeto modelos da Web](webapi/_static/add-web-project.png)

<span data-ttu-id="a6745-125">Em seguida, escolha o modelo de projeto de API da Web.</span><span class="sxs-lookup"><span data-stu-id="a6745-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="a6745-126">Migraremos o *ProductsApp* conteúdo para esse novo projeto.</span><span class="sxs-lookup"><span data-stu-id="a6745-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Caixa de diálogo nova aplicativo Web com o modelo de projeto de API da Web selecionado na lista de modelos do ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="a6745-128">Excluir o `Project_Readme.html` arquivo do novo projeto.</span><span class="sxs-lookup"><span data-stu-id="a6745-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="a6745-129">Agora, sua solução deve ser assim:</span><span class="sxs-lookup"><span data-stu-id="a6745-129">Your solution should now look like this:</span></span>

![Solução de aplicativo abrir no Gerenciador de soluções mostrando arquivos e pastas dos projetos ProductsApp e ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="a6745-131">Migrar a configuração</span><span class="sxs-lookup"><span data-stu-id="a6745-131">Migrate Configuration</span></span>

<span data-ttu-id="a6745-132">Não usa o ASP.NET Core *global. asax*, *Web. config*, ou *App_Start* pastas.</span><span class="sxs-lookup"><span data-stu-id="a6745-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="a6745-133">Em vez disso, todas as tarefas de inicialização são realizadas em *Startup.cs* na raiz do projeto (consulte [inicialização do aplicativo](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="a6745-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="a6745-134">No ASP.NET MVC de núcleo, roteamento baseado em atributo agora está incluído por padrão quando `UseMvc()` é chamado; e isso é a abordagem recomendada para configurar rotas de API da Web (e é como o projeto de starter API da Web trata de roteamento).</span><span class="sxs-lookup"><span data-stu-id="a6745-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="a6745-135">Supondo que você deseja usar o roteamento de atributo em seu projeto no futuro, nenhuma configuração adicional é necessária.</span><span class="sxs-lookup"><span data-stu-id="a6745-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="a6745-136">Simplesmente aplicar os atributos conforme necessário para seus controladores e ações, como é feito no exemplo `ValuesController` classe que está incluído no projeto de starter API da Web:</span><span class="sxs-lookup"><span data-stu-id="a6745-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="a6745-137">Observe a presença de *[controller]* na linha 8.</span><span class="sxs-lookup"><span data-stu-id="a6745-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="a6745-138">Roteamento baseado em atributo agora oferece suporte a determinados símbolos, como *[controller]* e *[ação]*.</span><span class="sxs-lookup"><span data-stu-id="a6745-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="a6745-139">Esses tokens são substituídos em tempo de execução com o nome do controlador ou ação, respectivamente, para que o atributo foi aplicado.</span><span class="sxs-lookup"><span data-stu-id="a6745-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="a6745-140">Isso serve para reduzir o número de cadeias de caracteres mágicas no projeto e garante que as rotas serão mantidas sincronizadas com os respectivos controladores e ações quando refatorações renomear automaticamente são aplicadas.</span><span class="sxs-lookup"><span data-stu-id="a6745-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="a6745-141">Para migrar o controlador de API de produtos, é necessário primeiro copiar *ProductsController* para o novo projeto.</span><span class="sxs-lookup"><span data-stu-id="a6745-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="a6745-142">Basta inclua o atributo da rota no controlador:</span><span class="sxs-lookup"><span data-stu-id="a6745-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="a6745-143">Você também precisará adicionar o `[HttpGet]` atributo para os dois métodos, desde que ambos devem ser chamados por meio de HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="a6745-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="a6745-144">Incluir a expectativa de um parâmetro de "id" no atributo para `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="a6745-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="a6745-145">Neste ponto, o roteamento está configurado corretamente. No entanto, podemos ainda não é possível testá-lo.</span><span class="sxs-lookup"><span data-stu-id="a6745-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="a6745-146">Outras alterações devem ser feitas antes de *ProductsController* serão compilados.</span><span class="sxs-lookup"><span data-stu-id="a6745-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="a6745-147">Migrar modelos e controladores</span><span class="sxs-lookup"><span data-stu-id="a6745-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="a6745-148">A última etapa no processo de migração para este projeto de API da Web simple é copiar os controladores e os modelos usarem.</span><span class="sxs-lookup"><span data-stu-id="a6745-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="a6745-149">Nesse caso, basta copiar *Controllers/ProductsController.cs* do projeto original para o novo.</span><span class="sxs-lookup"><span data-stu-id="a6745-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="a6745-150">Em seguida, copie toda a pasta de modelos de projeto original para o novo.</span><span class="sxs-lookup"><span data-stu-id="a6745-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="a6745-151">Ajustar os namespaces para coincidir com o novo nome do projeto (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="a6745-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="a6745-152">Neste ponto, você pode criar o aplicativo e você encontrará um número de erros de compilação.</span><span class="sxs-lookup"><span data-stu-id="a6745-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="a6745-153">Eles geralmente devem se encaixam nas seguintes categorias:</span><span class="sxs-lookup"><span data-stu-id="a6745-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="a6745-154">*ApiController* não existe</span><span class="sxs-lookup"><span data-stu-id="a6745-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="a6745-155">*System.Web.Http* namespace não existe</span><span class="sxs-lookup"><span data-stu-id="a6745-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="a6745-156">*IHttpActionResult* não existe</span><span class="sxs-lookup"><span data-stu-id="a6745-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="a6745-157">Felizmente, elas são muito fácil corrigir:</span><span class="sxs-lookup"><span data-stu-id="a6745-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="a6745-158">Alterar *ApiController* para *controlador* (talvez seja necessário adicionar *usando Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="a6745-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="a6745-159">Excluir qualquer uso instrução referindo-se a *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="a6745-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="a6745-160">Alterar qualquer método retornando *IHttpActionResult* para retornar um *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="a6745-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="a6745-161">Depois que essas alterações foram feitas e não utilizados usando instruções removido, o migrados *ProductsController* classe tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="a6745-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="a6745-162">Agora você deve ser capaz de executar o projeto migrado e navegue até */api/produtos*; e, você deve ver a lista completa de 3 produtos.</span><span class="sxs-lookup"><span data-stu-id="a6745-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="a6745-163">Navegue até */api/products/1* e você verá o primeiro produto.</span><span class="sxs-lookup"><span data-stu-id="a6745-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a><span data-ttu-id="a6745-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span><span class="sxs-lookup"><span data-stu-id="a6745-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span></span>

<span data-ttu-id="a6745-165">É uma ferramenta útil ao migrar ASP.NET Web API projetos ASP.NET Core o [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteca.</span><span class="sxs-lookup"><span data-stu-id="a6745-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="a6745-166">A correção de compatibilidade estende ASP.NET Core para permitir que um número de diferentes convenções de API Web 2 a ser usado.</span><span class="sxs-lookup"><span data-stu-id="a6745-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="a6745-167">O exemplo movido anteriormente neste documento é bastante básico para a correção de compatibilidade não era necessária.</span><span class="sxs-lookup"><span data-stu-id="a6745-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="a6745-168">Para projetos maiores, usando a correção de compatibilidade pode ser útil para temporariamente preencher a lacuna de API entre ASP.NET Core e ASP.NET Web API 2.</span><span class="sxs-lookup"><span data-stu-id="a6745-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="a6745-169">A correção de compatibilidade de API da Web destina-se a ser usado como uma medida temporária para facilitar a migração grandes projetos de API da Web para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6745-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="a6745-170">Ao longo do tempo, os projetos devem ser atualizados para usar padrões do ASP.NET Core em vez de usar a correção de compatibilidade.</span><span class="sxs-lookup"><span data-stu-id="a6745-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span> 

<span data-ttu-id="a6745-171">Recursos de compatibilidade incluídos Microsoft.AspNetCore.Mvc.WebApiCompatShim incluem:</span><span class="sxs-lookup"><span data-stu-id="a6745-171">Compatibility features included in Microsoft.AspNetCore.Mvc.WebApiCompatShim include:</span></span>

* <span data-ttu-id="a6745-172">Adiciona um `ApiController` tipo de forma que os tipos de base dos controladores não precisam ser atualizados.</span><span class="sxs-lookup"><span data-stu-id="a6745-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="a6745-173">Habilita a associação de modelo de estilo de API da Web.</span><span class="sxs-lookup"><span data-stu-id="a6745-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="a6745-174">Funções de associação ASP.NET Core MVC modelo da mesma forma que MVC 5, por padrão.</span><span class="sxs-lookup"><span data-stu-id="a6745-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="a6745-175">As alterações de correção de compatibilidade de modelo associação a ser mais semelhante a API Web 2 convenções de associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="a6745-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="a6745-176">Por exemplo, tipos complexos são vinculados automaticamente o corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="a6745-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="a6745-177">Estende a associação de modelo para que as ações do controlador podem usar parâmetros de tipo `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="a6745-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="a6745-178">Adiciona os formatadores de mensagem que permite ações para retornar resultados do tipo `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="a6745-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="a6745-179">Adiciona métodos de resposta adicionais que ações de API Web 2 podem ter usado para atender a respostas:</span><span class="sxs-lookup"><span data-stu-id="a6745-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
    * <span data-ttu-id="a6745-180">Geradores de HttpResponseMessage:</span><span class="sxs-lookup"><span data-stu-id="a6745-180">HttpResponseMessage generators:</span></span>
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * <span data-ttu-id="a6745-181">Métodos de ação de resultados:</span><span class="sxs-lookup"><span data-stu-id="a6745-181">Action result methods:</span></span>
        * `BadResuestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* <span data-ttu-id="a6745-182">Adiciona uma instância de `IContentNegotiator` ao contêiner de injeção de dependência do aplicativo e torna o tipos relacionados a negociação de conteúdo [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) disponíveis.</span><span class="sxs-lookup"><span data-stu-id="a6745-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="a6745-183">Isso inclui tipos como `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span><span class="sxs-lookup"><span data-stu-id="a6745-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="a6745-184">Para usar a correção de compatibilidade, você precisa:</span><span class="sxs-lookup"><span data-stu-id="a6745-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="a6745-185">Referência a [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="a6745-185">Reference the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="a6745-186">Registrar serviços da correção de compatibilidade com o contêiner de injeção de dependência do aplicativo chamando `services.AddWebApiConventions()` do aplicativo `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="a6745-186">Register the compatibility shim's services with the app's DI container by calling `services.AddWebApiConventions()` in the application's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="a6745-187">Definir rotas específicos de API da Web usando `MapWebApiRoute` no `IRouteBuilder` do aplicativo `IApplicationBuilder.UseMvc` chamar.</span><span class="sxs-lookup"><span data-stu-id="a6745-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the application's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="a6745-188">Resumo</span><span class="sxs-lookup"><span data-stu-id="a6745-188">Summary</span></span>

<span data-ttu-id="a6745-189">Migrando um projeto ASP.NET Web API simple para o ASP.NET MVC de núcleo é relativamente simples, graças ao suporte interno para APIs da Web em ASP.NET MVC de núcleo.</span><span class="sxs-lookup"><span data-stu-id="a6745-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="a6745-190">As principais partes que todos os projetos ASP.NET Web API precisará migrar são rotas, controladores e modelos, junto com as atualizações para os tipos usados por controladores e ações.</span><span class="sxs-lookup"><span data-stu-id="a6745-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
