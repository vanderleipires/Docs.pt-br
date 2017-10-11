<span data-ttu-id="2e268-101">Adicione as seguintes propriedades à classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="2e268-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="2e268-102">O campo `ID` é necessário para o banco de dados para a chave primária.</span><span class="sxs-lookup"><span data-stu-id="2e268-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="2e268-103">Adicionar uma classe de contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="2e268-103">Add a database context class</span></span>

<span data-ttu-id="2e268-104">Adicione uma classe derivada `DbContext` chamada *MovieContext.cs* à pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="2e268-104">Add a `DbContext` derived class named *MovieContext.cs* to the *Models* folder.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?range=1-12,14-17,19-21)]

<span data-ttu-id="2e268-105">O código anterior cria uma propriedade `DbSet` para o conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="2e268-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="2e268-106">Na terminologia do Entity Framework, um conjunto de entidades normalmente corresponde a uma tabela de banco de dados, enquanto uma entidade corresponde a uma linha na tabela.</span><span class="sxs-lookup"><span data-stu-id="2e268-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span> <span data-ttu-id="2e268-107">O nome da propriedade `DbSet` é `Movies`.</span><span class="sxs-lookup"><span data-stu-id="2e268-107">The `DbSet` property name is `Movies`.</span></span> <span data-ttu-id="2e268-108">Como o banco de dados usa nomes singulares, o exemplo substitui `OnModelCreating` para usar a forma singular (`Movie`) para o nome da tabela.</span><span class="sxs-lookup"><span data-stu-id="2e268-108">Since the database uses singular names, the sample overrides `OnModelCreating` to use the singular form (`Movie`) for the table name.</span></span>
