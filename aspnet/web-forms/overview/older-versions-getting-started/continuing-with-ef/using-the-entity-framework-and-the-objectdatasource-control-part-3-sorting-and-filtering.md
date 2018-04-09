---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Usando o Entity Framework 4.0 e o controle ObjectDataSource, parte 3: classificação e filtragem | Microsoft Docs'
author: tdykstra
description: Esta série de tutorial se baseia no aplicativo web Contoso Universidade que é criado pelo guia de Introdução com a série de tutoriais do Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: e412d3ad98a37931e7190a4909cb09fa2abfb3d0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Usando o Entity Framework 4.0 e o controle ObjectDataSource, parte 3: classificação e filtragem
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais se baseia no aplicativo da web Contoso Universidade que é criado pelo [guia de Introdução com o Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de tutoriais. Se você não concluir os tutoriais anteriores, como um ponto de partida para este tutorial você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você pode ter sido criado. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série tutorial completo. Se você tiver dúvidas sobre os tutoriais, você poderá postá-los para o [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


No tutorial anterior você implementou o padrão de repositório em um aplicativo de n camadas que usa o Entity Framework e o `ObjectDataSource` controle. Este tutorial mostra como fazer a classificação e filtragem e lidar com cenários de detalhes mestre. Você adicionará os seguintes aprimoramentos para o *Departments.aspx* página:

- Uma caixa de texto para permitir que os usuários selecionem departamentos por nome.
- Uma lista de cursos para cada departamento que é mostrada na grade.
- A capacidade de classificar clicando em títulos de coluna.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Adicionando a capacidade de classificar colunas do GridView

Abra o *Departments.aspx* página e adicione um `SortParameterName="sortExpression"` de atributo para o `ObjectDataSource` controle chamado `DepartmentsObjectDataSource`. (Posteriormente, você criará uma `GetDepartments` método que utiliza um parâmetro chamado `sortExpression`.) A marcação para a marca de abertura do controle agora se parece com o exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Adicionar o `AllowSorting="true"` a marca de abertura do atributo de `GridView` controle. A marcação para a marca de abertura do controle agora se parece com o exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

Em *Departments.aspx.cs*, definir a ordem de classificação padrão chamando o `GridView` do controle `Sort` método do `Page_Load` método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Você pode adicionar o código que classifica ou filtros na classe de lógica de negócios ou a classe do repositório. Se você pode fazer isso na classe de lógica de negócios, classificação ou filtragem de trabalho será feito depois que os dados são recuperados do banco de dados, porque a classe de lógica de negócios está trabalhando com um `IEnumerable` objeto retornado pelo repositório. Se você adicionar a classificação e filtragem de código na classe repositório e fazer isso antes de uma expressão LINQ ou consulta de objeto foi convertida em um `IEnumerable` do objeto, os comandos serão transmitidos para o banco de dados para processamento, que é geralmente mais eficiente. Neste tutorial você implementará a classificação e filtragem de uma maneira que faz com que o processamento deve ser feito pelo banco de dados — ou seja, no repositório.

Para adicionar o recurso de classificação, você deve adicionar um novo método para a interface do repositório e classes de repositório, bem como para a classe de lógica de negócios. No *ISchoolRepository.cs* de arquivo, adicione uma nova `GetDepartments` método que utiliza um `sortExpression` parâmetro que será usado para classificar a lista de departamentos que é retornada:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

O `sortExpression` parâmetro especificará a coluna para a classificação e a direção de classificação.

Adicione código para o novo método para o *SchoolRepository.cs* arquivo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Alterar existente sem parâmetros `GetDepartments` método para chamar o novo método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

No projeto de teste, adicione o seguinte método novo à *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Se você for criar testes de unidade dependem esse método retornar uma lista classificada, seria necessário classificar a lista antes de retorná-lo. Você não criar testes neste tutorial, para que o método pode simplesmente retornar a lista não classificada de departamentos.

No *SchoolBL.cs* arquivo, adicione o seguinte método novo à classe de lógica de negócios:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Esse código passa o parâmetro de classificação para o método de repositório.

Execute o *Departments.aspx* página.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Agora você pode clicar em qualquer cabeçalho de coluna para classificar por essa coluna. Se a coluna já está classificada, clicando no título inverte a direção de classificação.

## <a name="adding-a-search-box"></a>Adicionar uma caixa de pesquisa

Nesta seção você irá adicionar uma caixa de texto de pesquisa, vinculá-lo a `ObjectDataSource` controlar usando um parâmetro de controle e adicionar um método à classe de lógica de negócios para dar suporte a filtragem.

Abra o *Departments.aspx* página e adicione a seguinte marcação entre o título e o primeiro `ObjectDataSource` controle:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

No `ObjectDataSource` controle chamado `DepartmentsObjectDataSource`, faça o seguinte:

- Adicionar um `SelectParameters` elemento para um parâmetro denominado `nameSearchString` que obtém o valor inserido no `SearchTextBox` controle.
- Alterar o `SelectMethod` valor ao atributo `GetDepartmentsByName`. (Você criará esse método posteriormente.)

A marcação para o `ObjectDataSource` controle agora se parece com o exemplo a seguir:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

Em *ISchoolRepository.cs*, adicione um `GetDepartmentsByName` método que usa ambos `sortExpression` e `nameSearchString` parâmetros:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

Em *SchoolRepository.cs*, adicione o seguinte método novo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Esse código usa um `Where` método para selecionar os itens que contêm a cadeia de caracteres de pesquisa. Se a cadeia de caracteres estiver vazia, todos os registros serão selecionados. Observe que quando você especificar o método chama juntos em uma instrução como esta (`Include`, em seguida, `OrderBy`, em seguida, `Where`), o `Where` método sempre deve ser o último.

Alterar existente `GetDepartments` método que utiliza um `sortExpression` parâmetro ao chamar o novo método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

Em *MockSchoolRepository.cs* no projeto de teste, adicione o seguinte método novo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

Em *SchoolBL.cs*, adicione o seguinte método novo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Execute o *Departments.aspx* página e digite uma cadeia de caracteres de pesquisa para certificar-se de que a lógica de seleção funciona. Deixe a caixa de texto vazia e tente fazer uma pesquisa para certificar-se de que todos os registros são retornados.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Adicionando uma coluna de detalhes para cada linha de grade

Em seguida, você deseja ver todos os cursos para cada departamento exibido na célula da grade à direita. Para fazer isso, você usará uma instrução `GridView` controle e associação de dados para dados do `Courses` propriedade de navegação a `Department` entidade.

Abra *Departments.aspx* e na marcação para o `GridView` controlar, especifique um manipulador para o `RowDataBound` evento. A marcação para a marca de abertura do controle agora se parece com o exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Adicionar um novo `TemplateField` elemento após o `Administrator` campo do modelo:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Essa marcação cria um aninhada `GridView` controle que mostra o número e o título de uma lista de cursos. Não especificar uma fonte de dados porque você vai databind em código o `RowDataBound` manipulador.

Abra *Departments.aspx.cs* e adicione o seguinte manipulador para o `RowDataBound` evento:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Esse código obtém a `Department` converte a entidade de argumentos do evento, o `Courses` propriedade de navegação uma `List` coleta e databinds aninhada `GridView` à coleção.

Abra o *SchoolRepository.cs* de arquivo e especifique o carregamento rápido para o `Courses` propriedade de navegação, chamando o `Include` método na consulta de objeto que você criar no `GetDepartmentsByName` método. O `return` instrução o `GetDepartmentsByName` método agora se parece com o exemplo a seguir.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Execute a página. Além de classificação e filtragem de recurso que você adicionou anteriormente, o controle GridView agora mostra detalhes de curso aninhada para cada departamento.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Isso conclui a introdução aos cenários de classificação, filtragem e de detalhes mestre. O seguinte tutorial, você verá como lidar com simultaneidade.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Próximo](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
