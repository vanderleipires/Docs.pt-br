---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: Criando uma Interface de usuário de classificação personalizada (VB) | Microsoft Docs
author: rick-anderson
description: Ao exibir uma lista longa de dados classificados, pode ser muito útil para agrupar os dados relacionados com a introdução de linhas de separador. Neste tutorial, veremos como cre...
ms.author: aspnetcontent
ms.date: 08/15/2006
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e94bbda0ca89b409515db1e223637e3a554cd31
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835862"
---
<a name="creating-a-customized-sorting-user-interface-vb"></a>Criando uma Interface de usuário de classificação personalizada (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe) ou [baixar PDF](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> Ao exibir uma lista longa de dados classificados, pode ser muito útil para agrupar os dados relacionados com a introdução de linhas de separador. Neste tutorial, veremos como criar cada interface do usuário uma classificação.


## <a name="introduction"></a>Introdução

Ao exibir uma lista longa de dados classificados em que há apenas alguns valores diferentes na coluna classificada, um usuário final talvez seja difícil de discernir onde, exatamente, ocorrem os limites de diferença. Por exemplo, há 81 produtos no banco de dados, mas apenas nove opções de categoria diferentes (oito categorias exclusivas mais o `NULL` opção). Considere o caso de um usuário que está interessado em avaliar os produtos que se enquadram na categoria Frutos do mar. De uma página que liste *todos os* os produtos em uma única GridView, o usuário pode decidir sua melhor aposta é classificar os resultados por categoria, que serão agrupados todos os produtos de Frutos do mar juntos. Depois da classificação por categoria, o usuário, em seguida, precisa procurar em lista, procurando por onde os produtos agrupados de Frutos do mar começar e terminar. Uma vez que os resultados são classificados em ordem alfabética pelo nome da categoria Localizando os produtos de Frutos do mar não é difícil, mas ainda requer verificar atentamente a lista de itens na grade.

Para ajudar a realçar os limites entre grupos classificados, muitos sites empregam uma interface do usuário que adiciona um separador entre esses grupos. Separadores de como as mostradas na Figura 1 permite que um usuário localizar um grupo específico e identificar seus limites mais rapidamente, bem como determinar quais grupos distintos existem nos dados.


[![Cada grupo de categorias é claramente identificado](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**Figura 1**: cada grupo de categorias é claramente identificado ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image3.png))


Neste tutorial, veremos como criar cada interface do usuário uma classificação.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Etapa 1: Criando um GridView padrão e classificável

Antes, exploraremos como ampliar o GridView para fornecer a interface de classificação avançada, permitem que s crie primeiro um GridView padrão, classificável, que lista os produtos. Comece abrindo o `CustomSortingUI.aspx` página o `PagingAndSorting` pasta. Adicione um controle GridView à página, defina suas `ID` propriedade para `ProductList`e associá-lo para um novo ObjectDataSource. Configurar o ObjectDataSource para usar o `ProductsBLL` classe s `GetProducts()` método para selecionar registros.

Em seguida, configure o GridView, de modo que ele contém apenas o `ProductName`, `CategoryName`, `SupplierName`, e `UnitPrice` BoundFields e CheckBoxField descontinuado. Finalmente, configure o GridView para oferecer suporte à classificação, marcando a caixa de seleção Habilitar classificação na marca inteligente GridView s (ou definindo sua `AllowSorting` propriedade para `true`). Depois de fazer essas adições para o `CustomSortingUI.aspx` página, a marcação declarativa deve ser semelhante ao seguinte:


[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

Reserve um tempo para exibir nosso progresso até o momento em um navegador. Figura 2 mostra o GridView classificável quando seus dados são classificados por categoria em ordem alfabética.


[![O s GridView classificável os dados são ordenados por categoria](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**Figura 2**: s GridView classificável os dados são ordenados por categoria ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image6.png))


## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Etapa 2: Explorar técnicas para adicionar as linhas de separador

Com genérico, classificável GridView completo, tudo o que resta é ser capaz de adicionar as linhas de separador no GridView antes de cada grupo classificado exclusivo. Mas como essas linhas podem ser injetadas no GridView? Basicamente, precisamos para iterar pelas linhas s GridView, determinar onde as diferenças ocorrerem entre os valores na coluna classificada e, em seguida, adicione a linha do separador apropriado. Ao pensar sobre esse problema, pareça natural que a solução está em algum lugar em s GridView `RowDataBound` manipulador de eventos. Como discutimos na [formatação com base em dados personalizados](../custom-formatting/custom-formatting-based-upon-data-vb.md) tutorial, esse manipulador de eventos normalmente é usado quando a aplicação da formatação de nível de linha com base nos dados de linha s. No entanto, o `RowDataBound` manipulador de eventos não é a solução aqui, como linhas não podem ser adicionadas ao GridView programaticamente deste manipulador de eventos. O s GridView `Rows` coleção, na verdade, é somente leitura.

Para adicionar linhas à GridView, temos três opções:

- Adicione essas linhas de separador de metadados para os dados reais que estão associados a GridView
- Depois que o GridView foi associado a dados, adicionar mais `TableRow` controlam a coleção de instâncias para o s GridView
- Criar um controle de servidor personalizado que estende o controle GridView e substitui os métodos responsáveis pela construção da estrutura de s GridView

Criando um controle de servidor personalizado seria a melhor abordagem se essa funcionalidade era necessário em muitas páginas da web ou através de diversos sites. No entanto, ele seria abrange um pouco de código e uma exploração detalhada as profundezas do funcionamento interno GridView s. Portanto, não podemos considerar essa opção para este tutorial.

As outras duas opções adicionando linhas de separador para os dados reais que está sendo ligado ao GridView e manipular a coleção de controle GridView s após sua foi vinculado - atacar o problema de forma diferente e merecem uma discussão.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Adicionando linhas para os dados associados a GridView

Quando o GridView está associado a uma fonte de dados, ele cria um `GridViewRow` para cada registro retornado pela fonte de dados. Portanto, podemos pode injetar as linhas de separador necessário com a adição de registros de separador à fonte de dados antes de associá-lo ao GridView. Figura 3 ilustra esse conceito.


![Uma técnica envolve a adição de linhas de separador à fonte de dados](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**Figura 3**: uma técnica envolve a adição de linhas de separador à fonte de dados


Posso usar os registros de separador de termos entre aspas, porque não há nenhum registro do separador especial; em vez disso, podemos alguma forma deve sinalizar que um registro específico na fonte de dados serve como um separador em vez de uma linha de dados normal. Para nossos exemplos, podemos re associação de um `ProductsDataTable` instância de GridView, que é composta de `ProductRows`. Podemos pode sinalizar um registro como uma linha definindo sua `CategoryID` propriedade para `-1` (uma vez que esse valor não foi t existe normalmente).

Para utilizar essa técnica d precisamos execute as seguintes etapas:

1. Recuperar programaticamente os dados para associar a GridView (um `ProductsDataTable` instância)
2. Classificar os dados com base em s GridView `SortExpression` e `SortDirection` propriedades
3. Iterar por meio de `ProductsRows` no `ProductsDataTable`, procurando onde estão as diferenças na coluna classificada
4. Em cada limite de grupo, injetar um registro de separador `ProductsRow` instância na DataTable, que possui-s `CategoryID` definido como `-1` (ou qualquer designação foi decidida para marcar um registro como um registro de separador)
5. Depois de injetar as linhas de separador, associar programaticamente os dados para o GridView

Além destas cinco etapas, d também precisamos fornecer um manipulador de eventos para o s GridView `RowDataBound` eventos. Aqui, d verificada a cada `DataRow` e determinar se ele foi um separador de linha, um cujos `CategoryID` era `-1`. Nesse caso, estamos d provavelmente desejará ajustar sua formatação ou o texto exibido na célula (s).

Usando essa técnica para injetar o grupo de limites a classificação exige um pouco mais trabalhoso do que o descrito acima, como você precisará também fornecer um manipulador de eventos para o s GridView `Sorting` evento e manter controlam do `SortExpression` e `SortDirection` valores.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipulando o GridView s controlar a coleta de depois dela s foram vinculados a dados

Em vez dos dados de mensagens antes de associá-lo ao GridView, podemos adicionar as linhas de separador *depois de* foi associados os dados a GridView. O processo de vinculação de dados cria a hierarquia de controle GridView do s, que, na realidade, é simplesmente um `Table` instância composto de um conjunto de linhas, cada um deles é composta de uma coleção de células. Especificamente, a coleção de controle GridView s contém um `Table` objeto na raiz, um `GridViewRow` (que é derivado da `TableRow` classe) para cada registro no `DataSource` ligado ao GridView e um `TableCell` objeto em cada `GridViewRow` instância para cada campo de dados no `DataSource`.

Para adicionar linhas de separador entre cada grupo de classificação, é possível manipular diretamente essa hierarquia de controle quando ele tiver sido criado. Podemos pode ter certeza de que a hierarquia de controle GridView s foi criada pela última vez no momento em que a página está sendo processada. Portanto, essa abordagem substitui o `Page` classe s `Render` método, no ponto em que a hierarquia de controle final s GridView é atualizada para incluir as linhas de separador necessários. Figura 4 ilustra esse processo.


[![Uma técnica alternativa manipula a hierarquia de controle GridView s](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**Figura 4**: uma técnica alternativa manipula o s GridView hierarquia de controle ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image10.png))


Para este tutorial, vamos usar essa última abordagem para personalizar a experiência de usuário de classificação.

> [!NOTE]
> O código m apresentando neste tutorial se baseia no exemplo fornecido em [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) entrada de blog s [reproduzindo um pouco com o agrupamento de classificação de GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).


## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Etapa 3: Adicionando as linhas de separador para a hierarquia de controle GridView s

Como queremos adicionar as linhas de separador a hierarquia de controle GridView s depois de sua hierarquia de controle foi criada e criada pela última vez em que visita de página, que queremos executar essa adição ao final do ciclo de vida de página, mas antes do real c de GridView hierarquia de controle foi renderizada em HTML. O último ponto possível no qual podemos pode fazer isso é o `Page` classe s `Render` evento, que podemos podem ser substituídos em nossa classe code-behind usando a assinatura de método a seguir:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

Quando o `Page` classe s original `Render` método é invocado `base.Render(writer)` cada um dos controles na página será renderizada, gerando a marcação com base em sua hierarquia de controle. Portanto, é imperativo que ambos os chamamos `base.Render(writer)`, de modo que a página é renderizada, e o que podemos manipular o s de GridView de controle de hierarquia antes de chamar `base.Render(writer)`, de modo que as linhas de separador foram adicionadas para a hierarquia de controle GridView s antes dele s foi renderizado.

Para inserir os cabeçalhos do grupo classificar primeiro precisamos garantir que o usuário solicitou que os dados sejam classificadas. Por padrão, o conteúdo de s GridView não é classificado e, portanto, podemos don t precisar inserir quaisquer cabeçalhos de classificação do grupo.

> [!NOTE]
> Se você quiser que o GridView seja classificada por uma coluna específica quando a página é carregada pela primeira vez, chamar o GridView s `Sort` método na primeira visita de página (mas não em postbacks subsequentes). Para fazer isso, adicione essa chamada na `Page_Load` manipulador de eventos dentro de um `if (!Page.IsPostBack)` condicional. Voltar para o [paginação e classificação de dados do relatório](paging-and-sorting-report-data-vb.md) informações tutoriais para saber mais sobre o `Sort` método.


Supondo que os dados foram classificados, nossa próxima tarefa é determinar qual coluna de dados foi classificados por e, em seguida, para verificar as linhas procurando diferenças nessa coluna s valores. O código a seguir garante que os dados foram classificados e localiza a coluna pela qual os dados foram classificados:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

Se o GridView tem ainda a ser classificada, o GridView s `SortExpression` propriedade será não tiver sido definida. Portanto, só queremos adicionar as linhas de separador se essa propriedade tem algum valor. Se isso acontecer, em seguida precisamos determinar o índice da coluna pela qual os dados foi classificados. Isso é feito ao fazer loop por meio de s GridView `Columns` coleta, a pesquisa para a coluna cuja `SortExpression` propriedade for igual a s GridView `SortExpression` propriedade. O índice da coluna s, além de também capturamos o `HeaderText` propriedade, que é usada ao exibir as linhas de separador.

Com o índice da coluna pela qual os dados são classificados, a etapa final é enumerar as linhas de GridView. Para cada linha, precisamos determinar se o valor de s coluna classificada é diferente do valor de s de coluna de s classificados linha anterior. Se, por isso, precisamos inserir um novo `GridViewRow` instância para a hierarquia de controle. Isso é feito com o código a seguir:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

Esse código começa fazendo referência a programaticamente os `Table` do objeto encontrado na raiz da hierarquia de controle GridView s e criando uma variável de cadeia de caracteres denominada `lastValue`. `lastValue` é usado para comparar o valor da coluna de s classificados de linha atual com o valor de linha s anterior. Em seguida, o s GridView `Rows` coleção seja enumerada e para cada linha, o valor da coluna classificada é armazenado no `currentValue` variável.

> [!NOTE]
> Para determinar o valor da coluna s classificados linha específica, posso usar a célula s `Text` propriedade. Isso funciona bem para BoundFields, mas não funciona conforme o desejado para TemplateFields, CheckBoxFields e assim por diante. Vamos examinar como conta para os campos de GridView alternativos em breve.


O `currentValue` e `lastValue` , em seguida, em comparação com as variáveis. Se eles forem diferentes, precisamos adicionar uma nova linha do separador para a hierarquia de controle. Isso é feito por meio da determinação o índice do `GridViewRow` no `Table` objeto s `Rows` coleção, criando novos `GridViewRow` e `TableCell` instâncias e, em seguida, adicionando o `TableCell` e `GridViewRow` para o hierarquia de controle.

Observe que o separador de linha s solitário `TableCell` é formatado, de modo que se estende por toda a largura do GridView, é formatado usando o `SortHeaderRowStyle` classe CSS e tem seu `Text` tal que ela mostra ambos os grupo de classificação de nome de propriedade (como categoria) e o valor do grupo s (como bebidas). Por fim, `lastValue` é atualizado para o valor de `currentValue`.

A classe CSS usada para formatar a linha de cabeçalho de grupo classificação `SortHeaderRowStyle` deve ser especificado no `Styles.css` arquivo. Fique à vontade para usar quaisquer configurações de estilo agradá-lo; Eu usei o seguinte:


[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

Com o código atual, a interface de classificação adiciona cabeçalhos de grupo de classificação ao classificar por qualquer BoundField (consulte a Figura 5, que mostra uma captura de tela durante a classificação por fornecedor). No entanto, ao classificar por qualquer outro tipo de campo (como um CheckBoxField ou TemplateField), os cabeçalhos do grupo de classificação são nenhum lugar a ser localizada (veja a Figura 6).


[![A Interface de classificação inclui os cabeçalhos de grupo de classificação ao classificar por BoundFields](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**Figura 5**: A classificação Interface inclui classificação grupo cabeçalhos quando a classificação por BoundFields ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image13.png))


[![Os cabeçalhos do grupo de classificação estão ausentes ao classificar um CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**Figura 6**: os cabeçalhos de grupo de classificação estão ausentes ao classificar um CheckBoxField ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image16.png))


O motivo que faltam os cabeçalhos do grupo de classificação ao classificar por um CheckBoxField é porque o código atualmente usa apenas o `TableCell` s `Text` propriedade para determinar o valor da coluna classificada para cada linha. Para CheckBoxFields, o `TableCell` s `Text` propriedade é uma cadeia de caracteres vazia; em vez disso, o valor está disponível por meio de um controle de Web de caixa de seleção que reside dentro de `TableCell` s `Controls` coleção.

Para lidar com tipos de campo que não seja BoundFields, precisamos para ampliar o código em que o `currentValue` variável é atribuída para verificar a existência de uma caixa de seleção na `TableCell` s `Controls` coleção. Em vez de usar `currentValue = gvr.Cells(sortColumnIndex).Text`, substitua este código a seguir:


[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

Esse código examina a coluna classificada `TableCell` para a linha atual determinar se há uma todos os controles no `Controls` coleção. Se houver, e o primeiro controle é uma caixa de seleção, o `currentValue` variável é definida como Sim ou não, dependendo da caixa de seleção s `Checked` propriedade. Caso contrário, o valor é obtido do `TableCell` s `Text` propriedade. Essa lógica pode ser replicada para lidar com classificação para qualquer TemplateFields que podem existir em um GridView.

Com a adição de código acima, os cabeçalhos do grupo de classificação agora estão presentes quando a classificação por CheckBoxField descontinuados (veja a Figura 7).


[![Os cabeçalhos de grupo de classificação são agora presente ao classificar um CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**Figura 7**: os cabeçalhos de grupo de classificação são agora presente ao classificar um CheckBoxField ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image19.png))


> [!NOTE]
> Se você tiver os produtos com `NULL` banco de dados valores para o `CategoryID`, `SupplierID`, ou `UnitPrice` campos, esses valores serão exibidos como cadeias de caracteres vazias GridView por padrão, o que significa que o texto de linha s separador para esses produtos com `NULL`valores serão lidas como categoria: (ou seja, não há s sem nome após a categoria: como ocorre com categoria: Bebidas). Se você desejar um valor exibido aqui você pode definir o BoundFields [ `NullDisplayText` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) ao texto a ser exibido ou você pode adicionar uma instrução condicional ao método Render ao atribuir o `currentValue` para o separador linha s `Text` propriedade.


## <a name="summary"></a>Resumo

O GridView não inclui várias opções internas para personalização da interface de classificação. No entanto, com um pouco de código de nível baixo, ele é possível ajustar a hierarquia de controle GridView s para criar uma interface mais personalizada. Neste tutorial vimos como adicionar uma linha de separador de grupo de classificação para um GridView classificável, que identifica com mais facilidade os grupos distintos e esses limites de grupos. Para obter exemplos adicionais de interfaces de classificação personalizadas, fazer check-out [Scott Guthrie](https://weblogs.asp.net/scottgu/) s [alguns ASP.NET 2.0 GridView classificação dicas e truques](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) entrada de blog.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](sorting-custom-paged-data-vb.md)
