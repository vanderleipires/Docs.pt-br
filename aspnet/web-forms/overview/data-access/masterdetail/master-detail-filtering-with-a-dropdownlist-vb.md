---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: Mestre/detalhes filtragem com uma DropDownList (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como exibir os registros principais em um controle DropDownList e os detalhes do item de lista selecionado em um GridView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: d9d50da7f11d1494d49fbeaa18a45991e577cdb3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834083"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Mestre/detalhes filtragem com uma DropDownList (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) ou [baixar PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> Neste tutorial, veremos como exibir os registros principais em um controle DropDownList e os detalhes do item de lista selecionado em um GridView.


## <a name="introduction"></a>Introdução

Um tipo comum de relatório é o *relatório de mestre/detalhes*, no qual o relatório começa mostrando um conjunto de registros "mestres". O usuário pode, em seguida, fazer drill down em um dos registros mestres, assim, detalhes do registro mestre "." Mestre/detalhes relatórios é uma opção ideal para visualizar relações um-para-muitos, como um relatório que mostra todas as categorias e, em seguida, que permite ao usuário selecionar uma categoria específica e exibir seus produtos associados. Além disso, os relatórios mestre/detalhes são úteis para exibir informações detalhadas de tabelas particularmente "largos" (aqueles que têm muitas colunas). Por exemplo, o nível de "mestre" de um relatório de detalhes mestre pode mostrar apenas o nome e a unidade de preço do produto dos produtos no banco de dados e fazer uma busca detalhada em um determinado produto mostraria os campos adicionais do produto (categoria, fornecedor, quantidade por unidade, e assim por diante).

Há muitas maneiras com que um relatório mestre/detalhes pode ser implementado. Sobre isso e os próximos três tutoriais, examinaremos uma variedade de relatórios mestre/detalhes. Neste tutorial, veremos como exibir os registros principais em um [controle DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) e os detalhes do item de lista selecionado em um GridView. Em particular, o relatório de detalhes mestre deste tutorial lista as informações de categoria e produto.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Etapa 1: Exibir as categorias em uma DropDownList

Nosso relatório mestre/detalhes listará as categorias na DropDownList, com produtos do item de lista selecionado exibidos mais adiante na página em um GridView. A primeira tarefa à frente de nós, em seguida, é ter as categorias exibidas na DropDownList. Abrir o `FilterByDropDownList.aspx` página o `Filtering` pasta, arraste uma DropDownList da caixa de ferramentas para o designer da página e defina seu `ID` propriedade para `Categories`. Em seguida, clique no link na marca inteligente do DropDownList Escolher fonte de dados. Isso exibirá o Assistente de configuração de fonte de dados.


[![Especifique a fonte de dados do DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**Figura 1**: especifique fonte a DropDownList de dados ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))


Optar por adicionar um novo ObjectDataSource denominado `CategoriesDataSource` que invoca a `CategoriesBLL` da classe `GetCategories()` método.


[![Adicionar um novo ObjectDataSource chamado CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**Figura 2**: adicionar um novo ObjectDataSource nomeado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))


[![Optar por usar a classe CategoriesBLL](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**Figura 3**: escolha usar o `CategoriesBLL` classe ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))


[![Configurar o ObjectDataSource para usar o método GetCategories()](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**Figura 4**: configurar o ObjectDataSource para usar o `GetCategories()` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))


Depois de configurar o ObjectDataSource ainda precisamos especificar qual campo de fonte de dados deve ser exibido na DropDownList e o que um deve ser associado como o valor do item de lista. Ter o `CategoryName` campo, como a exibição e `CategoryID` como o valor para cada item de lista.


[![Ter a exibição DropDownList CategoryName campo e Use CategoryID como o valor](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**Figura 5**: têm a exibição DropDownList a `CategoryName` campo e Use `CategoryID` como o valor ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))


Neste ponto, temos um controle DropDownList que é preenchido com os registros da `Categories` tabela (tudo feito em cerca de seis segundos). Figura 6 mostra nosso progresso até o momento quando visualizado por meio de um navegador.


[![Uma lista suspensa lista as categorias atuais](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**Figura 6**: um menu suspenso lista as categorias atual ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Etapa 2: Adicionando o GridView de produtos

A última etapa em nosso relatório mestre/detalhes é listar os produtos associados a categoria selecionada. Para fazer isso, adicione um controle GridView à página e criar um novo ObjectDataSource chamado `productsDataSource`. Ter o `productsDataSource` controle selecionar seus dados do `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método.


[![Selecione o método GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**Figura 7**: selecione o `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))


Depois de escolher esse método, o assistente ObjectDataSource nos solicita o valor para o método *`categoryID`* parâmetro. Para usar o valor de selecionado `categories` DropDownList item define a origem do parâmetro ControlID para e de controle `Categories`.


[![Defina o parâmetro categoryID como o valor de Categories DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**Figura 8**: defina o *`categoryID`* parâmetro para o valor da `Categories` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))


Reserve um tempo para fazer check-out de nosso progresso em um navegador. Quando o primeiro visitando a página, esses produtos pertencem à categoria selecionada (Bebidas) são exibidas (conforme mostrado na Figura 9), mas alterar DropDownList não atualiza os dados. Isso ocorre porque um postback deve ocorrer para que o GridView atualizar. Para fazer isso, temos duas opções (nenhum deles exige gravar nenhum código):

- **Definir as categorias DropDownList**[propriedade AutoPostBack](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**como True.** (Você pode fazer isso, marcando a opção Enable AutoPostBack na marca inteligente da DropDownList.) Isso vai disparar um postback sempre que a DropDownList selecionado item for alterado pelo usuário. Portanto, quando o usuário seleciona uma nova categoria de erro do DropDownList ocorrerá um postback e GridView será atualizado com os produtos para a categoria selecionada recentemente. (Essa é a abordagem usada neste tutorial.)
- **Adicione um controle da Web de botão ao lado do DropDownList.** Defina seu `Text` propriedade para a atualização ou algo semelhante. Com essa abordagem, o usuário precisará selecionar uma nova categoria e, em seguida, clique no botão. Clicando no botão causam um postback e atualizar o GridView para listar os produtos da categoria selecionada.

As figuras 9 e 10 ilustram o relatório mestre/detalhes em ação.


[![Quando o primeiro visitando a página, os produtos de bebidas são exibidos](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**Figura 9**: quando o primeiro visitando a página, os produtos de bebidas são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))


[![Selecionar um novo produto (produzir) automaticamente faz com que um PostBack, atualizando o GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**Figura 10**: seleção de um novo produto (produzir) automaticamente faz com que um PostBack, atualizando o GridView ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Adicionando um Item de lista "-- Escolha uma categoria –"

Ao visitar primeiro o `FilterByDropDownList.aspx` página categorias primeiro item da lista do DropDownList (Bebidas) é selecionado por padrão, mostrando os produtos de bebidas no GridView. Em vez de Mostrar produtos da primeira categoria, que podemos querer ter em vez disso, um item de DropDownList selecionado que diz algo como "-- Escolha uma categoria –".

Para adicionar um novo item de lista a DropDownList, vá para a janela Propriedades e clique nas elipses no `Items` propriedade. Adicionar um novo item de lista com o `Text` "-- Escolha uma categoria –" e o `Value` `-1`.


[![Adicionar um – escolha uma categoria – Item de lista](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**Figura 11**: adicionar um – escolha uma categoria – Item de lista ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))


Como alternativa, você pode adicionar o item de lista, adicionando a seguinte marcação ao DropDownList:


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

Além disso, precisamos definir o controle de DropDownList `AppendDataBoundItems` como True porque quando as categorias são associadas ao DropDownList do ObjectDataSource elas vão substituir quaisquer itens da lista adicionados manualmente se `AppendDataBoundItems` não é True.


![Defina a propriedade AppendDataBoundItems como True](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**Figura 12**: defina o `AppendDataBoundItems` propriedade como True


Após essas alterações, quando visitar a página é selecionada a opção "-- Escolha uma categoria –" pela primeira vez e nenhum produto é exibido.


[![O carregamento de página inicial sem produtos são exibidos.](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**Figura 13**: no the inicial página carga sem produtos são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))


O motivo pelo qual não há produtos são exibidos quando porque o item de lista "-- Escolha uma categoria –" está selecionado é porque seu valor é `-1` e não há nenhum produto no banco de dados com um `CategoryID` de `-1`. Se esse for o comportamento desejado, você terminou no momento! Se, no entanto, você deseja exibir *todos os* das categorias de quando o item de lista "-- Escolha uma categoria –" está selecionado, volte para o `ProductsBLL` classe e personalizar o `GetProductsByCategoryID(categoryID)` , de modo que ele invoca o `GetProducts()` método se o que for passado em *`categoryID`* parâmetro é menor que zero:


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

A técnica usada aqui é semelhante à abordagem que usamos para exibir todos os fornecedores de volta a [parâmetros declarativos](../basic-reporting/declarative-parameters-cs.md) tutorial, embora para este exemplo, estamos usando um valor de `-1` para indicar que todos os registros devem ser recuperado em oposição a `Nothing`. Isso ocorre porque o *`categoryID`* parâmetro do `GetProductsByCategoryID(categoryID)` método espera como o valor do inteiro passado, enquanto que no tutorial parâmetros declarativos que estavam passando um parâmetro de entrada de cadeia de caracteres.

A Figura 14 mostra uma captura de tela de `FilterByDropDownList.aspx` quando a opção "-- Escolha uma categoria –" estiver selecionada. Aqui, todos os produtos são exibidos por padrão, e o usuário pode restringir a exibição ao escolher uma categoria específica.


[![Todos os produtos estão agora listados por padrão](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**Figura 14**: todos os produtos estão agora listados por padrão ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))


## <a name="summary"></a>Resumo

Ao exibir dados relacionados hierarquicamente, isso geralmente ajuda a apresentar os dados usando os relatórios mestre/detalhes, do qual o usuário pode começar a ler os dados da parte superior da hierarquia e fazer drill down nos detalhes. Neste tutorial, examinamos a criação de um relatório simples mestre/detalhes mostrando produtos de uma determinada categoria. Isso foi realizado usando DropDownList para a lista de categorias e GridView para os produtos que pertencem à categoria selecionada.

No [próximo tutorial](master-detail-filtering-with-two-dropdownlists-vb.md) , faremos o DropDownList interface, uma etapa adicional, usando duas DropDownLists.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
> [Próximo](master-detail-filtering-with-two-dropdownlists-vb.md)
