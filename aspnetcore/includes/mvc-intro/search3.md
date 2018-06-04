<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="3f0da-101">Agora, quando você enviar uma pesquisa, a URL conterá a cadeia de consulta da pesquisa.</span><span class="sxs-lookup"><span data-stu-id="3f0da-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="3f0da-102">A pesquisa também irá para o método de ação `HttpGet Index`, mesmo se você tiver um método `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="3f0da-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Janela do navegador mostrando a searchString=ghost= na URL e os filmes retornados, Ghostbusters e Ghostbusters 2, que contêm a palavra “ghost”](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="3f0da-104">A seguinte marcação mostra a alteração para a marcação `form`:</span><span class="sxs-lookup"><span data-stu-id="3f0da-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="3f0da-105">Adicionando uma pesquisa por gênero</span><span class="sxs-lookup"><span data-stu-id="3f0da-105">Adding Search by genre</span></span>

<span data-ttu-id="3f0da-106">Adicione a seguinte classe `MovieGenreViewModel` à pasta *Models*:</span><span class="sxs-lookup"><span data-stu-id="3f0da-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="3f0da-107">O modelo de exibição do gênero de filme conterá:</span><span class="sxs-lookup"><span data-stu-id="3f0da-107">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="3f0da-108">Uma lista de filmes.</span><span class="sxs-lookup"><span data-stu-id="3f0da-108">A list of movies.</span></span>
   * <span data-ttu-id="3f0da-109">Uma `SelectList` que contém a lista de gêneros.</span><span class="sxs-lookup"><span data-stu-id="3f0da-109">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="3f0da-110">Isso permitirá que o usuário selecione um gênero na lista.</span><span class="sxs-lookup"><span data-stu-id="3f0da-110">This will allow the user to select a genre from the list.</span></span>
   * <span data-ttu-id="3f0da-111">`movieGenre`, que contém o gênero selecionado.</span><span class="sxs-lookup"><span data-stu-id="3f0da-111">`movieGenre`, which contains the selected genre.</span></span>

<span data-ttu-id="3f0da-112">Substitua o método `Index` em `MoviesController.cs` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="3f0da-112">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="3f0da-113">O código a seguir é uma consulta `LINQ` que recupera todos os gêneros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3f0da-113">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="3f0da-114">A `SelectList` de gêneros é criada com a projeção dos gêneros distintos (não desejamos que nossa lista de seleção tenha gêneros duplicados).</span><span class="sxs-lookup"><span data-stu-id="3f0da-114">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="3f0da-115">Adicionando uma pesquisa por gênero à exibição Índice</span><span class="sxs-lookup"><span data-stu-id="3f0da-115">Adding search by genre to the Index view</span></span>

<span data-ttu-id="3f0da-116">Atualize `Index.cshtml` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f0da-116">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="3f0da-117">Examine a expressão lambda usada no seguinte Auxiliar de HTML:</span><span class="sxs-lookup"><span data-stu-id="3f0da-117">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
<span data-ttu-id="3f0da-118">No código anterior, o Auxiliar de HTML `DisplayNameFor` inspeciona a propriedade `Title` referenciada na expressão lambda para determinar o nome de exibição.</span><span class="sxs-lookup"><span data-stu-id="3f0da-118">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="3f0da-119">Como a expressão lambda é inspecionada em vez de avaliada, você não recebe uma violação de acesso quando `model`, `model.movies` ou `model.movies[0]` é `null` ou vazio.</span><span class="sxs-lookup"><span data-stu-id="3f0da-119">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.movies`, or `model.movies[0]` are `null` or empty.</span></span> <span data-ttu-id="3f0da-120">Quando a expressão lambda é avaliada (por exemplo, `@Html.DisplayFor(modelItem => item.Title)`), os valores da propriedade do modelo são avaliados.</span><span class="sxs-lookup"><span data-stu-id="3f0da-120">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="3f0da-121">Teste o aplicativo pesquisando por gênero, título do filme e por ambos.</span><span class="sxs-lookup"><span data-stu-id="3f0da-121">Test the app by searching by genre, by movie title, and by both.</span></span>
