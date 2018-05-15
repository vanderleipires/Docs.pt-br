---
title: Páginas Razor com o Entity Framework Core no ASP.NET Core – Tutorial 1 de 8
author: rick-anderson
description: Mostra como criar um aplicativo das Páginas do Razor usando o Entity Framework Core
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 99a8d158c896566c2f6e6c22e4b37b1956e21cbf
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="458f7-103">Páginas Razor com o Entity Framework Core no ASP.NET Core – Tutorial 1 de 8</span><span class="sxs-lookup"><span data-stu-id="458f7-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="458f7-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="458f7-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="458f7-105">O aplicativo Web de exemplo Contoso University demonstra como criar aplicativos Web ASP.NET Core 2.0 MVC usando o EF (Entity Framework) Core 2.0 e o Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="458f7-105">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="458f7-106">O aplicativo de exemplo é um site de uma Contoso University fictícia.</span><span class="sxs-lookup"><span data-stu-id="458f7-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="458f7-107">Ele inclui funcionalidades como admissão de alunos, criação de cursos e atribuições de instrutor.</span><span class="sxs-lookup"><span data-stu-id="458f7-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="458f7-108">Esta página é a primeira de uma série de tutoriais que explica como criar o aplicativo de exemplo Contoso University.</span><span class="sxs-lookup"><span data-stu-id="458f7-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="458f7-109">Baixe ou exiba o aplicativo concluído.</span><span class="sxs-lookup"><span data-stu-id="458f7-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="458f7-110">[Instruções de download](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="458f7-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="458f7-111">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="458f7-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<span data-ttu-id="458f7-112">Familiaridade com as [Páginas do Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="458f7-112">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="458f7-113">Os novos programadores devem concluir a [Introdução às Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start) antes de começar esta série.</span><span class="sxs-lookup"><span data-stu-id="458f7-113">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="458f7-114">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="458f7-114">Troubleshooting</span></span>

<span data-ttu-id="458f7-115">Se você tem algum problema que não consegue resolver, geralmente é possível encontrar a solução comparando seu código com o [estágio concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots).</span><span class="sxs-lookup"><span data-stu-id="458f7-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots).</span></span> <span data-ttu-id="458f7-116">Para obter uma lista de erros comuns e como resolvê-los, consulte [a seção Solução de problemas do último tutorial da série](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="458f7-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="458f7-117">Caso não encontre o que precisa na seção, poste uma pergunta no [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) sobre o [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="458f7-117">If you don't find what you need there, you can post a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="458f7-118">Esta série de tutoriais baseia-se no que foi feito nos tutoriais anteriores.</span><span class="sxs-lookup"><span data-stu-id="458f7-118">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="458f7-119">Considere a possibilidade de salvar uma cópia do projeto após a conclusão bem-sucedida de cada tutorial.</span><span class="sxs-lookup"><span data-stu-id="458f7-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="458f7-120">Caso tenha problemas, comece novamente no tutorial anterior em vez de voltar ao início.</span><span class="sxs-lookup"><span data-stu-id="458f7-120">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="458f7-121">Como alternativa, você pode baixar um [estágio concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) e começar novamente usando o estágio concluído.</span><span class="sxs-lookup"><span data-stu-id="458f7-121">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="458f7-122">O aplicativo Web Contoso University</span><span class="sxs-lookup"><span data-stu-id="458f7-122">The Contoso University web app</span></span>

<span data-ttu-id="458f7-123">O aplicativo criado nesses tutoriais é um site básico de universidade.</span><span class="sxs-lookup"><span data-stu-id="458f7-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="458f7-124">Os usuários podem exibir e atualizar informações de alunos, cursos e instrutores.</span><span class="sxs-lookup"><span data-stu-id="458f7-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="458f7-125">Veja a seguir algumas das telas criadas no tutorial.</span><span class="sxs-lookup"><span data-stu-id="458f7-125">Here are a few of the screens created in the tutorial.</span></span>

![Página Índice de Alunos](intro/_static/students-index.png)

![Página Editar Alunos](intro/_static/student-edit.png)

<span data-ttu-id="458f7-128">O estilo de interface do usuário deste site é próximo ao que é gerado pelos modelos internos.</span><span class="sxs-lookup"><span data-stu-id="458f7-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="458f7-129">O foco do tutorial é o EF Core com as Páginas do Razor, não a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="458f7-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="458f7-130">Criar um aplicativo Web das Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="458f7-130">Create a Razor Pages web app</span></span>

* <span data-ttu-id="458f7-131">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="458f7-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="458f7-132">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="458f7-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="458f7-133">Nomeie o projeto **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="458f7-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="458f7-134">É importante nomear o projeto *ContosoUniversity* para que os namespaces sejam correspondentes quando o código for copiado/colado.</span><span class="sxs-lookup"><span data-stu-id="458f7-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="458f7-135">![novo aplicativo Web ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="458f7-135">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="458f7-136">Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="458f7-136">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="458f7-137">![Aplicativo Web (Páginas do Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="458f7-137">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="458f7-138">Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador</span><span class="sxs-lookup"><span data-stu-id="458f7-138">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="458f7-139">Configurar o estilo do site</span><span class="sxs-lookup"><span data-stu-id="458f7-139">Set up the site style</span></span>

<span data-ttu-id="458f7-140">Algumas alterações configuram o menu do site, o layout e a home page.</span><span class="sxs-lookup"><span data-stu-id="458f7-140">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="458f7-141">Abra *Pages/_Layout.cshtml* e faça as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="458f7-141">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="458f7-142">Altere cada ocorrência de "ContosoUniversity" para "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="458f7-142">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="458f7-143">Há três ocorrências.</span><span class="sxs-lookup"><span data-stu-id="458f7-143">There are three occurrences.</span></span>

* <span data-ttu-id="458f7-144">Adicione entradas de menu para **Alunos**, **Cursos**, **Instrutores** e **Departamentos** e exclua a entrada de menu **Contato**.</span><span class="sxs-lookup"><span data-stu-id="458f7-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="458f7-145">As alterações são realçadas.</span><span class="sxs-lookup"><span data-stu-id="458f7-145">The changes are highlighted.</span></span> <span data-ttu-id="458f7-146">(Toda a marcação *não* é exibida.)</span><span class="sxs-lookup"><span data-stu-id="458f7-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="458f7-147">Em *Pages/Index.cshtml*, substitua o conteúdo do arquivo pelo seguinte código para substituir o texto sobre o ASP.NET e MVC pelo texto sobre este aplicativo:</span><span class="sxs-lookup"><span data-stu-id="458f7-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="458f7-148">Pressione CTRL+F5 para executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="458f7-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="458f7-149">A home page é exibida com as guias criadas nos seguintes tutoriais:</span><span class="sxs-lookup"><span data-stu-id="458f7-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Home page da Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="458f7-151">Criar o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="458f7-151">Create the data model</span></span>

<span data-ttu-id="458f7-152">Crie classes de entidade para o aplicativo Contoso University.</span><span class="sxs-lookup"><span data-stu-id="458f7-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="458f7-153">Comece com as três seguintes entidades:</span><span class="sxs-lookup"><span data-stu-id="458f7-153">Start with the following three entities:</span></span>

![Diagrama de Modelo de Dados Course-Enrollment-Student](intro/_static/data-model-diagram.png)

<span data-ttu-id="458f7-155">Há uma relação um-para-muitos entre as entidades `Student` e `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="458f7-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="458f7-156">Há uma relação um-para-muitos entre as entidades `Course` e `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="458f7-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="458f7-157">Um aluno pode se registrar em qualquer quantidade de cursos.</span><span class="sxs-lookup"><span data-stu-id="458f7-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="458f7-158">Um curso pode ter qualquer quantidade de alunos registrados.</span><span class="sxs-lookup"><span data-stu-id="458f7-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="458f7-159">Nas seções a seguir, é criada uma classe para cada uma dessas entidades.</span><span class="sxs-lookup"><span data-stu-id="458f7-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="458f7-160">A entidade Student</span><span class="sxs-lookup"><span data-stu-id="458f7-160">The Student entity</span></span>

![Diagrama da entidade Student](intro/_static/student-entity.png)

<span data-ttu-id="458f7-162">Crie uma pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="458f7-162">Create a *Models* folder.</span></span> <span data-ttu-id="458f7-163">Na pasta *Models*, crie um arquivo de classe chamado *Student.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="458f7-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="458f7-164">A propriedade `ID` se torna a coluna de chave primária da tabela de BD (banco de dados) que corresponde a essa classe.</span><span class="sxs-lookup"><span data-stu-id="458f7-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="458f7-165">Por padrão, o EF Core interpreta uma propriedade nomeada `ID` ou `classnameID` como a chave primária.</span><span class="sxs-lookup"><span data-stu-id="458f7-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="458f7-166">Em `classnameID`, `classname` é o nome da classe, como `Student` no exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="458f7-166">In `classnameID`, `classname` is the name of the class, such as `Student` in the preceding example.</span></span>

<span data-ttu-id="458f7-167">A propriedade `Enrollments` é uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="458f7-167">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="458f7-168">As propriedades de navegação vinculam-se a outras entidades que estão relacionadas a essa entidade.</span><span class="sxs-lookup"><span data-stu-id="458f7-168">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="458f7-169">Nesse caso, a propriedade `Enrollments` de uma `Student entity` armazena todas as entidades `Enrollment` relacionadas a essa `Student`.</span><span class="sxs-lookup"><span data-stu-id="458f7-169">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="458f7-170">Por exemplo, se uma linha Aluno no BD tiver duas linhas Registro relacionadas, a propriedade de navegação `Enrollments` conterá duas entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="458f7-170">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="458f7-171">Uma linha `Enrollment` relacionada é uma linha que contém o valor de chave primária do aluno na coluna `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="458f7-171">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="458f7-172">Por exemplo, suponha que o aluno com ID=1 tenha duas linhas na tabela `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="458f7-172">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="458f7-173">A tabela `Enrollment` tem duas linhas com `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="458f7-173">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="458f7-174">`StudentID` é uma chave estrangeira na tabela `Enrollment` que especifica o aluno na tabela `Student`.</span><span class="sxs-lookup"><span data-stu-id="458f7-174">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="458f7-175">Se uma propriedade de navegação puder armazenar várias entidades, a propriedade de navegação deverá ser um tipo de lista, como `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="458f7-175">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="458f7-176">`ICollection<T>` pode ser especificado ou um tipo como `List<T>` ou `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="458f7-176">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="458f7-177">Quando `ICollection<T>` é usado, o EF Core cria uma coleção `HashSet<T>` por padrão.</span><span class="sxs-lookup"><span data-stu-id="458f7-177">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="458f7-178">As propriedades de navegação que armazenam várias entidades são provenientes de relações muitos para muitos e um-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="458f7-178">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="458f7-179">A entidade Enrollment</span><span class="sxs-lookup"><span data-stu-id="458f7-179">The Enrollment entity</span></span>

![Diagrama da entidade Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="458f7-181">Na pasta *Models*, crie *Enrollment.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="458f7-181">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="458f7-182">A propriedade `EnrollmentID` é a chave primária.</span><span class="sxs-lookup"><span data-stu-id="458f7-182">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="458f7-183">Essa entidade usa o padrão `classnameID` em vez de `ID` como a entidade `Student`.</span><span class="sxs-lookup"><span data-stu-id="458f7-183">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="458f7-184">Normalmente, os desenvolvedores escolhem um padrão e o usam em todo o modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="458f7-184">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="458f7-185">Em um tutorial posterior, o uso de uma ID sem nome de classe é mostrado para facilitar a implementação da herança no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="458f7-185">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="458f7-186">A propriedade `Grade` é um `enum`.</span><span class="sxs-lookup"><span data-stu-id="458f7-186">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="458f7-187">O ponto de interrogação após a declaração de tipo `Grade` indica que a propriedade `Grade` permite valor nulo.</span><span class="sxs-lookup"><span data-stu-id="458f7-187">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="458f7-188">Uma nota nula é diferente de uma nota zero – nulo significa que uma nota não é conhecida ou que ainda não foi atribuída.</span><span class="sxs-lookup"><span data-stu-id="458f7-188">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="458f7-189">A propriedade `StudentID` é uma chave estrangeira e a propriedade de navegação correspondente é `Student`.</span><span class="sxs-lookup"><span data-stu-id="458f7-189">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="458f7-190">Uma entidade `Enrollment` está associada a uma entidade `Student` e, portanto, a propriedade contém uma única entidade `Student`.</span><span class="sxs-lookup"><span data-stu-id="458f7-190">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="458f7-191">A entidade `Student` é distinta da propriedade de navegação `Student.Enrollments`, que contém várias entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="458f7-191">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="458f7-192">A propriedade `CourseID` é uma chave estrangeira e a propriedade de navegação correspondente é `Course`.</span><span class="sxs-lookup"><span data-stu-id="458f7-192">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="458f7-193">Uma entidade `Enrollment` está associada a uma entidade `Course`.</span><span class="sxs-lookup"><span data-stu-id="458f7-193">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="458f7-194">O EF Core interpreta uma propriedade como uma chave estrangeira se ela é nomeada `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="458f7-194">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="458f7-195">Por exemplo, `StudentID` para a propriedade de navegação `Student`, pois a chave primária da entidade `Student` é `ID`.</span><span class="sxs-lookup"><span data-stu-id="458f7-195">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="458f7-196">Propriedades de chave estrangeira também podem ser nomeadas `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="458f7-196">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="458f7-197">Por exemplo, `CourseID`, pois a chave primária da entidade `Course` é `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="458f7-197">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="458f7-198">A entidade Course</span><span class="sxs-lookup"><span data-stu-id="458f7-198">The Course entity</span></span>

![Diagrama de entidade Curso](intro/_static/course-entity.png)

<span data-ttu-id="458f7-200">Na pasta *Models*, crie *Course.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="458f7-200">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="458f7-201">A propriedade `Enrollments` é uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="458f7-201">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="458f7-202">Uma entidade `Course` pode estar relacionada a qualquer quantidade de entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="458f7-202">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="458f7-203">O atributo `DatabaseGenerated` permite que o aplicativo especifique a chave primária em vez de fazer com que ela seja gerada pelo BD.</span><span class="sxs-lookup"><span data-stu-id="458f7-203">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="458f7-204">Criar o contexto de BD SchoolContext</span><span class="sxs-lookup"><span data-stu-id="458f7-204">Create the SchoolContext DB context</span></span>

<span data-ttu-id="458f7-205">A classe principal que coordena a funcionalidade do EF Core de um modelo de dados é a classe de contexto de BD.</span><span class="sxs-lookup"><span data-stu-id="458f7-205">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="458f7-206">O contexto de dados é derivado de `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="458f7-206">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="458f7-207">O contexto de dados especifica quais entidades são incluídas no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="458f7-207">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="458f7-208">Neste projeto, a classe é chamada `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="458f7-208">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="458f7-209">Na pasta do projeto, crie uma pasta chamada *Dados*.</span><span class="sxs-lookup"><span data-stu-id="458f7-209">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="458f7-210">Na pasta *Dados*, crie *SchoolContext.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="458f7-210">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="458f7-211">Esse código cria uma propriedade `DbSet` para cada conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="458f7-211">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="458f7-212">Na terminologia do EF Core:</span><span class="sxs-lookup"><span data-stu-id="458f7-212">In EF Core terminology:</span></span>

* <span data-ttu-id="458f7-213">Um conjunto de entidades normalmente corresponde a uma tabela de BD.</span><span class="sxs-lookup"><span data-stu-id="458f7-213">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="458f7-214">Uma entidade corresponde a uma linha da tabela.</span><span class="sxs-lookup"><span data-stu-id="458f7-214">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="458f7-215">`DbSet<Enrollment>` e `DbSet<Course>` podem ser omitidos.</span><span class="sxs-lookup"><span data-stu-id="458f7-215">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="458f7-216">O EF Core inclui-os de forma implícita porque a entidade `Student` referencia a entidade `Enrollment` e a entidade `Enrollment` referencia a entidade `Course`.</span><span class="sxs-lookup"><span data-stu-id="458f7-216">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="458f7-217">Para este tutorial, mantenha `DbSet<Enrollment>` e `DbSet<Course>` no `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="458f7-217">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="458f7-218">Quando o BD é criado, o EF Core cria tabelas que têm nomes iguais aos nomes de propriedade `DbSet`.</span><span class="sxs-lookup"><span data-stu-id="458f7-218">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="458f7-219">Nomes de propriedade para coleções são normalmente plurais (Students em vez de Student).</span><span class="sxs-lookup"><span data-stu-id="458f7-219">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="458f7-220">Os desenvolvedores não concordam sobre se os nomes de tabela devem estar no plural.</span><span class="sxs-lookup"><span data-stu-id="458f7-220">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="458f7-221">Para esses tutoriais, o comportamento padrão é substituído pela especificação de nomes singulares de tabela no DbContext.</span><span class="sxs-lookup"><span data-stu-id="458f7-221">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="458f7-222">Para especificar nomes singulares de tabela, adicione o seguinte código realçado:</span><span class="sxs-lookup"><span data-stu-id="458f7-222">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="458f7-223">Registrar o contexto com a injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="458f7-223">Register the context with dependency injection</span></span>

<span data-ttu-id="458f7-224">O ASP.NET Core inclui a [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="458f7-224">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="458f7-225">Serviços (como o contexto de BD do EF Core) são registrados com injeção de dependência durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="458f7-225">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="458f7-226">Os componentes que exigem esses serviços (como as Páginas do Razor) recebem esses serviços por meio de parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="458f7-226">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="458f7-227">O código de construtor que obtém uma instância de contexto do BD é mostrado mais adiante no tutorial.</span><span class="sxs-lookup"><span data-stu-id="458f7-227">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="458f7-228">Para registrar `SchoolContext` como um serviço, abra *Startup.cs* e adicione as linhas realçadas ao método `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="458f7-228">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="458f7-229">O nome da cadeia de conexão é passado para o contexto com a chamada de um método em um objeto `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="458f7-229">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="458f7-230">Para o desenvolvimento local, o [sistema de configuração do ASP.NET Core](xref:fundamentals/configuration/index) lê a cadeia de conexão do arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="458f7-230">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="458f7-231">Adicione instruções `using` aos namespaces `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="458f7-231">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="458f7-232">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="458f7-232">Build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="458f7-233">Abra o arquivo *appsettings.json* e adicione uma cadeia de conexão, conforme mostrado no seguinte código:</span><span class="sxs-lookup"><span data-stu-id="458f7-233">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="458f7-234">A cadeia de conexão anterior usa `ConnectRetryCount=0` para impedir o travamento do [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework).</span><span class="sxs-lookup"><span data-stu-id="458f7-234">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="458f7-235">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="458f7-235">SQL Server Express LocalDB</span></span>

<span data-ttu-id="458f7-236">A cadeia de conexão especifica um BD LocalDB do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="458f7-236">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="458f7-237">LocalDB é uma versão leve do Mecanismo de Banco de Dados do SQL Server Express destinado ao desenvolvimento de aplicativos, e não ao uso em produção.</span><span class="sxs-lookup"><span data-stu-id="458f7-237">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="458f7-238">O LocalDB é iniciado sob demanda e executado no modo de usuário e, portanto, não há nenhuma configuração complexa.</span><span class="sxs-lookup"><span data-stu-id="458f7-238">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="458f7-239">Por padrão, o LocalDB cria arquivos *.mdf* de BD no diretório `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="458f7-239">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="458f7-240">Adicionar um código para inicializar o BD com os dados de teste</span><span class="sxs-lookup"><span data-stu-id="458f7-240">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="458f7-241">O EF Core cria um BD vazio.</span><span class="sxs-lookup"><span data-stu-id="458f7-241">EF Core creates an empty DB.</span></span> <span data-ttu-id="458f7-242">Nesta seção, um método *Seed* é escrito para populá-lo com os dados de teste.</span><span class="sxs-lookup"><span data-stu-id="458f7-242">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="458f7-243">Na pasta *Dados*, crie um novo arquivo de classe chamado *DbInitializer.cs* e adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="458f7-243">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="458f7-244">O código verifica se há alunos no BD.</span><span class="sxs-lookup"><span data-stu-id="458f7-244">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="458f7-245">Se não houver nenhum aluno no BD, o BD será propagado com os dados de teste.</span><span class="sxs-lookup"><span data-stu-id="458f7-245">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="458f7-246">Ele carrega os dados de teste em matrizes em vez de em coleções `List<T>` para otimizar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="458f7-246">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="458f7-247">O método `EnsureCreated` cria o BD automaticamente para o contexto de BD.</span><span class="sxs-lookup"><span data-stu-id="458f7-247">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="458f7-248">Se o BD existir, `EnsureCreated` retornará sem modificar o BD.</span><span class="sxs-lookup"><span data-stu-id="458f7-248">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="458f7-249">Em *Program.cs*, modifique o método `Main` para fazer o seguinte:</span><span class="sxs-lookup"><span data-stu-id="458f7-249">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="458f7-250">Obtenha uma instância de contexto de BD do contêiner de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="458f7-250">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="458f7-251">Chame o método de semente passando a ele o contexto.</span><span class="sxs-lookup"><span data-stu-id="458f7-251">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="458f7-252">Descarte o contexto quando o método de semente for concluído.</span><span class="sxs-lookup"><span data-stu-id="458f7-252">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="458f7-253">O código a seguir mostra o arquivo *Program.cs* atualizado.</span><span class="sxs-lookup"><span data-stu-id="458f7-253">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="458f7-254">A primeira vez que o aplicativo é executado, o BD é criado e propagado com os dados de teste.</span><span class="sxs-lookup"><span data-stu-id="458f7-254">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="458f7-255">Quando o modelo de dados é atualizado:</span><span class="sxs-lookup"><span data-stu-id="458f7-255">When the data model is updated:</span></span>
* <span data-ttu-id="458f7-256">Exclua o BD.</span><span class="sxs-lookup"><span data-stu-id="458f7-256">Delete the DB.</span></span>
* <span data-ttu-id="458f7-257">Atualize o método de semente.</span><span class="sxs-lookup"><span data-stu-id="458f7-257">Update the seed method.</span></span>
* <span data-ttu-id="458f7-258">Execute o aplicativo e um novo BD propagado será criado.</span><span class="sxs-lookup"><span data-stu-id="458f7-258">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="458f7-259">Nos próximos tutoriais, o BD é atualizado quando o modelo de dados é alterado, sem excluir nem recriar o BD.</span><span class="sxs-lookup"><span data-stu-id="458f7-259">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="458f7-260">Adicionar ferramentas de scaffolding</span><span class="sxs-lookup"><span data-stu-id="458f7-260">Add scaffold tooling</span></span>

<span data-ttu-id="458f7-261">Nesta seção, o PMC (Console do Gerenciador de Pacotes) é usado para adicionar o pacote de geração de código da Web do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="458f7-261">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="458f7-262">Esse pacote é necessário para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="458f7-262">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="458f7-263">No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="458f7-263">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="458f7-264">No PMC (Console do Gerenciador de Pacotes), Insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="458f7-264">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="458f7-265">O comando anterior adiciona os pacotes NuGet ao arquivo \*.csproj:</span><span class="sxs-lookup"><span data-stu-id="458f7-265">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-csharp[](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="458f7-266">Gerar o modelo por scaffolding</span><span class="sxs-lookup"><span data-stu-id="458f7-266">Scaffold the model</span></span>

* <span data-ttu-id="458f7-267">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="458f7-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="458f7-268">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="458f7-268">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```

<span data-ttu-id="458f7-269">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="458f7-269">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="458f7-270">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="458f7-270">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="458f7-271">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="458f7-271">Build the project.</span></span> <span data-ttu-id="458f7-272">O build gera erros, como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="458f7-272">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="458f7-273">Altere `_context.Student` globalmente para `_context.Students` (ou seja, adicione um "s" a `Student`).</span><span class="sxs-lookup"><span data-stu-id="458f7-273">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="458f7-274">7 ocorrências foram encontradas e atualizadas.</span><span class="sxs-lookup"><span data-stu-id="458f7-274">7 occurrences are found and updated.</span></span> <span data-ttu-id="458f7-275">Esperamos corrigir [este bug](https://github.com/aspnet/Scaffolding/issues/633) na próxima versão.</span><span class="sxs-lookup"><span data-stu-id="458f7-275">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE [model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="458f7-276">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="458f7-276">Test the app</span></span>

<span data-ttu-id="458f7-277">Execute o aplicativo e selecione o link **Alunos**.</span><span class="sxs-lookup"><span data-stu-id="458f7-277">Run the app and select the **Students** link.</span></span> <span data-ttu-id="458f7-278">Dependendo da largura do navegador, o link **Alunos** é exibido na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="458f7-278">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="458f7-279">Se o link **Alunos** não estiver visível, clique no ícone de navegação no canto superior direito.</span><span class="sxs-lookup"><span data-stu-id="458f7-279">If the **Students** link isn't visible, click the navigation icon in the upper right corner.</span></span>

![Home page estreita da Contoso University](intro/_static/home-page-narrow.png)

<span data-ttu-id="458f7-281">Teste os links **Criar**, **Editar** e **Detalhes**.</span><span class="sxs-lookup"><span data-stu-id="458f7-281">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="458f7-282">Exibir o BD</span><span class="sxs-lookup"><span data-stu-id="458f7-282">View the DB</span></span>

<span data-ttu-id="458f7-283">Quando o aplicativo é iniciado, `DbInitializer.Initialize` chama `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="458f7-283">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="458f7-284">`EnsureCreated` detecta se o BD existe e cria um, se necessário.</span><span class="sxs-lookup"><span data-stu-id="458f7-284">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="458f7-285">Se não houver nenhum aluno no BD, o método `Initialize` adicionará alunos.</span><span class="sxs-lookup"><span data-stu-id="458f7-285">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="458f7-286">Abra o **SSOX** (Pesquisador de Objetos do SQL Server) no menu **Exibir** do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="458f7-286">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="458f7-287">No SSOX, clique em **(localdb)\MSSQLLocalDB > Bancos de Dados > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="458f7-287">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="458f7-288">Expanda o nó **Tabelas**.</span><span class="sxs-lookup"><span data-stu-id="458f7-288">Expand the **Tables** node.</span></span>

<span data-ttu-id="458f7-289">Clique com o botão direito do mouse na tabela **Aluno** e clique em **Exibir Dados** para ver as colunas criadas e as linhas inseridas na tabela.</span><span class="sxs-lookup"><span data-stu-id="458f7-289">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="458f7-290">Os arquivos <em>.mdf</em> e <em>.ldf</em> do BD estão localizados na pasta <em>C:\Users\\<yourusername></em>.</span><span class="sxs-lookup"><span data-stu-id="458f7-290">The <em>.mdf</em> and <em>.ldf</em> DB files are in the <em>C:\Users\\<yourusername></em> folder.</span></span>

<span data-ttu-id="458f7-291">`EnsureCreated` é chamado na inicialização do aplicativo, que permite que o seguinte fluxo de trabalho:</span><span class="sxs-lookup"><span data-stu-id="458f7-291">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="458f7-292">Exclua o BD.</span><span class="sxs-lookup"><span data-stu-id="458f7-292">Delete the DB.</span></span>
* <span data-ttu-id="458f7-293">Altere o esquema de BD (por exemplo, adicione um campo `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="458f7-293">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="458f7-294">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="458f7-294">Run the app.</span></span>

<span data-ttu-id="458f7-295">`EnsureCreated` cria um BD com a coluna `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="458f7-295">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="458f7-296">Convenções</span><span class="sxs-lookup"><span data-stu-id="458f7-296">Conventions</span></span>

<span data-ttu-id="458f7-297">A quantidade de código feita para que o EF Core crie um BD completo é mínima, devido ao uso de convenções ou de suposições feitas pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="458f7-297">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="458f7-298">Os nomes de propriedades `DbSet` são usadas como nomes de tabela.</span><span class="sxs-lookup"><span data-stu-id="458f7-298">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="458f7-299">Para entidades não referenciadas por uma propriedade `DbSet`, os nomes de classe de entidade são usados como nomes de tabela.</span><span class="sxs-lookup"><span data-stu-id="458f7-299">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="458f7-300">Os nomes de propriedade de entidade são usados para nomes de coluna.</span><span class="sxs-lookup"><span data-stu-id="458f7-300">Entity property names are used for column names.</span></span>

* <span data-ttu-id="458f7-301">As propriedades de entidade que são nomeadas ID ou classnameID são reconhecidas como propriedades de chave primária.</span><span class="sxs-lookup"><span data-stu-id="458f7-301">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="458f7-302">Uma propriedade é interpretada como uma propriedade de chave estrangeira se ela é nomeada *<navigation property name><primary key property name>* (por exemplo, `StudentID` para a propriedade de navegação `Student`, pois a chave primária da entidade `Student` é `ID`).</span><span class="sxs-lookup"><span data-stu-id="458f7-302">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="458f7-303">As propriedades de chave estrangeira podem ser nomeadas *<primary key property name>* (por exemplo, `EnrollmentID`, pois a chave primária da entidade `Enrollment` é `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="458f7-303">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="458f7-304">O comportamento convencional pode ser substituído.</span><span class="sxs-lookup"><span data-stu-id="458f7-304">Conventional behavior can be overridden.</span></span> <span data-ttu-id="458f7-305">Por exemplo, os nomes de tabela podem ser especificados de forma explícita, conforme mostrado anteriormente neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="458f7-305">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="458f7-306">Os nomes de coluna podem ser definidos de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="458f7-306">The column names can be explicitly set.</span></span> <span data-ttu-id="458f7-307">As chaves primária e estrangeira podem ser definidas de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="458f7-307">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="458f7-308">Código assíncrono</span><span class="sxs-lookup"><span data-stu-id="458f7-308">Asynchronous code</span></span>

<span data-ttu-id="458f7-309">A programação assíncrona é o modo padrão do ASP.NET Core e EF Core.</span><span class="sxs-lookup"><span data-stu-id="458f7-309">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="458f7-310">Um servidor Web tem um número limitado de threads disponíveis e, em situações de alta carga, todos os threads disponíveis podem estar em uso.</span><span class="sxs-lookup"><span data-stu-id="458f7-310">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="458f7-311">Quando isso acontece, o servidor não pode processar novas solicitações até que os threads são liberados.</span><span class="sxs-lookup"><span data-stu-id="458f7-311">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="458f7-312">Com um código síncrono, muitos threads podem ser vinculados enquanto realmente não são fazendo nenhum trabalho porque estão aguardando a conclusão da E/S.</span><span class="sxs-lookup"><span data-stu-id="458f7-312">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="458f7-313">Com um código assíncrono, quando um processo está aguardando a conclusão da E/S, seu thread é liberado para o servidor para ser usado para processar outras solicitações.</span><span class="sxs-lookup"><span data-stu-id="458f7-313">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="458f7-314">Como resultado, o código assíncrono permite que os recursos do servidor sejam usados com mais eficiência, e o servidor fica capacitado a manipular mais tráfego sem atrasos.</span><span class="sxs-lookup"><span data-stu-id="458f7-314">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="458f7-315">O código assíncrono introduz uma pequena quantidade de sobrecarga em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="458f7-315">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="458f7-316">Para situações de baixo tráfego, o impacto no desempenho é insignificante, enquanto para situações de alto tráfego, a melhoria de desempenho potencial é significativa.</span><span class="sxs-lookup"><span data-stu-id="458f7-316">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="458f7-317">No código a seguir, a palavra-chave `async`, o valor retornado `Task<T>`, a palavra-chave `await` e o método `ToListAsync` fazem o código ser executado de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="458f7-317">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="458f7-318">A palavra-chave `async` instrui o compilador a:</span><span class="sxs-lookup"><span data-stu-id="458f7-318">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="458f7-319">Gerar retornos de chamada para partes do corpo do método.</span><span class="sxs-lookup"><span data-stu-id="458f7-319">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="458f7-320">Criar automaticamente o objeto [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) que é retornado.</span><span class="sxs-lookup"><span data-stu-id="458f7-320">Automatically create the [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that's returned.</span></span> <span data-ttu-id="458f7-321">Para obter mais informações, consulte [Tipo de retorno de Tarefa](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="458f7-321">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="458f7-322">O tipo de retorno implícito `Task` representa um trabalho em andamento.</span><span class="sxs-lookup"><span data-stu-id="458f7-322">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="458f7-323">A palavra-chave `await` faz com que o compilador divida o método em duas partes.</span><span class="sxs-lookup"><span data-stu-id="458f7-323">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="458f7-324">A primeira parte termina com a operação que é iniciada de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="458f7-324">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="458f7-325">A segunda parte é colocada em um método de retorno de chamada que é chamado quando a operação é concluída.</span><span class="sxs-lookup"><span data-stu-id="458f7-325">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="458f7-326">`ToListAsync` é a versão assíncrona do método de extensão `ToList`.</span><span class="sxs-lookup"><span data-stu-id="458f7-326">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="458f7-327">Algumas coisas a serem consideradas ao escrever um código assíncrono que usa o EF Core:</span><span class="sxs-lookup"><span data-stu-id="458f7-327">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="458f7-328">Somente instruções que fazem com que consultas ou comandos sejam enviados ao BD são executadas de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="458f7-328">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="458f7-329">Isso inclui `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` e `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="458f7-329">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="458f7-330">Isso não inclui instruções que apenas alteram um `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="458f7-330">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="458f7-331">Um contexto do EF Core não é thread-safe: não tente realizar várias operações em paralelo.</span><span class="sxs-lookup"><span data-stu-id="458f7-331">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="458f7-332">Para aproveitar os benefícios de desempenho do código assíncrono, verifique se os pacotes de biblioteca (como para paginação) usam o código assíncrono se eles chamam métodos do EF Core que enviam consultas ao BD.</span><span class="sxs-lookup"><span data-stu-id="458f7-332">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="458f7-333">Para obter mais informações sobre a programação assíncrona no .NET, consulte [Visão geral da programação assíncrona](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="458f7-333">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="458f7-334">No próximo tutorial, as operações CRUD (criar, ler, atualizar e excluir) básicas são examinadas.</span><span class="sxs-lookup"><span data-stu-id="458f7-334">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="458f7-335">Avançar</span><span class="sxs-lookup"><span data-stu-id="458f7-335">Next</span></span>](xref:data/ef-rp/crud)
