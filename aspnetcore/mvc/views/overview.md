---
title: "Modos de exibição no núcleo do ASP.NET MVC"
author: ardalis
description: "Saiba como exibições de lidar com a apresentação de dados do aplicativo e a interação do usuário no ASP.NET MVC de núcleo."
keywords: ASP.NET Core, exibir, MVC, razor, viewmodel, viewbag, viewdata
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 4530d2f500dd887bf649a753283fb3e4af995322
ms.sourcegitcommit: c2f6c593d81fbd90e6ddd672fe0a5636d06b615a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2017
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="5802b-104">Modos de exibição no núcleo do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5802b-104">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="5802b-105">Por [Steve Smith](https://ardalis.com/) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5802b-105">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5802b-106">No **M**odelo -**V**ibir -**C**ontroller (MVC) padrão, o *exibição* trata a interação de usuário e a apresentação de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5802b-106">In the **M**odel-**V**iew-**C**ontroller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="5802b-107">Uma exibição é um modelo HTML com inseridos [marcação Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="5802b-107">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="5802b-108">Marcação Razor é um código que interage com a marcação HTML para produzir uma página da Web que é enviada ao cliente.</span><span class="sxs-lookup"><span data-stu-id="5802b-108">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="5802b-109">No ASP.NET MVC de núcleo, modos de exibição são *. cshtml* arquivos que usam o [linguagem de programação em c#](/dotnet/csharp/) na marcação Razor.</span><span class="sxs-lookup"><span data-stu-id="5802b-109">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="5802b-110">Geralmente, arquivos de exibição são agrupados em pastas denominadas para cada do aplicativo [controladores](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="5802b-110">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="5802b-111">As pastas são armazenadas em um *exibições* pasta na raiz do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="5802b-111">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Pasta de modos de exibição no Gerenciador de soluções do Visual Studio é aberta com a pasta base aberta para mostrar os arquivos cshtml, Contact.cshtml e About.cshtml](overview/_static/views_solution_explorer.png)

<span data-ttu-id="5802b-113">O *início* controlador é representado por um *início* pasta dentro de *exibições* pasta.</span><span class="sxs-lookup"><span data-stu-id="5802b-113">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="5802b-114">O *início* pasta contém os modos de exibição para o *sobre*, *contato*, e *índice* (home page) páginas da Web.</span><span class="sxs-lookup"><span data-stu-id="5802b-114">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="5802b-115">Quando um usuário solicita uma dessas três páginas da Web, ações do controlador no *início* controlador determinar qual dos três modos de exibição é usado para criar e retornar uma página da Web para o usuário.</span><span class="sxs-lookup"><span data-stu-id="5802b-115">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="5802b-116">Use [layouts](xref:mvc/views/layout) para fornecer seções da página da Web consistente e reduzir as repetições de código.</span><span class="sxs-lookup"><span data-stu-id="5802b-116">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="5802b-117">Layouts geralmente contêm o cabeçalho, elementos de navegação e o menu e o rodapé.</span><span class="sxs-lookup"><span data-stu-id="5802b-117">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="5802b-118">O cabeçalho e rodapé geralmente contêm marcação boilerplate muitos elementos de metadados e links para recursos de script e estilo.</span><span class="sxs-lookup"><span data-stu-id="5802b-118">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="5802b-119">Layouts de ajudam a evitar essa marcação clichê em exibições.</span><span class="sxs-lookup"><span data-stu-id="5802b-119">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="5802b-120">[Exibições parciais](xref:mvc/views/partial) reduzir a duplicação de código por meio do gerenciamento de componentes reutilizáveis de modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="5802b-120">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="5802b-121">Por exemplo, uma exibição parcial é útil para uma biografia do autor em um site de blog que aparece em vários modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="5802b-121">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="5802b-122">Uma biografia do autor é comum exibir conteúdo e não requer código a ser executado para produzir o conteúdo da página da Web.</span><span class="sxs-lookup"><span data-stu-id="5802b-122">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="5802b-123">Conteúdo de biografia do autor está disponível para o modo de exibição usado sozinho, associação de modelo para que usar uma exibição parcial para este tipo de conteúdo é ideal.</span><span class="sxs-lookup"><span data-stu-id="5802b-123">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="5802b-124">[Exibir componentes](xref:mvc/views/view-components) são modos de exibição semelhantes para parcial em que elas permitem que você reduza código repetitivo, mas eles são apropriados para exibir o conteúdo que requer a execução de código do servidor para renderizar a página da Web.</span><span class="sxs-lookup"><span data-stu-id="5802b-124">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="5802b-125">Exiba os componentes são úteis quando o conteúdo renderizado requer interação do banco de dados, como para um site do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="5802b-125">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="5802b-126">Componentes do modo de exibição não são limitados para associação de modelo para produzir saída de página da Web.</span><span class="sxs-lookup"><span data-stu-id="5802b-126">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="5802b-127">Benefícios do uso de exibições</span><span class="sxs-lookup"><span data-stu-id="5802b-127">Benefits of using views</span></span>

<span data-ttu-id="5802b-128">Modos de exibição ajudam a estabelecer um [ **S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) dentro de um aplicativo MVC, separando a marcação de interface de usuário do outras partes do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5802b-128">Views help to establish a [**S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="5802b-129">SoC design a seguir faz com que seu aplicativo modular, que fornece vários benefícios:</span><span class="sxs-lookup"><span data-stu-id="5802b-129">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="5802b-130">O aplicativo é mais fácil manter porque ela foi organizada melhor.</span><span class="sxs-lookup"><span data-stu-id="5802b-130">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="5802b-131">Modos de exibição geralmente são agrupados pelo recurso de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5802b-131">Views are generally grouped by app feature.</span></span> <span data-ttu-id="5802b-132">Isso torna mais fácil de localizar os modos de exibição relacionados ao trabalhar em um recurso.</span><span class="sxs-lookup"><span data-stu-id="5802b-132">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="5802b-133">As partes do aplicativo são acoplados de forma flexível.</span><span class="sxs-lookup"><span data-stu-id="5802b-133">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="5802b-134">Você pode criar e atualizar modos de exibição do aplicativo separadamente dos componentes de acesso de dados e lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="5802b-134">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="5802b-135">Você pode modificar os modos de exibição do aplicativo sem precisar necessariamente atualizar outras partes do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5802b-135">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="5802b-136">É mais fácil de testar as partes da interface de usuário do aplicativo, como os modos de exibição são unidades separadas.</span><span class="sxs-lookup"><span data-stu-id="5802b-136">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="5802b-137">Devido à melhor organização, é menos provável que você vai acidentalmente seções de repetição da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="5802b-137">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="5802b-138">Criar um modo de exibição</span><span class="sxs-lookup"><span data-stu-id="5802b-138">Creating a view</span></span>

<span data-ttu-id="5802b-139">Exibições que são específicas para um controlador são criadas no *modos de exibição / [ControllerName]* pasta.</span><span class="sxs-lookup"><span data-stu-id="5802b-139">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="5802b-140">Modos de exibição que são compartilhados entre os controladores são colocados no *exibições/compartilhadas* pasta.</span><span class="sxs-lookup"><span data-stu-id="5802b-140">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="5802b-141">Para criar um modo de exibição, adicionar um novo arquivo e dê a ele o mesmo nome que sua ação de controlador associado com o *. cshtml* extensão de arquivo.</span><span class="sxs-lookup"><span data-stu-id="5802b-141">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="5802b-142">Para criar uma exibição que corresponde ao *sobre* ação no *início* controlador, crie um *About.cshtml* de arquivos no *exibições/inicial*pasta:</span><span class="sxs-lookup"><span data-stu-id="5802b-142">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="5802b-143">*Razor* marcação começa com o `@` símbolo.</span><span class="sxs-lookup"><span data-stu-id="5802b-143">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="5802b-144">Código de execução c# instruções colocando c# em [blocos de código Razor](xref:mvc/views/razor#razor-code-blocks) definir chaves (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="5802b-144">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="5802b-145">Por exemplo, consulte a atribuição de "Sobre" `ViewData["Title"]` mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="5802b-145">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="5802b-146">Você pode exibir valores em HTML, simplesmente referenciando o valor com o `@` símbolo.</span><span class="sxs-lookup"><span data-stu-id="5802b-146">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="5802b-147">Consulte o conteúdo de `<h2>` e `<h3>` elementos acima.</span><span class="sxs-lookup"><span data-stu-id="5802b-147">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="5802b-148">O conteúdo da exibição mostrado acima é apenas parte da página inteira que é renderizada para o usuário.</span><span class="sxs-lookup"><span data-stu-id="5802b-148">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="5802b-149">O resto do layout da página e outros aspectos comuns do modo de exibição são especificados em outros arquivos de exibição.</span><span class="sxs-lookup"><span data-stu-id="5802b-149">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="5802b-150">Para obter mais informações, consulte o [tópico Layout](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="5802b-150">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="5802b-151">Como os controladores de especificam os modos de exibição</span><span class="sxs-lookup"><span data-stu-id="5802b-151">How controllers specify views</span></span>

<span data-ttu-id="5802b-152">Modos de exibição normalmente retornados por ações como um [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), que é um tipo de [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="5802b-152">Views are typically returned from actions as a [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="5802b-153">O método de ação pode criar e retornar um `ViewResult` diretamente, mas que normalmente não é feito.</span><span class="sxs-lookup"><span data-stu-id="5802b-153">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="5802b-154">Desde que a maioria dos controladores herdam [controlador](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), basta usar o `View` método auxiliar para retornar o `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="5802b-154">Since most controllers inherit from [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="5802b-155">*HomeController*</span><span class="sxs-lookup"><span data-stu-id="5802b-155">*HomeController.cs*</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="5802b-156">Quando essa ação retorna, o *About.cshtml* exibição mostrada na seção anterior é renderizada como a seguinte página da Web:</span><span class="sxs-lookup"><span data-stu-id="5802b-156">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![Sobre a página renderizada no navegador Edge](overview/_static/about-page.png)

<span data-ttu-id="5802b-158">O `View` método auxiliar tem várias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="5802b-158">The `View` helper method has several overloads.</span></span> <span data-ttu-id="5802b-159">Opcionalmente, você pode especificar:</span><span class="sxs-lookup"><span data-stu-id="5802b-159">You can optionally specify:</span></span>

* <span data-ttu-id="5802b-160">Uma exibição explícita para retornar:</span><span class="sxs-lookup"><span data-stu-id="5802b-160">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="5802b-161">Um [modelo](xref:mvc/models/model-binding) para passar para o modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="5802b-161">A [model](xref:mvc/models/model-binding) to pass to the the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="5802b-162">Um modo de exibição e um modelo:</span><span class="sxs-lookup"><span data-stu-id="5802b-162">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="5802b-163">Descoberta do modo de exibição</span><span class="sxs-lookup"><span data-stu-id="5802b-163">View discovery</span></span>

<span data-ttu-id="5802b-164">Quando uma ação retorna uma exibição, um processo chamado *descoberta do modo de exibição* ocorra.</span><span class="sxs-lookup"><span data-stu-id="5802b-164">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="5802b-165">Esse processo determina qual arquivo de exibição é usado com base no nome do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="5802b-165">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="5802b-166">O comportamento padrão da `View` método (`return View();`) deve retornar uma exibição com o mesmo nome que o método de ação do qual ele é chamado.</span><span class="sxs-lookup"><span data-stu-id="5802b-166">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="5802b-167">Por exemplo, o *sobre* `ActionResult` nome do método do controlador é usado para pesquisar um arquivo de modo de exibição denominado *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5802b-167">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="5802b-168">Primeiro, o tempo de execução examina o *modos de exibição / [ControllerName]* pasta para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="5802b-168">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="5802b-169">Se não encontrar uma exibição de correspondência, ele procura o *compartilhado* pasta para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="5802b-169">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="5802b-170">Não importa se você retornar implicitamente o `ViewResult` com `return View();` ou passar explicitamente o nome de exibição para o `View` método com `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="5802b-170">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="5802b-171">Em ambos os casos, a exibição descoberta pesquisa um arquivo de exibição correspondente nesta ordem:</span><span class="sxs-lookup"><span data-stu-id="5802b-171">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="5802b-172">*Modos de exibição /\[ControllerName]\[ViewName]. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5802b-172">*Views/\[ControllerName]\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="5802b-173">*Exibições/compartilhadas/\[ViewName]. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5802b-173">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="5802b-174">Um caminho de arquivo do modo de exibição pode ser fornecido em vez de um nome de exibição.</span><span class="sxs-lookup"><span data-stu-id="5802b-174">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="5802b-175">Se usar um caminho absoluto começando na raiz do aplicativo (se desejar começar com "/" ou "~ /"), o *. cshtml* extensão deve ser especificada:</span><span class="sxs-lookup"><span data-stu-id="5802b-175">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="5802b-176">Você também pode usar um caminho relativo para especificar os modos de exibição em diretórios diferentes sem o *. cshtml* extensão.</span><span class="sxs-lookup"><span data-stu-id="5802b-176">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="5802b-177">Dentro de `HomeController`, você pode retornar o *índice* modo de exibição de seu *gerenciar* exibições com um caminho relativo:</span><span class="sxs-lookup"><span data-stu-id="5802b-177">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="5802b-178">Da mesma forma, você pode indicar o diretório atual do controlador específico com o ". /" prefixo:</span><span class="sxs-lookup"><span data-stu-id="5802b-178">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="5802b-179">[Exibições parciais](xref:mvc/views/partial) e [exibir componentes](xref:mvc/views/view-components) usar mecanismos de descoberta semelhantes (mas não idêntica).</span><span class="sxs-lookup"><span data-stu-id="5802b-179">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="5802b-180">Você pode personalizar a convenção padrão para como os modos de exibição estão localizados dentro do aplicativo usando um personalizado [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span><span class="sxs-lookup"><span data-stu-id="5802b-180">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="5802b-181">Descoberta do modo de exibição depende Localizando exibir arquivos por nome de arquivo.</span><span class="sxs-lookup"><span data-stu-id="5802b-181">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="5802b-182">Se o sistema de arquivos subjacente diferencia maiusculas de minúsculas, nomes de exibição são provavelmente diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="5802b-182">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="5802b-183">Para compatibilidade em sistemas operacionais, diferenciar maiusculas de minúsculas entre o controlador e ação nomes e as pastas de exibição associada e arquivos.</span><span class="sxs-lookup"><span data-stu-id="5802b-183">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="5802b-184">Se você encontrar um erro que não é possível encontrar um arquivo de exibição ao trabalhar com um sistema de arquivos diferencia maiusculas de minúsculas, confirme se o uso de maiusculas e minúsculas corresponde entre o arquivo de modo de exibição solicitado e o nome do arquivo de exibição atual.</span><span class="sxs-lookup"><span data-stu-id="5802b-184">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="5802b-185">Siga a prática recomendada de organizar a estrutura de arquivos nos modos de exibição refletir as relações entre os controladores, ações e modos de exibição para facilidade de manutenção e clareza.</span><span class="sxs-lookup"><span data-stu-id="5802b-185">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="5802b-186">Transmitindo dados para modos de exibição</span><span class="sxs-lookup"><span data-stu-id="5802b-186">Passing data to views</span></span>

<span data-ttu-id="5802b-187">Você pode passar dados para exibições com várias abordagens.</span><span class="sxs-lookup"><span data-stu-id="5802b-187">You can pass data to views using several approaches.</span></span> <span data-ttu-id="5802b-188">A abordagem mais eficiente é especificar um [modelo](xref:mvc/models/model-binding) tipo na exibição.</span><span class="sxs-lookup"><span data-stu-id="5802b-188">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="5802b-189">Esse modelo é conhecido como um *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="5802b-189">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="5802b-190">Você pode passar uma instância do tipo viewmodel para o modo de exibição da ação.</span><span class="sxs-lookup"><span data-stu-id="5802b-190">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="5802b-191">Usar um viewmodel para passar dados para um modo de exibição permite que o modo de exibição tirar proveito dos *forte* verificação de tipo.</span><span class="sxs-lookup"><span data-stu-id="5802b-191">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="5802b-192">*Tipagem forte* (ou *fortemente tipada*) significa que todas as variáveis e constante tem um tipo definido explicitamente (por exemplo, `string`, `int`, ou `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="5802b-192">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="5802b-193">A validade dos tipos usados em uma exibição é verificada em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="5802b-193">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="5802b-194">[O Visual Studio](https://www.visualstudio.com/vs/) e [código do Visual Studio](https://code.visualstudio.com/) listam os membros de classe fortemente tipada usando um recurso chamado [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="5802b-194">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="5802b-195">Quando você deseja ver as propriedades de um viewmodel, digite o nome da variável para o viewmodel seguido por um período (`.`).</span><span class="sxs-lookup"><span data-stu-id="5802b-195">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="5802b-196">Isso ajuda você a escrever código mais rápido com menos erros.</span><span class="sxs-lookup"><span data-stu-id="5802b-196">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="5802b-197">Especifique um modelo usando o `@model` diretiva.</span><span class="sxs-lookup"><span data-stu-id="5802b-197">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="5802b-198">Usar o modelo com `@Model`:</span><span class="sxs-lookup"><span data-stu-id="5802b-198">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="5802b-199">Para fornecer o modelo para o modo de exibição, o controlador de passá-lo como um parâmetro:</span><span class="sxs-lookup"><span data-stu-id="5802b-199">To provide the model to the view, the controller passes it as a parameter:</span></span>

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

<span data-ttu-id="5802b-200">Não existem restrições sobre os tipos de modelo que você pode fornecer a uma exibição.</span><span class="sxs-lookup"><span data-stu-id="5802b-200">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="5802b-201">É recomendável usar **P**lain **O**ld **C**LR **O**viewmodels bjeto (POCO) com pouca ou nenhuma comportamento (métodos) definido.</span><span class="sxs-lookup"><span data-stu-id="5802b-201">We recommend using **P**lain **O**ld **C**LR **O**bject (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="5802b-202">Geralmente, classes viewmodel são armazenadas no *modelos* pasta ou um separado *ViewModels* pasta na raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5802b-202">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="5802b-203">O *endereço* viewmodel usado no exemplo acima é um viewmodel POCO armazenado em um arquivo chamado *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="5802b-203">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

> [!NOTE]
> <span data-ttu-id="5802b-204">Nada impede que você use as mesmas classes para seus tipos de viewmodel e seus tipos de modelo de negócios.</span><span class="sxs-lookup"><span data-stu-id="5802b-204">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="5802b-205">No entanto, o uso de modelos separados permite que seus modos de exibição variar independentemente da lógica de negócios e dados de partes de acesso do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5802b-205">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="5802b-206">Separação de modelos e viewmodels também oferece benefícios de segurança quando os modelos usam [associação de modelo](xref:mvc/models/model-binding) e [validação](xref:mvc/models/validation) para dados enviados para o aplicativo pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="5802b-206">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a><span data-ttu-id="5802b-207">Dados de tipo fraco (ViewData e ViewBag)</span><span class="sxs-lookup"><span data-stu-id="5802b-207">Weakly-typed data (ViewData and ViewBag)</span></span>

<span data-ttu-id="5802b-208">Observação: `ViewBag` não está disponível nas páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="5802b-208">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="5802b-209">Além dos modos de exibição fortemente tipada, modos de exibição têm acesso a um *tipo fraco* (também chamado de *tipadas vagamente*) coleta de dados.</span><span class="sxs-lookup"><span data-stu-id="5802b-209">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="5802b-210">Diferentemente dos tipos de alta seguras, *tipos fracos* (ou *perder tipos*) significa que você não declarar explicitamente o tipo de dados que você está usando.</span><span class="sxs-lookup"><span data-stu-id="5802b-210">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="5802b-211">Você pode usar a coleta de dados de tipo fraco para a passagem de pequenas quantidades de dados dentro e fora de controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="5802b-211">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="5802b-212">Transmitindo dados entre um...</span><span class="sxs-lookup"><span data-stu-id="5802b-212">Passing data between a ...</span></span>                        | <span data-ttu-id="5802b-213">Exemplo</span><span class="sxs-lookup"><span data-stu-id="5802b-213">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="5802b-214">Controlador e uma exibição</span><span class="sxs-lookup"><span data-stu-id="5802b-214">Controller and a view</span></span>                             | <span data-ttu-id="5802b-215">Preencher uma lista suspensa com dados.</span><span class="sxs-lookup"><span data-stu-id="5802b-215">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="5802b-216">Modo de exibição e um [exibição de layout](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="5802b-216">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="5802b-217">Definindo o  **\<título >** conteúdo de elemento na exibição de layout de um arquivo de modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="5802b-217">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="5802b-218">[Exibição parcial](xref:mvc/views/partial) e um modo de exibição</span><span class="sxs-lookup"><span data-stu-id="5802b-218">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="5802b-219">Um widget que exibe dados com base na página da Web que o usuário solicitou.</span><span class="sxs-lookup"><span data-stu-id="5802b-219">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="5802b-220">Essa coleção pode ser referenciada por meio de `ViewData` ou `ViewBag` propriedades nos controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="5802b-220">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="5802b-221">O `ViewData` propriedade é um dicionário de objetos do tipo fraco.</span><span class="sxs-lookup"><span data-stu-id="5802b-221">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="5802b-222">O `ViewBag` propriedade é um wrapper em torno de `ViewData` que fornece propriedades dinâmicas para subjacente `ViewData` coleção.</span><span class="sxs-lookup"><span data-stu-id="5802b-222">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="5802b-223">`ViewData`e `ViewBag` são resolvidos dinamicamente em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="5802b-223">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="5802b-224">Já que não oferecem a verificação de tipo de tempo de compilação, ambos são geralmente mais propensas a erros que o uso de um viewmodel.</span><span class="sxs-lookup"><span data-stu-id="5802b-224">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="5802b-225">Por esse motivo, alguns desenvolvedores preferem usar minimamente ou nunca `ViewData` e `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="5802b-225">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>


<a name="VD"></a>

<span data-ttu-id="5802b-226">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="5802b-226">**ViewData**</span></span>

<span data-ttu-id="5802b-227">`ViewData`é um [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) acessado por meio do objeto `string` chaves.</span><span class="sxs-lookup"><span data-stu-id="5802b-227">`ViewData` is a [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="5802b-228">Dados de cadeia de caracteres podem ser armazenados e usados diretamente, sem a necessidade de uma conversão, mas você deve converter outros `ViewData` valores para tipos específicos de objeto quando você extraí-los.</span><span class="sxs-lookup"><span data-stu-id="5802b-228">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="5802b-229">Você pode usar `ViewData` para passar dados de controladores para modos de exibição e em modos de exibição, incluindo [exibições parciais](xref:mvc/views/partial) e [layouts](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="5802b-229">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="5802b-230">A seguir está um exemplo que define valores para uma saudação e um endereço usando `ViewData` em uma ação:</span><span class="sxs-lookup"><span data-stu-id="5802b-230">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

<span data-ttu-id="5802b-231">Trabalhar com os dados em um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="5802b-231">Work with the data in a view:</span></span>

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

<span data-ttu-id="5802b-232">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="5802b-232">**ViewBag**</span></span>

<span data-ttu-id="5802b-233">Observação: `ViewBag` não está disponível nas páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="5802b-233">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="5802b-234">`ViewBag`é um [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) objeto que fornece acesso dinâmico para os objetos armazenados em `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="5802b-234">`ViewBag` is a [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="5802b-235">`ViewBag`pode ser mais conveniente trabalhar com, pois ele não requer conversão.</span><span class="sxs-lookup"><span data-stu-id="5802b-235">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="5802b-236">O exemplo a seguir mostra como usar `ViewBag` com o mesmo resultado usando `ViewData` acima:</span><span class="sxs-lookup"><span data-stu-id="5802b-236">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

<span data-ttu-id="5802b-237">**Usando ViewData e ViewBag simultaneamente**</span><span class="sxs-lookup"><span data-stu-id="5802b-237">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="5802b-238">Observação: `ViewBag` não está disponível nas páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="5802b-238">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="5802b-239">Como `ViewData` e `ViewBag` consulte subjacente mesmo `ViewData` coleção, você pode usar ambos `ViewData` e `ViewBag` e combinação de entre elas ao ler e gravar valores.</span><span class="sxs-lookup"><span data-stu-id="5802b-239">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="5802b-240">Definir o título usando `ViewBag` e a descrição usando `ViewData` na parte superior de um *About.cshtml* exibição:</span><span class="sxs-lookup"><span data-stu-id="5802b-240">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="5802b-241">Ler as propriedades, mas reverter o uso de `ViewData` e `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="5802b-241">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="5802b-242">No *cshtml* de arquivos, obtenha o título usando `ViewData` e obter a descrição usando `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="5802b-242">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="5802b-243">Lembre-se de que cadeias de caracteres não exigem uma conversão para `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="5802b-243">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="5802b-244">Você pode usar `@ViewData["Title"]` sem conversão.</span><span class="sxs-lookup"><span data-stu-id="5802b-244">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="5802b-245">Usando `ViewData` e `ViewBag` o mesmo Works de tempo, como faz a combinação de ler e gravar as propriedades.</span><span class="sxs-lookup"><span data-stu-id="5802b-245">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="5802b-246">A seguinte marcação é processada:</span><span class="sxs-lookup"><span data-stu-id="5802b-246">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="5802b-247">**Resumo das diferenças entre ViewData e ViewBag**</span><span class="sxs-lookup"><span data-stu-id="5802b-247">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="5802b-248">`ViewBag`não está disponível nas páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="5802b-248">`ViewBag` is not available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="5802b-249">Deriva [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary); portanto, tem as propriedades de dicionário que podem ser útil, por exemplo, `ContainsKey`, `Add`, `Remove`, e `Clear`.</span><span class="sxs-lookup"><span data-stu-id="5802b-249">Derives from [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="5802b-250">Chaves no dicionário são cadeias de caracteres, por isso é permitido espaço em branco.</span><span class="sxs-lookup"><span data-stu-id="5802b-250">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="5802b-251">Exemplo: `ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="5802b-251">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="5802b-252">Qualquer tipo diferente de um `string` devem ser convertidos no modo de exibição para usar `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="5802b-252">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="5802b-253">Deriva [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), que permite a criação de propriedades dinâmicas usando a notação de ponto (`@ViewBag.SomeKey = <value or object>`), e nenhuma conversão é necessária.</span><span class="sxs-lookup"><span data-stu-id="5802b-253">Derives from [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="5802b-254">A sintaxe de `ViewBag` torna mais rápida adicionar controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="5802b-254">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="5802b-255">Mais simples verificar se há valores nulos.</span><span class="sxs-lookup"><span data-stu-id="5802b-255">Simpler to check for null values.</span></span> <span data-ttu-id="5802b-256">Exemplo: `@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="5802b-256">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="5802b-257">**Quando usar ViewData ou ViewBag**</span><span class="sxs-lookup"><span data-stu-id="5802b-257">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="5802b-258">Ambos `ViewData` e `ViewBag` são igualmente abordagens válidas para transmitir pequenas quantidades de dados entre os controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="5802b-258">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="5802b-259">A escolha de qual delas usar é com base na preferência.</span><span class="sxs-lookup"><span data-stu-id="5802b-259">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="5802b-260">Você pode misturar e combinar `ViewData` e `ViewBag` objetos, no entanto, o código é mais fácil de ler e manter com uma abordagem usada de maneira consistente.</span><span class="sxs-lookup"><span data-stu-id="5802b-260">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="5802b-261">Ambas as abordagens são resolvidos dinamicamente em tempo de execução e, portanto, propenso a erros de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="5802b-261">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="5802b-262">Algumas equipes de desenvolvimento evitá-los.</span><span class="sxs-lookup"><span data-stu-id="5802b-262">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="5802b-263">Modos de exibição dinâmicos</span><span class="sxs-lookup"><span data-stu-id="5802b-263">Dynamic views</span></span>

<span data-ttu-id="5802b-264">Modos de exibição que não declare um modelo de tipo usando `@model` , mas que tem uma instância de modelo passada a eles (por exemplo, `return View(Address);`) pode fazer referência a propriedades da instância dinamicamente:</span><span class="sxs-lookup"><span data-stu-id="5802b-264">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="5802b-265">Esse recurso oferece a flexibilidade, mas não oferece proteção de compilação ou o IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="5802b-265">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="5802b-266">Se a propriedade não existir, falha na geração de página da Web em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="5802b-266">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="5802b-267">Mais recursos de exibição</span><span class="sxs-lookup"><span data-stu-id="5802b-267">More view features</span></span>

<span data-ttu-id="5802b-268">[Auxiliares da marca](xref:mvc/views/tag-helpers/intro) mais fácil adicionar o comportamento do lado do servidor para marcas HTML existentes.</span><span class="sxs-lookup"><span data-stu-id="5802b-268">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="5802b-269">O uso de auxiliares de marcação evita a necessidade de escrever código personalizado ou auxiliares em suas exibições.</span><span class="sxs-lookup"><span data-stu-id="5802b-269">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="5802b-270">Auxiliares de marca são aplicados como atributos para elementos HTML e são ignorados por editores que não podem processá-los.</span><span class="sxs-lookup"><span data-stu-id="5802b-270">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="5802b-271">Isso permite que você edite e renderizar a marcação de exibição em uma variedade de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="5802b-271">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="5802b-272">Gera a marcação HTML personalizada pode ser obtido com muitos auxiliares HTML interno.</span><span class="sxs-lookup"><span data-stu-id="5802b-272">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="5802b-273">Lógica de interface de usuário mais complexa pode ser tratada por [exibir componentes](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="5802b-273">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="5802b-274">Exibir componentes fornecem o mesmo SoC que controladores e oferecem exibições.</span><span class="sxs-lookup"><span data-stu-id="5802b-274">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="5802b-275">Eles podem eliminar a necessidade de ações e modos de exibição que lidam com dados usados por elementos de interface de usuário comuns.</span><span class="sxs-lookup"><span data-stu-id="5802b-275">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="5802b-276">Como muitos outros aspectos do ASP.NET Core, exibições de suporte [injeção de dependência](xref:fundamentals/dependency-injection), permitindo que os serviços estejam [injetadas nos modos de exibição](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5802b-276">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
