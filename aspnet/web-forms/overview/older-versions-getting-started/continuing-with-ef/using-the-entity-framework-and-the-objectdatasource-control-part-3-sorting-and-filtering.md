---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Usando o Entity Framework 4.0 e o controle ObjectDataSource, parte 3: classificando e filtrando | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais se baseia no aplicativo web Contoso University que é criado pelo Getting Started with a série de tutoriais do Entity Framework 4.0. EU...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 71e26dc9a63131842daf4aa1549d761690b723ea
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830389"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Usando o Entity Framework 4.0 e o controle ObjectDataSource, parte 3: classificando e filtrando
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais baseia-se no aplicativo web Contoso University que é criado pela [Introdução ao Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de tutoriais. Se você não concluir os tutoriais anteriores, como um ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você teria criado. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa. Se você tiver dúvidas sobre os tutoriais, você pode postá-los para o [Fórum do Entity Framework do ASP.NET](https://forums.asp.net/1227.aspx).


No tutorial anterior, você implementou o padrão de repositório em um aplicativo web de n camadas que usa o Entity Framework e o `ObjectDataSource` controle. Este tutorial mostra como fazer a classificação e filtragem e lidar com cenários de detalhes mestre. Você adicionará os seguintes aprimoramentos para o *Departments.aspx* página:

- Uma caixa de texto para permitir que os usuários selecionem departamentos por nome.
- Uma lista de cursos para cada departamento, que é mostrada na grade.
- A capacidade de classificar clicando em títulos de coluna.

[![Para Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Adicionando a capacidade para classificar colunas de GridView

Abra o *Departments.aspx* da página e adicionar um `SortParameterName="sortExpression"` de atributo para o `ObjectDataSource` controle chamado `DepartmentsObjectDataSource`. (Posteriormente, você criará um `GetDepartments` método que usa um parâmetro chamado `sortExpression`.) A marcação para a marca de abertura do controle agora se parece com o exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Adicione a `AllowSorting="true"` atributo à marca de abertura do `GridView` controle. A marcação para a marca de abertura do controle agora se parece com o exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

Na *Departments.aspx.cs*, definir a ordem de classificação padrão chamando o `GridView` do controle `Sort` método a partir de `Page_Load` método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Você pode adicionar código que classifica ou filtra na classe de lógica de negócios ou a classe de repositório. Se você fizer isso na classe de lógica de negócios, classificação ou filtragem de trabalho será feito depois que os dados são recuperados do banco de dados, porque a classe de lógica de negócios está trabalhando com um `IEnumerable` objeto retornado pelo repositório. Se você adicionar a classificação e filtragem de código na classe de repositório e fazer isso antes de uma expressão LINQ ou consulta de objeto foi convertida em um `IEnumerable` do objeto, os comandos serão transmitidos para o banco de dados para processamento, que é geralmente mais eficiente. Neste tutorial você implementará classificação e filtragem de forma que faz com que o processamento a ser feito pelo banco de dados — ou seja, no repositório.

Para adicionar o recurso de classificação, você deve adicionar um novo método para a interface de repositório e classes de repositório, bem como para a classe de lógica de negócios. No *ISchoolRepository.cs* do arquivo, adicione uma nova `GetDepartments` método que usa um `sortExpression` parâmetro que será usado para classificar a lista de departamentos que é retornada:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

O `sortExpression` parâmetro especificará a coluna para a classificação e a direção de classificação.

Adicione código para o novo método para o *SchoolRepository.cs* arquivo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Alterar existente sem parâmetros `GetDepartments` método para chamar o novo método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

No projeto de teste, adicione o seguinte método novo à *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Se você fosse criar os testes de unidade que dependem desse método retornar uma lista classificada, você precisa classificar a lista antes de retorná-lo. Você não criar testes como esse neste tutorial, para que o método pode apenas retornar a lista não classificada de departamentos.

No *SchoolBL.cs* de arquivo, adicione o seguinte método à classe de lógica de negócios:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Esse código passa o parâmetro de classificação para o método de repositório.

Execute o *Departments.aspx* página.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Agora você pode clicar em qualquer cabeçalho de coluna para classificar por essa coluna. Se a coluna já está classificada, clicando no título inverte a direção de classificação.

## <a name="adding-a-search-box"></a>Adicionando uma caixa de pesquisa

Nesta seção você irá adicionar uma caixa de texto de pesquisa, vinculá-lo para o `ObjectDataSource` controlar usando um parâmetro de controle e adicionar um método à classe de lógica de negócios para dar suporte à filtragem.

Abra o *Departments.aspx* da página e adicione a seguinte marcação entre o título e o primeiro `ObjectDataSource` controle:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

No `ObjectDataSource` controle denominado `DepartmentsObjectDataSource`, faça o seguinte:

- Adicionar um `SelectParameters` elemento para um parâmetro denominado `nameSearchString` que obtém o valor inserido no `SearchTextBox` controle.
- Alterar o `SelectMethod` valor de atributo `GetDepartmentsByName`. (Você criará esse método posteriormente.)

A marcação para o `ObjectDataSource` controle agora é semelhante ao exemplo a seguir:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

Na *ISchoolRepository.cs*, adicione uma `GetDepartmentsByName` método que usa ambos `sortExpression` e `nameSearchString` parâmetros:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

Na *SchoolRepository.cs*, adicione o seguinte método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Esse código usa um `Where` método para selecionar os itens que contêm a cadeia de caracteres de pesquisa. Se a cadeia de caracteres de pesquisa está vazia, todos os registros serão selecionados. Observe que, quando você especifica o método chama juntos em uma instrução como esta (`Include`, em seguida, `OrderBy`, em seguida, `Where`), o `Where` método sempre deve ser o último.

Alterar existente `GetDepartments` método que usa um `sortExpression` parâmetro ao chamar o novo método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

Na *MockSchoolRepository.cs* no projeto de teste, adicione o seguinte método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

Na *SchoolBL.cs*, adicione o seguinte método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Execute o *Departments.aspx* página e insira uma cadeia de caracteres de pesquisa para certificar-se de que a lógica de seleção funciona. Deixe a caixa de texto vazia e tente fazer uma pesquisa para certificar-se de que todos os registros são retornados.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Adicionando uma coluna de detalhes para cada linha de grade

Em seguida, você deseja ver todos os cursos para cada departamento exibido na célula à direita da grade. Para fazer isso, você usará um aninhados `GridView` databind e controle-o para dados do `Courses` propriedade de navegação do `Department` entidade.

Abra *Departments.aspx* e na marcação para o `GridView` controlar, especificar um manipulador para o `RowDataBound` eventos. A marcação para a marca de abertura do controle agora se parece com o exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Adicione um novo `TemplateField` elemento após o `Administrator` campo de modelo:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Essa marcação cria um aninhada `GridView` controle que mostra o número e o título de uma lista de cursos. Ele não especifica uma fonte de dados porque você vai databind-lo no código no `RowDataBound` manipulador.

Abra *Departments.aspx.cs* e adicione o seguinte manipulador para o `RowDataBound` evento:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Esse código obtém os `Department` converte a entidade de argumentos do evento, o `Courses` propriedade de navegação para um `List` coleta e databinds aninhada `GridView` à coleção.

Abra o *SchoolRepository.cs* de arquivo e especifique o carregamento adiantado para a `Courses` propriedade de navegação, chamando o `Include` método na consulta de objeto que você cria no `GetDepartmentsByName` método. O `return` instrução no `GetDepartmentsByName` método agora é semelhante ao exemplo a seguir.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Execute a página. Além da classificação e filtragem de recurso que você adicionou anteriormente, o controle GridView agora mostra detalhes do curso aninhado para cada departamento.

[![Para Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Isso conclui a introdução aos cenários de filtragem, classificação e master-detail. O próximo tutorial, você verá como manipular a simultaneidade.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Próximo](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
