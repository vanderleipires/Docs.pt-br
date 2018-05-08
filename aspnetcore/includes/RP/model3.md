<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="2ed69-101">Adicionar ferramentas de scaffold e executar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="2ed69-101">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="2ed69-102">Na linha de comando, execute os seguintes comandos de CLI do .NET Core:</span><span class="sxs-lookup"><span data-stu-id="2ed69-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="2ed69-103">O comando `add package` instala as ferramentas necessárias para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="2ed69-103">The `add package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="2ed69-104">O comando `ef migrations add InitialCreate` gera código para criar o esquema de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="2ed69-104">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="2ed69-105">O esquema é baseado no modelo especificado no `DbContext` (no arquivo *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="2ed69-105">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="2ed69-106">O argumento `InitialCreate` é usado para nomear as migrações.</span><span class="sxs-lookup"><span data-stu-id="2ed69-106">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="2ed69-107">Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração.</span><span class="sxs-lookup"><span data-stu-id="2ed69-107">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="2ed69-108">Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="2ed69-108">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="2ed69-109">O comando `ef database update` executa o método `Up` no arquivo *Migrations/\<time-stamp>_InitialCreate.cs*, que cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2ed69-109">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
