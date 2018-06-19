---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lendo dados relacionados com o Entity Framework em um aplicativo ASP.NET MVC | Microsoft Docs
author: tdykstra
description: /ajax/tutorials/using-ajax-control-toolkit-controls-and-control-extenders-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 06784d8b610856e71eae78b0db2d0253faedb955
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875313"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Ler os dados com o Entity Framework em um aplicativo ASP.NET MVC relacionados
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [baixar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio 2013. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


No tutorial anterior, você concluiu o modelo de dados de escola. Neste tutorial você ler e exibir dados relacionados — ou seja, os dados que o Entity Framework carrega em Propriedades de navegação.

As ilustrações a seguir mostram as páginas com as quais você trabalhará.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Carregamento lento, Eager e explícito de dados relacionados

Há várias maneiras que o Entity Framework pode carregar dados relacionados para as propriedades de navegação de uma entidade:

- *Carregamento lento*. Quando a entidade é lida pela primeira vez, os dados relacionados não são recuperados. No entanto, na primeira vez que você tenta acessar uma propriedade de navegação, os dados necessários para essa propriedade de navegação são recuperados automaticamente. Isso resulta em várias consultas enviadas ao banco de dados — um para a própria entidade e um cada vez que os dados para a entidade relacionados deve ser recuperado. O `DbContext` classe permite o carregamento preguiçoso por padrão. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Carregamento adiantado*. Quando a entidade é lida, os dados relacionados são recuperados com ela. Normalmente, isso resulta em uma única consulta de junção que recupera todos os dados necessários. Especifique o carregamento rápido usando o `Include` método.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Carregamento explícito*. Isso é semelhante ao carregamento lento, exceto que você explicitamente recuperar os dados relacionados no código; Isso não acontece automaticamente quando você acessa uma propriedade de navegação. Carregar dados relacionados manualmente fazendo com que a entrada de Gerenciador de estado do objeto para uma entidade e chamar o [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) método para coleções ou [Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) método para propriedades que contêm um entidade única. (No exemplo a seguir, se você quiser carregar a propriedade de navegação de administrador, você poderia substituir `Collection(x => x.Courses)` com `Reference(x => x.Administrator)`.) Normalmente, você usaria carregamento explícito somente quando você ativou a fora de carregamento lento.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Porque eles imediatamente não recuperarem os valores de propriedade, carregamento lento e carregamento explícito também são ambos conhecidos como *carregamento diferido*.

### <a name="performance-considerations"></a>Considerações sobre desempenho

Se você sabe que precisa de dados relacionados para cada entidade recuperada, o carregamento adiantado costuma oferecer o melhor desempenho, porque uma única consulta enviada para o banco de dados é geralmente mais eficiente do que consultas separadas para cada entidade recuperada. Nos exemplos acima, por exemplo, suponha que cada departamento tem dez cursos relacionados. O exemplo de carregamento rápido resultaria em apenas uma consulta única (associação) e uma única viagem de ida e ao banco de dados. O carregamento lento e exemplos de carregamento explícito ambos resultaria em onze consultas e onze viagens de ida e ao banco de dados. As viagens de ida e volta extras para o banco de dados são especialmente prejudiciais ao desempenho quando a latência é alta.

Por outro lado, em alguns cenários de carregamento lento é mais eficiente. Carregamento adiantado pode causar uma junção muito complexa para ser gerado, que o SQL Server não pode processar com eficiência. Ou, se você precisar acessar as propriedades de navegação de uma entidade somente para um subconjunto de um conjunto de entidades você estiver processando, carregamento preguiçoso pode executar melhor porque mais dados que você precisa recuperar o carregamento rápido. Se o desempenho for crítico, será melhor testar o desempenho das duas maneiras para fazer a melhor escolha.

Carregamento preguiçoso pode mascarar o código que causa problemas de desempenho. Por exemplo, o código que não especifica o carregamento adiantado ou explícito, mas processa um grande volume de entidades e usa várias propriedades de navegação em cada iteração pode ser muito eficiente (devido a muitos idas e voltas para o banco de dados). Um aplicativo que funciona bem em desenvolvimento usando um servidor SQL local pode ter problemas de desempenho quando movido para o banco de dados do Azure SQL devido ao aumento da latência de carregamento lento. As consultas de banco de dados com uma carga de teste realista de criação de perfil ajudará você a determinar se o carregamento lento é apropriado. Para obter mais informações, consulte [Desmistificando estratégias do Entity Framework: Carregando dados relacionados](https://msdn.microsoft.com/magazine/hh205756.aspx) e [usando o Entity Framework para reduzir a latência de rede para o SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Desabilitar o carregamento lento antes de serialização

Se você deixar de carregamento lento habilitado durante a serialização, você pode acabar consultando significativamente mais dados que você pretendia. Serialização geralmente funciona acessando cada propriedade em uma instância de um tipo. Acesso de propriedade dispara o carregamento lento e as entidades carregadas lentas são serializadas. O processo de serialização, em seguida, acessa cada propriedade de entidades lento carregado, potencialmente causando ainda mais o carregamento lento e serialização. Para evitar essa reação em cadeia fuga, ative carregamento antes de serializar uma entidade lento.

Serialização também pode ser complicada pelas classes proxy que usa o Entity Framework, conforme explicado no [tutorial cenários avançados](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

É uma maneira de evitar problemas de serialização serializar objetos de transferência de dados (DTOs) em vez de objetos de entidade, conforme mostrado no [usando API da Web com o Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) tutorial.

Se você não usar DTOs, você pode desabilitar o carregamento lento e evitar problemas de proxy por [desativar a criação de proxy](https://msdn.microsoft.com/data/jj592886.aspx).

Aqui estão algumas outras [maneiras de desabilitar o carregamento lento](https://msdn.microsoft.com/data/jj574232):

- Para propriedades de navegação específico, omita o `virtual` palavra-chave quando você declarar a propriedade.
- Para todas as propriedades de navegação, defina `LazyLoadingEnabled` para `false`, coloque o seguinte código no construtor de sua classe de contexto: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>Criar uma página de cursos esse nome de departamento exibe

O `Course` entidade inclui uma propriedade de navegação que contém o `Department` entidade do departamento de curso é atribuído a. Para exibir o nome do departamento atribuído em uma lista de cursos, você precisa obter o `Name` propriedade o `Department` entidade que o `Course.Department` propriedade de navegação.

Criar um controlador chamado `CourseController` (não CoursesController) para o `Course` tipo de entidade, usando as mesmas opções para o **controlador MVC 5 com modos de exibição usando o Entity Framework** scaffolder que você fez anteriormente para o `Student` controlador, conforme mostrado na ilustração a seguir:

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Abra *Controllers\CourseController.cs* e examine o `Index` método:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

O scaffolding automático especificou o carregamento adiantado para a propriedade de navegação `Department` usando o método `Include`.

Abra *Views\Course\Index.cshtml* e substitua o código de modelo com o código a seguir. As alterações são realçadas:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Você fez as seguintes alterações no código gerado por scaffolding:

- Alterar o título do **índice** para **cursos**.
- Adicionou uma coluna **Número** que mostra o valor da propriedade `CourseID`. Por padrão, as chaves primárias não são Scaffold porque normalmente eles não fazem sentidos para os usuários finais. No entanto, nesse caso, a chave primária é significativa e você deseja mostrá-la.
- Movido o **departamento** coluna à direita e alterado seu título. O scaffolder corretamente optou por exibir a `Name` propriedade o `Department` entidade, mas aqui na página do curso no cabeçalho da coluna deve ser **departamento** em vez de **nome**.

Observe que, para a coluna de departamento, o código de scaffolding exibe o `Name` propriedade o `Department` entidade que é carregada no `Department` propriedade de navegação:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Execute a página (selecione o **cursos** guia na home page do Contoso University) para ver a lista com nomes de departamento.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Criar uma página de instrutores que mostra os cursos e registros

Nesta seção você criará um controlador e exibir o `Instructor` entidade para exibir a página de professores:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Essa página lê e exibe dados relacionados das seguintes maneiras:

- A lista de instrutores exibe dados relacionados do `OfficeAssignment` entidade. As entidades `Instructor` e `OfficeAssignment` estão em uma relação um para zero ou um. Você usará o carregamento rápido para o `OfficeAssignment` entidades. Conforme explicado anteriormente, o carregamento adiantado é geralmente mais eficiente quando você precisa dos dados relacionados para todas as linhas recuperadas da tabela primária. Nesse caso, você deseja exibir atribuições de escritório para todos os instrutores exibidos.
- Quando o usuário seleciona um instrutor relacionado `Course` as entidades são exibidas. As entidades `Instructor` e `Course` estão em uma relação muitos para muitos. Você usará o carregamento rápido para o `Course` entidades e suas relacionados `Department` entidades. Nesse caso, o carregamento lento pode ser mais eficiente porque você precisa cursos somente para o instrutor selecionado. No entanto, este exemplo mostra como usar o carregamento adiantado para propriedades de navegação em entidades que estão nas propriedades de navegação.
- Quando o usuário seleciona um curso, relacionadas a dados a partir de `Enrollments` conjunto de entidades é exibido. As entidades `Course` e `Enrollment` estão em uma relação um-para-muitos. Você adicionará o carregamento explícito para `Enrollment` entidades e suas relacionados `Student` entidades. (O carregamento explícito não é necessário porque o carregamento lento está habilitado, mas isso mostra como fazer carregamento explícito.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Criar um modelo de exibição para o modo de exibição de índice do instrutor

A página de instrutores mostra três tabelas diferentes. Portanto, você criará um modelo de exibição que inclui três propriedades, cada uma contendo os dados de uma das tabelas.

No *ViewModels* pasta, criar *InstructorIndexData.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Criar o controlador de instrutor e modos de exibição

Criar um `InstructorController` (não InstructorsController) controlador com ações de leitura/gravação EF conforme mostrado na ilustração a seguir:

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Abra *Controllers\InstructorController.cs* e adicione um `using` instrução para o `ViewModels` namespace:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

O código scaffolding o `Index` método especifica carregamento adiantado somente para o `OfficeAssignment` propriedade de navegação:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Substitua o `Index` método com o código a seguir para carregar mais dados relacionados e colocá-la no modelo de exibição:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

O método aceita dados de rota opcional (`id`) e um parâmetro de cadeia de caracteres de consulta (`courseID`) que forneça os valores de ID do curso selecionado e instrutor selecionado e passar em todos os dados necessários para o modo de exibição. Os parâmetros são fornecidos pelos hiperlinks **Selecionar** na página.

O código começa com a criação de uma instância do modelo de exibição e colocando-a na lista de instrutores. O código especifica o carregamento rápido para o `Instructor.OfficeAssignment` e `Instructor.Courses` propriedade de navegação.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

A segunda `Include` método carrega cursos, e cada curso carregado faz carregamento rápido para o `Course.Department` propriedade de navegação.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Conforme mencionado anteriormente, o carregamento adiantado não é necessário, mas é feito para melhorar o desempenho. Como o modo de exibição sempre requer o `OfficeAssignment` entidade, é mais eficiente buscar que na mesma consulta. `Course` entidades são necessárias quando um instrutor é selecionado na página da web, para que carregamento adiantado é melhor do que o carregamento preguiçoso somente se a página é exibida com mais frequência um curso selecionado que sem.

Se uma ID de instrutor foi selecionada, o instrutor selecionado é recuperado da lista de instrutores no modelo de exibição. O modelo de exibição `Courses` propriedade, em seguida, é carregada com o `Course` entidades que instrutor `Courses` propriedade de navegação.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

O `Where` método retorna uma coleção, mas nesse caso os critérios são passados para o resultado em um único método `Instructor` entidade que está sendo retornada. O `Single` método converte a coleção em uma única `Instructor` entidade, que fornece acesso a essa entidade `Courses` propriedade.

Você usa o [único](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) método em uma coleção quando você souber que a coleção terá apenas um item. O `Single` método lançará uma exceção se a coleção passada para ele está vazia ou se houver mais de um item. Uma alternativa é [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), que retorna um valor padrão (`null` nesse caso) se a coleção está vazia. No entanto, nesse caso isso ainda resultar em uma exceção (de tentativa de encontrar um `Courses` propriedade em um `null` referência), e a mensagem de exceção menos claramente pode indicar a causa do problema. Quando você chama o `Single` método, você também pode passar do `Where` condição em vez de chamar o `Where` método separadamente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Em vez de:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Em seguida, se um curso foi selecionado, o curso selecionado é recuperado na lista de cursos no modelo de exibição. Em seguida, o modelo de exibição `Enrollments` propriedade é carregada com o `Enrollment` entidades que curso `Enrollments` propriedade de navegação.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Modificar a exibição do índice instrutor

Em *Views\Instructor\Index.cshtml*, substitua o código de modelo com o código a seguir. As alterações são realçadas:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Você fez as seguintes alterações no código existente:

- Alterou a classe de modelo para `InstructorIndexData`.
- Alterou o título de página de **Índice** para **Instrutores**.
- Adicionado um **Office** coluna que exibe `item.OfficeAssignment.Location` somente se `item.OfficeAssignment` não for nulo. (Como esta é uma relação um-para-zero-ou-um, talvez não haja um relacionados `OfficeAssignment` entidade.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Código adicionado dinamicamente adicionará `class="success"` para o `tr` elemento do instrutor selecionado. Isso define uma cor de plano de fundo para a linha selecionada usando um [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) classe. 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Adicionar uma nova `ActionLink` rotulada **selecione** imediatamente antes de outros links em cada linha, que faz com que a ID do instrutor selecionado ser enviado para o `Index` método.

Execute o aplicativo e selecione o **instrutores** guia. A página exibe o `Location` propriedade relacionados `OfficeAssignment` entidades e uma tabela vazia de célula quando há não relacionada `OfficeAssignment` entidade.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

No *Views\Instructor\Index.cshtml* arquivo, após o fechamento `table` elemento (no final do arquivo), adicione o código a seguir. Esse código exibe uma lista de cursos relacionados a um instrutor quando um instrutor é selecionado.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Esse código lê a propriedade `Courses` do modelo de exibição para exibir uma lista de cursos. Ele também fornece um `Select` hiperlink que envia a ID do curso selecionado para o `Index` método de ação.

Execute a página e selecione um instrutor. Agora, você verá uma grade que exibe os cursos atribuídos ao instrutor selecionado, e para cada curso, verá o nome do departamento atribuído.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Após o bloco de código que você acabou de adicionar, adicione o código a seguir. Isso exibe uma lista dos alunos que estão registrados em um curso quando esse curso é selecionado.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Esse código lê o `Enrollments` propriedade do modelo de exibição para exibir uma lista dos alunos inscritos no curso.

Execute a página e selecione um instrutor. Em seguida, selecione um curso para ver a lista de alunos registrados e suas notas.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>Adicionando o carregamento explícito

Abra *InstructorController.cs* e examine como o `Index` método obtém a lista de registros para um curso selecionado:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Quando você recuperar a lista de instrutores, você especificou o carregamento rápido para o `Courses` propriedade de navegação e para o `Department` propriedade cada curso. Em seguida, você coloca o `Courses` coleção no modelo de exibição, e agora você está acessando o `Enrollments` propriedade de navegação de uma entidade na coleção. Porque você não especificou o carregamento rápido para o `Course.Enrollments` propriedade de navegação, os dados da propriedade é exibido na página como resultado de carregamento lento.

Se você tiver desabilitado o carregamento lento sem alterar o código de outra forma, o `Enrollments` a propriedade deve ser nula, independentemente de quantos registros o curso, na verdade, teve. Nesse caso, para carregar o `Enrollments` propriedade, você precisaria especificar carregamento adiantado ou carregamento explícito. Você já viu como fazer o carregamento rápido. Para ver um exemplo de carregamento explícito, substitua o `Index` método com o código a seguir, que carrega explicitamente o `Enrollments` propriedade. O código alterado são realçadas.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Depois de obter selecionado `Course` entidade, o novo código carrega explicitamente esse curso `Enrollments` propriedade de navegação:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Em seguida, ele carrega explicitamente cada `Enrollment` entidade relacionada `Student` entidade:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Observe que você usar o `Collection` método para carregar uma propriedade de coleção, mas para uma propriedade que contém apenas uma entidade, você deve usar o `Reference` método.

Execute a página de índice do instrutor agora e você não verá nenhuma diferença no que é exibido na página, embora você alterou a como os dados são recuperados.

## <a name="summary"></a>Resumo

Agora, você usou todas as três maneiras (lentas, eager e explícitas) para carregar dados relacionados em Propriedades de navegação. No próximo tutorial, você aprenderá a atualizar dados relacionados.

Deixe comentários em como você gostou neste tutorial e nós poderíamos melhorar. Você também pode solicitar novos tópicos em [Mostrar-Me como com código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links para outros recursos do Entity Framework podem ser encontradas no [acesso a dados ASP.NET - recomendado recursos](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Próximo](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
