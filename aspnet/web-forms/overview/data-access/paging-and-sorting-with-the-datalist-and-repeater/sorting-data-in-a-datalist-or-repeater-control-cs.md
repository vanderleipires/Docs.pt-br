---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: Classificando dados em um controle DataList ou Repeater (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos como incluir a classificação de suporte no DataList e Repeater, bem como construir um DataList ou Repeater cujos dados podem...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 05fbc51d5341a4d3d634cbbc05c0e66a827b0394
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825399"
---
<a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>Classificação de dados em um controle DataList ou Repeater (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) ou [baixar PDF](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> Neste tutorial, examinaremos como incluir a classificação de suporte no DataList e Repeater, bem como construir um DataList ou Repeater cujos dados podem ser paginados e classificados.


## <a name="introduction"></a>Introdução

No [tutorial anterior](paging-report-data-in-a-datalist-or-repeater-control-cs.md) , examinamos como adicionar suporte à paginação para uma DataList. Criamos um novo método na `ProductsBLL` classe (`GetProductsAsPagedDataSource`) que retornou um `PagedDataSource` objeto. Quando associado a um DataList ou Repeater, o DataList ou Repeater exibiria apenas a página solicitada de dados. Essa técnica é semelhante ao que é usado internamente pelos controles GridView, DetailsView e FormView para fornecer a funcionalidade de paginação internos padrão.

Além de oferecer suporte à paginação, GridView também inclui fora da caixa de suporte à classificação. Nem o DataList nem Repeater fornece funcionalidade interna de classificação; No entanto, os recursos de classificação pode ser adicionado com um pouco de código. Neste tutorial, examinaremos como incluir a classificação de suporte no DataList e Repeater, bem como construir um DataList ou Repeater cujos dados podem ser paginados e classificados.

## <a name="a-review-of-sorting"></a>Uma revisão de classificação

Como vimos na [paginação e classificação de dados do relatório](../paging-and-sorting/paging-and-sorting-report-data-cs.md) tutorial, o controle GridView fornece fora da caixa de suporte à classificação. Cada campo de GridView pode ter um tipo de `SortExpression`, que indica o campo de dados pela qual classificar os dados. Quando o s GridView `AllowSorting` estiver definida como `true`, cada campo de GridView que tem um `SortExpression` valor da propriedade tem seu cabeçalho renderizado como um LinkButton. Quando um usuário clica em um cabeçalho de s de campo específico do GridView, ocorre um postback e os dados são classificados de acordo com o campo clicado s `SortExpression`.

O controle GridView possui um `SortExpression` propriedade também, que armazena o `SortExpression` do campo de GridView, os dados são classificados por. Além disso, um `SortDirection` propriedade indica se os dados são seja classificada em ordem crescente ou decrescente (se um usuário clica em um determinado link de cabeçalho de campo s GridView duas vezes em sucessão, a ordem de classificação é alternado).

Quando o GridView está associado a seu controle de fonte de dados, ele transmite sua `SortExpression` e `SortDirection` propriedades aos dados de controle de origem. O controle de fonte de dados recupera os dados e, em seguida, classifica-o acordo com os fornecidos `SortExpression` e `SortDirection` propriedades. Depois de classificar os dados, o controle de fonte de dados retorna-a GridView.

Para replicar essa funcionalidade com os controles DataList ou Repeater, devemos:

- Criar uma interface de classificação
- Lembre-se o campo de dados para classificar por e se deve classificar em ordem crescente ou decrescente
- Instrua o ObjectDataSource para classificar os dados por um campo de dados específico

Que abordaremos esses três tarefas nas etapas 3 e 4. Depois disso, vamos examinar como incluir tanto de paginação e classificação de suporte em um DataList ou Repeater.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Etapa 2: Exibindo produtos em um repetidor

Antes de nos preocupamos implementar qualquer uma das funcionalidades relacionadas à classificação, deixe s começar listando os produtos em um controle Repeater. Comece abrindo o `Sorting.aspx` página o `PagingSortingDataListRepeater` pasta. Adicionar um controle Repeater para a página da web, definindo sua `ID` propriedade para `SortableProducts`. De marca inteligente Repeater s, crie um novo ObjectDataSource denominado `ProductsDataSource` e configurá-lo para recuperar dados, o `ProductsBLL` classe s `GetProducts()` método. Selecione a que opção das listas suspensas nas guias de INSERT, UPDATE e DELETE (nenhum).


[![Criar um ObjectDataSource e configurá-lo para usar o método GetProductsAsPagedDataSource()](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Figura 1**: criar um ObjectDataSource e configurá-lo para usar o `GetProductsAsPagedDataSource()` método ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))


[![Defina a lista suspensa na atualização, inserção e excluir guias como (nenhum)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**Figura 2**: defina a lista suspensa na atualização, inserção e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))


Ao contrário de com DataList, o Visual Studio não cria automaticamente um `ItemTemplate` para o controle Repeater depois de associá-lo a uma fonte de dados. Além disso, devemos adicionar isso `ItemTemplate` declarativamente, como a marca inteligente do repetidor controle s não tem a opção de editar modelos encontrada no DataList s. Let s usam o mesmo `ItemTemplate` no tutorial anterior, a que é exibido o nome do produto s, fornecedor e categoria.

Depois de adicionar o `ItemTemplate`, Repeater e ObjectDataSource s marcação declarativa deve ser semelhante ao seguinte:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

Figura 3 mostra essa página quando visualizado por meio de um navegador.


[![Cada produto s nome, categoria e fornecedor é exibido](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Figura 3**: cada produto s nome, fornecedor e categoria é exibida ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Etapa 3: Instruindo o ObjectDataSource para classificar os dados

Para classificar os dados exibidos no repetidor, precisamos informar o ObjectDataSource da expressão de classificação pelo qual os dados devem ser classificados. Antes do ObjectDataSource recupera seus dados, primeiro acionará seu [ `Selecting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), que fornece uma oportunidade especificar uma expressão de classificação. O `Selecting` manipulador de eventos recebe um objeto do tipo [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), que tem uma propriedade chamada [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) do tipo [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). O `DataSourceSelectArguments` classe foi projetada para passar as solicitações relacionadas a dados de um consumidor de dados para o controle de fonte de dados e inclui um [ `SortExpression` propriedade](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Para passar informações de classificação da página ASP.NET para o ObjectDataSource, crie um manipulador de eventos para o `Selecting` evento e use o código a seguir:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

O *sortExpression* valor deve ser atribuído como o nome do campo de dados para classificar os dados por (como ProductName). Não há nenhuma propriedade relacionada a direção de classificação, portanto, se você quiser classificar os dados em ordem decrescente, acrescente a cadeia de caracteres DESC para o *sortExpression* valor (como ProductName DESC).

Vá em frente e experimente alguns valores embutidos diferentes para *sortExpression* e testar os resultados em um navegador. Como mostra a Figura 4, ao usar ProductName DESC como o *sortExpression*, os produtos são classificados por nome em ordem alfabética inversa.


[![Os produtos são classificados por nome em ordem alfabética inversa](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Figura 4**: os produtos são classificados por nome em ordem alfabética inversa ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Etapa 4: Criar a Interface de classificação e lembrar-se a expressão de classificação e a direção

Ativar o suporte de GridView de classificação converte o texto do cabeçalho cada campo classificável s em um LinkButton que, quando clicado, classifica os dados adequadamente. Uma interface desse tipo classificação faz sentido para o GridView, onde seus dados é claramente dispostos em colunas. Para os controles DataList e Repeater, no entanto, é necessária uma interface de classificação diferente. Uma interface comum de classificação para obter uma lista de dados (em vez de uma grade de dados), é uma lista suspensa que fornece os campos pelos quais os dados podem ser classificados. Deixe o s implementar uma interface desse tipo para este tutorial.

Adicionar um controle DropDownList da Web acima a `SortableProducts` Repeater e defina seu `ID` propriedade `SortBy`. Na janela Propriedades, clique nas reticências no `Items` propriedade para transferir anterior o Editor de coleção ListItem. Adicione `ListItem` s para classificar os dados pela `ProductName`, `CategoryName`, e `SupplierName` campos. Adicione também um `ListItem` para classificar os produtos por seu nome em ordem alfabética inversa.

O `ListItem` `Text` propriedades podem ser definidas para qualquer valor (como nome), mas o `Value` propriedades devem ser definidas para o nome do campo de dados (como ProductName). Para classificar os resultados em ordem decrescente, acrescente a cadeia de caracteres DESC para o nome do campo de dados, como ProductName DESC.


![Adicionar um item de lista para cada um dos campos de dados classificável](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Figura 5**: adicionar um `ListItem` para cada um dos campos de dados classificável


Finalmente, adicione um controle da Web de botão à direita da DropDownList. Defina suas `ID` para `RefreshRepeater` e seu `Text` propriedade para a atualização.

Depois de criar o `ListItem` s e adicionar o botão de atualização, a sintaxe declarativa de s do DropDownList e botão deve ser semelhante ao seguinte:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

Com a classificação DropDownList completa, em seguida precisamos atualizar o s ObjectDataSource `Selecting` manipulador de eventos, por isso que ele usa o selecionado `SortBy``ListItem` s `Value` propriedade em vez de uma expressão de classificação embutido em código.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

Neste ponto, quando o primeiro visitando a página de produtos inicialmente serão classificados pela `ProductName` campo de dados, como ela s a `SortBy` `ListItem` selecionada por padrão (veja a Figura 6). Selecionando a opção, como a categoria de classificação um diferentes e você clicar em atualizar, fazer com que um postback e reclassifica os dados pelo nome da categoria, como mostra a Figura 7.


[![Os produtos são inicialmente classificadas por seu nome](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Figura 6**: os produtos são inicialmente classificadas por seu nome ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))


[![Os produtos são agora são classificados por categoria](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Figura 7**: os produtos são agora são classificados por categoria ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))


> [!NOTE]
> Clicar no botão de atualização faz com que os dados automaticamente sejam classificados novamente porque o estado de exibição do repetidor s foi desabilitado, causando assim o Repeater reassociar a fonte de dados em cada postagem. Se você ve deixado o estado de exibição do repetidor s habilitado, alterar a classificação de lista suspensa lista ganhou um t tenha qualquer efeito sobre a ordem de classificação. Para corrigir isso, crie um manipulador de eventos para o botão Atualizar s `Click` reassociar o Repeater para sua fonte de dados e eventos (chamando o Repeater s `DataBind()` método).


## <a name="remembering-the-sort-expression-and-direction"></a>Lembrar-se a expressão de classificação e a direção

Ao criar um classificável DataList ou Repeater em uma página em que o não-classificação relacionados a postbacks podem ocorrer, ele imperativa de s que a expressão de classificação e a direção ser lembrados entre postbacks. Por exemplo, imagine que atualizamos o Repeater neste tutorial para incluir um botão Excluir em cada produto. Quando o usuário clica no botão Excluir d executamos algum código para excluir o produto selecionado e, em seguida, associar novamente os dados a serem Repetidor. Se os detalhes de classificação não são persistidos em postback, os dados exibidos na tela serão revertido para a ordem de classificação originais.

Para este tutorial, DropDownList implicitamente salva a classificação expressão e direção em seu estado de exibição para nós. Se estivéssemos usando uma interface de classificação diferente, uma com, digamos, de LinkButtons que forneceu as várias opções de classificação d é necessário tomar cuidado para lembrar a ordem de classificação entre postbacks. Isso pode ser feito ao armazenar os parâmetros de classificação no estado de exibição de página s, incluindo o parâmetro de classificação na querystring, ou por meio de alguma outra técnica de persistência de estado.

Futuras exemplos neste tutorial exploram como manter os detalhes de classificação no estado de exibição de s de página.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Etapa 5: Adicionando suporte à classificação para uma DataList que usa paginação padrão

No [tutorial anterior](paging-report-data-in-a-datalist-or-repeater-control-cs.md) , examinamos como implementar a paginação padrão com um DataList. Deixe o s estender este exemplo anterior para incluir a capacidade de classificar os dados paginados. Comece abrindo o `SortingWithDefaultPaging.aspx` e `Paging.aspx` páginas no `PagingSortingDataListRepeater` pasta. Do `Paging.aspx` , clique no botão fonte para exibir a marcação declarativa de s de página. Copie o texto selecionado (consulte a Figura 8) e cole-o na marcação declarativa de `SortingWithDefaultPaging.aspx` entre o `<asp:Content>` marcas.


[![Replicar a marcação declarativa na &lt;asp: Content&gt; marcas de Paging.aspx para SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Figura 8**: replicar a marcação declarativa em de `<asp:Content>` marcas de `Paging.aspx` ao `SortingWithDefaultPaging.aspx` ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))


Depois de copiar a marcação declarativa, copie os métodos e propriedades na `Paging.aspx` página de classe code-behind de s para a classe code-behind para `SortingWithDefaultPaging.aspx`. Em seguida, reserve um tempo para exibir o `SortingWithDefaultPaging.aspx` página em um navegador. Ele deve exibir a mesma funcionalidade e a mesma aparência que `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Aprimorando ProductsBLL para incluir um padrão de paginação e classificação de método

No tutorial anterior, criamos uma `GetProductsAsPagedDataSource(pageIndex, pageSize)` método na `ProductsBLL` classe que retornou um `PagedDataSource` objeto. Isso `PagedDataSource` objeto foi preenchido com *todas as* dos produtos (via o s BLL `GetProducts()` método), mas quando associado à DataList somente os registros correspondentes especificado *pageIndex* e *pageSize* parâmetros de entrada foram exibidos.

Neste tutorial, adicionamos suporte à classificação, especificando a expressão de classificação de s ObjectDataSource `Selecting` manipulador de eventos. Isso funciona bem quando o ObjectDataSource é retornado um objeto que pode ser classificado, como o `ProductsDataTable` retornado pelo `GetProducts()` método. No entanto, o `PagedDataSource` objeto retornado pelo `GetProductsAsPagedDataSource` método não dá suporte a classificação de fonte de dados interna. Em vez disso, é necessário classificar os resultados retornados do `GetProducts()` método *antes de* colocamos no `PagedDataSource`.

Para fazer isso, crie um novo método na `ProductsBLL` classe, `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Para classificar os `ProductsDataTable` retornado pelo `GetProducts()` método, especifique a `Sort` propriedade do seu padrão `DataTableView`:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

O `GetProductsSortedAsPagedDataSource` método difere apenas ligeiramente do `GetProductsAsPagedDataSource` método criado no tutorial anterior. Em particular, `GetProductsSortedAsPagedDataSource` aceita um parâmetro de entrada adicional `sortExpression` e atribui esse valor para o `Sort` propriedade o `ProductDataTable` s `DefaultView`. Algumas linhas de código mais tarde, o `PagedDataSource` s fonte de dados de objeto é atribuído a `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Chamar o método GetProductsSortedAsPagedDataSource e especificando o valor para o parâmetro de entrada SortExpression

Com o `GetProductsSortedAsPagedDataSource` método completo, a próxima etapa é fornecer o valor para esse parâmetro. O ObjectDataSource na `SortingWithDefaultPaging.aspx` está configurado para chamar o `GetProductsAsPagedDataSource` método e passa em dois parâmetros de entrada por meio de seus dois `QueryStringParameters`, que são especificados o `SelectParameters` coleção. Essas duas `QueryStringParameters` indicar que a origem para o `GetProductsAsPagedDataSource` método s *pageIndex* e *pageSize* parâmetros vêm os campos de cadeia de consulta `pageIndex` e `pageSize`.

Atualizar o s ObjectDataSource `SelectMethod` propriedade para que ele chama o novo `GetProductsSortedAsPagedDataSource` método. Em seguida, adicione um novo `QueryStringParameter` para que o *sortExpression* parâmetro de entrada é acessado no campo querystring `sortExpression`. Defina as `QueryStringParameter` s `DefaultValue` para ProductName.

Após essas alterações, a marcação declarativa do ObjectDataSource s deve ser semelhante:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

Neste ponto, o `SortingWithDefaultPaging.aspx` página será classificar seus resultados em ordem alfabética pelo nome do produto (veja a Figura 9). Isso ocorre porque, por padrão, um valor de ProductName é passado como o `GetProductsSortedAsPagedDataSource` método s *sortExpression* parâmetro.


[![Por padrão, os resultados são classificados por ProductName](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Figura 9**: por padrão, os resultados são classificados por `ProductName` ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))


Se você adicionar manualmente uma `sortExpression` campo de cadeia de consulta, como `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` os resultados serão classificados pelo `sortExpression`. No entanto, isso `sortExpression` parâmetro não está incluído na querystring ao mudar para uma página de dados diferente. Na verdade, clicando na página do próximo ou último botões usa nós de volta para `Paging.aspx`! Além disso, s há atualmente nenhuma classificação de interface. É a única maneira de que um usuário pode alterar a ordem de classificação dos dados paginados pela manipulação de cadeia de consulta diretamente.

## <a name="creating-the-sorting-interface"></a>Criando a Interface de classificação

Primeiro, precisamos atualizar o `RedirectUser` método para enviar ao usuário `SortingWithDefaultPaging.aspx` (em vez de `Paging.aspx`) e incluir o `sortExpression` valor na cadeia de consulta. Podemos também deve adicionar um somente leitura, nível de página chamado `SortExpression` propriedade. Essa propriedade, semelhante à `PageIndex` e `PageSize` propriedades criadas no tutorial anterior, retorna o valor da `sortExpression` querystring campo se ele existir e o padrão de valor (ProductName) caso contrário.

Atualmente, o `RedirectUser` método aceita apenas um único parâmetro de entrada de índice da página para exibir. No entanto, pode haver ocasiões em que é útil para redirecionar o usuário para uma determinada página de dados usando uma expressão de classificação que não seja o que é especificado na cadeia de consulta. Em alguns instantes, criaremos a interface de classificação para essa página, que inclui uma série de controles da Web de botão para classificar os dados por uma coluna especificada. Quando um desses botões é clicado, queremos que redirecionará o usuário passando o valor da expressão de classificação apropriado. Para fornecer essa funcionalidade, crie duas versões do `RedirectUser` método. O primeiro deles deverá aceitar apenas o índice de página para exibir, enquanto o segundo é aceita a expressão de índice e de classificação de página.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

No primeiro exemplo neste tutorial, criamos uma interface de classificação usando DropDownList. Neste exemplo, let s usar três controles da Web de botão posicionados acima uma DataList para classificar por `ProductName`, um para `CategoryName`e outro para `SupplierName`. Adicione três controles da Web de botão, definindo sua `ID` e `Text` propriedades adequadamente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

Em seguida, crie um `Click` manipulador de eventos para cada um. Os manipuladores de eventos deverá chamar o `RedirectUser` método, retornando o usuário para a primeira página usando a expressão de classificação apropriado.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Quando o primeiro visitando a página, os dados são classificados em ordem alfabética pelo nome do produto (consulte novamente a Figura 9). Clique no botão Avançar para Avançar para a segunda página de dados e clique na classificação por botão de categoria. Isso retorna nós para a primeira página de dados, classificados por nome de categoria (consulte a Figura 10). Da mesma forma, clicando em Classificar por botão de fornecedor classifica os dados por fornecedor a partir da primeira página de dados. A opção de classificação será lembrada conforme os dados são paginados por meio do. Figura 11 mostra a página após a classificação por categoria e, em seguida, avançando para a décimo terceiro página de dados.


[![Os produtos são classificados por categoria](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Figura 10**: os produtos são classificados por categoria ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))


[![A expressão de classificação é lembrada quando paginação por meio de dados do](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Figura 11**: A expressão de classificação é lembrada quando paginação por meio de dados do ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Etapa 6: A paginação personalizada por meio de registros em um repetidor

O exemplo de DataList examinados na etapa 5 páginas por meio de seus dados usando a técnica de paginação padrão ineficiente. Quando a paginação por meio de suficientemente grandes quantidades de dados, é imperativo que a paginação personalizada seja usado. Volta a [com eficiência por meio de grandes quantidades de dados de paginação](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) e [classificando dados paginados personalizado](../paging-and-sorting/sorting-custom-paged-data-cs.md) tutoriais, examinamos as diferenças entre padrão e a paginação personalizada e métodos criados na BLL para utilizando personalizado de paginação e classificação personalizados dados paginados. Em particular, esses dois tutoriais anteriores, adicionamos três métodos a seguir para o `ProductsBLL` classe:

- `GetProductsPaged(startRowIndex, maximumRows)` Retorna um subconjunto específico de registros começando *startRowIndex* e não excedendo *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` Retorna um subconjunto específico de registros classificados pela especificado *sortExpression* parâmetro de entrada.
- `TotalNumberOfProducts()` fornece o número total de registros no `Products` tabela de banco de dados.

Esses métodos podem ser usados com eficiência, página e classificar dados usando um controle DataList ou Repeater. Para ilustrar isso, deixe s comece criando um controle Repeater com suporte à paginação personalizada; em seguida, vamos adicionar recursos de classificação.

Abra o `SortingWithCustomPaging.aspx` página na `PagingSortingDataListRepeater` pasta e adicione um repetidor para a página, definindo seu `ID` propriedade para `Products`. De marca inteligente Repeater s, crie um novo ObjectDataSource chamado `ProductsDataSource`. Configurá-lo para selecionar seus dados a partir de `ProductsBLL` classe s `GetProductsPaged` método.


[![Configurar o ObjectDataSource para usar o método de GetProductsPaged ProductsBLL classe s](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Figura 12**: configurar o ObjectDataSource para usar o `ProductsBLL` classe s `GetProductsPaged` método ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))


Defina as listas suspensas na atualização, inserção e excluir guias como (nenhum) e, em seguida, clique no botão Avançar. O Assistente Configurar fonte de dados agora solicita as fontes do `GetProductsPaged` método s *startRowIndex* e *maximumRows* parâmetros de entrada. Na realidade, esses parâmetros de entrada são ignorados. Em vez disso, o *startRowIndex* e *maximumRows* valores serão passados por meio de `Arguments` propriedade em s ObjectDataSource `Selecting` manipulador de eventos, assim como como especificamos o *sortExpression* nesta demonstração de tutorial s primeiro. Portanto, deixe a fonte de parâmetro nas listas suspensas no assistente definido em nenhum.


[![Deixe o conjunto de fontes de parâmetros como None](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Figura 13**: deixar as fontes de parâmetro definido como None ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))


> [!NOTE]
> Fazer *não* definir o s ObjectDataSource `EnablePaging` propriedade `true`. Isso fará com que o ObjectDataSource incluir automaticamente seu próprio *startRowIndex* e *maximumRows* parâmetros para o `SelectMethod` s lista existente de parâmetro. O `EnablePaging` propriedade é útil quando a associação personalizada porque esses controles esperam que o comportamento específico do ObjectDataSource que s de dados a um controle GridView, DetailsView ou FormView paginados disponível apenas quando `EnablePaging` é de propriedade `true`. Já que temos que adicionar manualmente o suporte de paginação para o DataList e Repeater, deixe essa propriedade definida como `false` (o padrão), como vai inserir na funcionalidade necessária diretamente em nossa página do ASP.NET.


Finalmente, defina o Repeater s `ItemTemplate` para que o nome do produto s, categoria e fornecedor são mostrados. Após essas alterações, a sintaxe de declarativa Repeater e ObjectDataSource s deve ser semelhante ao seguinte:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

Reserve um tempo para visitar a página por meio de um navegador e observe que os registros não são retornados. Isso ocorre porque estamos ve ainda para especificar o *startRowIndex* e *maximumRows* valores de parâmetros; portanto, os valores de 0 estão sendo passados para ambos. Para especificar esses valores, crie um manipulador de eventos para o s ObjectDataSource `Selecting` eventos e definir esses parâmetros de valores de programaticamente para valores codificados de 0 a 5, respectivamente:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

Com essa alteração, a página, quando visualizado por meio de um navegador, mostra os cinco primeiros produtos.


[![Os cinco primeiros registros são exibidos](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Figura 14**: os primeiros cinco registros são exibidos ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))


> [!NOTE]
> Os produtos listados na Figura 14 acontecem seja classificada por nome do produto, porque o `GetProductsPaged` procedimento armazenado que executa a consulta de paginação personalizada eficiente ordena os resultados por `ProductName`.


Para permitir que o usuário percorrer as páginas, é necessário controlar o índice de linha de início e o número máximo de linhas e lembre-se esses valores em postagens. No exemplo de paginação padrão, usamos os campos de cadeia de consulta para manter esses valores; para esta demonstração, deixe s manter essas informações no estado de exibição de s de página. Crie as duas propriedades a seguir:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

Em seguida, atualize o código no manipulador de eventos de seleção para que ele use o `StartRowIndex` e `MaximumRows` propriedades em vez dos valores embutidos de 0 a 5:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

Neste momento, nossa página ainda mostra apenas os cinco primeiros registros. No entanto, com essas propriedades em vigor, podemos está pronto para criar nossa interface de paginação.

## <a name="adding-the-paging-interface"></a>Adicionando a Interface de paginação

Use s permitem que o mesmo primeiro, anterior, Avançar, paginação de última interface usada no exemplo de paginação padrão, incluindo a Web de rótulo de controle que exibe qual página de dados estiver sendo exibido e o número total de páginas existem. Adicione os quatro controles da Web de botão e o rótulo abaixo Repetidor.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

Em seguida, crie `Click` manipuladores de eventos para os quatro botões. Quando um desses botões é clicado, precisamos atualizar o `StartRowIndex` e associar novamente os dados a serem Repetidor. O código para os botões First, anterior e próxima é bastante simple, mas para o último botão como podemos determinar o índice de linha de início para a última página de dados? Para esse índice, bem como ser capaz de determinar que se os botões Próximo e último devem ser habilitados precisamos saber quantos registros no total estão sendo paginados por meio de computação. Podemos determinar isso chamando o `ProductsBLL` classe s `TotalNumberOfProducts()` método. Let s criar uma propriedade somente leitura, o nível de página chamada `TotalRowCount` que retorna os resultados do `TotalNumberOfProducts()` método:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

Com essa propriedade agora podemos determinar o último índice de linha de início s de página. Especificamente, ele s o resultado de inteiro do `TotalRowCount` menos 1 dividido por `MaximumRows`, multiplicado pelo `MaximumRows`. Agora podemos escrever a `Click` manipuladores de eventos para os quatro botões de interface de paginação:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Por fim, precisamos desabilitar os botões de primeiro e anterior na interface de paginação ao exibir a primeira página de dados e os botões Próximo e último ao exibir a última página. Para fazer isso, adicione o seguinte código para o s ObjectDataSource `Selecting` manipulador de eventos:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

Depois de adicionar essas `Click` manipuladores de eventos e o código para habilitar ou desabilitar os elementos de interface de paginação com base no índice de linha de início atual, a página de teste em um navegador. Como a Figura 15 ilustra ao primeiro visitar a página na primeira e botões anterior serão estão desabilitados. Clicar em Avançar mostra a segunda página de dados, enquanto clicar em último exibe a última página (consulte as figuras de 16 e 17). Ao exibir a última página de dados de botões de próximo e último estão desabilitados.


[![Os botões anterior e último estão desabilitadas ao exibir a primeira página de produtos](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Figura 15**: O anterior e o último botões estão desabilitadas ao exibir a primeira página de produtos ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))


[![A segunda página de produtos estão sendo exibida](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**Figura 16**: A página de segundo de produtos estão sendo exibida ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))


[![Clicar em último exibirá a página Final de dados](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Figura 17**: clicando em último exibe a página de dados Final ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Etapa 7: Incluindo classificação suporte com personalizado paginável Repeater

Agora que a paginação personalizada tiver sido implementada, oferecemos está pronto para incluir a classificação de suporte. O `ProductsBLL` classe s `GetProductsPagedAndSorted` método tem o mesmo *startRowIndex* e *maximumRows* parâmetros como de entrada `GetProductsPaged`, mas permite que mais  *sortExpression* parâmetro de entrada. Para usar o `GetProductsPagedAndSorted` método de `SortingWithCustomPaging.aspx`, é necessário executar as seguintes etapas:

1. Alterar o s ObjectDataSource `SelectMethod` propriedade de `GetProductsPaged` para `GetProductsPagedAndSorted`.
2. Adicionar um *sortExpression* `Parameter` objeto para o s ObjectDataSource `SelectParameters` coleção.
3. Criar uma privada, o nível de página `SortExpression` propriedade que persiste seu valor em postagens por meio do estado de exibição de página s.
4. Atualizar o s ObjectDataSource `Selecting` manipulador de eventos para atribuir o s ObjectDataSource *sortExpression* parâmetro o valor do nível de página `SortExpression` propriedade.
5. Crie a interface de classificação.

Comece atualizando o s ObjectDataSource `SelectMethod` propriedade e a adição de um *sortExpression* `Parameter`. Certifique-se de que o *sortExpression* `Parameter` s `Type` estiver definida como `String`. Depois de concluir essas duas primeiras tarefas, a marcação declarativa do ObjectDataSource s deve ser semelhante ao seguinte:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

Em seguida, precisamos de um nível de página `SortExpression` propriedade cujo valor é serializado no estado de exibição. Se nenhum valor de expressão de classificação tiver sido definido, use ProductName como o padrão:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

Antes do ObjectDataSource invoca o `GetProductsPagedAndSorted` método, precisamos definir o *sortExpression* `Parameter` para o valor da `SortExpression` propriedade. No `Selecting` manipulador de eventos, adicione a seguinte linha de código:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

Tudo o que resta é implementar a interface de classificação. Como fizemos no último exemplo, permite que s tenham a interface de classificação implementada usando três controles da Web de botão que permitem ao usuário classificar os resultados por fornecedor, categoria ou nome do produto.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Criar `Click` manipuladores de eventos para esses três controles de botão. No evento de redefinição de manipulador, o `StartRowIndex` como 0, defina o `SortExpression` para o valor apropriado e reassociar os dados a serem repetidor:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

Tudo que s é a ele! Embora houvesse um número de etapas para obter a paginação personalizada e a classificação implementado, as etapas eram muito semelhantes àquelas necessárias para a paginação padrão. Figura 18 mostra os produtos ao exibir a última página de dados quando classificadas por categoria.


[![A última página de dados, a classificação por categoria, é exibido](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**Figura 18**: O último dos dados da página, Sorted por categoria, é exibida ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))


> [!NOTE]
> Nos exemplos anteriores, quando a classificação por fornecedor que NomeDoFornecedor foi usado como a expressão de classificação. No entanto, para a implementação de paginação personalizada, precisamos usar CompanyName. Isso ocorre porque o procedimento armazenado responsável por implementar a paginação personalizada `GetProductsPagedAndSorted` passa a expressão de classificação para o `ROW_NUMBER()` palavra-chave, o `ROW_NUMBER()` palavra-chave requer o nome da coluna real em vez de um alias. Portanto, devemos usar `CompanyName` (o nome da coluna na `Suppliers` tabela) em vez do alias usado na `SELECT` consulta (`SupplierName`) para a expressão de classificação.


## <a name="summary"></a>Resumo

Nem o DataList e Repeater oferecem suporte interno a classificação, mas com um pouco de código e uma interface de classificação personalizada, essa funcionalidade pode ser adicionada. Ao implementar a classificação, mas não de paginação, a expressão de classificação pode ser especificada por meio de `DataSourceSelectArguments` objeto passado para o s ObjectDataSource `Select` método. Isso `DataSourceSelectArguments` objeto s `SortExpression` propriedade pode ser atribuída em s ObjectDataSource `Selecting` manipulador de eventos.

Para adicionar funcionalidades de classificação para um DataList ou Repeater que já oferece suporte à paginação, a abordagem mais fácil é personalizar a camada de lógica de negócios para incluir um método que aceita uma expressão de classificação. Essas informações, em seguida, podem ser passadas por meio de um parâmetro em s ObjectDataSource `SelectParameters`.

Esse tutorial conclui nossa análise de paginação e classificação com os controles DataList e Repeater. Nosso próximo e último tutorial irá examinar como adicionar controles da Web de botão para os modelos de s DataList e Repeater para fornecer alguma funcionalidade personalizada, iniciada pelo usuário em uma base por item.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi David Suru. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
> [Próximo](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
