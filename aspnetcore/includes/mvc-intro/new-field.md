<!-- This include not used by windows version -->
# <a name="adding-a-new-field"></a>Adicionando um novo campo

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial adicionará um novo campo à tabela `Movies`. Removeremos o banco de dados e criaremos um novo ao alterar o esquema (adicionar um novo campo). Este fluxo de trabalho funciona bem no início do desenvolvimento quando não temos nenhum dado de produção para preservar.

Depois que o aplicativo for implantado e você tiver dados que precisa preservar, não poderá remover o BD quando precisar alterar o esquema. As [Migrações do Entity Framework Code First](/ef/core/get-started/aspnetcore/new-db) permitem atualizar o esquema e migrar o banco de dados sem perder dados. As Migrações são um recurso popular ao usar o SQL Server, mas o SQLite não dá suporte a muitas operações de esquema de migração e, portanto, apenas migrações muito simples são possíveis. Consulte [Limitações do SQLite](/ef/core/providers/sqlite/limitations) para obter mais informações.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adicionando uma propriedade de classificação ao modelo de filme

Abra o arquivo *Models/Movie.cs* e adicione uma propriedade `Rating`:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]
::: moniker-end

Como você adicionou um novo campo à classe `Movie`, você também precisa atualizar a lista de permissões de associação para que essa nova propriedade seja incluída. Em *MoviesController.cs*, atualize o atributo `[Bind]` dos métodos de ação `Create` e `Edit` para incluir a propriedade `Rating`:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Você também precisa atualizar os modelos de exibição para exibir, criar e editar a nova propriedade `Rating` na exibição do navegador.

Edite o arquivo */Views/Movies/Index.cshtml* e adicione um campo `Rating`:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Atualize */Views/Movies/Create.cshtml* com um campo `Rating`.

O aplicativo não funcionará até que atualizemos o BD para incluir o novo campo. Se você executá-lo agora, obterá o seguinte `SqliteException`:

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

Você está vendo este erro porque a classe de modelo Movie atualizada é diferente do esquema da tabela Movie do banco de dados existente. (Não há nenhuma coluna `Rating` na tabela de banco de dados.)

Existem algumas abordagens para resolver o erro:

1. Remova o banco de dados e faça com que o Entity Framework recrie automaticamente o banco de dados com base no novo esquema de classe de modelo. Com essa abordagem, você perde os dados existentes no banco de dados – portanto, não é possível fazer isso com um banco de dados de produção! Muitas vezes, o uso de um inicializador para propagar um banco de dados com os dados de teste automaticamente é uma maneira produtiva de desenvolver um aplicativo.

2. Modifique manualmente o esquema do banco de dados existente para que ele corresponda às classes de modelo. A vantagem dessa abordagem é que você mantém os dados. Faça essa alteração manualmente ou criando um script de alteração de banco de dados.

3. Use as Migrações do Code First para atualizar o esquema de banco de dados.

Para este tutorial, removeremos e recriaremos o banco de dados quando o esquema for alterado. Execute o seguinte comando em um terminal para remover o BD:

`dotnet ef database drop`

Atualize a classe `SeedData` para que ela forneça um valor para a nova coluna. Uma alteração de amostra é mostrada abaixo, mas é recomendável fazer essa alteração em cada `new Movie`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Adicione o campo `Rating` às exibições `Edit`, `Details` e `Delete`.

Execute o aplicativo e verifique se você pode criar/editar/exibir filmes com um campo `Rating`. modelos.
