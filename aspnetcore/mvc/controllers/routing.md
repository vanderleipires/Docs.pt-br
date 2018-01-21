---
title: "O roteamento para ações do controlador"
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: 7559fa270a012082d04161c1cccd1dc8151d0c1c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="routing-to-controller-actions"></a><span data-ttu-id="7b897-102">O roteamento para ações do controlador</span><span class="sxs-lookup"><span data-stu-id="7b897-102">Routing to Controller Actions</span></span>

<span data-ttu-id="7b897-103">Por [Ryan Nowak](https://github.com/rynowak) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7b897-103">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7b897-104">Núcleo do ASP.NET MVC usa o roteamento [middleware](../../fundamentals/middleware.md) para corresponder as URLs de solicitações de entrada e mapeá-los para ações.</span><span class="sxs-lookup"><span data-stu-id="7b897-104">ASP.NET Core MVC uses the Routing [middleware](../../fundamentals/middleware.md) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="7b897-105">Rotas são definidas no código de inicialização ou atributos.</span><span class="sxs-lookup"><span data-stu-id="7b897-105">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="7b897-106">Rotas descrevem como os caminhos de URL devem corresponder às ações.</span><span class="sxs-lookup"><span data-stu-id="7b897-106">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="7b897-107">Rotas também são usadas para gerar URLs (para links) enviados em respostas.</span><span class="sxs-lookup"><span data-stu-id="7b897-107">Routes are also used to generate URLs (for links) sent out in responses.</span></span> 

<span data-ttu-id="7b897-108">Ações ou são roteadas convencionalmente ou atributo roteadas.</span><span class="sxs-lookup"><span data-stu-id="7b897-108">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="7b897-109">Colocar uma rota no controlador ou ação torna atributo roteado.</span><span class="sxs-lookup"><span data-stu-id="7b897-109">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="7b897-110">Consulte [misto roteamento](#routing-mixed-ref-label) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="7b897-110">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="7b897-111">Este documento explicará as interações entre MVC e roteamento e disponibilizar os aplicativos MVC típico como usar os recursos de roteamentos.</span><span class="sxs-lookup"><span data-stu-id="7b897-111">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="7b897-112">Consulte [roteamento](xref:fundamentals/routing) para obter detalhes sobre roteamento avançado.</span><span class="sxs-lookup"><span data-stu-id="7b897-112">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="7b897-113">Configurar o roteamento de Middleware</span><span class="sxs-lookup"><span data-stu-id="7b897-113">Setting up Routing Middleware</span></span>

<span data-ttu-id="7b897-114">No seu *configurar* método, você pode ver código semelhante a:</span><span class="sxs-lookup"><span data-stu-id="7b897-114">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="7b897-115">Dentro da chamada para `UseMvc`, `MapRoute` é usado para criar uma única rota, vamos analisar como o `default` rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-115">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="7b897-116">A maioria dos aplicativos MVC usará uma rota com um modelo semelhante para o `default` rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-116">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="7b897-117">O modelo de rota `"{controller=Home}/{action=Index}/{id?}"` pode corresponder a um caminho de URL como `/Products/Details/5` e extrairá os valores de rota `{ controller = Products, action = Details, id = 5 }` por gerar tokens para o caminho.</span><span class="sxs-lookup"><span data-stu-id="7b897-117">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="7b897-118">MVC tentará localizar um controlador nomeado `ProductsController` e execute a ação de `Details`:</span><span class="sxs-lookup"><span data-stu-id="7b897-118">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="7b897-119">Observe que, neste exemplo, associação de modelo usariam o valor de `id = 5` para definir o `id` parâmetro `5` ao invocar essa ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-119">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="7b897-120">Consulte o [modelo associação](../models/model-binding.md) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="7b897-120">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="7b897-121">Usando o `default` rota:</span><span class="sxs-lookup"><span data-stu-id="7b897-121">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="7b897-122">O modelo de rota:</span><span class="sxs-lookup"><span data-stu-id="7b897-122">The route template:</span></span>

* <span data-ttu-id="7b897-123">`{controller=Home}`define `Home` como padrão`controller`</span><span class="sxs-lookup"><span data-stu-id="7b897-123">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="7b897-124">`{action=Index}`define `Index` como padrão`action`</span><span class="sxs-lookup"><span data-stu-id="7b897-124">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="7b897-125">`{id?}`define `id` como opcionais</span><span class="sxs-lookup"><span data-stu-id="7b897-125">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="7b897-126">Padrão e parâmetros de rota opcional não precisa estar presente no caminho da URL para uma correspondência.</span><span class="sxs-lookup"><span data-stu-id="7b897-126">Default and optional route parameters do not need to be present in the URL path for a match.</span></span> <span data-ttu-id="7b897-127">Consulte [referência de modelo de rota](../../fundamentals/routing.md#route-template-reference) para obter uma descrição detalhada da sintaxe de modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-127">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="7b897-128">`"{controller=Home}/{action=Index}/{id?}"`pode corresponder ao caminho de URL `/` e produzirá os valores de rota `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="7b897-128">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="7b897-129">Os valores para `controller` e `action` fazer uso de valores padrão, `id` não produz um valor porque não há nenhum segmento correspondente no caminho da URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-129">The values for `controller` and `action` make use of the default values, `id` does not produce a value since there is no corresponding segment in the URL path.</span></span> <span data-ttu-id="7b897-130">MVC usa esses valores de rota para selecionar o `HomeController` e `Index` ação:</span><span class="sxs-lookup"><span data-stu-id="7b897-130">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="7b897-131">Usando essa definição de controlador e o modelo de rota, o `HomeController.Index` a ação deve ser executada para qualquer um dos seguintes caminhos de URL:</span><span class="sxs-lookup"><span data-stu-id="7b897-131">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="7b897-132">O método de conveniência `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="7b897-132">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="7b897-133">Pode ser usado para substituir:</span><span class="sxs-lookup"><span data-stu-id="7b897-133">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="7b897-134">`UseMvc`e `UseMvcWithDefaultRoute` adicionar uma instância de `RouterMiddleware` para o pipeline de middleware.</span><span class="sxs-lookup"><span data-stu-id="7b897-134">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="7b897-135">MVC não interage diretamente com middleware e usa o roteamento para tratar as solicitações.</span><span class="sxs-lookup"><span data-stu-id="7b897-135">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="7b897-136">MVC está conectado às rotas por meio de uma instância de `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="7b897-136">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="7b897-137">O código dentro de `UseMvc` é semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="7b897-137">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="7b897-138">`UseMvc`não define diretamente todas as rotas, ele adiciona um espaço reservado para a coleção de rotas para o `attribute` rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-138">`UseMvc` does not directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="7b897-139">A sobrecarga `UseMvc(Action<IRouteBuilder>)` permite que você adicione suas próprias rotas e também dá suporte a roteamento de atributo.</span><span class="sxs-lookup"><span data-stu-id="7b897-139">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="7b897-140">`UseMvc`e todas as variações adiciona um espaço reservado para a rota de atributo - roteamento de atributo está sempre disponível, independentemente de como você configura `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="7b897-140">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="7b897-141">`UseMvcWithDefaultRoute`define uma rota padrão e dá suporte a roteamento de atributo.</span><span class="sxs-lookup"><span data-stu-id="7b897-141">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="7b897-142">O [roteamento de atributo](#attribute-routing-ref-label) seção inclui mais detalhes sobre o roteamento de atributo.</span><span class="sxs-lookup"><span data-stu-id="7b897-142">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="7b897-143">Roteamento convencional</span><span class="sxs-lookup"><span data-stu-id="7b897-143">Conventional routing</span></span>

<span data-ttu-id="7b897-144">O `default` rota:</span><span class="sxs-lookup"><span data-stu-id="7b897-144">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="7b897-145">é um exemplo de um *roteamento convencional*.</span><span class="sxs-lookup"><span data-stu-id="7b897-145">is an example of a *conventional routing*.</span></span> <span data-ttu-id="7b897-146">Chamamos esse estilo *roteamento convencional* porque ele estabelece um *convenção* para caminhos de URL:</span><span class="sxs-lookup"><span data-stu-id="7b897-146">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="7b897-147">o primeiro segmento de caminho é mapeado para o nome do controlador</span><span class="sxs-lookup"><span data-stu-id="7b897-147">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="7b897-148">o segundo é mapeado para o nome da ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-148">the second maps to the action name.</span></span>

* <span data-ttu-id="7b897-149">o terceiro segmento é usado para um recurso opcional `id` usado para mapear para uma entidade de modelo</span><span class="sxs-lookup"><span data-stu-id="7b897-149">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="7b897-150">Usando esse `default` rota, o caminho da URL `/Products/List` mapeia para o `ProductsController.List` ação, e `/Blog/Article/17` mapeia para `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="7b897-150">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="7b897-151">Esse mapeamento é baseado nos nomes de controlador e ação **somente** e não se baseia em namespaces, locais de arquivo de origem ou parâmetros de método.</span><span class="sxs-lookup"><span data-stu-id="7b897-151">This mapping is based on the controller and action names **only** and is not based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="7b897-152">Usar o roteamento convencional com a rota padrão permite que você compilar o aplicativo rapidamente sem a necessidade de criar um novo padrão de URL para cada ação que você definir.</span><span class="sxs-lookup"><span data-stu-id="7b897-152">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="7b897-153">Para um aplicativo com ações de estilo CRUD, com consistência para as URLs em seus controladores pode ajudar a simplificar seu código e fazer sua interface do usuário mais previsível.</span><span class="sxs-lookup"><span data-stu-id="7b897-153">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="7b897-154">O `id` é definido como opcional pelo modelo de rota, o que significa que suas ações podem executar sem a ID fornecida como parte da URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-154">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="7b897-155">Geralmente o que acontecerá se `id` for omitido da URL é que ele será definido como `0` pela associação de modelo e, assim nenhuma entidade será encontrada na correspondência de banco de dados `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="7b897-155">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="7b897-156">Roteamento de atributo pode fornecer controle refinado para tornar a ID necessária para algumas ações e não para outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="7b897-156">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="7b897-157">Por convenção, a documentação incluirá parâmetros opcionais, como `id` quando eles são provavelmente aparecerão no uso correto.</span><span class="sxs-lookup"><span data-stu-id="7b897-157">By convention the documentation will include optional parameters like `id` when they are likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="7b897-158">Várias rotas</span><span class="sxs-lookup"><span data-stu-id="7b897-158">Multiple routes</span></span>

<span data-ttu-id="7b897-159">Você pode adicionar várias rotas dentro `UseMvc` adicionando mais chamadas para `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="7b897-159">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="7b897-160">Isso permite que você definir várias convenções ou adicionar rotas convencionais que são dedicadas a uma ação específica, como:</span><span class="sxs-lookup"><span data-stu-id="7b897-160">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="7b897-161">O `blog` rota aqui é um *dedicada rota convencional*, que significa que ele usa o sistema de roteamento convencional, mas é dedicado a uma ação específica.</span><span class="sxs-lookup"><span data-stu-id="7b897-161">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="7b897-162">Como `controller` e `action` não aparecem no modelo de rota como parâmetros, eles só podem ter os valores padrão e, portanto, essa rota sempre será mapeado para a ação `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="7b897-162">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="7b897-163">Rotas na coleção de rotas são ordenadas e serão processadas na ordem em que eles são adicionados.</span><span class="sxs-lookup"><span data-stu-id="7b897-163">Routes in the route collection are ordered, and will be processed in the order they are added.</span></span> <span data-ttu-id="7b897-164">Portanto neste exemplo, o `blog` rota será tentada antes do `default` rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-164">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="7b897-165">*Dedicado rotas convencionais* geralmente usam parâmetros de rota pega-tudo como `{*article}` para capturar a parte restante do caminho da URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-165">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="7b897-166">Isso pode fazer com que uma rota "muito greedy" significando corresponde URLs que você se destina a ser correspondido por outras rotas.</span><span class="sxs-lookup"><span data-stu-id="7b897-166">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="7b897-167">Coloque as rotas 'greedy' posteriormente na tabela de rotas para solucionar esse problema.</span><span class="sxs-lookup"><span data-stu-id="7b897-167">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="7b897-168">Fallback</span><span class="sxs-lookup"><span data-stu-id="7b897-168">Fallback</span></span>

<span data-ttu-id="7b897-169">Como parte do processamento da solicitação, o MVC verificará se os valores de rota podem ser usados para localizar um controlador e ação em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7b897-169">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="7b897-170">Se os valores de rota não corresponderem a uma ação, em seguida, a rota não é considerada uma correspondência e a próxima rota será tentada.</span><span class="sxs-lookup"><span data-stu-id="7b897-170">If the route values don't match an action then the route is not considered a match, and the next route will be tried.</span></span> <span data-ttu-id="7b897-171">Isso é chamado de *fallback*, e ele foi destinado para simplificar a casos em que se sobrepõem rotas convencionais.</span><span class="sxs-lookup"><span data-stu-id="7b897-171">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="7b897-172">Ações desambiguação</span><span class="sxs-lookup"><span data-stu-id="7b897-172">Disambiguating actions</span></span>

<span data-ttu-id="7b897-173">Quando duas ações correspondem por meio do roteamento, deve resolver a ambiguidade de MVC para escolher o candidato 'as' ou lançar uma exceção.</span><span class="sxs-lookup"><span data-stu-id="7b897-173">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="7b897-174">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7b897-174">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="7b897-175">Esse controlador define duas ações que deve corresponder ao caminho de URL `/Products/Edit/17` e encaminhar dados `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="7b897-175">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="7b897-176">Este é um padrão comum para controladores MVC onde `Edit(int)` mostra um formulário para editar um produto e `Edit(int, Product)` processa o formulário postado.</span><span class="sxs-lookup"><span data-stu-id="7b897-176">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="7b897-177">Para que isso aconteça MVC precisa escolher `Edit(int, Product)` quando a solicitação é um HTTP `POST` e `Edit(int)` quando o verbo HTTP for outro número.</span><span class="sxs-lookup"><span data-stu-id="7b897-177">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="7b897-178">O `HttpPostAttribute` ( `[HttpPost]` ) é uma implementação de `IActionConstraint` que só permitirá que a ação a ser selecionado quando o verbo HTTP é `POST`.</span><span class="sxs-lookup"><span data-stu-id="7b897-178">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="7b897-179">A presença de um `IActionConstraint` faz o `Edit(int, Product)` corresponde a um melhor que `Edit(int)`, portanto `Edit(int, Product)` será tentada primeiro.</span><span class="sxs-lookup"><span data-stu-id="7b897-179">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="7b897-180">Você só precisará escrever personalizado `IActionConstraint` implementações em cenários especializados, mas do importante compreender a função de atributos, como `HttpPostAttribute` -atributos semelhantes são definidas para outros verbos HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b897-180">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="7b897-181">Em roteamento convencional é comum para ações usar o mesmo nome de ação quando eles fazem parte de um `show form -> submit form` fluxo de trabalho.</span><span class="sxs-lookup"><span data-stu-id="7b897-181">In conventional routing it's common for actions to use the same action name when they are part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="7b897-182">A conveniência deste padrão se tornará mais aparente depois de revisar o [Noções básicas sobre IActionConstraint](#understanding-iactionconstraint) seção.</span><span class="sxs-lookup"><span data-stu-id="7b897-182">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="7b897-183">Se correspondem a várias rotas e MVC não é possível encontrar uma rota 'as', ela irá gerar um `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="7b897-183">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="7b897-184">Nomes de rota</span><span class="sxs-lookup"><span data-stu-id="7b897-184">Route names</span></span>

<span data-ttu-id="7b897-185">As cadeias de caracteres `"blog"` e `"default"` nos exemplos a seguir são os nomes de rota:</span><span class="sxs-lookup"><span data-stu-id="7b897-185">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="7b897-186">Os nomes de rota dar um nome lógico a rota para que a rota nomeada pode ser usada para a geração de URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-186">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="7b897-187">Isso simplifica a criação de URL quando a ordenação de rotas pode tornar complicada de geração de URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-187">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="7b897-188">Os nomes de rota devem ser exclusivo do nível de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7b897-188">Route names must be unique application-wide.</span></span>

<span data-ttu-id="7b897-189">Os nomes de rota não têm impacto em URL correspondente ou tratamento de solicitações; eles são usados apenas para geração de URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-189">Route names have no impact on URL matching or handling of requests; they are used only for URL generation.</span></span> <span data-ttu-id="7b897-190">[Roteamento](xref:fundamentals/routing) tem informações mais detalhadas sobre geração de URL, incluindo geração de URL em auxiliares MVC específicos.</span><span class="sxs-lookup"><span data-stu-id="7b897-190">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="7b897-191">Roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="7b897-191">Attribute routing</span></span>

<span data-ttu-id="7b897-192">Roteamento de atributo usa um conjunto de atributos para mapear ações diretamente para modelos de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-192">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="7b897-193">No exemplo a seguir, `app.UseMvc();` é usado no `Configure` método e nenhuma rota é passado.</span><span class="sxs-lookup"><span data-stu-id="7b897-193">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="7b897-194">O `HomeController` corresponderá a um conjunto de URLs semelhantes à qual a rota padrão `{controller=Home}/{action=Index}/{id?}` corresponderia:</span><span class="sxs-lookup"><span data-stu-id="7b897-194">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

<span data-ttu-id="7b897-195">O `HomeController.Index()` ação será executada para todos os caminhos de URL `/`, `/Home`, ou `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="7b897-195">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="7b897-196">Este exemplo realça uma diferença importante programação entre o atributo de roteamento e roteamento convencional.</span><span class="sxs-lookup"><span data-stu-id="7b897-196">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="7b897-197">Roteamento de atributo requer mais informações para especificar uma rota; a rota padrão convencional manipula rotas de forma mais sucinta.</span><span class="sxs-lookup"><span data-stu-id="7b897-197">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="7b897-198">No entanto, o roteamento de atributo permite (e exige) controle preciso dos quais modelos de rota se aplicam a cada ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-198">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="7b897-199">Com o nome do controlador e os nomes de ação de roteamento de atributo reproduzir **sem** função na qual a ação é selecionada.</span><span class="sxs-lookup"><span data-stu-id="7b897-199">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="7b897-200">Este exemplo corresponderá as mesmas URLs de exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="7b897-200">This example will match the same URLs as the previous example.</span></span>

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> <span data-ttu-id="7b897-201">Os modelos de rota acima não definem os parâmetros de rota para `action`, `area`, e `controller`.</span><span class="sxs-lookup"><span data-stu-id="7b897-201">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="7b897-202">Na verdade, esses parâmetros de rota não são permitidos em rotas de atributo.</span><span class="sxs-lookup"><span data-stu-id="7b897-202">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="7b897-203">Desde que o modelo de rota já está associado uma ação, não faz sentido para analisar o nome da ação da URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-203">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="7b897-204">Atributo de roteamento com atributos de Http [verbo]</span><span class="sxs-lookup"><span data-stu-id="7b897-204">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="7b897-205">Roteamento de atributo também pode fazer uso do `Http[Verb]` atributos como `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7b897-205">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="7b897-206">Todos esses atributos podem aceitar um modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-206">All of these attributes can accept a route template.</span></span> <span data-ttu-id="7b897-207">Este exemplo mostra duas ações que correspondam ao mesmo modelo de rota:</span><span class="sxs-lookup"><span data-stu-id="7b897-207">This example shows two actions that match the same route template:</span></span>

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

<span data-ttu-id="7b897-208">Para um caminho de URL como `/products` o `ProductsApi.ListProducts` ação será executada quando o verbo HTTP é `GET` e `ProductsApi.CreateProduct` será executado quando o verbo HTTP é `POST`.</span><span class="sxs-lookup"><span data-stu-id="7b897-208">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="7b897-209">Primeiro, o roteamento de atributo corresponde à URL com o conjunto de modelos de rota definidas por atributos de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-209">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="7b897-210">Depois que um modelo de rota corresponde, `IActionConstraint` restrições são aplicadas para determinar quais ações podem ser executadas.</span><span class="sxs-lookup"><span data-stu-id="7b897-210">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="7b897-211">Ao criar uma API REST, é raro que você deseja usar `[Route(...)]` em um método de ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-211">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="7b897-212">É melhor usar o mais específico `Http*Verb*Attributes` para ser preciso sobre o que suporta a sua API.</span><span class="sxs-lookup"><span data-stu-id="7b897-212">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="7b897-213">Esperam-se que os clientes de APIs REST saber o que mapeiam caminhos e verbos HTTP para determinadas operações lógicas.</span><span class="sxs-lookup"><span data-stu-id="7b897-213">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="7b897-214">Como uma rota de atributos se aplica a uma ação específica, é fácil fazer parâmetros necessários como parte da definição de modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-214">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="7b897-215">Neste exemplo, `id` é exigido como parte do caminho da URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-215">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="7b897-216">O `ProductsApi.GetProduct(int)` ação será executada para um caminho de URL como `/products/3` , mas não para um caminho de URL como `/products`.</span><span class="sxs-lookup"><span data-stu-id="7b897-216">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="7b897-217">Consulte [roteamento](../../fundamentals/routing.md) para obter uma descrição completa de modelos de rota e as opções relacionadas.</span><span class="sxs-lookup"><span data-stu-id="7b897-217">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="7b897-218">Nome da rota</span><span class="sxs-lookup"><span data-stu-id="7b897-218">Route Name</span></span>

<span data-ttu-id="7b897-219">O código a seguir define uma *nome da rota* de `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="7b897-219">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="7b897-220">Nomes de rota podem ser usados para gerar uma URL com base em uma rota específica.</span><span class="sxs-lookup"><span data-stu-id="7b897-220">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="7b897-221">Nomes de rota não têm impacto sobre a correspondência de comportamento de roteamento de URL e só são usados para geração de URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-221">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="7b897-222">Os nomes de rota devem ser exclusivo do nível de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7b897-222">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="7b897-223">Compare isso com o convencional *rota padrão*, que define o `id` parâmetro como opcional (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="7b897-223">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="7b897-224">Essa capacidade de especificar precisamente APIs tem vantagens, como permitindo `/products` e `/products/5` deve ser distribuída para ações diferentes.</span><span class="sxs-lookup"><span data-stu-id="7b897-224">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="7b897-225">Rotas de combinação</span><span class="sxs-lookup"><span data-stu-id="7b897-225">Combining routes</span></span>

<span data-ttu-id="7b897-226">Para tornar o roteamento de atributo menos repetitivas, atributos de rota do controlador são combinados com atributos de rota as ações individuais.</span><span class="sxs-lookup"><span data-stu-id="7b897-226">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="7b897-227">Os modelos de rota definidos no controlador são pré-anexados modelos nas ações de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-227">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="7b897-228">Colocar um atributo da rota no controlador torna **todos os** ações no controlador de usam o roteamento de atributo.</span><span class="sxs-lookup"><span data-stu-id="7b897-228">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="7b897-229">Neste exemplo, o caminho da URL `/products` pode corresponder a `ProductsApi.ListProducts`e o caminho da URL `/products/5` pode corresponder a `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="7b897-229">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="7b897-230">Ambas as ações correspondem apenas HTTP `GET` porque eles são decorados com o `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7b897-230">Both of these actions only match HTTP `GET` because they are decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="7b897-231">Rotear modelos aplicados a uma ação que começam com um `/` não combinada com modelos de rota aplicados ao controlador.</span><span class="sxs-lookup"><span data-stu-id="7b897-231">Route templates applied to an action that begin with a `/` do not get combined with route templates applied to the controller.</span></span> <span data-ttu-id="7b897-232">Este exemplo corresponde a um conjunto de caminhos de URL semelhante de *rota padrão*.</span><span class="sxs-lookup"><span data-stu-id="7b897-232">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Does not combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a><span data-ttu-id="7b897-233">Rotas de atributo de ordenação</span><span class="sxs-lookup"><span data-stu-id="7b897-233">Ordering attribute routes</span></span>

<span data-ttu-id="7b897-234">Em contraste com rotas convencionais que execute em uma ordem definida, o roteamento de atributo criará uma árvore e corresponde a todas as rotas simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="7b897-234">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="7b897-235">Isso se comporta como-se as entradas de rota foram colocadas em uma ordem ideal; as rotas mais específicas tenham a oportunidade de executar antes das rotas mais gerais.</span><span class="sxs-lookup"><span data-stu-id="7b897-235">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="7b897-236">Por exemplo, uma rota como `blog/search/{topic}` é mais específico que uma rota como `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="7b897-236">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="7b897-237">Falando logicamente o `blog/search/{topic}` rota 'executa' primeiro, por padrão, porque essa é a sensato somente ordenação.</span><span class="sxs-lookup"><span data-stu-id="7b897-237">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="7b897-238">Usando o roteamento convencional, o desenvolvedor é responsável pela colocação de rotas na ordem desejada.</span><span class="sxs-lookup"><span data-stu-id="7b897-238">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="7b897-239">Rotas de atributo podem configurar uma ordem, usando o `Order` propriedade de todos os atributos de rota do framework fornecido.</span><span class="sxs-lookup"><span data-stu-id="7b897-239">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="7b897-240">Rotas são processadas de acordo com a crescente de classificação de `Order` propriedade.</span><span class="sxs-lookup"><span data-stu-id="7b897-240">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="7b897-241">A ordem padrão é `0`.</span><span class="sxs-lookup"><span data-stu-id="7b897-241">The default order is `0`.</span></span> <span data-ttu-id="7b897-242">Definindo uma rota usando `Order = -1` será executado antes de rotas que não definem um pedido.</span><span class="sxs-lookup"><span data-stu-id="7b897-242">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="7b897-243">Definindo uma rota usando `Order = 1` será executado após a ordenação de rota padrão.</span><span class="sxs-lookup"><span data-stu-id="7b897-243">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="7b897-244">Evite dependendo `Order`.</span><span class="sxs-lookup"><span data-stu-id="7b897-244">Avoid depending on `Order`.</span></span> <span data-ttu-id="7b897-245">Se o seu espaço de URL requer valores de ordem explícita para rotear corretamente, é provavelmente confuso para os clientes.</span><span class="sxs-lookup"><span data-stu-id="7b897-245">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="7b897-246">Em geral a roteamento de atributo selecionará a rota correta com URL correspondente.</span><span class="sxs-lookup"><span data-stu-id="7b897-246">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="7b897-247">Se a ordem padrão usada para a geração de URL não está funcionando, usando o nome da rota como uma substituição é geralmente mais simples do que a aplicação de `Order` propriedade.</span><span class="sxs-lookup"><span data-stu-id="7b897-247">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="7b897-248">Token de substituição em modelos de rota ([controller] [ação] [área])</span><span class="sxs-lookup"><span data-stu-id="7b897-248">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="7b897-249">Para sua conveniência, rotas de atributo suportam *substituição de token* colocando um token quadrado chaves (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="7b897-249">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="7b897-250">Os tokens `[action]`, `[area]`, e `[controller]` substituirá os valores do nome do controlador da ação em que a rota é definida, o nome da ação e o nome da área.</span><span class="sxs-lookup"><span data-stu-id="7b897-250">The tokens `[action]`, `[area]`, and `[controller]` will be replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="7b897-251">Neste exemplo as ações podem corresponder a caminhos de URL conforme descrito nos comentários:</span><span class="sxs-lookup"><span data-stu-id="7b897-251">In this example the actions can match URL paths as described in the comments:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="7b897-252">Substituição do token ocorre como a última etapa de compilação as rotas de atributo.</span><span class="sxs-lookup"><span data-stu-id="7b897-252">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="7b897-253">O exemplo acima irão se comportar o mesmo que o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="7b897-253">The above example will behave the same as the following code:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="7b897-254">Rotas de atributo também podem ser combinadas com herança.</span><span class="sxs-lookup"><span data-stu-id="7b897-254">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="7b897-255">Isso é especialmente eficiente, combinado com a substituição do token.</span><span class="sxs-lookup"><span data-stu-id="7b897-255">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="7b897-256">Substituição do token também se aplica a nomes de rotas definidos por rotas de atributo.</span><span class="sxs-lookup"><span data-stu-id="7b897-256">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="7b897-257">`[Route("[controller]/[action]", Name="[controller]_[action]")]`irá gerar um nome exclusivo de rota para cada ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-257">`[Route("[controller]/[action]", Name="[controller]_[action]")]` will generate a unique route name for each action.</span></span>

<span data-ttu-id="7b897-258">Para corresponder ao delimitador de literal de substituição de token `[` ou `]`, escape-o pelo caractere de repetição (`[[` ou `]]`).</span><span class="sxs-lookup"><span data-stu-id="7b897-258">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="7b897-259">Várias rotas</span><span class="sxs-lookup"><span data-stu-id="7b897-259">Multiple Routes</span></span>

<span data-ttu-id="7b897-260">Dá suporte a roteamento definindo várias rotas que atingem a mesma ação de atributo.</span><span class="sxs-lookup"><span data-stu-id="7b897-260">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="7b897-261">O uso mais comum é para simular o comportamento do *rota convencional* conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="7b897-261">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="7b897-262">Colocar vários atributos de rota no controlador significa que cada um deles serão combinadas com cada um dos atributos de rota sobre os métodos de ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-262">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

<span data-ttu-id="7b897-263">Quando vários atributos de rota (que implementam `IActionConstraint`) são colocados em uma ação, em seguida, cada restrição ação combina com o modelo de rota do atributo que definiu.</span><span class="sxs-lookup"><span data-stu-id="7b897-263">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> <span data-ttu-id="7b897-264">Embora usar várias rotas nas ações pode parecer avançado, é melhor manter o espaço de URL do aplicativo simples e bem definidos.</span><span class="sxs-lookup"><span data-stu-id="7b897-264">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="7b897-265">Use várias rotas nas ações somente quando necessário, por exemplo, para dar suporte a clientes existentes.</span><span class="sxs-lookup"><span data-stu-id="7b897-265">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="7b897-266">Especificando parâmetros opcionais de rota de atributo, valores padrão e restrições</span><span class="sxs-lookup"><span data-stu-id="7b897-266">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="7b897-267">Rotas de atributo oferecem suporte a mesma sintaxe embutida convencionais rotas para especificar parâmetros opcionais, valores padrão e restrições.</span><span class="sxs-lookup"><span data-stu-id="7b897-267">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="7b897-268">Consulte [referência de modelo de rota](../../fundamentals/routing.md#route-template-reference) para obter uma descrição detalhada da sintaxe de modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-268">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="7b897-269">Atributos de rota personalizados usando`IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="7b897-269">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="7b897-270">Todos os atributos de rota fornecidos no framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implementa a `IRouteTemplateProvider` interface.</span><span class="sxs-lookup"><span data-stu-id="7b897-270">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="7b897-271">MVC procurará atributos em classes de controlador e os métodos de ação quando o aplicativo é iniciado e usa os que implementam `IRouteTemplateProvider` para criar o conjunto inicial de rotas.</span><span class="sxs-lookup"><span data-stu-id="7b897-271">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="7b897-272">Você pode implementar `IRouteTemplateProvider` para definir seus próprios atributos de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-272">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="7b897-273">Cada `IRouteTemplateProvider` permite que você defina uma única rota com um modelo de rota personalizados, solicitar e nome:</span><span class="sxs-lookup"><span data-stu-id="7b897-273">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="7b897-274">O atributo do exemplo acima configura automaticamente o `Template` para `"api/[controller]"` quando `[MyApiController]` é aplicada.</span><span class="sxs-lookup"><span data-stu-id="7b897-274">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="7b897-275">Usando o modelo de aplicativo para personalizar as rotas de atributo</span><span class="sxs-lookup"><span data-stu-id="7b897-275">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="7b897-276">O *modelo de aplicativo* é um modelo de objeto criado durante a inicialização com todos os metadados usados pelo MVC para rotear e executar ações.</span><span class="sxs-lookup"><span data-stu-id="7b897-276">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="7b897-277">O *modelo de aplicativo* inclui todos os dados coletados a partir de atributos de rota (por meio de `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="7b897-277">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="7b897-278">Você pode escrever *convenções* para modificar o modelo de aplicativo no momento da inicialização personalize o comportamento do roteamento.</span><span class="sxs-lookup"><span data-stu-id="7b897-278">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="7b897-279">Esta seção mostra um exemplo simples de personalização de roteamento usando o modelo de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7b897-279">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="7b897-280">Misto roteamento: atributo roteamento roteamento convencional do vs</span><span class="sxs-lookup"><span data-stu-id="7b897-280">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="7b897-281">Aplicativos MVC podem combinar o uso de roteamento convencional e roteamento de atributo.</span><span class="sxs-lookup"><span data-stu-id="7b897-281">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="7b897-282">É comum usar as rotas convencionais para controladores servindo páginas HTML para navegadores e roteamento para controladores que serve de APIs REST de atributo.</span><span class="sxs-lookup"><span data-stu-id="7b897-282">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="7b897-283">Ações ou são roteadas convencionalmente ou atributo roteadas.</span><span class="sxs-lookup"><span data-stu-id="7b897-283">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="7b897-284">Colocar uma rota no controlador ou ação torna atributo roteado.</span><span class="sxs-lookup"><span data-stu-id="7b897-284">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="7b897-285">Ações que definem rotas de atributo não podem ser acessadas por meio de rotas convencionais e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="7b897-285">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="7b897-286">**Qualquer** atributo da rota no controlador faz com que todas as ações no atributo controlador roteado.</span><span class="sxs-lookup"><span data-stu-id="7b897-286">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="7b897-287">O que distingue os dois tipos de sistemas de roteamentos é o processo aplicado depois que uma URL corresponde a um modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-287">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="7b897-288">No roteamento convencional, os valores de rota de correspondência são usados para escolher a ação e o controlador de uma tabela de pesquisa de todas as ações roteadas convencionais.</span><span class="sxs-lookup"><span data-stu-id="7b897-288">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="7b897-289">No roteamento de atributo, cada modelo já está associado uma ação e nenhuma pesquisa adicional é necessária.</span><span class="sxs-lookup"><span data-stu-id="7b897-289">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="7b897-290">Geração de URL</span><span class="sxs-lookup"><span data-stu-id="7b897-290">URL Generation</span></span>

<span data-ttu-id="7b897-291">Aplicativos MVC podem usar recursos de geração de URL do roteamento para gerar links de URL para ações.</span><span class="sxs-lookup"><span data-stu-id="7b897-291">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="7b897-292">Gerar URLs elimina codificar URLs, tornar o seu código mais robusto e sustentável.</span><span class="sxs-lookup"><span data-stu-id="7b897-292">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="7b897-293">Esta seção enfoca os recursos de geração de URL fornecidos pelo MVC e só aborda Noções básicas de como funciona a geração de URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-293">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="7b897-294">Consulte [roteamento](../../fundamentals/routing.md) para obter uma descrição detalhada da geração de URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-294">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="7b897-295">O `IUrlHelper` interface é a parte subjacente da infraestrutura entre MVC e roteamento para geração de URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-295">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="7b897-296">Você encontrará uma instância de `IUrlHelper` disponíveis por meio de `Url` propriedade em controladores, exibições e componentes do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="7b897-296">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="7b897-297">Neste exemplo, o `IUrlHelper` interface é usada por meio de `Controller.Url` propriedade para gerar uma URL para outra ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-297">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="7b897-298">Se o aplicativo está usando o padrão convencional de rota, o valor de `url` variável será a cadeia de caracteres de caminho de URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="7b897-298">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="7b897-299">Esse caminho de URL é criado pelo roteamento combinando os valores de rota da solicitação atual (valores de ambiente), com os valores passados para `Url.Action` e substituindo os valores para o modelo de rota:</span><span class="sxs-lookup"><span data-stu-id="7b897-299">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="7b897-300">Cada parâmetro de rota no modelo de rota tem seu valor substituído pelos nomes correspondentes com os valores e ambiente.</span><span class="sxs-lookup"><span data-stu-id="7b897-300">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="7b897-301">Um parâmetro de rota que não tem um valor pode usar um valor padrão se ele tem um ou ser ignorado se for opcional (como no caso de `id` neste exemplo).</span><span class="sxs-lookup"><span data-stu-id="7b897-301">A route parameter that does not have a value can use a default value if it has one, or be skipped if it is optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="7b897-302">Geração de URL falhará se qualquer parâmetro de rota necessária não tem um valor correspondente.</span><span class="sxs-lookup"><span data-stu-id="7b897-302">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="7b897-303">Se a falha na geração de URL para uma rota, a próxima rota é tentada até que todas as rotas tentou ou uma correspondência for encontrada.</span><span class="sxs-lookup"><span data-stu-id="7b897-303">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="7b897-304">O exemplo de `Url.Action` acima pressupõe roteamento convencional, mas URL geração funciona da mesma forma com o roteamento de atributo, embora os conceitos sejam diferentes.</span><span class="sxs-lookup"><span data-stu-id="7b897-304">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="7b897-305">Com o roteamento convencionais, os valores de rota são usados para expandir um modelo e os valores de rota para `controller` e `action` normalmente são exibidos no modelo - isso funciona porque as URLs correspondidas pelo roteamento aderem a um *convenção*.</span><span class="sxs-lookup"><span data-stu-id="7b897-305">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="7b897-306">No roteamento de atributo, valores de rota para `controller` e `action` não podem ser exibidas no modelo - eles são usados para procurar o modelo a ser usado.</span><span class="sxs-lookup"><span data-stu-id="7b897-306">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they are instead used to look up which template to use.</span></span>

<span data-ttu-id="7b897-307">Este exemplo usa o roteamento de atributo:</span><span class="sxs-lookup"><span data-stu-id="7b897-307">This example uses attribute routing:</span></span>

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="7b897-308">MVC cria uma tabela de pesquisa de todas as ações de atributo roteada e corresponderá a `controller` e `action` valores para selecionar o modelo de rota a ser usada para geração de URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-308">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="7b897-309">No exemplo acima, `custom/url/to/destination` é gerado.</span><span class="sxs-lookup"><span data-stu-id="7b897-309">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="7b897-310">Gerar URLs, o nome da ação</span><span class="sxs-lookup"><span data-stu-id="7b897-310">Generating URLs by action name</span></span>

<span data-ttu-id="7b897-311">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="7b897-311">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="7b897-312">`Action`) e todos os relacionados sobrecargas todos são baseados nessa ideia que você deseja especificar o que você está vinculando especificando um nome de controlador e ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-312">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="7b897-313">Ao usar `Url.Action`, valores de rota atual para `controller` e `action` especificados para você, o valor de `controller` e `action` fazem parte de ambos *valores ambiente* **e** *valores*.</span><span class="sxs-lookup"><span data-stu-id="7b897-313">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="7b897-314">O método `Url.Action`, sempre usa os valores atuais das `action` e `controller` e irá gerar um caminho de URL que roteia para a ação atual.</span><span class="sxs-lookup"><span data-stu-id="7b897-314">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="7b897-315">As tentativas de usar os valores em valores de ambiente para preencher informações que você não fornecer ao gerar uma URL de roteamento.</span><span class="sxs-lookup"><span data-stu-id="7b897-315">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="7b897-316">Usando uma rota como `{a}/{b}/{c}/{d}` e valores de ambiente `{ a = Alice, b = Bob, c = Carol, d = David }`, roteamento tem informações suficientes para gerar uma URL sem valores adicionais – desde que todas as rotas parâmetros têm um valor.</span><span class="sxs-lookup"><span data-stu-id="7b897-316">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="7b897-317">Se você adicionou o valor `{ d = Donovan }`, o valor `{ d = David }` seria ignorado, e o caminho da URL gerado seria `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="7b897-317">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="7b897-318">Caminhos de URL são hierárquicos.</span><span class="sxs-lookup"><span data-stu-id="7b897-318">URL paths are hierarchical.</span></span> <span data-ttu-id="7b897-319">No exemplo acima, se você adicionou o valor `{ c = Cheryl }`, ambos os valores `{ c = Carol, d = David }` será ignorado.</span><span class="sxs-lookup"><span data-stu-id="7b897-319">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="7b897-320">Nesse caso não temos mais um valor para `d` e haverá falha na geração de URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-320">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="7b897-321">Você precisa especificar o valor desejado de `c` e `d`.</span><span class="sxs-lookup"><span data-stu-id="7b897-321">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="7b897-322">Você pode esperar atingir esse problema com a rota padrão (`{controller}/{action}/{id?}`)- mas raramente você encontrará esse comportamento na prática como `Url.Action` será sempre especificar explicitamente um `controller` e `action` valor.</span><span class="sxs-lookup"><span data-stu-id="7b897-322">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="7b897-323">Mais sobrecargas de `Url.Action` também levar adicional *valores de rota* objeto para fornecer valores para parâmetros de rota que `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="7b897-323">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="7b897-324">Você verá mais isso usado com `id` como `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="7b897-324">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="7b897-325">Por convenção o *valores de rota* geralmente é um objeto de tipo anônimo, mas ele também pode ser um `IDictionary<>` ou um *simples objeto .NET antigo*.</span><span class="sxs-lookup"><span data-stu-id="7b897-325">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="7b897-326">Quaisquer valores de rota adicionais que não coincidem com os parâmetros de rota são colocados na cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="7b897-326">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="7b897-327">Para criar uma URL absoluta, use uma sobrecarga que aceita um `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="7b897-327">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="7b897-328">Gerar URLs pela rota</span><span class="sxs-lookup"><span data-stu-id="7b897-328">Generating URLs by route</span></span>

<span data-ttu-id="7b897-329">O código acima demonstrado a geração de uma URL, passando o nome do controlador e ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-329">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="7b897-330">`IUrlHelper`também fornece o `Url.RouteUrl` família dos métodos.</span><span class="sxs-lookup"><span data-stu-id="7b897-330">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="7b897-331">Esses métodos são semelhantes às `Url.Action`, mas eles não copiar os valores atuais das `action` e `controller` para os valores de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-331">These methods are similar to `Url.Action`, but they do not copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="7b897-332">O uso mais comum é especificar um nome de rota para usar uma rota específica para gerar a URL, geralmente *sem* especificando um nome de controlador ou ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-332">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="7b897-333">Gerar URLs em HTML</span><span class="sxs-lookup"><span data-stu-id="7b897-333">Generating URLs in HTML</span></span>

<span data-ttu-id="7b897-334">`IHtmlHelper`fornece o `HtmlHelper` métodos `Html.BeginForm` e `Html.ActionLink` para gerar `<form>` e `<a>` elementos respectivamente.</span><span class="sxs-lookup"><span data-stu-id="7b897-334">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="7b897-335">Esses métodos usam o `Url.Action` método para gerar uma URL e eles aceitam argumentos semelhantes.</span><span class="sxs-lookup"><span data-stu-id="7b897-335">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="7b897-336">O `Url.RouteUrl` programas para `HtmlHelper` são `Html.BeginRouteForm` e `Html.RouteLink` que tem uma funcionalidade semelhante.</span><span class="sxs-lookup"><span data-stu-id="7b897-336">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="7b897-337">TagHelpers gerar URLs através de `form` TagHelper e `<a>` TagHelper.</span><span class="sxs-lookup"><span data-stu-id="7b897-337">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="7b897-338">Ambos usam `IUrlHelper` para sua implementação.</span><span class="sxs-lookup"><span data-stu-id="7b897-338">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="7b897-339">Consulte [trabalhar com formulários](../views/working-with-forms.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="7b897-339">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="7b897-340">Em modos de exibição, o `IUrlHelper` está disponível por meio de `Url` propriedade para a geração de URL qualquer ad hoc não coberta por acima.</span><span class="sxs-lookup"><span data-stu-id="7b897-340">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="7b897-341">Gerar URLS nos resultados da ação</span><span class="sxs-lookup"><span data-stu-id="7b897-341">Generating URLS in Action Results</span></span>

<span data-ttu-id="7b897-342">Os exemplos anteriores mostraram usando `IUrlHelper` em um controlador, enquanto o uso mais comum em um controlador está gerar uma URL como parte do resultado de uma ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-342">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="7b897-343">O `ControllerBase` e `Controller` classes base fornecem métodos de conveniência para resultados de ação que fazem referência a outra ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-343">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="7b897-344">Um uso típico é para redirecionar após aceitar a entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="7b897-344">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

<span data-ttu-id="7b897-345">Os métodos de fábrica de resultados de ação sigam um padrão semelhante aos métodos `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="7b897-345">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="7b897-346">Caso especial para rotas convencionais dedicados</span><span class="sxs-lookup"><span data-stu-id="7b897-346">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="7b897-347">Roteamento convencional pode usar um tipo especial de definição da rota chamado um *dedicada rota convencional*.</span><span class="sxs-lookup"><span data-stu-id="7b897-347">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="7b897-348">No exemplo a seguir, a rota chamada `blog` é uma rota convencional dedicada.</span><span class="sxs-lookup"><span data-stu-id="7b897-348">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="7b897-349">Usando essas definições de rota, `Url.Action("Index", "Home")` irá gerar o caminho da URL `/` com o `default` rota, mas por quê?</span><span class="sxs-lookup"><span data-stu-id="7b897-349">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="7b897-350">Você pode imaginar os valores de rota `{ controller = Home, action = Index }` deve ser suficiente para gerar uma URL usando `blog`, e o resultado seria `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="7b897-350">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="7b897-351">Rotas convencionais dedicadas dependem de um comportamento especial de valores padrão que não tem um parâmetro de rota correspondente que impede que a rota "muito greedy" com a geração de URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-351">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="7b897-352">Nesse caso, os valores padrão são `{ controller = Blog, action = Article }`e nem `controller` nem `action` aparece como um parâmetro de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-352">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="7b897-353">Quando o roteamento executa geração de URL, os valores fornecidos devem corresponder aos valores de padrão.</span><span class="sxs-lookup"><span data-stu-id="7b897-353">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="7b897-354">Geração de URL usando `blog` falhará porque os valores `{ controller = Home, action = Index }` não correspondem ao `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="7b897-354">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="7b897-355">Roteamento, em seguida, volte para tentar `default`, que é bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="7b897-355">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="7b897-356">Áreas</span><span class="sxs-lookup"><span data-stu-id="7b897-356">Areas</span></span>

<span data-ttu-id="7b897-357">[Áreas](areas.md) são um recurso MVC usado para organizar as funcionalidades relacionadas em um grupo como um roteamento-namespace separado (para ações do controlador) e a estrutura de pasta (para modos de exibição).</span><span class="sxs-lookup"><span data-stu-id="7b897-357">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="7b897-358">Permite usar áreas que um aplicativo tiver vários controladores com o mesmo nome - desde que eles têm diferentes *áreas*.</span><span class="sxs-lookup"><span data-stu-id="7b897-358">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="7b897-359">Usando áreas cria uma hierarquia com a finalidade de roteamento, adicionando outro parâmetro de rota, `area` para `controller` e `action`.</span><span class="sxs-lookup"><span data-stu-id="7b897-359">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="7b897-360">Esta seção aborda como o roteamento interage com áreas - consulte [áreas](areas.md) para obter detalhes sobre como as áreas são usadas com exibições.</span><span class="sxs-lookup"><span data-stu-id="7b897-360">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="7b897-361">O exemplo a seguir configura o MVC para usar o padrão de rota convencional e um *de rota* para uma área chamada `Blog`:</span><span class="sxs-lookup"><span data-stu-id="7b897-361">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="7b897-362">Durante a correspondência de um caminho de URL como `/Manage/Users/AddUser`, a primeira rota produzirá os valores de rota `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="7b897-362">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="7b897-363">O `area` valor de rota é produzido por um valor padrão para `area`, na verdade a rota criada pelo `MapAreaRoute` é equivalente à seguinte:</span><span class="sxs-lookup"><span data-stu-id="7b897-363">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="7b897-364">`MapAreaRoute`cria uma rota usando um valor padrão e a restrição de `area` usando o nome da área fornecido, nesse caso `Blog`.</span><span class="sxs-lookup"><span data-stu-id="7b897-364">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="7b897-365">O valor padrão garante que a rota sempre produz `{ area = Blog, ... }`, a restrição requer que o valor `{ area = Blog, ... }` para geração de URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-365">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="7b897-366">Roteamento convencional é dependente de ordem.</span><span class="sxs-lookup"><span data-stu-id="7b897-366">Conventional routing is order-dependent.</span></span> <span data-ttu-id="7b897-367">Em geral, rotas com áreas devem ser colocadas anteriormente na tabela de rotas que elas sejam mais específicas que rotas sem uma área.</span><span class="sxs-lookup"><span data-stu-id="7b897-367">In general, routes with areas should be placed earlier in the route table as they are more specific than routes without an area.</span></span>

<span data-ttu-id="7b897-368">Usando o exemplo acima, os valores de rota corresponderia a ação a seguir:</span><span class="sxs-lookup"><span data-stu-id="7b897-368">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="7b897-369">O `AreaAttribute` é o que indica que um controlador como parte de uma área, dizemos que esse controlador está no `Blog` área.</span><span class="sxs-lookup"><span data-stu-id="7b897-369">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="7b897-370">Controladores sem um `[Area]` atributo não são membros de qualquer área e será **não** corresponder quando o `area` valor de rota é fornecido pelo roteamento.</span><span class="sxs-lookup"><span data-stu-id="7b897-370">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="7b897-371">No exemplo a seguir, somente o primeiro controlador listado pode corresponder aos valores de rota `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="7b897-371">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="7b897-372">O namespace de cada controlador é mostrado aqui para fins de integridade - caso contrário, os controladores teria uma nomenclatura de conflito e gerar um erro do compilador.</span><span class="sxs-lookup"><span data-stu-id="7b897-372">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="7b897-373">Namespaces de classe não têm efeito sobre roteamento do MVC.</span><span class="sxs-lookup"><span data-stu-id="7b897-373">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="7b897-374">Os primeiros dois controladores são membros das áreas e corresponder somente quando o nome do respectivos área é fornecido pelo `area` valor de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-374">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="7b897-375">O terceiro controlador não é um membro de qualquer área e pode única correspondência quando nenhum valor para `area` é fornecido pelo roteamento.</span><span class="sxs-lookup"><span data-stu-id="7b897-375">The third controller is not a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="7b897-376">Em termos de correspondência *nenhum valor*, a ausência do `area` valor é o mesmo que o valor de `area` era nulo ou cadeia de caracteres vazia.</span><span class="sxs-lookup"><span data-stu-id="7b897-376">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="7b897-377">Ao executar uma ação dentro de uma área, o valor para a rota `area` estarão disponíveis como um *valor ambiente* para roteamento para usar para a geração de URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-377">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="7b897-378">Isso significa que, por padrão áreas atuam *Autoadesivas* para geração de URL, como demonstrado pelo exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="7b897-378">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="7b897-379">Noções básicas sobre IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="7b897-379">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="7b897-380">Esta seção é uma análise aprofundada no framework internos e como MVC escolhe uma ação a ser executada.</span><span class="sxs-lookup"><span data-stu-id="7b897-380">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="7b897-381">Um aplicativo típico não será necessário um personalizado`IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="7b897-381">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="7b897-382">Você provavelmente já usou `IActionConstraint` mesmo se você não estiver familiarizado com a interface.</span><span class="sxs-lookup"><span data-stu-id="7b897-382">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="7b897-383">O `[HttpGet]` atributo e semelhante `[Http-VERB]` atributos implementam `IActionConstraint` para limitar a execução de um método de ação.</span><span class="sxs-lookup"><span data-stu-id="7b897-383">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="7b897-384">Supondo que a rota convencional do padrão, o caminho da URL `/Products/Edit` pode produzir valores `{ controller = Products, action = Edit }`, que corresponderia **ambos** das ações mostradas aqui.</span><span class="sxs-lookup"><span data-stu-id="7b897-384">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="7b897-385">Em `IActionConstraint` terminologia dizemos que ambas as ações são consideradas candidatos - como ambos correspondem aos dados de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-385">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="7b897-386">Quando o `HttpGetAttribute` é executado, ele indicará que *Edit()* é uma correspondência para *obter* e não é uma correspondência para qualquer outro verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b897-386">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and is not a match for any other HTTP verb.</span></span> <span data-ttu-id="7b897-387">O `Edit(...)` ação não tem restrições definidas e portanto corresponderá a qualquer verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b897-387">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="7b897-388">Portanto supondo uma `POST` - somente `Edit(...)` corresponde.</span><span class="sxs-lookup"><span data-stu-id="7b897-388">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="7b897-389">Mas, para um `GET` ambas as ações podem ainda corresponder - no entanto, uma ação com um `IActionConstraint` é sempre considerada *melhor* que uma ação sem.</span><span class="sxs-lookup"><span data-stu-id="7b897-389">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="7b897-390">Assim como `Edit()` tem `[HttpGet]` ele é considerado mais específico e será selecionado se podem corresponder a ambas as ações.</span><span class="sxs-lookup"><span data-stu-id="7b897-390">So because `Edit()` has `[HttpGet]` it is considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="7b897-391">Conceitualmente, `IActionConstraint` é uma forma de *sobrecarga*, mas em vez de métodos com o mesmo nome de sobrecarga, sobrecarga entre ações que correspondam a mesma URL.</span><span class="sxs-lookup"><span data-stu-id="7b897-391">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it is overloading between actions that match the same URL.</span></span> <span data-ttu-id="7b897-392">Roteamento de atributo também usa `IActionConstraint` e pode resultar em ações de controladores diferentes, ambos sendo considerados candidatos.</span><span class="sxs-lookup"><span data-stu-id="7b897-392">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="7b897-393">Implementando IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="7b897-393">Implementing IActionConstraint</span></span>

<span data-ttu-id="7b897-394">A maneira mais simples de implementar um `IActionConstraint` é criar uma classe derivada de `System.Attribute` e colocá-lo em suas ações e controladores.</span><span class="sxs-lookup"><span data-stu-id="7b897-394">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="7b897-395">MVC descobrirá automaticamente qualquer `IActionConstraint` que são aplicadas como atributos.</span><span class="sxs-lookup"><span data-stu-id="7b897-395">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="7b897-396">Você pode usar o modelo de aplicativo para aplicar restrições, e isso se deve provavelmente a abordagem mais flexível que permite a você para metaprogram como elas são aplicadas.</span><span class="sxs-lookup"><span data-stu-id="7b897-396">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they are applied.</span></span>

<span data-ttu-id="7b897-397">No exemplo a seguir, uma restrição escolhe uma ação com base em um *código de país* dos dados de rota.</span><span class="sxs-lookup"><span data-stu-id="7b897-397">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="7b897-398">O [completo de exemplo no GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="7b897-398">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

<span data-ttu-id="7b897-399">Você é responsável por implementar o `Accept` método e escolhendo um 'Order' para a restrição para executar.</span><span class="sxs-lookup"><span data-stu-id="7b897-399">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="7b897-400">Nesse caso, o `Accept` método `true` para indicar que a ação é uma correspondência quando o `country` rotear correspondências de valor.</span><span class="sxs-lookup"><span data-stu-id="7b897-400">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="7b897-401">Isso é diferente de um `RouteValueAttribute` que permite fallback para uma ação não atribuído.</span><span class="sxs-lookup"><span data-stu-id="7b897-401">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="7b897-402">O exemplo mostra que se você definir um `en-US` ação e um código de país como `fr-FR` volte a um controlador mais genérico que não tem `[CountrySpecific(...)]` aplicado.</span><span class="sxs-lookup"><span data-stu-id="7b897-402">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that does not have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="7b897-403">O `Order` propriedade decide qual *estágio* a restrição é parte do.</span><span class="sxs-lookup"><span data-stu-id="7b897-403">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="7b897-404">Restrições da ação executar em grupos com base no `Order`.</span><span class="sxs-lookup"><span data-stu-id="7b897-404">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="7b897-405">Por exemplo, todos o framework fornecidos atributos de método HTTP a usar o mesmo `Order` valor para que eles executados no mesmo estágio.</span><span class="sxs-lookup"><span data-stu-id="7b897-405">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="7b897-406">Você pode ter tantos estágios conforme necessário implementar suas políticas desejadas.</span><span class="sxs-lookup"><span data-stu-id="7b897-406">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="7b897-407">Para decidir sobre um valor para `Order` pense ou não a restrição deve ser aplicada antes de métodos HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b897-407">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="7b897-408">Números inferiores executado primeiro.</span><span class="sxs-lookup"><span data-stu-id="7b897-408">Lower numbers run first.</span></span>
