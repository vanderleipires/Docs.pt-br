---
title: "Componentes do modo de exibição"
author: rick-anderson
description: "Componentes do modo de exibição destinam-se em qualquer lugar que a lógica de processamento reutilizáveis."
keywords: "ASP.NET Core, componentes do modo de exibição, exibição parcial"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: ab4705b7-59d7-4f31-bc97-ea7f292fe926
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: 3bc6d3f85d8ea7fb9b72b18cfd9c5ec2d07293b0
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2017
---
# <a name="view-components"></a><span data-ttu-id="8c19a-104">Componentes do modo de exibição</span><span class="sxs-lookup"><span data-stu-id="8c19a-104">View components</span></span>

<span data-ttu-id="8c19a-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8c19a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8c19a-106">[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8c19a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introducing-view-components"></a><span data-ttu-id="8c19a-107">Introdução ao exibir componentes</span><span class="sxs-lookup"><span data-stu-id="8c19a-107">Introducing view components</span></span>

<span data-ttu-id="8c19a-108">Novo para ASP.NET MVC de núcleo, exiba os componentes são semelhantes às exibições parciais, mas eles são muito mais eficientes.</span><span class="sxs-lookup"><span data-stu-id="8c19a-108">New to ASP.NET Core MVC, view components are similar to partial views, but they are much more powerful.</span></span> <span data-ttu-id="8c19a-109">Componentes do modo de exibição não usar a associação de modelo e apenas dependem dos dados que você fornecer ao chamar nela.</span><span class="sxs-lookup"><span data-stu-id="8c19a-109">View components don’t use model binding, and only depend on the data you provide when calling into it.</span></span> <span data-ttu-id="8c19a-110">Um componente do modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="8c19a-110">A view component:</span></span>

* <span data-ttu-id="8c19a-111">Renderiza um bloco em vez de uma resposta inteira</span><span class="sxs-lookup"><span data-stu-id="8c19a-111">Renders a chunk rather than a whole response</span></span>
* <span data-ttu-id="8c19a-112">Inclui as mesmas separação de preocupações e os benefícios de capacidade de teste encontrados entre um controlador e o modo de exibição</span><span class="sxs-lookup"><span data-stu-id="8c19a-112">Includes the same separation-of-concerns and testability benefits found between a controller and view</span></span>
* <span data-ttu-id="8c19a-113">Pode ter parâmetros e lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="8c19a-113">Can have parameters and business logic</span></span>
* <span data-ttu-id="8c19a-114">É geralmente chamado de uma página de layout</span><span class="sxs-lookup"><span data-stu-id="8c19a-114">Is typically invoked from a layout page</span></span>

<span data-ttu-id="8c19a-115">Componentes de exibição destinam-se em qualquer lugar que a lógica de processamento reutilizável que é muito complexa para uma exibição parcial, como:</span><span class="sxs-lookup"><span data-stu-id="8c19a-115">View components are intended anywhere you have reusable rendering logic that is too complex for a partial view, such as:</span></span>

* <span data-ttu-id="8c19a-116">Menus de navegação dinâmica</span><span class="sxs-lookup"><span data-stu-id="8c19a-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="8c19a-117">Nuvem de marcas (onde ele consulta o banco de dados)</span><span class="sxs-lookup"><span data-stu-id="8c19a-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="8c19a-118">Painel de logon</span><span class="sxs-lookup"><span data-stu-id="8c19a-118">Login panel</span></span>
* <span data-ttu-id="8c19a-119">Carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="8c19a-119">Shopping cart</span></span>
* <span data-ttu-id="8c19a-120">Recentemente artigos publicados</span><span class="sxs-lookup"><span data-stu-id="8c19a-120">Recently published articles</span></span>
* <span data-ttu-id="8c19a-121">Conteúdo da barra lateral em um blog típico</span><span class="sxs-lookup"><span data-stu-id="8c19a-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="8c19a-122">Um painel de logon que deve ser renderizado em cada página e mostra links para fazer logoff ou de log, dependendo do estado do usuário de login</span><span class="sxs-lookup"><span data-stu-id="8c19a-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="8c19a-123">Um componente do modo de exibição consiste em duas partes: a classe (normalmente é derivado de [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) e o resultado retorna (normalmente um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="8c19a-123">A view component consists of two parts: the class (typically derived from [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="8c19a-124">Controladores, um componente do modo de exibição pode ser um POCO, mas a maioria dos desenvolvedores deseja aproveitar os métodos e propriedades disponíveis derivando de `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="8c19a-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="8c19a-125">Criando um componente de exibição</span><span class="sxs-lookup"><span data-stu-id="8c19a-125">Creating a view component</span></span>

<span data-ttu-id="8c19a-126">Esta seção contém os requisitos de alto nível para criar um componente de visualização.</span><span class="sxs-lookup"><span data-stu-id="8c19a-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="8c19a-127">Posteriormente neste artigo, vamos examinar cada etapa e criar um componente de visualização.</span><span class="sxs-lookup"><span data-stu-id="8c19a-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="8c19a-128">A classe de componente do modo de exibição</span><span class="sxs-lookup"><span data-stu-id="8c19a-128">The view component class</span></span>

<span data-ttu-id="8c19a-129">Uma classe de componente do modo de exibição pode ser criada por qualquer um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="8c19a-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="8c19a-130">Derivando de *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="8c19a-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="8c19a-131">Decoração de uma classe com o `[ViewComponent]` atributo ou derivado de uma classe com o `[ViewComponent]` atributo</span><span class="sxs-lookup"><span data-stu-id="8c19a-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="8c19a-132">Criando uma classe em que o nome termina com o sufixo *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="8c19a-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="8c19a-133">Como controladores, componentes de exibição devem ser públicos não aninhados, classes e não-abstrato.</span><span class="sxs-lookup"><span data-stu-id="8c19a-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="8c19a-134">O nome do componente de exibição é o nome da classe com o sufixo "ViewComponent" removido.</span><span class="sxs-lookup"><span data-stu-id="8c19a-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="8c19a-135">Ele também pode ser explicitamente especificado usando o `ViewComponentAttribute.Name` propriedade.</span><span class="sxs-lookup"><span data-stu-id="8c19a-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="8c19a-136">Uma classe de componente do modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="8c19a-136">A view component class:</span></span>

* <span data-ttu-id="8c19a-137">Dá suporte total ao construtor [injeção de dependência](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="8c19a-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="8c19a-138">Não faz parte do ciclo de vida de controlador, o que significa que você não pode usar [filtros](../controllers/filters.md) em um componente do modo de exibição</span><span class="sxs-lookup"><span data-stu-id="8c19a-138">Does not take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="8c19a-139">Métodos de componente do modo de exibição</span><span class="sxs-lookup"><span data-stu-id="8c19a-139">View component methods</span></span>

<span data-ttu-id="8c19a-140">Um componente de visualização define a lógica em uma `InvokeAsync` método que retorna um `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="8c19a-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="8c19a-141">Parâmetros vir diretamente da invocação do componente de exibição, não da associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="8c19a-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="8c19a-142">Um componente de visualização nunca diretamente manipula uma solicitação.</span><span class="sxs-lookup"><span data-stu-id="8c19a-142">A view component never directly handles a request.</span></span> <span data-ttu-id="8c19a-143">Normalmente, um componente de visualização inicializa um modelo e passá-lo para um modo de exibição, chamando o `View` método.</span><span class="sxs-lookup"><span data-stu-id="8c19a-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="8c19a-144">Em resumo, exiba os métodos de componente:</span><span class="sxs-lookup"><span data-stu-id="8c19a-144">In summary, view component methods:</span></span>

* <span data-ttu-id="8c19a-145">Definir um `InvokeAsync` método que retorna um`IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="8c19a-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="8c19a-146">Inicializa um modelo normalmente e passá-lo para um modo de exibição, chamando o `ViewComponent` `View` método</span><span class="sxs-lookup"><span data-stu-id="8c19a-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="8c19a-147">Parâmetros são provenientes do método de chamada, não HTTP, não há nenhuma associação de modelo</span><span class="sxs-lookup"><span data-stu-id="8c19a-147">Parameters come from the calling method, not HTTP, there is no model binding</span></span>
* <span data-ttu-id="8c19a-148">São não pode ser acessado diretamente como um ponto de extremidade HTTP, eles são chamados de seu código (normalmente em um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="8c19a-148">Are not reachable directly as an HTTP endpoint, they are invoked from your code (usually in a view).</span></span> <span data-ttu-id="8c19a-149">Um componente de visualização nunca manipula uma solicitação</span><span class="sxs-lookup"><span data-stu-id="8c19a-149">A view component never handles a request</span></span>
* <span data-ttu-id="8c19a-150">Estão sobrecarregados a assinatura em vez de todos os detalhes da solicitação HTTP atual</span><span class="sxs-lookup"><span data-stu-id="8c19a-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="8c19a-151">Caminho de pesquisa de modo de exibição</span><span class="sxs-lookup"><span data-stu-id="8c19a-151">View search path</span></span>

<span data-ttu-id="8c19a-152">Pesquisa o tempo de execução para o modo de exibição nos seguintes caminhos:</span><span class="sxs-lookup"><span data-stu-id="8c19a-152">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="8c19a-153">Modos de exibição /\<controller_name > /Components/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="8c19a-153">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="8c19a-154">Modos de exibição/Shared/componentes/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="8c19a-154">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="8c19a-155">O nome de exibição padrão para um componente de visualização é *padrão*, que significa que o arquivo de exibição geralmente será nomeado *cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8c19a-155">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="8c19a-156">Você pode especificar um nome de exibição diferente ao criar o resultado de componente do modo de exibição ou ao chamar o `View` método.</span><span class="sxs-lookup"><span data-stu-id="8c19a-156">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="8c19a-157">Recomendamos que você nomear o arquivo de exibição *cshtml* e usar o *compartilhado/exibições/componentes/\<view_component_name > /\<view_name >* caminho.</span><span class="sxs-lookup"><span data-stu-id="8c19a-157">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="8c19a-158">O `PriorityList` usa o componente de exibição usado neste exemplo *Views/Shared/Components/PriorityList/Default.cshtml* para o modo de exibição de componente de exibição.</span><span class="sxs-lookup"><span data-stu-id="8c19a-158">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="8c19a-159">Invocar um componente de visualização</span><span class="sxs-lookup"><span data-stu-id="8c19a-159">Invoking a view component</span></span>

<span data-ttu-id="8c19a-160">Para usar o componente de exibição, chame o seguinte em um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="8c19a-160">To use the view component, call the following inside a view:</span></span>

```html
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="8c19a-161">Os parâmetros serão passados para o `InvokeAsync` método.</span><span class="sxs-lookup"><span data-stu-id="8c19a-161">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="8c19a-162">O `PriorityList` exibição desenvolvidos no artigo é chamado da *Views/Todo/Index.cshtml* o arquivo de exibição.</span><span class="sxs-lookup"><span data-stu-id="8c19a-162">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="8c19a-163">A seguir, o `InvokeAsync` método for chamado com dois parâmetros:</span><span class="sxs-lookup"><span data-stu-id="8c19a-163">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="8c19a-164">Invocar um componente do modo de exibição como um auxiliar de marca</span><span class="sxs-lookup"><span data-stu-id="8c19a-164">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="8c19a-165">Para o ASP.NET Core 1.1 e superior, você pode invocar um componente do modo de exibição como uma [auxiliar de marca](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="8c19a-165">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="8c19a-166">Parâmetros de classe e método de maiusculas e minúsculas de Pascal para os auxiliares de marca são convertidos em seus [inferior caso kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="8c19a-166">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="8c19a-167">O auxiliar de marca para invocar um componente de exibição usa o `<vc></vc>` elemento.</span><span class="sxs-lookup"><span data-stu-id="8c19a-167">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="8c19a-168">O componente de exibição é especificado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="8c19a-168">The view component is specified as follows:</span></span>

```html
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="8c19a-169">Observação: Para usar um componente do modo de exibição como um auxiliar de marca, você deve registrar o assembly que contém o componente de exibição usando o `@addTagHelper` diretiva.</span><span class="sxs-lookup"><span data-stu-id="8c19a-169">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="8c19a-170">Por exemplo, se o seu componente de exibição está em um assembly chamado "MyWebApp", adicione a seguinte diretiva para o `_ViewImports.cshtml` arquivo:</span><span class="sxs-lookup"><span data-stu-id="8c19a-170">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```csharp
@addTagHelper *, MyWebApp
```

<span data-ttu-id="8c19a-171">Você pode registrar um componente de visualização como um auxiliar de marca para qualquer arquivo que referencia o componente de exibição.</span><span class="sxs-lookup"><span data-stu-id="8c19a-171">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="8c19a-172">Consulte [gerenciamento de escopo do auxiliar de marca](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) para obter mais informações sobre como registrar os auxiliares de marcação.</span><span class="sxs-lookup"><span data-stu-id="8c19a-172">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="8c19a-173">O `InvokeAsync` método usado neste tutorial:</span><span class="sxs-lookup"><span data-stu-id="8c19a-173">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="8c19a-174">Na marcação do auxiliar de marca:</span><span class="sxs-lookup"><span data-stu-id="8c19a-174">In Tag Helper markup:</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="8c19a-175">No exemplo acima, o `PriorityList` torna-se o componente de exibição `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="8c19a-175">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="8c19a-176">Os parâmetros para o componente de exibição são passados como atributos em letras minúsculas kebab.</span><span class="sxs-lookup"><span data-stu-id="8c19a-176">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="8c19a-177">Invocar um componente de visualização diretamente de um controlador</span><span class="sxs-lookup"><span data-stu-id="8c19a-177">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="8c19a-178">Componentes do modo de exibição normalmente são invocados em uma exibição, mas você pode invocá-los diretamente a partir de um método do controlador.</span><span class="sxs-lookup"><span data-stu-id="8c19a-178">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="8c19a-179">Enquanto os componentes do modo de exibição não definir pontos de extremidade como controladores, você pode implementar facilmente uma ação do controlador que retorna o conteúdo de um `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="8c19a-179">While view components do not define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="8c19a-180">Neste exemplo, o componente de exibição é chamado diretamente do controlador:</span><span class="sxs-lookup"><span data-stu-id="8c19a-180">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="8c19a-181">Passo a passo: Criando um componente de exibição simples</span><span class="sxs-lookup"><span data-stu-id="8c19a-181">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="8c19a-182">[Baixar](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compilar e testar o código inicial.</span><span class="sxs-lookup"><span data-stu-id="8c19a-182">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="8c19a-183">É um projeto simple com um `Todo` controlador que exibe uma lista de *Todo* itens.</span><span class="sxs-lookup"><span data-stu-id="8c19a-183">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Lista de ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="8c19a-185">Adicionar uma classe ViewComponent</span><span class="sxs-lookup"><span data-stu-id="8c19a-185">Add a ViewComponent class</span></span>

<span data-ttu-id="8c19a-186">Criar um *ViewComponents* pasta e adicione o seguinte `PriorityListViewComponent` classe:</span><span class="sxs-lookup"><span data-stu-id="8c19a-186">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="8c19a-187">Observações sobre o código:</span><span class="sxs-lookup"><span data-stu-id="8c19a-187">Notes on the code:</span></span>

* <span data-ttu-id="8c19a-188">Classes de componente do modo de exibição podem ser contidos em **qualquer** pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="8c19a-188">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="8c19a-189">Porque o nome da classe PriorityList**ViewComponent** termina com o sufixo **ViewComponent**, o tempo de execução usará a cadeia de caracteres "PriorityList" ao referenciar o componente de classe de um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="8c19a-189">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="8c19a-190">Explicarei que em mais detalhes posteriormente.</span><span class="sxs-lookup"><span data-stu-id="8c19a-190">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="8c19a-191">O `[ViewComponent]` atributo pode alterar o nome usado para fazer referência a um componente de visualização.</span><span class="sxs-lookup"><span data-stu-id="8c19a-191">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="8c19a-192">Por exemplo, pode ter dado a classe `XYZ` e aplicadas a `ViewComponent` atributo:</span><span class="sxs-lookup"><span data-stu-id="8c19a-192">For example, we could have named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="8c19a-193">O `[ViewComponent]` atributo acima informa o seletor de componente do modo de exibição para usar o nome `PriorityList` ao procurar os modos de exibição associados com o componente e como usar a cadeia de caracteres "PriorityList" ao referenciar o componente de classe de um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="8c19a-193">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="8c19a-194">Explicarei que em mais detalhes posteriormente.</span><span class="sxs-lookup"><span data-stu-id="8c19a-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="8c19a-195">O componente usa [injeção de dependência](../../fundamentals/dependency-injection.md) para disponibilizar o contexto de dados.</span><span class="sxs-lookup"><span data-stu-id="8c19a-195">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="8c19a-196">`InvokeAsync`expõe um método que pode ser chamado de um modo de exibição e pode levar um número arbitrário de argumentos.</span><span class="sxs-lookup"><span data-stu-id="8c19a-196">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="8c19a-197">O `InvokeAsync` método retorna o conjunto de `ToDo` itens que atendem a `isDone` e `maxPriority` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="8c19a-197">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="8c19a-198">Criar o modo de exibição do Razor de componente do modo de exibição</span><span class="sxs-lookup"><span data-stu-id="8c19a-198">Create the view component Razor view</span></span>

* <span data-ttu-id="8c19a-199">Criar o *compartilhado/exibições/componentes* pasta.</span><span class="sxs-lookup"><span data-stu-id="8c19a-199">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="8c19a-200">Essa pasta **deve** nomeado *componentes*.</span><span class="sxs-lookup"><span data-stu-id="8c19a-200">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="8c19a-201">Criar o *exibições/compartilhado/componentes/PriorityList* pasta.</span><span class="sxs-lookup"><span data-stu-id="8c19a-201">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="8c19a-202">Esse nome de pasta deve corresponder o nome da classe de componente do modo de exibição ou o nome da classe menos o sufixo (se seguido convenção e usado o *ViewComponent* sufixo no nome de classe).</span><span class="sxs-lookup"><span data-stu-id="8c19a-202">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="8c19a-203">Se você usou o `ViewComponent` atributo, o nome da classe precisa corresponder a designação de atributo.</span><span class="sxs-lookup"><span data-stu-id="8c19a-203">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="8c19a-204">Criar um *Views/Shared/Components/PriorityList/Default.cshtml* exibição Razor:[!code-html[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="8c19a-204">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-html[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="8c19a-205">O modo de exibição Razor usa uma lista de `TodoItem` e os exibe.</span><span class="sxs-lookup"><span data-stu-id="8c19a-205">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="8c19a-206">Se o componente de exibição `InvokeAsync` método não passa o nome do modo de exibição (como em nosso exemplo), *padrão* é usado para o nome de exibição por convenção.</span><span class="sxs-lookup"><span data-stu-id="8c19a-206">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="8c19a-207">Posteriormente no tutorial, mostrarei como passar o nome da exibição.</span><span class="sxs-lookup"><span data-stu-id="8c19a-207">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="8c19a-208">Para substituir o estilo padrão para um controlador específico, adicionar um modo de exibição para a pasta de exibição do controlador específico (por exemplo *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="8c19a-208">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="8c19a-209">Se o componente de exibição é específico de controlador, você pode adicioná-lo para a pasta do controlador específico (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="8c19a-209">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="8c19a-210">Adicionar um `div` que contém uma chamada para o componente de lista de prioridade para a parte inferior da *Views/Todo/index.cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="8c19a-210">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="8c19a-211">A marcação `@await Component.InvokeAsync` mostra a sintaxe para chamar componentes do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="8c19a-211">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="8c19a-212">O primeiro argumento é o nome do componente que você deseja invocar ou chamar.</span><span class="sxs-lookup"><span data-stu-id="8c19a-212">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="8c19a-213">Os parâmetros subsequentes são passados para o componente.</span><span class="sxs-lookup"><span data-stu-id="8c19a-213">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="8c19a-214">`InvokeAsync`pode levar a um número arbitrário de argumentos.</span><span class="sxs-lookup"><span data-stu-id="8c19a-214">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="8c19a-215">Teste o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8c19a-215">Test the app.</span></span> <span data-ttu-id="8c19a-216">A imagem a seguir mostra a lista de tarefas e os itens de prioridade:</span><span class="sxs-lookup"><span data-stu-id="8c19a-216">The following image shows the ToDo list and the priority items:</span></span>

![itens de lista e prioridade de tarefas](view-components/_static/pi.png)

<span data-ttu-id="8c19a-218">Você também pode chamar o componente Exibir diretamente do controlador:</span><span class="sxs-lookup"><span data-stu-id="8c19a-218">You can also call the view component directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![itens de prioridade da ação IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="8c19a-220">Especificando um nome de exibição</span><span class="sxs-lookup"><span data-stu-id="8c19a-220">Specifying a view name</span></span>

<span data-ttu-id="8c19a-221">Um componente de visualização complexo talvez seja necessário especificar um modo de exibição não padrão em algumas condições.</span><span class="sxs-lookup"><span data-stu-id="8c19a-221">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="8c19a-222">O código a seguir mostra como especificar o modo de exibição de "PVC" o `InvokeAsync` método.</span><span class="sxs-lookup"><span data-stu-id="8c19a-222">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="8c19a-223">Atualização de `InvokeAsync` método o `PriorityListViewComponent` classe.</span><span class="sxs-lookup"><span data-stu-id="8c19a-223">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="8c19a-224">Copie o *Views/Shared/Components/PriorityList/Default.cshtml* arquivo para uma exibição nomeada *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8c19a-224">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="8c19a-225">Adicione um título para indicar que o modo de exibição de PVC está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="8c19a-225">Add a heading to indicate the PVC view is being used.</span></span>

[!code-html[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="8c19a-226">Atualização *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8c19a-226">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="8c19a-227">Executar o aplicativo e verifique se o modo de exibição de PVC.</span><span class="sxs-lookup"><span data-stu-id="8c19a-227">Run the app and verify PVC view.</span></span>

![Componente de visualização de prioridade](view-components/_static/pvc.png)

<span data-ttu-id="8c19a-229">Se o modo de exibição de PVC não é processado, verifique se que você estiver chamando o componente de exibição com uma prioridade de 4 ou superior.</span><span class="sxs-lookup"><span data-stu-id="8c19a-229">If the PVC view is not rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="8c19a-230">Examine o caminho de exibição</span><span class="sxs-lookup"><span data-stu-id="8c19a-230">Examine the view path</span></span>

* <span data-ttu-id="8c19a-231">Altere o parâmetro de prioridade para três ou menos para que o modo de exibição de prioridade não é retornado.</span><span class="sxs-lookup"><span data-stu-id="8c19a-231">Change the priority parameter to three or less so the priority view is not returned.</span></span>
* <span data-ttu-id="8c19a-232">Renomeie temporariamente o *Views/Todo/Components/PriorityList/Default.cshtml* para *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8c19a-232">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="8c19a-233">Teste o aplicativo, você obterá o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="8c19a-233">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="8c19a-234">Cópia *Views/Todo/Components/PriorityList/1Default.cshtml* para *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8c19a-234">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="8c19a-235">Adicionar algumas marcações para o *compartilhado* exibição de componente de exibição de tarefas para indicar o modo de exibição é do *compartilhado* pasta.</span><span class="sxs-lookup"><span data-stu-id="8c19a-235">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="8c19a-236">Teste o **compartilhado** exibição do componente.</span><span class="sxs-lookup"><span data-stu-id="8c19a-236">Test the **Shared** component view.</span></span>

![Saída de tarefas com a exibição do componente compartilhado](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="8c19a-238">Evitando magic cadeias de caracteres</span><span class="sxs-lookup"><span data-stu-id="8c19a-238">Avoiding magic strings</span></span>

<span data-ttu-id="8c19a-239">Se você quiser que a segurança de tempo de compilação, você pode substituir o nome do componente de exibição embutida com o nome da classe.</span><span class="sxs-lookup"><span data-stu-id="8c19a-239">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="8c19a-240">Crie o componente de exibição sem o sufixo "ViewComponent":</span><span class="sxs-lookup"><span data-stu-id="8c19a-240">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="8c19a-241">Adicionar um `using` instrução para o Razor Exibir arquivo e use o `nameof` operador:</span><span class="sxs-lookup"><span data-stu-id="8c19a-241">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a><span data-ttu-id="8c19a-242">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="8c19a-242">Additional Resources</span></span>

* [<span data-ttu-id="8c19a-243">Injeção de dependência em exibições</span><span class="sxs-lookup"><span data-stu-id="8c19a-243">Dependency injection into views</span></span>](dependency-injection.md)
