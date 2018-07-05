---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: Exibindo informações de resumo no rodapé do GridView (c#) | Microsoft Docs
author: rick-anderson
description: Muitas vezes, informações de resumo são exibidas na parte inferior do relatório em uma linha de resumo. O controle GridView pode incluir uma linha de rodapé em cujas células, podemos pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 752a37f231f746573ad84d05677365216eaa48b7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368347"
---
<a name="displaying-summary-information-in-the-gridviews-footer-c"></a>Exibindo informações de resumo no rodapé do GridView (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe) ou [baixar PDF](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> Muitas vezes, informações de resumo são exibidas na parte inferior do relatório em uma linha de resumo. O controle GridView pode incluir uma linha de rodapé em cujas células pode programaticamente inserimos os dados de agregação. Neste tutorial, veremos como exibir dados de agregação nesta linha de rodapé.


## <a name="introduction"></a>Introdução

Além de ver cada um dos preços dos produtos, unidades em estoque, unidades na ordem e níveis de novo pedido, um usuário também pode estar interessado em informações de agregação, como o preço médio, o número total de unidades em estoque e assim por diante. Geralmente, essas informações de resumo são exibidas na parte inferior do relatório em uma linha de resumo. O controle GridView pode incluir uma linha de rodapé em cujas células pode programaticamente inserimos os dados de agregação.

Essa tarefa apresenta três desafios:

1. Configurando o GridView para exibir sua linha de rodapé
2. Determinando os dados de resumo; ou seja, como, computamos o preço médio ou o total de unidades no estoque?
3. Injetando dados de resumo nas células apropriadas da linha do rodapé

Neste tutorial, veremos como superar esses desafios. Especificamente, vamos criar uma página que lista as categorias em uma lista suspensa com os produtos da categoria selecionada exibidos em um GridView. O GridView incluirá uma linha de rodapé que mostra o preço médio e o número total de unidades em estoque e ordem de produtos dessa categoria.


[![Informações de resumo são exibidas na linha de rodapé do GridView](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**Figura 1**: informações de resumo é exibida na linha de rodapé do GridView ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))


Neste tutorial, com a categoria à interface de mestre/detalhes de produtos, tem como base os conceitos abordados anterior [filtragem de mestre/detalhes com uma DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial. Se você ainda não tiver trabalhado com o tutorial anterior, faça isso antes de prosseguir com este.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Etapa 1: Adicionando a DropDownList categorias e GridView de produtos

Antes de respeito sozinhos com a adição de informações de resumo para o rodapé do GridView, vamos primeiro simplesmente criar relatório mestre/detalhes. Depois de concluir a primeira etapa, vamos examinar como incluir dados de resumo.

Comece abrindo o `SummaryDataInFooter.aspx` página o `CustomFormatting` pasta. Adicione um controle DropDownList e defina suas `ID` para `Categories`. Em seguida, clique no link Escolher fonte de dados na marca inteligente do DropDownList e optar por adicionar um novo ObjectDataSource denominado `CategoriesDataSource` que invoca a `CategoriesBLL` da classe `GetCategories()` método.


[![Adicionar um novo ObjectDataSource chamado CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**Figura 2**: adicionar um novo ObjectDataSource nomeado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))


[![Ter o ObjectDataSource invocar o CategoriesBLL método da classe GetCategories()](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**Figura 3**: que invoque o ObjectDataSource a `CategoriesBLL` da classe `GetCategories()` método ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))


Depois de configurar o ObjectDataSource, o assistente retornará a configuração de fonte de dados do DropDownList assistente a partir do qual precisamos especificar o valor do campo de dados deve ser exibido e qual delas deve corresponder ao valor da DropDownList `ListItem` s. Ter o `CategoryName` campo exibido e use o `CategoryID` como o valor.


[![Use os campos de CategoryID e CategoryName como o texto e o valor para o ListItems, respectivamente](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**Figura 4**: Use o `CategoryName` e `CategoryID` campos como o `Text` e `Value` para o `ListItem` s, respectivamente ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))


Neste ponto, temos uma DropDownList (`Categories`) que lista as categorias no sistema. Agora, precisamos adicionar um GridView que lista os produtos que pertencem à categoria selecionada. Antes de fazer, no entanto, reserve um tempo para verificar a caixa de seleção Enable AutoPostBack na marca inteligente da DropDownList. Conforme discutido na *filtragem de mestre/detalhes com uma DropDownList* tutoriais, definindo a DropDownList `AutoPostBack` propriedade `true` a página será postada novamente sempre que o valor de DropDownList é alterado. Isso fará com que o GridView seja atualizada, mostrando os produtos para a categoria selecionada recentemente. Se o `AutoPostBack` estiver definida como `false` (o padrão), alterar a categoria não causará um postback e, portanto, não são atualizados os produtos listados.


[![Marque a caixa de seleção Habilitar AutoPostBack na marca inteligente do DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**Figura 5**: marque a seleção de AutoPostBack habilitar na marca inteligente do DropDownList ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))


Adicione um controle GridView à página para exibir os produtos para a categoria selecionada. Defina o GridView `ID` à `ProductsInCategory` e associá-lo para um novo ObjectDataSource chamado `ProductsInCategoryDataSource`.


[![Adicionar um novo ObjectDataSource chamado ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**Figura 6**: adicionar um novo ObjectDataSource nomeado `ProductsInCategoryDataSource` ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))


Configurar o ObjectDataSource para que ele chama o `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método.


[![Ter o ObjectDataSource invoca o método GetProductsByCategoryID(categoryID)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**Figura 7**: que invoque o ObjectDataSource a `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))


Uma vez que o `GetProductsByCategoryID(categoryID)` método assume um parâmetro de entrada, na etapa final do assistente, podemos especificar a origem do valor do parâmetro. Para exibir os produtos da categoria selecionada, tem o parâmetro retirado o `Categories` DropDownList.


[![Obter o valor do parâmetro categoryID na lista suspensa de categorias selecionadas](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**Figura 8**: obtenha o *`categoryID`* valor do parâmetro na lista suspensa de categorias selecionadas ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))


Depois de concluir o Assistente de GridView terá um BoundField para cada uma das propriedades do produto. Vamos limpar esses BoundFields para que somente o `ProductName`, `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` BoundFields são exibidos. Fique à vontade adicionar todas as configurações de nível de campo para o restante BoundFields (como a formatação a `UnitPrice` como uma moeda). Depois de fazer essas alterações, a marcação declarativa do GridView deve ser semelhante ao seguinte:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

Neste ponto, temos um relatório mestre/detalhes totalmente funcional que mostra o nome, preço unitário, unidades em estoque e unidades no pedido para os produtos que pertencem à categoria selecionada.


[![Obter o valor do parâmetro categoryID na lista suspensa de categorias selecionadas](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**Figura 9**: obtenha o *`categoryID`* valor do parâmetro na lista suspensa de categorias selecionadas ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Etapa 2: Exibir um rodapé em GridView

O controle GridView pode exibir a linha de cabeçalho e rodapé. Essas linhas são exibidas dependendo dos valores da `ShowHeader` e `ShowFooter` propriedades, respectivamente, com `ShowHeader` Padronizando para `true` e `ShowFooter` para `false`. Para incluir um rodapé no GridView simplesmente definir seus `ShowFooter` propriedade para `true`.


[![Definir ShowFooter propriedade do GridView como true](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**Figura 10**: defina o GridView `ShowFooter` propriedade `true` ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))


A linha de rodapé tem uma célula para cada um dos campos definidos em GridView; No entanto, essas células são vazias por padrão. Reserve um tempo para exibir nosso progresso em um navegador. Com o `ShowFooter` propriedade definida agora como `true`, GridView inclui uma linha de rodapé vazio.


[![O GridView agora inclui uma linha de rodapé](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**Figura 11**: O GridView agora inclui uma linha de rodapé ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))


A linha de rodapé na Figura 11 não se destacam, pois ela tem um plano de fundo branco. Vamos criar uma `FooterStyle` da classe CSS na `Styles.css` que especifica um plano de fundo vermelho escuro e, em seguida, configure o `GridView.skin` arquivo de capa no `DataWebControls` tema para atribuir essa classe CSS a GridView `FooterStyle`do `CssClass` propriedade. Se você precisar recordar temas e capas, voltar para o [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tutorial.

Comece adicionando a seguinte classe CSS à `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

O `FooterStyle` classe CSS é semelhante em estilo para o `HeaderStyle` classe, embora o `HeaderStyle`da cor de plano de fundo é sutileza mais escura e o texto é exibido em uma fonte em negrito. Além disso, o texto no rodapé é alinhado à direita, enquanto o texto do cabeçalho é centralizado.

Em seguida, para associar essa classe CSS ao rodapé do GridView, cada, abra o `GridView.skin` do arquivo no `DataWebControls` tema e defina o `FooterStyle`do `CssClass` propriedade. Após essa adição a marcação do arquivo deve ser semelhante:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

Como a captura de tela abaixo mostra, essa alteração torna o rodapé mais destacado.


[![Linha de rodapé do GridView agora tem uma cor de fundo avermelhado](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**Figura 12**: linha de rodapé do GridView The agora tem uma cor de plano de fundo avermelhado ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>Etapa 3: Computando os dados de resumo

Com o rodapé do GridView exibido, o próximo desafio voltados para nós é como os dados de resumo de computação. Há duas maneiras para calcular essas informações de agregação:

1. Por meio de uma consulta SQL, podemos pode emitir uma consulta adicional ao banco de dados para calcular os dados de resumo para uma determinada categoria. SQL inclui um número de funções de agregação junto com um `GROUP BY` cláusula para especificar os dados sobre o qual os dados devem ser resumidos. A seguinte consulta SQL seria trazer de volta as informações necessárias:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    É claro, você não iria querer emitir esta consulta diretamente a partir o `SummaryDataInFooter.aspx` página, mas em vez disso, criando um método na `ProductsTableAdapter` e o `ProductsBLL`.
2. Essas informações de computação, pois ele está sendo adicionado a GridView, conforme discutido em [formatação com base em dados personalizados](custom-formatting-based-upon-data-cs.md) tutorial, o GridView `RowDataBound` manipulador de eventos é acionado uma vez para cada linha que está sendo adicionada à GridView após sua foi associação de dados. Criando um manipulador de eventos para este evento podemos manter um execução total dos valores que queremos de agregação. Após a última linha de dados foi associada a GridView que temos os totais e as informações necessárias para calcular a média.

Eu normalmente empregam a segunda abordagem, como ele salva uma viagem para o banco de dados e os esforços necessários para implementar a funcionalidade de resumida na camada de acesso a dados e camada de lógica comercial, mas qualquer abordagem seria suficiente. Para este tutorial vamos usar a segunda opção e controlar o total usando o `RowDataBound` manipulador de eventos.

Criar uma `RowDataBound` manipulador de eventos para o GridView selecionando o GridView no Designer, clicando no ícone de raio da janela Propriedades e clicando duas vezes o `RowDataBound` eventos. Isso criará um novo manipulador de eventos chamado `ProductsInCategory_RowDataBound` no `SummaryDataInFooter.aspx` classe de code-behind da página.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

Para manter um total contínuo, precisamos definir variáveis fora do escopo do manipulador de eventos. Crie as quatro variáveis de nível de página a seguir:

- `_totalUnitPrice`, do tipo `decimal`
- `_totalNonNullUnitPriceCount`, do tipo `int`
- `_totalUnitsInStock`, do tipo `int`
- `_totalUnitsOnOrder`, do tipo `int`

Em seguida, escreva o código para incrementar essas três variáveis para cada linha de dados encontrados no `RowDataBound` manipulador de eventos.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

O `RowDataBound` manipulador de eventos é iniciado, garantindo que estamos lidando com uma DataRow. Uma vez que é estabelecida, o `Northwind.ProductsRow` instância que estava associada apenas para o `GridViewRow` do objeto no `e.Row` é armazenado na variável `product`. Em seguida, em execução variáveis total são incrementadas em valores de correspondente do produto atual (supondo que eles não contêm um banco de dados `NULL` valor). Podemos manter o controle de ambas as executadas `UnitPrice` total e o número de não -`NULL` `UnitPrice` registra porque o preço médio é o quociente desses dois números.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Etapa 4: Exibindo dados de resumo no rodapé

Com os dados de resumidos somados, a última etapa é para exibi-lo na linha de rodapé do GridView. Nesta tarefa, também, pode ser feita por meio de programação por meio de `RowDataBound` manipulador de eventos. Lembre-se de que o `RowDataBound` manipulador de eventos é acionado para *cada* linha que está associada ao controle GridView, incluindo a linha de rodapé. Portanto, podemos pode incrementar nosso manipulador de eventos para exibir os dados na linha de rodapé usando o seguinte código:


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

Desde que a linha de rodapé é adicionada ao GridView depois que todas as linhas de dados foram adicionadas, podemos pode ter certeza de que no momento, estamos prontos para exibir os dados de resumo no rodapé que cálculos total terá concluído. A última etapa, em seguida, é definir esses valores nas células do rodapé.

Para exibir texto em uma célula do rodapé específico, use `e.Row.Cells[index].Text = value`, onde o `Cells` indexação começa em 0. O código a seguir calcula o preço médio (o preço total dividido pelo número de produtos) e o exibe juntamente com o número total de unidades em estoque e unidades no pedido nas células apropriadas do rodapé do GridView.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

Figura 13 mostra o relatório depois que esse código foi adicionado. Observe como o `ToString("c")` faz com que as informações de resumo de preço médio a ser formatado como uma moeda.


[![Linha de rodapé do GridView agora tem uma cor de fundo avermelhado](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**Figura 13**: linha de rodapé do GridView The agora tem uma cor de plano de fundo avermelhado ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))


## <a name="summary"></a>Resumo

Exibir dados de resumo é um requisito comum de relatório e o controle GridView torna mais fácil de incluir essas informações em sua linha de rodapé. A linha de rodapé é exibida quando o GridView `ShowFooter` estiver definida como `true` e pode ter o texto em suas células definidas programaticamente através de `RowDataBound` manipulador de eventos. Os dados de resumo de computação pode ser realizada por consultar novamente o banco de dados ou por meio de código na classe de code-behind da página ASP.NET para computar programaticamente os dados de resumo.

Esse tutorial conclui nossa análise de formatação personalizada com os controles GridView, DetailsView e FormView. Nosso próximo tutorial inicia nossa exploração de inserindo, atualizando e excluindo dados usando esses mesmos controles.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](using-the-formview-s-templates-cs.md)
> [Próximo](custom-formatting-based-upon-data-vb.md)
