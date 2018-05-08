# <a name="working-with-sqlite-in-and-razor-pages"></a><span data-ttu-id="aaf9f-101">Trabalhando com SQLite e páginas Razor</span><span class="sxs-lookup"><span data-stu-id="aaf9f-101">Working with SQLite in and Razor Pages</span></span>

<span data-ttu-id="aaf9f-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aaf9f-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aaf9f-103">O objeto `MovieContext` cuida da tarefa de se conectar ao banco de dados e mapear objetos `Movie` para registros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="aaf9f-103">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="aaf9f-104">O contexto de banco de dados é registrado com o contêiner [Injeção de Dependência](xref:fundamentals/dependency-injection) no método `ConfigureServices` no arquivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aaf9f-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](code/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a><span data-ttu-id="aaf9f-105">SQLite</span><span class="sxs-lookup"><span data-stu-id="aaf9f-105">SQLite</span></span>

<span data-ttu-id="aaf9f-106">O site do [SQLite](https://www.sqlite.org/) afirma:</span><span class="sxs-lookup"><span data-stu-id="aaf9f-106">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="aaf9f-107">O SQLite é um mecanismo de banco de dados SQL independente, de alta confiabilidade, inserido, completo e de domínio público.</span><span class="sxs-lookup"><span data-stu-id="aaf9f-107">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="aaf9f-108">O SQLite é o mecanismo de banco de dados mais usado no mundo.</span><span class="sxs-lookup"><span data-stu-id="aaf9f-108">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="aaf9f-109">Há muitas ferramentas de terceiros que podem ser baixadas para gerenciar e exibir um banco de dados SQLite.</span><span class="sxs-lookup"><span data-stu-id="aaf9f-109">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="aaf9f-110">A imagem abaixo é do [Navegador do DB para SQLite](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="aaf9f-110">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="aaf9f-111">Se você tiver uma ferramenta favorita do SQLite, deixe um comentário sobre o que mais gosta dela.</span><span class="sxs-lookup"><span data-stu-id="aaf9f-111">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![Navegador do BD do SQLite mostrando o BD de filmes](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="aaf9f-113">Propagar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="aaf9f-113">Seed the database</span></span>

<span data-ttu-id="aaf9f-114">Crie uma nova classe chamada `SeedData` na pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="aaf9f-114">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="aaf9f-115">Substitua o código gerado pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="aaf9f-115">Replace the generated code with the following:</span></span>

[!code-csharp[](code/Models/SeedData.cs)]

<span data-ttu-id="aaf9f-116">Se houver um filme no BD, o inicializador de semeadura será retornado.</span><span class="sxs-lookup"><span data-stu-id="aaf9f-116">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="aaf9f-117">Adicionar o inicializador de semeadura</span><span class="sxs-lookup"><span data-stu-id="aaf9f-117">Add the seed initializer</span></span>

<span data-ttu-id="aaf9f-118">Adicione o inicializador de semeadura ao método `Main` no arquivo *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="aaf9f-118">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Program.cs)]

### <a name="test-the-app"></a><span data-ttu-id="aaf9f-119">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="aaf9f-119">Test the app</span></span>

<span data-ttu-id="aaf9f-120">Exclua todos os registros no BD (para que o método de semeadura seja executado).</span><span class="sxs-lookup"><span data-stu-id="aaf9f-120">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="aaf9f-121">Interrompa e inicie o aplicativo para propagar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="aaf9f-121">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="aaf9f-122">O aplicativo mostra os dados propagados.</span><span class="sxs-lookup"><span data-stu-id="aaf9f-122">The app shows the seeded data.</span></span>