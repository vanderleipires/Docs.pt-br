---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Mestre/detalhes usando uma lista com marcadores de registros mestre com um DataList de detalhes (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial serão compactados o relatório de duas páginas mestre/detalhes do tutorial anterior em uma única página, mostrando uma lista com marcadores de nomes de categoria em t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5b7fdcb5ba38e89b073960d6bce473c3a5aadf8b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389632"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Mestre/detalhes usando uma lista com marcadores de registros mestre com um DataList de detalhes (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) ou [baixar PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> Neste tutorial serão compactados o relatório de duas páginas mestre/detalhes do tutorial anterior em uma única página, mostrando uma lista com marcadores de nomes de categoria no lado esquerdo da tela e produtos da categoria selecionada no lado direito da tela.


## <a name="introduction"></a>Introdução

No [tutorial anterior](master-detail-filtering-acess-two-pages-datalist-vb.md) analisamos como separar um relatório mestre/detalhes em duas páginas. Na página mestra, usamos um controle Repeater para renderizar uma lista com marcadores de categorias. Cada nome de categoria foi um hiperlink que, quando clicado, seria fazer o usuário para a página de detalhes, onde um DataList de duas colunas mostrou os produtos que pertencem à categoria selecionada.

Neste tutorial serão compactados o tutorial de duas páginas em uma única página, mostrando uma lista com marcadores de nomes de categoria no lado esquerdo da tela com cada nome de categoria renderizado como um LinkButton. Clicar em um dos botões de link do nome da categoria induz um postback e associa os produtos a categoria selecionada a um DataList de duas colunas à direita da tela. Além de exibir cada nome de categoria s, repetidor à esquerda mostra o número total de produtos lá é para uma determinada categoria (veja a Figura 1).


[![A categoria s nome e o número Total de produtos são exibidos à esquerda](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Figura 1**: A categoria s nome e o número Total de produtos são exibidos à esquerda ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Etapa 1: Exibindo um repetidor na parte esquerda da tela

Para este tutorial, é necessário ter a lista com marcadores de categorias aparecem à esquerda dos produtos s categoria selecionada. Conteúdo dentro de uma página da web pode ser posicionado usando marcas HTML padrão elementos parágrafo, espaços não separáveis, `<table>` s e assim por diante ou por meio de técnicas de (CSS) de folha de estilos em cascata. Todos os nossos tutoriais até o momento usaram técnicas CSS para posicionamento. Quando criamos a interface do usuário de navegação em nossa página mestre na [páginas mestras e navegação no Site](../introduction/master-pages-and-site-navigation-vb.md) tutorial, usamos *posicionamento absoluto*, indicando o deslocamento de pixel precisos para a navegação lista e o conteúdo principal. Como alternativa, o CSS pode ser usado para posicionar um elemento para a direita ou esquerda do outro por meio *flutuante*. Podemos ter a lista com marcadores de categorias aparecem à esquerda dos produtos s categoria selecionada pelo flutuante repetidor para a esquerda do DataList

Abra o `CategoriesAndProducts.aspx` página do `DataListRepeaterFiltering` pasta e adicionar à página um Repeater e DataList. Defina o Repeater s `ID` à `Categories` e DataList s para `CategoryProducts`. Vá para a exibição de fonte e os controles Repeater e DataList dentro de suas próprias `<div>` elementos. Ou seja, coloque o Repeater dentro de um `<div>` elemento primeiro e, em seguida, DataList em seu próprio `<div>` elemento diretamente após o Repeater. Sua marcação neste ponto deve ser semelhante ao seguinte:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Para desencaixar o Repeater à esquerda do DataList, precisamos usar o `float` atributo de estilo CSS, da seguinte forma:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

O `float: left;` flutua primeiro `<div>` elemento à esquerda de um segundo. O `width` e `padding-right` configurações indicam a primeira `<div>` s `width` e a quantidade de preenchimento é adicionado entre o `<div>` conteúdo do elemento s e a margem direita. Para obter mais informações sobre flutuante elementos no CSS Confira as [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Em vez de especificar a configuração de estilo diretamente por meio do primeiro `<p>` elemento s `style` atributo, permitem que o s em vez disso, crie uma nova classe CSS no `Styles.css` denominada `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Em seguida, substituímos o `<div>` com `<div class="FloatLeft">`.

Depois de adicionar a classe CSS e configurando a marcação no `CategoriesAndProducts.aspx` página, vá para o Designer. Você deve ver o Repeater flutuante à esquerda do DataList (embora direita agora ambos são exibidos como caixas cinzas desde que criamos ve ainda para configurar suas fontes de dados ou modelos).


[![O Repeater é flutuante à esquerda do DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Figura 2**: O Repeater é flutuante à esquerda do DataList ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Etapa 2: Determinando o número de produtos para cada categoria

Com o s Repeater e DataList que cercam a marcação completa, está pronto para associar os dados da categoria a repetidor controlamos. No entanto, como mostra a lista com marcadores de categorias na Figura 1, além de cada nome de categoria s também é necessário exibir o número de produtos associados à categoria. Para acessar essas informações é possível:

- **Determine essas informações de classe de code-behind da página s ASP.NET.** Dada uma determinada *`categoryID`* podemos determinar o número de produtos associados ao chamar o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método. Esse método retorna um `ProductsDataTable` do objeto cuja `Count` propriedade indica quantos `ProductsRow` s existir, o que é o número de produtos para especificado *`categoryID`*. Podemos criar uma `ItemDataBound` manipulador de eventos para o Repeater que, para cada categoria associada a Repeater, chama o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método e inclui a contagem na saída.
- **Atualizar o `CategoriesDataTable` no conjunto de dados tipado para incluir um `NumberOfProducts` coluna.** Em seguida, podemos atualizar o `GetCategories()` método na `CategoriesDataTable` para incluir essas informações ou, como alternativa, deixar `GetCategories()` como-está e criar um novo `CategoriesDataTable` método chamado `GetCategoriesAndNumberOfProducts()`.

Deixe o s explorar ambas as técnicas. A primeira abordagem é mais simples de implementar já que estamos não t é necessário atualizar a camada de acesso a dados; No entanto, ele requer mais comunicações com o banco de dados. A chamada para o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método o `ItemDataBound` manipulador de eventos adiciona uma chamada de banco de dados extra para cada categoria exibida no Repetidor. Com essa técnica há *N* + chamadas de banco de 1 dados, onde *N* é o número de categorias exibidas no Repetidor. Com a segunda abordagem, a contagem de produto é retornada com informações sobre cada categoria do `CategoriesBLL` classe s `GetCategories()` (ou `GetCategoriesAndNumberOfProducts()`) método, resultando em apenas uma viagem ao banco de dados.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Determinando o número de produtos no manipulador de evento ItemDataBound

Determinando o número de produtos para cada categoria no repetidor s `ItemDataBound` manipulador de eventos não requer quaisquer modificações em nossa camada de acesso de dados existente. Todas as modificações podem ser feitas diretamente a `CategoriesAndProducts.aspx` página. Comece adicionando um novo ObjectDataSource chamado `CategoriesDataSource` por meio de marca inteligente Repeater s. Em seguida, configure a `CategoriesDataSource` ObjectDataSource para que ele recupera seus dados a partir de `CategoriesBLL` classe s `GetCategories()` método.


[![Configurar o ObjectDataSource para usar a classe CategoriesBLL s GetCategories() método](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Figura 3**: configurar o ObjectDataSource para usar o `CategoriesBLL` classe s `GetCategories()` método ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))


Cada item na `Categories` Repeater precisa ser clicáveis e, quando clicado, fazer com que o `CategoryProducts` DataList para exibir esses produtos para a categoria selecionada. Isso pode ser feito fazendo um hiperlink, vinculação de volta para esta página mesma para cada categoria (`CategoriesAndProducts.aspx`), mas passando o `CategoryID` por meio da cadeia de consulta, assim como o que vimos no tutorial anterior. A vantagem dessa abordagem é que uma página que exibe produtos de uma determinada categoria s pode ser marcada e indexada por um mecanismo de pesquisa.

Como alternativa, podemos tornar cada categoria LinkButton, que é a abordagem que usaremos para este tutorial. No botão LinkButton é renderizado no navegador do usuário s como um hiperlink, mas, quando clicado, induz um postback; no postback, o s DataList ObjectDataSource precisa ser atualizado para exibir os produtos que pertencem à categoria selecionada. Para este tutorial, usar um hiperlink que faz mais sentido do que usando LinkButton; No entanto, pode haver outros cenários em que é mais vantajoso usar LinkButton. Embora a abordagem de hiperlink seria ideal para este exemplo, permite que s em vez disso, explorar usando o LinkButton. Como veremos, usar um LinkButton apresenta alguns desafios que não se surgiria, caso contrário, com um hiperlink. Portanto, usar um LinkButton neste tutorial será realce esses desafios e ajudar a fornecer soluções para esses cenários em que podemos querer usar um LinkButton em vez de um hiperlink.

> [!NOTE]
> Você é incentivado a repetir este tutorial usando um controle de hiperlink ou `<a>` elemento em vez de no botão LinkButton.


A marcação a seguir mostra a sintaxe declarativa para repetidor e ObjectDataSource. Observe que os modelos de s Repeater renderizam uma lista com marcadores com cada item como um LinkButton:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> Para este tutorial o Repeater deve ter seu estado de exibição habilitado (Observe a omissão do `EnableViewState="False"` da sintaxe declarativa Repeater s). Na etapa 3, criaremos um manipulador de eventos para o s Repeater `ItemCommand` eventos nos quais vamos atualizar DataList s s ObjectDataSource `SelectParameters` coleção. O Repeater s `ItemCommand`, no entanto, ganha incêndio t, se o estado de exibição está desabilitado. Ver [um desafio de uma pergunta do ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) e [sua solução](http://scottonwriting.net/sowBlog/posts/1268.aspx) para obter mais informações sobre por que o estado de exibição deve ser habilitado para um repetidor s `ItemCommand` evento seja acionado.


No LinkButton com o `ID` valor da propriedade de `ViewCategory` não tem seu `Text` conjunto de propriedades. Se podemos apenas quisesse exibir o nome da categoria, seria definimos a propriedade Text declarativamente, por meio da sintaxe de associação de dados, da seguinte forma:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

No entanto, queremos mostrar o nome de categoria s *e* o número de produtos que pertencem a essa categoria. Essas informações podem ser recuperadas do repetidor s `ItemDataBound` manipulador de eventos, fazendo uma chamada para o `ProductBLL` classe s `GetCategoriesByProductID(categoryID)` método e determinar quantos registros é retornado em resultante `ProductsDataTable`, como o código a seguir ilustra:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Começamos, garantindo que podemos está trabalhando com um item de dados (um cujos `ItemType` é `Item` ou `AlternatingItem`) e, em seguida, fazer referência a `CategoriesRow` instância que tenha sido associada apenas à atual `RepeaterItem`. Em seguida, podemos determinar o número de produtos dessa categoria com a criação de uma instância das `ProductsBLL` classe, chamando seu `GetCategoriesByProductID(categoryID)` método e determinar o número de registros retornados usando o `Count` propriedade. Por fim, o `ViewCategory` LinkButton ItemTemplate é references e sua `Text` estiver definida como *CategoryName* (*NumberOfProductsInCategory*), onde  *NumberOfProductsInCategory* é formatado como um número com zero casas decimais.

> [!NOTE]
> Como alternativa, podemos poderia ter adicionado uma *função de formatação* para a classe de code-behind de s de página ASP.NET que aceita uma categoria s `CategoryName` e `CategoryID` valores e retorna o `CategoryName` concatenado com o número de produtos na categoria (conforme determinado por meio da chamada a `GetCategoriesByProductID(categoryID)` método). Os resultados de uma função formatação podem ser atribuídos declarativamente para o s LinkButton propriedade de texto, substituindo a necessidade do `ItemDataBound` manipulador de eventos. Consulte a [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) ou [formatação do DataList e Repeater com base em dados](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) tutoriais para obter mais informações sobre como usar funções de formatação.


Depois de adicionar esse manipulador de eventos, reserve um tempo para testar a página por meio de um navegador. Observe como cada categoria é listada em uma lista com marcadores, exibindo o nome da categoria s e o número de produtos associados à categoria (consulte a Figura 4).


[![Cada categoria s nome e o número de produtos são exibidos](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Figura 4**: cada categoria s nome e o número de produtos são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Atualizando o`CategoriesDataTable`e`CategoriesTableAdapter`para incluir o número de produtos para cada categoria

Em vez de determinar o número de produtos para cada categoria como ela s associados a Repeater, podemos pode simplificar esse processo, ajustando a `CategoriesDataTable` e `CategoriesTableAdapter` na camada de acesso a dados para incluir essas informações nativamente. Para conseguir isso, devemos adicionar uma nova coluna à `CategoriesDataTable` para manter o número de produtos associados. Para adicionar uma nova coluna a um DataTable, abra o conjunto de dados tipado (`App_Code\DAL\Northwind.xsd`), clique com botão direito na DataTable para modificar e escolha Adicionar / coluna. Adicionar uma nova coluna para o `CategoriesDataTable` (consulte a Figura 5).


[![Adicionar uma nova coluna para o CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Figura 5**: adicionar uma nova coluna para o `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))


Isso adicionará uma nova coluna chamada `Column1`, que pode ser alterado, simplesmente digitando um nome diferente. Renomeie esta coluna nova para `NumberOfProducts`. Em seguida, precisamos configurar propriedades essa coluna s. Clique na nova coluna e vá para a janela de propriedades. Alterar a coluna s `DataType` propriedade de `System.String` para `System.Int32` e defina o `ReadOnly` propriedade `True`, conforme mostrado na Figura 6.


![Definir as propriedades de somente leitura da nova coluna e tipo de dados](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Figura 6**: defina o `DataType` e `ReadOnly` propriedades da nova coluna


Enquanto o `CategoriesDataTable` agora tem um `NumberOfProducts` coluna, seu valor não é definido por qualquer uma das consultas do s correspondentes do TableAdapter. Podemos atualizar o `GetCategories()` retornado de método para retornar essa informação se quisermos que tais informações toda vez que as informações de categoria são recuperadas. Se, no entanto, precisamos pegar o número de produtos associados para as categorias em casos raros, (por exemplo, apenas como para este tutorial), então podemos deixar `GetCategories()` como-está e criar um novo método que retorna essas informações. Let s usam essa última abordagem, criando um novo método chamado `GetCategoriesAndNumberOfProducts()`.

Para adicionar essa nova `GetCategoriesAndNumberOfProducts()` método, o botão direito do mouse no `CategoriesTableAdapter` e escolha a nova consulta. Isso abre o TableAdapter consultar o Assistente de configuração, o qual estamos ve usado várias vezes nos tutoriais anteriores. Para esse método, inicie o assistente, indicando que a consulta usa uma instrução SQL ad hoc que retorna linhas.


[![Crie o método usando uma instrução de SQL Ad Hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Figura 7**: criar o método usando uma instrução de SQL Ad Hoc ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))


[![A instrução SQL retorna linhas](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Figura 8**: O SQL instrução retorna linhas ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))


A próxima tela do assistente solicita que nós da consulta usar. Para retornar cada categoria s `CategoryID`, `CategoryName`, e `Description` campos, juntamente com o número de produtos associados à categoria, usam o seguinte `SELECT` instrução:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]


[![Especifique a consulta para uso](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Figura 9**: especifique a consulta para uso ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))


Observe que a subconsulta que calcula o número de produtos associados com a categoria é um alias como `NumberOfProducts`. Essa correspondência nomenclatura faz com que o valor retornado por esta subconsulta a ser associada com o `CategoriesDataTable` s `NumberOfProducts` coluna.

Depois de inserir essa consulta, a última etapa é escolher o nome para o novo método. Use `FillWithNumberOfProducts` e `GetCategoriesAndNumberOfProducts` para o preenchimento uma DataTable e retornar uma DataTable padrões, respectivamente.


[![Nome de métodos FillWithNumberOfProducts novo do TableAdapter s e GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Figura 10**: nomear o TableAdapter novo s métodos `FillWithNumberOfProducts` e `GetCategoriesAndNumberOfProducts` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))


Neste momento a camada de acesso a dados foi estendida para incluir o número de produtos por categoria. Uma vez que nossa camada de apresentação encaminha todas as chamadas para o DAL por meio de uma camada de lógica de negócios separadas, precisamos adicionar um correspondente `GetCategoriesAndNumberOfProducts` método para o `CategoriesBLL` classe:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

Com o DAL e BLL completa, podemos re pronto para associar esses dados para o `Categories` Repeater em `CategoriesAndProducts.aspx`! Se ve você já criou um ObjectDataSource para repetidor do determinando o número de produtos na `ItemDataBound` seção do manipulador de eventos, excluir esse ObjectDataSource e remover o Repeater s `DataSourceID` propriedade definindo; sem confusão também o Repeater s `ItemDataBound` evento do manipulador de eventos, removendo o `Handles Categories.OnItemDataBound` sintaxe na classe code-behind ASP.NET.

Com o repetidor de volta em seu estado original, adicione um novo ObjectDataSource chamado `CategoriesDataSource` por meio de marca inteligente Repeater s. Configurar o ObjectDataSource para usar o `CategoriesBLL` classe, mas em vez de fazer com que ele use o `GetCategories()` método, ele tem usar `GetCategoriesAndNumberOfProducts()` em vez disso (veja a Figura 11).


[![Configurar o ObjectDataSource para usar o método GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Figura 11**: configurar o ObjectDataSource para usar o `GetCategoriesAndNumberOfProducts` método ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))


Em seguida, atualize o `ItemTemplate` para que o s LinkButton `Text` propriedade é atribuída declarativamente usando a sintaxe de associação de dados e inclui tanto o `CategoryName` e `NumberOfProducts` campos de dados. A marcação declarativa completa para o Repeater e o `CategoriesDataSource` ObjectDataSource seguem:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

A saída renderizada atualizando a DAL para incluir um `NumberOfProducts` coluna é o mesmo que usar o `ItemDataBound` abordagem do manipulador de eventos (consulte a Figura 4 para ver uma tela de captura do repetidor mostrando os nomes de categoria e o número de produtos).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Etapa 3: Exibindo produtos a categoria selecionada

Neste ponto, temos o `Categories` Repeater para exibir a lista de categorias, juntamente com o número de produtos em cada categoria. O Repeater usa LinkButton para cada categoria que, quando clicado, faz com que uma postagem, no qual ponto podemos precisar exibir esses produtos para a categoria selecionada no `CategoryProducts` DataList.

Um desafio voltados para nós é como ter DataList exibir apenas esses produtos para a categoria selecionada. No [mestre/detalhes usando um GridView mestre selecionável com um DetailsView detalhes](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) tutorial vimos como criar um GridView cujas linhas pode ser selecionada, com a linha selecionada s detalha sendo exibida em um DetailsView na mesma página. O s GridView ObjectDataSource retornadas informações sobre todos os produtos usando o `ProductsBLL` s `GetProducts()` método enquanto o s DetailsView ObjectDataSource recuperar informações sobre o produto selecionado usando o `GetProductsByProductID(productID)` método. O *`productID`* o valor do parâmetro foi fornecido declarativamente por meio de associação com o valor de s o GridView `SelectedValue` propriedade. Infelizmente, o Repeater não tem um `SelectedValue` propriedade e não pode servir como uma fonte de parâmetro.

> [!NOTE]
> Esse é um dos desafios que aparecem quando uso o LinkButton em um repetidor. Podemos tivesse usado um hiperlink para passar o `CategoryID` por meio da cadeia de consulta em vez disso, poderíamos usar esse campo de cadeia de consulta como a origem para o valor do parâmetro s.


Antes de nos preocupamos a falta de um `SelectedValue` propriedade para o Repeater, no entanto, permita s primeiro associar um ObjectDataSource DataList e especifique seu `ItemTemplate`.

Da marca inteligente DataList s, optar por adicionar um novo ObjectDataSource denominado `CategoryProductsDataSource` e configurá-lo para usar o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método. Uma vez que DataList neste tutorial oferece uma interface somente leitura, fique à vontade definir as listas suspensas em INSERT, UPDATE e excluir guias como (nenhum).


[![Configurar o ObjectDataSource para usar a classe ProductsBLL s GetProductsByCategoryID(categoryID) método](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Figura 12**: configurar o ObjectDataSource para uso `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))


Uma vez que o `GetProductsByCategoryID(categoryID)` método espera um parâmetro de entrada (*`categoryID`*), o Assistente Configurar fonte de dados nos permite especificar a origem do parâmetro s. As categorias tinham sido listadas em um GridView ou DataList, d definimos a lista de lista suspensa de origem do parâmetro para o controle e a ID de controle para o `ID` dos dados de controle da Web. No entanto, como a falta de Repeater um `SelectedValue` propriedade, ele não pode ser usado como uma fonte de parâmetro. Se você verificar, você encontrará a lista de suspensa ControlID só contém um controle `ID``CategoryProducts`, o `ID` do DataList.

Por enquanto, defina a lista de lista suspensa de origem do parâmetro como None. Poderemos programaticamente atribuir esse valor de parâmetro quando uma categoria que LinkButton é clicado no Repetidor.


[![Fazer não especificar uma fonte de parâmetro para o parâmetro categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Figura 13**: não especificar uma fonte de parâmetro para o *`categoryID`* parâmetro ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio gera automaticamente DataList s `ItemTemplate`. Substituir esse padrão `ItemTemplate` com o modelo usado no tutorial anterior; Além disso, defina DataList s `RepeatColumns` propriedade como 2. Depois de fazer essas alterações, a marcação declarativa para seu controle DataList e o seu ObjectDataSource associado deve ser semelhante ao seguinte:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Atualmente, o `CategoryProductsDataSource` s ObjectDataSource *`categoryID`* parâmetro nunca é definido para nenhum produto sejam exibido ao exibir a página. O que precisamos fazer é ter esse valor de parâmetro definido com base no `CategoryID` da categoria clicada no Repetidor. Isso apresenta dois desafios: primeiro, como podemos determinar quando um LinkButton em s repetidor `ItemTemplate` foi clicado; e segundo, como podemos determinar a `CategoryID` da categoria correspondente cujo LinkButton foi clicado?

No botão LinkButton como os controles de botão e ImageButton tem um `Click` evento e uma [ `Command` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). O `Click` evento foi projetado para simplesmente observam que foi clicado no botão LinkButton. Às vezes, no entanto, além de observar o que foi clicado no botão LinkButton também precisamos passar algumas informações extras para o manipulador de eventos. Se esse for o caso, o s LinkButton [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) e [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) propriedades podem ser atribuídas a essas informações extras. Em seguida, quando você clica no botão LinkButton, sua `Command` evento é acionado (em vez de seu `Click` eventos) e o manipulador de eventos recebe os valores da `CommandName` e `CommandArgument` propriedades.

Quando um `Command` é gerado de dentro de um modelo no repetidor, s o Repeater [ `ItemCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) é acionado e é passado a `CommandName` e `CommandArgument` valores no LinkButton clicado (ou botão ou ImageButton). Portanto, para determinar quando uma categoria LinkButton no repetidor foi clicada, é preciso fazer o seguinte:

1. Defina a `CommandName` propriedade no LinkButton no repetidor s `ItemTemplate` para algum valor (eu ve usado ListProducts). Definindo isso `CommandName` de valor, o s LinkButton `Command` evento é acionado quando o usuário clica no botão LinkButton.
2. Definir o s LinkButton `CommandArgument` propriedade para o valor do item atual s `CategoryID`.
3. Criar um manipulador de eventos para o Repeater s `ItemCommand` eventos. No conjunto de eventos manipulador, o `CategoryProductsDataSource` s ObjectDataSource `CategoryID` parâmetro para o valor passado na `CommandArgument`.

O seguinte `ItemTemplate` marcação para o Repeater categorias implementa as etapas 1 e 2. Observe como o `CommandArgument` valor é atribuído o item de dados s `CategoryID` usando a sintaxe de associação de dados:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Sempre que criar uma `ItemCommand` manipulador de eventos é prudente sempre primeiro, verifique a entrada `CommandName` valor porque *qualquer* `Command` evento gerado por *qualquer* botão, LinkButton, ou ImageButton dentro do repetidor fará com que o `ItemCommand` evento seja acionado. Enquanto atualmente só temos um tal LinkButton agora, no futuro nós (ou outro desenvolvedor em nossa equipe) pode adicionar controles de Web de botão adicionais para o Repeater que, quando clicado, gera o mesmo `ItemCommand` manipulador de eventos. Portanto, ele s melhor sempre verifique se você o `CommandName` propriedade e que apenas prosseguir com sua lógica de programação se ele corresponde ao valor esperado.

Depois de garantir que o passado `CommandName` ListProducts é igual a, o manipulador de eventos, em seguida, atribui o `CategoryProductsDataSource` s ObjectDataSource `CategoryID` parâmetro para o valor passado na `CommandArgument`. Essa modificação no s ObjectDataSource `SelectParameters` automaticamente faz com que DataList reassociar próprio à fonte de dados, mostrando os produtos para a categoria selecionada recentemente.


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Com essas adições, nosso tutorial está completo! Reserve um tempo para testá-lo em um navegador. Figura 14 mostra a tela quando o primeiro visitando a página. Uma vez que uma categoria ainda deve ser selecionado, não há produtos são exibidos. Clicar em uma categoria, como produzir, exibe os produtos na categoria de produto em uma exibição de duas colunas (veja a Figura 15).


[![Nenhum produto é exibidas quando primeiro visitando a página](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Figura 14**: os produtos não são exibidos quando primeiro visitando a página ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))


[![Clicar em listas de categorias de produzir os produtos correspondentes à direita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Figura 15**: clicando na categoria de produzir lista os produtos de correspondência para a direita ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))


## <a name="summary"></a>Resumo

Como vimos neste tutorial e anterior, relatórios mestre/detalhes podem ser distribuídas em duas páginas ou consolidados em um. Exibindo um relatório de detalhes/mestre em uma única página, no entanto, apresenta alguns desafios sobre como melhor layout mestre e registros de detalhes na página. No *mestre/detalhes usando um GridView mestre selecionável com um DetailsView detalhes* tutorial tínhamos os registros de detalhes são exibidos acima os registros mestres; neste tutorial, usamos as técnicas de CSS para ter o float registros mestre para o à esquerda dos detalhes.

Juntamente com exibindo relatórios de detalhes/mestre, também tivemos a oportunidade de explorar como recuperar o número de produtos associados com cada categoria, bem como executar a lógica do lado do servidor quando um LinkButton (ou botão ou ImageButton) é clicado de dentro um Repetidor.

Esse tutorial conclui nossa análise do relatórios mestre/detalhes com o DataList e Repeater. Nosso próximo conjunto de tutoriais ilustrará como adicionar, editar e excluir recursos para o controle DataList.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) um tutorial sobre flutuante elementos CSS com CSS
- [O posicionamento de CSS](http://www.brainjar.com/css/positioning/) obter mais informações sobre o posicionamento de elementos com CSS
- [Definindo o layout Out conteúdo com HTML](http://www.w3schools.com/html/html_layout.asp) usando `<table>` s e outros elementos HTML para posicionamento

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Zack Jones. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-acess-two-pages-datalist-vb.md)
