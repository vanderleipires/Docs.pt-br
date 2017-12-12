---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: "Introdução ao banco de dados do Entity Framework 4.0 primeiro e o ASP.NET 4 Web Forms - parte 4 | Microsoft Docs"
author: tdykstra
description: "O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 06d129384fc78db21ad1b9224781deab6a0e91a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Introdução ao banco de dados do Entity Framework 4.0 primeiro e 4 Web Forms do ASP.NET - parte 4
====================
Por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Trabalhando com dados relacionados

No tutorial anterior, você usou o `EntityDataSource` controle para filtrar, classificar e agrupar dados. Neste tutorial você exibir e atualizar dados relacionados.

Você criará a página de instrutores que mostra uma lista de professores. Quando você seleciona um instrutor, verá uma lista de cursos ministrada por esse instrutor. Quando você seleciona um curso, você pode ver os detalhes do curso e uma lista dos alunos inscritos no curso. Você pode editar o nome do professor, data de contratação e atribuição do office. A atribuição do office é um conjunto de entidade separada que você acessa por meio de uma propriedade de navegação.

Você pode vincular dados mestre para dados de detalhes na marcação ou no código. Nesta parte do tutorial, você usará os dois métodos.

[![Para Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Exibir e atualizar entidades relacionadas em um controle GridView

Criar uma nova página da web denominado *Instructors.aspx* que usa o *Site.Master* página mestra e adicione a seguinte marcação para o `Content` controle chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Essa marcação cria um `EntityDataSource` controle que seleciona professores e permite atualizações. O `div` elemento configura marcação para renderizar à esquerda para que você pode adicionar uma coluna à direita posteriormente.

Entre o `EntityDataSource` marcação e o fechamento `</div>` marca, adicione a seguinte marcação que cria um `GridView` controle e um `Label` controle que você usará para mensagens de erro:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Isso `GridView` controle permite a seleção de linha, realça a linha selecionada com uma cor de plano de fundo cinza claro e especifica manipuladores (que você criará mais tarde) para o `SelectedIndexChanged` e `Updating` eventos. Ela também especifica `PersonID` para o `DataKeyNames` propriedade, para que o valor da chave da linha selecionada pode ser passado para um outro controle que você adicionará posteriormente.

A última coluna contém a atribuição do office do instrutor, que é armazenada em uma propriedade de navegação do `Person` entidade porque se trata de uma entidade associada. Observe que o `EditItemTemplate` elemento especifica `Eval` em vez de `Bind`, pois o `GridView` controle não é possível associar diretamente às propriedades de navegação para atualizá-los. Você atualizará a atribuição do office no código. Para fazer isso, você precisará de uma referência para o `TextBox` controle e você irá obter e salvar que o `TextBox` do controle `Init` eventos.

Após o `GridView` controle é um `Label` controle que é usado para mensagens de erro. O controle `Visible` é de propriedade `false`, e o estado de exibição estiver desativado, para que o rótulo será exibido apenas quando código torna visível em resposta a um erro.

Abra o *Instructors.aspx.cs* de arquivo e adicione o seguinte `using` instrução:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Adicione um campo de classe privada imediatamente após a declaração de nome de classe parcial para manter uma referência para a caixa de texto de atribuição do office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Adicionar um stub para o `SelectedIndexChanged` manipulador de eventos que você adicionará código para mais tarde. Também adicione um manipulador para a atribuição do office `TextBox` do controle `Init` evento para que você pode armazenar uma referência para o `TextBox` controle. Você usará essa referência para obter o valor inserido para atualizar a entidade associada com a propriedade de navegação pelo usuário.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Você usará o `GridView` do controle `Updating` evento para atualizar o `Location` propriedade associado `OfficeAssignment` entidade. Adicione o seguinte manipulador para o `Updating` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Esse código é executado quando o usuário clica **atualização** em um `GridView` linha. O código usa LINQ to Entities para recuperar o `OfficeAssignment` entidade associado atual `Person` entidade, usando o `PersonID` da linha selecionada do argumento do evento.

O código, em seguida, usa uma das ações a seguir, dependendo do valor no `InstructorOfficeTextBox` controle:

- Se a caixa de texto tem um valor e não há nenhum `OfficeAssignment` entidade a ser atualizada, ela criará um.
- Se a caixa de texto tem um valor e há um `OfficeAssignment` entidade, ele atualizará o `Location` o valor da propriedade.
- Se a caixa de texto está vazia e uma `OfficeAssignment` entidade existe, ele exclui a entidade.

Depois disso, ele salva as alterações no banco de dados. Se ocorrer uma exceção, ele exibe uma mensagem de erro.

Execute a página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Clique em **editar** e alteram todos os campos para as caixas de texto.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Alterar qualquer um desses valores, inclusive **Office atribuição**. Clique em **atualização** e você verá as alterações refletidas na lista.

## <a name="displaying-related-entities-in-a-separate-control"></a>Exibir entidades relacionadas em um controle separado

Cada instrutor pode ensinar cursos de um ou mais, portanto, você adicionará um `EntityDataSource` controle e um `GridView` controle para listar os cursos associados com qualquer instrutor está selecionada nos instrutores `GridView` controle. Para criar um título e o `EntityDataSource` controlar para entidades de cursos, adicione a seguinte marcação entre a mensagem de erro `Label` controle e o fechamento `</div>` marca:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

O `Where` parâmetro contém o valor da `PersonID` do instrutor cuja linha está selecionada no `InstructorsGridView` controle. O `Where` propriedade contém um comando de Subseleção que obtém todos os respectivos `Person` entidades de um `Course` da entidade `People` propriedade de navegação e seleciona o `Course` somente se de entidade um o associado`Person`entidades contém selecionado `PersonID` valor.

Para criar o `GridView` controle., adicione a seguinte marcação imediatamente após o `CoursesEntityDataSource` controle (antes do fechamento `</div>` marca):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Porque nenhum curso será exibido se nenhum instrutor for selecionada, um `EmptyDataTemplate` elemento é incluído.

Execute a página.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Selecione um instrutor que tem um ou mais cursos atribuídos, e o curso ou cursos aparecem na lista. (Observação: Embora o esquema de banco de dados permite que vários cursos, nos dados de teste fornecidos com o banco de dados não instrutor realmente tem mais de um curso. Você pode adicionar o cursos para o banco de dados por conta própria usando o **Server Explorer** janela ou *CoursesAdd.aspx* página, que você adicionará um tutorial posterior.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

O `CoursesGridView` controle mostra apenas alguns campos do curso. Para exibir todos os detalhes de um curso, você usará um `DetailsView` controle do curso selecionado pelo usuário. Em *Instructors.aspx*, adicione a seguinte marcação após o fechamento `</div>` marca (certifique-se de colocar essa marcação **depois** marca div fechamento, não antes):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Essa marcação cria um `EntityDataSource` controle associado para o `Courses` conjunto de entidades. O `Where` propriedade seleciona um curso usando o `CourseID` valor da linha selecionada em cursos `GridView` controle. A marcação especifica um manipulador para o `Selected` evento, que você usará mais tarde para exibir notas de alunos, que é um nível inferior na hierarquia.

Em *Instructors.aspx.cs*, crie o seguinte stub para o `CourseDetailsEntityDataSource_Selected` método. (Você vai preencher esse stub posteriormente no tutorial; por agora, necessário para que a página irá compilar e executar).

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Execute a página.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Inicialmente não há nenhum detalhe de curso porque nenhum curso está selecionado. Selecione um instrutor que tem um curso atribuído e, em seguida, selecione um curso para ver os detalhes.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Usar EntityDataSource "" evento selecionado para exibir dados relacionados

Por fim, você deseja mostrar todos os alunos registrados e suas classificações do curso selecionado. Para fazer isso, você usará o `Selected` evento o `EntityDataSource` controle associado ao curso `DetailsView`.

Em *Instructors.aspx*, adicione a seguinte marcação após o `DetailsView` controle:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Essa marcação cria um `ListView` controle que exibe uma lista de alunos e suas classificações do curso selecionado. Nenhuma fonte de dados é especificado porque você vai databind o controle no código. O `EmptyDataTemplate` elemento fornece uma mensagem a ser exibida quando nenhum curso é selecionado, nesse caso, não há nenhum alunos para exibir. O `LayoutTemplate` elemento cria uma tabela HTML para exibir a lista e o `ItemTemplate` Especifica as colunas a exibir. A ID do estudante e a classificação do aluno são do `StudentGrade` entidade e o nome do aluno é do `Person` entidade que o Entity Framework disponibiliza no `Person` propriedade de navegação a `StudentGrade` entidade.

Em *Instructors.aspx.cs*, substitua o limite fragmentado `CourseDetailsEntityDataSource_Selected` método com o código a seguir:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

O argumento do evento para esse evento fornece os dados selecionados na forma de uma coleção, que terá zero itens se nada estiver selecionado ou um item se um `Course` entidade é selecionada. Se um `Course` entidade é selecionada, o código usa o `First` método para converter a coleção para um único objeto. Em seguida, ele obtém `StudentGrade` entidades da propriedade de navegação, converte-os em uma coleção e associa o `GradesListView` controle à coleção.

Isso é suficiente exibir notas, mas você deseja certificar-se de que a mensagem no modelo de dados vazio é exibida na primeira vez em que a página é exibida e sempre que um curso não estiver selecionado. Para fazer isso, crie o seguinte método, você chamará em dois locais:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Chame esse novo método do `Page_Load` método para exibir o tempo de primeiro do modelo de dados vazio a página é exibida. E chamá-la do `InstructorsGridView_SelectedIndexChanged` método porque esse evento é gerado quando um instrutor é selecionado, que significa que novos cursos são carregados nos cursos `GridView` controle e nenhum foi selecionado. Aqui estão as duas chamadas:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Execute a página.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Selecione um instrutor que tem um curso atribuído e, em seguida, selecione o curso.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Agora você já viu algumas maneiras de trabalhar com dados relacionados. O tutorial a seguir, você aprenderá a adicionar relações entre entidades existentes, como remover relações e como adicionar uma nova entidade que tem uma relação com uma entidade existente.

>[!div class="step-by-step"]
[Anterior](the-entity-framework-and-aspnet-getting-started-part-3.md)
[Próximo](the-entity-framework-and-aspnet-getting-started-part-5.md)
