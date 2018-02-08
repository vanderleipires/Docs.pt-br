---
title: "Componentes da exibição"
author: rick-anderson
description: "Os Componentes de Exibição destinam-se a qualquer momento em que há uma lógica de renderização reutilizável."
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: 27e77b8fa032c2b5be753a27db748b7499e27105
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="view-components"></a><span data-ttu-id="fc859-103">Componentes da exibição</span><span class="sxs-lookup"><span data-stu-id="fc859-103">View components</span></span>

<span data-ttu-id="fc859-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fc859-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fc859-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fc859-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introducing-view-components"></a><span data-ttu-id="fc859-106">Introdução aos componentes de exibição</span><span class="sxs-lookup"><span data-stu-id="fc859-106">Introducing view components</span></span>

<span data-ttu-id="fc859-107">Uma novidade no ASP.NET Core MVC, os componentes de exibição são semelhantes às exibições parciais, mas são muito mais eficientes.</span><span class="sxs-lookup"><span data-stu-id="fc859-107">New to ASP.NET Core MVC, view components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="fc859-108">Os componentes de exibição não usam a associação de modelos e dependem apenas dos dados fornecidos durante uma chamada a eles.</span><span class="sxs-lookup"><span data-stu-id="fc859-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="fc859-109">Um componente de exibição:</span><span class="sxs-lookup"><span data-stu-id="fc859-109">A view component:</span></span>

* <span data-ttu-id="fc859-110">Renderiza uma parte em vez de uma resposta inteira.</span><span class="sxs-lookup"><span data-stu-id="fc859-110">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="fc859-111">Inclui os mesmos benefícios de capacidade de teste e separação de interesses e encontrados entre um controlador e uma exibição.</span><span class="sxs-lookup"><span data-stu-id="fc859-111">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="fc859-112">Pode ter parâmetros e uma lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="fc859-112">Can have parameters and business logic.</span></span>
* <span data-ttu-id="fc859-113">É geralmente invocado em uma página de layout.</span><span class="sxs-lookup"><span data-stu-id="fc859-113">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="fc859-114">Os componentes de exibição destinam-se a qualquer momento em que há uma lógica de renderização reutilizável muito complexa para uma exibição parcial, como:</span><span class="sxs-lookup"><span data-stu-id="fc859-114">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="fc859-115">Menus de navegação dinâmica</span><span class="sxs-lookup"><span data-stu-id="fc859-115">Dynamic navigation menus</span></span>
* <span data-ttu-id="fc859-116">Nuvem de marcas (na qual ela consulta o banco de dados)</span><span class="sxs-lookup"><span data-stu-id="fc859-116">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="fc859-117">Painel de logon</span><span class="sxs-lookup"><span data-stu-id="fc859-117">Login panel</span></span>
* <span data-ttu-id="fc859-118">Carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="fc859-118">Shopping cart</span></span>
* <span data-ttu-id="fc859-119">Artigos publicados recentemente</span><span class="sxs-lookup"><span data-stu-id="fc859-119">Recently published articles</span></span>
* <span data-ttu-id="fc859-120">Conteúdo da barra lateral em um blog típico</span><span class="sxs-lookup"><span data-stu-id="fc859-120">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="fc859-121">Um painel de logon que é renderizado em cada página e mostra os links para logoff ou logon, dependendo do estado de logon do usuário</span><span class="sxs-lookup"><span data-stu-id="fc859-121">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="fc859-122">Um componente de exibição consiste em duas partes: a classe (normalmente derivada de [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) e o resultado que ele retorna (normalmente, uma exibição).</span><span class="sxs-lookup"><span data-stu-id="fc859-122">A view component consists of two parts: the class (typically derived from [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="fc859-123">Assim como os controladores, um componente de exibição pode ser um POCO, mas a maioria dos desenvolvedores desejará aproveitar os métodos e as propriedades disponíveis com a derivação de `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="fc859-123">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="fc859-124">Criando um componente de exibição</span><span class="sxs-lookup"><span data-stu-id="fc859-124">Creating a view component</span></span>

<span data-ttu-id="fc859-125">Esta seção contém os requisitos de alto nível para a criação de um componente de exibição.</span><span class="sxs-lookup"><span data-stu-id="fc859-125">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="fc859-126">Mais adiante neste artigo, examinaremos cada etapa em detalhes e criaremos um componente de exibição.</span><span class="sxs-lookup"><span data-stu-id="fc859-126">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="fc859-127">A classe de componente de exibição</span><span class="sxs-lookup"><span data-stu-id="fc859-127">The view component class</span></span>

<span data-ttu-id="fc859-128">Uma classe de componente de exibição pode ser criada por um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="fc859-128">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="fc859-129">Derivação de *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="fc859-129">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="fc859-130">Decoração de uma classe com o atributo `[ViewComponent]` ou derivação de uma classe com o atributo `[ViewComponent]`</span><span class="sxs-lookup"><span data-stu-id="fc859-130">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="fc859-131">Criação de uma classe em que o nome termina com o sufixo *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="fc859-131">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="fc859-132">Assim como os controladores, os componentes de exibição precisam ser classes públicas, não aninhadas e não abstratas.</span><span class="sxs-lookup"><span data-stu-id="fc859-132">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="fc859-133">O nome do componente de exibição é o nome da classe com o sufixo "ViewComponent" removido.</span><span class="sxs-lookup"><span data-stu-id="fc859-133">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="fc859-134">Também pode ser especificado de forma explícita com a propriedade `ViewComponentAttribute.Name`.</span><span class="sxs-lookup"><span data-stu-id="fc859-134">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="fc859-135">Uma classe de componente de exibição:</span><span class="sxs-lookup"><span data-stu-id="fc859-135">A view component class:</span></span>

* <span data-ttu-id="fc859-136">Dá suporte total à [injeção de dependência do construtor](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="fc859-136">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="fc859-137">Não participa do ciclo de vida do controlador, o que significa que não é possível usar [filtros](../controllers/filters.md) em um componente de exibição</span><span class="sxs-lookup"><span data-stu-id="fc859-137">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="fc859-138">Métodos de componente de exibição</span><span class="sxs-lookup"><span data-stu-id="fc859-138">View component methods</span></span>

<span data-ttu-id="fc859-139">Um componente de exibição define sua lógica em um método `InvokeAsync` que retorna um `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="fc859-139">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="fc859-140">Os parâmetros são recebidos diretamente da invocação do componente de exibição, não da associação de modelos.</span><span class="sxs-lookup"><span data-stu-id="fc859-140">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="fc859-141">Um componente de exibição nunca manipula uma solicitação diretamente.</span><span class="sxs-lookup"><span data-stu-id="fc859-141">A view component never directly handles a request.</span></span> <span data-ttu-id="fc859-142">Normalmente, um componente de exibição inicializa um modelo e passa-o para uma exibição chamando o método `View`.</span><span class="sxs-lookup"><span data-stu-id="fc859-142">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="fc859-143">Em resumo, os métodos de componente de exibição:</span><span class="sxs-lookup"><span data-stu-id="fc859-143">In summary, view component methods:</span></span>

* <span data-ttu-id="fc859-144">Definem um método `InvokeAsync` que retorna um `IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="fc859-144">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="fc859-145">Normalmente, inicializam um modelo e passam-o para uma exibição chamando o método `ViewComponent` `View`</span><span class="sxs-lookup"><span data-stu-id="fc859-145">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="fc859-146">Os parâmetros são recebidos do método de chamada, não do HTTP e não há nenhuma associação de modelos</span><span class="sxs-lookup"><span data-stu-id="fc859-146">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="fc859-147">Não podem ser acessados diretamente como um ponto de extremidade HTTP e são invocados no código (normalmente em uma exibição).</span><span class="sxs-lookup"><span data-stu-id="fc859-147">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="fc859-148">Um componente de exibição nunca manipula uma solicitação</span><span class="sxs-lookup"><span data-stu-id="fc859-148">A view component never handles a request</span></span>
* <span data-ttu-id="fc859-149">São sobrecarregados na assinatura, em vez de nos detalhes da solicitação HTTP atual</span><span class="sxs-lookup"><span data-stu-id="fc859-149">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="fc859-150">Caminho de pesquisa de exibição</span><span class="sxs-lookup"><span data-stu-id="fc859-150">View search path</span></span>

<span data-ttu-id="fc859-151">O tempo de execução pesquisa a exibição nos seguintes caminhos:</span><span class="sxs-lookup"><span data-stu-id="fc859-151">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="fc859-152">Views/\<nome_do_controlador>/Components/\<nome_do_componente_da_exibição>/\<nome_do_modo_de_exibição></span><span class="sxs-lookup"><span data-stu-id="fc859-152">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="fc859-153">Views/Shared/Components/\<nome_do_componente_da_exibição>/\<nome_do_modo_de_exibição></span><span class="sxs-lookup"><span data-stu-id="fc859-153">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="fc859-154">O nome de exibição padrão de um componente de exibição é *Default*, o que significa que o arquivo de exibição geralmente será nomeado *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fc859-154">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="fc859-155">Especifique outro nome de exibição ao criar o resultado do componente de exibição ou ao chamar o método `View`.</span><span class="sxs-lookup"><span data-stu-id="fc859-155">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="fc859-156">Recomendamos que você nomeie o arquivo de exibição *Default.cshtml* e use o caminho *Views/Shared/Components/\<nome_do_componente_da_exibição>/\<nome_do_modo_de_exibição>*.</span><span class="sxs-lookup"><span data-stu-id="fc859-156">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="fc859-157">O componente de exibição `PriorityList` usado nesta amostra usa *Views/Shared/Components/PriorityList/Default.cshtml* como a exibição do componente de exibição.</span><span class="sxs-lookup"><span data-stu-id="fc859-157">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="fc859-158">Invocando um componente de exibição</span><span class="sxs-lookup"><span data-stu-id="fc859-158">Invoking a view component</span></span>

<span data-ttu-id="fc859-159">Para usar o componente de exibição, chame o seguinte em uma exibição:</span><span class="sxs-lookup"><span data-stu-id="fc859-159">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="fc859-160">Os parâmetros serão passados para o método `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="fc859-160">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="fc859-161">O componente de exibição `PriorityList` desenvolvido no artigo é invocado por meio do arquivo de exibição *Views/Todo/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fc859-161">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="fc859-162">A seguir, o método `InvokeAsync` é chamado com dois parâmetros:</span><span class="sxs-lookup"><span data-stu-id="fc859-162">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="fc859-163">Invocando um componente de exibição como um Auxiliar de Marca</span><span class="sxs-lookup"><span data-stu-id="fc859-163">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="fc859-164">Para o ASP.NET Core 1.1 e superior, invoque um componente de exibição como um [Auxiliar de Marca](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="fc859-164">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="fc859-165">Os parâmetros de classe e de método na formatação Pascal Case para Auxiliares de Marca são convertidos em seu [kebab case em minúsculas](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="fc859-165">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="fc859-166">Para invocar um componente de exibição, o Auxiliar de Marca usa o elemento `<vc></vc>`.</span><span class="sxs-lookup"><span data-stu-id="fc859-166">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="fc859-167">O componente de exibição é especificado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="fc859-167">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="fc859-168">Observação: para usar um Componente de Exibição como um Auxiliar de Marca, é necessário registrar o assembly que contém o Componente de Exibição usando a diretiva `@addTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="fc859-168">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="fc859-169">Por exemplo, se o Componente de Exibição está em um assembly chamado "MyWebApp", adicione a seguinte diretiva ao arquivo `_ViewImports.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="fc859-169">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="fc859-170">Registre um Componente de Exibição como um Auxiliar de Marca em qualquer arquivo que referencia o Componente de Exibição.</span><span class="sxs-lookup"><span data-stu-id="fc859-170">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="fc859-171">Consulte [Gerenciando o escopo do Auxiliar de Marca](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) para obter mais informações sobre como registrar Auxiliares de Marca.</span><span class="sxs-lookup"><span data-stu-id="fc859-171">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="fc859-172">O método `InvokeAsync` usado neste tutorial:</span><span class="sxs-lookup"><span data-stu-id="fc859-172">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="fc859-173">Na marcação do Auxiliar de Marca:</span><span class="sxs-lookup"><span data-stu-id="fc859-173">In Tag Helper markup:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="fc859-174">Na amostra acima, o componente de exibição `PriorityList` torna-se `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="fc859-174">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="fc859-175">Os parâmetros para o componente de exibição são passados como atributos em kebab case em minúsculas.</span><span class="sxs-lookup"><span data-stu-id="fc859-175">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="fc859-176">Invocando um componente de exibição diretamente em um controlador</span><span class="sxs-lookup"><span data-stu-id="fc859-176">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="fc859-177">Os componentes de exibição normalmente são invocados em uma exibição, mas você pode invocá-los diretamente em um método do controlador.</span><span class="sxs-lookup"><span data-stu-id="fc859-177">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="fc859-178">Embora os componentes de exibição não definam pontos de extremidade como controladores, você pode implementar com facilidade uma ação do controlador que retorna o conteúdo de um `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="fc859-178">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="fc859-179">Neste exemplo, o componente de exibição é chamado diretamente no controlador:</span><span class="sxs-lookup"><span data-stu-id="fc859-179">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="fc859-180">Passo a passo: criando um componente de exibição simples</span><span class="sxs-lookup"><span data-stu-id="fc859-180">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="fc859-181">[Baixe](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compile e teste o código inicial.</span><span class="sxs-lookup"><span data-stu-id="fc859-181">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="fc859-182">É um projeto simples com um controlador `Todo` que exibe uma lista de itens *Todo*.</span><span class="sxs-lookup"><span data-stu-id="fc859-182">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Lista de ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="fc859-184">Adicionar uma classe ViewComponent</span><span class="sxs-lookup"><span data-stu-id="fc859-184">Add a ViewComponent class</span></span>

<span data-ttu-id="fc859-185">Crie uma pasta *ViewComponents* e adicione a seguinte classe `PriorityListViewComponent`:</span><span class="sxs-lookup"><span data-stu-id="fc859-185">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="fc859-186">Observações sobre o código:</span><span class="sxs-lookup"><span data-stu-id="fc859-186">Notes on the code:</span></span>

* <span data-ttu-id="fc859-187">As classes de componente de exibição podem ser contidas em **qualquer** pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="fc859-187">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="fc859-188">Como o nome da classe PriorityList**ViewComponent** termina com o sufixo **ViewComponent**, o tempo de execução usará a cadeia de caracteres "PriorityList" ao referenciar o componente de classe em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="fc859-188">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="fc859-189">Explicarei isso mais detalhadamente mais adiante.</span><span class="sxs-lookup"><span data-stu-id="fc859-189">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="fc859-190">O atributo `[ViewComponent]` pode alterar o nome usado para referenciar um componente de exibição.</span><span class="sxs-lookup"><span data-stu-id="fc859-190">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="fc859-191">Por exemplo, poderíamos nomear a classe `XYZ` e aplicar o atributo `ViewComponent`:</span><span class="sxs-lookup"><span data-stu-id="fc859-191">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="fc859-192">O atributo `[ViewComponent]` acima instrui o seletor de componente de exibição a usar o nome `PriorityList` ao procurar as exibições associadas ao componente e a usar a cadeia de caracteres "PriorityList" ao referenciar o componente de classe em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="fc859-192">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="fc859-193">Explicarei isso mais detalhadamente mais adiante.</span><span class="sxs-lookup"><span data-stu-id="fc859-193">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="fc859-194">O componente usa a [injeção de dependência](../../fundamentals/dependency-injection.md) para disponibilizar o contexto de dados.</span><span class="sxs-lookup"><span data-stu-id="fc859-194">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="fc859-195">`InvokeAsync` expõe um método que pode ser chamado em uma exibição e pode usar um número arbitrário de argumentos.</span><span class="sxs-lookup"><span data-stu-id="fc859-195">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="fc859-196">O método `InvokeAsync` retorna o conjunto de itens `ToDo` que atendem aos parâmetros `isDone` e `maxPriority`.</span><span class="sxs-lookup"><span data-stu-id="fc859-196">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="fc859-197">Criar a exibição do Razor do componente de exibição</span><span class="sxs-lookup"><span data-stu-id="fc859-197">Create the view component Razor view</span></span>

* <span data-ttu-id="fc859-198">Crie a pasta *Views/Shared/Components*.</span><span class="sxs-lookup"><span data-stu-id="fc859-198">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="fc859-199">Essa pasta **deve** nomeada *Components*.</span><span class="sxs-lookup"><span data-stu-id="fc859-199">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="fc859-200">Crie a pasta *Views/Shared/Components/PriorityList*.</span><span class="sxs-lookup"><span data-stu-id="fc859-200">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="fc859-201">Esse nome de pasta deve corresponder ao nome da classe do componente de exibição ou ao nome da classe menos o sufixo (se seguimos a convenção e usamos o sufixo *ViewComponent* no nome da classe).</span><span class="sxs-lookup"><span data-stu-id="fc859-201">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="fc859-202">Se você usou o atributo `ViewComponent`, o nome da classe precisa corresponder à designação de atributo.</span><span class="sxs-lookup"><span data-stu-id="fc859-202">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="fc859-203">Crie uma exibição do Razor *Views/Shared/Components/PriorityList/Default.cshtml*: [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="fc859-203">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="fc859-204">A exibição do Razor usa uma lista de `TodoItem` e exibe-os.</span><span class="sxs-lookup"><span data-stu-id="fc859-204">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="fc859-205">Se o método `InvokeAsync` do componente de exibição não passar o nome da exibição (como em nossa amostra), *Default* será usado como o nome da exibição, por convenção.</span><span class="sxs-lookup"><span data-stu-id="fc859-205">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="fc859-206">Mais adiante no tutorial, mostrarei como passar o nome da exibição.</span><span class="sxs-lookup"><span data-stu-id="fc859-206">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="fc859-207">Para substituir o estilo padrão de um controlador específico, adicione uma exibição à pasta de exibição específica ao controlador (por exemplo, *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="fc859-207">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="fc859-208">Se o componente de exibição é específico ao controlador, adicione-o à pasta específica ao controlador (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="fc859-208">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="fc859-209">Adicione um `div` que contém uma chamada ao componente da lista de prioridades à parte inferior do arquivo *Views/Todo/index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fc859-209">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="fc859-210">A marcação `@await Component.InvokeAsync` mostra a sintaxe para chamar componentes de exibição.</span><span class="sxs-lookup"><span data-stu-id="fc859-210">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="fc859-211">O primeiro argumento é o nome do componente que queremos invocar ou chamar.</span><span class="sxs-lookup"><span data-stu-id="fc859-211">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="fc859-212">Os parâmetros seguintes são passados para o componente.</span><span class="sxs-lookup"><span data-stu-id="fc859-212">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="fc859-213">`InvokeAsync` pode usar um número arbitrário de argumentos.</span><span class="sxs-lookup"><span data-stu-id="fc859-213">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="fc859-214">Teste o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fc859-214">Test the app.</span></span> <span data-ttu-id="fc859-215">A seguinte imagem mostra a lista ToDo e os itens de prioridade:</span><span class="sxs-lookup"><span data-stu-id="fc859-215">The following image shows the ToDo list and the priority items:</span></span>

![lista de tarefas pendentes e itens de prioridade](view-components/_static/pi.png)

<span data-ttu-id="fc859-217">Também chame o componente de exibição diretamente no controlador:</span><span class="sxs-lookup"><span data-stu-id="fc859-217">You can also call the view component directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![itens de prioridade da ação IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="fc859-219">Especificando um nome de exibição</span><span class="sxs-lookup"><span data-stu-id="fc859-219">Specifying a view name</span></span>

<span data-ttu-id="fc859-220">Um componente de exibição complexo pode precisar especificar uma exibição não padrão em algumas condições.</span><span class="sxs-lookup"><span data-stu-id="fc859-220">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="fc859-221">O código a seguir mostra como especificar a exibição "PVC" no método `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="fc859-221">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="fc859-222">Atualize o método `InvokeAsync` na classe `PriorityListViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="fc859-222">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="fc859-223">Copie o arquivo *Views/Shared/Components/PriorityList/Default.cshtml* para uma exibição nomeada *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fc859-223">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="fc859-224">Adicione um cabeçalho para indicar que a exibição PVC está sendo usada.</span><span class="sxs-lookup"><span data-stu-id="fc859-224">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="fc859-225">Atualize *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fc859-225">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="fc859-226">Execute o aplicativo e verifique a exibição PVC.</span><span class="sxs-lookup"><span data-stu-id="fc859-226">Run the app and verify PVC view.</span></span>

![Componente de exibição de prioridade](view-components/_static/pvc.png)

<span data-ttu-id="fc859-228">Se a exibição PVC não é renderizada, verifique se você está chamando o componente de exibição com uma prioridade igual a 4 ou superior.</span><span class="sxs-lookup"><span data-stu-id="fc859-228">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="fc859-229">Examinar o caminho de exibição</span><span class="sxs-lookup"><span data-stu-id="fc859-229">Examine the view path</span></span>

* <span data-ttu-id="fc859-230">Altere o parâmetro de prioridade para três ou menos para que a exibição de prioridade não seja retornada.</span><span class="sxs-lookup"><span data-stu-id="fc859-230">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="fc859-231">Renomeie temporariamente o *Views/Todo/Components/PriorityList/Default.cshtml* como *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fc859-231">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="fc859-232">Teste o aplicativo. Você obterá o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="fc859-232">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="fc859-233">Copie *Views/Todo/Components/PriorityList/1Default.cshtml* para *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fc859-233">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="fc859-234">Adicione uma marcação ao componente de exibição Todo *Shared* para indicar que a exibição foi obtida da pasta *Shared*.</span><span class="sxs-lookup"><span data-stu-id="fc859-234">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="fc859-235">Teste o componente de exibição **Shared**.</span><span class="sxs-lookup"><span data-stu-id="fc859-235">Test the **Shared** component view.</span></span>

![Saída de ToDo com o componente de exibição Shared](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="fc859-237">Evitando cadeias de caracteres mágicas</span><span class="sxs-lookup"><span data-stu-id="fc859-237">Avoiding magic strings</span></span>

<span data-ttu-id="fc859-238">Se deseja obter segurança em tempo de compilação, substitua o nome do componente de exibição embutido em código pelo nome da classe.</span><span class="sxs-lookup"><span data-stu-id="fc859-238">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="fc859-239">Crie o componente de exibição sem o sufixo "ViewComponent":</span><span class="sxs-lookup"><span data-stu-id="fc859-239">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="fc859-240">Adicione uma instrução `using` ao arquivo de exibição do Razor e use o operador `nameof`:</span><span class="sxs-lookup"><span data-stu-id="fc859-240">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a><span data-ttu-id="fc859-241">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="fc859-241">Additional resources</span></span>

* [<span data-ttu-id="fc859-242">Injeção de dependência em exibições</span><span class="sxs-lookup"><span data-stu-id="fc859-242">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
