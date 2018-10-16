---
title: Adicionar um modelo a um aplicativo Páginas Razor no ASP.NET Core
author: rick-anderson
description: Saiba como adicionar classes de gerenciamento de filmes em um banco de dados usando o EF Core (Entity Framework Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 5cd1e08ac52d352be23a280419d7456f685a03ad
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045595"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="9db95-103">Adicionar um modelo a um aplicativo Páginas Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9db95-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="9db95-104">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="9db95-104">Add a data model</span></span>

<span data-ttu-id="9db95-105">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **RazorPagesMovie** > **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="9db95-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="9db95-106">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="9db95-106">Name the folder *Models*.</span></span>

<span data-ttu-id="9db95-107">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="9db95-107">Right click the *Models* folder.</span></span> <span data-ttu-id="9db95-108">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="9db95-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="9db95-109">Nomeie a classe **Movie** e substitua o conteúdo da classe `Movie` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="9db95-109">Name the class **Movie** and replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="9db95-110">Fazer scaffold do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="9db95-110">Scaffold the movie model</span></span>

<span data-ttu-id="9db95-111">Nesta seção, é feito o scaffold do modelo de filme.</span><span class="sxs-lookup"><span data-stu-id="9db95-111">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="9db95-112">Ou seja, a ferramenta de scaffolding gera páginas para operações de CRUD (Criar, Ler, Atualizar e Excluir) para o modelo do filme.</span><span class="sxs-lookup"><span data-stu-id="9db95-112">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="9db95-113">Crie uma pasta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="9db95-113">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="9db95-114">No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta *Pages* > **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="9db95-114">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="9db95-115">Dê à pasta o nome *Movies*</span><span class="sxs-lookup"><span data-stu-id="9db95-115">Name the folder *Movies*</span></span>

<span data-ttu-id="9db95-116">No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta *Pages/Movies* pasta > **Adicionar** > **Novo item com scaffold**.</span><span class="sxs-lookup"><span data-stu-id="9db95-116">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagem das instruções anteriores.](model/_static/sca.png)

<span data-ttu-id="9db95-118">Na caixa de diálogo **Adicionar Scaffold**, selecione **Razor Pages usando o Entity Framework (CRUD)** > **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="9db95-118">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagem das instruções anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="9db95-120">Conclua a caixa de diálogo **Adicionar Razor Pages usando o Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="9db95-120">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="9db95-121">Na lista suspensa **Classe de modelo**, selecione **Filme (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="9db95-121">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="9db95-122">Na linha **Classe de contexto de dados**, selecione o sinal **+** (+) e aceite o nome gerado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="9db95-122">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="9db95-123">Na lista suspensa **Classe de contexto de dados**, selecione **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="9db95-123">In the **Data context class** drop down, select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="9db95-124">Selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="9db95-124">Select **Add**.</span></span>

![Imagem das instruções anteriores.](model/_static/arp.png)

<span data-ttu-id="9db95-126">O processo de scaffold criou e alterou os seguintes arquivos:</span><span class="sxs-lookup"><span data-stu-id="9db95-126">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="9db95-127">Arquivos criados</span><span class="sxs-lookup"><span data-stu-id="9db95-127">Files created</span></span>

* <span data-ttu-id="9db95-128">*Pages/Movies*: Criar, Excluir, Detalhes, Editar, Índice.</span><span class="sxs-lookup"><span data-stu-id="9db95-128">*Pages/Movies*: Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="9db95-129">Essas páginas serão detalhadas no próximo tutorial.</span><span class="sxs-lookup"><span data-stu-id="9db95-129">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="9db95-130">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="9db95-130">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="9db95-131">Atualizações de arquivo</span><span class="sxs-lookup"><span data-stu-id="9db95-131">File updates</span></span>

* <span data-ttu-id="9db95-132">*Startup.cs*: alterações nesse arquivo serão detalhadas na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="9db95-132">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="9db95-133">*appsettings.json*: a cadeia de conexão usada para se conectar a um banco de dados local é adicionada.</span><span class="sxs-lookup"><span data-stu-id="9db95-133">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="9db95-134">Examinar o contexto registrado com a injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="9db95-134">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="9db95-135">O ASP.NET Core é construído com a [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9db95-135">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="9db95-136">Serviços (como o contexto de BD do EF Core) são registrados com injeção de dependência durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9db95-136">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="9db95-137">Os componentes que exigem esses serviços (como as Páginas do Razor) recebem esses serviços por meio de parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="9db95-137">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="9db95-138">O código de construtor que obtém uma instância de contexto do BD será mostrado mais adiante no tutorial.</span><span class="sxs-lookup"><span data-stu-id="9db95-138">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="9db95-139">A ferramenta de scaffolding criou automaticamente um contexto de BD e o registrou no contêiner da injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="9db95-139">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="9db95-140">Examine o método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9db95-140">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="9db95-141">A linha destacada foi adicionada pelo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="9db95-141">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="9db95-142">A classe principal que coordena a funcionalidade do EF Core de um modelo de dados é a classe de contexto de BD.</span><span class="sxs-lookup"><span data-stu-id="9db95-142">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="9db95-143">O contexto de dados deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="9db95-143">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="9db95-144">O contexto de dados especifica quais entidades são incluídas no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="9db95-144">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="9db95-145">Neste projeto, a classe é chamada `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="9db95-145">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="9db95-146">O código anterior cria uma propriedade [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para o conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="9db95-146">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="9db95-147">Na terminologia do Entity Framework, um conjunto de entidades normalmente corresponde a uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9db95-147">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="9db95-148">Uma entidade corresponde a uma linha da tabela.</span><span class="sxs-lookup"><span data-stu-id="9db95-148">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="9db95-149">O nome da cadeia de conexão é passado para o contexto com a chamada de um método em um objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="9db95-149">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="9db95-150">Para o desenvolvimento local, o [sistema de configuração do ASP.NET Core](xref:fundamentals/configuration/index) lê a cadeia de conexão do arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9db95-150">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="9db95-151">Executar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="9db95-151">Perform initial migration</span></span>

<span data-ttu-id="9db95-152">Nesta seção, você deve usar o PMC (Console de Gerenciador de Pacotes) para:</span><span class="sxs-lookup"><span data-stu-id="9db95-152">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="9db95-153">Adicione uma migração inicial.</span><span class="sxs-lookup"><span data-stu-id="9db95-153">Add an initial migration.</span></span>
* <span data-ttu-id="9db95-154">Atualize o banco de dados com a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="9db95-154">Update the database with the initial migration.</span></span>

<span data-ttu-id="9db95-155">No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="9db95-155">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu do PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="9db95-157">No PMC, insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="9db95-157">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="9db95-158">Como alternativa, é possível usar os seguintes comandos do .NET Core CLI na pasta do projeto:</span><span class="sxs-lookup"><span data-stu-id="9db95-158">Alternatively, the following .NET Core CLI commands can be used from the project folder:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="9db95-159">Ignore a mensagem de aviso a seguir, pois você corrigirá isso em um tutorial posterior:</span><span class="sxs-lookup"><span data-stu-id="9db95-159">Ignore the following warning message, you fix that in a a later tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="9db95-160">O comando `Add-Migration` gera código para criar o esquema de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="9db95-160">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="9db95-161">O esquema é baseado no modelo especificado no `RazorPagesMovieContext` (no arquivo *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="9db95-161">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="9db95-162">O argumento `Initial` é usado para nomear as migrações.</span><span class="sxs-lookup"><span data-stu-id="9db95-162">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="9db95-163">Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração.</span><span class="sxs-lookup"><span data-stu-id="9db95-163">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="9db95-164">Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="9db95-164">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="9db95-165">O comando `Update-Database` executa o método `Up` no arquivo *Migrations/{time-stamp}_InitialCreate.cs*, que cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9db95-165">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="9db95-166">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="9db95-166">If you get the error:</span></span>

`SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.`

<span data-ttu-id="9db95-167">Você perdeu a [etapa de migrações](#pmc).</span><span class="sxs-lookup"><span data-stu-id="9db95-167">You missed the [migrations step](#pmc).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="9db95-168">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="9db95-168">Add a data model</span></span>

<span data-ttu-id="9db95-169">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **RazorPagesMovie** > **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="9db95-169">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="9db95-170">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="9db95-170">Name the folder *Models*.</span></span>

<span data-ttu-id="9db95-171">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="9db95-171">Right click the *Models* folder.</span></span> <span data-ttu-id="9db95-172">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="9db95-172">Select **Add** > **Class**.</span></span> <span data-ttu-id="9db95-173">Nomeie a classe **Movie** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="9db95-173">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="9db95-174">Adicionar uma cadeia de conexão de banco de dados</span><span class="sxs-lookup"><span data-stu-id="9db95-174">Add a database connection string</span></span>

<span data-ttu-id="9db95-175">Adicione uma cadeia de conexão ao arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9db95-175">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="9db95-176">Registrar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="9db95-176">Register the database context</span></span>

<span data-ttu-id="9db95-177">Registre o contexto do banco de dados com o contêiner de [injeção de dependência](xref:fundamentals/dependency-injection) no [método ConfigureServices da classe Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9db95-177">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="9db95-178">Compile o projeto para verificar se não há erros.</span><span class="sxs-lookup"><span data-stu-id="9db95-178">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="9db95-179">Adicionar ferramentas de scaffold e executar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="9db95-179">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="9db95-180">Nesta seção, você deve usar o PMC (Console de Gerenciador de Pacotes) para:</span><span class="sxs-lookup"><span data-stu-id="9db95-180">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="9db95-181">Adicione o pacote de geração de código da Web do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9db95-181">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="9db95-182">Esse pacote é necessário para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="9db95-182">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="9db95-183">Adicionar uma migração inicial.</span><span class="sxs-lookup"><span data-stu-id="9db95-183">Add an initial migration.</span></span>
* <span data-ttu-id="9db95-184">Atualize o banco de dados com a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="9db95-184">Update the database with the initial migration.</span></span>

<span data-ttu-id="9db95-185">No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="9db95-185">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu do PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="9db95-187">No PMC, insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="9db95-187">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="9db95-188">Como alternativa, os seguintes comandos da CLI do .NET Core podem ser usados:</span><span class="sxs-lookup"><span data-stu-id="9db95-188">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="9db95-189">Ignore a mensagem a seguir:</span><span class="sxs-lookup"><span data-stu-id="9db95-189">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="9db95-190">você corrigirá isso no próximo tutorial.</span><span class="sxs-lookup"><span data-stu-id="9db95-190">You fix that in the next tutorial.</span></span>

<span data-ttu-id="9db95-191">O comando `Install-Package` instala as ferramentas necessárias para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="9db95-191">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="9db95-192">O comando `Add-Migration` gera código para criar o esquema de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="9db95-192">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="9db95-193">O esquema é baseado no modelo especificado no `DbContext` (no arquivo *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="9db95-193">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="9db95-194">O argumento `Initial` é usado para nomear as migrações.</span><span class="sxs-lookup"><span data-stu-id="9db95-194">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="9db95-195">Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração.</span><span class="sxs-lookup"><span data-stu-id="9db95-195">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="9db95-196">Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="9db95-196">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="9db95-197">O comando `Update-Database` executa o método `Up` no arquivo *Migrations/{time-stamp}_InitialCreate.cs*, que cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9db95-197">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="9db95-198">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="9db95-198">Test the app</span></span>

* <span data-ttu-id="9db95-199">Executar o aplicativo e acrescentar `/Movies` à URL no navegador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="9db95-199">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="9db95-200">Teste o link **Criar**.</span><span class="sxs-lookup"><span data-stu-id="9db95-200">Test the **Create** link.</span></span>

  ![Criar página](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="9db95-202">Teste os links **Editar**, **Detalhes** e **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="9db95-202">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="9db95-203">Se você receber uma exceção SQL, verifique se executou migrações e atualizou o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9db95-203">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="9db95-204">O tutorial a seguir explica os arquivos criados por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="9db95-204">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9db95-205">[Anterior: Introdução](xref:tutorials/razor-pages/razor-pages-start)
> [Próximo: Páginas Razor geradas por scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="9db95-205">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
