---
title: Migrando de API da Web do ASP.NET
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4f0564b4-ed4e-4e1e-9755-c1144d21a0ef
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: 4acb7ccf7f944df5d08ac7faa342f0c72cf9d1a7
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2017
---
# <a name="migrating-from-aspnet-web-api"></a><span data-ttu-id="d0eb2-103">Migrando de API da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d0eb2-103">Migrating from ASP.NET Web API</span></span>

<span data-ttu-id="d0eb2-104">Por [Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="d0eb2-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="d0eb2-105">APIs da Web são serviços HTTP que alcançam uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="d0eb2-106">Núcleo do ASP.NET MVC inclui suporte para a criação de APIs da Web fornecendo uma maneira simples e consistente de criação de aplicativos web.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="d0eb2-107">Neste artigo, vamos demonstrar as etapas necessárias para migrar uma implementação da API da Web do ASP.NET Web API para MVC do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="d0eb2-108">[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d0eb2-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="d0eb2-109">Revisão ASP.NET Web API do projeto</span><span class="sxs-lookup"><span data-stu-id="d0eb2-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="d0eb2-110">Este artigo usa o projeto de exemplo, *ProductsApp*, criado no artigo [guia de Introdução ao ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) como ponto de partida.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="d0eb2-111">No projeto, um projeto de API da Web ASP.NET simple é configurado da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="d0eb2-112">Em *Global.asax.cs*, é feita uma chamada para `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="d0eb2-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="d0eb2-113">`WebApiConfig`é definido em *App_Start*, e tem apenas uma estática `Register` método:</span><span class="sxs-lookup"><span data-stu-id="d0eb2-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="d0eb2-114">Essa classe configura [roteamento de atributo](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), embora na verdade não está sendo usado no projeto.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-114">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="d0eb2-115">Ele também configura a tabela de roteamento que é usada pela API da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-115">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="d0eb2-116">Nesse caso, o ASP.NET Web API esperará URLs para corresponder ao formato */api/ {controller} / {id}*, com *{id}* opcionais.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="d0eb2-117">O *ProductsApp* projeto inclui apenas um controlador simple, que herda de `ApiController` e expõe dois métodos:</span><span class="sxs-lookup"><span data-stu-id="d0eb2-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="d0eb2-118">Por fim, o modelo *produto*, usada pelo *ProductsApp*, é uma classe simple:</span><span class="sxs-lookup"><span data-stu-id="d0eb2-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="d0eb2-119">Agora que temos um projeto simple da qual iniciar, podemos demonstrar como migrar este projeto de API da Web para ASP.NET MVC de núcleo.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="d0eb2-120">Criar o projeto de destino</span><span class="sxs-lookup"><span data-stu-id="d0eb2-120">Create the Destination Project</span></span>

<span data-ttu-id="d0eb2-121">Usando o Visual Studio, crie uma solução nova e vazia e nomeie- *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="d0eb2-122">Adicionar existente *ProductsApp* projeto a ele, em seguida, adicionar um novo projeto de aplicativo de Web de núcleo ASP.NET à solução.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="d0eb2-123">Nomeie o novo projeto *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-123">Name the new project *ProductsCore*.</span></span>

![Abrir uma caixa de diálogo Nova projeto modelos da Web](webapi/_static/add-web-project.png)

<span data-ttu-id="d0eb2-125">Em seguida, escolha o modelo de projeto de API da Web.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="d0eb2-126">Migraremos o *ProductsApp* conteúdo para esse novo projeto.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Caixa de diálogo nova aplicativo Web com o modelo de projeto de API da Web selecionado na lista de modelos do ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="d0eb2-128">Excluir o `Project_Readme.html` arquivo do novo projeto.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="d0eb2-129">Agora, sua solução deve ser assim:</span><span class="sxs-lookup"><span data-stu-id="d0eb2-129">Your solution should now look like this:</span></span>

![Solução de aplicativo abrir no Gerenciador de soluções mostrando arquivos e pastas dos projetos ProductsApp e ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="d0eb2-131">Migrar a configuração</span><span class="sxs-lookup"><span data-stu-id="d0eb2-131">Migrate Configuration</span></span>

<span data-ttu-id="d0eb2-132">Não usa o ASP.NET Core *global. asax*, *Web. config*, ou *App_Start* pastas.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="d0eb2-133">Em vez disso, todas as tarefas de inicialização são realizadas em *Startup.cs* na raiz do projeto (consulte [inicialização do aplicativo](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="d0eb2-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="d0eb2-134">No ASP.NET MVC de núcleo, roteamento baseado em atributo agora está incluído por padrão quando `UseMvc()` é chamado; e isso é a abordagem recomendada para configurar rotas de API da Web (e é como o projeto de starter API da Web trata de roteamento).</span><span class="sxs-lookup"><span data-stu-id="d0eb2-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

<span data-ttu-id="d0eb2-135">Supondo que você deseja usar o roteamento de atributo em seu projeto no futuro, nenhuma configuração adicional é necessária.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="d0eb2-136">Simplesmente aplicar os atributos conforme necessário para seus controladores e ações, como é feito no exemplo `ValuesController` classe que está incluído no projeto de starter API da Web:</span><span class="sxs-lookup"><span data-stu-id="d0eb2-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that is included in the Web API starter project:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="d0eb2-137">Observe a presença de *[controller]* na linha 8.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="d0eb2-138">Roteamento baseado em atributo agora oferece suporte a determinados símbolos, como *[controller]* e *[ação]*.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="d0eb2-139">Esses tokens são substituídos em tempo de execução com o nome do controlador ou ação, respectivamente, para que o atributo foi aplicado.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="d0eb2-140">Isso serve para reduzir o número de cadeias de caracteres mágicas no projeto e garante que as rotas serão mantidas sincronizadas com os respectivos controladores e ações quando refatorações renomear automaticamente são aplicadas.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="d0eb2-141">Para migrar o controlador de API de produtos, é necessário primeiro copiar *ProductsController* para o novo projeto.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="d0eb2-142">Basta inclua o atributo da rota no controlador:</span><span class="sxs-lookup"><span data-stu-id="d0eb2-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="d0eb2-143">Você também precisará adicionar o `[HttpGet]` atributo para os dois métodos, desde que ambos devem ser chamados por meio de HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="d0eb2-144">Incluir a expectativa de um parâmetro de "id" no atributo para `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="d0eb2-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="d0eb2-145">Neste ponto, o roteamento está configurado corretamente. No entanto, podemos ainda não é possível testá-lo.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="d0eb2-146">Outras alterações devem ser feitas antes de *ProductsController* serão compilados.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="d0eb2-147">Migrar modelos e controladores</span><span class="sxs-lookup"><span data-stu-id="d0eb2-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="d0eb2-148">A última etapa no processo de migração para este projeto de API da Web simple é copiar os controladores e os modelos usarem.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="d0eb2-149">Nesse caso, basta copiar *Controllers/ProductsController.cs* do projeto original para o novo.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="d0eb2-150">Em seguida, copie toda a pasta de modelos de projeto original para o novo.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="d0eb2-151">Ajustar os namespaces para coincidir com o novo nome do projeto (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="d0eb2-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="d0eb2-152">Neste ponto, você pode criar o aplicativo e você encontrará um número de erros de compilação.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="d0eb2-153">Eles geralmente devem se encaixam nas seguintes categorias:</span><span class="sxs-lookup"><span data-stu-id="d0eb2-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="d0eb2-154">*ApiController* não existe</span><span class="sxs-lookup"><span data-stu-id="d0eb2-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="d0eb2-155">*System.Web.Http* namespace não existe</span><span class="sxs-lookup"><span data-stu-id="d0eb2-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="d0eb2-156">*IHttpActionResult* não existe</span><span class="sxs-lookup"><span data-stu-id="d0eb2-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="d0eb2-157">Felizmente, elas são muito fácil corrigir:</span><span class="sxs-lookup"><span data-stu-id="d0eb2-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="d0eb2-158">Alterar *ApiController* para *controlador* (talvez seja necessário adicionar *usando Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="d0eb2-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="d0eb2-159">Excluir qualquer uso instrução referindo-se a *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="d0eb2-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="d0eb2-160">Alterar qualquer método retornando *IHttpActionResult* para retornar um *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="d0eb2-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="d0eb2-161">Depois que essas alterações foram feitas e não utilizados usando instruções removido, o migrados *ProductsController* classe tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="d0eb2-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="d0eb2-162">Agora você deve ser capaz de executar o projeto migrado e navegue até */api/produtos*; e, você deve ver a lista completa de 3 produtos.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="d0eb2-163">Navegue até */api/products/1* e você verá o primeiro produto.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="d0eb2-164">Resumo</span><span class="sxs-lookup"><span data-stu-id="d0eb2-164">Summary</span></span>

<span data-ttu-id="d0eb2-165">Migrando um projeto ASP.NET Web API simple para o ASP.NET MVC de núcleo é relativamente simples, graças ao suporte interno para APIs da Web em ASP.NET MVC de núcleo.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-165">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="d0eb2-166">As principais partes que todos os projetos ASP.NET Web API precisará migrar são rotas, controladores e modelos, junto com as atualizações para os tipos usados por controladores e ações.</span><span class="sxs-lookup"><span data-stu-id="d0eb2-166">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
