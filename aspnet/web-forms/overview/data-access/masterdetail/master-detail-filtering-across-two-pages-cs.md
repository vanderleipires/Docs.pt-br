---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: "Filtragem em duas páginas (c#) mestre/detalhes | Microsoft Docs"
author: rick-anderson
description: "Neste tutorial, implementaremos esse padrão usando um GridView para listar os fornecedores no banco de dados. Cada linha de fornecedor em GridView conterá um e de exibição..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 6ddce74be81cb3ea33df9f7a6b91eae604b83025
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Mestre/detalhes filtragem em duas páginas (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) ou [baixar PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> Neste tutorial, implementaremos esse padrão usando um GridView para listar os fornecedores no banco de dados. Cada linha de fornecedor em GridView conterá um link de produtos de modo que, quando clicado, levará o usuário para uma página separada que lista os produtos para o fornecedor selecionado.


## <a name="introduction"></a>Introdução

Nos dois tutoriais anteriores, vimos como [exibir relatórios mestre/detalhes em uma única página da web usando DropDownLists](master-detail-filtering-with-a-dropdownlist-cs.md) para [exibir os registros de "mestres" e um controle GridView ou DetailsView](master-detail-filtering-with-two-dropdownlists-cs.md) para exibir o " Detalhes". Outro padrão comum usado para relatórios de detalhes/mestre é o registro mestre em uma página da web e os detalhes exibidos em outro. Um site de fórum, como [fóruns do ASP.NET](https://forums.asp.net/), é um bom exemplo desse padrão na prática. Fóruns do ASP.NET são compostos de uma variedade de fóruns do guia de Introdução, formulários da Web, controles de apresentação de dados e assim por diante. Cada Fórum é composto de vários threads e cada thread é composto de um número de mensagens. Na home page fóruns do ASP.NET, os fóruns são listados. Clicar em um fórum levado a uma `ShowForum.aspx` página, que lista os threads de Fórum. Da mesma forma, clicando em um thread necessário para `ShowPost.aspx`, que exibe as postagens do thread foi clicado.

Neste tutorial, implementaremos esse padrão usando um GridView para listar os fornecedores no banco de dados. Cada linha de fornecedor em GridView conterá um link de produtos de modo que, quando clicado, levará o usuário para uma página separada que lista os produtos para o fornecedor selecionado.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Etapa 1: Adicionando`SupplierListMaster.aspx`e`ProductsForSupplierDetails.aspx`páginas para o`Filtering`pasta

Ao definir o layout de página no terceiro tutorial adicionamos um número de páginas "início" o `BasicReporting`, `Filtering`, e `CustomFormatting` pastas. No entanto, não adicionamos uma página inicial para este tutorial nesse momento, portanto, dedique um tempo para adicionar duas novas páginas para o `Filtering` pasta: `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx`lista os registros "mestres" (os fornecedores) ao `ProductsForSupplierDetails.aspx` exibirá os produtos para o fornecedor selecionado.

Ao criar essas duas novas páginas ser determinadas para associá-los com o `Site.master` página mestra.


![Adicionar as páginas de ProductsForSupplierDetails.aspx e SupplierListMaster.aspx para a pasta de filtragem](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Figura 1**: adicionar o `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx` páginas para o `Filtering` pasta


Além disso, ao adicionar novas páginas ao projeto, certifique-se atualizar o arquivo de mapa de site, `Web.sitemap`, adequadamente. Para este tutorial simplesmente adicionar o `SupplierListMaster.aspx` página para o mapa do site usando o seguinte conteúdo XML como um filho dos relatórios de filtragem `<siteMapNode>` elemento:


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Você pode ajudar a automatizar o processo de atualizar o arquivo de mapa do site quando adicionar novos ASP.NET páginas usando [K. Scott Allen](http://odetocode.com/Blogs/scott/)do Visual Studio de livre [macros de mapa de Site](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Etapa 2: Exibir a lista de fornecedor no`SupplierListMaster.aspx`

Com o `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx` páginas criadas, nossa próxima etapa é criar o GridView de fornecedores `SupplierListMaster.aspx`. Adicionar um controle GridView à página e associá-lo a um novo ObjectDataSource. Este ObjectDataSource deve usar o `SuppliersBLL` da classe `GetSuppliers()` método para retornar todos os fornecedores.


[![Selecione a classe SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Figura 2**: selecione o `SuppliersBLL` classe ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![Configurar o ObjectDataSource para usar o método GetSuppliers()](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Figura 3**: configurar o ObjectDataSource para usar o `GetSuppliers()` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image7.png))


É preciso incluir um link chamado exibir produtos em cada linha GridView que, quando clicado, leva o usuário para `ProductsForSupplierDetails.aspx` passando a linha selecionada `SupplierID` valor por meio de querystring. Por exemplo, se o usuário clica no link Exibir produtos para o fornecedor de Tokyo Traders (que tem um `SupplierID` valor de 4), eles devem ser enviados para `ProductsForSupplierDetails.aspx?SupplierID=4`.

Para fazer isso, adicione um [HyperLinkField](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.hyperlinkfield.aspx) GridView, que adiciona um hiperlink para cada linha em GridView. Iniciar clicando no link Editar colunas de marcas inteligentes do GridView. Em seguida, selecione o HyperLinkField da lista na parte superior esquerda e clique em Adicionar para incluir o HyperLinkField na lista de campos do GridView.


[![Adicionar um HyperLinkField a GridView](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Figura 4**: adicionar um HyperLinkField a GridView ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image10.png))


O HyperLinkField pode ser configurado para usar o mesmo texto ou URL valores no link em cada linha em GridView, ou pode basear esses valores nos valores de dados associados a cada linha específica. Para especificar um estático valor em todas as linhas usar o HyperLinkField `Text` ou `NavigateUrl` propriedades. Como queremos que o texto do link para ser o mesmo para todas as linhas, definir o HyperLinkField `Text` propriedade para exibir os produtos.


[![Defina a propriedade de texto do HyperLinkField para exibir os produtos](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Figura 5**: definir o HyperLinkField `Text` propriedade para exibir os produtos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image13.png))


Para definir o texto ou valores de URL a ser com base nos dados subjacentes associados à linha GridView, especifique o texto de campos de dados ou valores da URL devem ser extraídos no `DataTextField` ou `DataNavigateUrlFields` propriedades. `DataTextField`só pode ser definida como um único campo de dados; `DataNavigateUrlFields`, no entanto, pode ser definido como uma lista delimitada por vírgulas de campos de dados. Com frequência, é preciso basear o texto ou a URL em uma combinação de valor de campo de dados da linha atual e alguns marcação estática. Neste tutorial, por exemplo, queremos que a URL de links do HyperLinkField ser `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, onde  *`supplierID`*  é uma linha de cada GridView `SupplierID` valor. Observe que precisamos estáticos e controlados por dados valores aqui: o `ProductsForSupplierDetails.aspx?SupplierID=` parte da URL do link é estático, enquanto o  *`supplierID`*  parte é controlada por dados como seu valor é a cada linha própria `SupplierID` valor.

Para indicar uma combinação de valores estáticos e controlados por dados, use o `DataTextFormatString` e `DataNavigateUrlFormatString` propriedades. Nessas propriedades, digite a marcação estática conforme necessário e, em seguida, use o marcador `{0}` onde você deseja que o valor do campo especificado no `DataTextField` ou `DataNavigateUrlFields` propriedades a serem exibidos. Se o `DataNavigateUrlFields` propriedade tem uso especificado do vários campos `{0}` onde você deseja que o primeiro valor do campo inserido, `{1}` para o segundo valor de campo e assim por diante.

Aplicar isso em nosso tutorial, precisamos definir o `DataNavigateUrlFields` propriedade `SupplierID`, já que é o campo de dados cujo valor é necessário para personalizar em uma base por linha, e o `DataNavigateUrlFormatString` propriedade `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Configurar o HyperLinkField para incluir a URL do Link apropriado com base no SupplierID](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Figura 6**: configurar o HyperLinkField para incluir o adequada Link URL com base na `SupplierID` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image16.png))


Depois de adicionar o HyperLinkField, fique à vontade para personalizar e reordenar os campos do GridView. A marcação a seguir mostra GridView depois de feitas algumas personalizações secundárias do nível de campo.


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Reserve um tempo para exibir o `SupplierListMaster.aspx` página através de um navegador. Como mostra a Figura 7, a página atualmente lista todos os fornecedores, incluindo um link Exibir produtos. Clicando em Exibir produtos link o levará para `ProductsForSupplierDetails.aspx`, passando ao longo do fornecedor `SupplierID` na querystring.


[![Cada linha de fornecedor contém um Link de produtos do modo de exibição](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Figura 7**: cada linha de fornecedor contém um Link de produtos do modo de exibição ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Etapa 3: Lista os produtos do fornecedor no`ProductsForSupplierDetails.aspx`

Neste ponto o `SupplierListMaster.aspx` página está enviando os usuários `ProductsForSupplierDetails.aspx`, passando o fornecedor selecionado `SupplierID` na querystring. Etapa final do tutorial é exibir os produtos em um GridView no `ProductsForSupplierDetails.aspx` cujo `SupplierID` é igual a `SupplierID` passado querystring. Para fazer isso, inicie adicionando um controle GridView para o `ProductsForSupplierDetails.aspx` página, usando um novo controle ObjectDataSource chamado `ProductsBySupplierDataSource` que invoca o `GetProductsBySupplierID(supplierID)` método do `ProductsBLL` classe.


[![Adicionar um novo ObjectDataSource denominado ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Figura 8**: adicionar um novo ObjectDataSource nomeado `ProductsBySupplierDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![Selecione a classe ProductsBLL](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Figura 9**: selecione o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![Ter o ObjectDataSource invocar o método GetProductsBySupplierID(supplierID)](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Figura 10**: tem o ObjectDataSource invocar o `GetProductsBySupplierID(supplierID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image28.png))


A etapa final do Assistente para configurar a fonte de dados nos pede para fornecer a origem do `GetProductsBySupplierID(supplierID)` do método  *`supplierID`*  parâmetro. Para usar o valor de querystring, defina a origem do parâmetro QueryString e insira o nome do valor de querystring para usar na caixa de texto QueryStringField (`SupplierID`).


[![Preencher o valor do parâmetro do valor de Querystring SupplierID supplierID](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Figura 11**: preencher o  *`supplierID`*  valor do parâmetro de `SupplierID` valor de Querystring ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image31.png))


Isso é tudo que é necessário para que ele! A Figura 12 mostra o `ProductsForSupplierDetails.aspx` página quando visitado clicando no link de Tokyo Traders de `SupplierListMaster.aspx`.


[![Os produtos fornecidos pela Tokyo Traders são mostrados](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Figura 12**: os produtos fornecidos pela Tokyo Traders são mostrados ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Exibindo informações do fornecedor no`ProductsForSupplierDetails.aspx`

Como mostra a Figura 12, o `ProductsForSupplierDetails.aspx` página simplesmente lista os produtos que são fornecidos pelo `SupplierID` especificado na querystring. Alguém enviados diretamente para essa página, no entanto, não saberia que Figura 12 está mostrando produtos Tokyo Traders. Para corrigir isso, que podemos exibir informações de fornecedor também nesta página.

Comece adicionando um FormView acima os produtos GridView. Criar um novo controle ObjectDataSource chamado `SuppliersDataSource` que invoca o `SuppliersBLL` da classe `GetSupplierBySupplierID(supplierID)` método.


[![Selecione a classe SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Figura 13**: selecione o `SuppliersBLL` classe ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![Ter o ObjectDataSource invocar o método GetSupplierBySupplierID(supplierID)](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Figura 14**: tem o ObjectDataSource invocar o `GetSupplierBySupplierID(supplierID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image40.png))


Assim como acontece com o `ProductsBySupplierDataSource`, ter o  *`supplierID`*  parâmetro recebe o valor da `SupplierID` valor de querystring.


[![Preencher o valor do parâmetro do valor de Querystring SupplierID supplierID](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Figura 15**: preencher o  *`supplierID`*  valor do parâmetro de `SupplierID` valor de Querystring ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image43.png))


Ao associar o FormView a ObjectDataSource no modo Design, o Visual Studio criará automaticamente o FormView `ItemTemplate`, `InsertItemTemplate`, e `EditItemTemplate` com controles de rótulo e da Web da caixa de texto para cada um dos campos de dados retornados pelo ObjectDataSource. Como queremos apenas exibir fornecedor informações à vontade remover o `InsertItemTemplate` e `EditItemTemplate`. Em seguida, edite o ItemTemplate para que ele exibe o nome da empresa do fornecedor em um `<h3>` elemento e o endereço, cidade, país e número de telefone abaixo do nome da empresa. Como alternativa, você pode definir manualmente o FormView `DataSourceID` e crie o `ItemTemplate` marcação, como foi volta a "[exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" tutorial.

Após essas edições marcação declarativa de FormView deve ser semelhante ao seguinte:


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

A Figura 16 mostra uma captura de tela de `ProductsForSupplierDetails.aspx` página depois que as informações do fornecedor detalhadas acima foi incluídas.


[![A lista de produtos inclui um resumo sobre o fornecedor](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Figura 16**: A lista de produtos inclui um resumo sobre o fornecedor ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Aplicar o último toca para o`ProductsForSupplierDetails.aspx`interface do usuário

Para melhorar o usuário experiência para este relatório lá estão algumas inclusões devemos fazer o `ProductsForSupplierDetails.aspx` página. Atualmente a única maneira de um usuário pode ir do `ProductsForSupplierDetails.aspx` página de volta para a lista de fornecedores é clique do botão Voltar do seu navegador. Vamos adicionar um controle de hiperlink para o `ProductsForSupplierDetails.aspx` página links de volta para `SupplierListMaster.aspx`, fornecendo outra maneira para o usuário retornar à lista mestra.


[![Adicionar um controle de hiperlink para levar o usuário ao SupplierListMaster.aspx](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Figura 17**: adicionar um controle de hiperlink para levar o usuário voltar para `SupplierListMaster.aspx` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image49.png))


Se o usuário clica no link Exibir produtos para um fornecedor que não tenha todos os produtos, o `ProductsBySupplierDataSource` ObjectDataSource em `ProductsForSupplierDetails.aspx` não retornar nenhum resultado. O GridView associado a ObjectDataSource não processa qualquer marcação resultando em uma região em branco na página no navegador do usuário. Para maior clareza se comunicar com o usuário que não há nenhum produto associado com o fornecedor selecionado podemos definir o GridView `EmptyDataText` propriedade na mensagem que deseja exibir quando surge uma situação. Defini essa propriedade como "Não há nenhum produto fornecido por esse fornecedor"

Por padrão, todos os fornecedores no banco de dados Northwind fornecem pelo menos um produto. No entanto, para este tutorial, você modificado manualmente o `Products` para que o fornecedor Escargots Nouveaux não está mais associado a todos os produtos da tabela. Figura 18 mostra a página de detalhes para Escargots Nouveaux após essa alteração foi feita.


[![Os usuários serão informados de que o fornecedor não fornece todos os produtos](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Figura 18**: os usuários serão informados de que o fornecedor não fornece todos os produtos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>Resumo

Enquanto os relatórios mestre/detalhes podem exibir registros mestre e de detalhes em uma única página, em muitos sites eles são separados em duas páginas da web. Neste tutorial vimos como implementar tal relatório mestre/detalhes com os fornecedores listados em um GridView na página da web "mestre" e os produtos associados listados na página "Detalhes". Cada linha de fornecedor na página da web mestre continha um link para a página de detalhes do passado ao longo da linha `SupplierID` valor. Esses links específicas de linha podem ser adicionados facilmente usando HyperLinkField do GridView.

Na página de detalhes recuperar os produtos para o fornecedor especificado foi realizada invocando o `ProductsBLL` da classe `GetProductsBySupplierID(supplierID)` método. O  *`supplierID`*  o valor do parâmetro foi especificado declarativamente usando querystring como a origem do parâmetro. Também vimos como exibir os detalhes de fornecedor na página de detalhes do usando um FormView.

Nosso [tutorial próxima](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) é o final em relatórios de detalhes/mestre. Vamos examinar como exibir uma lista de produtos em um GridView onde cada linha tem um botão de seleção. Clicar no botão de seleção exibirá os detalhes do produto em um controle DetailsView na mesma página.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Giesenow Hilton. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](master-detail-filtering-with-two-dropdownlists-cs.md)
[Próximo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
