---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: "Criando uma Interface de usuário personalizadas de classificação (VB) | Microsoft Docs"
author: rick-anderson
description: "Ao exibir uma lista extensa de dados classificados, ele pode ser muito útil para agrupar os dados relacionados com a introdução de linhas de separador. Este tutorial, você verá como cre..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: c9d6229c88e4fd67f384a5ec459ed661f32f0a50
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-customized-sorting-user-interface-vb"></a>Criando uma Interface de usuário personalizadas de classificação (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe) ou [baixar PDF](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> Ao exibir uma lista extensa de dados classificados, ele pode ser muito útil para agrupar os dados relacionados com a introdução de linhas de separador. Neste tutorial, veremos como criar essa interface de usuário uma classificação.


## <a name="introduction"></a>Introdução

Ao exibir uma lista extensa de dados classificados em que há apenas uma série de valores diferentes na coluna classificada, um usuário final pode ser difícil discernir onde, exatamente, ocorrem os limites de diferença. Por exemplo, há 81 produtos no banco de dados, mas apenas nove opções de categoria diferente (oito categorias exclusivas mais o `NULL` opção). Considere o caso de um usuário que está interessado em examinar os produtos que se enquadram na categoria Frutos do mar. De uma página que lista *todos os* dos produtos em uma única GridView, o usuário pode decidir a melhor opção é classificar os resultados por categoria, que serão agrupados todos os produtos de Frutos do mar juntos. Depois da classificação por categoria, o usuário, em seguida, precisa procurar através da lista, procurando onde os produtos agrupados Frutos do mar começar e terminar. Como os resultados são classificados em ordem alfabética pelo nome da categoria localizar os produtos de Frutos do mar não é difícil, mas ela ainda requer verificar atentamente a lista de itens na grade.

Para ajudar a realçar os limites entre grupos classificados, muitos sites usam uma interface do usuário que adiciona um separador entre esses grupos. Separadores de como as mostradas na Figura 1 permite que um usuário localizar um grupo específico e identificar seus limites mais rapidamente, bem como determinar quais grupos distintos existem nos dados.


[![Cada grupo de categoria é claramente identificado](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**Figura 1**: cada grupo de categorias é claramente identificado ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image3.png))


Neste tutorial, veremos como criar essa interface de usuário uma classificação.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Etapa 1: Criando um GridView classificável, padrão

Antes, vamos falar sobre como aumentar o GridView para fornecer a interface aprimorada de classificação, permitem s primeiro criar um GridView padrão, classificável, que lista os produtos. Comece abrindo o `CustomSortingUI.aspx` página o `PagingAndSorting` pasta. Adicionar um controle GridView à página, defina seu `ID` propriedade `ProductList`e associá-lo a um novo ObjectDataSource. Configurar o ObjectDataSource para usar o `ProductsBLL` classe s `GetProducts()` método para selecionar registros.

Em seguida, configure o GridView, de modo que ele contém apenas o `ProductName`, `CategoryName`, `SupplierName`, e `UnitPrice` BoundFields e CheckBoxField descontinuado. Finalmente, configure o GridView para oferecer suporte a classificação, marcando a caixa de seleção Habilitar classificação na marca inteligente s GridView (ou definindo seu `AllowSorting` propriedade `true`). Depois de fazer essas adições para o `CustomSortingUI.aspx` página, a marcação declarativa deve ser semelhante à seguinte:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

Dedicar um tempo para exibir nosso progresso até o momento em um navegador. A Figura 2 mostra o GridView classificável quando seus dados são classificados por categoria em ordem alfabética.


[![O GridView classificável s dados são ordenados por categoria](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**Figura 2**: s a GridView classificável dados são ordenados por categoria ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Etapa 2: Explorar técnicas para adicionar as linhas de separador

Com genérico, sortable GridView concluída, tudo o que permanece é poderá adicionar as linhas de separador em GridView, antes de cada grupo classificado exclusivo. Mas como essas linhas podem ser introduzidas em GridView? Essencialmente, precisamos para iterar pelas linhas s GridView, determinar onde as diferenças ocorrerem entre os valores na coluna classificada e, em seguida, adicione a linha do separador apropriado. Ao pensar sobre esse problema, isso parece natural que a solução está em algum lugar em GridView s `RowDataBound` manipulador de eventos. Conforme abordado no [personalizado formatação com base em dados](../custom-formatting/custom-formatting-based-upon-data-vb.md) tutorial, esse manipulador de eventos é geralmente usado quando aplicar formatação de nível de linha com base nos dados de linha s. No entanto, o `RowDataBound` manipulador de eventos não é a solução aqui, linhas não podem ser adicionado a GridView programaticamente este manipulador de eventos. O GridView s `Rows` coleção, na verdade, é somente leitura.

Para adicionar linhas à GridView temos três opções:

- Adicionar essas linhas de separador de metadados para os dados reais que estão associados a GridView
- Depois que o GridView foi associado aos dados, adicionar adicional `TableRow` coleção de controle de instâncias para o s GridView
- Criar um controle de servidor personalizado que estende o controle GridView e substitui os métodos responsáveis por criar a estrutura de s GridView

Criando um controle de servidor personalizado seria a melhor abordagem se essa funcionalidade foi necessário em várias páginas da web ou em vários sites. No entanto, ele envolveria um pouco de código e uma exploração detalhada dentro da profundidade de tarefas GridView s internas. Portanto, vamos não vai considerar essa opção para este tutorial.

As outras duas opções adicionando linhas de separador para os dados reais que está sendo associada a GridView e manipulando a coleção de controle GridView s após seu foi vinculado - ataques o problema de forma diferente e merecem uma discussão.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Adicionando linhas para os dados associados a GridView

Quando o GridView é vinculado a uma fonte de dados, ele cria um `GridViewRow` para cada registro retornado pela fonte de dados. Portanto, pode inserimos as linhas necessárias separador adicionando registros de separador para a fonte de dados antes de associação a GridView. A Figura 3 ilustra esse conceito.


![Uma técnica envolve a adição de linhas de separador à fonte de dados](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**Figura 3**: uma técnica envolve a adição de linhas de separador à fonte de dados


Posso usar os registros de separador de termo entre aspas, porque não há nenhum registro separador especial; em vez disso, podemos deve alguma forma sinalizar que um determinado registro na fonte de dados serve como um separador em vez de uma linha de dados normal. Para nossos exemplos, podemos re associação um `ProductsDataTable` instância GridView, que é composta de `ProductRows`. Podemos pode sinalizar um registro como uma linha de separador definindo seu `CategoryID` propriedade `-1` (desde que tais um valor pôde inexistente normalmente).

Para utilizar essa técnica d é preciso executar as seguintes etapas:

1. Recuperar programaticamente os dados para associar a GridView (um `ProductsDataTable` instância)
2. Classificar os dados com base em GridView s `SortExpression` e `SortDirection` propriedades
3. Iterar por meio de `ProductsRows` no `ProductsDataTable`, procura onde estão as diferenças entre a coluna classificada
4. No limite de cada grupo, inserir um registro de separador `ProductsRow` instância na DataTable que tem s `CategoryID` definido como `-1` (ou qualquer designação foi decidida para marcar um registro como um registro de separador)
5. Depois de injetando as linhas de separador, programaticamente associar os dados a GridView

Além dessas cinco etapas, d também precisamos fornecer um manipulador de eventos para o s GridView `RowDataBound` eventos. Aqui, podemos d Verifique cada `DataRow` e determinar se ele foi um separador de linha, um cujo `CategoryID` configuração era `-1`. Nesse caso, nós d provavelmente desejará ajustar a formatação ou o texto exibido na célula (s).

Usando essa técnica para injetar os limites do grupo de classificação exige um pouco mais trabalho do descritas acima, você precisa também fornecer um manipulador de eventos para o s GridView `Sorting` eventos e manter controlam do `SortExpression` e `SortDirection` valores.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipulando o s GridView controlar coleção depois s foi ligação de dados

Em vez de mensagens de dados antes de associação a GridView, podemos adicionar linhas separador *depois* foi associados os dados a GridView. O processo de associação de dados cria a hierarquia de controle do GridView s, que, na realidade, é simplesmente um `Table` instância composto de um conjunto de linhas, cada uma delas é composta de um conjunto de células. Especificamente, a coleção de controle GridView s contém um `Table` objeto na raiz, um `GridViewRow` (que é derivada do `TableRow` classe) para cada registro da `DataSource` associado a GridView e um `TableCell` objeto em cada `GridViewRow` instância para cada campo de dados de `DataSource`.

Para adicionar linhas de separador entre cada grupo de classificação, é possível manipular diretamente essa hierarquia de controle depois que ela foi criada. Podemos pode ter certo de que a hierarquia de controle GridView s foi criada pela última vez no momento em que a página está sendo processada. Portanto, essa abordagem substitui o `Page` classe s `Render` método, no ponto em que a hierarquia de controle final s GridView é atualizada para incluir as linhas de separador necessários. A Figura 4 ilustra esse processo.


[![Uma técnica alternativa manipula a hierarquia de controle GridView s](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**Figura 4**: uma técnica alternativa manipula a s GridView hierarquia de controle ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image10.png))


Para este tutorial, vamos usar essa segunda abordagem para personalizar a experiência de usuário de classificação.

> [!NOTE]
> O código m apresentando neste tutorial se baseia no exemplo fornecido em [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) entrada de blog s [reproduzindo um Bit com o agrupamento de classificação de GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Etapa 3: Adicionando as linhas de separação para a hierarquia de controle GridView s

Como queremos adicionar linhas separador para a hierarquia de controle GridView s depois de sua hierarquia de controle foi criada e criada pela última vez em que visita de página, queremos realizar essa adição no final do ciclo de vida da página, mas antes do c GridView real hierarquia ontrole foi renderizada em HTML. É possível ponto mais recente em que podemos pode fazer isso a `Page` classe s `Render` evento, que podemos substituir em nossa classe code-behind usando a assinatura de método a seguir:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

Quando o `Page` classe s original `Render` método é invocado `base.Render(writer)` cada um dos controles na página será renderizada, gerando a marcação com base em sua hierarquia de controle. Portanto, é fundamental que ambos os chamamos `base.Render(writer)`, de modo que a página é renderizada, e que podemos manipular o s GridView controlam hierarquia antes de chamar `base.Render(writer)`, de modo que as linhas de separador foram adicionadas para a hierarquia de controle GridView s antes dele s foi processado.

Para inserir os cabeçalhos do grupo de classificação é preciso primeiro garantir que o usuário solicitou que os dados ser classificados. Por padrão, o conteúdo de s GridView não é classificado e, portanto, podemos don precisa inserir qualquer classificação cabeçalhos do grupo.

> [!NOTE]
> Se você quiser que o GridView sejam classificados por uma determinada coluna quando a página for carregada pela primeira vez, chame o GridView s `Sort` método na primeira visita de página (mas não em postagens subsequentes). Para fazer isso, adicione a chamada no `Page_Load` manipulador de eventos em um `if (!Page.IsPostBack)` condicional. Voltar para o [paginação e classificando dados de relatório](paging-and-sorting-report-data-vb.md) informações tutoriais para saber mais sobre o `Sort` método.


Supondo que os dados foram classificados, nossa próxima tarefa é determinar quais colunas de dados foi classificados por e, em seguida verificar as linhas procurando as diferenças na coluna s valores. O código a seguir garante que os dados foram classificados e localiza a coluna pela qual os dados foram classificados:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

Se tiver de GridView ainda seja classificada, a s GridView `SortExpression` propriedade será não tenha sido definida. Portanto, só queremos adicionar linhas separador se essa propriedade tem um valor. Em caso afirmativo, em seguida, precisamos determinar o índice da coluna pela qual os dados foi classificados. Isso é feito pelo loop através de GridView s `Columns` coleção, procurando a coluna cujo `SortExpression` propriedade é igual a s GridView `SortExpression` propriedade. Além do índice da coluna s, podemos também pegue o `HeaderText` propriedade, que é usada ao exibir as linhas de separador.

Com o índice da coluna pela qual os dados são classificados, a etapa final é enumerar linhas de GridView. Para cada linha, é preciso determinar se a coluna classificada s valor diferencia o valor anterior da linha s classificados coluna s. Se assim, é necessário inserir um novo `GridViewRow` instância na hierarquia de controle. Isso é feito com o código a seguir:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

Esse código inicia referenciando programaticamente o `Table` objeto encontrado na raiz da hierarquia do controle GridView s e criando uma variável de cadeia de caracteres denominada `lastValue`. `lastValue`é usado para comparar o valor da coluna de s classificados de linha atual com o valor de linha s anterior. Em seguida, o s GridView `Rows` coleção é enumerada e para cada linha, o valor da coluna classificada é armazenado no `currentValue` variável.

> [!NOTE]
> Para determinar o valor da coluna s classificados linha específica, use a célula s `Text` propriedade. Isso funciona bem para BoundFields, mas não funcionam conforme desejado para TemplateFields, CheckBoxFields e assim por diante. Vamos examinar como a conta para campos de GridView alternativos em breve.


O `currentValue` e `lastValue` variáveis são comparadas. Se eles forem diferentes, precisamos adicionar uma nova linha do separador para a hierarquia de controle. Isso é feito por meio da determinação do índice do `GridViewRow` no `Table` objeto s `Rows` coleção, criando novos `GridViewRow` e `TableCell` instâncias e, em seguida, adicionando o `TableCell` e `GridViewRow` para o hierarquia de controle.

Observe que o separador de linha única de s `TableCell` está formatado, de modo que se estende por toda a largura do GridView, é formatado usando o `SortHeaderRowStyle` classe CSS e tem seu `Text` , que mostra ambos os grupo de classificação de nome de propriedade (como a categoria) e o valor de s do grupo (como bebidas). Por fim, `lastValue` é atualizado para o valor de `currentValue`.

A classe CSS usada para formatar a linha de cabeçalho de grupo classificação `SortHeaderRowStyle` precisa ser especificado no `Styles.css` arquivo. Fique à vontade para usar quaisquer configurações de estilo contestar para você. Usei o seguinte:


[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

Com o código atual, a interface de classificação adiciona cabeçalhos de grupo de classificação ao classificar por qualquer BoundField (consulte a Figura 5, que mostra uma captura de tela, ao classificar por fornecedor). No entanto, ao classificar por qualquer outro tipo de campo (como um CheckBoxField ou TemplateField), os cabeçalhos do grupo de classificação são têm a ser localizada (veja a Figura 6).


[![A Interface de classificação inclui cabeçalhos de grupo de classificação ao classificar por BoundFields](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**Figura 5**: A classificação Interface inclui classificação grupo cabeçalhos quando a classificação por BoundFields ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image13.png))


[![Os cabeçalhos do grupo de classificação são ausente ao classificar um CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**Figura 6**: os cabeçalhos de grupo de classificação são ausente ao classificar um CheckBoxField ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image16.png))


O motivo pelo qual os cabeçalhos do grupo de classificação estiverem ausentes, quando a classificação por um CheckBoxField é porque o código atualmente usa apenas o `TableCell` s `Text` propriedade para determinar o valor da coluna de classificação para cada linha. Para CheckBoxFields, o `TableCell` s `Text` propriedade é uma cadeia de caracteres vazia; em vez disso, o valor está disponível através de um controle de caixa de seleção Web que reside o `TableCell` s `Controls` coleção.

Para lidar com tipos de campo que não sejam BoundFields, é preciso aumentar o código onde o `currentValue` variável é atribuída para verificar a existência de uma caixa de seleção no `TableCell` s `Controls` coleção. Em vez de usar `currentValue = gvr.Cells(sortColumnIndex).Text`, substitua este código a seguir:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

Esse código examina a coluna classificada `TableCell` para a linha atual determinar se há qualquer controle no `Controls` coleção. Se houver, e o primeiro controle é uma caixa de seleção, o `currentValue` variável for definida como Sim ou não, dependendo da caixa de seleção s `Checked` propriedade. Caso contrário, o valor é extraído o `TableCell` s `Text` propriedade. Essa lógica pode ser replicada para lidar com a classificação para qualquer TemplateFields que podem existir em GridView.

Com a adição de código acima, os cabeçalhos do grupo de classificação agora estão presentes quando a classificação por CheckBoxField Descontinuado (consulte a Figura 7).


[![Os cabeçalhos do grupo de classificação está agora presente ao classificar um CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**Figura 7**: cabeçalhos de grupo a classificação está agora presente ao classificar um CheckBoxField ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image19.png))


> [!NOTE]
> Se você tiver produtos com `NULL` valores para o banco de dados de `CategoryID`, `SupplierID`, ou `UnitPrice` campos, esses valores serão exibidos como cadeias de caracteres vazias em GridView por padrão, o que significa que o texto da linha s separador para esses produtos com `NULL`valores serão lidas como categoria: (ou seja, não há s sem nome após a categoria: como com categoria: Bebidas). Se você desejar um valor exibido aqui você pode definir o BoundFields [ `NullDisplayText` propriedade](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) para o texto que deseja exibir ou você pode adicionar uma instrução condicional no método de renderização ao atribuir a `currentValue` para o separador linha s `Text` propriedade.


## <a name="summary"></a>Resumo

GridView não inclui muitas opções integradas para personalizar a interface de classificação. No entanto, com um pouco de código de baixo nível, ele possível ajustar a hierarquia de controle GridView s para criar uma interface mais personalizada. Neste tutorial, vimos como adicionar uma linha de separador de grupo de classificação para um controle GridView classificável, que identifica mais facilmente os grupos distintos e esses limites de grupos. Para obter exemplos adicionais de interfaces de classificação personalizadas, check-out [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [alguns ASP.NET 2.0 GridView classificação dicas e truques](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) entrada de blog.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Anterior](sorting-custom-paged-data-vb.md)
