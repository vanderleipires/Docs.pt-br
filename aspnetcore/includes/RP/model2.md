Adicione as seguintes propriedades à classe `Movie`:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

O campo `ID` é necessário para o banco de dados para a chave primária.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Adicionar uma classe de contexto de banco de dados

Adicione a seguinte classe *MovieContext.cs* à pasta *Modelos*:  

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

O código anterior cria uma propriedade `DbSet` para o conjunto de entidades. Na terminologia do Entity Framework, um conjunto de entidades normalmente corresponde a uma tabela de banco de dados, enquanto uma entidade corresponde a uma linha na tabela.
