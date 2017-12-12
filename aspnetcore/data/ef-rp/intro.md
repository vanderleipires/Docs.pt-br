---
title: "Páginas Razor com o Entity Framework Core - 1 Tutorial de 8"
author: rick-anderson
description: "Mostra como criar um aplicativo de páginas Razor usando o Entity Framework Core"
keywords: Tutorial do ASP.NET Core, Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: d3bcf9aaf7fa809825a0ba8631ee52d3860b090d
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2017
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="64848-104">Guia de Introdução com páginas Razor e o Entity Framework Core usando o Visual Studio (1 a 8)</span><span class="sxs-lookup"><span data-stu-id="64848-104">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="64848-105">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="64848-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="64848-106">O aplicativo de web de exemplo Contoso University demonstra como criar aplicativos de web MVC do ASP.NET Core 2.0 usando o núcleo do Entity Framework (EF) 2.0 e o Visual Studio de 2017.</span><span class="sxs-lookup"><span data-stu-id="64848-106">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="64848-107">O aplicativo de exemplo é um site de uma universidade Contoso fictícia.</span><span class="sxs-lookup"><span data-stu-id="64848-107">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="64848-108">Ele inclui a funcionalidade como admissão do aluno, criação de curso e atribuições do instrutor.</span><span class="sxs-lookup"><span data-stu-id="64848-108">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="64848-109">Esta página é o primeiro em uma série de tutoriais que explicam como criar o aplicativo de exemplo Contoso University.</span><span class="sxs-lookup"><span data-stu-id="64848-109">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="64848-110">Baixar ou exibir o aplicativo concluído.</span><span class="sxs-lookup"><span data-stu-id="64848-110">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="64848-111">[Instruções de download](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="64848-111">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64848-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="64848-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a><span data-ttu-id="64848-113">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="64848-113">Troubleshooting</span></span>

<span data-ttu-id="64848-114">Se você tiver um problema que não é possível resolver, você pode encontrar a solução geralmente comparando seu código para o [concluída estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) ou [projeto concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="64848-114">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="64848-115">Para obter uma lista de erros comuns e como resolvê-los, consulte [a seção de solução de problemas do tutorial do último na série](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="64848-115">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="64848-116">Se você não encontrar o que você precisa lá, você pode postar uma pergunta em StackOverflow.com para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="64848-116">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="64848-117">Esta série de tutoriais amplia o que é feito nos tutoriais anteriores.</span><span class="sxs-lookup"><span data-stu-id="64848-117">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="64848-118">Considere a possibilidade de salvar uma cópia do projeto após cada tutorial foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="64848-118">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="64848-119">Se você tiver problemas, você poderá começar desde o tutorial anterior em vez de voltar ao início.</span><span class="sxs-lookup"><span data-stu-id="64848-119">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="64848-120">Como alternativa, você pode baixar um [concluída estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) e inicie novamente usando o estágio concluído.</span><span class="sxs-lookup"><span data-stu-id="64848-120">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="64848-121">O aplicativo web Contoso University</span><span class="sxs-lookup"><span data-stu-id="64848-121">The Contoso University web app</span></span>

<span data-ttu-id="64848-122">O aplicativo compilado nos tutoriais é um site university básico.</span><span class="sxs-lookup"><span data-stu-id="64848-122">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="64848-123">Os usuários podem exibir e atualizar aluno, curso e informações do instrutor.</span><span class="sxs-lookup"><span data-stu-id="64848-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="64848-124">Aqui estão algumas das telas do criado no tutorial.</span><span class="sxs-lookup"><span data-stu-id="64848-124">Here are a few of the screens created in the tutorial.</span></span>

![Página de índice de alunos](intro/_static/students-index.png)

![Página de edição de alunos](intro/_static/student-edit.png)

<span data-ttu-id="64848-127">O estilo de interface do usuário deste site é quase o que é gerado pelos modelos internos.</span><span class="sxs-lookup"><span data-stu-id="64848-127">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="64848-128">É o foco de tutorial no EF Core com páginas Razor, não a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="64848-128">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="64848-129">Criar um aplicativo da web de páginas Razor</span><span class="sxs-lookup"><span data-stu-id="64848-129">Create a Razor Pages web app</span></span>

* <span data-ttu-id="64848-130">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="64848-130">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="64848-131">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="64848-131">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="64848-132">Nomeie o projeto **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="64848-132">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="64848-133">É importante para o nome do projeto *ContosoUniversity* para os namespaces corresponda ao código é copiar/colar.</span><span class="sxs-lookup"><span data-stu-id="64848-133">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="64848-134">![novo aplicativo Web ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="64848-134">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="64848-135">Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="64848-135">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="64848-136">![Aplicativo Web (Páginas do Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="64848-136">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="64848-137">Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador</span><span class="sxs-lookup"><span data-stu-id="64848-137">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="64848-138">Definir o estilo de site</span><span class="sxs-lookup"><span data-stu-id="64848-138">Set up the site style</span></span>

<span data-ttu-id="64848-139">Algumas alterações de configurar o menu de site, o layout e a página inicial.</span><span class="sxs-lookup"><span data-stu-id="64848-139">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="64848-140">Abra *Pages/_Layout.cshtml* e faça as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="64848-140">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="64848-141">Alterar cada ocorrência de "ContosoUniversity" para "Contoso University."</span><span class="sxs-lookup"><span data-stu-id="64848-141">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="64848-142">Há três ocorrências.</span><span class="sxs-lookup"><span data-stu-id="64848-142">There are three occurrences.</span></span>

* <span data-ttu-id="64848-143">Adicionar entradas de menu de **alunos**, **cursos**, **instrutores**, e **departamentos**e exclua o **contato** entrada de menu.</span><span class="sxs-lookup"><span data-stu-id="64848-143">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="64848-144">As alterações são realçadas.</span><span class="sxs-lookup"><span data-stu-id="64848-144">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-39,47&range=1-50)]

<span data-ttu-id="64848-145">Em *Views/Home/Index.cshtml*, substitua o conteúdo do arquivo com o código a seguir para substituir o texto sobre o ASP.NET e MVC com o texto sobre este aplicativo:</span><span class="sxs-lookup"><span data-stu-id="64848-145">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="64848-146">Pressione CTRL+F5 para executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="64848-146">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="64848-147">A home page é exibida com as guias criadas nos tutoriais a seguir:</span><span class="sxs-lookup"><span data-stu-id="64848-147">The home page is displayed with tabs created in the following tutorials:</span></span>

![Home page do Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="64848-149">Criar o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="64848-149">Create the data model</span></span>

<span data-ttu-id="64848-150">Crie classes de entidade para o aplicativo da Contoso University.</span><span class="sxs-lookup"><span data-stu-id="64848-150">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="64848-151">Comece com as três seguintes entidades:</span><span class="sxs-lookup"><span data-stu-id="64848-151">Start with the following three entities:</span></span>

![Diagrama de modelo de dados do aluno de registro de curso](intro/_static/data-model-diagram.png)

<span data-ttu-id="64848-153">Há uma relação um-para-muitos entre `Student` e `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="64848-153">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="64848-154">Há uma relação um-para-muitos entre `Course` e `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="64848-154">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="64848-155">Um aluno pode registrar qualquer número de cursos.</span><span class="sxs-lookup"><span data-stu-id="64848-155">A student can enroll in any number of courses.</span></span> <span data-ttu-id="64848-156">Um curso pode ter qualquer número de alunos registrados nele.</span><span class="sxs-lookup"><span data-stu-id="64848-156">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="64848-157">As seções a seguir, uma classe para cada uma dessas entidades é criada.</span><span class="sxs-lookup"><span data-stu-id="64848-157">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="64848-158">A entidade do aluno</span><span class="sxs-lookup"><span data-stu-id="64848-158">The Student entity</span></span>

![Diagrama de entidade do aluno](intro/_static/student-entity.png)

<span data-ttu-id="64848-160">Criar um *modelos* pasta.</span><span class="sxs-lookup"><span data-stu-id="64848-160">Create a *Models* folder.</span></span> <span data-ttu-id="64848-161">No *modelos* pasta, crie um arquivo de classe chamado *Student.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="64848-161">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="64848-162">O `ID` propriedade torna-se a coluna de chave primária da tabela de banco de dados que corresponde a essa classe.</span><span class="sxs-lookup"><span data-stu-id="64848-162">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="64848-163">Por padrão, o núcleo de EF interpreta uma propriedade denominada `ID` ou `classnameID` como a chave primária.</span><span class="sxs-lookup"><span data-stu-id="64848-163">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="64848-164">O `Enrollments` propriedade é uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="64848-164">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="64848-165">Link de propriedades de navegação para outras entidades que estão relacionados a esta entidade.</span><span class="sxs-lookup"><span data-stu-id="64848-165">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="64848-166">Nesse caso, o `Enrollments` propriedade de um `Student entity` mantém todos o `Enrollment` entidades relacionadas ao `Student`.</span><span class="sxs-lookup"><span data-stu-id="64848-166">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="64848-167">Por exemplo, se uma linha de Student no banco de dados tem dois relacionados linhas de registro, o `Enrollments` propriedade de navegação contém dois `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="64848-167">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="64848-168">Um relacionados `Enrollment` linha é uma linha que contém o valor de chave primária que student no `StudentID` coluna.</span><span class="sxs-lookup"><span data-stu-id="64848-168">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="64848-169">Por exemplo, suponha que o aluno com ID = 1 tem duas linhas `Enrollment` tabela.</span><span class="sxs-lookup"><span data-stu-id="64848-169">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="64848-170">O `Enrollment` tabela tem duas linhas com `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="64848-170">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="64848-171">`StudentID`é uma chave estrangeira no `Enrollment` tabela que especifica o aluno no `Student` tabela.</span><span class="sxs-lookup"><span data-stu-id="64848-171">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="64848-172">Se uma propriedade de navegação pode conter várias entidades, a propriedade de navegação deve ser um tipo de lista, como `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="64848-172">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="64848-173">`ICollection<T>`pode ser especificado, ou um tipo, como `List<T>` ou `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="64848-173">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="64848-174">Quando `ICollection<T>` é usada, o núcleo do EF cria um `HashSet<T>` coleção por padrão.</span><span class="sxs-lookup"><span data-stu-id="64848-174">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="64848-175">Propriedades de navegação que contêm várias entidades provenientes de relações muitos-para-muitos e um-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="64848-175">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="64848-176">A entidade de registro</span><span class="sxs-lookup"><span data-stu-id="64848-176">The Enrollment entity</span></span>

![Diagrama de entidade do registro](intro/_static/enrollment-entity.png)

<span data-ttu-id="64848-178">No *modelos* pasta, criar *Enrollment.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="64848-178">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="64848-179">O `EnrollmentID` propriedade é a chave primária.</span><span class="sxs-lookup"><span data-stu-id="64848-179">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="64848-180">Esta entidade usa o `classnameID` padrão em vez de `ID` como o `Student` entidade.</span><span class="sxs-lookup"><span data-stu-id="64848-180">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="64848-181">Normalmente, os desenvolvedores escolher um padrão e usá-lo em todo o modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="64848-181">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="64848-182">Um tutorial posterior, usando uma ID sem classname é mostrado para tornar mais fácil de implementar a herança no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="64848-182">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="64848-183">O `Grade` propriedade é um `enum`.</span><span class="sxs-lookup"><span data-stu-id="64848-183">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="64848-184">O ponto de interrogação após o `Grade` declaração de tipo indica que o `Grade` propriedade é anulável.</span><span class="sxs-lookup"><span data-stu-id="64848-184">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="64848-185">Uma classificação que é null é diferente de uma classificação zero - nulo significa uma série não é conhecida ou ainda não foi atribuída.</span><span class="sxs-lookup"><span data-stu-id="64848-185">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="64848-186">O `StudentID` propriedade é uma chave estrangeira e a propriedade de navegação correspondente é `Student`.</span><span class="sxs-lookup"><span data-stu-id="64848-186">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="64848-187">Um `Enrollment` entidade está associada um `Student` entidade, portanto, a propriedade contém um único `Student` entidade.</span><span class="sxs-lookup"><span data-stu-id="64848-187">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="64848-188">O `Student` entidade difere do `Student.Enrollments` propriedade de navegação, que contém várias `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="64848-188">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="64848-189">O `CourseID` propriedade é uma chave estrangeira e a propriedade de navegação correspondente é `Course`.</span><span class="sxs-lookup"><span data-stu-id="64848-189">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="64848-190">Um `Enrollment` entidade está associada um `Course` entidade.</span><span class="sxs-lookup"><span data-stu-id="64848-190">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="64848-191">EF Core interpreta uma propriedade como uma chave estrangeira, se ele é nomeado `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="64848-191">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="64848-192">Por exemplo,`StudentID` para o `Student` propriedade de navegação, como o `Student` chave primária da entidade é `ID`.</span><span class="sxs-lookup"><span data-stu-id="64848-192">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="64848-193">Propriedades de chave estrangeira também podem ser nomeadas `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="64848-193">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="64848-194">Por exemplo, `CourseID` desde o `Course` chave primária da entidade é `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="64848-194">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="64848-195">A entidade de curso</span><span class="sxs-lookup"><span data-stu-id="64848-195">The Course entity</span></span>

![Diagrama de entidades de curso](intro/_static/course-entity.png)

<span data-ttu-id="64848-197">No *modelos* pasta, criar *Course.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="64848-197">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="64848-198">O `Enrollments` propriedade é uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="64848-198">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="64848-199">Um `Course` entidade pode estar relacionada a qualquer número de `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="64848-199">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="64848-200">O `DatabaseGenerated` atributo permite que o aplicativo especificar a chave primária em vez de ter o banco de dados gerá-lo.</span><span class="sxs-lookup"><span data-stu-id="64848-200">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="64848-201">Criar o contexto de banco de dados SchoolContext</span><span class="sxs-lookup"><span data-stu-id="64848-201">Create the SchoolContext DB context</span></span>

<span data-ttu-id="64848-202">A classe principal que coordena a funcionalidade principal do EF de um modelo de dados é a classe de contexto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64848-202">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="64848-203">O contexto de dados é derivado de `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="64848-203">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="64848-204">O contexto de dados especifica quais entidades são incluídas no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="64848-204">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="64848-205">Neste projeto, a classe é nomeada `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="64848-205">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="64848-206">Na pasta do projeto, crie uma pasta chamada *dados*.</span><span class="sxs-lookup"><span data-stu-id="64848-206">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="64848-207">No *dados* criar pasta *SchoolContext.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="64848-207">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="64848-208">Esse código cria um `DbSet` propriedade para cada conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="64848-208">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="64848-209">Na terminologia de EF principais:</span><span class="sxs-lookup"><span data-stu-id="64848-209">In EF Core terminology:</span></span>

* <span data-ttu-id="64848-210">Uma entidade definida normalmente corresponde a uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64848-210">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="64848-211">Uma entidade corresponde a uma linha na tabela.</span><span class="sxs-lookup"><span data-stu-id="64848-211">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="64848-212">`DbSet<Enrollment>`e `DbSet<Course>` pode ser omitido.</span><span class="sxs-lookup"><span data-stu-id="64848-212">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="64848-213">EF Core inclui-los ou implicitamente porque o `Student` referências de entidade de `Enrollment` entidade e o `Enrollment` referências de entidade o `Course` entidade.</span><span class="sxs-lookup"><span data-stu-id="64848-213">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="64848-214">Para este tutorial, mantenha `DbSet<Enrollment>` e `DbSet<Course>` no `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="64848-214">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="64848-215">Quando o banco de dados é criado, Core EF cria tabelas com nomes iguais a `DbSet` nomes de propriedade.</span><span class="sxs-lookup"><span data-stu-id="64848-215">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="64848-216">Nomes de propriedade para coleções são normalmente plurais (alunos em vez de aluno).</span><span class="sxs-lookup"><span data-stu-id="64848-216">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="64848-217">Os desenvolvedores não concordo sobre se os nomes de tabela devem ser plural.</span><span class="sxs-lookup"><span data-stu-id="64848-217">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="64848-218">Para esses tutoriais, o comportamento padrão é substituído pela especificação de nomes de tabela única no DbContext.</span><span class="sxs-lookup"><span data-stu-id="64848-218">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="64848-219">Para especificar nomes de tabela única, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="64848-219">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="64848-220">Registrar o contexto de injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="64848-220">Register the context with dependency injection</span></span>

<span data-ttu-id="64848-221">Inclui o ASP.NET Core [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="64848-221">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="64848-222">Serviços (como o contexto de DB de núcleo EF) são registrados com injeção de dependência durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="64848-222">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="64848-223">Componentes que exigem que esses serviços (como páginas Razor) são fornecidas esses serviços por meio de parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="64848-223">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="64848-224">O código de construtor que obtém uma instância de contexto do banco de dados é mostrado posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="64848-224">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="64848-225">Para registrar `SchoolContext` como um serviço, abra *Startup.cs*e adicione as linhas destacadas para o `ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="64848-225">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="64848-226">O nome da cadeia de caracteres de conexão é passado para o contexto chamando um método em um `DbContextOptionsBuilder` objeto.</span><span class="sxs-lookup"><span data-stu-id="64848-226">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="64848-227">Para desenvolvimento local, o [sistema de configuração do ASP.NET Core](xref:fundamentals/configuration/index) lê a cadeia de caracteres de conexão a *appSettings. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="64848-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="64848-228">Adicionar `using` instruções para `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore` namespaces.</span><span class="sxs-lookup"><span data-stu-id="64848-228">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="64848-229">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="64848-229">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="64848-230">Abra o *appSettings. JSON* de arquivos e adicionar uma cadeia de caracteres de conexão, conforme mostrado no código a seguir:</span><span class="sxs-lookup"><span data-stu-id="64848-230">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="64848-231">A cadeia de caracteres de conexão anterior usa `ConnectRetryCount=0` para evitar [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) de deslocamento.</span><span class="sxs-lookup"><span data-stu-id="64848-231">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="64848-232">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="64848-232">SQL Server Express LocalDB</span></span>

<span data-ttu-id="64848-233">A cadeia de caracteres de conexão Especifica um banco de dados do SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="64848-233">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="64848-234">LocalDB é uma versão leve do mecanismo de banco de dados do SQL Server Express e destina-se ao desenvolvimento de aplicativos, não o uso de produção.</span><span class="sxs-lookup"><span data-stu-id="64848-234">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="64848-235">O LocalDB é iniciado sob demanda e executado no modo de usuário e, portanto, não há nenhuma configuração complexa.</span><span class="sxs-lookup"><span data-stu-id="64848-235">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="64848-236">Por padrão, o LocalDB cria *. mdf* arquivos de banco de dados no `C:/Users/<user>` directory.</span><span class="sxs-lookup"><span data-stu-id="64848-236">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="64848-237">Adicione código para inicializar o banco de dados com dados de teste</span><span class="sxs-lookup"><span data-stu-id="64848-237">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="64848-238">EF principal cria um banco de dados vazio.</span><span class="sxs-lookup"><span data-stu-id="64848-238">EF Core creates an empty DB.</span></span> <span data-ttu-id="64848-239">Nesta seção, uma *semente* método é gravada para preenchê-la com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="64848-239">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="64848-240">No *dados* pasta, crie um novo arquivo de classe chamado *DbInitializer.cs* e adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="64848-240">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="64848-241">O código verifica se há algum dos alunos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64848-241">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="64848-242">Se não houver nenhum alunos no banco de dados, o banco de dados é propagado com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="64848-242">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="64848-243">Ele carrega os dados de teste em matrizes em vez de `List<T>` coleções para otimizar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="64848-243">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="64848-244">O `EnsureCreated` método cria automaticamente o banco de dados para o contexto do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64848-244">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="64848-245">Se o banco de dados existir, `EnsureCreated` retorna sem modificar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64848-245">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="64848-246">Em *Program.cs*, modifique o `Main` método para fazer o seguinte:</span><span class="sxs-lookup"><span data-stu-id="64848-246">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="64848-247">Obtenha uma instância de contexto do banco de dados do contêiner de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="64848-247">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="64848-248">Chame o método de propagação, transmitindo a ele o contexto.</span><span class="sxs-lookup"><span data-stu-id="64848-248">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="64848-249">Descarte o contexto quando o método de propagação é concluído.</span><span class="sxs-lookup"><span data-stu-id="64848-249">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="64848-250">O código a seguir mostra a atualização *Program.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="64848-250">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="64848-251">A primeira vez que o aplicativo é executado, o banco de dados é criado e propagado com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="64848-251">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="64848-252">Quando o modelo de dados é atualizado:</span><span class="sxs-lookup"><span data-stu-id="64848-252">When the data model is updated:</span></span>
* <span data-ttu-id="64848-253">Exclua o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64848-253">Delete the DB.</span></span>
* <span data-ttu-id="64848-254">Atualize o método de propagação.</span><span class="sxs-lookup"><span data-stu-id="64848-254">Update the seed method.</span></span>
* <span data-ttu-id="64848-255">Executar o aplicativo e um novo banco de dados de propagação é criado.</span><span class="sxs-lookup"><span data-stu-id="64848-255">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="64848-256">Em tutoriais subsequentes, o banco de dados é atualizado quando os dados de modelo alterações, sem excluir e recriar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64848-256">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="64848-257">Adicionar ferramentas de scaffold</span><span class="sxs-lookup"><span data-stu-id="64848-257">Add scaffold tooling</span></span>

<span data-ttu-id="64848-258">Nesta seção, o Console de Gerenciador de pacote (PMC) é usado para adicionar o pacote de geração de código do Visual Studio web.</span><span class="sxs-lookup"><span data-stu-id="64848-258">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="64848-259">Esse pacote é necessário para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="64848-259">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="64848-260">No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="64848-260">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="64848-261">No pacote Manager Console (PMC), digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="64848-261">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="64848-262">O comando anterior adiciona os pacotes do NuGet para o arquivo *. csproj:</span><span class="sxs-lookup"><span data-stu-id="64848-262">The previous command adds the NuGet packages to the *.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="64848-263">O modelo de Scaffold</span><span class="sxs-lookup"><span data-stu-id="64848-263">Scaffold the model</span></span>

* <span data-ttu-id="64848-264">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="64848-264">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="64848-265">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="64848-265">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="64848-266">Se o seguinte erro é gerado:</span><span class="sxs-lookup"><span data-stu-id="64848-266">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="64848-267">Execute o comando novamente e deixar um comentário na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="64848-267">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="64848-268">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="64848-268">Build the project.</span></span> <span data-ttu-id="64848-269">A compilação gera erros, como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="64848-269">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="64848-270">Globalmente altere `_context.Student` para `_context.Students` (ou seja, adicionar um "s" para `Student`).</span><span class="sxs-lookup"><span data-stu-id="64848-270">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="64848-271">7 ocorrências forem encontradas e atualizadas.</span><span class="sxs-lookup"><span data-stu-id="64848-271">7 occurrences are found and updated.</span></span> <span data-ttu-id="64848-272">Esperamos que corrigir [esse bug](https://github.com/aspnet/Scaffolding/issues/633) na próxima versão.</span><span class="sxs-lookup"><span data-stu-id="64848-272">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="64848-273">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="64848-273">Test the app</span></span>

<span data-ttu-id="64848-274">Execute o aplicativo e selecione o **alunos** link.</span><span class="sxs-lookup"><span data-stu-id="64848-274">Run the app and select the **Students** link.</span></span> <span data-ttu-id="64848-275">Dependendo da largura do navegador, o **alunos** link aparece na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="64848-275">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="64848-276">Se o **alunos** link não estiver visível, clique no ícone de navegação no canto superior direito.</span><span class="sxs-lookup"><span data-stu-id="64848-276">If the **Students** link is not visible, click the navigation icon in the upper right corner.</span></span>

![Página inicial da Contoso University estreita](intro/_static/home-page-narrow.png)

<span data-ttu-id="64848-278">Teste o **criar**, **editar**, e **detalhes** links.</span><span class="sxs-lookup"><span data-stu-id="64848-278">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="64848-279">Exibir o banco de dados</span><span class="sxs-lookup"><span data-stu-id="64848-279">View the DB</span></span>

<span data-ttu-id="64848-280">Quando o aplicativo é iniciado, `DbInitializer.Initialize` chamadas `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="64848-280">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="64848-281">`EnsureCreated`detecta se o banco de dados existe e cria um, se necessário.</span><span class="sxs-lookup"><span data-stu-id="64848-281">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="64848-282">Se não houver nenhum alunos no banco de dados, o `Initialize` método adiciona os alunos.</span><span class="sxs-lookup"><span data-stu-id="64848-282">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="64848-283">Abra **Pesquisador de objetos do SQL Server** (SSOX) da **exibição** menu do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="64848-283">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="64848-284">No SSOX, clique em **(localdb) \MSSQLLocalDB > bancos de dados > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="64848-284">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="64848-285">Expanda o **tabelas** nó.</span><span class="sxs-lookup"><span data-stu-id="64848-285">Expand the **Tables** node.</span></span>

<span data-ttu-id="64848-286">Com o botão direito do **aluno** de tabela e clique em **exibir dados** para ver as colunas criadas e as linhas inseridas na tabela.</span><span class="sxs-lookup"><span data-stu-id="64848-286">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="64848-287">O *. mdf* e *. ldf* arquivos de banco de dados estão no *C:\Users\<seu nome de usuário >* pasta.</span><span class="sxs-lookup"><span data-stu-id="64848-287">The *.mdf* and *.ldf* DB files are in the *C:\Users\<yourusername>* folder.</span></span>

<span data-ttu-id="64848-288">`EnsureCreated`é chamado no início do aplicativo, que permite que o fluxo de trabalho a seguir:</span><span class="sxs-lookup"><span data-stu-id="64848-288">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="64848-289">Exclua o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64848-289">Delete the DB.</span></span>
* <span data-ttu-id="64848-290">Alterar o esquema de banco de dados (por exemplo, adicionar um `EmailAddress` campo).</span><span class="sxs-lookup"><span data-stu-id="64848-290">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="64848-291">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="64848-291">Run the app.</span></span>

<span data-ttu-id="64848-292">`EnsureCreated`cria um banco de dados com o`EmailAddress` coluna.</span><span class="sxs-lookup"><span data-stu-id="64848-292">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="64848-293">Convenções</span><span class="sxs-lookup"><span data-stu-id="64848-293">Conventions</span></span>

<span data-ttu-id="64848-294">A quantidade de código escrito em ordem para EF principal criar um banco de dados completo é mínima devido ao uso de convenções ou suposições que torna o EF Core.</span><span class="sxs-lookup"><span data-stu-id="64848-294">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="64848-295">Os nomes de `DbSet` propriedades são usadas como nomes de tabela.</span><span class="sxs-lookup"><span data-stu-id="64848-295">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="64848-296">Para entidades que não é referenciadas por um `DbSet` propriedade, a classe da entidade nomes são usados como nomes de tabela.</span><span class="sxs-lookup"><span data-stu-id="64848-296">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="64848-297">Nomes de propriedade de entidade são usados para nomes de coluna.</span><span class="sxs-lookup"><span data-stu-id="64848-297">Entity property names are used for column names.</span></span>

* <span data-ttu-id="64848-298">Propriedades de entidade que são nomeadas ID ou classnameID são reconhecidas como propriedades de chave primárias.</span><span class="sxs-lookup"><span data-stu-id="64848-298">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="64848-299">Uma propriedade é interpretada como uma propriedade de chave estrangeira, se ele é nomeado  *<navigation property name> <primary key property name>*  (por exemplo, `StudentID` para o `Student` propriedade de navegação desde o `Student` é de chave primária da entidade `ID`).</span><span class="sxs-lookup"><span data-stu-id="64848-299">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="64848-300">Propriedades de chave estrangeira que podem ser nomeadas  *<primary key property name>*  (por exemplo, `EnrollmentID` desde o `Enrollment` chave primária da entidade é `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="64848-300">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="64848-301">Comportamento convencional pode ser substituído.</span><span class="sxs-lookup"><span data-stu-id="64848-301">Conventional behavior can be overridden.</span></span> <span data-ttu-id="64848-302">Por exemplo, os nomes de tabela podem ser especificados explicitamente, como mostrado anteriormente neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="64848-302">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="64848-303">Os nomes de coluna podem ser definidos explicitamente.</span><span class="sxs-lookup"><span data-stu-id="64848-303">The column names can be explicitly set.</span></span> <span data-ttu-id="64848-304">As chaves primária e estrangeira pode ser definida explicitamente.</span><span class="sxs-lookup"><span data-stu-id="64848-304">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="64848-305">Código assíncrono</span><span class="sxs-lookup"><span data-stu-id="64848-305">Asynchronous code</span></span>

<span data-ttu-id="64848-306">Programação assíncrona é o modo padrão do ASP.NET Core e EF Core.</span><span class="sxs-lookup"><span data-stu-id="64848-306">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="64848-307">Um servidor web tem um número limitado de threads disponíveis e, em situações de alta carga de todos os threads disponíveis podem estar em uso.</span><span class="sxs-lookup"><span data-stu-id="64848-307">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="64848-308">Quando isso acontece, o servidor não pode processar novas solicitações até que os threads são liberados.</span><span class="sxs-lookup"><span data-stu-id="64848-308">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="64848-309">Com código síncrono, muitos threads podem ser vinculados ao enquanto eles não são realmente fazendo qualquer trabalho porque está aguardando para e/s concluir.</span><span class="sxs-lookup"><span data-stu-id="64848-309">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="64848-310">Com um código assíncrono, quando um processo está aguardando e/s ser concluída, o thread é liberado para o servidor a ser usado para processar outras solicitações.</span><span class="sxs-lookup"><span data-stu-id="64848-310">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="64848-311">Como resultado, o código assíncrono permite que os recursos de servidor a ser usado com mais eficiência, e o servidor está habilitado para manipular mais tráfego sem atrasos.</span><span class="sxs-lookup"><span data-stu-id="64848-311">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="64848-312">Código assíncrono apresentam uma pequena quantidade de sobrecarga em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="64848-312">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="64848-313">Para situações de tráfego baixo, o impacto no desempenho é insignificante, durante situações de alto tráfego, a melhoria de desempenho potencial é significativa.</span><span class="sxs-lookup"><span data-stu-id="64848-313">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="64848-314">No código a seguir, o `async` palavra-chave, `Task<T>` retornar valor, `await` palavra-chave, e `ToListAsync` método tornar o código executar de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="64848-314">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="64848-315">O `async` palavra-chave informa ao compilador para:</span><span class="sxs-lookup"><span data-stu-id="64848-315">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="64848-316">Gere retornos de chamada para partes do corpo do método.</span><span class="sxs-lookup"><span data-stu-id="64848-316">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="64848-317">Criar automaticamente o [tarefa](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) objeto que é retornado.</span><span class="sxs-lookup"><span data-stu-id="64848-317">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that is returned.</span></span> <span data-ttu-id="64848-318">Para obter mais informações, consulte [o tipo de retorno de tarefa](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="64848-318">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="64848-319">Tipo de retorno de implícita `Task` representa um trabalho em andamento.</span><span class="sxs-lookup"><span data-stu-id="64848-319">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="64848-320">O `await` palavra-chave faz com que o compilador dividir o método em duas partes.</span><span class="sxs-lookup"><span data-stu-id="64848-320">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="64848-321">A primeira parte termina com a operação que é iniciada de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="64848-321">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="64848-322">A segunda parte é colocada em um método de retorno de chamada que é chamado quando a operação for concluída.</span><span class="sxs-lookup"><span data-stu-id="64848-322">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="64848-323">`ToListAsync`é a versão assíncrona do `ToList` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="64848-323">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="64848-324">Algumas coisas a serem consideradas ao escrever código assíncrono que usa EF Core:</span><span class="sxs-lookup"><span data-stu-id="64848-324">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="64848-325">Somente instruções que fazem com consultas ou comandos sejam enviados para o banco de dados que são executadas assincronamente.</span><span class="sxs-lookup"><span data-stu-id="64848-325">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="64848-326">Isso inclui, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, e `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="64848-326">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="64848-327">Ele não inclui instruções que apenas alterar uma `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="64848-327">It does not include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="64848-328">Um contexto de EF Core não é thread safe: não tenta realizar várias operações em paralelo.</span><span class="sxs-lookup"><span data-stu-id="64848-328">An EF Core context is not threaded safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="64848-329">Para tirar proveito dos benefícios de desempenho do código assíncrono, verifique se pacotes de biblioteca (como paginação) usam async se chamam métodos básicos de EF que enviam consultas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64848-329">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="64848-330">Para obter mais informações sobre a programação assíncrona em .NET, consulte [visão geral de Async](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="64848-330">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="64848-331">No tutorial de Avançar, CRUD básica (criar, ler, atualizar e excluir) operações são examinadas.</span><span class="sxs-lookup"><span data-stu-id="64848-331">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="64848-332">Avançar</span><span class="sxs-lookup"><span data-stu-id="64848-332">Next</span></span>](xref:data/ef-rp/crud)
