---
title: ASP.NET Core MVC com EF Core – migrações – 4 de 10
author: rick-anderson
description: Neste tutorial, você começa a usar o recurso de migrações do EF Core para gerenciar alterações do modelo de dados em um aplicativo ASP.NET Core MVC.
ms.author: tdykstra
ms.date: 03/15/2018
uid: data/ef-mvc/migrations
ms.openlocfilehash: f710b33ac1a6017b0e3d7e8c3e528675a41424bb
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37092936"
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a><span data-ttu-id="be77d-103">ASP.NET Core MVC com EF Core – migrações – 4 de 10</span><span class="sxs-lookup"><span data-stu-id="be77d-103">ASP.NET Core MVC with EF Core - Migrations - 4 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="be77d-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="be77d-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="be77d-105">O aplicativo web de exemplo Contoso University demonstra como criar aplicativos web do ASP.NET Core MVC usando o Entity Framework Core e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="be77d-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="be77d-106">Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="be77d-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="be77d-107">Neste tutorial, você começa usando o recurso de migrações do EF Core para o gerenciamento de alterações do modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="be77d-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="be77d-108">Em tutoriais seguintes, você adicionará mais migrações conforme você alterar o modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="be77d-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="be77d-109">Introdução às migrações</span><span class="sxs-lookup"><span data-stu-id="be77d-109">Introduction to migrations</span></span>

<span data-ttu-id="be77d-110">Quando você desenvolve um novo aplicativo, o modelo de dados é alterado com frequência e, sempre que o modelo é alterado, ele fica fora de sincronia com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="be77d-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="be77d-111">Você começou estes tutoriais configurando o Entity Framework para criar o banco de dados, caso ele não exista.</span><span class="sxs-lookup"><span data-stu-id="be77d-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="be77d-112">Em seguida, sempre que você alterar o modelo de dados – adicionar, remover, alterar classes de entidade ou alterar a classe DbContext –, poderá excluir o banco de dados e o EF criará um novo que corresponde ao modelo e o propagará com os dados de teste.</span><span class="sxs-lookup"><span data-stu-id="be77d-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="be77d-113">Esse método de manter o banco de dados em sincronia com o modelo de dados funciona bem até que você implante o aplicativo em produção.</span><span class="sxs-lookup"><span data-stu-id="be77d-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="be77d-114">Quando o aplicativo é executado em produção, ele normalmente armazena os dados que você deseja manter, e você não quer perder tudo sempre que fizer uma alteração, como a adição de uma nova coluna.</span><span class="sxs-lookup"><span data-stu-id="be77d-114">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="be77d-115">O recurso Migrações do EF Core resolve esse problema, permitindo que o EF atualize o esquema de banco de dados em vez de criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="be77d-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="be77d-116">Pacotes NuGet do Entity Framework Core para migrações</span><span class="sxs-lookup"><span data-stu-id="be77d-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="be77d-117">Para trabalhar com migrações, use o **PMC** (Console do Gerenciador de Pacotes) ou a CLI (interface de linha de comando).</span><span class="sxs-lookup"><span data-stu-id="be77d-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="be77d-118">Esses tutoriais mostram como usar comandos da CLI.</span><span class="sxs-lookup"><span data-stu-id="be77d-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="be77d-119">Encontre informações sobre o PMC no [final deste tutorial](#pmc).</span><span class="sxs-lookup"><span data-stu-id="be77d-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="be77d-120">As ferramentas do EF para a CLI (interface de linha de comando) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="be77d-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="be77d-121">Para instalar esse pacote, adicione-o à coleção `DotNetCliToolReference` no arquivo *.csproj*, conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="be77d-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="be77d-122">**Observação:** é necessário instalar este pacote editando o arquivo *.csproj*; não é possível usar o comando `install-package` ou a GUI do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="be77d-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="be77d-123">Edite o arquivo *.csproj* clicando com o botão direito do mouse no nome do projeto no **Gerenciador de Soluções** e selecionando **Editar ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="be77d-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="be77d-124">(Neste exemplo, os números de versão eram atuais no momento em que o tutorial foi escrito.)</span><span class="sxs-lookup"><span data-stu-id="be77d-124">(The version numbers in this example were current when the tutorial was written.)</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="be77d-125">Alterar a cadeia de conexão</span><span class="sxs-lookup"><span data-stu-id="be77d-125">Change the connection string</span></span>

<span data-ttu-id="be77d-126">No arquivo *appsettings.json*, altere o nome do banco de dados na cadeia de conexão para ContosoUniversity2 ou outro nome que você ainda não usou no computador que está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="be77d-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="be77d-127">Essa alteração configura o projeto, de modo que a primeira migração crie um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="be77d-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="be77d-128">Isso não é necessário para começar as migrações, mas você verá mais tarde o motivo pelo qual essa é uma boa ideia.</span><span class="sxs-lookup"><span data-stu-id="be77d-128">This isn't required to get started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="be77d-129">Como alternativa à alteração do nome do banco de dados, você pode excluir o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="be77d-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="be77d-130">Use o **SSOX** (Pesquisador de Objetos do SQL Server) ou o comando `database drop` da CLI:</span><span class="sxs-lookup"><span data-stu-id="be77d-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="be77d-131">A seção a seguir explica como executar comandos da CLI.</span><span class="sxs-lookup"><span data-stu-id="be77d-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="be77d-132">Criar uma migração inicial</span><span class="sxs-lookup"><span data-stu-id="be77d-132">Create an initial migration</span></span>

<span data-ttu-id="be77d-133">Salve as alterações e compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="be77d-133">Save your changes and build the project.</span></span> <span data-ttu-id="be77d-134">Em seguida, abra uma janela Comando e navegue para a pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="be77d-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="be77d-135">Esta é uma maneira rápida de fazer isso:</span><span class="sxs-lookup"><span data-stu-id="be77d-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="be77d-136">No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e escolha **Abrir no Explorador de Arquivos** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="be77d-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Abrir no item de menu do Explorador de Arquivos](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="be77d-138">Insira "cmd" na barra de endereços e pressione Enter.</span><span class="sxs-lookup"><span data-stu-id="be77d-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Abrir a janela Comando](migrations/_static/open-command-window.png)

<span data-ttu-id="be77d-140">Insira o seguinte comando na janela de comando:</span><span class="sxs-lookup"><span data-stu-id="be77d-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="be77d-141">Você verá uma saída semelhante à seguinte na janela Comando:</span><span class="sxs-lookup"><span data-stu-id="be77d-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="be77d-142">Se você receber uma mensagem de erro *Nenhum comando "dotnet-ef" executável correspondente encontrado*, consulte [esta postagem no blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) para ajudar a solucionar o problema.</span><span class="sxs-lookup"><span data-stu-id="be77d-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="be77d-143">Se você receber uma mensagem de erro "*Não é possível acessar o arquivo... ContosoUniversity.dll porque ele está sendo usado por outro processo*", localize o ícone do IIS Express na Bandeja do Sistema do Windows, clique com o botão direito do mouse nele e, em seguida, clique em **ContosoUniversity > Parar Site**.</span><span class="sxs-lookup"><span data-stu-id="be77d-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="be77d-144">Examinar os métodos Up e Down</span><span class="sxs-lookup"><span data-stu-id="be77d-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="be77d-145">Quando você executou o comando `migrations add`, o EF gerou o código que criará o banco de dados do zero.</span><span class="sxs-lookup"><span data-stu-id="be77d-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="be77d-146">Esse código está localizado na pasta *Migrations*, no arquivo chamado *\<timestamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="be77d-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="be77d-147">O método `Up` da classe `InitialCreate` cria as tabelas de banco de dados que correspondem aos conjuntos de entidades do modelo de dados, e o método `Down` exclui-as, conforme mostrado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="be77d-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="be77d-148">As migrações chamam o método `Up` para implementar as alterações do modelo de dados para uma migração.</span><span class="sxs-lookup"><span data-stu-id="be77d-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="be77d-149">Quando você insere um comando para reverter a atualização, as Migrações chamam o método `Down`.</span><span class="sxs-lookup"><span data-stu-id="be77d-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="be77d-150">Esse código destina-se à migração inicial que foi criada quando você inseriu o comando `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="be77d-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="be77d-151">O parâmetro de nome da migração ("InitialCreate" no exemplo) é usado para o nome do arquivo e pode ser o que você desejar.</span><span class="sxs-lookup"><span data-stu-id="be77d-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="be77d-152">É melhor escolher uma palavra ou frase que resume o que está sendo feito na migração.</span><span class="sxs-lookup"><span data-stu-id="be77d-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="be77d-153">Por exemplo, você pode nomear uma migração posterior "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="be77d-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="be77d-154">Se você criou a migração inicial quando o banco de dados já existia, o código de criação de banco de dados é gerado, mas ele não precisa ser executado porque o banco de dados já corresponde ao modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="be77d-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="be77d-155">Quando você implantar o aplicativo em outro ambiente no qual o banco de dados ainda não existe, esse código será executado para criar o banco de dados; portanto, é uma boa ideia testá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="be77d-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="be77d-156">É por isso que você alterou o nome do banco de dados na cadeia de conexão anteriormente – para que as migrações possam criar um novo do zero.</span><span class="sxs-lookup"><span data-stu-id="be77d-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="be77d-157">O instantâneo do modelo de dados</span><span class="sxs-lookup"><span data-stu-id="be77d-157">The data model snapshot</span></span>

<span data-ttu-id="be77d-158">As migrações criam um *instantâneo* do esquema de banco de dados atual em *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="be77d-158">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="be77d-159">Quando você adiciona uma migração, o EF determina o que foi alterado, comparando o modelo de dados com o arquivo de instantâneo.</span><span class="sxs-lookup"><span data-stu-id="be77d-159">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="be77d-160">Ao excluir uma migração, use o comando [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="be77d-160">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="be77d-161">`dotnet ef migrations remove` exclui a migração e garante que o instantâneo seja redefinido corretamente.</span><span class="sxs-lookup"><span data-stu-id="be77d-161">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="be77d-162">Confira [Migrações do EF Core em ambientes de equipe](/ef/core/managing-schemas/migrations/teams) para obter mais informações de como o arquivo de instantâneo é usado.</span><span class="sxs-lookup"><span data-stu-id="be77d-162">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="be77d-163">Aplicar a migração ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="be77d-163">Apply the migration to the database</span></span>

<span data-ttu-id="be77d-164">Na janela Comando, insira o comando a seguir para criar o banco de dados e tabelas nele.</span><span class="sxs-lookup"><span data-stu-id="be77d-164">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="be77d-165">A saída do comando é semelhante ao comando `migrations add`, exceto que os logs para os comandos SQL que configuram o banco de dados são exibidos.</span><span class="sxs-lookup"><span data-stu-id="be77d-165">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="be77d-166">A maioria dos logs é omitida na seguinte saída de exemplo.</span><span class="sxs-lookup"><span data-stu-id="be77d-166">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="be77d-167">Se você preferir não ver esse nível de detalhe em mensagens de log, altere o nível de log no arquivo *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="be77d-167">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="be77d-168">Para obter mais informações, consulte [Introdução ao log](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="be77d-168">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

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

<span data-ttu-id="be77d-169">Use o **Pesquisador de Objetos do SQL Server** para inspecionar o banco de dados como você fez no primeiro tutorial.</span><span class="sxs-lookup"><span data-stu-id="be77d-169">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="be77d-170">Você observará a adição de uma tabela __EFMigrationsHistory que controla quais migrações foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="be77d-170">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="be77d-171">Exiba os dados dessa tabela e você verá uma linha para a primeira migração.</span><span class="sxs-lookup"><span data-stu-id="be77d-171">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="be77d-172">(O último log no exemplo de saída da CLI anterior mostra a instrução INSERT que cria essa linha.)</span><span class="sxs-lookup"><span data-stu-id="be77d-172">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="be77d-173">Execute o aplicativo para verificar se tudo ainda funciona como antes.</span><span class="sxs-lookup"><span data-stu-id="be77d-173">Run the application to verify that everything still works the same as before.</span></span>

![Página Índice de Alunos](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="be77d-175">CLI (interface de linha de comando) vs. PMC (Console do Gerenciador de Pacotes)</span><span class="sxs-lookup"><span data-stu-id="be77d-175">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="be77d-176">As ferramentas do EF para gerenciamento de migrações estão disponíveis por meio dos comandos da CLI do .NET Core ou de cmdlets do PowerShell na janela **PMC** (Console do Gerenciador de Pacotes) do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="be77d-176">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="be77d-177">Este tutorial mostra como usar a CLI, mas você poderá usar o PMC se preferir.</span><span class="sxs-lookup"><span data-stu-id="be77d-177">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="be77d-178">Os comandos do EF para os comandos do PMC estão no pacote [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools).</span><span class="sxs-lookup"><span data-stu-id="be77d-178">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="be77d-179">Este pacote já está incluído no metapacote [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) e, portanto, não é necessário instalá-lo.</span><span class="sxs-lookup"><span data-stu-id="be77d-179">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="be77d-180">**Importante:** esse não é o mesmo pacote que é instalado para a CLI com a edição do arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="be77d-180">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="be77d-181">O nome deste termina com `Tools`, ao contrário do nome do pacote da CLI que termina com `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="be77d-181">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="be77d-182">Para obter mais informações sobre os comandos da CLI, consulte [CLI do .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="be77d-182">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="be77d-183">Para obter mais informações sobre os comandos do PMC, consulte [Console do Gerenciador de Pacotes (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="be77d-183">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="be77d-184">Resumo</span><span class="sxs-lookup"><span data-stu-id="be77d-184">Summary</span></span>

<span data-ttu-id="be77d-185">Neste tutorial, você viu como criar e aplicar sua primeira migração.</span><span class="sxs-lookup"><span data-stu-id="be77d-185">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="be77d-186">No próximo tutorial, você começará examinando tópicos mais avançados com a expansão do modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="be77d-186">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="be77d-187">Ao longo do processo, você criará e aplicará migrações adicionais.</span><span class="sxs-lookup"><span data-stu-id="be77d-187">Along the way you'll create and apply additional migrations.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="be77d-188">[Anterior](sort-filter-page.md)
> [Próximo](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="be77d-188">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>
