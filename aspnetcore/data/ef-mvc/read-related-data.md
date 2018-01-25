---
title: "Núcleo do ASP.NET MVC com núcleo EF - ler dados relacionados - 6 de 10"
author: tdykstra
description: "Neste tutorial você ler e exibir dados relacionados – ou seja, os dados que o Entity Framework carrega em Propriedades de navegação."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 2333ac70c77847ece1f90c9ff22eec30bc35fea1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a>Leitura relacionadas a dados - Core de EF com o tutorial do MVC do ASP.NET Core (6 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).

No tutorial anterior, você deve concluir o modelo de dados de escola. Neste tutorial, você ler e exibir dados relacionados – ou seja, os dados que o Entity Framework carrega em Propriedades de navegação.

As ilustrações a seguir mostram as páginas que você trabalhará com.

![Página de índice de cursos](read-related-data/_static/courses-index.png)

![Página de índice instrutores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Carregamento de dados relacionados lento eager e explícita

Há várias maneiras que o software de mapeamento relacional de objeto (ORM), como o Entity Framework pode carregar os dados relacionados nas propriedades de navegação de uma entidade:

* Carregamento adiantado. Quando a entidade é lido, os dados relacionados são recuperados com ela. Normalmente, isso resulta em uma consulta de junção único que recupera todos os dados que é necessário. Especifique o carregamento adiantado no Entity Framework Core usando o `Include` e `ThenInclude` métodos.

  ![Exemplo de carregamento rápido](read-related-data/_static/eager-loading.png)

  Você pode recuperar os dados em consultas separadas e EF "correções de" as propriedades de navegação.  Ou seja, EF adiciona automaticamente as entidades separadamente recuperadas onde eles pertencem nas propriedades de navegação de entidades previamente recuperadas. Para a consulta que recupera dados relacionados, você pode usar o `Load` método em vez de um método que retorna uma lista ou um objeto, como `ToList` ou `Single`.

  ![Exemplo de consultas separadas](read-related-data/_static/separate-queries.png)

* Carregamento explícito. Quando a entidade é lido pela primeira vez, os dados relacionados não são recuperados. Você escreve o código que recupera os dados relacionados a se ela for necessária. Como no caso de carregamento rápido com consultas separadas, explícita de carregamento dos resultados em várias consultas enviadas ao banco de dados. A diferença é que, com carregamento explícito, o código especifica as propriedades de navegação para ser carregado. No Entity Framework Core 1.1, você pode usar o `Load` método de fazer o carregamento explícito. Por exemplo:

  ![Exemplo de carregamento explícito](read-related-data/_static/explicit-loading.png)

* Carregamento preguiçoso. Quando a entidade é lido pela primeira vez, os dados relacionados não são recuperados. No entanto, na primeira vez que tentar acessar uma propriedade de navegação, os dados necessários para essa propriedade de navegação são recuperados automaticamente. Uma consulta é enviada para o banco de dados cada vez que tentar obter dados de uma propriedade de navegação pela primeira vez. Entity Framework Core 1.0 não oferece suporte a carregamento lento.

### <a name="performance-considerations"></a>Considerações sobre desempenho

Se você souber que você precisa de dados relacionados para cada entidade recuperada, carregamento adiantado geralmente oferece o melhor desempenho, porque uma única consulta enviada para o banco de dados é geralmente mais eficiente do que consultas separadas para cada entidade recuperada. Por exemplo, suponha que cada departamento tem dez cursos relacionados. Carregamento adiantado de todos os dados relacionados resultaria em apenas uma consulta única (associação) e uma única viagem de ida e ao banco de dados. Uma consulta separada para cursos para cada departamento resultaria em onze idas e voltas ao banco de dados. As extras idas e voltas para o banco de dados são especialmente prejudiciais ao desempenho quando a latência é alta.

Por outro lado, em alguns cenários consultas separadas é mais eficiente. Carregamento adiantado de todos os dados relacionados em uma consulta pode causar uma junção muito complexa para ser gerado, que o SQL Server não pode processar com eficiência. Ou, se você precisar acessar as propriedades de navegação de uma entidade somente para um subconjunto de um conjunto de entidades que você está processando, consultas separadas podem executar melhor, pois mais dados que você precisa recuperar o carregamento rápido de tudo, desde o início. Se o desempenho for crítico, é melhor testar o desempenho das duas maneiras para fazer a melhor escolha.

## <a name="create-a-courses-page-that-displays-department-name"></a>Criar uma página de cursos que exibe o nome do departamento

A entidade de curso inclui uma propriedade de navegação que contém a entidade de departamento do departamento de curso é atribuído a. Para exibir o nome do departamento atribuído em uma lista de cursos, você precisa obter a propriedade de nome da entidade de departamento que está no `Course.Department` propriedade de navegação.

Criar um controlador chamado CoursesController para o tipo de entidade do curso, usando as mesmas opções para o **controlador MVC com modos de exibição usando o Entity Framework** scaffolder que você usou anteriormente para o controlador de alunos, conforme o ilustração a seguir:

![Adicionar controlador de cursos](read-related-data/_static/add-courses-controller.png)

Abra *CoursesController.cs* e examine o `Index` método. O scaffolding automática tiver especificado o carregamento rápido para o `Department` propriedade de navegação usando o `Include` método.

Substitua o `Index` método com o código a seguir que usa um nome mais apropriado para o `IQueryable` que retorna as entidades de curso (`courses` em vez de `schoolContext`):

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

Abra *Views/Courses/Index.cshtml* e substitua o código de modelo com o código a seguir. As alterações são realçadas:

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Você fez as alterações a seguir para o código de scaffolding:

* Alterado o título do índice para cursos.

* Adicionado um **número** coluna mostra o `CourseID` o valor da propriedade. Por padrão, as chaves primárias não são Scaffold porque normalmente eles são sem sentido para os usuários finais. No entanto, nesse caso, a chave primária é significativa e você deseja mostrá-la.

* Alterado o **departamento** coluna para exibir o nome de departamento. Exibe o código de `Name` propriedade da entidade do departamento que é carregada no `Department` propriedade de navegação:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Execute o aplicativo e selecione o **cursos** guia para ver a lista com nomes de departamento.

![Página de índice de cursos](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Criar uma página de instrutores que mostra os cursos e registros

Nesta seção, você criará um controlador e o modo de exibição para a entidade instrutor para exibir a página de professores:

![Página de índice instrutores](read-related-data/_static/instructors-index.png)

Essa página lê e exibe dados relacionados das seguintes maneiras:

* A lista de instrutores exibe dados relacionados da entidade OfficeAssignment. As entidades instrutor e OfficeAssignment estão em uma relação um-para-zero-ou-um. Você usará o carregamento rápido para as entidades de OfficeAssignment. Conforme explicado anteriormente, o carregamento adiantado é geralmente mais eficiente quando você precisa dos dados relacionados para todas as linhas recuperadas da tabela primária. Nesse caso, você deseja exibir atribuições office para todos os instrutores exibidos.

* Quando o usuário seleciona um instrutor, entidades de curso relacionadas são exibidas. As entidades de curso e instrutor estão em uma relação muitos-para-muitos. Você usará o carregamento rápido para as entidades do curso e suas entidades relacionadas do departamento. Nesse caso, consultas separadas podem ser mais eficientes porque você precisa cursos somente para o instrutor selecionado. No entanto, este exemplo mostra como usar o carregamento rápido para propriedades de navegação dentro de entidades que são nas propriedades de navegação.

* Quando o usuário seleciona um curso, dados relacionados de conjunto de registros de entidades são exibidos. As entidades de curso e registro estão em uma relação um-para-muitos. Você usará consultas separadas para entidades de registro e suas entidades relacionadas do aluno.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Criar um modelo de exibição para o modo de exibição do índice do instrutor

A página de instrutores mostra dados de três tabelas diferentes. Portanto, você criará um modelo de exibição que inclui três propriedades, cada uma contém os dados para uma das tabelas.

No *SchoolViewModels* pasta, criar *InstructorIndexData.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Criar o controlador de instrutor e modos de exibição

Crie um controlador de instrutores com ações de leitura/gravação EF conforme mostrado na ilustração a seguir:

![Adicionar controlador instrutores](read-related-data/_static/add-instructors-controller.png)

Abra *InstructorsController.cs* e adicione um usando a instrução para o namespace ViewModels:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

Substitua o método de índice com o código a seguir para fazer o carregamento rápido de dados relacionados e colocá-lo no modelo de exibição.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

O método aceita dados de rota opcional (`id`) e um parâmetro de cadeia de caracteres de consulta (`courseID`) que fornecem os valores de ID do curso selecionado e instrutor selecionado. Os parâmetros fornecidos pelo **selecione** hiperlinks na página.

O código começa com a criação de uma instância do modelo de exibição e colocando-a lista de professores. O código especifica o carregamento rápido para o `Instructor.OfficeAssignment` e `Instructor.CourseAssignments` propriedades de navegação. Dentro de `CourseAssignments` propriedade, o `Course` propriedade é carregado e dentro desse, o `Enrollments` e `Department` propriedades são carregados e dentro de cada `Enrollment` entidade o `Student` propriedade é carregada.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

Como o modo de exibição sempre requer que a entidade OfficeAssignment, é mais eficiente buscar que na mesma consulta. Entidades de curso são necessárias quando instrutor é selecionado na página da web, portanto, uma única consulta é melhor do que várias consultas apenas se a página é exibida com mais frequência um curso selecionado que sem.

O código repete `CourseAssignments` e `Course` porque você precisa de duas propriedades de `Course`. A primeira cadeia de caracteres de `ThenInclude` chama obtém `CourseAssignment.Course`, `Course.Enrollments`, e `Enrollment.Student`.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

Nesse ponto no código, outra `ThenInclude` seria para propriedades de navegação de `Student`, que não é necessário. Mas chamada `Include` será reiniciada com `Instructor` propriedades, você precisa passar por cadeia novamente, especificando essa hora `Course.Department` em vez de `Course.Enrollments`.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

O código a seguir executa quando instrutor foi selecionado. O instrutor selecionado é recuperado da lista de instrutores no modelo de exibição. O modelo de exibição `Courses` propriedade, em seguida, é carregada com as entidades curso que instrutor `CourseAssignments` propriedade de navegação.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

O `Where` método retorna uma coleção, mas nesse caso os critérios passado para esse método resultam em apenas uma única entidade instrutor que está sendo retornada. O `Single` método converte a coleção em uma única entidade instrutor, que fornece acesso a essa entidade `CourseAssignments` propriedade. O `CourseAssignments` propriedade contém `CourseAssignment` entidades, do qual você deseja apenas relacionado `Course` entidades.

Você usa o `Single` método em uma coleção quando você souber que a coleção terá apenas um item. O único método gera uma exceção se a coleção passada para ele está vazia ou se houver mais de um item. Uma alternativa é `SingleOrDefault`, que retorna um valor padrão (null nesse caso) se a coleção está vazia. No entanto, nesse caso isso ainda resultar em uma exceção (de tentativa de encontrar um `Courses` propriedade em uma referência nula), e a mensagem de exceção menos claramente pode indicar a causa do problema. Quando você chama o `Single` método, você também pode passar no local em que a condição em vez de chamar o `Where` método separadamente:

```csharp
.Single(i => i.ID == id.Value)
```

Em vez de:

```csharp
.Where(I => i.ID == id.Value).Single()
```

Em seguida, se um curso foi selecionado, o curso selecionado é recuperado da lista de cursos no modelo de exibição. Em seguida, o modelo de exibição `Enrollments` propriedade é carregada com as entidades registro desse curso `Enrollments` propriedade de navegação.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a>Modificar a exibição do índice do instrutor

Em *Views/Instructors/Index.cshtml*, substitua o código de modelo com o código a seguir. As alterações são realçadas.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

As seguintes alterações feitas no código existente:

* Alterar a classe de modelo para `InstructorIndexData`.

* Alterar o título da página de **índice** para **instrutores**.

* Adicionado um **Office** coluna que exibe `item.OfficeAssignment.Location` somente se `item.OfficeAssignment` não é nulo. (Como esta é uma relação um-para-zero-ou-um, pode não haver uma entidade relacionada de OfficeAssignment.)

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Adicionado um **cursos** coluna que exibe os cursos ministrada por cada instrutor. Consulte [explícita transição de linha com `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) para obter mais informações sobre essa sintaxe do razor.

* Código adicionado dinamicamente adiciona `class="success"` para o `tr` elemento do instrutor selecionado. Isso define uma cor de plano de fundo para a linha selecionada usando uma classe de inicialização.

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Adicionado um novo hiperlink rotulado **selecione** imediatamente antes de outros links em cada linha, que faz com que a ID do instrutor selecionado a ser enviado para o `Index` método.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Execute o aplicativo e selecione o **instrutores** guia. A página exibe a propriedade Location de entidades relacionadas de OfficeAssignment e uma célula de tabela vazia quando não houver nenhuma entidade OfficeAssignment relacionada.

![Página de índice instrutores que nada selecionado](read-related-data/_static/instructors-index-no-selection.png)

No *Views/Instructors/Index.cshtml* arquivo, após o fechamento da tabela elemento (no final do arquivo), adicione o código a seguir. Este código exibe uma lista de cursos relacionados ao instrutor ao instrutor está selecionado.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

Esse código lê o `Courses` propriedade do modelo de exibição para exibir uma lista de cursos. Ele também fornece um **selecione** hiperlink que envia a ID do curso selecionado para o `Index` método de ação.

Atualize a página e selecionar um instrutor. Agora você verá uma grade que exibe os cursos atribuídos para o instrutor selecionado, e cada curso você ver o nome do departamento atribuído.

![Instrutor de página de índice instrutores selecionado](read-related-data/_static/instructors-index-instructor-selected.png)

Após o bloco de código que você acabou de adicionar, adicione o código a seguir. Isso exibe uma lista dos alunos que são registrados em um curso quando curso está selecionado.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

Esse código lê a propriedade de registros do modelo de exibição para exibir uma lista dos alunos inscritos no curso.

Atualize a página novamente e selecione um instrutor. Em seguida, selecione um curso para ver a lista de estudantes registrados e suas classificações.

![Instrutor de página de índice professores e curso selecionado](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a>Carregamento explícito

Quando você recuperou a lista de instrutores em *InstructorsController.cs*, você especificou o carregamento rápido para o `CourseAssignments` propriedade de navegação.

Suponha que o esperado usuários raramente deseja ver registros em um curso e um instrutor selecionado. Nesse caso, você talvez queira carregar os dados de registro somente se eles forem solicitados. Para ver um exemplo de como fazer carregamento explícito, substitua o `Index` método com o código a seguir, que remove o carregamento rápido para registros e os carrega essa propriedade explicitamente. As alterações de código são realçadas.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

O novo código descarta o *ThenInclude* método chama para dados de registro do código que recupera as entidades do instrutor. Se um curso e instrutor for selecionadas, o código realçado recupera entidades de registro para o curso selecionado e entidades de estudante para cada registro.

Execute que o aplicativo, vá para a página de índice instrutores agora e você não verá nenhuma diferença no que é exibido na página, embora você alterou a como os dados são recuperados.

## <a name="summary"></a>Resumo

Você agora usou carregamento adiantado com uma consulta e com várias consultas para ler os dados relacionados em Propriedades de navegação. O seguinte tutorial, você aprenderá como atualizar dados relacionados.

>[!div class="step-by-step"]
>[Anterior](complex-data-model.md)
>[Próximo](update-related-data.md)  
