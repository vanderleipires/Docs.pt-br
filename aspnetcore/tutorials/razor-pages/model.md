---
title: Adicionar um modelo a um aplicativo Páginas Razor no ASP.NET Core
author: rick-anderson
description: Saiba como adicionar classes de gerenciamento de filmes em um banco de dados usando o EF Core (Entity Framework Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 41a88e06afbe6e7accd03ff7b39aa69e15e0c0b4
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325807"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="e3fe6-103">Adicionar um modelo a um aplicativo Páginas Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3fe6-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="e3fe6-104">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="e3fe6-104">Add a data model</span></span>

<span data-ttu-id="e3fe6-105">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **RazorPagesMovie** > **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="e3fe6-106">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-106">Name the folder *Models*.</span></span>

<span data-ttu-id="e3fe6-107">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-107">Right click the *Models* folder.</span></span> <span data-ttu-id="e3fe6-108">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="e3fe6-109">Nomeie a classe **Movie** e substitua o conteúdo da classe `Movie` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-109">Name the class **Movie** and replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="e3fe6-110">Fazer scaffold do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="e3fe6-110">Scaffold the movie model</span></span>

<span data-ttu-id="e3fe6-111">Nesta seção, é feito o scaffold do modelo de filme.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-111">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="e3fe6-112">Ou seja, a ferramenta de scaffolding gera páginas para operações de CRUD (Criar, Ler, Atualizar e Excluir) para o modelo do filme.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-112">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="e3fe6-113">Crie uma pasta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-113">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="e3fe6-114">No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta *Páginas* > **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-114">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="e3fe6-115">Dê à pasta o nome *Movies*</span><span class="sxs-lookup"><span data-stu-id="e3fe6-115">Name the folder *Movies*</span></span>

<span data-ttu-id="e3fe6-116">No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta *Páginas/Filmes* pasta > **Adicionar** > **Novo item com scaffold**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-116">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagem das instruções anteriores.](model/_static/sca.png)

<span data-ttu-id="e3fe6-118">Na caixa de diálogo **Adicionar Scaffold**, selecione **Razor Pages usando o Entity Framework (CRUD)** > **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-118">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Imagem das instruções anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="e3fe6-120">Conclua a caixa de diálogo **Adicionar Razor Pages usando o Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-120">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="e3fe6-121">Na lista suspensa **Classe de modelo**, selecione **Filme (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-121">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="e3fe6-122">Na linha **Classe de contexto de dados**, selecione o sinal **+** (+) e aceite o nome gerado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-122">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="e3fe6-123">Selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-123">Select **Add**.</span></span>

![Imagem das instruções anteriores.](model/_static/arp.png)

<span data-ttu-id="e3fe6-125">O processo de scaffold cria e atualiza os arquivos a seguir:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-125">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="e3fe6-126">Arquivos criados</span><span class="sxs-lookup"><span data-stu-id="e3fe6-126">Files created</span></span>

* <span data-ttu-id="e3fe6-127">*Pages/Movies*: Criar, Excluir, Detalhes, Editar, Índice.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-127">*Pages/Movies*: Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="e3fe6-128">Essas páginas serão detalhadas no próximo tutorial.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-128">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="e3fe6-129">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="e3fe6-129">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="e3fe6-130">Arquivo atualizado</span><span class="sxs-lookup"><span data-stu-id="e3fe6-130">File updated</span></span>

* <span data-ttu-id="e3fe6-131">*Startup.cs*: alterações nesse arquivo serão detalhadas na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-131">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="e3fe6-132">*appsettings.json*: a cadeia de conexão usada para se conectar a um banco de dados local é adicionada.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-132">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="e3fe6-133">Examinar o contexto registrado com a injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="e3fe6-133">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="e3fe6-134">O ASP.NET Core é construído com a [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e3fe6-134">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e3fe6-135">Serviços (como o contexto de BD do EF Core) são registrados com injeção de dependência durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-135">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="e3fe6-136">Os componentes que exigem esses serviços (como as Páginas do Razor) recebem esses serviços por meio de parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-136">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="e3fe6-137">O código de construtor que obtém uma instância de contexto do BD será mostrado mais adiante no tutorial.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-137">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="e3fe6-138">A ferramenta de scaffolding criou automaticamente um contexto de BD e o registrou no contêiner da injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-138">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="e3fe6-139">Examine o método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-139">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="e3fe6-140">A linha destacada foi adicionada pelo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-140">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="e3fe6-141">A classe principal que coordena a funcionalidade do EF Core de um modelo de dados é a classe de contexto de BD.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-141">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="e3fe6-142">O contexto de dados deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="e3fe6-142">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="e3fe6-143">O contexto de dados especifica quais entidades são incluídas no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-143">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="e3fe6-144">Neste projeto, a classe é chamada `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-144">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="e3fe6-145">O código anterior cria uma propriedade [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para o conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-145">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="e3fe6-146">Na terminologia do Entity Framework, um conjunto de entidades normalmente corresponde a uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-146">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="e3fe6-147">Uma entidade corresponde a uma linha da tabela.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-147">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="e3fe6-148">O nome da cadeia de conexão é passado para o contexto com a chamada de um método em um objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="e3fe6-148">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="e3fe6-149">Para o desenvolvimento local, o [sistema de configuração do ASP.NET Core](xref:fundamentals/configuration/index) lê a cadeia de conexão do arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-149">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="e3fe6-150">Executar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="e3fe6-150">Perform initial migration</span></span>

<span data-ttu-id="e3fe6-151">Nesta seção, você deve usar o PMC (Console de Gerenciador de Pacotes) para:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-151">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="e3fe6-152">Adicione uma migração inicial.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-152">Add an initial migration.</span></span>
* <span data-ttu-id="e3fe6-153">Atualize o banco de dados com a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-153">Update the database with the initial migration.</span></span>

<span data-ttu-id="e3fe6-154">No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-154">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu do PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="e3fe6-156">No PMC, insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-156">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="e3fe6-157">Como alternativa, é possível usar os seguintes comandos do .NET Core CLI na pasta do projeto:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-157">Alternatively, the following .NET Core CLI commands can be used from the project folder:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="e3fe6-158">Ignore a mensagem de aviso a seguir, pois você corrigirá isso em um tutorial posterior:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-158">Ignore the following warning message, you fix that in a a later tutorial:</span></span>

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.
```

<span data-ttu-id="e3fe6-159">O comando `Add-Migration` gera código para criar o esquema de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-159">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e3fe6-160">O esquema é baseado no modelo especificado no `RazorPagesMovieContext` (no arquivo *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="e3fe6-160">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="e3fe6-161">O argumento `Initial` é usado para nomear as migrações.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-161">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="e3fe6-162">Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-162">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="e3fe6-163">Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-163">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="e3fe6-164">O comando `Update-Database` executa o método `Up` no arquivo *Migrations/{time-stamp}_InitialCreate.cs*, que cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-164">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="e3fe6-165">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-165">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="e3fe6-166">Você perdeu a [etapa de migrações](#pmc).</span><span class="sxs-lookup"><span data-stu-id="e3fe6-166">You missed the [migrations step](#pmc).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="e3fe6-167">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="e3fe6-167">Add a data model</span></span>

<span data-ttu-id="e3fe6-168">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **RazorPagesMovie** > **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-168">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="e3fe6-169">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-169">Name the folder *Models*.</span></span>

<span data-ttu-id="e3fe6-170">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-170">Right click the *Models* folder.</span></span> <span data-ttu-id="e3fe6-171">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-171">Select **Add** > **Class**.</span></span> <span data-ttu-id="e3fe6-172">Nomeie a classe **Movie** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-172">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="e3fe6-173">Adicionar uma cadeia de conexão de banco de dados</span><span class="sxs-lookup"><span data-stu-id="e3fe6-173">Add a database connection string</span></span>

<span data-ttu-id="e3fe6-174">Adicione uma cadeia de conexão ao arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-174">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="e3fe6-175">Registrar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="e3fe6-175">Register the database context</span></span>

<span data-ttu-id="e3fe6-176">Registre o contexto do banco de dados com o contêiner de [injeção de dependência](xref:fundamentals/dependency-injection) no [método ConfigureServices da classe Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="e3fe6-176">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="e3fe6-177">Compile o projeto para verificar se não há erros.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-177">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="e3fe6-178">Adicionar ferramentas de scaffold e executar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="e3fe6-178">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="e3fe6-179">Nesta seção, você deve usar o PMC (Console de Gerenciador de Pacotes) para:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-179">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="e3fe6-180">Adicione o pacote de geração de código da Web do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-180">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="e3fe6-181">Esse pacote é necessário para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-181">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="e3fe6-182">Adicionar uma migração inicial.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-182">Add an initial migration.</span></span>
* <span data-ttu-id="e3fe6-183">Atualize o banco de dados com a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-183">Update the database with the initial migration.</span></span>

<span data-ttu-id="e3fe6-184">No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-184">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu do PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="e3fe6-186">No PMC, insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-186">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="e3fe6-187">Como alternativa, os seguintes comandos da CLI do .NET Core podem ser usados:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-187">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="e3fe6-188">Ignore a mensagem a seguir:</span><span class="sxs-lookup"><span data-stu-id="e3fe6-188">Ignore the following message:</span></span>

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'
```

<span data-ttu-id="e3fe6-189">você corrigirá isso no próximo tutorial.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-189">You fix that in the next tutorial.</span></span>

<span data-ttu-id="e3fe6-190">O comando `Install-Package` instala as ferramentas necessárias para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-190">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="e3fe6-191">O comando `Add-Migration` gera código para criar o esquema de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-191">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e3fe6-192">O esquema é baseado no modelo especificado no `DbContext` (no arquivo *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="e3fe6-192">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="e3fe6-193">O argumento `Initial` é usado para nomear as migrações.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-193">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="e3fe6-194">Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-194">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="e3fe6-195">Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-195">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="e3fe6-196">O comando `Update-Database` executa o método `Up` no arquivo *Migrations/{time-stamp}_InitialCreate.cs*, que cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-196">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="e3fe6-197">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="e3fe6-197">Test the app</span></span>

* <span data-ttu-id="e3fe6-198">Executar o aplicativo e acrescentar `/Movies` à URL no navegador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="e3fe6-198">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="e3fe6-199">Teste o link **Criar**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-199">Test the **Create** link.</span></span>

  ![Criar página](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="e3fe6-201">Teste os links **Editar**, **Detalhes** e **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-201">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="e3fe6-202">Se você receber uma exceção SQL, verifique se executou migrações e atualizou o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-202">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="e3fe6-203">O tutorial a seguir explica os arquivos criados por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="e3fe6-203">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e3fe6-204">[Anterior: Introdução](xref:tutorials/razor-pages/razor-pages-start)
> [Próximo: Páginas Razor geradas por scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="e3fe6-204">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
