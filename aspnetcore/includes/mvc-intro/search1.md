# <a name="adding-search-to-an-aspnet-core-mvc-app"></a>Adicionando uma pesquisa a um aplicativo ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Nesta seção, você adiciona a funcionalidade de pesquisa ao método de ação `Index` que permite pesquisar filmes por *gênero* ou *nome*.

Atualize o método `Index` pelo seguinte código:
<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

A primeira linha do método de ação `Index` cria uma consulta [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) para selecionar os filmes:

```csharp
var movies = from m in _context.Movie
             select m;
```

A consulta é *somente* definida neste ponto; ela **não** foi executada no banco de dados.

Se o parâmetro `searchString` contiver uma cadeia de caracteres, a consulta de filmes será modificada para filtrar o valor da cadeia de caracteres de pesquisa:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

O código `s => s.Title.Contains()` acima é uma [Expressão Lambda](http://msdn.microsoft.com/library/bb397687.aspx). Lambdas são usados em consultas [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) baseadas em método como argumentos para métodos de operadores de consulta padrão, como o método [Where](http://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) ou `Contains` (usado no código acima). As consultas LINQ não são executadas quando são definidas ou quando são modificadas com uma chamada a um método como `Where`, `Contains` ou `OrderBy`. Em vez disso, a execução da consulta é adiada.  Isso significa que a avaliação de uma expressão é atrasada até que seu valor realizado seja, de fato, iterado ou o método `ToListAsync` seja chamado. Para obter mais informações sobre a execução de consulta adiada, consulte [Execução da consulta](http://msdn.microsoft.com/library/bb738633.aspx).

Observação: o método [Contains](http://msdn.microsoft.com/library/bb155125.aspx) é executado no banco de dados, não no código C# mostrado acima. A diferenciação de maiúsculas e minúsculas na consulta depende do banco de dados e do agrupamento. No SQL Server, [Contains](http://msdn.microsoft.com/library/bb155125.aspx) é mapeado para [SQL LIKE](http://msdn.microsoft.com/library/ms179859.aspx), que não diferencia maiúsculas de minúsculas. No SQLite, com o agrupamento padrão, ele diferencia maiúsculas de minúsculas.

Navegue para `/Movies/Index`. Acrescente uma cadeia de consulta, como `?searchString=Ghost`, à URL. Os filmes filtrados são exibidos.

![Exibição de índice](../../tutorials/first-mvc-app/search/_static/ghost.png)

Se você alterar a assinatura do método `Index` para que ele tenha um parâmetro chamado `id`, o parâmetro `id` corresponderá o espaço reservado `{id}` opcional com as rotas padrão definidas em *Startup.cs*.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]