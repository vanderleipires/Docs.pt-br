---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: Classificando dados em um DataList ou controle repetidor (VB) | Microsoft Docs
author: rick-anderson
description: "Neste tutorial, examinaremos como incluir suporte nos DataList e repetidor de classificação, e também como construir um DataList ou repetidor cujos dados podem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 0133a74454a7754f4f7087e2121c7387a1aef8a8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>Classificando dados em um controle de Repetidor (VB) ou de DataList
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe) ou [baixar PDF](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> Neste tutorial, examinaremos como incluir suporte nos DataList e repetidor de classificação, e também como construir um DataList ou repetidor cujos dados podem ser paginados e classificados.


## <a name="introduction"></a>Introdução

No [tutorial anterior](paging-report-data-in-a-datalist-or-repeater-control-vb.md) examinamos como adicionar suporte à paginação de DataList. Criamos um novo método no `ProductsBLL` classe (`GetProductsAsPagedDataSource`) que retornaram um `PagedDataSource` objeto. Quando associado a um DataList ou repetidor, DataList ou do repetidor exibirá apenas a página solicitada de dados. Essa técnica é semelhante à que é usada internamente pelos controles GridView, DetailsView e FormView para fornecer a funcionalidade de paginação padrão interna.

Além de oferecer suporte à paginação, GridView também inclui fora da caixa de classificação de suporte. O DataList nem repetidor fornece funcionalidade interna de classificação; No entanto, os recursos de classificação pode ser adicionada com um trecho de código. Neste tutorial, examinaremos como incluir suporte nos DataList e repetidor de classificação, e também como construir um DataList ou repetidor cujos dados podem ser paginados e classificados.

## <a name="a-review-of-sorting"></a>Uma revisão de classificação

Como vimos no [paginação e classificando dados de relatório](../paging-and-sorting/paging-and-sorting-report-data-vb.md) tutorial, o controle GridView fornece classificação suporte pronto para uso. Cada campo de GridView pode ter um tipo de `SortExpression`, que indica que o campo de dados pela qual classificar os dados. Quando o GridView s `AllowSorting` está definida como `true`, cada campo de GridView que tem um `SortExpression` o valor da propriedade tem seu cabeçalho renderizado como um LinkButton. Quando um usuário clica em um cabeçalho de s de campo específico do GridView, ocorre um postback e os dados são classificados de acordo com o campo clicado s `SortExpression`.

O controle GridView possui um `SortExpression` propriedade, que armazena o `SortExpression` do campo GridView os dados são classificados por. Além disso, um `SortDirection` propriedade indica se os dados a ser classificada em ordem crescente ou decrescente (se um usuário clica em um link do cabeçalho campo s particular GridView duas vezes em sucessão, a ordem de classificação é alternado).

Quando o GridView está associado a seu controle de fonte de dados, ele transmite seu `SortExpression` e `SortDirection` propriedades para os dados de controle de origem. O controle de fonte de dados recupera os dados e, em seguida, classifica os dados conforme fornecido `SortExpression` e `SortDirection` propriedades. Depois de classificar os dados, o controle de fonte de dados retorna-a GridView.

Para replicar essa funcionalidade com os controles DataList ou repetidor, devemos:

- Criar uma interface de classificação
- Lembre-se o campo de dados para classificar por e se deseja classificar em ordem crescente ou decrescente
- Instrua o ObjectDataSource para classificar os dados por um campo de dados específico

Que abordaremos esses três tarefas nas etapas 3 e 4. Depois disso, examinaremos como incluir paginação e classificação suporte em DataList ou Repetidor.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Etapa 2: Exibir os produtos em um repetidor

Antes de nos preocupamos com implementação da funcionalidade de classificação relacionadas, deixe-s relacionar os produtos em um controle Repetidor. Comece abrindo o `Sorting.aspx` página o `PagingSortingDataListRepeater` pasta. Adicionar um controle repetidor a página da web, definindo seu `ID` propriedade `SortableProducts`. Marca inteligente repetidor s, criar um novo ObjectDataSource denominado `ProductsDataSource` e configurá-lo para recuperar dados a `ProductsBLL` classe s `GetProducts()` método. Selecione a que opção nas listas suspensas nas guias de INSERT, UPDATE e DELETE (nenhum).


[![Criar um ObjectDataSource e configurá-lo para usar o método GetProductsAsPagedDataSource()](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figura 1**: criar um ObjectDataSource e configurá-lo para usar o `GetProductsAsPagedDataSource()` método ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))


[![Definir a lista suspensa na atualização, inserção e excluir guias como (nenhum)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**Figura 2**: definir a lista suspensa na atualização, inserção e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))


Ao contrário de com DataList, Visual Studio não cria automaticamente um `ItemTemplate` para o controle repetidor após a associação a uma fonte de dados. Além disso, devemos adicionar isso `ItemTemplate` declarativamente, como a marca inteligente de controle s repetidor não tem a opção de editar modelos encontrada em DataList s. S de permitem usar o mesmo `ItemTemplate` do tutorial anterior, que exibido o nome do produto s, fornecedor e categoria.

Depois de adicionar o `ItemTemplate`, a marcação de declarativa s repetidor e ObjectDataSource deve ser semelhante à seguinte:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

A Figura 3 mostra essa página quando visualizada através de um navegador.


[![Cada produto s nome, fornecedor e a categoria é exibido](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figura 3**: cada produto s nome, fornecedor e categoria é exibida ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Etapa 3: Instruindo ObjectDataSource para classificar os dados

Para classificar os dados exibidos no repetidor, é necessário informar o ObjectDataSource da expressão de classificação pelo qual os dados devem ser classificados. Antes do ObjectDataSource recupera seus dados, primeiro dispara seu [ `Selecting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), que fornece uma oportunidade especificar uma expressão de classificação. O `Selecting` manipulador de eventos é passado um objeto do tipo [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), que tem uma propriedade chamada [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) do tipo [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). O `DataSourceSelectArguments` classe é projetado para passar solicitações relacionadas a dados de um consumidor de dados para o controle de fonte de dados e inclui uma [ `SortExpression` propriedade](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Para transmitir informações de classificação de página ASP.NET para ObjectDataSource, criar um manipulador de eventos para o `Selecting` evento e use o código a seguir:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

O *sortExpression* valor deve ser atribuído como o nome do campo de dados para classificar os dados (como ProductName). Não há nenhuma propriedade relacionadas a direção de classificação, então se você deseja classificar os dados em ordem decrescente, acrescente a cadeia de caracteres DESC para o *sortExpression* valor (como ProductName DESC).

Vá em frente e experimente alguns valores embutidos diferentes para *sortExpression* e testar os resultados em um navegador. Como mostra a Figura 4, ao usar ProductName DESC como o *sortExpression*, os produtos são classificados por nome em ordem alfabética inversa.


[![Os produtos são classificados por nome em ordem alfabética inversa](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figura 4**: os produtos são classificados por nome em ordem alfabética inversa ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Etapa 4: Criar a Interface de classificação e lembre-se a expressão de classificação e a direção

Ativar a classificação de suporte em GridView converte cada texto de cabeçalho do campo classificável s em um LinkButton que, quando clicado, classifica os dados adequadamente. Essa é uma interface classificação faz sentido do GridView, onde seus dados é claramente dispostos em colunas. Para os controles DataList e repetidor, no entanto, é necessária uma interface de classificação diferente. Uma interface comum de classificação para obter uma lista de dados (em vez de uma grade de dados), é uma lista suspensa que fornece os campos pelos quais os dados podem ser classificados. Permitir que o s implementar tal interface para este tutorial.

Adicionar um controle de DropDownList Web acima de `SortableProducts` repetidor e defina seu `ID` propriedade `SortBy`. Na janela Propriedades, clique nas reticências no `Items` propriedade para trazer o ListItem Collection Editor. Adicionar `ListItem` s para classificar os dados de `ProductName`, `CategoryName`, e `SupplierName` campos. Também adicionar uma `ListItem` para classificar os produtos por seu nome em ordem alfabética inversa.

O `ListItem` `Text` propriedades podem ser definidas para qualquer valor (como nome), mas o `Value` propriedades devem ser definidas para o nome do campo de dados (como ProductName). Para classificar os resultados em ordem decrescente, acrescente DESC a cadeia de caracteres para o nome do campo de dados, como ProductName DESC.


![Adicionar um item de lista para cada um dos campos de dados classificável](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figura 5**: adicionar um `ListItem` para cada um dos campos de dados classificável


Finalmente, adicione um controle da Web do botão à direita do DropDownList. Definir seu `ID` para `RefreshRepeater` e sua `Text` propriedade para atualização.

Depois de criar o `ListItem` s e adicionar o botão de atualização, a sintaxe de declarativa s DropDownList e botão deve ser semelhante ao seguinte:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

Com a classificação DropDownList concluída, em seguida precisamos atualizar o s ObjectDataSource `Selecting` manipulador de eventos para que ele usa selecionado `SortBy``ListItem` s `Value` propriedade em vez de uma expressão de classificação embutido.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

Neste ponto, quando o primeiro visitar a página produtos inicialmente serão classificados pelo `ProductName` campo de dados, como s a `SortBy` `ListItem` selecionada por padrão (veja a Figura 6). Selecionando um outra opção, como a categoria de classificação e clicando em Atualizar causar um postback e reclassifica os dados pelo nome da categoria, como mostra a Figura 7.


[![Os produtos são inicialmente classificadas por seu nome](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**Figura 6**: os produtos são inicialmente classificadas por seu nome ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))


[![Os produtos são agora são classificados por categoria](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**Figura 7**: os produtos são agora são classificados por categoria ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))


> [!NOTE]
> Clicar no botão de atualização faz com que os dados automaticamente ser reclassificada porque o estado de exibição do repetidor s foi desabilitado, causando repetidor associar novamente a fonte de dados em cada postback. Se você salvar o estado de exibição do repetidor s ativado, alterar a classificação suspensa lista ganha t tenha qualquer efeito sobre a ordem de classificação. Para corrigir isso, crie um manipulador de eventos para o botão Atualizar s `Click` eventos e religue repetidor para sua fonte de dados (chamando repetidor s `DataBind()` método).


## <a name="remembering-the-sort-expression-and-direction"></a>Lembre-se a expressão de classificação e a direção

Ao criar um classificável DataList ou repetidor em uma página em que a classificação não relacionados postbacks poderão ocorrer, ele imperativo s que a expressão de classificação e a direção ser lembradas entre postbacks. Por exemplo, imagine que atualizamos repetidor deste tutorial para incluir um botão de exclusão com cada produto. Quando o usuário clica no botão Excluir d executamos um código para excluir o produto selecionado e, em seguida, associar novamente os dados a serem Repetidor. Se os detalhes de classificação não são persistidos em postback, os dados exibidos na tela serão revertido para a ordem de classificação originais.

Para este tutorial, DropDownList implicitamente salva a classificação expressão e a direção em seu estado de exibição para nós. Se foram usando uma interface de classificação diferente, uma com botões de link digamos, que forneceu as várias opções de classificação d precisamos tomar o cuidado de lembrar a ordem de classificação entre postbacks. Isso pode ser feito ao armazenar os parâmetros de classificação no estado de exibição de página s, incluindo o parâmetro de classificação na querystring ou por meio de outra técnica de persistência de estado.

Exemplos de futuras neste tutorial exploram como manter os detalhes de classificação no estado de exibição de página s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Etapa 5: Adicionando a classificação de suporte para uma DataList que usa paginação padrão

No [tutorial anterior](paging-report-data-in-a-datalist-or-repeater-control-vb.md) examinamos como implementar paginação padrão com DataList. Permitir que o s estender esse exemplo anterior para incluir a capacidade de classificar os dados paginados. Comece abrindo o `SortingWithDefaultPaging.aspx` e `Paging.aspx` páginas o `PagingSortingDataListRepeater` pasta. Do `Paging.aspx` , clique no botão fonte para exibir a marcação de página s declarativa. Copie o texto selecionado (consulte a Figura 8) e cole-o na marcação declarativa de `SortingWithDefaultPaging.aspx` entre o `<asp:Content>` marcas.


[![Replicar a marcação declarativa no &lt;asp: Content&gt; marcas de Paging.aspx SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**Figura 8**: replicar a marcação declarativa no `<asp:Content>` marcas de `Paging.aspx` para `SortingWithDefaultPaging.aspx` ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))


Depois de copiar a marcação declarativa, copie os métodos e propriedades no `Paging.aspx` página classe code-behind de s para a classe code-behind para `SortingWithDefaultPaging.aspx`. Em seguida, reserve um tempo para exibir o `SortingWithDefaultPaging.aspx` página em um navegador. Ele deve apresentar a mesma funcionalidade e aparência como `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Aprimorando ProductsBLL para incluir um padrão de paginação e método de classificação

No tutorial anterior criamos um `GetProductsAsPagedDataSource(pageIndex, pageSize)` método o `ProductsBLL` classe que retornou um `PagedDataSource` objeto. Isso `PagedDataSource` objeto foi preenchido com *todos os* dos produtos (via o s BLL `GetProducts()` método), mas quando associado a DataList somente os registros correspondentes especificado *pageIndex* e *pageSize* parâmetros de entrada foram exibidos.

Anteriormente neste tutorial, adicionamos suporte à classificação, especificando a expressão de classificação de s ObjectDataSource `Selecting` manipulador de eventos. Isso funciona bem quando o ObjectDataSource é retornado um objeto que pode ser classificado, como o `ProductsDataTable` retornado pelo `GetProducts()` método. No entanto, o `PagedDataSource` objeto retornado pelo `GetProductsAsPagedDataSource` método não dá suporte à classificação de sua fonte de dados interna. Em vez disso, é preciso classificar os resultados retornados pelo `GetProducts()` método *antes de* colocamos `PagedDataSource`.

Para fazer isso, crie um novo método no `ProductsBLL` classe `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Para classificar o `ProductsDataTable` retornado pelo `GetProducts()` método, especifique o `Sort` propriedade padrão `DataTableView`:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

O `GetProductsSortedAsPagedDataSource` método apenas difere um pouco do `GetProductsAsPagedDataSource` método criado no tutorial anterior. Em particular, `GetProductsSortedAsPagedDataSource` aceita um parâmetro de entrada adicional `sortExpression` e atribui esse valor para o `Sort` propriedade o `ProductDataTable` s `DefaultView`. Algumas linhas de código mais tarde, o `PagedDataSource` s fonte de dados de objeto é atribuído a `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Chamando o método GetProductsSortedAsPagedDataSource e especificando o valor para o parâmetro de entrada SortExpression

Com o `GetProductsSortedAsPagedDataSource` método concluído, a próxima etapa é fornecer o valor para esse parâmetro. ObjectDataSource em `SortingWithDefaultPaging.aspx` está configurado para chamar o `GetProductsAsPagedDataSource` método e passa a dois parâmetros de entrada por meio de dois `QueryStringParameters`, que são especificados a `SelectParameters` coleção. Esses dois `QueryStringParameters` indicar que a origem para o `GetProductsAsPagedDataSource` método s *pageIndex* e *pageSize* parâmetros vêm dos campos querystring `pageIndex` e `pageSize`.

Atualizar o s ObjectDataSource `SelectMethod` propriedade para que ele invoca o novo `GetProductsSortedAsPagedDataSource` método. Em seguida, adicione um novo `QueryStringParameter` para que o *sortExpression* parâmetro de entrada é acessado a partir do campo querystring `sortExpression`. Definir o `QueryStringParameter` s `DefaultValue` para ProductName.

Após essas alterações, a marcação declarativa de s ObjectDataSource deve parecer com:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

Neste ponto, o `SortingWithDefaultPaging.aspx` página classificará os resultados em ordem alfabética pelo nome do produto (consulte a Figura 9). Isso ocorre porque, por padrão, um valor de ProductName é passado como o `GetProductsSortedAsPagedDataSource` método s *sortExpression* parâmetro.


[![Por padrão, os resultados são classificados por ProductName](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**Figura 9**: por padrão, os resultados são classificados por `ProductName` ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))


Se você adicionar manualmente um `sortExpression` campo querystring como `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` os resultados serão classificados por especificado `sortExpression`. No entanto, isso `sortExpression` parâmetro não for incluído na querystring ao passar para outra página de dados. Na verdade, clicando na Avançar ou na última página botões usa nós de volta para `Paging.aspx`! Além disso, s há atualmente nenhuma classificação de interface. É a única maneira de que um usuário pode alterar a ordem de classificação dos dados pagináveis manipulando querystring diretamente.

## <a name="creating-the-sorting-interface"></a>Criar a Interface de classificação

Primeiro, precisamos atualizar o `RedirectUser` para enviar ao usuário `SortingWithDefaultPaging.aspx` (em vez de `Paging.aspx`) e incluir o `sortExpression` valor na querystring. Podemos também deve adicionar um somente leitura, nível de página chamado `SortExpression` propriedade. Essa propriedade, como o `PageIndex` e `PageSize` propriedades criadas no tutorial anterior, retorna o valor da `sortExpression` campo querystring se ele existe e o padrão de valor (ProductName) caso contrário.

Atualmente o `RedirectUser` método aceita apenas um único parâmetro de entrada de índice da página para exibir. No entanto, pode haver ocasiões em que deseja redirecionará o usuário para uma página específica de dados usando uma expressão de classificação que não seja o que é especificado na querystring. Em alguns momentos, criaremos a interface de classificação para essa página, que inclui uma série de controles da Web de botão de classificação dos dados por uma coluna especificada. Quando um desses botões é clicado, queremos redirecionará o usuário passar o valor da expressão de classificação apropriado. Para fornecer essa funcionalidade, crie duas versões do `RedirectUser` método. O primeiro deles deve aceitar apenas o índice de página para exibir, enquanto a segunda aceita a expressão de índice e de classificação de página.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

O primeiro exemplo neste tutorial, criamos uma interface de classificação usando DropDownList. Para este exemplo, permitem s usar três controles de botão Web posicionados acima do DataList uma classificação por `ProductName`, um para `CategoryName`e outra para `SupplierName`. Adicione os três controles da Web de botão, definindo suas `ID` e `Text` propriedades adequadamente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

Em seguida, crie um `Click` manipulador de eventos para cada um. Os manipuladores de eventos devem chamar o `RedirectUser` método, retornando o usuário para a primeira página usando a expressão de classificação apropriado.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Quando o primeiro visitando a página, os dados são classificados em ordem alfabética pelo nome do produto (consulte novamente a Figura 9). Clique no botão Avançar para ir para a segunda página de dados e clique em Classificar por botão de categoria. Isso retorna para a primeira página de dados, classificados por nome de categoria (consulte a Figura 10). Da mesma forma, a clicando em Classificar por botão Supplier classifica os dados por fornecedor a partir da primeira página de dados. A opção de classificação será lembrada conforme os dados são paginados por meio de. Figura 11 mostra a página após a classificação por categoria e, em seguida, aprimorando a décimo terceiro página de dados.


[![Os produtos são classificados por categoria](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**Figura 10**: os produtos são classificados por categoria ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))


[![A expressão de classificação é lembradas quando paginação por meio de dados do](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**Figura 11**: A expressão de classificação é lembradas quando paginação por meio de dados do ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Etapa 6: A paginação personalizada por meio de registros em um repetidor

O exemplo de DataList examinadas na etapa 5 páginas por meio de seus dados usando a técnica de paginação padrão ineficiente. Quando a paginação suficientemente grandes quantidades de dados, é essencial que a paginação personalizada seja usado. No [com eficiência por meio de grandes quantidades de dados de paginação](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) e [classificação personalizada paginável dados](../paging-and-sorting/sorting-custom-paged-data-vb.md) tutoriais, examinamos as diferenças entre padrão e paginação personalizada e métodos criados na BLL para utilizando personalizado de paginação e classificando dados pagináveis personalizados. Em particular, esses dois tutoriais anteriores, adicionamos os três métodos a seguir para o `ProductsBLL` classe:

- `GetProductsPaged(startRowIndex, maximumRows)`Retorna um subconjunto específico de registros que começam em *startRowIndex* e não exceda *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)`Retorna um subconjunto específico de registros classificados por especificado *sortExpression* parâmetro de entrada.
- `TotalNumberOfProducts()`fornece o número total de registros de `Products` tabela de banco de dados.

Esses métodos podem ser usados para a página e classificar dados usando um controle DataList ou repetidor com eficiência. Para ilustrar isso, permitem s comece criando um controle repetidor com suporte à paginação personalizada; em seguida, adicionaremos os recursos de classificação.

Abra o `SortingWithCustomPaging.aspx` página o `PagingSortingDataListRepeater` pasta e adicionar um repetidor para a página configuração seu `ID` propriedade `Products`. Marca inteligente repetidor s, criar um novo ObjectDataSource denominado `ProductsDataSource`. Configurá-lo para selecionar seus dados a partir de `ProductsBLL` classe s `GetProductsPaged` método.


[![Configurar o ObjectDataSource para usar o método de GetProductsPaged ProductsBLL classe s](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**Figura 12**: configurar o ObjectDataSource para usar o `ProductsBLL` classe s `GetProductsPaged` método ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))


Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum) e, em seguida, clique no botão Avançar. O Assistente Configurar fonte de dados agora solicita as fontes a `GetProductsPaged` método s *startRowIndex* e *maximumRows* parâmetros de entrada. Na realidade, esses parâmetros de entrada são ignorados. Em vez disso, o *startRowIndex* e *maximumRows* valores serão transmitidos no `Arguments` propriedade em ObjectDataSource s `Selecting` manipulador de eventos, assim como especificamos o *sortExpression* nesta demonstração primeiro tutorial s. Portanto, deixe a origem do parâmetro listas suspensas no assistente definido em nenhum.


[![Deixe o conjunto de fontes de parâmetro para nenhum](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**Figura 13**: deixar as fontes de parâmetro definido como None ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))


> [!NOTE]
> Fazer *não* definir o s ObjectDataSource `EnablePaging` propriedade `true`. Isso fará com que o ObjectDataSource incluir automaticamente seu próprio *startRowIndex* e *maximumRows* parâmetros para o `SelectMethod` s lista existente de parâmetro. O `EnablePaging` propriedade é útil quando a associação personalizada paginável dados a um controle GridView, DetailsView ou FormView porque esses controles esperam determinado comportamento de ObjectDataSource que s disponível apenas quando `EnablePaging` é de propriedade `true`. Como é necessário adicionar manualmente o suporte à paginação para o DataList e repetidor, deixe essa propriedade definida como `false` (o padrão), como podemos será implantado na funcionalidade necessária diretamente em nossa página do ASP.NET.


Finalmente, defina o repetidor s `ItemTemplate` para que o nome do produto s, categoria e fornecedor são mostrados. Após essas alterações, a sintaxe de declarativa s repetidor e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

Dedicar um tempo para visitar a página por meio de um navegador e observe que os registros não são retornados. Isso é porque nós ve ainda para especificar o *startRowIndex* e *maximumRows* valores de parâmetros; portanto, valores de 0 estão sendo passados para ambos. Para especificar esses valores, crie um manipulador de eventos para o s ObjectDataSource `Selecting` evento e defina esses parâmetros programaticamente valores para valores codificados de 0 a 5, respectivamente:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

Com essa alteração, a página, quando visualizada através de um navegador, mostra os cinco primeiros produtos.


[![Os primeiros cinco registros são exibidos](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**Figura 14**: O primeiro cinco registros são exibidos ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))


> [!NOTE]
> Os produtos listados na Figura 14 acontecem sejam classificados por nome do produto porque a `GetProductsPaged` procedimento armazenado que executa a consulta de paginação personalizada eficiente ordena os resultados por `ProductName`.


Para permitir que o usuário navegue pelas páginas, precisamos controlar o índice de linha inicial e o número máximo de linhas e memorizar esses valores entre postbacks. O exemplo de paginação padrão usamos querystring campos para manter esses valores; para esta demonstração, permitem s manter essas informações no estado de exibição de página s. Crie as duas propriedades a seguir:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

Em seguida, atualize o código no manipulador de eventos de selecionar para que ele usa o `StartRowIndex` e `MaximumRows` propriedades em vez de valores codificados de 0 a 5:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

Neste ponto nossa página ainda mostra apenas os cinco primeiros registros. No entanto, com essas propriedades em vigor, podemos re pronto para criar nossa interface de paginação.

## <a name="adding-the-paging-interface"></a>Adicionando a Interface de paginação

S permitem que usam o mesmo primeiro, anterior, Avançar, paginação última interface usada no exemplo de paginação padrão, incluindo o Web rótulo de controle que exibe qual página de dados estiver sendo exibido e o número total de páginas existem. Adicione os quatro controles da Web de botão e rótulo abaixo Repetidor.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

Em seguida, crie `Click` manipuladores de eventos para os quatro botões. Quando um desses botões é clicado, precisamos atualizar o `StartRowIndex` e associar novamente os dados a serem Repetidor. O código para os botões primeiro, anterior e próxima é simple o suficiente, mas para o último botão como podemos determinar o índice de linha inicial para a última página de dados? Para esse índice, bem como ser capaz de determinar que se os botões Próximo e último devem ser habilitados, precisamos saber quantos registros no total estão sendo enviada via pager por meio de computação. Podemos determinar isso chamando o `ProductsBLL` classe s `TotalNumberOfProducts()` método. Permitem s criar uma propriedade somente leitura, o nível de página chamada `TotalRowCount` que retorna os resultados de `TotalNumberOfProducts()` método:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

Com essa propriedade agora é possível determinar o último índice de linha de início s de página. Especificamente, ele s o resultado de inteiro do `TotalRowCount` menos 1 dividido por `MaximumRows`, multiplicado por `MaximumRows`. Agora podemos escrever o `Click` manipuladores de eventos para os quatro botões de interface de paginação:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

Por fim, é necessário desabilitar os botões primeiro e anterior na interface de paginação ao exibir a primeira página de dados e os botões Próximo e último ao exibir a última página. Para fazer isso, adicione o seguinte código para o s ObjectDataSource `Selecting` manipulador de eventos:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

Depois de adicionar essas `Click` manipuladores de eventos e o código para habilitar ou desabilitar os elementos da interface de paginação com base no índice de linha inicial atual, a página de teste em um navegador. Como ilustra a Figura 15, ao primeiro visitar a página na primeira e botões anterior serão estão desabilitados. Clicar em Avançar mostra a segunda página de dados, enquanto clicar última exibe a última página (consulte as figuras de 16 e 17). Ao exibir a última página de dados de botões de próximo e o último estão desabilitados.


[![Os botões anterior e última são desabilitadas quando exibir a primeira página de produtos](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**Figura 15**: botões última e a anterior são desabilitadas quando exibir a primeira página de produtos ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))


[![A segunda página de produtos estão sendo exibida](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**Figura 16**: A página segundo de produtos estão sendo exibida ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))


[![Clicar em último exibirá a página Final de dados](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**Figura 17**: clicar em último exibe a página Final de dados ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Etapa 7: Incluindo classificação suporte com personalizado paginável repetidor

Agora que a paginação personalizada tiver sido implementada, podemos re pronto para incluir a classificação de suporte. O `ProductsBLL` classe s `GetProductsPagedAndSorted` método tem o mesmo *startRowIndex* e *maximumRows* parâmetros de entrada `GetProductsPaged`, mas permite que outros  *sortExpression* parâmetro de entrada. Para usar o `GetProductsPagedAndSorted` método de `SortingWithCustomPaging.aspx`, é preciso executar as seguintes etapas:

1. Alterar o s ObjectDataSource `SelectMethod` propriedade `GetProductsPaged` para `GetProductsPagedAndSorted`.
2. Adicionar um *sortExpression* `Parameter` objeto para o s ObjectDataSource `SelectParameters` coleção.
3. Criar um privado, o nível de página `SortExpression` propriedade que mantém seu valor em postagens por meio do estado de exibição de página s.
4. Atualizar o s ObjectDataSource `Selecting` manipulador de eventos para atribuir a s ObjectDataSource *sortExpression* parâmetro o valor do nível de página `SortExpression` propriedade.
5. Crie a interface de classificação.

Inicie atualizando o s ObjectDataSource `SelectMethod` propriedade e adicionando um *sortExpression* `Parameter`. Verifique se o *sortExpression* `Parameter` s `Type` está definida como `String`. Depois de concluir as duas primeiras tarefas, a marcação declarativa de s ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

Em seguida, precisamos de um nível de página `SortExpression` propriedade cujo valor é serializado no estado de exibição. Se nenhum valor de expressão de classificação foi definida, use ProductName como padrão:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

Antes do ObjectDataSource invoca o `GetProductsPagedAndSorted` método precisamos definir o *sortExpression* `Parameter` para o valor da `SortExpression` propriedade. No `Selecting` manipulador de eventos, adicione a seguinte linha de código:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

Tudo o que falta é implementam a interface de classificação. Como foi o último exemplo, permitem s têm a classificação interface implementada usando três controles de botão Web que permitem ao usuário classificar os resultados por nome do produto, categoria ou fornecedor.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

Criar `Click` manipuladores de eventos para esses três controles de botão. No evento manipulador, redefina o `StartRowIndex` como 0, defina o `SortExpression` para o valor apropriado e reassociar os dados a serem repetidor:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

Tudo lá que s é a ele! Enquanto havia um número de etapas para paginação personalizada e a classificação implementado, as etapas foram muito semelhantes àquelas necessárias para paginação padrão. A Figura 18 mostra os produtos ao exibir a última página de dados quando classificadas por categoria.


[![A última página de dados, Sorted por categoria, são exibidos](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**Figura 18**: A página de dados do último, Sorted por categoria, é exibido ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))


> [!NOTE]
> Nos exemplos anteriores, quando a classificação pelo fornecedor que NomeDoFornecedor foi usado como a expressão de classificação. No entanto, para a implementação de paginação personalizada, é preciso usar CompanyName. Isso ocorre porque o procedimento armazenado responsável por implementar paginação personalizada `GetProductsPagedAndSorted` passa a expressão de classificação para o `ROW_NUMBER()` palavra-chave, o `ROW_NUMBER()` palavra-chave requer o nome da coluna real em vez de um alias. Portanto, é necessário usar `CompanyName` (o nome da coluna no `Suppliers` tabela) em vez de alias usado no `SELECT` consulta (`SupplierName`) para a expressão de classificação.


## <a name="summary"></a>Resumo

Nem o DataList nem repetidor oferecem suporte interno a classificação, mas com um pouco de código e uma interface de classificação personalizada, essa funcionalidade pode ser adicionada. Ao implementar a classificação, mas não de paginação, a expressão de classificação pode ser especificada por meio de `DataSourceSelectArguments` objeto passado para o s ObjectDataSource `Select` método. Isso `DataSourceSelectArguments` objeto s `SortExpression` propriedade pode ser atribuída em ObjectDataSource s `Selecting` manipulador de eventos.

Para adicionar recursos de classificação para um DataList ou repetidor que já oferece suporte à paginação, a abordagem mais fácil é personalizar a camada de lógica de negócios para incluir um método que aceita uma expressão de classificação. Essas informações, em seguida, podem ser passadas através de um parâmetro em ObjectDataSource s `SelectParameters`.

Este tutorial conclui nossa análise de paginação e classificação com os controles DataList e Repetidor. Nosso tutorial final e Avançar examinará como adicionar controles de botão Web para os modelos de s DataList e repetidor para fornecer alguma funcionalidade personalizada, iniciado pelo usuário em uma base por item.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi David Suru. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
