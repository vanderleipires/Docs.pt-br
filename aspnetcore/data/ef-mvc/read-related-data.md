---
title: "Núcleo do ASP.NET MVC com núcleo EF - ler dados relacionados - 6 de 10"
author: tdykstra
description: "Neste tutorial você ler e exibir dados relacionados – ou seja, os dados que o Entity Framework carrega em Propriedades de navegação."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 1321cb00a432669b4a97ad20063b6cf9ea75f24c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a><span data-ttu-id="1ee56-103">Leitura relacionadas a dados - Core de EF com o tutorial do MVC do ASP.NET Core (6 de 10)</span><span class="sxs-lookup"><span data-stu-id="1ee56-103">Reading related data - EF Core with ASP.NET Core MVC tutorial (6 of 10)</span></span>

<span data-ttu-id="1ee56-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1ee56-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1ee56-105">O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ee56-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="1ee56-106">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="1ee56-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="1ee56-107">No tutorial anterior, você deve concluir o modelo de dados de escola.</span><span class="sxs-lookup"><span data-stu-id="1ee56-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="1ee56-108">Neste tutorial, você ler e exibir dados relacionados – ou seja, os dados que o Entity Framework carrega em Propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="1ee56-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="1ee56-109">As ilustrações a seguir mostram as páginas que você trabalhará com.</span><span class="sxs-lookup"><span data-stu-id="1ee56-109">The following illustrations show the pages that you'll work with.</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

![Página de índice instrutores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="1ee56-112">Carregamento de dados relacionados lento eager e explícita</span><span class="sxs-lookup"><span data-stu-id="1ee56-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="1ee56-113">Há várias maneiras que o software de mapeamento relacional de objeto (ORM), como o Entity Framework pode carregar os dados relacionados nas propriedades de navegação de uma entidade:</span><span class="sxs-lookup"><span data-stu-id="1ee56-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="1ee56-114">Carregamento adiantado.</span><span class="sxs-lookup"><span data-stu-id="1ee56-114">Eager loading.</span></span> <span data-ttu-id="1ee56-115">Quando a entidade é lido, os dados relacionados são recuperados com ela.</span><span class="sxs-lookup"><span data-stu-id="1ee56-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="1ee56-116">Normalmente, isso resulta em uma consulta de junção único que recupera todos os dados que é necessário.</span><span class="sxs-lookup"><span data-stu-id="1ee56-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="1ee56-117">Especifique o carregamento adiantado no Entity Framework Core usando o `Include` e `ThenInclude` métodos.</span><span class="sxs-lookup"><span data-stu-id="1ee56-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Exemplo de carregamento rápido](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="1ee56-119">Você pode recuperar os dados em consultas separadas e EF "correções de" as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="1ee56-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="1ee56-120">Ou seja, EF adiciona automaticamente as entidades separadamente recuperadas onde eles pertencem nas propriedades de navegação de entidades previamente recuperadas.</span><span class="sxs-lookup"><span data-stu-id="1ee56-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="1ee56-121">Para a consulta que recupera dados relacionados, você pode usar o `Load` método em vez de um método que retorna uma lista ou um objeto, como `ToList` ou `Single`.</span><span class="sxs-lookup"><span data-stu-id="1ee56-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Exemplo de consultas separadas](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="1ee56-123">Carregamento explícito.</span><span class="sxs-lookup"><span data-stu-id="1ee56-123">Explicit loading.</span></span> <span data-ttu-id="1ee56-124">Quando a entidade é lido pela primeira vez, os dados relacionados não são recuperados.</span><span class="sxs-lookup"><span data-stu-id="1ee56-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="1ee56-125">Você escreve o código que recupera os dados relacionados a se ela for necessária.</span><span class="sxs-lookup"><span data-stu-id="1ee56-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="1ee56-126">Como no caso de carregamento rápido com consultas separadas, explícita de carregamento dos resultados em várias consultas enviadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1ee56-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="1ee56-127">A diferença é que, com carregamento explícito, o código especifica as propriedades de navegação para ser carregado.</span><span class="sxs-lookup"><span data-stu-id="1ee56-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="1ee56-128">No Entity Framework Core 1.1, você pode usar o `Load` método de fazer o carregamento explícito.</span><span class="sxs-lookup"><span data-stu-id="1ee56-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="1ee56-129">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="1ee56-129">For example:</span></span>

  ![Exemplo de carregamento explícito](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="1ee56-131">Carregamento preguiçoso.</span><span class="sxs-lookup"><span data-stu-id="1ee56-131">Lazy loading.</span></span> <span data-ttu-id="1ee56-132">Quando a entidade é lido pela primeira vez, os dados relacionados não são recuperados.</span><span class="sxs-lookup"><span data-stu-id="1ee56-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="1ee56-133">No entanto, na primeira vez que tentar acessar uma propriedade de navegação, os dados necessários para essa propriedade de navegação são recuperados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="1ee56-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="1ee56-134">Uma consulta é enviada para o banco de dados cada vez que tentar obter dados de uma propriedade de navegação pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="1ee56-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="1ee56-135">Entity Framework Core 1.0 não oferece suporte a carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="1ee56-135">Entity Framework Core 1.0 does not support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="1ee56-136">Considerações sobre desempenho</span><span class="sxs-lookup"><span data-stu-id="1ee56-136">Performance considerations</span></span>

<span data-ttu-id="1ee56-137">Se você souber que você precisa de dados relacionados para cada entidade recuperada, carregamento adiantado geralmente oferece o melhor desempenho, porque uma única consulta enviada para o banco de dados é geralmente mais eficiente do que consultas separadas para cada entidade recuperada.</span><span class="sxs-lookup"><span data-stu-id="1ee56-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="1ee56-138">Por exemplo, suponha que cada departamento tem dez cursos relacionados.</span><span class="sxs-lookup"><span data-stu-id="1ee56-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="1ee56-139">Carregamento adiantado de todos os dados relacionados resultaria em apenas uma consulta única (associação) e uma única viagem de ida e ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1ee56-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="1ee56-140">Uma consulta separada para cursos para cada departamento resultaria em onze idas e voltas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1ee56-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="1ee56-141">As extras idas e voltas para o banco de dados são especialmente prejudiciais ao desempenho quando a latência é alta.</span><span class="sxs-lookup"><span data-stu-id="1ee56-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="1ee56-142">Por outro lado, em alguns cenários consultas separadas é mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="1ee56-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="1ee56-143">Carregamento adiantado de todos os dados relacionados em uma consulta pode causar uma junção muito complexa para ser gerado, que o SQL Server não pode processar com eficiência.</span><span class="sxs-lookup"><span data-stu-id="1ee56-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="1ee56-144">Ou, se você precisar acessar as propriedades de navegação de uma entidade somente para um subconjunto de um conjunto de entidades que você está processando, consultas separadas podem executar melhor, pois mais dados que você precisa recuperar o carregamento rápido de tudo, desde o início.</span><span class="sxs-lookup"><span data-stu-id="1ee56-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="1ee56-145">Se o desempenho for crítico, é melhor testar o desempenho das duas maneiras para fazer a melhor escolha.</span><span class="sxs-lookup"><span data-stu-id="1ee56-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="1ee56-146">Criar uma página de cursos que exibe o nome do departamento</span><span class="sxs-lookup"><span data-stu-id="1ee56-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="1ee56-147">A entidade de curso inclui uma propriedade de navegação que contém a entidade de departamento do departamento de curso é atribuído a.</span><span class="sxs-lookup"><span data-stu-id="1ee56-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="1ee56-148">Para exibir o nome do departamento atribuído em uma lista de cursos, você precisa obter a propriedade de nome da entidade de departamento que está no `Course.Department` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="1ee56-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that is in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="1ee56-149">Criar um controlador chamado CoursesController para o tipo de entidade do curso, usando as mesmas opções para o **controlador MVC com modos de exibição usando o Entity Framework** scaffolder que você usou anteriormente para o controlador de alunos, conforme o ilustração a seguir:</span><span class="sxs-lookup"><span data-stu-id="1ee56-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Adicionar controlador de cursos](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="1ee56-151">Abra *CoursesController.cs* e examine o `Index` método.</span><span class="sxs-lookup"><span data-stu-id="1ee56-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="1ee56-152">O scaffolding automática tiver especificado o carregamento rápido para o `Department` propriedade de navegação usando o `Include` método.</span><span class="sxs-lookup"><span data-stu-id="1ee56-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="1ee56-153">Substitua o `Index` método com o código a seguir que usa um nome mais apropriado para o `IQueryable` que retorna as entidades de curso (`courses` em vez de `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="1ee56-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="1ee56-154">Abra *Views/Courses/Index.cshtml* e substitua o código de modelo com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="1ee56-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="1ee56-155">As alterações são realçadas:</span><span class="sxs-lookup"><span data-stu-id="1ee56-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="1ee56-156">Você fez as alterações a seguir para o código de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="1ee56-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="1ee56-157">Alterado o título do índice para cursos.</span><span class="sxs-lookup"><span data-stu-id="1ee56-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="1ee56-158">Adicionado um **número** coluna mostra o `CourseID` o valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="1ee56-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="1ee56-159">Por padrão, as chaves primárias não são Scaffold porque normalmente eles não fazem sentidos para os usuários finais.</span><span class="sxs-lookup"><span data-stu-id="1ee56-159">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="1ee56-160">No entanto, nesse caso, a chave primária é significativa e você deseja mostrá-la.</span><span class="sxs-lookup"><span data-stu-id="1ee56-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="1ee56-161">Alterado o **departamento** coluna para exibir o nome de departamento.</span><span class="sxs-lookup"><span data-stu-id="1ee56-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="1ee56-162">Exibe o código de `Name` propriedade da entidade do departamento que é carregada no `Department` propriedade de navegação:</span><span class="sxs-lookup"><span data-stu-id="1ee56-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="1ee56-163">Execute o aplicativo e selecione o **cursos** guia para ver a lista com nomes de departamento.</span><span class="sxs-lookup"><span data-stu-id="1ee56-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="1ee56-165">Criar uma página de instrutores que mostra os cursos e registros</span><span class="sxs-lookup"><span data-stu-id="1ee56-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="1ee56-166">Nesta seção, você criará um controlador e o modo de exibição para a entidade instrutor para exibir a página de professores:</span><span class="sxs-lookup"><span data-stu-id="1ee56-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Página de índice instrutores](read-related-data/_static/instructors-index.png)

<span data-ttu-id="1ee56-168">Essa página lê e exibe dados relacionados das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="1ee56-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="1ee56-169">A lista de instrutores exibe dados relacionados da entidade OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="1ee56-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="1ee56-170">As entidades instrutor e OfficeAssignment estão em uma relação um-para-zero-ou-um.</span><span class="sxs-lookup"><span data-stu-id="1ee56-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="1ee56-171">Você usará o carregamento rápido para as entidades de OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="1ee56-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="1ee56-172">Conforme explicado anteriormente, o carregamento adiantado é geralmente mais eficiente quando você precisa dos dados relacionados para todas as linhas recuperadas da tabela primária.</span><span class="sxs-lookup"><span data-stu-id="1ee56-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="1ee56-173">Nesse caso, você deseja exibir atribuições office para todos os instrutores exibidos.</span><span class="sxs-lookup"><span data-stu-id="1ee56-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="1ee56-174">Quando o usuário seleciona um instrutor, entidades de curso relacionadas são exibidas.</span><span class="sxs-lookup"><span data-stu-id="1ee56-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="1ee56-175">As entidades de curso e instrutor estão em uma relação muitos-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="1ee56-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="1ee56-176">Você usará o carregamento rápido para as entidades do curso e suas entidades relacionadas do departamento.</span><span class="sxs-lookup"><span data-stu-id="1ee56-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="1ee56-177">Nesse caso, consultas separadas podem ser mais eficientes porque você precisa cursos somente para o instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="1ee56-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="1ee56-178">No entanto, este exemplo mostra como usar o carregamento rápido para propriedades de navegação dentro de entidades que são nas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="1ee56-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="1ee56-179">Quando o usuário seleciona um curso, dados relacionados de conjunto de registros de entidades são exibidos.</span><span class="sxs-lookup"><span data-stu-id="1ee56-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="1ee56-180">As entidades de curso e registro estão em uma relação um-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="1ee56-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="1ee56-181">Você usará consultas separadas para entidades de registro e suas entidades relacionadas do aluno.</span><span class="sxs-lookup"><span data-stu-id="1ee56-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="1ee56-182">Criar um modelo de exibição para o modo de exibição do índice do instrutor</span><span class="sxs-lookup"><span data-stu-id="1ee56-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="1ee56-183">A página de instrutores mostra dados de três tabelas diferentes.</span><span class="sxs-lookup"><span data-stu-id="1ee56-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="1ee56-184">Portanto, você criará um modelo de exibição que inclui três propriedades, cada uma contém os dados para uma das tabelas.</span><span class="sxs-lookup"><span data-stu-id="1ee56-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="1ee56-185">No *SchoolViewModels* pasta, criar *InstructorIndexData.cs* e substitua o código existente pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="1ee56-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="1ee56-186">Criar o controlador de instrutor e modos de exibição</span><span class="sxs-lookup"><span data-stu-id="1ee56-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="1ee56-187">Crie um controlador de instrutores com ações de leitura/gravação EF conforme mostrado na ilustração a seguir:</span><span class="sxs-lookup"><span data-stu-id="1ee56-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Adicionar controlador instrutores](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="1ee56-189">Abra *InstructorsController.cs* e adicione um usando a instrução para o namespace ViewModels:</span><span class="sxs-lookup"><span data-stu-id="1ee56-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="1ee56-190">Substitua o método de índice com o código a seguir para fazer o carregamento rápido de dados relacionados e colocá-lo no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="1ee56-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="1ee56-191">O método aceita dados de rota opcional (`id`) e um parâmetro de cadeia de caracteres de consulta (`courseID`) que fornecem os valores de ID do curso selecionado e instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="1ee56-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="1ee56-192">Os parâmetros fornecidos pelo **selecione** hiperlinks na página.</span><span class="sxs-lookup"><span data-stu-id="1ee56-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="1ee56-193">O código começa com a criação de uma instância do modelo de exibição e colocando-a lista de professores.</span><span class="sxs-lookup"><span data-stu-id="1ee56-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="1ee56-194">O código especifica o carregamento rápido para o `Instructor.OfficeAssignment` e `Instructor.CourseAssignments` propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="1ee56-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="1ee56-195">Dentro de `CourseAssignments` propriedade, o `Course` propriedade é carregado e dentro desse, o `Enrollments` e `Department` propriedades são carregados e dentro de cada `Enrollment` entidade o `Student` propriedade é carregada.</span><span class="sxs-lookup"><span data-stu-id="1ee56-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="1ee56-196">Como o modo de exibição sempre requer que a entidade OfficeAssignment, é mais eficiente buscar que na mesma consulta.</span><span class="sxs-lookup"><span data-stu-id="1ee56-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="1ee56-197">Entidades de curso são necessárias quando instrutor é selecionado na página da web, portanto, uma única consulta é melhor do que várias consultas apenas se a página é exibida com mais frequência um curso selecionado que sem.</span><span class="sxs-lookup"><span data-stu-id="1ee56-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="1ee56-198">O código repete `CourseAssignments` e `Course` porque você precisa de duas propriedades de `Course`.</span><span class="sxs-lookup"><span data-stu-id="1ee56-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="1ee56-199">A primeira cadeia de caracteres de `ThenInclude` chama obtém `CourseAssignment.Course`, `Course.Enrollments`, e `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="1ee56-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="1ee56-200">Nesse ponto no código, outra `ThenInclude` seria para propriedades de navegação de `Student`, que não é necessário.</span><span class="sxs-lookup"><span data-stu-id="1ee56-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="1ee56-201">Mas chamada `Include` será reiniciada com `Instructor` propriedades, você precisa passar por cadeia novamente, especificando essa hora `Course.Department` em vez de `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="1ee56-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="1ee56-202">O código a seguir executa quando instrutor foi selecionado.</span><span class="sxs-lookup"><span data-stu-id="1ee56-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="1ee56-203">O instrutor selecionado é recuperado da lista de instrutores no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="1ee56-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="1ee56-204">O modelo de exibição `Courses` propriedade, em seguida, é carregada com as entidades curso que instrutor `CourseAssignments` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="1ee56-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="1ee56-205">O `Where` método retorna uma coleção, mas nesse caso os critérios passado para esse método resultam em apenas uma única entidade instrutor que está sendo retornada.</span><span class="sxs-lookup"><span data-stu-id="1ee56-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="1ee56-206">O `Single` método converte a coleção em uma única entidade instrutor, que fornece acesso a essa entidade `CourseAssignments` propriedade.</span><span class="sxs-lookup"><span data-stu-id="1ee56-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="1ee56-207">O `CourseAssignments` propriedade contém `CourseAssignment` entidades, do qual você deseja apenas relacionado `Course` entidades.</span><span class="sxs-lookup"><span data-stu-id="1ee56-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="1ee56-208">Você usa o `Single` método em uma coleção quando você souber que a coleção terá apenas um item.</span><span class="sxs-lookup"><span data-stu-id="1ee56-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="1ee56-209">O único método gera uma exceção se a coleção passada para ele está vazia ou se houver mais de um item.</span><span class="sxs-lookup"><span data-stu-id="1ee56-209">The Single method throws an exception if the collection passed to it is empty or if there's more than one item.</span></span> <span data-ttu-id="1ee56-210">Uma alternativa é `SingleOrDefault`, que retorna um valor padrão (null nesse caso) se a coleção está vazia.</span><span class="sxs-lookup"><span data-stu-id="1ee56-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="1ee56-211">No entanto, nesse caso isso ainda resultar em uma exceção (de tentativa de encontrar um `Courses` propriedade em uma referência nula), e a mensagem de exceção menos claramente pode indicar a causa do problema.</span><span class="sxs-lookup"><span data-stu-id="1ee56-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="1ee56-212">Quando você chama o `Single` método, você também pode passar no local em que a condição em vez de chamar o `Where` método separadamente:</span><span class="sxs-lookup"><span data-stu-id="1ee56-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="1ee56-213">Em vez de:</span><span class="sxs-lookup"><span data-stu-id="1ee56-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="1ee56-214">Em seguida, se um curso foi selecionado, o curso selecionado é recuperado da lista de cursos no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="1ee56-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="1ee56-215">Em seguida, o modelo de exibição `Enrollments` propriedade é carregada com as entidades registro desse curso `Enrollments` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="1ee56-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="1ee56-216">Modificar a exibição do índice do instrutor</span><span class="sxs-lookup"><span data-stu-id="1ee56-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="1ee56-217">Em *Views/Instructors/Index.cshtml*, substitua o código de modelo com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="1ee56-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="1ee56-218">As alterações são realçadas.</span><span class="sxs-lookup"><span data-stu-id="1ee56-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="1ee56-219">As seguintes alterações feitas no código existente:</span><span class="sxs-lookup"><span data-stu-id="1ee56-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="1ee56-220">Alterar a classe de modelo para `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="1ee56-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="1ee56-221">Alterar o título da página de **índice** para **instrutores**.</span><span class="sxs-lookup"><span data-stu-id="1ee56-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="1ee56-222">Adicionado um **Office** coluna que exibe `item.OfficeAssignment.Location` somente se `item.OfficeAssignment` não for nulo.</span><span class="sxs-lookup"><span data-stu-id="1ee56-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="1ee56-223">(Como esta é uma relação um-para-zero-ou-um, pode não haver uma entidade relacionada de OfficeAssignment.)</span><span class="sxs-lookup"><span data-stu-id="1ee56-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="1ee56-224">Adicionado um **cursos** coluna que exibe os cursos ministrada por cada instrutor.</span><span class="sxs-lookup"><span data-stu-id="1ee56-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="1ee56-225">Consulte [explícita transição de linha com `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) para obter mais informações sobre essa sintaxe do razor.</span><span class="sxs-lookup"><span data-stu-id="1ee56-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="1ee56-226">Código adicionado dinamicamente adiciona `class="success"` para o `tr` elemento do instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="1ee56-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="1ee56-227">Isso define uma cor de plano de fundo para a linha selecionada usando uma classe de inicialização.</span><span class="sxs-lookup"><span data-stu-id="1ee56-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="1ee56-228">Adicionado um novo hiperlink rotulado **selecione** imediatamente antes de outros links em cada linha, que faz com que a ID do instrutor selecionado a ser enviado para o `Index` método.</span><span class="sxs-lookup"><span data-stu-id="1ee56-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="1ee56-229">Execute o aplicativo e selecione o **instrutores** guia. A página exibe a propriedade Location de entidades relacionadas de OfficeAssignment e uma célula de tabela vazia quando não houver nenhuma entidade OfficeAssignment relacionada.</span><span class="sxs-lookup"><span data-stu-id="1ee56-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Página de índice instrutores que nada selecionado](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="1ee56-231">No *Views/Instructors/Index.cshtml* arquivo, após o fechamento da tabela elemento (no final do arquivo), adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="1ee56-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="1ee56-232">Este código exibe uma lista de cursos relacionados ao instrutor ao instrutor está selecionado.</span><span class="sxs-lookup"><span data-stu-id="1ee56-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="1ee56-233">Esse código lê o `Courses` propriedade do modelo de exibição para exibir uma lista de cursos.</span><span class="sxs-lookup"><span data-stu-id="1ee56-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="1ee56-234">Ele também fornece um **selecione** hiperlink que envia a ID do curso selecionado para o `Index` método de ação.</span><span class="sxs-lookup"><span data-stu-id="1ee56-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="1ee56-235">Atualize a página e selecionar um instrutor.</span><span class="sxs-lookup"><span data-stu-id="1ee56-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="1ee56-236">Agora você verá uma grade que exibe os cursos atribuídos para o instrutor selecionado, e cada curso você ver o nome do departamento atribuído.</span><span class="sxs-lookup"><span data-stu-id="1ee56-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Instrutor de página de índice instrutores selecionado](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="1ee56-238">Após o bloco de código que você acabou de adicionar, adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="1ee56-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="1ee56-239">Isso exibe uma lista dos alunos que são registrados em um curso quando curso está selecionado.</span><span class="sxs-lookup"><span data-stu-id="1ee56-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="1ee56-240">Esse código lê a propriedade de registros do modelo de exibição para exibir uma lista dos alunos inscritos no curso.</span><span class="sxs-lookup"><span data-stu-id="1ee56-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="1ee56-241">Atualize a página novamente e selecione um instrutor.</span><span class="sxs-lookup"><span data-stu-id="1ee56-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="1ee56-242">Em seguida, selecione um curso para ver a lista de estudantes registrados e suas classificações.</span><span class="sxs-lookup"><span data-stu-id="1ee56-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Instrutor de página de índice professores e curso selecionado](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="1ee56-244">Carregamento explícito</span><span class="sxs-lookup"><span data-stu-id="1ee56-244">Explicit loading</span></span>

<span data-ttu-id="1ee56-245">Quando você recuperou a lista de instrutores em *InstructorsController.cs*, você especificou o carregamento rápido para o `CourseAssignments` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="1ee56-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="1ee56-246">Suponha que o esperado usuários raramente deseja ver registros em um curso e um instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="1ee56-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="1ee56-247">Nesse caso, você talvez queira carregar os dados de registro somente se eles forem solicitados.</span><span class="sxs-lookup"><span data-stu-id="1ee56-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="1ee56-248">Para ver um exemplo de como fazer carregamento explícito, substitua o `Index` método com o código a seguir, que remove o carregamento rápido para registros e os carrega essa propriedade explicitamente.</span><span class="sxs-lookup"><span data-stu-id="1ee56-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="1ee56-249">As alterações de código são realçadas.</span><span class="sxs-lookup"><span data-stu-id="1ee56-249">The code changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="1ee56-250">O novo código descarta o *ThenInclude* método chama para dados de registro do código que recupera as entidades do instrutor.</span><span class="sxs-lookup"><span data-stu-id="1ee56-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="1ee56-251">Se um curso e instrutor for selecionadas, o código realçado recupera entidades de registro para o curso selecionado e entidades de estudante para cada registro.</span><span class="sxs-lookup"><span data-stu-id="1ee56-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="1ee56-252">Execute que o aplicativo, vá para a página de índice instrutores agora e você não verá nenhuma diferença no que é exibido na página, embora você alterou a como os dados são recuperados.</span><span class="sxs-lookup"><span data-stu-id="1ee56-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="1ee56-253">Resumo</span><span class="sxs-lookup"><span data-stu-id="1ee56-253">Summary</span></span>

<span data-ttu-id="1ee56-254">Você agora usou carregamento adiantado com uma consulta e com várias consultas para ler os dados relacionados em Propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="1ee56-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="1ee56-255">O seguinte tutorial, você aprenderá como atualizar dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="1ee56-255">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="1ee56-256">[Anterior](complex-data-model.md)
>[Próximo](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="1ee56-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  
