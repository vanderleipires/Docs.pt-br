---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
title: "Mestre/detalhes usando um GridView mestre selecionável com um Details DetailView (c#) | Microsoft Docs"
author: rick-anderson
description: "Este tutorial terá um GridView cujas linhas incluem o nome e o preço de cada produto junto com um botão de seleção. Clicar no botão Selecionar para uma particu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0f982827-f8f9-420d-b36b-57b23f5aa519
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5f0d380ee411116844f42a542c12050513721eb1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-c"></a>Mestre/detalhes usando um GridView mestre selecionável com um Details DetailView (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_10_CS.exe) ou [baixar PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/datatutorial10cs1.pdf)

> Este tutorial terá um GridView cujas linhas incluem o nome e o preço de cada produto junto com um botão de seleção. Clicar no botão Selecionar para um determinado produto fará com que seus detalhes completos para ser exibido em um controle DetailsView na mesma página.


## <a name="introduction"></a>Introdução

No [tutorial anterior](master-detail-filtering-across-two-pages-cs.md) vimos como criar um relatório de detalhes/mestre usando duas páginas da web: uma página da web "mestre", do qual é exibida a lista de fornecedores; e uma página da web "Detalhes" que lista os produtos fornecidos por selecionado fornecedor. Esse formato de relatório de duas páginas pode ser condensado em uma página. Este tutorial terá um GridView cujas linhas incluem o nome e o preço de cada produto junto com um botão de seleção. Clicar no botão Selecionar para um determinado produto fará com que seus detalhes completos para ser exibido em um controle DetailsView na mesma página.


[![Clique no botão de seleção exibe detalhes do produto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image1.png)

**Figura 1**: clicar no botão de seleção exibe detalhes do produto ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Etapa 1: Criando um controle GridView selecionável

Lembre-se que o mestre/detalhes duas páginas de relatório que cada registro mestre incluído um hyperlink que, quando clicado, enviado ao usuário para a página de detalhes, passando a linha clicada `SupplierID` valor na querystring. Esse tipo de hiperlink foi adicionado a cada linha GridView usando um HyperLinkField. Para o relatório de detalhes/mestre única página, será necessário um botão para cada GridView linha que, quando clicado, mostra os detalhes. O controle GridView pode ser configurado para incluir um botão de seleção para cada linha que causa um postback e marca a linha como o GridView [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Comece adicionando um controle GridView para o `DetailsBySelecting.aspx` página o `Filtering` pasta, definindo seu `ID` propriedade `ProductsGrid`. Em seguida, adicione um novo ObjectDataSource denominado `AllProductsDataSource` que invoca o `ProductsBLL` da classe `GetProducts()` método.


[![Criar um ObjectDataSource denominado AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image4.png)

**Figura 2**: criar um ObjectDataSource nomeado `AllProductsDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image6.png))


[![Use a classe ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image7.png)

**Figura 3**: Use o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image9.png))


[![Configurar o ObjectDataSource para invocar o método GetProducts()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image10.png)

**Figura 4**: configurar o ObjectDataSource para chamar o `GetProducts()` método ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image12.png))


Editar os campos do GridView remover tudo, exceto o `ProductName` e `UnitPrice` BoundFields. Além disso, fique à vontade para personalizar esses BoundFields conforme necessário, como a formatação a `UnitPrice` BoundField como uma moeda e alterando o `HeaderText` propriedades da BoundFields. Essas etapas podem ser realizadas graficamente, clicando no link Editar colunas de marcas inteligentes do GridView, ou configurando manualmente a sintaxe declarativa.


[![Remova todos, exceto o ProductName e UnitPrice BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image13.png)

**Figura 5**: Remover tudo, exceto o `ProductName` e `UnitPrice` BoundFields ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image15.png))


A marcação final do GridView é:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample1.aspx)]

Em seguida, é preciso marcar GridView como selecionável, que adicionará um botão de seleção para cada linha. Para fazer isso, basta marcar a caixa de seleção Habilitar seleção de marcas inteligentes do GridView.


[![Verifique as linhas do GridView selecionável](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image16.png)

**Figura 6**: tornar linhas selecionável o GridView ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image18.png))


A opção Habilitar seleção de verificação adiciona uma CommandField para o `ProductsGrid` GridView com seu `ShowSelectButton` propriedade definida como True. Isso resulta em um botão de seleção para cada linha de GridView, conforme ilustra a Figura 6. Por padrão, os botões de Select são renderizados como botões de link, mas você pode usar botões ou ImageButtons em vez disso, por meio de CommandField `ButtonType` propriedade.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample2.aspx)]

Quando o botão de seleção de uma linha GridView é clicado um postback tem lugar e a GridView `SelectedRow` propriedade é atualizada. Além de `SelectedRow` propriedade GridView fornece o [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), e [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) propriedades. O `SelectedIndex` propriedade retorna o índice da linha selecionada, enquanto o `SelectedValue` e `SelectedDataKey` propriedades retornam valores com base no GridView [propriedade DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

O `DataKeyNames` propriedade é usada para associar um ou mais campos de dados com cada linha de valores e é comumente usado para informações de identificação exclusivo de dados subjacentes com cada linha GridView de atributo. O `SelectedValue` propriedade retorna o valor do primeiro `DataKeyNames` campo de dados para a linha selecionada enquanto o `SelectedDataKey` propriedade retorna a linha selecionada `DataKey` objeto, que contém todos os valores para os campos de chave de dados especificado Essa linha.

O `DataKeyNames` propriedade é definida automaticamente para os campos de dados identificando exclusivamente quando você associa uma fonte de dados a um GridView, DetailsView ou FormView por meio do Designer. Embora essa propriedade foi definida para que possamos automaticamente nos tutoriais anteriores, os exemplos teria funcionado sem o `DataKeyNames` propriedade especificada. No entanto, para o GridView selecionável neste tutorial, bem como para tutoriais futuros na qual podemos vai ser examinando inserindo, atualizando e excluindo, o `DataKeyNames` propriedade deve ser definida corretamente. Reserve um tempo para garantir que o GridView `DataKeyNames` está definida como `ProductID`.

Vamos ver nosso progresso até o momento através de um navegador. Observe que o GridView lista o nome e o preço de todos os produtos juntamente com um LinkButton selecione. Clique no botão de seleção faz com que um postback. Na etapa 2, veremos como ter um DetailsView responder esta postagem, exibindo os detalhes para o produto selecionado.


[![Cada linha de produto contém um LinkButton Select](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image19.png)

**Figura 7**: cada linha de produto contém um LinkButton selecione ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image21.png))


### <a name="highlighting-the-selected-row"></a>Realce a linha selecionada

O `ProductsGrid` GridView tem um `SelectedRowStyle` propriedade que pode ser usada para definir o estilo visual para a linha selecionada. Usada corretamente, isso pode melhorar a experiência do usuário mais claramente mostrando que linha em GridView está selecionada no momento. Para este tutorial, vamos dar a linha selecionada realçado com um plano de fundo amarelo.

Assim como acontece com nossos tutoriais anteriores, vamos tentar manter as configurações relacionadas a estética definidas como classes CSS. Portanto, crie uma nova classe CSS em `Styles.css` chamado `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample3.css)]

Para aplicar essa classe CSS para o `SelectedRowStyle` propriedade de *todos os* GridViews em nossa série de tutoriais, editar o `GridView.skin` da aparência no `DataWebControls` tema para incluir o `SelectedRowStyle` configurações conforme mostrado abaixo:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample4.aspx)]

Com essa adição, a linha selecionada de GridView agora é realçada com uma cor de plano de fundo amarelo.


[![Personalizar a aparência da linha selecionada usando a propriedade de SelectedRowStyle do GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image22.png)

**Figura 8**: personalizar o uso de aparência da linha selecionada de GridView `SelectedRowStyle` propriedade ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Etapa 2: Exibir detalhes do produto selecionado em um DetailsView

Com o `ProductsGrid` concluir do GridView, tudo o que resta é adicionar um DetailsView que exibe informações sobre o produto específico selecionado. Adicionar um controle DetailsView acima GridView e criar um novo ObjectDataSource denominado `ProductDetailsDataSource`. Como queremos que este DetailsView para exibir informações específicas sobre o produto selecionado, configure o `ProductDetailsDataSource` para usar o `ProductsBLL` da classe `GetProductByProductID(productID)` método.


[![Chamar o ProductsBLL método da classe GetProductByProductID(productID)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image25.png)

**Figura 9**: chamar o `ProductsBLL` da classe `GetProductByProductID(productID)` método ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image27.png))


Ter o  *`productID`*  valor do parâmetro obtido do controle GridView `SelectedValue` propriedade. Conforme abordado anteriormente, o GridView `SelectedValue` propriedade retorna os primeiros dados de valor para a linha selecionada da chave. Portanto, é fundamental que o GridView `DataKeyNames` está definida como `ProductID`, de modo que a linha selecionada `ProductID` valor é retornado por `SelectedValue`.


[![Defina o parâmetro productID como propriedade SelectedValue do GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image28.png)

**Figura 10**: definir o  *`productID`*  parâmetro a GridView `SelectedValue` propriedade ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image30.png))


Uma vez o `productDetailsDataSource` ObjectDataSource foi configurado corretamente e associado a DetailsView, este tutorial está concluído! Quando a página for visitada primeiro nenhuma linha é selecionada, para que o GridView `SelectedValue` propriedade retorna `null`. Como não há nenhum produto com um `NULL` `ProductID` valor, nenhum registro é retornado pelo `GetProductByProductID(productID)` método, o que significa que o DetailsView não será exibida (consulte a Figura 11). Após clicar em GridView selecione botão de uma linha, um postback tem lugar e DetailsView é atualizada. Dessa vez o GridView `SelectedValue` propriedade retorna o `ProductID` da linha selecionada, o `GetProductByProductID(productID)` método retorna um `ProductsDataTable` com informações sobre um produto em particular e DetailsView mostra esses detalhes (veja a Figura 12).


[![Quando visitado primeiro, apenas o GridView é exibido](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image31.png)

**Figura 11**: quando visitado pela primeira vez, somente o GridView é exibido ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image33.png))


[![Ao selecionar uma linha, são exibidos os detalhes do produto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image34.png)

**Figura 12**: ao selecionar uma linha, os detalhes do produto são exibidas ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image36.png))


## <a name="summary"></a>Resumo

Neste e os anteriores três tutoriais que vimos várias técnicas para exibir relatórios de detalhes/mestre. Neste tutorial, examinamos usando um GridView selecionável para hospedar os registros mestres e um DetailsView para exibir detalhes sobre o registro mestre selecionado na mesma página. Os tutoriais anteriores examinamos como exibir relatórios de detalhes/mestre usando DropDownLists e exibindo registros mestres em uma página da web e registros de detalhe em outro.

Este tutorial conclui nossa análise do relatórios mestre/detalhes. Começando com o próximo tutorialwe começar nosso exploração de formatação personalizada com o GridView, DetailsView e FormView. Veremos como personalizar a aparência desses controles com base nos dados associados a eles, como resumir os dados no rodapé do GridView e como usar modelos para obter um maior grau de controle sobre o layout.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Giesenow Hilton. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](master-detail-filtering-across-two-pages-cs.md)
[Próximo](master-detail-filtering-with-a-dropdownlist-vb.md)
