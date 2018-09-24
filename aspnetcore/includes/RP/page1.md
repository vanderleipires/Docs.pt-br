# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="c2475-101">Páginas do Razor geradas por scaffolding no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2475-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c2475-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c2475-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c2475-103">Este tutorial examina as Páginas do Razor criadas por scaffolding no tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="c2475-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="c2475-104">[Exiba ou baixe](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) a amostra.</span><span class="sxs-lookup"><span data-stu-id="c2475-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="c2475-105">As páginas Criar, Excluir, Detalhes e Editar.</span><span class="sxs-lookup"><span data-stu-id="c2475-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="c2475-106">Examine o Modelo de Página, *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2475-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

<span data-ttu-id="c2475-107">As Páginas do Razor são derivadas de `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="c2475-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="c2475-108">Por convenção, a classe derivada de `PageModel` é chamada de `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="c2475-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="c2475-109">O construtor usa [injeção de dependência](xref:fundamentals/dependency-injection) para adicionar o `MovieContext` à página.</span><span class="sxs-lookup"><span data-stu-id="c2475-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="c2475-110">Todas as páginas geradas por scaffolding seguem esse padrão.</span><span class="sxs-lookup"><span data-stu-id="c2475-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="c2475-111">Consulte [Código assíncrono](xref:data/ef-rp/intro#asynchronous-code) para obter mais informações sobre a programação assíncrona com o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c2475-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="c2475-112">Quando uma solicitação é feita à página, o método `OnGetAsync` retorna uma lista de filmes para a Página do Razor.</span><span class="sxs-lookup"><span data-stu-id="c2475-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="c2475-113">`OnGetAsync` ou `OnGet` é chamado em uma Página do Razor para inicializar o estado da página.</span><span class="sxs-lookup"><span data-stu-id="c2475-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="c2475-114">Nesse caso, `OnGetAsync` obtém uma lista de filmes e os exibe.</span><span class="sxs-lookup"><span data-stu-id="c2475-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="c2475-115">Quando `OnGet` retorna `void` ou `OnGetAsync` retorna `Task`, então nenhum método de retorno é usado.</span><span class="sxs-lookup"><span data-stu-id="c2475-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="c2475-116">Quando o tipo de retorno for `IActionResult` ou `Task<IActionResult>`, é necessário fornecer uma instrução de retorno.</span><span class="sxs-lookup"><span data-stu-id="c2475-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="c2475-117">Por exemplo, o método `OnPostAsync` do arquivo *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2475-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<span data-ttu-id="c2475-118">Examine a Página do Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c2475-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="c2475-119">O Razor pode fazer a transição do HTML em C# ou em marcação específica do Razor.</span><span class="sxs-lookup"><span data-stu-id="c2475-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="c2475-120">Quando um símbolo `@` é seguido por uma [palavra-chave reservada do Razor](xref:mvc/views/razor#razor-reserved-keywords), ele faz a transição para marcação específica do Razor, caso contrário, ele faz a transição para C#.</span><span class="sxs-lookup"><span data-stu-id="c2475-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="c2475-121">A diretiva do Razor `@page` transforma o arquivo em uma ação do MVC &mdash;, o que significa que ele pode manipular as solicitações.</span><span class="sxs-lookup"><span data-stu-id="c2475-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="c2475-122">`@page` deve ser a primeira diretiva do Razor em uma página.</span><span class="sxs-lookup"><span data-stu-id="c2475-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="c2475-123">`@page` é um exemplo de transição para a marcação específica do Razor.</span><span class="sxs-lookup"><span data-stu-id="c2475-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="c2475-124">Consulte [Sintaxe Razor](xref:mvc/views/razor#razor-syntax) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="c2475-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="c2475-125">Examine a expressão lambda usada no auxiliar HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="c2475-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="c2475-126">O auxiliar HTML `DisplayNameFor` inspeciona a propriedade `Title` referenciada na expressão lambda para determinar o nome de exibição.</span><span class="sxs-lookup"><span data-stu-id="c2475-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="c2475-127">A expressão lambda é inspecionada em vez de avaliada.</span><span class="sxs-lookup"><span data-stu-id="c2475-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="c2475-128">Isso significa que não há nenhuma violação de acesso quando `model`, `model.Movie` ou `model.Movie[0]` são `null` ou vazios.</span><span class="sxs-lookup"><span data-stu-id="c2475-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="c2475-129">Quando a expressão lambda é avaliada (por exemplo, com `@Html.DisplayFor(modelItem => item.Title)`), os valores de propriedade do modelo são avaliados.</span><span class="sxs-lookup"><span data-stu-id="c2475-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="c2475-130">A diretiva @model</span><span class="sxs-lookup"><span data-stu-id="c2475-130">The @model directive</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="c2475-131">A diretiva `@model` especifica o tipo de modelo passado para a Página do Razor.</span><span class="sxs-lookup"><span data-stu-id="c2475-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="c2475-132">No exemplo anterior, a linha `@model` torna a classe derivada de `PageModel` disponível para a Página do Razor.</span><span class="sxs-lookup"><span data-stu-id="c2475-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="c2475-133">O modelo é usado nos [auxiliares HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` e `@Html.DisplayName` na página.</span><span class="sxs-lookup"><span data-stu-id="c2475-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="c2475-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData e layout</span><span class="sxs-lookup"><span data-stu-id="c2475-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="c2475-135">Considere o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="c2475-135">Consider the following code:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="c2475-136">O código realçado anterior é um exemplo de transição do Razor para C#.</span><span class="sxs-lookup"><span data-stu-id="c2475-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="c2475-137">Os caracteres `{` e `}` circunscrevem um bloco de código C#.</span><span class="sxs-lookup"><span data-stu-id="c2475-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="c2475-138">A classe base `PageModel` tem uma propriedade de dicionário `ViewData` que pode ser usada para adicionar os dados que você deseja passar para uma exibição.</span><span class="sxs-lookup"><span data-stu-id="c2475-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="c2475-139">Você adiciona objetos ao dicionário `ViewData` usando um padrão de chave/valor.</span><span class="sxs-lookup"><span data-stu-id="c2475-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="c2475-140">No exemplo anterior, a propriedade "Title" é adicionada ao dicionário `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="c2475-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c2475-141">A propriedade "Título" é usada no arquivo *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c2475-141">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="c2475-142">A marcação a seguir mostra as primeiras linhas do arquivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c2475-142">The following markup shows the first few lines of the *Pages/Shared/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c2475-143">A propriedade "Título" é usada no arquivo *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c2475-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="c2475-144">A marcação a seguir mostra as primeiras linhas do arquivo *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c2475-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="c2475-145">A linha `@*Markup removed for brevity.*@` é um comentário do Razor.</span><span class="sxs-lookup"><span data-stu-id="c2475-145">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="c2475-146">Ao contrário de comentários HTML (`<!-- -->`), comentários do Razor não são enviados ao cliente.</span><span class="sxs-lookup"><span data-stu-id="c2475-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="c2475-147">Execute o aplicativo e teste os links no projeto (**Início**, **Sobre**, **Contato**, **Criar**, **Editar** e **Excluir**).</span><span class="sxs-lookup"><span data-stu-id="c2475-147">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="c2475-148">Cada página define o título, que pode ser visto na guia do navegador. Quando você coloca um indicador em uma página, o título é usado para o indicador.</span><span class="sxs-lookup"><span data-stu-id="c2475-148">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="c2475-149">*Pages/Index.cshtml* e *Pages/Movies/Index.cshtml* atualmente têm o mesmo título, mas você pode modificá-los para terem valores diferentes.</span><span class="sxs-lookup"><span data-stu-id="c2475-149">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="c2475-150">Talvez você não consiga inserir casas decimais ou vírgulas no campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="c2475-150">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="c2475-151">Para dar suporte à [validação do jQuery](https://jqueryvalidation.org/) para localidades de idiomas diferentes do inglês que usam uma vírgula (“,”) para um ponto decimal e formatos de data diferentes do inglês dos EUA, você deve tomar medidas para globalizar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c2475-151">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="c2475-152">Veja [Problema 4076 do GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) para obter instruções sobre como adicionar casas decimais.</span><span class="sxs-lookup"><span data-stu-id="c2475-152">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="c2475-153">A propriedade `Layout` é definida no arquivo *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c2475-153">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="c2475-154">A marcação anterior define o arquivo de layout como *Pages/Shared/_Layout.cshtml* para todos os arquivos do Razor na pasta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="c2475-154">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="c2475-155">Veja [Layout](xref:razor-pages/index#layout) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="c2475-155">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="c2475-156">Atualizar o layout</span><span class="sxs-lookup"><span data-stu-id="c2475-156">Update the layout</span></span>

<span data-ttu-id="c2475-157">Altere o elemento `<title>` no arquivo *Pages/Shared/_Layout.cshtml* para usar uma cadeia de caracteres mais curta.</span><span class="sxs-lookup"><span data-stu-id="c2475-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="c2475-158">Localizar o elemento de âncora a seguir no arquivo *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c2475-158">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="c2475-159">Substitua o elemento anterior pela marcação a seguir.</span><span class="sxs-lookup"><span data-stu-id="c2475-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="c2475-160">O elemento de âncora anterior é um [Auxiliar de Marcas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c2475-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="c2475-161">Nesse caso, ele é o [Auxiliar de Marcas de Âncora](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="c2475-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="c2475-162">O atributo e valor do auxiliar de marcas `asp-page="/Movies/Index"` cria um link para a Página do Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="c2475-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="c2475-163">Salve suas alterações e teste o aplicativo clicando no link **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="c2475-163">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="c2475-164">Consulte o arquivo [cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) no GitHub.</span><span class="sxs-lookup"><span data-stu-id="c2475-164">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="c2475-165">O modelo Criar página</span><span class="sxs-lookup"><span data-stu-id="c2475-165">The Create page model</span></span>

<span data-ttu-id="c2475-166">Examine o modelo de página *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2475-166">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]

::: moniker-end


<span data-ttu-id="c2475-167">O método `OnGet` inicializa qualquer estado necessário para a página.</span><span class="sxs-lookup"><span data-stu-id="c2475-167">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="c2475-168">A página Criar não tem nenhum estado para inicializar, assim, `Page` é retornado.</span><span class="sxs-lookup"><span data-stu-id="c2475-168">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="c2475-169">Mais adiante no tutorial, você verá o estado de inicialização do método `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="c2475-169">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="c2475-170">O método `Page` cria um objeto `PageResult` que renderiza a página *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c2475-170">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="c2475-171">A propriedade `Movie` usa o atributo `[BindProperty]` para aceitar o [model binding](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="c2475-171">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="c2475-172">Quando o formulário Criar posta os valores de formulário, o tempo de execução do ASP.NET Core associa os valores postados ao modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="c2475-172">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="c2475-173">O método `OnPostAsync` é executado quando a página posta dados de formulário:</span><span class="sxs-lookup"><span data-stu-id="c2475-173">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="c2475-174">Se há algum erro de modelo, o formulário é reexibido juntamente com quaisquer dados de formulário postados.</span><span class="sxs-lookup"><span data-stu-id="c2475-174">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="c2475-175">A maioria dos erros de modelo podem ser capturados no lado do cliente antes do formulário ser enviado.</span><span class="sxs-lookup"><span data-stu-id="c2475-175">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="c2475-176">Um exemplo de um erro de modelo é postar, para o campo de data, um valor que não pode ser convertido em uma data.</span><span class="sxs-lookup"><span data-stu-id="c2475-176">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="c2475-177">Falaremos sobre a validação do lado do cliente e a validação de modelo posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="c2475-177">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="c2475-178">Se não há nenhum erro de modelo, os dados são salvos e o navegador é redirecionado à página Índice.</span><span class="sxs-lookup"><span data-stu-id="c2475-178">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="c2475-179">A Página do Razor Criar</span><span class="sxs-lookup"><span data-stu-id="c2475-179">The Create Razor Page</span></span>

<span data-ttu-id="c2475-180">Examine o arquivo na Página do Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c2475-180">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
