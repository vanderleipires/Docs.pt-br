---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: Paginação de dados de relatório em um controle DataList ou Repeater (VB) | Microsoft Docs
author: rick-anderson
description: Durante a paginação automática nem o DataList nem repetidor de oferta ou suporte à classificação, este tutorial mostra como adicionar suporte à paginação no DataList ou Repeater,...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 64615f126f87cec7a96f86385ee7a717fdcdd103
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832436"
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Paginação de dados de relatório em um controle DataList ou Repeater (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) ou [baixar PDF](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Enquanto o DataList nem Repeater oferta automática de paginação ou classificação de suporte, este tutorial mostra como adicionar suporte à paginação no DataList ou Repeater, que permite a exibição interfaces muito mais flexível de paginação e dados.


## <a name="introduction"></a>Introdução

Paginação e classificação são dois recursos muito comuns ao exibir dados em um aplicativo online. Por exemplo, ao pesquisar livros sobre o ASP.NET em uma livraria online, pode haver centenas de livros, mas o relatório listando os resultados da pesquisa lista apenas dez correspondências por página. Além disso, os resultados podem ser classificados por título, preço, contagem de páginas, nome do autor e assim por diante. Como discutimos na [paginação e classificação de dados do relatório](../paging-and-sorting/paging-and-sorting-report-data-vb.md) tutorial, os controles GridView, DetailsView e FormView fornecem suporte interno de paginação que pode ser habilitado na escala de uma caixa de seleção. O GridView também inclui suporte à classificação.

Infelizmente, nem o DataList nem Repeater oferecem paginação ou suporte à classificação automática. Neste tutorial, examinaremos como adicionar suporte à paginação no DataList ou Repeater. Manualmente, deve criar a interface de paginação, exibir a página apropriada de registros e lembre-se a página está sendo visitada em postagens. Enquanto isso levar mais tempo e código do que com o GridView, DetailsView ou FormView, DataList e Repeater permitem muito mais flexível de paginação e dados de interfaces de exibição.

> [!NOTE]
> Este tutorial se concentra exclusivamente na paginação. No próximo tutorial voltaremos nossa atenção à adição de recursos de classificação.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Etapa 1: Adicionando a paginação e classificação Tutorial páginas da Web

Antes de começar este tutorial, deixe s primeiro dedique uns momentos para adicionar as páginas do ASP.NET, precisaremos para este tutorial e o próximo. Comece criando uma nova pasta no projeto chamado `PagingSortingDataListRepeater`. Em seguida, adicione as seguintes páginas ASP.NET cinco nesta pasta, manter todos eles configurado para usar a página mestra `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Crie uma pasta PagingSortingDataListRepeater e adicione as páginas do Tutorial do ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figura 1**: criar um `PagingSortingDataListRepeater` pasta e adicione as páginas do Tutorial do ASP.NET


Em seguida, abra o `Default.aspx` da página e arraste o `SectionLevelTutorialListing.ascx` controle de usuário do `UserControls` pasta para a superfície de Design. Esse controle de usuário que criamos na [páginas mestras e navegação no Site](../introduction/master-pages-and-site-navigation-vb.md) tutorial, enumera o mapa do site e exibe esses tutoriais na seção atual em uma lista com marcadores.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Figura 2**: Adicione o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))


Para fazer com que a lista com marcadores exiba a paginação e classificação tutoriais, criaremos, precisamos adicioná-los ao mapa do site. Abra o `Web.sitemap` arquivo e adicione a marcação a seguir após a edição e exclusão com a marcação de nó de mapa de site DataList:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]


![Atualizar o mapa de Site para incluir as novas páginas do ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Figura 3**: atualizar o mapa de Site para incluir as novas páginas do ASP.NET


## <a name="a-review-of-paging"></a>Uma revisão de paginação

Nos tutoriais anteriores, vimos como paginar os dados nos controles GridView, DetailsView e FormView. Esses três controles oferecem uma forma simples de paginação chamada *paginação padrão* que pode ser implementada, simplesmente marcando a opção de habilitar a paginação na marca inteligente do controle s. Com paginação padrão, sempre que uma página de dados é solicitada na primeira página visite ou quando o usuário navega para uma página diferente dos dados GridView, DetailsView, ou controle FormView novamente solicita *todos os* dos dados das ObjectDataSource. Ele, em seguida, recorte o determinado conjunto de registros a serem exibidos considerando o índice da página solicitada e o número de registros a serem exibidos por página. Discutimos a paginação padrão em detalhes na [paginação e classificação de dados do relatório](../paging-and-sorting/paging-and-sorting-report-data-vb.md) tutorial.

Uma vez que a paginação padrão solicita novamente todos os registros de cada página, não é prático quando a paginação por meio de suficientemente grandes quantidades de dados. Por exemplo, imagine paginação por meio de 50.000 registros com um tamanho de página de 10. Cada vez que o usuário move para uma nova página, todos os registros de 50.000 devem ser recuperados do banco de dados, mesmo apenas dez deles é exibidas.

*Paginação personalizada* resolve as preocupações de desempenho da paginação padrão captando somente o subconjunto exato de registros a serem exibidos na página solicitada. Ao implementar a paginação personalizada, podemos deve escrever a consulta SQL que retorna apenas o conjunto correto de registros com eficiência. Vimos como criar uma consulta usando o SQL Server 2005 s nova [ `ROW_NUMBER()` palavra-chave](http://www.4guysfromrolla.com/webtech/010406-1.shtml) volta a [com eficiência de paginação por meio de grandes quantidades de dados](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) tutorial.

Para implementar a paginação padrão nos controles DataList ou Repeater, podemos usar o [ `PagedDataSource` classe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) como um wrapper em torno de `ProductsDataTable` cujo conteúdo está sendo paginado. O `PagedDataSource` classe tem um `DataSource` propriedade que pode ser atribuída a qualquer objeto enumerável e [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) e [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) propriedades que indicam quantos registros a Mostre por página e o índice da página atual. Depois que essas propriedades foram definidas, o `PagedDataSource` pode ser usado como a fonte de dados de qualquer controle da Web de dados. O `PagedDataSource`, quando enumerada, será somente retornar o subconjunto apropriado de registros de sua interna `DataSource` com base nas `PageSize` e `CurrentPageIndex` propriedades. A Figura 4 ilustra a funcionalidade do `PagedDataSource` classe.


![O PagedDataSource encapsula um objeto enumerável com uma Interface paginável](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Figura 4**: O `PagedDataSource` encapsula um objeto enumerável com uma Interface paginável


O `PagedDataSource` objeto pode ser criado e configurado diretamente da camada de lógica de negócios e associado a uma DataList ou Repeater por meio de um ObjectDataSource, ou pode ser criada e configurada diretamente na classe de code-behind de página s ASP.NET. Se a segunda abordagem é usada, devemos abrir mão de usar o ObjectDataSource e em vez disso, associar os dados paginados no DataList ou Repeater programaticamente.

O `PagedDataSource` objeto também tem propriedades para dar suporte a paginação personalizada. No entanto, podemos ignorar usando um `PagedDataSource` para a paginação personalizada porque já temos métodos BLL no `ProductsBLL` classe criada para a paginação personalizada que retornam os registros precisos para exibir.

Neste tutorial, examinaremos implementar paginação padrão em uma DataList, adicionando um novo método para o `ProductsBLL` classe que retorna adequadamente configurado `PagedDataSource` objeto. No próximo tutorial, veremos como usar a paginação personalizada.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Etapa 2: Adicionando um método de paginação padrão na camada de lógica de negócios

O `ProductsBLL` classe atualmente tem um método para retornar todas as informações de produto `GetProducts()` e outra para retornar um subconjunto específico de produtos em um índice inicial `GetProductsPaged(startRowIndex, maximumRows)`. Com a paginação padrão, o GridView, DetailsView e FormView controla todo o uso de `GetProducts()` método para recuperar todos os produtos, mas, em seguida, use um `PagedDataSource` internamente para exibir somente o subconjunto correto de registros. Para replicar essa funcionalidade com os controles DataList e Repeater, podemos criar um novo método na BLL que imita esse comportamento.

Adicione um método para o `ProductsBLL` classe denominada `GetProductsAsPagedDataSource` que utiliza dois parâmetros de entrada de inteiro:

- `pageIndex` o índice da página para exibir, indexados em zero, e
- `pageSize` o número de registros a serem exibidos por página.

`GetProductsAsPagedDataSource` começa recuperando *todos os* os registros da `GetProducts()`. Em seguida, ele cria um `PagedDataSource` objeto, definindo seu `CurrentPageIndex` e `PageSize` propriedades para os valores passados na `pageIndex` e `pageSize` parâmetros. O método é concluído, retornando isso configurado `PagedDataSource`:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Etapa 3: Exibir informações sobre o produto em um DataList de usar a paginação padrão

Com o `GetProductsAsPagedDataSource` método adicionado para o `ProductsBLL` classe, agora podemos criar um DataList ou Repeater que fornece a paginação padrão. Comece abrindo o `Paging.aspx` página o `PagingSortingDataListRepeater` pasta e arraste uma DataList da caixa de ferramentas para o Designer, definindo DataList s `ID` propriedade para `ProductsDefaultPaging`. De marca inteligente DataList s, crie um novo ObjectDataSource denominado `ProductsDefaultPagingDataSource` e configure-o para que ele recupere dados usando o `GetProductsAsPagedDataSource` método.


[![Criar um ObjectDataSource e configurá-lo para usar (de) o GetProductsAsPagedDataSource método](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figura 5**: criar um ObjectDataSource e configurá-lo para usar o `GetProductsAsPagedDataSource` `()` método ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


Defina as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Defina a lista suspensa na atualização, inserção e excluir guias como (nenhum)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figura 6**: defina a lista suspensa na atualização, inserção e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


Uma vez que o `GetProductsAsPagedDataSource` método espera dois parâmetros de entrada, o assistente solicita para a origem desses valores de parâmetro.

O índice da página e os valores de tamanho de página devem ser memorizados entre postbacks. Eles podem ser armazenados no estado de exibição, persistidas querystring, armazenados em variáveis de sessão ou lembradas usando alguma outra técnica. Para este tutorial, usaremos a cadeia de consulta, que tem a vantagem de permitir que uma determinada página de dados para ser um indicador.

Em particular, use a querystring campos pageIndex e pageSize para o `pageIndex` e `pageSize` parâmetros, respectivamente (veja a Figura 7). Reserve um tempo para definir os valores padrão para esses parâmetros, como os valores de querystring ganhas um t estar presente quando um usuário visita primeiro nessa página. Para `pageIndex`, defina o valor padrão como 0 (o que mostrará a primeira página de dados) e `pageSize` valor padrão de s para 4.


[![Usar cadeia de consulta como a origem para os parâmetros pageIndex e pageSize](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figura 7**: Use a cadeia de consulta como a fonte para o `pageIndex` e `pageSize` parâmetros ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))


Depois de configurar o ObjectDataSource, o Visual Studio cria automaticamente um `ItemTemplate` para DataList. Personalizar o `ItemTemplate` para que somente o nome do produto s, categoria e fornecedor são mostrados. Também definir s DataList `RepeatColumns` propriedade como 2, seu `Width` de 100% e seu `ItemStyle` s `Width` como 50%. Essas configurações de largura fornecerá espaçamento igual para as duas colunas.

Depois de fazer essas alterações, a marcação de s DataList e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Uma vez que estamos não estão executando qualquer atualização ou funcionalidade de exclusão neste tutorial, você poderá desabilitar o estado de exibição do DataList s para reduzir o tamanho da página renderizada.


Quando inicialmente ao visitar essa página por meio de um navegador, nem o `pageIndex` nem `pageSize` parâmetros querystring são fornecidos. Portanto, os valores padrão de 0 e 4 são usados. Como mostra a Figura 8, isso resulta em uma DataList que exibe os quatro primeiros produtos.


[![Os primeiros quatro produtos são listados](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Figura 8**: os primeiros quatro produtos são listados ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))


Sem uma interface de paginação, daí s simples no momento, não significa que um usuário navegue para a segunda página de dados. Vamos criar uma interface de paginação na etapa 4. Por enquanto, porém, paginação só pode ser realizada diretamente especificando os critérios de paginação na querystring. Por exemplo, para exibir a segunda página, altere a URL na barra de endereços do navegador s da `Paging.aspx` para `Paging.aspx?pageIndex=2` e pressione Enter. Isso faz com que a segunda página de dados a serem exibidos (consulte a Figura 9).


[![A segunda página de dados é exibido](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Figura 9**: O segundo dos dados da página é exibido ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Etapa 4: Criar a Interface de paginação

Há uma variedade de diferentes interfaces de paginação que pode ser implementada. Os controles GridView, DetailsView e FormView fornecem quatro interfaces diferentes para escolher entre:

- **Em seguida, anterior** os usuários podem ir a uma página por vez, para um próximo ou anterior.
- **Next, Previous, First, Last** além dos botões Próximo e anterior, essa interface inclui primeiro e último botões para mover para a primeira ou última página.
- **Numérico** lista os números de página na interface de paginação, permitindo que um usuário ir rapidamente para uma determinada página.
- **Numérico, primeiro, último** os números de páginas numéricas, além de inclui botões para mover para a primeira ou última página.

Para o DataList e Repeater, somos responsáveis por decidir por uma interface de paginação e implementá-lo. Isso envolve a criação de controles da Web necessários na página e exibindo a página solicitada quando um botão específico de interface de paginação é clicado. Além disso, determinados controles de interface de paginação talvez precise ser desabilitada. Por exemplo, ao exibir a primeira página de dados usando o próximo, anterior, primeiro, última interface, botões a primeira e a anterior deve ser desabilitada.

Para este tutorial, use s permitem que o próximo, anterior, primeiro, última interface. Adicione quatro controles da Web de botão para a página e defina suas `ID` s a serem `FirstPage`, `PrevPage`, `NextPage`, e `LastPage`. Defina as `Text` propriedades a serem &lt; &lt; primeiro, &lt; Prev, ao lado &gt;e o último &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Em seguida, crie um `Click` manipulador de eventos para cada um desses botões. Em alguns instantes, adicionaremos o código necessário para exibir a página solicitada.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Lembrar-se o número Total de registros sendo paginado por meio de

Independentemente da interface de paginação selecionada, precisamos de computação e lembre-se o número total de registros sendo paginado por meio do. A contagem total de linhas (em conjunto com o tamanho da página) determina o número total de páginas de dados está sendo paginado, que determina quais controles de interface de paginação são adicionados ou estão habilitados. Em seguida, Previous, First, última interface de que estamos criando, a contagem de páginas é usado de duas maneiras:

- Para determinar se estamos está exibindo a última página, caso em que os botões Próximo e último estão desabilitados.
- Se o usuário clica no botão última que precisamos whisk-los para a última contagem de página, cujo índice é um menor do que a página.

A contagem de páginas é calculada como o limite máximo da contagem total de linhas é dividida pelo tamanho da página. Por exemplo, se nós a paginação por meio de 79 registros com quatro registros por página, em seguida, a contagem de páginas é de 20 (o limite máximo de 79 / 4). Se estivermos utilizando a interface de paginação numérico, essas informações nos informem sobre quantos botões numéricos de página para exibir; Se nossa interface de paginação inclui botões de próximo ou último, a contagem de páginas é usada para determinar quando deve desabilitar os botões Próximo ou último.

Se a interface de paginação inclui um botão de última, é imperativo que o número total de registros sendo paginado por meio de ser lembrados entre postbacks, para que quando o último botão é clicado podemos determinar o último índice de página. Para facilitar isso, crie um `TotalRowCount` propriedade em que a classe de code-behind de s de página ASP.NET que persiste seu valor para o estado de exibição:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Além `TotalRowCount`, reserve um minuto para criar as propriedades de nível de página somente leitura para acessar facilmente o índice da página, o tamanho da página e contagem de página:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Determinando o número Total de registros sendo paginado por meio de

O `PagedDataSource` objeto retornado de s ObjectDataSource `Select()` método tem dentro dele *todos os* dos registros de produto, mesmo que apenas um subconjunto deles são exibidas no DataList. O `PagedDataSource` s [ `Count` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) retorna apenas o número de itens que serão exibidos no DataList; o [ `DataSourceCount` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) retorna o número total de itens dentro de `PagedDataSource`. Portanto, precisamos atribuir o s de página do ASP.NET `TotalRowCount` o valor da propriedade do `PagedDataSource` s `DataSourceCount` propriedade.

Para fazer isso, crie um manipulador de eventos para o s ObjectDataSource `Selected` eventos. No `Selected` manipulador de eventos, temos acesso ao valor de retorno os s ObjectDataSource `Select()` método nesse caso, o `PagedDataSource`.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Exibindo a página solicitada de dados

Quando o usuário clica em um dos botões na interface de paginação, é necessário exibir a página solicitada de dados. Como os parâmetros de paginação são especificados por meio da cadeia de consulta, para mostrar a página solicitada de dados de uso `Response.Redirect(url)` para que o usuário s navegador solicitar novamente o `Paging.aspx` página com os parâmetros de paginação apropriado. Por exemplo, para exibir a segunda página de dados, podemos redirecionar o usuário `Paging.aspx?pageIndex=1`.

Para facilitar isso, crie uma `RedirectUser(sendUserToPageIndex)` método que redireciona o usuário para `Paging.aspx?pageIndex=sendUserToPageIndex`. Em seguida, chame esse método com o botão quatro `Click` manipuladores de eventos. No `FirstPage` `Click` manipulador de eventos, chame `RedirectUser(0)`, para enviá-los para a primeira página; na `PrevPage` `Click` manipulador de eventos, use `PageIndex - 1` como o índice de página, e assim por diante.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Com o `Click` concluir de manipuladores de eventos, os registros de DataList s podem ser paginados por meio de clicando nos botões. Dedique uns momentos para experimentar!

## <a name="disabling-paging-interface-controls"></a>Desabilitando os controles de Interface de paginação

Atualmente, todos os quatro botões são habilitados, independentemente da página que está sendo visualizada. No entanto, gostaríamos de desabilitar os botões de primeiro e anterior ao mostrar a primeira página de dados e os botões Próximo e último ao mostrar a última página. O `PagedDataSource` objeto retornado por s o ObjectDataSource `Select()` método tem propriedades [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) e [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) que podemos examinar para determinar se estamos vendo a primeira ou última página de dados.

Adicione o seguinte para o s ObjectDataSource `Selected` manipulador de eventos:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Com esse acréscimo, os botões primeiro e anterior serão desabilitados ao exibir a primeira página, enquanto os botões Próximo e último serão desabilitados ao exibir a última página.

Let s concluir a interface de paginação, informando ao usuário qual página eles re exibindo no momento e o número total de páginas existem. Adicione um controle da Web do rótulo para a página e defina suas `ID` propriedade para `CurrentPageNumber`. Defina suas `Text` propriedade no ObjectDataSource s selecionados manipulador de eventos tais que ele inclui a página atual que está sendo visualizada (`PageIndex + 1`) e o número total de páginas (`PageCount`).


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

A Figura 10 mostra `Paging.aspx` quando visitado pela primeira vez. Uma vez que a cadeia de consulta estiver vazio, o padrão é DataList mostrando os primeiros quatro produtos; os primeiro e anterior botões serão desabilitados. Clicar em Avançar exibe os próximos quatro registros (veja a Figura 11); os botões primeiro e anterior agora estão habilitados.


[![A primeira página de dados é exibido](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Figura 10**: O primeiro dos dados da página é exibido ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))


[![A segunda página de dados é exibido](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Figura 11**: O segundo dos dados da página é exibido ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))


> [!NOTE]
> A interface de paginação pode ser aprimorada ainda mais, permitindo que o usuário especifique quantas páginas para exibir por página. Por exemplo, uma DropDownList foi possível adicionar opções de tamanho de página de listagem, como 5, 10, 25, 50 e todos. Ao selecionar um tamanho de página, o usuário precisaria ser redirecionado para `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Eu deixo implementar esse aprimoramento como um exercício para o leitor.


## <a name="using-custom-paging"></a>Usando a paginação personalizada

As páginas de DataList por meio de seus dados usando a técnica de paginação padrão ineficiente. Quando a paginação por meio de suficientemente grandes quantidades de dados, é imperativo que a paginação personalizada seja usado. Embora os detalhes de implementação diferem ligeiramente, os conceitos por trás da implementação da paginação personalizada no DataList são o mesmo que paginação padrão. Com a paginação personalizada, use o `ProductBLL` classe s `GetProductsPaged` método (em vez de `GetProductsAsPagedDataSource`). Conforme discutido na [eficiente de paginação por meio de grandes quantidades de dados](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) tutorial, `GetProductsPaged` deve ser passado como o número de índice e o máximo da linha de início das linhas a serem retornadas. Esses parâmetros podem ser mantidos por meio de querystring, assim como o `pageIndex` e `pageSize` parâmetros usados em default de paginação.

Desde s não existe nenhum `PagedDataSource` com paginação personalizada, técnicas alternativas devem ser usadas para determinar o número total de registros sendo paginado por meio e se podemos re exibindo a primeira ou última página de dados. O `TotalNumberOfProducts()` método no `ProductsBLL` classe retorna o número total de pager por meio de produtos. Para determinar se a primeira página de dados estiver sendo exibida, examine o índice de linha de início se for zero, em seguida, a primeira página estiver sendo exibida. A última página estiver sendo exibida se o índice de linha de início mais o máximo de linhas para retornar é maior que ou igual ao número total de registros sendo paginado por meio do.

Vamos explorar Implementando a paginação personalizada mais detalhadamente no próximo tutorial.

## <a name="summary"></a>Resumo

Enquanto o DataList nem Repeater oferece o fora do suporte à paginação de caixa encontrado no GridView, DetailsView e FormView controla, tal funcionalidade pode ser adicionada com um mínimo de esforço. A maneira mais fácil de implementar a paginação padrão é encapsular todo o conjunto de produtos em um `PagedDataSource` e, em seguida, associar o `PagedDataSource` à DataList ou Repeater. Neste tutorial, adicionamos a `GetProductsAsPagedDataSource` método para o `ProductsBLL` classe para retornar o `PagedDataSource`. O `ProductsBLL` classe já contém os métodos necessários para a paginação personalizada `GetProductsPaged` e `TotalNumberOfProducts`.

Juntamente com recuperando o conjunto exato de registros a serem exibidos para paginação personalizada de um ou todos os registros em um `PagedDataSource` para paginação padrão, também precisamos adicionar manualmente a interface de paginação. Para este tutorial criamos um Next, Previous, First última interface com quatro controles da Web de botão. Além disso, foi adicionado um controle de rótulo exibindo o número da página atual e o número total de páginas.

No próximo tutorial, veremos como adicionar suporte à classificação à DataList e Repeater. Também veremos como criar uma DataList que pode ser paginado e classificado (com exemplos que usam o padrão e a paginação personalizada).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisores de avanço para este tutorial foram Liz Shulok, Ken Pespisa e Bernadette Leigh. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [Próximo](sorting-data-in-a-datalist-or-repeater-control-vb.md)
