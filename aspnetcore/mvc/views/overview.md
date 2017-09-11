---
title: "Visão geral de modos de exibição"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 7abfa7ef855eb95e1a27ba6a699dd923c9e4d7c0
ms.sourcegitcommit: 6ece943781d8a56784bb6160f14da85210d3fcea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a><span data-ttu-id="ed9b3-103">Renderização HTML com exibições do MVC do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed9b3-103">Rendering HTML with views in ASP.NET Core MVC</span></span>

<span data-ttu-id="ed9b3-104">Por [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="ed9b3-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="ed9b3-105">Controladores do ASP.NET MVC Core podem retornar resultados formatados usando *exibições*.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-105">ASP.NET Core MVC controllers can return formatted results using *views*.</span></span>

## <a name="what-are-views"></a><span data-ttu-id="ed9b3-106">Quais são os modos de exibição?</span><span class="sxs-lookup"><span data-stu-id="ed9b3-106">What are Views?</span></span>

<span data-ttu-id="ed9b3-107">O padrão Model-View-Controller (MVC), o *exibição* encapsula os detalhes de apresentação de interação do usuário com o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-107">In the Model-View-Controller (MVC) pattern, the *view* encapsulates the presentation details of the user's interaction with the app.</span></span> <span data-ttu-id="ed9b3-108">Modos de exibição são modelos HTML com o código inserido que geram conteúdo para enviar ao cliente.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-108">Views are HTML templates with embedded code that generate content to send to the client.</span></span> <span data-ttu-id="ed9b3-109">Exibições usam [sintaxe Razor](razor.md), que permite que o código interagir com HTML com o mínimo de código ou cerimônia.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-109">Views use [Razor syntax](razor.md), which allows code to interact with HTML with minimal code or ceremony.</span></span>

<span data-ttu-id="ed9b3-110">Modos de exibição do ASP.NET MVC de núcleo são *. cshtml* arquivos armazenados por padrão em um *exibições* pasta dentro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-110">ASP.NET Core MVC views are *.cshtml* files stored by default in a *Views* folder within the application.</span></span> <span data-ttu-id="ed9b3-111">Normalmente, cada controlador terá sua própria pasta, na qual são modos de exibição para ações do controlador específico.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-111">Typically, each controller will have its own folder, in which are views for specific controller actions.</span></span>

![Pasta de modos de exibição no Gerenciador de soluções](overview/_static/views_solution_explorer.png)

<span data-ttu-id="ed9b3-113">Além dos modos de exibição de ação específica, [exibições parciais](partial.md), [layouts e outros arquivos de modo de exibição especial](layout.md) pode ser usado para ajudar a reduzir a repetição e permitir a reutilização em modos de exibição do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-113">In addition to action-specific views, [partial views](partial.md), [layouts, and other special view files](layout.md) can be used to help reduce repetition and allow for reuse within the app's views.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="ed9b3-114">Benefícios do uso de exibições</span><span class="sxs-lookup"><span data-stu-id="ed9b3-114">Benefits of Using Views</span></span>

<span data-ttu-id="ed9b3-115">Fornecem exibições [separação de preocupações](http://deviq.com/separation-of-concerns/) dentro de um aplicativo MVC, encapsulando a marcação de nível de interface do usuário separadamente de lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-115">Views provide [separation of concerns](http://deviq.com/separation-of-concerns/) within an MVC app, encapsulating user interface level markup separately from business logic.</span></span> <span data-ttu-id="ed9b3-116">ASP.NET MVC exibições usam [sintaxe Razor](razor.md) para alternar entre HTML marcação e o servidor lógica simples.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-116">ASP.NET MVC views use [Razor syntax](razor.md) to make switching between HTML markup and server side logic painless.</span></span> <span data-ttu-id="ed9b3-117">Comuns, repetitivos aspectos da interface de usuário do aplicativo podem ser reutilizados facilmente entre exibições usando [diretivas compartilhadas e layout](layout.md) ou [exibições parciais](partial.md).</span><span class="sxs-lookup"><span data-stu-id="ed9b3-117">Common, repetitive aspects of the app's user interface can easily be reused between views using [layout and shared directives](layout.md) or [partial views](partial.md).</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="ed9b3-118">Criar um modo de exibição</span><span class="sxs-lookup"><span data-stu-id="ed9b3-118">Creating a View</span></span>

<span data-ttu-id="ed9b3-119">Exibições que são específicas para um controlador são criadas no *modos de exibição / [ControllerName]* pasta.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-119">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="ed9b3-120">Modos de exibição que são compartilhados entre os controladores são colocados no */exibições/compartilhado* pasta.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-120">Views that are shared among controllers are placed in the */Views/Shared* folder.</span></span> <span data-ttu-id="ed9b3-121">Nomeie o arquivo de exibição o mesmo que sua ação de controlador associado e adicione o *. cshtml* extensão de arquivo.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-121">Name the view file the same as its associated controller action, and add the *.cshtml* file extension.</span></span> <span data-ttu-id="ed9b3-122">Por exemplo, para criar uma exibição para o *sobre* ação o *início* controlador, você deve criar o *About.cshtml* do arquivo no  * /exibições/inicial*pasta.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-122">For example, to create a view for the *About* action on the *Home* controller, you would create the *About.cshtml* file in the */Views/Home* folder.</span></span>

<span data-ttu-id="ed9b3-123">Um exemplo de arquivo de exibição (*About.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="ed9b3-123">A sample view file (*About.cshtml*):</span></span>

<span data-ttu-id="ed9b3-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="ed9b3-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span></span>

<span data-ttu-id="ed9b3-125">*Razor* código é indicado pelo `@` símbolo.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-125">*Razor* code is denoted by the `@` symbol.</span></span> <span data-ttu-id="ed9b3-126">C# instruções são executadas dentro de blocos de código definir chaves do Razor (`{` `}`), como a atribuição de "Sobre" para o `ViewData["Title"]` elemento mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-126">C# statements are run within Razor code blocks set off by curly braces (`{` `}`), such as the assignment of "About" to the `ViewData["Title"]` element shown above.</span></span> <span data-ttu-id="ed9b3-127">Razor pode ser usado para exibir valores em HTML, simplesmente referenciando o valor com o `@` símbolo, conforme mostrado no `<h2>` e `<h3>` elementos acima.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-127">Razor can be used to display values within HTML by simply referencing the value with the `@` symbol, as shown within the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="ed9b3-128">Essa exibição se concentra na parte da saída para os quais é responsável.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-128">This view focuses on just the portion of the output for which it is responsible.</span></span> <span data-ttu-id="ed9b3-129">O resto do layout da página e outros aspectos comuns da exibição, são especificados em outro lugar.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-129">The rest of the page's layout, and other common aspects of the view, are specified elsewhere.</span></span> <span data-ttu-id="ed9b3-130">Saiba mais sobre [layout e a lógica de modo de exibição compartilhado](layout.md).</span><span class="sxs-lookup"><span data-stu-id="ed9b3-130">Learn more about [layout and shared view logic](layout.md).</span></span>

## <a name="how-do-controllers-specify-views"></a><span data-ttu-id="ed9b3-131">Como fazer controladores especificar modos de exibição?</span><span class="sxs-lookup"><span data-stu-id="ed9b3-131">How do Controllers Specify Views?</span></span>

<span data-ttu-id="ed9b3-132">Modos de exibição normalmente retornados por ações como um `ViewResult`.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-132">Views are typically returned from actions as a `ViewResult`.</span></span> <span data-ttu-id="ed9b3-133">O método de ação pode criar e retornar um `ViewResult` diretamente, mas mais comumente se herda seu controlador de `Controller`, você simplesmente usará o `View` método auxiliar, como este exemplo demonstra:</span><span class="sxs-lookup"><span data-stu-id="ed9b3-133">Your action method can create and return a `ViewResult` directly, but more commonly if your controller inherits from `Controller`, you'll simply use the `View` helper method, as this example demonstrates:</span></span>

<span data-ttu-id="ed9b3-134">*HomeController*</span><span class="sxs-lookup"><span data-stu-id="ed9b3-134">*HomeController.cs*</span></span>

<span data-ttu-id="ed9b3-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span><span class="sxs-lookup"><span data-stu-id="ed9b3-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span></span>

<span data-ttu-id="ed9b3-136">O `View` método auxiliar tem várias sobrecargas para facilitar o retorno de modos de exibição para os desenvolvedores de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-136">The `View` helper method has several overloads to make returning views easier for app developers.</span></span> <span data-ttu-id="ed9b3-137">Opcionalmente, você pode especificar um modo de exibição para retornar, bem como um objeto de modelo para passar para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-137">You can optionally specify a view to return, as well as a model object to pass to the view.</span></span>

<span data-ttu-id="ed9b3-138">Quando essa ação retorna, o *About.cshtml* exibição mostrada acima é renderizada:</span><span class="sxs-lookup"><span data-stu-id="ed9b3-138">When this action returns, the *About.cshtml* view shown above is rendered:</span></span>

![Sobre a página](overview/_static/about-page.png)

### <a name="view-discovery"></a><span data-ttu-id="ed9b3-140">Descoberta do modo de exibição</span><span class="sxs-lookup"><span data-stu-id="ed9b3-140">View Discovery</span></span>

<span data-ttu-id="ed9b3-141">Quando uma ação retorna uma exibição, um processo chamado *descoberta do modo de exibição* ocorra.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-141">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="ed9b3-142">Esse processo determina qual arquivo de exibição será usado.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-142">This process determines which view file will be used.</span></span> <span data-ttu-id="ed9b3-143">A menos que um arquivo de modo de exibição específico for especificado, o tempo de execução procura por um modo de exibição específicos do controlador, em seguida, procura primeiro nome de exibição correspondente no *compartilhado* pasta.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-143">Unless a specific view file is specified, the runtime looks for a controller-specific view first, then looks for matching view name in the *Shared* folder.</span></span>

<span data-ttu-id="ed9b3-144">Quando uma ação retorna o `View` método, da seguinte forma `return View();`, o nome da ação é usado como o nome da exibição.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-144">When an action returns the `View` method, like so `return View();`, the action name is used as the view name.</span></span> <span data-ttu-id="ed9b3-145">Por exemplo, se isso foi chamado de um método de ação denominado "Index", seria equivalente para passar um nome de exibição de "Index".</span><span class="sxs-lookup"><span data-stu-id="ed9b3-145">For example, if this were called from an action method named "Index", it would be equivalent to passing in a view name of "Index".</span></span> <span data-ttu-id="ed9b3-146">Um nome de exibição pode ser passado explicitamente para o método (`return View("SomeView");`).</span><span class="sxs-lookup"><span data-stu-id="ed9b3-146">A view name can be explicitly passed to the method (`return View("SomeView");`).</span></span> <span data-ttu-id="ed9b3-147">Em ambos os casos, a descoberta do modo de exibição procura um arquivo de exibição correspondente no:</span><span class="sxs-lookup"><span data-stu-id="ed9b3-147">In both of these cases, view discovery searches for a matching view file in:</span></span>

   1. <span data-ttu-id="ed9b3-148">Modos de exibição /\<ControllerName > /\<ViewName >. cshtml</span><span class="sxs-lookup"><span data-stu-id="ed9b3-148">Views/\<ControllerName>/\<ViewName>.cshtml</span></span>

   2. <span data-ttu-id="ed9b3-149">Exibições/compartilhadas/\<ViewName >. cshtml</span><span class="sxs-lookup"><span data-stu-id="ed9b3-149">Views/Shared/\<ViewName>.cshtml</span></span>

>[!TIP]
> <span data-ttu-id="ed9b3-150">É recomendável seguir a convenção de simplesmente retornar `View()` de ações quando possível, já que resulta em mais flexível e mais fácil para refatorar o código.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-150">We recommend following the convention of simply returning `View()` from actions when possible, as it results in more flexible, easier to refactor code.</span></span>

<span data-ttu-id="ed9b3-151">Um caminho de arquivo do modo de exibição pode ser fornecido em vez de um nome de exibição.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-151">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="ed9b3-152">Se usar um caminho absoluto começando na raiz do aplicativo (se desejar começar com "/" ou "~ /"), o *. cshtml* extensão deve ser especificada como parte do caminho do arquivo (por exemplo, `return View("Views/Home/About.cshtml");`).</span><span class="sxs-lookup"><span data-stu-id="ed9b3-152">If using an absolute path starting at the application root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified as part of the file path (for example, `return View("Views/Home/About.cshtml");`).</span></span> <span data-ttu-id="ed9b3-153">Como alternativa, você pode usar um caminho relativo do diretório específicos do controlador no *exibições* directory para especificar os modos de exibição em diretórios diferentes (por exemplo, `return View("../Manage/Index");` dentro de `HomeController`).</span><span class="sxs-lookup"><span data-stu-id="ed9b3-153">Alternatively, you can use a relative path from the controller-specific directory within the *Views* directory to specify views in different directories (for example, `return View("../Manage/Index");` inside the `HomeController`).</span></span> <span data-ttu-id="ed9b3-154">Da mesma forma, você pode percorrer o diretório atual do controlador específico (por exemplo, `return View("./About");`).</span><span class="sxs-lookup"><span data-stu-id="ed9b3-154">Similarly, you can traverse the current controller-specific directory (for example, `return View("./About");`).</span></span> <span data-ttu-id="ed9b3-155">Observe que os caminhos relativos não usam o *. cshtml* extensão.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-155">Note that relative paths don't use the *.cshtml* extension.</span></span> <span data-ttu-id="ed9b3-156">Como mencionado anteriormente, siga a prática recomendada de organizar a estrutura de arquivos para modos de exibição refletir as relações entre os controladores, ações e modos de exibição para facilidade de manutenção e clareza.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-156">As previously mentioned, follow the best practice of organizing the file structure for views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

> [!NOTE]
> <span data-ttu-id="ed9b3-157">[Exibições parciais](partial.md) e [exibir componentes](view-components.md) usar mecanismos de descoberta semelhantes (mas não idêntica).</span><span class="sxs-lookup"><span data-stu-id="ed9b3-157">[Partial views](partial.md) and [view components](view-components.md) use similar (but not identical) discovery mechanisms.</span></span>

> [!NOTE]
> <span data-ttu-id="ed9b3-158">Você pode personalizar a convenção padrão sobre onde os modos de exibição estão localizados dentro do aplicativo usando um personalizado `IViewLocationExpander`.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-158">You can customize the default convention regarding where views are located within the app by using a custom `IViewLocationExpander`.</span></span>

>[!TIP]
> <span data-ttu-id="ed9b3-159">Nomes de exibição podem diferenciar maiusculas de minúsculas dependendo do sistema de arquivos subjacente.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-159">View names may be case sensitive depending on the underlying file system.</span></span> <span data-ttu-id="ed9b3-160">Para compatibilidade em sistemas operacionais, sempre corresponde caso entre o controlador e nomes de ação e as pastas de exibição associada e nomes de arquivos.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-160">For compatibility across operating systems, always match case between controller and action names and associated view folders and filenames.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="ed9b3-161">Transmitindo dados para modos de exibição</span><span class="sxs-lookup"><span data-stu-id="ed9b3-161">Passing Data to Views</span></span>

<span data-ttu-id="ed9b3-162">Você pode passar dados para modos de exibição usando vários mecanismos.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-162">You can pass data to views using several mechanisms.</span></span> <span data-ttu-id="ed9b3-163">A abordagem mais eficiente é especificar um *modelo* tipo no modo de exibição (conhecido como um *viewmodel*, para diferenciá-lo dos tipos de modelo de domínio de negócios) e, em seguida, passar uma instância deste tipo para o modo de exibição da ação.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-163">The most robust approach is to specify a *model* type in the view (commonly referred to as a *viewmodel*, to distinguish it from business domain model types), and then pass an instance of this type to the view from the action.</span></span> <span data-ttu-id="ed9b3-164">Recomendamos que você use um modelo ou um modelo de exibição para passar dados para um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-164">We recommend you use a model or view model to pass data to a view.</span></span> <span data-ttu-id="ed9b3-165">Isso permite que o modo de exibição aproveitar a verificação de tipo forte.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-165">This allows the view to take advantage of strong type checking.</span></span> <span data-ttu-id="ed9b3-166">Você pode especificar um modelo para um modo de exibição usando o `@model` diretiva:</span><span class="sxs-lookup"><span data-stu-id="ed9b3-166">You can specify a model for a view using the `@model` directive:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="ed9b3-167">Depois que um modelo tiver sido especificado para um modo de exibição, a instância enviada para o modo de exibição pode ser acessada em uma maneira fortemente tipado usando `@Model` como mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-167">Once a model has been specified for a view, the instance sent to the view can be accessed in a strongly-typed manner using `@Model` as shown above.</span></span> <span data-ttu-id="ed9b3-168">Para fornecer uma instância do tipo de modelo para o modo de exibição, o controlador de passá-lo como um parâmetro:</span><span class="sxs-lookup"><span data-stu-id="ed9b3-168">To provide an instance of the model type to the view, the controller passes it as a parameter:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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

<span data-ttu-id="ed9b3-169">Não existem restrições sobre os tipos que podem ser fornecidos para um modo de exibição como um modelo.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-169">There are no restrictions on the types that can be provided to a view as a model.</span></span> <span data-ttu-id="ed9b3-170">É recomendável passando simples antigo CLR Object (POCO) Exibir modelos com o comportamento de pouca ou nenhuma lógica de negócios pode ser encapsulada em outro lugar no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-170">We recommend passing Plain Old CLR Object (POCO) view models with little or no behavior, so that business logic can be encapsulated elsewhere in the app.</span></span> <span data-ttu-id="ed9b3-171">Um exemplo dessa abordagem é o *endereço* viewmodel usado no exemplo acima:</span><span class="sxs-lookup"><span data-stu-id="ed9b3-171">An example of this approach is the *Address* viewmodel used in the example above:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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
> <span data-ttu-id="ed9b3-172">Nada impede que você use as mesmas classes como seus tipos de modelo de negócios e seus tipos de modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-172">Nothing prevents you from using the same classes as your business model types and your display model types.</span></span> <span data-ttu-id="ed9b3-173">No entanto, mantê-los separada permite que seus modos de exibição variar independentemente do seu modelo de domínio ou de persistência e pode oferecer alguns benefícios de segurança (para modelos que os usuários enviará para o aplicativo usando [modelo associação](../models/model-binding.md)).</span><span class="sxs-lookup"><span data-stu-id="ed9b3-173">However, keeping them separate allows your views to vary independently from your domain or persistence model, and can offer some security benefits as well (for models that users will send to the app using [model binding](../models/model-binding.md)).</span></span>

### <a name="loosely-typed-data"></a><span data-ttu-id="ed9b3-174">Sem rigidez de tipos de dados</span><span class="sxs-lookup"><span data-stu-id="ed9b3-174">Loosely Typed Data</span></span>

<span data-ttu-id="ed9b3-175">Além dos modos de exibição fortemente tipados, todas as exibições têm acesso a uma coleção sem rigidez de tipos de dados.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-175">In addition to strongly typed views, all views have access to a loosely typed collection of data.</span></span> <span data-ttu-id="ed9b3-176">Essa mesma coleção pode ser referenciada através de `ViewData` ou `ViewBag` propriedades nos controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-176">This same collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="ed9b3-177">O `ViewBag` propriedade é um wrapper em torno de `ViewData` que fornece uma exibição dinâmica nessa coleção.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-177">The `ViewBag` property is a wrapper around `ViewData` that provides a dynamic view over that collection.</span></span> <span data-ttu-id="ed9b3-178">Não é uma coleção separada.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-178">It is not a separate collection.</span></span>

<span data-ttu-id="ed9b3-179">`ViewData`um objeto de dicionário que é acessado por meio de `string` chaves.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-179">`ViewData` is a dictionary object accessed through `string` keys.</span></span> <span data-ttu-id="ed9b3-180">Você pode armazenar e recuperar objetos nele, e você precisará converta-os para um tipo específico, quando você extraí-los.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-180">You can store and retrieve objects in it, and you'll need to cast them to a specific type when you extract them.</span></span> <span data-ttu-id="ed9b3-181">Você pode usar `ViewData` para passar dados de um controlador para modos de exibição, bem como em exibições (exibições parciais e layouts).</span><span class="sxs-lookup"><span data-stu-id="ed9b3-181">You can use `ViewData` to pass data from a controller to views, as well as within views (and partial views and layouts).</span></span> <span data-ttu-id="ed9b3-182">Dados de cadeia de caracteres podem ser armazenados e usados diretamente, sem a necessidade de uma conversão.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-182">String data can be stored and used directly, without the need for a cast.</span></span>

<span data-ttu-id="ed9b3-183">Definir alguns valores para `ViewData` em uma ação:</span><span class="sxs-lookup"><span data-stu-id="ed9b3-183">Set some values for `ViewData` in an action:</span></span>

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

<span data-ttu-id="ed9b3-184">Trabalhar com os dados em um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="ed9b3-184">Work with the data in a view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

<span data-ttu-id="ed9b3-185">O `ViewBag` objetos fornece acesso dinâmico para os objetos armazenados em `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-185">The `ViewBag` objects provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="ed9b3-186">Isso pode ser mais conveniente trabalhar com, pois ele não requer conversão.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-186">This can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="ed9b3-187">O mesmo exemplo acima, usando `ViewBag` em vez de um fortemente tipada `address` instância no modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="ed9b3-187">The same example as above, using `ViewBag` instead of a strongly typed `address` instance in the view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> <span data-ttu-id="ed9b3-188">Desde que se referem ao mesmo subjacente `ViewData` coleção, você pode misturar e corresponder entre `ViewData` e `ViewBag` ao ler e gravar valores, se for conveniente.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-188">Since both refer to the same underlying `ViewData` collection, you can mix and match between `ViewData` and `ViewBag` when reading and writing values, if convenient.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="ed9b3-189">Modos de exibição dinâmicos</span><span class="sxs-lookup"><span data-stu-id="ed9b3-189">Dynamic Views</span></span>

<span data-ttu-id="ed9b3-190">Modos de exibição que não declare um tipo de modelo, mas tem uma instância de modelo passada para elas podem fazer referência a esta instância dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-190">Views that do not declare a model type but have a model instance passed to them can reference this instance dynamically.</span></span> <span data-ttu-id="ed9b3-191">Por exemplo, se uma instância do `Address` é passado para uma exibição que não declara um `@model`, a exibição ainda será capaz de fazer referência às propriedades da instância dinamicamente conforme mostrado:</span><span class="sxs-lookup"><span data-stu-id="ed9b3-191">For example, if an instance of `Address` is passed to a view that doesn't declare an `@model`, the view would still be able to refer to the instance's properties dynamically as shown:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="ed9b3-192">Esse recurso pode oferecer alguma flexibilidade, mas não oferece nenhuma proteção de compilação ou o IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-192">This feature can offer some flexibility, but does not offer any compilation protection or IntelliSense.</span></span> <span data-ttu-id="ed9b3-193">Se a propriedade não existir, a página falhará em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-193">If the property doesn't exist, the page will fail at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="ed9b3-194">Mais recursos de exibição</span><span class="sxs-lookup"><span data-stu-id="ed9b3-194">More View Features</span></span>

<span data-ttu-id="ed9b3-195">[Auxiliares da marca](tag-helpers/intro.md) mais fácil adicionar o comportamento do lado do servidor para marcas HTML existentes, evitando a necessidade de usar código personalizado ou auxiliares em modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-195">[Tag helpers](tag-helpers/intro.md) make it easy to add server-side behavior to existing HTML tags, avoiding the need to use custom code or helpers within views.</span></span> <span data-ttu-id="ed9b3-196">Os auxiliares de marca são aplicados como atributos para elementos HTML, que são ignorados por editores que não estão familiarizados com eles, permitindo que a marcação de exibição a ser editado e renderizados em uma variedade de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-196">Tag helpers are applied as attributes to HTML elements, which are ignored by editors that aren't familiar with them, allowing view markup to be edited and rendered in a variety of tools.</span></span> <span data-ttu-id="ed9b3-197">Auxiliares de marcação têm muitos usos e, em particular pode tornar [trabalhar com formulários](working-with-forms.md) muito mais fácil.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-197">Tag helpers have many uses, and in particular can make [working with forms](working-with-forms.md) much easier.</span></span>

<span data-ttu-id="ed9b3-198">Gera a marcação HTML personalizada pode ser obtido com muitos auxiliares HTML interno e uma lógica mais complexa da interface do usuário (possivelmente com seus próprios requisitos de dados) pode ser encapsulada em [exibir componentes](view-components.md).</span><span class="sxs-lookup"><span data-stu-id="ed9b3-198">Generating custom HTML markup can be achieved with many built-in HTML Helpers, and more complex UI logic (potentially with its own data requirements) can be encapsulated in [View Components](view-components.md).</span></span> <span data-ttu-id="ed9b3-199">Exibir componentes fornecem a mesma separação de preocupações que oferecem controladores e exibições e podem eliminar a necessidade de ações e modos de exibição para lidar com dados usados por elementos de interface de usuário comuns.</span><span class="sxs-lookup"><span data-stu-id="ed9b3-199">View components provide the same separation of concerns that controllers and views offer, and can eliminate the need for actions and views to deal with data used by common UI elements.</span></span>

<span data-ttu-id="ed9b3-200">Como muitos outros aspectos do ASP.NET Core, exibições de suporte [injeção de dependência](../../fundamentals/dependency-injection.md), permitindo que os serviços estejam [injetadas nos modos de exibição](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ed9b3-200">Like many other aspects of ASP.NET Core, views support [dependency injection](../../fundamentals/dependency-injection.md), allowing services to be [injected into views](dependency-injection.md).</span></span>
