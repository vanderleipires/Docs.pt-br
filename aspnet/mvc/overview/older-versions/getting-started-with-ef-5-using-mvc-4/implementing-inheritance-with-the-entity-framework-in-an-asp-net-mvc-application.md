---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementando a herança com o Entity Framework em um aplicativo ASP.NET MVC (8 de 10) | Microsoft Docs
author: tdykstra
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d3c3d19a62f601d007b0fac031ade1eef9f35771
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378930"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a><span data-ttu-id="de57c-103">Implementando a herança com o Entity Framework em um aplicativo ASP.NET MVC (8 de 10)</span><span class="sxs-lookup"><span data-stu-id="de57c-103">Implementing Inheritance with the Entity Framework in an ASP.NET MVC Application (8 of 10)</span></span>
====================
<span data-ttu-id="de57c-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="de57c-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="de57c-105">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="de57c-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="de57c-106">Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="de57c-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="de57c-107">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="de57c-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="de57c-108">Você pode iniciar a série de tutoriais de início ou [baixar um projeto inicial para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) e comece por aqui.</span><span class="sxs-lookup"><span data-stu-id="de57c-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="de57c-109">Se você enfrentar um problema que você não conseguir resolver, [baixar o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema.</span><span class="sxs-lookup"><span data-stu-id="de57c-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="de57c-110">Em geral, você pode encontrar a solução ao problema comparando seu código com o código completo.</span><span class="sxs-lookup"><span data-stu-id="de57c-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="de57c-111">Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="de57c-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="de57c-112">No tutorial anterior, você tratou exceções de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="de57c-112">In the previous tutorial you handled concurrency exceptions.</span></span> <span data-ttu-id="de57c-113">Este tutorial mostrará como implementar a herança no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="de57c-113">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="de57c-114">Na programação orientada a objeto, você pode usar a herança para eliminar código redundante.</span><span class="sxs-lookup"><span data-stu-id="de57c-114">In object-oriented programming, you can use inheritance to eliminate redundant code.</span></span> <span data-ttu-id="de57c-115">Neste tutorial, você alterará as classes `Instructor` e `Student`, de modo que elas derivem de uma classe base `Person` que contém propriedades, como `LastName`, comuns a instrutores e alunos.</span><span class="sxs-lookup"><span data-stu-id="de57c-115">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="de57c-116">Você não adicionará nem alterará as páginas da Web, mas alterará uma parte do código, e essas alterações serão refletidas automaticamente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="de57c-116">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a><span data-ttu-id="de57c-117">Tabela por hierarquia versus a herança de tabela por tipo</span><span class="sxs-lookup"><span data-stu-id="de57c-117">Table-per-Hierarchy versus Table-per-Type Inheritance</span></span>

<span data-ttu-id="de57c-118">Na programação orientada a objeto, você pode usar a herança para torná-lo mais fácil trabalhar com classes relacionadas.</span><span class="sxs-lookup"><span data-stu-id="de57c-118">In object-oriented programming, you can use inheritance to make it easier to work with related classes.</span></span> <span data-ttu-id="de57c-119">Por exemplo, o `Instructor` e `Student` as classes no `School` modelo de dados compartilham várias propriedades, que resulta em código redundante:</span><span class="sxs-lookup"><span data-stu-id="de57c-119">For example, the `Instructor` and `Student` classes in the `School` data model share several properties, which results in redundant code:</span></span>

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

<span data-ttu-id="de57c-121">Suponha que você deseje eliminar o código redundante para as propriedades compartilhadas pelas entidades `Instructor` e `Student`.</span><span class="sxs-lookup"><span data-stu-id="de57c-121">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="de57c-122">Você pode criar uma `Person` classe que contém apenas essas propriedades compartilhadas base, em seguida, fazer a `Instructor` e `Student` entidades herdam dessa classe base, conforme mostrado na ilustração a seguir:</span><span class="sxs-lookup"><span data-stu-id="de57c-122">You could create a `Person` base class which contains only those shared properties, then make the `Instructor` and `Student` entities inherit from that base class, as shown in the following illustration:</span></span>

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

<span data-ttu-id="de57c-124">Há várias maneiras pelas quais essa estrutura de herança pode ser representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="de57c-124">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="de57c-125">Você poderia ter um `Person` tabela que inclui informações sobre os alunos e instrutores em uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="de57c-125">You could have a `Person` table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="de57c-126">Algumas das colunas pode aplicar somente a instrutores (`HireDate`), algumas somente a alunos (`EnrollmentDate`), alguns para ambos (`LastName`, `FirstName`).</span><span class="sxs-lookup"><span data-stu-id="de57c-126">Some of the columns could apply only to instructors (`HireDate`), some only to students (`EnrollmentDate`), some to both (`LastName`, `FirstName`).</span></span> <span data-ttu-id="de57c-127">Normalmente, você teria uma *discriminador* coluna para indicar qual tipo cada linha representa.</span><span class="sxs-lookup"><span data-stu-id="de57c-127">Typically, you'd have a *discriminator* column to indicate which type each row represents.</span></span> <span data-ttu-id="de57c-128">Por exemplo, a coluna discriminatória pode ter "Instrutor" para instrutores e "Aluno" para alunos.</span><span class="sxs-lookup"><span data-stu-id="de57c-128">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Tabela por hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

<span data-ttu-id="de57c-130">Esse padrão de geração de uma estrutura de herança de entidade de uma tabela de banco de dados individual é chamado *tabela por hierarquia* herança (TPH).</span><span class="sxs-lookup"><span data-stu-id="de57c-130">This pattern of generating an entity inheritance structure from a single database table is called *table-per-hierarchy* (TPH) inheritance.</span></span>

<span data-ttu-id="de57c-131">Uma alternativa é fazer com que o banco de dados se pareça mais com a estrutura de herança.</span><span class="sxs-lookup"><span data-stu-id="de57c-131">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="de57c-132">Por exemplo, você poderia ter apenas os campos de nome `Person` de tabela e ter separado `Instructor` e `Student` tabelas com os campos de data.</span><span class="sxs-lookup"><span data-stu-id="de57c-132">For example, you could have only the name fields in the `Person` table and have separate `Instructor` and `Student` tables with the date fields.</span></span>

![Tabela por type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

<span data-ttu-id="de57c-134">Esse padrão de criação de uma tabela de banco de dados para cada classe de entidade é chamada *tabela por tipo* herança (TPT).</span><span class="sxs-lookup"><span data-stu-id="de57c-134">This pattern of making a database table for each entity class is called *table per type* (TPT) inheritance.</span></span>

<span data-ttu-id="de57c-135">Padrões de herança TPH oferecem melhor desempenho geralmente no Entity Framework que os padrões de herança TPT, porque os padrões TPT podem resultar em consultas de junção complexas.</span><span class="sxs-lookup"><span data-stu-id="de57c-135">TPH inheritance patterns generally deliver better performance in the Entity Framework than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span> <span data-ttu-id="de57c-136">Este tutorial demonstra como implementar a herança TPH.</span><span class="sxs-lookup"><span data-stu-id="de57c-136">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="de57c-137">Você terá de fazer isso executando as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="de57c-137">You'll do that by performing the following steps:</span></span>

- <span data-ttu-id="de57c-138">Criar uma `Person` de classe e altere o `Instructor` e `Student` classes sejam derivadas de `Person`.</span><span class="sxs-lookup"><span data-stu-id="de57c-138">Create a `Person` class and change the `Instructor` and `Student` classes to derive from `Person`.</span></span>
- <span data-ttu-id="de57c-139">Adicione o código de mapeamento do modelo de banco de dados para a classe de contexto do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="de57c-139">Add model-to-database mapping code to the database context class.</span></span>
- <span data-ttu-id="de57c-140">Alteração `InstructorID` e `StudentID` referências em todo o projeto para `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="de57c-140">Change `InstructorID` and `StudentID` references throughout the project to `PersonID`.</span></span>

## <a name="creating-the-person-class"></a><span data-ttu-id="de57c-141">Criando a classe Person</span><span class="sxs-lookup"><span data-stu-id="de57c-141">Creating the Person Class</span></span>

 <span data-ttu-id="de57c-142">Observação: Você não será capaz de compilar o projeto depois de criar as classes abaixo até que você atualize os controladores que usa essas classes.</span><span class="sxs-lookup"><span data-stu-id="de57c-142">Note: You won't be able to compile the project after creating the classes below until you update the controllers that uses these classes.</span></span> 

<span data-ttu-id="de57c-143">No *modelos* pasta, crie *Person.cs* e substitua o código de modelo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="de57c-143">In the *Models* folder, create *Person.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="de57c-144">Na *Instructor.cs*, derivar o `Instructor` classe o `Person` de classe e remova os campos nomes e chaves.</span><span class="sxs-lookup"><span data-stu-id="de57c-144">In *Instructor.cs*, derive the `Instructor` class from the `Person` class and remove the key and name fields.</span></span> <span data-ttu-id="de57c-145">O código será semelhante ao seguinte exemplo:</span><span class="sxs-lookup"><span data-stu-id="de57c-145">The code will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="de57c-146">Fazer alterações semelhantes ao *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="de57c-146">Make similar changes to *Student.cs*.</span></span> <span data-ttu-id="de57c-147">O `Student` classe se parecerá com o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="de57c-147">The `Student` class will look like the following example:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a><span data-ttu-id="de57c-148">Adicionando o tipo de entidade Person ao modelo</span><span class="sxs-lookup"><span data-stu-id="de57c-148">Adding the Person Entity Type to the Model</span></span>

<span data-ttu-id="de57c-149">Na *SchoolContext.cs*, adicione uma `DbSet` propriedade para o `Person` tipo de entidade:</span><span class="sxs-lookup"><span data-stu-id="de57c-149">In *SchoolContext.cs*, add a `DbSet` property for the `Person` entity type:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="de57c-150">Isso é tudo o que o Entity Framework precisa para configurar a herança de tabela por hierarquia.</span><span class="sxs-lookup"><span data-stu-id="de57c-150">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="de57c-151">Como você verá, quando o banco de dados é recriado, ele terá um `Person` de tabela em vez do `Student` e `Instructor` tabelas.</span><span class="sxs-lookup"><span data-stu-id="de57c-151">As you'll see, when the database is re-created, it will have a `Person` table in place of the `Student` and `Instructor` tables.</span></span>

## <a name="changing-instructorid-and-studentid-to-personid"></a><span data-ttu-id="de57c-152">Alterando InstructorID e StudentID para PersonID</span><span class="sxs-lookup"><span data-stu-id="de57c-152">Changing InstructorID and StudentID to PersonID</span></span>

<span data-ttu-id="de57c-153">Na *SchoolContext.cs*, na instrução de mapeamento de curso do instrutor, altere `MapRightKey("InstructorID")` para `MapRightKey("PersonID")`:</span><span class="sxs-lookup"><span data-stu-id="de57c-153">In *SchoolContext.cs*, in the Instructor-Course mapping statement, change `MapRightKey("InstructorID")` to `MapRightKey("PersonID")`:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

<span data-ttu-id="de57c-154">Essa alteração não é necessária. ela apenas altera o nome da coluna InstructorID na tabela de junção de muitos-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="de57c-154">This change isn't required; it just changes the name of the InstructorID column in the many-to-many join table.</span></span> <span data-ttu-id="de57c-155">Se você deixou o nome como InstructorID, o aplicativo ainda funciona corretamente.</span><span class="sxs-lookup"><span data-stu-id="de57c-155">If you left the name as InstructorID, the application would still work correctly.</span></span> <span data-ttu-id="de57c-156">Aqui está concluído *SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="de57c-156">Here is the completed *SchoolContext.cs*:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

<span data-ttu-id="de57c-157">Em seguida, você precisará alterar `InstructorID` para `PersonID` e `StudentID` ao `PersonID` em todo o projeto ***exceto*** nos arquivos de migrações de carimbo de data / hora no *migrações* pasta.</span><span class="sxs-lookup"><span data-stu-id="de57c-157">Next you need to change `InstructorID` to `PersonID` and `StudentID` to `PersonID` throughout the project ***except*** in the time-stamped migrations files in the *Migrations* folder.</span></span> <span data-ttu-id="de57c-158">Para fazer isso você localizar e abrir apenas os arquivos que precisam ser alteradas e executar uma alteração global em arquivos abertos.</span><span class="sxs-lookup"><span data-stu-id="de57c-158">To do that you'll find and open only the files that need to be changed, then perform a global change on the opened files.</span></span> <span data-ttu-id="de57c-159">O único arquivo na *migrações* pasta deve ser alterada é *Migrations\Configuration.cs.*</span><span class="sxs-lookup"><span data-stu-id="de57c-159">The only file in the *Migrations* folder you should change is *Migrations\Configuration.cs.*</span></span>

1. > [!IMPORTANT]
   > <span data-ttu-id="de57c-160">Comece fechando todos os arquivos abertos no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de57c-160">Begin by closing all the open files in Visual Studio.</span></span>
2. <span data-ttu-id="de57c-161">Clique em **localizar e substituir – localizar todos os arquivos** na **editar** menu e, em seguida, pesquise todos os arquivos no projeto que contêm `InstructorID`.</span><span class="sxs-lookup"><span data-stu-id="de57c-161">Click **Find and Replace -- Find all Files** in the **Edit** menu, and then search for all files in the project that contain `InstructorID`.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. <span data-ttu-id="de57c-162">Abra cada arquivo na **Find Results** janela ***exceto*** o &lt;carimbo de data / hora&gt;*\_. CS* arquivos de migração na *Migrações* pasta, clicando duas vezes em uma linha para cada arquivo.</span><span class="sxs-lookup"><span data-stu-id="de57c-162">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. <span data-ttu-id="de57c-163">Abra o **substituir nos arquivos** caixa de diálogo e altere **examinar** para **todos os documentos abertos**.</span><span class="sxs-lookup"><span data-stu-id="de57c-163">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
5. <span data-ttu-id="de57c-164">Use o **substituir nos arquivos** caixa de diálogo para alterar todas as `InstructorID` para `PersonID.`</span><span class="sxs-lookup"><span data-stu-id="de57c-164">Use the **Replace in Files** dialog to change all `InstructorID` to `PersonID.`</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. <span data-ttu-id="de57c-165">Localizar todos os arquivos no projeto que contêm `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="de57c-165">Find all the files in the project that contain `StudentID`.</span></span>
7. <span data-ttu-id="de57c-166">Abra cada arquivo na **Find Results** janela ***exceto*** o &lt;carimbo de data / hora&gt;*\_\*. CS* arquivos de migração no *migrações* pasta, clicando duas vezes em uma linha para cada arquivo.</span><span class="sxs-lookup"><span data-stu-id="de57c-166">Open each file in the **Find Results** window ***except*** the &lt;time-stamp&gt;*\_\*.cs* migration files in the *Migrations* folder, by double-clicking one line for each file.</span></span>  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. <span data-ttu-id="de57c-167">Abra o **substituir nos arquivos** caixa de diálogo e altere **examinar** para **todos os documentos abertos**.</span><span class="sxs-lookup"><span data-stu-id="de57c-167">Open the **Replace in Files** dialog and change **Look in** to **All Open Documents**.</span></span>
9. <span data-ttu-id="de57c-168">Use o **substituir nos arquivos** caixa de diálogo para alterar todas `StudentID` para `PersonID`.</span><span class="sxs-lookup"><span data-stu-id="de57c-168">Use the **Replace in Files** dialog to change all `StudentID` to `PersonID`.</span></span>   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. <span data-ttu-id="de57c-169">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="de57c-169">Build the project.</span></span>

<span data-ttu-id="de57c-170">(Observe que isso demonstra um *desvantagem* do `classnameID` padrão para chaves primárias de nomenclatura.</span><span class="sxs-lookup"><span data-stu-id="de57c-170">(Note that this demonstrates a *disadvantage* of the `classnameID` pattern for naming primary keys.</span></span> <span data-ttu-id="de57c-171">Se você tivesse chamado ID, chaves primárias sem prefixar o nome de classe *nenhum* renomeando seria necessário agora.)</span><span class="sxs-lookup"><span data-stu-id="de57c-171">If you had named primary keys ID without prefixing the class name, *no* renaming would be necessary now.)</span></span>

## <a name="create-and-update-a-migrations-file"></a><span data-ttu-id="de57c-172">Criar e atualizar um arquivo de migrações</span><span class="sxs-lookup"><span data-stu-id="de57c-172">Create and Update a Migrations File</span></span>

<span data-ttu-id="de57c-173">No pacote Manager Console (PMC), digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="de57c-173">In the Package Manager Console (PMC), enter the following command:</span></span>

`Add-Migration Inheritance`

<span data-ttu-id="de57c-174">Execute o `Update-Database` comando no PMC.</span><span class="sxs-lookup"><span data-stu-id="de57c-174">Run the `Update-Database` command in the PMC.</span></span> <span data-ttu-id="de57c-175">O comando falhará neste momento porque temos os dados existentes que as migrações não sabe como tratar.</span><span class="sxs-lookup"><span data-stu-id="de57c-175">The command will fail at this point because we have existing data that migrations doesn't know how to handle.</span></span> <span data-ttu-id="de57c-176">Você obterá o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="de57c-176">You get the following error:</span></span>

<span data-ttu-id="de57c-177">*A instrução ALTER TABLE entrou em conflito com a restrição FOREIGN KEY "FK\_dbo. Departamento\_dbo. Pessoa\_PersonID ". O conflito ocorreu no banco de dados "ContosoUniversity" tabela "dbo. "Person", coluna 'PersonID'.*</span><span class="sxs-lookup"><span data-stu-id="de57c-177">*The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK\_dbo.Department\_dbo.Person\_PersonID". The conflict occurred in database "ContosoUniversity", table "dbo.Person", column 'PersonID'.*</span></span>

<span data-ttu-id="de57c-178">Abra *migrações\&lt; carimbo de hora&gt;\_Inheritance.cs* e substitua o `Up` método com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="de57c-178">Open *Migrations\&lt;timestamp&gt;\_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

<span data-ttu-id="de57c-179">Execute o `update-database` comando novamente.</span><span class="sxs-lookup"><span data-stu-id="de57c-179">Run the `update-database` command again.</span></span>

> [!NOTE]
> <span data-ttu-id="de57c-180">É possível receber outros erros durante a migração de dados e fazer alterações de esquema.</span><span class="sxs-lookup"><span data-stu-id="de57c-180">It's possible to get other errors when migrating data and making schema changes.</span></span> <span data-ttu-id="de57c-181">Se você obtiver erros de migração não é possível resolver, você pode continuar com o tutorial, alterando a cadeia de conexão na *Web. config* arquivo ou excluir o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="de57c-181">If you get migration errors you can't resolve, you can continue with the tutorial by changing the connection string in the *Web.config* file or deleting the database.</span></span> <span data-ttu-id="de57c-182">A abordagem mais simples é renomear o banco de dados do *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="de57c-182">The simplest approach is to rename the database in the *Web.config* file.</span></span> <span data-ttu-id="de57c-183">Por exemplo, altere o nome de banco de dados para o CU\_testar, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="de57c-183">For example, change the database name to CU\_test as shown in the following example:</span></span>
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> <span data-ttu-id="de57c-184">Com um novo banco de dados, não há nenhum dado para migrar e o `update-database` comando é muito mais provável de ser concluído sem erros.</span><span class="sxs-lookup"><span data-stu-id="de57c-184">With a new database, there is no data to migrate, and the `update-database` command is much more likely to complete without errors.</span></span> <span data-ttu-id="de57c-185">Para obter instruções sobre como excluir o banco de dados, consulte [como descartar um banco de dados do Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="de57c-185">For instructions on how to delete the database, see [How to Drop a Database from Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).</span></span> <span data-ttu-id="de57c-186">Se você usar essa abordagem para continuar com o tutorial, ignore a etapa de implantação no final deste tutorial, uma vez que o site implantado obteria o mesmo erro quando ele é automaticamente executado as migrações.</span><span class="sxs-lookup"><span data-stu-id="de57c-186">If you take this approach in order to continue with the tutorial, skip the deployment step at the end of this tutorial, since the deployed site would get the same error when it runs migrations automatically.</span></span> <span data-ttu-id="de57c-187">Se você quiser solucionar o erro migrações, o melhor recurso é um dos fóruns do Entity Framework ou StackOverflow.com.</span><span class="sxs-lookup"><span data-stu-id="de57c-187">If you want to troubleshoot a migrations error, the best resource is one of the Entity Framework forums or StackOverflow.com.</span></span>


## <a name="testing"></a><span data-ttu-id="de57c-188">Testes</span><span class="sxs-lookup"><span data-stu-id="de57c-188">Testing</span></span>

<span data-ttu-id="de57c-189">Executar o site e tente várias páginas.</span><span class="sxs-lookup"><span data-stu-id="de57c-189">Run the site and try various pages.</span></span> <span data-ttu-id="de57c-190">Tudo funciona da mesma maneira que antes.</span><span class="sxs-lookup"><span data-stu-id="de57c-190">Everything works the same as it did before.</span></span>

<span data-ttu-id="de57c-191">Na **Gerenciador de servidores** expandir **SchoolContext** e, em seguida, **tabelas**, e você verá que o **aluno** e **instrutor**  tabelas foram substituídas por um **pessoa** tabela.</span><span class="sxs-lookup"><span data-stu-id="de57c-191">In **Server Explorer,** expand **SchoolContext** and then **Tables**, and you see that the **Student** and **Instructor** tables have been replaced by a **Person** table.</span></span> <span data-ttu-id="de57c-192">Expanda o **pessoa** tabela e você ver que ele tem todas as colunas que costumavam estar na **aluno** e **instrutor** tabelas.</span><span class="sxs-lookup"><span data-stu-id="de57c-192">Expand the **Person** table and you see that it has all of the columns that used to be in the **Student** and **Instructor** tables.</span></span>

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="de57c-194">Clique com o botão direito do mouse na tabela Person e, em seguida, clique em **Mostrar Dados da Tabela** para ver a coluna discriminatória.</span><span class="sxs-lookup"><span data-stu-id="de57c-194">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="de57c-195">O diagrama a seguir ilustra a estrutura do novo banco de dados de escola:</span><span class="sxs-lookup"><span data-stu-id="de57c-195">The following diagram illustrates the structure of the new School database:</span></span>

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a><span data-ttu-id="de57c-197">Resumo</span><span class="sxs-lookup"><span data-stu-id="de57c-197">Summary</span></span>

<span data-ttu-id="de57c-198">Herança de tabela por hierarquia agora foi implementada para o `Person`, `Student`, e `Instructor` classes.</span><span class="sxs-lookup"><span data-stu-id="de57c-198">Table-per-hierarchy inheritance has now been implemented for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="de57c-199">Para obter mais informações sobre esta e outras estruturas de herança, consulte [estratégias de mapeamento de herança](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) no blog de Morteza Manavi.</span><span class="sxs-lookup"><span data-stu-id="de57c-199">For more information about this and other inheritance structures, see [Inheritance Mapping Strategies](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) on Morteza Manavi's blog.</span></span> <span data-ttu-id="de57c-200">O próximo tutorial, você verá algumas maneiras de implementar o repositório e unidade de padrões de trabalho.</span><span class="sxs-lookup"><span data-stu-id="de57c-200">In the next tutorial you'll see some ways to implement the repository and unit of work patterns.</span></span>

<span data-ttu-id="de57c-201">Links para outros recursos do Entity Framework podem ser encontradas na [mapa de conteúdo de acesso do ASP.NET dados](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="de57c-201">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="de57c-202">[Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Próximo](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="de57c-202">[Previous](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)</span></span>
