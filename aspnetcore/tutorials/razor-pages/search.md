---
title: Adicionar a pesquisa às Páginas Razor do ASP.NET Core
author: rick-anderson
description: Mostra como adicionar uma pesquisa às Páginas Razor do ASP.NET Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: 80292f8cfecd5363fb8acc8578f9bb0ca9ee5969
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090157"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a><span data-ttu-id="1b8a9-103">Adicionar a pesquisa às Páginas Razor do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1b8a9-103">Add search to ASP.NET Core Razor Pages</span></span>

<span data-ttu-id="1b8a9-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1b8a9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1b8a9-105">Neste documento, a funcionalidade de pesquisa é adicionada à página de Índice que permite pesquisar filmes por *gênero* ou *nome*.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-105">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="1b8a9-106">Adicione as seguintes propriedades realçadas em *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1b8a9-106">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end

* <span data-ttu-id="1b8a9-107">`SearchString`: contém o texto que os usuários inserem na caixa de texto de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-107">`SearchString`: contains the text users enter in the search text box.</span></span>
* <span data-ttu-id="1b8a9-108">`Genres`: contém a lista de gêneros.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-108">`Genres`: contains the list of genres.</span></span> <span data-ttu-id="1b8a9-109">Isso permite que o usuário selecione um gênero na lista.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-109">This allows the user to select a genre from the list.</span></span>
* <span data-ttu-id="1b8a9-110">`MovieGenre`: contém o gênero específico que o usuário seleciona (por exemplo, "Oeste").</span><span class="sxs-lookup"><span data-stu-id="1b8a9-110">`MovieGenre`: contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="1b8a9-111">Você trabalhará com as propriedades `Genres` e `MovieGenre` neste documento.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-111">You'll work with the `Genres` and `MovieGenre` properties later in this document.</span></span>

<span data-ttu-id="1b8a9-112">Atualize o método `OnGetAsync` da página de Índice pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="1b8a9-112">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="1b8a9-113">A primeira linha do método `OnGetAsync` cria uma consulta [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) para selecionar os filmes:</span><span class="sxs-lookup"><span data-stu-id="1b8a9-113">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="1b8a9-114">A consulta é *somente* definida neste ponto; ela **não** foi executada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-114">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="1b8a9-115">Se o parâmetro `searchString` contiver uma cadeia de caracteres, a consulta de filmes será modificada para filtrar a cadeia de caracteres de pesquisa:</span><span class="sxs-lookup"><span data-stu-id="1b8a9-115">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="1b8a9-116">O código `s => s.Title.Contains()` é uma [Expressão Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="1b8a9-116">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="1b8a9-117">Lambdas são usados em consultas [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) baseadas em método como argumentos para métodos de operadores de consulta padrão, como o método [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) ou `Contains` (usado no código anterior).</span><span class="sxs-lookup"><span data-stu-id="1b8a9-117">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="1b8a9-118">As consultas LINQ não são executadas quando são definidas ou quando são modificadas com uma chamada a um método (como `Where`, `Contains` ou `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="1b8a9-118">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="1b8a9-119">Em vez disso, a execução da consulta é adiada.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-119">Rather, query execution is deferred.</span></span> <span data-ttu-id="1b8a9-120">Isso significa que a avaliação de uma expressão é atrasada até que seu valor realizado seja iterado ou o método `ToListAsync` seja chamado.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-120">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="1b8a9-121">Consulte [Execução de consulta](/dotnet/framework/data/adonet/ef/language-reference/query-execution) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-121">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="1b8a9-122">**Observação:** o método [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) é executado no banco de dados, não no código C#.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-122">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="1b8a9-123">A diferenciação de maiúsculas e minúsculas na consulta depende do banco de dados e da ordenação.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-123">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="1b8a9-124">No SQL Server, `Contains` é mapeado para [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), que não diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-124">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="1b8a9-125">No SQLite, com a ordenação padrão, ele diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-125">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="1b8a9-126">Por fim, a linha final do método `OnGetAsync` preenche a propriedade `SearchString` com o valor de pesquisa do usuário.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-126">Lastly, the final line of the `OnGetAsync` method populates the `SearchString` property with the user's search value.</span></span> <span data-ttu-id="1b8a9-127">Com a propriedade `SearchString` preenchida, o valor de pesquisa é mantido na caixa de pesquisa após a execução da pesquisa.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-127">With the `SearchString` property populated, the search value is retained in the search box after the search executes.</span></span>

<span data-ttu-id="1b8a9-128">Navegue para a página Movies e acrescente uma cadeia de consulta, como `?searchString=Ghost`, à URL (por exemplo, `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="1b8a9-128">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="1b8a9-129">Os filmes filtrados são exibidos.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-129">The filtered movies are displayed.</span></span>

![Exibição de índice](search/_static/ghost.png)

<span data-ttu-id="1b8a9-131">Se o modelo de rota a seguir for adicionado à página de Índice, a cadeia de caracteres de pesquisa poderá ser passada como um segmento de URL (por exemplo, `http://localhost:5000/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="1b8a9-131">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="1b8a9-132">A restrição da rota anterior permite a pesquisa do título como dados de rota (um segmento de URL), em vez de como um valor de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-132">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="1b8a9-133">O `?` em `"{searchString?}"` significa que esse é um parâmetro de rota opcional.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-133">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Exibição de índice com a palavra “ghost” adicionada à URL e uma lista de filmes retornados com dois filmes, Ghostbusters e Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="1b8a9-135">No entanto, você não pode esperar que os usuários modifiquem a URL para pesquisar um filme.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-135">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="1b8a9-136">Nesta etapa, a interface do usuário é adicionada para filtrar filmes.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-136">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="1b8a9-137">Se você adicionou a restrição de rota `"{searchString?}"`, remova-a.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-137">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="1b8a9-138">Abra o arquivo *Pages/Movies/Index.cshtml* e, em seguida, adicione a marcação `<form>` realçada no seguinte código:</span><span class="sxs-lookup"><span data-stu-id="1b8a9-138">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="1b8a9-139">A marcação `<form>` HTML usa o [Auxiliar de Marcação de Formulário](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="1b8a9-139">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="1b8a9-140">Quando o formulário é enviado, a cadeia de caracteres de filtro é enviada para a página *Pages/Movies/Index*.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-140">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="1b8a9-141">Salve as alterações e teste o filtro.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-141">Save the changes and test the filter.</span></span>

![Exibição de índice com a palavra “ghost” digitada na caixa de texto Filtro de título](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="1b8a9-143">Pesquisar por gênero</span><span class="sxs-lookup"><span data-stu-id="1b8a9-143">Search by genre</span></span>

<span data-ttu-id="1b8a9-144">Atualize o método `OnGetAsync` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="1b8a9-144">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="1b8a9-145">O código a seguir é uma consulta LINQ que recupera todos os gêneros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-145">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="1b8a9-146">O `SelectList` de gêneros é criado com a projeção dos gêneros distintos.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-146">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="1b8a9-147">Adicionando uma pesquisa por gênero</span><span class="sxs-lookup"><span data-stu-id="1b8a9-147">Adding search by genre</span></span>

<span data-ttu-id="1b8a9-148">Atualize *Index.cshtml* da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="1b8a9-148">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="1b8a9-149">Teste o aplicativo pesquisando por gênero, título do filme e por ambos.</span><span class="sxs-lookup"><span data-stu-id="1b8a9-149">Test the app by searching by genre, by movie title, and by both.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1b8a9-150">[Anterior: Atualizando as páginas](xref:tutorials/razor-pages/da1)
> [Próximo: Adicionando um novo campo](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="1b8a9-150">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
