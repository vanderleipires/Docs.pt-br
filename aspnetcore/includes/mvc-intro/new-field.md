# <a name="adding-a-new-field"></a><span data-ttu-id="c0371-101">Adicionando um novo campo</span><span class="sxs-lookup"><span data-stu-id="c0371-101">Adding a new field</span></span>

<span data-ttu-id="c0371-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c0371-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c0371-103">Este tutorial adicionará um novo campo à tabela `Movies`.</span><span class="sxs-lookup"><span data-stu-id="c0371-103">This tutorial will add a new field to the `Movies` table.</span></span> <span data-ttu-id="c0371-104">Removeremos o banco de dados e criaremos um novo ao alterar o esquema (adicionar um novo campo).</span><span class="sxs-lookup"><span data-stu-id="c0371-104">We'll drop the database and create a new one when we change the schema (add a new field).</span></span> <span data-ttu-id="c0371-105">Este fluxo de trabalho funciona bem no início do desenvolvimento quando não temos nenhum dado de produção para preservar.</span><span class="sxs-lookup"><span data-stu-id="c0371-105">This workflow works well early in development when we don't have any production data to perserve.</span></span>

<span data-ttu-id="c0371-106">Depois que o aplicativo for implantado e você tiver dados que precisa preservar, não poderá remover o BD quando precisar alterar o esquema.</span><span class="sxs-lookup"><span data-stu-id="c0371-106">Once your app is deployed and you have data that you need to perserve, you can't drop your DB when you need to change the schema.</span></span> <span data-ttu-id="c0371-107">As [Migrações do Entity Framework Code First](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) permitem atualizar o esquema e migrar o banco de dados sem perder dados.</span><span class="sxs-lookup"><span data-stu-id="c0371-107">Entity Framework [Code First Migrations](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) allows you to update your schema and migrate the database without losing data.</span></span> <span data-ttu-id="c0371-108">As Migrações são um recurso popular ao usar o SQL Server, mas o SQLite não dá suporte a muitas operações de esquema de migração e, portanto, apenas migrações muito simples são possíveis.</span><span class="sxs-lookup"><span data-stu-id="c0371-108">Migrations is a popular feature when using SQL Server, but SQLlite doesn't support many migration schema operations, so only very simply migrations are possible.</span></span> <span data-ttu-id="c0371-109">Consulte [Limitações do SQLite](https://docs.microsoft.com/ef/core/providers/sqlite/limitations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="c0371-109">See [SQLite Limitations](https://docs.microsoft.com/ef/core/providers/sqlite/limitations) for more information.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="c0371-110">Adicionando uma propriedade de classificação ao modelo de filme</span><span class="sxs-lookup"><span data-stu-id="c0371-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="c0371-111">Abra o arquivo *Models/Movie.cs* e adicione uma propriedade `Rating`:</span><span class="sxs-lookup"><span data-stu-id="c0371-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="c0371-112">Como você adicionou um novo campo à classe `Movie`, você também precisa atualizar a lista de permissões de associação para que essa nova propriedade seja incluída.</span><span class="sxs-lookup"><span data-stu-id="c0371-112">Because you've added a new field to the `Movie` class, you also need to update the binding whitelist so this new property will be included.</span></span> <span data-ttu-id="c0371-113">Em *MoviesController.cs*, atualize o atributo `[Bind]` dos métodos de ação `Create` e `Edit` para incluir a propriedade `Rating`:</span><span class="sxs-lookup"><span data-stu-id="c0371-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="c0371-114">Você também precisa atualizar os modelos de exibição para exibir, criar e editar a nova propriedade `Rating` na exibição do navegador.</span><span class="sxs-lookup"><span data-stu-id="c0371-114">You also need to update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="c0371-115">Edite o arquivo */Views/Movies/Index.cshtml* e adicione um campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="c0371-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="c0371-116">Atualize */Views/Movies/Create.cshtml* com um campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c0371-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

<span data-ttu-id="c0371-117">O aplicativo não funcionará até que atualizemos o BD para incluir o novo campo.</span><span class="sxs-lookup"><span data-stu-id="c0371-117">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="c0371-118">Se você executá-lo agora, obterá o seguinte `SqliteException`:</span><span class="sxs-lookup"><span data-stu-id="c0371-118">If you run it now, you'll get the following `SqliteException`:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

<span data-ttu-id="c0371-119">Você está vendo este erro porque a classe de modelo Movie atualizada é diferente do esquema da tabela Movie do banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="c0371-119">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="c0371-120">(Não há nenhuma coluna `Rating` na tabela de banco de dados.)</span><span class="sxs-lookup"><span data-stu-id="c0371-120">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="c0371-121">Existem algumas abordagens para resolver o erro:</span><span class="sxs-lookup"><span data-stu-id="c0371-121">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="c0371-122">Remova o banco de dados e faça com que o Entity Framework recrie automaticamente o banco de dados com base no novo esquema de classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="c0371-122">Drop the database and have the Entity Framework automatically re-create the database based on the new model class schema.</span></span> <span data-ttu-id="c0371-123">Com essa abordagem, você perde os dados existentes no banco de dados – portanto, não é possível fazer isso com um banco de dados de produção!</span><span class="sxs-lookup"><span data-stu-id="c0371-123">With this approach, you lose existing data in the database — so you can't do this with a production database!</span></span> <span data-ttu-id="c0371-124">Muitas vezes, o uso de um inicializador para propagar um banco de dados com os dados de teste automaticamente é uma maneira produtiva de desenvolver um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c0371-124">Using an initializer to automatically seed a database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="c0371-125">Modifique manualmente o esquema do banco de dados existente para que ele corresponda às classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="c0371-125">Manually modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="c0371-126">A vantagem dessa abordagem é que você mantém os dados.</span><span class="sxs-lookup"><span data-stu-id="c0371-126">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="c0371-127">Faça essa alteração manualmente ou criando um script de alteração de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c0371-127">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="c0371-128">Use as Migrações do Code First para atualizar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c0371-128">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="c0371-129">Para este tutorial, removeremos e recriaremos o banco de dados quando o esquema for alterado.</span><span class="sxs-lookup"><span data-stu-id="c0371-129">For this tutorial, we'll drop and re-create the database when the schema changes.</span></span> <span data-ttu-id="c0371-130">Execute o seguinte comando em um terminal para remover o BD:</span><span class="sxs-lookup"><span data-stu-id="c0371-130">Run the following command from a terminal to drop the db:</span></span>

`dotnet ef database drop`

<span data-ttu-id="c0371-131">Atualize a classe `SeedData` para que ela forneça um valor para a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="c0371-131">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="c0371-132">Uma alteração de amostra é mostrada abaixo, mas é recomendável fazer essa alteração em cada `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="c0371-132">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="c0371-133">Adicione o campo `Rating` às exibições `Edit`, `Details` e `Delete`.</span><span class="sxs-lookup"><span data-stu-id="c0371-133">Add the `Rating` field to the `Edit`, `Details`, and `Delete` view.</span></span>

<span data-ttu-id="c0371-134">Execute o aplicativo e verifique se você pode criar/editar/exibir filmes com um campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c0371-134">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="c0371-135">modelos.</span><span class="sxs-lookup"><span data-stu-id="c0371-135">templates.</span></span>
