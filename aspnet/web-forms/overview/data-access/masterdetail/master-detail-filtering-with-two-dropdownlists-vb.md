---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
title: Mestre/detalhes filtragem com duas DropDownLists (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial se expande a relação mestre/detalhes para adicionar uma terceira camada, usando dois controles DropDownList para selecionar o desejado superiores de pai e avô...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 11ae4f64-01ba-4823-95f4-a2fe1f84f7d7
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
msc.type: authoredcontent
ms.openlocfilehash: 20df98f1aacb046bb9ec9fa5ad03e008dc234509
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824985"
---
<a name="masterdetail-filtering-with-two-dropdownlists-vb"></a>Mestre/detalhes filtragem com duas DropDownLists (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_8_VB.exe) ou [baixar PDF](master-detail-filtering-with-two-dropdownlists-vb/_static/datatutorial08vb1.pdf)

> Este tutorial se expande a relação mestre/detalhes para adicionar uma terceira camada, usando dois controles DropDownList para selecionar os registros pai e avô desejados.


## <a name="introduction"></a>Introdução

No [tutorial anterior](master-detail-filtering-with-a-dropdownlist-vb.md) , examinamos como exibir um relatório simples mestre/detalhes usando uma única DropDownList preenchida com as categorias e GridView mostrando os produtos que pertencem à categoria selecionada. Esse padrão de relatório funciona bem quando exibir os registros que têm uma relação um-para-muitos e podem ser facilmente estendidos para trabalhar para cenários que incluem várias relações um-para-muitos. Por exemplo, um sistema de entrada de pedidos teria tabelas que correspondem aos clientes, pedidos e itens de linha do pedido. Um determinado cliente pode ter várias ordens de cada pedido que consiste em vários itens. Esses dados podem ser apresentados ao usuário com duas DropDownLists e GridView. A primeira DropDownList teria um item de lista para cada cliente no banco de dados com o segundo de um conteúdo que está sendo os pedidos feitos pelo cliente selecionado. Um GridView listaria os itens de linha da ordem de selecionada.

Enquanto o banco de dados Northwind incluem as informações de detalhes do pedido/de pedido de cliente canônico no seu `Customers`, `Orders`, e `Order Details` tabelas, essas tabelas não são capturadas em nossa arquitetura. No entanto, podemos pode ilustrar ainda usando duas DropDownLists dependentes. A primeira DropDownList listará as categorias e o segundo os produtos que pertencem à categoria selecionada. Um DetailsView, em seguida, listará os detalhes do produto selecionado.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Etapa 1: Criando e preenchendo a DropDownList categorias

Nossa primeira meta é adicionar a DropDownList que lista as categorias. Essas etapas foram examinadas em detalhes no tutorial anterior, mas são resumidas aqui para fins de integridade.

Abra o `MasterDetailsDetails.aspx` página na `Filtering` pasta, adicione uma DropDownList para a página, defina seu `ID` propriedade a ser `Categories`e, em seguida, clique no link de configurar fonte de dados na sua marca inteligente. No Assistente de configuração de fonte de dados escolha Adicionar uma nova fonte de dados.


[![Adicionar uma nova fonte de dados para a DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image1.png)

**Figura 1**: adicionar uma nova fonte de dados para a DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image3.png))


Nova fonte de dados, naturalmente, haverá um ObjectDataSource. Nomeie esse novo ObjectDataSource `CategoriesDataSource` e que ele invoque o `CategoriesBLL` do objeto `GetCategories()` método.


[![Optar por usar a classe CategoriesBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image4.png)

**Figura 2**: escolha usar o `CategoriesBLL` classe ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image6.png))


[![Configurar o ObjectDataSource para usar o método GetCategories()](master-detail-filtering-with-two-dropdownlists-vb/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image7.png)

**Figura 3**: configurar o ObjectDataSource para usar o `GetCategories()` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image9.png))


Depois de configurar o ObjectDataSource ainda precisamos especificar qual campo de fonte de dados deve ser exibido no `Categories` DropDownList e qual delas deve ser configurado como o valor do item de lista. Defina as `CategoryName` campo, como a exibição e `CategoryID` como o valor para cada item de lista.


[![Ter a exibição DropDownList CategoryName campo e Use CategoryID como o valor](master-detail-filtering-with-two-dropdownlists-vb/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image10.png)

**Figura 4**: têm a exibição DropDownList a `CategoryName` campo e Use `CategoryID` como o valor ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image12.png))


Neste ponto, temos um controle DropDownList (`Categories`) que é preenchida com os registros da `Categories` tabela. Quando o usuário escolhe uma nova categoria na lista suspensa, desejaremos um postback ocorra para atualizar o produto DropDownList, vamos criar na etapa 2. Portanto, marque a opção de habilitar AutoPostBack do `categories` SmartTag do DropDownList.


[![Habilitar AutoPostBack de DropDownList categorias](master-detail-filtering-with-two-dropdownlists-vb/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image13.png)

**Figura 5**: Enable AutoPostBack para o `Categories` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Etapa 2: Exibindo produtos da categoria selecionada em uma segunda DropDownList

Com o `Categories` DropDownList concluído, nossa próxima etapa é exibir uma DropDownList dos produtos que pertencem à categoria selecionada. Para fazer isso, adicione outro DropDownList para a página chamada `ProductsByCategory`. Assim como acontece com o `Categories` DropDownList, crie um novo ObjectDataSource para a `ProductsByCategory` DropDownList chamado `ProductsByCategoryDataSource`.


[![Adicionar uma nova fonte de dados para a ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image16.png)

**Figura 6**: adicionar uma nova fonte de dados para o `ProductsByCategory` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image18.png))


[![Criar um novo ObjectDataSource chamado ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-vb/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image19.png)

**Figura 7**: criar um novo ObjectDataSource nomeado `ProductsByCategoryDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image21.png))


Uma vez que o `ProductsByCategory` DropDownList precisa exibir apenas os produtos que pertencem à categoria selecionada, ter o ObjectDataSource invocar o `GetProductsByCategoryID(categoryID)` método a partir o `ProductsBLL` objeto.


[![Optar por usar a classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image22.png)

**Figura 8**: escolha usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image24.png))


[![Configurar o ObjectDataSource para usar o método GetProductsByCategoryID(categoryID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image25.png)

**Figura 9**: configurar o ObjectDataSource para usar o `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image27.png))


Na etapa final do assistente, é necessário especificar o valor da *`categoryID`* parâmetro. Atribuir a esse parâmetro para o item selecionado do `Categories` DropDownList.


[![Extrair o valor do parâmetro categoryID na lista suspensa de categorias](master-detail-filtering-with-two-dropdownlists-vb/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image28.png)

**Figura 10**: extrair o *`categoryID`* valor de parâmetro a `Categories` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image30.png))


Com o ObjectDataSource configurado, tudo o que resta é especificar quais campos da fonte de dados são usados para a exibição e o valor dos itens do DropDownList. Exibição de `ProductName` do campo e use o `ProductID` campo como o valor.


[![Especifique os campos de origem de dados usados para texto e as propriedades de valor dos ListItems do DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image31.png)

**Figura 11**: especifique os campos de fonte de dados usada para a DropDownList `ListItem` s' `Text` e `Value` propriedades ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image33.png))


Com o ObjectDataSource e `ProductsByCategory` DropDownList configurado nossa página exibirá duas DropDownLists: a primeira lista todas as categorias enquanto o segundo listará os produtos que pertencem à categoria selecionada. Quando o usuário seleciona uma nova categoria na lista suspensa primeiro, ocorrerá um postback e a DropDownList segundo será sejam associadas novamente, mostrando os produtos que pertencem à categoria selecionada recentemente. As figuras 12 e 13 show `MasterDetailsDetails.aspx` em ação quando visualizado por meio de um navegador.


[![A categoria de bebidas é selecionada quando o primeiro visitando a página,](master-detail-filtering-with-two-dropdownlists-vb/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image34.png)

**Figura 12**: quando o primeiro visitando a página, a categoria de bebidas é selecionada ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image36.png))


[![Escolher uma categoria diferente exibe produtos a nova categoria](master-detail-filtering-with-two-dropdownlists-vb/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image37.png)

**Figura 13**: escolhendo um diversos vídeos de categoria de produtos da nova categoria ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image39.png))


Atualmente, o `productsByCategory` DropDownList, quando alteradas, faz *não* causam um postback. No entanto, será queremos que um postback ocorra depois que adicionarmos um DetailsView para exibir os detalhes do produto selecionado (etapa 3). Portanto, marque a caixa de seleção Enable AutoPostBack o `productsByCategory` SmartTag do DropDownList.


[![Habilitar o recurso de AutoPostBack de DropDownList productsByCategory](master-detail-filtering-with-two-dropdownlists-vb/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image40.png)

**Figura 14**: habilitar o recurso AutoPostBack para o `productsByCategory` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Etapa 3: Usando um DetailsView para exibir detalhes para o produto selecionado

A etapa final é exibir os detalhes para o produto selecionado em um DetailsView. Para fazer isso, adicione um DetailsView à página, defina suas `ID` propriedade para `ProductDetails`e crie um novo ObjectDataSource para ele. Configurar este ObjectDataSource para efetuar pull de seus dados a partir o `ProductsBLL` da classe `GetProductByProductID(productID)` método usando o valor selecionado da `ProductsByCategory` DropDownList para o valor da *`productID`* parâmetro.


[![Optar por usar a classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image43.png)

**Figura 15**: escolha usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image45.png))


[![Configurar o ObjectDataSource para usar o método GetProductByProductID(productID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image46.png)

**Figura 16**: configurar o ObjectDataSource para usar o `GetProductByProductID(productID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image48.png))


[![Extrair o valor do parâmetro productID do ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image49.png)

**Figura 17**: extrair o *`productID`* valor de parâmetro a `ProductsByCategory` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image51.png))


Você pode optar por exibir qualquer um dos campos disponíveis a `ProductDetails` DetailsView. Optamos por para remover o `ProductID`, `SupplierID`, e `CategoryID` campos e reordenadas e formatados os campos restantes. Além disso, eu limpo o DetailsView `Height` e `Width` propriedades, permitindo que o DetailsView expandir para a largura necessária para melhor exibição de seus dados em vez de ter que ele restrito a um tamanho especificado. A marcação completa aparece abaixo:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample1.aspx)]

Dedique uns momentos para experimentar o `MasterDetailsDetails.aspx` página em um navegador. À primeira vista pode parecer que tudo está funcionando conforme o desejado, mas há um problema sutil. Quando você escolhe uma nova categoria a `ProductsByCategory` DropDownList é atualizada para incluir esses produtos para a categoria selecionada, mas o `ProductDetails` DetailsView continuação mostrar as informações anteriores do produto. DetailsView é atualizado ao escolher um produto diferente para a categoria selecionada. Além disso, se você testar minuciosamente o suficiente, você descobrirá que se você escolher continuamente novas categorias (por exemplo, escolher Bebidas do `Categories` DropDownList e, em seguida, Condimentos, em seguida, doces) cada outra seleção de categoria faz com que o `ProductDetails`DetailsView a ser atualizado.

Para ajudar a concretizar a esse problema, vamos examinar um exemplo específico. Quando você primeiramente visite a página de categoria Bebidas está selecionada e os produtos relacionados são carregados no `ProductsByCategory` DropDownList. Chai é o produto selecionado e seus detalhes são exibidos no `ProductDetails` DetailsView, conforme mostrado na Figura 18.


[![Detalhes do produto selecionado são exibidos em um DetailsView](master-detail-filtering-with-two-dropdownlists-vb/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image52.png)

**Figura 18**: detalhes do produto selecionado o são exibidas em um DetailsView ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image54.png))


Se você alterar a seleção de categoria de bebidas para Condimentos, ocorre um postback e o `ProductsByCategory` DropDownList é atualizada adequadamente, mas DetailsView ainda exibe detalhes de Chai.


[![Detalhes de anteriormente selecionado do produto são continua sendo exibido](master-detail-filtering-with-two-dropdownlists-vb/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image55.png)

**Figura 19**: detalhes do produto selecionado anteriormente o são exibidas ainda ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image57.png))


Um novo produto na lista de separação DetailsView é atualizada conforme o esperado. Se você escolher uma nova categoria depois de alterar o produto, DetailsView novamente não atualizar. No entanto, se em vez de escolher um novo produto você selecionou uma nova categoria, seria atualizar DetailsView. O que no mundo está acontecendo aqui?

O problema é um problema de temporização no ciclo de vida da página. Sempre que uma página é solicitada a que ele passa um número de etapas de como sua renderização. Em uma destas etapas o ObjectDataSource controla a verificação para ver se qualquer um dos seus `SelectParameters` valores foram alterados. Se assim, o controle de Web de dados associado a ObjectDataSource sabe que precisa atualizar sua exibição. Por exemplo, quando uma nova categoria for selecionada, o `ProductsByCategoryDataSource` ObjectDataSource detecta que seus valores de parâmetro foram alterados e o `ProductsByCategory` DropDownList associa novamente em si, obtenção de produtos para a categoria selecionada.

O problema que surge nessa situação é que o ponto do ciclo de vida de página que verificam as ObjectDataSources parâmetros alterados ocorre *antes de* a reassociação dos controles da Web de dados associados. Portanto, ao selecionar uma nova categoria de `ProductsByCategoryDataSource` ObjectDataSource detecta uma alteração no valor do seu parâmetro. O ObjectDataSource usado pelas `ProductDetails` DetailsView, no entanto, não observar essas alterações porque o `ProductsByCategory` DropDownList ainda não sejam associadas novamente. Posteriormente no ciclo de vida de `ProductsByCategory` DropDownList associa novamente ao seu ObjectDataSource, pegando os produtos para a categoria selecionada recentemente. Enquanto o `ProductsByCategory` valor do DropDownList for alterado, o `ProductDetails` ObjectDataSource de DetailsView já fez sua verificação de valor de parâmetro; portanto, DetailsView exibe seus resultados anteriores. Essa interação é representada na Figura 20.


[![O valor de ProductsByCategory DropDownList muda depois ObjectDataSource de ProductDetails DetailsView verifica se há alterações](master-detail-filtering-with-two-dropdownlists-vb/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image58.png)

**Figura 20**: O `ProductsByCategory` DropDownList valor alterações após a `ProductDetails` ObjectDataSource verifica se de DetailsView há alterações ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image60.png))


Para corrigir isso, precisamos reassociar explicitamente o `ProductDetails` DetailsView após o `ProductsByCategory` DropDownList foi associado. Podemos pode fazer isso chamando o `ProductDetails` ovládacího prvku DetailsView `DataBind()` método quando o `ProductsByCategory` do DropDownList `DataBound` evento é acionado. Adicione o seguinte código de manipulador de eventos para o `MasterDetailsDetails.aspx` classe de code-behind da página (consulte a "[definir programaticamente o valores do parâmetro ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" para uma discussão sobre como adicionar um manipulador de eventos):


[!code-vb[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample2.vb)]

Após esta chamada explícita para o `ProductDetails` ovládacího prvku DetailsView `DataBind()` foi adicionado o método, o tutorial funciona conforme o esperado. Destaques da Figura 21 como isso foi alterado remediado nosso problema anterior.


[![ProductDetails DetailsView é vinculação de dados evento é acionado da explicitamente atualizada quando o ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image61.png)

**Figura 21**: O `ProductDetails` DetailsView é explicitamente atualizada quando o `ProductsByCategory` do DropDownList `DataBound` evento é acionado ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-vb/_static/image63.png))


## <a name="summary"></a>Resumo

DropDownList serve como um elemento de interface do usuário ideal para os relatórios mestre/detalhes onde há uma relação um-para-muitos entre os registros mestre e de detalhes. No tutorial anterior, vimos como usar uma única DropDownList para filtrar os produtos exibidos por categoria selecionada. Neste tutorial substituído GridView de produtos com uma DropDownList e usado um DetailsView para exibir os detalhes do produto selecionado. Os conceitos discutidos neste tutorial podem ser facilmente ampliados para modelos de dados que envolvem várias relações um-para-muitos, como clientes, pedidos e itens do pedido. Em geral, você sempre pode adicionar uma DropDownList para cada uma das entidades "um" nas relações um-para-muitos.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Hilton Giesenow. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-a-dropdownlist-vb.md)
> [Próximo](master-detail-filtering-across-two-pages-vb.md)
