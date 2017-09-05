---
title: "Núcleo do ASP.NET MVC com núcleo EF - Migrations - 4 de 10"
author: tdykstra
description: "Neste tutorial, você começar a usar o recurso de migrações EF principal para o gerenciamento de alterações do modelo de dados em um aplicativo MVC do ASP.NET Core."
keywords: "Migrações do ASP.NET Core, Entity Framework Core,"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 81f6c9c2-a819-4f3a-97a4-4b0503b56c26
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: 4d81099d1ab97a8a49d96657153a54aa96dd6bf8
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2017
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a><span data-ttu-id="31015-104">Migrações - Core EF com o tutorial do MVC do ASP.NET Core (4 de 10)</span><span class="sxs-lookup"><span data-stu-id="31015-104">Migrations - EF Core with ASP.NET Core MVC tutorial (4 of 10)</span></span>

<span data-ttu-id="31015-105">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="31015-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="31015-106">O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31015-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="31015-107">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="31015-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="31015-108">Neste tutorial, você começar a usar o recurso de migrações EF principal para o gerenciamento de alterações do modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="31015-108">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="31015-109">Em tutoriais subsequentes, você adicionará mais migrações que você alterar o modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="31015-109">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="31015-110">Introdução às migrações</span><span class="sxs-lookup"><span data-stu-id="31015-110">Introduction to migrations</span></span>

<span data-ttu-id="31015-111">Quando você desenvolve um novo aplicativo, o modelo de dados é alterado com frequência e cada vez que as alterações do modelo, ele obtém fora de sincronia com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="31015-111">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="31015-112">Iniciar esses tutoriais, configurando o Entity Framework para criar o banco de dados se ele não existir.</span><span class="sxs-lookup"><span data-stu-id="31015-112">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="31015-113">Em seguida, cada vez que você alterar o modelo de dados - adiciona, remove, alterar as classes de entidade ou alterar sua classe DbContext – você pode excluir o banco de dados e EF cria um novo que corresponde ao modelo e propaga-lo com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="31015-113">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="31015-114">Esse método de manter o banco de dados em sincronia com o modelo de dados funciona bem até que você implantar o aplicativo para produção.</span><span class="sxs-lookup"><span data-stu-id="31015-114">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="31015-115">Quando o aplicativo é executado em produção que normalmente está armazenando os dados que você deseja manter, e você não quiser perder tudo o que cada vez que você fizer uma alteração, como adicionar uma nova coluna.</span><span class="sxs-lookup"><span data-stu-id="31015-115">When the application is running in production it is usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="31015-116">O recurso de migrações de núcleo EF resolve esse problema, permitindo que o EF atualizar o esquema de banco de dados em vez de criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="31015-116">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="31015-117">Pacotes do Entity Framework Core NuGet para migrações</span><span class="sxs-lookup"><span data-stu-id="31015-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="31015-118">Para trabalhar com migrações, você pode usar o **Package Manager Console** (PMC) ou a interface de linha de comando (CLI).</span><span class="sxs-lookup"><span data-stu-id="31015-118">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="31015-119">Esses tutoriais mostram como usar comandos CLI.</span><span class="sxs-lookup"><span data-stu-id="31015-119">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="31015-120">Informações sobre o PMC estão no [o fim deste tutorial](#pmc).</span><span class="sxs-lookup"><span data-stu-id="31015-120">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="31015-121">As ferramentas EF para a interface de linha de comando (CLI) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="31015-121">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="31015-122">Para instalar este pacote, adicione-o para o `DotNetCliToolReference` coleção no *. csproj* de arquivo, como mostrado.</span><span class="sxs-lookup"><span data-stu-id="31015-122">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="31015-123">**Observação:** é necessário instalar este pacote editando o *. csproj* arquivo; não é possível usar o `install-package` comando ou a GUI do Gerenciador de pacote.</span><span class="sxs-lookup"><span data-stu-id="31015-123">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="31015-124">Você pode editar o *. csproj* arquivo clicando com o nome do projeto no **Solution Explorer** e selecionando **ContosoUniversity.csproj editar**.</span><span class="sxs-lookup"><span data-stu-id="31015-124">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="31015-125">(Neste exemplo, os números de versão foram atuais quando o tutorial foi escrito).</span><span class="sxs-lookup"><span data-stu-id="31015-125">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="31015-126">Altere a cadeia de conexão</span><span class="sxs-lookup"><span data-stu-id="31015-126">Change the connection string</span></span>

<span data-ttu-id="31015-127">No *appSettings. JSON* arquivo, altere o nome do banco de dados na cadeia de conexão para ContosoUniversity2 ou algum outro nome que você ainda não usado no computador que você está usando.</span><span class="sxs-lookup"><span data-stu-id="31015-127">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

<span data-ttu-id="31015-128">[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]</span><span class="sxs-lookup"><span data-stu-id="31015-128">[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]</span></span>

<span data-ttu-id="31015-129">Essa alteração define o projeto para que a primeira migração irá criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="31015-129">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="31015-130">Isso não é necessário para começar a trabalhar com migrações, mas você verá posteriormente por que ele é uma boa ideia.</span><span class="sxs-lookup"><span data-stu-id="31015-130">This isn't required for getting started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="31015-131">Como alternativa para alterar o nome do banco de dados, você pode excluir o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="31015-131">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="31015-132">Use **Pesquisador de objetos do SQL Server** (SSOX) ou o `database drop` comando CLI:</span><span class="sxs-lookup"><span data-stu-id="31015-132">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="31015-133">A seção a seguir explica como executar comandos CLI.</span><span class="sxs-lookup"><span data-stu-id="31015-133">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="31015-134">Criar uma migração inicial</span><span class="sxs-lookup"><span data-stu-id="31015-134">Create an initial migration</span></span>

<span data-ttu-id="31015-135">Salve suas alterações e compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="31015-135">Save your changes and build the project.</span></span> <span data-ttu-id="31015-136">Em seguida, abra uma janela de comando e navegue até a pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="31015-136">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="31015-137">Aqui está uma maneira rápida de fazer isso:</span><span class="sxs-lookup"><span data-stu-id="31015-137">Here's a quick way to do that:</span></span>

* <span data-ttu-id="31015-138">Em **Solution Explorer**, clique com o botão direito e escolha **abrir no Explorador de arquivos** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="31015-138">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Abrir no item de menu do Explorador de arquivos](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="31015-140">Digite "cmd" na barra de endereços e pressione Enter.</span><span class="sxs-lookup"><span data-stu-id="31015-140">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Abrir janela de comando](migrations/_static/open-command-window.png)

<span data-ttu-id="31015-142">Digite o seguinte comando na janela de comando:</span><span class="sxs-lookup"><span data-stu-id="31015-142">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="31015-143">Você verá a saída semelhante à seguinte na janela de comando:</span><span class="sxs-lookup"><span data-stu-id="31015-143">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="31015-144">Se você vir uma mensagem de erro *sem executável encontrado correspondente comando "dotnet-ef"*, consulte [esta postagem de blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) para solucionar o problema.</span><span class="sxs-lookup"><span data-stu-id="31015-144">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="31015-145">Se você vir uma mensagem de erro "*não é possível acessar o arquivo... ContosoUniversity.dll porque ele está sendo usado por outro processo.* ", localize o ícone do IIS Express na bandeja de sistema do Windows, clique duas vezes e clique em **ContosoUniversity > Parar Site**.</span><span class="sxs-lookup"><span data-stu-id="31015-145">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="31015-146">Examine cima e para baixo métodos</span><span class="sxs-lookup"><span data-stu-id="31015-146">Examine the Up and Down methods</span></span>

<span data-ttu-id="31015-147">Quando você executou o `migrations add` comando EF o código gerado que criará o banco de dados do zero.</span><span class="sxs-lookup"><span data-stu-id="31015-147">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="31015-148">Esse código está no *migrações* pasta, no arquivo nomeado  *\<timestamp > _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="31015-148">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="31015-149">O `Up` método o `InitialCreate` classe cria as tabelas de banco de dados que correspondem aos conjuntos de entidade do modelo de dados, e o `Down` método exclui-los, conforme mostrado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="31015-149">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

<span data-ttu-id="31015-150">[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]</span><span class="sxs-lookup"><span data-stu-id="31015-150">[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]</span></span>

<span data-ttu-id="31015-151">Chamadas de migrações de `Up` método para implementar as alterações do modelo de dados para uma migração.</span><span class="sxs-lookup"><span data-stu-id="31015-151">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="31015-152">Quando você insere um comando para reverter a atualização, chamadas de migrações de `Down` método.</span><span class="sxs-lookup"><span data-stu-id="31015-152">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="31015-153">Esse código é para a migração inicial que foi criada quando você inseriu o `migrations add InitialCreate` comando.</span><span class="sxs-lookup"><span data-stu-id="31015-153">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="31015-154">O parâmetro de nome de migração ("InitialCreate" no exemplo) é usado para o nome do arquivo e pode ser que desejar.</span><span class="sxs-lookup"><span data-stu-id="31015-154">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="31015-155">É melhor escolher uma palavra ou frase que resume o que está sendo feito a migração.</span><span class="sxs-lookup"><span data-stu-id="31015-155">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="31015-156">Por exemplo, você pode nomear uma migração posterior "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="31015-156">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="31015-157">Se você criou a migração inicial quando o banco de dados já existe, o código de criação do banco de dados é gerado, mas ele não precisa ser executado porque o banco de dados já coincide com o modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="31015-157">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="31015-158">Quando você implanta o aplicativo para outro ambiente onde o banco de dados não existe ainda, esse código será executado para criar o banco de dados, portanto é uma boa ideia para testá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="31015-158">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="31015-159">É por isso que você alterou o nome do banco de dados na cadeia de conexão anteriormente – para que as migrações podem criar um novo a partir do zero.</span><span class="sxs-lookup"><span data-stu-id="31015-159">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="31015-160">Examine o instantâneo do modelo de dados</span><span class="sxs-lookup"><span data-stu-id="31015-160">Examine the data model snapshot</span></span>

<span data-ttu-id="31015-161">Migrações também cria um *instantâneo* do esquema de banco de dados atual em *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="31015-161">Migrations also creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="31015-162">Esse código é semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="31015-162">Here's what that code looks like:</span></span>

<span data-ttu-id="31015-163">[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]</span><span class="sxs-lookup"><span data-stu-id="31015-163">[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]</span></span>

<span data-ttu-id="31015-164">Porque o esquema de banco de dados atual é representado no código, Core EF não precisa interagir com o banco de dados para criar as migrações.</span><span class="sxs-lookup"><span data-stu-id="31015-164">Because the current database schema is represented in code, EF Core doesn't have to interact with the database to create migrations.</span></span> <span data-ttu-id="31015-165">Quando você adiciona uma migração, EF determina o que mudou, comparando o modelo de dados para o arquivo de instantâneo.</span><span class="sxs-lookup"><span data-stu-id="31015-165">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="31015-166">EF interage com o banco de dados somente quando é necessário atualizar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="31015-166">EF interacts with the database only when it has to update the database.</span></span> 

<span data-ttu-id="31015-167">O arquivo de instantâneo deve ser mantido em sincronia com as migrações que criá-la, para que você não pode remover uma migração bastando excluir o arquivo chamado  *\<timestamp > _\<migrationname >. CS*.</span><span class="sxs-lookup"><span data-stu-id="31015-167">The snapshot file has to be kept in sync with the migrations that create it, so you can't remove a migration just by deleting the file named  *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="31015-168">Se você excluir esse arquivo, as migrações restantes serão fora de sincronia com o arquivo de instantâneo do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="31015-168">If you delete that file, the remaining migrations will be out of sync with the database snapshot file.</span></span> <span data-ttu-id="31015-169">Para excluir a última migração que você adicionou, use o [remover migrações de ef dotnet](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) comando.</span><span class="sxs-lookup"><span data-stu-id="31015-169">To delete the last migration that you added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="31015-170">Aplicar a migração para o banco de dados</span><span class="sxs-lookup"><span data-stu-id="31015-170">Apply the migration to the database</span></span>

<span data-ttu-id="31015-171">Na janela de comando, digite o seguinte comando para criar o banco de dados e tabelas dentro dele.</span><span class="sxs-lookup"><span data-stu-id="31015-171">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="31015-172">A saída do comando é semelhante de `migrations add` de comando, exceto que você consulte os logs para o SQL comandos que configurar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="31015-172">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="31015-173">A maioria dos logs é omitida na saída de exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="31015-173">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="31015-174">Se você preferir não ver esse nível de detalhe em mensagens de log, você pode alterar os níveis de log no *appsettings. Development.JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="31015-174">If you prefer not to see this level of detail in log messages, you can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="31015-175">Para obter mais informações, consulte [Introdução ao registro em log](xref:fundamentals/logging).</span><span class="sxs-lookup"><span data-stu-id="31015-175">For more information, see [Introduction to logging](xref:fundamentals/logging).</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="31015-176">Use **Pesquisador de objetos do SQL Server** para inspecionar o banco de dados como você fez no primeiro tutorial.</span><span class="sxs-lookup"><span data-stu-id="31015-176">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="31015-177">Observe a adição de uma tabela __EFMigrationsHistory que controla quais migrações foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="31015-177">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="31015-178">Exibir os dados nessa tabela e você verá uma linha para a migração primeiro.</span><span class="sxs-lookup"><span data-stu-id="31015-178">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="31015-179">(O último log no exemplo de saída CLI anterior mostra a instrução INSERT que cria essa linha.)</span><span class="sxs-lookup"><span data-stu-id="31015-179">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="31015-180">Execute o aplicativo para verificar se tudo funciona ainda o mesmo de antes.</span><span class="sxs-lookup"><span data-stu-id="31015-180">Run the application to verify that everything still works the same as before.</span></span>

![Página de índice de alunos](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="31015-182">Interface de linha de comando (CLI) vs. Package Manager Console (PMC)</span><span class="sxs-lookup"><span data-stu-id="31015-182">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="31015-183">O EF ferramentas para gerenciar migrações está disponível a partir de comandos .NET Core CLI ou cmdlets do PowerShell no Visual Studio **Package Manager Console** janela (PMC).</span><span class="sxs-lookup"><span data-stu-id="31015-183">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="31015-184">Este tutorial mostra como usar a CLI, mas você pode usar o PMC se você preferir.</span><span class="sxs-lookup"><span data-stu-id="31015-184">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="31015-185">Os comandos EF para os comandos PMC estão no [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pacote.</span><span class="sxs-lookup"><span data-stu-id="31015-185">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="31015-186">Este pacote já está incluído no [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, portanto não é necessário instalá-lo.</span><span class="sxs-lookup"><span data-stu-id="31015-186">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="31015-187">**Importante:** isso não é o mesmo pacote que você instala para CLI editando o *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="31015-187">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="31015-188">O nome deste termina `Tools`, ao contrário do nome do pacote CLI que termina em `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="31015-188">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="31015-189">Para obter mais informações sobre os comandos CLI, consulte [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="31015-189">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="31015-190">Para obter mais informações sobre os comandos PMC, consulte [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="31015-190">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="31015-191">Resumo</span><span class="sxs-lookup"><span data-stu-id="31015-191">Summary</span></span>

<span data-ttu-id="31015-192">Neste tutorial, você viu como criar e aplicar a migração primeiro.</span><span class="sxs-lookup"><span data-stu-id="31015-192">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="31015-193">O seguinte tutorial, você começará olhando para tópicos mais avançados, expandindo o modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="31015-193">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="31015-194">Ao longo do processo, você vai criar e aplicar migrações adicionais.</span><span class="sxs-lookup"><span data-stu-id="31015-194">Along the way you'll create and apply additional migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="31015-195">[Anterior](sort-filter-page.md)
[Próximo](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="31015-195">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  
