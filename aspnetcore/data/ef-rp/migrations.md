---
title: Páginas Razor com o EF Core no ASP.NET Core – Migrações – 4 de 8
author: rick-anderson
description: Neste tutorial, você começa a usar o recurso de migrações do EF Core para gerenciar alterações do modelo de dados em um aplicativo ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: 690beaabeab098cf9b764730b1bf1bd04bf6b003
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="e98d2-103">Páginas Razor com o EF Core no ASP.NET Core – Migrações – 4 de 8</span><span class="sxs-lookup"><span data-stu-id="e98d2-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="e98d2-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e98d2-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="e98d2-105">Neste tutorial, o recurso de migrações do EF Core para o gerenciamento de alterações do modelo de dados é usado.</span><span class="sxs-lookup"><span data-stu-id="e98d2-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="e98d2-106">Caso tenha problemas que não consiga resolver, baixe o [aplicativo concluído para este estágio](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="e98d2-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="e98d2-107">Quando um novo aplicativo é desenvolvido, o modelo de dados é alterado com frequência.</span><span class="sxs-lookup"><span data-stu-id="e98d2-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="e98d2-108">Sempre que o modelo é alterado, ele fica fora de sincronia com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e98d2-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="e98d2-109">Este tutorial começa configurando o Entity Framework para criar o banco de dados, caso ele não exista.</span><span class="sxs-lookup"><span data-stu-id="e98d2-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="e98d2-110">Sempre que o modelo de dados é alterado:</span><span class="sxs-lookup"><span data-stu-id="e98d2-110">Each time the data model changes:</span></span>

* <span data-ttu-id="e98d2-111">O BD é removido.</span><span class="sxs-lookup"><span data-stu-id="e98d2-111">The DB is dropped.</span></span>
* <span data-ttu-id="e98d2-112">O EF cria um novo que corresponde ao modelo.</span><span class="sxs-lookup"><span data-stu-id="e98d2-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="e98d2-113">O aplicativo propaga o BD com os dados de teste.</span><span class="sxs-lookup"><span data-stu-id="e98d2-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="e98d2-114">Essa abordagem para manter o BD em sincronia com o modelo de dados funciona bem até que você implante o aplicativo em produção.</span><span class="sxs-lookup"><span data-stu-id="e98d2-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="e98d2-115">Quando o aplicativo é executado em produção, normalmente, ele armazena dados que precisam ser mantidos.</span><span class="sxs-lookup"><span data-stu-id="e98d2-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="e98d2-116">O aplicativo não pode começar com um BD de teste sempre que uma alteração é feita (como a adição de uma nova coluna).</span><span class="sxs-lookup"><span data-stu-id="e98d2-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="e98d2-117">O recurso Migrações do EF Core resolve esse problema, permitindo que o EF Core atualize o esquema de BD em vez de criar um novo BD.</span><span class="sxs-lookup"><span data-stu-id="e98d2-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="e98d2-118">Em vez de remover e recriar o BD quando o modelo de dados é alterado, as migrações atualizam o esquema e retêm os dados existentes.</span><span class="sxs-lookup"><span data-stu-id="e98d2-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="e98d2-119">Pacotes NuGet do Entity Framework Core para migrações</span><span class="sxs-lookup"><span data-stu-id="e98d2-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="e98d2-120">Para trabalhar com migrações, use o **PMC** (Console do Gerenciador de Pacotes) ou a CLI (interface de linha de comando).</span><span class="sxs-lookup"><span data-stu-id="e98d2-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="e98d2-121">Esses tutoriais mostram como usar comandos da CLI.</span><span class="sxs-lookup"><span data-stu-id="e98d2-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="e98d2-122">Encontre informações sobre o PMC no [final deste tutorial](#pmc).</span><span class="sxs-lookup"><span data-stu-id="e98d2-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="e98d2-123">As ferramentas do EF Core para a CLI (interface de linha de comando) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="e98d2-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="e98d2-124">Para instalar esse pacote, adicione-o à coleção `DotNetCliToolReference` no arquivo *.csproj*, conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="e98d2-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="e98d2-125">**Observação:** esse pacote precisa ser instalado pela edição do arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e98d2-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="e98d2-126">O comando `install-package` ou a GUI do gerenciador de pacotes não pode ser usado para instalar esse pacote.</span><span class="sxs-lookup"><span data-stu-id="e98d2-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="e98d2-127">Edite o arquivo *.csproj* clicando com o botão direito do mouse no nome do projeto no **Gerenciador de Soluções** e selecionando **Editar ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="e98d2-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="e98d2-128">A seguinte marcação mostra o arquivo *.csproj* atualizado com as ferramentas da CLI do EF Core realçadas:</span><span class="sxs-lookup"><span data-stu-id="e98d2-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="e98d2-129">No exemplo anterior, os números de versão eram atuais no momento em que o tutorial foi escrito.</span><span class="sxs-lookup"><span data-stu-id="e98d2-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="e98d2-130">Use a mesma versão das ferramentas da CLI do EF Core encontrada nos outros pacotes.</span><span class="sxs-lookup"><span data-stu-id="e98d2-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="e98d2-131">Alterar a cadeia de conexão</span><span class="sxs-lookup"><span data-stu-id="e98d2-131">Change the connection string</span></span>

<span data-ttu-id="e98d2-132">No arquivo *appsettings.json*, altere o nome do BD na cadeia de conexão para ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="e98d2-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="e98d2-133">A alteração do nome do BD na cadeia de conexão faz com que a primeira migração crie um novo BD.</span><span class="sxs-lookup"><span data-stu-id="e98d2-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="e98d2-134">Um novo BD é criado porque um banco de dados com esse nome não existe.</span><span class="sxs-lookup"><span data-stu-id="e98d2-134">A new DB is created because one with that name doesn't exist.</span></span> <span data-ttu-id="e98d2-135">A alteração da cadeia de conexão não é necessária para começar a usar as migrações.</span><span class="sxs-lookup"><span data-stu-id="e98d2-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="e98d2-136">Uma alternativa à alteração do nome do BD é excluir o BD.</span><span class="sxs-lookup"><span data-stu-id="e98d2-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="e98d2-137">Use o **SSOX** (Pesquisador de Objetos do SQL Server) ou o comando `database drop` da CLI:</span><span class="sxs-lookup"><span data-stu-id="e98d2-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="e98d2-138">A seção a seguir explica como executar comandos da CLI.</span><span class="sxs-lookup"><span data-stu-id="e98d2-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="e98d2-139">Criar uma migração inicial</span><span class="sxs-lookup"><span data-stu-id="e98d2-139">Create an initial migration</span></span>

<span data-ttu-id="e98d2-140">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="e98d2-140">Build the project.</span></span>

<span data-ttu-id="e98d2-141">Abra uma janela Comando e navegue para a pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="e98d2-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="e98d2-142">A pasta do projeto contém o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e98d2-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="e98d2-143">Insira o seguinte na janela Comando:</span><span class="sxs-lookup"><span data-stu-id="e98d2-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="e98d2-144">A janela Comando exibe informações semelhantes às seguintes:</span><span class="sxs-lookup"><span data-stu-id="e98d2-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="e98d2-145">Se a migração falhar com a mensagem "*não é possível acessar o arquivo... ContosoUniversity.dll porque ele está sendo usado por outro processo.*"</span><span class="sxs-lookup"><span data-stu-id="e98d2-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="e98d2-146">será exibido:</span><span class="sxs-lookup"><span data-stu-id="e98d2-146">is displayed:</span></span>

* <span data-ttu-id="e98d2-147">Pare o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e98d2-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="e98d2-148">Saia e reinicie o Visual Studio ou</span><span class="sxs-lookup"><span data-stu-id="e98d2-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="e98d2-149">Encontre o ícone do IIS Express na Bandeja do Sistema do Windows.</span><span class="sxs-lookup"><span data-stu-id="e98d2-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="e98d2-150">Clique com o botão direito do mouse no ícone do IIS Express e, em seguida, clique em **ContosoUniversity > Parar Site**.</span><span class="sxs-lookup"><span data-stu-id="e98d2-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="e98d2-151">Se a mensagem de erro "Falha no build."</span><span class="sxs-lookup"><span data-stu-id="e98d2-151">If the error message "Build failed."</span></span> <span data-ttu-id="e98d2-152">for exibida, execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="e98d2-152">is displayed, run the command again.</span></span> <span data-ttu-id="e98d2-153">Se você receber esse erro, deixe uma observação na parte inferior deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="e98d2-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="e98d2-154">Examinar os métodos Up e Down</span><span class="sxs-lookup"><span data-stu-id="e98d2-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="e98d2-155">O comando `migrations add` do EF Core gerou um código do qual criar o BD.</span><span class="sxs-lookup"><span data-stu-id="e98d2-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="e98d2-156">Esse código de migrações está localizado no arquivo *Migrations\<timestamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="e98d2-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="e98d2-157">O método `Up` da classe `InitialCreate` cria as tabelas de BD que correspondem aos conjuntos de entidades do modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="e98d2-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="e98d2-158">O método `Down` exclui-os, conforme mostrado no seguinte exemplo:</span><span class="sxs-lookup"><span data-stu-id="e98d2-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="e98d2-159">As migrações chamam o método `Up` para implementar as alterações do modelo de dados para uma migração.</span><span class="sxs-lookup"><span data-stu-id="e98d2-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="e98d2-160">Quando você insere um comando para reverter a atualização, as migrações chamam o método `Down`.</span><span class="sxs-lookup"><span data-stu-id="e98d2-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="e98d2-161">O código anterior refere-se à migração inicial.</span><span class="sxs-lookup"><span data-stu-id="e98d2-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="e98d2-162">Esse código foi criado quando o comando `migrations add InitialCreate` foi executado.</span><span class="sxs-lookup"><span data-stu-id="e98d2-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="e98d2-163">O parâmetro de nome da migração ("InitialCreate" no exemplo) é usado para o nome do arquivo.</span><span class="sxs-lookup"><span data-stu-id="e98d2-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="e98d2-164">O nome da migração pode ser qualquer nome de arquivo válido.</span><span class="sxs-lookup"><span data-stu-id="e98d2-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="e98d2-165">É melhor escolher uma palavra ou frase que resume o que está sendo feito na migração.</span><span class="sxs-lookup"><span data-stu-id="e98d2-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="e98d2-166">Por exemplo, uma migração que adicionou uma tabela de departamento pode ser chamada "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="e98d2-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="e98d2-167">Se a migração inicial foi criada e o BD existe:</span><span class="sxs-lookup"><span data-stu-id="e98d2-167">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="e98d2-168">O código de criação do BD é gerado.</span><span class="sxs-lookup"><span data-stu-id="e98d2-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="e98d2-169">O código de criação do BD não precisa ser executado porque o BD já corresponde ao modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="e98d2-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="e98d2-170">Se o código de criação do BD for executado, ele não fará nenhuma alteração porque o BD já corresponde ao modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="e98d2-170">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="e98d2-171">Quando o aplicativo é implantado em um novo ambiente, o código de criação do BD precisa ser executado para criar o BD.</span><span class="sxs-lookup"><span data-stu-id="e98d2-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="e98d2-172">Anteriormente, a cadeia de conexão foi alterada para usar um novo nome para o BD.</span><span class="sxs-lookup"><span data-stu-id="e98d2-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="e98d2-173">O BD especificado não existe e, portanto, as migrações criam o BD.</span><span class="sxs-lookup"><span data-stu-id="e98d2-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="e98d2-174">O instantâneo do modelo de dados</span><span class="sxs-lookup"><span data-stu-id="e98d2-174">The data model snapshot</span></span>

<span data-ttu-id="e98d2-175">As migrações criam um *instantâneo* do esquema de banco de dados atual em *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="e98d2-175">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="e98d2-176">Quando você adiciona uma migração, o EF determina o que foi alterado, comparando o modelo de dados com o arquivo de instantâneo.</span><span class="sxs-lookup"><span data-stu-id="e98d2-176">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="e98d2-177">Ao excluir uma migração, use o comando [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="e98d2-177">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="e98d2-178">`dotnet ef migrations remove` exclui a migração e garante que o instantâneo seja redefinido corretamente.</span><span class="sxs-lookup"><span data-stu-id="e98d2-178">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="e98d2-179">Confira [Migrações do EF Core em ambientes de equipe](/ef/core/managing-schemas/migrations/teams) para obter mais informações de como o arquivo de instantâneo é usado.</span><span class="sxs-lookup"><span data-stu-id="e98d2-179">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="e98d2-180">Remover EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="e98d2-180">Remove EnsureCreated</span></span>

<span data-ttu-id="e98d2-181">Para o desenvolvimento inicial, o comando `EnsureCreated` foi usado.</span><span class="sxs-lookup"><span data-stu-id="e98d2-181">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="e98d2-182">Neste tutorial, as migrações são usadas.</span><span class="sxs-lookup"><span data-stu-id="e98d2-182">In this tutorial, migrations is used.</span></span> <span data-ttu-id="e98d2-183">`EnsureCreated` tem as seguintes limitações:</span><span class="sxs-lookup"><span data-stu-id="e98d2-183">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="e98d2-184">Ignora as migrações e cria o BD e o esquema.</span><span class="sxs-lookup"><span data-stu-id="e98d2-184">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="e98d2-185">Não cria uma tabela de migrações.</span><span class="sxs-lookup"><span data-stu-id="e98d2-185">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="e98d2-186">*Não* pode ser usado com migrações.</span><span class="sxs-lookup"><span data-stu-id="e98d2-186">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="e98d2-187">Foi projetado para teste ou criação rápida de protótipos em que o BD é removido e recriado com frequência.</span><span class="sxs-lookup"><span data-stu-id="e98d2-187">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="e98d2-188">Remova a seguinte linha de `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="e98d2-188">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="e98d2-189">Aplicar a migração ao BD em desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="e98d2-189">Apply the migration to the DB in development</span></span>

<span data-ttu-id="e98d2-190">Na janela Comando, insira o seguinte para criar o BD e as tabelas.</span><span class="sxs-lookup"><span data-stu-id="e98d2-190">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="e98d2-191">Observação: se o comando `update` retornar o erro "Falha no build":</span><span class="sxs-lookup"><span data-stu-id="e98d2-191">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="e98d2-192">Execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="e98d2-192">Run the command again.</span></span>
* <span data-ttu-id="e98d2-193">Se ele falhar novamente, saia do Visual Studio e, em seguida, execute o comando `update`.</span><span class="sxs-lookup"><span data-stu-id="e98d2-193">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="e98d2-194">Deixe uma mensagem na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="e98d2-194">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="e98d2-195">A saída do comando é semelhante à saída de comando `migrations add`.</span><span class="sxs-lookup"><span data-stu-id="e98d2-195">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="e98d2-196">No comando anterior, os logs para os comandos SQL que configuraram o BD são exibidos.</span><span class="sxs-lookup"><span data-stu-id="e98d2-196">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="e98d2-197">A maioria dos logs é omitida na seguinte saída de exemplo:</span><span class="sxs-lookup"><span data-stu-id="e98d2-197">Most of the logs are omitted in the following sample output:</span></span>

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

<span data-ttu-id="e98d2-198">Para reduzir o nível de detalhes em mensagens de log, altere os níveis de log no arquivo *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="e98d2-198">To reduce the level of detail in log messages, change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="e98d2-199">Para obter mais informações, consulte [Introdução ao log](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="e98d2-199">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="e98d2-200">Use o **Pesquisador de Objetos do SQL Server** para inspecionar o BD.</span><span class="sxs-lookup"><span data-stu-id="e98d2-200">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="e98d2-201">Observe a adição de uma tabela `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="e98d2-201">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="e98d2-202">A tabela `__EFMigrationsHistory` controla quais migrações foram aplicadas ao BD.</span><span class="sxs-lookup"><span data-stu-id="e98d2-202">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="e98d2-203">Exiba os dados na `__EFMigrationsHistory` tabela; ela mostra uma linha para a primeira migração.</span><span class="sxs-lookup"><span data-stu-id="e98d2-203">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="e98d2-204">O último log no exemplo de saída da CLI anterior mostra a instrução INSERT que cria essa linha.</span><span class="sxs-lookup"><span data-stu-id="e98d2-204">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="e98d2-205">Execute o aplicativo e verifique se tudo funciona.</span><span class="sxs-lookup"><span data-stu-id="e98d2-205">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="e98d2-206">Aplicando migrações na produção</span><span class="sxs-lookup"><span data-stu-id="e98d2-206">Applying migrations in production</span></span>

<span data-ttu-id="e98d2-207">Recomendamos que os aplicativos de produção **não** chamem [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) na inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e98d2-207">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="e98d2-208">`Migrate` não deve ser chamado em um aplicativo no farm de servidores.</span><span class="sxs-lookup"><span data-stu-id="e98d2-208">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="e98d2-209">Por exemplo, se o aplicativo foi implantado na nuvem com escalabilidade horizontal (várias instâncias do aplicativo estão sendo executadas).</span><span class="sxs-lookup"><span data-stu-id="e98d2-209">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="e98d2-210">A migração de banco de dados deve ser feita como parte da implantação e de maneira controlada.</span><span class="sxs-lookup"><span data-stu-id="e98d2-210">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="e98d2-211">Abordagens de migração de banco de dados de produção incluem:</span><span class="sxs-lookup"><span data-stu-id="e98d2-211">Production database migration approaches include:</span></span>

* <span data-ttu-id="e98d2-212">Uso de migrações para criar scripts SQL e uso dos scripts SQL na implantação.</span><span class="sxs-lookup"><span data-stu-id="e98d2-212">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="e98d2-213">Execução de `dotnet ef database update` em um ambiente controlado.</span><span class="sxs-lookup"><span data-stu-id="e98d2-213">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="e98d2-214">O EF Core usa a tabela `__MigrationsHistory` para ver se uma migração precisa ser executada.</span><span class="sxs-lookup"><span data-stu-id="e98d2-214">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="e98d2-215">Se o BD estiver atualizado, nenhuma migração será executada.</span><span class="sxs-lookup"><span data-stu-id="e98d2-215">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="e98d2-216">CLI (interface de linha de comando) vs. PMC (Console do Gerenciador de Pacotes)</span><span class="sxs-lookup"><span data-stu-id="e98d2-216">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="e98d2-217">As ferramentas do EF Core para o gerenciamento de migrações estão disponíveis em:</span><span class="sxs-lookup"><span data-stu-id="e98d2-217">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="e98d2-218">Comandos da CLI do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e98d2-218">.NET Core CLI commands.</span></span>
* <span data-ttu-id="e98d2-219">Os cmdlets do PowerShell na janela do **PMC** (Console do Gerenciador de Pacotes) no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e98d2-219">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="e98d2-220">Este tutorial mostra como usar a CLI; alguns desenvolvedores preferem usar o PMC.</span><span class="sxs-lookup"><span data-stu-id="e98d2-220">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="e98d2-221">Os comandos do EF Core para o PMC estão no pacote [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools).</span><span class="sxs-lookup"><span data-stu-id="e98d2-221">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="e98d2-222">Este pacote está incluído no metapacote [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) e, portanto, não é necessário instalá-lo.</span><span class="sxs-lookup"><span data-stu-id="e98d2-222">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="e98d2-223">**Importante:** esse não é o mesmo pacote que é instalado para a CLI com a edição do arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e98d2-223">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="e98d2-224">O nome deste termina com `Tools`, ao contrário do nome do pacote da CLI que termina com `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="e98d2-224">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="e98d2-225">Para obter mais informações sobre os comandos da CLI, consulte [CLI do .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="e98d2-225">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="e98d2-226">Para obter mais informações sobre os comandos do PMC, consulte [Console do Gerenciador de Pacotes (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="e98d2-226">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="e98d2-227">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="e98d2-227">Troubleshooting</span></span>

<span data-ttu-id="e98d2-228">Baixe o [aplicativo concluído para este estágio](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="e98d2-228">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="e98d2-229">O aplicativo gera a seguinte exceção:</span><span class="sxs-lookup"><span data-stu-id="e98d2-229">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="e98d2-230">Solução: execute `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="e98d2-230">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="e98d2-231">Se o comando `update` retornar o erro "Falha no build":</span><span class="sxs-lookup"><span data-stu-id="e98d2-231">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="e98d2-232">Execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="e98d2-232">Run the command again.</span></span>
* <span data-ttu-id="e98d2-233">Deixe uma mensagem na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="e98d2-233">Leave a message at the bottom of the page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e98d2-234">[Anterior](xref:data/ef-rp/sort-filter-page)
> [Próximo](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="e98d2-234">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
