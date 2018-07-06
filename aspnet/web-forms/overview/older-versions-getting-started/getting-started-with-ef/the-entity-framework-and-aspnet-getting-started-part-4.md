---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Introdução ao banco de dados do Entity Framework 4.0 First e 4 do ASP.NET Web Forms - parte 4 | Microsoft Docs
author: tdykstra
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 5f8b1c15fbfd2d65b603013db3902b42faa40665
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836782"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Introdução ao banco de dados do Entity Framework 4.0 First e 4 do Web Forms do ASP.NET – parte 4
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial na série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Trabalhando com dados relacionados

No tutorial anterior, você usou o `EntityDataSource` controle para filtrar, classificar e agrupar dados. Neste tutorial, você vai exibir e atualizar dados relacionados.

Você criará a página instrutores que mostra uma lista de instrutores. Quando você seleciona um instrutor, você pode ver uma lista de cursos ministrados por instrutor. Quando você seleciona um curso, você verá os detalhes do curso e uma lista de alunos registrados no curso. Você pode editar o nome do professor, data de contratação e atribuição de escritório. A atribuição de escritório é um conjunto de entidade separada que você acessa por meio de uma propriedade de navegação.

Você pode vincular dados mestre para dados de detalhes na marcação ou no código. Nesta parte do tutorial, você usará os dois métodos.

[![Para Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Exibindo e Atualizando entidades relacionadas em um controle GridView

Criar uma nova página da web denominado *Instructors.aspx* que usa o *Master* página mestra e adicione a seguinte marcação para o `Content` controle chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Essa marcação cria um `EntityDataSource` controle que seleciona os instrutores e permite atualizações. O `div` elemento configura a marcação para renderizar à esquerda, para que você pode adicionar uma coluna à direita, mais tarde.

Entre os `EntityDataSource` marcação e o fechamento `</div>` marca, adicione a seguinte marcação cria um `GridView` controle e um `Label` controle que você usará para mensagens de erro:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Isso `GridView` controle habilita a seleção de linha, realça a linha selecionada com uma cor de plano de fundo cinza claro e especifica manipuladores (que você criará posteriormente) para o `SelectedIndexChanged` e `Updating` eventos. Ela também especifica `PersonID` para o `DataKeyNames` propriedade, para que o valor da chave da linha selecionada possa ser passado para outro controle que você adicionará mais tarde.

A última coluna contém a atribuição de escritório do instrutor, que é armazenada em uma propriedade de navegação do `Person` entidade porque se trata de uma entidade associada. Observe que o `EditItemTemplate` elemento especifica `Eval` em vez de `Bind`, pois o `GridView` controle não é possível associar diretamente às propriedades de navegação para atualizá-los. Você atualizará a atribuição de escritório no código. Para fazer isso, você precisará de uma referência para o `TextBox` controle e você vai obter e salvar isso em de `TextBox` do controle `Init` eventos.

Seguindo a `GridView` controle é um `Label` controle que é usado para mensagens de erro. O controle `Visible` é de propriedade `false`, e o estado de exibição estiver desativado, para que o rótulo será exibida somente quando código se torna visível em resposta a um erro.

Abra o *Instructors.aspx.cs* do arquivo e adicione o seguinte `using` instrução:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Adicione um campo de classe privada imediatamente após a declaração de nome de classe parcial para manter uma referência para a caixa de texto de atribuição do office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Adicionar um stub para o `SelectedIndexChanged` manipulador de eventos que você adicionará código para uso posterior. Também adicionar um manipulador para a atribuição de escritório `TextBox` do controle `Init` evento para que você possa armazenar uma referência para o `TextBox` controle. Você usará essa referência para obter o valor inserido para atualizar a entidade associada com a propriedade de navegação pelo usuário.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Você usará o `GridView` do controle `Updating` eventos para atualizar o `Location` propriedade do associado `OfficeAssignment` entidade. Adicione o seguinte manipulador para o `Updating` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Esse código é executado quando o usuário clica **atualização** em um `GridView` linha. O código usa LINQ to Entities para recuperar o `OfficeAssignment` entidade que está associada com atual `Person` entidade, usando o `PersonID` da linha selecionada do argumento do evento.

O código, em seguida, usa uma das ações a seguir, dependendo do valor no `InstructorOfficeTextBox` controle:

- Se a caixa de texto tem um valor e não há nenhum `OfficeAssignment` entidade a ser atualizada, ela será criada.
- Se a caixa de texto tem um valor e há uma `OfficeAssignment` entidade, ela atualiza o `Location` valor da propriedade.
- Se a caixa de texto está vazia e um `OfficeAssignment` entidade existir, ele exclui a entidade.

Depois disso, ele salva as alterações no banco de dados. Se ocorrer uma exceção, ele exibirá uma mensagem de erro.

Execute a página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Clique em **editar** e alteram de todos os campos para as caixas de texto.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Alterar qualquer um desses valores, incluindo **atribuição de escritório**. Clique em **atualização** e você verá as alterações refletidas na lista.

## <a name="displaying-related-entities-in-a-separate-control"></a>Exibir entidades relacionadas em um controle separado

Cada instrutor pode ministrar cursos de um ou mais, portanto, você adicionará um `EntityDataSource` controle e um `GridView` controle para listar os cursos associados seja qual for o instrutor é selecionado nos instrutores `GridView` controle. Para criar um título e o `EntityDataSource` de controle para entidades de cursos, adicione a seguinte marcação entre a mensagem de erro `Label` controle e o fechamento `</div>` marca:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

O `Where` parâmetro contém o valor da `PersonID` do instrutor cuja linha está selecionada no `InstructorsGridView` controle. O `Where` propriedade contém um comando de Subseleção que obtém todos os respectivos `Person` entidades de uma `Course` da entidade `People` propriedade de navegação e seleciona o `Course` somente se de entidade um do associado `Person`entidades contém selecionado `PersonID` valor.

Para criar o `GridView` controle. Adicione a seguinte marcação imediatamente após o `CoursesEntityDataSource` controle (antes do fechamento `</div>` marca):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Porque nenhum curso será exibido se nenhum instrutor é selecionado, um `EmptyDataTemplate` elemento for incluído.

Execute a página.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Selecione um instrutor que tem um ou mais cursos atribuídos, e o curso ou cursos aparecem na lista. (Observação: Embora o esquema de banco de dados permite que vários cursos, nos dados de teste fornecidos com o banco de dados sem instrutor, na verdade, tem mais de um curso. Você pode adicionar cursos no banco de dados por conta própria usando o **Gerenciador de servidores** janela ou o *CoursesAdd.aspx* página, que você adicionará um tutorial posterior.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

O `CoursesGridView` controle mostra apenas alguns campos do curso. Para exibir todos os detalhes para um curso, você usará um `DetailsView` controle para o curso selecionado pelo usuário. Na *Instructors.aspx*, adicione a marcação a seguir após o fechamento `</div>` marca (Verifique se você colocar essa marcação **depois** a div de fechamento de marca, não antes dele):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Essa marcação cria um `EntityDataSource` que está associado ao controle de `Courses` conjunto de entidades. O `Where` propriedade seleciona um curso usando o `CourseID` o valor da linha selecionada em cursos `GridView` controle. A marcação especifica um manipulador para o `Selected` evento, que você usará posteriormente para exibir as notas de alunos, que é outro nível inferior na hierarquia.

Na *Instructors.aspx.cs*, crie o seguinte stub para o `CourseDetailsEntityDataSource_Selected` método. (Você vai preencher esse stub posteriormente no tutorial; por enquanto, você precisa dela para que a página será compilado e executado).

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Execute a página.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Inicialmente não há nenhum detalhe de curso porque nenhum curso é selecionado. Selecione um instrutor que tem um curso atribuído e, em seguida, selecione um curso para ver os detalhes.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Usar o EntityDataSource "selecionado" evento para exibir dados relacionados

Por fim, você deseja mostrar todos os alunos registrados e suas notas para o curso selecionado. Para fazer isso, você usará o `Selected` eventos do `EntityDataSource` controle vinculado ao curso `DetailsView`.

Na *Instructors.aspx*, adicione a seguinte marcação após o `DetailsView` controle:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Essa marcação cria um `ListView` controle que exibe uma lista de alunos e suas notas para o curso selecionado. Nenhuma fonte de dados é especificado porque você vai databind do controle no código. O `EmptyDataTemplate` elemento fornece uma mensagem a ser exibido quando nenhum curso está selecionado — nesse caso, não há nenhum aluno ser exibido. O `LayoutTemplate` elemento cria uma tabela HTML para exibir a lista e o `ItemTemplate` Especifica as colunas a exibir. A ID do aluno e o aluno que são do `StudentGrade` entidade e o nome do aluno é do `Person` entidade que disponibiliza o Entity Framework na `Person` propriedade de navegação do `StudentGrade` entidade.

Na *Instructors.aspx.cs*, substitua o fora de fazer o stub `CourseDetailsEntityDataSource_Selected` método com o código a seguir:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

O argumento do evento para este evento fornece os dados selecionados na forma de uma coleção, que terá zero itens se nada estiver selecionado ou um item se um `Course` entidade é selecionada. Se um `Course` entidade é selecionada, o código usa o `First` método para converter a coleção para um único objeto. Em seguida, ele obtém `StudentGrade` converte-os em uma coleção de entidades da propriedade de navegação e associa o `GradesListView` controle à coleção.

Isso é suficiente exibir as notas de um, mas você deseja certificar-se de que a mensagem no modelo de dados vazio é exibida na primeira vez em que a página é exibida e sempre que um curso não estiver selecionado. Para fazer isso, crie o seguinte método, você chamará de dois locais:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Chame esse novo método da `Page_Load` método para exibir a hora de modelo, o primeiro de dados vazio a página é exibida. E chamá-lo na `InstructorsGridView_SelectedIndexChanged` método porque esse evento é gerado quando um instrutor é selecionado, o que significa que novos cursos são carregados no cursos `GridView` controle e nenhum está selecionado ainda. Aqui estão as duas chamadas:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Execute a página.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Selecione um instrutor que tem um curso atribuído e, em seguida, selecione o curso.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Agora você já viu algumas maneiras de trabalhar com dados relacionados. No tutorial a seguir, você aprenderá a adicionar relações entre entidades existentes, como remover relações e como adicionar uma nova entidade que tem uma relação com uma entidade existente.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-5.md)
