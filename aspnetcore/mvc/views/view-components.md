---
title: Componentes de exibição no ASP.NET Core
author: rick-anderson
description: Saiba como os componentes de exibição são usados no ASP.NET Core e como adicioná-los aos aplicativos.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: cdf44fc15ac64497b2589e8b7b289beb94c5b2c4
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962678"
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="0471e-103">Componentes de exibição no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0471e-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="0471e-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0471e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0471e-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0471e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="view-components"></a><span data-ttu-id="0471e-106">Componentes da exibição</span><span class="sxs-lookup"><span data-stu-id="0471e-106">View components</span></span>

<span data-ttu-id="0471e-107">Os componentes de exibição são semelhantes às exibições parciais, mas são muito mais eficientes.</span><span class="sxs-lookup"><span data-stu-id="0471e-107">View components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="0471e-108">Os componentes de exibição não usam a associação de modelos e dependem apenas dos dados fornecidos durante uma chamada a eles.</span><span class="sxs-lookup"><span data-stu-id="0471e-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="0471e-109">Este artigo foi escrito com o ASP.NET Core MVC, mas os componentes de exibição também funcionam com Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="0471e-109">This article was written using ASP.NET Core MVC, but view components also work with Razor Pages.</span></span>

<span data-ttu-id="0471e-110">Um componente de exibição:</span><span class="sxs-lookup"><span data-stu-id="0471e-110">A view component:</span></span>

* <span data-ttu-id="0471e-111">Renderiza uma parte em vez de uma resposta inteira.</span><span class="sxs-lookup"><span data-stu-id="0471e-111">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="0471e-112">Inclui os mesmos benefícios de capacidade de teste e separação de interesses e encontrados entre um controlador e uma exibição.</span><span class="sxs-lookup"><span data-stu-id="0471e-112">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="0471e-113">Pode ter parâmetros e uma lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="0471e-113">Can have parameters and business logic.</span></span>
* <span data-ttu-id="0471e-114">É geralmente invocado em uma página de layout.</span><span class="sxs-lookup"><span data-stu-id="0471e-114">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="0471e-115">Os componentes de exibição destinam-se a qualquer momento em que há uma lógica de renderização reutilizável muito complexa para uma exibição parcial, como:</span><span class="sxs-lookup"><span data-stu-id="0471e-115">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="0471e-116">Menus de navegação dinâmica</span><span class="sxs-lookup"><span data-stu-id="0471e-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="0471e-117">Nuvem de marcas (na qual ela consulta o banco de dados)</span><span class="sxs-lookup"><span data-stu-id="0471e-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="0471e-118">Painel de logon</span><span class="sxs-lookup"><span data-stu-id="0471e-118">Login panel</span></span>
* <span data-ttu-id="0471e-119">Carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="0471e-119">Shopping cart</span></span>
* <span data-ttu-id="0471e-120">Artigos publicados recentemente</span><span class="sxs-lookup"><span data-stu-id="0471e-120">Recently published articles</span></span>
* <span data-ttu-id="0471e-121">Conteúdo da barra lateral em um blog típico</span><span class="sxs-lookup"><span data-stu-id="0471e-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="0471e-122">Um painel de logon que é renderizado em cada página e mostra os links para logoff ou logon, dependendo do estado de logon do usuário</span><span class="sxs-lookup"><span data-stu-id="0471e-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="0471e-123">Um componente de exibição consiste em duas partes: a classe (normalmente derivada de [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) e o resultado que ele retorna (normalmente, uma exibição).</span><span class="sxs-lookup"><span data-stu-id="0471e-123">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="0471e-124">Assim como os controladores, um componente de exibição pode ser um POCO, mas a maioria dos desenvolvedores desejará aproveitar os métodos e as propriedades disponíveis com a derivação de `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="0471e-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="0471e-125">Criando um componente de exibição</span><span class="sxs-lookup"><span data-stu-id="0471e-125">Creating a view component</span></span>

<span data-ttu-id="0471e-126">Esta seção contém os requisitos de alto nível para a criação de um componente de exibição.</span><span class="sxs-lookup"><span data-stu-id="0471e-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="0471e-127">Mais adiante neste artigo, examinaremos cada etapa em detalhes e criaremos um componente de exibição.</span><span class="sxs-lookup"><span data-stu-id="0471e-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="0471e-128">A classe de componente de exibição</span><span class="sxs-lookup"><span data-stu-id="0471e-128">The view component class</span></span>

<span data-ttu-id="0471e-129">Uma classe de componente de exibição pode ser criada por um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="0471e-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="0471e-130">Derivação de *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="0471e-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="0471e-131">Decoração de uma classe com o atributo `[ViewComponent]` ou derivação de uma classe com o atributo `[ViewComponent]`</span><span class="sxs-lookup"><span data-stu-id="0471e-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="0471e-132">Criação de uma classe em que o nome termina com o sufixo *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="0471e-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="0471e-133">Assim como os controladores, os componentes de exibição precisam ser classes públicas, não aninhadas e não abstratas.</span><span class="sxs-lookup"><span data-stu-id="0471e-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="0471e-134">O nome do componente de exibição é o nome da classe com o sufixo "ViewComponent" removido.</span><span class="sxs-lookup"><span data-stu-id="0471e-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="0471e-135">Também pode ser especificado de forma explícita com a propriedade `ViewComponentAttribute.Name`.</span><span class="sxs-lookup"><span data-stu-id="0471e-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="0471e-136">Uma classe de componente de exibição:</span><span class="sxs-lookup"><span data-stu-id="0471e-136">A view component class:</span></span>

* <span data-ttu-id="0471e-137">Dá suporte total à [injeção de dependência do construtor](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="0471e-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="0471e-138">Não participa do ciclo de vida do controlador, o que significa que não é possível usar [filtros](../controllers/filters.md) em um componente de exibição</span><span class="sxs-lookup"><span data-stu-id="0471e-138">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="0471e-139">Métodos de componente de exibição</span><span class="sxs-lookup"><span data-stu-id="0471e-139">View component methods</span></span>

<span data-ttu-id="0471e-140">Um componente de exibição define sua lógica em um método `InvokeAsync` que retorna um `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="0471e-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="0471e-141">Os parâmetros são recebidos diretamente da invocação do componente de exibição, não da associação de modelos.</span><span class="sxs-lookup"><span data-stu-id="0471e-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="0471e-142">Um componente de exibição nunca manipula uma solicitação diretamente.</span><span class="sxs-lookup"><span data-stu-id="0471e-142">A view component never directly handles a request.</span></span> <span data-ttu-id="0471e-143">Normalmente, um componente de exibição inicializa um modelo e passa-o para uma exibição chamando o método `View`.</span><span class="sxs-lookup"><span data-stu-id="0471e-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="0471e-144">Em resumo, os métodos de componente de exibição:</span><span class="sxs-lookup"><span data-stu-id="0471e-144">In summary, view component methods:</span></span>

* <span data-ttu-id="0471e-145">Definem um método `InvokeAsync` que retorna um `IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="0471e-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="0471e-146">Normalmente, inicializam um modelo e passam-o para uma exibição chamando o método `ViewComponent` `View`</span><span class="sxs-lookup"><span data-stu-id="0471e-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="0471e-147">Os parâmetros são recebidos do método de chamada, não do HTTP e não há nenhuma associação de modelos</span><span class="sxs-lookup"><span data-stu-id="0471e-147">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="0471e-148">Não podem ser acessados diretamente como um ponto de extremidade HTTP e são invocados no código (normalmente em uma exibição).</span><span class="sxs-lookup"><span data-stu-id="0471e-148">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="0471e-149">Um componente de exibição nunca manipula uma solicitação</span><span class="sxs-lookup"><span data-stu-id="0471e-149">A view component never handles a request</span></span>
* <span data-ttu-id="0471e-150">São sobrecarregados na assinatura, em vez de nos detalhes da solicitação HTTP atual</span><span class="sxs-lookup"><span data-stu-id="0471e-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="0471e-151">Caminho de pesquisa de exibição</span><span class="sxs-lookup"><span data-stu-id="0471e-151">View search path</span></span>

<span data-ttu-id="0471e-152">O tempo de execução pesquisa a exibição nos seguintes caminhos:</span><span class="sxs-lookup"><span data-stu-id="0471e-152">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="0471e-153">Views/\<nome_do_controlador>/Components/\<nome_do_componente_da_exibição>/\<nome_do_modo_de_exibição></span><span class="sxs-lookup"><span data-stu-id="0471e-153">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="0471e-154">Views/Shared/Components/\<nome_do_componente_da_exibição>/\<nome_do_modo_de_exibição></span><span class="sxs-lookup"><span data-stu-id="0471e-154">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="0471e-155">O nome de exibição padrão de um componente de exibição é *Default*, o que significa que o arquivo de exibição geralmente será nomeado *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0471e-155">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="0471e-156">Especifique outro nome de exibição ao criar o resultado do componente de exibição ou ao chamar o método `View`.</span><span class="sxs-lookup"><span data-stu-id="0471e-156">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="0471e-157">Recomendamos que você nomeie o arquivo de exibição *Default.cshtml* e use o caminho *Views/Shared/Components/\<nome_do_componente_da_exibição>/\<nome_do_modo_de_exibição>*.</span><span class="sxs-lookup"><span data-stu-id="0471e-157">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="0471e-158">O componente de exibição `PriorityList` usado nesta amostra usa *Views/Shared/Components/PriorityList/Default.cshtml* como a exibição do componente de exibição.</span><span class="sxs-lookup"><span data-stu-id="0471e-158">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="0471e-159">Invocando um componente de exibição</span><span class="sxs-lookup"><span data-stu-id="0471e-159">Invoking a view component</span></span>

<span data-ttu-id="0471e-160">Para usar o componente de exibição, chame o seguinte em uma exibição:</span><span class="sxs-lookup"><span data-stu-id="0471e-160">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="0471e-161">Os parâmetros serão passados para o método `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="0471e-161">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="0471e-162">O componente de exibição `PriorityList` desenvolvido no artigo é invocado por meio do arquivo de exibição *Views/Todo/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0471e-162">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="0471e-163">A seguir, o método `InvokeAsync` é chamado com dois parâmetros:</span><span class="sxs-lookup"><span data-stu-id="0471e-163">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="0471e-164">Invocando um componente de exibição como um Auxiliar de Marca</span><span class="sxs-lookup"><span data-stu-id="0471e-164">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="0471e-165">Para o ASP.NET Core 1.1 e superior, invoque um componente de exibição como um [Auxiliar de Marca](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="0471e-165">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="0471e-166">Os parâmetros de classe e de método na formatação Pascal Case para Auxiliares de Marca são convertidos em seu [kebab case em minúsculas](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="0471e-166">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="0471e-167">Para invocar um componente de exibição, o Auxiliar de Marca usa o elemento `<vc></vc>`.</span><span class="sxs-lookup"><span data-stu-id="0471e-167">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="0471e-168">O componente de exibição é especificado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0471e-168">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="0471e-169">Observação: para usar um Componente de Exibição como um Auxiliar de Marca, é necessário registrar o assembly que contém o Componente de Exibição usando a diretiva `@addTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="0471e-169">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="0471e-170">Por exemplo, se o Componente de Exibição está em um assembly chamado "MyWebApp", adicione a seguinte diretiva ao arquivo `_ViewImports.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="0471e-170">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="0471e-171">Registre um Componente de Exibição como um Auxiliar de Marca em qualquer arquivo que referencia o Componente de Exibição.</span><span class="sxs-lookup"><span data-stu-id="0471e-171">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="0471e-172">Consulte [Gerenciando o escopo do Auxiliar de Marca](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) para obter mais informações sobre como registrar Auxiliares de Marca.</span><span class="sxs-lookup"><span data-stu-id="0471e-172">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="0471e-173">O método `InvokeAsync` usado neste tutorial:</span><span class="sxs-lookup"><span data-stu-id="0471e-173">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="0471e-174">Na marcação do Auxiliar de Marca:</span><span class="sxs-lookup"><span data-stu-id="0471e-174">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="0471e-175">Na amostra acima, o componente de exibição `PriorityList` torna-se `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="0471e-175">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="0471e-176">Os parâmetros para o componente de exibição são passados como atributos em kebab case em minúsculas.</span><span class="sxs-lookup"><span data-stu-id="0471e-176">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="0471e-177">Invocando um componente de exibição diretamente em um controlador</span><span class="sxs-lookup"><span data-stu-id="0471e-177">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="0471e-178">Os componentes de exibição normalmente são invocados em uma exibição, mas você pode invocá-los diretamente em um método do controlador.</span><span class="sxs-lookup"><span data-stu-id="0471e-178">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="0471e-179">Embora os componentes de exibição não definam pontos de extremidade como controladores, você pode implementar com facilidade uma ação do controlador que retorna o conteúdo de um `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="0471e-179">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="0471e-180">Neste exemplo, o componente de exibição é chamado diretamente no controlador:</span><span class="sxs-lookup"><span data-stu-id="0471e-180">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="0471e-181">Passo a passo: criando um componente de exibição simples</span><span class="sxs-lookup"><span data-stu-id="0471e-181">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="0471e-182">[Baixe](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compile e teste o código inicial.</span><span class="sxs-lookup"><span data-stu-id="0471e-182">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="0471e-183">É um projeto simples com um controlador `Todo` que exibe uma lista de itens *Todo*.</span><span class="sxs-lookup"><span data-stu-id="0471e-183">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Lista de ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="0471e-185">Adicionar uma classe ViewComponent</span><span class="sxs-lookup"><span data-stu-id="0471e-185">Add a ViewComponent class</span></span>

<span data-ttu-id="0471e-186">Crie uma pasta *ViewComponents* e adicione a seguinte classe `PriorityListViewComponent`:</span><span class="sxs-lookup"><span data-stu-id="0471e-186">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="0471e-187">Observações sobre o código:</span><span class="sxs-lookup"><span data-stu-id="0471e-187">Notes on the code:</span></span>

* <span data-ttu-id="0471e-188">As classes de componente de exibição podem ser contidas em **qualquer** pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="0471e-188">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="0471e-189">Como o nome da classe PriorityList**ViewComponent** termina com o sufixo **ViewComponent**, o tempo de execução usará a cadeia de caracteres "PriorityList" ao referenciar o componente de classe em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="0471e-189">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="0471e-190">Explicarei isso mais detalhadamente mais adiante.</span><span class="sxs-lookup"><span data-stu-id="0471e-190">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="0471e-191">O atributo `[ViewComponent]` pode alterar o nome usado para referenciar um componente de exibição.</span><span class="sxs-lookup"><span data-stu-id="0471e-191">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="0471e-192">Por exemplo, poderíamos nomear a classe `XYZ` e aplicar o atributo `ViewComponent`:</span><span class="sxs-lookup"><span data-stu-id="0471e-192">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="0471e-193">O atributo `[ViewComponent]` acima instrui o seletor de componente de exibição a usar o nome `PriorityList` ao procurar as exibições associadas ao componente e a usar a cadeia de caracteres "PriorityList" ao referenciar o componente de classe em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="0471e-193">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="0471e-194">Explicarei isso mais detalhadamente mais adiante.</span><span class="sxs-lookup"><span data-stu-id="0471e-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="0471e-195">O componente usa a [injeção de dependência](../../fundamentals/dependency-injection.md) para disponibilizar o contexto de dados.</span><span class="sxs-lookup"><span data-stu-id="0471e-195">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="0471e-196">`InvokeAsync` expõe um método que pode ser chamado em uma exibição e pode usar um número arbitrário de argumentos.</span><span class="sxs-lookup"><span data-stu-id="0471e-196">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="0471e-197">O método `InvokeAsync` retorna o conjunto de itens `ToDo` que atendem aos parâmetros `isDone` e `maxPriority`.</span><span class="sxs-lookup"><span data-stu-id="0471e-197">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="0471e-198">Criar a exibição do Razor do componente de exibição</span><span class="sxs-lookup"><span data-stu-id="0471e-198">Create the view component Razor view</span></span>

* <span data-ttu-id="0471e-199">Crie a pasta *Views/Shared/Components*.</span><span class="sxs-lookup"><span data-stu-id="0471e-199">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="0471e-200">Essa pasta **deve** nomeada *Components*.</span><span class="sxs-lookup"><span data-stu-id="0471e-200">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="0471e-201">Crie a pasta *Views/Shared/Components/PriorityList*.</span><span class="sxs-lookup"><span data-stu-id="0471e-201">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="0471e-202">Esse nome de pasta deve corresponder ao nome da classe do componente de exibição ou ao nome da classe menos o sufixo (se seguimos a convenção e usamos o sufixo *ViewComponent* no nome da classe).</span><span class="sxs-lookup"><span data-stu-id="0471e-202">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="0471e-203">Se você usou o atributo `ViewComponent`, o nome da classe precisa corresponder à designação de atributo.</span><span class="sxs-lookup"><span data-stu-id="0471e-203">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="0471e-204">Crie uma exibição do Razor *Views/Shared/Components/PriorityList/Default.cshtml*: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="0471e-204">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="0471e-205">A exibição do Razor usa uma lista de `TodoItem` e exibe-os.</span><span class="sxs-lookup"><span data-stu-id="0471e-205">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="0471e-206">Se o método `InvokeAsync` do componente de exibição não passar o nome da exibição (como em nossa amostra), *Default* será usado como o nome da exibição, por convenção.</span><span class="sxs-lookup"><span data-stu-id="0471e-206">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="0471e-207">Mais adiante no tutorial, mostrarei como passar o nome da exibição.</span><span class="sxs-lookup"><span data-stu-id="0471e-207">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="0471e-208">Para substituir o estilo padrão de um controlador específico, adicione uma exibição à pasta de exibição específica ao controlador (por exemplo, *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="0471e-208">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="0471e-209">Se o componente de exibição é específico ao controlador, adicione-o à pasta específica ao controlador (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="0471e-209">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="0471e-210">Adicione um `div` que contém uma chamada ao componente da lista de prioridades à parte inferior do arquivo *Views/Todo/index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0471e-210">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="0471e-211">A marcação `@await Component.InvokeAsync` mostra a sintaxe para chamar componentes de exibição.</span><span class="sxs-lookup"><span data-stu-id="0471e-211">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="0471e-212">O primeiro argumento é o nome do componente que queremos invocar ou chamar.</span><span class="sxs-lookup"><span data-stu-id="0471e-212">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="0471e-213">Os parâmetros seguintes são passados para o componente.</span><span class="sxs-lookup"><span data-stu-id="0471e-213">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="0471e-214">`InvokeAsync` pode usar um número arbitrário de argumentos.</span><span class="sxs-lookup"><span data-stu-id="0471e-214">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="0471e-215">Teste o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0471e-215">Test the app.</span></span> <span data-ttu-id="0471e-216">A seguinte imagem mostra a lista ToDo e os itens de prioridade:</span><span class="sxs-lookup"><span data-stu-id="0471e-216">The following image shows the ToDo list and the priority items:</span></span>

![lista de tarefas pendentes e itens de prioridade](view-components/_static/pi.png)

<span data-ttu-id="0471e-218">Também chame o componente de exibição diretamente no controlador:</span><span class="sxs-lookup"><span data-stu-id="0471e-218">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![itens de prioridade da ação IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="0471e-220">Especificando um nome de exibição</span><span class="sxs-lookup"><span data-stu-id="0471e-220">Specifying a view name</span></span>

<span data-ttu-id="0471e-221">Um componente de exibição complexo pode precisar especificar uma exibição não padrão em algumas condições.</span><span class="sxs-lookup"><span data-stu-id="0471e-221">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="0471e-222">O código a seguir mostra como especificar a exibição "PVC" no método `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="0471e-222">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="0471e-223">Atualize o método `InvokeAsync` na classe `PriorityListViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="0471e-223">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="0471e-224">Copie o arquivo *Views/Shared/Components/PriorityList/Default.cshtml* para uma exibição nomeada *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0471e-224">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="0471e-225">Adicione um cabeçalho para indicar que a exibição PVC está sendo usada.</span><span class="sxs-lookup"><span data-stu-id="0471e-225">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="0471e-226">Atualize *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0471e-226">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="0471e-227">Execute o aplicativo e verifique a exibição PVC.</span><span class="sxs-lookup"><span data-stu-id="0471e-227">Run the app and verify PVC view.</span></span>

![Componente de exibição de prioridade](view-components/_static/pvc.png)

<span data-ttu-id="0471e-229">Se a exibição PVC não é renderizada, verifique se você está chamando o componente de exibição com uma prioridade igual a 4 ou superior.</span><span class="sxs-lookup"><span data-stu-id="0471e-229">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="0471e-230">Examinar o caminho de exibição</span><span class="sxs-lookup"><span data-stu-id="0471e-230">Examine the view path</span></span>

* <span data-ttu-id="0471e-231">Altere o parâmetro de prioridade para três ou menos para que a exibição de prioridade não seja retornada.</span><span class="sxs-lookup"><span data-stu-id="0471e-231">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="0471e-232">Renomeie temporariamente o *Views/Todo/Components/PriorityList/Default.cshtml* como *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0471e-232">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="0471e-233">Teste o aplicativo. Você obterá o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="0471e-233">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="0471e-234">Copie *Views/Todo/Components/PriorityList/1Default.cshtml* para *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0471e-234">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="0471e-235">Adicione uma marcação ao componente de exibição Todo *Shared* para indicar que a exibição foi obtida da pasta *Shared*.</span><span class="sxs-lookup"><span data-stu-id="0471e-235">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="0471e-236">Teste o componente de exibição **Shared**.</span><span class="sxs-lookup"><span data-stu-id="0471e-236">Test the **Shared** component view.</span></span>

![Saída de ToDo com o componente de exibição Shared](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="0471e-238">Evitando cadeias de caracteres mágicas</span><span class="sxs-lookup"><span data-stu-id="0471e-238">Avoiding magic strings</span></span>

<span data-ttu-id="0471e-239">Se deseja obter segurança em tempo de compilação, substitua o nome do componente de exibição embutido em código pelo nome da classe.</span><span class="sxs-lookup"><span data-stu-id="0471e-239">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="0471e-240">Crie o componente de exibição sem o sufixo "ViewComponent":</span><span class="sxs-lookup"><span data-stu-id="0471e-240">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="0471e-241">Adicione uma instrução `using` ao arquivo de exibição do Razor e use o operador `nameof`:</span><span class="sxs-lookup"><span data-stu-id="0471e-241">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="additional-resources"></a><span data-ttu-id="0471e-242">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="0471e-242">Additional resources</span></span>

* [<span data-ttu-id="0471e-243">Injeção de dependência em exibições</span><span class="sxs-lookup"><span data-stu-id="0471e-243">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
