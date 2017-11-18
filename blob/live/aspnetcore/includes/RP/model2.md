<span data-ttu-id="6e372-101">Adicione as seguintes propriedades à classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="6e372-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="6e372-102">O campo `ID` é necessário para o banco de dados para a chave primária.</span><span class="sxs-lookup"><span data-stu-id="6e372-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="6e372-103">Adicionar uma classe de contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="6e372-103">Add a database context class</span></span>

<span data-ttu-id="6e372-104">Adicione uma classe derivada `DbContext` chamada *MovieContext.cs* à pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="6e372-104">Add a `DbContext` derived class named *MovieContext.cs* to the *Models* folder.</span></span>
[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

<span data-ttu-id="6e372-105">O código anterior cria uma propriedade `DbSet` para o conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="6e372-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="6e372-106">Na terminologia do Entity Framework, um conjunto de entidades normalmente corresponde a uma tabela de banco de dados, enquanto uma entidade corresponde a uma linha na tabela.</span><span class="sxs-lookup"><span data-stu-id="6e372-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
