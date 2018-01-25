---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Atualizando dados relacionados com o Entity Framework em um aplicativo ASP.NET MVC | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 205d5ddcd0c3240c87ec5705a6676215eb67942d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Atualizando dados relacionados com o Entity Framework em um aplicativo ASP.NET MVC
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [baixar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio 2013. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


No tutorial anterior, você exibiu dados relacionados. Neste tutorial, você vai atualizar dados relacionados. Para a maioria das relações, isso pode ser feito Atualizando campos de chave estrangeira ou propriedades de navegação. Para relações muitos-para-muitos, o Entity Framework não expõe a tabela de junção diretamente, para que você adiciona e remove entidades de e para as propriedades de navegação apropriado.

As ilustrações a seguir mostram algumas das páginas que você trabalhará com.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Editar instrutor com cursos](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizar a criar e editar páginas de cursos

Quando uma nova entidade de curso é criada, ele deve ter uma relação de um departamento existente. Para facilitar isso, o código de scaffolding inclui os métodos do controlador e criar e editar modos de exibição que incluem uma lista suspensa para selecionar o departamento. Define a lista suspensa de `Course.DepartmentID` propriedade de chave estrangeira, e isso é tudo o que o Entity Framework precisa para carregar o `Department` propriedade de navegação com apropriada `Department` entidade. Você vai usar o código de scaffolding, mas altere ligeiramente para adicionar o tratamento de erros e classificar a lista suspensa.

Em *CourseController.cs*, exclua as quatro `Create` e `Edit` métodos e substituí-los com o código a seguir:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Adicione o seguinte `using` instrução no início do arquivo:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

O `PopulateDepartmentsDropDownList` método obtém uma lista de todos os departamentos classificados por nome, cria um `SelectList` coleção para uma lista suspensa e passa a coleção para o modo de exibição em um `ViewBag` propriedade. O método aceita opcional `selectedDepartment` parâmetro que permite que o código de chamada especificar o item que será selecionado quando a lista suspensa é renderizada. A exibição passará o nome `DepartmentID` para o [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) auxiliar e o auxiliar sabe para examinar o `ViewBag` de objeto para um `SelectList` chamado `DepartmentID`.

O `HttpGet` `Create` chamadas de método de `PopulateDepartmentsDropDownList` método sem definir o item selecionado, como para um novo curso o departamento não for estabelecido ainda:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

O `HttpGet` `Edit` método define o item selecionado, com base na identificação do departamento que já está atribuído ao curso sendo editado:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

O `HttpPost` métodos para ambos `Create` e `Edit` também incluir o código que define o item selecionado quando eles exibir novamente a página após um erro:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Esse código garante que quando a página é reexibida para mostrar a mensagem de erro, qualquer departamento foi selecionado permaneça selecionado.

As exibições de curso já são Scaffold com listas suspensas para o campo de departamento, mas não deseja que a legenda da ID do departamento para esse campo, por isso, verifique o seguinte realçado alterar para o *Views\Course\Create.cshtml* de arquivos para Altere a legenda.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Fazer a mesma alteração no *Views\Course\Edit.cshtml*.

Normalmente o scaffolder não scaffold de uma chave primária porque o valor da chave é gerado pelo banco de dados e não pode ser alterado e não é um valor significativo a ser exibida aos usuários. Para entidades de curso o scaffolder incluem uma caixa de texto para o `CourseID` campo porque ele entende que o `DatabaseGeneratedOption.None` atributo significa que o usuário deve ser capaz de inserir o valor de chave primária. Mas ele não entende que porque o número é significativo deseja vê-lo em outros modos, portanto você precisa adicioná-lo manualmente.

Em *Views\Course\Edit.cshtml*, adicionar um campo de número de curso antes do **título** campo. Porque ele é a chave primária, ele é exibido, mas ele não pode ser alterado.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Já existe um campo oculto (`Html.HiddenFor` auxiliar) para o número de curso no modo de edição. Adicionando um *Html.LabelFor* auxiliar não elimina a necessidade de campo oculto porque ele não faz o número a ser incluído nos dados de postagem quando o usuário clica **salvar** na página Editar.

Em *Views\Course\Delete.cshtml* e *Views\Course\Details.cshtml*, altere a legenda de nome de departamento do "Nome" para "Departamento" e adicione um campo de número de curso antes do **título**  campo.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Execute o **criar** página (exibir a página de índice do curso e clique em **criar novo**) e insira os dados para um novo curso:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Clique em **Criar**. A página de índice de curso é exibida com o curso novo adicionado à lista. O nome do departamento na lista de páginas de índice é obtido da propriedade de navegação, mostrando que a relação foi estabelecida corretamente.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Execute o **editar** página (exibir a página de índice do curso e clique em **editar** em um curso).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Alterar dados na página e clique em **salvar**. A página de índice de curso é exibida com os dados de curso atualizado.

## <a name="adding-an-edit-page-for-instructors"></a>Adicionando uma página de edição para instrutores

Quando você editar um registro de instrutor, você deseja ser capaz de atualizar a atribuição do office do instrutor. O `Instructor` entidade tem uma relação um-para-zero-ou-um com o `OfficeAssignment` entidade, o que significa que você deve tratar as situações a seguir:

- Se o usuário apaga a atribuição do office e que originalmente tinha um valor, você deve remover e excluir o `OfficeAssignment` entidade.
- Se o usuário insere um valor de atribuição do office e ele foi originalmente vazio, você deve criar um novo `OfficeAssignment` entidade.
- Se o usuário altera o valor de uma atribuição de escritório, você deve alterar o valor em existente `OfficeAssignment` entidade.

Abra *InstructorController.cs* e examine o `HttpGet` `Edit` método:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

O código de scaffolding aqui não é o que você deseja. Configurar dados para uma lista suspensa, mas você precisa é de uma caixa de texto. Substitua este método com o código a seguir:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Esse código descarta o `ViewBag` instrução e adiciona o carregamento rápido para associado `OfficeAssignment` entidade. Não é possível executar o carregamento rápido com o `Find` método, portanto, o `Where` e `Single` métodos são usados em vez disso, selecione o instrutor.

Substitua o `HttpPost` `Edit` método com o código a seguir. que trata de atualizações de atribuição do office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

A referência ao `RetryLimitExceededException` requer um `using` instrução; para adicioná-lo, clique com botão direito `RetryLimitExceededException`e, em seguida, clique em **resolver** - **usando System.Data.Entity.Infrastructure**.

![Exceção de nova tentativa de resolver](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

O código faz o seguinte:

- Altera o nome do método para `EditPost` porque a assinatura agora é o mesmo que o `HttpGet` método (o `ActionName` atributo especifica que a URL /Edit/ ainda é usada).
- Obtém a atual `Instructor` entidade do banco de dados usando o carregamento rápido para o `OfficeAssignment` propriedade de navegação. Isso é o mesmo que você fez o `HttpGet` `Edit` método.
- Atualiza recuperada `Instructor` entidade com valores de associador de modelo. O [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) sobrecarga usada permite que você *lista branca* as propriedades que você deseja incluir. Isso impede o excesso de lançamento, conforme explicado em [segundo tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Define se o local do escritório estiver em branco, o `Instructor.OfficeAssignment` propriedade como nulo para que a linha relacionada a `OfficeAssignment` tabela será excluída.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Salva as alterações no banco de dados.

Em *Views\Instructor\Edit.cshtml*, após o `div` elementos para o **data de contratação** campo, adicionar um novo campo para editar o local do escritório:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Execute a página (selecione o **instrutores** guia e, em seguida, clique em **editar** em instrutor). Alterar o **escritório** e clique em **salvar**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Adicionar atribuições de curso para o instrutor Editar página

Instrutores podem ensinar qualquer número de cursos. Agora você vai aprimorar a página Editar instrutor adicionando a capacidade de alterar as atribuições de curso usando um grupo de caixas de seleção, conforme mostrado na captura de tela a seguir:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

A relação entre o `Course` e `Instructor` entidades é muitos-para-muitos, o que significa que você não tem acesso direto às propriedades de chave estrangeiras que estão na tabela de junção. Em vez disso, você pode adicionar e remover entidades de e para o `Instructor.Courses` propriedade de navegação.

A interface do usuário que permite que você altere os cursos instrutor é atribuído a é um grupo de caixas de seleção. É exibida uma caixa de seleção para cada curso no banco de dados, e aqueles que o instrutor atribuído atualmente à são selecionados. O usuário pode selecionar ou desmarcar as caixas de seleção para alterar as atribuições de curso. Se o número de cursos muito maior, você provavelmente desejará usar um método diferente de apresentar os dados no modo de exibição, mas você deve usar o mesmo método de manipulação de propriedades de navegação para criar ou excluir relações.

Para fornecer dados para o modo de exibição de lista de caixas de seleção, você usará uma classe de modelo de exibição. Criar *AssignedCourseData.cs* no *ViewModels* pasta e substitua o código existente pelo seguinte código:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Em *InstructorController.cs*, substitua o `HttpGet` `Edit` método com o código a seguir. As alterações são realçadas.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

O código adiciona o carregamento rápido para o `Courses` propriedade de navegação e chama o novo `PopulateAssignedCourseData` método para fornecer informações para a matriz de caixa de seleção usando o `AssignedCourseData` exibir classe de modelo.

O código de `PopulateAssignedCourseData` método lê todas `Course` entidades para carregar uma lista de cursos usando a exibição de classe de modelo. Para cada curso, o código verifica se o curso existe do instrutor `Courses` propriedade de navegação. Para criar pesquisa eficiente ao verificar se um curso é atribuído para o instrutor, os cursos atribuídos para o instrutor são colocados em um [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) coleção. O `Assigned` está definida como `true` cursos instrutor é atribuído. O modo de exibição usará essa propriedade para determinar qual seleção caixas devem ser exibidas como selecionado. Por fim, a lista é passada para a exibição em um `ViewBag` propriedade.

Em seguida, adicione o código que é executado quando o usuário clica **salvar**. Substitua o `EditPost` método com o código a seguir, que chama um novo método que atualiza o `Courses` propriedade de navegação a `Instructor` entidade. As alterações são realçadas.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

A assinatura do método agora é diferente de `HttpGet` `Edit` método, para que o nome do método é alterado de `EditPost` para `Edit`.

Como o modo de exibição não tem uma coleção de `Course` entidades, o associador de modelo não é possível atualizar automaticamente o `Courses` propriedade de navegação. Em vez de usar o associador de modelo para atualizar o `Courses` propriedade de navegação, você poderá fazer isso no novo `UpdateInstructorCourses` método. Portanto, você precisa excluir o `Courses` propriedade de associação de modelo. Isso não requer nenhuma alteração ao código que chama [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) porque você está usando o *lista branca* de sobrecarga e `Courses` não estiver na lista de inclusão.

Se nenhuma seleção caixas tiverem sido selecionadas, o código em `UpdateInstructorCourses` inicializa o `Courses` propriedade de navegação com uma coleção vazia:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

O código, em seguida, executa um loop em todos os cursos no banco de dados e verifica cada curso contra os atualmente atribuídos ao instrutor em relação àqueles que foram selecionados no modo de exibição. Para facilitar a pesquisas eficientes, este último duas coleções são armazenadas em `HashSet` objetos.

Se a caixa de seleção para um curso foi selecionada, mas o curso não está no `Instructor.Courses` propriedade de navegação, o curso é adicionado à coleção na propriedade de navegação.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Se a caixa de seleção para um curso não foi selecionada, mas o curso no `Instructor.Courses` propriedade de navegação, o curso é removida da propriedade de navegação.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Em *Views\Instructor\Edit.cshtml*, adicionar um **cursos** campo com uma matriz de caixas de seleção, adicionando o seguinte código imediatamente após o `div` elementos para o `OfficeAssignment` campo e antes do `div` elemento para o **salvar** botão:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Depois de colar o código, se quebras de linha e o recuo não aparecem como aqui, corrigir tudo manualmente para que ele se parece com o que você vê aqui. O recuo não precisa ser perfeito, mas o `@</tr><tr>`, `@:<td>`, `@:</td>`, e `@</tr>` linhas devem ser em uma única linha conforme mostrado, ou você receberá um erro de tempo de execução.

Esse código cria uma tabela HTML que tem três colunas. Em cada coluna é uma caixa de seleção, seguida de uma legenda que consiste em número e o título. Todas as caixas de seleção tem o mesmo nome ("selectedCourses"), que informa o associador de modelo devem ser tratados como um grupo. O `value` atributo de cada caixa de seleção é definido como o valor de `CourseID.` quando a página é enviada, o associador de modelo passa uma matriz para o controlador que consiste o `CourseID` valores para apenas as caixas de seleção que estão selecionados.

Quando as caixas de seleção são inicialmente renderizadas, aqueles que estão atribuídos ao instrutor de cursos tem `checked` atributos, que seleciona (exibe-os check).

Depois de alterar as atribuições de curso, você desejará ser capaz de verificar as alterações quando o site retorna para o `Index` página. Portanto, você precisa adicionar uma coluna à tabela na página. Nesse caso, você não precisa usar o `ViewBag` do objeto, porque as informações que você deseja exibir já estão no `Courses` propriedade de navegação a `Instructor` entidade que você está passando para a página do modelo.

Em *Views\Instructor\Index.cshtml*, adicione um **cursos** título imediatamente após o **Office** título, conforme mostrado no exemplo a seguir:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Em seguida, adicione uma nova célula de detalhe imediatamente após a célula de detalhes de local do office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Execute o **índice instrutor** página para ver os cursos atribuídos a cada instrutor:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Clique em **editar** em instrutor para ver a página de edição.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

Alterar algumas atribuições do curso e clique em **salvar**. As alterações feitas são refletidas na página de índice.

 Observação: A abordagem usada aqui para editar dados de curso instrutor funciona bem quando há um número limitado de cursos. Para coleções que são muito maiores, uma interface de usuário diferente e um método de atualização diferente seria necessárias.  
 

## <a name="update-the-deleteconfirmed-method"></a>Atualizar o método DeleteConfirmed

Em *InstructorController.cs*, exclua o `DeleteConfirmed` método e inserir o código a seguir em seu lugar.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Este código faz as seguintes alterações:

- Se o instrutor foi atribuído como administrador de qualquer departamento, remove a atribuição de instrutor do departamento. Sem esse código, você obterá um erro de integridade referencial, se você tentou excluir um instrutor que foi atribuído como administrador de um departamento.

Esse código não lidar com o cenário de um professor atribuído como administrador para vários departamentos. O último tutorial, você adicionará código que impede essa situação aconteça.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Adicionar local do escritório e cursos para a página criar

Em *InstructorController.cs*, exclua o `HttpGet` e `HttpPost` `Create` métodos e, em seguida, adicione o seguinte código em seu lugar:


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Esse código é semelhante ao que você viu para os métodos de edição exceto que inicialmente não cursos são selecionados. O `HttpGet` `Create` chamadas de método de `PopulateAssignedCourseData` método não porque pode haver cursos selecionados, mas na ordem para fornecer uma coleção vazia para o `foreach` loop no modo de exibição (caso contrário, o código de exibição lançaria uma exceção de referência nula ).

O método de criação de HttpPost adiciona cada curso selecionado para a propriedade de navegação de cursos antes do código de modelo que verifica se há erros de validação e adiciona o novo instrutor ao banco de dados. Cursos são adicionados, mesmo se houver erros de modelo para que quando há erros de modelo (por exemplo, o usuário uma data inválida de chave) para que quando a página é exibida novamente com uma mensagem de erro, as seleções de curso que foram feitas serão restauradas automaticamente.

Observe que para poder adicionar o cursos para o `Courses` propriedade de navegação é necessário inicializar a propriedade como uma coleção vazia:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Como alternativa para fazer isso no código do controlador, você pode fazer no modelo instrutor alterando o getter de propriedade para criar automaticamente a coleção se ele não existir, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Se você modificar o `Courses` propriedade dessa forma, você pode remover o código de inicialização de propriedade explícita no controlador.

Em *Views\Instructor\Create.cshtml*, adicione uma caixa de texto de local de escritório e caixas de seleção do curso depois que o campo de data de contratação e antes do **enviar** botão.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Depois de colar o código, corrigi quebras de linha e recuo como você fez anteriormente para a página de edição.

Execute a página criar e adicionar um instrutor.

![Criar instrutor com cursos](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>Processamento de transações

Conforme explicado no [tutorial de funcionalidade básica de CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), por padrão o Entity Framework implicitamente implementa transações. Para cenários em que você precisa de mais controle – por exemplo, se você quiser incluir operações feitas fora do Entity Framework em uma transação – consulte [trabalhando com transações](https://msdn.microsoft.com/data/dn456843) no MSDN.

## <a name="summary"></a>Resumo

Agora você concluiu esta introdução ao trabalhar com dados relacionados. Até agora esses tutoriais você já trabalhou com o código que faz a e/s síncronas. Você pode fazer com que o aplicativo usar recursos do servidor web com mais eficiência com a implementação de código assíncrono, e é o que você fará o seguinte tutorial.

Deixe comentários em como você gostou neste tutorial e nós poderíamos melhorar. Você também pode solicitar novos tópicos em [Mostrar-Me como com código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links para outros recursos do Entity Framework podem ser encontradas no [acesso a dados ASP.NET - recomendado recursos](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Próximo](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
