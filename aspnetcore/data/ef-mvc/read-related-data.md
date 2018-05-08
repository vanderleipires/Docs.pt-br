---
title: ASP.NET Core MVC com o EF Core – ler dados relacionados – 6 de 10
author: tdykstra
description: Neste tutorial, você lerá e exibirá dados relacionados – ou seja, os dados que o Entity Framework carrega nas propriedades de navegação.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 6ee4b0db5bf4d1781ce44f1aff8331680ca8686c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a><span data-ttu-id="c1aef-103">ASP.NET Core MVC com o EF Core – ler dados relacionados – 6 de 10</span><span class="sxs-lookup"><span data-stu-id="c1aef-103">ASP.NET Core MVC with EF Core - Read Related Data - 6 of 10</span></span>

<span data-ttu-id="c1aef-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c1aef-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c1aef-105">O aplicativo web de exemplo Contoso University demonstra como criar aplicativos web do ASP.NET Core MVC usando o Entity Framework Core e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1aef-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="c1aef-106">Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="c1aef-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="c1aef-107">No tutorial anterior, você concluiu o modelo de dados Escola.</span><span class="sxs-lookup"><span data-stu-id="c1aef-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="c1aef-108">Neste tutorial, você lerá e exibirá dados relacionados – ou seja, os dados que o Entity Framework carrega nas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="c1aef-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="c1aef-109">As ilustrações a seguir mostram as páginas com as quais você trabalhará.</span><span class="sxs-lookup"><span data-stu-id="c1aef-109">The following illustrations show the pages that you'll work with.</span></span>

![Página Índice de Cursos](read-related-data/_static/courses-index.png)

![Página Índice de Instrutores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="c1aef-112">Carregamento adiantado, explícito e lento de dados relacionados</span><span class="sxs-lookup"><span data-stu-id="c1aef-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="c1aef-113">Há várias maneiras pelas quais um software ORM (Object-Relational Mapping), como o Entity Framework, pode carregar dados relacionados nas propriedades de navegação de uma entidade:</span><span class="sxs-lookup"><span data-stu-id="c1aef-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="c1aef-114">Carregamento adiantado.</span><span class="sxs-lookup"><span data-stu-id="c1aef-114">Eager loading.</span></span> <span data-ttu-id="c1aef-115">Quando a entidade é lida, os dados relacionados são recuperados com ela.</span><span class="sxs-lookup"><span data-stu-id="c1aef-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="c1aef-116">Normalmente, isso resulta em uma única consulta de junção que recupera todos os dados necessários.</span><span class="sxs-lookup"><span data-stu-id="c1aef-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="c1aef-117">Especifique o carregamento adiantado no Entity Framework Core usando os métodos `Include` e `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Exemplo de carregamento adiantado](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="c1aef-119">Recupere alguns dos dados em consultas separadas e o EF "corrigirá" as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="c1aef-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="c1aef-120">Ou seja, o EF adiciona de forma automática as entidades recuperadas separadamente no local em que pertencem nas propriedades de navegação de entidades recuperadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c1aef-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="c1aef-121">Para a consulta que recupera dados relacionados, você pode usar o método `Load` em vez de um método que retorna uma lista ou um objeto, como `ToList` ou `Single`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Exemplo de consultas separadas](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="c1aef-123">Carregamento explícito.</span><span class="sxs-lookup"><span data-stu-id="c1aef-123">Explicit loading.</span></span> <span data-ttu-id="c1aef-124">Quando a entidade é lida pela primeira vez, os dados relacionados não são recuperados.</span><span class="sxs-lookup"><span data-stu-id="c1aef-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="c1aef-125">Você escreve o código que recupera os dados relacionados se eles são necessários.</span><span class="sxs-lookup"><span data-stu-id="c1aef-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="c1aef-126">Como no caso do carregamento adiantado com consultas separadas, o carregamento explícito resulta no envio de várias consultas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c1aef-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="c1aef-127">A diferença é que, com o carregamento explícito, o código especifica as propriedades de navegação a serem carregadas.</span><span class="sxs-lookup"><span data-stu-id="c1aef-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="c1aef-128">No Entity Framework Core 1.1, você pode usar o método `Load` para fazer o carregamento explícito.</span><span class="sxs-lookup"><span data-stu-id="c1aef-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="c1aef-129">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c1aef-129">For example:</span></span>

  ![Exemplo de carregamento explícito](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="c1aef-131">Carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="c1aef-131">Lazy loading.</span></span> <span data-ttu-id="c1aef-132">Quando a entidade é lida pela primeira vez, os dados relacionados não são recuperados.</span><span class="sxs-lookup"><span data-stu-id="c1aef-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="c1aef-133">No entanto, na primeira vez que você tenta acessar uma propriedade de navegação, os dados necessários para essa propriedade de navegação são recuperados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c1aef-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="c1aef-134">Uma consulta é enviada ao banco de dados sempre que você tenta obter dados de uma propriedade de navegação pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="c1aef-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="c1aef-135">O Entity Framework Core 1.0 não dá suporte ao carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="c1aef-135">Entity Framework Core 1.0 doesn't support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="c1aef-136">Considerações sobre desempenho</span><span class="sxs-lookup"><span data-stu-id="c1aef-136">Performance considerations</span></span>

<span data-ttu-id="c1aef-137">Se você sabe que precisa de dados relacionados para cada entidade recuperada, o carregamento adiantado costuma oferecer o melhor desempenho, porque uma única consulta enviada para o banco de dados é geralmente mais eficiente do que consultas separadas para cada entidade recuperada.</span><span class="sxs-lookup"><span data-stu-id="c1aef-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="c1aef-138">Por exemplo, suponha que cada departamento tenha dez cursos relacionados.</span><span class="sxs-lookup"><span data-stu-id="c1aef-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="c1aef-139">O carregamento adiantado de todos os dados relacionados resultará em apenas uma única consulta (junção) e uma única viagem de ida e volta para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c1aef-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="c1aef-140">Uma consulta separada para cursos de cada departamento resultará em onze viagens de ida e volta para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c1aef-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="c1aef-141">As viagens de ida e volta extras para o banco de dados são especialmente prejudiciais ao desempenho quando a latência é alta.</span><span class="sxs-lookup"><span data-stu-id="c1aef-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="c1aef-142">Por outro lado, em alguns cenários, consultas separadas são mais eficientes.</span><span class="sxs-lookup"><span data-stu-id="c1aef-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="c1aef-143">O carregamento adiantado de todos os dados relacionados em uma consulta pode fazer com que uma junção muito complexa seja gerada, que o SQL Server não consegue processar com eficiência.</span><span class="sxs-lookup"><span data-stu-id="c1aef-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="c1aef-144">Ou se precisar acessar as propriedades de navegação de uma entidade somente para um subconjunto de um conjunto de entidades que está sendo processado, consultas separadas poderão ter um melhor desempenho, pois o carregamento adiantado de tudo desde o início recupera mais dados do que você precisa.</span><span class="sxs-lookup"><span data-stu-id="c1aef-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="c1aef-145">Se o desempenho for crítico, será melhor testar o desempenho das duas maneiras para fazer a melhor escolha.</span><span class="sxs-lookup"><span data-stu-id="c1aef-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="c1aef-146">Criar uma página Courses que exibe o nome do Departamento</span><span class="sxs-lookup"><span data-stu-id="c1aef-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="c1aef-147">A entidade Course inclui uma propriedade de navegação que contém a entidade Department do departamento ao qual o curso é atribuído.</span><span class="sxs-lookup"><span data-stu-id="c1aef-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="c1aef-148">Para exibir o nome do departamento atribuído em uma lista de cursos, você precisa obter a propriedade Name da entidade Department que está na propriedade de navegação `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that's in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="c1aef-149">Crie um controlador chamado CoursesController para o tipo de entidade Course, usando as mesmas opções para o scaffolder **Controlador MVC com exibições, usando o Entity Framework** que você usou anteriormente para o controlador Alunos, conforme mostrado na seguinte ilustração:</span><span class="sxs-lookup"><span data-stu-id="c1aef-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Adicionar o controlador Courses](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="c1aef-151">Abra *CoursesController.cs* e examine o método `Index`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="c1aef-152">O scaffolding automático especificou o carregamento adiantado para a propriedade de navegação `Department` usando o método `Include`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="c1aef-153">Substitua o método `Index` pelo seguinte código, que usa um nome mais apropriado para o `IQueryable` que retorna as entidades Course (`courses` em vez de `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="c1aef-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="c1aef-154">Abra *Views/Courses/Index.cshtml* e substitua o código de modelo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c1aef-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="c1aef-155">As alterações são realçadas:</span><span class="sxs-lookup"><span data-stu-id="c1aef-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="c1aef-156">Você fez as seguintes alterações no código gerado por scaffolding:</span><span class="sxs-lookup"><span data-stu-id="c1aef-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="c1aef-157">Alterou o cabeçalho de Índice para Cursos.</span><span class="sxs-lookup"><span data-stu-id="c1aef-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="c1aef-158">Adicionou uma coluna **Número** que mostra o valor da propriedade `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="c1aef-159">Por padrão, as chaves primárias não são geradas por scaffolding porque normalmente não têm sentido para os usuários finais.</span><span class="sxs-lookup"><span data-stu-id="c1aef-159">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="c1aef-160">No entanto, nesse caso, a chave primária é significativa e você deseja mostrá-la.</span><span class="sxs-lookup"><span data-stu-id="c1aef-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="c1aef-161">Alterou a coluna **Departamento** para que ela exiba o nome de departamento.</span><span class="sxs-lookup"><span data-stu-id="c1aef-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="c1aef-162">O código exibe a propriedade `Name` da entidade Department que é carregada na propriedade de navegação `Department`:</span><span class="sxs-lookup"><span data-stu-id="c1aef-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="c1aef-163">Execute o aplicativo e selecione a guia **Cursos** para ver a lista com nomes de departamentos.</span><span class="sxs-lookup"><span data-stu-id="c1aef-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Página Índice de Cursos](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="c1aef-165">Criar uma página Instrutores que mostra Cursos e Registros</span><span class="sxs-lookup"><span data-stu-id="c1aef-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="c1aef-166">Nesta seção, você criará um controlador e uma exibição para a entidade Instructor para exibir a página Instrutores:</span><span class="sxs-lookup"><span data-stu-id="c1aef-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Página Índice de Instrutores](read-related-data/_static/instructors-index.png)

<span data-ttu-id="c1aef-168">Essa página lê e exibe dados relacionados das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="c1aef-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="c1aef-169">A lista de instrutores exibe dados relacionados da entidade OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="c1aef-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="c1aef-170">As entidades Instructor e OfficeAssignment estão em uma relação um para zero ou um.</span><span class="sxs-lookup"><span data-stu-id="c1aef-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="c1aef-171">Você usará o carregamento adiantado para as entidades OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="c1aef-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="c1aef-172">Conforme explicado anteriormente, o carregamento adiantado é geralmente mais eficiente quando você precisa dos dados relacionados para todas as linhas recuperadas da tabela primária.</span><span class="sxs-lookup"><span data-stu-id="c1aef-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="c1aef-173">Nesse caso, você deseja exibir atribuições de escritório para todos os instrutores exibidos.</span><span class="sxs-lookup"><span data-stu-id="c1aef-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="c1aef-174">Quando o usuário seleciona um instrutor, as entidades Course relacionadas são exibidas.</span><span class="sxs-lookup"><span data-stu-id="c1aef-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="c1aef-175">As entidades Instructor e Course estão em uma relação muitos para muitos.</span><span class="sxs-lookup"><span data-stu-id="c1aef-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="c1aef-176">Você usará o carregamento adiantado para as entidades Course e suas entidades Department relacionadas.</span><span class="sxs-lookup"><span data-stu-id="c1aef-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="c1aef-177">Nesse caso, consultas separadas podem ser mais eficientes porque você precisa de cursos somente para o instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1aef-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="c1aef-178">No entanto, este exemplo mostra como usar o carregamento adiantado para propriedades de navegação em entidades que estão nas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="c1aef-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="c1aef-179">Quando o usuário seleciona um curso, dados relacionados do conjunto de entidades Enrollments são exibidos.</span><span class="sxs-lookup"><span data-stu-id="c1aef-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="c1aef-180">As entidades Course e Enrollment estão em uma relação um para muitos.</span><span class="sxs-lookup"><span data-stu-id="c1aef-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="c1aef-181">Você usará consultas separadas para entidades Enrollment e suas entidades Student relacionadas.</span><span class="sxs-lookup"><span data-stu-id="c1aef-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="c1aef-182">Criar um modelo de exibição para a exibição Índice de Instrutor</span><span class="sxs-lookup"><span data-stu-id="c1aef-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="c1aef-183">A página Instrutores mostra dados de três tabelas diferentes.</span><span class="sxs-lookup"><span data-stu-id="c1aef-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="c1aef-184">Portanto, você criará um modelo de exibição que inclui três propriedades, cada uma contendo os dados de uma das tabelas.</span><span class="sxs-lookup"><span data-stu-id="c1aef-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="c1aef-185">Na pasta *SchoolViewModels*, crie *InstructorIndexData.cs* e substitua o código existente pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="c1aef-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="c1aef-186">Criar exibições e o controlador Instrutor</span><span class="sxs-lookup"><span data-stu-id="c1aef-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="c1aef-187">Crie um controlador Instrutores com ações de leitura/gravação do EF, conforme mostrado na seguinte ilustração:</span><span class="sxs-lookup"><span data-stu-id="c1aef-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Adicionar o controlador Instrutores](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="c1aef-189">Abra *InstructorsController.cs* e adicione um usando a instrução para o namespace ViewModels:</span><span class="sxs-lookup"><span data-stu-id="c1aef-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="c1aef-190">Substitua o método Index pelo código a seguir para fazer o carregamento adiantado de dados relacionados e colocá-los no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c1aef-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="c1aef-191">O método aceita dados de rota opcionais (`id`) e um parâmetro de cadeia de caracteres de consulta (`courseID`) que fornece os valores de ID do curso e do instrutor selecionados.</span><span class="sxs-lookup"><span data-stu-id="c1aef-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="c1aef-192">Os parâmetros são fornecidos pelos hiperlinks **Selecionar** na página.</span><span class="sxs-lookup"><span data-stu-id="c1aef-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="c1aef-193">O código começa com a criação de uma instância do modelo de exibição e colocando-a na lista de instrutores.</span><span class="sxs-lookup"><span data-stu-id="c1aef-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="c1aef-194">O código especifica o carregamento adiantado para as propriedades de navegação `Instructor.OfficeAssignment` e `Instructor.CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="c1aef-195">Dentro da propriedade `CourseAssignments`, a propriedade `Course` é carregada e, dentro dela, as propriedades `Enrollments` e `Department` são carregadas e, dentro de cada entidade `Enrollment`, a propriedade `Student` é carregada.</span><span class="sxs-lookup"><span data-stu-id="c1aef-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="c1aef-196">Como a exibição sempre exige a entidade OfficeAssignment, é mais eficiente buscar isso na mesma consulta.</span><span class="sxs-lookup"><span data-stu-id="c1aef-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="c1aef-197">As entidades Course são necessárias quando um instrutor é selecionado na página da Web; portanto, uma única consulta é melhor do que várias consultas apenas se a página é exibida com mais frequência com um curso selecionado do que sem ele.</span><span class="sxs-lookup"><span data-stu-id="c1aef-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="c1aef-198">O código repete `CourseAssignments` e `Course` porque você precisa de duas propriedades de `Course`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="c1aef-199">A primeira cadeia de caracteres de chamadas `ThenInclude` obtém `CourseAssignment.Course`, `Course.Enrollments` e `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="c1aef-200">Nesse ponto do código, outro `ThenInclude` se refere às propriedades de navegação de `Student`, que não é necessário.</span><span class="sxs-lookup"><span data-stu-id="c1aef-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="c1aef-201">Mas a chamada a `Include` é reiniciada com propriedades `Instructor` e, portanto, você precisa passar pela cadeia novamente, dessa vez, especificando `Course.Department` em vez de `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="c1aef-202">O código a seguir é executado quando o instrutor é selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1aef-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="c1aef-203">O instrutor selecionado é recuperado da lista de instrutores no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c1aef-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="c1aef-204">Em seguida, a propriedade `Courses` do modelo de exibição é carregada com as entidades Course da propriedade de navegação `CourseAssignments` desse instrutor.</span><span class="sxs-lookup"><span data-stu-id="c1aef-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="c1aef-205">O método `Where` retorna uma coleção, mas nesse caso, os critérios passado para esse método resultam no retorno de apenas uma única entidade Instructor.</span><span class="sxs-lookup"><span data-stu-id="c1aef-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="c1aef-206">O método `Single` converte a coleção em uma única entidade Instructor, que fornece acesso à propriedade `CourseAssignments` dessa entidade.</span><span class="sxs-lookup"><span data-stu-id="c1aef-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="c1aef-207">A propriedade `CourseAssignments` contém entidades `CourseAssignment`, das quais você deseja apenas entidades `Course` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="c1aef-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="c1aef-208">Use o método `Single` em uma coleção quando souber que a coleção terá apenas um item.</span><span class="sxs-lookup"><span data-stu-id="c1aef-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="c1aef-209">O método Single gera uma exceção se a coleção passada para ele está vazia ou se há mais de um item.</span><span class="sxs-lookup"><span data-stu-id="c1aef-209">The Single method throws an exception if the collection passed to it's empty or if there's more than one item.</span></span> <span data-ttu-id="c1aef-210">Uma alternativa é `SingleOrDefault`, que retorna um valor padrão (nulo, nesse caso) se a coleção está vazia.</span><span class="sxs-lookup"><span data-stu-id="c1aef-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="c1aef-211">No entanto, nesse caso, isso ainda resultará em uma exceção (da tentativa de encontrar uma propriedade `Courses` em uma referência nula), e a mensagem de exceção menos claramente indicará a causa do problema.</span><span class="sxs-lookup"><span data-stu-id="c1aef-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="c1aef-212">Quando você chama o método `Single`, também pode passar a condição Where, em vez de chamar o método `Where` separadamente:</span><span class="sxs-lookup"><span data-stu-id="c1aef-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="c1aef-213">Em vez de:</span><span class="sxs-lookup"><span data-stu-id="c1aef-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="c1aef-214">Em seguida, se um curso foi selecionado, o curso selecionado é recuperado na lista de cursos no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c1aef-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="c1aef-215">Em seguida, a propriedade `Enrollments` do modelo de exibição é carregada com as entidades Enrollment da propriedade de navegação `Enrollments` desse curso.</span><span class="sxs-lookup"><span data-stu-id="c1aef-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="c1aef-216">Modificar a exibição Índice de Instrutor</span><span class="sxs-lookup"><span data-stu-id="c1aef-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="c1aef-217">Em *Views/Instructors/Index.cshtml*, substitua o código de modelo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c1aef-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="c1aef-218">As alterações são realçadas.</span><span class="sxs-lookup"><span data-stu-id="c1aef-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="c1aef-219">Você fez as seguintes alterações no código existente:</span><span class="sxs-lookup"><span data-stu-id="c1aef-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="c1aef-220">Alterou a classe de modelo para `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="c1aef-221">Alterou o título de página de **Índice** para **Instrutores**.</span><span class="sxs-lookup"><span data-stu-id="c1aef-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="c1aef-222">Adicionou uma coluna **Office** que exibe `item.OfficeAssignment.Location` somente se `item.OfficeAssignment` não é nulo.</span><span class="sxs-lookup"><span data-stu-id="c1aef-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="c1aef-223">(Como essa é uma relação um para zero ou um, pode não haver uma entidade OfficeAssignment relacionada.)</span><span class="sxs-lookup"><span data-stu-id="c1aef-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="c1aef-224">Adicionou uma coluna **Courses** que exibe os cursos ministrados por cada instrutor.</span><span class="sxs-lookup"><span data-stu-id="c1aef-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="c1aef-225">Consulte [Transição de linha explícita com `@:`](xref:mvc/views/razor#explicit-line-transition-with-) para obter mais informações sobre essa sintaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="c1aef-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="c1aef-226">Adicionou um código que adiciona `class="success"` dinamicamente ao elemento `tr` do instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1aef-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="c1aef-227">Isso define uma cor da tela de fundo para a linha selecionada usando uma classe Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="c1aef-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="c1aef-228">Adicionou um novo hiperlink rotulado **Selecionar** imediatamente antes dos outros links em cada linha, o que faz com que a ID do instrutor selecionado seja enviada para o método `Index`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="c1aef-229">Execute o aplicativo e selecione a guia **Instrutores**. A página exibe a propriedade Location das entidades OfficeAssignment relacionadas e uma célula de tabela vazia quando não há nenhuma entidade OfficeAssignment relacionada.</span><span class="sxs-lookup"><span data-stu-id="c1aef-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Página Índice de Instrutores – nenhuma opção selecionada](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="c1aef-231">No arquivo *Views/Instructors/Index.cshtml*, após o elemento de tabela de fechamento (ao final do arquivo), adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c1aef-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="c1aef-232">Esse código exibe uma lista de cursos relacionados a um instrutor quando um instrutor é selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1aef-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="c1aef-233">Esse código lê a propriedade `Courses` do modelo de exibição para exibir uma lista de cursos.</span><span class="sxs-lookup"><span data-stu-id="c1aef-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="c1aef-234">Também fornece um hiperlink **Selecionar** que envia a ID do curso selecionado para o método de ação `Index`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="c1aef-235">Atualize a página e selecione um instrutor.</span><span class="sxs-lookup"><span data-stu-id="c1aef-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="c1aef-236">Agora, você verá uma grade que exibe os cursos atribuídos ao instrutor selecionado, e para cada curso, verá o nome do departamento atribuído.</span><span class="sxs-lookup"><span data-stu-id="c1aef-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Página Índice de Instrutores – instrutor selecionado](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="c1aef-238">Após o bloco de código que você acabou de adicionar, adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c1aef-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="c1aef-239">Isso exibe uma lista dos alunos que estão registrados em um curso quando esse curso é selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1aef-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="c1aef-240">Esse código lê a propriedade Enrollments do modelo de exibição para exibir uma lista dos alunos registrados no curso.</span><span class="sxs-lookup"><span data-stu-id="c1aef-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="c1aef-241">Atualize a página novamente e selecione um instrutor.</span><span class="sxs-lookup"><span data-stu-id="c1aef-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="c1aef-242">Em seguida, selecione um curso para ver a lista de alunos registrados e suas notas.</span><span class="sxs-lookup"><span data-stu-id="c1aef-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Página Índice de Instrutores – instrutor e curso selecionados](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="c1aef-244">Carregamento explícito</span><span class="sxs-lookup"><span data-stu-id="c1aef-244">Explicit loading</span></span>

<span data-ttu-id="c1aef-245">Quando você recuperou a lista de instrutores em *InstructorsController.cs*, você especificou o carregamento adiantado para a propriedade de navegação `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="c1aef-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="c1aef-246">Suponha que os usuários esperados raramente desejem ver registros em um curso e um instrutor selecionados.</span><span class="sxs-lookup"><span data-stu-id="c1aef-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="c1aef-247">Nesse caso, talvez você deseje carregar os dados de registro somente se eles forem solicitados.</span><span class="sxs-lookup"><span data-stu-id="c1aef-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="c1aef-248">Para ver um exemplo de como fazer carregamento explícito, substitua o método `Index` pelo código a seguir, que remove o carregamento adiantado para Enrollments e carrega essa propriedade de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="c1aef-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="c1aef-249">As alterações de código são realçadas.</span><span class="sxs-lookup"><span data-stu-id="c1aef-249">The code changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="c1aef-250">O novo código remove as chamadas do método *ThenInclude* para dados de registro do código que recupera as entidades do instrutor.</span><span class="sxs-lookup"><span data-stu-id="c1aef-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="c1aef-251">Se um curso e um instrutor são selecionados, o código realçado recupera entidades Enrollment para o curso selecionado e as entidades Student para cada Enrollment.</span><span class="sxs-lookup"><span data-stu-id="c1aef-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="c1aef-252">Execute que o aplicativo, acesse a página Índice de Instrutores agora e você não verá nenhuma diferença no que é exibido na página, embora você tenha alterado a maneira como os dados são recuperados.</span><span class="sxs-lookup"><span data-stu-id="c1aef-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="c1aef-253">Resumo</span><span class="sxs-lookup"><span data-stu-id="c1aef-253">Summary</span></span>

<span data-ttu-id="c1aef-254">Agora, você usou o carregamento adiantado com uma consulta e com várias consultas para ler dados relacionados nas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="c1aef-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="c1aef-255">No próximo tutorial, você aprenderá a atualizar dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="c1aef-255">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="c1aef-256">[Anterior](complex-data-model.md)
>[Próximo](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="c1aef-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  
