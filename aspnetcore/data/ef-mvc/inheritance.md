---
title: "Núcleo do ASP.NET MVC com núcleo EF - herança - 9 de 10"
author: tdykstra
description: "Este tutorial mostrará a implementação de herança no modelo de dados, usando o Entity Framework Core em um aplicativo do ASP.NET Core."
keywords: "Herança de ASP.NET Core, Entity Framework Core,"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 41dc0db7-6f17-453e-aba6-633430609c74
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 6102b426cb5aff78fedb9389df229cd8100e4f36
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2017
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a><span data-ttu-id="165c4-104">Herança - Core EF com o tutorial do MVC do ASP.NET Core (9 de 10)</span><span class="sxs-lookup"><span data-stu-id="165c4-104">Inheritance - EF Core with ASP.NET Core MVC tutorial (9 of 10)</span></span>

<span data-ttu-id="165c4-105">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="165c4-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="165c4-106">O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="165c4-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="165c4-107">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="165c4-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="165c4-108">No tutorial anterior, você tratou exceções de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="165c4-108">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="165c4-109">Este tutorial mostra como implementar a herança no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="165c4-109">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="165c4-110">Em programação orientada a objeto, você pode usar a herança para facilitar a reutilização de código.</span><span class="sxs-lookup"><span data-stu-id="165c4-110">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="165c4-111">Neste tutorial, você alterará a `Instructor` e `Student` classes para que eles derivam de um `Person` classe que contém propriedades, como base `LastName` que são comuns a professores e alunos.</span><span class="sxs-lookup"><span data-stu-id="165c4-111">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="165c4-112">Você não adicionar ou alterar todas as páginas da web, mas você alterará a parte do código e essas alterações serão refletidas automaticamente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="165c4-112">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="165c4-113">Opções para o mapeamento de herança para as tabelas de banco de dados</span><span class="sxs-lookup"><span data-stu-id="165c4-113">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="165c4-114">O `Instructor` e `Student` classes no modelo de dados School têm várias propriedades que são idênticas:</span><span class="sxs-lookup"><span data-stu-id="165c4-114">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Classes de Student e instrutor](inheritance/_static/no-inheritance.png)

<span data-ttu-id="165c4-116">Suponha que você deseja eliminar o código de redundância para as propriedades que são compartilhadas pelo `Instructor` e `Student` entidades.</span><span class="sxs-lookup"><span data-stu-id="165c4-116">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="165c4-117">Ou você quiser escrever um serviço que pode formatar nomes sem cuidar se o nome da origem do instrutor ou um aluno.</span><span class="sxs-lookup"><span data-stu-id="165c4-117">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="165c4-118">Você pode criar um `Person` classe base que contém apenas as propriedades compartilhadas e faça o `Instructor` e `Student` classes herdam a classe base, conforme mostrado na ilustração a seguir:</span><span class="sxs-lookup"><span data-stu-id="165c4-118">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Classes de Student e instrutor derivar da classe pessoa](inheritance/_static/inheritance.png)

<span data-ttu-id="165c4-120">Há várias maneiras que essa estrutura de herança poderia ser representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="165c4-120">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="165c4-121">Você pode ter uma tabela da pessoa que inclui informações sobre os alunos e instrutores em uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="165c4-121">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="165c4-122">Algumas das colunas podem aplicar somente a instrutores (HireDate), alguns somente para os alunos (EnrollmentDate), alguns para os dois (Sobrenome, nome).</span><span class="sxs-lookup"><span data-stu-id="165c4-122">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="165c4-123">Normalmente, você teria uma coluna discriminadora para indicar qual tipo de cada linha representa.</span><span class="sxs-lookup"><span data-stu-id="165c4-123">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="165c4-124">Por exemplo, coluna discriminadora pode ter "Instrutor" para "Aluno" e instrutores para estudantes.</span><span class="sxs-lookup"><span data-stu-id="165c4-124">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Exemplo de tabela por hierarquia](inheritance/_static/tph.png)

<span data-ttu-id="165c4-126">Esse padrão de geração de uma estrutura de herança de entidade de uma tabela de banco de dados é chamado de herança (TPH) de tabela por hierarquia.</span><span class="sxs-lookup"><span data-stu-id="165c4-126">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="165c4-127">Uma alternativa é fazer com que o banco de dados pareça mais com a estrutura de herança.</span><span class="sxs-lookup"><span data-stu-id="165c4-127">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="165c4-128">Por exemplo, você pode ter apenas os campos de nome na tabela Person e ter tabelas separadas do aluno e do instrutor com os campos de data.</span><span class="sxs-lookup"><span data-stu-id="165c4-128">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Herança de tabela por tipo](inheritance/_static/tpt.png)

<span data-ttu-id="165c4-130">Esse padrão de criação de uma tabela de banco de dados para cada classe de entidade é chamado de tabela por herança de tipo (TPT).</span><span class="sxs-lookup"><span data-stu-id="165c4-130">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="165c4-131">Outra opção é mapear todos os tipos de não-abstrato para tabelas individuais.</span><span class="sxs-lookup"><span data-stu-id="165c4-131">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="165c4-132">Todas as propriedades de uma classe, incluindo propriedades herdadas, mapeiam para colunas da tabela correspondente.</span><span class="sxs-lookup"><span data-stu-id="165c4-132">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="165c4-133">Esse padrão é chamado de herança de classe de tabela por concreto TPC ().</span><span class="sxs-lookup"><span data-stu-id="165c4-133">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="165c4-134">Se você tiver implementado herança TPC para as classes da pessoa, do aluno e do instrutor como mostrado anteriormente, as tabelas Student e instrutor seria não diferentes depois de implementar a herança do que antes.</span><span class="sxs-lookup"><span data-stu-id="165c4-134">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="165c4-135">Padrões de herança TPC e TPH geralmente oferecem melhor desempenho de padrões de herança TPT, porque padrões TPT podem resultar em consultas de junção complexas.</span><span class="sxs-lookup"><span data-stu-id="165c4-135">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="165c4-136">Este tutorial demonstra como implementar a herança TPH.</span><span class="sxs-lookup"><span data-stu-id="165c4-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="165c4-137">TPH é o padrão de herança única que o Entity Framework Core oferece suporte.</span><span class="sxs-lookup"><span data-stu-id="165c4-137">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="165c4-138">Você vai fazer é criar um `Person` classe, altere o `Instructor` e `Student` classes derivar de `Person`, adicione a nova classe para o `DbContext`e crie uma migração.</span><span class="sxs-lookup"><span data-stu-id="165c4-138">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP] 
> <span data-ttu-id="165c4-139">Considere a possibilidade de salvar uma cópia do projeto antes de fazer as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="165c4-139">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="165c4-140">Em seguida, se você tiver problemas e precisa recomeçar, ela será mais fácil de iniciar do projeto em vez de reverter as etapas executadas para este tutorial ou que será salvo novamente para o início da série inteira.</span><span class="sxs-lookup"><span data-stu-id="165c4-140">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="165c4-141">Criar a classe pessoa</span><span class="sxs-lookup"><span data-stu-id="165c4-141">Create the Person class</span></span>

<span data-ttu-id="165c4-142">Na pasta de modelos, criar Person.cs e substitua o código de modelo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="165c4-142">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="165c4-143">Verifique o aluno e do instrutor classes herdam a pessoa</span><span class="sxs-lookup"><span data-stu-id="165c4-143">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="165c4-144">Em *Instructor.cs*, a classe do instrutor é derivado da classe pessoa e remover os campos nome e chave.</span><span class="sxs-lookup"><span data-stu-id="165c4-144">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="165c4-145">O código será semelhante o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="165c4-145">The code will look like the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="165c4-146">Fazer as mesmas alterações em *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="165c4-146">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="165c4-147">Adicione o tipo de entidade de pessoa para o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="165c4-147">Add the Person entity type to the data model</span></span>

<span data-ttu-id="165c4-148">Adicione o tipo de entidade de pessoa para *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="165c4-148">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="165c4-149">As novas linhas são realçadas.</span><span class="sxs-lookup"><span data-stu-id="165c4-149">The new lines are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="165c4-150">Isso é tudo o que o Entity Framework precisa para configurar a herança de tabela por hierarquia.</span><span class="sxs-lookup"><span data-stu-id="165c4-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="165c4-151">Como você verá, quando o banco de dados é atualizado, ele terá uma tabela pessoa no lugar as tabelas Student e instrutor.</span><span class="sxs-lookup"><span data-stu-id="165c4-151">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="165c4-152">Criar e personalizar o código de migração</span><span class="sxs-lookup"><span data-stu-id="165c4-152">Create and customize migration code</span></span>

<span data-ttu-id="165c4-153">Salve suas alterações e compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="165c4-153">Save your changes and build the project.</span></span> <span data-ttu-id="165c4-154">Em seguida, abra a janela de comando na pasta do projeto e digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="165c4-154">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="165c4-155">Não execute o `database update` comando ainda.</span><span class="sxs-lookup"><span data-stu-id="165c4-155">Don't run the `database update` command yet.</span></span> <span data-ttu-id="165c4-156">Esse comando resultará em perda de dados porque ele descarte a tabela instrutor e renomeie a tabela de alunos a pessoa.</span><span class="sxs-lookup"><span data-stu-id="165c4-156">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="165c4-157">Você precisa fornecer o código personalizado para preservar os dados existentes.</span><span class="sxs-lookup"><span data-stu-id="165c4-157">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="165c4-158">Abra *migrações /\<timestamp > _Inheritance.cs* e substitua o `Up` método com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="165c4-158">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="165c4-159">Esse código cuida das seguintes tarefas de atualização de banco de dados:</span><span class="sxs-lookup"><span data-stu-id="165c4-159">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="165c4-160">Remove as restrições de chave estrangeira e índices que apontam para a tabela de alunos.</span><span class="sxs-lookup"><span data-stu-id="165c4-160">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="165c4-161">Renomeia a tabela instrutor como pessoa e faz as alterações necessárias para ele armazenar dados de aluno:</span><span class="sxs-lookup"><span data-stu-id="165c4-161">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="165c4-162">Adiciona EnrollmentDate anulável para estudantes.</span><span class="sxs-lookup"><span data-stu-id="165c4-162">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="165c4-163">Adiciona a coluna discriminadora para indicar se uma linha é para um aluno ou instrutor.</span><span class="sxs-lookup"><span data-stu-id="165c4-163">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="165c4-164">Torna HireDate anulável como linhas de aluno não têm datas de contratação.</span><span class="sxs-lookup"><span data-stu-id="165c4-164">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="165c4-165">Adiciona um campo temporário que será usado para atualizar chaves estrangeiras que apontam para os alunos.</span><span class="sxs-lookup"><span data-stu-id="165c4-165">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="165c4-166">Quando você copia os alunos para a tabela pessoa que receberão novos valores de chave primária.</span><span class="sxs-lookup"><span data-stu-id="165c4-166">When you copy students into the Person table they'll get new primary key values.</span></span>

* <span data-ttu-id="165c4-167">Copia os dados da tabela de estudante na tabela Person.</span><span class="sxs-lookup"><span data-stu-id="165c4-167">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="165c4-168">Isso faz com que os alunos novos valores de chave primária foi atribuído.</span><span class="sxs-lookup"><span data-stu-id="165c4-168">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="165c4-169">Correções de valores de chave estrangeira que apontam para os alunos.</span><span class="sxs-lookup"><span data-stu-id="165c4-169">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="165c4-170">Recria índices, agora apontá-los para a tabela Person e restrições de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="165c4-170">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="165c4-171">(Se você tiver usado o GUID, em vez de inteiro como o tipo de chave primário, os valores de chave primária do aluno não precisam alterar e várias dessas etapas foi foi omitidas).</span><span class="sxs-lookup"><span data-stu-id="165c4-171">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="165c4-172">Execute o `database update` comando:</span><span class="sxs-lookup"><span data-stu-id="165c4-172">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="165c4-173">(Em um sistema de produção faça as alterações correspondentes a `Down` método caso você teve de usá-la para voltar para a versão anterior do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="165c4-173">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="165c4-174">Para este tutorial, você não usará o `Down` método.)</span><span class="sxs-lookup"><span data-stu-id="165c4-174">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE] 
> <span data-ttu-id="165c4-175">É possível obter outros erros ao fazer alterações de esquema em um banco de dados que contém dados existentes.</span><span class="sxs-lookup"><span data-stu-id="165c4-175">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="165c4-176">Se você obtiver erros de migração que você não conseguir resolver, você pode alterar o nome do banco de dados na cadeia de conexão ou excluir o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="165c4-176">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="165c4-177">Com um novo banco de dados, não há nenhum dado para migrar e o comando de atualização de banco de dados é mais provável de ser concluído sem erros.</span><span class="sxs-lookup"><span data-stu-id="165c4-177">With a new database, there is no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="165c4-178">Para excluir o banco de dados, use SSOX ou execute o `database drop` comando CLI.</span><span class="sxs-lookup"><span data-stu-id="165c4-178">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="165c4-179">Testar com herança implementada</span><span class="sxs-lookup"><span data-stu-id="165c4-179">Test with inheritance implemented</span></span>

<span data-ttu-id="165c4-180">Execute o aplicativo e tente várias páginas.</span><span class="sxs-lookup"><span data-stu-id="165c4-180">Run the app and try various pages.</span></span> <span data-ttu-id="165c4-181">Tudo funciona da mesma forma que antes.</span><span class="sxs-lookup"><span data-stu-id="165c4-181">Everything works the same as it did before.</span></span>

<span data-ttu-id="165c4-182">Em **Pesquisador de objetos do SQL Server**, expanda **conexões de dados/SchoolContext** e **tabelas**, e você verá que as tabelas Student e instrutor foram substituídas por um Tabela Person.</span><span class="sxs-lookup"><span data-stu-id="165c4-182">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="165c4-183">Abra o designer de tabela pessoa e você verá que ele tem todas as colunas que costumava ser nas tabelas Student e instrutor.</span><span class="sxs-lookup"><span data-stu-id="165c4-183">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Tabela Person em SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="165c4-185">Clique com botão direito a tabela Person e, em seguida, clique em **Mostrar dados da tabela** para ver a coluna discriminadora.</span><span class="sxs-lookup"><span data-stu-id="165c4-185">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Tabela Person em SSOX - dados de tabela](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="165c4-187">Resumo</span><span class="sxs-lookup"><span data-stu-id="165c4-187">Summary</span></span>

<span data-ttu-id="165c4-188">Você implementou a herança de tabela por hierarquia para o `Person`, `Student`, e `Instructor` classes.</span><span class="sxs-lookup"><span data-stu-id="165c4-188">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="165c4-189">Para obter mais informações sobre a herança em Entity Framework Core, consulte [herança](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="165c4-189">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="165c4-190">O seguinte tutorial, você verá como manipular uma variedade de cenários relativamente avançados do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="165c4-190">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="165c4-191">[Anterior](concurrency.md)
[Próximo](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="165c4-191">[Previous](concurrency.md)
[Next](advanced.md)</span></span>  
