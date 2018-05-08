---
title: Introdução a Páginas do Razor no ASP.NET Core
author: Rick-Anderson
description: Saiba como as Páginas Razor no ASP.NET Core tornam a codificação de cenários centrados em página mais fácil e mais produtiva do que com o uso de MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: 08866543d5b510b86c6af1896a9bd41ae0053ecf
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="0a0b6-103">Introdução a Páginas do Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a0b6-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="0a0b6-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="0a0b6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="0a0b6-105">Páginas do Razor é um novo recurso do ASP.NET Core MVC que torna a codificação de cenários focados em página mais fácil e produtiva.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-105">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="0a0b6-106">Se você estiver procurando um tutorial que utiliza a abordagem Modelo-Exibição-Controlador, consulte a [Introdução ao ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="0a0b6-107">Este documento proporciona uma introdução a páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="0a0b6-108">Este não é um tutorial passo a passo.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="0a0b6-109">Se você achar que algumas das seções são muito avançadas, consulte a [Introdução a Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="0a0b6-110">Para obter uma visão geral do ASP.NET Core, consulte a [Introdução ao ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a0b6-111">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="0a0b6-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="0a0b6-112">Criando um projeto de Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="0a0b6-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a0b6-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a0b6-113">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0a0b6-114">Consulte a [Introdução a Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start) para obter instruções detalhadas sobre como criar um projeto de Páginas do Razor usando o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a0b6-115">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="0a0b6-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0a0b6-116">Da linha de comando, execute `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-116">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="0a0b6-117">Abra o arquivo *.csproj* gerado do Visual Studio para Mac.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-117">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a0b6-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a0b6-118">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="0a0b6-119">Da linha de comando, execute `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-119">Run `dotnet new razor` from the command line.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0a0b6-120">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="0a0b6-120">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="0a0b6-121">Da linha de comando, execute `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-121">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="0a0b6-122">Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="0a0b6-122">Razor Pages</span></span>

<span data-ttu-id="0a0b6-123">O Páginas do Razor está habilitado em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-123">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="0a0b6-124">Considere uma página básica: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="0a0b6-124">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="0a0b6-125">O código anterior é muito parecido com um arquivo de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-125">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="0a0b6-126">O que o torna diferentes é a diretiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-126">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="0a0b6-127">`@page` transforma o arquivo em uma ação do MVC – o que significa que ele trata solicitações diretamente, sem passar por um controlador.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-127">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="0a0b6-128">`@page` deve ser a primeira diretiva do Razor em uma página.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-128">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="0a0b6-129">`@page` afeta o comportamento de outros constructos do Razor.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-129">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="0a0b6-130">Uma página semelhante, usando uma classe `PageModel`, é mostrada nos dois arquivos a seguir.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-130">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="0a0b6-131">O arquivo *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-131">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="0a0b6-132">O modelo de página *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-132">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="0a0b6-133">Por convenção, o arquivo de classe `PageModel` tem o mesmo nome que o arquivo na Página do Razor com *.cs* acrescentado.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-133">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="0a0b6-134">Por exemplo, a Página do Razor anterior é *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-134">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="0a0b6-135">O arquivo que contém a classe `PageModel` é chamado *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-135">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="0a0b6-136">As associações de caminhos de URL para páginas são determinadas pelo local da página no sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-136">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="0a0b6-137">A tabela a seguir mostra um caminho de Página do Razor e a URL correspondente:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-137">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="0a0b6-138">Caminho e nome do arquivo</span><span class="sxs-lookup"><span data-stu-id="0a0b6-138">File name and path</span></span>               | <span data-ttu-id="0a0b6-139">URL correspondente</span><span class="sxs-lookup"><span data-stu-id="0a0b6-139">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="0a0b6-140">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-140">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="0a0b6-141">`/` ou `/Index`</span><span class="sxs-lookup"><span data-stu-id="0a0b6-141">`/` or `/Index`</span></span> |
| <span data-ttu-id="0a0b6-142">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-142">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="0a0b6-143">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-143">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="0a0b6-144">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-144">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="0a0b6-145">`/Store` ou `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="0a0b6-145">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="0a0b6-146">Notas:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-146">Notes:</span></span>

* <span data-ttu-id="0a0b6-147">O tempo de execução procura arquivos de Páginas do Razor na pasta *Pages* por padrão.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-147">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="0a0b6-148">`Index` é a página padrão quando uma URL não inclui uma página.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-148">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="0a0b6-149">Escrevendo um formulário básico</span><span class="sxs-lookup"><span data-stu-id="0a0b6-149">Writing a basic form</span></span>

<span data-ttu-id="0a0b6-150">Os recursos de Páginas do Razor são projetados para tornar fáceis os padrões comuns usados com navegadores da Web.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-150">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="0a0b6-151">[Associação de modelos](xref:mvc/models/model-binding), [auxiliares de marcas](xref:mvc/views/tag-helpers/intro) e auxiliares HTML *funcionam todos apenas* com as propriedades definidas em uma classe de Página do Razor.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-151">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="0a0b6-152">Considere uma página que implementa um formulário básico "Fale conosco" para o modelo `Contact`:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-152">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="0a0b6-153">Para as amostras neste documento, o `DbContext` é inicializado no arquivo [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-153">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="0a0b6-154">O modelo de dados:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-154">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="0a0b6-155">O contexto do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-155">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="0a0b6-156">O arquivo de exibição *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-156">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="0a0b6-157">O modelo de página *Pages/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-157">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="0a0b6-158">Por convenção, a classe `PageModel` é chamada de `<PageName>Model` e está no mesmo namespace que a página.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-158">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="0a0b6-159">A classe `PageModel` permite separar a lógica de uma página da respectiva apresentação.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-159">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="0a0b6-160">Ela define manipuladores para as solicitações enviadas e os dados usados para renderizar a página.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-160">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="0a0b6-161">Esta separação permite gerenciar as dependências da página por meio de [injeção de dependência](xref:fundamentals/dependency-injection) e realizar um [teste de unidade](xref:testing/razor-pages-testing) nas páginas.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-161">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="0a0b6-162">A página tem um *método de manipulador* `OnPostAsync`, que é executado em solicitações `POST` (quando um usuário posta o formulário).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-162">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="0a0b6-163">Você pode adicionar métodos de manipulador para qualquer verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-163">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="0a0b6-164">Os manipuladores mais comuns são:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-164">The most common handlers are:</span></span>

* <span data-ttu-id="0a0b6-165">`OnGet` para inicializar o estado necessário para a página.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-165">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="0a0b6-166">Amostra de [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-166">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="0a0b6-167">`OnPost` para manipular envios de formulário.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-167">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="0a0b6-168">O sufixo de nomenclatura `Async` é opcional, mas geralmente é usado por convenção para funções assíncronas.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-168">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="0a0b6-169">O código `OnPostAsync` no exemplo anterior tem aparência semelhante ao que você normalmente escreve em um controlador.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-169">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="0a0b6-170">O código anterior é comum para as Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-170">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="0a0b6-171">A maioria dos primitivos MVC como [associação de modelos](xref:mvc/models/model-binding), [validação](xref:mvc/models/validation) e resultados da ação são compartilhados.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-171">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="0a0b6-172">O método `OnPostAsync` anterior:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-172">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="0a0b6-173">O fluxo básico de `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-173">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="0a0b6-174">Verifique se há erros de validação.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-174">Check for validation errors.</span></span>

*  <span data-ttu-id="0a0b6-175">Se não houver nenhum erro, salve os dados e redirecione.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-175">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="0a0b6-176">Se houver erros, mostre a página novamente com as mensagens de validação.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-176">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="0a0b6-177">A validação do lado do cliente é idêntica para aplicativos ASP.NET Core MVC tradicionais.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-177">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="0a0b6-178">Em muitos casos, erros de validação seriam detectados no cliente e nunca enviados ao servidor.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-178">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="0a0b6-179">Quando os dados são inseridos com êxito, o método de manipulador `OnPostAsync` chama o método auxiliar `RedirectToPage` para retornar uma instância de `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-179">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="0a0b6-180">`RedirectToPage` é um novo resultado de ação, semelhante a `RedirectToAction` ou `RedirectToRoute`, mas personalizado para páginas.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-180">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="0a0b6-181">Na amostra anterior, ele redireciona para a página de Índice raiz (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-181">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="0a0b6-182">`RedirectToPage` é descrito em detalhes na seção [Geração de URLs para páginas](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-182">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="0a0b6-183">Quando o formulário enviado tem erros de validação (que são passados para o servidor), o método de manipulador `OnPostAsync` chama o método auxiliar `Page`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-183">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="0a0b6-184">`Page` retorna uma instância de `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-184">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="0a0b6-185">Retornar `Page` é semelhante a como as ações em controladores retornam `View`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-185">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="0a0b6-186">`PageResult` é o tipo de retorno <!-- Review  --> padrão para um método de manipulador.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-186">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="0a0b6-187">Um método de manipulador que retorna `void` renderiza a página.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-187">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="0a0b6-188">A propriedade `Customer` usa o atributo `[BindProperty]` para aceitar a associação de modelos.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-188">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="0a0b6-189">Páginas do Razor, por padrão, associam as propriedades somente com verbos não GET.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-189">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="0a0b6-190">A associação de propriedades pode reduzir a quantidade de código que você precisa escrever.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-190">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="0a0b6-191">A associação reduz o código usando a mesma propriedade para renderizar os campos de formulário (`<input asp-for="Customer.Name" />`) e aceitar a entrada.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-191">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="0a0b6-192">Por motivos de segurança, você deve optar por associar os dados da solicitação GET às propriedades do modelo de página.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-192">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="0a0b6-193">Verifique a entrada do usuário antes de mapeá-la para as propriedades.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-193">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="0a0b6-194">Aceitar esse comportamento é útil quando você cria recursos que contam com a cadeia de caracteres de consulta ou com os valores de rota.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-194">Opting in to this behavior is useful when building features which rely on query string or route values.</span></span>
>
> <span data-ttu-id="0a0b6-195">Para associar uma propriedade às solicitações GET, defina a propriedade `SupportsGet` do atributo `[BindProperty]` como `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="0a0b6-195">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="0a0b6-196">A home page (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="0a0b6-196">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="0a0b6-197">O arquivo code-behind *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-197">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="0a0b6-198">O arquivo *cshtml* contém a marcação a seguir para criar um link de edição para cada contato:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-198">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="0a0b6-199">O [auxiliar de marcas de âncora](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) usou o atributo `asp-route-{value}` para gerar um link para a página Edit.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-199">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="0a0b6-200">O link contém dados de rota com a ID de contato.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-200">The link contains route data with the contact ID.</span></span> <span data-ttu-id="0a0b6-201">Por exemplo, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-201">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="0a0b6-202">O arquivo *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-202">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="0a0b6-203">A primeira linha contém a diretiva `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-203">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="0a0b6-204">A restrição de roteamento `"{id:int}"` informa à página para aceitar solicitações para a página que contêm dados da rota `int`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-204">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="0a0b6-205">Se uma solicitação para a página não contém dados de rota que podem ser convertidos em um `int`, o tempo de execução retorna um erro HTTP 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-205">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="0a0b6-206">Para tornar a ID opcional, acrescente `?` à restrição de rota:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-206">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="0a0b6-207">O arquivo *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-207">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="0a0b6-208">O arquivo *Index.cshtml* também contém a marcação para criar um botão de exclusão para cada contato de cliente:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-208">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="0a0b6-209">Quando o botão de exclusão é renderizado em HTML, seu `formaction` inclui parâmetros para:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-209">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="0a0b6-210">A ID de contato do cliente especificada pelo atributo `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-210">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="0a0b6-211">O `handler` especificado pelo atributo `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-211">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="0a0b6-212">Este é um exemplo de um botão de exclusão renderizado com uma ID de contato do cliente de `1`:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-212">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="0a0b6-213">Quando o botão é selecionado, uma solicitação de formulário `POST` é enviada para o servidor.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-213">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="0a0b6-214">Por convenção, o nome do método do manipulador é selecionado com base no valor do parâmetro `handler` de acordo com o esquema `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-214">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="0a0b6-215">Como o `handler` é `delete` neste exemplo, o método do manipulador `OnPostDeleteAsync` é usado para processar a solicitação `POST`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-215">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="0a0b6-216">Se o `asp-page-handler` for definido como um valor diferente, como `remove`, um método de manipulador de página com o nome `OnPostRemoveAsync` será selecionado.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-216">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="0a0b6-217">O método `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-217">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="0a0b6-218">Aceita o `id` da cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-218">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="0a0b6-219">Consulta o banco de dados para o contato de cliente com `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-219">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="0a0b6-220">Se o contato do cliente for encontrado, eles serão removidos da lista de contatos do cliente.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-220">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="0a0b6-221">O banco de dados é atualizado.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-221">The database is updated.</span></span>
* <span data-ttu-id="0a0b6-222">Chama `RedirectToPage` para redirecionar para a página de índice de raiz (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-222">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="0a0b6-223">Gerenciar solicitações HEAD com o manipulador OnGet</span><span class="sxs-lookup"><span data-stu-id="0a0b6-223">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="0a0b6-224">Geralmente, um manipulador HEAD é criado e chamado para solicitações HEAD:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-224">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span>

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="0a0b6-225">Caso nenhum manipulador HEAD (`OnHead`) seja definido, as Páginas Razor voltam a chamar o manipulador de página GET (`OnGet`) no ASP.NET Core 2.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-225">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="0a0b6-226">Aceite o seguinte comportamento com o [método SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc), no `Startup.Configure` para ASP.NET Core 2.1 a 2.x:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-226">Opt in to this behavior with the [SetCompatibilityVersion method](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) in `Startup.Configure` for ASP.NET Core 2.1 to 2.x:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="0a0b6-227">`SetCompatibilityVersion` define de forma eficiente a opção de Páginas Razor `AllowMappingHeadRequestsToGetHandler` como `true`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-227">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span> <span data-ttu-id="0a0b6-228">O comportamento é de aceitação até a liberação da versão prévia 1 do ASP.NET Core 3.0 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-228">The behavior is opt-in until the release of ASP.NET Core 3.0 Preview 1 or later.</span></span> <span data-ttu-id="0a0b6-229">Cada versão principal do ASP.NET Core adota todos os comportamentos de liberação de patch da versão anterior.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-229">Each major version of ASP.NET Core adopts all of the patch release behaviors of the previous version.</span></span>

<span data-ttu-id="0a0b6-230">O comportamento de aceitação global da liberação dos patches 2.1 a 2.x pode ser evitado com uma configuração de aplicativo que mapeia solicitações HEAD para o manipulador GET.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-230">Global opt-in behavior for patch releases 2.1 to 2.x can be avoided with an app configuration that maps HEAD requests to the GET handler.</span></span> <span data-ttu-id="0a0b6-231">Defina a opção de Páginas Razor `AllowMappingHeadRequestsToGetHandler` como `true`, sem chamar `SetCompatibilityVersion`, no `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-231">Set the `AllowMappingHeadRequestsToGetHandler` Razor Pages option to `true` without calling `SetCompatibilityVersion` in `Startup.Configure`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```
::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="0a0b6-232">XSRF/CSRF e Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="0a0b6-232">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="0a0b6-233">Você não precisa escrever nenhum código para [validação antifalsificação](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-233">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="0a0b6-234">Validação e geração de token antifalsificação são automaticamente incluídas nas Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-234">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="0a0b6-235">Usando Layouts, parciais, modelos e auxiliares de marcas com Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="0a0b6-235">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="0a0b6-236">As Páginas funcionam com todos os recursos do mecanismo de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-236">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="0a0b6-237">Layouts, parciais, modelos, auxiliares de marcas, *_ViewStart.cshtml* e *_ViewImports.cshtml* funcionam da mesma forma que funcionam exibições convencionais do Razor.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-237">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="0a0b6-238">Organizaremos essa página aproveitando alguns desses recursos.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-238">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="0a0b6-239">Adicione uma [página de layout](xref:mvc/views/layout) a *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-239">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="0a0b6-240">O [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="0a0b6-240">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="0a0b6-241">Controla o layout de cada página (a menos que a página opte por não usar o layout).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-241">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="0a0b6-242">Importa estruturas HTML como JavaScript e folhas de estilo.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-242">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="0a0b6-243">Veja [página de layout](xref:mvc/views/layout) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-243">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="0a0b6-244">A propriedade [Layout](xref:mvc/views/layout#specifying-a-layout) é definida em *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-244">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="0a0b6-245">**Observação**: o layout está na pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-245">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="0a0b6-246">As páginas buscam outras exibições (layouts, modelos, parciais) hierarquicamente, iniciando na mesma pasta que a página atual.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-246">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="0a0b6-247">Um layout na pasta *Pages* pode ser usado em qualquer Página do Razor na pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-247">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="0a0b6-248">Recomendamos que você **não** coloque o arquivo de layout na pasta *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-248">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="0a0b6-249">*Views/Shared* é um padrão de exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-249">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="0a0b6-250">As Páginas do Razor devem confiar na hierarquia de pasta e não nas convenções de caminho.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-250">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="0a0b6-251">A pesquisa de modo de exibição de uma Página do Razor inclui a pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-251">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="0a0b6-252">Os layouts, modelos e parciais que você está usando com controladores MVC e exibições do Razor convencionais *apenas funcionam*.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-252">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="0a0b6-253">Adicione um arquivo *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-253">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="0a0b6-254">`@namespace` é explicado posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-254">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="0a0b6-255">A diretiva `@addTagHelper` coloca os [auxiliares de marcas internos](xref:mvc/views/tag-helpers/builtin-th/Index) em todas as páginas na pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-255">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="0a0b6-256">Quando a diretiva `@namespace` é usada explicitamente em uma página:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-256">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="0a0b6-257">A diretiva define o namespace da página.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-257">The directive sets the namespace for the page.</span></span> <span data-ttu-id="0a0b6-258">A diretiva `@model` não precisa incluir o namespace.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-258">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="0a0b6-259">Quando a diretiva `@namespace` está contida em *_ViewImports.cshtml*, o namespace especificado fornece o prefixo do namespace gerado na página que importa a diretiva `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-259">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="0a0b6-260">O restante do namespace gerado (a parte do sufixo) é o caminho relativo separado por ponto entre a pasta que contém *_ViewImports.cshtml* e a pasta que contém a página.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-260">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="0a0b6-261">Por exemplo, o arquivo code-behind *Pages/Customers/Edit.cshtml.cs* define explicitamente o namespace:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-261">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="0a0b6-262">O arquivo *Pages/_ViewImports.cshtml* define o namespace a seguir:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-262">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="0a0b6-263">O namespace gerado para a Página do Razor *Pages/Customers/Edit.cshtml* é o mesmo que o do arquivo code-behind.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-263">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="0a0b6-264">A diretiva `@namespace` foi projetada de modo que as classes C# adicionadas a um projeto e o código gerado pelas páginas *funcione* sem a necessidade de adicionar uma diretiva `@using` para o arquivo code-behind.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-264">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="0a0b6-265">**Observação:** `@namespace` também funciona com exibições do Razor convencionais.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-265">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="0a0b6-266">O arquivo de exibição *Pages/Create.cshtml* original:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-266">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="0a0b6-267">O arquivo de exibição *Pages/Create.cshtml* atualizado:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-267">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="0a0b6-268">O [projeto inicial de Páginas do Razor](#rpvs17) contém o *Pages/_ValidationScriptsPartial.cshtml*, que conecta a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-268">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="0a0b6-269">Geração de URL para Páginas</span><span class="sxs-lookup"><span data-stu-id="0a0b6-269">URL generation for Pages</span></span>

<span data-ttu-id="0a0b6-270">A página `Create`, exibida anteriormente, usa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-270">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="0a0b6-271">O aplicativo tem a estrutura de arquivos/pastas a seguir:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-271">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="0a0b6-272">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-272">*/Pages*</span></span>

  * <span data-ttu-id="0a0b6-273">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-273">*Index.cshtml*</span></span>
  * <span data-ttu-id="0a0b6-274">*/Clientes*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-274">*/Customers*</span></span>

    * <span data-ttu-id="0a0b6-275">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-275">*Create.cshtml*</span></span>
    * <span data-ttu-id="0a0b6-276">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-276">*Edit.cshtml*</span></span>
    * <span data-ttu-id="0a0b6-277">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-277">*Index.cshtml*</span></span>

<span data-ttu-id="0a0b6-278">As páginas *Pages/Customers/Create.cshtml* e *Pages/Customers/Edit.cshtml* redirecionam para o *Pages/Index.cshtml* após êxito.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-278">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="0a0b6-279">A cadeia de caracteres `/Index` faz parte do URI para acessar a página anterior.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-279">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="0a0b6-280">A cadeia de caracteres `/Index` pode ser usada para gerar URIs para a página *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-280">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="0a0b6-281">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-281">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="0a0b6-282">O nome da página é o caminho para a página da pasta raiz */Pages* (incluindo um `/` à direita, por exemplo, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-282">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="0a0b6-283">As amostras anteriores de geração de URL são muito mais ricas em recursos do que apenas codificar uma URL.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-283">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="0a0b6-284">A geração de URL usa [roteamento](xref:mvc/controllers/routing) e pode gerar e codificar parâmetros de acordo com o modo como a rota é definida no caminho de destino.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-284">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="0a0b6-285">A Geração de URL para páginas dá suporte a nomes relativos.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-285">URL generation for pages supports relative names.</span></span> <span data-ttu-id="0a0b6-286">A tabela a seguir mostra qual página de Índice é selecionada com diferentes parâmetros `RedirectToPage` de *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-286">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="0a0b6-287">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="0a0b6-287">RedirectToPage(x)</span></span>| <span data-ttu-id="0a0b6-288">Página</span><span class="sxs-lookup"><span data-stu-id="0a0b6-288">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="0a0b6-289">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="0a0b6-289">RedirectToPage("/Index")</span></span> | <span data-ttu-id="0a0b6-290">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-290">*Pages/Index*</span></span> |
| <span data-ttu-id="0a0b6-291">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="0a0b6-291">RedirectToPage("./Index");</span></span> | <span data-ttu-id="0a0b6-292">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-292">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="0a0b6-293">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="0a0b6-293">RedirectToPage("../Index")</span></span> | <span data-ttu-id="0a0b6-294">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-294">*Pages/Index*</span></span> |
| <span data-ttu-id="0a0b6-295">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="0a0b6-295">RedirectToPage("Index")</span></span>  | <span data-ttu-id="0a0b6-296">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="0a0b6-296">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="0a0b6-297">`RedirectToPage("Index")`, `RedirectToPage("./Index")` e `RedirectToPage("../Index")` são <em>nomes relativos</em>.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-297">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="0a0b6-298">O parâmetro `RedirectToPage` é <em>combinado</em> com o caminho da página atual para calcular o nome da página de destino.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-298">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="0a0b6-299">Vinculação de nome relativo é útil ao criar sites com uma estrutura complexa.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-299">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="0a0b6-300">Se você usar nomes relativos para vincular entre páginas em uma pasta, você poderá renomear essa pasta.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-300">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="0a0b6-301">Todos os links ainda funcionarão (porque eles não incluirão o nome da pasta).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-301">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="0a0b6-302">TempData</span><span class="sxs-lookup"><span data-stu-id="0a0b6-302">TempData</span></span>

<span data-ttu-id="0a0b6-303">O ASP.NET Core expõe a propriedade [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) em um [controlador](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-303">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="0a0b6-304">Essa propriedade armazena dados até eles serem lidos.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-304">This property stores data until it's read.</span></span> <span data-ttu-id="0a0b6-305">Os métodos `Keep` e `Peek` podem ser usados para examinar os dados sem exclusão.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-305">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="0a0b6-306">`TempData` é útil para redirecionamento nos casos em que os dados são necessários para mais de uma única solicitação.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-306">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="0a0b6-307">O atributo `[TempData]` é novo no ASP.NET Core 2.0 e tem suporte em controladores e páginas.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-307">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="0a0b6-308">Os conjuntos de código a seguir definem o valor de `Message` usando `TempData`:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-308">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="0a0b6-309">A marcação a seguir no arquivo *Pages/Customers/Index.cshtml* exibe o valor de `Message` usando `TempData`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-309">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="0a0b6-310">O modelo de página *Pages/Customers/Index.cshtml.cs* aplica o atributo `[TempData]` à propriedade `Message`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-310">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="0a0b6-311">Consulte [TempData](xref:fundamentals/app-state#temp) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-311">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="0a0b6-312">Vários manipuladores por página</span><span class="sxs-lookup"><span data-stu-id="0a0b6-312">Multiple handlers per page</span></span>

<span data-ttu-id="0a0b6-313">A página a seguir gera marcação para dois manipuladores de página usando o auxiliar de marcas `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-313">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="0a0b6-314">O formulário no exemplo anterior tem dois botões de envio, cada um usando o `FormActionTagHelper` para enviar para uma URL diferente.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-314">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="0a0b6-315">O atributo `asp-page-handler` é um complemento para `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-315">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="0a0b6-316">`asp-page-handler` gera URLs que enviam para cada um dos métodos de manipulador definidos por uma página.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-316">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="0a0b6-317">`asp-page` não foi especificado porque a amostra está vinculando à página atual.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-317">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="0a0b6-318">O modelo de página:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-318">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="0a0b6-319">O código anterior usa *métodos de manipulador nomeados*.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-319">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="0a0b6-320">Métodos de manipulador nomeados são criados colocando o texto no nome após `On<HTTP Verb>` e antes de `Async` (se houver).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-320">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="0a0b6-321">No exemplo anterior, os métodos de página são OnPost**JoinList**Async e OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-321">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="0a0b6-322">Com *OnPost* e *Async* removidos, os nomes de manipulador são `JoinList` e `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-322">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="0a0b6-323">Usando o código anterior, o caminho da URL que envia a `OnPostJoinListAsync` é `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-323">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="0a0b6-324">O caminho da URL que envia a `OnPostJoinListUCAsync` é `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-324">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>



## <a name="customizing-routing"></a><span data-ttu-id="0a0b6-325">Personalizando o roteamento</span><span class="sxs-lookup"><span data-stu-id="0a0b6-325">Customizing Routing</span></span>

<span data-ttu-id="0a0b6-326">Se você não deseja a cadeia de consulta `?handler=JoinList` na URL, você pode alterar a rota para colocar o nome do manipulador na parte do caminho da URL.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-326">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="0a0b6-327">Você pode personalizar a rota adicionando um modelo de rota entre aspas duplas após a diretiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-327">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="0a0b6-328">A rota anterior coloca o nome do manipulador no caminho da URL em vez da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-328">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="0a0b6-329">O `?` após `handler` significa que o parâmetro de rota é opcional.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-329">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="0a0b6-330">Você pode usar `@page` para adicionar parâmetros e segmentos adicionais a uma rota de página.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-330">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="0a0b6-331">Tudo que está lá é **acrescentado** à rota padrão da página.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-331">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="0a0b6-332">Não há suporte para o uso de um caminho absoluto ou virtual para alterar a rota da página (como `"~/Some/Other/Path"`).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-332">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="0a0b6-333">Configuração e definições</span><span class="sxs-lookup"><span data-stu-id="0a0b6-333">Configuration and settings</span></span>

<span data-ttu-id="0a0b6-334">Para configurar opções avançadas, use o método de extensão `AddRazorPagesOptions` no construtor de MVC:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-334">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="0a0b6-335">No momento, você pode usar o `RazorPagesOptions` para definir o diretório raiz para páginas ou adicionar as convenções de modelo de aplicativo para páginas.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-335">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="0a0b6-336">Permitiremos mais extensibilidade dessa maneira no futuro.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-336">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="0a0b6-337">Para pré-compilar exibições, consulte [Compilação de exibição do Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-337">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="0a0b6-338">[Baixar ou exibir código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="0a0b6-338">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="0a0b6-339">Consulte a [Introdução a Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start), que se baseia nesta introdução.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-339">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="0a0b6-340">Especificar que as Páginas Razor estão na raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="0a0b6-340">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="0a0b6-341">Por padrão, as Páginas Razor estão na raiz do diretório */Pages*.</span><span class="sxs-lookup"><span data-stu-id="0a0b6-341">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="0a0b6-342">Adicione [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) em [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) para especificar que as Páginas Razor estão na raiz do conteúdo ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="0a0b6-342">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="0a0b6-343">Especificar que as Páginas Razor estão em um diretório raiz personalizado</span><span class="sxs-lookup"><span data-stu-id="0a0b6-343">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="0a0b6-344">Adicione [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) em [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) para especificar que as Páginas Razor estão em um diretório raiz personalizado no aplicativo (forneça um caminho relativo):</span><span class="sxs-lookup"><span data-stu-id="0a0b6-344">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="0a0b6-345">Consulte também</span><span class="sxs-lookup"><span data-stu-id="0a0b6-345">See also</span></span>

* [<span data-ttu-id="0a0b6-346">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a0b6-346">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="0a0b6-347">Sintaxe Razor</span><span class="sxs-lookup"><span data-stu-id="0a0b6-347">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="0a0b6-348">Introdução a Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="0a0b6-348">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="0a0b6-349">Convenções de autorização de Páginas Razor</span><span class="sxs-lookup"><span data-stu-id="0a0b6-349">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="0a0b6-350">Provedores de modelo personalizado de página e rota de Páginas Razor</span><span class="sxs-lookup"><span data-stu-id="0a0b6-350">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="0a0b6-351">Testes de integração e unidade de Páginas Razor</span><span class="sxs-lookup"><span data-stu-id="0a0b6-351">Razor Pages unit and integration tests</span></span>](xref:testing/razor-pages-testing)
