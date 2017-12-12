---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
title: Filtragem com DropDownList (c#) mestre/detalhes | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como exibir os registros mestres em um controle DropDownList e os detalhes do item da lista selecionado em um controle GridView.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 53e659cc-eefb-40c1-a1dc-559481c99443
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 4632d3939204a954ed4fac88a04b0fea9bb15c83
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Mestre/detalhes filtragem com DropDownList (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_7_CS.exe) ou [baixar PDF](master-detail-filtering-with-a-dropdownlist-cs/_static/datatutorial07cs1.pdf)

> Neste tutorial, veremos como exibir os registros mestres em um controle DropDownList e os detalhes do item da lista selecionado em um controle GridView.


## <a name="introduction"></a>Introdução

Um tipo comum de relatório é o *relatório de detalhes/mestre*, em que o relatório começa mostrando um conjunto de registros "mestres". O usuário pode, em seguida, fazer drill down em um dos registros mestres, assim, detalhes do registro mestre "." Detalhes/mestre relatórios é uma opção ideal para visualizar relações um-para-muitos, como um relatório mostrando todas as categorias e, em seguida, permitir que um usuário selecione uma categoria específica e exibir seus produtos associados. Além disso, os relatórios mestre/detalhes são úteis para exibir informações detalhadas de tabelas particularmente "grande" (aqueles que têm muitas colunas). Por exemplo, o nível de "mestre" de um relatório de detalhes/mestre pode mostrar apenas o nome e a unidade de preço do produto de produtos no banco de dados e fazer uma busca detalhada em um determinado produto mostra os campos adicionais do produto (categoria, fornecedor, quantidade por unidade, e assim por diante).

Há muitas maneiras com que um relatório de detalhes/mestre pode ser implementado. Sobre isso e os próximos três tutoriais vamos examinar uma variedade de relatórios mestre/detalhes. Neste tutorial, veremos como exibir os registros mestres em um [controle DropDownList](https://msdn.microsoft.com/en-us/library/dtx91y0z.aspx) e os detalhes do item da lista selecionado em um controle GridView. Em particular, o relatório de detalhes/mestre neste tutorial listará informações de categoria e produto.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Etapa 1: Exibindo as categorias em DropDownList

Nosso relatório mestre/detalhes listará as categorias de DropDownList, com produtos do item de lista selecionado exibidos mais abaixo na página em um controle GridView. A primeira tarefa à frente de nós, em seguida, é ter as categorias exibidas no DropDownList. Abra o `FilterByDropDownList.aspx` página o `Filtering` pasta, arraste DropDownList da caixa de ferramentas para o designer da página e defina seu `ID` propriedade `Categories`. Em seguida, clique no link de marca inteligente do DropDownList Escolher fonte de dados. Isso exibirá o Assistente de configuração de fonte de dados.


[![Especifique a fonte de dados do DropDownList](master-detail-filtering-with-a-dropdownlist-cs/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image1.png)

**Figura 1**: especificar fonte de dados do DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image3.png))


Escolha adicionar um novo ObjectDataSource denominado `CategoriesDataSource` que invoca o `CategoriesBLL` da classe `GetCategories()` método.


[![Adicionar um novo ObjectDataSource denominado CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-cs/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image4.png)

**Figura 2**: adicionar um novo ObjectDataSource nomeado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image6.png))


[![Optar por usar a classe CategoriesBLL](master-detail-filtering-with-a-dropdownlist-cs/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image7.png)

**Figura 3**: escolher usar o `CategoriesBLL` classe ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image9.png))


[![Configurar o ObjectDataSource para usar o método GetCategories()](master-detail-filtering-with-a-dropdownlist-cs/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image10.png)

**Figura 4**: configurar o ObjectDataSource para usar o `GetCategories()` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image12.png))


Depois de configurar o ObjectDataSource ainda é necessário especificar o campo de fonte de dados deve ser exibido em DropDownList e quais um deve ser associado como o valor do item de lista. Ter o `CategoryName` campo como a exibição e `CategoryID` como o valor para cada item de lista.


[![Ter a exibição DropDownList CategoryName campo e Use CategoryID como o valor](master-detail-filtering-with-a-dropdownlist-cs/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image13.png)

**Figura 5**: têm a exibição DropDownList a `CategoryName` campo e Use `CategoryID` como o valor ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image15.png))


Neste ponto, temos um controle DropDownList que é preenchido com os registros da `Categories` tabela (todos realizada em cerca de cinco segundos). A Figura 6 mostra nosso progresso até o momento quando visualizada através de um navegador.


[![Uma lista suspensa lista as categorias atuais](master-detail-filtering-with-a-dropdownlist-cs/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image16.png)

**Figura 6**: A lista suspensa lista as categorias atual ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Etapa 2: Adicionando produtos GridView

A última etapa em nosso relatório mestre/detalhes é listar os produtos associados a categoria selecionada. Para fazer isso, adicione um controle GridView à página e criar um novo ObjectDataSource denominado `productsDataSource`. Ter o `productsDataSource` controle analisar seus dados a partir de `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método.


[![Selecione o método GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-cs/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image19.png)

**Figura 7**: selecione o `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image21.png))


Depois de escolher esse método, o assistente ObjectDataSource solicita a nós para o valor para o método  *`categoryID`*  parâmetro. Para usar o valor do selecionado `categories` item DropDownList define a origem de parâmetro de controle e ControlID para `Categories`.


[![Defina o parâmetro categoryID com o valor de DropDownList categorias](master-detail-filtering-with-a-dropdownlist-cs/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image22.png)

**Figura 8**: definir o  *`categoryID`*  parâmetro para o valor da `Categories` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image24.png))


Responda fazer check-out nosso progresso em um navegador. Ao primeiro visitar a página, os produtos pertencem à categoria selecionada (Bebidas) são exibidas (conforme mostrado na Figura 9), mas alterar DropDownList não atualiza os dados. Isso ocorre porque um postback deve ocorrer para que o GridView atualizar. Para fazer isso, temos duas opções (nenhum deles requer gravar nenhum código):

- **Definir as categorias DropDownList**[propriedade AutoPostBack](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**como True.** (Você pode fazer isso marcando a opção Enable AutoPostBack na marca inteligente do DropDownList.) Isso vai disparar um postback sempre que a DropDownList selecionada item é alterado pelo usuário. Portanto, quando o usuário seleciona uma nova categoria na lista suspensa um postback ocorrerá e GridView será atualizada com os produtos para a categoria selecionada recentemente. (Essa é a abordagem usados neste tutorial.)
- **Adicione um controle de botão Web ao lado de DropDownList.** Definir seu `Text` propriedade para atualização ou algo semelhante. Com essa abordagem, o usuário deverá selecionar uma nova categoria e, em seguida, clique no botão. Clique no botão causar um postback e atualizar o GridView para listar os produtos da categoria selecionada.

Figuras 9 e 10 ilustram o relatório de detalhes/mestre em ação.


[![Quando o primeiro visitando a página, os produtos de bebidas são exibidos](master-detail-filtering-with-a-dropdownlist-cs/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image25.png)

**Figura 9**: ao primeiro visitar a página, os produtos de bebidas são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image27.png))


[![Selecionar um novo produto (produção) automaticamente faz com que um PostBack, atualizando o GridView](master-detail-filtering-with-a-dropdownlist-cs/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image28.png)

**Figura 10**: selecionar um novo produto (produção) automaticamente faz com que um PostBack, atualizando o GridView ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Adicionar um Item de lista "-- Escolha uma categoria –"

Ao visitar primeiro o `FilterByDropDownList.aspx` página as categorias primeiro item da lista do DropDownList (Bebidas) é selecionado por padrão, mostrando os produtos de bebidas em GridView. Em vez de Mostrar produtos da primeira categoria, que queremos ter em vez disso, um item DropDownList selecionado que diz que algo como "-- Escolha uma categoria –".

Para adicionar um novo item de lista a DropDownList, vá para a janela de propriedades e clique nas elipses no `Items` propriedade. Adicionar um novo item de lista com o `Text` "-- Escolha uma categoria –" e o `Value` `-1`.


[![Adicionar um – escolha uma categoria - Item de lista](master-detail-filtering-with-a-dropdownlist-cs/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image31.png)

**Figura 11**: adicionar um – escolha uma categoria - Item de lista ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image33.png))


Como alternativa, você pode adicionar o item de lista, adicionando a seguinte marcação para DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample1.aspx)]

Além disso, é preciso definir o controle de DropDownList `AppendDataBoundItems` como True porque quando as categorias são associadas a DropDownList de ObjectDataSource vai substituir itens de lista adicionados manualmente se `AppendDataBoundItems` não é True.


![Defina a propriedade AppendDataBoundItems como True](master-detail-filtering-with-a-dropdownlist-cs/_static/image34.png)

**Figura 12**: definir o `AppendDataBoundItems` propriedade como True


Após essas alterações, quando o primeiro visitar a página é selecionada a opção "-- Escolha uma categoria –" e nenhum produto é exibido.


[![O carregamento de página inicial sem produtos são exibidos.](master-detail-filtering-with-a-dropdownlist-cs/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image35.png)

**Figura 13**: sobre o inicial página carga não produtos são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image37.png))


O motivo pelo qual não há produtos são exibidos quando porque o item de lista "-- Escolha uma categoria –" é selecionado é porque seu valor é `-1` e não há nenhum produto no banco de dados com um `CategoryID` de `-1`. Se esse é o comportamento desejado e terminar agora! Se, no entanto, você deseja exibir *todos os* das categorias de quando o item de lista "-- Escolha uma categoria –" é selecionado, retorne para o `ProductsBLL` classe e personalizar o `GetProductsByCategoryID(categoryID)` método para que ele chama o `GetProducts()` método se passado na  *`categoryID`*  parâmetro é menor que zero:

[!code-csharp[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample2.cs)]

A técnica usada aqui é similar à abordagem são usados para exibir todos os fornecedores de volta a [parâmetros declarativos](../basic-reporting/declarative-parameters-cs.md) tutorial, embora para este exemplo, estamos usando um valor de `-1` para indicar que todos os registros devem ser recuperar em vez de `null`. Isso ocorre porque o  *`categoryID`*  parâmetro o `GetProductsByCategoryID(categoryID)` método espera como valor de inteiro passado, enquanto o tutorial de parâmetros declarativos foram passando um parâmetro de entrada de cadeia de caracteres.

A Figura 14 mostra uma captura de tela de `FilterByDropDownList.aspx` quando a opção "-- Escolha uma categoria –" estiver selecionada. Aqui, todos os produtos são exibidos por padrão, e o usuário pode restringir a exibição, escolhendo uma categoria específica.


[![Todos os produtos estão agora listados por padrão](master-detail-filtering-with-a-dropdownlist-cs/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image38.png)

**Figura 14**: todos os produtos estão agora listados por padrão ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-cs/_static/image40.png))


## <a name="summary"></a>Resumo

Ao exibir dados relacionados hierarquicamente, isso geralmente ajuda a apresentar os dados usando os relatórios de detalhes/mestre, do qual o usuário pode começar a examinar os dados da parte superior da hierarquia e fazer drill down nos detalhes. Neste tutorial examinamos a criação de um relatório simples mestre/detalhes mostrando produtos de uma determinada categoria. Isso foi realizado usando DropDownList para a lista de categorias e GridView para os produtos que pertencem à categoria selecionada.

No [tutorial próxima](master-detail-filtering-with-two-dropdownlists-cs.md) levaremos o DropDownList interface, uma etapa adicional, usando dois DropDownLists.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Avançar](master-detail-filtering-with-two-dropdownlists-cs.md)
