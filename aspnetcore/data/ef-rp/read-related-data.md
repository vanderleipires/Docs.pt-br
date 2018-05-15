---
title: Páginas Razor com o EF Core no ASP.NET Core – Ler dados relacionados – 6 de 8
author: rick-anderson
description: Neste tutorial, você lê e exibe dados relacionados – ou seja, os dados que o Entity Framework carrega nas propriedades de navegação.
manager: wpickett
ms.author: riande
ms.date: 11/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 55d9b6743c7d97dc9a354bae218b1fac69d7b6bc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="bebd1-103">Páginas Razor com o EF Core no ASP.NET Core – Ler dados relacionados – 6 de 8</span><span class="sxs-lookup"><span data-stu-id="bebd1-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="bebd1-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bebd1-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="bebd1-105">Neste tutorial, os dados relacionados são lidos e exibidos.</span><span class="sxs-lookup"><span data-stu-id="bebd1-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="bebd1-106">Dados relacionados são dados que o EF Core carrega nas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="bebd1-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="bebd1-107">Caso tenha problemas que não consiga resolver, baixe o [aplicativo concluído para este estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="bebd1-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="bebd1-108">As seguintes ilustrações mostram as páginas concluídas para este tutorial:</span><span class="sxs-lookup"><span data-stu-id="bebd1-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Página Índice de Cursos](read-related-data/_static/courses-index.png)

![Página Índice de Instrutores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="bebd1-111">Carregamento adiantado, explícito e lento de dados relacionados</span><span class="sxs-lookup"><span data-stu-id="bebd1-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="bebd1-112">Há várias maneiras pelas quais o EF Core pode carregar dados relacionados nas propriedades de navegação de uma entidade:</span><span class="sxs-lookup"><span data-stu-id="bebd1-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="bebd1-113">[Carregamento adiantado](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="bebd1-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="bebd1-114">O carregamento adiantado é quando uma consulta para um tipo de entidade também carrega entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="bebd1-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="bebd1-115">Quando a entidade é lida, seus dados relacionados são recuperados.</span><span class="sxs-lookup"><span data-stu-id="bebd1-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="bebd1-116">Normalmente, isso resulta em uma única consulta de junção que recupera todos os dados necessários.</span><span class="sxs-lookup"><span data-stu-id="bebd1-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="bebd1-117">O EF Core emitirá várias consultas para alguns tipos de carregamento adiantado.</span><span class="sxs-lookup"><span data-stu-id="bebd1-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="bebd1-118">A emissão de várias consultas pode ser mais eficiente do que era o caso para algumas consultas no EF6 quando havia uma única consulta.</span><span class="sxs-lookup"><span data-stu-id="bebd1-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="bebd1-119">O carregamento adiantado é especificado com os métodos `Include` e `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Exemplo de carregamento adiantado](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="bebd1-121">O carregamento adiantado envia várias consultas quando a navegação de coleção é incluída:</span><span class="sxs-lookup"><span data-stu-id="bebd1-121">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="bebd1-122">Uma consulta para a consulta principal</span><span class="sxs-lookup"><span data-stu-id="bebd1-122">One query for the main query</span></span> 
  * <span data-ttu-id="bebd1-123">Uma consulta para cada "borda" de coleção na árvore de carregamento.</span><span class="sxs-lookup"><span data-stu-id="bebd1-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="bebd1-124">Separe consultas com `Load`: os dados podem ser recuperados em consultas separadas e o EF Core "corrige" as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="bebd1-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="bebd1-125">"Correção" significa que o EF Core popula automaticamente as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="bebd1-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="bebd1-126">A separação de consultas com `Load` é mais parecida com o carregamento explícito do que com o carregamento adiantado.</span><span class="sxs-lookup"><span data-stu-id="bebd1-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Exemplo de consultas separadas](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="bebd1-128">Observação: o EF Core corrige automaticamente as propriedades de navegação para outras entidades que foram carregadas anteriormente na instância do contexto.</span><span class="sxs-lookup"><span data-stu-id="bebd1-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="bebd1-129">Mesmo se os dados de uma propriedade de navegação *não* foram incluídos de forma explícita, a propriedade ainda pode ser populada se algumas ou todas as entidades relacionadas foram carregadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="bebd1-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="bebd1-130">[Carregamento explícito](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="bebd1-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="bebd1-131">Quando a entidade é lida pela primeira vez, os dados relacionados não são recuperados.</span><span class="sxs-lookup"><span data-stu-id="bebd1-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="bebd1-132">Um código precisa ser escrito para recuperar os dados relacionados quando eles forem necessários.</span><span class="sxs-lookup"><span data-stu-id="bebd1-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="bebd1-133">O carregamento explícito com consultas separadas resulta no envio de várias consultas ao BD.</span><span class="sxs-lookup"><span data-stu-id="bebd1-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="bebd1-134">Com o carregamento explícito, o código especifica as propriedades de navegação a serem carregadas.</span><span class="sxs-lookup"><span data-stu-id="bebd1-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="bebd1-135">Use o método `Load` para fazer o carregamento explícito.</span><span class="sxs-lookup"><span data-stu-id="bebd1-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="bebd1-136">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="bebd1-136">For example:</span></span>

  ![Exemplo de carregamento explícito](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="bebd1-138">[Carregamento lento](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="bebd1-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="bebd1-139">[No momento, o EF Core não dá suporte ao carregamento lento](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="bebd1-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="bebd1-140">Quando a entidade é lida pela primeira vez, os dados relacionados não são recuperados.</span><span class="sxs-lookup"><span data-stu-id="bebd1-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="bebd1-141">Na primeira vez que uma propriedade de navegação é acessada, os dados necessários para essa propriedade de navegação são recuperados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="bebd1-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="bebd1-142">Uma consulta é enviada para o BD sempre que uma propriedade de navegação é acessada pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="bebd1-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="bebd1-143">O operador `Select` carrega somente os dados relacionados necessários.</span><span class="sxs-lookup"><span data-stu-id="bebd1-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="bebd1-144">Criar uma página Courses que exibe o nome do departamento</span><span class="sxs-lookup"><span data-stu-id="bebd1-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="bebd1-145">A entidade Course inclui uma propriedade de navegação que contém a entidade `Department`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="bebd1-146">A entidade `Department` contém o departamento ao qual o curso é atribuído.</span><span class="sxs-lookup"><span data-stu-id="bebd1-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="bebd1-147">Para exibir o nome do departamento atribuído em uma lista de cursos:</span><span class="sxs-lookup"><span data-stu-id="bebd1-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="bebd1-148">Obtenha a propriedade `Name` da entidade `Department`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="bebd1-149">A entidade `Department` é obtida da propriedade de navegação `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="bebd1-151">Gerar o modelo Curso por scaffolding</span><span class="sxs-lookup"><span data-stu-id="bebd1-151">Scaffold the Course model</span></span>

* <span data-ttu-id="bebd1-152">Saia do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bebd1-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="bebd1-153">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="bebd1-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="bebd1-154">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="bebd1-154">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

<span data-ttu-id="bebd1-155">O comando anterior gera o modelo `Course` por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="bebd1-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="bebd1-156">Abra o projeto no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bebd1-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="bebd1-157">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="bebd1-157">Build the project.</span></span> <span data-ttu-id="bebd1-158">O build gera erros, como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="bebd1-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="bebd1-159">Altere `_context.Course` globalmente para `_context.Courses` (ou seja, adicione um "s" a `Course`).</span><span class="sxs-lookup"><span data-stu-id="bebd1-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="bebd1-160">7 ocorrências foram encontradas e atualizadas.</span><span class="sxs-lookup"><span data-stu-id="bebd1-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="bebd1-161">Abra *Pages/Courses/Index.cshtml.cs* e examine o método `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="bebd1-162">O mecanismo de scaffolding especificou o carregamento adiantado para a propriedade de navegação `Department`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="bebd1-163">O método `Include` especifica o carregamento adiantado.</span><span class="sxs-lookup"><span data-stu-id="bebd1-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="bebd1-164">Execute o aplicativo e selecione o link **Cursos**.</span><span class="sxs-lookup"><span data-stu-id="bebd1-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="bebd1-165">A coluna de departamento exibe a `DepartmentID`, que não é útil.</span><span class="sxs-lookup"><span data-stu-id="bebd1-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="bebd1-166">Atualize o método `OnGetAsync` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="bebd1-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="bebd1-167">O código anterior adiciona `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="bebd1-168">`AsNoTracking` melhora o desempenho porque as entidades retornadas não são controladas.</span><span class="sxs-lookup"><span data-stu-id="bebd1-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="bebd1-169">As entidades não são controladas porque elas não são atualizadas no contexto atual.</span><span class="sxs-lookup"><span data-stu-id="bebd1-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="bebd1-170">Atualize *Views/Courses/Index.cshtml* com a seguinte marcação realçada:</span><span class="sxs-lookup"><span data-stu-id="bebd1-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="bebd1-171">As seguintes alterações foram feitas na biblioteca gerada por código em scaffolding:</span><span class="sxs-lookup"><span data-stu-id="bebd1-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="bebd1-172">Alterou o cabeçalho de Índice para Cursos.</span><span class="sxs-lookup"><span data-stu-id="bebd1-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="bebd1-173">Adicionou uma coluna **Número** que mostra o valor da propriedade `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="bebd1-174">Por padrão, as chaves primárias não são geradas por scaffolding porque normalmente não têm sentido para os usuários finais.</span><span class="sxs-lookup"><span data-stu-id="bebd1-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="bebd1-175">No entanto, nesse caso, a chave primária é significativa.</span><span class="sxs-lookup"><span data-stu-id="bebd1-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="bebd1-176">Alterou a coluna **Departamento** para que ela exiba o nome de departamento.</span><span class="sxs-lookup"><span data-stu-id="bebd1-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="bebd1-177">O código exibe a propriedade `Name` da entidade `Department` que é carregada na propriedade de navegação `Department`:</span><span class="sxs-lookup"><span data-stu-id="bebd1-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="bebd1-178">Execute o aplicativo e selecione a guia **Cursos** para ver a lista com nomes de departamentos.</span><span class="sxs-lookup"><span data-stu-id="bebd1-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Página Índice de Cursos](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="bebd1-180">Carregando dados relacionados com Select</span><span class="sxs-lookup"><span data-stu-id="bebd1-180">Loading related data with Select</span></span>

<span data-ttu-id="bebd1-181">O método `OnGetAsync` carrega dados relacionados com o método `Include`:</span><span class="sxs-lookup"><span data-stu-id="bebd1-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="bebd1-182">O operador `Select` carrega somente os dados relacionados necessários.</span><span class="sxs-lookup"><span data-stu-id="bebd1-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="bebd1-183">Para itens únicos, como o `Department.Name`, ele usa um SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="bebd1-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="bebd1-184">Para coleções, ele usa outro acesso ao banco de dados, assim como o operador `Include` em coleções.</span><span class="sxs-lookup"><span data-stu-id="bebd1-184">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="bebd1-185">O seguinte código carrega dados relacionados com o método `Select`:</span><span class="sxs-lookup"><span data-stu-id="bebd1-185">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="bebd1-186">O `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="bebd1-186">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="bebd1-187">Consulte [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) para obter um exemplo completo.</span><span class="sxs-lookup"><span data-stu-id="bebd1-187">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="bebd1-188">Criar uma página Instrutores que mostra Cursos e Registros</span><span class="sxs-lookup"><span data-stu-id="bebd1-188">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="bebd1-189">Nesta seção, a página Instrutores é criada.</span><span class="sxs-lookup"><span data-stu-id="bebd1-189">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="bebd1-190">![Página Índice de Instrutores](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="bebd1-190">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="bebd1-191">Essa página lê e exibe dados relacionados das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="bebd1-191">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="bebd1-192">A lista de instrutores exibe dados relacionados da entidade `OfficeAssignment` (Office na imagem anterior).</span><span class="sxs-lookup"><span data-stu-id="bebd1-192">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="bebd1-193">As entidades `Instructor` e `OfficeAssignment` estão em uma relação um para zero ou um.</span><span class="sxs-lookup"><span data-stu-id="bebd1-193">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="bebd1-194">O carregamento adiantado é usado para as entidades `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-194">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="bebd1-195">O carregamento adiantado costuma ser mais eficiente quando os dados relacionados precisam ser exibidos.</span><span class="sxs-lookup"><span data-stu-id="bebd1-195">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="bebd1-196">Nesse caso, as atribuições de escritório para os instrutores são exibidas.</span><span class="sxs-lookup"><span data-stu-id="bebd1-196">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="bebd1-197">Quando o usuário seleciona um instrutor (Pedro na imagem anterior), as entidades `Course` relacionadas são exibidas.</span><span class="sxs-lookup"><span data-stu-id="bebd1-197">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="bebd1-198">As entidades `Instructor` e `Course` estão em uma relação muitos para muitos.</span><span class="sxs-lookup"><span data-stu-id="bebd1-198">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="bebd1-199">O carregamento adiantado é usado para entidades `Course` e suas entidades `Department` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="bebd1-199">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="bebd1-200">Nesse caso, consultas separadas podem ser mais eficientes porque somente os cursos para o instrutor selecionado são necessários.</span><span class="sxs-lookup"><span data-stu-id="bebd1-200">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="bebd1-201">Este exemplo mostra como usar o carregamento adiantado para propriedades de navegação em entidades que estão nas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="bebd1-201">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="bebd1-202">Quando o usuário seleciona um curso (Química na imagem anterior), os dados relacionados da entidade `Enrollments` são exibidos.</span><span class="sxs-lookup"><span data-stu-id="bebd1-202">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="bebd1-203">Na imagem anterior, o nome do aluno e a nota são exibidos.</span><span class="sxs-lookup"><span data-stu-id="bebd1-203">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="bebd1-204">As entidades `Course` e `Enrollment` estão em uma relação um-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="bebd1-204">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="bebd1-205">Criar um modelo de exibição para a exibição Índice de Instrutor</span><span class="sxs-lookup"><span data-stu-id="bebd1-205">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="bebd1-206">A página Instrutores mostra dados de três tabelas diferentes.</span><span class="sxs-lookup"><span data-stu-id="bebd1-206">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="bebd1-207">É criado um modelo de exibição que inclui as três entidades que representam as três tabelas.</span><span class="sxs-lookup"><span data-stu-id="bebd1-207">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="bebd1-208">Na pasta *SchoolViewModels*, crie *InstructorIndexData.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="bebd1-208">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="bebd1-209">Gerar o modelo Instrutor por scaffolding</span><span class="sxs-lookup"><span data-stu-id="bebd1-209">Scaffold the Instructor model</span></span>

* <span data-ttu-id="bebd1-210">Saia do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bebd1-210">Exit Visual Studio.</span></span>
* <span data-ttu-id="bebd1-211">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="bebd1-211">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="bebd1-212">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="bebd1-212">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

<span data-ttu-id="bebd1-213">O comando anterior gera o modelo `Instructor` por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="bebd1-213">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="bebd1-214">Abra o projeto no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bebd1-214">Open the project in Visual Studio.</span></span>

<span data-ttu-id="bebd1-215">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="bebd1-215">Build the project.</span></span> <span data-ttu-id="bebd1-216">O build gera erros.</span><span class="sxs-lookup"><span data-stu-id="bebd1-216">The build generates errors.</span></span>

<span data-ttu-id="bebd1-217">Altere `_context.Instructor` globalmente para `_context.Instructors` (ou seja, adicione um "s" a `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="bebd1-217">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="bebd1-218">7 ocorrências foram encontradas e atualizadas.</span><span class="sxs-lookup"><span data-stu-id="bebd1-218">7 occurrences are found and updated.</span></span>

<span data-ttu-id="bebd1-219">Execute o aplicativo e navegue para a página Instrutores.</span><span class="sxs-lookup"><span data-stu-id="bebd1-219">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="bebd1-220">Substitua *Pages/Instructors/Index.cshtml.cs* pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="bebd1-220">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-99)]

<span data-ttu-id="bebd1-221">O método `OnGetAsync` aceita dados de rota opcionais para a ID do instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="bebd1-221">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="bebd1-222">Examine a consulta na página *Pages/Instructors/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="bebd1-222">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="bebd1-223">A consulta tem duas inclusões:</span><span class="sxs-lookup"><span data-stu-id="bebd1-223">The query has two includes:</span></span>

* <span data-ttu-id="bebd1-224">`OfficeAssignment`: exibido na [exibição de instrutores](#IP).</span><span class="sxs-lookup"><span data-stu-id="bebd1-224">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="bebd1-225">`CourseAssignments`: que exibe os cursos ministrados.</span><span class="sxs-lookup"><span data-stu-id="bebd1-225">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="bebd1-226">Atualizar a página Índice de instrutores</span><span class="sxs-lookup"><span data-stu-id="bebd1-226">Update the instructors Index page</span></span>

<span data-ttu-id="bebd1-227">Atualize *Pages/Instructors/Index.cshtml* com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="bebd1-227">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="bebd1-228">A marcação anterior faz as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="bebd1-228">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="bebd1-229">Atualiza a diretiva `page` de `@page` para `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-229">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="bebd1-230">`"{id:int?}"` é um modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="bebd1-230">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="bebd1-231">O modelo de rota altera cadeias de consulta de inteiro na URL para dados de rota.</span><span class="sxs-lookup"><span data-stu-id="bebd1-231">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="bebd1-232">Por exemplo, clicar no link **Selecionar** de um o instrutor apenas com a diretiva `@page` produz uma URL semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="bebd1-232">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="bebd1-233">Quando a diretiva de página é `@page "{id:int?}"`, a URL anterior é:</span><span class="sxs-lookup"><span data-stu-id="bebd1-233">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="bebd1-234">O título de página é **Instrutores**.</span><span class="sxs-lookup"><span data-stu-id="bebd1-234">Page title is **Instructors**.</span></span>
* <span data-ttu-id="bebd1-235">Adicionou uma coluna **Office** que exibe `item.OfficeAssignment.Location` somente se `item.OfficeAssignment` não é nulo.</span><span class="sxs-lookup"><span data-stu-id="bebd1-235">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="bebd1-236">Como essa é uma relação um para zero ou um, pode não haver uma entidade OfficeAssignment relacionada.</span><span class="sxs-lookup"><span data-stu-id="bebd1-236">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="bebd1-237">Adicionou uma coluna **Courses** que exibe os cursos ministrados por cada instrutor.</span><span class="sxs-lookup"><span data-stu-id="bebd1-237">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="bebd1-238">Consulte [Transição de linha explícita com `@:`](xref:mvc/views/razor#explicit-line-transition-with-) para obter mais informações sobre essa sintaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="bebd1-238">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="bebd1-239">Adicionou um código que adiciona `class="success"` dinamicamente ao elemento `tr` do instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="bebd1-239">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="bebd1-240">Isso define uma cor da tela de fundo para a linha selecionada usando uma classe Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="bebd1-240">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="bebd1-241">Adicionou um novo hiperlink rotulado **Selecionar**.</span><span class="sxs-lookup"><span data-stu-id="bebd1-241">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="bebd1-242">Este link envia a ID do instrutor selecionado para o método `Index` e define uma cor da tela de fundo.</span><span class="sxs-lookup"><span data-stu-id="bebd1-242">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="bebd1-243">Execute o aplicativo e selecione a guia **Instrutores**. A página exibe o `Location` (escritório) da entidade `OfficeAssignment` relacionada.</span><span class="sxs-lookup"><span data-stu-id="bebd1-243">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="bebd1-244">Se OfficeAssignment é nulo, uma célula de tabela vazia é exibida.</span><span class="sxs-lookup"><span data-stu-id="bebd1-244">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Página Índice de Instrutores – nenhuma opção selecionada](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="bebd1-246">Clique no link **Selecionar**.</span><span class="sxs-lookup"><span data-stu-id="bebd1-246">Click on the **Select** link.</span></span> <span data-ttu-id="bebd1-247">O estilo de linha é alterado.</span><span class="sxs-lookup"><span data-stu-id="bebd1-247">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="bebd1-248">Adicionar cursos ministrados pelo instrutor selecionado</span><span class="sxs-lookup"><span data-stu-id="bebd1-248">Add courses taught by selected instructor</span></span>

<span data-ttu-id="bebd1-249">Atualize o método `OnGetAsync` em *Pages/Instructors/Index.cshtml.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="bebd1-249">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="bebd1-250">Examine a consulta atualizada:</span><span class="sxs-lookup"><span data-stu-id="bebd1-250">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="bebd1-251">A consulta anterior adiciona as entidades `Department`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-251">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="bebd1-252">O código a seguir é executado quando o instrutor é selecionado (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="bebd1-252">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="bebd1-253">O instrutor selecionado é recuperado da lista de instrutores no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="bebd1-253">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="bebd1-254">Em seguida, a propriedade `Courses` do modelo de exibição é carregada com as entidades `Course` da propriedade de navegação `CourseAssignments` desse instrutor.</span><span class="sxs-lookup"><span data-stu-id="bebd1-254">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="bebd1-255">O método `Where` retorna uma coleção.</span><span class="sxs-lookup"><span data-stu-id="bebd1-255">The `Where` method returns a collection.</span></span> <span data-ttu-id="bebd1-256">No método `Where` anterior, uma única entidade `Instructor` é retornada.</span><span class="sxs-lookup"><span data-stu-id="bebd1-256">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="bebd1-257">O método `Single` converte a coleção em uma única entidade `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-257">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="bebd1-258">A entidade `Instructor` fornece acesso à propriedade `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-258">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="bebd1-259">`CourseAssignments` fornece acesso às entidades `Course` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="bebd1-259">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Instrutor para Cursos m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="bebd1-261">O método `Single` é usado em uma coleção quando a coleção tem apenas um item.</span><span class="sxs-lookup"><span data-stu-id="bebd1-261">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="bebd1-262">O método `Single` gera uma exceção se a coleção está vazia ou se há mais de um item.</span><span class="sxs-lookup"><span data-stu-id="bebd1-262">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="bebd1-263">Uma alternativa é `SingleOrDefault`, que retorna um valor padrão (nulo, nesse caso) se a coleção está vazia.</span><span class="sxs-lookup"><span data-stu-id="bebd1-263">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="bebd1-264">O uso de `SingleOrDefault` é uma coleção vazia:</span><span class="sxs-lookup"><span data-stu-id="bebd1-264">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="bebd1-265">Resulta em uma exceção (da tentativa de encontrar uma propriedade `Courses` em uma referência nula).</span><span class="sxs-lookup"><span data-stu-id="bebd1-265">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="bebd1-266">A mensagem de exceção indica menos claramente a causa do problema.</span><span class="sxs-lookup"><span data-stu-id="bebd1-266">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="bebd1-267">O seguinte código popula a propriedade `Enrollments` do modelo de exibição quando um curso é selecionado:</span><span class="sxs-lookup"><span data-stu-id="bebd1-267">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="bebd1-268">Adicione a seguinte marcação ao final da Página do Razor *Pages/Courses/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="bebd1-268">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="bebd1-269">A marcação anterior exibe uma lista de cursos relacionados a um instrutor quando um instrutor é selecionado.</span><span class="sxs-lookup"><span data-stu-id="bebd1-269">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="bebd1-270">Teste o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bebd1-270">Test the app.</span></span> <span data-ttu-id="bebd1-271">Clique em um link **Selecionar** na página Instrutores.</span><span class="sxs-lookup"><span data-stu-id="bebd1-271">Click on a **Select** link on the instructors page.</span></span>

![Página Índice de Instrutores – instrutor selecionado](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="bebd1-273">Mostrar dados de alunos</span><span class="sxs-lookup"><span data-stu-id="bebd1-273">Show student data</span></span>

<span data-ttu-id="bebd1-274">Nesta seção, o aplicativo é atualizado para mostrar os dados de alunos de um curso selecionado.</span><span class="sxs-lookup"><span data-stu-id="bebd1-274">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="bebd1-275">Atualize a consulta no método `OnGetAsync` em *Pages/Instructors/Index.cshtml.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="bebd1-275">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="bebd1-276">Atualize *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bebd1-276">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="bebd1-277">Adicione a seguinte marcação ao final do arquivo:</span><span class="sxs-lookup"><span data-stu-id="bebd1-277">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="bebd1-278">A marcação anterior exibe uma lista dos alunos registrados no curso selecionado.</span><span class="sxs-lookup"><span data-stu-id="bebd1-278">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="bebd1-279">Atualize a página e selecione um instrutor.</span><span class="sxs-lookup"><span data-stu-id="bebd1-279">Refresh the page and select an instructor.</span></span> <span data-ttu-id="bebd1-280">Selecione um curso para ver a lista de alunos registrados e suas notas.</span><span class="sxs-lookup"><span data-stu-id="bebd1-280">Select a course to see the list of enrolled students and their grades.</span></span>

![Página Índice de Instrutores – instrutor e curso selecionados](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="bebd1-282">Usando Single</span><span class="sxs-lookup"><span data-stu-id="bebd1-282">Using Single</span></span>

<span data-ttu-id="bebd1-283">O método `Single` pode passar a condição `Where` em vez de chamar o método `Where` separadamente:</span><span class="sxs-lookup"><span data-stu-id="bebd1-283">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="bebd1-284">A abordagem `Single` anterior não oferece nenhum benefício em relação ao uso de `Where`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-284">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="bebd1-285">Alguns desenvolvedores preferem o estilo de abordagem `Single`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-285">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="bebd1-286">Carregamento explícito</span><span class="sxs-lookup"><span data-stu-id="bebd1-286">Explicit loading</span></span>

<span data-ttu-id="bebd1-287">O código atual especifica o carregamento adiantado para `Enrollments` e `Students`:</span><span class="sxs-lookup"><span data-stu-id="bebd1-287">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="bebd1-288">Suponha que os usuários raramente desejem ver registros em um curso.</span><span class="sxs-lookup"><span data-stu-id="bebd1-288">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="bebd1-289">Nesse caso, uma otimização será carregar apenas os dados de registro se eles forem solicitados.</span><span class="sxs-lookup"><span data-stu-id="bebd1-289">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="bebd1-290">Nesta seção, o `OnGetAsync` é atualizado para usar o carregamento explícito de `Enrollments` e `Students`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-290">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="bebd1-291">Atualize o `OnGetAsync` com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="bebd1-291">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="bebd1-292">O código anterior remove as chamadas do método *ThenInclude* para dados de registro e de alunos.</span><span class="sxs-lookup"><span data-stu-id="bebd1-292">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="bebd1-293">Se um curso é selecionado, o código realçado recupera:</span><span class="sxs-lookup"><span data-stu-id="bebd1-293">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="bebd1-294">As entidades `Enrollment` para o curso selecionado.</span><span class="sxs-lookup"><span data-stu-id="bebd1-294">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="bebd1-295">As entidades `Student` para cada `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-295">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="bebd1-296">Observe que o código anterior comenta `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="bebd1-296">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="bebd1-297">As propriedades de navegação apenas podem ser carregadas de forma explícita para entidades controladas.</span><span class="sxs-lookup"><span data-stu-id="bebd1-297">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="bebd1-298">Teste o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bebd1-298">Test the app.</span></span> <span data-ttu-id="bebd1-299">De uma perspectiva dos usuários, o aplicativo se comporta de forma idêntica à versão anterior.</span><span class="sxs-lookup"><span data-stu-id="bebd1-299">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="bebd1-300">O próximo tutorial mostra como atualizar os dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="bebd1-300">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="bebd1-301">[Anterior](xref:data/ef-rp/complex-data-model)
>[Próximo](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="bebd1-301">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
