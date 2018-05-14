<!--
[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Agora, quando você enviar uma pesquisa, a URL conterá a cadeia de consulta da pesquisa. A pesquisa também irá para o método de ação `HttpGet Index`, mesmo se você tiver um método `HttpPost Index`.

![Janela do navegador mostrando a searchString=ghost= na URL e os filmes retornados, Ghostbusters e Ghostbusters 2, que contêm a palavra “ghost”](../../tutorials/first-mvc-app/search/_static/search_get.png)

A seguinte marcação mostra a alteração para a marcação `form`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a>Adicionando uma pesquisa por gênero

Adicione a seguinte classe `MovieGenreViewModel` à pasta *Models*:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

O modelo de exibição do gênero de filme conterá:

   * Uma lista de filmes.
   * Uma `SelectList` que contém a lista de gêneros. Isso permitirá que o usuário selecione um gênero na lista.
   * `movieGenre`, que contém o gênero selecionado.

Substitua o método `Index` em `MoviesController.cs` pelo seguinte código:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

O código a seguir é uma consulta `LINQ` que recupera todos os gêneros do banco de dados.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

A `SelectList` de gêneros é criada com a projeção dos gêneros distintos (não desejamos que nossa lista de seleção tenha gêneros duplicados).

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a>Adicionando uma pesquisa por gênero à exibição Índice

Atualize `Index.cshtml` da seguinte maneira:

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

Examine a expressão lambda usada no seguinte Auxiliar de HTML:

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
No código anterior, o Auxiliar de HTML `DisplayNameFor` inspeciona a propriedade `Title` referenciada na expressão lambda para determinar o nome de exibição. Como a expressão lambda é inspecionada em vez de avaliada, você não recebe uma violação de acesso quando `model`, `model.movies` ou `model.movies[0]` é `null` ou vazio. Quando a expressão lambda é avaliada (por exemplo, `@Html.DisplayFor(modelItem => item.Title)`), os valores da propriedade do modelo são avaliados.

Teste o aplicativo pesquisando por gênero, título do filme e por ambos.
