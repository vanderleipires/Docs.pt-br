---
title: Adicione um modelo a um aplicativo ASP.NET Core MVC
author: rick-anderson
description: Adicione um modelo a um aplicativo ASP.NET Core simples.
manager: wpickett
ms.author: riande
ms.date: 12/8/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 802cb458cb05579b97256022b56d6f97a03d2f1a
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34687786"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="6cef8-103">Adicione um modelo a um aplicativo ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="6cef8-103">Add a model to an ASP.NET Core MVC app</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="6cef8-104">Clique com o botão direito do mouse na pasta *Models* > **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="6cef8-104">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="6cef8-105">Nomeie a classe **Movie** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="6cef8-105">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="6cef8-106">O campo `ID` é necessário para o banco de dados para a chave primária.</span><span class="sxs-lookup"><span data-stu-id="6cef8-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="6cef8-107">Compile o projeto para verificar se não há erros.</span><span class="sxs-lookup"><span data-stu-id="6cef8-107">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="6cef8-108">Agora você tem um **M**odelo no aplicativo **M**VC.</span><span class="sxs-lookup"><span data-stu-id="6cef8-108">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="6cef8-109">Gerando um controlador por scaffolding</span><span class="sxs-lookup"><span data-stu-id="6cef8-109">Scaffolding a controller</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6cef8-110">No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta *Controladores* **> Adicionar > Novo Item com Scaffold**.</span><span class="sxs-lookup"><span data-stu-id="6cef8-110">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![exibição da etapa acima](adding-model/_static/add_controller21.png)

<span data-ttu-id="6cef8-112">Na caixa de diálogo **Adicionar Scaffold**, toque em **Controlador MVC com exibições, usando o Entity Framework > Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="6cef8-112">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Caixa de diálogo Adicionar Scaffold](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="6cef8-114">No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta *Controllers* **> Adicionar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="6cef8-114">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![exibição da etapa acima](adding-model/_static/add_controller.png)

<span data-ttu-id="6cef8-116">Se a caixa de diálogo **Adicionar Dependências do MVC** for exibida:</span><span class="sxs-lookup"><span data-stu-id="6cef8-116">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="6cef8-117">[Atualize o Visual Studio para a última versão](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6cef8-117">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="6cef8-118">Versões do Visual Studio anteriores a 15.5 mostram essa caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="6cef8-118">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="6cef8-119">Se não puder atualizar, selecione **ADICIONAR** e, em seguida, siga as etapas de adição do controlador novamente.</span><span class="sxs-lookup"><span data-stu-id="6cef8-119">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="6cef8-120">Na caixa de diálogo **Adicionar Scaffold**, toque em **Controlador MVC com exibições, usando o Entity Framework > Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="6cef8-120">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Caixa de diálogo Adicionar Scaffold](adding-model/_static/add_scaffold2.png)

::: moniker-end

<span data-ttu-id="6cef8-122">Preencha a caixa de diálogo **Adicionar Controlador**:</span><span class="sxs-lookup"><span data-stu-id="6cef8-122">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="6cef8-123">**Classe de modelo:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="6cef8-123">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="6cef8-124">**Classe de contexto de dados:** selecione o ícone **+** e adicione o **MvcMovie.Models.MvcMovieContext** padrão</span><span class="sxs-lookup"><span data-stu-id="6cef8-124">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Adicionar contexto de dados](adding-model/_static/dc.png)

* <span data-ttu-id="6cef8-126">**Exibições:** mantenha o padrão de cada opção marcado</span><span class="sxs-lookup"><span data-stu-id="6cef8-126">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="6cef8-127">**Nome do controlador:** mantenha o *MoviesController* padrão</span><span class="sxs-lookup"><span data-stu-id="6cef8-127">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="6cef8-128">Toque em **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="6cef8-128">Tap **Add**</span></span>

![Caixa de diálogo Adicionar Controlador](adding-model/_static/add_controller2.png)

<span data-ttu-id="6cef8-130">O Visual Studio cria:</span><span class="sxs-lookup"><span data-stu-id="6cef8-130">Visual Studio creates:</span></span>

* <span data-ttu-id="6cef8-131">Uma [classe de contexto de banco de dados](xref:data/ef-mvc/intro#create-the-database-context) do Entity Framework Core (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="6cef8-131">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="6cef8-132">Um controlador de filmes (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="6cef8-132">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="6cef8-133">Arquivos de exibição do Razor para as páginas Criar, Excluir, Detalhes, Editar e Índice (<em>Views/Movies/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="6cef8-133">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="6cef8-134">A criação automática do contexto de banco de dados e das exibições e métodos de ação [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (criar, ler, atualizar e excluir) é conhecida como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="6cef8-134">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="6cef8-135">Logo você terá um aplicativo Web totalmente funcional que permitirá que você gerencie um banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="6cef8-135">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="6cef8-136">Se você executar o aplicativo e clicar no link **Filme do MVC**, receberá um erro semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="6cef8-136">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="6cef8-137">Você precisa criar o banco de dados e usará o recurso [Migrações](xref:data/ef-mvc/migrations) do EF Core para fazer isso.</span><span class="sxs-lookup"><span data-stu-id="6cef8-137">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="6cef8-138">As Migrações permitem criar um banco de dados que corresponde ao seu modelo de dados e atualizar o esquema de banco de dados quando o modelo de dados é alterado.</span><span class="sxs-lookup"><span data-stu-id="6cef8-138">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="6cef8-139">Adicionar ferramentas do EF e executar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="6cef8-139">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="6cef8-140">Nesta seção, você usará o PMC (Console de Gerenciador de Pacotes) para:</span><span class="sxs-lookup"><span data-stu-id="6cef8-140">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="6cef8-141">Adicione o pacote de Ferramentas do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6cef8-141">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="6cef8-142">Esse pacote é necessário adicionar migrações e atualizar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6cef8-142">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="6cef8-143">Adicione uma migração inicial.</span><span class="sxs-lookup"><span data-stu-id="6cef8-143">Add an initial migration.</span></span>
* <span data-ttu-id="6cef8-144">Atualize o banco de dados com a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="6cef8-144">Update the database with the initial migration.</span></span>

<span data-ttu-id="6cef8-145">No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="6cef8-145">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Menu do PMC](adding-model/_static/pmc.png)

<span data-ttu-id="6cef8-147">No PMC, insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="6cef8-147">In the PMC, enter the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="6cef8-148">Ignore a mensagem de erro a seguir, estamos corrigindo-a no próximo tutorial:</span><span class="sxs-lookup"><span data-stu-id="6cef8-148">Ignore the following error message, we fix it in the next tutorial:</span></span>

<span data-ttu-id="6cef8-149">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span><span class="sxs-lookup"><span data-stu-id="6cef8-149">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span></span>  
      <span data-ttu-id="6cef8-150">*Nenhum tipo foi especificado para a coluna decimal "Preço" no tipo de entidade "Filme". Isso fará com que valores sejam truncados silenciosamente se não couberem na precisão e na escala padrão. Especifique explicitamente o tipo de coluna do SQL Server que pode acomodar todos os valores usando 'ForHasColumnType()'.*</span><span class="sxs-lookup"><span data-stu-id="6cef8-150">*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="6cef8-151">**Observação:** se você receber um erro com o comando `Install-Package`, abra o Gerenciador de Pacotes NuGet e pesquise pelo pacote `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="6cef8-151">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="6cef8-152">Isso permite que você instale o pacote ou verifique se ele já está instalado.</span><span class="sxs-lookup"><span data-stu-id="6cef8-152">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="6cef8-153">Observação: consulte a [abordagem da CLI](#cli) caso você tenha problemas com o PMC.</span><span class="sxs-lookup"><span data-stu-id="6cef8-153">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

::: moniker-end

<span data-ttu-id="6cef8-154">O comando `Add-Migration` cria um código para criar o esquema de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="6cef8-154">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="6cef8-155">O esquema é baseado no modelo especificado no `DbContext` (no arquivo *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="6cef8-155">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="6cef8-156">O argumento `Initial` é usado para nomear as migrações.</span><span class="sxs-lookup"><span data-stu-id="6cef8-156">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="6cef8-157">Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração.</span><span class="sxs-lookup"><span data-stu-id="6cef8-157">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="6cef8-158">Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="6cef8-158">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="6cef8-159">O comando `Update-Database` executa o método `Up` no arquivo *Migrations/\<time-stamp>_Initial.cs*, que cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6cef8-159">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="6cef8-160">Execute as etapas anteriores usando a CLI (interface de linha de comando) em vez do PMC:</span><span class="sxs-lookup"><span data-stu-id="6cef8-160">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="6cef8-161">Adicione as [ferramentas do EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) ao arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="6cef8-161">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="6cef8-162">Execute os seguintes comandos no console (no diretório do projeto):</span><span class="sxs-lookup"><span data-stu-id="6cef8-162">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  <span data-ttu-id="6cef8-163">Se você executar o aplicativo e obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="6cef8-163">If you run the app and get the error:</span></span>

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="6cef8-164">Provavelmente você não terá executado `dotnet ef database update`.</span><span class="sxs-lookup"><span data-stu-id="6cef8-164">You probably have not run `dotnet ef database update`.</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="6cef8-165">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span><span class="sxs-lookup"><span data-stu-id="6cef8-165">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="6cef8-166">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="6cef8-166">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span></span>
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Menu contextual do IntelliSense em um item de Modelo listando as propriedades disponíveis para ID, Preço, Data de Lançamento e Título](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="6cef8-168">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="6cef8-168">Additional resources</span></span>

* [<span data-ttu-id="6cef8-169">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="6cef8-169">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="6cef8-170">Globalização e localização</span><span class="sxs-lookup"><span data-stu-id="6cef8-170">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="6cef8-171">[Anterior – Adicionando uma exibição](adding-view.md)
> [Próximo – Trabalhando com o SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="6cef8-171">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
