---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Mestre/detalhes usando uma lista com marcadores de registros mestre com detalhes DataList (VB) | Microsoft Docs
author: rick-anderson
description: "Neste tutorial, será compactar o relatório de duas páginas mestre/detalhes do tutorial anterior em uma única página, mostrando uma lista com marcadores de nomes de categoria em t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 613ad1fb101a168c79310c9dc7bf731be264f889
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Mestre/detalhes usando uma lista com marcadores de registros mestre com detalhes DataList (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) ou [baixar PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> Neste tutorial, será compactar o relatório de duas páginas mestre/detalhes do tutorial anterior em uma única página, mostrando uma lista com marcadores de nomes de categoria no lado esquerdo da tela e produtos da categoria selecionada no lado direito da tela.


## <a name="introduction"></a>Introdução

No [tutorial anterior](master-detail-filtering-acess-two-pages-datalist-vb.md) examinamos como separar um relatório mestre/detalhes em duas páginas. Na página mestra, usamos um controle repetidor para processar uma lista com marcadores de categorias. Cada nome de categoria foi um hiperlink que, quando clicado, seria tirar o usuário para a página de detalhes, onde duas colunas DataList mostrou os produtos que pertencem à categoria selecionada.

Neste tutorial, será compactar o tutorial de duas páginas em uma única página, mostrando uma lista com marcadores de nomes de categoria no lado esquerdo da tela com cada nome de categoria renderizado como um LinkButton. Clicar em um dos botões de link do nome da categoria induzir um postback e associa os produtos a categoria selecionada ao DataList duas colunas à direita da tela. Além de exibir cada nome de categoria s, repetidor à esquerda mostra o número total de produtos há é para uma determinada categoria (consulte a Figura 1).


[![A categoria s nome e o número Total de produtos são exibidos à esquerda](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Figura 1**: s categoria o nome e o número Total de produtos são exibidos à esquerda ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Etapa 1: Exibir um repetidor no lado esquerdo da tela

Para este tutorial, precisamos ter a lista com marcadores de categorias aparecem à esquerda dos produtos s categoria selecionada. Conteúdo em uma página da web pode ser posicionado usando marcas HTML padrão elementos parágrafo, espaços não separáveis, `<table>` s e assim por diante ou por meio de técnicas de (CSS) da folha de estilos em cascata. Todos os nossos tutoriais até o momento usou as técnicas de CSS para posicionamento. Quando criamos a interface do usuário de navegação na nossa página mestre a [páginas mestras e navegação de Site](../introduction/master-pages-and-site-navigation-vb.md) tutorial usamos *posicionamento absoluto*, indicando o deslocamento de pixel precisos para a navegação lista e o conteúdo principal. Como alternativa, o CSS pode ser usado para posicionar um elemento para a direita ou esquerda do outro por meio de *flutuante*. Podemos ter a lista com marcadores de categorias aparecem à esquerda dos produtos s categoria selecionada por flutuante repetidor à esquerda do DataList

Abrir o `CategoriesAndProducts.aspx` página a partir de `DataListRepeaterFiltering` pasta e adicionar à página um repetidor e DataList. Definir repetidor s `ID` para `Categories` e DataList s para `CategoryProducts`. Vá para o modo de exibição de fonte e os controles do repetidor e DataList dentro de seus próprios `<div>` elementos. Ou seja, coloque o repetidor dentro de um `<div>` elemento primeiro e, em seguida, DataList no seu próprio `<div>` elemento diretamente após Repetidor. A marcação agora deve ser semelhante ao seguinte:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Para float repetidor à esquerda do DataList, precisamos usar o `float` atributo de estilo CSS, da seguinte forma:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

O `float: left;` flutua primeiro `<div>` elemento à esquerda de um segundo. O `width` e `padding-right` configurações indicam o primeiro `<div>` s `width` e a quantidade de preenchimento é adicionado entre o `<div>` conteúdo de elemento s e a margem direita. Para obter mais informações sobre flutuante elementos no CSS check-out de [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Em vez de especificar a configuração de estilo diretamente por meio do primeiro `<p>` elemento s `style` de atributo, permitem que o s em vez disso, crie uma nova classe CSS em `Styles.css` chamado `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Em seguida, substituímos o `<div>` com `<div class="FloatLeft">`.

Depois de adicionar a classe CSS e configurar a marcação no `CategoriesAndProducts.aspx` página, vá para o Designer. Você deve ver o repetidor flutuante à esquerda do DataList (embora direita agora ambos apenas são exibidos como caixas cinzas desde ve ainda devem configurar suas fontes de dados ou modelos).


[![Repetidor é flutuante à esquerda do DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Figura 2**: O repetidor é flutuante à esquerda do DataList ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Etapa 2: Determinar o número de produtos para cada categoria

Com o s repetidor e DataList ao redor marcação concluída, podemos re pronto para associar os dados de categoria ao repetidor controlar. No entanto, como mostra a lista com marcadores de categorias na Figura 1, além de cada nome de categoria s também precisamos exibir o número de produtos associados à categoria. Para acessar essas informações, pode:

- **Determine essas informações de classe de code-behind da página s ASP.NET.** Dado um determinado  *`categoryID`*  podemos determinar o número de produtos associados ao chamar o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método. Este método retorna um `ProductsDataTable` do objeto cuja `Count` propriedade indica quantos `ProductsRow` s existe, que é o número de produtos para especificado  *`categoryID`* . Podemos criar um `ItemDataBound` manipulador de eventos repetidor que, para cada categoria associada ao repetidor, chama o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método e inclui a contagem na saída.
- **Atualização de `CategoriesDataTable` no conjunto de dados tipado para incluir um `NumberOfProducts` coluna.** Podemos, em seguida, atualizar o `GetCategories()` método o `CategoriesDataTable` para incluir essas informações ou, Alternativamente, deixe `GetCategories()` como-é e criar um novo `CategoriesDataTable` método chamado `GetCategoriesAndNumberOfProducts()`.

Permitir que o s explorar ambas as técnicas. A primeira abordagem é mais simples de implementar, já que estamos não precisa atualizar a camada de acesso de dados; No entanto, ele exige mais comunicações com o banco de dados. A chamada para o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método o `ItemDataBound` manipulador de eventos adiciona uma chamada de banco de dados extra para cada categoria exibida no Repetidor. Com essa técnica, há *N* + chamadas de banco de 1 dados, onde *N* é o número de categorias exibidas no Repetidor. Com a segunda abordagem, a contagem de produto é retornada com informações sobre cada categoria do `CategoriesBLL` classe s `GetCategories()` (ou `GetCategoriesAndNumberOfProducts()`) método, resultando assim em apenas uma viagem ao banco de dados.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Determinando o número de produtos no manipulador de eventos ItemDataBound

Determinar o número de produtos para cada categoria no repetidor s `ItemDataBound` manipulador de eventos não exigem modificações para nossa camada de acesso de dados existente. Todas as modificações podem ser feitas diretamente a `CategoriesAndProducts.aspx` página. Comece adicionando um novo ObjectDataSource denominado `CategoriesDataSource` por meio de marca inteligente repetidor s. Em seguida, configure o `CategoriesDataSource` ObjectDataSource para que ela recupera seus dados a partir de `CategoriesBLL` classe s `GetCategories()` método.


[![Configurar o ObjectDataSource para usar a classe CategoriesBLL s GetCategories() método](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Figura 3**: configurar o ObjectDataSource para usar o `CategoriesBLL` classe s `GetCategories()` método ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))


Cada item de `Categories` repetidor precisa ser clicáveis e, quando clicado, fazer com que o `CategoryProducts` DataList para exibir os produtos para a categoria selecionada. Isso pode ser feito, tornando um hiperlink, vinculando novamente essa mesma página de cada categoria (`CategoriesAndProducts.aspx`), mas passando o `CategoryID` por meio de querystring, assim como vimos no tutorial anterior. A vantagem dessa abordagem é que uma página que exibe produtos de uma determinada categoria s pode ser um indicador e indexada por um mecanismo de pesquisa.

Como alternativa, podemos tornar cada categoria LinkButton, que é a abordagem que usaremos para este tutorial. O LinkButton é renderizado no navegador do usuário s como um hiperlink, mas, quando clicado, induzir um postback; no postback, o s DataList ObjectDataSource deve ser atualizado para exibir os produtos que pertencem à categoria selecionada. Neste tutorial, usar um hiperlink faz mais sentido que o uso de um LinkButton; No entanto, pode haver outros cenários em que é mais vantajoso usar LinkButton. Enquanto a abordagem de hiperlink seria ideal para este exemplo, permitem s em vez disso, explorar usando o LinkButton. Como veremos, usar um LinkButton apresenta alguns desafios que não se surgiria, caso contrário, com um hiperlink. Portanto, usar um LinkButton neste tutorial será realce esses desafios e ajudar a fornecer soluções para esses cenários onde podemos querer usar um LinkButton em vez de um hiperlink.

> [!NOTE]
> Você é incentivado a repetir este tutorial usando um controle de hiperlink ou `<a>` elemento em vez de no LinkButton.


A marcação a seguir mostra a sintaxe declarativa para repetidor e ObjectDataSource. Observe que os modelos de s repetidor renderizam uma lista com marcadores com cada item como um LinkButton:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> Para este tutorial repetidor deve ter seu estado de exibição habilitado (Observe a omissão do `EnableViewState="False"` da sintaxe declarativa repetidor s). Na etapa 3, criará um manipulador de eventos para repetidor s `ItemCommand` eventos em que estamos será atualizando DataList s s ObjectDataSource `SelectParameters` coleção. Repetidor s `ItemCommand`, no entanto, ganha incêndio t se o estado de exibição está desabilitado. Consulte [um desafio de uma pergunta ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) e [sua solução](http://scottonwriting.net/sowBlog/posts/1268.aspx) para obter mais informações sobre por que o estado de exibição deve ser habilitado para um repetidor s `ItemCommand` evento seja acionado.


No LinkButton com o `ID` valor da propriedade `ViewCategory` não tem seu `Text` conjunto de propriedades. Se tivesse gostaríamos de exibir o nome da categoria, ter definiríamos a propriedade Text declarativamente, por meio da sintaxe de associação de dados, da seguinte forma:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

No entanto, queremos mostrar o nome de categoria s *e* o número de produtos que pertencem a essa categoria. Essas informações podem ser recuperadas do repetidor s `ItemDataBound` manipulador de eventos, fazendo uma chamada para o `ProductBLL` classe s `GetCategoriesByProductID(categoryID)` método e a determinar quantos registros são retornados em resultante `ProductsDataTable`, como o código a seguir ilustra:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Começamos, garantindo que nós está trabalhando com um item de dados (um cujo `ItemType` é `Item` ou `AlternatingItem`) e, em seguida, fazer referência a `CategoriesRow` instância apenas associado ao atual `RepeaterItem`. Em seguida, podemos determinar o número de produtos para essa categoria, criando uma instância do `ProductsBLL` classe chamando seu `GetCategoriesByProductID(categoryID)` método e determinar o número de registros retornados usando o `Count` propriedade. Por fim, o `ViewCategory` LinkButton ItemTemplate faz referências e sua `Text` está definida como *CategoryName* (*NumberOfProductsInCategory*), onde  *NumberOfProductsInCategory* é formatado como um número com zero casas decimais.

> [!NOTE]
> Como alternativa, podemos poderia ter adicionado uma *formatação função* para a classe de code-behind de páginas ASP.NET que aceita uma categoria s `CategoryName` e `CategoryID` valores e retorna o `CategoryName` concatenado com o número de produtos na categoria (conforme determinado chamando o `GetCategoriesByProductID(categoryID)` método). Os resultados de uma função formatação podem ser atribuídos declarativamente para o s LinkButton propriedade de texto, substituindo a necessidade do `ItemDataBound` manipulador de eventos. Consulte o [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) ou [formatação de DataList e com base em dados repetidor](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) tutoriais para obter mais informações sobre como usar funções de formatação.


Depois de adicionar este manipulador de eventos, dedique alguns momentos para testar a página por meio de um navegador. Observe como cada categoria é listada em uma lista com marcadores, exibindo o nome da categoria s e o número de produtos associados a categoria (consulte a Figura 4).


[![Cada categoria s nome e o número de produtos são exibidos](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Figura 4**: s de cada categoria nome e número de produtos são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Atualizando o`CategoriesDataTable`e`CategoriesTableAdapter`para incluir o número de produtos para cada categoria

Em vez de determinar o número de produtos para cada categoria como s associado ao repetidor, é possível simplificar esse processo, ajustando o `CategoriesDataTable` e `CategoriesTableAdapter` na camada de acesso a dados para incluir essas informações nativamente. Para fazer isso, devemos adicionar uma nova coluna à `CategoriesDataTable` para manter o número de produtos associados. Para adicionar uma nova coluna a um DataTable, abra o conjunto de dados tipado (`App_Code\DAL\Northwind.xsd`), clique duas vezes em DataTable para modificar e escolha Adicionar / coluna. Adicionar uma nova coluna para o `CategoriesDataTable` (consulte a Figura 5).


[![Adicionar uma nova coluna para o CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Figura 5**: adicionar uma nova coluna para o `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))


Isso adicionará uma nova coluna chamada `Column1`, que você pode alterar digitando um nome diferente. Renomear essa nova coluna para `NumberOfProducts`. Em seguida, é preciso configurar estas propriedades de coluna s. Clique na nova coluna e vá para a janela de propriedades. Alterar a coluna s `DataType` propriedade `System.String` para `System.Int32` e defina o `ReadOnly` propriedade `True`, conforme mostrado na Figura 6.


![Definir as propriedades de somente leitura da nova coluna e tipo de dados](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Figura 6**: definir o `DataType` e `ReadOnly` propriedades da nova coluna


Enquanto o `CategoriesDataTable` agora tem um `NumberOfProducts` coluna, seu valor não é definido por qualquer uma das consultas de s TableAdapter correspondentes. Podemos atualizar o `GetCategories()` método para retornar essas informações se quisermos tais informações retornado toda vez que as informações de categoria são recuperadas. Se, no entanto, precisamos obter o número de produtos associados para as categorias em casos raros (por exemplo, como apenas para este tutorial), é possível deixar `GetCategories()` como-é e criar um novo método que retorna essas informações. Permitem s usar essa segunda abordagem, criando um novo método chamado `GetCategoriesAndNumberOfProducts()`.

Adicione novos `GetCategoriesAndNumberOfProducts()` método, com o botão direito no `CategoriesTableAdapter` e escolha nova consulta. Isso traz up Assistente de configuração, que é de consulta do TableAdapter ve usado várias vezes em tutoriais anteriores. Para esse método, inicie o assistente, indicando que a consulta usa uma instrução SQL ad hoc que retorna linhas.


[![Crie o método usando uma instrução de SQL Ad Hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Figura 7**: criar o método usando uma instrução de SQL Ad Hoc ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))


[![A instrução SQL retorna linhas](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Figura 8**: O SQL instrução retorna linhas ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))


A próxima tela do assistente solicitará a nós da consulta usar. Para retornar cada categoria s `CategoryID`, `CategoryName`, e `Description` campos, juntamente com o número de produtos associados à categoria, usam a seguinte `SELECT` instrução:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]


[![Especifique a consulta para uso](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Figura 9**: especifique a consulta para uso ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))


Observe que a subconsulta que calcula o número de produtos associados a categoria é um alias como `NumberOfProducts`. Essa correspondência nomenclatura faz com que o valor retornado por esta subconsulta deve ser associado a `CategoriesDataTable` s `NumberOfProducts` coluna.

Depois de inserir essa consulta, a última etapa é escolher o nome para o novo método. Use `FillWithNumberOfProducts` e `GetCategoriesAndNumberOfProducts` para o preenchimento uma DataTable e retornar uma DataTable padrões, respectivamente.


[![Nome do novo FillWithNumberOfProducts de métodos de s TableAdapter e GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Figura 10**: nomear o s novo TableAdapter métodos `FillWithNumberOfProducts` e `GetCategoriesAndNumberOfProducts` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))


Neste ponto da camada de acesso a dados foi estendida para incluir o número de produtos por categoria. Como todos os nossos camada de apresentação encaminha todas as chamadas para a DAL por meio de uma camada de lógica de negócios separada que precisamos adicionar um correspondente `GetCategoriesAndNumberOfProducts` método para o `CategoriesBLL` classe:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

Com a DAL e BLL completa, podemos re pronto para associar esses dados para o `Categories` repetidor em `CategoriesAndProducts.aspx`! Se que você já criou um ObjectDataSource para repetidor do determinando o número de produtos no `ItemDataBound` seção do manipulador de eventos, exclua este ObjectDataSource e remover repetidor s `DataSourceID` propriedade configuração; sem confusão também o Repetidor s `ItemDataBound` evento do manipulador de eventos, removendo o `Handles Categories.OnItemDataBound` sintaxe na classe code-behind ASP.NET.

Com repetidor em seu estado original, adicione um novo ObjectDataSource denominado `CategoriesDataSource` por meio de marca inteligente repetidor s. Configurar o ObjectDataSource para usar o `CategoriesBLL` classe, mas em vez de fazer com que ele use o `GetCategories()` método, ele tem usar `GetCategoriesAndNumberOfProducts()` em vez disso, (consulte a Figura 11).


[![Configurar o ObjectDataSource para usar o método GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Figura 11**: configurar o ObjectDataSource para usar o `GetCategoriesAndNumberOfProducts` método ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))


Em seguida, atualize o `ItemTemplate` para que o s LinkButton `Text` propriedade recebe declarativamente usando a sintaxe de associação de dados e inclui tanto o `CategoryName` e `NumberOfProducts` campos de dados. A marcação declarativa completa para repetidor e `CategoriesDataSource` ObjectDataSource seguem:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

A saída renderizada atualizando a DAL para incluir um `NumberOfProducts` coluna é o mesmo que usar o `ItemDataBound` abordagem de manipulador de eventos (consulte a Figura 4 para ver uma tela de captura do repetidor mostrando os nomes de categoria e o número de produtos).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Etapa 3: Exibir os produtos a categoria selecionada

Neste ponto, temos o `Categories` repetidor exibindo a lista de categorias, juntamente com o número de produtos em cada categoria. Repetidor usa LinkButton para cada categoria que, quando clicado, faz com que uma postagem, em que ponto é necessário para exibir os produtos para a categoria selecionada no `CategoryProducts` DataList.

Um desafio voltada para nós é como ter DataList exibir apenas os produtos para a categoria selecionada. No [mestre/detalhes usando um GridView mestre selecionável com um DetailsView detalhes](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) tutorial vimos como criar um controle GridView cujas linhas pode ser selecionada, com a linha selecionada s detalhes que está sendo exibido em um DetailsView na mesma página. O GridView s ObjectDataSource retornou informações sobre todos os produtos usando a `ProductsBLL` s `GetProducts()` método enquanto a s DetailsView ObjectDataSource recuperar informações sobre o produto selecionado usando o `GetProductsByProductID(productID)` método. O  *`productID`*  o valor do parâmetro foi fornecido declarativamente pelo associá-lo com o valor da s GridView `SelectedValue` propriedade. Infelizmente, repetidor não tem um `SelectedValue` propriedade e não pode servir como uma fonte de parâmetro.

> [!NOTE]
> Isso é um dos desafios que aparecem ao usar o LinkButton em um repetidor. Podemos tiver usado um hiperlink para passar o `CategoryID` por meio de querystring em vez disso, podemos usar esse campo QueryString como a origem para o valor do parâmetro s.


Antes que se preocupar sobre a falta de um `SelectedValue` para repetidor, porém, let de propriedade s primeiro associar DataList um ObjectDataSource e especifique seu `ItemTemplate`.

Marca inteligente DataList s, optar por adicionar um novo ObjectDataSource denominado `CategoryProductsDataSource` e configurá-lo para usar o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método. Como o DataList neste tutorial oferece uma interface somente leitura, fique à vontade para definir a lista suspensa de INSERT, UPDATE e excluir guias como (nenhum).


[![Configurar o ObjectDataSource para usar a classe ProductsBLL s GetProductsByCategoryID(categoryID) método](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Figura 12**: configurar o ObjectDataSource usar `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))


Como o `GetProductsByCategoryID(categoryID)` método espera um parâmetro de entrada (*`categoryID`*), o Assistente Configurar fonte de dados nos permite especificar a origem do parâmetro s. As categorias estivessem listadas em um controle GridView ou DataList, d fizemos lista suspensa de origem de parâmetros para controle e a ID de controle para o `ID` dos dados de controle da Web. No entanto, como a falta de Repetidor um `SelectedValue` propriedade, ele não pode ser usado como uma fonte de parâmetro. Se você verificar, você verá que a lista suspensa ControlID contém apenas um controle `ID``CategoryProducts`, o `ID` do DataList.

Por enquanto, defina a lista suspensa de origem de parâmetros como None. Terminaremos programaticamente atribuir esse valor de parâmetro quando uma categoria que LinkButton é clicado no Repetidor.


[![Faça não especificar uma fonte de parâmetro para o parâmetro categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Figura 13**: não especificar uma fonte de parâmetro para o  *`categoryID`*  parâmetro ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio gera automaticamente o DataList s `ItemTemplate`. Substituir esse padrão `ItemTemplate` com o modelo usado no tutorial anterior; Além disso, defina o DataList s `RepeatColumns` propriedade para 2. Após fazer essas alterações a marcação declarativa para DataList e seu associado ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Atualmente, o `CategoryProductsDataSource` ObjectDataSource s  *`categoryID`*  parâmetro nunca é definido para nenhum produto sejam exibido quando a página de exibição. O que precisamos fazer é ter esse valor de parâmetro definido com base no `CategoryID` da categoria clicada no Repetidor. Isso apresenta dois desafios: primeiro, como podemos para determinar quando um LinkButton no repetidor s `ItemTemplate` foi clicado; e o segundo, como podemos determinar a `CategoryID` da categoria correspondente cujo LinkButton foi clicado?

LinkButton com os controles de botão e ImageButton tem um `Click` eventos e uma [ `Command` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). O `Click` evento foi projetado para simplesmente Observe que no LinkButton foi clicado. Às vezes, no entanto, além de observar que o LinkButton clicado também precisamos passar informações extras para o manipulador de eventos. Se esse for o caso, o s LinkButton [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) e [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) propriedades podem ser atribuídas a essas informações extras. Em seguida, quando você clica no botão LinkButton, seu `Command` evento ser acionado (em vez de seu `Click` eventos) e o manipulador de eventos é passado os valores da `CommandName` e `CommandArgument` propriedades.

Quando um `Command` é gerado de dentro de um modelo no repetidor, o s repetidor [ `ItemCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) é acionado e é passado a `CommandName` e `CommandArgument` valores no LinkButton clicado (ou botão ou ImageButton). Portanto, para determinar quando uma categoria LinkButton no repetidor foi clicada, é preciso fazer o seguinte:

1. Definir o `CommandName` propriedade LinkButton no repetidor s `ItemTemplate` para algum valor (usou o ListProducts). Definindo isso `CommandName` de valor, o s LinkButton `Command` evento é acionado quando o LinkButton é clicado.
2. Definir o s LinkButton `CommandArgument` propriedade para o valor do item atual s `CategoryID`.
3. Criar um manipulador de eventos para repetidor s `ItemCommand` eventos. No conjunto de eventos manipulador, o `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parâmetro para o valor passado na `CommandArgument`.

O seguinte `ItemTemplate` marcação para repetidor categorias implementa as etapas 1 e 2. Observe como o `CommandArgument` valor é atribuído o item de dados s `CategoryID` usando a sintaxe de associação de dados:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Sempre que criar um `ItemCommand` manipulador de eventos, é recomendável sempre primeiro verifique a entrada `CommandName` valor porque *qualquer* `Command` evento gerado por *qualquer* botão, LinkButton, ou ImageButton em repetidor fará com que o `ItemCommand` evento seja acionado. Enquanto atualmente só temos um tal LinkButton agora, no futuro, (ou outro desenvolvedor em nossa equipe) pode adicionar controles da Web de botão adicionais a repetidor que, quando clicado, gera o mesmo `ItemCommand` manipulador de eventos. Portanto, ele s melhor sempre verifique se você marcou a `CommandName` propriedade e continuar somente com sua lógica de programação se corresponde ao valor esperado.

Depois de garantir que transmitido `CommandName` valor é igual a ListProducts, o manipulador de eventos, em seguida, atribui o `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parâmetro para o valor passado na `CommandArgument`. A modificação da s ObjectDataSource `SelectParameters` automaticamente faz com que o DataList reassociar-se à fonte de dados, mostrando os produtos para a categoria selecionada recentemente.


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Com essas adições, nosso tutorial está concluído! Responda testá-lo em um navegador. Figura 14 mostra a tela ao primeiro visitar a página. Como uma categoria ainda deve ser selecionado, nenhum produto é exibido. Clicar em uma categoria, como produzir, exibe os produtos na categoria de produto em uma exibição de duas colunas (consulte a Figura 15).


[![Nenhum produto é exibidas quando primeiro visitar a página](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Figura 14**: produtos não são exibidos quando primeiro visitar a página ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))


[![Clicando em listas de categorias produzir os produtos correspondentes à direita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Figura 15**: clicando na categoria produzir lista os produtos correspondentes à direita ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))


## <a name="summary"></a>Resumo

Como vimos neste tutorial e anterior, relatórios mestre/detalhes podem ser distribuídas em duas páginas ou consolidados em um. Exibindo um relatório de detalhes/mestre em uma única página, no entanto, apresenta alguns desafios sobre como melhor layout mestre e registros de detalhes na página. No *mestre/detalhes usando um GridView mestre selecionável com um DetailsView detalhes* tutorial tínhamos os registros de detalhes são exibidas abaixo os registros mestres; neste tutorial, usamos as técnicas de CSS para ter o valor de float registros mestre para o à esquerda dos detalhes.

Além de exibir relatórios de detalhes/mestre, também foi a oportunidade de explorar como recuperar o número de produtos associados a cada categoria, bem como executar lógica de servidor quando um LinkButton (ou botão ou ImageButton) é clicado em um Repetidor.

Este tutorial conclui nossa análise do relatórios mestre/detalhes com o controle DataList e Repetidor. Nosso próximo conjunto de tutoriais ilustrará como adicionar, editar e excluir recursos para o controle DataList.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) um tutorial flutuante elementos CSS com CSS
- [Posicionamento CSS](http://www.brainjar.com/css/positioning/) obter mais informações sobre o posicionamento de elementos com o CSS
- [Definir o layout de limite conteúdo HTML](http://www.w3schools.com/html/html_layout.asp) usando `<table>` s e outros elementos HTML para posicionamento

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Zack Jones. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](master-detail-filtering-acess-two-pages-datalist-vb.md)
