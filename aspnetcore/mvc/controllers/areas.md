---
title: "Áreas"
author: rick-anderson
description: "Mostra como trabalhar com áreas."
keywords: "ASP.NET Core, áreas, roteamento, modos de exibição"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 5e16d5e8-5696-4cb2-8ec7-d36be305c922
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: cd0302fa1668979df9bbd6cb36f82742d325c5e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="areas"></a><span data-ttu-id="5be1a-104">Áreas</span><span class="sxs-lookup"><span data-stu-id="5be1a-104">Areas</span></span>

<span data-ttu-id="5be1a-105">Por [Dhananjay Kumar](https://twitter.com/debug_mode) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5be1a-105">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5be1a-106">Áreas são um recurso do ASP.NET MVC usado para organizar as funcionalidades relacionadas em um grupo como um namespace separado (para o roteamento) e a estrutura de pasta (para modos de exibição).</span><span class="sxs-lookup"><span data-stu-id="5be1a-106">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="5be1a-107">Usando áreas cria uma hierarquia com a finalidade de roteamento, adicionando outro parâmetro de rota, `area`, `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="5be1a-107">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="5be1a-108">Áreas fornecem uma forma de particionar um aplicativo da Web MVC do ASP.NET Core grande em menores agrupamentos funcionais.</span><span class="sxs-lookup"><span data-stu-id="5be1a-108">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="5be1a-109">Uma área é efetivamente uma estrutura MVC dentro de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5be1a-109">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="5be1a-110">Em um projeto MVC, componentes lógicos como modelo, o controlador e o modo de exibição são mantidos em pastas diferentes e MVC usa convenções de nomenclatura para criar a relação entre esses componentes.</span><span class="sxs-lookup"><span data-stu-id="5be1a-110">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="5be1a-111">Para um aplicativo grande, pode ser vantajoso para dividir o aplicativo em áreas separadas de nível alto de funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="5be1a-111">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="5be1a-112">Por exemplo, um aplicativo de comércio eletrônico com várias unidades de negócios, como check-out, cobrança e pesquisa etc. Cada uma dessas unidades têm seus próprios modos de exibição do componente lógico, controladores e modelos.</span><span class="sxs-lookup"><span data-stu-id="5be1a-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="5be1a-113">Nesse cenário, você pode usar áreas para particionar fisicamente os componentes comerciais no mesmo projeto.</span><span class="sxs-lookup"><span data-stu-id="5be1a-113">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="5be1a-114">Uma área pode ser definida como menores unidades funcionais em um projeto MVC do ASP.NET Core com seu próprio conjunto de controladores, modos de exibição e modelos.</span><span class="sxs-lookup"><span data-stu-id="5be1a-114">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="5be1a-115">Considere o uso de áreas em um MVC projeto quando:</span><span class="sxs-lookup"><span data-stu-id="5be1a-115">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="5be1a-116">Seu aplicativo é composto de vários componentes funcionais alto nível que devem ser separados logicamente</span><span class="sxs-lookup"><span data-stu-id="5be1a-116">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="5be1a-117">Você deseja particionar seu projeto MVC para que cada área funcional pode ser trabalhada independentemente</span><span class="sxs-lookup"><span data-stu-id="5be1a-117">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="5be1a-118">Recursos de área:</span><span class="sxs-lookup"><span data-stu-id="5be1a-118">Area features:</span></span>

* <span data-ttu-id="5be1a-119">Um aplicativo MVC do ASP.NET Core pode ter qualquer número de áreas</span><span class="sxs-lookup"><span data-stu-id="5be1a-119">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="5be1a-120">Cada área tem seus próprios controladores, modelos e modos de exibição</span><span class="sxs-lookup"><span data-stu-id="5be1a-120">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="5be1a-121">Permite organizar projetos MVC grandes em vários componentes de alto nível que podem ser executados independentemente</span><span class="sxs-lookup"><span data-stu-id="5be1a-121">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="5be1a-122">Oferece suporte a vários controladores com o mesmo nome - desde que eles têm diferentes *áreas*</span><span class="sxs-lookup"><span data-stu-id="5be1a-122">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="5be1a-123">Vamos dar uma olhada em um exemplo para ilustrar como áreas são criadas e usadas.</span><span class="sxs-lookup"><span data-stu-id="5be1a-123">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="5be1a-124">Digamos que você tenha um aplicativo de armazenamento que tem dois grupos distintos de controladores e exibições: produtos e serviços.</span><span class="sxs-lookup"><span data-stu-id="5be1a-124">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="5be1a-125">Uma pasta comum estrutura que usando áreas do MVC aparência abaixo:</span><span class="sxs-lookup"><span data-stu-id="5be1a-125">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="5be1a-126">Nome do projeto</span><span class="sxs-lookup"><span data-stu-id="5be1a-126">Project name</span></span>

  * <span data-ttu-id="5be1a-127">Áreas</span><span class="sxs-lookup"><span data-stu-id="5be1a-127">Areas</span></span>

    * <span data-ttu-id="5be1a-128">Produtos</span><span class="sxs-lookup"><span data-stu-id="5be1a-128">Products</span></span>

      * <span data-ttu-id="5be1a-129">Controladores</span><span class="sxs-lookup"><span data-stu-id="5be1a-129">Controllers</span></span>

        * <span data-ttu-id="5be1a-130">HomeController</span><span class="sxs-lookup"><span data-stu-id="5be1a-130">HomeController.cs</span></span>

        * <span data-ttu-id="5be1a-131">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="5be1a-131">ManageController.cs</span></span>

      * <span data-ttu-id="5be1a-132">Exibições</span><span class="sxs-lookup"><span data-stu-id="5be1a-132">Views</span></span>

        * <span data-ttu-id="5be1a-133">Home</span><span class="sxs-lookup"><span data-stu-id="5be1a-133">Home</span></span>

          * <span data-ttu-id="5be1a-134">Cshtml</span><span class="sxs-lookup"><span data-stu-id="5be1a-134">Index.cshtml</span></span>

        * <span data-ttu-id="5be1a-135">Gerenciar</span><span class="sxs-lookup"><span data-stu-id="5be1a-135">Manage</span></span>

          * <span data-ttu-id="5be1a-136">Cshtml</span><span class="sxs-lookup"><span data-stu-id="5be1a-136">Index.cshtml</span></span>

    * <span data-ttu-id="5be1a-137">Serviços</span><span class="sxs-lookup"><span data-stu-id="5be1a-137">Services</span></span>

      * <span data-ttu-id="5be1a-138">Controladores</span><span class="sxs-lookup"><span data-stu-id="5be1a-138">Controllers</span></span>

        * <span data-ttu-id="5be1a-139">HomeController</span><span class="sxs-lookup"><span data-stu-id="5be1a-139">HomeController.cs</span></span>

      * <span data-ttu-id="5be1a-140">Exibições</span><span class="sxs-lookup"><span data-stu-id="5be1a-140">Views</span></span>

        * <span data-ttu-id="5be1a-141">Home</span><span class="sxs-lookup"><span data-stu-id="5be1a-141">Home</span></span>

          * <span data-ttu-id="5be1a-142">Cshtml</span><span class="sxs-lookup"><span data-stu-id="5be1a-142">Index.cshtml</span></span>

<span data-ttu-id="5be1a-143">Quando tenta MVC renderizar uma exibição em uma área, por padrão, ele tenta pesquisar nos seguintes locais:</span><span class="sxs-lookup"><span data-stu-id="5be1a-143">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="5be1a-144">Estes são os locais padrão que podem ser alterados por meio de `AreaViewLocationFormats` no `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="5be1a-144">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="5be1a-145">Por exemplo, o código em vez de ter o nome da pasta como 'Áreas', abaixo foi alterado para 'Categorias'.</span><span class="sxs-lookup"><span data-stu-id="5be1a-145">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="5be1a-146">Observe que é a estrutura do *modos de exibição* pasta é a única que é considerada importante aqui e, como o conteúdo do restante das pastas *controladores* e *modelos* does **não** importa.</span><span class="sxs-lookup"><span data-stu-id="5be1a-146">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="5be1a-147">Por exemplo, você não precisa ter um *controladores* e *modelos* pasta todos.</span><span class="sxs-lookup"><span data-stu-id="5be1a-147">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="5be1a-148">Isso funciona porque o conteúdo de *controladores* e *modelos* é apenas o código que é compilado em um. dll, enquanto que o conteúdo do *exibições* não será até que uma solicitação para que modo de exibição foi feito.</span><span class="sxs-lookup"><span data-stu-id="5be1a-148">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* is not until a request to that view has been made.</span></span>

<span data-ttu-id="5be1a-149">Depois que você definiu a hierarquia de pastas, você precisa informar ao MVC que cada controlador está associado uma área.</span><span class="sxs-lookup"><span data-stu-id="5be1a-149">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="5be1a-150">Você pode fazer isso, decorando o nome do controlador com o `[Area]` atributo.</span><span class="sxs-lookup"><span data-stu-id="5be1a-150">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

<span data-ttu-id="5be1a-151">Configure uma definição de rota que funciona com as áreas recém-criado.</span><span class="sxs-lookup"><span data-stu-id="5be1a-151">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="5be1a-152">O [roteamento para ações do controlador](routing.md) artigo apresenta detalhes sobre como criar definições de rota, incluindo o uso convencionais rotas versus rotas de atributo.</span><span class="sxs-lookup"><span data-stu-id="5be1a-152">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="5be1a-153">Neste exemplo, vamos usar uma rota convencional.</span><span class="sxs-lookup"><span data-stu-id="5be1a-153">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="5be1a-154">Para fazer isso, abra o *Startup.cs* de arquivo e modificá-lo adicionando o `areaRoute` chamado de definição da rota abaixo.</span><span class="sxs-lookup"><span data-stu-id="5be1a-154">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

<span data-ttu-id="5be1a-155">Navegando para `http://<yourApp>/products`, o `Index` método de ação a `HomeController` no `Products` área será invocada.</span><span class="sxs-lookup"><span data-stu-id="5be1a-155">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="5be1a-156">Geração de link</span><span class="sxs-lookup"><span data-stu-id="5be1a-156">Link Generation</span></span>

* <span data-ttu-id="5be1a-157">Geração de links de uma ação dentro de uma área com base em controlador de outra ação dentro do mesmo controlador.</span><span class="sxs-lookup"><span data-stu-id="5be1a-157">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="5be1a-158">Digamos que o caminho da solicitação atual é como`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="5be1a-158">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="5be1a-159">Sintaxe de HtmlHelper:`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="5be1a-159">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="5be1a-160">Sintaxe de TagHelper:`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="5be1a-160">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="5be1a-161">Observe que não é necessário fornecer os valores de 'área' e 'controller' aqui porque eles já estão disponíveis no contexto da solicitação atual.</span><span class="sxs-lookup"><span data-stu-id="5be1a-161">Note that we need not supply the 'area' and 'controller' values here as they are already available in the context of the current request.</span></span> <span data-ttu-id="5be1a-162">Esses tipos de valores são chamados `ambient` valores.</span><span class="sxs-lookup"><span data-stu-id="5be1a-162">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="5be1a-163">Geração de links de uma ação dentro de uma área com base no controlador de outra ação em um controlador diferente</span><span class="sxs-lookup"><span data-stu-id="5be1a-163">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="5be1a-164">Digamos que o caminho da solicitação atual é como`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="5be1a-164">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="5be1a-165">Sintaxe de HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="5be1a-165">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="5be1a-166">Sintaxe de TagHelper:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="5be1a-166">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="5be1a-167">Observe que o valor de ambiente de uma área é usado aqui, mas o valor de 'controller' é especificado explicitamente acima.</span><span class="sxs-lookup"><span data-stu-id="5be1a-167">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="5be1a-168">Geração de links de uma ação dentro de uma área, controlador para outra ação com base em um controlador diferente e uma área diferente.</span><span class="sxs-lookup"><span data-stu-id="5be1a-168">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="5be1a-169">Digamos que o caminho da solicitação atual é como`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="5be1a-169">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="5be1a-170">Sintaxe de HtmlHelper:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="5be1a-170">HtmlHelper syntax: `@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="5be1a-171">Sintaxe de TagHelper:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="5be1a-171">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span></span>

  <span data-ttu-id="5be1a-172">Observe que aqui sem valores de ambiente são usados.</span><span class="sxs-lookup"><span data-stu-id="5be1a-172">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="5be1a-173">Geração de links de uma ação dentro de um controlador de área com base em outra ação em um controlador diferente e **não** em uma área.</span><span class="sxs-lookup"><span data-stu-id="5be1a-173">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="5be1a-174">Sintaxe de HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="5be1a-174">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="5be1a-175">Sintaxe de TagHelper:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="5be1a-175">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="5be1a-176">Desde que você deseja gerar links para uma área não-com base em ação do controlador, vazio é o valor de ambiente para 'área' aqui.</span><span class="sxs-lookup"><span data-stu-id="5be1a-176">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="5be1a-177">Áreas de publicação</span><span class="sxs-lookup"><span data-stu-id="5be1a-177">Publishing Areas</span></span>

<span data-ttu-id="5be1a-178">Todos os `*.cshtml` e `wwwroot/**` para dar saída quando os arquivos são publicados `<Project Sdk="Microsoft.NET.Sdk.Web">` está incluído no *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5be1a-178">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
