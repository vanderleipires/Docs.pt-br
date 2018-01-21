---
title: "Núcleo do ASP.NET MVC com núcleo EF - atualização relacionadas a dados - 7 de 10"
author: tdykstra
description: "Neste tutorial atualizará os dados relacionados ao atualizar os campos de chave estrangeira e propriedades de navegação."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 0e4df407a1ca15aa5baa2b7226be1cf91902a583
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a><span data-ttu-id="450fe-103">Atualizando dados relacionados - Core de EF com o tutorial do MVC do ASP.NET Core (7 de 10)</span><span class="sxs-lookup"><span data-stu-id="450fe-103">Updating related data - EF Core with ASP.NET Core MVC tutorial (7 of 10)</span></span>

<span data-ttu-id="450fe-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="450fe-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="450fe-105">O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="450fe-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="450fe-106">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="450fe-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="450fe-107">No tutorial anterior, você exibiu dados relacionados. Neste tutorial atualizará os dados relacionados ao atualizar os campos de chave estrangeira e propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="450fe-107">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="450fe-108">As ilustrações a seguir mostram algumas das páginas que você trabalhará com.</span><span class="sxs-lookup"><span data-stu-id="450fe-108">The following illustrations show some of the pages that you'll work with.</span></span>

![Página de edição de curso](update-related-data/_static/course-edit.png)

![Página de edição do instrutor](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="450fe-111">Personalizar a criar e editar páginas de cursos</span><span class="sxs-lookup"><span data-stu-id="450fe-111">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="450fe-112">Quando uma nova entidade de curso é criada, ele deve ter uma relação de um departamento existente.</span><span class="sxs-lookup"><span data-stu-id="450fe-112">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="450fe-113">Para facilitar isso, o código de scaffolding inclui os métodos do controlador e criar e editar modos de exibição que incluem uma lista suspensa para selecionar o departamento.</span><span class="sxs-lookup"><span data-stu-id="450fe-113">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="450fe-114">Define a lista suspensa de `Course.DepartmentID` propriedade de chave estrangeira, e isso é tudo o que o Entity Framework precisa para carregar o `Department` propriedade de navegação com a entidade de departamento apropriada.</span><span class="sxs-lookup"><span data-stu-id="450fe-114">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="450fe-115">Você vai usar o código de scaffolding, mas altere ligeiramente para adicionar o tratamento de erros e classificar a lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="450fe-115">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="450fe-116">Em *CoursesController.cs*, exclua os quatro métodos de criar e editar e substituí-los com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="450fe-116">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="450fe-117">Após o `Edit` método HttpPost, criar um novo método que carrega informações de departamento para obter a lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="450fe-117">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="450fe-118">O `PopulateDepartmentsDropDownList` método obtém uma lista de todos os departamentos classificados por nome, cria um `SelectList` coleção para uma lista suspensa e passa a coleção para o modo de exibição `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="450fe-118">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="450fe-119">O método aceita opcional `selectedDepartment` parâmetro que permite que o código de chamada especificar o item que será selecionado quando a lista suspensa é renderizada.</span><span class="sxs-lookup"><span data-stu-id="450fe-119">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="450fe-120">O modo de exibição passará o nome "DepartmentID" para o `<select>` auxiliar de marca e o auxiliar sabe para examinar o `ViewBag` de objeto para um `SelectList` denominado "DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="450fe-120">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="450fe-121">O HttpGet `Create` chamadas de método de `PopulateDepartmentsDropDownList` método sem definir o item selecionado, como para um novo curso o departamento não for estabelecido ainda:</span><span class="sxs-lookup"><span data-stu-id="450fe-121">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="450fe-122">O HttpGet `Edit` método define o item selecionado, com base na identificação do departamento que já está atribuído ao curso sendo editado:</span><span class="sxs-lookup"><span data-stu-id="450fe-122">The HttpGet `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="450fe-123">Os métodos de HttpPost para ambos `Create` e `Edit` também incluir o código que define o item selecionado quando eles exibir novamente a página após um erro.</span><span class="sxs-lookup"><span data-stu-id="450fe-123">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="450fe-124">Isso garante que quando a página é reexibida para mostrar a mensagem de erro, qualquer departamento foi selecionado permanece selecionado.</span><span class="sxs-lookup"><span data-stu-id="450fe-124">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="450fe-125">Adicione. AsNoTracking para obter detalhes e excluir métodos</span><span class="sxs-lookup"><span data-stu-id="450fe-125">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="450fe-126">Para otimizar o desempenho dos detalhes do curso e excluir páginas, adicionar `AsNoTracking` chamadas no `Details` e HttpGet `Delete` métodos.</span><span class="sxs-lookup"><span data-stu-id="450fe-126">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="450fe-127">Modificar os modos de curso</span><span class="sxs-lookup"><span data-stu-id="450fe-127">Modify the Course views</span></span>

<span data-ttu-id="450fe-128">Em *Views/Courses/Create.cshtml*, adicione uma opção "Selecione departamento" para o **departamento** suspensa lista, altere a legenda do **DepartmentID** para  **Departamento**e adicionar uma mensagem de validação.</span><span class="sxs-lookup"><span data-stu-id="450fe-128">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="450fe-129">Em *Views/Courses/Edit.cshtml*, fazer a mesma alteração para o campo de departamento que você acabou de fazer no *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="450fe-129">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="450fe-130">Também na *Views/Courses/Edit.cshtml*, adicionar um campo de número de curso antes do **título** campo.</span><span class="sxs-lookup"><span data-stu-id="450fe-130">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="450fe-131">Como o número é a chave primária, ele é exibido, mas ele não pode ser alterado.</span><span class="sxs-lookup"><span data-stu-id="450fe-131">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="450fe-132">Já existe um campo oculto (`<input type="hidden">`) para o número de curso no modo de edição.</span><span class="sxs-lookup"><span data-stu-id="450fe-132">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="450fe-133">Adicionando um `<label>` auxiliar de marca não elimina a necessidade de campo oculto porque ele não faz o número a ser incluído nos dados de postagem quando o usuário clica **salvar** no **editar** página.</span><span class="sxs-lookup"><span data-stu-id="450fe-133">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="450fe-134">Em *Views/Courses/Delete.cshtml*, adicione um campo de número de curso na parte superior e alterar a ID de departamento para o nome do departamento.</span><span class="sxs-lookup"><span data-stu-id="450fe-134">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="450fe-135">Em *Views/Courses/Details.cshtml*, fazer a mesma alteração que você acabou de fazer para *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="450fe-135">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="450fe-136">Testar as páginas de curso</span><span class="sxs-lookup"><span data-stu-id="450fe-136">Test the Course pages</span></span>

<span data-ttu-id="450fe-137">Executar o aplicativo, selecione o **cursos** , clique em **criar novo**e inserir dados para um novo curso:</span><span class="sxs-lookup"><span data-stu-id="450fe-137">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Página de criação de curso](update-related-data/_static/course-create.png)

<span data-ttu-id="450fe-139">Clique em **Criar**.</span><span class="sxs-lookup"><span data-stu-id="450fe-139">Click **Create**.</span></span> <span data-ttu-id="450fe-140">A página de índice de cursos é exibida com o curso novo adicionado à lista.</span><span class="sxs-lookup"><span data-stu-id="450fe-140">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="450fe-141">O nome do departamento na lista de páginas de índice é obtido da propriedade de navegação, mostrando que a relação foi estabelecida corretamente.</span><span class="sxs-lookup"><span data-stu-id="450fe-141">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="450fe-142">Clique em **editar** em um curso na página de índice de cursos.</span><span class="sxs-lookup"><span data-stu-id="450fe-142">Click **Edit** on a course in the Courses Index page.</span></span>

![Página de edição de curso](update-related-data/_static/course-edit.png)

<span data-ttu-id="450fe-144">Alterar dados na página e clique em **salvar**.</span><span class="sxs-lookup"><span data-stu-id="450fe-144">Change data on the page and click **Save**.</span></span> <span data-ttu-id="450fe-145">A página de índice de cursos é exibida com os dados de curso atualizado.</span><span class="sxs-lookup"><span data-stu-id="450fe-145">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="450fe-146">Adicionar uma página de edição para instrutores</span><span class="sxs-lookup"><span data-stu-id="450fe-146">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="450fe-147">Quando você editar um registro de instrutor, você deseja ser capaz de atualizar a atribuição do office do instrutor.</span><span class="sxs-lookup"><span data-stu-id="450fe-147">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="450fe-148">A entidade do instrutor tem uma relação um-para-zero-ou-um com a entidade OfficeAssignment, o que significa que seu código deve manipular as seguintes situações:</span><span class="sxs-lookup"><span data-stu-id="450fe-148">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="450fe-149">Se o usuário apaga a atribuição do office e que originalmente tinha um valor, exclua a entidade OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="450fe-149">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="450fe-150">Se o usuário insere um valor de atribuição do office e originalmente estava vazio, crie uma nova entidade OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="450fe-150">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="450fe-151">Se o usuário altera o valor de uma atribuição de office, altere o valor em uma entidade OfficeAssignment existente.</span><span class="sxs-lookup"><span data-stu-id="450fe-151">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="450fe-152">Atualize o controlador instrutores</span><span class="sxs-lookup"><span data-stu-id="450fe-152">Update the Instructors controller</span></span>

<span data-ttu-id="450fe-153">Em *InstructorsController.cs*, altere o código no HttpGet `Edit` método para que ele carrega a entidade de instrutor `OfficeAssignment` propriedade de navegação e chamadas `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="450fe-153">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

<span data-ttu-id="450fe-154">Substitua o HttpPost `Edit` método com o seguinte código para manipular atualizações de atribuição do office:</span><span class="sxs-lookup"><span data-stu-id="450fe-154">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="450fe-155">O código faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="450fe-155">The code does the following:</span></span>

-  <span data-ttu-id="450fe-156">Altera o nome do método para `EditPost` porque a assinatura agora é o mesmo que o HttpGet `Edit` método (o `ActionName` atributo especifica que o `/Edit/` URL ainda é usada).</span><span class="sxs-lookup"><span data-stu-id="450fe-156">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="450fe-157">Obtém a entidade atual do instrutor do banco de dados usando para o carregamento rápido de `OfficeAssignment` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="450fe-157">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="450fe-158">Isso é o mesmo que você fez o HttpGet `Edit` método.</span><span class="sxs-lookup"><span data-stu-id="450fe-158">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="450fe-159">Atualiza a entidade recuperada do instrutor com valores de associador de modelo.</span><span class="sxs-lookup"><span data-stu-id="450fe-159">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="450fe-160">O `TryUpdateModel` sobrecarga permite que você adicionar à lista branca as propriedades que você deseja incluir.</span><span class="sxs-lookup"><span data-stu-id="450fe-160">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="450fe-161">Isso impede o excesso de lançamento, conforme explicado no [segundo tutorial](crud.md).</span><span class="sxs-lookup"><span data-stu-id="450fe-161">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   <span data-ttu-id="450fe-162">Se o local do escritório estiver em branco, define a propriedade de Instructor.OfficeAssignment como nulo para que a linha na tabela OfficeAssignment será excluída.</span><span class="sxs-lookup"><span data-stu-id="450fe-162">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="450fe-163">Salva as alterações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="450fe-163">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="450fe-164">Atualizar a exibição instrutor editar</span><span class="sxs-lookup"><span data-stu-id="450fe-164">Update the Instructor Edit view</span></span>

<span data-ttu-id="450fe-165">Em *Views/Instructors/Edit.cshtml*, adicionar um novo campo para editar o local do escritório, no final antes do **salvar** botão:</span><span class="sxs-lookup"><span data-stu-id="450fe-165">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="450fe-166">Executar o aplicativo, selecione o **instrutores** guia e, em seguida, clique em **editar** em instrutor.</span><span class="sxs-lookup"><span data-stu-id="450fe-166">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="450fe-167">Alterar o **escritório** e clique em **salvar**.</span><span class="sxs-lookup"><span data-stu-id="450fe-167">Change the **Office Location** and click **Save**.</span></span>

![Página de edição do instrutor](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="450fe-169">Adicionar atribuições de curso para a página Editar instrutor</span><span class="sxs-lookup"><span data-stu-id="450fe-169">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="450fe-170">Instrutores podem ensinar qualquer número de cursos.</span><span class="sxs-lookup"><span data-stu-id="450fe-170">Instructors may teach any number of courses.</span></span> <span data-ttu-id="450fe-171">Agora você vai aprimorar a página Editar instrutor adicionando a capacidade de alterar as atribuições de curso usando um grupo de caixas de seleção, conforme mostrado na captura de tela a seguir:</span><span class="sxs-lookup"><span data-stu-id="450fe-171">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Página Editar instrutor de cursos](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="450fe-173">A relação entre as entidades do curso e instrutor é muitos-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="450fe-173">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="450fe-174">Para adicionar e remover relações, você adicionar e remover entidades de e para o conjunto de entidades de junção CourseAssignments.</span><span class="sxs-lookup"><span data-stu-id="450fe-174">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="450fe-175">A interface do usuário que permite que você altere os cursos instrutor é atribuído a é um grupo de caixas de seleção.</span><span class="sxs-lookup"><span data-stu-id="450fe-175">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="450fe-176">É exibida uma caixa de seleção para cada curso no banco de dados, e aqueles que o instrutor atribuído atualmente à são selecionados.</span><span class="sxs-lookup"><span data-stu-id="450fe-176">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="450fe-177">O usuário pode selecionar ou desmarcar as caixas de seleção para alterar as atribuições de curso.</span><span class="sxs-lookup"><span data-stu-id="450fe-177">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="450fe-178">Se o número de cursos muito maior, você provavelmente desejará usar um método diferente de apresentar os dados no modo de exibição, mas você usaria o mesmo método de manipulação de uma entidade de associação para criar ou excluir relações.</span><span class="sxs-lookup"><span data-stu-id="450fe-178">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="450fe-179">Atualize o controlador instrutores</span><span class="sxs-lookup"><span data-stu-id="450fe-179">Update the Instructors controller</span></span>

<span data-ttu-id="450fe-180">Para fornecer dados para o modo de exibição de lista de caixas de seleção, você usará uma classe de modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="450fe-180">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="450fe-181">Criar *AssignedCourseData.cs* no *SchoolViewModels* pasta e substitua o código existente pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="450fe-181">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="450fe-182">Em *InstructorsController.cs*, substitua o HttpGet `Edit` método com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="450fe-182">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="450fe-183">As alterações são realçadas.</span><span class="sxs-lookup"><span data-stu-id="450fe-183">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="450fe-184">O código adiciona o carregamento rápido para o `Courses` propriedade de navegação e chama o novo `PopulateAssignedCourseData` método para fornecer informações para a matriz de caixa de seleção usando o `AssignedCourseData` exibir classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="450fe-184">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="450fe-185">O código de `PopulateAssignedCourseData` método lê todas as entidades de curso para carregar uma lista de cursos usando a classe de modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="450fe-185">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="450fe-186">Para cada curso, o código verifica se o curso existe do instrutor `Courses` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="450fe-186">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="450fe-187">Para criar pesquisa eficiente ao verificar se um curso é atribuído para o instrutor, os cursos atribuídos para o instrutor são colocados em um `HashSet` coleção.</span><span class="sxs-lookup"><span data-stu-id="450fe-187">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="450fe-188">O `Assigned` está definida como true para cursos instrutor é atribuído a.</span><span class="sxs-lookup"><span data-stu-id="450fe-188">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="450fe-189">O modo de exibição usará essa propriedade para determinar qual seleção caixas devem ser exibidas como selecionado.</span><span class="sxs-lookup"><span data-stu-id="450fe-189">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="450fe-190">Por fim, a lista é passada para o modo de exibição `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="450fe-190">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="450fe-191">Em seguida, adicione o código que é executado quando o usuário clica **salvar**.</span><span class="sxs-lookup"><span data-stu-id="450fe-191">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="450fe-192">Substitua o `EditPost` método com o seguinte código e adicionar um novo método que atualiza o `Courses` propriedade de navegação da entidade do instrutor.</span><span class="sxs-lookup"><span data-stu-id="450fe-192">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="450fe-193">A assinatura do método agora é diferente do HttpGet `Edit` método, para que o nome do método é alterado de `EditPost` para `Edit`.</span><span class="sxs-lookup"><span data-stu-id="450fe-193">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="450fe-194">Como o modo de exibição não tem uma coleção de entidades de curso, o associador de modelo não é possível atualizar automaticamente o `CourseAssignments` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="450fe-194">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="450fe-195">Em vez de usar o associador de modelo para atualizar o `CourseAssignments` propriedade de navegação, você pode fazer isso no novo `UpdateInstructorCourses` método.</span><span class="sxs-lookup"><span data-stu-id="450fe-195">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="450fe-196">Portanto, você precisa excluir o `CourseAssignments` propriedade de associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="450fe-196">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="450fe-197">Isso não requer nenhuma alteração ao código que chama `TryUpdateModel` porque você está usando a sobrecarga de lista branca e `CourseAssignments` não estiver na lista de inclusão.</span><span class="sxs-lookup"><span data-stu-id="450fe-197">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="450fe-198">Se nenhuma seleção caixas tiverem sido selecionadas, o código em `UpdateInstructorCourses` inicializa o `CourseAssignments` propriedade de navegação com uma coleção vazia e retorna:</span><span class="sxs-lookup"><span data-stu-id="450fe-198">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="450fe-199">O código, em seguida, executa um loop em todos os cursos no banco de dados e verifica cada curso contra os atualmente atribuídos ao instrutor em relação àqueles que foram selecionados no modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="450fe-199">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="450fe-200">Para facilitar a pesquisas eficientes, este último duas coleções são armazenadas em `HashSet` objetos.</span><span class="sxs-lookup"><span data-stu-id="450fe-200">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="450fe-201">Se a caixa de seleção para um curso foi selecionada, mas o curso não está no `Instructor.CourseAssignments` propriedade de navegação, o curso é adicionado à coleção na propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="450fe-201">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="450fe-202">Se a caixa de seleção para um curso não foi selecionada, mas o curso no `Instructor.CourseAssignments` propriedade de navegação, o curso é removida da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="450fe-202">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="450fe-203">Atualizar os modos de exibição do instrutor</span><span class="sxs-lookup"><span data-stu-id="450fe-203">Update the Instructor views</span></span>

<span data-ttu-id="450fe-204">Em *Views/Instructors/Edit.cshtml*, adicionar um **cursos** campo com uma matriz de caixas de seleção, adicionando o seguinte código imediatamente após o `div` elementos para o **Office**  campo e antes do `div` elemento para o **salvar** botão.</span><span class="sxs-lookup"><span data-stu-id="450fe-204">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE] 
> <span data-ttu-id="450fe-205">Quando você colar o código no Visual Studio, quebras de linha serão alteradas de forma que interrompe o código.</span><span class="sxs-lookup"><span data-stu-id="450fe-205">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="450fe-206">Pressione Ctrl + Z uma vez para desfazer a formatação automática.</span><span class="sxs-lookup"><span data-stu-id="450fe-206">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="450fe-207">Isso será corrigido as quebras de linha para que eles se parecer com o que você vê aqui.</span><span class="sxs-lookup"><span data-stu-id="450fe-207">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="450fe-208">O recuo não precisa ser perfeito, mas o `@</tr><tr>`, `@:<td>`, `@:</td>`, e `@:</tr>` linhas devem ser em uma única linha conforme mostrado, ou você receberá um erro de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="450fe-208">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="450fe-209">Com o bloco de código novo selecionado, pressione Tab três vezes para alinhar o novo código com o código existente.</span><span class="sxs-lookup"><span data-stu-id="450fe-209">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="450fe-210">Você pode verificar o status desse problema [aqui](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="450fe-210">You can check the status of this problem [here](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="450fe-211">Esse código cria uma tabela HTML que tem três colunas.</span><span class="sxs-lookup"><span data-stu-id="450fe-211">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="450fe-212">Em cada coluna é uma caixa de seleção, seguida de uma legenda que consiste em número e o título.</span><span class="sxs-lookup"><span data-stu-id="450fe-212">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="450fe-213">Todas as caixas de seleção tem o mesmo nome ("selectedCourses"), que informa o associador de modelo devem ser tratados como um grupo.</span><span class="sxs-lookup"><span data-stu-id="450fe-213">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="450fe-214">O atributo de valor de cada caixa de seleção é definido como o valor de `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="450fe-214">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="450fe-215">Quando a página é enviada, o associador de modelo passa uma matriz para o controlador que consiste o `CourseID` valores para apenas as caixas de seleção que estão selecionados.</span><span class="sxs-lookup"><span data-stu-id="450fe-215">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="450fe-216">Quando as caixas de seleção são inicialmente renderizadas, aqueles que estão atribuídos ao instrutor de cursos verificou atributos, que seleciona-los (exibe-os check).</span><span class="sxs-lookup"><span data-stu-id="450fe-216">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="450fe-217">Executar o aplicativo, selecione o **instrutores** guia e, em seguida, clique em **editar** em instrutor para ver o **editar** página.</span><span class="sxs-lookup"><span data-stu-id="450fe-217">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Página Editar instrutor de cursos](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="450fe-219">Alterar algumas atribuições do curso e clique em Salvar.</span><span class="sxs-lookup"><span data-stu-id="450fe-219">Change some course assignments and click Save.</span></span> <span data-ttu-id="450fe-220">As alterações feitas são refletidas na página de índice.</span><span class="sxs-lookup"><span data-stu-id="450fe-220">The changes you make are reflected on the Index page.</span></span>

> [!NOTE] 
> <span data-ttu-id="450fe-221">A abordagem usada aqui para editar dados de curso instrutor funciona bem quando há um número limitado de cursos.</span><span class="sxs-lookup"><span data-stu-id="450fe-221">The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="450fe-222">Para coleções que são muito maiores, uma interface de usuário diferente e um método de atualização diferente seria necessárias.</span><span class="sxs-lookup"><span data-stu-id="450fe-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="450fe-223">Atualizar a página de exclusão</span><span class="sxs-lookup"><span data-stu-id="450fe-223">Update the Delete page</span></span>

<span data-ttu-id="450fe-224">Em *InstructorsController.cs*, exclua o `DeleteConfirmed` método e inserir o código a seguir em seu lugar.</span><span class="sxs-lookup"><span data-stu-id="450fe-224">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="450fe-225">Este código faz as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="450fe-225">This code makes the following changes:</span></span>

* <span data-ttu-id="450fe-226">Eager faz carregar para o `CourseAssignments` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="450fe-226">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="450fe-227">Você precisa incluir isso ou EF não conhece relacionados `CourseAssignment` entidades e não excluí-los.</span><span class="sxs-lookup"><span data-stu-id="450fe-227">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="450fe-228">Para evitar a necessidade de lê-los aqui você pode configurar a exclusão em cascata no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="450fe-228">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="450fe-229">Se o instrutor a ser excluído é atribuído como administrador de todos os departamentos, remove a atribuição de instrutor dos departamentos.</span><span class="sxs-lookup"><span data-stu-id="450fe-229">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="450fe-230">Adicionar local do escritório e cursos para a página criar</span><span class="sxs-lookup"><span data-stu-id="450fe-230">Add office location and courses to the Create page</span></span>

<span data-ttu-id="450fe-231">Em *InstructorsController.cs*, exclua o HttpGet e HttpPost `Create` métodos e, em seguida, adicione o seguinte código em seu lugar:</span><span class="sxs-lookup"><span data-stu-id="450fe-231">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="450fe-232">Esse código é semelhante ao que você viu o `Edit` métodos, exceto que inicialmente não cursos estão selecionados.</span><span class="sxs-lookup"><span data-stu-id="450fe-232">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="450fe-233">O HttpGet `Create` chamadas de método de `PopulateAssignedCourseData` método não porque pode haver cursos selecionados, mas na ordem para fornecer uma coleção vazia para o `foreach` loop no modo de exibição (caso contrário, o código de exibição deve lançar uma exceção de referência nula).</span><span class="sxs-lookup"><span data-stu-id="450fe-233">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="450fe-234">O HttpPost `Create` método adiciona cada curso selecionado para o `CourseAssignments` antes que ele verifica se há erros de validação e adiciona o novo instrutor ao banco de dados de propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="450fe-234">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="450fe-235">Cursos são adicionados, mesmo se houver erros de modelo para que quando houver erros de modelo (por exemplo, o usuário uma data inválida de chave) e a página é exibida novamente com uma mensagem de erro, as seleções de curso que foram feitas serão restauradas automaticamente.</span><span class="sxs-lookup"><span data-stu-id="450fe-235">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="450fe-236">Observe que para poder adicionar o cursos para o `CourseAssignments` propriedade de navegação é necessário inicializar a propriedade como uma coleção vazia:</span><span class="sxs-lookup"><span data-stu-id="450fe-236">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="450fe-237">Como alternativa para fazer isso no código do controlador, você pode fazer no modelo instrutor alterando o getter de propriedade para criar automaticamente a coleção se ele não existir, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="450fe-237">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

<span data-ttu-id="450fe-238">Se você modificar o `CourseAssignments` propriedade dessa forma, você pode remover o código de inicialização de propriedade explícita no controlador.</span><span class="sxs-lookup"><span data-stu-id="450fe-238">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="450fe-239">Em *Views/Instructor/Create.cshtml*, adicione uma caixa de texto de local de escritório e marque caixas de cursos antes do botão de envio.</span><span class="sxs-lookup"><span data-stu-id="450fe-239">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="450fe-240">Como no caso de página de edição, [corrija a formatação se o Visual Studio reformata o código quando você o colar](#notepad).</span><span class="sxs-lookup"><span data-stu-id="450fe-240">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="450fe-241">Testar o aplicativo em execução e criando um instrutor.</span><span class="sxs-lookup"><span data-stu-id="450fe-241">Test by running the app and creating an instructor.</span></span> 

## <a name="handling-transactions"></a><span data-ttu-id="450fe-242">Processamento de transações</span><span class="sxs-lookup"><span data-stu-id="450fe-242">Handling Transactions</span></span>

<span data-ttu-id="450fe-243">Conforme explicado no [tutorial CRUD](crud.md), a estrutura da entidade implementa implicitamente transações.</span><span class="sxs-lookup"><span data-stu-id="450fe-243">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="450fe-244">Para cenários em que você precisa de mais controle – por exemplo, se você quiser incluir operações feitas fora do Entity Framework em uma transação – consulte [transações](https://docs.microsoft.com/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="450fe-244">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="450fe-245">Resumo</span><span class="sxs-lookup"><span data-stu-id="450fe-245">Summary</span></span>

<span data-ttu-id="450fe-246">Agora você concluiu a introdução ao trabalho com dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="450fe-246">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="450fe-247">O seguinte tutorial, você verá como lidar com conflitos de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="450fe-247">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="450fe-248">[Anterior](read-related-data.md)
[Próximo](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="450fe-248">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>  
