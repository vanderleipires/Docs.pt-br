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
ms.openlocfilehash: 86f9eceb5b8646e371811fa4611a4509ff652231
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/09/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="f9f2c-104">Guia de Introdução com páginas Razor e o Entity Framework Core usando o Visual Studio (1 a 8)</span><span class="sxs-lookup"><span data-stu-id="f9f2c-104">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="f9f2c-105">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f9f2c-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f9f2c-106">O aplicativo de web de exemplo Contoso University demonstra como criar aplicativos de web MVC do ASP.NET Core 2.0 usando o núcleo do Entity Framework (EF) 2.0 e o Visual Studio de 2017.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-106">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="f9f2c-107">O aplicativo de exemplo é um site de uma universidade Contoso fictícia.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-107">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="f9f2c-108">Ele inclui a funcionalidade como admissão do aluno, criação de curso e atribuições do instrutor.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-108">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="f9f2c-109">Esta página é o primeiro em uma série de tutoriais que explicam como criar o aplicativo de exemplo Contoso University.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-109">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="f9f2c-110">Baixar ou exibir o aplicativo concluído.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-110">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="f9f2c-111">[Instruções de download](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-111">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f9f2c-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="f9f2c-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

<span data-ttu-id="f9f2c-113">Familiaridade com [páginas Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-113">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="f9f2c-114">Os programadores novo devem ser concluída [começar com páginas Razor](xref:tutorials/razor-pages/razor-pages-start) antes de iniciar esta série.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-114">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f9f2c-115">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="f9f2c-115">Troubleshooting</span></span>

<span data-ttu-id="f9f2c-116">Se você tiver um problema que não é possível resolver, você pode encontrar a solução geralmente comparando seu código para o [concluída estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) ou [projeto concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-116">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="f9f2c-117">Para obter uma lista de erros comuns e como resolvê-los, consulte [a seção de solução de problemas do tutorial do último na série](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-117">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="f9f2c-118">Se você não encontrar o que você precisa lá, você pode postar uma pergunta em StackOverflow.com para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-118">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="f9f2c-119">Esta série de tutoriais amplia o que é feito nos tutoriais anteriores.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-119">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="f9f2c-120">Considere a possibilidade de salvar uma cópia do projeto após cada tutorial foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-120">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="f9f2c-121">Se você tiver problemas, você poderá começar desde o tutorial anterior em vez de voltar ao início.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-121">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="f9f2c-122">Como alternativa, você pode baixar um [concluída estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) e inicie novamente usando o estágio concluído.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-122">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="f9f2c-123">O aplicativo web Contoso University</span><span class="sxs-lookup"><span data-stu-id="f9f2c-123">The Contoso University web app</span></span>

<span data-ttu-id="f9f2c-124">O aplicativo compilado nos tutoriais é um site university básico.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-124">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="f9f2c-125">Os usuários podem exibir e atualizar aluno, curso e informações do instrutor.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-125">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="f9f2c-126">Aqui estão algumas das telas do criado no tutorial.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-126">Here are a few of the screens created in the tutorial.</span></span>

![Página de índice de alunos](intro/_static/students-index.png)

![Página de edição de alunos](intro/_static/student-edit.png)

<span data-ttu-id="f9f2c-129">O estilo de interface do usuário deste site é quase o que é gerado pelos modelos internos.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-129">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="f9f2c-130">É o foco de tutorial no EF Core com páginas Razor, não a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-130">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="f9f2c-131">Criar um aplicativo da web de páginas Razor</span><span class="sxs-lookup"><span data-stu-id="f9f2c-131">Create a Razor Pages web app</span></span>

* <span data-ttu-id="f9f2c-132">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-132">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f9f2c-133">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-133">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="f9f2c-134">Nomeie o projeto **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-134">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="f9f2c-135">É importante para o nome do projeto *ContosoUniversity* para os namespaces corresponda ao código é copiar/colar.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-135">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="f9f2c-136">![novo aplicativo Web ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="f9f2c-136">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="f9f2c-137">Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-137">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="f9f2c-138">![Aplicativo Web (Páginas do Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="f9f2c-138">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="f9f2c-139">Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador</span><span class="sxs-lookup"><span data-stu-id="f9f2c-139">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="f9f2c-140">Definir o estilo de site</span><span class="sxs-lookup"><span data-stu-id="f9f2c-140">Set up the site style</span></span>

<span data-ttu-id="f9f2c-141">Algumas alterações de configurar o menu de site, o layout e a página inicial.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-141">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="f9f2c-142">Abra *Pages/_Layout.cshtml* e faça as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-142">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="f9f2c-143">Alterar cada ocorrência de "ContosoUniversity" para "Contoso University."</span><span class="sxs-lookup"><span data-stu-id="f9f2c-143">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="f9f2c-144">Há três ocorrências.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-144">There are three occurrences.</span></span>

* <span data-ttu-id="f9f2c-145">Adicionar entradas de menu de **alunos**, **cursos**, **instrutores**, e **departamentos**e exclua o **contato** entrada de menu.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-145">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="f9f2c-146">As alterações são realçadas.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-146">The changes are highlighted.</span></span> <span data-ttu-id="f9f2c-147">(Todos os a marcação é *não* exibidos.)</span><span class="sxs-lookup"><span data-stu-id="f9f2c-147">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="f9f2c-148">Em *Pages/Index.cshtml*, substitua o conteúdo do arquivo com o código a seguir para substituir o texto sobre o ASP.NET e MVC com o texto sobre este aplicativo:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-148">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="f9f2c-149">Pressione CTRL+F5 para executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-149">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="f9f2c-150">A home page é exibida com as guias criadas nos tutoriais a seguir:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-150">The home page is displayed with tabs created in the following tutorials:</span></span>

![Home page do Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="f9f2c-152">Criar o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="f9f2c-152">Create the data model</span></span>

<span data-ttu-id="f9f2c-153">Crie classes de entidade para o aplicativo da Contoso University.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-153">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="f9f2c-154">Comece com as três seguintes entidades:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-154">Start with the following three entities:</span></span>

![Diagrama de modelo de dados do aluno de registro de curso](intro/_static/data-model-diagram.png)

<span data-ttu-id="f9f2c-156">Há uma relação um-para-muitos entre `Student` e `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-156">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="f9f2c-157">Há uma relação um-para-muitos entre `Course` e `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-157">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="f9f2c-158">Um aluno pode registrar qualquer número de cursos.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-158">A student can enroll in any number of courses.</span></span> <span data-ttu-id="f9f2c-159">Um curso pode ter qualquer número de alunos registrados nele.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-159">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="f9f2c-160">As seções a seguir, uma classe para cada uma dessas entidades é criada.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-160">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="f9f2c-161">A entidade do aluno</span><span class="sxs-lookup"><span data-stu-id="f9f2c-161">The Student entity</span></span>

![Diagrama de entidade do aluno](intro/_static/student-entity.png)

<span data-ttu-id="f9f2c-163">Criar um *modelos* pasta.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-163">Create a *Models* folder.</span></span> <span data-ttu-id="f9f2c-164">No *modelos* pasta, crie um arquivo de classe chamado *Student.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-164">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="f9f2c-165">O `ID` propriedade torna-se a coluna de chave primária da tabela de banco de dados que corresponde a essa classe.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-165">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="f9f2c-166">Por padrão, o núcleo de EF interpreta uma propriedade denominada `ID` ou `classnameID` como a chave primária.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-166">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="f9f2c-167">O `Enrollments` propriedade é uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-167">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="f9f2c-168">Link de propriedades de navegação para outras entidades que estão relacionados a esta entidade.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-168">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="f9f2c-169">Nesse caso, o `Enrollments` propriedade de um `Student entity` mantém todos o `Enrollment` entidades relacionadas ao `Student`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-169">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="f9f2c-170">Por exemplo, se uma linha de Student no banco de dados tem dois relacionados linhas de registro, o `Enrollments` propriedade de navegação contém dois `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-170">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="f9f2c-171">Um relacionados `Enrollment` linha é uma linha que contém o valor de chave primária que student no `StudentID` coluna.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-171">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="f9f2c-172">Por exemplo, suponha que o aluno com ID = 1 tem duas linhas `Enrollment` tabela.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-172">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="f9f2c-173">O `Enrollment` tabela tem duas linhas com `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-173">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="f9f2c-174">`StudentID`é uma chave estrangeira no `Enrollment` tabela que especifica o aluno no `Student` tabela.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-174">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="f9f2c-175">Se uma propriedade de navegação pode conter várias entidades, a propriedade de navegação deve ser um tipo de lista, como `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-175">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="f9f2c-176">`ICollection<T>`pode ser especificado, ou um tipo, como `List<T>` ou `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-176">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="f9f2c-177">Quando `ICollection<T>` é usada, o núcleo do EF cria um `HashSet<T>` coleção por padrão.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-177">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="f9f2c-178">Propriedades de navegação que contêm várias entidades provenientes de relações muitos-para-muitos e um-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-178">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="f9f2c-179">A entidade de registro</span><span class="sxs-lookup"><span data-stu-id="f9f2c-179">The Enrollment entity</span></span>

![Diagrama de entidade do registro](intro/_static/enrollment-entity.png)

<span data-ttu-id="f9f2c-181">No *modelos* pasta, criar *Enrollment.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-181">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="f9f2c-182">O `EnrollmentID` propriedade é a chave primária.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-182">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="f9f2c-183">Esta entidade usa o `classnameID` padrão em vez de `ID` como o `Student` entidade.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-183">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="f9f2c-184">Normalmente, os desenvolvedores escolher um padrão e usá-lo em todo o modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-184">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="f9f2c-185">Um tutorial posterior, usando uma ID sem classname é mostrado para tornar mais fácil de implementar a herança no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-185">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="f9f2c-186">O `Grade` propriedade é um `enum`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-186">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="f9f2c-187">O ponto de interrogação após o `Grade` declaração de tipo indica que o `Grade` propriedade é anulável.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-187">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="f9f2c-188">Uma classificação que é null é diferente de uma classificação zero - nulo significa uma série não é conhecida ou ainda não foi atribuída.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-188">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="f9f2c-189">O `StudentID` propriedade é uma chave estrangeira e a propriedade de navegação correspondente é `Student`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-189">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="f9f2c-190">Um `Enrollment` entidade está associada um `Student` entidade, portanto, a propriedade contém um único `Student` entidade.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-190">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="f9f2c-191">O `Student` entidade difere do `Student.Enrollments` propriedade de navegação, que contém várias `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-191">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="f9f2c-192">O `CourseID` propriedade é uma chave estrangeira e a propriedade de navegação correspondente é `Course`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-192">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="f9f2c-193">Um `Enrollment` entidade está associada um `Course` entidade.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-193">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="f9f2c-194">EF Core interpreta uma propriedade como uma chave estrangeira, se ele é nomeado `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-194">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="f9f2c-195">Por exemplo,`StudentID` para o `Student` propriedade de navegação, como o `Student` chave primária da entidade é `ID`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-195">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="f9f2c-196">Propriedades de chave estrangeira também podem ser nomeadas `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-196">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="f9f2c-197">Por exemplo, `CourseID` desde o `Course` chave primária da entidade é `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-197">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="f9f2c-198">A entidade de curso</span><span class="sxs-lookup"><span data-stu-id="f9f2c-198">The Course entity</span></span>

![Diagrama de entidades de curso](intro/_static/course-entity.png)

<span data-ttu-id="f9f2c-200">No *modelos* pasta, criar *Course.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-200">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="f9f2c-201">O `Enrollments` propriedade é uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-201">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="f9f2c-202">Um `Course` entidade pode estar relacionada a qualquer número de `Enrollment` entidades.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-202">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="f9f2c-203">O `DatabaseGenerated` atributo permite que o aplicativo especificar a chave primária em vez de ter o banco de dados gerá-lo.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-203">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="f9f2c-204">Criar o contexto de banco de dados SchoolContext</span><span class="sxs-lookup"><span data-stu-id="f9f2c-204">Create the SchoolContext DB context</span></span>

<span data-ttu-id="f9f2c-205">A classe principal que coordena a funcionalidade principal do EF de um modelo de dados é a classe de contexto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-205">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="f9f2c-206">O contexto de dados é derivado de `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-206">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="f9f2c-207">O contexto de dados especifica quais entidades são incluídas no modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-207">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="f9f2c-208">Neste projeto, a classe é nomeada `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-208">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="f9f2c-209">Na pasta do projeto, crie uma pasta chamada *dados*.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-209">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="f9f2c-210">No *dados* criar pasta *SchoolContext.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-210">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="f9f2c-211">Esse código cria um `DbSet` propriedade para cada conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-211">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="f9f2c-212">Na terminologia de EF principais:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-212">In EF Core terminology:</span></span>

* <span data-ttu-id="f9f2c-213">Uma entidade definida normalmente corresponde a uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-213">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="f9f2c-214">Uma entidade corresponde a uma linha na tabela.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-214">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="f9f2c-215">`DbSet<Enrollment>`e `DbSet<Course>` pode ser omitido.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-215">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="f9f2c-216">EF Core inclui-los ou implicitamente porque o `Student` referências de entidade de `Enrollment` entidade e o `Enrollment` referências de entidade o `Course` entidade.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-216">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="f9f2c-217">Para este tutorial, mantenha `DbSet<Enrollment>` e `DbSet<Course>` no `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-217">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="f9f2c-218">Quando o banco de dados é criado, Core EF cria tabelas com nomes iguais a `DbSet` nomes de propriedade.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-218">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="f9f2c-219">Nomes de propriedade para coleções são normalmente plurais (alunos em vez de aluno).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-219">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="f9f2c-220">Os desenvolvedores não concordo sobre se os nomes de tabela devem ser plural.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-220">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="f9f2c-221">Para esses tutoriais, o comportamento padrão é substituído pela especificação de nomes de tabela única no DbContext.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-221">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="f9f2c-222">Para especificar nomes de tabela única, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-222">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="f9f2c-223">Registrar o contexto de injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="f9f2c-223">Register the context with dependency injection</span></span>

<span data-ttu-id="f9f2c-224">Inclui o ASP.NET Core [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-224">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f9f2c-225">Serviços (como o contexto de DB de núcleo EF) são registrados com injeção de dependência durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-225">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="f9f2c-226">Componentes que exigem que esses serviços (como páginas Razor) são fornecidas esses serviços por meio de parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-226">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="f9f2c-227">O código de construtor que obtém uma instância de contexto do banco de dados é mostrado posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-227">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="f9f2c-228">Para registrar `SchoolContext` como um serviço, abra *Startup.cs*e adicione as linhas destacadas para o `ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-228">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="f9f2c-229">O nome da cadeia de caracteres de conexão é passado para o contexto chamando um método em um `DbContextOptionsBuilder` objeto.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-229">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="f9f2c-230">Para desenvolvimento local, o [sistema de configuração do ASP.NET Core](xref:fundamentals/configuration/index) lê a cadeia de caracteres de conexão a *appSettings. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-230">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="f9f2c-231">Adicionar `using` instruções para `ContosoUniversity.Data` e `Microsoft.EntityFrameworkCore` namespaces.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-231">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="f9f2c-232">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-232">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="f9f2c-233">Abra o *appSettings. JSON* de arquivos e adicionar uma cadeia de caracteres de conexão, conforme mostrado no código a seguir:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-233">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="f9f2c-234">A cadeia de caracteres de conexão anterior usa `ConnectRetryCount=0` para evitar [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) de deslocamento.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-234">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="f9f2c-235">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="f9f2c-235">SQL Server Express LocalDB</span></span>

<span data-ttu-id="f9f2c-236">A cadeia de caracteres de conexão Especifica um banco de dados do SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-236">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="f9f2c-237">LocalDB é uma versão leve do mecanismo de banco de dados do SQL Server Express e destina-se ao desenvolvimento de aplicativos, não o uso de produção.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-237">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="f9f2c-238">O LocalDB é iniciado sob demanda e executado no modo de usuário e, portanto, não há nenhuma configuração complexa.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-238">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="f9f2c-239">Por padrão, o LocalDB cria *. mdf* arquivos de banco de dados no `C:/Users/<user>` directory.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-239">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="f9f2c-240">Adicione código para inicializar o banco de dados com dados de teste</span><span class="sxs-lookup"><span data-stu-id="f9f2c-240">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="f9f2c-241">EF principal cria um banco de dados vazio.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-241">EF Core creates an empty DB.</span></span> <span data-ttu-id="f9f2c-242">Nesta seção, uma *semente* método é gravada para preenchê-la com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-242">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="f9f2c-243">No *dados* pasta, crie um novo arquivo de classe chamado *DbInitializer.cs* e adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-243">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="f9f2c-244">O código verifica se há algum dos alunos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-244">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="f9f2c-245">Se não houver nenhum alunos no banco de dados, o banco de dados é propagado com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-245">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="f9f2c-246">Ele carrega os dados de teste em matrizes em vez de `List<T>` coleções para otimizar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-246">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="f9f2c-247">O `EnsureCreated` método cria automaticamente o banco de dados para o contexto do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-247">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="f9f2c-248">Se o banco de dados existir, `EnsureCreated` retorna sem modificar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-248">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="f9f2c-249">Em *Program.cs*, modifique o `Main` método para fazer o seguinte:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-249">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="f9f2c-250">Obtenha uma instância de contexto do banco de dados do contêiner de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-250">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="f9f2c-251">Chame o método de propagação, transmitindo a ele o contexto.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-251">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="f9f2c-252">Descarte o contexto quando o método de propagação é concluído.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-252">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="f9f2c-253">O código a seguir mostra a atualização *Program.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-253">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="f9f2c-254">A primeira vez que o aplicativo é executado, o banco de dados é criado e propagado com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-254">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="f9f2c-255">Quando o modelo de dados é atualizado:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-255">When the data model is updated:</span></span>
* <span data-ttu-id="f9f2c-256">Exclua o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-256">Delete the DB.</span></span>
* <span data-ttu-id="f9f2c-257">Atualize o método de propagação.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-257">Update the seed method.</span></span>
* <span data-ttu-id="f9f2c-258">Executar o aplicativo e um novo banco de dados de propagação é criado.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-258">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="f9f2c-259">Em tutoriais subsequentes, o banco de dados é atualizado quando os dados de modelo alterações, sem excluir e recriar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-259">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="f9f2c-260">Adicionar ferramentas de scaffold</span><span class="sxs-lookup"><span data-stu-id="f9f2c-260">Add scaffold tooling</span></span>

<span data-ttu-id="f9f2c-261">Nesta seção, o Console de Gerenciador de pacote (PMC) é usado para adicionar o pacote de geração de código do Visual Studio web.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-261">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="f9f2c-262">Esse pacote é necessário para executar o mecanismo de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-262">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="f9f2c-263">No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-263">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="f9f2c-264">No pacote Manager Console (PMC), digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-264">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="f9f2c-265">O comando anterior adiciona os pacotes do NuGet para o arquivo *. csproj:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-265">The previous command adds the NuGet packages to the *.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="f9f2c-266">O modelo de Scaffold</span><span class="sxs-lookup"><span data-stu-id="f9f2c-266">Scaffold the model</span></span>

* <span data-ttu-id="f9f2c-267">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f9f2c-268">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-268">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="f9f2c-269">Se o seguinte erro é gerado:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-269">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="f9f2c-270">Execute o comando novamente e deixar um comentário na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-270">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="f9f2c-271">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-271">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="f9f2c-272">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-272">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="f9f2c-273">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-273">Build the project.</span></span> <span data-ttu-id="f9f2c-274">A compilação gera erros, como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-274">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="f9f2c-275">Globalmente altere `_context.Student` para `_context.Students` (ou seja, adicionar um "s" para `Student`).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-275">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="f9f2c-276">7 ocorrências forem encontradas e atualizadas.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-276">7 occurrences are found and updated.</span></span> <span data-ttu-id="f9f2c-277">Esperamos que corrigir [esse bug](https://github.com/aspnet/Scaffolding/issues/633) na próxima versão.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-277">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="f9f2c-278">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="f9f2c-278">Test the app</span></span>

<span data-ttu-id="f9f2c-279">Execute o aplicativo e selecione o **alunos** link.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-279">Run the app and select the **Students** link.</span></span> <span data-ttu-id="f9f2c-280">Dependendo da largura do navegador, o **alunos** link aparece na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-280">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="f9f2c-281">Se o **alunos** link não estiver visível, clique no ícone de navegação no canto superior direito.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-281">If the **Students** link is not visible, click the navigation icon in the upper right corner.</span></span>

![Página inicial da Contoso University estreita](intro/_static/home-page-narrow.png)

<span data-ttu-id="f9f2c-283">Teste o **criar**, **editar**, e **detalhes** links.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-283">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="f9f2c-284">Exibir o banco de dados</span><span class="sxs-lookup"><span data-stu-id="f9f2c-284">View the DB</span></span>

<span data-ttu-id="f9f2c-285">Quando o aplicativo é iniciado, `DbInitializer.Initialize` chamadas `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-285">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="f9f2c-286">`EnsureCreated`detecta se o banco de dados existe e cria um, se necessário.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-286">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="f9f2c-287">Se não houver nenhum alunos no banco de dados, o `Initialize` método adiciona os alunos.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-287">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="f9f2c-288">Abra **Pesquisador de objetos do SQL Server** (SSOX) da **exibição** menu do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-288">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="f9f2c-289">No SSOX, clique em **(localdb) \MSSQLLocalDB > bancos de dados > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-289">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="f9f2c-290">Expanda o **tabelas** nó.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-290">Expand the **Tables** node.</span></span>

<span data-ttu-id="f9f2c-291">Com o botão direito do **aluno** de tabela e clique em **exibir dados** para ver as colunas criadas e as linhas inseridas na tabela.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-291">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="f9f2c-292">O *. mdf* e *. ldf* arquivos de banco de dados estão no *C:\Users\\ <yourusername>*  pasta.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-292">The *.mdf* and *.ldf* DB files are in the *C:\Users\\<yourusername>* folder.</span></span>

<span data-ttu-id="f9f2c-293">`EnsureCreated`é chamado no início do aplicativo, que permite que o fluxo de trabalho a seguir:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-293">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="f9f2c-294">Exclua o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-294">Delete the DB.</span></span>
* <span data-ttu-id="f9f2c-295">Alterar o esquema de banco de dados (por exemplo, adicionar um `EmailAddress` campo).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-295">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="f9f2c-296">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-296">Run the app.</span></span>

<span data-ttu-id="f9f2c-297">`EnsureCreated`cria um banco de dados com o`EmailAddress` coluna.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-297">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="f9f2c-298">Convenções</span><span class="sxs-lookup"><span data-stu-id="f9f2c-298">Conventions</span></span>

<span data-ttu-id="f9f2c-299">A quantidade de código escrito em ordem para EF principal criar um banco de dados completo é mínima devido ao uso de convenções ou suposições que torna o EF Core.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-299">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="f9f2c-300">Os nomes de `DbSet` propriedades são usadas como nomes de tabela.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-300">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="f9f2c-301">Para entidades que não é referenciadas por um `DbSet` propriedade, a classe da entidade nomes são usados como nomes de tabela.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-301">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="f9f2c-302">Nomes de propriedade de entidade são usados para nomes de coluna.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-302">Entity property names are used for column names.</span></span>

* <span data-ttu-id="f9f2c-303">Propriedades de entidade que são nomeadas ID ou classnameID são reconhecidas como propriedades de chave primárias.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-303">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="f9f2c-304">Uma propriedade é interpretada como uma propriedade de chave estrangeira, se ele é nomeado  *<navigation property name> <primary key property name>*  (por exemplo, `StudentID` para o `Student` propriedade de navegação desde o `Student` é de chave primária da entidade `ID`).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-304">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="f9f2c-305">Propriedades de chave estrangeira que podem ser nomeadas  *<primary key property name>*  (por exemplo, `EnrollmentID` desde o `Enrollment` chave primária da entidade é `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-305">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="f9f2c-306">Comportamento convencional pode ser substituído.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-306">Conventional behavior can be overridden.</span></span> <span data-ttu-id="f9f2c-307">Por exemplo, os nomes de tabela podem ser especificados explicitamente, como mostrado anteriormente neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-307">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="f9f2c-308">Os nomes de coluna podem ser definidos explicitamente.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-308">The column names can be explicitly set.</span></span> <span data-ttu-id="f9f2c-309">As chaves primária e estrangeira pode ser definida explicitamente.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-309">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="f9f2c-310">Código assíncrono</span><span class="sxs-lookup"><span data-stu-id="f9f2c-310">Asynchronous code</span></span>

<span data-ttu-id="f9f2c-311">Programação assíncrona é o modo padrão do ASP.NET Core e EF Core.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-311">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="f9f2c-312">Um servidor web tem um número limitado de threads disponíveis e, em situações de alta carga de todos os threads disponíveis podem estar em uso.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-312">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="f9f2c-313">Quando isso acontece, o servidor não pode processar novas solicitações até que os threads são liberados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-313">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="f9f2c-314">Com código síncrono, muitos threads podem ser vinculados ao enquanto eles não são realmente fazendo qualquer trabalho porque está aguardando para e/s concluir.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-314">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="f9f2c-315">Com um código assíncrono, quando um processo está aguardando e/s ser concluída, o thread é liberado para o servidor a ser usado para processar outras solicitações.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-315">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="f9f2c-316">Como resultado, o código assíncrono permite que os recursos de servidor a ser usado com mais eficiência, e o servidor está habilitado para manipular mais tráfego sem atrasos.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-316">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="f9f2c-317">Código assíncrono apresentam uma pequena quantidade de sobrecarga em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-317">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="f9f2c-318">Para situações de tráfego baixo, o impacto no desempenho é insignificante, durante situações de alto tráfego, a melhoria de desempenho potencial é significativa.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-318">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="f9f2c-319">No código a seguir, o `async` palavra-chave, `Task<T>` retornar valor, `await` palavra-chave, e `ToListAsync` método tornar o código executar de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-319">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="f9f2c-320">O `async` palavra-chave informa ao compilador para:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-320">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="f9f2c-321">Gere retornos de chamada para partes do corpo do método.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-321">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="f9f2c-322">Criar automaticamente o [tarefa](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) objeto que é retornado.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-322">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that is returned.</span></span> <span data-ttu-id="f9f2c-323">Para obter mais informações, consulte [o tipo de retorno de tarefa](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-323">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="f9f2c-324">Tipo de retorno de implícita `Task` representa um trabalho em andamento.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-324">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="f9f2c-325">O `await` palavra-chave faz com que o compilador dividir o método em duas partes.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-325">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="f9f2c-326">A primeira parte termina com a operação que é iniciada de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-326">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="f9f2c-327">A segunda parte é colocada em um método de retorno de chamada que é chamado quando a operação for concluída.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-327">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="f9f2c-328">`ToListAsync`é a versão assíncrona do `ToList` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-328">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="f9f2c-329">Algumas coisas a serem consideradas ao escrever código assíncrono que usa EF Core:</span><span class="sxs-lookup"><span data-stu-id="f9f2c-329">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="f9f2c-330">Somente instruções que fazem com consultas ou comandos sejam enviados para o banco de dados que são executadas assincronamente.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-330">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="f9f2c-331">Isso inclui, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, e `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-331">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="f9f2c-332">Ele não inclui instruções que apenas alterar uma `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-332">It does not include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="f9f2c-333">Um contexto de EF Core não é thread safe: não tenta realizar várias operações em paralelo.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-333">An EF Core context is not threaded safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="f9f2c-334">Para tirar proveito dos benefícios de desempenho do código assíncrono, verifique se pacotes de biblioteca (como paginação) usam async se chamam métodos básicos de EF que enviam consultas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-334">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="f9f2c-335">Para obter mais informações sobre a programação assíncrona em .NET, consulte [visão geral de Async](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="f9f2c-335">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="f9f2c-336">No tutorial de Avançar, CRUD básica (criar, ler, atualizar e excluir) operações são examinadas.</span><span class="sxs-lookup"><span data-stu-id="f9f2c-336">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f9f2c-337">Avançar</span><span class="sxs-lookup"><span data-stu-id="f9f2c-337">Next</span></span>](xref:data/ef-rp/crud)
