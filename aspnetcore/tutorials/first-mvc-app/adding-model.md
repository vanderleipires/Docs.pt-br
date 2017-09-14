---
title: Adicionando um modelo a um aplicativo ASP.NET Core MVC
author: rick-anderson
description: Adicione um modelo a um aplicativo ASP.NET Core simples.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 8d0bebc22e1cfdc6d9b213d0c3159a7dab988020
ms.sourcegitcommit: 0bd3f6ec577c648dd777877e97572ec2da1b36c4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="b4f0e-104">Observação: os modelos do ASP.NET Core 2.0 contêm a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="b4f0e-105">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **MvcMovie** > **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-105">In Solution Explorer, right click the **MvcMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="b4f0e-106">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-106">Name the folder *Models*.</span></span>

<span data-ttu-id="b4f0e-107">Clique com o botão direito do mouse na pasta *Models* > **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-107">Right click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="b4f0e-108">Nomeie a classe **Movie** e adicione as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="b4f0e-108">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="b4f0e-109">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="b4f0e-109">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span></span>

<span data-ttu-id="b4f0e-110">O campo `ID` é necessário para o banco de dados para a chave primária.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-110">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="b4f0e-111">Compile o projeto para verificar se não há erros.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-111">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="b4f0e-112">Agora você tem um **M**odelo no aplicativo **M**VC.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-112">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="b4f0e-113">Gerando um controlador por scaffolding</span><span class="sxs-lookup"><span data-stu-id="b4f0e-113">Scaffolding a controller</span></span>

<span data-ttu-id="b4f0e-114">No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta *Controllers* **> Adicionar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-114">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![exibição da etapa acima](adding-model/_static/add_controller.png)

<span data-ttu-id="b4f0e-116">Na caixa de diálogo **Adicionar Dependências do MVC**, selecione **Dependências Mínimas** e **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-116">In the **Add MVC Dependencies** dialog, select **Minimal Dependencies**, and select **Add**.</span></span>

![exibição da etapa acima](adding-model/_static/add_depend.png)

<span data-ttu-id="b4f0e-118">O Visual Studio adiciona as dependências necessárias para gerar um controlador por scaffolding, mas o próprio controlador não é criado.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-118">Visual Studio adds the dependencies needed to scaffold a controller, but the controller itself is not created.</span></span> <span data-ttu-id="b4f0e-119">A próximo invocação de **> Adicionar > Controlador** cria o controlador.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-119">The next invoke of **> Add > Controller** creates the controller.</span></span> 

<span data-ttu-id="b4f0e-120">No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta *Controllers* **> Adicionar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-120">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![exibição da etapa acima](adding-model/_static/add_controller.png)

<span data-ttu-id="b4f0e-122">Na caixa de diálogo **Adicionar Scaffold**, toque em **Controlador MVC com exibições, usando o Entity Framework > Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-122">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Caixa de diálogo Adicionar Scaffold](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="b4f0e-124">Preencha a caixa de diálogo **Adicionar Controlador**:</span><span class="sxs-lookup"><span data-stu-id="b4f0e-124">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="b4f0e-125">**Classe de modelo:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="b4f0e-125">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="b4f0e-126">**Classe de contexto de dados:** selecione o ícone **+** e adicione o **MvcMovie.Models.MvcMovieContext** padrão</span><span class="sxs-lookup"><span data-stu-id="b4f0e-126">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Adicionar contexto de dados](adding-model/_static/dc.png)

* <span data-ttu-id="b4f0e-128">**Exibições:** mantenha o padrão de cada opção marcado</span><span class="sxs-lookup"><span data-stu-id="b4f0e-128">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="b4f0e-129">**Nome do controlador:** mantenha o *MoviesController* padrão</span><span class="sxs-lookup"><span data-stu-id="b4f0e-129">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="b4f0e-130">Toque em **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="b4f0e-130">Tap **Add**</span></span>

![Caixa de diálogo Adicionar Controlador](adding-model/_static/add_controller2.png)

<span data-ttu-id="b4f0e-132">O Visual Studio cria:</span><span class="sxs-lookup"><span data-stu-id="b4f0e-132">Visual Studio creates:</span></span>

* <span data-ttu-id="b4f0e-133">Uma [classe de contexto de banco de dados](xref:data/ef-mvc/intro#create-the-database-context) do Entity Framework Core (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="b4f0e-133">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="b4f0e-134">Um controlador de filmes (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="b4f0e-134">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="b4f0e-135">Arquivos de exibição do Razor para as páginas Criar, Excluir, Detalhes, Editar e Índice (*Views/Movies/&ast;.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="b4f0e-135">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="b4f0e-136">A criação automática do contexto de banco de dados e das exibições e métodos de ação [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (criar, ler, atualizar e excluir) é conhecida como *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-136">The automatic creation of the database context and [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="b4f0e-137">Logo você terá um aplicativo Web totalmente funcional que permitirá que você gerencie um banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-137">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="b4f0e-138">Se você executar o aplicativo e clicar no link **Mvc Movie**, receberá um erro semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="b4f0e-138">If you run the app and click on the **Mvc Movie** link, you'll get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="b4f0e-139">Você precisa criar o banco de dados e usará o recurso [Migrações](xref:data/ef-mvc/migrations) do EF Core para fazer isso.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-139">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="b4f0e-140">As Migrações permitem criar um banco de dados que corresponde ao seu modelo de dados e atualizar o esquema de banco de dados quando o modelo de dados é alterado.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-140">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="b4f0e-141">Adicionar ferramentas do EF e executar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="b4f0e-141">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="b4f0e-142">Nesta seção, você usará o PMC (Console de Gerenciador de Pacotes) para:</span><span class="sxs-lookup"><span data-stu-id="b4f0e-142">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="b4f0e-143">Adicione o pacote de Ferramentas do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-143">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="b4f0e-144">Esse pacote é necessário adicionar migrações e atualizar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-144">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="b4f0e-145">Adicione uma migração inicial.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-145">Add an initial migration.</span></span>
* <span data-ttu-id="b4f0e-146">Atualize o banco de dados com a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-146">Update the database with the initial migration.</span></span>

<span data-ttu-id="b4f0e-147">No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-147">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Menu do PMC](adding-model/_static/pmc.png)

<span data-ttu-id="b4f0e-149">No PMC, insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="b4f0e-149">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="b4f0e-150">Observação: consulte a [abordagem da CLI](#cli) caso tenha problemas com o PMC.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-150">Note: See the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="b4f0e-151">O comando `Add-Migration` cria um código para criar o esquema de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-151">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="b4f0e-152">O esquema é baseado no modelo especificado no `DbContext` (no arquivo *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="b4f0e-152">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="b4f0e-153">O argumento `Initial` é usado para nomear as migrações.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-153">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="b4f0e-154">Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-154">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="b4f0e-155">Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-155">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="b4f0e-156">O comando `Update-Database` executa o método `Up` no arquivo *Migrations/\<time-stamp>_InitialCreate.cs*, que cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-156">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="b4f0e-157"><a name="cli"></a> Execute as etapas anteriores usando a CLI (interface de linha de comando) em vez do PMC:</span><span class="sxs-lookup"><span data-stu-id="b4f0e-157"><a name="cli"></a> You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="b4f0e-158">Adicione as [ferramentas do EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) ao arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="b4f0e-158">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="b4f0e-159">Execute os seguintes comandos no console (no diretório do projeto):</span><span class="sxs-lookup"><span data-stu-id="b4f0e-159">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="b4f0e-160">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="b4f0e-160">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span></span>

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Menu contextual do IntelliSense em um item de Modelo listando as propriedades disponíveis para ID, Preço, Data de Lançamento e Título](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="b4f0e-162">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="b4f0e-162">Additional resources</span></span>

* [<span data-ttu-id="b4f0e-163">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="b4f0e-163">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="b4f0e-164">Globalização e localização</span><span class="sxs-lookup"><span data-stu-id="b4f0e-164">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="b4f0e-165">[Anterior – Adicionando uma exibição](adding-view.md)
[Próximo – Trabalhando com o SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="b4f0e-165">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
