---
title: "Núcleo do ASP.NET MVC com núcleo EF - herança - 9 de 10"
author: tdykstra
description: "Este tutorial mostrará a implementação de herança no modelo de dados, usando o Entity Framework Core em um aplicativo do ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: a4ae696bdd114ab9c36d1218f753fa3d515f2300
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a><span data-ttu-id="47290-103">Herança - Core EF com o tutorial do MVC do ASP.NET Core (9 de 10)</span><span class="sxs-lookup"><span data-stu-id="47290-103">Inheritance - EF Core with ASP.NET Core MVC tutorial (9 of 10)</span></span>

<span data-ttu-id="47290-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="47290-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="47290-105">O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="47290-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="47290-106">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="47290-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="47290-107">No tutorial anterior, você tratou exceções de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="47290-107">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="47290-108">Este tutorial mostra como implementar a herança no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="47290-108">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="47290-109">Em programação orientada a objeto, você pode usar a herança para facilitar a reutilização de código.</span><span class="sxs-lookup"><span data-stu-id="47290-109">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="47290-110">Neste tutorial, você alterará a `Instructor` e `Student` classes para que eles derivam de um `Person` classe que contém propriedades, como base `LastName` que são comuns a professores e alunos.</span><span class="sxs-lookup"><span data-stu-id="47290-110">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="47290-111">Você não adicionar ou alterar todas as páginas da web, mas você alterará a parte do código e essas alterações serão refletidas automaticamente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="47290-111">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="47290-112">Opções para o mapeamento de herança para as tabelas de banco de dados</span><span class="sxs-lookup"><span data-stu-id="47290-112">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="47290-113">O `Instructor` e `Student` classes no modelo de dados School têm várias propriedades que são idênticas:</span><span class="sxs-lookup"><span data-stu-id="47290-113">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Classes de Student e instrutor](inheritance/_static/no-inheritance.png)

<span data-ttu-id="47290-115">Suponha que você deseja eliminar o código de redundância para as propriedades que são compartilhadas pelo `Instructor` e `Student` entidades.</span><span class="sxs-lookup"><span data-stu-id="47290-115">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="47290-116">Ou você quiser escrever um serviço que pode formatar nomes sem cuidar se o nome da origem do instrutor ou um aluno.</span><span class="sxs-lookup"><span data-stu-id="47290-116">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="47290-117">Você pode criar um `Person` classe base que contém apenas as propriedades compartilhadas e faça o `Instructor` e `Student` classes herdam a classe base, conforme mostrado na ilustração a seguir:</span><span class="sxs-lookup"><span data-stu-id="47290-117">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Classes de Student e instrutor derivar da classe pessoa](inheritance/_static/inheritance.png)

<span data-ttu-id="47290-119">Há várias maneiras que essa estrutura de herança poderia ser representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="47290-119">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="47290-120">Você pode ter uma tabela da pessoa que inclui informações sobre os alunos e instrutores em uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="47290-120">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="47290-121">Algumas das colunas podem aplicar somente a instrutores (HireDate), alguns somente para os alunos (EnrollmentDate), alguns para os dois (Sobrenome, nome).</span><span class="sxs-lookup"><span data-stu-id="47290-121">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="47290-122">Normalmente, você teria uma coluna discriminadora para indicar qual tipo de cada linha representa.</span><span class="sxs-lookup"><span data-stu-id="47290-122">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="47290-123">Por exemplo, coluna discriminadora pode ter "Instrutor" para "Aluno" e instrutores para estudantes.</span><span class="sxs-lookup"><span data-stu-id="47290-123">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Exemplo de tabela por hierarquia](inheritance/_static/tph.png)

<span data-ttu-id="47290-125">Esse padrão de geração de uma estrutura de herança de entidade de uma tabela de banco de dados é chamado de herança (TPH) de tabela por hierarquia.</span><span class="sxs-lookup"><span data-stu-id="47290-125">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="47290-126">Uma alternativa é fazer com que o banco de dados pareça mais com a estrutura de herança.</span><span class="sxs-lookup"><span data-stu-id="47290-126">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="47290-127">Por exemplo, você pode ter apenas os campos de nome na tabela Person e ter tabelas separadas do aluno e do instrutor com os campos de data.</span><span class="sxs-lookup"><span data-stu-id="47290-127">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Herança de tabela por tipo](inheritance/_static/tpt.png)

<span data-ttu-id="47290-129">Esse padrão de criação de uma tabela de banco de dados para cada classe de entidade é chamado de tabela por herança de tipo (TPT).</span><span class="sxs-lookup"><span data-stu-id="47290-129">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="47290-130">Outra opção é mapear todos os tipos de não-abstrato para tabelas individuais.</span><span class="sxs-lookup"><span data-stu-id="47290-130">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="47290-131">Todas as propriedades de uma classe, incluindo propriedades herdadas, mapeiam para colunas da tabela correspondente.</span><span class="sxs-lookup"><span data-stu-id="47290-131">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="47290-132">Esse padrão é chamado de herança de classe de tabela por concreto TPC ().</span><span class="sxs-lookup"><span data-stu-id="47290-132">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="47290-133">Se você tiver implementado herança TPC para as classes da pessoa, do aluno e do instrutor como mostrado anteriormente, as tabelas Student e instrutor seria não diferentes depois de implementar a herança do que antes.</span><span class="sxs-lookup"><span data-stu-id="47290-133">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="47290-134">Padrões de herança TPC e TPH geralmente oferecem melhor desempenho de padrões de herança TPT, porque padrões TPT podem resultar em consultas de junção complexas.</span><span class="sxs-lookup"><span data-stu-id="47290-134">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="47290-135">Este tutorial demonstra como implementar a herança TPH.</span><span class="sxs-lookup"><span data-stu-id="47290-135">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="47290-136">TPH é o padrão de herança única que o Entity Framework Core oferece suporte.</span><span class="sxs-lookup"><span data-stu-id="47290-136">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="47290-137">Você vai fazer é criar um `Person` classe, altere o `Instructor` e `Student` classes derivar de `Person`, adicione a nova classe para o `DbContext`e crie uma migração.</span><span class="sxs-lookup"><span data-stu-id="47290-137">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP] 
> <span data-ttu-id="47290-138">Considere a possibilidade de salvar uma cópia do projeto antes de fazer as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="47290-138">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="47290-139">Em seguida, se você tiver problemas e precisa recomeçar, ela será mais fácil de iniciar do projeto em vez de reverter as etapas executadas para este tutorial ou que será salvo novamente para o início da série inteira.</span><span class="sxs-lookup"><span data-stu-id="47290-139">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="47290-140">Criar a classe pessoa</span><span class="sxs-lookup"><span data-stu-id="47290-140">Create the Person class</span></span>

<span data-ttu-id="47290-141">Na pasta de modelos, criar Person.cs e substitua o código de modelo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="47290-141">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="47290-142">Verifique o aluno e do instrutor classes herdam a pessoa</span><span class="sxs-lookup"><span data-stu-id="47290-142">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="47290-143">Em *Instructor.cs*, a classe do instrutor é derivado da classe pessoa e remover os campos nome e chave.</span><span class="sxs-lookup"><span data-stu-id="47290-143">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="47290-144">O código será semelhante o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="47290-144">The code will look like the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="47290-145">Fazer as mesmas alterações em *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="47290-145">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="47290-146">Adicione o tipo de entidade de pessoa para o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="47290-146">Add the Person entity type to the data model</span></span>

<span data-ttu-id="47290-147">Adicione o tipo de entidade de pessoa para *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="47290-147">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="47290-148">As novas linhas são realçadas.</span><span class="sxs-lookup"><span data-stu-id="47290-148">The new lines are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="47290-149">Isso é tudo o que o Entity Framework precisa para configurar a herança de tabela por hierarquia.</span><span class="sxs-lookup"><span data-stu-id="47290-149">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="47290-150">Como você verá, quando o banco de dados é atualizado, ele terá uma tabela pessoa no lugar as tabelas Student e instrutor.</span><span class="sxs-lookup"><span data-stu-id="47290-150">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="47290-151">Criar e personalizar o código de migração</span><span class="sxs-lookup"><span data-stu-id="47290-151">Create and customize migration code</span></span>

<span data-ttu-id="47290-152">Salve suas alterações e compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="47290-152">Save your changes and build the project.</span></span> <span data-ttu-id="47290-153">Em seguida, abra a janela de comando na pasta do projeto e digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="47290-153">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="47290-154">Não execute o `database update` comando ainda.</span><span class="sxs-lookup"><span data-stu-id="47290-154">Don't run the `database update` command yet.</span></span> <span data-ttu-id="47290-155">Esse comando resultará em perda de dados porque ele descarte a tabela instrutor e renomeie a tabela de alunos a pessoa.</span><span class="sxs-lookup"><span data-stu-id="47290-155">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="47290-156">Você precisa fornecer o código personalizado para preservar os dados existentes.</span><span class="sxs-lookup"><span data-stu-id="47290-156">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="47290-157">Abra *migrações /\<timestamp > _Inheritance.cs* e substitua o `Up` método com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="47290-157">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="47290-158">Esse código cuida das seguintes tarefas de atualização de banco de dados:</span><span class="sxs-lookup"><span data-stu-id="47290-158">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="47290-159">Remove as restrições de chave estrangeira e índices que apontam para a tabela de alunos.</span><span class="sxs-lookup"><span data-stu-id="47290-159">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="47290-160">Renomeia a tabela instrutor como pessoa e faz as alterações necessárias para ele armazenar dados de aluno:</span><span class="sxs-lookup"><span data-stu-id="47290-160">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="47290-161">Adiciona EnrollmentDate anulável para estudantes.</span><span class="sxs-lookup"><span data-stu-id="47290-161">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="47290-162">Adiciona a coluna discriminadora para indicar se uma linha é para um aluno ou instrutor.</span><span class="sxs-lookup"><span data-stu-id="47290-162">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="47290-163">Torna HireDate anulável como linhas de aluno não têm datas de contratação.</span><span class="sxs-lookup"><span data-stu-id="47290-163">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="47290-164">Adiciona um campo temporário que será usado para atualizar chaves estrangeiras que apontam para os alunos.</span><span class="sxs-lookup"><span data-stu-id="47290-164">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="47290-165">Quando você copia os alunos para a tabela Person recebem novos valores de chave primária.</span><span class="sxs-lookup"><span data-stu-id="47290-165">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="47290-166">Copia os dados da tabela de estudante na tabela Person.</span><span class="sxs-lookup"><span data-stu-id="47290-166">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="47290-167">Isso faz com que os alunos novos valores de chave primária foi atribuído.</span><span class="sxs-lookup"><span data-stu-id="47290-167">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="47290-168">Correções de valores de chave estrangeira que apontam para os alunos.</span><span class="sxs-lookup"><span data-stu-id="47290-168">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="47290-169">Recria índices, agora apontá-los para a tabela Person e restrições de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="47290-169">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="47290-170">(Se você tiver usado o GUID, em vez de inteiro como o tipo de chave primário, os valores de chave primária do aluno não precisam alterar e várias dessas etapas foi foi omitidas).</span><span class="sxs-lookup"><span data-stu-id="47290-170">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="47290-171">Execute o `database update` comando:</span><span class="sxs-lookup"><span data-stu-id="47290-171">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="47290-172">(Em um sistema de produção faça as alterações correspondentes a `Down` método caso você teve de usá-la para voltar para a versão anterior do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="47290-172">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="47290-173">Para este tutorial, você não usará o `Down` método.)</span><span class="sxs-lookup"><span data-stu-id="47290-173">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE] 
> <span data-ttu-id="47290-174">É possível obter outros erros ao fazer alterações de esquema em um banco de dados que contém dados existentes.</span><span class="sxs-lookup"><span data-stu-id="47290-174">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="47290-175">Se você obtiver erros de migração que você não conseguir resolver, você pode alterar o nome do banco de dados na cadeia de conexão ou excluir o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="47290-175">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="47290-176">Com um novo banco de dados, não há nenhum dado para migrar e o comando de atualização de banco de dados é mais provável de ser concluído sem erros.</span><span class="sxs-lookup"><span data-stu-id="47290-176">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="47290-177">Para excluir o banco de dados, use SSOX ou execute o `database drop` comando CLI.</span><span class="sxs-lookup"><span data-stu-id="47290-177">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="47290-178">Testar com herança implementada</span><span class="sxs-lookup"><span data-stu-id="47290-178">Test with inheritance implemented</span></span>

<span data-ttu-id="47290-179">Execute o aplicativo e tente várias páginas.</span><span class="sxs-lookup"><span data-stu-id="47290-179">Run the app and try various pages.</span></span> <span data-ttu-id="47290-180">Tudo funciona da mesma forma que antes.</span><span class="sxs-lookup"><span data-stu-id="47290-180">Everything works the same as it did before.</span></span>

<span data-ttu-id="47290-181">Em **Pesquisador de objetos do SQL Server**, expanda **conexões de dados/SchoolContext** e **tabelas**, e você verá que as tabelas Student e instrutor foram substituídas por um Tabela Person.</span><span class="sxs-lookup"><span data-stu-id="47290-181">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="47290-182">Abra o designer de tabela pessoa e você verá que ele tem todas as colunas que costumava ser nas tabelas Student e instrutor.</span><span class="sxs-lookup"><span data-stu-id="47290-182">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Tabela Person em SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="47290-184">Clique com botão direito a tabela Person e, em seguida, clique em **Mostrar dados da tabela** para ver a coluna discriminadora.</span><span class="sxs-lookup"><span data-stu-id="47290-184">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Tabela Person em SSOX - dados de tabela](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="47290-186">Resumo</span><span class="sxs-lookup"><span data-stu-id="47290-186">Summary</span></span>

<span data-ttu-id="47290-187">Você implementou a herança de tabela por hierarquia para o `Person`, `Student`, e `Instructor` classes.</span><span class="sxs-lookup"><span data-stu-id="47290-187">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="47290-188">Para obter mais informações sobre a herança em Entity Framework Core, consulte [herança](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="47290-188">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="47290-189">O seguinte tutorial, você verá como manipular uma variedade de cenários relativamente avançados do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="47290-189">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="47290-190">[Anterior](concurrency.md)
[Próximo](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="47290-190">[Previous](concurrency.md)
[Next](advanced.md)</span></span>  
