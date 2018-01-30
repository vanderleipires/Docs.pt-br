---
title: "Páginas Razor com núcleo EF - ler dados relacionados - 6 de 8"
author: rick-anderson
description: "Neste tutorial você ler e exibe dados relacionados – ou seja, os dados que o Entity Framework carrega em Propriedades de navegação."
manager: wpickett
ms.author: riande
ms.date: 11/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ccb1e95ae2b43fd0a4c4b1ac9ed58a4d474ab3b6
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="c1f50-103">Leitura relacionadas a dados - Core de EF com páginas Razor (6 de 8)</span><span class="sxs-lookup"><span data-stu-id="c1f50-103">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="c1f50-104">Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c1f50-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="c1f50-105">Neste tutorial, os dados relacionados é lido e exibidos.</span><span class="sxs-lookup"><span data-stu-id="c1f50-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="c1f50-106">Dados relacionados são dados EF Core carrega em Propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="c1f50-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="c1f50-107">Se você tiver problemas, você não conseguir resolver, baixe o [aplicativo concluído para este estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="c1f50-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="c1f50-108">As ilustrações a seguir mostram as páginas concluídas para este tutorial:</span><span class="sxs-lookup"><span data-stu-id="c1f50-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

![Página de índice instrutores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="c1f50-111">Carregamento de dados relacionados lento eager e explícita</span><span class="sxs-lookup"><span data-stu-id="c1f50-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="c1f50-112">Há várias maneiras que o EF Core pode carregar dados relacionados para as propriedades de navegação de uma entidade:</span><span class="sxs-lookup"><span data-stu-id="c1f50-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="c1f50-113">[Carregamento adiantado](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="c1f50-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="c1f50-114">Carregamento adiantado é quando uma consulta para um tipo de entidade também carrega entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="c1f50-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="c1f50-115">Quando a entidade é lida, seus dados relacionados são recuperados.</span><span class="sxs-lookup"><span data-stu-id="c1f50-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="c1f50-116">Normalmente, isso resulta em uma consulta de junção único que recupera todos os dados que é necessário.</span><span class="sxs-lookup"><span data-stu-id="c1f50-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="c1f50-117">Núcleo EF emitirá várias consultas para alguns tipos de carregamento rápido.</span><span class="sxs-lookup"><span data-stu-id="c1f50-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="c1f50-118">Emitir várias consultas pode ser mais eficiente do que era o caso para algumas consultas em EF6 qual havia uma única consulta.</span><span class="sxs-lookup"><span data-stu-id="c1f50-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="c1f50-119">Carregamento adiantado é especificado com o `Include` e `ThenInclude` métodos.</span><span class="sxs-lookup"><span data-stu-id="c1f50-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![Exemplo de carregamento rápido](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="c1f50-121">Carregamento adiantado envia várias consultas quando uma coleção nvavigation é incluído:</span><span class="sxs-lookup"><span data-stu-id="c1f50-121">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="c1f50-122">Uma consulta para a consulta principal</span><span class="sxs-lookup"><span data-stu-id="c1f50-122">One query for the main query</span></span> 
 * <span data-ttu-id="c1f50-123">Uma consulta para cada coleção "borda", na árvore de carga.</span><span class="sxs-lookup"><span data-stu-id="c1f50-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="c1f50-124">Separar consultas com `Load`: os dados podem ser recuperados em consultas separadas e Core EF "correções de" as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="c1f50-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="c1f50-125">significa "correções para cima" que o EF Core preenche automaticamente as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="c1f50-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="c1f50-126">Separar consultas com `Load` é mais parecida com explícito de carregamento de carregamento rápido.</span><span class="sxs-lookup"><span data-stu-id="c1f50-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Exemplo de consultas separadas](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="c1f50-128">Observação: EF Core corrige automaticamente as propriedades de navegação para outras entidades que foram previamente carregadas para a instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="c1f50-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="c1f50-129">Mesmo se os dados de uma propriedade de navegação são *não* explicitamente incluído, a propriedade ainda pode ser populada se algumas ou todas as entidades relacionadas foram carregadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c1f50-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="c1f50-130">[Carregamento explícito](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="c1f50-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="c1f50-131">Quando a entidade é lido pela primeira vez, os dados relacionados não são recuperados.</span><span class="sxs-lookup"><span data-stu-id="c1f50-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="c1f50-132">Código deve ser escrito para recuperar os dados relacionados quando ele é necessário.</span><span class="sxs-lookup"><span data-stu-id="c1f50-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="c1f50-133">Carregamento explícito com consultas separadas resulta em várias consultas enviadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c1f50-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="c1f50-134">Carregamento explícito, o código especifica as propriedades de navegação para ser carregado.</span><span class="sxs-lookup"><span data-stu-id="c1f50-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="c1f50-135">Use o `Load` método de fazer o carregamento explícito.</span><span class="sxs-lookup"><span data-stu-id="c1f50-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="c1f50-136">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c1f50-136">For example:</span></span>

 ![Exemplo de carregamento explícito](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="c1f50-138">[Carregamento preguiçoso](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="c1f50-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="c1f50-139">[EF Core atualmente não dá suporte a carregamento lento](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="c1f50-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="c1f50-140">Quando a entidade é lido pela primeira vez, os dados relacionados não são recuperados.</span><span class="sxs-lookup"><span data-stu-id="c1f50-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="c1f50-141">Na primeira vez que uma propriedade de navegação é acessada, os dados necessários para essa propriedade de navegação são recuperados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c1f50-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="c1f50-142">Uma consulta é enviada para o banco de dados sempre que uma propriedade de navegação seja acessada pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="c1f50-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="c1f50-143">O `Select` operador carrega somente os dados relacionados necessários.</span><span class="sxs-lookup"><span data-stu-id="c1f50-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="c1f50-144">Criar uma página de cursos que exibe o nome do departamento</span><span class="sxs-lookup"><span data-stu-id="c1f50-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="c1f50-145">A entidade de curso inclui uma propriedade de navegação que contém o `Department` entidade.</span><span class="sxs-lookup"><span data-stu-id="c1f50-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="c1f50-146">O `Department` entidade contém o departamento de curso é atribuído a.</span><span class="sxs-lookup"><span data-stu-id="c1f50-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="c1f50-147">Para exibir o nome do departamento atribuído em uma lista de cursos:</span><span class="sxs-lookup"><span data-stu-id="c1f50-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="c1f50-148">Obter o `Name` propriedade o `Department` entidade.</span><span class="sxs-lookup"><span data-stu-id="c1f50-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="c1f50-149">O `Department` entidade vem do `Course.Department` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="c1f50-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Departamento](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="c1f50-151">O modelo de curso o Scaffold</span><span class="sxs-lookup"><span data-stu-id="c1f50-151">Scaffold the Course model</span></span>

* <span data-ttu-id="c1f50-152">Sair do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1f50-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="c1f50-153">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="c1f50-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="c1f50-154">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="c1f50-154">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="c1f50-155">Os scaffolds comando anterior a `Course` modelo.</span><span class="sxs-lookup"><span data-stu-id="c1f50-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="c1f50-156">Abra o projeto no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1f50-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="c1f50-157">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="c1f50-157">Build the project.</span></span> <span data-ttu-id="c1f50-158">A compilação gera erros, como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="c1f50-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="c1f50-159">Globalmente altere `_context.Course` para `_context.Courses` (ou seja, adicionar um "s" para `Course`).</span><span class="sxs-lookup"><span data-stu-id="c1f50-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="c1f50-160">7 ocorrências forem encontradas e atualizadas.</span><span class="sxs-lookup"><span data-stu-id="c1f50-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="c1f50-161">Abra *Pages/Courses/Index.cshtml.cs* e examine o `OnGetAsync` método.</span><span class="sxs-lookup"><span data-stu-id="c1f50-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="c1f50-162">O mecanismo de scaffolding especificado carregamento rápido para o `Department` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="c1f50-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="c1f50-163">O `Include` método especifica carregamento rápido.</span><span class="sxs-lookup"><span data-stu-id="c1f50-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="c1f50-164">Execute o aplicativo e selecione o **cursos** link.</span><span class="sxs-lookup"><span data-stu-id="c1f50-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="c1f50-165">Exibe a coluna do departamento a `DepartmentID`, que não é útil.</span><span class="sxs-lookup"><span data-stu-id="c1f50-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="c1f50-166">Atualize o método `OnGetAsync` pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="c1f50-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="c1f50-167">Adiciona o código anterior `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="c1f50-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="c1f50-168">`AsNoTracking`melhora o desempenho porque as entidades retornadas não são rastreadas.</span><span class="sxs-lookup"><span data-stu-id="c1f50-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="c1f50-169">As entidades não são rastreadas porque eles não são atualizados no contexto atual.</span><span class="sxs-lookup"><span data-stu-id="c1f50-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="c1f50-170">Atualização *Views/Courses/Index.cshtml* com a seguinte marcação realçada:</span><span class="sxs-lookup"><span data-stu-id="c1f50-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="c1f50-171">As seguintes alterações foram feitas para o código de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="c1f50-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="c1f50-172">Alterado o título do índice para cursos.</span><span class="sxs-lookup"><span data-stu-id="c1f50-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="c1f50-173">Adicionado um **número** coluna mostra o `CourseID` o valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="c1f50-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="c1f50-174">Por padrão, as chaves primárias não são Scaffold porque normalmente eles são sem sentido para os usuários finais.</span><span class="sxs-lookup"><span data-stu-id="c1f50-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="c1f50-175">No entanto, nesse caso a chave primária é significativa.</span><span class="sxs-lookup"><span data-stu-id="c1f50-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="c1f50-176">Alterado o **departamento** coluna para exibir o nome de departamento.</span><span class="sxs-lookup"><span data-stu-id="c1f50-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="c1f50-177">Exibe o código de `Name` propriedade do `Department` entidade que é carregada no `Department` propriedade de navegação:</span><span class="sxs-lookup"><span data-stu-id="c1f50-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="c1f50-178">Execute o aplicativo e selecione o **cursos** guia para ver a lista com nomes de departamento.</span><span class="sxs-lookup"><span data-stu-id="c1f50-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Página de índice de cursos](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="c1f50-180">Ao carregar os dados com Select relacionados</span><span class="sxs-lookup"><span data-stu-id="c1f50-180">Loading related data with Select</span></span>

<span data-ttu-id="c1f50-181">O `OnGetAsync` método carrega dados relacionados com o `Include` método:</span><span class="sxs-lookup"><span data-stu-id="c1f50-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="c1f50-182">O `Select` operador carrega somente os dados relacionados necessários.</span><span class="sxs-lookup"><span data-stu-id="c1f50-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="c1f50-183">Para itens únicos, como o `Department.Name` usa uma junção interna do SQL.</span><span class="sxs-lookup"><span data-stu-id="c1f50-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="c1f50-184">Para coleções ele usa outro acesso de banco de dados, mas também o.`Include`</span><span class="sxs-lookup"><span data-stu-id="c1f50-184">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="c1f50-185">operador em coleções.</span><span class="sxs-lookup"><span data-stu-id="c1f50-185">operator on collections.</span></span>

<span data-ttu-id="c1f50-186">O código a seguir carrega os dados relacionados com o `Select` método:</span><span class="sxs-lookup"><span data-stu-id="c1f50-186">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="c1f50-187">O `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="c1f50-187">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="c1f50-188">Consulte [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) para obter um exemplo completo.</span><span class="sxs-lookup"><span data-stu-id="c1f50-188">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="c1f50-189">Criar uma página de instrutores que mostra os cursos e registros</span><span class="sxs-lookup"><span data-stu-id="c1f50-189">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="c1f50-190">Nesta seção, a página de instrutores é criada.</span><span class="sxs-lookup"><span data-stu-id="c1f50-190">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="c1f50-191">![Página de índice instrutores](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="c1f50-191">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="c1f50-192">Essa página lê e exibe dados relacionados das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="c1f50-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="c1f50-193">A lista de instrutores exibe dados relacionados do `OfficeAssignment` entidade (Office na imagem anterior).</span><span class="sxs-lookup"><span data-stu-id="c1f50-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="c1f50-194">O `Instructor` e `OfficeAssignment` são entidades em uma relação um-para-zero-ou-um.</span><span class="sxs-lookup"><span data-stu-id="c1f50-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="c1f50-195">Carregamento adiantado é usado para o `OfficeAssignment` entidades.</span><span class="sxs-lookup"><span data-stu-id="c1f50-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="c1f50-196">Carregamento adiantado é geralmente mais eficiente quando os dados relacionados que precise ser exibido.</span><span class="sxs-lookup"><span data-stu-id="c1f50-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="c1f50-197">Nesse caso, as atribuições do office para os instrutores são exibidas.</span><span class="sxs-lookup"><span data-stu-id="c1f50-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="c1f50-198">Quando o usuário seleciona um instrutor (Harui na imagem anterior), relacionado `Course` as entidades são exibidas.</span><span class="sxs-lookup"><span data-stu-id="c1f50-198">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="c1f50-199">O `Instructor` e `Course` são entidades em uma relação muitos-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="c1f50-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="c1f50-200">Carregamento para rápido o `Course` entidades e suas relacionados `Department` entidades é usado.</span><span class="sxs-lookup"><span data-stu-id="c1f50-200">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="c1f50-201">Nesse caso, consultas separadas podem ser mais eficientes porque são necessários somente o cursos para o instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1f50-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="c1f50-202">Este exemplo mostra como usar o carregamento rápido para propriedades de navegação em entidades que estão nas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="c1f50-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="c1f50-203">Quando o usuário seleciona um curso (química na imagem anterior), dados de relacionados a `Enrollments` entidade é exibida.</span><span class="sxs-lookup"><span data-stu-id="c1f50-203">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="c1f50-204">A imagem anterior, classificação e nome do aluno são exibidos.</span><span class="sxs-lookup"><span data-stu-id="c1f50-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="c1f50-205">O `Course` e `Enrollment` são entidades em uma relação um-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="c1f50-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="c1f50-206">Criar um modelo de exibição para o modo de exibição do índice do instrutor</span><span class="sxs-lookup"><span data-stu-id="c1f50-206">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="c1f50-207">A página de instrutores mostra dados de três tabelas diferentes.</span><span class="sxs-lookup"><span data-stu-id="c1f50-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="c1f50-208">Um modelo de exibição é criado que inclui as três entidades que representam as três tabelas.</span><span class="sxs-lookup"><span data-stu-id="c1f50-208">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="c1f50-209">No *SchoolViewModels* pasta, criar *InstructorIndexData.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="c1f50-209">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="c1f50-210">Scaffold modelo instrutor</span><span class="sxs-lookup"><span data-stu-id="c1f50-210">Scaffold the Instructor model</span></span>

* <span data-ttu-id="c1f50-211">Sair do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1f50-211">Exit Visual Studio.</span></span>
* <span data-ttu-id="c1f50-212">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="c1f50-212">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="c1f50-213">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="c1f50-213">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="c1f50-214">Os scaffolds comando anterior a `Instructor` modelo.</span><span class="sxs-lookup"><span data-stu-id="c1f50-214">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="c1f50-215">Abra o projeto no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1f50-215">Open the project in Visual Studio.</span></span>

<span data-ttu-id="c1f50-216">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="c1f50-216">Build the project.</span></span> <span data-ttu-id="c1f50-217">A compilação gera erros.</span><span class="sxs-lookup"><span data-stu-id="c1f50-217">The build generates errors.</span></span>

<span data-ttu-id="c1f50-218">Globalmente altere `_context.Instructor` para `_context.Instructors` (ou seja, adicionar um "s" para `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="c1f50-218">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="c1f50-219">7 ocorrências forem encontradas e atualizadas.</span><span class="sxs-lookup"><span data-stu-id="c1f50-219">7 occurrences are found and updated.</span></span>

<span data-ttu-id="c1f50-220">Executar o aplicativo e navegue até a página de professores.</span><span class="sxs-lookup"><span data-stu-id="c1f50-220">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="c1f50-221">Substituir *Pages/Instructors/Index.cshtml.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="c1f50-221">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="c1f50-222">O `OnGetAsync` método aceita dados de rota opcional para a ID do instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1f50-222">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="c1f50-223">Examine a consulta no *Pages/Instructors/Index.cshtml* página:</span><span class="sxs-lookup"><span data-stu-id="c1f50-223">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="c1f50-224">A consulta tem duas inclui:</span><span class="sxs-lookup"><span data-stu-id="c1f50-224">The query has two includes:</span></span>

* <span data-ttu-id="c1f50-225">`OfficeAssignment`: Exibido no [exibição instrutores](#IP).</span><span class="sxs-lookup"><span data-stu-id="c1f50-225">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="c1f50-226">`CourseAssignments`: O que leva os cursos ministrada.</span><span class="sxs-lookup"><span data-stu-id="c1f50-226">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="c1f50-227">Atualizar a página de índice instrutores</span><span class="sxs-lookup"><span data-stu-id="c1f50-227">Update the instructors Index page</span></span>

<span data-ttu-id="c1f50-228">Atualização *Pages/Instructors/Index.cshtml* com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="c1f50-228">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="c1f50-229">A marcação anterior faz as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="c1f50-229">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="c1f50-230">Atualizações de `page` diretiva de `@page` para `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="c1f50-230">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="c1f50-231">`"{id:int?}"`é um modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="c1f50-231">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="c1f50-232">O modelo de rota altera cadeias de caracteres de consulta de inteiro na URL para dados de rota.</span><span class="sxs-lookup"><span data-stu-id="c1f50-232">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="c1f50-233">Por exemplo, clicando no **selecione** link para o instrutor quando a diretiva de página produz uma URL semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="c1f50-233">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="c1f50-234">Quando a diretiva de página é `@page "{id:int?}"`, a URL do anterior é:</span><span class="sxs-lookup"><span data-stu-id="c1f50-234">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="c1f50-235">Título da página é **instrutores**.</span><span class="sxs-lookup"><span data-stu-id="c1f50-235">Page title is **Instructors**.</span></span>
* <span data-ttu-id="c1f50-236">Adicionado um **Office** coluna que exibe `item.OfficeAssignment.Location` somente se `item.OfficeAssignment` não é nulo.</span><span class="sxs-lookup"><span data-stu-id="c1f50-236">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="c1f50-237">Como esta é uma relação um-para-zero-ou-um, pode não haver uma entidade OfficeAssignment relacionada.</span><span class="sxs-lookup"><span data-stu-id="c1f50-237">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="c1f50-238">Adicionado um **cursos** coluna que exibe os cursos ministrada por cada instrutor.</span><span class="sxs-lookup"><span data-stu-id="c1f50-238">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="c1f50-239">Consulte [explícita transição de linha com `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) para obter mais informações sobre essa sintaxe do razor.</span><span class="sxs-lookup"><span data-stu-id="c1f50-239">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="c1f50-240">Código adicionado dinamicamente adiciona `class="success"` para o `tr` elemento do instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1f50-240">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="c1f50-241">Isso define uma cor de plano de fundo para a linha selecionada usando uma classe de inicialização.</span><span class="sxs-lookup"><span data-stu-id="c1f50-241">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="c1f50-242">Adicionado um novo hiperlink rotulado **selecione**.</span><span class="sxs-lookup"><span data-stu-id="c1f50-242">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="c1f50-243">Este link envia a ID do instrutor selecionado para o `Index` método e define uma cor de plano de fundo.</span><span class="sxs-lookup"><span data-stu-id="c1f50-243">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="c1f50-244">Execute o aplicativo e selecione o **instrutores** guia. A página exibe o `Location` (office) de relacionado `OfficeAssignment` entidade.</span><span class="sxs-lookup"><span data-stu-id="c1f50-244">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="c1f50-245">Se OfficeAssignment' é nulo, uma célula de tabela vazia será exibida.</span><span class="sxs-lookup"><span data-stu-id="c1f50-245">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Página de índice instrutores que nada selecionado](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="c1f50-247">Clique no **selecione** link.</span><span class="sxs-lookup"><span data-stu-id="c1f50-247">Click on the **Select** link.</span></span> <span data-ttu-id="c1f50-248">As alterações de estilo de linha.</span><span class="sxs-lookup"><span data-stu-id="c1f50-248">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="c1f50-249">Adicionar cursos ministrados por instrutor selecionado</span><span class="sxs-lookup"><span data-stu-id="c1f50-249">Add courses taught by selected instructor</span></span>

<span data-ttu-id="c1f50-250">Atualização de `OnGetAsync` método *Pages/Instructors/Index.cshtml.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="c1f50-250">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="c1f50-251">Examine a consulta atualizada:</span><span class="sxs-lookup"><span data-stu-id="c1f50-251">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="c1f50-252">A consulta anterior adiciona o `Department` entidades.</span><span class="sxs-lookup"><span data-stu-id="c1f50-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="c1f50-253">O código a seguir executa quando instrutor é selecionado (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="c1f50-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="c1f50-254">O instrutor selecionado é recuperado da lista de instrutores no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c1f50-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="c1f50-255">O modelo de exibição `Courses` propriedade é carregada com o `Course` entidades que instrutor `CourseAssignments` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="c1f50-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="c1f50-256">O `Where` método retorna uma coleção.</span><span class="sxs-lookup"><span data-stu-id="c1f50-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="c1f50-257">Na anterior `Where` método, um único `Instructor` entidade for retornada.</span><span class="sxs-lookup"><span data-stu-id="c1f50-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="c1f50-258">O `Single` método converte a coleção em uma única `Instructor` entidade.</span><span class="sxs-lookup"><span data-stu-id="c1f50-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="c1f50-259">O `Instructor` entidade fornece acesso para o `CourseAssignments` propriedade.</span><span class="sxs-lookup"><span data-stu-id="c1f50-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="c1f50-260">`CourseAssignments`fornece acesso a relacionado `Course` entidades.</span><span class="sxs-lookup"><span data-stu-id="c1f50-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M instrutor para cursos](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="c1f50-262">O `Single` método é usado em uma coleção quando a coleção tem apenas um item.</span><span class="sxs-lookup"><span data-stu-id="c1f50-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="c1f50-263">O `Single` método lançará uma exceção se a coleção está vazia ou se houver mais de um item.</span><span class="sxs-lookup"><span data-stu-id="c1f50-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="c1f50-264">Uma alternativa é `SingleOrDefault`, que retorna um valor padrão (null nesse caso) se a coleção está vazia.</span><span class="sxs-lookup"><span data-stu-id="c1f50-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="c1f50-265">Usando `SingleOrDefault` em uma coleção vazia:</span><span class="sxs-lookup"><span data-stu-id="c1f50-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="c1f50-266">Resulta em uma exceção (de tentativa de encontrar um `Courses` propriedade em uma referência nula).</span><span class="sxs-lookup"><span data-stu-id="c1f50-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="c1f50-267">A mensagem de exceção indicaria menos claramente a causa do problema.</span><span class="sxs-lookup"><span data-stu-id="c1f50-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="c1f50-268">O código a seguir preenche o modelo de exibição `Enrollments` propriedade quando um curso é selecionado:</span><span class="sxs-lookup"><span data-stu-id="c1f50-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="c1f50-269">Adicione a seguinte marcação para o fim do *Pages/Courses/Index.cshtml* Razor de página:</span><span class="sxs-lookup"><span data-stu-id="c1f50-269">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="c1f50-270">A marcação anterior exibe uma lista de cursos relacionados ao instrutor ao instrutor está selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1f50-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="c1f50-271">Teste o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c1f50-271">Test the app.</span></span> <span data-ttu-id="c1f50-272">Clique em uma **selecione** link na página de professores.</span><span class="sxs-lookup"><span data-stu-id="c1f50-272">Click on a **Select** link on the instructors page.</span></span>

![Instrutor de página de índice instrutores selecionado](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="c1f50-274">Mostrar dados do aluno</span><span class="sxs-lookup"><span data-stu-id="c1f50-274">Show student data</span></span>

<span data-ttu-id="c1f50-275">Nesta seção, o aplicativo é atualizado para mostrar os dados de aluno para um curso selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1f50-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="c1f50-276">Atualizar a consulta a `OnGetAsync` método *Pages/Instructors/Index.cshtml.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="c1f50-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="c1f50-277">Atualização *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c1f50-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="c1f50-278">Adicione a seguinte marcação para o final do arquivo:</span><span class="sxs-lookup"><span data-stu-id="c1f50-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="c1f50-279">A marcação anterior exibe uma lista dos alunos que são registrados no curso selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1f50-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="c1f50-280">Atualize a página e selecionar um instrutor.</span><span class="sxs-lookup"><span data-stu-id="c1f50-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="c1f50-281">Selecione um curso para ver a lista de estudantes registrados e suas classificações.</span><span class="sxs-lookup"><span data-stu-id="c1f50-281">Select a course to see the list of enrolled students and their grades.</span></span>

![Instrutor de página de índice professores e curso selecionado](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="c1f50-283">Usando um único</span><span class="sxs-lookup"><span data-stu-id="c1f50-283">Using Single</span></span>

<span data-ttu-id="c1f50-284">O `Single` método pode passar o `Where` condição em vez de chamar o `Where` método separadamente:</span><span class="sxs-lookup"><span data-stu-id="c1f50-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="c1f50-285">Anterior `Single` abordagem não fornece nenhuma benefícios com o uso `Where`.</span><span class="sxs-lookup"><span data-stu-id="c1f50-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="c1f50-286">Alguns desenvolvedores preferem o `Single` abordagem estilo.</span><span class="sxs-lookup"><span data-stu-id="c1f50-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="c1f50-287">Carregamento explícito</span><span class="sxs-lookup"><span data-stu-id="c1f50-287">Explicit loading</span></span>

<span data-ttu-id="c1f50-288">O código atual Especifica o carregamento rápido para `Enrollments` e `Students`:</span><span class="sxs-lookup"><span data-stu-id="c1f50-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="c1f50-289">Suponha que os usuários desejam ver registros em um curso raramente.</span><span class="sxs-lookup"><span data-stu-id="c1f50-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="c1f50-290">Nesse caso, uma otimização seria carregar apenas os dados de registro se solicitado.</span><span class="sxs-lookup"><span data-stu-id="c1f50-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="c1f50-291">Nesta seção, o `OnGetAsync` é atualizado para usar o carregamento explícito de `Enrollments` e `Students`.</span><span class="sxs-lookup"><span data-stu-id="c1f50-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="c1f50-292">Atualização de `OnGetAsync` com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="c1f50-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="c1f50-293">O código anterior elimina o *ThenInclude* chamadas de método para dados de registro e do aluno.</span><span class="sxs-lookup"><span data-stu-id="c1f50-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="c1f50-294">Se um curso for selecionado, o código realçado recupera:</span><span class="sxs-lookup"><span data-stu-id="c1f50-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="c1f50-295">O `Enrollment` entidades para o curso selecionado.</span><span class="sxs-lookup"><span data-stu-id="c1f50-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="c1f50-296">O `Student` entidades para cada `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c1f50-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="c1f50-297">Observe anterior código comenta `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="c1f50-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="c1f50-298">Propriedades de navegação só podem ser carregadas explicitamente para entidades controladas.</span><span class="sxs-lookup"><span data-stu-id="c1f50-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="c1f50-299">Teste o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c1f50-299">Test the app.</span></span> <span data-ttu-id="c1f50-300">De uma perspectiva de usuários, o aplicativo se comporta de forma idêntica para a versão anterior.</span><span class="sxs-lookup"><span data-stu-id="c1f50-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="c1f50-301">O seguinte tutorial mostra como atualizar os dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="c1f50-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="c1f50-302">[Anterior](xref:data/ef-rp/complex-data-model)
>[Próximo](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="c1f50-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
