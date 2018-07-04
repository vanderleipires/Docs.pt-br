---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: Controles (VB) de Web de dados aninhadas | Microsoft Docs
author: rick-anderson
description: Neste tutorial, exploraremos como usar um repetidor aninhado em outro Repeater. Os exemplos ilustrará como popular o Repeater interno ambos os d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f7980d22d6ebc15a033cca321644a2bf1d4e3bb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390013"
---
<a name="nested-data-web-controls-vb"></a>Controles da Web de dados aninhadas (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe) ou [baixar PDF](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> Neste tutorial, exploraremos como usar um repetidor aninhado em outro Repeater. Os exemplos ilustrará como popular o Repeater interno declarativamente e programaticamente.


## <a name="introduction"></a>Introdução

Além do HTML estático e a sintaxe de associação de dados, os modelos também podem incluir controles da Web e controles de usuário. Esses controles de Web podem ter suas propriedades atribuídas por meio da sintaxe de ligação de dados declarativa, ou podem ser acessados por meio de programação nos manipuladores de eventos do lado do servidor apropriado.

Inserindo controles dentro de um modelo, a aparência e experiência do usuário pode ser personalizada e aprimorada. Por exemplo, nos [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial, vimos como personalizar a exibição de s GridView, adicionando um controle de calendário em um TemplateField para mostrar um funcionário data de contratação s; o [adicionando Controles de validação para a edição e inserção de Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) e [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutoriais, vimos como personalizar a edição e inserção interfaces adicionando validação controles, caixas de texto, DropDownLists e outros controles de Web.

Modelos também podem conter outros controles da Web de dados. Ou seja, podemos ter uma DataList que contém outro DataList (ou Repeater ou GridView ou DetailsView e assim por diante) dentro dos modelos. O desafio com uma interface desse tipo está se associando os dados apropriados para a controle da Web interna de dados. Há algumas abordagens diferentes disponíveis, variando de opções declarativas usando o ObjectDataSource para aqueles programáticos.

Neste tutorial, exploraremos como usar um repetidor aninhado em outro Repeater. O Repeater externo contém um item para cada categoria no banco de dados, exibindo o nome da categoria s e a descrição. Cada item de categoria s Repeater interna exibirá informações para cada produto que pertencem a essa categoria (veja a Figura 1) em uma lista com marcadores. Nossos exemplos ilustrará como popular o Repeater interno declarativamente e programaticamente.


[![Cada categoria, juntamente com seus produtos, estão listados](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**Figura 1**: cada categoria, juntamente com seus produtos, são listados ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-vb/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Etapa 1: Criando a listagem de categoria

Quando a criação de uma página que usa aninhados controles da Web de dados, posso é útil ao design, criar e testar o controle de Web de dados mais externa pela primeira vez, sem se preocupar com até mesmo sobre o controle aninhado interno. Portanto, deixe s comece percorrer as etapas necessárias para adicionar um repetidor para a página que lista o nome e descrição para cada categoria.

Comece abrindo o `NestedControls.aspx` página na `DataListRepeaterBasics` pasta e adicione um controle Repeater para a página, definindo seu `ID` propriedade para `CategoryList`. Da marca inteligente Repeater s, optar por criar um novo ObjectDataSource chamado `CategoriesDataSource`.


[![Nomeie o novo ObjectDataSource CategoriesDataSource](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**Figura 2**: Nomeie o novo ObjectDataSource `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-vb/_static/image6.png))


Configurar o ObjectDataSource para que ele efetua pull de seus dados a partir de `CategoriesBLL` classe s `GetCategories` método.


[![Configurar o ObjectDataSource para usar o método de GetCategories CategoriesBLL classe s](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**Figura 3**: configurar o ObjectDataSource para usar o `CategoriesBLL` classe s `GetCategories` método ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-vb/_static/image9.png))


Para especificar o modelo de s repetidor conteúdo precisamos ir para a exibição de fonte e inserir manualmente a sintaxe declarativa. Adicionar um `ItemTemplate` que exibe o nome da categoria s em um `<h4>` elemento e a descrição da categoria s em um elemento de parágrafo (`<p>`). Além disso, let s separar cada categoria com uma régua horizontal (`<hr>`). Depois de fazer essas alterações sua página deve conter a sintaxe declarativa para o Repeater e ObjectDataSource é semelhante ao seguinte:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

Figura 4 mostra nosso progresso quando visualizado por meio de um navegador.


[![Cada categoria s nome e a descrição estiver listado, separados por uma régua Horizontal](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**Figura 4**: cada categoria s nome e descrição estiver listado, separados por uma régua Horizontal ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-vb/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Etapa 2: Adicionando repetidor aninhada produto

Com a categoria de listagem completa, nossa próxima tarefa é adicionar um repetidor para a `CategoryList` s `ItemTemplate` que exibe informações sobre os produtos que pertencem à categoria apropriada. Há várias maneiras, que podemos recuperar os dados para este repetidor interna, dois dos quais exploraremos daqui a pouco. Por enquanto, let s apenas criar os produtos Repeater dentro de `CategoryList` Repeater s `ItemTemplate`. Especificamente, permite que s tenham o exibição Repeater cada produto em uma lista com marcadores com cada item de lista incluindo o nome do produto s e o preço do produto.

Para criar esse Repeater, precisamos inserir manualmente a sintaxe declarativa do repetidor s interna e os modelos para o `CategoryList` s `ItemTemplate`. Adicione a seguinte marcação dentro de `CategoryList` Repeater s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Etapa 3: Associar os produtos específicas de categoria a repetidor ProductsByCategoryList

Se você visitar a página por meio de um navegador neste ponto, sua tela terá a mesma aparência como na Figura 4 porque estamos ve ainda para associar todos os dados para o Repeater. Há várias maneiras que podemos pegar os registros de produto apropriado e associá-las a Repeater, mais eficiente do que outras pessoas. O principal desafio é obter de volta os produtos adequados para a categoria especificada.

Os dados para associar ao controle Repeater interno ou podem ser acessados declarativamente, por meio de um ObjectDataSource na `CategoryList` Repeater s `ItemTemplate`, ou programaticamente, da página do ASP.NET s de página code-behind. Da mesma forma, esses dados podem ser associados a repetidor interna ou declarativamente - por meio do repetidor s interna `DataSourceID` propriedade ou por meio da sintaxe de ligação de dados declarativa ou programaticamente referenciando o Repeater interno no `CategoryList` Repeater s `ItemDataBound` manipulador de eventos, definir programaticamente seu `DataSource` propriedade e chamar seu `DataBind()` método. Deixe o s explorar cada uma dessas abordagens.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Acessando os dados de forma declarativa com um controle ObjectDataSource e o`ItemDataBound`manipulador de eventos

Desde que criamos ve usado extensivamente em toda esta série de tutoriais, a escolha mais natural para acessar dados para este exemplo é continuar com o ObjectDataSource ObjectDataSource. O `ProductsBLL` classe tem um `GetProductsByCategoryID(categoryID)` método que retorna informações sobre os produtos que pertencem ao especificado *`categoryID`*. Portanto, podemos adicionar um ObjectDataSource para o `CategoryList` Repeater s `ItemTemplate` e configurá-lo para acessar seus dados desse método de classe s.

Infelizmente, o Repeater permitir seus modelos a serem editados por meio da exibição de Design, portanto, precisamos adicionar a sintaxe declarativa para este controle ObjectDataSource manualmente. A sintaxe a seguir mostra a `CategoryList` Repeater s `ItemTemplate` depois de adicionar esse novo ObjectDataSource (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

Ao usar a abordagem do ObjectDataSource precisamos definir a `ProductsByCategoryList` Repeater s `DataSourceID` propriedade para o `ID` do ObjectDataSource (`ProductsByCategoryDataSource`). Além disso, observe que nossa ObjectDataSource tem um `<asp:Parameter>` elemento que especifica a *`categoryID`* valor que será passado para o `GetProductsByCategoryID(categoryID)` método. Mas como podemos especificar esse valor? O ideal é que podemos d poderá definir apenas a `DefaultValue` propriedade do `<asp:Parameter>` elemento usando a sintaxe de associação de dados, da seguinte forma:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

Infelizmente, a sintaxe de associação de dados só é válido em controles que têm um `DataBinding` eventos. O `Parameter` esse evento não tem a classe e, portanto, a sintaxe acima é ilegal e resultará em um erro de tempo de execução.

Para definir esse valor, precisamos criar um manipulador de eventos para o `CategoryList` Repeater s `ItemDataBound` eventos. Lembre-se de que o `ItemDataBound` evento é acionado uma vez para cada item associado ao Repetidor. Portanto, cada vez que esse evento é acionado para repetidor externa podemos atribuir atual `CategoryID` de valor para o `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` parâmetro.

Crie um manipulador de eventos para o `CategoryList` Repeater s `ItemDataBound` evento com o código a seguir:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

Esse manipulador de eventos é iniciado, garantindo que podemos re lidar com dados de um item em vez do item de cabeçalho, rodapé ou separador. Em seguida, fazemos referência real `CategoriesRow` instância que tenha sido associada apenas à atual `RepeaterItem`. Por fim, podemos referenciar o ObjectDataSource na `ItemTemplate` e atribuir seu `CategoryID` valor de parâmetro para o `CategoryID` do atual `RepeaterItem`.

Com este manipulador de eventos, o `ProductsByCategoryList` Repeater em cada `RepeaterItem` está associado a esses produtos no `RepeaterItem` categoria s. Figura 5 mostra uma captura de tela da saída resultante.


[![O Repeater externo lista cada categoria; o interna lista os produtos dessa categoria](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**Figura 5**: O Repeater lista cada categoria externo; as listas de uma Inner os produtos dessa categoria ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-vb/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Acessando os produtos por categoria de dados de forma programática

Em vez de usar um ObjectDataSource para recuperar os produtos para a categoria atual, criamos um método em nossa classe de code-behind do ASP.NET s de página (ou no `App_Code` pasta ou em um projeto de biblioteca de classes separado) que retorna o conjunto apropriado de produtos quando passado um `CategoryID`. Imagine que tivemos esse tipo de método em nossa classe de code-behind s de página ASP.NET e que ele foi nomeado `GetProductsInCategory(categoryID)`. Com esse método em vigor, foi possível associar os produtos para a categoria atual ao repetidor interna usando a seguinte sintaxe declarativa:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

S repetidor `DataSource` propriedade usa a sintaxe de associação de dados para indicar que os dados vêm de `GetProductsInCategory(categoryID)` método. Uma vez que `Eval("CategoryID")` retorna um valor do tipo `Object`, podemos converter o objeto para um `Integer` antes de passá-lo no `GetProductsInCategory(categoryID)` método. Observe que o `CategoryID` acessados por meio de associação de dados sintaxe Eis a `CategoryID` na *externa* Repeater (`CategoryList`), aquele que s associado aos registros no `Categories` tabela. Portanto, nós sabemos que `CategoryID` não pode ser um banco de dados `NULL` valor, por isso, podemos pode converter cegamente o `Eval` método sem verificar se podemos re lidar com um `DBNull`.

Com essa abordagem, precisamos criar a `GetProductsInCategory(categoryID)` método e fazê-lo a recuperar o conjunto apropriado de produtos dado fornecido *`categoryID`*. Podemos fazer isso, simplesmente retornando o `ProductsDataTable` retornado pela `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método. Permitir que o s crie o `GetProductsInCategory(categoryID)` método em que a classe code-behind para nosso `NestedControls.aspx` página. Fazer isso usando o código a seguir:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

Esse método simplesmente cria uma instância das `ProductsBLL` método e retorna os resultados do `GetProductsByCategoryID(categoryID)` método. Observe que o método deve ser marcado `Public` ou `Protected`; se o método é marcado `Private`, não será acessível a partir de marcação declarativa de s de página ASP.NET.

Depois de fazer essas alterações para usar essa técnica de novo, reserve um tempo para exibir a página por meio de um navegador. A saída deve ser idêntica à saída ao usar o ObjectDataSource e `ItemDataBound` abordagem do manipulador de eventos (consulte novamente a Figura 5 para ver uma tela de captura).

> [!NOTE]
> Pode parecer que o trabalho para criar o `GetProductsInCategory(categoryID)` método na classe de code-behind de página s ASP.NET. Afinal de contas, esse método simplesmente cria uma instância das `ProductsBLL` de classe e retorna os resultados da sua `GetProductsByCategoryID(categoryID)` método. Por que não basta chamar este método diretamente da sintaxe de vinculação de dados no repetidor interna, como: `DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`? Embora essa sintaxe ganhou funcionam com nossa implementação atual do `ProductsBLL` classe (uma vez que o `GetProductsByCategoryID(categoryID)` método é um método de instância), você poderia modificar `ProductsBLL` para incluir um estático `GetProductsByCategoryID(categoryID)` método ou ter a classe incluem um estático `Instance()` método para retornar uma nova instância do `ProductsBLL` classe.


Embora essas modificações eliminaria a necessidade do `GetProductsInCategory(categoryID)` método na classe de code-behind de página s ASP.NET, o método da classe de lógica nos dá mais flexibilidade ao trabalhar com os dados recuperados, como veremos daqui a pouco.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Recuperar todas as informações de produto ao mesmo tempo

As duas técnicas concluírem podemos ve examinado pegar esses produtos para a categoria atual, fazendo uma chamada para o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método (a primeira abordagem faziam isso por meio de um ObjectDataSource, do segundo até o `GetProductsInCategory(categoryID)` método no classe code-behind). Cada vez que esse método é chamado, as chamadas da camada de lógica comercial para baixo até a camada de acesso a dados, que consulta o banco de dados com uma instrução SQL que retorna linhas do `Products` tabela cujos `CategoryID` campo corresponde ao parâmetro de entrada fornecido.

Considerando *N* categorias no sistema, essa abordagem, obteremos *N* + 1 chamadas para a consulta de um banco de dados do banco de dados para obter todas as categorias e, em seguida, *N* chamadas para obter os produtos específicas para cada categoria. No entanto, pode, recuperamos todos os dados necessários no banco de dados apenas duas chamadas uma chamada para obter todas as categorias e outro para obter todos os produtos. Assim que tivermos todos os produtos, é possível filtrar esses produtos assim que somente os produtos correspondência atual `CategoryID` são associados a essa categoria s Repeater interna.

Para fornecer essa funcionalidade, só precisamos fazer uma pequena modificação a `GetProductsInCategory(categoryID)` método em nossa classe de code-behind s de página ASP.NET. Em vez de cegamente retornar os resultados do `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método, podemos pode em vez disso, acessar pela primeira vez *todos os* dos produtos (se eles foram t foi acessado já) e, em seguida, retornar apenas a exibição filtrada da produtos com base no passado `CategoryID`.


[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

Observe a adição da variável de nível de página, `allProducts`. Isso mantém informações sobre todos os produtos e é populado na primeira vez o `GetProductsInCategory(categoryID)` método é invocado. Depois de garantir que o `allProducts` objeto foi criado e preenchido, o método filtra os resultados de s DataTable, de modo que somente as linhas cujo `CategoryID` corresponde à especificada `CategoryID` estão acessíveis. Essa abordagem reduz o número de vezes que o banco de dados é acessado a partir *N* + 1 para dois.

Esse aprimoramento não introduz nenhuma alteração à marcação renderizada da página, nem ela oferece volta menos registros que a outra abordagem. Ele simplesmente reduz o número de chamadas para o banco de dados.

> [!NOTE]
> Um intuitivamente talvez motivo reduzindo o número de acessos ao banco de dados certamente seria melhorar desempenho. No entanto, isso não pode ser o caso. Se você tiver um grande número de produtos cujos `CategoryID` está `NULL`, por exemplo, em seguida, a chamada para o `GetProducts` método retorna um número de produtos que nunca serão exibidos. Além disso, retornar todos os produtos pode ser um desperdício se você está mostrando apenas um subconjunto das categorias, que pode ser o caso se você tiver implementado a paginação.


Como sempre, quando se trata de analisar o desempenho de duas técnicas, a medida apenas certeiras é executar testes controlados adaptados para os cenários de caso aplicativo s comuns.

## <a name="summary"></a>Resumo

Neste tutorial vimos como aninhar um controle da Web dentro de outra, de dados examinando especificamente como ter um repetidor externa exibir um item para cada categoria com um repetidor interna listando os produtos para cada categoria em uma lista com marcadores. O principal desafio na criação de uma interface de usuário aninhado se encontra no acesso e associando os dados corretos para o controle de Web de dados interna. Há uma variedade de técnicas disponíveis, que dois deles, examinamos neste tutorial. A primeira abordagem examinada usado um ObjectDataSource nos dados de externa, controle de Web s `ItemTemplate` que foi associado ao controle da Web interna de dados por meio de seu `DataSourceID` propriedade. A segunda técnica acessados os dados por meio de um método na classe de code-behind da página s ASP.NET. Esse método, em seguida, pode ser associado aos dados internos de controle de Web s `DataSource` propriedade por meio da sintaxe de associação de dados.

Embora a interface de usuário aninhados examinada neste tutorial usado um repetidor aninhado em um repetidor, essas técnicas podem ser estendidas a outros controles de Web de dados. Você pode aninhar um repetidor dentro de um GridView ou um GridView dentro uma DataList e assim por diante.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Zack Jones e Liz Shulok. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
