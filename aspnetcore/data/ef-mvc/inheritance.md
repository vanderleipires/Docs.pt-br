---
title: ASP.NET Core MVC com EF Core – herança – 9 de 10
author: rick-anderson
description: Este tutorial mostrará como implementar a herança no modelo de dados, usando o Entity Framework Core em um aplicativo ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 1ddca97d0a68311e8c6fa793ec4245c7ffef1337
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153427"
---
# <a name="aspnet-core-mvc-with-ef-core---inheritance---9-of-10"></a><span data-ttu-id="c404c-103">ASP.NET Core MVC com EF Core – herança – 9 de 10</span><span class="sxs-lookup"><span data-stu-id="c404c-103">ASP.NET Core MVC with EF Core - Inheritance - 9 of 10</span></span>

<span data-ttu-id="c404c-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c404c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c404c-105">O aplicativo web de exemplo Contoso University demonstra como criar aplicativos web do ASP.NET Core MVC usando o Entity Framework Core e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c404c-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="c404c-106">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="c404c-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="c404c-107">No tutorial anterior, você tratou exceções de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="c404c-107">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="c404c-108">Este tutorial mostrará como implementar a herança no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="c404c-108">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="c404c-109">Na programação orientada a objeto, você pode usar a herança para facilitar a reutilização de código.</span><span class="sxs-lookup"><span data-stu-id="c404c-109">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="c404c-110">Neste tutorial, você alterará as classes `Instructor` e `Student`, de modo que elas derivem de uma classe base `Person` que contém propriedades, como `LastName`, comuns a instrutores e alunos.</span><span class="sxs-lookup"><span data-stu-id="c404c-110">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="c404c-111">Você não adicionará nem alterará as páginas da Web, mas alterará uma parte do código, e essas alterações serão refletidas automaticamente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c404c-111">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="c404c-112">Opções para o mapeamento de herança para as tabelas de banco de dados</span><span class="sxs-lookup"><span data-stu-id="c404c-112">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="c404c-113">As classes `Instructor` e `Student` no modelo de dados Escola têm várias propriedades idênticas:</span><span class="sxs-lookup"><span data-stu-id="c404c-113">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Classes Student e Instructor](inheritance/_static/no-inheritance.png)

<span data-ttu-id="c404c-115">Suponha que você deseje eliminar o código redundante para as propriedades compartilhadas pelas entidades `Instructor` e `Student`.</span><span class="sxs-lookup"><span data-stu-id="c404c-115">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="c404c-116">Ou você deseja escrever um serviço que pode formatar nomes sem se preocupar se o nome foi obtido de um instrutor ou um aluno.</span><span class="sxs-lookup"><span data-stu-id="c404c-116">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="c404c-117">Você pode criar uma classe base `Person` que contém apenas essas propriedades compartilhadas e, em seguida, fazer com que as classes `Instructor` e `Student` herdem dessa classe base, conforme mostrado na seguinte ilustração:</span><span class="sxs-lookup"><span data-stu-id="c404c-117">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Classes Student e Instructor derivando da classe Person](inheritance/_static/inheritance.png)

<span data-ttu-id="c404c-119">Há várias maneiras pelas quais essa estrutura de herança pode ser representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c404c-119">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="c404c-120">Você pode ter uma tabela Person que inclui informações sobre alunos e instrutores em uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="c404c-120">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="c404c-121">Algumas das colunas podem se aplicar somente a instrutores (HireDate), algumas somente a alunos (EnrollmentDate) e outras a ambos (LastName, FirstName).</span><span class="sxs-lookup"><span data-stu-id="c404c-121">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="c404c-122">Normalmente, você terá uma coluna discriminatória para indicar qual tipo cada linha representa.</span><span class="sxs-lookup"><span data-stu-id="c404c-122">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="c404c-123">Por exemplo, a coluna discriminatória pode ter "Instrutor" para instrutores e "Aluno" para alunos.</span><span class="sxs-lookup"><span data-stu-id="c404c-123">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Exemplo de tabela por hierarquia](inheritance/_static/tph.png)

<span data-ttu-id="c404c-125">Esse padrão de geração de uma estrutura de herança de entidade com base em uma tabela de banco de dados individual é chamado de herança TPH (tabela por hierarquia).</span><span class="sxs-lookup"><span data-stu-id="c404c-125">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="c404c-126">Uma alternativa é fazer com que o banco de dados se pareça mais com a estrutura de herança.</span><span class="sxs-lookup"><span data-stu-id="c404c-126">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="c404c-127">Por exemplo, você pode ter apenas os campos de nome na tabela Pessoa e as tabelas separadas Instrutor e Aluno com os campos de data.</span><span class="sxs-lookup"><span data-stu-id="c404c-127">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Herança de tabela por tipo](inheritance/_static/tpt.png)

<span data-ttu-id="c404c-129">Esse padrão de criação de uma tabela de banco de dados para cada classe de entidade é chamado de herança TPT (tabela por tipo).</span><span class="sxs-lookup"><span data-stu-id="c404c-129">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="c404c-130">Outra opção é mapear todos os tipos não abstratos para tabelas individuais.</span><span class="sxs-lookup"><span data-stu-id="c404c-130">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="c404c-131">Todas as propriedades de uma classe, incluindo propriedades herdadas, são mapeadas para colunas da tabela correspondente.</span><span class="sxs-lookup"><span data-stu-id="c404c-131">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="c404c-132">Esse padrão é chamado de herança TPC (Tabela por Classe Concreta).</span><span class="sxs-lookup"><span data-stu-id="c404c-132">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="c404c-133">Se você tiver implementado a herança TPC para as classes Person, Student e Instructor, conforme mostrado anteriormente, as tabelas Aluno e Instrutor não parecerão diferentes após a implementação da herança do que antes.</span><span class="sxs-lookup"><span data-stu-id="c404c-133">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="c404c-134">Em geral, os padrões de herança TPC e TPH oferecem melhor desempenho do que os padrões de herança TPT, porque os padrões TPT podem resultar em consultas de junção complexas.</span><span class="sxs-lookup"><span data-stu-id="c404c-134">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="c404c-135">Este tutorial demonstra como implementar a herança TPH.</span><span class="sxs-lookup"><span data-stu-id="c404c-135">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="c404c-136">TPH é o único padrão de herança compatível com o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c404c-136">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="c404c-137">O que você fará é criar uma classe `Person`, alterar as classes `Instructor` e `Student` para que elas derivem de `Person`, adicionar a nova classe ao `DbContext` e criar uma migração.</span><span class="sxs-lookup"><span data-stu-id="c404c-137">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP] 
> <span data-ttu-id="c404c-138">Considere a possibilidade de salvar uma cópia do projeto antes de fazer as alterações a seguir.</span><span class="sxs-lookup"><span data-stu-id="c404c-138">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="c404c-139">Em seguida, se você tiver problemas e precisar recomeçar, será mais fácil começar do projeto salvo, em vez de reverter as etapas executadas para este tutorial ou voltar ao início da série inteira.</span><span class="sxs-lookup"><span data-stu-id="c404c-139">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="c404c-140">Criar a classe Person</span><span class="sxs-lookup"><span data-stu-id="c404c-140">Create the Person class</span></span>

<span data-ttu-id="c404c-141">Na pasta Models, crie Person.cs e substitua o código de modelo pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="c404c-141">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="c404c-142">Fazer com que as classes Student e Instructor herdem de Person</span><span class="sxs-lookup"><span data-stu-id="c404c-142">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="c404c-143">Em *Instructor.cs*, derive a classe Instructor da classe Person e remova os campos de nome e chave.</span><span class="sxs-lookup"><span data-stu-id="c404c-143">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="c404c-144">O código será semelhante ao seguinte exemplo:</span><span class="sxs-lookup"><span data-stu-id="c404c-144">The code will look like the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="c404c-145">Faça as mesmas alterações em *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="c404c-145">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="c404c-146">Adicionar o tipo de entidade Person ao modelo de dados</span><span class="sxs-lookup"><span data-stu-id="c404c-146">Add the Person entity type to the data model</span></span>

<span data-ttu-id="c404c-147">Adicione o tipo de entidade Person a *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="c404c-147">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="c404c-148">As novas linhas são realçadas.</span><span class="sxs-lookup"><span data-stu-id="c404c-148">The new lines are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="c404c-149">Isso é tudo o que o Entity Framework precisa para configurar a herança de tabela por hierarquia.</span><span class="sxs-lookup"><span data-stu-id="c404c-149">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="c404c-150">Como você verá, quando o banco de dados for atualizado, ele terá uma tabela Pessoa no lugar das tabelas Aluno e Instrutor.</span><span class="sxs-lookup"><span data-stu-id="c404c-150">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="c404c-151">Criar e personalizar o código de migração</span><span class="sxs-lookup"><span data-stu-id="c404c-151">Create and customize migration code</span></span>

<span data-ttu-id="c404c-152">Salve as alterações e compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="c404c-152">Save your changes and build the project.</span></span> <span data-ttu-id="c404c-153">Em seguida, abra a janela Comando na pasta do projeto e insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="c404c-153">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="c404c-154">Não execute o comando `database update` ainda.</span><span class="sxs-lookup"><span data-stu-id="c404c-154">Don't run the `database update` command yet.</span></span> <span data-ttu-id="c404c-155">Esse comando resultará em perda de dados porque ele removerá a tabela Instrutor e renomeará a tabela Aluno como Pessoa.</span><span class="sxs-lookup"><span data-stu-id="c404c-155">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="c404c-156">Você precisa fornecer o código personalizado para preservar os dados existentes.</span><span class="sxs-lookup"><span data-stu-id="c404c-156">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="c404c-157">Abra *Migrations/\<timestamp>_Inheritance.cs* e substitua o método `Up` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="c404c-157">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="c404c-158">Este código é responsável pelas seguintes tarefas de atualização de banco de dados:</span><span class="sxs-lookup"><span data-stu-id="c404c-158">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="c404c-159">Remove as restrições de chave estrangeira e índices que apontam para a tabela Aluno.</span><span class="sxs-lookup"><span data-stu-id="c404c-159">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="c404c-160">Renomeia a tabela Instrutor como Pessoa e faz as alterações necessárias para que ela armazene dados de Aluno:</span><span class="sxs-lookup"><span data-stu-id="c404c-160">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="c404c-161">Adiciona EnrollmentDate que permite valo nulo para os alunos.</span><span class="sxs-lookup"><span data-stu-id="c404c-161">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="c404c-162">Adiciona a coluna Discriminatória para indicar se uma linha refere-se a um aluno ou um instrutor.</span><span class="sxs-lookup"><span data-stu-id="c404c-162">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="c404c-163">Faz com que HireDate permita valor nulo, pois linhas de alunos não terão datas de contratação.</span><span class="sxs-lookup"><span data-stu-id="c404c-163">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="c404c-164">Adiciona um campo temporário que será usado para atualizar chaves estrangeiras que apontam para alunos.</span><span class="sxs-lookup"><span data-stu-id="c404c-164">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="c404c-165">Quando você copiar os alunos para a tabela Person, eles receberão novos valores de chave primária.</span><span class="sxs-lookup"><span data-stu-id="c404c-165">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="c404c-166">Copia os dados da tabela Aluno para a tabela Pessoa.</span><span class="sxs-lookup"><span data-stu-id="c404c-166">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="c404c-167">Isso faz com que os alunos recebam novos valores de chave primária.</span><span class="sxs-lookup"><span data-stu-id="c404c-167">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="c404c-168">Corrige valores de chave estrangeira que apontam para alunos.</span><span class="sxs-lookup"><span data-stu-id="c404c-168">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="c404c-169">Recria restrições de chave estrangeira e índices, agora apontando-os para a tabela Person.</span><span class="sxs-lookup"><span data-stu-id="c404c-169">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="c404c-170">(Se você tiver usado o GUID, em vez de inteiro como o tipo de chave primária, os valores de chave primária dos alunos não precisarão ser alterados e várias dessas etapas poderão ser omitidas.)</span><span class="sxs-lookup"><span data-stu-id="c404c-170">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="c404c-171">Execute o comando `database update`:</span><span class="sxs-lookup"><span data-stu-id="c404c-171">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="c404c-172">(Em um sistema de produção, você fará as alterações correspondentes no método `Down`, caso já tenha usado isso para voltar à versão anterior do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c404c-172">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="c404c-173">Para este tutorial, você não usará o método `Down`.)</span><span class="sxs-lookup"><span data-stu-id="c404c-173">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE] 
> <span data-ttu-id="c404c-174">É possível receber outros erros ao fazer alterações de esquema em um banco de dados que contém dados existentes.</span><span class="sxs-lookup"><span data-stu-id="c404c-174">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="c404c-175">Se você receber erros de migração que não consegue resolver, altere o nome do banco de dados na cadeia de conexão ou exclua o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c404c-175">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="c404c-176">Com um novo banco de dados, não há nenhum dado a ser migrado e o comando de atualização de banco de dados terá uma probabilidade maior de ser concluído sem erros.</span><span class="sxs-lookup"><span data-stu-id="c404c-176">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="c404c-177">Para excluir o banco de dados, use o SSOX ou execute o comando `database drop` da CLI.</span><span class="sxs-lookup"><span data-stu-id="c404c-177">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="c404c-178">Testar com a herança implementada</span><span class="sxs-lookup"><span data-stu-id="c404c-178">Test with inheritance implemented</span></span>

<span data-ttu-id="c404c-179">Execute o aplicativo e teste várias páginas.</span><span class="sxs-lookup"><span data-stu-id="c404c-179">Run the app and try various pages.</span></span> <span data-ttu-id="c404c-180">Tudo funciona da mesma maneira que antes.</span><span class="sxs-lookup"><span data-stu-id="c404c-180">Everything works the same as it did before.</span></span>

<span data-ttu-id="c404c-181">No **Pesquisador de Objetos do SQL Server**, expanda **Data Connections/SchoolContext** e, em seguida, **Tabelas** e você verá que as tabelas Aluno e Instrutor foram substituídas por uma tabela Pessoa.</span><span class="sxs-lookup"><span data-stu-id="c404c-181">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="c404c-182">Abra o designer de tabela Pessoa e você verá que ela contém todas as colunas que costumavam estar nas tabelas Aluno e Instrutor.</span><span class="sxs-lookup"><span data-stu-id="c404c-182">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Tabela Person no SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="c404c-184">Clique com o botão direito do mouse na tabela Person e, em seguida, clique em **Mostrar Dados da Tabela** para ver a coluna discriminatória.</span><span class="sxs-lookup"><span data-stu-id="c404c-184">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Tabela Person no SSOX – dados de tabela](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="c404c-186">Resumo</span><span class="sxs-lookup"><span data-stu-id="c404c-186">Summary</span></span>

<span data-ttu-id="c404c-187">Você implementou a herança de tabela por hierarquia para as classes `Person`, `Student` e `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="c404c-187">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="c404c-188">Para obter mais informações sobre a herança no Entity Framework Core, consulte [Herança](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="c404c-188">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="c404c-189">No próximo tutorial, você verá como lidar com uma variedade de cenários relativamente avançados do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c404c-189">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c404c-190">[Anterior](concurrency.md)
> [Próximo](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="c404c-190">[Previous](concurrency.md)
[Next](advanced.md)</span></span>  
