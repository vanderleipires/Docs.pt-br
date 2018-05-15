---
title: Páginas Razor com o EF Core no ASP.NET Core – Atualizar dados relacionados – 7 de 8
author: rick-anderson
description: Neste tutorial, você atualizará dados relacionados pela atualização dos campos de chave estrangeira e das propriedades de navegação.
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 2eff6cd5f4bb737cb79875c9b04c889914376cd0
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a><span data-ttu-id="486fa-103">Páginas Razor com o EF Core no ASP.NET Core – Atualizar dados relacionados – 7 de 8</span><span class="sxs-lookup"><span data-stu-id="486fa-103">Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8</span></span>

<span data-ttu-id="486fa-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="486fa-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="486fa-105">Este tutorial demonstra como atualizar dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="486fa-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="486fa-106">Caso tenha problemas que não consiga resolver, baixe o [aplicativo concluído para este estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="486fa-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="486fa-107">As ilustrações a seguir mostram algumas das páginas concluídas.</span><span class="sxs-lookup"><span data-stu-id="486fa-107">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="486fa-108">![Página Editar Curso](update-related-data/_static/course-edit.png)
![Página Editar Instrutor](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="486fa-108">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="486fa-109">Examine e teste as páginas de cursos Criar e Editar.</span><span class="sxs-lookup"><span data-stu-id="486fa-109">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="486fa-110">Crie um novo curso.</span><span class="sxs-lookup"><span data-stu-id="486fa-110">Create a new course.</span></span> <span data-ttu-id="486fa-111">O departamento é selecionado por sua chave primária (um inteiro), não pelo nome.</span><span class="sxs-lookup"><span data-stu-id="486fa-111">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="486fa-112">Edite o novo curso.</span><span class="sxs-lookup"><span data-stu-id="486fa-112">Edit the new course.</span></span> <span data-ttu-id="486fa-113">Quando concluir o teste, exclua o novo curso.</span><span class="sxs-lookup"><span data-stu-id="486fa-113">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="486fa-114">Criar uma classe base para compartilhar um código comum</span><span class="sxs-lookup"><span data-stu-id="486fa-114">Create a base class to share common code</span></span>

<span data-ttu-id="486fa-115">As páginas Cursos/Criar e Cursos/Editar precisam de uma lista de nomes de departamentos.</span><span class="sxs-lookup"><span data-stu-id="486fa-115">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="486fa-116">Crie a classe base *Pages/Courses/DepartmentNamePageModel.cshtml.cs* para as páginas Criar e Editar:</span><span class="sxs-lookup"><span data-stu-id="486fa-116">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="486fa-117">O código anterior cria uma [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) para conter a lista de nomes de departamentos.</span><span class="sxs-lookup"><span data-stu-id="486fa-117">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="486fa-118">Se `selectedDepartment` for especificado, esse departamento estará selecionado na `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="486fa-118">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="486fa-119">As classes de modelo da página Criar e Editar serão derivadas de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="486fa-119">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="486fa-120">Personalizar as páginas Courses</span><span class="sxs-lookup"><span data-stu-id="486fa-120">Customize the Courses Pages</span></span>

<span data-ttu-id="486fa-121">Quando uma nova entidade de curso é criada, ela precisa ter uma relação com um departamento existente.</span><span class="sxs-lookup"><span data-stu-id="486fa-121">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="486fa-122">Para adicionar um departamento durante a criação de um curso, a classe base para Create e Edit contém uma lista suspensa para seleção do departamento.</span><span class="sxs-lookup"><span data-stu-id="486fa-122">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="486fa-123">A lista suspensa define a propriedade `Course.DepartmentID` de FK (chave estrangeira).</span><span class="sxs-lookup"><span data-stu-id="486fa-123">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="486fa-124">O EF Core usa a `Course.DepartmentID` FK para carregar a propriedade de navegação `Department`.</span><span class="sxs-lookup"><span data-stu-id="486fa-124">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Criar curso](update-related-data/_static/ddl.png)

<span data-ttu-id="486fa-126">Atualize o modelo da página Criar com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="486fa-126">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="486fa-127">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="486fa-127">The preceding code:</span></span>

* <span data-ttu-id="486fa-128">Deriva de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="486fa-128">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="486fa-129">Usa `TryUpdateModelAsync` para impedir o [excesso de postagem](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="486fa-129">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="486fa-130">Substitui `ViewData["DepartmentID"]` por `DepartmentNameSL` (da classe base).</span><span class="sxs-lookup"><span data-stu-id="486fa-130">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="486fa-131">`ViewData["DepartmentID"]` é substituído pelo `DepartmentNameSL` fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="486fa-131">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="486fa-132">Modelos fortemente tipados são preferíveis aos fracamente tipados.</span><span class="sxs-lookup"><span data-stu-id="486fa-132">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="486fa-133">Para obter mais informações, consulte [Dados fracamente tipados (ViewData e ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="486fa-133">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="486fa-134">Atualizar a página Criar Cursos</span><span class="sxs-lookup"><span data-stu-id="486fa-134">Update the Courses Create page</span></span>

<span data-ttu-id="486fa-135">Atualize *Pages/Courses/Create.cshtml* com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="486fa-135">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="486fa-136">A marcação anterior faz as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="486fa-136">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="486fa-137">Altera a legenda de **DepartmentID** para **Departamento**.</span><span class="sxs-lookup"><span data-stu-id="486fa-137">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="486fa-138">Substitui `"ViewBag.DepartmentID"` por `DepartmentNameSL` (da classe base).</span><span class="sxs-lookup"><span data-stu-id="486fa-138">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="486fa-139">Adiciona a opção "Selecionar Departamento".</span><span class="sxs-lookup"><span data-stu-id="486fa-139">Adds the "Select Department" option.</span></span> <span data-ttu-id="486fa-140">Essa alteração renderiza "Selecionar Departamento", em vez do departamento primeiro.</span><span class="sxs-lookup"><span data-stu-id="486fa-140">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="486fa-141">Adiciona uma mensagem de validação quando o departamento não está selecionado.</span><span class="sxs-lookup"><span data-stu-id="486fa-141">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="486fa-142">A Página do Razor usa a opção [Selecionar Auxiliar de Marcação](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="486fa-142">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="486fa-143">Teste a página Criar.</span><span class="sxs-lookup"><span data-stu-id="486fa-143">Test the Create page.</span></span> <span data-ttu-id="486fa-144">A página Criar exibe o nome do departamento em vez de a ID do departamento.</span><span class="sxs-lookup"><span data-stu-id="486fa-144">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="486fa-145">Atualize a página Editar Cursos.</span><span class="sxs-lookup"><span data-stu-id="486fa-145">Update the Courses Edit page.</span></span>

<span data-ttu-id="486fa-146">Atualize o modelo de página de edição com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="486fa-146">Update the edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="486fa-147">As alterações são semelhantes às feitas no modelo da página Criar.</span><span class="sxs-lookup"><span data-stu-id="486fa-147">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="486fa-148">No código anterior, `PopulateDepartmentsDropDownList` passa a ID do departamento, que seleciona o departamento especificado na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="486fa-148">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="486fa-149">Atualize *Pages/Courses/Edit.cshtml* com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="486fa-149">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="486fa-150">A marcação anterior faz as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="486fa-150">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="486fa-151">Exibe a ID do curso.</span><span class="sxs-lookup"><span data-stu-id="486fa-151">Displays the course ID.</span></span> <span data-ttu-id="486fa-152">Geralmente, a PK (chave primária) de uma entidade não é exibida.</span><span class="sxs-lookup"><span data-stu-id="486fa-152">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="486fa-153">Em geral, PKs não têm sentido para os usuários.</span><span class="sxs-lookup"><span data-stu-id="486fa-153">PKs are usually meaningless to users.</span></span> <span data-ttu-id="486fa-154">Nesse caso, o PK é o número do curso.</span><span class="sxs-lookup"><span data-stu-id="486fa-154">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="486fa-155">Altera a legenda de **DepartmentID** para **Departamento**.</span><span class="sxs-lookup"><span data-stu-id="486fa-155">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="486fa-156">Substitui `"ViewBag.DepartmentID"` por `DepartmentNameSL` (da classe base).</span><span class="sxs-lookup"><span data-stu-id="486fa-156">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="486fa-157">Adiciona a opção "Selecionar Departamento".</span><span class="sxs-lookup"><span data-stu-id="486fa-157">Adds the "Select Department" option.</span></span> <span data-ttu-id="486fa-158">Essa alteração renderiza "Selecionar Departamento", em vez do departamento primeiro.</span><span class="sxs-lookup"><span data-stu-id="486fa-158">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="486fa-159">Adiciona uma mensagem de validação quando o departamento não está selecionado.</span><span class="sxs-lookup"><span data-stu-id="486fa-159">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="486fa-160">A página contém um campo oculto (`<input type="hidden">`) para o número do curso.</span><span class="sxs-lookup"><span data-stu-id="486fa-160">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="486fa-161">A adição de um auxiliar de marcação `<label>` com `asp-for="Course.CourseID"` não elimina a necessidade do campo oculto.</span><span class="sxs-lookup"><span data-stu-id="486fa-161">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="486fa-162">`<input type="hidden">` é necessário para que o número seja incluído nos dados postados quando o usuário clicar em **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="486fa-162">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="486fa-163">Teste o código atualizado.</span><span class="sxs-lookup"><span data-stu-id="486fa-163">Test the updated code.</span></span> <span data-ttu-id="486fa-164">Crie, edite e exclua um curso.</span><span class="sxs-lookup"><span data-stu-id="486fa-164">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="486fa-165">Adicionar AsNoTracking aos modelos de página Detalhes e Excluir</span><span class="sxs-lookup"><span data-stu-id="486fa-165">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="486fa-166">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) pode melhorar o desempenho quando o acompanhamento não é necessário.</span><span class="sxs-lookup"><span data-stu-id="486fa-166">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="486fa-167">Adicione `AsNoTracking` ao modelo de página Excluir e Detalhes.</span><span class="sxs-lookup"><span data-stu-id="486fa-167">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="486fa-168">O seguinte código mostra o modelo de página Excluir atualizado:</span><span class="sxs-lookup"><span data-stu-id="486fa-168">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="486fa-169">Atualize o método `OnGetAsync` no arquivo *Pages/Courses/Details.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="486fa-169">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="486fa-170">Modificar as páginas Excluir e Detalhes</span><span class="sxs-lookup"><span data-stu-id="486fa-170">Modify the Delete and Details pages</span></span>

<span data-ttu-id="486fa-171">Atualize a página Excluir do Razor com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="486fa-171">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="486fa-172">Faça as mesmas alterações na página Detalhes.</span><span class="sxs-lookup"><span data-stu-id="486fa-172">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="486fa-173">Testar as páginas Curso</span><span class="sxs-lookup"><span data-stu-id="486fa-173">Test the Course pages</span></span>

<span data-ttu-id="486fa-174">Teste as páginas Criar, Editar, Detalhes e Excluir.</span><span class="sxs-lookup"><span data-stu-id="486fa-174">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="486fa-175">Atualizar as páginas do instrutor</span><span class="sxs-lookup"><span data-stu-id="486fa-175">Update the instructor pages</span></span>

<span data-ttu-id="486fa-176">As seções a seguir atualizam as páginas do instrutor.</span><span class="sxs-lookup"><span data-stu-id="486fa-176">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="486fa-177">Adicionar local do escritório</span><span class="sxs-lookup"><span data-stu-id="486fa-177">Add office location</span></span>

<span data-ttu-id="486fa-178">Ao editar um registro de instrutor, é recomendável atualizar a atribuição de escritório do instrutor.</span><span class="sxs-lookup"><span data-stu-id="486fa-178">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="486fa-179">A entidade `Instructor` tem uma relação um para zero ou um com a entidade `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="486fa-179">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="486fa-180">O código do instrutor precisa manipular:</span><span class="sxs-lookup"><span data-stu-id="486fa-180">The instructor code must handle:</span></span>

* <span data-ttu-id="486fa-181">Se o usuário limpar a atribuição de escritório, exclua a entidade `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="486fa-181">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="486fa-182">Se o usuário inserir uma atribuição de escritório e ela estava vazia, crie uma nova entidade `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="486fa-182">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="486fa-183">Se o usuário alterar a atribuição de escritório, atualize a entidade `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="486fa-183">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="486fa-184">Atualize o modelo de página Editar Instrutores com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="486fa-184">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="486fa-185">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="486fa-185">The preceding code:</span></span>

- <span data-ttu-id="486fa-186">Obtém a entidade `Instructor` atual do banco de dados usando o carregamento adiantado para a propriedade de navegação `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="486fa-186">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="486fa-187">Atualiza a entidade `Instructor` recuperada com valores do associador de modelos.</span><span class="sxs-lookup"><span data-stu-id="486fa-187">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="486fa-188">`TryUpdateModel` impede o [excesso de postagem](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="486fa-188">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="486fa-189">Se o local do escritório estiver em branco, `Instructor.OfficeAssignment` será definido como nulo.</span><span class="sxs-lookup"><span data-stu-id="486fa-189">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="486fa-190">Quando `Instructor.OfficeAssignment` é nulo, a linha relacionada na tabela `OfficeAssignment` é excluída.</span><span class="sxs-lookup"><span data-stu-id="486fa-190">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="486fa-191">Atualizar a página Editar Instrutor</span><span class="sxs-lookup"><span data-stu-id="486fa-191">Update the instructor Edit page</span></span>

<span data-ttu-id="486fa-192">Atualize *Pages/Instructors/Edit.cshtml* com o local do escritório:</span><span class="sxs-lookup"><span data-stu-id="486fa-192">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="486fa-193">Verifique se você pode alterar o local do escritório de um instrutor.</span><span class="sxs-lookup"><span data-stu-id="486fa-193">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="486fa-194">Adicionar atribuições de Curso à página Editar Instrutor</span><span class="sxs-lookup"><span data-stu-id="486fa-194">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="486fa-195">Os instrutores podem ministrar a quantidade de cursos que desejarem.</span><span class="sxs-lookup"><span data-stu-id="486fa-195">Instructors may teach any number of courses.</span></span> <span data-ttu-id="486fa-196">Nesta seção, você adiciona a capacidade de alterar as atribuições de curso.</span><span class="sxs-lookup"><span data-stu-id="486fa-196">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="486fa-197">A seguinte imagem mostra a página Editar Instrutor atualizada:</span><span class="sxs-lookup"><span data-stu-id="486fa-197">The following image shows the updated instructor Edit page:</span></span>

![Página Editar Instrutor com cursos](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="486fa-199">`Course` e `Instructor` têm uma relação muitos para muitos.</span><span class="sxs-lookup"><span data-stu-id="486fa-199">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="486fa-200">Para adicionar e remover relações, adicione e remova entidades do conjunto de entidades de junção `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="486fa-200">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="486fa-201">As caixas de seleção permitem alterações em cursos aos quais um instrutor é atribuído.</span><span class="sxs-lookup"><span data-stu-id="486fa-201">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="486fa-202">Uma caixa de seleção é exibida para cada curso no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="486fa-202">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="486fa-203">Os cursos aos quais o instrutor é atribuído estão marcados.</span><span class="sxs-lookup"><span data-stu-id="486fa-203">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="486fa-204">O usuário pode marcar ou desmarcar as caixas de seleção para alterar as atribuições de curso.</span><span class="sxs-lookup"><span data-stu-id="486fa-204">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="486fa-205">Se a quantidade de cursos for muito maior:</span><span class="sxs-lookup"><span data-stu-id="486fa-205">If the number of courses were much greater:</span></span>

* <span data-ttu-id="486fa-206">Provavelmente, você usará outra interface do usuário para exibir os cursos.</span><span class="sxs-lookup"><span data-stu-id="486fa-206">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="486fa-207">O método de manipulação de uma entidade de junção para criar ou excluir relações não será alterado.</span><span class="sxs-lookup"><span data-stu-id="486fa-207">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="486fa-208">Adicionar classes para dar suporte às páginas Criar e Editar Instrutor</span><span class="sxs-lookup"><span data-stu-id="486fa-208">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="486fa-209">Crie *SchoolViewModels/AssignedCourseData.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="486fa-209">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="486fa-210">A classe `AssignedCourseData` contém dados para criar as caixas de seleção para os cursos atribuídos por um instrutor.</span><span class="sxs-lookup"><span data-stu-id="486fa-210">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="486fa-211">Crie a classe base *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="486fa-211">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="486fa-212">A `InstructorCoursesPageModel` é a classe base que será usada para os modelos de página Editar e Criar.</span><span class="sxs-lookup"><span data-stu-id="486fa-212">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="486fa-213">`PopulateAssignedCourseData` lê todas as entidades `Course` para popular `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="486fa-213">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="486fa-214">Para cada curso, o código define a `CourseID`, o título e se o instrutor está ou não atribuído ao curso.</span><span class="sxs-lookup"><span data-stu-id="486fa-214">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="486fa-215">Um [HashSet](/dotnet/api/system.collections.generic.hashset-1) é usado para criar pesquisas eficientes.</span><span class="sxs-lookup"><span data-stu-id="486fa-215">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="486fa-216">Modelo de página Editar Instrutor</span><span class="sxs-lookup"><span data-stu-id="486fa-216">Instructors Edit page model</span></span>

<span data-ttu-id="486fa-217">Atualize o modelo de página Editar Instrutor com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="486fa-217">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="486fa-218">O código anterior manipula as alterações de atribuição de escritório.</span><span class="sxs-lookup"><span data-stu-id="486fa-218">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="486fa-219">Atualize a Exibição do Razor do instrutor:</span><span class="sxs-lookup"><span data-stu-id="486fa-219">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="486fa-220">Quando você cola o código no Visual Studio, as quebras de linha são alteradas de uma forma que divide o código.</span><span class="sxs-lookup"><span data-stu-id="486fa-220">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="486fa-221">Pressione Ctrl+Z uma vez para desfazer a formatação automática.</span><span class="sxs-lookup"><span data-stu-id="486fa-221">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="486fa-222">A tecla de atalho Ctrl+Z corrige as quebras de linha para que elas se pareçam com o que você vê aqui.</span><span class="sxs-lookup"><span data-stu-id="486fa-222">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="486fa-223">O recuo não precisa ser perfeito, mas cada uma das linhas `@</tr><tr>`, `@:<td>`, `@:</td>` e `@:</tr>` precisa estar em uma única linha, conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="486fa-223">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="486fa-224">Com o bloco de novo código selecionado, pressione Tab três vezes para alinhar o novo código com o código existente.</span><span class="sxs-lookup"><span data-stu-id="486fa-224">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="486fa-225">Vote ou examine o status deste bug [com este link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="486fa-225">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="486fa-226">Esse código anterior cria uma tabela HTML que contém três colunas.</span><span class="sxs-lookup"><span data-stu-id="486fa-226">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="486fa-227">Cada coluna tem uma caixa de seleção e uma legenda que contém o número e o título do curso.</span><span class="sxs-lookup"><span data-stu-id="486fa-227">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="486fa-228">Todas as caixas de seleção têm o mesmo nome ("selectedCourses").</span><span class="sxs-lookup"><span data-stu-id="486fa-228">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="486fa-229">O uso do mesmo nome instrui o associador de modelos a tratá-las como um grupo.</span><span class="sxs-lookup"><span data-stu-id="486fa-229">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="486fa-230">O atributo de valor de cada caixa de seleção é definido como `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="486fa-230">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="486fa-231">Quando a página é postada, o associador de modelos passa uma matriz que consiste nos valores `CourseID` para apenas as caixas de seleção marcadas.</span><span class="sxs-lookup"><span data-stu-id="486fa-231">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="486fa-232">Quando as caixas de seleção são inicialmente renderizadas, os cursos atribuídos ao instrutor têm atributos marcados.</span><span class="sxs-lookup"><span data-stu-id="486fa-232">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="486fa-233">Execute o aplicativo e teste a página Editar Instrutor atualizada.</span><span class="sxs-lookup"><span data-stu-id="486fa-233">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="486fa-234">Altere algumas atribuições de curso.</span><span class="sxs-lookup"><span data-stu-id="486fa-234">Change some course assignments.</span></span> <span data-ttu-id="486fa-235">As alterações são refletidas na página Índice.</span><span class="sxs-lookup"><span data-stu-id="486fa-235">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="486fa-236">Observação: a abordagem usada aqui para editar os dados de curso do instrutor funciona bem quando há uma quantidade limitada de cursos.</span><span class="sxs-lookup"><span data-stu-id="486fa-236">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="486fa-237">Para coleções muito maiores, uma interface do usuário e um método de atualização diferentes são mais utilizáveis e eficientes.</span><span class="sxs-lookup"><span data-stu-id="486fa-237">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="486fa-238">Atualizar a página Criar Instrutor</span><span class="sxs-lookup"><span data-stu-id="486fa-238">Update the instructors Create page</span></span>

<span data-ttu-id="486fa-239">Atualize o modelo de página Criar instrutor com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="486fa-239">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="486fa-240">O código anterior é semelhante ao código de *Pages/Instructors/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="486fa-240">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="486fa-241">Atualize a página Criar instrutor do Razor com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="486fa-241">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="486fa-242">Teste a página Criar Instrutor.</span><span class="sxs-lookup"><span data-stu-id="486fa-242">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="486fa-243">Atualizar a página Excluir</span><span class="sxs-lookup"><span data-stu-id="486fa-243">Update the Delete page</span></span>

<span data-ttu-id="486fa-244">Atualize o modelo da página Excluir com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="486fa-244">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="486fa-245">O código anterior faz as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="486fa-245">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="486fa-246">Usa o carregamento adiantado para a propriedade de navegação `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="486fa-246">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="486fa-247">`CourseAssignments` deve ser incluído ou eles não são excluídos quando o instrutor é excluído.</span><span class="sxs-lookup"><span data-stu-id="486fa-247">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="486fa-248">Para evitar a necessidade de lê-las, configure a exclusão em cascata no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="486fa-248">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="486fa-249">Se o instrutor a ser excluído é atribuído como administrador de qualquer departamento, remove a atribuição de instrutor desse departamento.</span><span class="sxs-lookup"><span data-stu-id="486fa-249">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="486fa-250">[Anterior](xref:data/ef-rp/read-related-data)
> [Próximo](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="486fa-250">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
