---
title: "Introdução a Páginas do Razor no ASP.NET Core"
author: Rick-Anderson
description: "Este documento fornece uma visão geral do uso das páginas Razor no ASP.NET Core para facilitar o desenvolvimento de cenários focados em página."
keywords: "ASP.NET Core, Páginas Razor"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: 36dd2ad01f93ab1093bad84a58504a150c70ea16
ms.sourcegitcommit: 4925a91ef4130ddb333f187ab13defe66f2c6cef
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="b1d21-104">Introdução a Páginas do Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1d21-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="b1d21-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="b1d21-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="b1d21-106">Páginas do Razor é um novo recurso do ASP.NET Core MVC que torna a codificação de cenários focados em página mais fácil e produtiva.</span><span class="sxs-lookup"><span data-stu-id="b1d21-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="b1d21-107">Se você estiver procurando um tutorial que usa a abordagem Modelo-Exibição-Controlador, consulte a [Introdução ao ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="b1d21-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="b1d21-108">Este documento proporciona uma introdução a páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="b1d21-108">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="b1d21-109">Este não é um tutorial passo a passo.</span><span class="sxs-lookup"><span data-stu-id="b1d21-109">It's not a step by step tutorial.</span></span> <span data-ttu-id="b1d21-110">Se você achar que algumas das seções são difíceis de entender, consulte [Introdução a Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="b1d21-110">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="b1d21-111">Pré-requisitos do ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="b1d21-111">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="b1d21-112">Instale o [.NET Core](https://www.microsoft.com/net/core) 2.0.0 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="b1d21-112">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="b1d21-113">Se você estiver usando o Visual Studio, instale o [Visual Studio](https://www.visualstudio.com/vs/) 2017 versão 15.3 ou posterior com as cargas de trabalho a seguir:</span><span class="sxs-lookup"><span data-stu-id="b1d21-113">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="b1d21-114">**ASP.NET e desenvolvimento para a Web**</span><span class="sxs-lookup"><span data-stu-id="b1d21-114">**ASP.NET and web development**</span></span>
* <span data-ttu-id="b1d21-115">**Desenvolvimento entre plataformas do .NET Core**</span><span class="sxs-lookup"><span data-stu-id="b1d21-115">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="b1d21-116">Criando um projeto de Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="b1d21-116">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b1d21-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1d21-117">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="b1d21-118">Consulte a [Introdução a Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start) para obter instruções detalhadas sobre como criar um projeto de Páginas do Razor usando o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b1d21-118">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b1d21-119">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="b1d21-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b1d21-120">Da linha de comando, execute `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-120">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="b1d21-121">Abra o arquivo *.csproj* gerado do Visual Studio para Mac.</span><span class="sxs-lookup"><span data-stu-id="b1d21-121">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b1d21-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1d21-122">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="b1d21-123">Da linha de comando, execute `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-123">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b1d21-124">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="b1d21-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="b1d21-125">Da linha de comando, execute `dotnet new razor`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-125">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="b1d21-126">Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="b1d21-126">Razor Pages</span></span>

<span data-ttu-id="b1d21-127">O Páginas do Razor está habilitado em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b1d21-127">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="b1d21-128">Considere uma página básica: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="b1d21-128">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="b1d21-129">O código anterior é muito parecido com um arquivo de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="b1d21-129">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="b1d21-130">O que o torna diferentes é a diretiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-130">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="b1d21-131">`@page` transforma o arquivo em uma ação do MVC – o que significa que ele trata solicitações diretamente, sem passar por um controlador.</span><span class="sxs-lookup"><span data-stu-id="b1d21-131">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="b1d21-132">`@page` deve ser a primeira diretiva do Razor em uma página.</span><span class="sxs-lookup"><span data-stu-id="b1d21-132">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="b1d21-133">`@page` afeta o comportamento de outros constructos do Razor.</span><span class="sxs-lookup"><span data-stu-id="b1d21-133">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="b1d21-134">Uma página semelhante, usando uma classe `PageModel`, é mostrada nos dois arquivos a seguir.</span><span class="sxs-lookup"><span data-stu-id="b1d21-134">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="b1d21-135">O arquivo *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b1d21-135">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="b1d21-136">O arquivo “code-behind” *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b1d21-136">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="b1d21-137">Por convenção, o arquivo de classe `PageModel` tem o mesmo nome que o arquivo na Página do Razor com *.cs* acrescentado.</span><span class="sxs-lookup"><span data-stu-id="b1d21-137">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="b1d21-138">Por exemplo, a Página do Razor anterior é *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b1d21-138">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="b1d21-139">O arquivo que contém a classe `PageModel` é chamado *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="b1d21-139">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="b1d21-140">As associações de caminhos de URL para páginas são determinadas pelo local da página no sistema de arquivos.</span><span class="sxs-lookup"><span data-stu-id="b1d21-140">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="b1d21-141">A tabela a seguir mostra um caminho de Página do Razor e a URL correspondente:</span><span class="sxs-lookup"><span data-stu-id="b1d21-141">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="b1d21-142">Caminho e nome do arquivo</span><span class="sxs-lookup"><span data-stu-id="b1d21-142">File name and path</span></span>               | <span data-ttu-id="b1d21-143">URL correspondente</span><span class="sxs-lookup"><span data-stu-id="b1d21-143">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="b1d21-144">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b1d21-144">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="b1d21-145">`/` ou `/Index`</span><span class="sxs-lookup"><span data-stu-id="b1d21-145">`/` or `/Index`</span></span> |
| <span data-ttu-id="b1d21-146">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b1d21-146">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="b1d21-147">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b1d21-147">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="b1d21-148">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b1d21-148">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="b1d21-149">`/Store` ou `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="b1d21-149">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="b1d21-150">Notas:</span><span class="sxs-lookup"><span data-stu-id="b1d21-150">Notes:</span></span>

* <span data-ttu-id="b1d21-151">O tempo de execução procura arquivos de Páginas do Razor na pasta *Pages* por padrão.</span><span class="sxs-lookup"><span data-stu-id="b1d21-151">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="b1d21-152">`Index` é a página padrão quando uma URL não inclui uma página.</span><span class="sxs-lookup"><span data-stu-id="b1d21-152">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="b1d21-153">Escrevendo um formulário básico</span><span class="sxs-lookup"><span data-stu-id="b1d21-153">Writing a basic form</span></span>

<span data-ttu-id="b1d21-154">Os recursos de Páginas do Razor são projetados para tornar fáceis os padrões comuns usados com navegadores da Web.</span><span class="sxs-lookup"><span data-stu-id="b1d21-154">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="b1d21-155">[Associação de modelos](xref:mvc/models/model-binding), [auxiliares de marcas](xref:mvc/views/tag-helpers/intro) e auxiliares HTML *funcionam todos apenas* com as propriedades definidas em uma classe de Página do Razor.</span><span class="sxs-lookup"><span data-stu-id="b1d21-155">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="b1d21-156">Considere uma página que implementa um formulário básico "Fale conosco" para o modelo `Contact`:</span><span class="sxs-lookup"><span data-stu-id="b1d21-156">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="b1d21-157">Para as amostras neste documento, o `DbContext` é inicializado no arquivo [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="b1d21-157">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="b1d21-158">O modelo de dados:</span><span class="sxs-lookup"><span data-stu-id="b1d21-158">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="b1d21-159">O contexto do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="b1d21-159">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="b1d21-160">O arquivo de exibição *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b1d21-160">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="b1d21-161">O arquivo code-behind *Pages/Create.cshtml.cs* para a exibição:</span><span class="sxs-lookup"><span data-stu-id="b1d21-161">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="b1d21-162">Por convenção, a classe `PageModel` é chamada de `<PageName>Model` e está no mesmo namespace que a página.</span><span class="sxs-lookup"><span data-stu-id="b1d21-162">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="b1d21-163">O uso de um arquivo code-behind `PageModel` dá suporte a teste de unidade, mas exige que você grave uma classe e um construtor explícitos.</span><span class="sxs-lookup"><span data-stu-id="b1d21-163">Using a `PageModel` code-behind file supports unit testing, but requires you to write an explicit constructor and class.</span></span> <span data-ttu-id="b1d21-164">Páginas sem arquivos code-behind `PageModel` dão suporte a compilação de tempo de execução, o que pode ser uma vantagem no desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="b1d21-164">Pages without `PageModel` code-behind files support runtime compilation, which can be an advantage in development.</span></span>  <!-- review: advantage because you can make changes and refresh the browser without explicitly compiling the app -->

<span data-ttu-id="b1d21-165">A página tem um *método de manipulador* `OnPostAsync`, que é executado em solicitações `POST` (quando um usuário posta o formulário).</span><span class="sxs-lookup"><span data-stu-id="b1d21-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="b1d21-166">Você pode adicionar métodos de manipulador para qualquer verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="b1d21-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="b1d21-167">Os manipuladores mais comuns são:</span><span class="sxs-lookup"><span data-stu-id="b1d21-167">The most common handlers are:</span></span>

* <span data-ttu-id="b1d21-168">`OnGet` para inicializar o estado necessário para a página.</span><span class="sxs-lookup"><span data-stu-id="b1d21-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="b1d21-169">Amostra de [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="b1d21-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="b1d21-170">`OnPost` para manipular envios de formulário.</span><span class="sxs-lookup"><span data-stu-id="b1d21-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="b1d21-171">O sufixo de nomenclatura `Async` é opcional, mas geralmente é usado por convenção para funções assíncronas.</span><span class="sxs-lookup"><span data-stu-id="b1d21-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="b1d21-172">O código `OnPostAsync` no exemplo anterior tem aparência semelhante ao que você normalmente escreve em um controlador.</span><span class="sxs-lookup"><span data-stu-id="b1d21-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="b1d21-173">O código anterior é comum para as Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="b1d21-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="b1d21-174">A maioria dos primitivos MVC como [associação de modelos](xref:mvc/models/model-binding), [validação](xref:mvc/models/validation) e resultados da ação são compartilhados.</span><span class="sxs-lookup"><span data-stu-id="b1d21-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="b1d21-175">O método `OnPostAsync` anterior:</span><span class="sxs-lookup"><span data-stu-id="b1d21-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="b1d21-176">O fluxo básico de `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="b1d21-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="b1d21-177">Verifique se há erros de validação.</span><span class="sxs-lookup"><span data-stu-id="b1d21-177">Check for validation errors.</span></span>

*  <span data-ttu-id="b1d21-178">Se não houver nenhum erro, salve os dados e redirecione.</span><span class="sxs-lookup"><span data-stu-id="b1d21-178">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="b1d21-179">Se houver erros, mostre a página novamente com as mensagens de validação.</span><span class="sxs-lookup"><span data-stu-id="b1d21-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="b1d21-180">A validação do lado do cliente é idêntica para aplicativos ASP.NET Core MVC tradicionais.</span><span class="sxs-lookup"><span data-stu-id="b1d21-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="b1d21-181">Em muitos casos, erros de validação seriam detectados no cliente e nunca enviados ao servidor.</span><span class="sxs-lookup"><span data-stu-id="b1d21-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="b1d21-182">Quando os dados são inseridos com êxito, o método de manipulador `OnPostAsync` chama o método auxiliar `RedirectToPage` para retornar uma instância de `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="b1d21-183">`RedirectToPage` é um novo resultado de ação, semelhante a `RedirectToAction` ou `RedirectToRoute`, mas personalizado para páginas.</span><span class="sxs-lookup"><span data-stu-id="b1d21-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="b1d21-184">Na amostra anterior, ele redireciona para a página de Índice raiz (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="b1d21-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="b1d21-185">`RedirectToPage` é descrito em detalhes na seção [Geração de URLs para páginas](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="b1d21-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="b1d21-186">Quando o formulário enviado tem erros de validação (que são passados para o servidor), o método de manipulador `OnPostAsync` chama o método auxiliar `Page`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="b1d21-187">`Page` retorna uma instância de `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="b1d21-188">Retornar `Page` é semelhante a como as ações em controladores retornam `View`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="b1d21-189">`PageResult` é o tipo de retorno <!-- Review  --> padrão para um método de manipulador.</span><span class="sxs-lookup"><span data-stu-id="b1d21-189">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="b1d21-190">Um método de manipulador que retorna `void` renderiza a página.</span><span class="sxs-lookup"><span data-stu-id="b1d21-190">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="b1d21-191">A propriedade `Customer` usa o atributo `[BindProperty]` para aceitar a associação de modelos.</span><span class="sxs-lookup"><span data-stu-id="b1d21-191">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="b1d21-192">Páginas do Razor, por padrão, associam as propriedades somente com verbos não GET.</span><span class="sxs-lookup"><span data-stu-id="b1d21-192">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="b1d21-193">A associação de propriedades pode reduzir a quantidade de código que você precisa escrever.</span><span class="sxs-lookup"><span data-stu-id="b1d21-193">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="b1d21-194">A associação reduz o código usando a mesma propriedade para renderizar os campos de formulário (`<input asp-for="Customer.Name" />`) e aceitar a entrada.</span><span class="sxs-lookup"><span data-stu-id="b1d21-194">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="b1d21-195">A home page (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="b1d21-195">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="b1d21-196">O arquivo code-behind *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b1d21-196">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="b1d21-197">O arquivo *cshtml* contém a marcação a seguir para criar um link de edição para cada contato:</span><span class="sxs-lookup"><span data-stu-id="b1d21-197">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="b1d21-198">O [auxiliar de marcas de âncora](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) usou o atributo [asp-route-{valor}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route) para gerar um link para a página Edit.</span><span class="sxs-lookup"><span data-stu-id="b1d21-198">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route) attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="b1d21-199">O link contém dados de rota com a ID de contato.</span><span class="sxs-lookup"><span data-stu-id="b1d21-199">The link contains route data with the contact ID.</span></span> <span data-ttu-id="b1d21-200">Por exemplo, `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-200">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="b1d21-201">O arquivo *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b1d21-201">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="b1d21-202">A primeira linha contém a diretiva `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-202">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="b1d21-203">A restrição de roteamento `"{id:int}"` informa à página para aceitar solicitações para a página que contêm dados da rota `int`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-203">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="b1d21-204">Se uma solicitação para a página não contém dados de rota que podem ser convertidos em um `int`, o tempo de execução retorna um erro HTTP 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="b1d21-204">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="b1d21-205">O arquivo *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b1d21-205">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="b1d21-206">O arquivo *Index.cshtml* também contém a marcação para criar um botão de exclusão para cada contato de cliente:</span><span class="sxs-lookup"><span data-stu-id="b1d21-206">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="b1d21-207">Quando o botão de exclusão é renderizado em HTML, seu `formaction` inclui parâmetros para:</span><span class="sxs-lookup"><span data-stu-id="b1d21-207">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="b1d21-208">A ID de contato do cliente especificada pelo atributo `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-208">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="b1d21-209">O `handler` especificado pelo atributo `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-209">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="b1d21-210">Este é um exemplo de um botão de exclusão renderizado com uma ID de contato do cliente de `1`:</span><span class="sxs-lookup"><span data-stu-id="b1d21-210">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="b1d21-211">Quando o botão é selecionado, uma solicitação de formulário `POST` é enviada para o servidor.</span><span class="sxs-lookup"><span data-stu-id="b1d21-211">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="b1d21-212">Por convenção, o nome do método do manipulador é selecionado com base no valor do parâmetro `handler` de acordo com o esquema `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-212">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="b1d21-213">Como o `handler` é `delete` neste exemplo, o método do manipulador `OnPostDeleteAsync` é usado para processar a solicitação `POST`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-213">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="b1d21-214">Se o `asp-page-handler` for definido como um valor diferente, como `remove`, um método de manipulador de página com o nome `OnPostRemoveAsync` será selecionado.</span><span class="sxs-lookup"><span data-stu-id="b1d21-214">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="b1d21-215">O método `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="b1d21-215">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="b1d21-216">Aceita o `id` da cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="b1d21-216">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="b1d21-217">Consulta o banco de dados para o contato de cliente com `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-217">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="b1d21-218">Se o contato do cliente for encontrado, eles serão removidos da lista de contatos do cliente.</span><span class="sxs-lookup"><span data-stu-id="b1d21-218">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="b1d21-219">O banco de dados é atualizado.</span><span class="sxs-lookup"><span data-stu-id="b1d21-219">The database is updated.</span></span>
* <span data-ttu-id="b1d21-220">Chama `RedirectToPage` para redirecionar para a página de índice de raiz (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="b1d21-220">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="b1d21-221">XSRF/CSRF e Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="b1d21-221">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="b1d21-222">Você não precisa escrever nenhum código para [validação antifalsificação](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="b1d21-222">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="b1d21-223">Validação e geração de token antifalsificação são automaticamente incluídas nas Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="b1d21-223">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="b1d21-224">Usando Layouts, parciais, modelos e auxiliares de marcas com Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="b1d21-224">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="b1d21-225">As Páginas funcionam com todos os recursos do mecanismo de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="b1d21-225">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="b1d21-226">Layouts, parciais, modelos, auxiliares de marcas, *_ViewStart.cshtml* e *_ViewImports.cshtml* funcionam da mesma forma que funcionam exibições convencionais do Razor.</span><span class="sxs-lookup"><span data-stu-id="b1d21-226">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="b1d21-227">Organizaremos essa página aproveitando alguns desses recursos.</span><span class="sxs-lookup"><span data-stu-id="b1d21-227">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="b1d21-228">Adicione uma [página de layout](xref:mvc/views/layout) a *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b1d21-228">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="b1d21-229">O [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="b1d21-229">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="b1d21-230">Controla o layout de cada página (a menos que a página opte por não usar o layout).</span><span class="sxs-lookup"><span data-stu-id="b1d21-230">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="b1d21-231">Importa estruturas HTML como JavaScript e folhas de estilo.</span><span class="sxs-lookup"><span data-stu-id="b1d21-231">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="b1d21-232">Veja [página de layout](xref:mvc/views/layout) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="b1d21-232">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="b1d21-233">A propriedade [Layout](xref:mvc/views/layout#specifying-a-layout) é definida em *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b1d21-233">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="b1d21-234">**Observação**: o layout está na pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="b1d21-234">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="b1d21-235">As páginas buscam outras exibições (layouts, modelos, parciais) hierarquicamente, iniciando na mesma pasta que a página atual.</span><span class="sxs-lookup"><span data-stu-id="b1d21-235">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="b1d21-236">Um layout na pasta *Pages* pode ser usado em qualquer Página do Razor na pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="b1d21-236">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="b1d21-237">Recomendamos que você **não** coloque o arquivo de layout na pasta *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="b1d21-237">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="b1d21-238">*Views/Shared* é um padrão de exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="b1d21-238">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="b1d21-239">As Páginas do Razor devem confiar na hierarquia de pasta e não nas convenções de caminho.</span><span class="sxs-lookup"><span data-stu-id="b1d21-239">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="b1d21-240">A pesquisa de modo de exibição de uma Página do Razor inclui a pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="b1d21-240">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="b1d21-241">Os layouts, modelos e parciais que você está usando com controladores MVC e exibições do Razor convencionais *apenas funcionam*.</span><span class="sxs-lookup"><span data-stu-id="b1d21-241">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="b1d21-242">Adicione um arquivo *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b1d21-242">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="b1d21-243">`@namespace` é explicado posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="b1d21-243">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="b1d21-244">A diretiva `@addTagHelper` coloca os [auxiliares de marcas internos](xref:mvc/views/tag-helpers/builtin-th/Index) em todas as páginas na pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="b1d21-244">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="b1d21-245">Quando a diretiva `@namespace` é usada explicitamente em uma página:</span><span class="sxs-lookup"><span data-stu-id="b1d21-245">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="b1d21-246">A diretiva define o namespace da página.</span><span class="sxs-lookup"><span data-stu-id="b1d21-246">The directive sets the namespace for the page.</span></span> <span data-ttu-id="b1d21-247">A diretiva `@model` não precisa incluir o namespace.</span><span class="sxs-lookup"><span data-stu-id="b1d21-247">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="b1d21-248">Quando a diretiva `@namespace` está contida em *_ViewImports.cshtml*, o namespace especificado fornece o prefixo do namespace gerado na página que importa a diretiva `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-248">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="b1d21-249">O restante do namespace gerado (a parte do sufixo) é o caminho relativo separado por ponto entre a pasta que contém *_ViewImports.cshtml* e a pasta que contém a página.</span><span class="sxs-lookup"><span data-stu-id="b1d21-249">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="b1d21-250">Por exemplo, o arquivo code-behind *Pages/Customers/Edit.cshtml.cs* define explicitamente o namespace:</span><span class="sxs-lookup"><span data-stu-id="b1d21-250">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="b1d21-251">O arquivo *Pages/_ViewImports.cshtml* define o namespace a seguir:</span><span class="sxs-lookup"><span data-stu-id="b1d21-251">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="b1d21-252">O namespace gerado para a Página do Razor *Pages/Customers/Edit.cshtml* é o mesmo que o do arquivo code-behind.</span><span class="sxs-lookup"><span data-stu-id="b1d21-252">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="b1d21-253">A diretiva `@namespace` foi projetada de modo que as classes C# adicionadas a um projeto e o código gerado pelas páginas *funcione* sem a necessidade de adicionar uma diretiva `@using` para o arquivo code-behind.</span><span class="sxs-lookup"><span data-stu-id="b1d21-253">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="b1d21-254">**Observação:** `@namespace` também funciona com exibições do Razor convencionais.</span><span class="sxs-lookup"><span data-stu-id="b1d21-254">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="b1d21-255">O arquivo de exibição *Pages/Create.cshtml* original:</span><span class="sxs-lookup"><span data-stu-id="b1d21-255">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="b1d21-256">O arquivo de exibição *Pages/Create.cshtml* atualizado:</span><span class="sxs-lookup"><span data-stu-id="b1d21-256">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="b1d21-257">O [projeto inicial de Páginas do Razor](#rpvs17) contém o *Pages/_ValidationScriptsPartial.cshtml*, que conecta a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="b1d21-257">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="b1d21-258">Geração de URL para Páginas</span><span class="sxs-lookup"><span data-stu-id="b1d21-258">URL generation for Pages</span></span>

<span data-ttu-id="b1d21-259">A página `Create`, exibida anteriormente, usa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="b1d21-259">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="b1d21-260">O aplicativo tem a estrutura de arquivos/pastas a seguir:</span><span class="sxs-lookup"><span data-stu-id="b1d21-260">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="b1d21-261">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="b1d21-261">*/Pages*</span></span>

  * <span data-ttu-id="b1d21-262">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b1d21-262">*Index.cshtml*</span></span>
  * <span data-ttu-id="b1d21-263">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="b1d21-263">*/Customer*</span></span>

    * <span data-ttu-id="b1d21-264">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b1d21-264">*Create.cshtml*</span></span>
    * <span data-ttu-id="b1d21-265">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b1d21-265">*Edit.cshtml*</span></span>
    * <span data-ttu-id="b1d21-266">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b1d21-266">*Index.cshtml*</span></span>

<span data-ttu-id="b1d21-267">As páginas *Pages/Customers/Create.cshtml* e *Pages/Customers/Edit.cshtml* redirecionam para o *Pages/Index.cshtml* após êxito.</span><span class="sxs-lookup"><span data-stu-id="b1d21-267">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="b1d21-268">A cadeia de caracteres `/Index` faz parte do URI para acessar a página anterior.</span><span class="sxs-lookup"><span data-stu-id="b1d21-268">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="b1d21-269">A cadeia de caracteres `/Index` pode ser usada para gerar URIs para a página *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b1d21-269">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="b1d21-270">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b1d21-270">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="b1d21-271">O nome da página é o caminho para a página da pasta raiz */Pages* (incluindo um `/` à direita, por exemplo, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="b1d21-271">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="b1d21-272">As amostras anteriores de geração de URL são muito mais ricas em recursos do que apenas codificar uma URL.</span><span class="sxs-lookup"><span data-stu-id="b1d21-272">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="b1d21-273">A geração de URL usa [roteamento](xref:mvc/controllers/routing) e pode gerar e codificar parâmetros de acordo com o modo como a rota é definida no caminho de destino.</span><span class="sxs-lookup"><span data-stu-id="b1d21-273">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="b1d21-274">A Geração de URL para páginas dá suporte a nomes relativos.</span><span class="sxs-lookup"><span data-stu-id="b1d21-274">URL generation for pages supports relative names.</span></span> <span data-ttu-id="b1d21-275">A tabela a seguir mostra qual página de Índice é selecionada com diferentes parâmetros `RedirectToPage` de *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b1d21-275">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="b1d21-276">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="b1d21-276">RedirectToPage(x)</span></span>| <span data-ttu-id="b1d21-277">Página</span><span class="sxs-lookup"><span data-stu-id="b1d21-277">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="b1d21-278">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="b1d21-278">RedirectToPage("/Index")</span></span> | <span data-ttu-id="b1d21-279">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="b1d21-279">*Pages/Index*</span></span> |
| <span data-ttu-id="b1d21-280">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="b1d21-280">RedirectToPage("./Index");</span></span> | <span data-ttu-id="b1d21-281">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="b1d21-281">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="b1d21-282">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="b1d21-282">RedirectToPage("../Index")</span></span> | <span data-ttu-id="b1d21-283">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="b1d21-283">*Pages/Index*</span></span> |
| <span data-ttu-id="b1d21-284">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="b1d21-284">RedirectToPage("Index")</span></span>  | <span data-ttu-id="b1d21-285">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="b1d21-285">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="b1d21-286">`RedirectToPage("Index")`, `RedirectToPage("./Index")` e `RedirectToPage("../Index")` são *nomes relativos*.</span><span class="sxs-lookup"><span data-stu-id="b1d21-286">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="b1d21-287">O parâmetro `RedirectToPage` é *combinado* com o caminho da página atual para calcular o nome da página de destino.</span><span class="sxs-lookup"><span data-stu-id="b1d21-287">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="b1d21-288">Vinculação de nome relativo é útil ao criar sites com uma estrutura complexa.</span><span class="sxs-lookup"><span data-stu-id="b1d21-288">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="b1d21-289">Se você usar nomes relativos para vincular entre páginas em uma pasta, você poderá renomear essa pasta.</span><span class="sxs-lookup"><span data-stu-id="b1d21-289">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="b1d21-290">Todos os links ainda funcionarão (porque eles não incluirão o nome da pasta).</span><span class="sxs-lookup"><span data-stu-id="b1d21-290">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="b1d21-291">TempData</span><span class="sxs-lookup"><span data-stu-id="b1d21-291">TempData</span></span>

<span data-ttu-id="b1d21-292">O ASP.NET Core expõe a propriedade [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) em um [controlador](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="b1d21-292">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="b1d21-293">Essa propriedade armazena dados até eles serem lidos.</span><span class="sxs-lookup"><span data-stu-id="b1d21-293">This property stores data until it is read.</span></span> <span data-ttu-id="b1d21-294">Os métodos `Keep` e `Peek` podem ser usados para examinar os dados sem exclusão.</span><span class="sxs-lookup"><span data-stu-id="b1d21-294">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="b1d21-295">`TempData` é útil para redirecionamento nos casos em que os dados são necessários para mais de uma única solicitação.</span><span class="sxs-lookup"><span data-stu-id="b1d21-295">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="b1d21-296">O atributo `[TempData]` é novo no ASP.NET Core 2.0 e tem suporte em controladores e páginas.</span><span class="sxs-lookup"><span data-stu-id="b1d21-296">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="b1d21-297">Os conjuntos de código a seguir definem o valor de `Message` usando `TempData`:</span><span class="sxs-lookup"><span data-stu-id="b1d21-297">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="b1d21-298">A marcação a seguir no arquivo *Pages/Customers/Index.cshtml* exibe o valor de `Message` usando `TempData`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-298">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="b1d21-299">O arquivo code-behind *Pages/Customers/Index.cshtml.cs* aplica o atributo `[TempData]` à propriedade `Message`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-299">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="b1d21-300">Consulte [TempData](xref:fundamentals/app-state#temp) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="b1d21-300">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="b1d21-301">Vários manipuladores por página</span><span class="sxs-lookup"><span data-stu-id="b1d21-301">Multiple handlers per page</span></span>

<span data-ttu-id="b1d21-302">A página a seguir gera marcação para dois manipuladores de página usando o auxiliar de marcas `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="b1d21-302">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="b1d21-303">O formulário no exemplo anterior tem dois botões de envio, cada um usando o `FormActionTagHelper` para enviar para uma URL diferente.</span><span class="sxs-lookup"><span data-stu-id="b1d21-303">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="b1d21-304">O atributo `asp-page-handler` é um complemento para `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-304">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="b1d21-305">`asp-page-handler` gera URLs que enviam para cada um dos métodos de manipulador definidos por uma página.</span><span class="sxs-lookup"><span data-stu-id="b1d21-305">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="b1d21-306">`asp-page` não foi especificado porque a amostra está vinculando à página atual.</span><span class="sxs-lookup"><span data-stu-id="b1d21-306">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="b1d21-307">O arquivo code-behind:</span><span class="sxs-lookup"><span data-stu-id="b1d21-307">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="b1d21-308">O código anterior usa *métodos de manipulador nomeados*.</span><span class="sxs-lookup"><span data-stu-id="b1d21-308">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="b1d21-309">Métodos de manipulador nomeados são criados colocando o texto no nome após `On<HTTP Verb>` e antes de `Async` (se houver).</span><span class="sxs-lookup"><span data-stu-id="b1d21-309">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="b1d21-310">No exemplo anterior, os métodos de página são OnPost**JoinList**Async e OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="b1d21-310">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="b1d21-311">Com *OnPost* e *Async* removidos, os nomes de manipulador são `JoinList` e `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-311">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="b1d21-312">Usando o código anterior, o caminho da URL que envia a `OnPostJoinListAsync` é `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-312">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="b1d21-313">O caminho da URL que envia a `OnPostJoinListUCAsync` é `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-313">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="b1d21-314">Personalizando o roteamento</span><span class="sxs-lookup"><span data-stu-id="b1d21-314">Customizing Routing</span></span>

<span data-ttu-id="b1d21-315">Se você não deseja a cadeia de consulta `?handler=JoinList` na URL, você pode alterar a rota para colocar o nome do manipulador na parte do caminho da URL.</span><span class="sxs-lookup"><span data-stu-id="b1d21-315">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="b1d21-316">Você pode personalizar a rota adicionando um modelo de rota entre aspas duplas após a diretiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="b1d21-316">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="b1d21-317">A rota anterior coloca o nome do manipulador no caminho da URL em vez da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="b1d21-317">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="b1d21-318">O `?` após `handler` significa que o parâmetro de rota é opcional.</span><span class="sxs-lookup"><span data-stu-id="b1d21-318">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="b1d21-319">Você pode usar `@page` para adicionar parâmetros e segmentos adicionais a uma rota de página.</span><span class="sxs-lookup"><span data-stu-id="b1d21-319">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="b1d21-320">Tudo que está lá está **acrescentado** à rota padrão da página.</span><span class="sxs-lookup"><span data-stu-id="b1d21-320">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="b1d21-321">Não há suporte para o uso de um caminho absoluto ou virtual para alterar a rota da página (como `"~/Some/Other/Path"`).</span><span class="sxs-lookup"><span data-stu-id="b1d21-321">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="b1d21-322">Configuração e definições</span><span class="sxs-lookup"><span data-stu-id="b1d21-322">Configuration and settings</span></span>

<span data-ttu-id="b1d21-323">Para configurar opções avançadas, use o método de extensão `AddRazorPagesOptions` no construtor de MVC:</span><span class="sxs-lookup"><span data-stu-id="b1d21-323">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="b1d21-324">No momento, você pode usar o `RazorPagesOptions` para definir o diretório raiz para páginas ou adicionar as convenções de modelo de aplicativo para páginas.</span><span class="sxs-lookup"><span data-stu-id="b1d21-324">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="b1d21-325">Permitiremos mais extensibilidade dessa maneira no futuro.</span><span class="sxs-lookup"><span data-stu-id="b1d21-325">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="b1d21-326">Para pré-compilar exibições, consulte [Compilação de exibição do Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="b1d21-326">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="b1d21-327">[Baixar ou exibir código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="b1d21-327">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="b1d21-328">Consulte [Introdução a Páginas do Razor no ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), que se baseia nesta introdução.</span><span class="sxs-lookup"><span data-stu-id="b1d21-328">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="b1d21-329">Especificar que as Páginas Razor estão na raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="b1d21-329">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="b1d21-330">Por padrão, as Páginas Razor estão na raiz do diretório */Pages*.</span><span class="sxs-lookup"><span data-stu-id="b1d21-330">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="b1d21-331">Adicione [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) em [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) para especificar que as Páginas Razor estão na raiz do conteúdo ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="b1d21-331">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="b1d21-332">Especificar que as Páginas Razor estão em um diretório raiz personalizado</span><span class="sxs-lookup"><span data-stu-id="b1d21-332">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="b1d21-333">Adicione [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) em [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) para especificar que as Páginas Razor estão em um diretório raiz personalizado no aplicativo (forneça um caminho relativo):</span><span class="sxs-lookup"><span data-stu-id="b1d21-333">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="b1d21-334">Consulte também</span><span class="sxs-lookup"><span data-stu-id="b1d21-334">See also</span></span>

* [<span data-ttu-id="b1d21-335">Introdução a Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="b1d21-335">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="b1d21-336">Convenções de autorização de Páginas Razor</span><span class="sxs-lookup"><span data-stu-id="b1d21-336">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="b1d21-337">Provedores de modelo personalizado de página e rota de Páginas Razor</span><span class="sxs-lookup"><span data-stu-id="b1d21-337">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="b1d21-338">Testes de integração e unidade de Páginas Razor</span><span class="sxs-lookup"><span data-stu-id="b1d21-338">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
