---
title: Migrar da API Web ASP.NET para ASP.NET Core
author: ardalis
description: Saiba como migrar uma implementação de API da web da API de Web do ASP.NET 4.x para ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/01/2018
uid: migration/webapi
ms.openlocfilehash: f5d886a7c3182b5cd372762ade67c2e748051049
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207271"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="6536b-103">Migrar da API Web ASP.NET para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6536b-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="6536b-104">Por [Scott Addie](https://twitter.com/scott_addie) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6536b-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6536b-105">Uma API da Web do ASP.NET 4. x é um serviço HTTP que atinja uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="6536b-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="6536b-106">Unifica o ASP.NET Core MVC do ASP.NET do 4.x e modelos de aplicativo de API da Web em um modelo de programação mais simples conhecido como o ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="6536b-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="6536b-107">Este artigo demonstra as etapas necessárias para migrar da API de Web do ASP.NET 4.x para ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="6536b-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="6536b-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([como baixar](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6536b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6536b-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="6536b-109">Prerequisites</span></span>

* [<span data-ttu-id="6536b-110">SDK do .NET Core 2.1 ou posteriores</span><span class="sxs-lookup"><span data-stu-id="6536b-110">.NET Core 2.1 SDK or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="6536b-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versão 15.7.3 ou posterior com a carga de trabalho do **ASP.NET e desenvolvimento para a Web**</span><span class="sxs-lookup"><span data-stu-id="6536b-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="6536b-112">Examine o projeto de API da Web do ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="6536b-112">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="6536b-113">Como ponto de partida, este artigo usa o *ProductsApp* projeto criado no [Introdução à API Web ASP.NET 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span><span class="sxs-lookup"><span data-stu-id="6536b-113">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="6536b-114">Nesse projeto, um projeto de API Web simples ASP.NET 4.x está configurado da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="6536b-114">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="6536b-115">Na *Global.asax.cs*, é feita uma chamada para `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="6536b-115">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="6536b-116">`WebApiConfig` é definido na *App_Start* pasta.</span><span class="sxs-lookup"><span data-stu-id="6536b-116">`WebApiConfig` is defined in the *App_Start* folder.</span></span> <span data-ttu-id="6536b-117">Ele tem apenas um estático `Register` método:</span><span class="sxs-lookup"><span data-stu-id="6536b-117">It has just one static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15-20)]

<span data-ttu-id="6536b-118">Essa classe configura [roteamento de atributo](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), embora na verdade não está sendo usado no projeto.</span><span class="sxs-lookup"><span data-stu-id="6536b-118">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="6536b-119">Ele também configura a tabela de roteamento, que é usada pela API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6536b-119">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="6536b-120">Nesse caso, a API da Web do ASP.NET 4.x espera que as URLs para corresponder ao formato `/api/{controller}/{id}`, com `{id}` opcionais.</span><span class="sxs-lookup"><span data-stu-id="6536b-120">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="6536b-121">O *ProductsApp* projeto inclui um controlador.</span><span class="sxs-lookup"><span data-stu-id="6536b-121">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="6536b-122">O controlador herda de `ApiController` e expõe dois métodos:</span><span class="sxs-lookup"><span data-stu-id="6536b-122">The controller inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="6536b-123">O `Product` modelo usado pelo `ProductsController` é uma classe simples:</span><span class="sxs-lookup"><span data-stu-id="6536b-123">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="6536b-124">As seções a seguir demonstram a migração do projeto API da Web para ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="6536b-124">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="6536b-125">Criar o projeto de destino</span><span class="sxs-lookup"><span data-stu-id="6536b-125">Create destination project</span></span>

<span data-ttu-id="6536b-126">Conclua as seguintes etapas no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6536b-126">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="6536b-127">Vá para **arquivo** > **nova** > **projeto** > **outros tipos de projeto**  >  **Soluções do visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="6536b-127">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="6536b-128">Selecione **solução em branco**e o nome da solução *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="6536b-128">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="6536b-129">Clique o **Okey** botão.</span><span class="sxs-lookup"><span data-stu-id="6536b-129">Click the **OK** button.</span></span>
* <span data-ttu-id="6536b-130">Adicionar existente *ProductsApp* projeto à solução.</span><span class="sxs-lookup"><span data-stu-id="6536b-130">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="6536b-131">Adicione um novo **aplicativo Web ASP.NET Core** projeto à solução.</span><span class="sxs-lookup"><span data-stu-id="6536b-131">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="6536b-132">Selecione o **.NET Core** estrutura na lista suspensa de destino e, em seguida, selecione o **API** modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="6536b-132">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="6536b-133">Nomeie o projeto *ProductsCore*e clique no **Okey** botão.</span><span class="sxs-lookup"><span data-stu-id="6536b-133">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="6536b-134">Agora, a solução contém dois projetos.</span><span class="sxs-lookup"><span data-stu-id="6536b-134">The solution now contains two projects.</span></span> <span data-ttu-id="6536b-135">As seções a seguir explicam a migrar os *ProductsApp* o conteúdo do projeto para o *ProductsCore* projeto.</span><span class="sxs-lookup"><span data-stu-id="6536b-135">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="6536b-136">Migrar configuração</span><span class="sxs-lookup"><span data-stu-id="6536b-136">Migrate configuration</span></span>

<span data-ttu-id="6536b-137">ASP.NET Core não usa o *App_Start* pasta ou o *global. asax* arquivo e o *Web. config* arquivo for adicionado no momento da publicação.</span><span class="sxs-lookup"><span data-stu-id="6536b-137">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="6536b-138">*Startup.CS* é o substituto do *global. asax* e está localizado na raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="6536b-138">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="6536b-139">O `Startup` classe manipula todas as tarefas de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6536b-139">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="6536b-140">Para obter mais informações, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="6536b-140">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="6536b-141">No ASP.NET Core MVC, o roteamento de atributo é incluído por padrão quando <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> é chamado no `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="6536b-141">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="6536b-142">O seguinte `UseMvc` chamar substitui o *ProductsApp* do projeto *app_start* arquivo:</span><span class="sxs-lookup"><span data-stu-id="6536b-142">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="6536b-143">Migrar modelos e controladores</span><span class="sxs-lookup"><span data-stu-id="6536b-143">Migrate models and controllers</span></span>

<span data-ttu-id="6536b-144">Copiar sobre a *ProductApp* controlador do projeto e o modelo utilizado.</span><span class="sxs-lookup"><span data-stu-id="6536b-144">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="6536b-145">Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="6536b-145">Follow these steps:</span></span>

1. <span data-ttu-id="6536b-146">Cópia *Controllers/ProductsController.cs* do projeto original para o novo.</span><span class="sxs-lookup"><span data-stu-id="6536b-146">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="6536b-147">Copiar todo o *modelos* pasta do projeto original para o novo.</span><span class="sxs-lookup"><span data-stu-id="6536b-147">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="6536b-148">Alterar os namespaces dos arquivos copiados para coincidir com o novo nome do projeto (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="6536b-148">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="6536b-149">Ajustar a `using ProductsApp.Models;` instrução no *ProductsController.cs* muito.</span><span class="sxs-lookup"><span data-stu-id="6536b-149">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="6536b-150">Neste ponto, criando os resultados do aplicativo em um número de erros de compilação.</span><span class="sxs-lookup"><span data-stu-id="6536b-150">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="6536b-151">Os erros ocorrem porque os seguintes componentes não existem no ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="6536b-151">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="6536b-152">Classe `ApiController`</span><span class="sxs-lookup"><span data-stu-id="6536b-152">`ApiController` class</span></span>
* <span data-ttu-id="6536b-153">Namespace `System.Web.Http`</span><span class="sxs-lookup"><span data-stu-id="6536b-153">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="6536b-154">`IHttpActionResult` Interface</span><span class="sxs-lookup"><span data-stu-id="6536b-154">`IHttpActionResult` interface</span></span>

<span data-ttu-id="6536b-155">Corrija os erros da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="6536b-155">Fix the errors as follows:</span></span>

1. <span data-ttu-id="6536b-156">Alteração `ApiController` para <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="6536b-156">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="6536b-157">Adicione `using Microsoft.AspNetCore.Mvc;` para resolver o `ControllerBase` referência.</span><span class="sxs-lookup"><span data-stu-id="6536b-157">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="6536b-158">Excluir `using System.Web.Http;`.</span><span class="sxs-lookup"><span data-stu-id="6536b-158">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="6536b-159">Alterar o `GetProduct` tipo de retorno da ação de `IHttpActionResult` para `ActionResult<Product>`.</span><span class="sxs-lookup"><span data-stu-id="6536b-159">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

## <a name="configure-routing"></a><span data-ttu-id="6536b-160">Configurar o roteamento</span><span class="sxs-lookup"><span data-stu-id="6536b-160">Configure routing</span></span>

<span data-ttu-id="6536b-161">Configure o roteamento da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="6536b-161">Configure routing as follows:</span></span>

1. <span data-ttu-id="6536b-162">Decore o `ProductsController` classe com os seguintes atributos:</span><span class="sxs-lookup"><span data-stu-id="6536b-162">Decorate the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="6536b-163">Precedente [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) configura o atributo padrão de roteamento de atributo do controlador.</span><span class="sxs-lookup"><span data-stu-id="6536b-163">The preceding [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="6536b-164">O [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atributo faz o roteamento de um requisito para todas as ações nesse controlador de atributo.</span><span class="sxs-lookup"><span data-stu-id="6536b-164">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="6536b-165">Roteamento de atributo oferece suporte a tokens, como `[controller]` e `[action]`.</span><span class="sxs-lookup"><span data-stu-id="6536b-165">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="6536b-166">Em tempo de execução, cada token é substituído com o nome do controlador ou ação, respectivamente, para o qual o atributo foi aplicado.</span><span class="sxs-lookup"><span data-stu-id="6536b-166">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="6536b-167">Os tokens de reduzem o número de cadeias de caracteres mágicas no projeto.</span><span class="sxs-lookup"><span data-stu-id="6536b-167">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="6536b-168">Os tokens de também garantem rotas permanecerem sincronizadas com os correspondentes controladores e ações quando automático renomear refatorações são aplicadas.</span><span class="sxs-lookup"><span data-stu-id="6536b-168">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="6536b-169">Habilitar as solicitações HTTP Get para o `ProductController` ações:</span><span class="sxs-lookup"><span data-stu-id="6536b-169">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="6536b-170">Aplicar a [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) atributo para o `GetAllProducts` ação.</span><span class="sxs-lookup"><span data-stu-id="6536b-170">Apply the [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="6536b-171">Aplicar a `[HttpGet("{id}")]` de atributo para o `GetProduct` ação.</span><span class="sxs-lookup"><span data-stu-id="6536b-171">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="6536b-172">Após essas alterações e a remoção do não utilizado `using` instruções *ProductsController.cs* arquivo tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="6536b-172">After these changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="6536b-173">Execute o projeto migrado e navegue até `/api/products`.</span><span class="sxs-lookup"><span data-stu-id="6536b-173">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="6536b-174">É exibida uma lista completa dos três produtos.</span><span class="sxs-lookup"><span data-stu-id="6536b-174">A full list of three products appears.</span></span> <span data-ttu-id="6536b-175">Navegue para `/api/products/1`.</span><span class="sxs-lookup"><span data-stu-id="6536b-175">Browse to `/api/products/1`.</span></span> <span data-ttu-id="6536b-176">O primeiro produto será exibida.</span><span class="sxs-lookup"><span data-stu-id="6536b-176">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="6536b-177">Correção de compatibilidade</span><span class="sxs-lookup"><span data-stu-id="6536b-177">Compatibility shim</span></span>

<span data-ttu-id="6536b-178">O [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteca fornece um shim de compatibilidade para mover projetos de API da Web do ASP.NET 4.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6536b-178">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="6536b-179">O shim de compatibilidade se estende do ASP.NET Core para dar suporte a uma série de convenções da API de Web do ASP.NET 4. x 2.</span><span class="sxs-lookup"><span data-stu-id="6536b-179">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="6536b-180">O exemplo portado anteriormente neste documento é bastante básico que o shim de compatibilidade era desnecessário.</span><span class="sxs-lookup"><span data-stu-id="6536b-180">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="6536b-181">Para projetos maiores, usem o shim de compatibilidade pode ser útil para temporariamente preenchendo a lacuna de API entre o ASP.NET Core e ASP.NET 4.x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="6536b-181">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="6536b-182">O shim de compatibilidade de API da Web destina-se a ser usado como uma medida temporária para dar suporte à migração grande API da Web projetos do ASP.NET 4.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6536b-182">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="6536b-183">Ao longo do tempo, os projetos devem ser atualizados para usar os padrões do ASP.NET Core em vez de usar o shim de compatibilidade.</span><span class="sxs-lookup"><span data-stu-id="6536b-183">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="6536b-184">Recursos de compatibilidade incluídos no `Microsoft.AspNetCore.Mvc.WebApiCompatShim` incluem:</span><span class="sxs-lookup"><span data-stu-id="6536b-184">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="6536b-185">Adiciona um `ApiController` tipo de forma que os tipos de base dos controladores não precisam ser atualizados.</span><span class="sxs-lookup"><span data-stu-id="6536b-185">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="6536b-186">Habilita a associação de modelo de estilo de API da Web.</span><span class="sxs-lookup"><span data-stu-id="6536b-186">Enables Web API-style model binding.</span></span> <span data-ttu-id="6536b-187">Funções de associação de maneira semelhante do ASP.NET de modelo do ASP.NET Core MVC 4. x MVC 5, por padrão.</span><span class="sxs-lookup"><span data-stu-id="6536b-187">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="6536b-188">As alterações de correção de compatibilidade associação de modelo para ser mais semelhante a convenções de associação de modelo do ASP.NET Web API 2 de 4. x.</span><span class="sxs-lookup"><span data-stu-id="6536b-188">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="6536b-189">Por exemplo, tipos complexos são vinculados automaticamente do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="6536b-189">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="6536b-190">Estende a associação de modelo para que as ações do controlador podem usar parâmetros de tipo `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="6536b-190">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="6536b-191">Adiciona os formatadores de mensagem, permitindo que as ações para retornar os resultados do tipo `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="6536b-191">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="6536b-192">Adiciona métodos de resposta adicionais que ações de API Web 2 podem ter usado para servir as respostas:</span><span class="sxs-lookup"><span data-stu-id="6536b-192">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="6536b-193">`HttpResponseMessage` geradores de:</span><span class="sxs-lookup"><span data-stu-id="6536b-193">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="6536b-194">Métodos de ação de resultados:</span><span class="sxs-lookup"><span data-stu-id="6536b-194">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="6536b-195">Adiciona uma instância do `IContentNegotiator` para o aplicativo disponibiliza os tipos de conteúdo relacionado a negociação do e do contêiner de DI (injeção) de dependência [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span><span class="sxs-lookup"><span data-stu-id="6536b-195">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="6536b-196">Exemplos de tais tipos `DefaultContentNegotiator` e `MediaTypeFormatter`.</span><span class="sxs-lookup"><span data-stu-id="6536b-196">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="6536b-197">Para usar o shim de compatibilidade:</span><span class="sxs-lookup"><span data-stu-id="6536b-197">To use the compatibility shim:</span></span>

1. <span data-ttu-id="6536b-198">Instalar o [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="6536b-198">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="6536b-199">Registrar os serviços do shim de compatibilidade com o contêiner de injeção de dependência do aplicativo chamando `services.AddMvc().AddWebApiConventions()` em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6536b-199">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="6536b-200">Definir web específicos de API roteia usando `MapWebApiRoute` sobre o `IRouteBuilder` no aplicativo do `IApplicationBuilder.UseMvc` chamar.</span><span class="sxs-lookup"><span data-stu-id="6536b-200">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6536b-201">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="6536b-201">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
