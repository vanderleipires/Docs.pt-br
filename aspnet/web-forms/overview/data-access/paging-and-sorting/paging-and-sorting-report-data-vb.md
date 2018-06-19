---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: Paginação e classificação relatam dados (VB) | Microsoft Docs
author: rick-anderson
description: Paginação e a classificação são dois recursos muito comuns ao exibir dados em um aplicativo online. Neste tutorial vamos dar uma olhada primeiro adicionar a classificação e...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: c5e7e110d436caa7b7526eae105fde601367007a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887233"
---
<a name="paging-and-sorting-report-data-vb"></a>Paginação e classificando dados de relatório (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe) ou [baixar PDF](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> Paginação e a classificação são dois recursos muito comuns ao exibir dados em um aplicativo online. Neste tutorial vamos dar uma olhada primeiro adicionar classificação e paginação aos nossos relatórios, que, em seguida, criaremos em tutoriais futuros.


## <a name="introduction"></a>Introdução

Paginação e a classificação são dois recursos muito comuns ao exibir dados em um aplicativo online. Por exemplo, ao procurar manuais do ASP.NET em uma livraria online, pode haver centenas de livros, mas o relatório lista os resultados da pesquisa lista apenas dez correspondências por página. Além disso, os resultados podem ser classificados por título, preço, contagem de páginas, nome do autor e assim por diante. Durante os últimos 23 tutoriais examinar como criar uma variedade de relatórios, incluindo interfaces que permitem adicionar, editar e excluir dados, podemos ve não visto como classificar dados e o único exemplos de paginação é var visto foram com DetailsView e FormView controles.

Neste tutorial, veremos como adicionar classificação e paginação aos nossos relatórios, que podem ser feitos verificando simplesmente algumas caixas de seleção. Infelizmente, essa implementação simples tem suas desvantagens que a interface classificação deixa um pouco para ser desejado e as rotinas de paginação não são projetadas para paginação eficiente grandes conjuntos de resultados. Tutoriais futuros explorará como superar as limitações de-de-prontos paginação e a classificação de soluções.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Etapa 1: Adicionar a paginação e a classificação Tutorial páginas da Web

Antes de começar este tutorial, permitem s primeiro dedicar um tempo para adicionar as páginas ASP.NET que vamos precisar para este tutorial e as próximas três. Comece criando uma nova pasta no projeto chamado `PagingAndSorting`. Em seguida, adicione as seguintes páginas ASP.NET cinco nesta pasta, ter todos os configurado para usar a página mestra `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Crie uma pasta PagingAndSorting e adicionar as páginas ASP.NET Tutorial](paging-and-sorting-report-data-vb/_static/image1.png)

**Figura 1**: Crie uma pasta PagingAndSorting e adicionar as páginas ASP.NET Tutorial


Em seguida, abra o `Default.aspx` página e arraste o `SectionLevelTutorialListing.ascx` controle de usuário da `UserControls` pasta para a superfície de Design. Este controle de usuário que criamos no [páginas mestras e navegação de Site](../introduction/master-pages-and-site-navigation-vb.md) tutorial, enumera o mapa do site e exibe os tutoriais na seção atual em uma lista com marcadores.


![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](paging-and-sorting-report-data-vb/_static/image2.png)

**Figura 2**: adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx


Para ter a lista com marcadores exibem a paginação e a classificação tutoriais que criará, precisamos adicioná-los para o mapa do site. Abra o `Web.sitemap` de arquivo e adicione a seguinte marcação após a marcação de nó de mapa de site de edição, inserir e excluir:


[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]


![Atualizar o mapa de Site para incluir as novas páginas ASP.NET](paging-and-sorting-report-data-vb/_static/image3.png)

**Figura 3**: atualizar o mapa de Site para incluir as novas páginas ASP.NET


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Etapa 2: Exibir informações de produto em um controle GridView

Antes de realmente implementamos paginação e recursos de classificação, deixe-s primeiro criar um padrão não-srotable, GridView não-paginável que lista as informações de produto. Esta é uma tarefa é var feita várias vezes durante esta série de tutoriais assim essas etapas deve estar familiarizado. Comece abrindo o `SimplePagingSorting.aspx` página e arraste um controle GridView da caixa de ferramentas para o Designer, definindo seu `ID` propriedade `Products`. Em seguida, crie um novo ObjectDataSource que usa a classe ProductsBLL s `GetProducts()` método para retornar todas as informações de produto.


![Recuperar informações sobre todos os produtos usando o método GetProducts()](paging-and-sorting-report-data-vb/_static/image4.png)

**Figura 4**: recuperar informações sobre todos os produtos usando o método GetProducts()


Como esse relatório é um relatório somente leitura, não há s não precisa mapear o s ObjectDataSource `Insert()`, `Update()`, ou `Delete()` métodos correspondente `ProductsBLL` métodos; portanto, escolha (nenhum) na lista suspensa para a atualização, inserção, e excluir guias.


![Escolha (nenhum) de opção na lista suspensa na atualização, inserção e excluir guias](paging-and-sorting-report-data-vb/_static/image5.png)

**Figura 5**: escolha (nenhum) de opção na lista suspensa na atualização, inserção e excluir guias


Em seguida, permitir que s personalizar os campos de s GridView para que sejam exibidos apenas os nomes de produtos, fornecedores, categorias, preços e status descontinuados. Além disso, fique à vontade para fazer qualquer formatação de nível de campo for alterada, como ajuste de `HeaderText` propriedades ou formatação do preço como uma moeda. Após essas alterações, a marcação declarativa do GridView s deve ser semelhante ao seguinte:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

A Figura 6 mostra nosso progresso até o momento quando visualizada através de um navegador. Observe que a página lista todos os produtos em uma única tela, mostrando cada nome de produto s, a categoria, o fornecedor, o preço e descontinuado status.


[![Cada um dos produtos listados](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**Figura 6**: cada um dos produtos listados ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-vb/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Etapa 3: Adicionando suporte à paginação

Listando *todos os* dos produtos em uma única tela pode levar a sobrecarga de informações para o usuário usando os dados. Para ajudar a tornar os resultados mais gerenciáveis, podemos dividir os dados em páginas menores de dados e permitir que o usuário percorrer os dados em uma página por vez. Para realizar isso basta marcar a caixa de seleção Habilitar paginação de marca inteligente GridView s (Isso define o GridView s [ `AllowPaging` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) para `true`).


[![Marque a caixa de seleção Habilitar paginação para adicionar suporte à paginação](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**Figura 7**: marcar a opção habilitar paginação para adicionar suporte para paginação ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-vb/_static/image11.png))


Habilitar paginação limita o número de registros mostrados por página e adiciona um *interface paginação* a GridView. A interface de paginação padrão, mostrada na Figura 7, é uma série de números de página, permitindo que o usuário navegue rapidamente de uma página de dados para outro. Essa interface de paginação deve parecer familiar, como podemos ve assistir ao adicionar o suporte à paginação para os controles de DetailsView e FormView nos tutoriais anteriores.

Controles de DetailsView e FormView mostram apenas um único registro por página. O GridView, no entanto, a consulta seu [ `PageSize` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) para determinar quantos registros devem ser mostrados por página (essa propriedade padroniza como um valor de 10).

Essa interface de paginação GridView, DetailsView e FormView s pode ser personalizado usando as seguintes propriedades:

- `PagerStyle` indica as informações de estilo para a interface de paginação; pode especificar configurações como `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`e assim por diante.
- `PagerSettings` contém muitas propriedades que pode personalizar a funcionalidade da interface de paginação; `PageButtonCount` indica o número máximo de números de página numérico exibidos na interface de paginação (o padrão é 10); o [ `Mode` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indica como a interface de paginação funciona e pode ser definida como: 

    - `NextPrevious` mostra um botões Próximo e anterior, permitindo que o usuário para a etapa frente ou para trás uma página por vez
    - `NextPreviousFirstLast` Além dos botões Próximo e anterior, primeiro e último botões também são incluídos, permitindo que o usuário mover rapidamente para a primeira ou última página de dados
    - `Numeric` mostra uma série de números de página, permitindo que o usuário imediatamente saltar para qualquer página
    - `NumericFirstLast` Além dos números de página inclui botões primeiro e último, permitindo que o usuário mover rapidamente para a primeira ou última página de dados. os botões de primeira/última são exibidos apenas se todos os números de páginas numéricas de não podem ser ajustado

Além disso, o GridView, DetailsView e FormView oferecem o `PageIndex` e `PageCount` propriedades, que indicam a página atual que está sendo exibido e o número total de páginas de dados, respectivamente. O `PageIndex` propriedade é indexada começando com 0, que significa que, ao exibir a primeira página de dados `PageIndex` será igual a 0. `PageCount`, por outro lado, inicia a contagem em 1, o que significa que `PageIndex` é limitado aos valores entre 0 e `PageCount - 1`.

Permitir que o s dedicar um tempo para melhorar a aparência de padrão de nossa interface de paginação do GridView s. Especificamente, permitem que s que a interface de paginação alinhado à direita com um plano de fundo cinza claro. Em vez de definir essas propriedades diretamente por meio de GridView s `PagerStyle` propriedade permitem s criar uma classe CSS em `Styles.css` chamado `PagerRowStyle` e, em seguida, atribua o `PagerStyle` s `CssClass` propriedade por meio de nosso tema. Comece abrindo `Styles.css` e a definição da classe adicionando o seguinte CSS:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

Em seguida, abra o `GridView.skin` de arquivos no `DataWebControls` pasta dentro de `App_Themes` pasta. Conforme abordado no *páginas mestras e navegação de Site* tutoriais, capa arquivos podem ser usados para especificar os valores de propriedade padrão para um controle da Web. Portanto, aumentar as configurações existentes para incluir a configuração do `PagerStyle` s `CssClass` propriedade `PagerRowStyle`. Além disso, permitem s configurar a interface de paginação para no máximo Mostrar botões de cinco páginas numéricas usando o `NumericFirstLast` interface de paginação.


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>A experiência do usuário de paginação

A Figura 8 mostra a página da web quando visitado por meio de um navegador depois que o GridView s habilitar paginação caixa de seleção foi verificada e o `PagerStyle` e `PagerSettings` configurações foram feitas por meio de `GridView.skin` arquivo. Observação apenas dez registros são exibidos e a interface de paginação indica que estamos estiver exibindo a primeira página de dados.


[![Com a paginação habilitada, somente um subconjunto dos registros são exibidos em um momento](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**Figura 8**: com paginação habilitada, somente um subconjunto dos registros são exibidos em um momento ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-vb/_static/image14.png))


Quando o usuário clica em um dos números de página na interface de paginação, um postback tem lugar e a página recarrega mostrando que solicitou os registros de páginas. A Figura 9 mostra os resultados depois de aceitar para exibir a página final de dados. Observe que a página final tem apenas um registro; Isso ocorre porque há 81 registros no total, resultando em oito páginas de 10 registros por página mais de uma página com um registro única.


[![Clicando em um número de página causa um Postback e mostra o subconjunto apropriado de registros](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**Figura 9**: clicando em um número de página causa um Postback e mostra o subconjunto de registros apropriados ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-vb/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Fluxo de trabalho paginação s do servidor

Quando o usuário final clicar em um botão na interface de paginação, um postback tem lugar e começa o seguinte fluxo de trabalho do lado do servidor:

1. S GridView (ou DetailsView ou FormView) `PageIndexChanging` evento ser acionado
2. ObjectDataSource novamente solicitações *todos os* dos dados da BLL; o s GridView `PageIndex` e `PageSize` valores de propriedade são usados para determinar quais registros retornados de BLL precisam ser exibidos em GridView
3. O GridView s `PageIndexChanged` evento ser acionado

Na etapa 2, o ObjectDataSource solicitações novamente todos os dados da fonte de dados. Esse estilo de paginação é conhecido como *paginação padrão*, como o comportamento de paginação de s usado por padrão ao definir o `AllowPaging` propriedade `true`. Padrão paginar os dados controle Web ingenuamente recupera todos os registros de cada página de dados, mesmo que apenas um subconjunto de registros na verdade são renderizados em HTML que s enviada para o navegador. A menos que o banco de dados é armazenado em cache pelo BLL ou ObjectDataSource, paginação padrão não está funciona para suficientemente grandes conjuntos de resultados ou para aplicativos web com muitos usuários simultâneos.

O seguinte tutorial, examinaremos como implementar *paginação personalizada*. Com a paginação personalizada, você pode instruir especificamente ObjectDataSource para recuperar apenas o conjunto exato de registros necessários para a página solicitada de dados. Como você pode imaginar, paginação personalizada melhora significativamente a eficiência de paginação por meio de conjuntos de resultados grandes.

> [!NOTE]
> Enquanto a paginação padrão não é adequada quando a paginação por meio de suficientemente grandes conjuntos de resultados ou para sites com muitos usuários simultâneos, observe que a paginação personalizada requer mais esforço para implementar e a alterações e não é tão simple quanto a verificação de uma caixa de seleção (como é o padrão paginação). Portanto, a paginação padrão pode ser a opção ideal para pequenos e de baixo tráfego de sites ou quando conjuntos de paginação por meio de resultados relativamente pequeno, como s muito mais fácil e rápido implementar.


Por exemplo, se sabemos que nunca teremos mais de 100 produtos em nosso banco de dados, o ganho de desempenho mínimo aproveitado por paginação personalizada provavelmente é deslocado pelo esforço necessário para implementá-la. Se, no entanto, talvez um dia temos milhares ou dezenas de milhares de produtos, *não* implementar paginação personalizada seria muito degradar a escalabilidade do nosso aplicativo.

## <a name="step-4-customizing-the-paging-experience"></a>Etapa 4: Personalizar a experiência de paginação

Os controles da Web de dados fornecem uma série de propriedades que podem ser usadas para aprimorar a experiência do usuário s paginação. O `PageCount` propriedade, por exemplo, indica o número total de páginas existe, enquanto o `PageIndex` propriedade indica a página atual que está sendo visitada e pode ser definida para mover rapidamente de um usuário para uma página específica. Para ilustrar como usar essas propriedades para aprimorar a experiência do usuário s paginação, permitem que o s adicionar um rótulo da Web controlam a nossa página que informa ao usuário que página eles re visitando atualmente, junto com um controle DropDownList que permita ir rapidamente para uma determinada página .

Primeiro, adicione um controle de rótulo Web para sua página, defina seu `ID` propriedade `PagingInformation`e limpar sua `Text` propriedade. Em seguida, crie um manipulador de eventos para o s GridView `DataBound` evento e adicione o seguinte código:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

Este manipulador de eventos atribui o `PagingInformation` rótulo s `Text` propriedade a uma mensagem informando ao usuário a página está visitando atualmente `Products.PageIndex + 1` fora do número total de páginas `Products.PageCount` (adicionamos 1 para o `Products.PageIndex` propriedade porque `PageIndex` é indexada começando com 0). Escolhi atribuir este rótulo s `Text` propriedade no `DataBound` manipulador de eventos em vez do `PageIndexChanged` manipulador de eventos porque a `DataBound` evento é acionado sempre que dados são associados a GridView enquanto o `PageIndexChanged` apenas o manipulador de eventos Acionado quando o índice de página é alterado. Quando o GridView é inicialmente dados vinculados na primeira página visita o `PageIndexChanging` fire do evento t (enquanto o `DataBound` does de evento).

Com essa adição, o usuário agora é mostrado uma mensagem indicando que página visitado e é o número total de páginas de dados.


[![O número da página atual e o número Total de páginas são exibidas](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**Figura 10**: O número da página atual e o número Total de páginas são exibidas ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-vb/_static/image20.png))


O controle de rótulo, além de permitir que s também adicionar um controle DropDownList que lista os números de página em GridView com a página exibida atualmente selecionada. A ideia aqui é que o usuário pode ir rapidamente da página atual para outro, basta selecionar o novo índice de página na lista suspensa. Comece adicionando DropDownList para o Designer, definindo seu `ID` propriedade `PageList` e marcando a opção Enable AutoPostBack de marca inteligente.

Em seguida, retornar para o `DataBound` manipulador de eventos e adicione o seguinte código:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

Esse código começa limpando os itens a `PageList` DropDownList. Isso pode parecer supérfluo, desde que um t de seria esperar que o número de páginas para alterar, mas outros usuários podem ser usando o sistema simultaneamente, adicionando ou removendo os registros da `Products` tabela. As inserções ou exclusões podem alterar o número de páginas de dados.

Em seguida, precisamos criar os números de página novamente e fazer com que mapeia para o GridView atual `PageIndex` selecionada por padrão. Podemos fazer isso com um loop de 0 a `PageCount - 1`, adicionando um novo `ListItem` em cada iteração e a configuração de seu `Selected` a propriedade como true se o índice de iteração atual for igual a s GridView `PageIndex` propriedade.

Finalmente, precisamos criar um manipulador de eventos para o s DropDownList `SelectedIndexChanged` evento, que é acionado sempre que o usuário selecionar um item diferente da lista. Para criar este manipulador de eventos, simplesmente clique duas vezes em DropDownList no Designer, adicione o seguinte código:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

Como mostra a Figura 11, simplesmente alterando a s GridView `PageIndex` propriedade faz com que os dados sejam associadas novamente a GridView. Em GridView s `DataBound` manipulador de eventos, DropDownList apropriado `ListItem` está selecionado.


[![O usuário é direcionado automaticamente para a sexta página quando selecionar o Item da lista suspensa página 6](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**Figura 11**: O usuário é direcionado automaticamente para a sexta página quando selecionar o Item da lista suspensa página 6 ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-vb/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Etapa 5: Adicionando suporte à classificação bidirecional

Adicionando suporte à classificação bidirecional é tão simple quanto a adição de suporte à paginação basta marcar a opção de habilitar a classificação de marca inteligente GridView s (que define o GridView s [ `AllowSorting` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) para `true`). Isso renderiza cada um dos cabeçalhos dos campos GridView s como botões de link que, quando clicado, causar um postback e retornar os dados classificados pela coluna clicada em ordem crescente. Clique o mesmo cabeçalho LinkButton novamente novamente para classificar os dados em ordem decrescente.

> [!NOTE]
> Se você estiver usando uma camada de acesso a dados personalizados em vez de um conjunto de dados tipado, você não pode ter uma opção de habilitar a classificação da marca inteligente de s GridView. Somente GridViews associados a fontes de dados que dão suporte nativo de classificação têm essa caixa de seleção disponível. O conjunto de dados tipado oferece suporte à classificação de caixa como o DataTable do ADO.NET fornece um `Sort` método que, quando chamado, classifica os s DataTable DataRows usando os critérios especificados.


Se seu DAL não retornar objetos nativamente suporte classificação, que você precisará configurar o ObjectDataSource para transmitir informações de classificação para a camada de lógica de negócios, que pode classificar os dados ou os dados classificado pela DAL. Exploraremos como classificar dados em que a lógica de negócios e as camadas de acesso de dados em um tutorial futuras.

Os botões de link de classificação são renderizados como hiperlinks HTML, cujas cores atuais (azuis para um link não visitado e vermelho escuro para um link visitado) podem conflitar com a cor de plano de fundo da linha do cabeçalho. Em vez disso, permitem s tem todos os links de linha de cabeçalho, exibidos em branco, independentemente se eles ve foi visitado ou não. Isso pode ser feito adicionando o seguinte para o `Styles.css` classe:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

Indica a seguinte sintaxe para usar texto em branco ao exibir os hiperlinks dentro de um elemento que usa a classe HeaderStyle.

Após essa adição de CSS, ao visitar a página por meio de um navegador sua tela deve ser semelhante à Figura 12. Em particular, a Figura 12 mostra os resultados depois que o link do preço campo s cabeçalho foi clicado.


[![Os resultados tenham sido classificados pelo UnitPrice em ordem crescente](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**Figura 12**: os resultados tenham sido classificados pelo UnitPrice em ordem crescente ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-vb/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Examinando o fluxo de trabalho de classificação

GridView todos os campos BoundField CheckBoxField, TemplateField e assim por diante tem um `SortExpression` propriedade que indica a expressão que deve ser usada para classificar os dados quando que s campo link do cabeçalho de classificação é clicado. GridView também tem um `SortExpression` propriedade. Quando um cabeçalho de classificação LinkButton é clicado, o GridView atribui esse campo s `SortExpression` o valor para o seu `SortExpression` propriedade. Em seguida, os dados são recuperados novamente do ObjectDataSource e classificados de acordo com o GridView s `SortExpression` propriedade. A lista a seguir fornece detalhes sobre a sequência de etapas que ocorre quando um usuário final classifica os dados em um controle GridView:

1. O GridView s [evento Sorting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) é acionado
2. O GridView s [ `SortExpression` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) é definido como o `SortExpression` do campo cujo cabeçalho classificação LinkButton foi clicado
3. ObjectDataSource recupera todos os dados de BLL novamente e, em seguida, classifica os dados usando o s GridView `SortExpression`
4. O GridView s `PageIndex` propriedade é redefinida para 0, que significa que quando o usuário a classificação é retornado para a primeira página de dados (supondo que o suporte à paginação foi implementado)
5. O GridView s [ `Sorted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) é acionado

Como com paginação padrão, a opção de classificação padrão recupera novamente *todos os* os registros de BLL. Ao usar a classificação sem paginação ou ao usar a classificação com não padrão paginação, há s nenhuma maneira de contornar esse problema (no cache do banco de dados) de desempenho. No entanto, como você verá um tutorial futuras, ele possível classificar os dados com eficiência ao usar a paginação personalizada.

Ao associar um ObjectDataSource a GridView através da lista suspensa na marca inteligente s GridView, cada campo de GridView automaticamente tem seu `SortExpression` propriedade atribuída para o nome do campo de dados a `ProductsRow` classe. Por exemplo, o `ProductName` BoundField s `SortExpression` é definido como `ProductName`, conforme mostrado na seguinte marcação declarativa:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

Um campo pode ser configurado para que ele s não classificável limpando seu `SortExpression` propriedade (atribuí-la a uma cadeia de caracteres vazia). Para ilustrar isso, imagine que nós não foi quiser permitir que os clientes classificar nossos produtos por preço. O `UnitPrice` BoundField s `SortExpression` propriedade pode ser removida da marcação declarativa ou por meio da caixa de diálogo de campos (que pode ser acessada clicando no link Editar colunas na marca inteligente GridView s).


![Os resultados tenham sido classificados pelo UnitPrice em ordem crescente](paging-and-sorting-report-data-vb/_static/image27.png)

**Figura 13**: os resultados tenham sido classificados pelo UnitPrice em ordem crescente


Uma vez o `SortExpression` propriedade foi removida para o `UnitPrice` BoundField, o cabeçalho é renderizado como texto em vez de um link, impedindo que os usuários de classificação dos dados por preço.


[![Removendo a propriedade SortExpression, os usuários não podem classificar os produtos por preço](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**Figura 14**: Removendo a propriedade SortExpression, os usuários não podem classificar os produtos por preço ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-vb/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Classificando programaticamente GridView

Você também pode classificar o conteúdo de GridView programaticamente usando o s GridView [ `Sort` método](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Basta passar o `SortExpression` valor pelo qual classificar junto com o [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` ou `Descending`), e os dados do GridView s será reclassificados.

Imagine que o motivo desativamos a classificação pelo `UnitPrice` porque estamos preocupados com a possibilidade que nossos clientes simplesmente compraria apenas os produtos com preços mais baixo. No entanto, queremos recomende comprar os produtos mais caros, portanto gostamos de d-los para poder classificar os produtos por preço, mas apenas de preço mais caro para o menor.

Para realizar isso adicionar um controle de botão Web para a página, defina seu `ID` propriedade para `SortPriceDescending`e sua `Text` propriedade para classificar por preço. Em seguida, crie um manipulador de eventos para o botão s `Click` eventos clicando duas vezes o controle de botão no Designer. Adicione o seguinte código para este manipulador de eventos:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

Este botão retorna o usuário para a primeira página com os produtos classificados por preço, do mais caro para menos cara (veja a Figura 15).


[![Clicar no botão ordena os produtos do mais caro para o menor](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**Figura 15**: clicar no botão ordena os produtos do mais caro para o menor ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-vb/_static/image33.png))


## <a name="summary"></a>Resumo

Neste tutorial, vimos como implementar o padrão de paginação e recursos de classificação, dos quais eram tão fácil quanto marcar uma caixa de seleção! Quando um usuário classifica ou páginas de dados, abre-se um fluxo de trabalho semelhante:

1. Um postback tem lugar
2. Os dados de controle de Web s previamente nível evento ser acionado (`PageIndexChanging` ou `Sorting`)
3. Novamente, todos os dados são recuperados por ObjectDataSource
4. Os dados de controle de Web s pós-nível evento ser acionado (`PageIndexChanged` ou `Sorted`)

Enquanto a implementação básica paginação e a classificação é muito simples, mais esforço deve exercido utilizar a paginação personalizada mais eficiente ou para aumentar ainda mais a interface de paginação ou classificação. Tutoriais futuros explorará estes tópicos.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](creating-a-customized-sorting-user-interface-cs.md)
> [Próximo](efficiently-paging-through-large-amounts-of-data-vb.md)
