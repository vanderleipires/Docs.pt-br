---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: "Paginação de dados de relatório em uma DataList ou controle repetidor (VB) | Microsoft Docs"
author: rick-anderson
description: "Enquanto o DataList nem repetidor oferta a paginação automática ou suporte à classificação, este tutorial mostra como adicionar suporte à paginação de DataList ou repetidor,..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 66f1065c41352f355dd5f1be43443165df909b93
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Dados de relatório de paginação em DataList ou controle repetidor (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) ou [baixar PDF](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Enquanto o DataList nem repetidor oferta automática paginação ou classificação de suporte, este tutorial mostra como adicionar suporte à paginação de DataList ou repetidor, que permite a exibição interfaces muito mais flexível de paginação e dados.


## <a name="introduction"></a>Introdução

Paginação e a classificação são dois recursos muito comuns ao exibir dados em um aplicativo online. Por exemplo, ao procurar manuais do ASP.NET em uma livraria online, pode haver centenas de livros, mas o relatório lista os resultados da pesquisa lista apenas dez correspondências por página. Além disso, os resultados podem ser classificados por título, preço, contagem de páginas, nome do autor e assim por diante. Conforme abordado no [paginação e classificando dados de relatório](../paging-and-sorting/paging-and-sorting-report-data-vb.md) tutorial, os controles GridView, DetailsView e FormView todos fornecem suporte interno de paginação que pode ser habilitado em escala de uma caixa de seleção. GridView também inclui suporte a classificação.

Infelizmente, nem o DataList nem repetidor oferecem paginação ou suporte de classificação automática. Neste tutorial, examinaremos como adicionar suporte à paginação de DataList ou Repetidor. Manualmente, deve criar a interface de paginação, exibir a página apropriada de registros e lembre-se a página está sendo visitada em postagens. Enquanto isso levar mais tempo e código do que com o GridView, DetailsView ou FormView, o DataList e repetidor permitem muito mais flexível de paginação e dados interfaces de exibição.

> [!NOTE]
> Este tutorial concentra-se exclusivamente no paginação. O tutorial Avançar voltaremos nossa atenção à adição de recursos de classificação.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Etapa 1: Adicionar a paginação e a classificação Tutorial páginas da Web

Antes de começar este tutorial, permitem s primeiro dedicar um tempo para adicionar as páginas ASP.NET que vamos precisar para este tutorial e o outro. Comece criando uma nova pasta no projeto chamado `PagingSortingDataListRepeater`. Em seguida, adicione as seguintes páginas ASP.NET cinco nesta pasta, ter todos os configurado para usar a página mestra `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Crie uma pasta PagingSortingDataListRepeater e adicionar as páginas ASP.NET Tutorial](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figura 1**: criar um `PagingSortingDataListRepeater` pasta e adicionar as páginas ASP.NET Tutorial


Em seguida, abra o `Default.aspx` página e arraste o `SectionLevelTutorialListing.ascx` controle de usuário da `UserControls` pasta para a superfície de Design. Este controle de usuário que criamos no [páginas mestras e navegação de Site](../introduction/master-pages-and-site-navigation-vb.md) tutorial, enumera o mapa do site e exibe os tutoriais na seção atual em uma lista com marcadores.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Figura 2**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))


Para ter a lista com marcadores exibem a paginação e a classificação tutoriais que criará, precisamos adicioná-los para o mapa do site. Abra o `Web.sitemap` de arquivo e adicione a seguinte marcação após a edição e exclusão com a marcação de nó de mapa de site DataList:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]


![Atualizar o mapa de Site para incluir as novas páginas ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Figura 3**: atualizar o mapa de Site para incluir as novas páginas ASP.NET


## <a name="a-review-of-paging"></a>Uma revisão de paginação

Nos tutoriais anteriores, vimos como página de dados em controles GridView, DetailsView e FormView. Esses três controles oferecem uma forma simple de paginação chamada *paginação padrão* que podem ser implementadas, simplesmente marcando a opção habilitar paginação na marca inteligente controle s. Com paginação padrão, cada vez que uma página de dados é solicitada na primeira página visite ou quando o usuário navega para outra página de dados do GridView, DetailsView, ou controle FormView novamente solicitações *todos os* dos dados das ObjectDataSource. Ele então recortes o conjunto específico de registros a serem exibidos considerando o índice da página solicitada e o número de registros a serem exibidos por página. Discutimos paginação padrão detalhadamente o [paginação e classificando dados de relatório](../paging-and-sorting/paging-and-sorting-report-data-vb.md) tutorial.

Como a paginação padrão novamente solicitações de todos os registros de cada página, não é prático quando a paginação suficientemente grandes quantidades de dados. Por exemplo, imagine paginação por meio de 50.000 registros com um tamanho de página de 10. Cada vez que o usuário move para uma nova página, todos os 50.000 registros devem ser recuperados do banco de dados, mesmo apenas dez deles é exibidas.

*Paginação personalizada* resolve os problemas de desempenho da paginação padrão captando somente o subconjunto preciso de registros para exibir a página solicitada. Ao implementar a paginação personalizada, podemos deve escrever a consulta SQL com eficiência retornará apenas o conjunto correto de registros. Vimos como criar essa consulta usando o SQL Server 2005 s novo [ `ROW_NUMBER()` palavra-chave](http://www.4guysfromrolla.com/webtech/010406-1.shtml) novamente o [com eficiência paginação por meio de grandes quantidades de dados](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) tutorial.

Para implementar paginação padrão nos controles DataList ou repetidor, podemos usar o [ `PagedDataSource` classe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) como um wrapper em torno de `ProductsDataTable` cujo conteúdo está sendo enviada via pager. O `PagedDataSource` classe tiver um `DataSource` propriedade que pode ser atribuída a qualquer objeto enumerável e [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) e [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) propriedades que indicam quantos registros a Mostre por página e o índice da página atual. Depois que essas propriedades foram definidas, o `PagedDataSource` pode ser usado como a fonte de dados de qualquer dado de controle da Web. O `PagedDataSource`, quando enumerada, irá retornar somente o subconjunto apropriado dos registros de seu interna `DataSource` com base no `PageSize` e `CurrentPageIndex` propriedades. A Figura 4 ilustra a funcionalidade do `PagedDataSource` classe.


![O PagedDataSource encapsula um objeto enumerável com uma Interface paginável](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Figura 4**: O `PagedDataSource` encapsular um objeto enumerável com uma Interface paginável


O `PagedDataSource` objeto pode ser criado e configurado diretamente da camada de lógica de negócios e associado a um DataList ou repetidor por meio de um ObjectDataSource, ou pode ser criada e configurada diretamente na classe por trás do código em páginas ASP.NET. Se a segunda abordagem é usada, devemos abrir mão de usar o ObjectDataSource e em vez disso, associar os dados do paginada para o DataList ou repetidor programaticamente.

O `PagedDataSource` objeto também tem propriedades para oferecer suporte a paginação personalizada. No entanto, podemos ignorar usando um `PagedDataSource` para paginação personalizada porque já temos métodos BLL no `ProductsBLL` classe projetada para paginação personalizada que retornam os registros precisos para exibir.

Neste tutorial, examinaremos implementar paginação padrão em DataList adicionando um novo método para o `ProductsBLL` classe que retorna um adequadamente configurado `PagedDataSource` objeto. O seguinte tutorial, veremos como usar a paginação personalizada.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Etapa 2: Adicionando um método de paginação padrão na camada de lógica de negócios

O `ProductsBLL` classe atualmente tem um método para retornar todas as informações de produto `GetProducts()` e outra para retornar um subconjunto específico de produtos em um índice inicial `GetProductsPaged(startRowIndex, maximumRows)`. Com paginação padrão, o GridView, DetailsView e FormView controla todo o uso de `GetProducts()` método para recuperar todos os produtos, mas, em seguida, use um `PagedDataSource` internamente para exibir somente o subconjunto correto de registros. Para replicar essa funcionalidade com os controles DataList e repetidor, podemos criar um novo método na BLL que imita esse comportamento.

Adicione um método para o `ProductsBLL` classe denominada `GetProductsAsPagedDataSource` que usa dois parâmetros de entrada de inteiro:

- `pageIndex`o índice da página para exibir, indexado em zero, e
- `pageSize`o número de registros a serem exibidos por página.

`GetProductsAsPagedDataSource`inicia, recuperando *todos os* os registros de `GetProducts()`. Ele cria um `PagedDataSource` objeto, definindo seu `CurrentPageIndex` e `PageSize` propriedades para os valores passados na `pageIndex` e `pageSize` parâmetros. O método é concluído, retornando essa configuração `PagedDataSource`:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Etapa 3: Exibindo informações de produto em DataList usando paginação padrão

Com o `GetProductsAsPagedDataSource` método adicionado para o `ProductsBLL` classe, agora podemos criar um DataList ou repetidor que fornece a paginação padrão. Comece abrindo o `Paging.aspx` página o `PagingSortingDataListRepeater` pasta e arraste uma DataList da caixa de ferramentas para o Designer, definindo o DataList s `ID` propriedade `ProductsDefaultPaging`. Marca inteligente DataList s, criar um novo ObjectDataSource denominado `ProductsDefaultPagingDataSource` e configurá-lo para que ele recupere dados usando o `GetProductsAsPagedDataSource` método.


[![Criar um ObjectDataSource e configurá-lo para usar (de) o GetProductsAsPagedDataSource método](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figura 5**: criar um ObjectDataSource e configurá-lo para usar o `GetProductsAsPagedDataSource` `()` método ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Definir a lista suspensa na atualização, inserção e excluir guias como (nenhum)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figura 6**: definir a lista suspensa na atualização, inserção e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


Desde o `GetProductsAsPagedDataSource` método espera dois parâmetros de entrada, o assistente solicitará que nós para a fonte desses valores de parâmetro.

O índice de página e os valores de tamanho de página devem ser lembrados entre postbacks. Eles podem ser armazenados em estado de exibição, persistidos querystring, armazenados em variáveis de sessão ou lembradas usando outra técnica. Para este tutorial, usaremos querystring, que tem a vantagem de permitir que uma determinada página de dados a ser marcadas.

Em particular, use o querystring campos pageIndex e pageSize para o `pageIndex` e `pageSize` parâmetros, respectivamente (consulte a Figura 7). Dedique alguns momentos para definir os valores padrão para esses parâmetros, como os valores de querystring ganhas t estar presente quando um usuário acessa primeiro nessa página. Para `pageIndex`, defina o valor padrão como 0 (que será exibida a primeira página de dados) e `pageSize` valor padrão de 4.


[![Usar QueryString como a origem para os parâmetros de pageIndex e pageSize](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figura 7**: usar QueryString como a origem para o `pageIndex` e `pageSize` parâmetros ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))


Depois de configurar o ObjectDataSource, o Visual Studio cria automaticamente um `ItemTemplate` para DataList. Personalizar o `ItemTemplate` para que somente o nome do produto s, categoria e fornecedor são mostrados. Também definir DataList s `RepeatColumns` propriedade para 2, seu `Width` para 100% e sua `ItemStyle` s `Width` 50%. Essas configurações de largura fornecerá espaçamento igual para as duas colunas.

Depois de fazer essas alterações, a marcação de s DataList e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Desde que não estão executando qualquer atualização ou excluir funcionalidade neste tutorial, você poderá desabilitar o estado de exibição de s DataList para reduzir o tamanho de página renderizada.


Quando inicialmente visita a esta página por meio de um navegador, nem o `pageIndex` nem `pageSize` parâmetros querystring são fornecidos. Portanto, são usados os valores padrão de 0 e 4. Como mostra a Figura 8, isso resulta em DataList que exibe os quatro primeiros produtos.


[![Os primeiros quatro produtos listados](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Figura 8**: O primeiro quatro produtos listados ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))


Sem uma interface de paginação, há s simples no momento não significa que um usuário navegar para a segunda página de dados. Vamos criar uma interface de paginação na etapa 4. Por enquanto, porém, paginação só pode ser realizada especificando os critérios de paginação diretamente na querystring. Por exemplo, para exibir a segunda página, altere a URL na barra de endereços do navegador s do `Paging.aspx` para `Paging.aspx?pageIndex=2` e pressione Enter. Isso faz com que a segunda página de dados a serem exibidos (consulte a Figura 9).


[![A segunda página de dados é exibido](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Figura 9**: A página de dados secundário é exibido ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Etapa 4: Criar a Interface de paginação

Há uma variedade de interfaces de paginação diferentes que podem ser implementadas. Os controles GridView, DetailsView e FormView fornecem quatro interfaces diferentes para escolher entre:

- **Em seguida, anterior** os usuários podem mover uma página por vez, para um próximo ou anterior.
- **Em seguida, anterior, primeiro, último** além dos botões Próximo e anterior, essa interface inclui botões primeiro e último para mover para a primeira ou última página.
- **Numérico** lista os números de página na interface de paginação, permitindo que um usuário ir rapidamente para uma determinada página.
- **Numérico, primeiro, último** além dos números de páginas numéricas, inclui botões para mover para a primeira ou última página.

Para o DataList e repetidor, estamos responsáveis por decidir por uma interface de paginação e implementá-lo. Isso envolve a criação de controles da Web necessárias na página e exibindo a página solicitada quando um determinado botão de interface de paginação é clicado. Além disso, alguns controles de interface de paginação talvez precise ser desabilitado. Por exemplo, ao exibir a primeira página de dados usando o próximo, anterior, primeiro, última interface, o primeiro e o anterior botões deve ser desabilitada.

Para este tutorial, o uso de s permitem que o próximo, anterior, primeiro, última interface. Adicione quatro controles de botão Web para a página e defina suas `ID` s para `FirstPage`, `PrevPage`, `NextPage`, e `LastPage`. Definir o `Text` propriedades &lt; &lt; primeiro, &lt; anterior, ao lado &gt;e o último &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Em seguida, crie um `Click` manipulador de eventos para cada um desses botões. Em alguns momentos, adicionaremos o código necessário para exibir a página solicitada.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Lembre-se o número Total de registros sendo enviada via pager por meio de

Independentemente da interface de paginação selecionada, precisamos calcular e lembre-se o número total de registros sendo enviada via pager por meio de. O número total de linhas (em conjunto com o tamanho da página) determina a quantidade total de páginas de dados está sendo enviada via pager, que determina quais controles de interface de paginação são adicionados ou estão habilitados. No próximo, anterior, primeiro, interface última que estamos criando, a contagem de páginas é usado de duas maneiras:

- Para determinar se estamos estiver exibindo a última página, caso em que os botões Próximo e último estão desabilitados.
- Se o usuário clica no botão de última que precisamos whisk-los para a última contagem de página, cujo índice é um menor do que a página.

A contagem de páginas é calculada como o limite da contagem total de linhas dividida pelo tamanho da página. Por exemplo, se nós são paginação 79 registros com quatro por página, em seguida, a contagem de páginas é de 20 (o limite de 79 / 4). Se estamos usando a interface de paginação numérico, essas informações nos informam sobre a quantidade de botões de páginas numéricas para exibir; Se nossa interface paginação inclui botões de próximo ou último, a contagem de páginas é usada para determinar quando deve desabilitar os botões Próximo ou último.

Se a interface de paginação inclui um botão de última, é essencial que o número total de registros sendo enviada via pager a ser lembradas em postagens, para que quando o último botão é clicado podemos determinar o último índice de página. Para facilitar isso, crie um `TotalRowCount` propriedade na classe ASP.NET páginas de lógica que mantém seu valor para o estado de exibição:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Além `TotalRowCount`, reserve um minuto para criar as propriedades de nível de página somente leitura para acessar facilmente o índice de página, o tamanho da página e a contagem de páginas:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Determinando o número Total de registros sendo enviada via pager por meio de

O `PagedDataSource` objeto retornado de s ObjectDataSource `Select()` método tem nele *todos os* dos registros de produto, mesmo que apenas um subconjunto delas são exibidas em DataList. O `PagedDataSource` s [ `Count` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) retorna somente o número de itens que serão exibidos em DataList; o [ `DataSourceCount` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) retorna o número total de itens dentro de `PagedDataSource`. Portanto, é preciso atribuir a páginas do ASP.NET `TotalRowCount` o valor da propriedade do `PagedDataSource` s `DataSourceCount` propriedade.

Para fazer isso, crie um manipulador de eventos para o s ObjectDataSource `Selected` eventos. No `Selected` manipulador de eventos, temos acesso ao valor de retorno da s ObjectDataSource `Select()` método nesse caso, o `PagedDataSource`.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Exibindo a página solicitada de dados

Quando o usuário clica em um dos botões na interface de paginação, é necessário exibir a página solicitada de dados. Como os parâmetros de paginação são especificados por meio de querystring, para mostrar a página solicitada de dados de uso `Response.Redirect(url)` para que o usuário s navegador solicitar novamente o `Paging.aspx` página com os parâmetros apropriados de paginação. Por exemplo, para exibir a segunda página de dados, podemos redirecionar o usuário `Paging.aspx?pageIndex=1`.

Para facilitar isso, crie um `RedirectUser(sendUserToPageIndex)` método que redireciona o usuário para `Paging.aspx?pageIndex=sendUserToPageIndex`. Em seguida, chame este método do botão quatro `Click` manipuladores de eventos. No `FirstPage` `Click` manipulador de eventos, chamada `RedirectUser(0)`, enviá-los para a primeira página; o `PrevPage` `Click` manipulador de eventos, use `PageIndex - 1` como o índice de página, e assim por diante.


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Com o `Click` manipuladores de eventos concluído, os registros de DataList s podem ser paginados por meio de clicando nos botões. Reserve um tempo para Experimente!

## <a name="disabling-paging-interface-controls"></a>Desabilitando os controles de Interface de paginação

Todos os quatro botões são habilitados no momento, independentemente da página que está sendo exibida. No entanto, desejamos desabilitar os botões primeiro e anterior ao mostrar a primeira página de dados e os botões Próximo e último ao mostrar a última página. O `PagedDataSource` objeto retornado pela s ObjectDataSource `Select()` método tem propriedades [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) e [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) que podemos examinar para determinar se estamos vendo a primeira ou última página de dados.

Adicione o seguinte para o s ObjectDataSource `Selected` manipulador de eventos:


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Com essa adição, os botões primeiro e anterior serão desabilitados ao exibir a primeira página, enquanto os botões Próximo e último serão desabilitados ao exibir a última página.

Permitem s concluir a interface de paginação, informando ao usuário página o que eles re exibindo no momento e o número total de páginas existem. Adicionar um controle da Web de rótulo para a página e definir seu `ID` propriedade `CurrentPageNumber`. Definir seu `Text` propriedade em ObjectDataSource s selecionados manipulador de eventos tais que inclui a página atual que está sendo exibida (`PageIndex + 1`) e o número total de páginas (`PageCount`).


[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

A Figura 10 mostra `Paging.aspx` quando visitado primeiro. Desde que a querystring é vazia, o padrão é DataList mostrando os quatro primeiros produtos; os botões primeiro e anterior são desabilitados. Clicar em Avançar exibe os registros de quatro (consulte a Figura 11); os botões primeiro e anterior agora estão habilitados.


[![A primeira página de dados é exibido](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Figura 10**: O primeiro dos dados da página é exibido ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))


[![A segunda página de dados é exibido](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Figura 11**: A página de dados secundário é exibido ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))


> [!NOTE]
> A interface de paginação pode ser aprimorada ainda mais, permitindo que o usuário especifique quantas páginas para ler por página. Por exemplo, DropDownList foi possível adicionar opções de tamanho de página de listagem como 5, 10, 25, 50 e todos. Ao selecionar um tamanho de página, o usuário precisa ser redirecionado para `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Posso deixar implementar esse aprimoramento como um exercício para o leitor.


## <a name="using-custom-paging"></a>Usar a paginação personalizada

As páginas de DataList por meio de seus dados usando a técnica de paginação padrão ineficiente. Quando a paginação suficientemente grandes quantidades de dados, é essencial que a paginação personalizada seja usado. Embora os detalhes de implementação são um pouco diferem, os conceitos por trás de implementar paginação personalizada em DataList são iguais às paginação padrão. Com a paginação personalizada, use o `ProductBLL` classe s `GetProductsPaged` método (em vez de `GetProductsAsPagedDataSource`). Como discutido o [com eficiência paginação por meio de grandes quantidades de dados](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) tutorial, `GetProductsPaged` deve ser passado como o número de índice e o máximo da linha de início das linhas a serem retornadas. Esses parâmetros podem ser mantidos por meio de querystring assim como o `pageIndex` e `pageSize` parâmetros usados no padrão de paginação.

Desde s não há nenhum `PagedDataSource` com paginação personalizada, técnicas alternativas devem ser usadas para determinar o número total de registros sendo enviada via pager com e se é re exibindo a primeira ou última página de dados. O `TotalNumberOfProducts()` método o `ProductsBLL` classe retorna o número total de produtos por meio de pager. Para determinar se a primeira página de dados estiver sendo exibida, examine o índice de linha inicial se for zero, em seguida, a primeira página estiver sendo exibida. A última página estiver sendo exibida se o índice de linha inicial e o máximo de linhas para retornar é maior que ou igual ao número total de registros sendo enviada via pager por meio de.

Vamos explorar implementar paginação personalizada mais detalhadamente no tutorial Avançar.

## <a name="summary"></a>Resumo

Enquanto o DataList nem repetidor oferece o fora de encontrado suporte a paginação em GridView, DetailsView e FormView controla, essa funcionalidade pode ser adicionada com um mínimo de esforço. A maneira mais fácil de implementar paginação padrão é encapsular todo o conjunto de produtos de uma `PagedDataSource` e, em seguida, associar a `PagedDataSource` ao DataList ou Repetidor. Neste tutorial, adicionamos a `GetProductsAsPagedDataSource` método para o `ProductsBLL` classe para retornar o `PagedDataSource`. O `ProductsBLL` classe já contém os métodos necessários para paginação personalizada `GetProductsPaged` e `TotalNumberOfProducts`.

Junto com a recuperar o conjunto exato de registros a serem exibidos para paginação personalizada de um ou todos os registros em um `PagedDataSource` para paginação padrão, também precisamos adicionar manualmente a interface de paginação. Para este tutorial criamos um próximo, anterior, primeiro, última interface com quatro controles da Web de botão. Além disso, foi adicionado um controle de rótulo exibindo o número da página atual e o número total de páginas.

O tutorial Avançar veremos como adicionar suporte à classificação de DataList e Repetidor. Também veremos como criar uma DataList que pode ser paginada e classificada (com exemplos que usam o padrão e a paginação personalizada).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Liz Shulok, Ken Pespisa e Bernadette Leigh. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](sorting-data-in-a-datalist-or-repeater-control-cs.md)
[Próximo](sorting-data-in-a-datalist-or-repeater-control-vb.md)
