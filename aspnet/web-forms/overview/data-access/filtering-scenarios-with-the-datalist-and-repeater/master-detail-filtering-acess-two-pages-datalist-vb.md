---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
title: Mestre/detalhes filtragem em duas páginas (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial vamos examinar como separar um relatório mestre/detalhes em duas páginas. Na página 'mestre', usamos um controle Repeater para renderizar uma lista de categ...
ms.author: aspnetcontent
ms.date: 10/30/2010
ms.assetid: f1a1be2c-6fd9-4a09-916e-aa1b98d5cf17
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5cfbd685344bdd223f8d07f8bad5a54b63735839
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813678"
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Mestre/detalhes filtragem em duas páginas (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_VB.exe) ou [baixar PDF](master-detail-filtering-acess-two-pages-datalist-vb/_static/datatutorial34vb1.pdf)

> Neste tutorial vamos examinar como separar um relatório mestre/detalhes em duas páginas. Na página de "master", usamos um controle Repeater para renderizar uma lista de categorias que, quando clicado, levará o usuário para a página "Detalhes" onde uma DataList de duas colunas mostra os produtos que pertencem à categoria selecionada.


## <a name="introduction"></a>Introdução

No [filtragem de mestre/detalhes em duas páginas](../masterdetail/master-detail-filtering-across-two-pages-vb.md) tutorial, examinamos esse padrão usando um GridView para exibir todos os fornecedores no sistema. Este GridView incluído um HyperLinkField, que é renderizado como um link para uma segunda página, passando o `SupplierID` na querystring. A segunda página usado um GridView para listar os produtos fornecidos pelo fornecedor selecionado.

Esses relatórios mestre/detalhes de duas páginas podem ser feitos usando os controles DataList e Repeater também. A única diferença é que nem DataList nem repetidor fornece suporte para o controle HyperLinkField. Em vez disso, devemos adicionar um controle de hiperlink Web ou um elemento âncora HTML (`<a>`) dentro do controle `ItemTemplate`. O hiperlink `NavigateUrl` propriedade ou a âncora `href` atributo, em seguida, pode ser personalizado usando abordagens declarativas ou através de programação.

Neste tutorial, exploraremos um exemplo que lista as categorias em uma lista com marcadores em uma única página usando um controle Repeater. Cada item de lista incluirá o nome e uma descrição da categoria com o nome de categoria, exibido como um link para uma segunda página. Ao clicar nesse link será whisk o usuário para a segunda página, onde um DataList mostrará os produtos que pertencem à categoria selecionada.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Etapa 1: Exibir as categorias em uma lista com marcadores

A primeira etapa na criação de qualquer relatório mestre/detalhes é comece exibindo os registros de "master". Portanto, nossa primeira tarefa é exibir as categorias na página de "master". Abra o `CategoryListMaster.aspx` página o `DataListRepeaterFiltering` pasta, adicione um controle Repeater e, na marca inteligente, optar por adicionar um novo ObjectDataSource. Configurar o novo ObjectDataSource para que ele acessa seus dados a partir de `CategoriesBLL` da classe `GetCategories` método (veja a Figura 1).


[![Configurar o ObjectDataSource para usar o CategoriesBLL método da classe GetCategories](master-detail-filtering-acess-two-pages-datalist-vb/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image1.png)

**Figura 1**: configurar o ObjectDataSource para usar o `CategoriesBLL` da classe `GetCategories` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image3.png))


Em seguida, defina os modelos do repetidor, de modo que ele exibe cada nome de categoria e uma descrição como um item em uma lista com marcadores. Vamos ainda não se preocupe sobre ter cada categoria de link para a página de detalhes. O exemplo a seguir mostra a marcação declarativa para o Repeater e ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample1.aspx)]

Com essa marcação completa, reserve um tempo para exibir nosso progresso através de um navegador. Como mostra a Figura 2, o Repeater é renderizado como uma lista com marcadores mostrando o nome e uma descrição de cada categoria.


[![Cada categoria é exibida como um Item de lista com marcadores](master-detail-filtering-acess-two-pages-datalist-vb/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image4.png)

**Figura 2**: cada categoria é exibida como um Item de lista com marcadores ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Etapa 2: Transformando o nome da categoria em um Link para a página de detalhes

Para permitir que um usuário exibir as informações de "Detalhes" para uma determinada categoria, precisamos adicionar um link para cada lista com marcadores de item que, quando clicado, levará o usuário para a segunda página (`ProductsForCategoryDetails.aspx`). Essa segunda página, em seguida, exibirá os produtos para a categoria selecionada usando uma DataList. Para determinar a categoria cuja link foi clicado, precisamos passar a categoria clicada `CategoryID` para a segunda página por algum outro mecanismo. A maneira mais simples e mais simples de transferir dados escalar de uma página para outra é por meio da cadeia de consulta, que é a opção que usaremos neste tutorial. Em particular, o `ProductsForCategoryDetails.aspx` página esperarão selecionado *`categoryID`* valor a ser passado por meio de um campo de cadeia de consulta chamado `CategoryID`. Por exemplo, para exibir os produtos para a categoria de bebidas, que tem um `CategoryID` de 1, um usuário visitar `ProductsForCategoryDetails.aspx?CategoryID=1`.

Para criar um hiperlink para cada item de lista com marcadores no repetidor, precisamos adicionar um controle de hiperlink Web ou um elemento âncora HTML (`<a>`) para o `ItemTemplate`. Em cenários em que o hiperlink é exibida a mesma para cada linha, qualquer uma das abordagens será suficiente. Para repetidores, eu prefiro usando o elemento de âncora. Para usar o elemento de âncora, atualize o ItemTemplate do repetidor para:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample2.aspx)]

Observe que o `CategoryID` pode ser injetado diretamente dentro do elemento de âncora `href` atributo; no entanto, para então ser determinadas delimitar o `href` o valor do atributo com apóstrofos (e Observação aspas) desde o `Eval` método dentro de `href` atributo delimita sua cadeia de caracteres (`"CategoryID"`) com as aspas. Como alternativa, um controle de Web de hiperlink pode ser usado em vez disso:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample3.aspx)]

Observe como a parte estática da URL — `ProductsForCategoryDetails.aspx?CategoryID` — é acrescentado ao resultado de `Eval("CategoryID")` diretamente dentro da sintaxe de vinculação de dados usando a concatenação de cadeia de caracteres.

Uma vantagem de usar o controle de hiperlink é que ele pode ser programaticamente acessado pelo Repeater `ItemDataBound` manipulador de eventos, se necessário. Por exemplo, você talvez queira exibir o nome da categoria como texto em vez de um link para categorias de produtos não associados. Essa verificação poderia ser executada por meio de programação na `ItemDataBound` manipulador de eventos; para categorias sem nenhum associada produtos, o hiperlink `NavigateUrl` propriedade pode ser definida como uma cadeia de caracteres em branco, resultando em que nome de categoria específico renderização como texto sem formatação (em vez de um link). Voltar para o [formatação do DataList e Repeater com base na Data](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) tutorial para obter mais informações sobre a formatação do DataList e do Repeater conteúdo com base em lógica programática por meio do `ItemDataBound` manipulador de eventos.

Se você estiver acompanhando, fique à vontade usar o elemento de âncora ou uma abordagem de controle de hiperlink na página. Independentemente da abordagem, ao exibir a página por meio de um navegador que cada nome de categoria deve ser renderizado como um link para `ProductsForCategoryDetails.aspx`, passando o aplicável `CategoryID` valor (veja a Figura 3).


[![Os nomes de categoria se vincular ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-vb/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image7.png)

**Figura 3**: A categoria nomes agora Link para `ProductsForCategoryDetails.aspx` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Etapa 3: Listando os produtos que pertencem à categoria selecionada

Com o `CategoryListMaster.aspx` página completa, estamos prontos para voltar nossas atenções para a página "Detalhes", a implementação `ProductsForCategoryDetails.aspx`. Abrir essa página, arraste uma DataList da caixa de ferramentas para o Designer e defina suas `ID` propriedade para `ProductsInCategory`. Em seguida, na marca inteligente do DataList optar por adicionar um novo ObjectDataSource para a página, nomeando- `ProductsInCategoryDataSource`. Configurá-lo, de modo que ele chama o `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método; defina na lista suspensa lista nas guias de INSERT, UPDATE e DELETE como (nenhum).


[![Configurar o ObjectDataSource para usar o ProductsBLL método da classe GetProductsByCategoryID(categoryID)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image10.png)

**Figura 4**: configurar o ObjectDataSource para usar o `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image12.png))


Uma vez que o `GetProductsByCategoryID(categoryID)` método aceita um parâmetro de entrada (*`categoryID`*), o Assistente de fonte de dados escolha nos oferece uma oportunidade de especificar a origem do parâmetro. Defina a origem do parâmetro QueryString usando o QueryStringField `CategoryID`.


[![Use o campo Querystring CategoryID como origem do parâmetro](master-detail-filtering-acess-two-pages-datalist-vb/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image13.png)

**Figura 5**: Use Querystring Field `CategoryID` como origem do parâmetro ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image15.png))


Como vimos nos tutoriais anteriores, após concluir o Assistente de escolher fonte de dados, o Visual Studio cria automaticamente um `ItemTemplate` para DataList que lista cada nome de campo de dados e o valor. Substitua esse modelo um que liste apenas o produto nome, fornecedor e preço. Além disso, defina o DataList `RepeatColumns` propriedade como 2. Após essas alterações, DataList e o do ObjectDataSource marcação declarativa deve ser semelhante ao seguinte:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample4.aspx)]

Para exibir esta página em ação, desde o `CategoryListMaster.aspx` página; em seguida, clique em um link na lista com marcadores de categorias. Isso levará você até `ProductsForCategoryDetails.aspx`, passando ao longo de `CategoryID` por meio da cadeia de consulta. O `ProductsInCategoryDataSource` ObjectDataSource em `ProductsForCategoryDetails.aspx` , em seguida, obter apenas os produtos para a categoria especificada e exibi-los no DataList, que renderiza os dois produtos por linha. A Figura 6 mostra uma captura de tela `ProductsForCategoryDetails.aspx` ao exibir as bebidas.


[![São exibidas as bebidas, duas por linha](master-detail-filtering-acess-two-pages-datalist-vb/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image16.png)

**Figura 6**: O bebidas são exibidas, duas por linha ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Etapa 4: Exibindo informações de categoria em ProductsForCategoryDetails.aspx

Quando um usuário clica em uma categoria na `CategoryListMaster.aspx`, eles são levados para `ProductsForCategoryDetails.aspx` e mostra os produtos que pertencem à categoria selecionada. No entanto, no `ProductsForCategoryDetails.aspx` não há nenhum dicas visuais sobre qual categoria foi selecionada. Um usuário que pretende clique Bebidas, mas Condimentos acidentalmente clicados, não tem nenhuma maneira de perceber seu erro assim que chegam `ProductsForCategoryDetails.aspx`. Para minimizar esse problema potencial, podemos exibir informações sobre a categoria selecionada — seu nome e descrição — na parte superior do `ProductsForCategoryDetails.aspx` página.

Para fazer isso, adicione um FormView acima do controle Repeater na `ProductsForCategoryDetails.aspx`. Em seguida, adicione um novo ObjectDataSource para a página na chamada de marca inteligente de FormView `CategoryDataSource` e configurá-lo para usar o `CategoriesBLL` da classe `GetCategoryByCategoryID(categoryID)` método.


[![Acessar informações sobre a categoria por meio GetCategoryByCategoryID(categoryID) método da classe CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-vb/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image19.png)

**Figura 7**: acessar informações sobre a categoria por meio de `CategoriesBLL` da classe `GetCategoryByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image21.png))


Assim como acontece com o `ProductsInCategoryDataSource` ObjectDataSource adicionado na etapa 3, o `CategoryDataSource`do Assistente Configurar fonte de dados nos solicita uma fonte para o `GetCategoryByCategoryID(categoryID)` parâmetro de entrada do método. Use as exatamente as mesmas configurações como antes, configurando a fonte de parâmetro como QueryString e o valor de QueryStringField como `CategoryID` (consulte novamente a Figura 5).

Depois de concluir o assistente, o Visual Studio cria automaticamente um `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate` para FormView. Uma vez que estamos oferecendo uma interface somente leitura, fique à vontade remover o `EditItemTemplate` e `InsertItemTemplate`. Além disso, fique à vontade personalizar o FormView `ItemTemplate`. Após remover os modelos supérfluos e personalizando o ItemTemplate, marcação declarativa seus FormView e ObjectDataSource deve ser semelhante ao seguinte:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample5.aspx)]

Figura 8 mostra uma tela de captura ao exibir esta página por meio de um navegador.

> [!NOTE]
> Além de FormView, eu também adicionou um controle de hiperlink acima FormView que levará o usuário de volta para a lista de categorias (`CategoryListMaster.aspx`). Fique à vontade para colocar este link em outro lugar ou para omiti-lo completamente.


[![Informações de categoria são agora exibido na parte superior da página](master-detail-filtering-acess-two-pages-datalist-vb/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image22.png)

**Figura 8**: informações de categoria é agora exibido na parte superior da página ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Etapa 5: Exibir uma mensagem se nenhum produto pertence à categoria selecionada

O `CategoryListMaster.aspx` página lista todas as categorias no sistema, independentemente de se há algum associados a produtos. Se um usuário clica em uma categoria com nenhum produtos associados, DataList no `ProductsForCategoryDetails.aspx` não será renderizado, como sua fonte de dados não terá todos os itens. Como vimos nos tutoriais anteriores, o GridView fornece um `EmptyDataText` propriedade que pode ser usada para especificar uma mensagem de texto a ser exibido se não há registros na fonte de dados. Infelizmente, nem o DataList nem Repeater tem essa propriedade.

Para exibir uma mensagem informando ao usuário que não há nenhum produto correspondente para a categoria selecionada, precisamos adicionar um rótulo de controle para a página cuja `Text` propriedade recebe a mensagem a ser exibida no caso em que nenhum produto correspondente. Em seguida, precisamos definir programaticamente seu `Visible` propriedade com base em se ou não DataList contém todos os itens.

Para fazer isso, comece adicionando um rótulo abaixo DataList. Defina suas `ID` propriedade para `NoProductsMessage` e seu `Text` propriedade como "Não há nenhum produto para a categoria selecionada..." Em seguida, precisamos definir este rótulo de forma programática `Visible` propriedade com base em se ou não todos os dados foi vinculados ao `ProductsInCategory` DataList. Essa atribuição deve ser feita depois que os dados foi associados à DataList. Para o GridView, DetailsView e FormView, criamos um manipulador de eventos para o controle `DataBound` evento, que é acionado após a vinculação de dados. No entanto, nem DataList nem repetidor tem um `DataBound` eventos disponíveis.

Este exemplo em particular, podemos atribuir o rótulo `Visible` propriedade em de `Page_Load` manipulador de eventos, uma vez que os dados serão atribuídos à DataList antes que a página `Load` eventos. No entanto, essa abordagem não funcionaria em geral, como os dados do ObjectDataSource podem ser associados à DataList posteriormente no ciclo de vida da página. Por exemplo, se os dados exibidos baseia-se se o valor em outro controle, como ela é ao exibir um relatório de mestre/detalhes usando DropDownList para armazenar os registros de "master", os dados não podem se para o controle de Web de dados até que o `PreRender` estágio no ciclo de vida da página.

Uma solução que funcionará para todos os casos é atribuir a `Visible` propriedade para `False` do DataList `ItemDataBound` (ou `ItemCreated`) o manipulador de eventos ao associar um tipo de item de `Item` ou `AlternatingItem`. Nesse caso, nós sabemos que não há dados de pelo menos um item na fonte de dados e, portanto, pode ocultar o `NoProductsMessage` rótulo. Além de manipulador de eventos, também precisamos de um manipulador de eventos para o DataList `DataBinding` evento, onde podemos inicializar o rótulo `Visible` propriedade `True`. Uma vez que o `DataBinding` evento será acionado antes do `ItemDataBound` eventos, o rótulo `Visible` propriedade será definida inicialmente como `True`; se houver quaisquer itens de dados, no entanto, ele será definido como `False`. O código a seguir implementa essa lógica:

[!code-vb[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample6.vb)]

Todas as categorias no banco de dados Northwind são associadas com um ou mais produtos. Para testar esse recurso, eu tiver ajustado manualmente o banco de dados Northwind para este tutorial, reatribuindo todos os produtos associados à categoria de produzir (`CategoryID` = 7) para a categoria de Frutos do Mar (`CategoryID` = 8). Isso pode ser feito do Gerenciador de servidores, escolha a nova consulta e usando o seguinte `UPDATE` instrução:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample7.sql)]

Depois de atualizar o banco de dados adequadamente, retornar o `CategoryListMaster.aspx` da página e clique no link de produzir. Uma vez que não existem mais todos os produtos que pertencem à categoria de produção, você deve ver a mensagem "Não há nenhum produto para a categoria selecionada...", conforme mostrado na Figura 9.


[![Uma mensagem será exibida se não houver nenhum produtos pertencentes à categoria selecionada](master-detail-filtering-acess-two-pages-datalist-vb/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image25.png)

**Figura 9**: A mensagem é exibida se não houver nenhum produtos pertencentes à categoria selecionada ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image27.png))


## <a name="summary"></a>Resumo

Enquanto os relatórios mestre/detalhes podem exibir registros mestre e de detalhes em uma única página, em muitos sites eles são separados em duas páginas da web. Neste tutorial vimos como implementar tal relatório mestre/detalhes fazendo com que as categorias listadas em uma lista com marcadores usando um repetidor na página da web "mestre" e os produtos associados listados na página "Detalhes". Cada item da lista na página da web mestre continha um link para a página de detalhes do passado ao longo da linha `CategoryID` valor.

Na página de detalhes ao recuperar esses produtos para o fornecedor especificado foi realizado por meio de `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método. O *`categoryID`* o valor do parâmetro foi especificado declarativamente usando o `CategoryID` valor de cadeia de consulta como a origem do parâmetro. Também examinamos como exibir detalhes da categoria na página de detalhes usando um FormView e como exibir uma mensagem se não houvesse nenhum produto que pertencem à categoria selecionada.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Zack Jones e Liz Shulok. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
> [Próximo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)
