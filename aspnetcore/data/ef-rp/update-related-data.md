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
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a>Atualizando dados relacionados - EF Core Razor páginas (7, 8)

Por [Tom Dykstra](https://github.com/tdykstra), e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Este tutorial demonstra como atualizar dados relacionados. Se você tiver problemas, você não conseguir resolver, baixe o [aplicativo concluído para este estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).

As ilustrações a seguir mostra algumas das páginas concluídas.

![Página de edição de curso](update-related-data/_static/course-edit.png)
![instrutor Editar página](update-related-data/_static/instructor-edit-courses.png)

Examine e testar as páginas de curso criar e editar. Crie um novo curso. O departamento é selecionado por sua chave primária (um inteiro), não seu nome. Edite o curso de novo. Quando você concluir o teste, exclua o curso de novo.

## <a name="create-a-base-class-to-share-common-code"></a>Criar uma classe base para compartilhar código comum

As páginas criar/cursos e cursos/editar cada precisam de uma lista de nomes de departamento. Criar o *Pages/Courses/DepartmentNamePageModel.cshtml.cs* a classe base para criar e editar páginas:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

O código anterior cria um [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) para conter a lista de nomes de departamento. Se `selectedDepartment` for especificado, esse departamento for selecionado no `SelectList`.

As classes de modelo de página de criar e editar serão derivado `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Personalizar as páginas de cursos

Quando uma nova entidade de curso é criada, ele deve ter uma relação de um departamento existente. Para adicionar um departamento durante a criação de um curso, a classe base para criar e editar contém uma lista suspensa para selecionar o departamento. Define a lista suspensa de `Course.DepartmentID` propriedade de chave estrangeira (FK). Núcleo EF usa o `Course.DepartmentID` FK ao carregar o `Department` propriedade de navegação.

![Criar curso](update-related-data/_static/ddl.png)

Atualize o modelo de página de criação com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

O código anterior:

* Deriva de `DepartmentNamePageModel`.
* Usa `TryUpdateModelAsync` para evitar [mais](xref:data/ef-rp/crud#overposting).
* Substitui `ViewData["DepartmentID"]` com `DepartmentNameSL` (da classe base).

`ViewData["DepartmentID"]`é substituído com rigidez de tipos `DepartmentNameSL`. Modelos com rigidez de tipos são preferenciais em digitado sem rigidez. Para obter mais informações, consulte [digitado sem rigidez dados (ViewData e ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Atualize a página Criar cursos

Atualização *Pages/Courses/Create.cshtml* com a seguinte marcação:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

A marcação anterior faz as seguintes alterações:

* Altera a legenda do **DepartmentID** para **departamento**.
* Substitui `"ViewBag.DepartmentID"` com `DepartmentNameSL` (da classe base).
* Adiciona a opção "Selecione departamento". Essa alteração renderiza "Selecione departamento" em vez do departamento primeiro.
* Adiciona uma mensagem de validação quando o departamento não está selecionado.

A página Razor usa o [selecione auxiliar de marca](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Criar página de teste. A página Criar exibe o nome do departamento em vez da ID do departamento.

### <a name="update-the-courses-edit-page"></a>Atualize a página Editar cursos.

Atualize o modelo de página de edição com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

As alterações são semelhantes às feitas no modelo de página de criação. No código anterior, `PopulateDepartmentsDropDownList` aprovado na ID de departamento, que selecione o departamento especificado na lista suspensa.

Atualização *Pages/Courses/Edit.cshtml* com a seguinte marcação:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

A marcação anterior faz as seguintes alterações:

* Exibe a ID do curso. Geralmente a chave primária (PK) de uma entidade não é exibida. PKs são geralmente muito sentidos para os usuários. Nesse caso, a CP é o número de curso.
* Altera a legenda do **DepartmentID** para **departamento**.
* Substitui `"ViewBag.DepartmentID"` com `DepartmentNameSL` (da classe base).
* Adiciona a opção "Selecione departamento". Essa alteração renderiza "Selecione departamento" em vez do departamento primeiro.
* Adiciona uma mensagem de validação quando o departamento não está selecionado.

A página contém um campo oculto (`<input type="hidden">`) para o número de curso. Adicionando um `<label>` marca auxiliar com `asp-for="Course.CourseID"` não elimina a necessidade para o campo oculto. `<input type="hidden">`é necessário para o número a ser incluído nos dados de postagem quando o usuário clica **salvar**.

Teste o código atualizado. Criar, editar e excluir um curso.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Adicionar AsNoTracking os detalhes e excluir modelos de página

[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) pode melhorar o desempenho quando o controle não é necessário. Adicionar `AsNoTracking` para o modelo de página de detalhes e excluir. O código a seguir mostra o modelo de página Excluir atualizado:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Atualização de `OnGetAsync` método o *Pages/Courses/Details.cshtml.cs* arquivo:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Modifique as páginas de exclusão e detalhes

Atualize a página Excluir Razor com a seguinte marcação:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Fazer as mesmas alterações para a página de detalhes.

### <a name="test-the-course-pages"></a>Testar as páginas de curso

Teste de criar, editar, detalhes e excluir.

## <a name="update-the-instructor-pages"></a>Atualizar as páginas do instrutor

As seções a seguir atualiza as páginas do instrutor.

### <a name="add-office-location"></a>Adicionar local do escritório

Ao editar um registro de instrutor, convém atualizar a atribuição do office do instrutor. O `Instructor` entidade tem uma relação um-para-zero-ou-um com o `OfficeAssignment` entidade. O código do instrutor deve tratar:

* Se o usuário limpar a atribuição do office, exclua o `OfficeAssignment` entidade.
* Se o usuário insere uma atribuição de escritório e estava vazio, crie um novo `OfficeAssignment` entidade.
* Se o usuário altera a atribuição do office, atualize o `OfficeAssignment` entidade.

Atualize o modelo de página de edição de instrutores com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

O código anterior:

- Obtém a atual `Instructor` entidade do banco de dados usando o carregamento rápido para o `OfficeAssignment` propriedade de navegação.
- Atualiza recuperada `Instructor` entidade com valores de associador de modelo. `TryUpdateModel`impede que [mais](xref:data/ef-rp/crud#overposting).
- Define se o local do escritório estiver em branco, `Instructor.OfficeAssignment` como null. Quando `Instructor.OfficeAssignment` é null, a linha relacionada a `OfficeAssignment` tabela for excluída.

### <a name="update-the-instructor-edit-page"></a>Atualizar a página de edição do instrutor

Atualização *Pages/Instructors/Edit.cshtml* com o local do escritório:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Verifique se que você pode alterar um escritório de professores.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Adicionar atribuições de curso para a página de edição do instrutor

Instrutores podem ensinar qualquer número de cursos. Nesta seção, você adiciona a capacidade de alterar as atribuições de curso. A imagem a seguir mostra o instrutor atualizado Editar página:

![Página Editar instrutor de cursos](update-related-data/_static/instructor-edit-courses.png)

`Course`e `Instructor` tem uma relação muitos-para-muitos. Para adicionar e remover relações, você pode adicionar e remover entidades de `CourseAssignments` unir o conjunto de entidades.

Caixas de seleção Permitir alterações para cursos a que instrutor é atribuído. É exibida uma caixa de seleção para cada curso no banco de dados. Cursos atribuído para o instrutor são verificados. O usuário pode selecionar ou desmarcar as caixas de seleção para alterar as atribuições de curso. Se o número de cursos eram muito maior:

* Você provavelmente usa uma interface de usuário diferente para exibir os cursos.
* Não altere o método de manipulação de uma entidade de associação para criar ou excluir relações.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Adicionar classes para dar suporte a criar e editar páginas instrutor

Criar *SchoolViewModels/AssignedCourseData.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

O `AssignedCourseData` classe contém dados para criar as caixas de seleção para cursos atribuídos pelo instrutor.

Criar o *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* classe base:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

O `InstructorCoursesPageModel` é a classe base que será usado para editar e criar modelos de página. `PopulateAssignedCourseData`lê todos `Course` entidades para preencher `AssignedCourseDataList`. Para cada curso, o código define o `CourseID`, título e se deve ou não o instrutor é atribuído ao curso. Um [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) é usado para criar pesquisas eficientes.

### <a name="instructors-edit-page-model"></a>Modelo de página de edição de instrutores

Atualize o modelo de página de edição do instrutor com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

O código anterior gerencia alterações de atribuição do office.

Atualize o modo de exibição Razor instrutor:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Quando você colar o código no Visual Studio, quebras de linha são alteradas de maneira que quebre o código. Pressione Ctrl + Z uma vez para desfazer a formatação automática. CTRL + Z corrige as quebras de linha, de modo que eles se parecer com o que você vê aqui. O recuo não precisa ser perfeito, mas o `@</tr><tr>`, `@:<td>`, `@:</td>`, e `@:</tr>` linhas devem ser em uma única linha conforme mostrado. Com o bloco de código novo selecionado, pressione Tab três vezes para alinhar o novo código com o código existente. Vote em ou examinar o status do bug [com este link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

O código anterior cria uma tabela HTML que tem três colunas. Cada coluna tem uma caixa de seleção e uma legenda que contém o número e título. Todas as caixas de seleção tem o mesmo nome ("selectedCourses"). Usando o mesmo nome informa o associador de modelo para tratá-los como um grupo. O atributo de valor de cada caixa de seleção é definido como `CourseID`. Quando a página é enviada, o associador de modelo passa uma matriz que consiste o `CourseID` valores para apenas as caixas de seleção estão selecionadas.

Quando as caixas de seleção são inicialmente renderizadas, cursos atribuídos para o instrutor verificou atributos.

Execute o aplicativo e testar a página de edição instrutores atualizado. Altere algumas atribuições de curso. As alterações são refletidas na página de índice.

Observação: A abordagem usada aqui para editar dados de curso instrutor funciona bem quando há um número limitado de cursos. Para coleções que são muito maiores, uma interface de usuário diferente e um método de atualização diferente pode ser mais útil e eficiente.

### <a name="update-the-instructors-create-page"></a>Atualizar a página de criação de instrutores

Atualize o modelo de página de criação de instrutor com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

O código anterior é semelhante do *Pages/Instructors/Edit.cshtml.cs* código.

Atualize a página de criar Razor do instrutor com a seguinte marcação:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Teste a página de criação do instrutor.

## <a name="update-the-delete-page"></a>Atualizar a página de exclusão

Atualize o modelo de página de exclusão com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

O código anterior faz as seguintes alterações:

* Usa o carregamento rápido para o `CourseAssignments` propriedade de navegação. `CourseAssignments`deve ser incluído ou eles não são excluídos quando o instrutor é excluído. Para evitar a necessidade de lê-los, configure a exclusão em cascata no banco de dados.

* Se o instrutor a ser excluído é atribuído como administrador de todos os departamentos, remove a atribuição de instrutor dos departamentos.

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/read-related-data)
[Próximo](xref:data/ef-rp/concurrency)
