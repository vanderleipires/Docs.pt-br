Adicione as seguintes propriedades à classe `Movie`:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

O campo `ID` é necessário para o banco de dados para a chave primária.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Adicionar uma classe de contexto de banco de dados

Adicione uma classe derivada `DbContext` chamada *MovieContext.cs* à pasta *Modelos*.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?range=1-12,14-17,19-21)]

O código anterior cria uma propriedade `DbSet` para o conjunto de entidades. Na terminologia do Entity Framework, um conjunto de entidades normalmente corresponde a uma tabela de banco de dados, enquanto uma entidade corresponde a uma linha na tabela. O nome da propriedade `DbSet` é `Movies`. Como o banco de dados usa nomes singulares, o exemplo substitui `OnModelCreating` para usar a forma singular (`Movie`) para o nome da tabela.
