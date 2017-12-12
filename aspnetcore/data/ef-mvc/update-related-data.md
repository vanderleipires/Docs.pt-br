---
title: "Núcleo do ASP.NET MVC com núcleo EF - atualização relacionadas a dados - 7 de 10"
author: tdykstra
description: "Neste tutorial atualizará os dados relacionados ao atualizar os campos de chave estrangeira e propriedades de navegação."
keywords: Une ASP.NET Core, Entity Framework Core, os dados relacionados
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 67bd162b-bfb7-4750-9e7f-705228b5288c
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: b59782bccce00f3940da4ec8bcff768aff8fa4ef
ms.sourcegitcommit: ccf08615ad59bc6f654560de33b93396113a2eb0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/11/2017
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a>Atualizando dados relacionados - Core de EF com o tutorial do MVC do ASP.NET Core (7 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).

No tutorial anterior, você exibiu dados relacionados. Neste tutorial atualizará os dados relacionados ao atualizar os campos de chave estrangeira e propriedades de navegação.

As ilustrações a seguir mostram algumas das páginas que você trabalhará com.

![Página de edição de curso](update-related-data/_static/course-edit.png)

![Página de edição do instrutor](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizar a criar e editar páginas de cursos

Quando uma nova entidade de curso é criada, ele deve ter uma relação de um departamento existente. Para facilitar isso, o código de scaffolding inclui os métodos do controlador e criar e editar modos de exibição que incluem uma lista suspensa para selecionar o departamento. Define a lista suspensa de `Course.DepartmentID` propriedade de chave estrangeira, e isso é tudo o que o Entity Framework precisa para carregar o `Department` propriedade de navegação com a entidade de departamento apropriada. Você vai usar o código de scaffolding, mas altere ligeiramente para adicionar o tratamento de erros e classificar a lista suspensa.

Em *CoursesController.cs*, exclua os quatro métodos de criar e editar e substituí-los com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

Após o `Edit` método HttpPost, criar um novo método que carrega informações de departamento para obter a lista suspensa.

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

O `PopulateDepartmentsDropDownList` método obtém uma lista de todos os departamentos classificados por nome, cria um `SelectList` coleção para uma lista suspensa e passa a coleção para o modo de exibição `ViewBag`. O método aceita opcional `selectedDepartment` parâmetro que permite que o código de chamada especificar o item que será selecionado quando a lista suspensa é renderizada. O modo de exibição passará o nome "DepartmentID" para o `<select>` auxiliar de marca e o auxiliar sabe para examinar o `ViewBag` de objeto para um `SelectList` denominado "DepartmentID".

O HttpGet `Create` chamadas de método de `PopulateDepartmentsDropDownList` método sem definir o item selecionado, como para um novo curso o departamento não for estabelecido ainda:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

O HttpGet `Edit` método define o item selecionado, com base na identificação do departamento que já está atribuído ao curso sendo editado:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

Os métodos de HttpPost para ambos `Create` e `Edit` também incluir o código que define o item selecionado quando eles exibir novamente a página após um erro. Isso garante que quando a página é reexibida para mostrar a mensagem de erro, qualquer departamento foi selecionado permanece selecionado.

### <a name="add-asnotracking-to-details-and-delete-methods"></a>Adicione. AsNoTracking para obter detalhes e excluir métodos

Para otimizar o desempenho dos detalhes do curso e excluir páginas, adicionar `AsNoTracking` chamadas no `Details` e HttpGet `Delete` métodos.

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>Modificar os modos de curso

Em *Views/Courses/Create.cshtml*, adicione uma opção "Selecione departamento" para o **departamento** suspensa lista, altere a legenda do **DepartmentID** para  **Departamento**e adicionar uma mensagem de validação.

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

Em *Views/Courses/Edit.cshtml*, fazer a mesma alteração para o campo de departamento que você acabou de fazer no *Create.cshtml*.

Também na *Views/Courses/Edit.cshtml*, adicionar um campo de número de curso antes do **título** campo. Como o número é a chave primária, ele é exibido, mas ele não pode ser alterado.

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

Já existe um campo oculto (`<input type="hidden">`) para o número de curso no modo de edição. Adicionando um `<label>` auxiliar de marca não elimina a necessidade de campo oculto porque ele não faz o número a ser incluído nos dados de postagem quando o usuário clica **salvar** no **editar** página.

Em *Views/Courses/Delete.cshtml*, adicione um campo de número de curso na parte superior e alterar a ID de departamento para o nome do departamento.

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

Em *Views/Courses/Details.cshtml*, fazer a mesma alteração que você acabou de fazer para *Delete.cshtml*.

### <a name="test-the-course-pages"></a>Testar as páginas de curso

Executar o aplicativo, selecione o **cursos** , clique em **criar novo**e inserir dados para um novo curso:

![Página de criação de curso](update-related-data/_static/course-create.png)

Clique em **Criar**. A página de índice de cursos é exibida com o curso novo adicionado à lista. O nome do departamento na lista de páginas de índice é obtido da propriedade de navegação, mostrando que a relação foi estabelecida corretamente.

Clique em **editar** em um curso na página de índice de cursos.

![Página de edição de curso](update-related-data/_static/course-edit.png)

Alterar dados na página e clique em **salvar**. A página de índice de cursos é exibida com os dados de curso atualizado.

## <a name="add-an-edit-page-for-instructors"></a>Adicionar uma página de edição para instrutores

Quando você editar um registro de instrutor, você deseja ser capaz de atualizar a atribuição do office do instrutor. A entidade do instrutor tem uma relação um-para-zero-ou-um com a entidade OfficeAssignment, o que significa que seu código deve manipular as seguintes situações:

* Se o usuário apaga a atribuição do office e que originalmente tinha um valor, exclua a entidade OfficeAssignment.

* Se o usuário insere um valor de atribuição do office e originalmente estava vazio, crie uma nova entidade OfficeAssignment.

* Se o usuário altera o valor de uma atribuição de office, altere o valor em uma entidade OfficeAssignment existente.

### <a name="update-the-instructors-controller"></a>Atualize o controlador instrutores

Em *InstructorsController.cs*, altere o código no HttpGet `Edit` método para que ele carrega a entidade de instrutor `OfficeAssignment` propriedade de navegação e chamadas `AsNoTracking`:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

Substitua o HttpPost `Edit` método com o seguinte código para manipular atualizações de atribuição do office:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

O código faz o seguinte:

-  Altera o nome do método para `EditPost` porque a assinatura agora é o mesmo que o HttpGet `Edit` método (o `ActionName` atributo especifica que o `/Edit/` URL ainda é usada).

-  Obtém a entidade atual do instrutor do banco de dados usando para o carregamento rápido de `OfficeAssignment` propriedade de navegação. Isso é o mesmo que você fez o HttpGet `Edit` método.

-  Atualiza a entidade recuperada do instrutor com valores de associador de modelo. O `TryUpdateModel` sobrecarga permite que você adicionar à lista branca as propriedades que você deseja incluir. Isso impede o excesso de lançamento, conforme explicado no [segundo tutorial](crud.md).

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   Se o local do escritório estiver em branco, define a propriedade de Instructor.OfficeAssignment como nulo para que a linha na tabela OfficeAssignment será excluída.

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- Salva as alterações no banco de dados.

### <a name="update-the-instructor-edit-view"></a>Atualizar a exibição instrutor editar

Em *Views/Instructors/Edit.cshtml*, adicionar um novo campo para editar o local do escritório, no final antes do **salvar** botão:

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

Executar o aplicativo, selecione o **instrutores** guia e, em seguida, clique em **editar** em instrutor. Alterar o **escritório** e clique em **salvar**.

![Página de edição do instrutor](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Adicionar atribuições de curso para a página Editar instrutor

Instrutores podem ensinar qualquer número de cursos. Agora você vai aprimorar a página Editar instrutor adicionando a capacidade de alterar as atribuições de curso usando um grupo de caixas de seleção, conforme mostrado na captura de tela a seguir:

![Página Editar instrutor de cursos](update-related-data/_static/instructor-edit-courses.png)

A relação entre as entidades do curso e instrutor é muitos-para-muitos. Para adicionar e remover relações, você adicionar e remover entidades de e para o conjunto de entidades de junção CourseAssignments.

A interface do usuário que permite que você altere os cursos instrutor é atribuído a é um grupo de caixas de seleção. É exibida uma caixa de seleção para cada curso no banco de dados, e aqueles que o instrutor atribuído atualmente à são selecionados. O usuário pode selecionar ou desmarcar as caixas de seleção para alterar as atribuições de curso. Se o número de cursos muito maior, você provavelmente desejará usar um método diferente de apresentar os dados no modo de exibição, mas você usaria o mesmo método de manipulação de uma entidade de associação para criar ou excluir relações.

### <a name="update-the-instructors-controller"></a>Atualize o controlador instrutores

Para fornecer dados para o modo de exibição de lista de caixas de seleção, você usará uma classe de modelo de exibição.

Criar *AssignedCourseData.cs* no *SchoolViewModels* pasta e substitua o código existente pelo seguinte código:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

Em *InstructorsController.cs*, substitua o HttpGet `Edit` método com o código a seguir. As alterações são realçadas.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

O código adiciona o carregamento rápido para o `Courses` propriedade de navegação e chama o novo `PopulateAssignedCourseData` método para fornecer informações para a matriz de caixa de seleção usando o `AssignedCourseData` exibir classe de modelo.

O código de `PopulateAssignedCourseData` método lê todas as entidades de curso para carregar uma lista de cursos usando a classe de modelo de exibição. Para cada curso, o código verifica se o curso existe do instrutor `Courses` propriedade de navegação. Para criar pesquisa eficiente ao verificar se um curso é atribuído para o instrutor, os cursos atribuídos para o instrutor são colocados em um `HashSet` coleção. O `Assigned` está definida como true para cursos instrutor é atribuído a. O modo de exibição usará essa propriedade para determinar qual seleção caixas devem ser exibidas como selecionado. Por fim, a lista é passada para o modo de exibição `ViewData`.

Em seguida, adicione o código que é executado quando o usuário clica **salvar**. Substitua o `EditPost` método com o seguinte código e adicionar um novo método que atualiza o `Courses` propriedade de navegação da entidade do instrutor.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

A assinatura do método agora é diferente do HttpGet `Edit` método, para que o nome do método é alterado de `EditPost` para `Edit`.

Como o modo de exibição não tem uma coleção de entidades de curso, o associador de modelo não é possível atualizar automaticamente o `CourseAssignments` propriedade de navegação. Em vez de usar o associador de modelo para atualizar o `CourseAssignments` propriedade de navegação, você pode fazer isso no novo `UpdateInstructorCourses` método. Portanto, você precisa excluir o `CourseAssignments` propriedade de associação de modelo. Isso não requer nenhuma alteração ao código que chama `TryUpdateModel` porque você está usando a sobrecarga de lista branca e `CourseAssignments` não estiver na lista de inclusão.

Se nenhuma seleção caixas tiverem sido selecionadas, o código em `UpdateInstructorCourses` inicializa o `CourseAssignments` propriedade de navegação com uma coleção vazia e retorna:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

O código, em seguida, executa um loop em todos os cursos no banco de dados e verifica cada curso contra os atualmente atribuídos ao instrutor em relação àqueles que foram selecionados no modo de exibição. Para facilitar a pesquisas eficientes, este último duas coleções são armazenadas em `HashSet` objetos.

Se a caixa de seleção para um curso foi selecionada, mas o curso não está no `Instructor.CourseAssignments` propriedade de navegação, o curso é adicionado à coleção na propriedade de navegação.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

Se a caixa de seleção para um curso não foi selecionada, mas o curso no `Instructor.CourseAssignments` propriedade de navegação, o curso é removida da propriedade de navegação.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>Atualizar os modos de exibição do instrutor

Em *Views/Instructors/Edit.cshtml*, adicionar um **cursos** campo com uma matriz de caixas de seleção, adicionando o seguinte código imediatamente após o `div` elementos para o **Office**  campo e antes do `div` elemento para o **salvar** botão.

<a id="notepad"></a>
> [!NOTE] 
> Quando você colar o código no Visual Studio, quebras de linha serão alteradas de forma que interrompe o código.  Pressione Ctrl + Z uma vez para desfazer a formatação automática.  Isso será corrigido as quebras de linha para que eles se parecer com o que você vê aqui. O recuo não precisa ser perfeito, mas o `@</tr><tr>`, `@:<td>`, `@:</td>`, e `@:</tr>` linhas devem ser em uma única linha conforme mostrado, ou você receberá um erro de tempo de execução. Com o bloco de código novo selecionado, pressione Tab três vezes para alinhar o novo código com o código existente. Você pode verificar o status desse problema [aqui](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

Esse código cria uma tabela HTML que tem três colunas. Em cada coluna é uma caixa de seleção, seguida de uma legenda que consiste em número e o título. Todas as caixas de seleção tem o mesmo nome ("selectedCourses"), que informa o associador de modelo devem ser tratados como um grupo. O atributo de valor de cada caixa de seleção é definido como o valor de `CourseID`. Quando a página é enviada, o associador de modelo passa uma matriz para o controlador que consiste o `CourseID` valores para apenas as caixas de seleção que estão selecionados.

Quando as caixas de seleção são inicialmente renderizadas, aqueles que estão atribuídos ao instrutor de cursos verificou atributos, que seleciona-los (exibe-os check).

Executar o aplicativo, selecione o **instrutores** guia e, em seguida, clique em **editar** em instrutor para ver o **editar** página.

![Página Editar instrutor de cursos](update-related-data/_static/instructor-edit-courses.png)

Alterar algumas atribuições do curso e clique em Salvar. As alterações feitas são refletidas na página de índice.

> [!NOTE] 
> A abordagem usada aqui para editar dados de curso instrutor funciona bem quando há um número limitado de cursos. Para coleções que são muito maiores, uma interface de usuário diferente e um método de atualização diferente seria necessárias.

## <a name="update-the-delete-page"></a>Atualizar a página de exclusão

Em *InstructorsController.cs*, exclua o `DeleteConfirmed` método e inserir o código a seguir em seu lugar.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

Este código faz as seguintes alterações:

* Eager faz carregar para o `CourseAssignments` propriedade de navegação.  Você precisa incluir isso ou EF não conhece relacionados `CourseAssignment` entidades e não excluí-los.  Para evitar a necessidade de lê-los aqui você pode configurar a exclusão em cascata no banco de dados.

* Se o instrutor a ser excluído é atribuído como administrador de todos os departamentos, remove a atribuição de instrutor dos departamentos.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Adicionar local do escritório e cursos para a página criar

Em *InstructorsController.cs*, exclua o HttpGet e HttpPost `Create` métodos e, em seguida, adicione o seguinte código em seu lugar:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

Esse código é semelhante ao que você viu o `Edit` métodos, exceto que inicialmente não cursos estão selecionados. O HttpGet `Create` chamadas de método de `PopulateAssignedCourseData` método não porque pode haver cursos selecionados, mas na ordem para fornecer uma coleção vazia para o `foreach` loop no modo de exibição (caso contrário, o código de exibição deve lançar uma exceção de referência nula).

O HttpPost `Create` método adiciona cada curso selecionado para o `CourseAssignments` antes que ele verifica se há erros de validação e adiciona o novo instrutor ao banco de dados de propriedade de navegação. Cursos são adicionados, mesmo se houver erros de modelo para que quando houver erros de modelo (por exemplo, o usuário uma data inválida de chave) e a página é exibida novamente com uma mensagem de erro, as seleções de curso que foram feitas serão restauradas automaticamente.

Observe que para poder adicionar o cursos para o `CourseAssignments` propriedade de navegação é necessário inicializar a propriedade como uma coleção vazia:

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

Como alternativa para fazer isso no código do controlador, você pode fazer no modelo instrutor alterando o getter de propriedade para criar automaticamente a coleção se ele não existir, conforme mostrado no exemplo a seguir:

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

Se você modificar o `CourseAssignments` propriedade dessa forma, você pode remover o código de inicialização de propriedade explícita no controlador.

Em *Views/Instructor/Create.cshtml*, adicione uma caixa de texto de local de escritório e marque caixas de cursos antes do botão de envio. Como no caso de página de edição, [corrija a formatação se o Visual Studio reformata o código quando você o colar](#notepad).

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

Testar o aplicativo em execução e criando um instrutor. 

## <a name="handling-transactions"></a>Processamento de transações

Conforme explicado no [tutorial CRUD](crud.md), a estrutura da entidade implementa implicitamente transações. Para cenários em que você precisa de mais controle – por exemplo, se você quiser incluir operações feitas fora do Entity Framework em uma transação – consulte [transações](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="summary"></a>Resumo

Agora você concluiu a introdução ao trabalho com dados relacionados. O seguinte tutorial, você verá como lidar com conflitos de simultaneidade.

>[!div class="step-by-step"]
[Anterior](read-related-data.md)
[Próximo](concurrency.md)  
