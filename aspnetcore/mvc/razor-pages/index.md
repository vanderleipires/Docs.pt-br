---
title: "Introdução a Páginas do Razor no ASP.NET Core"
author: Rick-Anderson
description: "Visão geral de Páginas do Razor no ASP.NET Core"
keywords: "ASP.NET Core, Páginas do Razor"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: 9301b99aed8fcb3bef91abf0fb269c4052cdb7e2
ms.sourcegitcommit: 87900dffec8ad84a0f74357b23343e215f354dcb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/01/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="d02bb-104">Introdução a Páginas do Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d02bb-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="d02bb-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="d02bb-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="d02bb-106">Páginas do Razor é um novo recurso do ASP.NET Core MVC que torna a codificação de cenários focados em página mais fácil e produtiva.</span><span class="sxs-lookup"><span data-stu-id="d02bb-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="d02bb-107">Se você estiver procurando um tutorial que usa a abordagem Modelo-Exibição-Controlador, consulte a [Introdução ao ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="d02bb-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="d02bb-108">Pré-requisitos do ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="d02bb-108">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="d02bb-109">Instale o [.NET Core](https://dot.net/core) 2.0.0 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="d02bb-109">Install [.NET Core](https://dot.net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="d02bb-110">Se você estiver usando o Visual Studio, instale o [Visual Studio](https://www.visualstudio.com/vs/) 15.3 ou posterior com as cargas de trabalho a seguir:</span><span class="sxs-lookup"><span data-stu-id="d02bb-110">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="d02bb-111">**ASP.NET e desenvolvimento para a Web**</span><span class="sxs-lookup"><span data-stu-id="d02bb-111">**ASP.NET and web development**</span></span>
* <span data-ttu-id="d02bb-112">**Desenvolvimento entre plataformas do .NET Core**</span><span class="sxs-lookup"><span data-stu-id="d02bb-112">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="d02bb-113">Criando um projeto de Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="d02bb-113">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d02bb-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d02bb-114">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="d02bb-115">Consulte a [Introdução a Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start) para obter instruções detalhadas sobre como criar um projeto de Páginas do Razor usando o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d02bb-115">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d02bb-116">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="d02bb-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d02bb-117">Da linha de comando, execute `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-117">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="d02bb-118">Abra o arquivo *.csproj* gerado do Visual Studio para Mac.</span><span class="sxs-lookup"><span data-stu-id="d02bb-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d02bb-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d02bb-119">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="d02bb-120">Da linha de comando, execute `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-120">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d02bb-121">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="d02bb-121">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="d02bb-122">Da linha de comando, execute `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-122">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="d02bb-123">Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="d02bb-123">Razor Pages</span></span>

<span data-ttu-id="d02bb-124">O Páginas do Razor está habilitado em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d02bb-124">Razor Pages is enabled in *Startup.cs*:</span></span>

<span data-ttu-id="d02bb-125">[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=Startup)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-125">[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=Startup)]</span></span>

<span data-ttu-id="d02bb-126">Considere uma página básica: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="d02bb-126">Consider a basic page: <a name="OnGet"></a></span></span>

<span data-ttu-id="d02bb-127">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-127">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]</span></span>

<span data-ttu-id="d02bb-128">O código anterior é muito parecido com um arquivo de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="d02bb-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="d02bb-129">O que o torna diferentes é a diretiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="d02bb-130">`@page` transforma o arquivo em uma ação do MVC – o que significa que ele trata solicitações diretamente, sem passar por um controlador.</span><span class="sxs-lookup"><span data-stu-id="d02bb-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="d02bb-131">`@page` deve ser a primeira diretiva do Razor em uma página.</span><span class="sxs-lookup"><span data-stu-id="d02bb-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="d02bb-132">`@page` afeta o comportamento de outros constructos do Razor.</span><span class="sxs-lookup"><span data-stu-id="d02bb-132">`@page` affects the behavior of other Razor constructs.</span></span> <span data-ttu-id="d02bb-133">A diretiva [@functions](xref:mvc/views/razor#functions) habilita o conteúdo em nível de função.</span><span class="sxs-lookup"><span data-stu-id="d02bb-133">The [@functions](xref:mvc/views/razor#functions) directive enables function-level content.</span></span>

<span data-ttu-id="d02bb-134">Uma página semelhante, com o `PageModel` em um arquivo separado, é mostrada nos dois arquivos a seguir.</span><span class="sxs-lookup"><span data-stu-id="d02bb-134">A similar page, with the `PageModel` in a separate file, is shown in the following two files.</span></span> <span data-ttu-id="d02bb-135">O arquivo *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d02bb-135">The *Pages/Index2.cshtml* file:</span></span>

<span data-ttu-id="d02bb-136">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-136">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]</span></span>

<span data-ttu-id="d02bb-137">O arquivo code-behind *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d02bb-137">The *Pages/Index2.cshtml.cs* 'code-behind' file:</span></span>

<span data-ttu-id="d02bb-138">[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-138">[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]</span></span>

<span data-ttu-id="d02bb-139">Por convenção, o arquivo de classe `PageModel` tem o mesmo nome que o arquivo na Página do Razor com *.cs* acrescentado.</span><span class="sxs-lookup"><span data-stu-id="d02bb-139">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="d02bb-140">Por exemplo, a Página do Razor anterior é *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d02bb-140">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="d02bb-141">O arquivo que contém a classe `PageModel` é chamado *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="d02bb-141">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="d02bb-142">Para páginas simples, não há problemas em mesclar a classe `PageModel` com a marcação do Razor.</span><span class="sxs-lookup"><span data-stu-id="d02bb-142">For simple pages, mixing the `PageModel` class with the Razor markup is fine.</span></span> <span data-ttu-id="d02bb-143">Para código mais complexo, é uma melhor prática manter o código de modelo de página separado.</span><span class="sxs-lookup"><span data-stu-id="d02bb-143">For more complex code, it's a best practice to keep the page model code separate.</span></span>

<span data-ttu-id="d02bb-144">As associações de caminhos de URL para páginas são determinadas pelo local da página no sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="d02bb-144">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="d02bb-145">A tabela a seguir mostra um caminho de Página do Razor e a URL correspondente:</span><span class="sxs-lookup"><span data-stu-id="d02bb-145">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="d02bb-146">Caminho e nome do arquivo</span><span class="sxs-lookup"><span data-stu-id="d02bb-146">File name and path</span></span>               | <span data-ttu-id="d02bb-147">URL correspondente</span><span class="sxs-lookup"><span data-stu-id="d02bb-147">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="d02bb-148">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d02bb-148">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="d02bb-149">`/` ou `/Index`</span><span class="sxs-lookup"><span data-stu-id="d02bb-149">`/` or `/Index`</span></span> |
| <span data-ttu-id="d02bb-150">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d02bb-150">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="d02bb-151">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d02bb-151">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="d02bb-152">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d02bb-152">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="d02bb-153">`/Store` ou `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="d02bb-153">`/Store` or `/Store/Index`</span></span>  |

<span data-ttu-id="d02bb-154">Notas:</span><span class="sxs-lookup"><span data-stu-id="d02bb-154">Notes:</span></span>

* <span data-ttu-id="d02bb-155">O tempo de execução procura arquivos de Páginas do Razor na pasta *Pages* por padrão.</span><span class="sxs-lookup"><span data-stu-id="d02bb-155">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="d02bb-156">`Index` é a página padrão quando uma URL não inclui uma página.</span><span class="sxs-lookup"><span data-stu-id="d02bb-156">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="d02bb-157">Escrevendo um formulário básico</span><span class="sxs-lookup"><span data-stu-id="d02bb-157">Writing a basic form</span></span>

<span data-ttu-id="d02bb-158">Os recursos de Páginas do Razor são projetados para tornar fáceis os padrões comuns usados com navegadores da Web.</span><span class="sxs-lookup"><span data-stu-id="d02bb-158">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="d02bb-159">[Associação de modelos](xref:mvc/models/model-binding), [auxiliares de marcas](xref:mvc/views/tag-helpers/intro) e auxiliares HTML *funcionam todos apenas* com as propriedades definidas em uma classe de Página do Razor.</span><span class="sxs-lookup"><span data-stu-id="d02bb-159">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="d02bb-160">Considere uma página que implementa um formulário básico "Fale conosco" para o modelo `Contact`:</span><span class="sxs-lookup"><span data-stu-id="d02bb-160">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="d02bb-161">Para as amostras neste documento, o `DbContext` é inicializado no arquivo [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="d02bb-161">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

<span data-ttu-id="d02bb-162">[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-162">[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]</span></span>

<span data-ttu-id="d02bb-163">O modelo de dados:</span><span class="sxs-lookup"><span data-stu-id="d02bb-163">The data model:</span></span>

<span data-ttu-id="d02bb-164">[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-164">[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]</span></span>

<span data-ttu-id="d02bb-165">O arquivo de exibição *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d02bb-165">The *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="d02bb-166">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-166">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]</span></span>

<span data-ttu-id="d02bb-167">O arquivo code-behind *Pages/Create.cshtml.cs* para a exibição:</span><span class="sxs-lookup"><span data-stu-id="d02bb-167">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

<span data-ttu-id="d02bb-168">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=ALL)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-168">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=ALL)]</span></span>

<span data-ttu-id="d02bb-169">Por convenção, a classe `PageModel` é chamada de `<PageName>Model` e está no mesmo namespace que a página.</span><span class="sxs-lookup"><span data-stu-id="d02bb-169">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span> <span data-ttu-id="d02bb-170">Não é necessário fazer muitas alterações para converter de uma página usando `@functions` para definir manipuladores e uma página usando uma classe `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-170">Not much change is needed to convert from a page using `@functions` to define handlers and a page using a `PageModel` class.</span></span>

<span data-ttu-id="d02bb-171">O uso de um arquivo code-behind `PageModel` dá suporte a teste de unidade, mas exige que você grave uma classe e um construtor explícitos.</span><span class="sxs-lookup"><span data-stu-id="d02bb-171">Using a `PageModel` code-behind file supports unit testing, but requires you to write an explicit constructor and class.</span></span> <span data-ttu-id="d02bb-172">Páginas sem arquivos code-behind `PageModel` dão suporte a compilação de tempo de execução, o que pode ser uma vantagem no desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="d02bb-172">Pages without `PageModel` code-behind files support runtime compilation, which can be an advantage in development.</span></span>  <!-- review: advantage because you can make changes and refresh the browser without explicitly compiling the app -->

<span data-ttu-id="d02bb-173">A página tem um *método de manipulador* `OnPostAsync`, que é executado em solicitações `POST` (quando um usuário posta o formulário).</span><span class="sxs-lookup"><span data-stu-id="d02bb-173">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="d02bb-174">Você pode adicionar métodos de manipulador para qualquer verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="d02bb-174">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="d02bb-175">Os manipuladores mais comuns são:</span><span class="sxs-lookup"><span data-stu-id="d02bb-175">The most common handlers are:</span></span>

* <span data-ttu-id="d02bb-176">`OnGet` para inicializar o estado necessário para a página.</span><span class="sxs-lookup"><span data-stu-id="d02bb-176">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="d02bb-177">Amostra de [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="d02bb-177">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="d02bb-178">`OnPost` para manipular envios de formulário.</span><span class="sxs-lookup"><span data-stu-id="d02bb-178">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="d02bb-179">O sufixo de nomenclatura `Async` é opcional, mas geralmente é usado por convenção para funções assíncronas.</span><span class="sxs-lookup"><span data-stu-id="d02bb-179">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="d02bb-180">O código `OnPostAsync` no exemplo anterior tem aparência semelhante ao que você normalmente escreve em um controlador.</span><span class="sxs-lookup"><span data-stu-id="d02bb-180">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="d02bb-181">O código anterior é comum para as Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="d02bb-181">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="d02bb-182">A maioria dos primitivos MVC como [associação de modelos](xref:mvc/models/model-binding), [validação](xref:mvc/models/validation) e resultados da ação são compartilhados.</span><span class="sxs-lookup"><span data-stu-id="d02bb-182">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="d02bb-183">O método `OnPostAsync` anterior:</span><span class="sxs-lookup"><span data-stu-id="d02bb-183">The previous `OnPostAsync` method:</span></span>

<span data-ttu-id="d02bb-184">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-184">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync)]</span></span>

<span data-ttu-id="d02bb-185">O fluxo básico de `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="d02bb-185">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="d02bb-186">Verifique se há erros de validação.</span><span class="sxs-lookup"><span data-stu-id="d02bb-186">Check for validation errors.</span></span>

*  <span data-ttu-id="d02bb-187">Se não houver nenhum erro, salve os dados e redirecione.</span><span class="sxs-lookup"><span data-stu-id="d02bb-187">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="d02bb-188">Se houver erros, mostre a página novamente com as mensagens de validação.</span><span class="sxs-lookup"><span data-stu-id="d02bb-188">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="d02bb-189">A validação do lado do cliente é idêntica para aplicativos ASP.NET Core MVC tradicionais.</span><span class="sxs-lookup"><span data-stu-id="d02bb-189">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="d02bb-190">Em muitos casos, erros de validação seriam detectados no cliente e nunca enviados ao servidor.</span><span class="sxs-lookup"><span data-stu-id="d02bb-190">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="d02bb-191">Quando os dados são inseridos com êxito, o método de manipulador `OnPostAsync` chama o método auxiliar `RedirectToPage` para retornar uma instância de `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-191">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="d02bb-192">`RedirectToPage` é um novo resultado de ação, semelhante a `RedirectToAction` ou `RedirectToRoute`, mas personalizado para páginas.</span><span class="sxs-lookup"><span data-stu-id="d02bb-192">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="d02bb-193">Na amostra anterior, ele redireciona para a página de Índice raiz (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="d02bb-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="d02bb-194">`RedirectToPage` é descrito em detalhes na seção [Geração de URLs para páginas](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="d02bb-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="d02bb-195">Quando o formulário enviado tem erros de validação (que são passados para o servidor), o método de manipulador `OnPostAsync` chama o método auxiliar `Page`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-195">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="d02bb-196">`Page` retorna uma instância de `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-196">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="d02bb-197">Retornar `Page` é semelhante a como as ações em controladores retornam `View`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-197">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="d02bb-198">`PageResult` é o tipo de retorno <!-- Review  --> padrão para um método de manipulador.</span><span class="sxs-lookup"><span data-stu-id="d02bb-198">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="d02bb-199">Um método de manipulador que retorna `void` renderiza a página.</span><span class="sxs-lookup"><span data-stu-id="d02bb-199">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="d02bb-200">A propriedade `Customer` usa o atributo `[BindProperty]` para aceitar a associação de modelos.</span><span class="sxs-lookup"><span data-stu-id="d02bb-200">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

<span data-ttu-id="d02bb-201">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=PageModel&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-201">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=PageModel&highlight=10-11)]</span></span>

<span data-ttu-id="d02bb-202">Páginas do Razor, por padrão, associam as propriedades somente com verbos não GET.</span><span class="sxs-lookup"><span data-stu-id="d02bb-202">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="d02bb-203">A associação de propriedades pode reduzir a quantidade de código que você precisa escrever.</span><span class="sxs-lookup"><span data-stu-id="d02bb-203">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="d02bb-204">A associação reduz o código usando a mesma propriedade para renderizar os campos de formulário (`<input asp-for="Customer.Name" />`) e aceitar a entrada.</span><span class="sxs-lookup"><span data-stu-id="d02bb-204">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="d02bb-205">O código a seguir mostra a versão combinada da página de criação:</span><span class="sxs-lookup"><span data-stu-id="d02bb-205">The following code shows the combined version of the create page:</span></span>

<span data-ttu-id="d02bb-206">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/CreateCombined.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-206">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/CreateCombined.cshtml)]</span></span>

<span data-ttu-id="d02bb-207">Em vez de usar `@model`, estamos usufruindo de um novo recurso para Páginas.</span><span class="sxs-lookup"><span data-stu-id="d02bb-207">Rather than using `@model`, we're taking advantage of a new feature for Pages.</span></span> <span data-ttu-id="d02bb-208">Por padrão, a classe derivada de `Page` gerada *é* o modelo.</span><span class="sxs-lookup"><span data-stu-id="d02bb-208">By default, the generated `Page`-derived class *is* the model.</span></span> <span data-ttu-id="d02bb-209">Usar um *modelo de exibição* com exibições do Razor é uma melhor prática.</span><span class="sxs-lookup"><span data-stu-id="d02bb-209">Using a *view model* with Razor views is a best practice.</span></span> <span data-ttu-id="d02bb-210">Com Páginas, você obtém um modelo de exibição *automaticamente*.</span><span class="sxs-lookup"><span data-stu-id="d02bb-210">With Pages, you get a view model *automatically*.</span></span>

<span data-ttu-id="d02bb-211">A principal mudança é substituir a injeção de construtor com propriedades injetadas (`@inject`).</span><span class="sxs-lookup"><span data-stu-id="d02bb-211">The main change is replacing constructor injection with injected (`@inject`) properties.</span></span> <span data-ttu-id="d02bb-212">Essa página usa [@inject](xref:mvc/views/razor#inject) para [injeção de dependência de construtor](xref:mvc/controllers/dependency-injection#constructor-injection).</span><span class="sxs-lookup"><span data-stu-id="d02bb-212">This page uses [@inject](xref:mvc/views/razor#inject) for [constructor dependency injection](xref:mvc/controllers/dependency-injection#constructor-injection).</span></span> <span data-ttu-id="d02bb-213">A instrução `@inject` gera e inicializa a propriedade, `Db` que é usada em `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-213">The `@inject` statement generates and initializes the `Db` property that is used in `OnPostAsync`.</span></span> <span data-ttu-id="d02bb-214">As propriedades injetadas (`@inject`) são definidas antes de métodos de manipulador serem executados.</span><span class="sxs-lookup"><span data-stu-id="d02bb-214">Injected (`@inject`) properties are set before handler methods run.</span></span>


<span data-ttu-id="d02bb-215">A home page (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="d02bb-215">The home page (*Index.cshtml*):</span></span>

<span data-ttu-id="d02bb-216">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-216">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]</span></span>

<span data-ttu-id="d02bb-217">O arquivo code-behind *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d02bb-217">The code behind *Index.cshtml.cs* file:</span></span>

<span data-ttu-id="d02bb-218">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-218">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]</span></span>

<span data-ttu-id="d02bb-219">O arquivo *cshtml* contém a marcação a seguir para criar um link de edição para cada contato:</span><span class="sxs-lookup"><span data-stu-id="d02bb-219">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

```html
<a asp-page="./Edit" asp-route-id="@contact.Id">edit</a>
```

<span data-ttu-id="d02bb-220">O [auxiliar de marcas de âncora](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) usou o atributo [asp-route-{valor}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route) para gerar um link para a página Edit.</span><span class="sxs-lookup"><span data-stu-id="d02bb-220">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) used the [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route) attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="d02bb-221">O link contém dados de rota com a ID de contato.</span><span class="sxs-lookup"><span data-stu-id="d02bb-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="d02bb-222">Por exemplo, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-222">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="d02bb-223">O arquivo *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d02bb-223">The *Pages/Edit.cshtml* file:</span></span>

<span data-ttu-id="d02bb-224">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-224">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]</span></span>

<span data-ttu-id="d02bb-225">A primeira linha contém a diretiva `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-225">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="d02bb-226">A restrição de roteamento `"{id:int}"` informa à página para aceitar solicitações para a página que contêm dados da rota `int`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-226">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="d02bb-227">Se uma solicitação para a página não contém dados de rota que podem ser convertidos em um `int`, o tempo de execução retorna um erro HTTP 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="d02bb-227">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="d02bb-228">O arquivo *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d02bb-228">The *Pages/Edit.cshtml.cs* file:</span></span>

<span data-ttu-id="d02bb-229">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-229">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="d02bb-230">XSRF/CSRF e Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="d02bb-230">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="d02bb-231">Você não precisa escrever nenhum código para [validação antifalsificação](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="d02bb-231">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="d02bb-232">Validação e geração de token antifalsificação são automaticamente incluídas nas Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="d02bb-232">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="d02bb-233">Usando Layouts, parciais, modelos e auxiliares de marcas com Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="d02bb-233">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="d02bb-234">As Páginas funcionam com todos os recursos do mecanismo de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="d02bb-234">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="d02bb-235">Layouts, parciais, modelos, auxiliares de marcas, *_ViewStart.cshtml* e *_ViewImports.cshtml* funcionam da mesma forma que funcionam exibições convencionais do Razor.</span><span class="sxs-lookup"><span data-stu-id="d02bb-235">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="d02bb-236">Organizaremos essa página aproveitando alguns desses recursos.</span><span class="sxs-lookup"><span data-stu-id="d02bb-236">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="d02bb-237">Adicione uma [página de layout](xref:mvc/views/layout) a *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d02bb-237">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

<span data-ttu-id="d02bb-238">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-238">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]</span></span>

<span data-ttu-id="d02bb-239">O [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="d02bb-239">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="d02bb-240">Controla o layout de cada página (a menos que a página opte por não usar o layout).</span><span class="sxs-lookup"><span data-stu-id="d02bb-240">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="d02bb-241">Importa estruturas HTML como JavaScript e folhas de estilo.</span><span class="sxs-lookup"><span data-stu-id="d02bb-241">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="d02bb-242">Veja [página de layout](xref:mvc/views/layout) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="d02bb-242">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="d02bb-243">A propriedade [Layout](xref:mvc/views/layout#specifying-a-layout) é definida em *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d02bb-243">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

<span data-ttu-id="d02bb-244">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-244">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]</span></span>

<span data-ttu-id="d02bb-245">Observação: o layout está na pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="d02bb-245">Note: The layout is in the *Pages* folder.</span></span> <span data-ttu-id="d02bb-246">As páginas buscam outras exibições (layouts, modelos, parciais) hierarquicamente, iniciando na mesma pasta que a página atual.</span><span class="sxs-lookup"><span data-stu-id="d02bb-246">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="d02bb-247">Um layout na pasta *Pages* pode ser usado em qualquer Página do Razor na pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="d02bb-247">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="d02bb-248">Recomendamos que você **não** coloque o arquivo de layout na pasta *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="d02bb-248">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="d02bb-249">*Views/Shared* é um padrão de exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="d02bb-249">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="d02bb-250">As Páginas do Razor devem confiar na hierarquia de pasta e não nas convenções de caminho.</span><span class="sxs-lookup"><span data-stu-id="d02bb-250">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="d02bb-251">A pesquisa de modo de exibição de uma Página do Razor inclui a pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="d02bb-251">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="d02bb-252">Os layouts, modelos e parciais que você está usando com controladores MVC e exibições do Razor convencionais *apenas funcionam*.</span><span class="sxs-lookup"><span data-stu-id="d02bb-252">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="d02bb-253">Adicione um arquivo *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d02bb-253">Add a *Pages/_ViewImports.cshtml* file:</span></span>

<span data-ttu-id="d02bb-254">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-254">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="d02bb-255">`@namespace` é explicado posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="d02bb-255">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="d02bb-256">A diretiva `@addTagHelper` coloca os [auxiliares de marcas internos](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/) em todas as páginas na pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="d02bb-256">The `@addTagHelper` directive brings in the [built-in Tag Helpers](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="d02bb-257">Quando a diretiva `@namespace` é usada explicitamente em uma página:</span><span class="sxs-lookup"><span data-stu-id="d02bb-257">When the `@namespace` directive is used explicitly on a page:</span></span>

<span data-ttu-id="d02bb-258">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-258">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]</span></span>

<span data-ttu-id="d02bb-259">A diretiva define o namespace da página.</span><span class="sxs-lookup"><span data-stu-id="d02bb-259">The directive sets the namespace for the page.</span></span> <span data-ttu-id="d02bb-260">A diretiva `@model` não precisa incluir o namespace.</span><span class="sxs-lookup"><span data-stu-id="d02bb-260">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="d02bb-261">Quando a diretiva `@namespace` está contida em *_ViewImports.cshtml*, o namespace especificado fornece o prefixo do namespace gerado na página que importa a diretiva `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-261">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="d02bb-262">O restante do namespace gerado (a parte do sufixo) é o caminho relativo separado por ponto entre a pasta que contém *_ViewImports.cshtml* e a pasta que contém a página.</span><span class="sxs-lookup"><span data-stu-id="d02bb-262">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="d02bb-263">Por exemplo, o arquivo code-behind *Pages/Customers/Edit.cshtml.cs* define explicitamente o namespace:</span><span class="sxs-lookup"><span data-stu-id="d02bb-263">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

<span data-ttu-id="d02bb-264">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=namespace)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-264">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=namespace)]</span></span>

<span data-ttu-id="d02bb-265">O arquivo *Pages/_ViewImports.cshtml* define o namespace a seguir:</span><span class="sxs-lookup"><span data-stu-id="d02bb-265">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

<span data-ttu-id="d02bb-266">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-266">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]</span></span>

<span data-ttu-id="d02bb-267">O namespace gerado para a Página do Razor *Pages/Customers/Edit.cshtml* é o mesmo que o do arquivo code-behind.</span><span class="sxs-lookup"><span data-stu-id="d02bb-267">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="d02bb-268">A diretiva `@namespace` foi projetada de modo que as classes C# adicionadas a um projeto e o código gerado pelas páginas *funcione* sem a necessidade de adicionar uma diretiva `@using` para o arquivo code-behind.</span><span class="sxs-lookup"><span data-stu-id="d02bb-268">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="d02bb-269">Observação: `@namespace` também funciona com exibições do Razor convencionais.</span><span class="sxs-lookup"><span data-stu-id="d02bb-269">Note: `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="d02bb-270">O arquivo de exibição *Pages/Create.cshtml* original:</span><span class="sxs-lookup"><span data-stu-id="d02bb-270">The original *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="d02bb-271">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-271">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]</span></span>

<span data-ttu-id="d02bb-272">A página atualizada:</span><span class="sxs-lookup"><span data-stu-id="d02bb-272">The updated page:</span></span>

<span data-ttu-id="d02bb-273">O arquivo de exibição *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d02bb-273">The *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="d02bb-274">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-274">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]</span></span>

<span data-ttu-id="d02bb-275">O [projeto inicial de Páginas do Razor](#rpvs17) contém o *Pages/_ValidationScriptsPartial.cshtml*, que conecta a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="d02bb-275">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="d02bb-276">Geração de URL para Páginas</span><span class="sxs-lookup"><span data-stu-id="d02bb-276">URL generation for Pages</span></span>

<span data-ttu-id="d02bb-277">A página `Create`, exibida anteriormente, usa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="d02bb-277">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

<span data-ttu-id="d02bb-278">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-278">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync&highlight=10)]</span></span>

<span data-ttu-id="d02bb-279">O aplicativo tem a estrutura de arquivos/pastas a seguir</span><span class="sxs-lookup"><span data-stu-id="d02bb-279">The app has the following file/folder structure</span></span>

* <span data-ttu-id="d02bb-280">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="d02bb-280">*/Pages*</span></span>

  * <span data-ttu-id="d02bb-281">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d02bb-281">*Index.cshtml*</span></span>
  * <span data-ttu-id="d02bb-282">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="d02bb-282">*/Customer*</span></span>

    * <span data-ttu-id="d02bb-283">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d02bb-283">*Create.cshtml*</span></span>
    * <span data-ttu-id="d02bb-284">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d02bb-284">*Edit.cshtml*</span></span>
    * <span data-ttu-id="d02bb-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d02bb-285">*Index.cshtml*</span></span>

<span data-ttu-id="d02bb-286">As páginas *Pages/Customers/Create.cshtml* e *Pages/Customers/Edit.cshtml* redirecionam para o *Pages/Index.cshtml* após êxito.</span><span class="sxs-lookup"><span data-stu-id="d02bb-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="d02bb-287">A cadeia de caracteres `/Index` faz parte do URI para acessar a página anterior.</span><span class="sxs-lookup"><span data-stu-id="d02bb-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="d02bb-288">A cadeia de caracteres `/Index` pode ser usada para gerar URIs para a página *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d02bb-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="d02bb-289">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d02bb-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="d02bb-290">O nome da página é o caminho para a página da pasta raiz */Pages* (incluindo um `/` à direita, por exemplo, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="d02bb-290">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="d02bb-291">As amostras anteriores de geração de URL são muito mais ricas em recursos do que apenas codificar uma URL.</span><span class="sxs-lookup"><span data-stu-id="d02bb-291">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="d02bb-292">A geração de URL usa [roteamento](xref:mvc/controllers/routing) e pode gerar e codificar parâmetros de acordo com o modo como a rota é definida no caminho de destino.</span><span class="sxs-lookup"><span data-stu-id="d02bb-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="d02bb-293">A Geração de URL para páginas dá suporte a nomes relativos.</span><span class="sxs-lookup"><span data-stu-id="d02bb-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="d02bb-294">A tabela a seguir mostra qual página de Índice é selecionada com diferentes parâmetros `RedirectToPage` de *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d02bb-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="d02bb-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="d02bb-295">RedirectToPage(x)</span></span>| <span data-ttu-id="d02bb-296">Página</span><span class="sxs-lookup"><span data-stu-id="d02bb-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="d02bb-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="d02bb-297">RedirectToPage("/Index")</span></span> | <span data-ttu-id="d02bb-298">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="d02bb-298">*Pages/Index*</span></span> |
| <span data-ttu-id="d02bb-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="d02bb-299">RedirectToPage("./Index");</span></span> | <span data-ttu-id="d02bb-300">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="d02bb-300">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="d02bb-301">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="d02bb-301">RedirectToPage("../Index")</span></span> | <span data-ttu-id="d02bb-302">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="d02bb-302">*Pages/Index*</span></span> |
| <span data-ttu-id="d02bb-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="d02bb-303">RedirectToPage("Index")</span></span>  | <span data-ttu-id="d02bb-304">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="d02bb-304">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="d02bb-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")` e `RedirectToPage("../Index")` são *nomes relativos*.</span><span class="sxs-lookup"><span data-stu-id="d02bb-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="d02bb-306">O parâmetro `RedirectToPage` é *combinado* com o caminho da página atual para calcular o nome da página de destino.</span><span class="sxs-lookup"><span data-stu-id="d02bb-306">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="d02bb-307">Vinculação de nome relativo é útil ao criar sites com uma estrutura complexa.</span><span class="sxs-lookup"><span data-stu-id="d02bb-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="d02bb-308">Se você usar nomes relativos para vincular entre páginas em uma pasta, você poderá renomear essa pasta.</span><span class="sxs-lookup"><span data-stu-id="d02bb-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="d02bb-309">Todos os links ainda funcionarão (porque eles não incluirão o nome da pasta).</span><span class="sxs-lookup"><span data-stu-id="d02bb-309">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="d02bb-310">TempData</span><span class="sxs-lookup"><span data-stu-id="d02bb-310">TempData</span></span>

<span data-ttu-id="d02bb-311">O ASP.NET Core expõe a propriedade [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) em um [controlador](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="d02bb-311">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="d02bb-312">Essa propriedade armazena dados até eles serem lidos.</span><span class="sxs-lookup"><span data-stu-id="d02bb-312">This property stores data until it is read.</span></span> <span data-ttu-id="d02bb-313">Os métodos `Keep` e `Peek` podem ser usados para examinar os dados sem exclusão.</span><span class="sxs-lookup"><span data-stu-id="d02bb-313">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="d02bb-314">`TempData` é útil para redirecionamento nos casos em que os dados são necessários para mais de uma única solicitação.</span><span class="sxs-lookup"><span data-stu-id="d02bb-314">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="d02bb-315">O atributo `[TempData]` é novo no ASP.NET Core 2.0 e tem suporte em controladores e páginas.</span><span class="sxs-lookup"><span data-stu-id="d02bb-315">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="d02bb-316">Os conjuntos de código a seguir definem o valor de `Message` usando `TempData`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-316">The following code sets the value of `Message` using `TempData`.</span></span>
<span data-ttu-id="d02bb-317">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,27-28&name=snippetTemp)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-317">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,27-28&name=snippetTemp)]</span></span>

<span data-ttu-id="d02bb-318">A marcação a seguir no arquivo *Pages/Customers/Index.cshtml* exibe o valor de `Message` usando `TempData`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-318">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="d02bb-319">O arquivo code-behind *Pages/Customers/Index.cshtml.cs* aplica o atributo `[TempData]` à propriedade `Message`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-319">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="d02bb-320">Consulte [TempData](xref:fundamentals/app-state#temp) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="d02bb-320">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="d02bb-321">Vários manipuladores por página</span><span class="sxs-lookup"><span data-stu-id="d02bb-321">Multiple handlers per page</span></span>

<span data-ttu-id="d02bb-322">A página a seguir gera marcação para dois manipuladores de página usando o auxiliar de marcas `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="d02bb-322">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

<span data-ttu-id="d02bb-323">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-323">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span></span>

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="d02bb-324">O formulário no exemplo anterior tem dois botões de envio, cada um usando o `FormActionTagHelper` para enviar para uma URL diferente.</span><span class="sxs-lookup"><span data-stu-id="d02bb-324">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="d02bb-325">O atributo `asp-page-handler` é um complemento para `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-325">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="d02bb-326">`asp-page-handler` gera URLs que enviam para cada um dos métodos de manipulador definidos por uma página.</span><span class="sxs-lookup"><span data-stu-id="d02bb-326">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="d02bb-327">`asp-page` não foi especificado porque a amostra está vinculando à página atual.</span><span class="sxs-lookup"><span data-stu-id="d02bb-327">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="d02bb-328">O arquivo code-behind:</span><span class="sxs-lookup"><span data-stu-id="d02bb-328">The code-behind file:</span></span>

<span data-ttu-id="d02bb-329">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-329">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]</span></span>

<span data-ttu-id="d02bb-330">O código anterior usa *métodos de manipulador nomeados*.</span><span class="sxs-lookup"><span data-stu-id="d02bb-330">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="d02bb-331">Métodos de manipulador nomeados são criados colocando o texto no nome após `On<HTTP Verb>` e antes de `Async` (se houver).</span><span class="sxs-lookup"><span data-stu-id="d02bb-331">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="d02bb-332">No exemplo anterior, os métodos de página são OnPost**JoinList**Async e OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="d02bb-332">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="d02bb-333">Com *OnPost* e *Async* removidos, os nomes de manipulador são `JoinList` e `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-333">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

<span data-ttu-id="d02bb-334">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-334">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span></span>

<span data-ttu-id="d02bb-335">Usando o código anterior, o caminho da URL que envia a `OnPostJoinListAsync` é `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-335">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="d02bb-336">O caminho da URL que envia a `OnPostJoinListUCAsync` é `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-336">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="d02bb-337">Personalizando o roteamento</span><span class="sxs-lookup"><span data-stu-id="d02bb-337">Customizing Routing</span></span>

<span data-ttu-id="d02bb-338">Se você não deseja a cadeia de consulta `?handler=JoinList` na URL, você pode alterar a rota para colocar o nome do manipulador na parte do caminho da URL.</span><span class="sxs-lookup"><span data-stu-id="d02bb-338">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="d02bb-339">Você pode personalizar a rota adicionando um modelo de rota entre aspas duplas após a diretiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="d02bb-339">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

<span data-ttu-id="d02bb-340">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-340">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]</span></span>


<span data-ttu-id="d02bb-341">A rota anterior coloca o nome do manipulador no caminho da URL em vez da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="d02bb-341">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="d02bb-342">O `?` após `handler` significa que o parâmetro de rota é opcional.</span><span class="sxs-lookup"><span data-stu-id="d02bb-342">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="d02bb-343">Você pode usar `@page` para adicionar parâmetros e segmentos adicionais a uma rota de página.</span><span class="sxs-lookup"><span data-stu-id="d02bb-343">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="d02bb-344">Tudo que está lá está **acrescentado** à rota padrão da página.</span><span class="sxs-lookup"><span data-stu-id="d02bb-344">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="d02bb-345">Não há suporte para o uso de um caminho absoluto ou virtual para alterar a rota da página (como `"~/Some/Other/Path"`).</span><span class="sxs-lookup"><span data-stu-id="d02bb-345">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="d02bb-346">Configuração e definições</span><span class="sxs-lookup"><span data-stu-id="d02bb-346">Configuration and settings</span></span>

<span data-ttu-id="d02bb-347">Para configurar opções avançadas, use o método de extensão `AddRazorPagesOptions` no construtor de MVC:</span><span class="sxs-lookup"><span data-stu-id="d02bb-347">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

<span data-ttu-id="d02bb-348">[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d02bb-348">[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet1)]</span></span>

<span data-ttu-id="d02bb-349">No momento, você pode usar o `RazorPagesOptions` para definir o diretório raiz para páginas ou adicionar as convenções de modelo de aplicativo para páginas.</span><span class="sxs-lookup"><span data-stu-id="d02bb-349">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="d02bb-350">Esperamos habilitar mais extensibilidade dessa maneira no futuro.</span><span class="sxs-lookup"><span data-stu-id="d02bb-350">We hope to enable more extensibility this way in the future.</span></span>

<span data-ttu-id="d02bb-351">Para pré-compilar exibições, consulte [Compilação de exibição do Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="d02bb-351">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="d02bb-352">[Baixar ou exibir código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="d02bb-352">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="d02bb-353">Consulte [Introdução a Páginas do Razor no ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), que se baseia nesta introdução.</span><span class="sxs-lookup"><span data-stu-id="d02bb-353">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>
