---
title: Adicionar um modelo a um aplicativo Páginas Razor no ASP.NET Core
author: rick-anderson
description: Saiba como adicionar classes de gerenciamento de filmes em um banco de dados usando o EF Core (Entity Framework Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 508cca07fa96c20e228d2c55c9fb101f7fc3cb02
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327546"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="585b4-103">Adicionar um modelo a um aplicativo Páginas Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="585b4-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="585b4-104">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="585b4-104">Add a data model</span></span>

<span data-ttu-id="585b4-105">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **RazorPagesMovie** > **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="585b4-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="585b4-106">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="585b4-106">Name the folder *Models*.</span></span>

<span data-ttu-id="585b4-107">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="585b4-107">Right click the *Models* folder.</span></span> <span data-ttu-id="585b4-108">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="585b4-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="585b4-109">Nomeie a classe **Movie** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="585b4-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="585b4-110">Substitua o conteúdo da classe `Movie` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="585b4-110">Replace the contents of the `Movie` class with the following code:</span></span>

<span data-ttu-id="585b4-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="585b4-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="585b4-112">Fazer scaffold do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="585b4-112">Scaffold the movie model</span></span>

<span data-ttu-id="585b4-113">Nesta seção, é feito o scaffold do modelo de filme.</span><span class="sxs-lookup"><span data-stu-id="585b4-113">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="585b4-114">Ou seja, a ferramenta de scaffolding gera páginas para operações de CRUD (Criar, Ler, Atualizar e Excluir) para o modelo do filme.</span><span class="sxs-lookup"><span data-stu-id="585b4-114">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="585b4-115">Crie uma pasta *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="585b4-115">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="585b4-116">No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta *Páginas* > **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="585b4-116">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="585b4-117">Dê à pasta o nome *Filmes*</span><span class="sxs-lookup"><span data-stu-id="585b4-117">Name the folder *Movies*</span></span>

<span data-ttu-id="585b4-118">No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta *Páginas/Filmes* pasta > **Adicionar** > **Novo item com scaffold**.</span><span class="sxs-lookup"><span data-stu-id="585b4-118">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Imagem das instruções anteriores.](model/_static/sca.png)

<span data-ttu-id="585b4-120">Na caixa de diálogo **Adicionar Scaffold**, selecione **Razor Pages usando o Entity Framework (CRUD)** > **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="585b4-120">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Imagem das instruções anteriores.](model/_static/add_scaffold.png)

<span data-ttu-id="585b4-122">Conclua a caixa de diálogo **Adicionar Razor Pages usando o Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="585b4-122">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="585b4-123">Na lista suspensa **Classe de modelo**, selecione **Filme (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="585b4-123">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="585b4-124">Na linha **Classe de contexto de dados**, selecione o sinal **+** (+) e aceite o nome gerado **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="585b4-124">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="585b4-125">Na lista suspensa **Classe de contexto de dados**, selecione **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="585b4-125">In the **Data context class** drop down,  select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="585b4-126">Selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="585b4-126">Select **Add**.</span></span>

![Imagem das instruções anteriores.](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="585b4-128">Executar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="585b4-128">Perform initial migration</span></span>

<span data-ttu-id="585b4-129">Nesta seção, você deve usar o PMC (Console de Gerenciador de Pacotes) para:</span><span class="sxs-lookup"><span data-stu-id="585b4-129">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="585b4-130">Adicione uma migração inicial.</span><span class="sxs-lookup"><span data-stu-id="585b4-130">Add an initial migration.</span></span>
* <span data-ttu-id="585b4-131">Atualize o banco de dados com a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="585b4-131">Update the database with the initial migration.</span></span>

<span data-ttu-id="585b4-132">No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="585b4-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu do PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="585b4-134">No PMC, insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="585b4-134">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="585b4-135">Como alternativa, os seguintes comandos da CLI do .NET Core podem ser usados:</span><span class="sxs-lookup"><span data-stu-id="585b4-135">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="585b4-136">Ignore a seguinte mensagem de aviso, você corrigirá isso no próximo tutorial:</span><span class="sxs-lookup"><span data-stu-id="585b4-136">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="585b4-137">O comando `Add-Migration` gera código para criar o esquema de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="585b4-137">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="585b4-138">O esquema é baseado no modelo especificado no `RazorPagesMovieContext` (no arquivo *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="585b4-138">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="585b4-139">O argumento `Initial` é usado para nomear as migrações.</span><span class="sxs-lookup"><span data-stu-id="585b4-139">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="585b4-140">Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração.</span><span class="sxs-lookup"><span data-stu-id="585b4-140">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="585b4-141">Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="585b4-141">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="585b4-142">O comando `Update-Database` executa o método `Up` no arquivo *Migrations/{time-stamp}_InitialCreate.cs*, que cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="585b4-142">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="585b4-143">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="585b4-143">If you get the error:</span></span>

<span data-ttu-id="585b4-144">SqlException: não é possível abrir o banco de dados "RazorPagesMovieContext-GUID" solicitado pelo logon.</span><span class="sxs-lookup"><span data-stu-id="585b4-144">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="585b4-145">O logon falhou.</span><span class="sxs-lookup"><span data-stu-id="585b4-145">The login failed.</span></span>
<span data-ttu-id="585b4-146">O logon falhou para o usuário 'User-name'.</span><span class="sxs-lookup"><span data-stu-id="585b4-146">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="585b4-147">Você perdeu a [etapa de migrações](#pmc).</span><span class="sxs-lookup"><span data-stu-id="585b4-147">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="585b4-148">Adicionar um modelo de dados</span><span class="sxs-lookup"><span data-stu-id="585b4-148">Add a data model</span></span>

<span data-ttu-id="585b4-149">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **RazorPagesMovie** > **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="585b4-149">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="585b4-150">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="585b4-150">Name the folder *Models*.</span></span>

<span data-ttu-id="585b4-151">Clique com o botão direito do mouse na pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="585b4-151">Right click the *Models* folder.</span></span> <span data-ttu-id="585b4-152">Selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="585b4-152">Select **Add** > **Class**.</span></span> <span data-ttu-id="585b4-153">Nomeie a classe **Movie** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="585b4-153">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="585b4-154">Adicionar uma cadeia de conexão de banco de dados</span><span class="sxs-lookup"><span data-stu-id="585b4-154">Add a database connection string</span></span>

<span data-ttu-id="585b4-155">Adicione uma cadeia de conexão ao arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="585b4-155">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="585b4-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="585b4-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="585b4-157">Registrar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="585b4-157">Register the database context</span></span>

<span data-ttu-id="585b4-158">Registre o contexto do banco de dados com o contêiner de [injeção de dependência](xref:fundamentals/dependency-injection) no [método ConfigureServices da classe Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="585b4-158">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

<span data-ttu-id="585b4-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span><span class="sxs-lookup"><span data-stu-id="585b4-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span></span>

<span data-ttu-id="585b4-160">Compile o projeto para verificar se não há erros.</span><span class="sxs-lookup"><span data-stu-id="585b4-160">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="585b4-161">Adicionar ferramentas de scaffold e executar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="585b4-161">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="585b4-162">Nesta seção, você deve usar o PMC (Console de Gerenciador de Pacotes) para:</span><span class="sxs-lookup"><span data-stu-id="585b4-162">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="585b4-163">Adicione o pacote de geração de código da Web do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="585b4-163">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="585b4-164">Esse pacote é necessário para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="585b4-164">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="585b4-165">Adicionar uma migração inicial.</span><span class="sxs-lookup"><span data-stu-id="585b4-165">Add an initial migration.</span></span>
* <span data-ttu-id="585b4-166">Atualize o banco de dados com a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="585b4-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="585b4-167">No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="585b4-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu do PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="585b4-169">No PMC, insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="585b4-169">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="585b4-170">Como alternativa, os seguintes comandos da CLI do .NET Core podem ser usados:</span><span class="sxs-lookup"><span data-stu-id="585b4-170">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="585b4-171">Ignore a mensagem a seguir:</span><span class="sxs-lookup"><span data-stu-id="585b4-171">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="585b4-172">você corrigirá isso no próximo tutorial.</span><span class="sxs-lookup"><span data-stu-id="585b4-172">You fix that in the next tutorial.</span></span>

<span data-ttu-id="585b4-173">O comando `Install-Package` instala as ferramentas necessárias para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="585b4-173">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="585b4-174">O comando `Add-Migration` gera código para criar o esquema de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="585b4-174">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="585b4-175">O esquema é baseado no modelo especificado no `DbContext` (no arquivo *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="585b4-175">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="585b4-176">O argumento `Initial` é usado para nomear as migrações.</span><span class="sxs-lookup"><span data-stu-id="585b4-176">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="585b4-177">Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração.</span><span class="sxs-lookup"><span data-stu-id="585b4-177">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="585b4-178">Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="585b4-178">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="585b4-179">O comando `Update-Database` executa o método `Up` no arquivo *Migrations/{time-stamp}_InitialCreate.cs*, que cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="585b4-179">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="585b4-180">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="585b4-180">Test the app</span></span>

* <span data-ttu-id="585b4-181">Executar o aplicativo e acrescentar `/Movies` à URL no navegador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="585b4-181">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="585b4-182">Teste o link **Criar**.</span><span class="sxs-lookup"><span data-stu-id="585b4-182">Test the **Create** link.</span></span>

  ![Criar página](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="585b4-184">Teste os links **Editar**, **Detalhes** e **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="585b4-184">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="585b4-185">Se você receber uma exceção SQL, verifique se executou migrações e atualizou o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="585b4-185">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="585b4-186">O tutorial a seguir explica os arquivos criados por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="585b4-186">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="585b4-187">[Anterior: Introdução](xref:tutorials/razor-pages/razor-pages-start)
> [Próximo: Páginas Razor geradas por scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="585b4-187">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
