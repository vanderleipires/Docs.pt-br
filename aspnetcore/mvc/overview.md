---
title: "Visão geral do núcleo do ASP.NET MVC"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 3e95ccd8aeda54a050cd656ea0c831fd928f53a2
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="4c68e-103">Visão geral do núcleo do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4c68e-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="4c68e-104">Por [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="4c68e-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="4c68e-105">Núcleo do ASP.NET MVC é uma estrutura avançada para a criação de aplicativos web e APIs usando Model-View-Controller design padrão.</span><span class="sxs-lookup"><span data-stu-id="4c68e-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="4c68e-106">O que é o padrão MVC?</span><span class="sxs-lookup"><span data-stu-id="4c68e-106">What is the MVC pattern?</span></span>

<span data-ttu-id="4c68e-107">O padrão de arquitetura Model-View-Controller (MVC) separa um aplicativo em três grupos principais de componentes: modelos, exibições e controladores.</span><span class="sxs-lookup"><span data-stu-id="4c68e-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="4c68e-108">Esse padrão ajuda a alcançar [separação de preocupações](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="4c68e-108">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="4c68e-109">Solicitações do usuário usando esse padrão, são roteadas para um controlador que é responsável para trabalhar com o modelo para executar ações de usuário e/ou recuperar os resultados de consultas.</span><span class="sxs-lookup"><span data-stu-id="4c68e-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="4c68e-110">O controlador escolhe a exibição a ser exibida para o usuário e fornece-o com os dados de modelo requer.</span><span class="sxs-lookup"><span data-stu-id="4c68e-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="4c68e-111">O diagrama a seguir mostra os três componentes principais e quais fazem referência a outras pessoas:</span><span class="sxs-lookup"><span data-stu-id="4c68e-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![Padrão MVC](overview/_static/mvc.png)

<span data-ttu-id="4c68e-113">Essa descrição das responsabilidades ajuda você a dimensionar o aplicativo em termos de complexidade porque é mais fácil de código, depurar e testar algo (modelo, exibição ou controlador) que tem um único trabalho (e segue o [princípio da responsabilidade única ](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="4c68e-113">This delineation of responsibilities helps you scale the application in terms of complexity because it’s easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="4c68e-114">É mais difícil de atualização, testar e depurar código que tem dependências que abrange dois ou mais dessas três áreas.</span><span class="sxs-lookup"><span data-stu-id="4c68e-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="4c68e-115">Por exemplo, a lógica da interface de usuário tende a ser alterado com mais frequência do que a lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="4c68e-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="4c68e-116">Se a lógica de negócios e o código de apresentação é combinada em um único objeto, você precisa modificar um objeto que contém a lógica de negócios sempre que alterar a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="4c68e-116">If presentation code and business logic are combined in a single object, you have to modify an object containing business logic every time you change the user interface.</span></span> <span data-ttu-id="4c68e-117">Isso é adequado para introduzir erros e exigem o novo teste de toda a lógica de negócios depois de alterar cada interface mínima do usuário.</span><span class="sxs-lookup"><span data-stu-id="4c68e-117">This is likely to introduce errors and require the retesting of all business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="4c68e-118">O modo de exibição e o controlador dependem do modelo.</span><span class="sxs-lookup"><span data-stu-id="4c68e-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="4c68e-119">No entanto, o modelo depende o modo de exibição, nem o controlador.</span><span class="sxs-lookup"><span data-stu-id="4c68e-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="4c68e-120">Esse é um dos principais benefícios da separação.</span><span class="sxs-lookup"><span data-stu-id="4c68e-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="4c68e-121">Essa separação permite que o modelo a ser criado e testado independentemente da apresentação visual.</span><span class="sxs-lookup"><span data-stu-id="4c68e-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="4c68e-122">Responsabilidades do modelo</span><span class="sxs-lookup"><span data-stu-id="4c68e-122">Model Responsibilities</span></span>

<span data-ttu-id="4c68e-123">O modelo em um aplicativo MVC representa o estado do aplicativo e qualquer lógica de negócios ou operações que devem ser executadas por ele.</span><span class="sxs-lookup"><span data-stu-id="4c68e-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="4c68e-124">Lógica de negócios deve ser encapsulada no modelo, juntamente com qualquer lógica de implementação para persistir o estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4c68e-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="4c68e-125">Exibições fortemente tipadas normalmente usará tipos ViewModel especificamente criados para conter os dados para exibir no modo de exibição; o controlador será criar e preencher essas instâncias ViewModel do modelo.</span><span class="sxs-lookup"><span data-stu-id="4c68e-125">Strongly-typed views will typically use ViewModel types specifically designed to contain the data to display on that view; the controller will create and populate these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="4c68e-126">Há várias maneiras de organizar o modelo em um aplicativo que usa o padrão arquitetônico MVC.</span><span class="sxs-lookup"><span data-stu-id="4c68e-126">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="4c68e-127">Saiba mais sobre alguns [tipos diferentes de tipos de modelo](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="4c68e-127">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="4c68e-128">Responsabilidades do modo de exibição</span><span class="sxs-lookup"><span data-stu-id="4c68e-128">View Responsibilities</span></span>

<span data-ttu-id="4c68e-129">Modos de exibição são responsáveis por apresentar conteúdo por meio da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="4c68e-129">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="4c68e-130">Eles usam o [mecanismo de exibição Razor](#razor-view-engine) para inserir o código .NET em uma marcação HTML.</span><span class="sxs-lookup"><span data-stu-id="4c68e-130">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="4c68e-131">Deve haver mínimo lógica em modos de exibição e qualquer lógica neles deve relacionar a apresentação do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="4c68e-131">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="4c68e-132">Se você encontrar arquivos a necessidade de executar uma grande quantidade de lógica no modo de exibição para exibir os dados de um modelo complexo, considere usar um [componentes da exibição](views/view-components.md), ViewModel, ou um modelo de exibição para simplificar a exibição.</span><span class="sxs-lookup"><span data-stu-id="4c68e-132">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="4c68e-133">Responsabilidades do controlador</span><span class="sxs-lookup"><span data-stu-id="4c68e-133">Controller Responsibilities</span></span>

<span data-ttu-id="4c68e-134">Os controladores são os componentes que lidar com a interação do usuário, trabalhar com o modelo e, por fim, selecionam um modo de exibição para renderizar.</span><span class="sxs-lookup"><span data-stu-id="4c68e-134">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="4c68e-135">Em um aplicativo MVC, a exibição só mostra informações; o controlador manipula e responde à entrada do usuário e interação.</span><span class="sxs-lookup"><span data-stu-id="4c68e-135">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="4c68e-136">O padrão MVC, o controlador é o ponto de entrada inicial e é responsável por selecionar qual modelo de tipos para trabalhar com e modo de exibição para renderizar (portanto, seu nome - controla como o aplicativo responde a uma determinada solicitação).</span><span class="sxs-lookup"><span data-stu-id="4c68e-136">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="4c68e-137">Controladores não devem ser excessivamente complicados por muitas responsabilidades.</span><span class="sxs-lookup"><span data-stu-id="4c68e-137">Controllers should not be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="4c68e-138">Para manter a lógica do controlador de se tornar excessivamente complexas, use o [princípio da responsabilidade única](http://deviq.com/single-responsibility-principle/) a lógica de negócios por push fora o controlador e o modelo de domínio.</span><span class="sxs-lookup"><span data-stu-id="4c68e-138">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="4c68e-139">Se você achar que as ações do controlador com frequência executam os mesmos tipos de ações, você pode seguir o [não repetitivo princípio](http://deviq.com/don-t-repeat-yourself/) movendo essas ações comuns em [filtros](#filters).</span><span class="sxs-lookup"><span data-stu-id="4c68e-139">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="4c68e-140">O que é o núcleo de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4c68e-140">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="4c68e-141">A estrutura MVC do ASP.NET Core é uma fonte leve, abra, estrutura de apresentação testável altamente otimizada para uso com o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4c68e-141">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="4c68e-142">Núcleo do ASP.NET MVC fornece uma maneira com base em padrões para criar sites dinâmicos que habilitam uma separação limpa de preocupações.</span><span class="sxs-lookup"><span data-stu-id="4c68e-142">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="4c68e-143">Ele lhe dá controle total sobre a marcação, dá suporte ao desenvolvimento amigável a TDD e usa os padrões da web mais recentes.</span><span class="sxs-lookup"><span data-stu-id="4c68e-143">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="4c68e-144">Recursos</span><span class="sxs-lookup"><span data-stu-id="4c68e-144">Features</span></span>

<span data-ttu-id="4c68e-145">Núcleo do ASP.NET MVC inclui o seguinte:</span><span class="sxs-lookup"><span data-stu-id="4c68e-145">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="4c68e-146">Roteamento</span><span class="sxs-lookup"><span data-stu-id="4c68e-146">Routing</span></span>](#routing)
* [<span data-ttu-id="4c68e-147">Associação de modelo</span><span class="sxs-lookup"><span data-stu-id="4c68e-147">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="4c68e-148">Validação de modelo</span><span class="sxs-lookup"><span data-stu-id="4c68e-148">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="4c68e-149">Injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="4c68e-149">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="4c68e-150">Filtros</span><span class="sxs-lookup"><span data-stu-id="4c68e-150">Filters</span></span>](#filters)
* [<span data-ttu-id="4c68e-151">Áreas</span><span class="sxs-lookup"><span data-stu-id="4c68e-151">Areas</span></span>](#areas)
* [<span data-ttu-id="4c68e-152">APIs da Web</span><span class="sxs-lookup"><span data-stu-id="4c68e-152">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="4c68e-153">Capacidade de teste</span><span class="sxs-lookup"><span data-stu-id="4c68e-153">Testability</span></span>](#testability)
* [<span data-ttu-id="4c68e-154">Mecanismo de exibição Razor</span><span class="sxs-lookup"><span data-stu-id="4c68e-154">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="4c68e-155">Modos de exibição fortemente tipados</span><span class="sxs-lookup"><span data-stu-id="4c68e-155">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="4c68e-156">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="4c68e-156">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="4c68e-157">Componentes do modo de exibição</span><span class="sxs-lookup"><span data-stu-id="4c68e-157">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="4c68e-158">Roteamento</span><span class="sxs-lookup"><span data-stu-id="4c68e-158">Routing</span></span>

<span data-ttu-id="4c68e-159">ASP.NET MVC de núcleo é criado na parte superior do [roteamento ASP.NET Core](../fundamentals/routing.md), um componente de mapeamento de URL poderoso que permite que você crie aplicativos que têm URLs abrangentes e pesquisáveis.</span><span class="sxs-lookup"><span data-stu-id="4c68e-159">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="4c68e-160">Isso permite que você defina URL padrões de nomenclatura do seu aplicativo que funcionam bem para (SEO) de otimização do mecanismo de pesquisa e para a geração de link, sem levar em consideração como os arquivos no servidor web são organizados.</span><span class="sxs-lookup"><span data-stu-id="4c68e-160">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="4c68e-161">Você pode definir suas rotas usando uma sintaxe de modelo de rota conveniente que dá suporte a restrições de valor de rota, padrões e valores opcionais.</span><span class="sxs-lookup"><span data-stu-id="4c68e-161">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="4c68e-162">*Roteamento baseado em convenção* permite definir globalmente a URL de formatos que seu aplicativo aceita e como cada um dos formatos é mapeado para um método de ação específica em determinado controlador.</span><span class="sxs-lookup"><span data-stu-id="4c68e-162">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="4c68e-163">Quando uma solicitação de entrada é recebida, o mecanismo de roteamento analisa a URL corresponde a um dos formatos de URL definidos e, em seguida, chama o método de ação do controlador associado.</span><span class="sxs-lookup"><span data-stu-id="4c68e-163">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="4c68e-164">*Roteamento de atributo* permite que você especifique as informações de roteamento de decoração de seus controladores e ações com atributos que definem rotas do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4c68e-164">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="4c68e-165">Isso significa que suas definições de rota são colocadas ao lado do controlador e ação com a qual está associados.</span><span class="sxs-lookup"><span data-stu-id="4c68e-165">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [1, 4]}} -->

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a><span data-ttu-id="4c68e-166">Associação de modelo</span><span class="sxs-lookup"><span data-stu-id="4c68e-166">Model binding</span></span>

<span data-ttu-id="4c68e-167">Núcleo do ASP.NET MVC [associação de modelo](models/model-binding.md) converte dados de solicitação de cliente (valores de formulário, os dados de rota, parâmetros de cadeia de caracteres de consulta, os cabeçalhos HTTP) em objetos que o controlador pode manipular.</span><span class="sxs-lookup"><span data-stu-id="4c68e-167">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="4c68e-168">Como resultado, a lógica de controlador não precisa fazer o trabalho de descobrir os dados de solicitação de entrada; ele simplesmente tem os dados como parâmetros para os métodos de ação.</span><span class="sxs-lookup"><span data-stu-id="4c68e-168">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="4c68e-169">validação de modelo</span><span class="sxs-lookup"><span data-stu-id="4c68e-169">Model validation</span></span>

<span data-ttu-id="4c68e-170">Dá suporte ao MVC do ASP.NET Core [validação](models/validation.md) decorando seu objeto de modelo com atributos de validação de anotação de dados.</span><span class="sxs-lookup"><span data-stu-id="4c68e-170">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="4c68e-171">Os atributos de validação são verificadas no lado do cliente antes que os valores são postados no servidor, bem como no servidor antes da ação de controlador é chamado.</span><span class="sxs-lookup"><span data-stu-id="4c68e-171">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [4, 5, 8, 9]}} -->

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

<span data-ttu-id="4c68e-172">Uma ação do controlador:</span><span class="sxs-lookup"><span data-stu-id="4c68e-172">A controller action:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [3]}} -->

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // If we got this far, something failed, redisplay form
    return View(model);
}
```

<span data-ttu-id="4c68e-173">A estrutura tratará a validação de solicitação de dados no cliente e no servidor.</span><span class="sxs-lookup"><span data-stu-id="4c68e-173">The framework will handle validating request data both on the client and on the server.</span></span> <span data-ttu-id="4c68e-174">Lógica de validação especificada em tipos de modelo é adicionada aos modos de exibição renderizados como anotações discretas e é imposta no navegador com [jQuery validação](http://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="4c68e-174">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](http://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="4c68e-175">Injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="4c68e-175">Dependency injection</span></span>

<span data-ttu-id="4c68e-176">ASP.NET Core tem suporte interno para [injeção de dependência (DI)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="4c68e-176">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="4c68e-177">No ASP.NET MVC de núcleo, [controladores](controllers/dependency-injection.md) pode solicitação necessários serviços por meio de seus construtores, possibilitando que siga a [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="4c68e-177">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="4c68e-178">O aplicativo também pode usar [arquivos no modo de exibição de injeção de dependência](views/dependency-injection.md), usando o `@inject` diretiva:</span><span class="sxs-lookup"><span data-stu-id="4c68e-178">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@inject SomeService ServiceName
<!DOCTYPE html>
<html>
<head>
  <title>@ServiceName.GetTitle</title>
</head>
<body>
  <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a><span data-ttu-id="4c68e-179">Filtros</span><span class="sxs-lookup"><span data-stu-id="4c68e-179">Filters</span></span>

<span data-ttu-id="4c68e-180">[Filtros](controllers/filters.md) ajudar os desenvolvedores a encapsular resolvem preocupações, como o tratamento de exceção ou autorização.</span><span class="sxs-lookup"><span data-stu-id="4c68e-180">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="4c68e-181">Filtros de habilitar a lógica de pré e pós-processamento personalizada em execução para os métodos de ação e podem ser configurados para execução em determinados pontos no pipeline de execução para uma determinada solicitação.</span><span class="sxs-lookup"><span data-stu-id="4c68e-181">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="4c68e-182">Filtros podem ser aplicados a controladores ou ações como atributos (ou podem ser executados global).</span><span class="sxs-lookup"><span data-stu-id="4c68e-182">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="4c68e-183">Vários filtros (como `Authorize`) são incluídas na estrutura.</span><span class="sxs-lookup"><span data-stu-id="4c68e-183">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {

```

### <a name="areas"></a><span data-ttu-id="4c68e-184">Áreas</span><span class="sxs-lookup"><span data-stu-id="4c68e-184">Areas</span></span>

<span data-ttu-id="4c68e-185">[Áreas](controllers/areas.md) fornecem uma maneira de particionar um aplicativo da Web MVC do ASP.NET Core grande em menores agrupamentos funcionais.</span><span class="sxs-lookup"><span data-stu-id="4c68e-185">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="4c68e-186">Uma área é efetivamente uma estrutura MVC dentro de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4c68e-186">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="4c68e-187">Em um projeto MVC, componentes lógicos como modelo, o controlador e o modo de exibição são mantidos em pastas diferentes e MVC usa convenções de nomenclatura para criar a relação entre esses componentes.</span><span class="sxs-lookup"><span data-stu-id="4c68e-187">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="4c68e-188">Para um aplicativo grande, pode ser vantajoso para dividir o aplicativo em áreas separadas de nível alto de funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="4c68e-188">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="4c68e-189">Por exemplo, um aplicativo de comércio eletrônico com várias unidades de negócios, como check-out, cobrança e pesquisa etc. Cada uma dessas unidades têm seus próprios modos de exibição do componente lógico, controladores e modelos.</span><span class="sxs-lookup"><span data-stu-id="4c68e-189">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="4c68e-190">APIs da Web</span><span class="sxs-lookup"><span data-stu-id="4c68e-190">Web APIs</span></span>

<span data-ttu-id="4c68e-191">Além de ser uma excelente plataforma para a criação de sites da web, MVC do ASP.NET Core tem excelente suporte para a criação de APIs da Web.</span><span class="sxs-lookup"><span data-stu-id="4c68e-191">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="4c68e-192">Você pode criar serviços que podem alcançar uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="4c68e-192">You can build services that can reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="4c68e-193">A estrutura inclui suporte para a negociação de conteúdo HTTP com suporte interno para [dados de formatação](models/formatting.md) como JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="4c68e-193">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="4c68e-194">Gravar [formatadores personalizados](advanced/custom-formatters.md) para adicionar suporte para seus próprios formatos.</span><span class="sxs-lookup"><span data-stu-id="4c68e-194">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="4c68e-195">Use geração de link para habilitar o suporte para hipermídia.</span><span class="sxs-lookup"><span data-stu-id="4c68e-195">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="4c68e-196">Habilitar o suporte para [recursos entre origens (CORS) de compartilhamento](http://www.w3.org/TR/cors/) para que suas APIs da Web podem ser compartilhados entre vários aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="4c68e-196">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="4c68e-197">Capacidade de teste</span><span class="sxs-lookup"><span data-stu-id="4c68e-197">Testability</span></span>

<span data-ttu-id="4c68e-198">Uso da estrutura de injeção de dependência e interfaces, é adequado para testes de unidade e a estrutura inclui recursos (como um provedor TestHost e InMemory para Entity Framework) que tornam [testes de integração](../testing/integration-testing.md) rápido e fácil também.</span><span class="sxs-lookup"><span data-stu-id="4c68e-198">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="4c68e-199">Saiba mais sobre [lógica do controlador de teste](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="4c68e-199">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="4c68e-200">Mecanismo de exibição Razor</span><span class="sxs-lookup"><span data-stu-id="4c68e-200">Razor view engine</span></span>

<span data-ttu-id="4c68e-201">[Modos de exibição do ASP.NET MVC Core](views/overview.md) usar o [mecanismo de exibição Razor](views/razor.md) renderizar exibições.</span><span class="sxs-lookup"><span data-stu-id="4c68e-201">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="4c68e-202">Razor é uma linguagem de marcação de modelo compact, expressivas e fluidos para definir modos de exibição usando o código c# inserido.</span><span class="sxs-lookup"><span data-stu-id="4c68e-202">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="4c68e-203">Razor é usado para gerar dinamicamente o conteúdo da web no servidor.</span><span class="sxs-lookup"><span data-stu-id="4c68e-203">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="4c68e-204">Corretamente, você pode combinar código de servidor com o código e conteúdo do lado cliente.</span><span class="sxs-lookup"><span data-stu-id="4c68e-204">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="4c68e-205">Usando o mecanismo de exibição Razor você pode definir [layouts](views/layout.md), [exibições parciais](views/partial.md) e seções substituíveis.</span><span class="sxs-lookup"><span data-stu-id="4c68e-205">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="4c68e-206">Modos de exibição fortemente tipados</span><span class="sxs-lookup"><span data-stu-id="4c68e-206">Strongly typed views</span></span>

<span data-ttu-id="4c68e-207">Modos de exibição Razor do MVC podem ser fortemente tipados com base no seu modelo.</span><span class="sxs-lookup"><span data-stu-id="4c68e-207">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="4c68e-208">Controladores podem passar um modelo fortemente tipado para exibições habilitando seus modos de exibição para que a verificação de tipo e suporte do IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="4c68e-208">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="4c68e-209">Por exemplo, a exibição a seguir define um modelo do tipo `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="4c68e-209">For example, the following view defines a model of type `IEnumerable<Product>`:</span></span>

```html
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="4c68e-210">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="4c68e-210">Tag Helpers</span></span>

<span data-ttu-id="4c68e-211">[Auxiliares da marcação](views/tag-helpers/intro.md) habilitar código do lado do servidor participar de criação e renderização de elementos HTML em arquivos do Razor.</span><span class="sxs-lookup"><span data-stu-id="4c68e-211">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="4c68e-212">Você pode usar os auxiliares de marcação para definir marcas personalizadas (por exemplo, `<environment>`) ou para modificar o comportamento de marcas existentes (por exemplo, `<label>`).</span><span class="sxs-lookup"><span data-stu-id="4c68e-212">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="4c68e-213">Auxiliares de marcação vincular a elementos específicos com base no nome do elemento e seus atributos.</span><span class="sxs-lookup"><span data-stu-id="4c68e-213">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="4c68e-214">Eles fornecem os benefícios de renderização do lado do servidor e ainda preservar um experiência de edição de HTML.</span><span class="sxs-lookup"><span data-stu-id="4c68e-214">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="4c68e-215">Há muitos auxiliares de marcação interna para tarefas comuns - como a criação de formulários, links, ativos de carregamento e pacotes mais - e ainda mais disponíveis em repositórios GitHub públicos e como NuGet.</span><span class="sxs-lookup"><span data-stu-id="4c68e-215">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="4c68e-216">Auxiliares de marca são criados no c#, e eles se destinam a elementos HTML com base no nome do elemento, o nome do atributo ou marca pai.</span><span class="sxs-lookup"><span data-stu-id="4c68e-216">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="4c68e-217">Por exemplo, o interno LinkTagHelper pode ser usado para criar um link para o `Login` ação o `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="4c68e-217">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3]}} -->

```html
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="4c68e-218">O `EnvironmentTagHelper` pode ser usado para incluir scripts diferentes em exibições (por exemplo, minimizada ou raw) com base no ambiente de tempo de execução, como desenvolvimento, teste ou produção:</span><span class="sxs-lookup"><span data-stu-id="4c68e-218">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 3, 4, 9]}} -->

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

<span data-ttu-id="4c68e-219">Auxiliares de marcação fornecem uma experiência de desenvolvimento compatível com HTML e um ambiente rico de IntelliSense para criar marcação HTML e Razor.</span><span class="sxs-lookup"><span data-stu-id="4c68e-219">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="4c68e-220">A maioria os auxiliares de marca internos elementos HTML existentes de destino e fornece os atributos do lado do servidor para o elemento.</span><span class="sxs-lookup"><span data-stu-id="4c68e-220">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="4c68e-221">Componentes do modo de exibição</span><span class="sxs-lookup"><span data-stu-id="4c68e-221">View Components</span></span>

<span data-ttu-id="4c68e-222">[Exibir componentes](views/view-components.md) permitem a lógica de processamento do pacote e reutilizá-lo em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4c68e-222">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="4c68e-223">Elas são semelhantes às [exibições parciais](views/partial.md), mas com a lógica associada.</span><span class="sxs-lookup"><span data-stu-id="4c68e-223">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
