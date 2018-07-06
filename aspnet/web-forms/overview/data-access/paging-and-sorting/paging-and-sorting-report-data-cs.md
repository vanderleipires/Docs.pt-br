---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: Paginação e classificação relatórios de dados (c#) | Microsoft Docs
author: rick-anderson
description: Paginação e classificação são dois recursos muito comuns ao exibir dados em um aplicativo online. Neste tutorial vamos dar uma primeira olhada na adição de classificação e...
ms.author: aspnetcontent
ms.date: 08/15/2006
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 741404bda11fd1d5776a7493b95ffe5d0c61fce2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819339"
---
<a name="paging-and-sorting-report-data-c"></a>Paginação e classificação de dados do relatório (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe) ou [baixar PDF](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)

> Paginação e classificação são dois recursos muito comuns ao exibir dados em um aplicativo online. Neste tutorial vamos dar uma primeira olhada na adição da classificação e paginação aos nossos relatórios, que iremos desenvolver em tutoriais futuros.


## <a name="introduction"></a>Introdução

Paginação e classificação são dois recursos muito comuns ao exibir dados em um aplicativo online. Por exemplo, ao pesquisar livros sobre o ASP.NET em uma livraria online, pode haver centenas de livros, mas o relatório listando os resultados da pesquisa lista apenas dez correspondências por página. Além disso, os resultados podem ser classificados por título, preço, contagem de páginas, nome do autor e assim por diante. Durante os últimos 23 tutoriais examinei como criar uma variedade de relatórios, incluindo as interfaces que permitem adicionar, editar e excluir dados, podemos ve não examinado como classificar dados e a única paginação exemplos podemos ve visto ter sido com DetailsView e FormView controles.

Neste tutorial, veremos como adicionar classificação e paginação aos nossos relatórios, que podem ser feitos verificando simplesmente algumas caixas de seleção. Infelizmente, essa implementação simples tem suas desvantagens que a interface classificação deixa um pouco a desejar e as rotinas de paginação não são projetadas para paginação de grandes conjuntos de resultados com eficiência. Tutoriais do futuros explorará como superar as limitações do out-of-the-box paginação e classificação de soluções.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Etapa 1: Adicionando a paginação e classificação Tutorial páginas da Web

Antes de começar este tutorial, deixe s primeiro dedique uns momentos para adicionar as páginas do ASP.NET, precisaremos para este tutorial e os próximos três. Comece criando uma nova pasta no projeto chamado `PagingAndSorting`. Em seguida, adicione as seguintes páginas ASP.NET cinco nesta pasta, manter todos eles configurado para usar a página mestra `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Crie uma pasta PagingAndSorting e adicione as páginas do Tutorial do ASP.NET](paging-and-sorting-report-data-cs/_static/image1.png)

**Figura 1**: Crie uma pasta PagingAndSorting e adicione as páginas do Tutorial do ASP.NET


Em seguida, abra o `Default.aspx` da página e arraste o `SectionLevelTutorialListing.ascx` controle de usuário do `UserControls` pasta para a superfície de Design. Esse controle de usuário que criamos na [páginas mestras e navegação no Site](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumera o mapa do site e exibe esses tutoriais na seção atual em uma lista com marcadores.


![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](paging-and-sorting-report-data-cs/_static/image2.png)

**Figura 2**: adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx


Para fazer com que a lista com marcadores exiba a paginação e classificação tutoriais, criaremos, precisamos adicioná-los ao mapa do site. Abra o `Web.sitemap` arquivo e adicione a marcação a seguir após a marcação de nó de mapa de site de edição, inserção e exclusão:


[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]


![Atualizar o mapa de Site para incluir as novas páginas do ASP.NET](paging-and-sorting-report-data-cs/_static/image3.png)

**Figura 3**: atualizar o mapa de Site para incluir as novas páginas do ASP.NET


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Etapa 2: Exibindo informações de produto em um GridView

Antes de nós implementamos realmente paging e recursos de classificação, deixe s primeiro criar um padrão não-srotable, não-paginável GridView que lista as informações do produto. Essa é uma tarefa é ve feito muitas vezes antes em toda esta série de tutoriais essas etapas deve estar familiarizado. Comece abrindo o `SimplePagingSorting.aspx` da página e arraste um controle GridView na caixa de ferramentas para o Designer, definindo seu `ID` propriedade `Products`. Em seguida, crie um novo ObjectDataSource que usa a classe ProductsBLL s `GetProducts()` método para retornar todas as informações de produto.


![Recuperar informações sobre todos os produtos usando o método GetProducts()](paging-and-sorting-report-data-cs/_static/image4.png)

**Figura 4**: recuperar informações sobre todos os produtos usando o método GetProducts()


Como esse relatório é um relatório somente leitura, não há s não precisa mapear o s ObjectDataSource `Insert()`, `Update()`, ou `Delete()` métodos correspondente `ProductsBLL` métodos; portanto, escolha (nenhum) na lista suspensa para a atualização, inserção, e guias de exclusão.


![Escolha (nenhum) opção na lista suspensa na atualização, inserção e excluir guias](paging-and-sorting-report-data-cs/_static/image5.png)

**Figura 5**: escolha (nenhum) opção na lista suspensa na atualização, inserção e excluir guias


Em seguida, permitir que s personalize os campos de s GridView para que sejam exibidos apenas os nomes de produtos, fornecedores, categorias, preços e status descontinuados. Além disso, fique à vontade para fazer qualquer formatação de nível de campo for alterado, como ajuste o `HeaderText` propriedades ou o preço como uma moeda de formatação. Após essas alterações, sua marcação declarativa do GridView s deve ser semelhante ao seguinte:


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

Figura 6 mostra nosso progresso até o momento quando visualizado por meio de um navegador. Observe que a página lista todos os produtos em uma única tela, mostrando cada nome de produto s, a categoria, o fornecedor, o preço e descontinuado status.


[![Cada um dos produtos listados](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)

**Figura 6**: cada um dos produtos são listados ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Etapa 3: Adicionando suporte à paginação

Listando *todos os* dos produtos em uma única tela pode levar a sobrecarga de informações para o usuário analisando os dados. Para ajudar a tornar os resultados mais gerenciáveis, podemos dividir os dados em páginas menores de dados e permitir que o usuário percorrer os dados em uma página por vez. Para realizar isso basta marcar a caixa de seleção Habilitar paginação da marca inteligente GridView s (Isso define o s GridView [ `AllowPaging` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) para `true`).


[![Marque a caixa de seleção Habilitar paginação para adicionar suporte à paginação](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)

**Figura 7**: marque a habilitar a paginação de seleção para adicionar suporte a paginação ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image11.png))


Habilitar paginação limita o número de registros mostrados por página e adiciona uma *interface de paginação* ao GridView. A interface de paginação padrão, mostrada na Figura 7, é uma série de números de página, permitindo que o usuário navegue rapidamente de uma página de dados para outro. Essa interface de paginação deve parecer familiar, como podemos ver visto ao adicionar suporte à paginação aos controles DetailsView e FormView nos tutoriais anteriores.

Controles de DetailsView e FormView mostram apenas um único registro por página. O controle GridView, no entanto, a consulta seu [ `PageSize` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) para determinar quantos registros devem ser exibidos por página (padrão dessa propriedade é um valor de 10).

Essa interface de paginação GridView, DetailsView e FormView s pode ser personalizado usando as seguintes propriedades:

- `PagerStyle` indica as informações de estilo para a interface de paginação; pode especificar configurações como `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`e assim por diante.
- `PagerSettings` contém um grupo de propriedades que pode personalizar a funcionalidade da interface de paginação; `PageButtonCount` indica o número máximo de números de página numérico exibidos na interface de paginação (o padrão é 10); a [ `Mode` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indica como a interface de paginação funciona e pode ser definida como: 

    - `NextPrevious` mostra um botões Próximo e anterior, permitindo que o usuário para a etapa frente ou para trás uma página por vez
    - `NextPreviousFirstLast` Além dos botões Próximo e anterior, primeiro e último botões também são incluídas, permitindo que o usuário mover rapidamente para a primeira ou última página de dados
    - `Numeric` mostra uma série de números de página, permitindo que o usuário ir imediatamente para qualquer página
    - `NumericFirstLast` Além de números de página, inclui botões primeiro e último, permitindo que o usuário mover rapidamente para a primeira ou última página de dados. os primeira/última botões são mostrados somente se todos os números de páginas numéricas pode não caber

Além disso, o GridView, DetailsView e FormView oferecem a `PageIndex` e `PageCount` propriedades, que indicam a página atual que está sendo exibido e o número total de páginas de dados, respectivamente. O `PageIndex` propriedade é indexada começando em 0, o que significa que, ao exibir a primeira página de dados `PageIndex` será igual a 0. `PageCount`, por outro lado, inicia a contagem em 1, o que significa que `PageIndex` é limitado aos valores entre 0 e `PageCount - 1`.

Deixe o s levar alguns instantes para melhorar a aparência padrão de nossa interface de paginação do GridView s. Especificamente, permite que s tenham a interface de paginação alinhado à direita com um plano de fundo cinza claro. Em vez de definir essas propriedades diretamente por meio de s GridView `PagerStyle` propriedade, let s criar uma classe CSS no `Styles.css` denominado `PagerRowStyle` e, em seguida, atribua o `PagerStyle` s `CssClass` propriedade por meio de nosso tema. Comece abrindo `Styles.css` e definição de classe adicionando o seguinte CSS:


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

Em seguida, abra o `GridView.skin` arquivo o `DataWebControls` pasta dentro a `App_Themes` pasta. Como discutimos na *páginas mestras e navegação no Site* arquivos de capa, tutoriais podem ser usados para especificar os valores de propriedade padrão para um controle de Web. Portanto, aumentar as configurações existentes para incluir a configuração do `PagerStyle` s `CssClass` propriedade para `PagerRowStyle`. Além disso, let s configurar a interface de paginação para no máximo Mostrar botões de cinco páginas numéricas usando o `NumericFirstLast` interface de paginação.


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>A experiência do usuário de paginação

A Figura 8 mostra a página da web quando acessadas por meio de um navegador depois que o GridView s habilitar paginação caixa de seleção foi marcada e o `PagerStyle` e `PagerSettings` configurações foram feitas por meio de `GridView.skin` arquivo. Observe como apenas dez registros são mostrados e a interface de paginação indica que estamos está exibindo a primeira página de dados.


[![Com a paginação habilitada, somente um subconjunto de registros são exibidos por vez](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)

**Figura 8**: com paginação habilitado, somente um subconjunto de registros são exibidos por vez ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image14.png))


Quando o usuário clica em um dos números de página na interface de paginação, massacre um postback e a página recarrega mostrando que a solicitada registros s de página. Figura 9 mostra os resultados depois de aceitar para exibir a página final de dados. Observe que a página final tem apenas um registro; Isso ocorre porque há 81 registros no total, resultando em oito páginas de 10 registros por página mais de uma página com um único registro.


[![Clicar em um número de página causa um Postback e mostra um subconjunto apropriado de registros](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)

**Figura 9**: clicar em um número de página causa um Postback e mostra o subconjunto de registros apropriados ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Fluxo de trabalho de servidor s de paginação

Quando o usuário final clica em um botão na interface de paginação, um postback massacre e começa o seguinte fluxo de trabalho do lado do servidor:

1. S GridView (ou DetailsView ou FormView) `PageIndexChanging` evento é acionado
2. O ObjectDataSource novamente solicita *todos os* dos dados da BLL; s o GridView `PageIndex` e `PageSize` valores de propriedade são usados para determinar quais registros retornados da BLL precisam ser exibido em um GridView
3. O s GridView `PageIndexChanged` evento é acionado

Na etapa 2, o ObjectDataSource solicita novamente todos os dados da fonte de dados. Esse estilo de paginação é conhecido como *paginação padrão*, como ela s o comportamento de paginação usado por padrão ao definir o `AllowPaging` propriedade `true`. Com o padrão paginar os dados controle Web ingenuamente recupera todos os registros de cada página de dados, mesmo que apenas um subconjunto de registros, na verdade, são renderizados em HTML que s enviada para o navegador. A menos que o banco de dados é armazenado em cache pelo BLL ou ObjectDataSource, paginação padrão não funciona para conjuntos de resultados grande o suficiente ou para aplicativos web com muitos usuários simultâneos.

No próximo tutorial, examinaremos como implementar *paginação personalizada*. Com a paginação personalizada, você pode instruir especificamente o ObjectDataSource para recuperar apenas o conjunto exato de registros necessários para a página solicitada de dados. Como você pode imaginar, paginação personalizada melhora significativamente a eficiência de paginação por meio de conjuntos de resultados grandes.

> [!NOTE]
> Enquanto a paginação padrão não é adequada quando a paginação por meio de conjuntos de resultados grande o suficiente ou para sites com muitos usuários simultâneos, perceber que a paginação personalizada requer mais esforço para implementar e a alterações e não é tão simple quanto marcando uma caixa de seleção (como é padrão paginação). Portanto, a paginação padrão pode ser a escolha ideal para sites pequenos e de baixo tráfego, ou quando define a paginação por meio de resultados relativamente pequeno, pois ele s muito mais fácil e rápido para implementar.


Por exemplo, se nós sabemos que nunca vai ter mais de 100 produtos em nosso banco de dados, o ganho de desempenho mínimo aproveitado por paginação personalizada é deslocado provavelmente pelo esforço necessário implementá-lo. Se, no entanto, talvez um dia temos milhares ou dezenas de milhares de produtos, *não* Implementando a paginação personalizada seria bastante degradar a escalabilidade do nosso aplicativo.

## <a name="step-4-customizing-the-paging-experience"></a>Etapa 4: Personalizar a experiência de paginação

Os controles da Web de dados fornecem uma série de propriedades que podem ser usados para aprimorar a experiência do usuário s paginação. O `PageCount` propriedade, por exemplo, indica o número total de páginas de lá estão, enquanto o `PageIndex` propriedade indica a página atual que está sendo visitada e pode ser definida para um usuário se mover rapidamente para uma página específica. Para ilustrar como usar essas propriedades para aprimorar a experiência do usuário s paginação, permitem que o s adicione um rótulo da Web de controle à nossa página informando ao usuário qual página eles re visitando atualmente, junto com um controle DropDownList que lhes permite ir rapidamente para uma determinada página .

Primeiro, adicione um controle de rótulo Web à sua página, defina suas `ID` propriedade para `PagingInformation`e limpe seu `Text` propriedade. Em seguida, crie um manipulador de eventos para o s GridView `DataBound` eventos e adicione o seguinte código:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

Atribui esse manipulador de eventos do `PagingInformation` rótulo s `Text` propriedade a uma mensagem informando ao usuário a página está visitando atualmente `Products.PageIndex + 1` fora do número total de páginas `Products.PageCount` (adicionamos 1 para o `Products.PageIndex` propriedade porque `PageIndex` são indexados começando em 0). Eu escolhi a atribuir esse rótulo s `Text` propriedade no `DataBound` manipulador de eventos em vez da `PageIndexChanged` manipulador de eventos porque o `DataBound` evento é acionado sempre que dados forem vinculados à GridView, enquanto o `PageIndexChanged` apenas o manipulador de eventos Acionado quando o índice de página é alterado. Quando o GridView inicialmente é dados associados na primeira página visita, o `PageIndexChanging` fire do evento t (enquanto o `DataBound` evento faz).

Com esse acréscimo, o usuário agora é mostrado uma mensagem indicando qual página eles estão visitando e é o número de páginas total de dados existe.


[![O número da página atual e o número Total de páginas são exibidas](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)

**Figura 10**: O número da página atual e o número Total de páginas são exibidas ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image20.png))


Além do controle de rótulo, deixe s também adicionar um controle DropDownList que lista os números de página no GridView com a página exibida atualmente selecionada. A ideia aqui é que o usuário pode passar rapidamente da página atual para outro, simplesmente selecionando o novo índice de página na lista suspensa. Comece adicionando uma DropDownList para o Designer, definindo sua `ID` propriedade para `PageList` e marcando a opção Enable AutoPostBack na sua marca inteligente.

Em seguida, retornar para o `DataBound` manipulador de eventos e adicione o seguinte código:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

Esse código começa com a limpeza dos itens no `PageList` DropDownList. Isso pode parecer supérfluo, desde que t de não seria uma espera que o número de páginas para alterar, mas outros usuários podem estar usando o sistema ao mesmo tempo, adicionar ou remover registros da `Products` tabela. Tais inserções ou exclusões podem alterar o número de páginas de dados.

Em seguida, precisamos criar os números de página novamente e já tem o que é mapeado para o GridView atual `PageIndex` selecionada por padrão. Podemos fazer isso com um loop de 0 a `PageCount - 1`, adicionando um novo `ListItem` em cada iteração e a configuração de seu `Selected` a propriedade como true se o índice de iteração atual for igual a s GridView `PageIndex` propriedade.

Por fim, precisamos criar um manipulador de eventos para o s DropDownList `SelectedIndexChanged` evento, que é acionado sempre que o usuário selecionar um item diferente da lista. Para criar esse manipulador de eventos, simplesmente clique duas vezes em DropDownList no Designer, e em seguida, adicione o seguinte código:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

Como mostra a Figura 11, simplesmente alterando o GridView s `PageIndex` propriedade faz com que os dados sejam associadas novamente para o GridView. Em s GridView `DataBound` manipulador de eventos, DropDownList apropriado `ListItem` está selecionado.


[![O usuário é levado automaticamente para o sexto página quando selecionando o Item de lista suspensa página 6](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)

**Figura 11**: O usuário é levado automaticamente para o sexto página quando selecionando o Item de lista suspensa página 6 ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Etapa 5: Adicionando suporte à classificação de Bi-direcional

A adição de suporte à classificação de bi-direcional é tão simple quanto adicionar suporte à paginação basta marcar a opção de habilitar a classificação da marca inteligente GridView s (que define o s GridView [ `AllowSorting` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) para `true`). Isso renderiza cada um dos cabeçalhos dos campos GridView s como LinkButtons que, quando clicado, fazer com que um postback e retornar os dados classificados pela coluna clicada em ordem crescente. Novamente clicando no mesmo cabeçalho LinkButton novamente classifica os dados em ordem decrescente.

> [!NOTE]
> Se você estiver usando uma camada de acesso a dados personalizados em vez de um conjunto de dados tipado, você não pode ter uma opção de habilitar a classificação na marca inteligente s GridView. Somente GridViews associados a fontes de dados com suporte nativo à classificação têm essa caixa de seleção disponível. O conjunto de dados tipado fornece suporte à classificação de out-of-the-box, desde que o DataTable do ADO.NET fornece um `Sort` método que, quando invocado, classifica os s DataTable DataRows usando os critérios especificados.


Se sua DAL não retornar objetos nativamente dar suporte à classificação, que você precisará configurar o ObjectDataSource para passar informações de classificação para a camada de lógica de negócios, que pode classificar os dados ou ter os dados classificados pela DAL. Vamos explorar como classificar dados em que a lógica de negócios e as camadas de acesso de dados em um tutorial futuro.

Os botões de link de classificação são renderizados como hiperlinks do HTML, cujas cores atuais (azuis para um link não visitado e vermelho escuro para um link visitado) estar em conflito com a cor do plano de fundo da linha de cabeçalho. Em vez disso, let s têm todos os links de linha de cabeçalho exibidos em branco, independentemente se eles ve foi visitado ou não. Isso pode ser feito adicionando o seguinte para o `Styles.css` classe:


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

Essa sintaxe indica o uso de texto em branco ao exibir os hiperlinks dentro de um elemento que usa a classe HeaderStyle.

Após essa adição do CSS, ao visitar a página por meio de um navegador sua tela deve ser semelhante a Figura 12. Em particular, a Figura 12 mostra os resultados depois que o link do preço campo s cabeçalho foi clicado.


[![Os resultados tenham sido classificados com o UnitPrice em ordem crescente](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)

**Figura 12**: os resultados tenham sido classificados com o UnitPrice em ordem crescente ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Examinando o fluxo de trabalho de classificação

GridView todos os campos BoundField CheckBoxField, TemplateField, e assim por diante têm um `SortExpression` propriedade que indica a expressão que deve ser usada para classificar os dados quando que o campo s classificando o link do cabeçalho é clicado. O GridView também tem um `SortExpression` propriedade. Quando um cabeçalho de classificação LinkButton é clicado, o GridView atribui que o campo s `SortExpression` de valor para seus `SortExpression` propriedade. Em seguida, os dados são recuperados novamente do ObjectDataSource e classificados de acordo com o GridView s `SortExpression` propriedade. A lista a seguir fornece detalhes sobre a sequência de etapas que ocorre quando um usuário final classifica os dados em um GridView:

1. O s GridView [evento Sorting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) é acionado
2. O s GridView [ `SortExpression` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) é definido como o `SortExpression` do campo cujo cabeçalho classificação LinkButton foi clicado
3. O ObjectDataSource novamente recupera todos os dados da BLL e, em seguida, classifica os dados usando o s GridView `SortExpression`
4. O s GridView `PageIndex` propriedade é redefinida como 0, que significa que, quando o usuário de classificação é retornado para a primeira página de dados (supondo que o suporte à paginação foi implementado)
5. O s GridView [ `Sorted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) é acionado

Como com paginação padrão, a opção de classificação padrão novamente recupera *todos os* os registros da BLL. Ao usar a classificação sem paginação ou ao usar a classificação com não padrão paginação, daí s nenhuma maneira de contornar esse problema (sem cache do banco de dados) de desempenho. No entanto, como veremos em um tutorial futuro, ele é possível classificar os dados com eficiência ao usar a paginação personalizada.

Ao associar um ObjectDataSource a GridView através da lista suspensa na marca inteligente s GridView, cada campo de GridView automaticamente tem seu `SortExpression` atribuída ao nome do campo de dados de propriedade a `ProductsRow` classe. Por exemplo, o `ProductName` s BoundField `SortExpression` é definido como `ProductName`, conforme mostrado na seguinte marcação declarativa:


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

Um campo pode ser configurado para que ele s não classificável, limpando sua `SortExpression` propriedade (atribuindo-o a uma cadeia de caracteres vazia). Para ilustrar isso, imagine que estamos t quiser permitir que nossos clientes classificar nossos produtos pelo preço. O `UnitPrice` BoundField s `SortExpression` propriedade pode ser removida da marcação declarativa ou por meio da caixa de diálogo de campos (que pode ser acessada clicando no link Edit Columns na marca inteligente GridView s).


![Os resultados tenham sido classificados com o UnitPrice em ordem crescente](paging-and-sorting-report-data-cs/_static/image27.png)

**Figura 13**: os resultados tenham sido classificados com o UnitPrice em ordem crescente


Uma vez a `SortExpression` propriedade foi removida para o `UnitPrice` BoundField, o cabeçalho é renderizado como texto em vez de um link, impedindo que os usuários de classificação dos dados pelo preço.


[![Removendo a propriedade SortExpression, os usuários podem não classificar os produtos pelo preço](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)

**Figura 14**: ao remover a propriedade SortExpression, os usuários não podem classificar produtos pelo preço ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Classificação por meio de programação de GridView

Você também pode classificar o conteúdo de GridView programaticamente usando o s GridView [ `Sort` método](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Simplesmente passe o `SortExpression` valor pelo qual classificar juntamente com o [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` ou `Descending`), e os dados do GridView s será classificados novamente.

Imagine que o motivo pelo qual desativamos a classificação pelo `UnitPrice` foi pois estávamos preocupados com a possibilidade que nossos clientes simplesmente compraria apenas os produtos com preços mais baixos. No entanto, queremos incentivá-los a comprar produtos mais caros, portanto, gostaríamos de d-los para poder classificar os produtos pelo preço, mas apenas de preço mais caro para o menor.

Para realizar isso adicione um controle da Web de botão para a página, defina sua `ID` propriedade para `SortPriceDescending`e seu `Text` propriedade para classificar por preço. Em seguida, crie um manipulador de eventos para o botão s `Click` evento clicando duas vezes no controle de botão no Designer. Adicione o seguinte código para este manipulador de eventos:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

Clicar nesse botão retorna o usuário para a primeira página com os produtos classificados por preço, do mais caro para menos cara (veja a Figura 15).


[![Clicar no botão ordena os produtos do mais caro para o menos](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)

**Figura 15**: clique no botão ordena a produtos da mais cara para o menos ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image33.png))


## <a name="summary"></a>Resumo

Neste tutorial que vimos como implementar o padrão de paginação e classificação de recursos, os quais ambos eram tão fácil quanto marcar uma caixa de seleção! Quando um usuário classifica ou páginas de dados, abre-se um fluxo de trabalho semelhante:

1. Um postback massacre
2. Os dados de controle de Web s pré-nível de evento é acionado (`PageIndexChanging` ou `Sorting`)
3. Todos os dados recuperados novamente com o ObjectDataSource
4. Os dados de controle de Web s pós-evento aciona o nível (`PageIndexChanged` ou `Sorted`)

Enquanto a implementação básica paginação e classificação é muito simples, deve ser exercido mais esforço para utilizar a paginação personalizada mais eficiente ou para melhorar ainda mais a interface de paginação ou classificação. Tutoriais do futuros explorará esses tópicos.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Avançar](efficiently-paging-through-large-amounts-of-data-cs.md)
