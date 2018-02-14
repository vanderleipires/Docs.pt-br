---
title: "Páginas do Razor com o Entity Framework Core – tutorial 1 de 8"
author: rick-anderson
description: "Mostra como criar um aplicativo das Páginas do Razor usando o Entity Framework Core"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 091f34da347d52ba8e3e87779ddc4aeb790c2800
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/31/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="7da58-103">Introdução às Páginas do Razor e ao Entity Framework Core usando o Visual Studio (1 de 8)</span><span class="sxs-lookup"><span data-stu-id="7da58-103">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="7da58-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7da58-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7da58-105">O aplicativo Web de exemplo Contoso University demonstra como criar aplicativos Web ASP.NET Core 2.0 MVC usando o EF (Entity Framework) Core 2.0 e o Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="7da58-105">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="7da58-106">O aplicativo de exemplo é um site de uma Contoso University fictícia.</span><span class="sxs-lookup"><span data-stu-id="7da58-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="7da58-107">Ele inclui funcionalidades como admissão de alunos, criação de cursos e atribuições de instrutor.</span><span class="sxs-lookup"><span data-stu-id="7da58-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="7da58-108">Esta página é a primeira de uma série de tutoriais que explica como criar o aplicativo de exemplo Contoso University.</span><span class="sxs-lookup"><span data-stu-id="7da58-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="7da58-109">Baixe ou exiba o aplicativo concluído.</span><span class="sxs-lookup"><span data-stu-id="7da58-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="7da58-110">[Instruções de download](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="7da58-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7da58-111">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="7da58-111">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

<span data-ttu-id="7da58-112">Familiaridade com as [Páginas do Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="7da58-112">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="7da58-113">Os novos programadores devem concluir a [Introdução às Páginas do Razor](xref:tutorials/razor-pages/razor-pages-start) antes de começar esta série.</span><span class="sxs-lookup"><span data-stu-id="7da58-113">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7da58-114">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="7da58-114">Troubleshooting</span></span>

<span data-ttu-id="7da58-115">Caso tenha um problema que não consiga resolver, em geral, você poderá encontrar a solução comparando o código com o [estágio concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) ou o [projeto concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="7da58-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="7da58-116">Para obter uma lista de erros comuns e como resolvê-los, consulte [a seção Solução de problemas do último tutorial da série](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="7da58-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="7da58-117">Caso não encontre o que precisa na seção, poste uma pergunta no [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) sobre o [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="7da58-117">If you don't find what you need there, you can post a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="7da58-118">Esta série de tutoriais baseia-se no que foi feito nos tutoriais anteriores.</span><span class="sxs-lookup"><span data-stu-id="7da58-118">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="7da58-119">Considere a possibilidade de salvar uma cópia do projeto após a conclusão bem-sucedida de cada tutorial.</span><span class="sxs-lookup"><span data-stu-id="7da58-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="7da58-120">Caso tenha problemas, comece novamente no tutorial anterior em vez de voltar ao início.</span><span class="sxs-lookup"><span data-stu-id="7da58-120">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="7da58-121">Como alternativa, você pode baixar um [estágio concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) e começar novamente usando o estágio concluído.</span><span class="sxs-lookup"><span data-stu-id="7da58-121">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="7da58-122">O aplicativo Web Contoso University</span><span class="sxs-lookup"><span data-stu-id="7da58-122">The Contoso University web app</span></span>

<span data-ttu-id="7da58-123">O aplicativo criado nesses tutoriais é um site básico de universidade.</span><span class="sxs-lookup"><span data-stu-id="7da58-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="7da58-124">Os usuários podem exibir e atualizar informações de alunos, cursos e instrutores.</span><span class="sxs-lookup"><span data-stu-id="7da58-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="7da58-125">Veja a seguir algumas das telas criadas no tutorial.</span><span class="sxs-lookup"><span data-stu-id="7da58-125">Here are a few of the screens created in the tutorial.</span></span>

![Página Índice de Alunos](intro/_static/students-index.png)

![Página Editar Alunos](intro/_static/student-edit.png)

<span data-ttu-id="7da58-128">O estilo de interface do usuário deste site é próximo ao que é gerado pelos modelos internos.</span><span class="sxs-lookup"><span data-stu-id="7da58-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="7da58-129">O foco do tutorial é o EF Core com as Páginas do Razor, não a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="7da58-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="7da58-130">Criar um aplicativo Web das Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="7da58-130">Create a Razor Pages web app</span></span>

* <span data-ttu-id="7da58-131">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="7da58-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7da58-132">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7da58-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="7da58-133">Nomeie o projeto **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="7da58-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="7da58-134">É importante nomear o projeto *ContosoUniversity* para que os namespaces sejam correspondentes quando o código for copiado/colado.</span><span class="sxs-lookup"><span data-stu-id="7da58-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="7da58-135">![novo aplicativo Web ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="7da58-135">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="7da58-136">Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="7da58-136">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="7da58-137">![Aplicativo Web (Páginas do Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="7da58-137">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="7da58-138">Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador</span><span class="sxs-lookup"><span data-stu-id="7da58-138">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="7da58-139">Configurar o estilo do site</span><span class="sxs-lookup"><span data-stu-id="7da58-139">Set up the site style</span></span>

<span data-ttu-id="7da58-140">Algumas alterações configuram o menu do site, o layout e a home page.</span><span class="sxs-lookup"><span data-stu-id="7da58-140">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="7da58-141">Abra *Pages/_Layout.cshtml* e faça as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="7da58-141">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="7da58-142">Altere cada ocorrência de "ContosoUniversity" para "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="7da58-142">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="7da58-143">Há três ocorrências.</span><span class="sxs-lookup"><span data-stu-id="7da58-143">There are three occurrences.</span></span>

* <span data-ttu-id="7da58-144">Adicione entradas de menu para **Alunos**, **Cursos**, **Instrutores** e **Departamentos** e exclua a entrada de menu **Contato**.</span><span class="sxs-lookup"><span data-stu-id="7da58-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="7da58-145">As alterações são realçadas.</span><span class="sxs-lookup"><span data-stu-id="7da58-145">The changes are highlighted.</span></span> <span data-ttu-id="7da58-146">(Toda a marcação *não* é exibida.)</span><span class="sxs-lookup"><span data-stu-id="7da58-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="7da58-147">Em *Pages/Index.cshtml*, substitua o conteúdo do arquivo pelo seguinte código para substituir o texto sobre o ASP.NET e MVC pelo texto sobre este aplicativo:</span><span class="sxs-lookup"><span data-stu-id="7da58-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="7da58-148">Pressione CTRL+F5 para executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="7da58-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="7da58-149">A home page é exibida com as guias criadas nos seguintes tutoriais:</span><span class="sxs-lookup"><span data-stu-id="7da58-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Home page da Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="7da58-151">Criar o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="7da58-151">Create the data model</span></span>

<span data-ttu-id="7da58-152">Crie classes de entidade para o aplicativo Contoso University.</span><span class="sxs-lookup"><span data-stu-id="7da58-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="7da58-153">Comece com as três seguintes entidades:</span><span class="sxs-lookup"><span data-stu-id="7da58-153">Start with the following three entities:</span></span>

![Diagrama de Modelo de Dados Course-Enrollment-Student](intro/_static/data-model-diagram.png)

<span data-ttu-id="7da58-155">Há uma relação um-para-muitos entre as entidades `Student` e `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7da58-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="7da58-156">Há uma relação um-para-muitos entre as entidades `Course` e `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7da58-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="7da58-157">Um aluno pode se registrar em qualquer quantidade de cursos.</span><span class="sxs-lookup"><span data-stu-id="7da58-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="7da58-158">Um curso pode ter qualquer quantidade de alunos registrados.</span><span class="sxs-lookup"><span data-stu-id="7da58-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="7da58-159">Nas seções a seguir, é criada uma classe para cada uma dessas entidades.</span><span class="sxs-lookup"><span data-stu-id="7da58-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="7da58-160">A entidade Student</span><span class="sxs-lookup"><span data-stu-id="7da58-160">The Student entity</span></span>

![Diagrama da entidade Student](intro/_static/student-entity.png)

<span data-ttu-id="7da58-162">Crie uma pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="7da58-162">Create a *Models* folder.</span></span> <span data-ttu-id="7da58-163">Na pasta *Models*, crie um arquivo de classe chamado *Student.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="7da58-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="7da58-164">A propriedade `ID` se torna a coluna de chave primária da tabela de BD (banco de dados) que corresponde a essa classe.</span><span class="sxs-lookup"><span data-stu-id="7da58-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="7da58-165">Por padrão, o EF Core interpreta uma propriedade nomeada `ID` ou `classnameID` como a chave primária.</span><span class="sxs-lookup"><span data-stu-id="7da58-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="7da58-166">A propriedade `Enrollments` é uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="7da58-166">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="7da58-167">As propriedades de navegação vinculam-se a outras entidades que estão relacionadas a essa entidade.</span><span class="sxs-lookup"><span data-stu-id="7da58-167">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="7da58-168">Nesse caso, a propriedade `Enrollments` de uma `Student entity` armazena todas as entidades `Enrollment` relacionadas a essa `Student`.</span><span class="sxs-lookup"><span data-stu-id="7da58-168">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="7da58-169">Por exemplo, se uma linha Aluno no BD tiver duas linhas Registro relacionadas, a propriedade de navegação `Enrollments` conterá duas entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7da58-169">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="7da58-170">Uma linha `Enrollment` relacionada é uma linha que contém o valor de chave primária do aluno na coluna `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="7da58-170">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="7da58-171">Por exemplo, suponha que o aluno com ID=1 tenha duas linhas na tabela `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7da58-171">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="7da58-172">A tabela `Enrollment` tem duas linhas com `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="7da58-172">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="7da58-173">`StudentID` é uma chave estrangeira na tabela `Enrollment` que especifica o aluno na tabela `Student`.</span><span class="sxs-lookup"><span data-stu-id="7da58-173">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="7da58-174">Se uma propriedade de navegação puder armazenar várias entidades, a propriedade de navegação deverá ser um tipo de lista, como `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="7da58-174">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="7da58-175">`ICollection<T>` pode ser especificado ou um tipo como `List<T>` ou `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="7da58-175">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="7da58-176">Quando `ICollection<T>` é usado, o EF Core cria uma coleção `HashSet<T>` por padrão.</span><span class="sxs-lookup"><span data-stu-id="7da58-176">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="7da58-177">As propriedades de navegação que armazenam várias entidades são provenientes de relações muitos para muitos e um-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="7da58-177">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="7da58-178">A entidade Enrollment</span><span class="sxs-lookup"><span data-stu-id="7da58-178">The Enrollment entity</span></span>

![Diagrama da entidade Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="7da58-180">Na pasta *Models*, crie *Enrollment.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="7da58-180">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="7da58-181">A propriedade `EnrollmentID` é a chave primária.</span><span class="sxs-lookup"><span data-stu-id="7da58-181">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="7da58-182">Essa entidade usa o padrão `classnameID` em vez de `ID` como a entidade `Student`.</span><span class="sxs-lookup"><span data-stu-id="7da58-182">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="7da58-183">Normalmente, os desenvolvedores escolhem um padrão e o usam em todo o modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="7da58-183">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="7da58-184">Em um tutorial posterior, o uso de uma ID sem nome de classe é mostrado para facilitar a implementação da herança no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="7da58-184">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="7da58-185">A propriedade `Grade` é um `enum`.</span><span class="sxs-lookup"><span data-stu-id="7da58-185">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="7da58-186">O ponto de interrogação após a declaração de tipo `Grade` indica que a propriedade `Grade` permite valor nulo.</span><span class="sxs-lookup"><span data-stu-id="7da58-186">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="7da58-187">Uma nota nula é diferente de uma nota zero – nulo significa que uma nota não é conhecida ou que ainda não foi atribuída.</span><span class="sxs-lookup"><span data-stu-id="7da58-187">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="7da58-188">A propriedade `StudentID` é uma chave estrangeira e a propriedade de navegação correspondente é `Student`.</span><span class="sxs-lookup"><span data-stu-id="7da58-188">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="7da58-189">Uma entidade `Enrollment` está associada a uma entidade `Student` e, portanto, a propriedade contém uma única entidade `Student`.</span><span class="sxs-lookup"><span data-stu-id="7da58-189">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="7da58-190">A entidade `Student` é distinta da propriedade de navegação `Student.Enrollments`, que contém várias entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7da58-190">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="7da58-191">A propriedade `CourseID` é uma chave estrangeira e a propriedade de navegação correspondente é `Course`.</span><span class="sxs-lookup"><span data-stu-id="7da58-191">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="7da58-192">Uma entidade `Enrollment` está associada a uma entidade `Course`.</span><span class="sxs-lookup"><span data-stu-id="7da58-192">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="7da58-193">O EF Core interpreta uma propriedade como uma chave estrangeira se ela é nomeada `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="7da58-193">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="7da58-194">Por exemplo, `StudentID` para a propriedade de navegação `Student`, pois a chave primária da entidade `Student` é `ID`.</span><span class="sxs-lookup"><span data-stu-id="7da58-194">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="7da58-195">Propriedades de chave estrangeira também podem ser nomeadas `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="7da58-195">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="7da58-196">Por exemplo, `CourseID`, pois a chave primária da entidade `Course` é `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="7da58-196">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="7da58-197">A entidade Course</span><span class="sxs-lookup"><span data-stu-id="7da58-197">The Course entity</span></span>

![Diagrama de entidade Curso](intro/_static/course-entity.png)

<span data-ttu-id="7da58-199">Na pasta *Models*, crie *Course.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="7da58-199">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="7da58-200">A propriedade `Enrollments` é uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="7da58-200">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="7da58-201">Uma entidade `Course` pode estar relacionada a qualquer quantidade de entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="7da58-201">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="7da58-202">O atributo `DatabaseGenerated` permite que o aplicativo especifique a chave primária em vez de fazer com que ela seja gerada pelo BD.</span><span class="sxs-lookup"><span data-stu-id="7da58-202">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="7da58-203">Criar o contexto de BD SchoolContext</span><span class="sxs-lookup"><span data-stu-id="7da58-203">Create the SchoolContext DB context</span></span>

<span data-ttu-id="7da58-204">A classe principal que coordena a funcionalidade do EF Core de um modelo de dados é a classe de contexto de BD.</span><span class="sxs-lookup"><span data-stu-id="7da58-204">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="7da58-205">O contexto de dados é derivado de `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="7da58-205">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="7da58-206">O contexto de dados especifica quais entidades são incluídas no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="7da58-206">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="7da58-207">Neste projeto, a classe é chamada `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="7da58-207">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="7da58-208">Na pasta do projeto, crie uma pasta chamada *Dados*.</span><span class="sxs-lookup"><span data-stu-id="7da58-208">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="7da58-209">Na pasta *Dados*, crie *SchoolContext.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="7da58-209">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="7da58-210">Esse código cria uma propriedade `DbSet` para cada conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="7da58-210">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="7da58-211">Na terminologia do EF Core:</span><span class="sxs-lookup"><span data-stu-id="7da58-211">In EF Core terminology:</span></span>

* <span data-ttu-id="7da58-212">Um conjunto de entidades normalmente corresponde a uma tabela de BD.</span><span class="sxs-lookup"><span data-stu-id="7da58-212">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="7da58-213">Uma entidade corresponde a uma linha da tabela.</span><span class="sxs-lookup"><span data-stu-id="7da58-213">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="7da58-214">`DbSet<Enrollment>` e `DbSet<Course>` podem ser omitidos.</span><span class="sxs-lookup"><span data-stu-id="7da58-214">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="7da58-215">O EF Core inclui-os de forma implícita porque a entidade `Student` referencia a entidade `Enrollment` e a entidade `Enrollment` referencia a entidade `Course`.</span><span class="sxs-lookup"><span data-stu-id="7da58-215">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="7da58-216">Para este tutorial, mantenha `DbSet<Enrollment>` e `DbSet<Course>` no `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="7da58-216">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="7da58-217">Quando o BD é criado, o EF Core cria tabelas que têm nomes iguais aos nomes de propriedade `DbSet`.</span><span class="sxs-lookup"><span data-stu-id="7da58-217">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="7da58-218">Nomes de propriedade para coleções são normalmente plurais (Students em vez de Student).</span><span class="sxs-lookup"><span data-stu-id="7da58-218">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="7da58-219">Os desenvolvedores não concordam sobre se os nomes de tabela devem estar no plural.</span><span class="sxs-lookup"><span data-stu-id="7da58-219">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="7da58-220">Para esses tutoriais, o comportamento padrão é substituído pela especificação de nomes singulares de tabela no DbContext.</span><span class="sxs-lookup"><span data-stu-id="7da58-220">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="7da58-221">Para especificar nomes singulares de tabela, adicione o seguinte código realçado:</span><span class="sxs-lookup"><span data-stu-id="7da58-221">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="7da58-222">Registrar o contexto com a injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="7da58-222">Register the context with dependency injection</span></span>

<span data-ttu-id="7da58-223">O ASP.NET Core inclui a [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7da58-223">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7da58-224">Serviços (como o contexto de BD do EF Core) são registrados com injeção de dependência durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7da58-224">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="7da58-225">Os componentes que exigem esses serviços (como as Páginas do Razor) recebem esses serviços por meio de parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="7da58-225">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="7da58-226">O código de construtor que obtém uma instância de contexto do BD é mostrado mais adiante no tutorial.</span><span class="sxs-lookup"><span data-stu-id="7da58-226">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="7da58-227">Para registrar `SchoolContext` como um serviço, abra *Startup.cs* e adicione as linhas realçadas ao método `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7da58-227">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="7da58-228">O nome da cadeia de conexão é passado para o contexto com a chamada de um método em um objeto `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7da58-228">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="7da58-229">Para o desenvolvimento local, o [sistema de configuração do ASP.NET Core](xref:fundamentals/configuration/index) lê a cadeia de conexão do arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7da58-229">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="7da58-230">Adicione instruções `using` aos namespaces `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="7da58-230">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="7da58-231">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="7da58-231">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="7da58-232">Abra o arquivo *appsettings.json* e adicione uma cadeia de conexão, conforme mostrado no seguinte código:</span><span class="sxs-lookup"><span data-stu-id="7da58-232">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="7da58-233">A cadeia de conexão anterior usa `ConnectRetryCount=0` para impedir o travamento do [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework).</span><span class="sxs-lookup"><span data-stu-id="7da58-233">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="7da58-234">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="7da58-234">SQL Server Express LocalDB</span></span>

<span data-ttu-id="7da58-235">A cadeia de conexão especifica um BD LocalDB do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7da58-235">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="7da58-236">LocalDB é uma versão leve do Mecanismo de Banco de Dados do SQL Server Express destinado ao desenvolvimento de aplicativos, e não ao uso em produção.</span><span class="sxs-lookup"><span data-stu-id="7da58-236">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="7da58-237">O LocalDB é iniciado sob demanda e executado no modo de usuário e, portanto, não há nenhuma configuração complexa.</span><span class="sxs-lookup"><span data-stu-id="7da58-237">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="7da58-238">Por padrão, o LocalDB cria arquivos *.mdf* de BD no diretório `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="7da58-238">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="7da58-239">Adicionar um código para inicializar o BD com os dados de teste</span><span class="sxs-lookup"><span data-stu-id="7da58-239">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="7da58-240">O EF Core cria um BD vazio.</span><span class="sxs-lookup"><span data-stu-id="7da58-240">EF Core creates an empty DB.</span></span> <span data-ttu-id="7da58-241">Nesta seção, um método *Seed* é escrito para populá-lo com os dados de teste.</span><span class="sxs-lookup"><span data-stu-id="7da58-241">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="7da58-242">Na pasta *Dados*, crie um novo arquivo de classe chamado *DbInitializer.cs* e adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="7da58-242">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="7da58-243">O código verifica se há alunos no BD.</span><span class="sxs-lookup"><span data-stu-id="7da58-243">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="7da58-244">Se não houver nenhum aluno no BD, o BD será propagado com os dados de teste.</span><span class="sxs-lookup"><span data-stu-id="7da58-244">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="7da58-245">Ele carrega os dados de teste em matrizes em vez de em coleções `List<T>` para otimizar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="7da58-245">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="7da58-246">O método `EnsureCreated` cria o BD automaticamente para o contexto de BD.</span><span class="sxs-lookup"><span data-stu-id="7da58-246">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="7da58-247">Se o BD existir, `EnsureCreated` retornará sem modificar o BD.</span><span class="sxs-lookup"><span data-stu-id="7da58-247">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="7da58-248">Em *Program.cs*, modifique o método `Main` para fazer o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7da58-248">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="7da58-249">Obtenha uma instância de contexto de BD do contêiner de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="7da58-249">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="7da58-250">Chame o método de semente passando a ele o contexto.</span><span class="sxs-lookup"><span data-stu-id="7da58-250">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="7da58-251">Descarte o contexto quando o método de semente for concluído.</span><span class="sxs-lookup"><span data-stu-id="7da58-251">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="7da58-252">O código a seguir mostra o arquivo *Program.cs* atualizado.</span><span class="sxs-lookup"><span data-stu-id="7da58-252">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="7da58-253">A primeira vez que o aplicativo é executado, o BD é criado e propagado com os dados de teste.</span><span class="sxs-lookup"><span data-stu-id="7da58-253">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="7da58-254">Quando o modelo de dados é atualizado:</span><span class="sxs-lookup"><span data-stu-id="7da58-254">When the data model is updated:</span></span>
* <span data-ttu-id="7da58-255">Exclua o BD.</span><span class="sxs-lookup"><span data-stu-id="7da58-255">Delete the DB.</span></span>
* <span data-ttu-id="7da58-256">Atualize o método de semente.</span><span class="sxs-lookup"><span data-stu-id="7da58-256">Update the seed method.</span></span>
* <span data-ttu-id="7da58-257">Execute o aplicativo e um novo BD propagado será criado.</span><span class="sxs-lookup"><span data-stu-id="7da58-257">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="7da58-258">Nos próximos tutoriais, o BD é atualizado quando o modelo de dados é alterado, sem excluir nem recriar o BD.</span><span class="sxs-lookup"><span data-stu-id="7da58-258">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="7da58-259">Adicionar ferramentas de scaffolding</span><span class="sxs-lookup"><span data-stu-id="7da58-259">Add scaffold tooling</span></span>

<span data-ttu-id="7da58-260">Nesta seção, o PMC (Console do Gerenciador de Pacotes) é usado para adicionar o pacote de geração de código da Web do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7da58-260">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="7da58-261">Esse pacote é necessário para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="7da58-261">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="7da58-262">No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="7da58-262">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="7da58-263">No PMC (Console do Gerenciador de Pacotes), Insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="7da58-263">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="7da58-264">O comando anterior adiciona os pacotes NuGet ao arquivo \*.csproj:</span><span class="sxs-lookup"><span data-stu-id="7da58-264">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="7da58-265">Gerar o modelo por scaffolding</span><span class="sxs-lookup"><span data-stu-id="7da58-265">Scaffold the model</span></span>

* <span data-ttu-id="7da58-266">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="7da58-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7da58-267">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="7da58-267">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="7da58-268">Se o seguinte erro for gerado:</span><span class="sxs-lookup"><span data-stu-id="7da58-268">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="7da58-269">Execute o comando novamente e deixe um comentário na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="7da58-269">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="7da58-270">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="7da58-270">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="7da58-271">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="7da58-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="7da58-272">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="7da58-272">Build the project.</span></span> <span data-ttu-id="7da58-273">O build gera erros, como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7da58-273">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="7da58-274">Altere `_context.Student` globalmente para `_context.Students` (ou seja, adicione um "s" a `Student`).</span><span class="sxs-lookup"><span data-stu-id="7da58-274">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="7da58-275">7 ocorrências foram encontradas e atualizadas.</span><span class="sxs-lookup"><span data-stu-id="7da58-275">7 occurrences are found and updated.</span></span> <span data-ttu-id="7da58-276">Esperamos corrigir [este bug](https://github.com/aspnet/Scaffolding/issues/633) na próxima versão.</span><span class="sxs-lookup"><span data-stu-id="7da58-276">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="7da58-277">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="7da58-277">Test the app</span></span>

<span data-ttu-id="7da58-278">Execute o aplicativo e selecione o link **Alunos**.</span><span class="sxs-lookup"><span data-stu-id="7da58-278">Run the app and select the **Students** link.</span></span> <span data-ttu-id="7da58-279">Dependendo da largura do navegador, o link **Alunos** é exibido na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="7da58-279">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="7da58-280">Se o link **Alunos** não estiver visível, clique no ícone de navegação no canto superior direito.</span><span class="sxs-lookup"><span data-stu-id="7da58-280">If the **Students** link isn't visible, click the navigation icon in the upper right corner.</span></span>

![Home page estreita da Contoso University](intro/_static/home-page-narrow.png)

<span data-ttu-id="7da58-282">Teste os links **Criar**, **Editar** e **Detalhes**.</span><span class="sxs-lookup"><span data-stu-id="7da58-282">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="7da58-283">Exibir o BD</span><span class="sxs-lookup"><span data-stu-id="7da58-283">View the DB</span></span>

<span data-ttu-id="7da58-284">Quando o aplicativo é iniciado, `DbInitializer.Initialize` chama `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="7da58-284">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="7da58-285">`EnsureCreated` detecta se o BD existe e cria um, se necessário.</span><span class="sxs-lookup"><span data-stu-id="7da58-285">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="7da58-286">Se não houver nenhum aluno no BD, o método `Initialize` adicionará alunos.</span><span class="sxs-lookup"><span data-stu-id="7da58-286">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="7da58-287">Abra o **SSOX** (Pesquisador de Objetos do SQL Server) no menu **Exibir** do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7da58-287">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="7da58-288">No SSOX, clique em **(localdb)\MSSQLLocalDB > Bancos de Dados > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="7da58-288">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="7da58-289">Expanda o nó **Tabelas**.</span><span class="sxs-lookup"><span data-stu-id="7da58-289">Expand the **Tables** node.</span></span>

<span data-ttu-id="7da58-290">Clique com o botão direito do mouse na tabela **Aluno** e clique em **Exibir Dados** para ver as colunas criadas e as linhas inseridas na tabela.</span><span class="sxs-lookup"><span data-stu-id="7da58-290">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="7da58-291">Os arquivos *.mdf* e *.ldf* do BD estão localizados na pasta *C:\Users\\<yourusername>*.</span><span class="sxs-lookup"><span data-stu-id="7da58-291">The *.mdf* and *.ldf* DB files are in the *C:\Users\\<yourusername>* folder.</span></span>

<span data-ttu-id="7da58-292">`EnsureCreated` é chamado na inicialização do aplicativo, que permite que o seguinte fluxo de trabalho:</span><span class="sxs-lookup"><span data-stu-id="7da58-292">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="7da58-293">Exclua o BD.</span><span class="sxs-lookup"><span data-stu-id="7da58-293">Delete the DB.</span></span>
* <span data-ttu-id="7da58-294">Altere o esquema de BD (por exemplo, adicione um campo `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="7da58-294">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="7da58-295">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7da58-295">Run the app.</span></span>

<span data-ttu-id="7da58-296">`EnsureCreated` cria um BD com a coluna `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="7da58-296">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="7da58-297">Convenções</span><span class="sxs-lookup"><span data-stu-id="7da58-297">Conventions</span></span>

<span data-ttu-id="7da58-298">A quantidade de código feita para que o EF Core crie um BD completo é mínima, devido ao uso de convenções ou de suposições feitas pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="7da58-298">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="7da58-299">Os nomes de propriedades `DbSet` são usadas como nomes de tabela.</span><span class="sxs-lookup"><span data-stu-id="7da58-299">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="7da58-300">Para entidades não referenciadas por uma propriedade `DbSet`, os nomes de classe de entidade são usados como nomes de tabela.</span><span class="sxs-lookup"><span data-stu-id="7da58-300">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="7da58-301">Os nomes de propriedade de entidade são usados para nomes de coluna.</span><span class="sxs-lookup"><span data-stu-id="7da58-301">Entity property names are used for column names.</span></span>

* <span data-ttu-id="7da58-302">As propriedades de entidade que são nomeadas ID ou classnameID são reconhecidas como propriedades de chave primária.</span><span class="sxs-lookup"><span data-stu-id="7da58-302">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="7da58-303">Uma propriedade é interpretada como uma propriedade de chave estrangeira se ela é nomeada *<navigation property name><primary key property name>* (por exemplo, `StudentID` para a propriedade de navegação `Student`, pois a chave primária da entidade `Student` é `ID`).</span><span class="sxs-lookup"><span data-stu-id="7da58-303">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="7da58-304">As propriedades de chave estrangeira podem ser nomeadas *<primary key property name>* (por exemplo, `EnrollmentID`, pois a chave primária da entidade `Enrollment` é `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="7da58-304">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="7da58-305">O comportamento convencional pode ser substituído.</span><span class="sxs-lookup"><span data-stu-id="7da58-305">Conventional behavior can be overridden.</span></span> <span data-ttu-id="7da58-306">Por exemplo, os nomes de tabela podem ser especificados de forma explícita, conforme mostrado anteriormente neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="7da58-306">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="7da58-307">Os nomes de coluna podem ser definidos de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="7da58-307">The column names can be explicitly set.</span></span> <span data-ttu-id="7da58-308">As chaves primária e estrangeira podem ser definidas de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="7da58-308">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="7da58-309">Código assíncrono</span><span class="sxs-lookup"><span data-stu-id="7da58-309">Asynchronous code</span></span>

<span data-ttu-id="7da58-310">A programação assíncrona é o modo padrão do ASP.NET Core e EF Core.</span><span class="sxs-lookup"><span data-stu-id="7da58-310">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="7da58-311">Um servidor Web tem um número limitado de threads disponíveis e, em situações de alta carga, todos os threads disponíveis podem estar em uso.</span><span class="sxs-lookup"><span data-stu-id="7da58-311">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="7da58-312">Quando isso acontece, o servidor não pode processar novas solicitações até que os threads são liberados.</span><span class="sxs-lookup"><span data-stu-id="7da58-312">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="7da58-313">Com um código síncrono, muitos threads podem ser vinculados enquanto realmente não são fazendo nenhum trabalho porque estão aguardando a conclusão da E/S.</span><span class="sxs-lookup"><span data-stu-id="7da58-313">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="7da58-314">Com um código assíncrono, quando um processo está aguardando a conclusão da E/S, seu thread é liberado para o servidor para ser usado para processar outras solicitações.</span><span class="sxs-lookup"><span data-stu-id="7da58-314">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="7da58-315">Como resultado, o código assíncrono permite que os recursos do servidor sejam usados com mais eficiência, e o servidor fica capacitado a manipular mais tráfego sem atrasos.</span><span class="sxs-lookup"><span data-stu-id="7da58-315">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="7da58-316">O código assíncrono introduz uma pequena quantidade de sobrecarga em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="7da58-316">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="7da58-317">Para situações de baixo tráfego, o impacto no desempenho é insignificante, enquanto para situações de alto tráfego, a melhoria de desempenho potencial é significativa.</span><span class="sxs-lookup"><span data-stu-id="7da58-317">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="7da58-318">No código a seguir, a palavra-chave `async`, o valor retornado `Task<T>`, a palavra-chave `await` e o método `ToListAsync` fazem o código ser executado de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="7da58-318">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="7da58-319">A palavra-chave `async` instrui o compilador a:</span><span class="sxs-lookup"><span data-stu-id="7da58-319">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="7da58-320">Gerar retornos de chamada para partes do corpo do método.</span><span class="sxs-lookup"><span data-stu-id="7da58-320">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="7da58-321">Criar automaticamente o objeto [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) que é retornado.</span><span class="sxs-lookup"><span data-stu-id="7da58-321">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that's returned.</span></span> <span data-ttu-id="7da58-322">Para obter mais informações, consulte [Tipo de retorno de Tarefa](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="7da58-322">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="7da58-323">O tipo de retorno implícito `Task` representa um trabalho em andamento.</span><span class="sxs-lookup"><span data-stu-id="7da58-323">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="7da58-324">A palavra-chave `await` faz com que o compilador divida o método em duas partes.</span><span class="sxs-lookup"><span data-stu-id="7da58-324">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="7da58-325">A primeira parte termina com a operação que é iniciada de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="7da58-325">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="7da58-326">A segunda parte é colocada em um método de retorno de chamada que é chamado quando a operação é concluída.</span><span class="sxs-lookup"><span data-stu-id="7da58-326">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="7da58-327">`ToListAsync` é a versão assíncrona do método de extensão `ToList`.</span><span class="sxs-lookup"><span data-stu-id="7da58-327">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="7da58-328">Algumas coisas a serem consideradas ao escrever um código assíncrono que usa o EF Core:</span><span class="sxs-lookup"><span data-stu-id="7da58-328">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="7da58-329">Somente instruções que fazem com que consultas ou comandos sejam enviados ao BD são executadas de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="7da58-329">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="7da58-330">Isso inclui `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` e `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="7da58-330">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="7da58-331">Isso não inclui instruções que apenas alteram um `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="7da58-331">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="7da58-332">Um contexto do EF Core não é thread-safe: não tente realizar várias operações em paralelo.</span><span class="sxs-lookup"><span data-stu-id="7da58-332">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="7da58-333">Para aproveitar os benefícios de desempenho do código assíncrono, verifique se os pacotes de biblioteca (como para paginação) usam o código assíncrono se eles chamam métodos do EF Core que enviam consultas ao BD.</span><span class="sxs-lookup"><span data-stu-id="7da58-333">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="7da58-334">Para obter mais informações sobre a programação assíncrona no .NET, consulte [Visão geral da programação assíncrona](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="7da58-334">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="7da58-335">No próximo tutorial, as operações CRUD (criar, ler, atualizar e excluir) básicas são examinadas.</span><span class="sxs-lookup"><span data-stu-id="7da58-335">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7da58-336">Avançar</span><span class="sxs-lookup"><span data-stu-id="7da58-336">Next</span></span>](xref:data/ef-rp/crud)
