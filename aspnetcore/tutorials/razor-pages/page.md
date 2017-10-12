---
title: "Páginas do Razor geradas por scaffolding no ASP.NET Core"
author: rick-anderson
description: "Explica as Páginas do Razor geradas por scaffolding."
keywords: "ASP.NET Core, Páginas do Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 09/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: 7ae83b9bdadf5ebf8846b0c09c585da406708d12
ms.sourcegitcommit: 94b7e0f95b92c98b182a93d2b3dc0287e5f97976
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2017
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="3ed61-104">Páginas do Razor geradas por scaffolding no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ed61-104">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="3ed61-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3ed61-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3ed61-106">Este tutorial examina as Páginas do Razor criadas por scaffolding no tópico [Adicionar um modelo](xref:tutorials/razor-pages/model#scaffold-the-movie-model) do tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="3ed61-106">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial topic [Adding a model](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span></span> 

<span data-ttu-id="3ed61-107">[Exiba ou baixe](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) a amostra.</span><span class="sxs-lookup"><span data-stu-id="3ed61-107">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="3ed61-108">As páginas Criar, Excluir, Detalhes e Editar.</span><span class="sxs-lookup"><span data-stu-id="3ed61-108">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="3ed61-109">Analise o arquivo code-behind *Pages/Movies/Index.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="3ed61-109">Examine the *Pages/Movies/Index.cshtml.cs* code-behind file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>

<span data-ttu-id="3ed61-110">As Páginas do Razor são derivadas de `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="3ed61-110">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="3ed61-111">Por convenção, a classe derivada de `PageModel` é chamada de `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="3ed61-111">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="3ed61-112">O construtor usa [injeção de dependência](xref:fundamentals/dependency-injection) para adicionar o `MovieContext` à página.</span><span class="sxs-lookup"><span data-stu-id="3ed61-112">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="3ed61-113">Todas as páginas geradas por scaffolding seguem esse padrão.</span><span class="sxs-lookup"><span data-stu-id="3ed61-113">All the scaffolded pages follow this pattern.</span></span>

<span data-ttu-id="3ed61-114">Quando uma solicitação é feita à página, o método `OnGetAsync` retorna uma lista de filmes para a Página do Razor.</span><span class="sxs-lookup"><span data-stu-id="3ed61-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="3ed61-115">`OnGetAsync` ou `OnGet` é chamado em uma Página do Razor para inicializar o estado da página.</span><span class="sxs-lookup"><span data-stu-id="3ed61-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="3ed61-116">Nesse caso, `OnGetAsync` obtém uma lista de filmes para exibir.</span><span class="sxs-lookup"><span data-stu-id="3ed61-116">In this case, `OnGetAsync` gets a list of movies to display.</span></span>

<span data-ttu-id="3ed61-117">Examine a Página do Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3ed61-117">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="3ed61-118">O Razor pode fazer a transição do HTML em C# ou em marcação específica do Razor.</span><span class="sxs-lookup"><span data-stu-id="3ed61-118">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="3ed61-119">Quando um símbolo `@` é seguido por uma [palavra-chave reservada do Razor](xref:mvc/views/razor#razor-reserved-keywords), ele faz a transição para marcação específica do Razor, caso contrário, ele faz a transição para C#.</span><span class="sxs-lookup"><span data-stu-id="3ed61-119">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="3ed61-120">A diretiva do Razor `@page` transforma o arquivo em uma ação do MVC &mdash;, o que significa que ele pode manipular as solicitações.</span><span class="sxs-lookup"><span data-stu-id="3ed61-120">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="3ed61-121">`@page` deve ser a primeira diretiva do Razor em uma página.</span><span class="sxs-lookup"><span data-stu-id="3ed61-121">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="3ed61-122">`@page` é um exemplo de transição para a marcação específica do Razor.</span><span class="sxs-lookup"><span data-stu-id="3ed61-122">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="3ed61-123">Consulte [Sintaxe Razor](xref:mvc/views/razor#razor-syntax) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="3ed61-123">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="3ed61-124">Examine a expressão lambda usada no auxiliar HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="3ed61-124">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="3ed61-125">O auxiliar HTML `DisplayNameFor` inspeciona a propriedade `Title` referenciada na expressão lambda para determinar o nome de exibição.</span><span class="sxs-lookup"><span data-stu-id="3ed61-125">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="3ed61-126">A expressão lambda é inspecionada em vez de avaliada.</span><span class="sxs-lookup"><span data-stu-id="3ed61-126">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="3ed61-127">Isso significa que não há nenhuma violação de acesso quando `model`, `model.Movie` ou `model.Movie[0]` são `null` ou vazios.</span><span class="sxs-lookup"><span data-stu-id="3ed61-127">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="3ed61-128">Quando a expressão lambda é avaliada (por exemplo, com `@Html.DisplayFor(modelItem => item.Title)`), os valores de propriedade do modelo são avaliados.</span><span class="sxs-lookup"><span data-stu-id="3ed61-128">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="3ed61-129">A diretiva @model </span><span class="sxs-lookup"><span data-stu-id="3ed61-129">The @model directive</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="3ed61-130">A diretiva `@model` especifica o tipo de modelo passado para a Página do Razor.</span><span class="sxs-lookup"><span data-stu-id="3ed61-130">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="3ed61-131">No exemplo anterior, a linha `@model` torna a classe derivada de `PageModel` disponível para a Página do Razor.</span><span class="sxs-lookup"><span data-stu-id="3ed61-131">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="3ed61-132">O modelo é usado nos [auxiliares HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` e `@Html.DisplayName` na página.</span><span class="sxs-lookup"><span data-stu-id="3ed61-132">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="3ed61-133"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData e layout</span><span class="sxs-lookup"><span data-stu-id="3ed61-133"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="3ed61-134">Considere o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="3ed61-134">Consider the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

<span data-ttu-id="3ed61-135">O código realçado anterior é um exemplo de transição do Razor para C#.</span><span class="sxs-lookup"><span data-stu-id="3ed61-135">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="3ed61-136">Os caracteres `{` e `}` circunscrevem um bloco de código C#.</span><span class="sxs-lookup"><span data-stu-id="3ed61-136">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="3ed61-137">A classe base `PageModel` tem uma propriedade de dicionário `ViewData` que pode ser usada para adicionar os dados que você deseja passar para uma exibição.</span><span class="sxs-lookup"><span data-stu-id="3ed61-137">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="3ed61-138">Você adiciona objetos ao dicionário `ViewData` usando um padrão de chave/valor.</span><span class="sxs-lookup"><span data-stu-id="3ed61-138">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="3ed61-139">No exemplo anterior, a propriedade "Title" é adicionada ao dicionário `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="3ed61-139">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="3ed61-140">A propriedade "Título" é usada no arquivo *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3ed61-140">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="3ed61-141">A marcação a seguir mostra as primeiras linhas do arquivo *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3ed61-141">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

<span data-ttu-id="3ed61-142">A linha `@*Markup removed for brevity.*@` é um comentário do Razor.</span><span class="sxs-lookup"><span data-stu-id="3ed61-142">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="3ed61-143">Ao contrário de comentários HTML (`<!-- -->`), comentários do Razor não são enviados ao cliente.</span><span class="sxs-lookup"><span data-stu-id="3ed61-143">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="3ed61-144">Execute o aplicativo e teste os links no projeto (**Início**, **Sobre**, **Contato**, **Criar**, **Editar** e **Excluir**).</span><span class="sxs-lookup"><span data-stu-id="3ed61-144">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="3ed61-145">Cada página define o título, que pode ser visto na guia do navegador. Quando você coloca um indicador em uma página, o título é usado para o indicador.</span><span class="sxs-lookup"><span data-stu-id="3ed61-145">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="3ed61-146">*Pages/Index.cshtml* e *Pages/Movies/Index.cshtml* atualmente têm o mesmo título, mas você pode modificá-los para terem valores diferentes.</span><span class="sxs-lookup"><span data-stu-id="3ed61-146">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="3ed61-147">A propriedade `Layout` é definida no arquivo *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3ed61-147">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="3ed61-148">A marcação anterior define o arquivo de layout *Pages/_Layout.cshtml* para todos os arquivos do Razor na pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="3ed61-148">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="3ed61-149">Veja [Layout](xref:mvc/razor-pages/index#layout) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="3ed61-149">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="3ed61-150">Atualizar o layout</span><span class="sxs-lookup"><span data-stu-id="3ed61-150">Update the layout</span></span>

<span data-ttu-id="3ed61-151">Altere o elemento `<title>` no arquivo *Pages/_Layout.cshtml* para usar uma cadeia de caracteres mais curta.</span><span class="sxs-lookup"><span data-stu-id="3ed61-151">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="3ed61-152">Localizar o elemento de âncora a seguir no arquivo *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3ed61-152">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="3ed61-153">Substitua o elemento anterior pela marcação a seguir.</span><span class="sxs-lookup"><span data-stu-id="3ed61-153">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="3ed61-154">O elemento de âncora anterior é um [Auxiliar de Marcas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="3ed61-154">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="3ed61-155">Nesse caso, ele é o [Auxiliar de Marcas de Âncora](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="3ed61-155">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="3ed61-156">O atributo e valor do auxiliar de marcas `asp-page="/Movies/Index"` cria um link para a Página do Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="3ed61-156">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="3ed61-157">Salve suas alterações e teste o aplicativo clicando no link **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="3ed61-157">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="3ed61-158">Consulte o arquivo [cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) no GitHub.</span><span class="sxs-lookup"><span data-stu-id="3ed61-158">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-code-behind-page"></a><span data-ttu-id="3ed61-159">Página code-behind Criar</span><span class="sxs-lookup"><span data-stu-id="3ed61-159">The Create code-behind page</span></span>

<span data-ttu-id="3ed61-160">Examine o arquivo code-behind *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="3ed61-160">Examine the *Pages/Movies/Create.cshtml.cs* code-behind file:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="3ed61-161">O método `OnGet` inicializa qualquer estado necessário para a página.</span><span class="sxs-lookup"><span data-stu-id="3ed61-161">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="3ed61-162">A página Criar não tem nenhum estado para inicializar.</span><span class="sxs-lookup"><span data-stu-id="3ed61-162">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="3ed61-163">O método `Page` cria um objeto `PageResult` que renderiza a página *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3ed61-163">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="3ed61-164">A propriedade `Movie` usa o atributo `[BindProperty]` para aceitar a [associação de modelos](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="3ed61-164">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="3ed61-165">Quando o formulário Criar posta os valores de formulário, o tempo de execução do ASP.NET Core associa os valores postados ao modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="3ed61-165">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="3ed61-166">O método `OnPostAsync` é executado quando a página posta dados de formulário:</span><span class="sxs-lookup"><span data-stu-id="3ed61-166">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="3ed61-167">Se há algum erro de modelo, o formulário é reexibido juntamente com quaisquer dados de formulário postados.</span><span class="sxs-lookup"><span data-stu-id="3ed61-167">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="3ed61-168">A maioria dos erros de modelo podem ser capturados no lado do cliente antes do formulário ser enviado.</span><span class="sxs-lookup"><span data-stu-id="3ed61-168">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="3ed61-169">Um exemplo de um erro de modelo é postar, para o campo de data, um valor que não pode ser convertido em uma data.</span><span class="sxs-lookup"><span data-stu-id="3ed61-169">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="3ed61-170">Falaremos sobre a validação do lado do cliente e a validação de modelo posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="3ed61-170">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="3ed61-171">Se não há nenhum erro de modelo, os dados são salvos e o navegador é redirecionado à página Índice.</span><span class="sxs-lookup"><span data-stu-id="3ed61-171">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="3ed61-172">A Página do Razor Criar</span><span class="sxs-lookup"><span data-stu-id="3ed61-172">The Create Razor Page</span></span>

<span data-ttu-id="3ed61-173">Examine o arquivo na Página do Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3ed61-173">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<span data-ttu-id="3ed61-174">O Visual Studio exibe a marca `<form method="post">` em uma fonte diferente usada para os auxiliares de marcas.</span><span class="sxs-lookup"><span data-stu-id="3ed61-174">Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers.</span></span> <span data-ttu-id="3ed61-175">O elemento `<form method="post">` é um [auxiliar de marcas de formulário](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="3ed61-175">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="3ed61-176">O auxiliar de marcas de formulário inclui automaticamente um [token antifalsificação](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="3ed61-176">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

![Exibição de VS17 da página Create.cshtml](page/_static/th.png)

<span data-ttu-id="3ed61-178">O mecanismo de scaffolding cria marcação do Razor para cada campo no modelo (exceto a ID) semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="3ed61-178">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="3ed61-179">Os [auxiliares de marcas de validação](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` e ` <span asp-validation-for`) exibem erros de validação.</span><span class="sxs-lookup"><span data-stu-id="3ed61-179">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="3ed61-180">A validação será abordada em mais detalhes posteriormente nesta série.</span><span class="sxs-lookup"><span data-stu-id="3ed61-180">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="3ed61-181">O [auxiliar de marcas de rótulo](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) gera a legenda do rótulo e o atributo `for` para a propriedade `Title`.</span><span class="sxs-lookup"><span data-stu-id="3ed61-181">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="3ed61-182">O [auxiliar de marcas de entrada](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) usa os atributos [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) e produz os atributos HTML necessários para validação jQuery no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="3ed61-182">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="3ed61-183">O tutorial a seguir explica o LocalDB do SQL Server e a propagação do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3ed61-183">The next tutorial explains SQL Server LocalDB and seeding the database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3ed61-184">[Anterior: adicionando um modelo](xref:tutorials/razor-pages/model)
[Próximo: LocalDB do SQL Server](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="3ed61-184">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span></span>
