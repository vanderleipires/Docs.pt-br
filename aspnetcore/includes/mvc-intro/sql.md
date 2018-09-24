# <a name="work-with-sqlite-in-an-aspnet-core-mvc-app"></a>Trabalhar com o SQLite em um aplicativo ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

O objeto `MvcMovieContext` cuida da tarefa de se conectar ao banco de dados e mapear objetos `Movie` para registros do banco de dados. O contexto de banco de dados é registrado com o contêiner [Injeção de Dependência](xref:fundamentals/dependency-injection) no método `ConfigureServices` no arquivo *Startup.cs*:

[!code-csharp[](~/tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a>SQLite

O site do [SQLite](https://www.sqlite.org/) afirma:

> O SQLite é um mecanismo de banco de dados SQL independente, de alta confiabilidade, inserido, completo e de domínio público. O SQLite é o mecanismo de banco de dados mais usado no mundo.

Há muitas ferramentas de terceiros que podem ser baixadas para gerenciar e exibir um banco de dados SQLite. A imagem abaixo é do [Navegador do DB para SQLite](http://sqlitebrowser.org/). Se você tiver uma ferramenta favorita do SQLite, deixe um comentário sobre o que mais gosta dela.

![Navegador do BD do SQLite mostrando o BD de filmes](~/tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Propagar o banco de dados

Crie uma nova classe chamada `SeedData` na pasta *Models*. Substitua o código gerado pelo seguinte:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Se houver um filme no BD, o inicializador de semeadura será retornado.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Adicionar o inicializador de semeadura

Adicione o inicializador de semeadura ao método `Main` no arquivo *Program.cs*:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

::: moniker-end

### <a name="test-the-app"></a>Testar o aplicativo

Exclua todos os registros no BD (para que o método de semeadura seja executado). Interrompa e inicie o aplicativo para propagar o banco de dados.
   
O aplicativo mostra os dados propagados.

![Navegador aberto no aplicativo MVC de Filmes mostrando os dados de filmes](~/tutorials/first-mvc-app/working-with-sql/_static/m55.png)
