---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Mestre/detalhes filtragem com uma DropDownList (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, podemos ver como exibir relatórios mestre/detalhes em uma única página da web usando DropDownLists para exibir os registros de 'mestres' e uma DataList para ex...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: ed2631e49786c81075099cca6941d98ba3b67e37
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818246"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Mestre/detalhes filtragem com uma DropDownList (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) ou [baixar PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> Neste tutorial, podemos ver como exibir relatórios mestre/detalhes em uma única página da web usando DropDownLists para exibir os registros "mestres" e uma DataList para exibir os "Detalhes".


## <a name="introduction"></a>Introdução

O relatório de detalhes mestre, primeiro criamos usando um GridView no anterior [filtragem de mestre/detalhes com uma DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) início do tutorial, mostrando um conjunto de registros "mestres". O usuário pode, em seguida, fazer drill down em um dos registros mestres, assim, detalhes do registro mestre "." Relatórios mestre/detalhes são uma opção ideal para visualizar relações um-para-muitos e para exibir informações detalhadas de tabelas particularmente "larga" (aqueles que têm muitas colunas). Exploramos como implementar os relatórios mestre/detalhes usando os controles GridView e DetailsView nos tutoriais anteriores. Neste tutorial e nas próximas duas, podemos vai reexaminar esses conceitos, mas o foco sobre como usar o DataList e Repeater controla em vez disso.

Neste tutorial, vamos examinar usando DropDownList para conter os registros de "master", com os registros de "Detalhes" exibidos no DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Etapa 1: Adicionar as páginas da Web Tutorial de mestre/detalhes

Antes de começar este tutorial, primeiro vamos adicionar a pasta e as páginas do ASP.NET, precisaremos para este tutorial e as próximas duas lidando com os relatórios mestre/detalhes usando os controles DataList e Repeater. Comece criando uma nova pasta no projeto chamado `DataListRepeaterFiltering`. Em seguida, adicione as seguintes páginas ASP.NET cinco nesta pasta, manter todos eles configurado para usar a página mestra `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Crie uma pasta DataListRepeaterFiltering e adicione as páginas do Tutorial do ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Figura 1**: criar um `DataListRepeaterFiltering` pasta e adicione as páginas do Tutorial do ASP.NET


Em seguida, abra o `Default.aspx` da página e arraste o `SectionLevelTutorialListing.ascx` controle de usuário do `UserControls` pasta para a superfície de Design. Esse controle de usuário que criamos na [páginas mestras e navegação no Site](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumera o mapa do site e exibe os tutoriais da seção atual em uma lista com marcadores.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Figura 2**: Adicione o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


Para ter a exibição de lista com marcadores os tutoriais de mestre/detalhes, criaremos, precisamos adicioná-los ao mapa do site. Abra o `Web.sitemap` arquivo e adicione a marcação a seguir após a marcação de nó de mapa de site "Exibindo dados com o DataList e Repeater":

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![Atualizar o mapa de Site para incluir as novas páginas do ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Figura 3**: atualizar o mapa de Site para incluir as novas páginas do ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Etapa 2: Exibindo as categorias na DropDownList

Nosso relatório mestre/detalhes listará as categorias na DropDownList, com produtos do item de lista selecionado exibidos mais adiante na página em um DataList. A primeira tarefa à frente de nós, em seguida, é ter as categorias exibidas na DropDownList. Comece abrindo o `FilterByDropDownList.aspx` página o `DataListRepeaterFiltering` pasta e arraste uma DropDownList da caixa de ferramentas para o designer da página. Em seguida, defina a DropDownList `ID` propriedade para `Categories`. Clique no link na marca inteligente do DropDownList Escolher fonte de dados e criar um novo ObjectDataSource chamado `CategoriesDataSource`.


[![Adicionar um novo ObjectDataSource chamado CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Figura 4**: adicionar um novo ObjectDataSource nomeado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


Configurar o novo ObjectDataSource, de modo que ele chama o `CategoriesBLL` da classe `GetCategories()` método. Depois de configurar o ObjectDataSource ainda precisamos especificar qual campo de fonte de dados deve ser exibido na DropDownList e que um deve ser associado como o valor para cada item de lista. Ter o `CategoryName` campo, como a exibição e `CategoryID` como o valor para cada item de lista.


[![Ter a exibição DropDownList CategoryName campo e Use CategoryID como o valor](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Figura 5**: têm a exibição DropDownList a `CategoryName` campo e Use `CategoryID` como o valor ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


Neste ponto, temos um controle DropDownList que é preenchido com os registros da `Categories` tabela (tudo feito em cerca de seis segundos). Figura 6 mostra nosso progresso até o momento quando visualizado por meio de um navegador.


[![Uma lista suspensa lista as categorias atuais](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Figura 6**: um menu suspenso lista as categorias atual ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Etapa 2: Adicionando DataList produtos

A última etapa do nosso relatório mestre/detalhes é listar os produtos associados a categoria selecionada. Para fazer isso, adicione uma DataList à página e criar um novo ObjectDataSource chamado `ProductsByCategoryDataSource`. Ter o `ProductsByCategoryDataSource` recuperar seus dados, controle de `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método. Como esse relatório mestre/detalhes é somente leitura, escolha a que opção nas guias de INSERT, UPDATE e DELETE (nenhum).


[![Selecione o método GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Figura 7**: selecione o `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


Depois de clicar em Avançar, o assistente ObjectDataSource nos solicita a origem do valor para o `GetProductsByCategoryID(categoryID)` do método *`categoryID`* parâmetro. Para usar o valor de selecionado `categories` DropDownList item define a origem do parâmetro ControlID para e de controle `Categories`.


[![Defina o parâmetro categoryID como o valor de Categories DropDownList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Figura 8**: defina o *`categoryID`* parâmetro para o valor da `Categories` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


Após a conclusão do Assistente Configurar fonte de dados, o Visual Studio gerará automaticamente um `ItemTemplate` para DataList que exibe o nome e o valor de cada campo de dados. Vamos melhorar DataList em vez disso, usar um `ItemTemplate` que exibe apenas o nome do produto, categoria, fornecedor, quantidade por unidade e preço junto com um `SeparatorTemplate` que injeta um `<hr>` elemento entre cada item. Vou usar o `ItemTemplate` de um exemplo na [exibindo dados com os controles DataList e Repeater](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) tutorial, mas fique à vontade para usar qualquer marcação de modelo que você encontre visualmente mais atraente.

Depois de fazer essas alterações, DataList e marcação do seu ObjectDataSource devem ser semelhantes ao seguinte:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Reserve um tempo para fazer check-out de nosso progresso em um navegador. Quando o primeiro visitando a página, os produtos que pertencem à categoria selecionada (Bebidas) são exibidos (conforme mostrado na Figura 9), mas alterar DropDownList não atualiza os dados. Isso ocorre porque um postback deve ocorrer para DataList atualizar. Para fazer isso, pode definir a DropDownList `AutoPostBack` propriedade para `true` ou adicionar um controle da Web de botão para a página. Para este tutorial, optamos por para definir a DropDownList `AutoPostBack` propriedade para `true`.

As figuras 9 e 10 ilustram o relatório mestre/detalhes em ação.


[![Quando o primeiro visitando a página, os produtos de bebidas são exibidos](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Figura 9**: quando o primeiro visitando a página, os produtos de bebidas são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![Selecionar um novo produto (produzir) automaticamente faz com que um PostBack, atualizando DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Figura 10**: seleção de um novo produto (produzir) automaticamente faz com que um PostBack, atualizando DataList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Adicionando um Item de lista "-- Escolha uma categoria –"

Ao visitar primeiro o `FilterByDropDownList.aspx` página categorias primeiro item da lista do DropDownList (Bebidas) é selecionado por padrão, mostrando os produtos de bebidas no DataList. No *filtragem de mestre/detalhes com uma DropDownList* tutorial que adicionamos uma opção "-- Escolha uma categoria –" DropDownList que foi selecionada por padrão e, quando selecionado, exibida *todos os* da produtos no banco de dados. Essa abordagem foi gerenciável ao listar os produtos em um GridView, pois cada linha de produto ocupou uma pequena quantidade de espaço na tela. No entanto, com o DataList e informações de cada produto consome uma parte maior da tela. Ainda vamos adicionar uma opção "-- Escolha uma categoria –" e que ele seja selecionada por padrão, mas em vez de precisá-lo Mostrar todos os produtos quando selecionado, vamos configurá-lo para que ele mostre nenhum produto.

Para adicionar um novo item de lista a DropDownList, vá para a janela Propriedades e clique nas elipses no `Items` propriedade. Adicionar um novo item de lista com o `Text` "-- Escolha uma categoria –" e o `Value` `0`.


![Adicionar um](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Figura 11**: adicionar um Item de lista "-- Escolha uma categoria –"


Como alternativa, você pode adicionar o item de lista, adicionando a seguinte marcação ao DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Além disso, precisamos definir o controle de DropDownList `AppendDataBoundItems` à `true` porque se ele for definido como `false` (o padrão), quando as categorias são associadas ao DropDownList do ObjectDataSource elas vão substituir qualquer lista adicionados manualmente itens.


![Defina a propriedade AppendDataBoundItems como True](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Figura 12**: defina o `AppendDataBoundItems` propriedade como True


O motivo pelo qual escolhemos o valor `0` para obter a lista de "-- Escolha uma categoria –" item é porque não há nenhuma categoria no sistema com um valor de `0`, portanto, não há registros de produto serão retornados quando o item de lista "-- Escolha uma categoria –" está selecionado. Para confirmar isso, reserve um tempo para visitar a página por meio de um navegador. Como mostra a Figura 13, quando inicialmente exibindo a página que o item de lista "-- Escolha uma categoria –" está selecionado e nenhum produto é exibido.


[![Quando o](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Figura 13**: quando o Item de lista "-- Escolha uma categoria –" é selecionado, os produtos não são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


Se, em vez disso, exibiria *todos os* dos produtos quando a opção "-- Escolha uma categoria –" é selecionada, use um valor de `-1` em vez disso. O leitor astuto deve se lembrar que back na *filtragem de mestre/detalhes com uma DropDownList* tutorial, atualizamos o `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método para que se um *`categoryID`* valor de `-1` foi passado no produto de todos os registros foram retornados.

## <a name="summary"></a>Resumo

Ao exibir dados relacionados hierarquicamente, isso geralmente ajuda a apresentar os dados usando os relatórios mestre/detalhes, do qual o usuário pode começar a ler os dados da parte superior da hierarquia e fazer drill down nos detalhes. Neste tutorial, examinamos a criação de um relatório simples mestre/detalhes mostrando produtos de uma determinada categoria. Isso foi realizado usando DropDownList para a lista de categorias e uma DataList para os produtos que pertencem à categoria selecionada.

O próximo tutorial, examinaremos a separar os registros mestre e de detalhes em duas páginas. Na primeira página, uma lista de registros "mestres" será exibida, com um link para exibir os detalhes. Clicar no link, o usuário para a segunda página, que exibe os detalhes para o registro mestre selecionado será whisk.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Randy Schmidt. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](master-detail-filtering-acess-two-pages-datalist-cs.md)
