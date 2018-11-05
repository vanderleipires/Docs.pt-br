<a name="cli"></a>

## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="629bf-101">Adicionar ferramentas de scaffold e executar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="629bf-101">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="629bf-102">Adicione as seguintes linhas ao arquivo *RazorPagesMovie.csproj*, logo antes da marca de fechamento `</Project>`:</span><span class="sxs-lookup"><span data-stu-id="629bf-102">Add the following lines to the *RazorPagesMovie.csproj* file, just before the closing `</Project>` tag:</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.1.0-preview1-final"/>
</ItemGroup>
```
  
<span data-ttu-id="629bf-103">Na linha de comando, execute os seguintes comandos de CLI do .NET Core:</span><span class="sxs-lookup"><span data-stu-id="629bf-103">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="629bf-104">O elemento `DotNetCliToolReference` e o comando `add package` instalam as ferramentas necessárias para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="629bf-104">The `DotNetCliToolReference` element and the `add package` command install the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="629bf-105">O comando `ef migrations add InitialCreate` gera código para criar o esquema de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="629bf-105">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="629bf-106">O esquema é baseado no modelo especificado no `DbContext` (no arquivo *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="629bf-106">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="629bf-107">O argumento `InitialCreate` é usado para nomear as migrações.</span><span class="sxs-lookup"><span data-stu-id="629bf-107">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="629bf-108">Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração.</span><span class="sxs-lookup"><span data-stu-id="629bf-108">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="629bf-109">Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="629bf-109">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="629bf-110">O comando `ef database update` executa o método `Up` no arquivo *Migrations/\<time-stamp>_InitialCreate.cs*, que cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="629bf-110">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
