---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: Dados aninhados controles (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, exploraremos como usar um repetidor aninhada em outra Repetidor. Os exemplos ilustrará como preencher repetidor interna ambos d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: d8bb5eae2003273fa8d8a06cc4adaa959378f1e2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882956"
---
<a name="nested-data-web-controls-vb"></a>Controles da Web de dados aninhadas (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe) ou [baixar PDF](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> Neste tutorial, exploraremos como usar um repetidor aninhada em outra Repetidor. Os exemplos ilustrará como preencher repetidor interna declarativamente e programaticamente.


## <a name="introduction"></a>Introdução

Além de HTML estático e a sintaxe de associação de dados, modelos também podem incluir controles da Web e controles de usuário. Esses controles da Web podem ter suas propriedades atribuído por meio da sintaxe de vinculação de dados declarativa, ou podem ser acessados por meio de programação em manipuladores de eventos do servidor apropriados.

Inserindo controles dentro de um modelo, a aparência e experiência do usuário pode ser personalizada e aprimorada. Por exemplo, o [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial, vimos como personalizar a exibição de s GridView adicionando um controle de calendário em um TemplateField para mostrar um funcionário data de contratação s; no [adicionando Controles de validação para a edição e inserindo Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) e [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutoriais, vimos como personalizar a edição e interfaces inserindo adicionando validação controles de caixas de texto, DropDownLists e outros controles da Web.

Modelos também podem conter outros controles da Web de dados. Ou seja, podemos ter DataList que contém outra DataList (ou repetidor ou GridView ou DetailsView e assim por diante) dentro dos modelos. O desafio com tal interface está se associando os dados apropriados para os controle da Web de dados internos. Há algumas abordagens diferentes disponíveis, variando de opções declarativas usando ObjectDataSource às programático.

Neste tutorial, exploraremos como usar um repetidor aninhada em outra Repetidor. Repetidor externa contém um item para cada categoria no banco de dados, exibindo o nome da categoria s e a descrição. Cada item de categoria s repetidor interna exibirá informações para cada produto que pertencem a essa categoria (consulte a Figura 1) em uma lista com marcadores. Nossos exemplos ilustrará como preencher repetidor interna declarativamente e programaticamente.


[![Cada categoria, acompanhados de seus produtos, listados](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**Figura 1**: cada categoria, acompanhados de seus produtos, são listados ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-vb/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Etapa 1: Criando a lista de categoria

Quando criar uma página que usa aninhados controles da Web de dados, eu seja útil para criar, criar e testar o controle de Web externo de dados pela primeira vez, sem se preocupar com até mesmo sobre o controle aninhado interno. Portanto, permitem s iniciar percorrer as etapas necessárias para adicionar um repetidor para a página que lista o nome e uma descrição para cada categoria.

Comece abrindo o `NestedControls.aspx` página o `DataListRepeaterBasics` pasta e adicionar um controle repetidor a configuração de página seu `ID` propriedade `CategoryList`. Marca inteligente repetidor s, escolher criar um novo ObjectDataSource denominado `CategoriesDataSource`.


[![Nome do novo ObjectDataSource CategoriesDataSource](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**Figura 2**: nomear o novo ObjectDataSource `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-vb/_static/image6.png))


Configurar ObjectDataSource para que ele obtém seus dados a partir de `CategoriesBLL` classe s `GetCategories` método.


[![Configurar o ObjectDataSource para usar o método de GetCategories CategoriesBLL classe s](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**Figura 3**: configurar o ObjectDataSource para usar o `CategoriesBLL` classe s `GetCategories` método ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-vb/_static/image9.png))


Para especificar o modelo de s repetidor conteúdo precisamos ir para a exibição de fonte e insira manualmente a sintaxe declarativa. Adicionar uma `ItemTemplate` que exibe o nome da categoria s em um `<h4>` elemento e a descrição da categoria s em um elemento de parágrafo (`<p>`). Além disso, permitem s separar cada categoria com uma régua horizontal (`<hr>`). Após fazer essas alterações sua página deve conter a sintaxe declarativa para o repetidor e ObjectDataSource é semelhante ao seguinte:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

A Figura 4 mostra nosso progresso quando visualizada através de um navegador.


[![Cada categoria s nome e descrição estiver listado, separados por uma régua Horizontal](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**Figura 4**: s de cada categoria nome e descrição estiver listado, separados por uma regra Horizontal ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-vb/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Etapa 2: Adicionando repetidor aninhada produto

Com a lista completa de categoria, nossa próxima tarefa é adicionar um repetidor o `CategoryList` s `ItemTemplate` que exibe informações sobre os produtos que pertencem à categoria apropriada. Há várias maneiras que podemos recuperar os dados para este repetidor interna, dois dos quais vamos explorar em breve. Por enquanto, vamos s apenas criar os produtos repetidor dentro de `CategoryList` repetidor s `ItemTemplate`. Especificamente, permitem que s a exibição repetidor cada produto em uma lista com marcadores com cada item de lista incluindo o nome do produto s e o preço do produto.

Para criar este repetidor precisamos inserir manualmente a sintaxe declarativa de Repetidor s interna e os modelos para o `CategoryList` s `ItemTemplate`. Adicione a seguinte marcação dentro de `CategoryList` repetidor s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Etapa 3: Associação de produtos específicas de categoria para repetidor ProductsByCategoryList

Se você visitar a página por meio de um navegador neste ponto, sua tela terão a mesma aparência como na Figura 4 porque podemos ver ainda para associar os dados a Repetidor. Existem algumas maneiras que podemos obter os registros de produto apropriadas e associá-las a repetidor, mais eficiente do que outras pessoas. O principal desafio é obter de volta os produtos adequados para a categoria especificada.

Os dados para associar ao controle repetidor interno ou podem ser acessados declarativamente, por meio de um ObjectDataSource no `CategoryList` repetidor s `ItemTemplate`, ou programaticamente, a página de code-behind de páginas ASP.NET. Da mesma forma, esses dados podem ser associados a repetidor interna ou declarativamente - por meio do repetidor interna s `DataSourceID` propriedade ou por meio da sintaxe de associação de dados declarativa ou programaticamente referenciando repetidor interna no `CategoryList` repetidor s `ItemDataBound` manipulador de eventos, como definir programaticamente seu `DataSource` propriedade e chamar sua `DataBind()` método. Permitir que o s explorar cada uma dessas abordagens.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Acessando os dados de forma declarativa com um controle ObjectDataSource e`ItemDataBound`manipulador de eventos

Desde que podemos usou ObjectDataSource extensivamente esta série de tutoriais, a escolha mais natural para acessar dados para este exemplo é utilizar o ObjectDataSource. O `ProductsBLL` classe tiver um `GetProductsByCategoryID(categoryID)` método que retorna informações sobre os produtos que pertencem a especificado *`categoryID`*. Portanto, podemos adicionar um ObjectDataSource para o `CategoryList` repetidor s `ItemTemplate` e configurá-lo para acessar os dados desse método de classe s.

Infelizmente, o repetidor permitir seu modelo a ser editado por meio da exibição de Design para adicionar a sintaxe declarativa para este controle ObjectDataSource manualmente. A sintaxe a seguir mostra o `CategoryList` repetidor s `ItemTemplate` depois de adicionar este novo ObjectDataSource (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

Ao usar a abordagem de ObjectDataSource, precisamos definir o `ProductsByCategoryList` repetidor s `DataSourceID` propriedade para o `ID` do ObjectDataSource (`ProductsByCategoryDataSource`). Além disso, observe que nossa ObjectDataSource tem um `<asp:Parameter>` elemento que especifica o *`categoryID`* valor que será passado para o `GetProductsByCategoryID(categoryID)` método. Mas como podemos especificar esse valor? Idealmente, d é ser capaz de definir o `DefaultValue` propriedade o `<asp:Parameter>` elemento usando a sintaxe de associação de dados, da seguinte forma:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

Infelizmente, a sintaxe de associação de dados só é válido em controles que têm um `DataBinding` eventos. O `Parameter` classe não tem um evento e, portanto, a sintaxe acima é inválida e resulta em um erro de tempo de execução.

Para definir esse valor, precisamos criar um manipulador de eventos para o `CategoryList` repetidor s `ItemDataBound` eventos. Lembre-se de que o `ItemDataBound` evento dispara uma vez para cada item associado ao Repetidor. Portanto, sempre que esse evento é acionado para repetidor externa podemos atribuir atual `CategoryID` o valor para o `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` parâmetro.

Criar um manipulador de eventos para o `CategoryList` repetidor s `ItemDataBound` evento com o código a seguir:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

Este manipulador de eventos é iniciado, garantindo que podemos re lidar com dados de um item em vez do item de cabeçalho, rodapé ou separador. Em seguida, é preciso fazer referência a real `CategoriesRow` associado apenas a atual instância `RepeaterItem`. Por fim, é preciso fazer referência ObjectDataSource no `ItemTemplate` e atribuir seu `CategoryID` valor do parâmetro de `CategoryID` do atual `RepeaterItem`.

Com este manipulador de eventos, o `ProductsByCategoryList` repetidor em cada `RepeaterItem` está associado a esses produtos no `RepeaterItem` categoria s. A Figura 5 mostra uma captura de tela da saída resultante.


[![Repetidor externa lista cada categoria. o interna lista os produtos dessa categoria](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**Figura 5**: O repetidor lista cada categoria externo; as listas de uma Inner os produtos dessa categoria ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-vb/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Acessando os produtos por categoria de dados programaticamente

Em vez de usar um ObjectDataSource para recuperar os produtos para a categoria atual, foi possível criar um método nossa classe de code-behind de páginas ASP.NET (ou o `App_Code` pasta ou em um projeto de biblioteca de classes separado) que retorna o conjunto apropriado de produtos quando passado um `CategoryID`. Imagine que tivemos que tal método em nossa classe de code-behind de páginas ASP.NET e que foi nomeada `GetProductsInCategory(categoryID)`. Com esse método, podemos pode associar os produtos para a categoria atual a repetidor interna usando a seguinte sintaxe declarativa:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

Repetidor s `DataSource` propriedade usa a sintaxe de associação de dados para indicar que os dados vêm de `GetProductsInCategory(categoryID)` método. Como `Eval("CategoryID")` retorna um valor do tipo `Object`, podemos converter o objeto para um `Integer` antes de passá-lo no `GetProductsInCategory(categoryID)` método. Observe que o `CategoryID` acessados por meio de associação de dados sintaxe Eis o `CategoryID` no *externa* repetidor (`CategoryList`), o que s associado aos registros no `Categories` tabela. Portanto, sabemos que `CategoryID` não pode ser um banco de dados `NULL` valor, por isso, podemos pode converter cegamente o `Eval` método sem verificar se podemos re lidar com um `DBNull`.

Com essa abordagem, é preciso criar o `GetProductsInCategory(categoryID)` método e recuperar o conjunto apropriado de produtos fornecido fornecido *`categoryID`*. Podemos fazer isso, basta retornando o `ProductsDataTable` retornado pelo `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método. Permitir que o s criar o `GetProductsInCategory(categoryID)` método na classe por trás do código para nosso `NestedControls.aspx` página. Faça isso usando o código a seguir:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

Este método simplesmente cria uma instância do `ProductsBLL` método e retorna os resultados de `GetProductsByCategoryID(categoryID)` método. Observe que o método deve ser marcado `Public` ou `Protected`; se o método está marcado como `Private`, não estará acessível da marcação declarativa de páginas ASP.NET.

Após fazer essas alterações para usar essa técnica de novo, reserve um tempo para exibir a página por meio de um navegador. A saída deve ser idêntica de saída ao usar o ObjectDataSource e `ItemDataBound` abordagem de manipulador de eventos (consulte novamente a Figura 5 para ver uma tela de captura).

> [!NOTE]
> Pode parecer busywork para criar o `GetProductsInCategory(categoryID)` método na classe por trás do código em páginas ASP.NET. Afinal, este método simplesmente cria uma instância do `ProductsBLL` classe e retorna os resultados da sua `GetProductsByCategoryID(categoryID)` método. Por que não basta chamar este método diretamente da sintaxe de associação de dados no repetidor interna, como: `DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`? Embora essa sintaxe ganha funcionam com nossa implementação atual do `ProductsBLL` classe (como o `GetProductsByCategoryID(categoryID)` método é um método de instância), você poderia modificar `ProductsBLL` para incluir um estático `GetProductsByCategoryID(categoryID)` método ou ter a classe incluir static `Instance()` método para retornar uma nova instância do `ProductsBLL` classe.


Enquanto essas modificações eliminar a necessidade do `GetProductsInCategory(categoryID)` método na classe por trás do código em páginas ASP.NET, o método da classe por trás do código nos dá mais flexibilidade ao trabalhar com os dados recuperados, conforme veremos em breve.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Recuperar todas as informações de produto de uma vez

As duas técnicas anteriores é ve examinada Pegue os produtos para a categoria atual, fazendo uma chamada para o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método (a primeira abordagem faziam isso por meio de um ObjectDataSource, segundo o `GetProductsInCategory(categoryID)` método no classe code-behind). Cada vez que esse método é chamado, as chamadas de camada de lógica de negócios para a camada de acesso a dados, que consulta o banco de dados com uma instrução SQL que retorna linhas de `Products` tabela cujos `CategoryID` campo coincide com o parâmetro de entrada fornecido.

Considerando *N* categorias no sistema, essa abordagem, obterá *N* + 1 chamadas para a consulta de um banco de dados do banco de dados para obter todas as categorias e, em seguida, *N* chamadas para obter os produtos específico para cada categoria. No entanto, pode recuperar todos os dados necessários no banco de dados apenas duas chamadas uma chamada para obter todas as categorias e outro para obter todos os produtos. Assim que tivermos todos os produtos, é possível filtrar os produtos para que somente os produtos correspondência atual `CategoryID` estão associados a essa categoria s repetidor interna.

Para fornecer essa funcionalidade, precisamos apenas fazer uma pequena modificação para o `GetProductsInCategory(categoryID)` método em nossa classe de code-behind de páginas do ASP.NET. Em vez de cegamente retornar os resultados do `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método, podemos em vez disso, acessar pela primeira vez *todos os* dos produtos (se eles ainda t foi acessado já) e, em seguida, retornar apenas a exibição filtrada da produtos com base no passado `CategoryID`.


[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

Observe a adição da variável de nível de página, `allProducts`. Isso mantém informações sobre todos os produtos e é populado na primeira vez o `GetProductsInCategory(categoryID)` método é invocado. Depois de garantir que o `allProducts` objeto foi criado e preenchido, o método filtra os resultados de s de DataTable, de modo que somente as linhas cujo `CategoryID` corresponde especificado `CategoryID` estão acessíveis. Essa abordagem reduz o número de vezes que o banco de dados é acessado de *N* + 1 até dois.

Esse aprimoramento não introduz nenhuma alteração à marcação da página renderizada, nem traz volta menos registros que a outra abordagem. Ele simplesmente reduz o número de chamadas para o banco de dados.

> [!NOTE]
> Uma maneira intuitiva pode motivo que reduz o número de acessos de banco de dados seria certamente melhorar o desempenho. No entanto, isso não pode ser o caso. Se você tiver um grande número de produtos cujos `CategoryID` é `NULL`, por exemplo, então a chamada para o `GetProducts` método retorna um número de produtos que nunca são exibidos. Além disso, retornar todos os produtos pode ser desnecessário se você está mostrando apenas um subconjunto das categorias, que pode ser o caso se você tiver implementado paginação.


Como sempre, quando se trata de analisar o desempenho de duas técnicas, a medida só certeiro é executar testes controlados, sob medidos para seu aplicativo s cenários comuns de maiusculas.

## <a name="summary"></a>Resumo

Neste tutorial, vimos como aninhar dados de um controle da Web dentro de outra, especificamente examinando como ter um repetidor externa exibem um item para cada categoria com um repetidor interna listando os produtos para cada categoria em uma lista com marcadores. O principal desafio na criação de uma interface de usuário aninhada está em acessar e associação de dados correto para o controle de Web de dados interna. Há uma variedade de técnicas disponíveis, que dois dos quais examinamos neste tutorial. A primeira abordagem examinada usado um ObjectDataSource nos dados externos controle Web s `ItemTemplate` que estava associado ao controle de Web de dados interna por meio de seu `DataSourceID` propriedade. A segunda técnica acessados os dados por meio de um método na classe de code-behind da página s ASP.NET. Esse método, em seguida, pode ser associado aos dados do internos controle Web s `DataSource` propriedade por meio da sintaxe de associação de dados.

Enquanto a interface de usuário aninhados examinada neste tutorial usada um repetidor aninhado dentro de um repetidor, essas técnicas podem ser estendidas para os outros controles da Web de dados. Você pode aninhar um repetidor dentro de um controle GridView ou um GridView dentro de DataList e assim por diante.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Zack Jones e Liz Shulok. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
