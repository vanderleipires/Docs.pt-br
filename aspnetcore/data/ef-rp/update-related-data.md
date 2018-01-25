---
title: "Páginas Razor com núcleo EF - atualizar dados relacionados - 7 de 8"
author: rick-anderson
description: "Neste tutorial atualizará os dados relacionados ao atualizar os campos de chave estrangeira e propriedades de navegação."
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 236589d0202a7f30f1e1a9d69902000fd9a2dd71
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="920e3-103">Atualizando dados relacionados - EF Core Razor páginas (7, 8)</span><span class="sxs-lookup"><span data-stu-id="920e3-103">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="920e3-104">Por [Tom Dykstra](https://github.com/tdykstra), e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="920e3-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="920e3-105">Este tutorial demonstra como atualizar dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="920e3-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="920e3-106">Se você tiver problemas, você não conseguir resolver, baixe o [aplicativo concluído para este estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="920e3-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="920e3-107">As ilustrações a seguir mostra algumas das páginas concluídas.</span><span class="sxs-lookup"><span data-stu-id="920e3-107">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="920e3-108">![Página de edição de curso](update-related-data/_static/course-edit.png)
![instrutor Editar página](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="920e3-108">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="920e3-109">Examine e testar as páginas de curso criar e editar.</span><span class="sxs-lookup"><span data-stu-id="920e3-109">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="920e3-110">Crie um novo curso.</span><span class="sxs-lookup"><span data-stu-id="920e3-110">Create a new course.</span></span> <span data-ttu-id="920e3-111">O departamento é selecionado por sua chave primária (um inteiro), não seu nome.</span><span class="sxs-lookup"><span data-stu-id="920e3-111">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="920e3-112">Edite o curso de novo.</span><span class="sxs-lookup"><span data-stu-id="920e3-112">Edit the new course.</span></span> <span data-ttu-id="920e3-113">Quando você concluir o teste, exclua o curso de novo.</span><span class="sxs-lookup"><span data-stu-id="920e3-113">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="920e3-114">Criar uma classe base para compartilhar código comum</span><span class="sxs-lookup"><span data-stu-id="920e3-114">Create a base class to share common code</span></span>

<span data-ttu-id="920e3-115">As páginas criar/cursos e cursos/editar cada precisam de uma lista de nomes de departamento.</span><span class="sxs-lookup"><span data-stu-id="920e3-115">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="920e3-116">Criar o *Pages/Courses/DepartmentNamePageModel.cshtml.cs* a classe base para criar e editar páginas:</span><span class="sxs-lookup"><span data-stu-id="920e3-116">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="920e3-117">O código anterior cria um [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) para conter a lista de nomes de departamento.</span><span class="sxs-lookup"><span data-stu-id="920e3-117">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="920e3-118">Se `selectedDepartment` for especificado, esse departamento for selecionado no `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="920e3-118">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="920e3-119">As classes de modelo de página de criar e editar serão derivado `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="920e3-119">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="920e3-120">Personalizar as páginas de cursos</span><span class="sxs-lookup"><span data-stu-id="920e3-120">Customize the Courses Pages</span></span>

<span data-ttu-id="920e3-121">Quando uma nova entidade de curso é criada, ele deve ter uma relação de um departamento existente.</span><span class="sxs-lookup"><span data-stu-id="920e3-121">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="920e3-122">Para adicionar um departamento durante a criação de um curso, a classe base para criar e editar contém uma lista suspensa para selecionar o departamento.</span><span class="sxs-lookup"><span data-stu-id="920e3-122">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="920e3-123">Define a lista suspensa de `Course.DepartmentID` propriedade de chave estrangeira (FK).</span><span class="sxs-lookup"><span data-stu-id="920e3-123">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="920e3-124">Núcleo EF usa o `Course.DepartmentID` FK ao carregar o `Department` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="920e3-124">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Criar curso](update-related-data/_static/ddl.png)

<span data-ttu-id="920e3-126">Atualize o modelo de página de criação com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="920e3-126">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="920e3-127">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="920e3-127">The preceding code:</span></span>

* <span data-ttu-id="920e3-128">Deriva de `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="920e3-128">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="920e3-129">Usa `TryUpdateModelAsync` para evitar [mais](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="920e3-129">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="920e3-130">Substitui `ViewData["DepartmentID"]` com `DepartmentNameSL` (da classe base).</span><span class="sxs-lookup"><span data-stu-id="920e3-130">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="920e3-131">`ViewData["DepartmentID"]`é substituído com rigidez de tipos `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="920e3-131">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="920e3-132">Modelos com rigidez de tipos são preferenciais em digitado sem rigidez.</span><span class="sxs-lookup"><span data-stu-id="920e3-132">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="920e3-133">Para obter mais informações, consulte [digitado sem rigidez dados (ViewData e ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="920e3-133">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="920e3-134">Atualize a página Criar cursos</span><span class="sxs-lookup"><span data-stu-id="920e3-134">Update the Courses Create page</span></span>

<span data-ttu-id="920e3-135">Atualização *Pages/Courses/Create.cshtml* com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="920e3-135">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="920e3-136">A marcação anterior faz as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="920e3-136">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="920e3-137">Altera a legenda do **DepartmentID** para **departamento**.</span><span class="sxs-lookup"><span data-stu-id="920e3-137">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="920e3-138">Substitui `"ViewBag.DepartmentID"` com `DepartmentNameSL` (da classe base).</span><span class="sxs-lookup"><span data-stu-id="920e3-138">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="920e3-139">Adiciona a opção "Selecione departamento".</span><span class="sxs-lookup"><span data-stu-id="920e3-139">Adds the "Select Department" option.</span></span> <span data-ttu-id="920e3-140">Essa alteração renderiza "Selecione departamento" em vez do departamento primeiro.</span><span class="sxs-lookup"><span data-stu-id="920e3-140">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="920e3-141">Adiciona uma mensagem de validação quando o departamento não está selecionado.</span><span class="sxs-lookup"><span data-stu-id="920e3-141">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="920e3-142">A página Razor usa o [selecione auxiliar de marca](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="920e3-142">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="920e3-143">Criar página de teste.</span><span class="sxs-lookup"><span data-stu-id="920e3-143">Test the Create page.</span></span> <span data-ttu-id="920e3-144">A página Criar exibe o nome do departamento em vez da ID do departamento.</span><span class="sxs-lookup"><span data-stu-id="920e3-144">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="920e3-145">Atualize a página Editar cursos.</span><span class="sxs-lookup"><span data-stu-id="920e3-145">Update the Courses Edit page.</span></span>

<span data-ttu-id="920e3-146">Atualize o modelo de página de edição com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="920e3-146">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="920e3-147">As alterações são semelhantes às feitas no modelo de página de criação.</span><span class="sxs-lookup"><span data-stu-id="920e3-147">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="920e3-148">No código anterior, `PopulateDepartmentsDropDownList` aprovado na ID de departamento, que selecione o departamento especificado na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="920e3-148">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="920e3-149">Atualização *Pages/Courses/Edit.cshtml* com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="920e3-149">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="920e3-150">A marcação anterior faz as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="920e3-150">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="920e3-151">Exibe a ID do curso.</span><span class="sxs-lookup"><span data-stu-id="920e3-151">Displays the course ID.</span></span> <span data-ttu-id="920e3-152">Geralmente a chave primária (PK) de uma entidade não é exibida.</span><span class="sxs-lookup"><span data-stu-id="920e3-152">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="920e3-153">PKs são geralmente muito sentidos para os usuários.</span><span class="sxs-lookup"><span data-stu-id="920e3-153">PKs are usually meaningless to users.</span></span> <span data-ttu-id="920e3-154">Nesse caso, a CP é o número de curso.</span><span class="sxs-lookup"><span data-stu-id="920e3-154">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="920e3-155">Altera a legenda do **DepartmentID** para **departamento**.</span><span class="sxs-lookup"><span data-stu-id="920e3-155">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="920e3-156">Substitui `"ViewBag.DepartmentID"` com `DepartmentNameSL` (da classe base).</span><span class="sxs-lookup"><span data-stu-id="920e3-156">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="920e3-157">Adiciona a opção "Selecione departamento".</span><span class="sxs-lookup"><span data-stu-id="920e3-157">Adds the "Select Department" option.</span></span> <span data-ttu-id="920e3-158">Essa alteração renderiza "Selecione departamento" em vez do departamento primeiro.</span><span class="sxs-lookup"><span data-stu-id="920e3-158">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="920e3-159">Adiciona uma mensagem de validação quando o departamento não está selecionado.</span><span class="sxs-lookup"><span data-stu-id="920e3-159">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="920e3-160">A página contém um campo oculto (`<input type="hidden">`) para o número de curso.</span><span class="sxs-lookup"><span data-stu-id="920e3-160">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="920e3-161">Adicionando um `<label>` marca auxiliar com `asp-for="Course.CourseID"` não elimina a necessidade para o campo oculto.</span><span class="sxs-lookup"><span data-stu-id="920e3-161">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="920e3-162">`<input type="hidden">`é necessário para o número a ser incluído nos dados de postagem quando o usuário clica **salvar**.</span><span class="sxs-lookup"><span data-stu-id="920e3-162">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="920e3-163">Teste o código atualizado.</span><span class="sxs-lookup"><span data-stu-id="920e3-163">Test the updated code.</span></span> <span data-ttu-id="920e3-164">Criar, editar e excluir um curso.</span><span class="sxs-lookup"><span data-stu-id="920e3-164">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="920e3-165">Adicionar AsNoTracking os detalhes e excluir modelos de página</span><span class="sxs-lookup"><span data-stu-id="920e3-165">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="920e3-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) pode melhorar o desempenho quando o controle não é necessário.</span><span class="sxs-lookup"><span data-stu-id="920e3-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="920e3-167">Adicionar `AsNoTracking` para o modelo de página de detalhes e excluir.</span><span class="sxs-lookup"><span data-stu-id="920e3-167">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="920e3-168">O código a seguir mostra o modelo de página Excluir atualizado:</span><span class="sxs-lookup"><span data-stu-id="920e3-168">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="920e3-169">Atualização de `OnGetAsync` método o *Pages/Courses/Details.cshtml.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="920e3-169">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="920e3-170">Modifique as páginas de exclusão e detalhes</span><span class="sxs-lookup"><span data-stu-id="920e3-170">Modify the Delete and Details pages</span></span>

<span data-ttu-id="920e3-171">Atualize a página Excluir Razor com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="920e3-171">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="920e3-172">Fazer as mesmas alterações para a página de detalhes.</span><span class="sxs-lookup"><span data-stu-id="920e3-172">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="920e3-173">Testar as páginas de curso</span><span class="sxs-lookup"><span data-stu-id="920e3-173">Test the Course pages</span></span>

<span data-ttu-id="920e3-174">Teste de criar, editar, detalhes e excluir.</span><span class="sxs-lookup"><span data-stu-id="920e3-174">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="920e3-175">Atualizar as páginas do instrutor</span><span class="sxs-lookup"><span data-stu-id="920e3-175">Update the instructor pages</span></span>

<span data-ttu-id="920e3-176">As seções a seguir atualiza as páginas do instrutor.</span><span class="sxs-lookup"><span data-stu-id="920e3-176">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="920e3-177">Adicionar local do escritório</span><span class="sxs-lookup"><span data-stu-id="920e3-177">Add office location</span></span>

<span data-ttu-id="920e3-178">Ao editar um registro de instrutor, convém atualizar a atribuição do office do instrutor.</span><span class="sxs-lookup"><span data-stu-id="920e3-178">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="920e3-179">O `Instructor` entidade tem uma relação um-para-zero-ou-um com o `OfficeAssignment` entidade.</span><span class="sxs-lookup"><span data-stu-id="920e3-179">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="920e3-180">O código do instrutor deve tratar:</span><span class="sxs-lookup"><span data-stu-id="920e3-180">The instructor code must handle:</span></span>

* <span data-ttu-id="920e3-181">Se o usuário limpar a atribuição do office, exclua o `OfficeAssignment` entidade.</span><span class="sxs-lookup"><span data-stu-id="920e3-181">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="920e3-182">Se o usuário insere uma atribuição de escritório e estava vazio, crie um novo `OfficeAssignment` entidade.</span><span class="sxs-lookup"><span data-stu-id="920e3-182">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="920e3-183">Se o usuário altera a atribuição do office, atualize o `OfficeAssignment` entidade.</span><span class="sxs-lookup"><span data-stu-id="920e3-183">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="920e3-184">Atualize o modelo de página de edição de instrutores com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="920e3-184">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="920e3-185">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="920e3-185">The preceding code:</span></span>

- <span data-ttu-id="920e3-186">Obtém a atual `Instructor` entidade do banco de dados usando o carregamento rápido para o `OfficeAssignment` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="920e3-186">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="920e3-187">Atualiza recuperada `Instructor` entidade com valores de associador de modelo.</span><span class="sxs-lookup"><span data-stu-id="920e3-187">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="920e3-188">`TryUpdateModel`impede que [mais](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="920e3-188">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="920e3-189">Define se o local do escritório estiver em branco, `Instructor.OfficeAssignment` como null.</span><span class="sxs-lookup"><span data-stu-id="920e3-189">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="920e3-190">Quando `Instructor.OfficeAssignment` é null, a linha relacionada a `OfficeAssignment` tabela for excluída.</span><span class="sxs-lookup"><span data-stu-id="920e3-190">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="920e3-191">Atualizar a página de edição do instrutor</span><span class="sxs-lookup"><span data-stu-id="920e3-191">Update the instructor Edit page</span></span>

<span data-ttu-id="920e3-192">Atualização *Pages/Instructors/Edit.cshtml* com o local do escritório:</span><span class="sxs-lookup"><span data-stu-id="920e3-192">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="920e3-193">Verifique se que você pode alterar um escritório de professores.</span><span class="sxs-lookup"><span data-stu-id="920e3-193">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="920e3-194">Adicionar atribuições de curso para a página de edição do instrutor</span><span class="sxs-lookup"><span data-stu-id="920e3-194">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="920e3-195">Instrutores podem ensinar qualquer número de cursos.</span><span class="sxs-lookup"><span data-stu-id="920e3-195">Instructors may teach any number of courses.</span></span> <span data-ttu-id="920e3-196">Nesta seção, você adiciona a capacidade de alterar as atribuições de curso.</span><span class="sxs-lookup"><span data-stu-id="920e3-196">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="920e3-197">A imagem a seguir mostra o instrutor atualizado Editar página:</span><span class="sxs-lookup"><span data-stu-id="920e3-197">The following image shows the updated instructor Edit page:</span></span>

![Página Editar instrutor de cursos](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="920e3-199">`Course`e `Instructor` tem uma relação muitos-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="920e3-199">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="920e3-200">Para adicionar e remover relações, você pode adicionar e remover entidades de `CourseAssignments` unir o conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="920e3-200">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="920e3-201">Caixas de seleção Permitir alterações para cursos a que instrutor é atribuído.</span><span class="sxs-lookup"><span data-stu-id="920e3-201">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="920e3-202">É exibida uma caixa de seleção para cada curso no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="920e3-202">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="920e3-203">Cursos atribuído para o instrutor são verificados.</span><span class="sxs-lookup"><span data-stu-id="920e3-203">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="920e3-204">O usuário pode selecionar ou desmarcar as caixas de seleção para alterar as atribuições de curso.</span><span class="sxs-lookup"><span data-stu-id="920e3-204">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="920e3-205">Se o número de cursos eram muito maior:</span><span class="sxs-lookup"><span data-stu-id="920e3-205">If the number of courses were much greater:</span></span>

* <span data-ttu-id="920e3-206">Você provavelmente usa uma interface de usuário diferente para exibir os cursos.</span><span class="sxs-lookup"><span data-stu-id="920e3-206">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="920e3-207">Não altere o método de manipulação de uma entidade de associação para criar ou excluir relações.</span><span class="sxs-lookup"><span data-stu-id="920e3-207">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="920e3-208">Adicionar classes para dar suporte a criar e editar páginas instrutor</span><span class="sxs-lookup"><span data-stu-id="920e3-208">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="920e3-209">Criar *SchoolViewModels/AssignedCourseData.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="920e3-209">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="920e3-210">O `AssignedCourseData` classe contém dados para criar as caixas de seleção para cursos atribuídos pelo instrutor.</span><span class="sxs-lookup"><span data-stu-id="920e3-210">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="920e3-211">Criar o *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* classe base:</span><span class="sxs-lookup"><span data-stu-id="920e3-211">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="920e3-212">O `InstructorCoursesPageModel` é a classe base que será usado para editar e criar modelos de página.</span><span class="sxs-lookup"><span data-stu-id="920e3-212">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="920e3-213">`PopulateAssignedCourseData`lê todos `Course` entidades para preencher `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="920e3-213">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="920e3-214">Para cada curso, o código define o `CourseID`, título e se deve ou não o instrutor é atribuído ao curso.</span><span class="sxs-lookup"><span data-stu-id="920e3-214">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="920e3-215">Um [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) é usado para criar pesquisas eficientes.</span><span class="sxs-lookup"><span data-stu-id="920e3-215">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="920e3-216">Modelo de página de edição de instrutores</span><span class="sxs-lookup"><span data-stu-id="920e3-216">Instructors Edit page model</span></span>

<span data-ttu-id="920e3-217">Atualize o modelo de página de edição do instrutor com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="920e3-217">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="920e3-218">O código anterior gerencia alterações de atribuição do office.</span><span class="sxs-lookup"><span data-stu-id="920e3-218">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="920e3-219">Atualize o modo de exibição Razor instrutor:</span><span class="sxs-lookup"><span data-stu-id="920e3-219">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="920e3-220">Quando você colar o código no Visual Studio, quebras de linha são alteradas de maneira que quebre o código.</span><span class="sxs-lookup"><span data-stu-id="920e3-220">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="920e3-221">Pressione Ctrl + Z uma vez para desfazer a formatação automática.</span><span class="sxs-lookup"><span data-stu-id="920e3-221">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="920e3-222">CTRL + Z corrige as quebras de linha, de modo que eles se parecer com o que você vê aqui.</span><span class="sxs-lookup"><span data-stu-id="920e3-222">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="920e3-223">O recuo não precisa ser perfeito, mas o `@</tr><tr>`, `@:<td>`, `@:</td>`, e `@:</tr>` linhas devem ser em uma única linha conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="920e3-223">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="920e3-224">Com o bloco de código novo selecionado, pressione Tab três vezes para alinhar o novo código com o código existente.</span><span class="sxs-lookup"><span data-stu-id="920e3-224">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="920e3-225">Vote em ou examinar o status do bug [com este link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="920e3-225">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="920e3-226">O código anterior cria uma tabela HTML que tem três colunas.</span><span class="sxs-lookup"><span data-stu-id="920e3-226">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="920e3-227">Cada coluna tem uma caixa de seleção e uma legenda que contém o número e título.</span><span class="sxs-lookup"><span data-stu-id="920e3-227">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="920e3-228">Todas as caixas de seleção tem o mesmo nome ("selectedCourses").</span><span class="sxs-lookup"><span data-stu-id="920e3-228">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="920e3-229">Usando o mesmo nome informa o associador de modelo para tratá-los como um grupo.</span><span class="sxs-lookup"><span data-stu-id="920e3-229">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="920e3-230">O atributo de valor de cada caixa de seleção é definido como `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="920e3-230">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="920e3-231">Quando a página é enviada, o associador de modelo passa uma matriz que consiste o `CourseID` valores para apenas as caixas de seleção estão selecionadas.</span><span class="sxs-lookup"><span data-stu-id="920e3-231">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="920e3-232">Quando as caixas de seleção são inicialmente renderizadas, cursos atribuídos para o instrutor verificou atributos.</span><span class="sxs-lookup"><span data-stu-id="920e3-232">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="920e3-233">Execute o aplicativo e testar a página de edição instrutores atualizado.</span><span class="sxs-lookup"><span data-stu-id="920e3-233">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="920e3-234">Altere algumas atribuições de curso.</span><span class="sxs-lookup"><span data-stu-id="920e3-234">Change some course assignments.</span></span> <span data-ttu-id="920e3-235">As alterações são refletidas na página de índice.</span><span class="sxs-lookup"><span data-stu-id="920e3-235">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="920e3-236">Observação: A abordagem usada aqui para editar dados de curso instrutor funciona bem quando há um número limitado de cursos.</span><span class="sxs-lookup"><span data-stu-id="920e3-236">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="920e3-237">Para coleções que são muito maiores, uma interface de usuário diferente e um método de atualização diferente pode ser mais útil e eficiente.</span><span class="sxs-lookup"><span data-stu-id="920e3-237">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="920e3-238">Atualizar a página de criação de instrutores</span><span class="sxs-lookup"><span data-stu-id="920e3-238">Update the instructors Create page</span></span>

<span data-ttu-id="920e3-239">Atualize o modelo de página de criação de instrutor com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="920e3-239">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="920e3-240">O código anterior é semelhante do *Pages/Instructors/Edit.cshtml.cs* código.</span><span class="sxs-lookup"><span data-stu-id="920e3-240">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="920e3-241">Atualize a página de criar Razor do instrutor com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="920e3-241">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="920e3-242">Teste a página de criação do instrutor.</span><span class="sxs-lookup"><span data-stu-id="920e3-242">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="920e3-243">Atualizar a página de exclusão</span><span class="sxs-lookup"><span data-stu-id="920e3-243">Update the Delete page</span></span>

<span data-ttu-id="920e3-244">Atualize o modelo de página de exclusão com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="920e3-244">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="920e3-245">O código anterior faz as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="920e3-245">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="920e3-246">Usa o carregamento rápido para o `CourseAssignments` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="920e3-246">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="920e3-247">`CourseAssignments`deve ser incluído ou eles não são excluídos quando o instrutor é excluído.</span><span class="sxs-lookup"><span data-stu-id="920e3-247">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="920e3-248">Para evitar a necessidade de lê-los, configure a exclusão em cascata no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="920e3-248">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="920e3-249">Se o instrutor a ser excluído é atribuído como administrador de todos os departamentos, remove a atribuição de instrutor dos departamentos.</span><span class="sxs-lookup"><span data-stu-id="920e3-249">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="920e3-250">[Anterior](xref:data/ef-rp/read-related-data)
[Próximo](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="920e3-250">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
