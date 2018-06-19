---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lendo dados relacionados com o Entity Framework em um aplicativo ASP.NET MVC (5 de 10) | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 831f5e0a8b57907ea012c10c1d1f8ff166f5e88b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876677"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Leitura relacionadas a dados com o Entity Framework em um aplicativo ASP.NET MVC (5 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Você pode iniciar a série de tutoriais do início ou [baixar um projeto inicial para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) e comece aqui.
> 
> > [!NOTE] 
> > 
> > Se você tiver um problema que não é possível resolver, [baixar o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tentar reproduzir o problema. Geralmente, você pode encontrar a solução para o problema, comparando o seu código para o código completo. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


No tutorial anterior, você concluiu o modelo de dados de escola. Neste tutorial você ler e exibir dados relacionados — ou seja, os dados que o Entity Framework carrega em Propriedades de navegação.

As ilustrações a seguir mostram as páginas com as quais você trabalhará.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Carregamento lento, Eager e explícito de dados relacionados

Há várias maneiras que o Entity Framework pode carregar dados relacionados para as propriedades de navegação de uma entidade:

- *Carregamento lento*. Quando a entidade é lida pela primeira vez, os dados relacionados não são recuperados. No entanto, na primeira vez que você tenta acessar uma propriedade de navegação, os dados necessários para essa propriedade de navegação são recuperados automaticamente. Isso resulta em várias consultas enviadas ao banco de dados — um para a própria entidade e um cada vez que os dados para a entidade relacionados deve ser recuperado. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Carregamento adiantado*. Quando a entidade é lida, os dados relacionados são recuperados com ela. Normalmente, isso resulta em uma única consulta de junção que recupera todos os dados necessários. Especifique o carregamento rápido usando o `Include` método.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Carregamento explícito*. Isso é semelhante ao carregamento lento, exceto que você explicitamente recuperar os dados relacionados no código; Isso não acontece automaticamente quando você acessa uma propriedade de navegação. Carregar dados relacionados manualmente fazendo com que a entrada de Gerenciador de estado do objeto para uma entidade e chamar o `Collection.Load` método para coleções ou `Reference.Load` método para propriedades que mantêm uma única entidade. (No exemplo a seguir, se você quiser carregar a propriedade de navegação de administrador, você poderia substituir `Collection(x => x.Courses)` com `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Porque eles imediatamente não recuperarem os valores de propriedade, carregamento lento e carregamento explícito também são ambos conhecidos como *carregamento diferido*.

Em geral, se você souber terá dados relacionados para cada entidade recuperado, eager carregamento oferece o melhor desempenho, porque uma única consulta enviada para o banco de dados é geralmente mais eficiente do que consultas separadas para cada entidade recuperada. Nos exemplos acima, por exemplo, suponha que cada departamento tem dez cursos relacionados. O exemplo de carregamento rápido resultaria em apenas uma consulta única (associação) e uma única viagem de ida e ao banco de dados. O carregamento lento e exemplos de carregamento explícito ambos resultaria em onze consultas e onze viagens de ida e ao banco de dados. As viagens de ida e volta extras para o banco de dados são especialmente prejudiciais ao desempenho quando a latência é alta.

Por outro lado, em alguns cenários de carregamento lento é mais eficiente. Carregamento adiantado pode causar uma junção muito complexa para ser gerado, que o SQL Server não pode processar com eficiência. Ou se você precisar acessar as propriedades de navegação de uma entidade somente para um subconjunto de um conjunto de entidades estiver processamento, carregamento lento pode executar melhor porque mais dados que você precisa recuperar o carregamento rápido. Se o desempenho for crítico, será melhor testar o desempenho das duas maneiras para fazer a melhor escolha.

Normalmente, você usaria carregamento explícito somente quando você ativou a fora de carregamento lento. É um cenário quando você deve ativar o logoff de carregamento lento durante a serialização. Serialização e carregamento preguiçoso não misturam bem, e se você não tiver cuidado que você pode acabar consultando dados significativamente mais que o previsto quando lento carregamento está habilitado. Serialização geralmente funciona acessando cada propriedade em uma instância de um tipo. Acesso de propriedade dispara o carregamento lento e as entidades carregadas lentas são serializadas. O processo de serialização, em seguida, acessa cada propriedade de entidades lento carregado, potencialmente causando ainda mais o carregamento lento e serialização. Para evitar essa reação em cadeia fuga, ative carregamento antes de serializar uma entidade lento.

A classe de contexto de banco de dados executa carregamento preguiçoso por padrão. Há duas maneiras de desabilitar o carregamento lento:

- Para propriedades de navegação específico, omita o `virtual` palavra-chave quando você declarar a propriedade.
- Para todas as propriedades de navegação, defina `LazyLoadingEnabled` para `false`. Por exemplo, você pode colocar o código a seguir no construtor de sua classe de contexto: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Carregamento preguiçoso pode mascarar o código que causa problemas de desempenho. Por exemplo, o código que não especifica o carregamento adiantado ou explícito, mas processa um grande volume de entidades e usa várias propriedades de navegação em cada iteração pode ser muito eficiente (devido a muitos idas e voltas para o banco de dados). Um aplicativo que funciona bem em desenvolvimento usando um servidor SQL local pode ter problemas de desempenho quando movido para o banco de dados do Azure SQL devido ao aumento da latência de carregamento lento. As consultas de banco de dados com uma carga de teste realista de criação de perfil ajudará você a determinar se o carregamento lento é apropriado. Para obter mais informações, consulte [Desmistificando estratégias do Entity Framework: Carregando dados relacionados](https://msdn.microsoft.com/magazine/hh205756.aspx) e [usando o Entity Framework para reduzir a latência de rede para o SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Criar uma página de índice de cursos esse nome de departamento exibe

O `Course` entidade inclui uma propriedade de navegação que contém o `Department` entidade do departamento de curso é atribuído a. Para exibir o nome do departamento atribuído em uma lista de cursos, você precisa obter o `Name` propriedade o `Department` entidade que o `Course.Department` propriedade de navegação.

Criar um controlador chamado `CourseController` para o `Course` tipo de entidade, usando as mesmas opções que você fez anteriormente para o `Student` controlador, conforme mostrado na ilustração a seguir (exceto a imagem, ao contrário de sua classe de contexto está no namespace DAL, não modelos namespace):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Abra *Controllers\CourseController.cs* e examine o `Index` método:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

O scaffolding automático especificou o carregamento adiantado para a propriedade de navegação `Department` usando o método `Include`.

Abra *Views\Course\Index.cshtml* e substitua o código existente com o código a seguir. As alterações são realçadas:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Você fez as seguintes alterações no código gerado por scaffolding:

- Alterar o título do **índice** para **cursos**.
- Mover os links de linha à esquerda.
- Adicionada uma coluna sob o título **número** que mostra o `CourseID` o valor da propriedade. (Por padrão, as chaves primárias não são Scaffold porque normalmente eles não fazem sentidos para os usuários finais. No entanto, nesse caso, a chave primária é significativa e você deseja mostrá-la.)
- Alterado o último cabeçalho da coluna de **DepartmentID** (o nome da chave estrangeira para a `Department` entidade) para **departamento**.

Observe que, para a última coluna, o código de scaffolding exibe o `Name` propriedade o `Department` entidade que é carregada no `Department` propriedade de navegação:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Execute a página (selecione o **cursos** guia na home page do Contoso University) para ver a lista com nomes de departamento.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Criar uma página de índice instrutores que mostra os cursos e registros

Nesta seção você criará um controlador e exibir o `Instructor` entidade para exibir a página de índice instrutores:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Essa página lê e exibe dados relacionados das seguintes maneiras:

- A lista de instrutores exibe dados relacionados do `OfficeAssignment` entidade. As entidades `Instructor` e `OfficeAssignment` estão em uma relação um para zero ou um. Você usará o carregamento rápido para o `OfficeAssignment` entidades. Conforme explicado anteriormente, o carregamento adiantado é geralmente mais eficiente quando você precisa dos dados relacionados para todas as linhas recuperadas da tabela primária. Nesse caso, você deseja exibir atribuições de escritório para todos os instrutores exibidos.
- Quando o usuário seleciona um instrutor relacionado `Course` as entidades são exibidas. As entidades `Instructor` e `Course` estão em uma relação muitos para muitos. Você usará o carregamento rápido para o `Course` entidades e suas relacionados `Department` entidades. Nesse caso, o carregamento lento pode ser mais eficiente porque você precisa cursos somente para o instrutor selecionado. No entanto, este exemplo mostra como usar o carregamento adiantado para propriedades de navegação em entidades que estão nas propriedades de navegação.
- Quando o usuário seleciona um curso, relacionadas a dados a partir de `Enrollments` conjunto de entidades é exibido. As entidades `Course` e `Enrollment` estão em uma relação um-para-muitos. Você adicionará o carregamento explícito para `Enrollment` entidades e suas relacionados `Student` entidades. (O carregamento explícito não é necessário porque o carregamento lento está habilitado, mas isso mostra como fazer carregamento explícito.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Criar um modelo de exibição para o modo de exibição de índice do instrutor

A página de índice do instrutor mostra três tabelas diferentes. Portanto, você criará um modelo de exibição que inclui três propriedades, cada uma contendo os dados de uma das tabelas.

No *ViewModels* pasta, criar *InstructorIndexData.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Adicionar um estilo para as linhas selecionadas

Para marcar as linhas selecionadas, é necessário uma cor de plano de fundo diferentes. Para fornecer um estilo para essa interface do usuário, adicione o seguinte código à seção `/* info and errors */` na *Content\Site.css*, conforme mostrado abaixo:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Criar o controlador de instrutor e modos de exibição

Criar um `InstructorController` controlador conforme mostrado na ilustração a seguir:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Abra *Controllers\InstructorController.cs* e adicione um `using` instrução para o `ViewModels` namespace:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

O código scaffolding o `Index` método especifica carregamento adiantado somente para o `OfficeAssignment` propriedade de navegação:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Substitua o `Index` método com o código a seguir para carregar mais dados relacionados e colocá-la no modelo de exibição:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

O método aceita dados de rota opcional (`id`) e um parâmetro de cadeia de caracteres de consulta (`courseID`) que forneça os valores de ID do curso selecionado e instrutor selecionado e passar em todos os dados necessários para o modo de exibição. Os parâmetros são fornecidos pelos hiperlinks **Selecionar** na página.

> [!TIP]
> 
> **Dados de rota**
> 
> Dados de rota são dados que o associador de modelo encontrado em um segmento de URL especificado na tabela de roteamento. Por exemplo, a rota padrão especifica `controller`, `action`, e `id` segmentos:
> 
> routes.MapRoute(  
>  nome: "Padrão"  
>  url: "{controller}/{action}/{id}",  
>  padrões: new {controlador = "Home", ação = "Index", id = UrlParameter.Optional}  
> );
> 
> Na URL a seguir mapeia a rota padrão `Instructor` como o `controller`, `Index` como o `action` e 1 como o `id`; esses são valores de dados de rota.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID = 2021" é um valor de cadeia de caracteres de consulta. O associador de modelo também funcionarão se você passar o `id` como um valor de cadeia de caracteres de consulta:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> As URLs são criadas pelo `ActionLink` instruções no modo de exibição Razor. No código a seguir, o `id` parâmetro corresponde a rota padrão, portanto `id` é adicionada aos dados de rota.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> No código a seguir, `courseID` não corresponde a um parâmetro de rota padrão, portanto, é adicionado como uma cadeia de caracteres de consulta.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


O código começa com a criação de uma instância do modelo de exibição e colocando-a na lista de instrutores. O código especifica o carregamento rápido para o `Instructor.OfficeAssignment` e `Instructor.Courses` propriedade de navegação.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

A segunda `Include` método carrega cursos, e cada curso carregado faz carregamento rápido para o `Course.Department` propriedade de navegação.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Conforme mencionado anteriormente, o carregamento adiantado não é necessário, mas é feito para melhorar o desempenho. Como o modo de exibição sempre requer o `OfficeAssignment` entidade, é mais eficiente buscar que na mesma consulta. `Course` entidades são necessárias quando um instrutor é selecionado na página da web, para que carregamento adiantado é melhor do que o carregamento preguiçoso somente se a página é exibida com mais frequência um curso selecionado que sem.

Se uma ID de instrutor foi selecionada, o instrutor selecionado é recuperado da lista de instrutores no modelo de exibição. O modelo de exibição `Courses` propriedade, em seguida, é carregada com o `Course` entidades que instrutor `Courses` propriedade de navegação.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

O `Where` método retorna uma coleção, mas nesse caso os critérios são passados para o resultado em um único método `Instructor` entidade que está sendo retornada. O `Single` método converte a coleção em uma única `Instructor` entidade, que fornece acesso a essa entidade `Courses` propriedade.

Você usa o [único](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) método em uma coleção quando você souber que a coleção terá apenas um item. O `Single` método lançará uma exceção se a coleção passada para ele está vazia ou se houver mais de um item. Uma alternativa é [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), que retorna um valor padrão (`null` nesse caso) se a coleção está vazia. No entanto, nesse caso isso ainda resultar em uma exceção (de tentativa de encontrar um `Courses` propriedade em um `null` referência), e a mensagem de exceção menos claramente pode indicar a causa do problema. Quando você chama o `Single` método, você também pode passar do `Where` condição em vez de chamar o `Where` método separadamente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Em vez de:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Em seguida, se um curso foi selecionado, o curso selecionado é recuperado na lista de cursos no modelo de exibição. Em seguida, o modelo de exibição `Enrollments` propriedade é carregada com o `Enrollment` entidades que curso `Enrollments` propriedade de navegação.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Modificando a exibição do índice instrutor

Em *Views\Instructor\Index.cshtml*, substitua o código existente com o código a seguir. As alterações são realçadas:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Você fez as seguintes alterações no código existente:

- Alterou a classe de modelo para `InstructorIndexData`.
- Alterou o título de página de **Índice** para **Instrutores**.
- Mover as colunas de link de linha à esquerda.
- Removida a **FullName** coluna.
- Adicionado um **Office** coluna que exibe `item.OfficeAssignment.Location` somente se `item.OfficeAssignment` não for nulo. (Como esta é uma relação um-para-zero-ou-um, talvez não haja um relacionados `OfficeAssignment` entidade.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Código adicionado dinamicamente adicionará `class="selectedrow"` para o `tr` elemento do instrutor selecionado. Isso define uma cor de plano de fundo para a linha selecionada usando a classe CSS que você criou anteriormente. (O `valign` atributo será útil para o tutorial a seguir quando você adiciona uma coluna de várias linhas para a tabela.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Adicionar uma nova `ActionLink` rotulada **selecione** imediatamente antes de outros links em cada linha, que faz com que a ID do instrutor selecionado ser enviado para o `Index` método.

Execute o aplicativo e selecione o **instrutores** guia. A página exibe o `Location` propriedade relacionados `OfficeAssignment` entidades e uma tabela vazia de célula quando há não relacionada `OfficeAssignment` entidade.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

No *Views\Instructor\Index.cshtml* arquivo, após o fechamento `table` elemento (no final do arquivo), adicione o seguinte código realçado. Isso exibe uma lista de cursos relacionados ao instrutor ao instrutor está selecionado.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Esse código lê a propriedade `Courses` do modelo de exibição para exibir uma lista de cursos. Ele também fornece um `Select` hiperlink que envia a ID do curso selecionado para o `Index` método de ação.

> [!NOTE]
> O *. CSS* arquivo é armazenado em cache por navegadores. Se você não vir as alterações quando você executar o aplicativo, faça uma atualização de disco rígida (mantenha pressionada a tecla CTRL enquanto clica o **atualização** botão ou pressione CTRL + F5).


Execute a página e selecione um instrutor. Agora, você verá uma grade que exibe os cursos atribuídos ao instrutor selecionado, e para cada curso, verá o nome do departamento atribuído.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Após o bloco de código que você acabou de adicionar, adicione o código a seguir. Isso exibe uma lista dos alunos que estão registrados em um curso quando esse curso é selecionado.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Esse código lê o `Enrollments` propriedade do modelo de exibição para exibir uma lista dos alunos inscritos no curso.

Execute a página e selecione um instrutor. Em seguida, selecione um curso para ver a lista de alunos registrados e suas notas.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Adicionando o carregamento explícito

Abra *InstructorController.cs* e examine como o `Index` método obtém a lista de registros para um curso selecionado:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Quando você recuperar a lista de instrutores, você especificou o carregamento rápido para o `Courses` propriedade de navegação e para o `Department` propriedade cada curso. Em seguida, você coloca o `Courses` coleção no modelo de exibição, e agora você está acessando o `Enrollments` propriedade de navegação de uma entidade na coleção. Porque você não especificou o carregamento rápido para o `Course.Enrollments` propriedade de navegação, os dados da propriedade é exibido na página como resultado de carregamento lento.

Se você tiver desabilitado o carregamento lento sem alterar o código de outra forma, o `Enrollments` a propriedade deve ser nula, independentemente de quantos registros o curso, na verdade, teve. Nesse caso, para carregar o `Enrollments` propriedade, você precisaria especificar carregamento adiantado ou carregamento explícito. Você já viu como fazer o carregamento rápido. Para ver um exemplo de carregamento explícito, substitua o `Index` método com o código a seguir, que carrega explicitamente o `Enrollments` propriedade. O código alterado são realçadas.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Depois de obter selecionado `Course` entidade, o novo código carrega explicitamente esse curso `Enrollments` propriedade de navegação:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Em seguida, ele carrega explicitamente cada `Enrollment` entidade relacionada `Student` entidade:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Observe que você usar o `Collection` método para carregar uma propriedade de coleção, mas para uma propriedade que contém apenas uma entidade, você deve usar o `Reference` método. Você pode executar a página de índice do instrutor agora e você não verá nenhuma diferença no que é exibido na página, embora você alterou a como os dados são recuperados.

## <a name="summary"></a>Resumo

Agora, você usou todas as três maneiras (lentas, eager e explícitas) para carregar dados relacionados em Propriedades de navegação. No próximo tutorial, você aprenderá a atualizar dados relacionados.

Links para outros recursos do Entity Framework podem ser encontradas no [ASP.NET mapa de conteúdo de acesso de dados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Próximo](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
