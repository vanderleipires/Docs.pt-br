---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Filtragem com DropDownList (c#) mestre/detalhes | Microsoft Docs
author: rick-anderson
description: Neste tutorial, podemos ver como exibir relatórios mestre/detalhes em uma única página da web usando DropDownLists para exibir os registros de 'master' e uma DataList para ex...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c84902ccf028c976246380abfaebb6a76c573603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880668"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Mestre/detalhes filtragem com DropDownList (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) ou [baixar PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> Neste tutorial, podemos ver como exibir relatórios mestre/detalhes em uma única página da web usando DropDownLists para exibir os registros de "mestres" e uma DataList para exibir os "Detalhes".


## <a name="introduction"></a>Introdução

O relatório de detalhes/mestre, que são criados pela primeira vez usando um controle GridView no anteriores [filtragem de mestre/detalhes com um DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial, começa mostrando um conjunto de registros "mestres". O usuário pode, em seguida, fazer drill down em um dos registros mestres, assim, detalhes do registro mestre "." Relatórios de detalhes/mestre são uma opção ideal para visualizar relações um-para-muitos e para exibir informações detalhadas de tabelas particularmente "grande" (aqueles que têm muitas colunas). Podemos depois de explorar como implementar relatórios mestre/detalhes usando os controles GridView e DetailsView nos tutoriais anteriores. Este tutorial e as próximas duas, podemos será reexaminar esses conceitos, mas o foco no uso de DataList repetidor controles e em vez disso.

Neste tutorial, vamos examinar usando DropDownList para conter os registros "mestres", com os registros de "Detalhes" exibidos em DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Etapa 1: Adicionar a páginas da Web Tutorial mestre/detalhes

Antes de começar este tutorial, primeiro vamos adicionar a pasta e páginas ASP.NET que vamos precisar para este tutorial e as próximas duas lidar com relatórios mestre/detalhes usando os controles DataList e Repetidor. Comece criando uma nova pasta no projeto chamado `DataListRepeaterFiltering`. Em seguida, adicione as seguintes páginas ASP.NET cinco nesta pasta, ter todos os configurado para usar a página mestra `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Crie uma pasta DataListRepeaterFiltering e adicionar as páginas ASP.NET Tutorial](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Figura 1**: criar um `DataListRepeaterFiltering` pasta e adicionar as páginas ASP.NET Tutorial


Em seguida, abra o `Default.aspx` página e arraste o `SectionLevelTutorialListing.ascx` controle de usuário da `UserControls` pasta para a superfície de Design. Este controle de usuário que criamos no [páginas mestras e navegação de Site](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumera o mapa do site e exibe os tutoriais na seção atual em uma lista com marcadores.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Figura 2**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


Para ter a exibição de lista com marcadores os tutoriais de detalhes/mestre que é criado, precisamos adicioná-los ao mapa do site. Abra o `Web.sitemap` de arquivo e adicione a seguinte marcação após a marcação de nó de mapa de site "Exibindo dados com o DataList e repetidor":

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![Atualizar o mapa de Site para incluir as novas páginas ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Figura 3**: atualizar o mapa de Site para incluir as novas páginas ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Etapa 2: Exibindo as categorias em DropDownList

Nosso relatório mestre/detalhes listará as categorias de DropDownList, com produtos do item de lista selecionado exibidos mais abaixo na página em DataList. A primeira tarefa à frente de nós, em seguida, é ter as categorias exibidas no DropDownList. Comece abrindo o `FilterByDropDownList.aspx` página o `DataListRepeaterFiltering` pasta e arraste DropDownList da caixa de ferramentas para o designer da página. Em seguida, defina a DropDownList `ID` propriedade `Categories`. Clique no link de marca inteligente do DropDownList Escolher fonte de dados e criar um novo ObjectDataSource denominado `CategoriesDataSource`.


[![Adicionar um novo ObjectDataSource denominado CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Figura 4**: adicionar um novo ObjectDataSource nomeado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


Configurar o novo ObjectDataSource, de modo que ele chama o `CategoriesBLL` da classe `GetCategories()` método. Depois de configurar o ObjectDataSource ainda é necessário especificar o campo de fonte de dados deve ser exibido na DropDownList e que um deve ser associado como o valor para cada item da lista. Ter o `CategoryName` campo como a exibição e `CategoryID` como o valor para cada item de lista.


[![Ter a exibição DropDownList CategoryName campo e Use CategoryID como o valor](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Figura 5**: têm a exibição DropDownList a `CategoryName` campo e Use `CategoryID` como o valor ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


Neste ponto, temos um controle DropDownList que é preenchido com os registros da `Categories` tabela (todos realizada em cerca de cinco segundos). A Figura 6 mostra nosso progresso até o momento quando visualizada através de um navegador.


[![Uma lista suspensa lista as categorias atuais](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Figura 6**: A lista suspensa lista as categorias atual ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Etapa 2: Adicionando DataList produtos

A última etapa do nosso relatório mestre/detalhes é listar os produtos associados a categoria selecionada. Para fazer isso, adicione uma DataList para a página e criar um novo ObjectDataSource denominado `ProductsByCategoryDataSource`. Ter o `ProductsByCategoryDataSource` controle recuperar seus dados a partir de `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método. Como esse relatório de detalhes/mestre é somente leitura, escolha a que opção nas guias de INSERT, UPDATE e DELETE (nenhum).


[![Selecione o método GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Figura 7**: selecione o `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


Depois de clicar em Avançar, o assistente ObjectDataSource solicita a nós para a fonte do valor para o `GetProductsByCategoryID(categoryID)` do método *`categoryID`* parâmetro. Para usar o valor do selecionado `categories` item DropDownList define a origem de parâmetro de controle e ControlID para `Categories`.


[![Defina o parâmetro categoryID com o valor de DropDownList categorias](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Figura 8**: definir o *`categoryID`* parâmetro para o valor da `Categories` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


Após concluir o Assistente Configurar fonte de dados, o Visual Studio gerará automaticamente um `ItemTemplate` para DataList que exibe o nome e valor de cada campo de dados. Vamos melhorar DataList para usar em vez disso, um `ItemTemplate` que exibe apenas o nome do produto, categoria, fornecedor, quantidade por unidade e preço junto com um `SeparatorTemplate` que insere um `<hr>` elemento entre cada item. Vou usar o `ItemTemplate` de exemplo de [exibindo dados com os controles de Repetidor e DataList](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) tutorial, mas fique à vontade para usar qualquer marcação de modelo encontrar mais atraentes.

Depois de fazer essas alterações, DataList e marcação do seu ObjectDataSource devem ser semelhantes ao seguinte:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Responda fazer check-out nosso progresso em um navegador. Quando o primeiro visitando a página, os produtos que pertencem à categoria selecionada (Bebidas) são exibidos (conforme mostrado na Figura 9), mas alterar DropDownList não atualiza os dados. Isso ocorre porque um postback deve ocorrer para o DataList atualizar. Para fazer isso, também pode definir a DropDownList `AutoPostBack` propriedade `true` ou adicionar um controle de botão Web para a página. Para este tutorial, eu tenha optado por definir a DropDownList `AutoPostBack` propriedade `true`.

Figuras 9 e 10 ilustram o relatório de detalhes/mestre em ação.


[![Quando o primeiro visitando a página, os produtos de bebidas são exibidos](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Figura 9**: ao primeiro visitar a página, os produtos de bebidas são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![Selecionar um novo produto (produção) automaticamente faz com que um PostBack, atualizando DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Figura 10**: selecionar um novo produto (produção) automaticamente faz com que um PostBack, atualizando DataList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Adicionar um Item de lista "-- Escolha uma categoria –"

Ao visitar primeiro o `FilterByDropDownList.aspx` página as categorias primeiro item da lista do DropDownList (Bebidas) é selecionado por padrão, mostrando os produtos de bebidas em DataList. No *filtragem de mestre/detalhes com um DropDownList* tutorial adicionamos uma opção "-- Escolha uma categoria –" para DropDownList que foi selecionada por padrão e, quando selecionado, exibida *todos os* da produtos no banco de dados. Essa abordagem estava gerenciável ao listar os produtos em um GridView, como cada linha de produto levou a uma pequena quantidade de espaço na tela. No entanto, com DataList, informações de cada produto consomem uma quantidade maior parte da tela. Ainda vamos adicionar uma opção "-- Escolha uma categoria –" e que ele seja selecionada por padrão, mas em vez de ter-Mostrar todos os produtos quando selecionada, vamos configurá-lo para que não mostre nenhum produto.

Para adicionar um novo item de lista a DropDownList, vá para a janela de propriedades e clique nas elipses no `Items` propriedade. Adicionar um novo item de lista com o `Text` "-- Escolha uma categoria –" e o `Value` `0`.


![Adicionar um](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Figura 11**: adicionar um Item de lista "-- Escolha uma categoria –"


Como alternativa, você pode adicionar o item de lista, adicionando a seguinte marcação para DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Além disso, é preciso definir o controle de DropDownList `AppendDataBoundItems` para `true` porque se ele for definido como `false` (o padrão), quando as categorias são associadas a DropDownList de ObjectDataSource elas vão substituir qualquer lista adicionados manualmente itens.


![Defina a propriedade AppendDataBoundItems como True](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Figura 12**: definir o `AppendDataBoundItems` propriedade como True


O motivo que escolhemos o valor `0` para obter a lista de "-- Escolha uma categoria –" item é porque não há nenhuma categoria no sistema com um valor de `0`, portanto, nenhum registro de produto será retornado quando o item de lista "-- Escolha uma categoria –" está selecionado. Para confirmar isso, responda para visitar a página por meio de um navegador. Como mostrado na Figura 13, quando inicialmente exibindo a página do item de lista "-- Escolha uma categoria –" estiver selecionado e nenhum produto é exibido.


[![Quando o](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Figura 13**: quando o Item de lista "-- Escolha uma categoria –" é selecionado, os produtos não são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


Se, em vez disso, você exibirá *todos os* dos produtos quando a opção "-- Escolha uma categoria –" estiver selecionada, use um valor de `-1` em vez disso. O um leitor deve se lembrar que novamente o *filtragem de mestre/detalhes com um DropDownList* tutorial atualizamos o `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método para que se um *`categoryID`* valor de `-1` passou em todos os produtos de registros foi retornado.

## <a name="summary"></a>Resumo

Ao exibir dados relacionados hierarquicamente, isso geralmente ajuda a apresentar os dados usando os relatórios de detalhes/mestre, do qual o usuário pode começar a examinar os dados da parte superior da hierarquia e fazer drill down nos detalhes. Neste tutorial examinamos a criação de um relatório simples mestre/detalhes mostrando produtos de uma determinada categoria. Isso foi realizado usando DropDownList para a lista de categorias e DataList para os produtos que pertencem à categoria selecionada.

O seguinte tutorial, examinaremos separando os registros mestre e de detalhes em duas páginas. Na primeira página, uma lista de registros "mestres" aparecerá, com um link para exibir os detalhes. Clicar no link, o usuário para a segunda página, que exibe os detalhes do registro mestre selecionado será whisk.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Randy Schmidt. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](master-detail-filtering-acess-two-pages-datalist-cs.md)
