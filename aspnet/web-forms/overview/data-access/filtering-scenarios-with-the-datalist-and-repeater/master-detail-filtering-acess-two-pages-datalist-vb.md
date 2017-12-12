---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
title: "Mestre/detalhes filtragem em duas páginas (VB) | Microsoft Docs"
author: rick-anderson
description: "Este tutorial examinamos como separar um relatório mestre/detalhes em duas páginas. Na página 'mestre', usamos um controle repetidor para renderizar uma lista de categ..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2010
ms.topic: article
ms.assetid: f1a1be2c-6fd9-4a09-916e-aa1b98d5cf17
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f43fa998b81800cb1a2b7796ebb3922fc1caeb8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Filtragem em duas páginas (VB) mestre/detalhes
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_VB.exe) ou [baixar PDF](master-detail-filtering-acess-two-pages-datalist-vb/_static/datatutorial34vb1.pdf)

> Este tutorial examinamos como separar um relatório mestre/detalhes em duas páginas. Na página "mestre", usamos um controle repetidor para renderizar uma lista de categorias que, quando clicado, levará o usuário para a página "Detalhes" onde DataList duas colunas mostra os produtos que pertencem à categoria selecionada.


## <a name="introduction"></a>Introdução

No [filtragem de mestre/detalhes em duas páginas](../masterdetail/master-detail-filtering-across-two-pages-vb.md) tutorial, examinamos esse padrão usando um controle GridView para exibir todos os fornecedores no sistema. Este GridView incluído um HyperLinkField, que é renderizado como um link para uma segunda página, passando o `SupplierID` na querystring. A segunda página usado um GridView para listar os produtos fornecidos pelo fornecedor selecionado.

Esses relatórios de duas páginas mestre/detalhado podem ser feitos usando controles DataList e repetidor também. A única diferença é que nem o DataList nem repetidor fornece suporte para o controle HyperLinkField. Em vez disso, devemos adicionar um controle HyperLink Web ou um elemento âncora HTML (`<a>`) dentro do controle `ItemTemplate`. O hiperlink `NavigateUrl` propriedade ou a âncora `href` atributo poderá então ser personalizado usando abordagens declarativas ou através de programação.

Neste tutorial, vamos explorar de um exemplo que lista as categorias em uma lista com marcadores em uma página usando um controle Repetidor. Cada item de lista incluirá o nome e a descrição, a categoria com o nome da categoria exibido como um link para uma segunda página. Clicar nesse link será whisk o usuário para a segunda página, onde uma DataList mostrará os produtos que pertencem à categoria selecionada.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Etapa 1: Exibindo as categorias em uma lista com marcadores

A primeira etapa na criação de qualquer relatório mestre/detalhes é começar a exibir os registros de "mestres". Portanto, a primeira tarefa é exibir as categorias na página "mestre". Abra o `CategoryListMaster.aspx` página o `DataListRepeaterFiltering` pasta, adicionar um controle repetidor e, de marca inteligente, optar por adicionar um novo ObjectDataSource. Configurar o novo ObjectDataSource para que ele acessa seus dados a partir de `CategoriesBLL` da classe `GetCategories` método (consulte a Figura 1).


[![Configurar o ObjectDataSource para usar o CategoriesBLL método da classe GetCategories](master-detail-filtering-acess-two-pages-datalist-vb/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image1.png)

**Figura 1**: configurar o ObjectDataSource para usar o `CategoriesBLL` da classe `GetCategories` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image3.png))


Em seguida, defina os modelos do repetidor, de modo que ele exibe cada nome de categoria e a descrição como um item em uma lista com marcadores. Vamos ainda não se preocupe com cada categoria de link para a página de detalhes. O exemplo a seguir mostra a marcação declarativa para o repetidor e ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample1.aspx)]

Com essa marcação concluída, dedique alguns momentos para exibir nosso andamento por meio de um navegador. Como mostra a Figura 2, repetidor é renderizada como uma lista com marcadores mostrando o nome e a descrição de cada categoria.


[![Cada categoria é exibida como um Item de lista com marcadores](master-detail-filtering-acess-two-pages-datalist-vb/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image4.png)

**Figura 2**: cada categoria é exibida como um Item de lista com marcadores ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Etapa 2: Transformar o nome da categoria em um Link para a página de detalhes

Para permitir que um usuário exibir as informações de "Detalhes" para uma determinada categoria, precisamos adicionar um link para cada lista com marcadores de item que, quando clicado, levará o usuário para a segunda página (`ProductsForCategoryDetails.aspx`). Essa segunda página, em seguida, exibirá os produtos para a categoria selecionada usando uma DataList. Para determinar a categoria cujo link foi clicado, precisamos passar a categoria clicada `CategoryID` para a segunda página por algum outro mecanismo. A maneira mais simples e mais simples para transferir dados escalar de uma página para outra é por meio de querystring, que é a opção que vamos usar neste tutorial. Em particular, o `ProductsForCategoryDetails.aspx` página esperará selecionado  *`categoryID`*  valor a ser passado por meio de um campo querystring chamado `CategoryID`. Por exemplo, para exibir os produtos para a categoria de bebidas, que tem um `CategoryID` de 1, um usuário deve visitar `ProductsForCategoryDetails.aspx?CategoryID=1`.

Para criar um hiperlink para cada item de lista com marcadores no repetidor, precisamos adicionar um controle HyperLink Web ou um elemento âncora HTML (`<a>`) para o `ItemTemplate`. Em cenários onde o hiperlink é exibido o mesmo para cada linha, qualquer abordagem será suficiente. Para repetidores, prefiro usando o elemento de âncora. Para usar o elemento de âncora, atualize ItemTemplate do repetidor para:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample2.aspx)]

Observe que o `CategoryID` pode ser inserida diretamente dentro do elemento de âncora `href` atributo; no entanto, para então ser determinadas delimitar o `href` o valor do atributo com apóstrofos (e observe aspas) desde o `Eval` método dentro de `href` atributo delimita sua cadeia de caracteres (`"CategoryID"`) com aspas. Como alternativa, um controle de Web de hiperlink pode ser usado em vez disso:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample3.aspx)]

Observe como a parte estática do URL — `ProductsForCategoryDetails.aspx?CategoryID` — é anexado ao resultado da `Eval("CategoryID")` diretamente dentro da sintaxe de associação de dados usando a concatenação de cadeia de caracteres.

Uma vantagem de usar o controle de hiperlink é que pode ser programaticamente acessado a partir do repetidor `ItemDataBound` manipulador de eventos, se necessário. Por exemplo, você talvez queira exibir o nome da categoria como texto em vez de um link para categorias de produtos não associados. Tal verificação possa ser executada por meio de programação no `ItemDataBound` manipulador de eventos; para categorias sem nenhum associada produtos, o hiperlink `NavigateUrl` propriedade pode ser definida como uma cadeia de caracteres em branco, resultando assim nesse nome de categoria específica renderização como texto sem formatação (em vez de um link). Voltar ao [formatação de DataList e repetidor com base em dados](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) tutorial para obter mais informações sobre formatação de DataList e do repetidor conteúdo com base em lógica programática por meio de `ItemDataBound` manipulador de eventos.

Se você estiver acompanhando, fique à vontade para usar o elemento de âncora ou uma abordagem de controle de hiperlink na página. Independentemente da abordagem, ao exibir a página por meio de um navegador que cada nome de categoria deve ser renderizado como um link para `ProductsForCategoryDetails.aspx`, passando o aplicável `CategoryID` valor (consulte a Figura 3).


[![Os nomes das categorias vincular ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-vb/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image7.png)

**Figura 3**: A categoria de nomes de Link agora para `ProductsForCategoryDetails.aspx` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Etapa 3: Lista os produtos que pertencem à categoria selecionada

Com o `CategoryListMaster.aspx` página conclua, estamos prontos para voltar nossa atenção para a página "Detalhes", a implementação `ProductsForCategoryDetails.aspx`. Abrir essa página, arraste uma DataList da caixa de ferramentas para o Designer e defina seu `ID` propriedade `ProductsInCategory`. Em seguida, na marca inteligente do DataList escolha Adicionar um novo ObjectDataSource para a página, nomeando- `ProductsInCategoryDataSource`. Configurá-lo, de modo que ele chama o `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método; o conjunto de listas suspensas nas guias de INSERT, UPDATE e DELETE para (nenhum).


[![Configurar o ObjectDataSource para usar o ProductsBLL método da classe GetProductsByCategoryID(categoryID)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image10.png)

**Figura 4**: configurar o ObjectDataSource para usar o `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image12.png))


Como o `GetProductsByCategoryID(categoryID)` método aceita um parâmetro de entrada (*`categoryID`*), o Assistente de fonte de dados escolha nos oferece a oportunidade de especificar a origem do parâmetro. Defina a origem do parâmetro QueryString usando o QueryStringField `CategoryID`.


[![Use o campo Querystring CategoryID como origem do parâmetro](master-detail-filtering-acess-two-pages-datalist-vb/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image13.png)

**Figura 5**: usar Querystring Field `CategoryID` como origem do parâmetro ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image15.png))


Como vimos nos tutoriais anteriores, após concluir o Assistente de escolher fonte de dados, o Visual Studio cria automaticamente um `ItemTemplate` para DataList que lista cada nome de campo de dados e o valor. Substitua este modelo uma que lista apenas o produto nome, fornecedor e preço. Além disso, defina o DataList `RepeatColumns` propriedade para 2. Após essas alterações, marcação declarativa seu DataList e do ObjectDataSource deve ser semelhante ao seguinte:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample4.aspx)]

Para exibir a página em ação, inicie a partir do `CategoryListMaster.aspx` página; em seguida, clique em um link na lista com marcadores de categorias. Isso levará você para `ProductsForCategoryDetails.aspx`, passando junto o `CategoryID` por meio de querystring. O `ProductsInCategoryDataSource` ObjectDataSource em `ProductsForCategoryDetails.aspx` , em seguida, obter apenas os produtos para a categoria especificada e exibi-los em DataList, que renderiza os dois produtos por linha. A Figura 6 mostra uma captura de tela de `ProductsForCategoryDetails.aspx` ao exibir as bebidas.


[![As bebidas são exibidas, dois por linha](master-detail-filtering-acess-two-pages-datalist-vb/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image16.png)

**Figura 6**: Bebidas a são exibidas, dois por linha ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Etapa 4: Exibindo informações de categoria em ProductsForCategoryDetails.aspx

Quando um usuário clica em uma categoria em `CategoryListMaster.aspx`, eles são aplicados para `ProductsForCategoryDetails.aspx` e mostra os produtos que pertencem à categoria selecionada. No entanto, no `ProductsForCategoryDetails.aspx` não há nenhum indicações visuais sobre qual categoria foi selecionada. Um usuário deve clicar em Bebidas, mas Condimentos clicados acidentalmente, não tem como perceber que o erro assim que chegam `ProductsForCategoryDetails.aspx`. Para aliviar esse problema potencial, podemos exibir informações sobre a categoria selecionada — seu nome e descrição, na parte superior do `ProductsForCategoryDetails.aspx` página.

Para fazer isso, adicione um FormView acima do controle repetidor na `ProductsForCategoryDetails.aspx`. Em seguida, adicione um novo ObjectDataSource para a página de marca inteligente de FormView denominada `CategoryDataSource` e configurá-lo para usar o `CategoriesBLL` da classe `GetCategoryByCategoryID(categoryID)` método.


[![Acessar informações sobre a categoria por meio do método GetCategoryByCategoryID(categoryID) da classe CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-vb/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image19.png)

**Figura 7**: acessar informações sobre a categoria por meio de `CategoriesBLL` da classe `GetCategoryByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image21.png))


Assim como acontece com o `ProductsInCategoryDataSource` ObjectDataSource adicionado na etapa 3, o `CategoryDataSource`do Assistente para configurar fonte de dados nos solicitará uma fonte para o `GetCategoryByCategoryID(categoryID)` parâmetro de entrada do método. Use as exatamente as mesmas configurações como antes, definir a origem do parâmetro QueryString e o valor QueryStringField `CategoryID` (consulte novamente a Figura 5).

Depois de concluir o assistente, o Visual Studio cria automaticamente um `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate` de FormView. Já que estamos fornecendo uma interface somente leitura, fique à vontade para remover o `EditItemTemplate` e `InsertItemTemplate`. Além disso, fique à vontade para personalizar o FormView `ItemTemplate`. Após remover os modelos supérfluos e personalizando ItemTemplate, marcação declarativa o FormView e do ObjectDataSource deve ser semelhante ao seguinte:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample5.aspx)]

A Figura 8 mostra uma tela de captura ao exibir esta página por meio de um navegador.

> [!NOTE]
> Além de FormView também ter adicionado um controle de hiperlink acima FormView que levará o usuário de volta para a lista de categorias (`CategoryListMaster.aspx`). Fique à vontade para colocar este link em outro lugar ou para omiti-lo completamente.


[![Informações de categoria são exibidos agora na parte superior da página](master-detail-filtering-acess-two-pages-datalist-vb/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image22.png)

**Figura 8**: informações de categoria são exibidos agora na parte superior da página ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Etapa 5: Exibindo uma mensagem se nenhum produto pertence à categoria selecionada

O `CategoryListMaster.aspx` página lista todas as categorias no sistema, independentemente de se há alguma associados a produtos. Se um usuário clica em uma categoria com nenhuma produtos associados, DataList em `ProductsForCategoryDetails.aspx` não serão processados, como sua fonte de dados não terá todos os itens. Como vimos nos tutoriais anteriores, GridView fornece um `EmptyDataText` propriedade que pode ser usada para especificar uma mensagem de texto a ser exibido se não há nenhum registro na fonte de dados. Infelizmente, nem o DataList nem repetidor tem essa propriedade.

Para exibir uma mensagem informando ao usuário que não há nenhum produto correspondente para a categoria selecionada, precisamos adicionar um rótulo de controle para a página cuja `Text` propriedade recebe a mensagem a ser exibida se nenhum produto correspondente. Em seguida, precisamos definir programaticamente seu `Visible` propriedade com base em se ou não o DataList contém todos os itens.

Para fazer isso, comece adicionando um rótulo abaixo do DataList. Definir seu `ID` propriedade `NoProductsMessage` e sua `Text` propriedade como "Não há nenhum produto para a categoria selecionada..." Em seguida, precisamos definir este rótulo `Visible` propriedade com base em se nenhum dado foi associado ao `ProductsInCategory` DataList. Essa atribuição deve ser feita depois que os dados foi associados à DataList. Para o GridView, DetailsView e FormView, foi possível criar um manipulador de eventos para o controle `DataBound` evento, que é acionado após a associação de dados. No entanto, nem DataList nem repetidor tem um `DataBound` eventos disponíveis.

Para esse exemplo específico é possível atribuir o rótulo `Visible` propriedade o `Page_Load` manipulador de eventos, como os dados serão foram atribuídos a DataList antes da página `Load` eventos. No entanto, essa abordagem não funcionaria em geral, como os dados de ObjectDataSource podem ser associados ao DataList posteriormente no ciclo de vida da página. Por exemplo, se os dados exibidos se baseia no valor de outro controle, como ao exibir um relatório de detalhes/mestre usando DropDownList para manter os registros de "mestres", os dados não podem recuperar para o controle da Web de dados até que o `PreRender` estágio no ciclo de vida da página.

Uma solução que funcionará para todos os casos é atribuir o `Visible` propriedade `False` do DataList `ItemDataBound` (ou `ItemCreated`) manipulador de eventos ao associar um tipo de item de `Item` ou `AlternatingItem`. Nesse caso sabemos que não há dados de pelo menos um item na fonte de dados e, portanto, pode ocultar o `NoProductsMessage` rótulo. Além de manipulador de eventos, também é necessário um manipulador de eventos para o DataList `DataBinding` evento, onde podemos inicializar o rótulo `Visible` propriedade `True`. Como o `DataBinding` evento é acionado antes do `ItemDataBound` eventos, o rótulo `Visible` propriedade será definida inicialmente como `True`; se houver qualquer item de dados, no entanto, ele será definido como `False`. O código a seguir implementa esta lógica:

[!code-vb[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample6.vb)]

Todas as categorias no banco de dados Northwind são associadas com um ou mais produtos. Para testar esse recurso, tenha ajustado manualmente o banco de dados Northwind para este tutorial, reatribuindo todos os produtos associados à categoria de produzir (`CategoryID` = 7) para a categoria de Frutos do Mar (`CategoryID` = 8). Isso pode ser feito no Gerenciador de servidores, escolhendo nova consulta e usando os seguintes `UPDATE` instrução:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample7.sql)]

Depois de atualizar o banco de dados adequadamente, retornar o `CategoryListMaster.aspx` página e clique no link produzir. Como não há todos os produtos que pertencem à categoria de produção, você deve ver o "Não há nenhum produto para a categoria selecionada..." mensagem, conforme mostrado na Figura 9.


[![Uma mensagem é exibida se não houver nenhum produtos pertencentes à categoria selecionada](master-detail-filtering-acess-two-pages-datalist-vb/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image25.png)

**Figura 9**: uma mensagem é exibida se não houver nenhum produtos pertencentes à categoria selecionada ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-vb/_static/image27.png))


## <a name="summary"></a>Resumo

Enquanto os relatórios mestre/detalhes podem exibir registros mestre e de detalhes em uma única página, em muitos sites eles são separados em duas páginas da web. Neste tutorial vimos como implementar tal relatório mestre/detalhes categorias listadas em uma lista com marcadores usando um repetidor na página da web "mestre" e os produtos associados listados na página "Detalhes". Cada item da lista na página da web mestre continha um link para a página de detalhes do passado ao longo da linha `CategoryID` valor.

Na página de detalhes, recuperar os produtos para o fornecedor especificado foi realizado por meio de `ProductsBLL` da classe `GetProductsByCategoryID(categoryID)` método. O  *`categoryID`*  o valor do parâmetro foi especificado usando o `CategoryID` valor de querystring como a origem do parâmetro. Também vimos como exibir detalhes de categoria na página de detalhes do usando um FormView e como exibir uma mensagem se não houvesse nenhum produto que pertencem à categoria selecionada.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Zack Jones e Liz Shulok. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
[Próximo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)
