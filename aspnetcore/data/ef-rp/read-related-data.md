---
title: Páginas Razor com o EF Core no ASP.NET Core – Ler dados relacionados – 6 de 8
author: rick-anderson
description: Neste tutorial, você lê e exibe dados relacionados – ou seja, os dados que o Entity Framework carrega nas propriedades de navegação.
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: bcea6aa6018a937979b8e0aaa2edcdd96da41559
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202673"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="83150-103">Páginas Razor com o EF Core no ASP.NET Core – Ler dados relacionados – 6 de 8</span><span class="sxs-lookup"><span data-stu-id="83150-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="83150-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="83150-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="83150-105">Neste tutorial, os dados relacionados são lidos e exibidos.</span><span class="sxs-lookup"><span data-stu-id="83150-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="83150-106">Dados relacionados são dados que o EF Core carrega nas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="83150-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="83150-107">Caso tenha problemas que não consiga resolver, baixe o [aplicativo concluído para este estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="83150-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="83150-108">As seguintes ilustrações mostram as páginas concluídas para este tutorial:</span><span class="sxs-lookup"><span data-stu-id="83150-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Página Índice de Cursos](read-related-data/_static/courses-index.png)

![Página Índice de Instrutores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="83150-111">Carregamento adiantado, explícito e lento de dados relacionados</span><span class="sxs-lookup"><span data-stu-id="83150-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="83150-112">Há várias maneiras pelas quais o EF Core pode carregar dados relacionados nas propriedades de navegação de uma entidade:</span><span class="sxs-lookup"><span data-stu-id="83150-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="83150-113">[Carregamento adiantado](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="83150-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="83150-114">O carregamento adiantado é quando uma consulta para um tipo de entidade também carrega entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="83150-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="83150-115">Quando a entidade é lida, seus dados relacionados são recuperados.</span><span class="sxs-lookup"><span data-stu-id="83150-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="83150-116">Normalmente, isso resulta em uma única consulta de junção que recupera todos os dados necessários.</span><span class="sxs-lookup"><span data-stu-id="83150-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="83150-117">O EF Core emitirá várias consultas para alguns tipos de carregamento adiantado.</span><span class="sxs-lookup"><span data-stu-id="83150-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="83150-118">A emissão de várias consultas pode ser mais eficiente do que era o caso para algumas consultas no EF6 quando havia uma única consulta.</span><span class="sxs-lookup"><span data-stu-id="83150-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="83150-119">O carregamento adiantado é especificado com os métodos `Include` e `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="83150-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Exemplo de carregamento adiantado](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="83150-121">O carregamento adiantado envia várias consultas quando a navegação de coleção é incluída:</span><span class="sxs-lookup"><span data-stu-id="83150-121">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="83150-122">Uma consulta para a consulta principal</span><span class="sxs-lookup"><span data-stu-id="83150-122">One query for the main query</span></span> 
  * <span data-ttu-id="83150-123">Uma consulta para cada "borda" de coleção na árvore de carregamento.</span><span class="sxs-lookup"><span data-stu-id="83150-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="83150-124">Separe consultas com `Load`: os dados podem ser recuperados em consultas separadas e o EF Core "corrige" as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="83150-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="83150-125">"Correção" significa que o EF Core popula automaticamente as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="83150-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="83150-126">A separação de consultas com `Load` é mais parecida com o carregamento explícito do que com o carregamento adiantado.</span><span class="sxs-lookup"><span data-stu-id="83150-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Exemplo de consultas separadas](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="83150-128">Observação: o EF Core corrige automaticamente as propriedades de navegação para outras entidades que foram carregadas anteriormente na instância do contexto.</span><span class="sxs-lookup"><span data-stu-id="83150-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="83150-129">Mesmo se os dados de uma propriedade de navegação *não* foram incluídos de forma explícita, a propriedade ainda pode ser populada se algumas ou todas as entidades relacionadas foram carregadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="83150-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="83150-130">[Carregamento explícito](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="83150-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="83150-131">Quando a entidade é lida pela primeira vez, os dados relacionados não são recuperados.</span><span class="sxs-lookup"><span data-stu-id="83150-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="83150-132">Um código precisa ser escrito para recuperar os dados relacionados quando eles forem necessários.</span><span class="sxs-lookup"><span data-stu-id="83150-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="83150-133">O carregamento explícito com consultas separadas resulta no envio de várias consultas ao BD.</span><span class="sxs-lookup"><span data-stu-id="83150-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="83150-134">Com o carregamento explícito, o código especifica as propriedades de navegação a serem carregadas.</span><span class="sxs-lookup"><span data-stu-id="83150-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="83150-135">Use o método `Load` para fazer o carregamento explícito.</span><span class="sxs-lookup"><span data-stu-id="83150-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="83150-136">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="83150-136">For example:</span></span>

  ![Exemplo de carregamento explícito](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="83150-138">[Carregamento lento](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="83150-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="83150-139">[No momento, o EF Core não dá suporte ao carregamento lento](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="83150-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="83150-140">Quando a entidade é lida pela primeira vez, os dados relacionados não são recuperados.</span><span class="sxs-lookup"><span data-stu-id="83150-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="83150-141">Na primeira vez que uma propriedade de navegação é acessada, os dados necessários para essa propriedade de navegação são recuperados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="83150-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="83150-142">Uma consulta é enviada para o BD sempre que uma propriedade de navegação é acessada pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="83150-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="83150-143">O operador `Select` carrega somente os dados relacionados necessários.</span><span class="sxs-lookup"><span data-stu-id="83150-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="83150-144">Criar uma página Courses que exibe o nome do departamento</span><span class="sxs-lookup"><span data-stu-id="83150-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="83150-145">A entidade Course inclui uma propriedade de navegação que contém a entidade `Department`.</span><span class="sxs-lookup"><span data-stu-id="83150-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="83150-146">A entidade `Department` contém o departamento ao qual o curso é atribuído.</span><span class="sxs-lookup"><span data-stu-id="83150-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="83150-147">Para exibir o nome do departamento atribuído em uma lista de cursos:</span><span class="sxs-lookup"><span data-stu-id="83150-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="83150-148">Obtenha a propriedade `Name` da entidade `Department`.</span><span class="sxs-lookup"><span data-stu-id="83150-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="83150-149">A entidade `Department` é obtida da propriedade de navegação `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="83150-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="83150-151">Gerar o modelo Curso por scaffolding</span><span class="sxs-lookup"><span data-stu-id="83150-151">Scaffold the Course model</span></span>

* <span data-ttu-id="83150-152">Saia do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83150-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="83150-153">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="83150-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="83150-154">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="83150-154">Run the following command:</span></span>

  ```console
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

<span data-ttu-id="83150-155">O comando anterior gera o modelo `Course` por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="83150-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="83150-156">Abra o projeto no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83150-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="83150-157">Abra *Pages/Courses/Index.cshtml.cs* e examine o método `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="83150-157">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="83150-158">O mecanismo de scaffolding especificou o carregamento adiantado para a propriedade de navegação `Department`.</span><span class="sxs-lookup"><span data-stu-id="83150-158">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="83150-159">O método `Include` especifica o carregamento adiantado.</span><span class="sxs-lookup"><span data-stu-id="83150-159">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="83150-160">Execute o aplicativo e selecione o link **Cursos**.</span><span class="sxs-lookup"><span data-stu-id="83150-160">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="83150-161">A coluna de departamento exibe a `DepartmentID`, que não é útil.</span><span class="sxs-lookup"><span data-stu-id="83150-161">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="83150-162">Atualize o método `OnGetAsync` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="83150-162">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="83150-163">O código anterior adiciona `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="83150-163">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="83150-164">`AsNoTracking` melhora o desempenho porque as entidades retornadas não são controladas.</span><span class="sxs-lookup"><span data-stu-id="83150-164">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="83150-165">As entidades não são controladas porque elas não são atualizadas no contexto atual.</span><span class="sxs-lookup"><span data-stu-id="83150-165">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="83150-166">Atualize *Pages/Courses/Index.cshtml* com a seguinte marcação realçada:</span><span class="sxs-lookup"><span data-stu-id="83150-166">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="83150-167">As seguintes alterações foram feitas na biblioteca gerada por código em scaffolding:</span><span class="sxs-lookup"><span data-stu-id="83150-167">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="83150-168">Alterou o cabeçalho de Índice para Cursos.</span><span class="sxs-lookup"><span data-stu-id="83150-168">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="83150-169">Adicionou uma coluna **Número** que mostra o valor da propriedade `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="83150-169">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="83150-170">Por padrão, as chaves primárias não são geradas por scaffolding porque normalmente não têm sentido para os usuários finais.</span><span class="sxs-lookup"><span data-stu-id="83150-170">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="83150-171">No entanto, nesse caso, a chave primária é significativa.</span><span class="sxs-lookup"><span data-stu-id="83150-171">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="83150-172">Alterou a coluna **Departamento** para que ela exiba o nome de departamento.</span><span class="sxs-lookup"><span data-stu-id="83150-172">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="83150-173">O código exibe a propriedade `Name` da entidade `Department` que é carregada na propriedade de navegação `Department`:</span><span class="sxs-lookup"><span data-stu-id="83150-173">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="83150-174">Execute o aplicativo e selecione a guia **Cursos** para ver a lista com nomes de departamentos.</span><span class="sxs-lookup"><span data-stu-id="83150-174">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Página Índice de Cursos](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="83150-176">Carregando dados relacionados com Select</span><span class="sxs-lookup"><span data-stu-id="83150-176">Loading related data with Select</span></span>

<span data-ttu-id="83150-177">O método `OnGetAsync` carrega dados relacionados com o método `Include`:</span><span class="sxs-lookup"><span data-stu-id="83150-177">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="83150-178">O operador `Select` carrega somente os dados relacionados necessários.</span><span class="sxs-lookup"><span data-stu-id="83150-178">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="83150-179">Para itens únicos, como o `Department.Name`, ele usa um SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="83150-179">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="83150-180">Para coleções, ele usa outro acesso ao banco de dados, assim como o operador `Include` em coleções.</span><span class="sxs-lookup"><span data-stu-id="83150-180">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="83150-181">O seguinte código carrega dados relacionados com o método `Select`:</span><span class="sxs-lookup"><span data-stu-id="83150-181">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="83150-182">O `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="83150-182">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="83150-183">Consulte [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) para obter um exemplo completo.</span><span class="sxs-lookup"><span data-stu-id="83150-183">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="83150-184">Criar uma página Instrutores que mostra Cursos e Registros</span><span class="sxs-lookup"><span data-stu-id="83150-184">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="83150-185">Nesta seção, a página Instrutores é criada.</span><span class="sxs-lookup"><span data-stu-id="83150-185">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="83150-186">![Página Índice de Instrutores](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="83150-186">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="83150-187">Essa página lê e exibe dados relacionados das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="83150-187">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="83150-188">A lista de instrutores exibe dados relacionados da entidade `OfficeAssignment` (Office na imagem anterior).</span><span class="sxs-lookup"><span data-stu-id="83150-188">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="83150-189">As entidades `Instructor` e `OfficeAssignment` estão em uma relação um para zero ou um.</span><span class="sxs-lookup"><span data-stu-id="83150-189">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="83150-190">O carregamento adiantado é usado para as entidades `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="83150-190">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="83150-191">O carregamento adiantado costuma ser mais eficiente quando os dados relacionados precisam ser exibidos.</span><span class="sxs-lookup"><span data-stu-id="83150-191">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="83150-192">Nesse caso, as atribuições de escritório para os instrutores são exibidas.</span><span class="sxs-lookup"><span data-stu-id="83150-192">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="83150-193">Quando o usuário seleciona um instrutor (Pedro na imagem anterior), as entidades `Course` relacionadas são exibidas.</span><span class="sxs-lookup"><span data-stu-id="83150-193">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="83150-194">As entidades `Instructor` e `Course` estão em uma relação muitos para muitos.</span><span class="sxs-lookup"><span data-stu-id="83150-194">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="83150-195">O carregamento adiantado é usado para entidades `Course` e suas entidades `Department` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="83150-195">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="83150-196">Nesse caso, consultas separadas podem ser mais eficientes porque somente os cursos para o instrutor selecionado são necessários.</span><span class="sxs-lookup"><span data-stu-id="83150-196">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="83150-197">Este exemplo mostra como usar o carregamento adiantado para propriedades de navegação em entidades que estão nas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="83150-197">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="83150-198">Quando o usuário seleciona um curso (Química na imagem anterior), os dados relacionados da entidade `Enrollments` são exibidos.</span><span class="sxs-lookup"><span data-stu-id="83150-198">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="83150-199">Na imagem anterior, o nome do aluno e a nota são exibidos.</span><span class="sxs-lookup"><span data-stu-id="83150-199">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="83150-200">As entidades `Course` e `Enrollment` estão em uma relação um-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="83150-200">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="83150-201">Criar um modelo de exibição para a exibição Índice de Instrutor</span><span class="sxs-lookup"><span data-stu-id="83150-201">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="83150-202">A página Instrutores mostra dados de três tabelas diferentes.</span><span class="sxs-lookup"><span data-stu-id="83150-202">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="83150-203">É criado um modelo de exibição que inclui as três entidades que representam as três tabelas.</span><span class="sxs-lookup"><span data-stu-id="83150-203">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="83150-204">Na pasta *SchoolViewModels*, crie *InstructorIndexData.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="83150-204">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="83150-205">Gerar o modelo Instrutor por scaffolding</span><span class="sxs-lookup"><span data-stu-id="83150-205">Scaffold the Instructor model</span></span>

* <span data-ttu-id="83150-206">Saia do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83150-206">Exit Visual Studio.</span></span>
* <span data-ttu-id="83150-207">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="83150-207">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="83150-208">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="83150-208">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

<span data-ttu-id="83150-209">O comando anterior gera o modelo `Instructor` por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="83150-209">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="83150-210">Abra o projeto no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83150-210">Open the project in Visual Studio.</span></span>

<span data-ttu-id="83150-211">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="83150-211">Build the project.</span></span> <span data-ttu-id="83150-212">O build gera erros.</span><span class="sxs-lookup"><span data-stu-id="83150-212">The build generates errors.</span></span>

<span data-ttu-id="83150-213">Altere `_context.Instructor` globalmente para `_context.Instructors` (ou seja, adicione um "s" a `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="83150-213">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="83150-214">7 ocorrências foram encontradas e atualizadas.</span><span class="sxs-lookup"><span data-stu-id="83150-214">7 occurrences are found and updated.</span></span>

<span data-ttu-id="83150-215">Execute o aplicativo e navegue para a página Instrutores.</span><span class="sxs-lookup"><span data-stu-id="83150-215">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="83150-216">Substitua *Pages/Instructors/Index.cshtml.cs* pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="83150-216">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="83150-217">O método `OnGetAsync` aceita dados de rota opcionais para a ID do instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="83150-217">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="83150-218">Examine a consulta no arquivo *Pages/Instructors/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="83150-218">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="83150-219">A consulta tem duas inclusões:</span><span class="sxs-lookup"><span data-stu-id="83150-219">The query has two includes:</span></span>

* <span data-ttu-id="83150-220">`OfficeAssignment`: exibido na [exibição de instrutores](#IP).</span><span class="sxs-lookup"><span data-stu-id="83150-220">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="83150-221">`CourseAssignments`: que exibe os cursos ministrados.</span><span class="sxs-lookup"><span data-stu-id="83150-221">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="83150-222">Atualizar a página Índice de instrutores</span><span class="sxs-lookup"><span data-stu-id="83150-222">Update the instructors Index page</span></span>

<span data-ttu-id="83150-223">Atualize *Pages/Instructors/Index.cshtml* com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="83150-223">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="83150-224">A marcação anterior faz as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="83150-224">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="83150-225">Atualiza a diretiva `page` de `@page` para `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="83150-225">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="83150-226">`"{id:int?}"` é um modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="83150-226">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="83150-227">O modelo de rota altera cadeias de consulta de inteiro na URL para dados de rota.</span><span class="sxs-lookup"><span data-stu-id="83150-227">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="83150-228">Por exemplo, clicar no link **Selecionar** de um o instrutor apenas com a diretiva `@page` produz uma URL semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="83150-228">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="83150-229">Quando a diretiva de página é `@page "{id:int?}"`, a URL anterior é:</span><span class="sxs-lookup"><span data-stu-id="83150-229">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="83150-230">O título de página é **Instrutores**.</span><span class="sxs-lookup"><span data-stu-id="83150-230">Page title is **Instructors**.</span></span>
* <span data-ttu-id="83150-231">Adicionou uma coluna **Office** que exibe `item.OfficeAssignment.Location` somente se `item.OfficeAssignment` não é nulo.</span><span class="sxs-lookup"><span data-stu-id="83150-231">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="83150-232">Como essa é uma relação um para zero ou um, pode não haver uma entidade OfficeAssignment relacionada.</span><span class="sxs-lookup"><span data-stu-id="83150-232">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="83150-233">Adicionou uma coluna **Courses** que exibe os cursos ministrados por cada instrutor.</span><span class="sxs-lookup"><span data-stu-id="83150-233">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="83150-234">Consulte [Transição de linha explícita com `@:`](xref:mvc/views/razor#explicit-line-transition-with-) para obter mais informações sobre essa sintaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="83150-234">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="83150-235">Adicionou um código que adiciona `class="success"` dinamicamente ao elemento `tr` do instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="83150-235">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="83150-236">Isso define uma cor da tela de fundo para a linha selecionada usando uma classe Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="83150-236">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="83150-237">Adicionou um novo hiperlink rotulado **Selecionar**.</span><span class="sxs-lookup"><span data-stu-id="83150-237">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="83150-238">Este link envia a ID do instrutor selecionado para o método `Index` e define uma cor da tela de fundo.</span><span class="sxs-lookup"><span data-stu-id="83150-238">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="83150-239">Execute o aplicativo e selecione a guia **Instrutores**. A página exibe o `Location` (escritório) da entidade `OfficeAssignment` relacionada.</span><span class="sxs-lookup"><span data-stu-id="83150-239">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="83150-240">Se OfficeAssignment é nulo, uma célula de tabela vazia é exibida.</span><span class="sxs-lookup"><span data-stu-id="83150-240">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Página Índice de Instrutores – nenhuma opção selecionada](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="83150-242">Clique no link **Selecionar**.</span><span class="sxs-lookup"><span data-stu-id="83150-242">Click on the **Select** link.</span></span> <span data-ttu-id="83150-243">O estilo de linha é alterado.</span><span class="sxs-lookup"><span data-stu-id="83150-243">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="83150-244">Adicionar cursos ministrados pelo instrutor selecionado</span><span class="sxs-lookup"><span data-stu-id="83150-244">Add courses taught by selected instructor</span></span>

<span data-ttu-id="83150-245">Atualize o método `OnGetAsync` em *Pages/Instructors/Index.cshtml.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="83150-245">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="83150-246">Adicione `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="83150-246">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="83150-247">Examine a consulta atualizada:</span><span class="sxs-lookup"><span data-stu-id="83150-247">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="83150-248">A consulta anterior adiciona as entidades `Department`.</span><span class="sxs-lookup"><span data-stu-id="83150-248">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="83150-249">O código a seguir é executado quando o instrutor é selecionado (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="83150-249">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="83150-250">O instrutor selecionado é recuperado da lista de instrutores no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="83150-250">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="83150-251">Em seguida, a propriedade `Courses` do modelo de exibição é carregada com as entidades `Course` da propriedade de navegação `CourseAssignments` desse instrutor.</span><span class="sxs-lookup"><span data-stu-id="83150-251">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="83150-252">O método `Where` retorna uma coleção.</span><span class="sxs-lookup"><span data-stu-id="83150-252">The `Where` method returns a collection.</span></span> <span data-ttu-id="83150-253">No método `Where` anterior, uma única entidade `Instructor` é retornada.</span><span class="sxs-lookup"><span data-stu-id="83150-253">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="83150-254">O método `Single` converte a coleção em uma única entidade `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="83150-254">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="83150-255">A entidade `Instructor` fornece acesso à propriedade `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="83150-255">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="83150-256">`CourseAssignments` fornece acesso às entidades `Course` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="83150-256">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Instrutor para Cursos m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="83150-258">O método `Single` é usado em uma coleção quando a coleção tem apenas um item.</span><span class="sxs-lookup"><span data-stu-id="83150-258">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="83150-259">O método `Single` gera uma exceção se a coleção está vazia ou se há mais de um item.</span><span class="sxs-lookup"><span data-stu-id="83150-259">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="83150-260">Uma alternativa é `SingleOrDefault`, que retorna um valor padrão (nulo, nesse caso) se a coleção está vazia.</span><span class="sxs-lookup"><span data-stu-id="83150-260">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="83150-261">O uso de `SingleOrDefault` é uma coleção vazia:</span><span class="sxs-lookup"><span data-stu-id="83150-261">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="83150-262">Resulta em uma exceção (da tentativa de encontrar uma propriedade `Courses` em uma referência nula).</span><span class="sxs-lookup"><span data-stu-id="83150-262">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="83150-263">A mensagem de exceção indica menos claramente a causa do problema.</span><span class="sxs-lookup"><span data-stu-id="83150-263">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="83150-264">O seguinte código popula a propriedade `Enrollments` do modelo de exibição quando um curso é selecionado:</span><span class="sxs-lookup"><span data-stu-id="83150-264">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="83150-265">Adicione a seguinte marcação ao final do Razor Page *Pages/Courses/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="83150-265">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="83150-266">A marcação anterior exibe uma lista de cursos relacionados a um instrutor quando um instrutor é selecionado.</span><span class="sxs-lookup"><span data-stu-id="83150-266">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="83150-267">Teste o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="83150-267">Test the app.</span></span> <span data-ttu-id="83150-268">Clique em um link **Selecionar** na página Instrutores.</span><span class="sxs-lookup"><span data-stu-id="83150-268">Click on a **Select** link on the instructors page.</span></span>

![Página Índice de Instrutores – instrutor selecionado](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="83150-270">Mostrar dados de alunos</span><span class="sxs-lookup"><span data-stu-id="83150-270">Show student data</span></span>

<span data-ttu-id="83150-271">Nesta seção, o aplicativo é atualizado para mostrar os dados de alunos de um curso selecionado.</span><span class="sxs-lookup"><span data-stu-id="83150-271">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="83150-272">Atualize a consulta no método `OnGetAsync` em *Pages/Instructors/Index.cshtml.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="83150-272">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="83150-273">Atualize *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="83150-273">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="83150-274">Adicione a seguinte marcação ao final do arquivo:</span><span class="sxs-lookup"><span data-stu-id="83150-274">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="83150-275">A marcação anterior exibe uma lista dos alunos registrados no curso selecionado.</span><span class="sxs-lookup"><span data-stu-id="83150-275">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="83150-276">Atualize a página e selecione um instrutor.</span><span class="sxs-lookup"><span data-stu-id="83150-276">Refresh the page and select an instructor.</span></span> <span data-ttu-id="83150-277">Selecione um curso para ver a lista de alunos registrados e suas notas.</span><span class="sxs-lookup"><span data-stu-id="83150-277">Select a course to see the list of enrolled students and their grades.</span></span>

![Página Índice de Instrutores – instrutor e curso selecionados](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="83150-279">Usando Single</span><span class="sxs-lookup"><span data-stu-id="83150-279">Using Single</span></span>

<span data-ttu-id="83150-280">O método `Single` pode passar a condição `Where` em vez de chamar o método `Where` separadamente:</span><span class="sxs-lookup"><span data-stu-id="83150-280">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="83150-281">A abordagem `Single` anterior não oferece nenhum benefício em relação ao uso de `Where`.</span><span class="sxs-lookup"><span data-stu-id="83150-281">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="83150-282">Alguns desenvolvedores preferem o estilo de abordagem `Single`.</span><span class="sxs-lookup"><span data-stu-id="83150-282">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="83150-283">Carregamento explícito</span><span class="sxs-lookup"><span data-stu-id="83150-283">Explicit loading</span></span>

<span data-ttu-id="83150-284">O código atual especifica o carregamento adiantado para `Enrollments` e `Students`:</span><span class="sxs-lookup"><span data-stu-id="83150-284">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="83150-285">Suponha que os usuários raramente desejem ver registros em um curso.</span><span class="sxs-lookup"><span data-stu-id="83150-285">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="83150-286">Nesse caso, uma otimização será carregar apenas os dados de registro se eles forem solicitados.</span><span class="sxs-lookup"><span data-stu-id="83150-286">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="83150-287">Nesta seção, o `OnGetAsync` é atualizado para usar o carregamento explícito de `Enrollments` e `Students`.</span><span class="sxs-lookup"><span data-stu-id="83150-287">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="83150-288">Atualize o `OnGetAsync` com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="83150-288">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="83150-289">O código anterior remove as chamadas do método *ThenInclude* para dados de registro e de alunos.</span><span class="sxs-lookup"><span data-stu-id="83150-289">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="83150-290">Se um curso é selecionado, o código realçado recupera:</span><span class="sxs-lookup"><span data-stu-id="83150-290">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="83150-291">As entidades `Enrollment` para o curso selecionado.</span><span class="sxs-lookup"><span data-stu-id="83150-291">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="83150-292">As entidades `Student` para cada `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="83150-292">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="83150-293">Observe que o código anterior comenta `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="83150-293">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="83150-294">As propriedades de navegação apenas podem ser carregadas de forma explícita para entidades controladas.</span><span class="sxs-lookup"><span data-stu-id="83150-294">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="83150-295">Teste o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="83150-295">Test the app.</span></span> <span data-ttu-id="83150-296">De uma perspectiva dos usuários, o aplicativo se comporta de forma idêntica à versão anterior.</span><span class="sxs-lookup"><span data-stu-id="83150-296">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="83150-297">O próximo tutorial mostra como atualizar os dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="83150-297">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="83150-298">[Anterior](xref:data/ef-rp/complex-data-model)
>[Próximo](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="83150-298">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
