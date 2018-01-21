---
title: "Páginas Razor com núcleo EF - Migrations - 4 de 8"
author: rick-anderson
description: "Neste tutorial, você começar a usar o recurso de migrações EF principal para gerenciar as alterações do modelo de dados em um aplicativo MVC do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 26fbda99b0c1dfa2d09cf387e43f3123c58215f8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a><span data-ttu-id="886e2-103">Migrações - Core EF com tutorial páginas Razor (4 de 8)</span><span class="sxs-lookup"><span data-stu-id="886e2-103">Migrations - EF Core with Razor Pages tutorial (4 of 8)</span></span>

<span data-ttu-id="886e2-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="886e2-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="886e2-105">Neste tutorial, o recurso de migrações de EF principal para o gerenciamento de alterações do modelo de dados é usado.</span><span class="sxs-lookup"><span data-stu-id="886e2-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="886e2-106">Se você tiver problemas, você não conseguir resolver, baixe o [aplicativo concluído para este estágio](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="886e2-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="886e2-107">Quando um novo aplicativo é desenvolvido, os modelo de dados de alterações com frequência.</span><span class="sxs-lookup"><span data-stu-id="886e2-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="886e2-108">Cada vez que as alterações do modelo, o modelo obtém fora de sincronia com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="886e2-109">Este tutorial Introdução ao configurar o Entity Framework para criar o banco de dados se ele não existir.</span><span class="sxs-lookup"><span data-stu-id="886e2-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="886e2-110">Cada vez que os dados de alterações do modelo:</span><span class="sxs-lookup"><span data-stu-id="886e2-110">Each time the data model changes:</span></span>

* <span data-ttu-id="886e2-111">O banco de dados é descartado.</span><span class="sxs-lookup"><span data-stu-id="886e2-111">The DB is dropped.</span></span>
* <span data-ttu-id="886e2-112">EF cria um novo que corresponde ao modelo.</span><span class="sxs-lookup"><span data-stu-id="886e2-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="886e2-113">O aplicativo propaga o banco de dados com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="886e2-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="886e2-114">Essa abordagem para manter o banco de dados em sincronia com o modelo de dados funciona bem até que você implantar o aplicativo para produção.</span><span class="sxs-lookup"><span data-stu-id="886e2-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="886e2-115">Quando o aplicativo é executado em produção, normalmente ele está armazenando dados que precisam ser mantidas.</span><span class="sxs-lookup"><span data-stu-id="886e2-115">When the app is running in production, it is usually storing data that needs to be maintained.</span></span> <span data-ttu-id="886e2-116">O aplicativo não pode começar com um teste de banco de dados sempre que uma alteração é feita (como adicionar uma nova coluna).</span><span class="sxs-lookup"><span data-stu-id="886e2-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="886e2-117">O recurso de migrações de núcleo EF resolve esse problema, permitindo que o EF principal atualizar o esquema de banco de dados em vez de criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="886e2-118">Em vez de descartar e recriar o banco de dados quando os dados de alterações do modelo, migrações atualiza o esquema e mantém os dados existentes.</span><span class="sxs-lookup"><span data-stu-id="886e2-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="886e2-119">Pacotes NuGet do Entity Framework Core para migrações</span><span class="sxs-lookup"><span data-stu-id="886e2-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="886e2-120">Para trabalhar com migrações, use o **Package Manager Console** (PMC) ou a interface de linha de comando (CLI).</span><span class="sxs-lookup"><span data-stu-id="886e2-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="886e2-121">Esses tutoriais mostram como usar comandos CLI.</span><span class="sxs-lookup"><span data-stu-id="886e2-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="886e2-122">Informações sobre o PMC estão no [o fim deste tutorial](#pmc).</span><span class="sxs-lookup"><span data-stu-id="886e2-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="886e2-123">As ferramentas de EF principal para a interface de linha de comando (CLI) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="886e2-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="886e2-124">Para instalar este pacote, adicione-o para o `DotNetCliToolReference` coleção no *. csproj* de arquivo, como mostrado.</span><span class="sxs-lookup"><span data-stu-id="886e2-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="886e2-125">**Observação:** este pacote deve ser instalado, editando o *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="886e2-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="886e2-126">O`install-package` comando ou a GUI do Gerenciador de pacote não pode ser usado para instalar este pacote.</span><span class="sxs-lookup"><span data-stu-id="886e2-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="886e2-127">Editar o *. csproj* arquivo clicando com o nome do projeto no **Solution Explorer** e selecionando **ContosoUniversity.csproj editar**.</span><span class="sxs-lookup"><span data-stu-id="886e2-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="886e2-128">A marcação a seguir mostra a atualização *. csproj* arquivo com as ferramentas de EF Core CLI realçado:</span><span class="sxs-lookup"><span data-stu-id="886e2-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="886e2-129">No exemplo anterior, os números de versão foram atuais quando o tutorial foi escrito.</span><span class="sxs-lookup"><span data-stu-id="886e2-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="886e2-130">Use a mesma versão das ferramentas de EF Core CLI encontrado nos outros pacotes.</span><span class="sxs-lookup"><span data-stu-id="886e2-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="886e2-131">Altere a cadeia de conexão</span><span class="sxs-lookup"><span data-stu-id="886e2-131">Change the connection string</span></span>

<span data-ttu-id="886e2-132">No *appSettings. JSON* de arquivo, altere o nome do banco de dados na cadeia de conexão para ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="886e2-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="886e2-133">Alterar o nome do banco de dados na cadeia de conexão faz com que a migração primeiro criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="886e2-134">Um novo banco de dados é criado como uma com esse nome não existe.</span><span class="sxs-lookup"><span data-stu-id="886e2-134">A new DB is created because one with that name does not exist.</span></span> <span data-ttu-id="886e2-135">Alterando a cadeia de conexão não é necessário para começar a trabalhar com migrações.</span><span class="sxs-lookup"><span data-stu-id="886e2-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="886e2-136">Uma alternativa para alterar o nome do banco de dados está excluindo o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="886e2-137">Use **Pesquisador de objetos do SQL Server** (SSOX) ou o `database drop` comando CLI:</span><span class="sxs-lookup"><span data-stu-id="886e2-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="886e2-138">A seção a seguir explica como executar comandos CLI.</span><span class="sxs-lookup"><span data-stu-id="886e2-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="886e2-139">Criar uma migração inicial</span><span class="sxs-lookup"><span data-stu-id="886e2-139">Create an initial migration</span></span>

<span data-ttu-id="886e2-140">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="886e2-140">Build the project.</span></span>

<span data-ttu-id="886e2-141">Abra uma janela de comando e navegue até a pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="886e2-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="886e2-142">A pasta do projeto contém o *Startup.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="886e2-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="886e2-143">Digite o seguinte na janela de comando:</span><span class="sxs-lookup"><span data-stu-id="886e2-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="886e2-144">A janela de comando exibe informações semelhantes ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="886e2-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="886e2-145">Se a migração falhar com a mensagem "*não é possível acessar o arquivo... ContosoUniversity.dll porque ele está sendo usado por outro processo.* "</span><span class="sxs-lookup"><span data-stu-id="886e2-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="886e2-146">é exibida:</span><span class="sxs-lookup"><span data-stu-id="886e2-146">is displayed:</span></span>

* <span data-ttu-id="886e2-147">Parar o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="886e2-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="886e2-148">Saia e reinicie o Visual Studio, ou</span><span class="sxs-lookup"><span data-stu-id="886e2-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="886e2-149">Localize o IIS Express ícone na bandeja do sistema do Windows.</span><span class="sxs-lookup"><span data-stu-id="886e2-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="886e2-150">Clique no ícone do IIS Express e, em seguida, clique em **ContosoUniversity > Parar Site**.</span><span class="sxs-lookup"><span data-stu-id="886e2-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="886e2-151">Se a mensagem de erro "Falha na compilação."</span><span class="sxs-lookup"><span data-stu-id="886e2-151">If the error message "Build failed."</span></span> <span data-ttu-id="886e2-152">é exibido, execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="886e2-152">is displayed, run the command again.</span></span> <span data-ttu-id="886e2-153">Se você receber esse erro, deixe uma nota na parte inferior deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="886e2-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="886e2-154">Examine cima e para baixo métodos</span><span class="sxs-lookup"><span data-stu-id="886e2-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="886e2-155">O comando Core EF `migrations add` gerado código para criar o banco de dados do.</span><span class="sxs-lookup"><span data-stu-id="886e2-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="886e2-156">Esse código de migrações está no *migrações\<timestamp > _InitialCreate.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="886e2-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="886e2-157">O `Up` método o `InitialCreate` classe cria as tabelas de banco de dados que correspondem aos conjuntos de entidades de modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="886e2-158">O `Down` método exclui-los, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="886e2-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="886e2-159">Chamadas de migrações de `Up` método para implementar as alterações do modelo de dados para uma migração.</span><span class="sxs-lookup"><span data-stu-id="886e2-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="886e2-160">Quando você insere um comando para reverter a atualização, chamadas de migrações de `Down` método.</span><span class="sxs-lookup"><span data-stu-id="886e2-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="886e2-161">O código anterior é para a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="886e2-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="886e2-162">Esse código foi criado quando o `migrations add InitialCreate` comando foi executado.</span><span class="sxs-lookup"><span data-stu-id="886e2-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="886e2-163">O parâmetro de nome de migração ("InitialCreate" no exemplo) é usado para o nome do arquivo.</span><span class="sxs-lookup"><span data-stu-id="886e2-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="886e2-164">O nome de migração pode ser qualquer nome de arquivo válido.</span><span class="sxs-lookup"><span data-stu-id="886e2-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="886e2-165">É melhor escolher uma palavra ou frase que resume o que está sendo feito a migração.</span><span class="sxs-lookup"><span data-stu-id="886e2-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="886e2-166">Por exemplo, uma migração que adicionou uma tabela de departamento pode ser chamada "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="886e2-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="886e2-167">Se a migração inicial é criada e o banco de dados será encerrado:</span><span class="sxs-lookup"><span data-stu-id="886e2-167">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="886e2-168">O código de criação do banco de dados é gerado.</span><span class="sxs-lookup"><span data-stu-id="886e2-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="886e2-169">O código de criação do banco de dados não precisa ser executado porque o banco de dados já coincide com o modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="886e2-170">Se o código de criação do banco de dados for executado, ele não faz quaisquer alterações porque o banco de dados já coincide com o modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-170">If the The DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="886e2-171">Quando o aplicativo é implantado para um novo ambiente, o código de criação do banco de dados deve ser executado para criar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="886e2-172">Anteriormente, a cadeia de caracteres de conexão foi alterada para usar um novo nome para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="886e2-173">O banco de dados especificado não existe, migrações cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="886e2-174">Examine o instantâneo do modelo de dados</span><span class="sxs-lookup"><span data-stu-id="886e2-174">Examine the data model snapshot</span></span>

<span data-ttu-id="886e2-175">Migrações cria um *instantâneo* do esquema de banco de dados atual em *Migrations/SchoolContextModelSnapshot.cs*:</span><span class="sxs-lookup"><span data-stu-id="886e2-175">Migrations creates a *snapshot* of the current DB schema in *Migrations/SchoolContextModelSnapshot.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="886e2-176">Porque o esquema de banco de dados atual é representado no código, Core EF não precisa interagir com o banco de dados para criar as migrações.</span><span class="sxs-lookup"><span data-stu-id="886e2-176">Because the current DB schema is represented in code, EF Core doesn't have to interact with the DB to create migrations.</span></span> <span data-ttu-id="886e2-177">Quando você adiciona uma migração, Core EF determina o que mudou, comparando o modelo de dados para o arquivo de instantâneo.</span><span class="sxs-lookup"><span data-stu-id="886e2-177">When you add a migration, EF Core determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="886e2-178">EF principal interage com o banco de dados somente quando é necessário atualizar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-178">EF Core interacts with the DB only when it has to update the DB.</span></span>

<span data-ttu-id="886e2-179">O arquivo de instantâneo deve ser sincronizado com as migrações que o criou.</span><span class="sxs-lookup"><span data-stu-id="886e2-179">The snapshot file must be in sync with the migrations that created it.</span></span> <span data-ttu-id="886e2-180">A migração não pode ser removida, excluindo o arquivo chamado  *\<timestamp > _\<migrationname >. CS*.</span><span class="sxs-lookup"><span data-stu-id="886e2-180">A migration can't be removed by deleting the file named *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="886e2-181">Se esse arquivo for excluído, as migrações restantes estão fora de sincronia com o arquivo de instantâneo do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-181">If that file is deleted, the remaining migrations are out of sync with the DB snapshot file.</span></span> <span data-ttu-id="886e2-182">Para excluir a última migração adicionada, use o [remover migrações de ef dotnet](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) comando.</span><span class="sxs-lookup"><span data-stu-id="886e2-182">To delete the last migration added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="886e2-183">Remover EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="886e2-183">Remove EnsureCreated</span></span>

<span data-ttu-id="886e2-184">Para o desenvolvimento inicial, o `EnsureCreated` comando foi usado.</span><span class="sxs-lookup"><span data-stu-id="886e2-184">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="886e2-185">Neste tutorial, migrações é usado.</span><span class="sxs-lookup"><span data-stu-id="886e2-185">In this tutorial, migrations is used.</span></span> <span data-ttu-id="886e2-186">`EnsureCreated`tem os seguintes limatitions:</span><span class="sxs-lookup"><span data-stu-id="886e2-186">`EnsureCreated` has the following limatitions:</span></span>

* <span data-ttu-id="886e2-187">Ignora as migrações e cria o banco de dados e o esquema.</span><span class="sxs-lookup"><span data-stu-id="886e2-187">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="886e2-188">Não crie uma tabela de migrações.</span><span class="sxs-lookup"><span data-stu-id="886e2-188">Does not create a migrations table.</span></span>
* <span data-ttu-id="886e2-189">Pode *não* ser usado com migrações.</span><span class="sxs-lookup"><span data-stu-id="886e2-189">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="886e2-190">Foi projetado para teste ou rápido de protótipos onde o banco de dados é descartado e recriado com frequência.</span><span class="sxs-lookup"><span data-stu-id="886e2-190">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="886e2-191">Remova a seguinte linha de `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="886e2-191">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="886e2-192">Aplicar a migração para o banco de dados em desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="886e2-192">Apply the migration to the DB in development</span></span>

<span data-ttu-id="886e2-193">Na janela de comando, digite o seguinte para criar o banco de dados e tabelas.</span><span class="sxs-lookup"><span data-stu-id="886e2-193">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="886e2-194">Observação: Se o `update` comando retorna o erro "Falha na compilação.":</span><span class="sxs-lookup"><span data-stu-id="886e2-194">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="886e2-195">Execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="886e2-195">Run the command again.</span></span>
* <span data-ttu-id="886e2-196">Se ele falhar novamente, sair do Visual Studio e, em seguida, execute o `update` comando.</span><span class="sxs-lookup"><span data-stu-id="886e2-196">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="886e2-197">Deixe uma mensagem na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="886e2-197">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="886e2-198">A saída do comando é semelhante do `migrations add` saída de comando.</span><span class="sxs-lookup"><span data-stu-id="886e2-198">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="886e2-199">No comando anterior, os logs para os comandos SQL que configurar o banco de dados são exibidos.</span><span class="sxs-lookup"><span data-stu-id="886e2-199">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="886e2-200">A maioria dos logs é omitida na saída de exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="886e2-200">Most of the logs are omitted in the following sample output:</span></span>

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

<span data-ttu-id="886e2-201">Para reduzir o nível de detalhes em mensagens de log, pode alterar os níveis de log no *appsettings. Development.JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="886e2-201">To reduce the level of detail in log messages, can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="886e2-202">Para obter mais informações, consulte [Introdução ao registro em log](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="886e2-202">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="886e2-203">Use **Pesquisador de objetos do SQL Server** para inspecionar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-203">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="886e2-204">Observe a adição de um `__EFMigrationsHistory` tabela.</span><span class="sxs-lookup"><span data-stu-id="886e2-204">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="886e2-205">O `__EFMigrationsHistory` tabela mantém o controle de quais migrações foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="886e2-205">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="886e2-206">Exibir os dados de `__EFMigrationsHistory` tabela, ele mostra uma linha para a migração primeiro.</span><span class="sxs-lookup"><span data-stu-id="886e2-206">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="886e2-207">O último log no exemplo de saída CLI anterior mostra a instrução INSERT que cria essa linha.</span><span class="sxs-lookup"><span data-stu-id="886e2-207">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="886e2-208">Executar o aplicativo e verificar se tudo funciona.</span><span class="sxs-lookup"><span data-stu-id="886e2-208">Run the app and verify that everything works.</span></span>

## <a name="appling-migrations-in-production"></a><span data-ttu-id="886e2-209">Migrações de appling em produção</span><span class="sxs-lookup"><span data-stu-id="886e2-209">Appling migrations in production</span></span>

<span data-ttu-id="886e2-210">É recomendável que os aplicativos de produção devem **não** chamar [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) na inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="886e2-210">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="886e2-211">`Migrate`não deve ser chamado de um aplicativo no farm de servidores.</span><span class="sxs-lookup"><span data-stu-id="886e2-211">`Migrate` should not be called from an app in server farm.</span></span> <span data-ttu-id="886e2-212">Por exemplo, se o aplicativo foi implantado com expansão (várias instâncias do aplicativo estão executando) de nuvem.</span><span class="sxs-lookup"><span data-stu-id="886e2-212">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="886e2-213">Migração do banco de dados deve ser feita como parte da implantação e de maneira controlada.</span><span class="sxs-lookup"><span data-stu-id="886e2-213">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="886e2-214">Abordagens de migração do banco de dados de produção incluem:</span><span class="sxs-lookup"><span data-stu-id="886e2-214">Production database migration approaches include:</span></span>

* <span data-ttu-id="886e2-215">Uso de migrações para criar scripts SQL e usando os scripts SQL na implantação.</span><span class="sxs-lookup"><span data-stu-id="886e2-215">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="886e2-216">Executando `dotnet ef database update` de um ambiente controlado.</span><span class="sxs-lookup"><span data-stu-id="886e2-216">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="886e2-217">Núcleo EF usa o `__MigrationsHistory` tabela para ver se quaisquer migrações precisam executar.</span><span class="sxs-lookup"><span data-stu-id="886e2-217">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="886e2-218">Se o banco de dados é atualizado, nenhuma migração é executada.</span><span class="sxs-lookup"><span data-stu-id="886e2-218">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="886e2-219">Interface de linha de comando (CLI) vs. Package Manager Console (PMC)</span><span class="sxs-lookup"><span data-stu-id="886e2-219">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="886e2-220">O núcleo de EF ferramentas para gerenciar migrações está disponível em:</span><span class="sxs-lookup"><span data-stu-id="886e2-220">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="886e2-221">Comandos de CLI do .NET core.</span><span class="sxs-lookup"><span data-stu-id="886e2-221">.NET Core CLI commands.</span></span>
* <span data-ttu-id="886e2-222">Os cmdlets do PowerShell no Visual Studio **Package Manager Console** janela (PMC).</span><span class="sxs-lookup"><span data-stu-id="886e2-222">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="886e2-223">Este tutorial mostra como usar a CLI, alguns desenvolvedores preferem usando o PMC.</span><span class="sxs-lookup"><span data-stu-id="886e2-223">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="886e2-224">Os comandos de EF principal para o PMC estão no [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pacote.</span><span class="sxs-lookup"><span data-stu-id="886e2-224">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="886e2-225">Este pacote está incluído no [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, portanto não é necessário instalá-lo.</span><span class="sxs-lookup"><span data-stu-id="886e2-225">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="886e2-226">**Importante:** isso não é o mesmo pacote que você instala para CLI editando o *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="886e2-226">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="886e2-227">O nome deste termina `Tools`, ao contrário do nome do pacote CLI que termina em `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="886e2-227">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="886e2-228">Para obter mais informações sobre os comandos CLI, consulte [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="886e2-228">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="886e2-229">Para obter mais informações sobre os comandos PMC, consulte [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="886e2-229">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="886e2-230">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="886e2-230">Troubleshooting</span></span>

<span data-ttu-id="886e2-231">Baixe o [aplicativo concluído para este estágio](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="886e2-231">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="886e2-232">O aplicativo gera a seguinte exceção:</span><span class="sxs-lookup"><span data-stu-id="886e2-232">The app generates the following exception:</span></span>

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="886e2-233">Solução: execute`dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="886e2-233">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="886e2-234">Se o `update` comando retorna o erro "Falha na compilação.":</span><span class="sxs-lookup"><span data-stu-id="886e2-234">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="886e2-235">Execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="886e2-235">Run the command again.</span></span>
* <span data-ttu-id="886e2-236">Deixe uma mensagem na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="886e2-236">Leave a message at the bottom of the page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="886e2-237">[Anterior](xref:data/ef-rp/sort-filter-page)
[Próximo](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="886e2-237">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
