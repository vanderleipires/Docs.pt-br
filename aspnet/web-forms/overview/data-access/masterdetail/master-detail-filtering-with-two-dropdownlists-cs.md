---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: Filtragem com dois DropDownLists (c#) mestre/detalhes | Microsoft Docs
author: rick-anderson
description: "Este tutorial expande a relação mestre/detalhes para adicionar uma terceira camada, usando dois controles DropDownList para selecionar o recor pai e avô desejado..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: c3898158f251daf0fac899fe7c18ac03322114b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-with-two-dropdownlists-c"></a>Mestre/detalhes filtragem com dois DropDownLists (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe) ou [baixar PDF](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> Este tutorial se expande a relação mestre/detalhes para adicionar uma terceira camada, usando dois controles DropDownList para selecionar os registros pai e avô desejados.


## <a name="introduction"></a>Introdução

No [tutorial anterior](master-detail-filtering-with-a-dropdownlist-cs.md) examinamos como exibir um relatório de detalhes/mestre simples usando um único DropDownList preenchido com as categorias e GridView mostrando os produtos que pertencem à categoria selecionada. Esse padrão de relatório funciona bem quando exibir os registros que têm uma relação um-para-muitos e podem ser facilmente estendidos para cenários que incluem várias relações um-para-muitos. Por exemplo, um sistema de entrada de ordem teria tabelas que correspondem aos clientes, pedidos e itens de linha. Um determinado cliente pode ter várias ordens de cada pedido consiste em vários itens. Esses dados podem ser apresentados ao usuário com dois DropDownLists e GridView. A primeira DropDownList teria um item de lista para cada cliente no banco de dados com o segundo um conteúdo que está sendo os pedidos feitos pelo cliente selecionado. Lista os itens de linha da ordem de selecionado um GridView.

Enquanto o banco de dados Northwind inclui as informações de detalhes de pedido/cliente/ordem canônica no seu `Customers`, `Orders`, e `Order Details` tabelas, essas tabelas não são capturadas em nossa arquitetura. No entanto, ainda estamos pode ilustrar usando dois DropDownLists dependentes. A primeira DropDownList listará as categorias e o segundo os produtos que pertencem à categoria selecionada. Um DetailsView listará, em seguida, os detalhes do produto selecionado.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Etapa 1: Criando e populando DropDownList categorias

Nosso objetivo primeiro é adicionar DropDownList que lista as categorias. Essas etapas foram examinadas em detalhes no tutorial anterior, mas são resumidas aqui para fins de integridade.

Abra o `MasterDetailsDetails.aspx` página o `Filtering` pasta, adicionar DropDownList para a página, defina seu `ID` propriedade `Categories`e, em seguida, clique no link de configurar fonte de dados em sua marca inteligente. O Assistente de configuração de fonte de dados optar por adicionar uma nova fonte de dados.


[![Adicionar uma nova fonte de dados para DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**Figura 1**: adicionar uma nova fonte de dados para DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))


A nova fonte de dados, naturalmente, haverá um ObjectDataSource. Nomeie esse novo ObjectDataSource `CategoriesDataSource` e invocar o `CategoriesBLL` do objeto `GetCategories()` método.


[![Optar por usar a classe CategoriesBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**Figura 2**: escolher usar o `CategoriesBLL` classe ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))


[![Configurar o ObjectDataSource para usar o método GetCategories()](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**Figura 3**: configurar o ObjectDataSource para usar o `GetCategories()` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))


Depois de configurar o ObjectDataSource ainda precisamos especificar qual campo de fonte de dados deve ser exibido no `Categories` DropDownList e qual deve ser configurado como o valor do item de lista. Definir o `CategoryName` campo como a exibição e `CategoryID` como o valor para cada item de lista.


[![Ter a exibição DropDownList CategoryName campo e Use CategoryID como o valor](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**Figura 4**: têm a exibição DropDownList a `CategoryName` campo e Use `CategoryID` como o valor ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))


Neste ponto, temos um controle DropDownList (`Categories`) que é preenchida com os registros da `Categories` tabela. Quando o usuário escolhe uma nova categoria na lista suspensa, será necessário um postback para ocorrer para atualizar o produto DropDownList, vamos criar na etapa 2. Portanto, verifique se a opção Enable AutoPostBack do `categories` marca inteligente do DropDownList.


[![Habilitar AutoPostBack para DropDownList categorias](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**Figura 5**: Enable AutoPostBack para o `Categories` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Etapa 2: Exibir produtos da categoria selecionada em DropDownList segundo

Com o `Categories` DropDownList concluída, nossa próxima etapa é exibir DropDownList dos produtos que pertencem à categoria selecionada. Para fazer isso, adicione outro DropDownList para a página chamada `ProductsByCategory`. Assim como acontece com o `Categories` DropDownList, criar um novo ObjectDataSource para o `ProductsByCategory` DropDownList denominado `ProductsByCategoryDataSource`.


[![Adicionar uma nova fonte de dados para ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**Figura 6**: adicionar uma nova fonte de dados para o `ProductsByCategory` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))


[![Criar um novo ObjectDataSource denominado ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**Figura 7**: criar um novo ObjectDataSource nomeado `ProductsByCategoryDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))


Como o `ProductsByCategory` DropDownList precisa exibir apenas os produtos que pertencem à categoria selecionada, ter ObjectDataSource invocar o `GetProductsByCategoryID(categoryID)` método do `ProductsBLL` objeto.


[![Optar por usar a classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**Figura 8**: escolher usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))


[![Configurar o ObjectDataSource para usar o método GetProductsByCategoryID(categoryID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**Figura 9**: configurar o ObjectDataSource para usar o `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))


Na etapa final do assistente, é preciso especificar o valor da  *`categoryID`*  parâmetro. Atribuir este parâmetro para o item selecionado do `Categories` DropDownList.


[![Extrair o valor do parâmetro categoryID na lista suspensa de categorias](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**Figura 10**: Puxe a  *`categoryID`*  valor do parâmetro de `Categories` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))


Com o ObjectDataSource configurado, tudo o que permanece é para especificar quais campos de fonte de dados são usados para a exibição e o valor dos itens do DropDownList. Exibição de `ProductName` campo e use o `ProductID` campo como o valor.


[![Especifique os campos de fonte de dados usados para o texto dos ListItems do DropDownList e as propriedades de valor](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**Figura 11**: especifique os campos de fonte de dados usada para a DropDownList `ListItem` s' `Text` e `Value` propriedades ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))


Com o ObjectDataSource e `ProductsByCategory` DropDownList configurado nossa página exibirá dois DropDownLists: a primeira lista todas as categorias enquanto a segunda lista os produtos que pertencem à categoria selecionada. Quando o usuário seleciona uma nova categoria na lista suspensa primeiro, ocorrerá um postback e DropDownList segundo será religado, mostrando os produtos que pertencem à categoria selecionada recentemente. 12 de figuras e mostrar 13 `MasterDetailsDetails.aspx` em ação quando visualizada através de um navegador.


[![Quando o primeiro visitando a página, a categoria de bebidas está selecionada](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**Figura 12**: ao primeiro visitar a página, a categoria de bebidas é selecionada ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))


[![Escolher uma categoria diferente exibe produtos a nova categoria](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**Figura 13**: escolhendo um exibe categorias diferentes de produtos da nova categoria ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))


Atualmente o `productsByCategory` DropDownList, quando alteradas, *não* causar um postback. No entanto, queremos um postback para ocorrer depois que podemos adicionar um DetailsView para exibir os detalhes do produto selecionado (etapa 3). Portanto, verifique se a caixa de seleção Habilitar AutoPostBack o `productsByCategory` marca inteligente do DropDownList.


[![Habilitar o recurso de AutoPostBack para o productsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**Figura 14**: habilitar o recurso AutoPostBack para o `productsByCategory` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Etapa 3: Usando um DetailsView para exibir detalhes do produto selecionado

A etapa final é exibir os detalhes do produto selecionado em um DetailsView. Para fazer isso, adicione um DetailsView à página, defina seu `ID` propriedade `ProductDetails`e criar um novo ObjectDataSource para ele. Configurar este ObjectDataSource para efetuar o pull de seus dados a partir o `ProductsBLL` da classe `GetProductByProductID(productID)` método usando o valor selecionado o `ProductsByCategory` DropDownList para o valor da  *`productID`*  parâmetro.


[![Optar por usar a classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**Figura 15**: escolher usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))


[![Configurar o ObjectDataSource para usar o método GetProductByProductID(productID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**Figura 16**: configurar o ObjectDataSource para usar o `GetProductByProductID(productID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))


[![Extrair o valor do parâmetro productID de ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**Figura 17**: Puxe a  *`productID`*  valor do parâmetro de `ProductsByCategory` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))


Você pode optar por exibir os campos disponíveis em DetailsView. Você optou por remover o `ProductID`, `SupplierID`, e `CategoryID` campos e reordenadas e formatados os campos restantes. Além disso, eu limpo de DetailsView `Height` e `Width` propriedades, permitindo que o DetailsView expandir a largura necessária para melhor exibição seus dados em vez de fazer com que ele restrito a um tamanho especificado. A marcação completa é exibida abaixo:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

Reserve um tempo para testar o `MasterDetailsDetails.aspx` página em um navegador. À primeira vista pode parecer que tudo está funcionando conforme o desejado mas problemas sutis. Quando você escolhe uma nova categoria de `ProductsByCategory` DropDownList é atualizada para incluir os produtos para a categoria selecionada, mas o `ProductDetails` DetailsView continuação mostrar as informações anteriores do produto. O DetailsView é atualizado ao escolher um produto diferente para a categoria selecionada. Além disso, se você testar completamente o suficiente, você verá que se você escolher continuamente novas categorias (como escolher Bebidas do `Categories` DropDownList e, em seguida, Condimentos, em seguida, doces) cada outra seleção de categoria faz com que o `ProductDetails`DetailsView a ser atualizado.

Para ajudar a concretizar a esse problema, vamos examinar um exemplo específico. Quando visitar a página de categoria de bebidas está selecionada e os produtos relacionados são carregados no `ProductsByCategory` DropDownList. Chai é o produto selecionado e seus detalhes são exibidos no `ProductDetails` DetailsView, conforme mostrado na Figura 18.


[![Detalhes do produto selecionado são exibidos em um DetailsView](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**Figura 18**: detalhes do produto selecionado o são exibidas em um DetailsView ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))


Se você alterar a seleção de categoria de bebidas para Condimentos, ocorre um postback e `ProductsByCategory` DropDownList é atualizado adequadamente, mas o DetailsView ainda exibe detalhes de Chai.


[![Detalhes de anteriormente selecionada do produto são exibidas ainda](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**Figura 19**: detalhes do produto selecionada anteriormente o são exibidas ainda ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))


Um novo produto da lista de separação atualiza DetailsView conforme o esperado. Se você escolher uma nova categoria depois de alterar o produto, DetailsView novamente não atualizar. No entanto, se em vez de escolher um novo produto, você selecionou uma nova categoria, seria atualizar DetailsView. O que o mundo está acontecendo aqui?

O problema é um problema de sincronização no ciclo de vida da página. Sempre que uma página é solicitada a que ele passa um número de etapas de como sua renderização. Em uma das seguintes etapas ObjectDataSource controla a verificação para ver se qualquer um dos seus `SelectParameters` valores foram alterados. Se assim, o controle da Web de dados associados a ObjectDataSource sabe que precisa atualizar sua exibição. Por exemplo, quando uma nova categoria é marcada, o `ProductsByCategoryDataSource` ObjectDataSource detecta que seus valores de parâmetro foram alteradas e o `ProductsByCategory` DropDownList reconecta, obtenção de produtos para a categoria selecionada.

O problema que surge nessa situação é que o ponto do ciclo de vida de página que verificam as ObjectDataSources parâmetros alterados ocorre *antes de* a reassociação dos controles da Web de dados associados. Portanto, ao selecionar uma nova categoria de `ProductsByCategoryDataSource` ObjectDataSource detecta uma alteração no valor do seu parâmetro. Usado por ObjectDataSource o `ProductDetails` DetailsView, no entanto, não observe tais alterações porque o `ProductsByCategory` DropDownList ainda precisa ser vinculada outra vez. Posteriormente no ciclo de vida de `ProductsByCategory` DropDownList associa novamente ao seu ObjectDataSource, captura os produtos para a categoria selecionada recentemente. Enquanto o `ProductsByCategory` valor do DropDownList foi alterado, o `ProductDetails` ObjectDataSource de DetailsView já realizou sua verificação de valor do parâmetro; portanto, DetailsView exibe seus resultados anteriores. Essa interação é representada na Figura 20.


[![O valor de ProductsByCategory DropDownList alterado depois ObjectDataSource de ProductDetails DetailsView procura as alterações](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**Figura 20**: O `ProductsByCategory` DropDownList valor alterações após a `ProductDetails` ObjectDataSource procura de DetailsView as alterações ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))


Para corrigir isso, precisamos explicitamente reassociar o `ProductDetails` DetailsView após o `ProductsByCategory` DropDownList foi vinculado. Podemos pode fazer isso chamando o `ProductDetails` de DetailsView `DataBind()` método quando o `ProductsByCategory` do DropDownList `DataBound` evento ser acionado. Adicione o seguinte código de manipulador de eventos para o `MasterDetailsDetails.aspx` classe de code-behind da página (consulte o "[definir programaticamente o valores do parâmetro ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" para obter mais informações sobre como adicionar um manipulador de eventos):


[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

Após essa chamada explícita para o `ProductDetails` de DetailsView `DataBind()` método foi adicionado, o tutorial funciona conforme o esperado. Figura 21 destaca como isso alterado corrigido nosso problema anterior.


[![O ProductDetails DetailsView é ligação de dados evento ser acionado da explicitamente atualizada quando o ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**Figura 21**: O `ProductDetails` DetailsView é explicitamente atualizada quando o `ProductsByCategory` do DropDownList `DataBound` evento ser acionado ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))


## <a name="summary"></a>Resumo

DropDownList serve como um elemento de interface do usuário ideal para relatórios mestre/detalhes onde há uma relação um-para-muitos entre os registros mestre e de detalhes. No tutorial anterior, vimos como usar um único DropDownList para filtrar os produtos exibidos por categoria selecionada. Neste tutorial substituído GridView de produtos com DropDownList e usado um DetailsView para exibir os detalhes do produto selecionado. Os conceitos abordados neste tutorial facilmente podem ser estendidos para modelos de dados que envolvem várias relações um-para-muitos, como clientes, pedidos e itens do pedido. Em geral, você sempre pode adicionar DropDownList para cada uma das entidades "um" nas relações um-para-muitos.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Giesenow Hilton. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](master-detail-filtering-with-a-dropdownlist-cs.md)
[Próximo](master-detail-filtering-across-two-pages-cs.md)
