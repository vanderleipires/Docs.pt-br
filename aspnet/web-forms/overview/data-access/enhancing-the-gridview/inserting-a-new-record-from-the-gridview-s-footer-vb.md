---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: Inserindo um novo registro do rodapé do GridView (VB) | Microsoft Docs
author: rick-anderson
description: Enquanto o controle GridView não fornece suporte interno para inserir um novo registro de dados, este tutorial mostra como incrementar o GridView para incluir um...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 0c661190125e818d3abaf54f50a0067d0944a956
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830644"
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>Inserindo um novo registro do rodapé do GridView (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe) ou [baixar PDF](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> Enquanto o controle GridView não fornece suporte interno para inserir um novo registro de dados, este tutorial mostra como incrementar o GridView para incluir uma interface de inserção.


## <a name="introduction"></a>Introdução

Conforme discutido na [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) tutorial, os controles GridView, DetailsView e FormView Web cada incluem recursos de modificação de dados internos. Quando usado com controles de fonte de dados declarativa, esses três controles da Web podem ser rápida e facilmente configurados para modificar dados – e em cenários sem a necessidade de escrever uma única linha de código. Infelizmente, somente os controles DetailsView e FormView fornecem interna inserir, editar e excluir recursos. O GridView oferece apenas a edição e exclusão de suporte. No entanto, com um pouco graxa de canto, podemos pode aumentar o GridView para incluir uma interface de inserção.

Para adicionar recursos inserindo a GridView, somos responsáveis por decidir como os novos registros serão adicionados, criando a interface de inserção e escrever o código para inserir o novo registro. Neste tutorial, veremos adicionando a interface de inserção para o rodapé do GridView s de linhas (veja a Figura 1). A célula do rodapé para cada coluna inclui o elemento de dados apropriado coleção usuário interface (uma caixa de texto para o nome do produto s, DropDownList para o fornecedor e assim por diante). Também precisamos de uma coluna para uma adição botão que, quando clicado, causará um postback e inserir um novo registro para o `Products` usando os valores fornecidos na linha de rodapé da tabela.


[![A linha de rodapé fornece uma Interface para adicionar novos produtos](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**Figura 1**: A linha de rodapé fornece uma Interface para adição de novos produtos ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Etapa 1: Exibir informações sobre o produto em um GridView

Antes de nós mesmos estamos envolvem com a criação da interface de inserção no rodapé do GridView de s, deixe o primeiro foco sobre como adicionar um controle GridView à página que lista os produtos no banco de dados. Comece abrindo o `InsertThroughFooter.aspx` página o `EnhancedGridView` pasta e arraste um controle GridView da caixa de ferramentas para o Designer, definindo o s GridView `ID` propriedade para `Products`. Em seguida, use a marca inteligente do GridView s vinculá-la a um novo ObjectDataSource chamado `ProductsDataSource`.


[![Criar um novo ObjectDataSource chamado ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**Figura 2**: criar um novo ObjectDataSource nomeado `ProductsDataSource` ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


Configurar o ObjectDataSource para usar o `ProductsBLL` classe s `GetProducts()` método para recuperar informações sobre o produto. Para este tutorial, deixe s focalizar estritamente adicionando recursos de inserção e não se preocupar sobre edição e exclusão. Portanto, certifique-se de que a lista suspensa na guia Inserir é definida como `AddProduct()` e que as listas suspensas nas guias de UPDATE e DELETE são definidas como (nenhum).


[![Mapear o método AddProduct para o método de Insert () do ObjectDataSource s](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**Figura 3**: mapa de `AddProduct` método no s ObjectDataSource `Insert()` método ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![Define a atualização e exclusão guias listas suspensas para (nenhum)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**Figura 4**: defina a atualização e excluir guias menu suspenso lista como (nenhum) ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


Depois de concluir o Assistente de configurar fonte de dados s ObjectDataSource, Visual Studio adicionará automaticamente campos para o GridView para cada um dos campos de dados correspondentes. Por enquanto, deixe todos os campos adicionados pelo Visual Studio. Mais tarde neste tutorial vamos voltar e remover alguns dos campos cujos valores don t precisam ser especificado ao adicionar um novo registro.

Como há perto de 80 produtos no banco de dados, um usuário terá que rolar até a parte inferior da página da web para adicionar um novo registro. Portanto, deixe s habilitar paginação tornar a interface de inserção mais visível e acessível. Para ativar a paginação, basta marcar a caixa de seleção Habilitar paginação a marca inteligente do GridView s.

Neste ponto, GridView e ObjectDataSource s marcação declarativa deve ser semelhante ao seguinte:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![Todos os campos de dados de produto são exibidos em um GridView paginável](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**Figura 5**: todos os campos de dados de produto são exibidos em um GridView paginável ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>Etapa 2: Adicionando uma linha de rodapé

Juntamente com seu cabeçalho e linhas de dados, o GridView inclui uma linha de rodapé. As linhas de cabeçalho e rodapé são exibidas dependendo dos valores de s o GridView [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) e [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) propriedades. Para mostrar a linha de rodapé, basta definir a `ShowFooter` propriedade para `True`. Como ilustra a Figura 6, definindo o `ShowFooter` propriedade para `True` adiciona uma linha de rodapé à grade.


[![Para exibir a linha de rodapé, defina ShowFooter como True](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**Figura 6**: para exibir a linha de rodapé, definida `ShowFooter` à `True` ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))


Observe que a linha de rodapé tem uma cor de plano de fundo vermelho escuro. Isso ocorre devido ao tema DataWebControls é criada e aplicada a todas as páginas de volta a [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) tutorial. Especificamente, o `GridView.skin` arquivo configura o `FooterStyle` propriedade tal que usa o `FooterStyle` classe CSS. O `FooterStyle` classe é definida em `Styles.css` da seguinte maneira:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> Podemos var explorado usando a linha de rodapé do GridView s nos tutoriais anteriores. Se necessário, consulte a [exibindo informações de resumo no rodapé do GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) tutorial para uma atualização.


Depois de definir a `ShowFooter` propriedade para `True`, reserve um tempo para exibir a saída em um navegador. Atualmente, o rodapé linha contém qualquer texto ou controles da Web. Na etapa 3, modificaremos o rodapé para cada campo de GridView para que ele inclui a interface apropriada de inserção.


[![A linha de rodapé vazio é exibido acima a paginação de controles de Interface](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**Figura 7**: linha o rodapé vazio é exibido acima a paginação de controles de Interface ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Etapa 3: Personalizar a linha de rodapé

Volta a [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial vimos como bastante personalizar a exibição de uma determinada coluna de GridView usando TemplateFields (em vez de BoundFields ou CheckBoxFields); em [ Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) analisamos Usando TemplateFields para personalizar a interface de edição em um GridView. Lembre-se de que um TemplateField é composto de vários modelos que define a combinação de marcação, controles da Web e sintaxe de associação de dados usado para determinados tipos de linhas. O `ItemTemplate`, por exemplo, especifica o modelo usado para linhas de somente leitura, enquanto o `EditItemTemplate` define o modelo para a linha editável.

Juntamente com o `ItemTemplate` e `EditItemTemplate`, o TemplateField também inclui um `FooterTemplate` que especifica o conteúdo para a linha de rodapé. Portanto, podemos adicionar os controles da Web necessários para cada s de campo inserindo a interface para o `FooterTemplate`. Para iniciar, converta todos os campos no GridView TemplateFields. Isso pode ser feito clicando no link Edit Columns na GridView s marca inteligente, selecionando cada campo no canto inferior esquerdo e, em seguida, clicando em converter este campo em um TemplateField link.


![Converter cada campo em um TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**Figura 8**: converter cada campo em um TemplateField


Na converter este campo em um TemplateField transforma o tipo do campo atual em um TemplateField equivalente. Por exemplo, cada BoundField é substituído por um TemplateField com um `ItemTemplate` que contém um rótulo que exibe o campo de dados correspondente e um `EditItemTemplate` que exibe o campo de dados em uma caixa de texto. O `ProductName` BoundField foi convertido na TemplateField a seguinte marcação:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

Da mesma forma, o `Discontinued` CheckBoxField foi convertido em um TemplateField cuja `ItemTemplate` e `EditItemTemplate` contém um controle de caixa de seleção Web (com o `ItemTemplate` s caixa de seleção desabilitada). Somente leitura `ProductID` BoundField foi convertido em um TemplateField com um controle Label em ambos os `ItemTemplate` e `EditItemTemplate`. Em resumo, o campo em um TemplateField convertendo um GridView existente é uma maneira rápida e fácil para alternar para o TemplateField mais personalizável sem perder qualquer campo s funcionalidade existente.

Desde o GridView, está trabalhando t suporte de edição, fique à vontade remover o `EditItemTemplate` de cada TemplateField, deixando apenas o `ItemTemplate`. Depois de fazer isso, sua marcação declarativa do GridView s deve ser semelhante ao seguinte:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

Agora que cada campo de GridView foi convertido em um TemplateField, podemos inserir a interface de inserção apropriada em cada campo s `FooterTemplate`. Alguns dos campos não terá uma interface de inserção (`ProductID`, por exemplo); outros irão variar nos controles da Web usados para coletar as novas informações de produto s.

Para criar a interface de edição, escolha o link Editar modelos da marca inteligente s GridView. Em seguida, na lista suspensa, selecione o campo apropriado s `FooterTemplate` e arraste o controle apropriado na caixa de ferramentas para o Designer.


[![Adicionar a Interface apropriada de inserção para cada FooterTemplate s de campo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**Figura 9**: adicionar a Interface apropriada de inserção para cada campo s `FooterTemplate` ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


A lista com marcadores a seguir enumera os campos de GridView, especificando a interface de inserção para adicionar:

- `ProductID` Nenhum.
- `ProductName` Adicione uma caixa de texto e defina suas `ID` para `NewProductName`. Adicione um controle RequiredFieldValidator também para garantir que o usuário insere um valor para o novo nome de produto s.
- `SupplierID` Nenhum.
- `CategoryID` Nenhum.
- `QuantityPerUnit` Adicione uma caixa de texto, definindo sua `ID` para `NewQuantityPerUnit`.
- `UnitPrice` Adicione uma caixa de texto chamada `NewUnitPrice` e um CompareValidator que garante que o valor inserido é um valor de moeda, maior que ou igual a zero.
- `UnitsInStock` usar uma caixa de texto cuja `ID` é definido como `NewUnitsInStock`. Inclua um CompareValidator que garante que o valor inserido é um valor inteiro maior ou igual a zero.
- `UnitsOnOrder` usar uma caixa de texto cuja `ID` é definido como `NewUnitsOnOrder`. Inclua um CompareValidator que garante que o valor inserido é um valor inteiro maior ou igual a zero.
- `ReorderLevel` usar uma caixa de texto cuja `ID` é definido como `NewReorderLevel`. Inclua um CompareValidator que garante que o valor inserido é um valor inteiro maior ou igual a zero.
- `Discontinued` Adicione uma caixa de seleção, definindo sua `ID` para `NewDiscontinued`.
- `CategoryName` Adicione uma DropDownList e defina suas `ID` para `NewCategoryID`. Associá-lo a um novo ObjectDataSource denominado `CategoriesDataSource` e configurá-lo para usar o `CategoriesBLL` classe s `GetCategories()` método. Ter o s DropDownList `ListItem` exibição s a `CategoryName` dados de campo, usando o `CategoryID` campo de dados como seus valores.
- `SupplierName` Adicione uma DropDownList e defina suas `ID` para `NewSupplierID`. Associá-lo a um novo ObjectDataSource denominado `SuppliersDataSource` e configurá-lo para usar o `SuppliersBLL` classe s `GetSuppliers()` método. Ter o s DropDownList `ListItem` exibição s a `CompanyName` dados de campo, usando o `SupplierID` campo de dados como seus valores.

Para cada um dos controles de validação, desmarque as `ForeColor` propriedade para que o `FooterStyle` cor de primeiro plano branco s da classe CSS será usado em vez do padrão vermelho. Também use o `ErrorMessage` propriedade para obter uma descrição detalhada, mas defina o `Text` propriedade como um asterisco. Para impedir que o texto do controle s validação fazendo com que a interface de inserção para ficar em duas linhas, defina as `FooterStyle` s `Wrap` propriedade como false para cada um do `FooterTemplate` s que usam um controle de validação. Por fim, adicione um controle ValidationSummary abaixo a GridView e defina sua `ShowMessageBox` propriedade para `True` e seu `ShowSummary` propriedade `False`.

Ao adicionar um novo produto, é necessário fornecer o `CategoryID` e `SupplierID`. Essas informações são capturadas por meio de DropDownLists nas células do rodapé para o `CategoryName` e `SupplierName` campos. Optei por usar esses campos em vez de `CategoryID` e `SupplierID` TemplateFields como linhas de usuário na grade s dados é provavelmente mais interessado em ver os nomes de categoria e fornecedor em vez de seus valores de ID. Uma vez que o `CategoryID` e `SupplierID` valores agora estão sendo capturados na `CategoryName` e `SupplierName` interfaces de inserção de s de campo, podemos remover o `CategoryID` e `SupplierID` TemplateFields de GridView.

Da mesma forma, o `ProductID` não é usado ao adicionar um novo produto, portanto, o `ProductID` TemplateField também pode ser removido. No entanto, deixe o s deixar o `ProductID` campo na grade. Além de caixas de texto, DropDownLists, caixas de seleção e controles de validação que compõem a interface de inserção, precisaremos também uma adição botão que, quando clicado, executa a lógica para adicionar o novo produto no banco de dados. Na etapa 4, incluiremos um botão Add na interface de inserção na `ProductID` TemplateField s `FooterTemplate`.

Fique à vontade para melhorar a aparência de vários campos de GridView. Por exemplo, você talvez queira formatar a `UnitPrice` Alinhar à direita de valores como uma moeda, o `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` campos e atualizar o `HeaderText` valores para o TemplateFields.

Depois de criar a enorme quantidade de inserção de interfaces na `FooterTemplate` s, removendo a `SupplierID`, e `CategoryID` TemplateFields e melhorar a estética da grade por meio de formatação e alinhando TemplateFields, seu s GridView declarativo marcação deve ser semelhante ao seguinte:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

Quando visualizado por meio de um navegador, a linha de rodapé do GridView s agora inclui concluído inserindo interface (consulte a Figura 10). Neste ponto, o inserção t da interface incluem um meio para que o usuário indique que s she inseriu os dados para o novo produto e deseja inserir um novo registro de banco de dados. Além disso, podemos ver ainda para abordar como os dados inseridos no rodapé se traduzirá em um novo registro no `Products` banco de dados. Na etapa 4, examinaremos como incluir um botão Add à interface de inserção e como executar código no postback quando ele s clicado. Etapa 5 mostra como inserir um novo registro usando os dados do rodapé.


[![O rodapé do GridView fornece uma Interface para adicionar um novo registro](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**Figura 10**: O rodapé do GridView fornece uma Interface para adicionar um novo registro ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Etapa 4: Incluindo um botão Add na Interface de inserção

É necessário incluir um botão Add em algum lugar na interface de inserção, pois o s da linha de rodapé inserindo interface atualmente não tem os meios para o usuário indicar que ele tem terminado de inserir as novas informações de produto s. Isso poderia ser colocado em um dos existentes `FooterTemplate` s ou podemos poderia adicionar uma nova coluna à grade para essa finalidade. Para este tutorial, vamos s colocar no botão Adicionar na `ProductID` TemplateField s `FooterTemplate`.

No Designer, clique no link Editar modelos na marca inteligente GridView s e, em seguida, escolha o `ProductID` campo s `FooterTemplate` na lista suspensa. Adicionar um controle da Web de botão (ou um LinkButton ou ImageButton, se você preferir) para o modelo, definindo sua identificação para `AddProduct`, seus `CommandName` para inserir e seu `Text` propriedade para adicionar, conforme mostrado na Figura 11.


[![Coloque o botão Adicionar s ProductID TemplateField noFooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**Figura 11**: colocar o botão Adicionar na `ProductID` s TemplateField `FooterTemplate` ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


Depois que você tiver incluído o botão Adicionar, teste a página em um navegador. Observe que, ao clicar no botão Adicionar com dados inválidos na interface de inserção, o postback é circuited curtas e controle ValidationSummary indica dados inválidos (veja a Figura 12). Os dados apropriados inseridos, clicando no botão Adicionar faz com que um postback. Nenhum registro é adicionado ao banco de dados, no entanto. Precisamos escrever um pouco de código para realmente executar a inserção.


[![O botão Adicionar s Postback é curto Circuited se não houver dados inválidos na Interface de inserção](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**Figura 12**: O botão Adicionar s Postback é curto Circuited se não houver dados inválidos na Interface de inserção ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> Os controles de validação na interface de inserção não foram atribuídos a um grupo de validação. Isso funciona bem desde que a interface de inserção é o único conjunto de controles de validação na página. Se, no entanto, há outros controles de validação na página (como controles de validação na interface de edição de grade s), os controles de validação na inserção de interface e adicionar botão s `ValidationGroup` propriedades devem ser atribuídas como o mesmo valor de so como para Associe esses controles a um grupo de validação em particular. Ver [dissecando os controles de validação no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) para obter mais informações sobre como particionar os controles de validação e botões em uma página em grupos de validação.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Etapa 5: Inserindo um novo registro para o`Products`tabela

Ao utilizar os recursos internos de edição de GridView, GridView manipula automaticamente todo o trabalho necessário para executar a atualização. Em particular, quando o botão de atualização é clicado ele copia os valores inseridos de interface de edição para os parâmetros no s ObjectDataSource `UpdateParameters` coleta e aciona a atualização, invocando o s ObjectDataSource `Update()` método. Uma vez que o GridView não fornece tal funcionalidade interna para a inserção, devemos implementar código que chama o s ObjectDataSource `Insert()` método e copia os valores da inserção de interface para o s ObjectDataSource `InsertParameters` coleção .

Essa lógica de inserção deve ser executada depois que foi clicado no botão Adicionar. Conforme discutido na [adicionar e responder a botões em um GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md) tutorial, sempre que um botão, LinkButton ou ImageButton em um GridView é clicado, o GridView s `RowCommand` evento é acionado em um postback. Esse evento é acionado se o botão, LinkButton ou ImageButton foi adicionado explicitamente, como o botão Adicionar na linha de rodapé ou se ele foi adicionado automaticamente pelo GridView (como botões de link na parte superior de cada coluna quando habilitar a classificação é selecionado, ou o Botões de link na interface de paginação quando habilitar a paginação está selecionada).

Portanto, para responder ao usuário clicar no botão Adicionar, precisamos criar um manipulador de eventos para o s GridView `RowCommand` eventos. Uma vez que esse evento é acionado sempre que *qualquer* ImageButton GridView, LinkButton ou botão é clicado, ele s vital que estamos apenas prosseguir com a lógica de inserção se o `CommandName` propriedade transmitido os mapeamentos de manipulador de eventos para o `CommandName` o valor do botão Adicionar (inserção). Além disso, podemos também somente deverá continuar se os controles de validação relatam dados válidos. Para acomodar isso, crie um manipulador de eventos para o `RowCommand` evento com o código a seguir:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> Você pode estar se perguntando por que o manipulador de eventos incomodá-verificando o `Page.IsValid` propriedade. Afinal de contas, t obtida a postagem ser suprimidos se dados inválidos são fornecidos na interface do inserindo? Essa suposição está correta, desde que o usuário não tiver desabilitado o JavaScript ou tomou medidas para driblar a lógica de validação do lado do cliente. Em suma, um nunca dependa estritamente validação do lado do cliente; sempre deve ser executada uma verificação do lado do servidor quanto à validade antes de trabalhar com os dados.


Na etapa 1 criamos a `ProductsDataSource` ObjectDataSource, de modo que suas `Insert()` método é mapeado para o `ProductsBLL` classe s `AddProduct` método. Para inserir o novo registro para o `Products` tabela, podemos simplesmente invocar o s ObjectDataSource `Insert()` método:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

Agora que o `Insert()` método foi invocado, tudo o que resta fazer é copiar os valores da interface de inserção para os parâmetros passados para o `ProductsBLL` classe s `AddProduct` método. Como vimos na [examinando os eventos associados inserindo, atualizando e excluindo](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) tutorial, isso pode ser feito com o ObjectDataSource s `Inserting` eventos. No `Inserting` eventos, precisamos referenciar programaticamente os controles do `Products` rodapé do GridView s de linhas e atribuir os valores para o `e.InputParameters` coleção. Se o usuário omite um valor como deixar o `ReorderLevel` em branco de caixa de texto, precisamos especificar que o valor inserido no banco de dados deve ser `NULL`. Uma vez que o `AddProducts` método aceita tipos anuláveis para os campos de banco de dados que permite valor nulo, simplesmente use um tipo anulável e defina seu valor como `Nothing` no caso em que a entrada do usuário for omitida.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

Com o `Inserting` manipulador de eventos concluído, novos registros podem ser adicionados à `Products` tabela de banco de dados por meio da linha de rodapé do GridView s. Vá em frente e tente adicionar vários novos produtos.

## <a name="enhancing-and-customizing-the-add-operation"></a>Aprimorar e personalizar a operação de adição

No momento, clicando no botão Adicionar adiciona um novo registro na tabela de banco de dados, mas não fornece qualquer tipo de comentários visuais que o registro tenha sido adicionado com êxito. O ideal é que uma caixa de chamadas alerta controle ou no lado do cliente da Web do rótulo informa o usuário que sua inserção foi concluída com êxito. Posso deixar isso como um exercício para o leitor.

O GridView usado neste tutorial não se aplica a uma ordem de classificação para os produtos listados, nem permite que o usuário final classificar os dados. Consequentemente, os registros são ordenados como estão no banco de dados por seu campo de chave primária. Uma vez que cada novo registro tem um `ProductID` valor maior que o último deles, sempre que um novo produto é adicionado, ele é é incluído no final da grade. Portanto, você talvez queira enviar automaticamente o usuário para a última página do GridView, depois de adicionar um novo registro. Isso pode ser feito adicionando a seguinte linha de código após a chamada para `ProductsDataSource.Insert()` no `RowCommand` manipulador de eventos para indicar que o usuário precisa ser enviada para a última página após a associação dos dados a GridView:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` é uma variável booliana de nível de página que inicialmente é atribuída um valor de `False`. Em s GridView `DataBound` manipulador de eventos, se `SendUserToLastPage` é false, o `PageIndex` propriedade é atualizada para enviar o usuário para a última página.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

O motivo pelo qual o `PageIndex` propriedade é definida na `DataBound` manipulador de eventos (em oposição ao `RowCommand` manipulador de eventos) é porque quando o `RowCommand` manipulador de eventos é acionado, ve ainda para adicionar o novo registro para o `Products` tabela de banco de dados. Portanto, nos `RowCommand` o último índice de página de manipulador de eventos (`PageCount - 1`) representa o índice da última página *antes de* foi adicionado o novo produto. Para a maioria dos produtos que estão sendo adicionados, o último índice de página é o mesmo depois de adicionar o novo produto. Mas quando o produto adicionado resulta em um novo último índice de página, se atualizarmos incorretamente a `PageIndex` no `RowCommand` manipulador de eventos, em seguida, podemos serão levados para a segunda à última página (o último índice de página antes de adicionar o novo produto) em vez de última página novo i ndice. Uma vez que o `DataBound` manipulador de eventos é acionado depois que o novo produto foi adicionado e os dados se a grade, definindo o `PageIndex` propriedade existe sabemos que estamos aproveitando o último índice de página correta.

Por fim, o GridView usado neste tutorial é bastante ampla devido ao número de campos que devem ser coletados para adicionar um novo produto. Devido a essa largura um layout vertical de s DetailsView talvez preferencial. O s GridView largura geral poderia ser reduzida por coletar menos entradas. Talvez, don precisa coletar as `UnitsOnOrder`, `UnitsInStock`, e `ReorderLevel` campos ao adicionar um novo produto, caso em que esses campos pôde ser removidos do GridView.

Para ajustar os dados coletados, podemos usar uma das duas abordagens:

- Continuar a usar o `AddProduct` método que espera valores para o `UnitsOnOrder`, `UnitsInStock`, e `ReorderLevel` campos. No `Inserting` manipulador de eventos, fornecer embutido em código, o padrão de valores a serem usados para essas entradas que foram removidas da interface de inserção.
- Criar uma nova sobrecarga da `AddProduct` método no `ProductsBLL` classe que não aceita entradas para o `UnitsOnOrder`, `UnitsInStock`, e `ReorderLevel` campos. Em seguida, na página do ASP.NET, configure o ObjectDataSource para usar essa nova sobrecarga.

Cada opção funcionará igualmente bem. Após tutoriais usamos a última opção, criando várias sobrecargas para o `ProductsBLL` classe s `UpdateProduct` método.

## <a name="summary"></a>Resumo

Os recursos internos de inserção encontrados na DetailsView e FormView não tem de GridView, mas com um pouco de esforço, uma interface de inserção pode ser adicionada à linha de rodapé. Para exibir a linha de rodapé em um GridView simplesmente definir seus `ShowFooter` propriedade para `True`. O conteúdo da linha de rodapé pode ser personalizado para cada campo ao converter o campo em um TemplateField e adicionando a inserção de interface para o `FooterTemplate`. Como vimos neste tutorial, o `FooterTemplate` pode conter botões, caixas de texto, DropDownLists, caixas de seleção, os controles de fonte de dados para popular os controles de Web controlado por dados (como DropDownLists) e controles de validação. Juntamente com controles para coletar a entrada do usuário s, é necessário um botão Adicionar, LinkButton ou ImageButton.

Quando o botão Adicionar é clicado, o s ObjectDataSource `Insert()` método é chamado para iniciar o fluxo de trabalho de inserção. O ObjectDataSource, em seguida, chamará o método insert configurado (o `ProductsBLL` classe s `AddProduct` método neste tutorial). Podemos deve copiar os valores de s GridView inserindo a interface para o s ObjectDataSource `InsertParameters` coleção antes do método de inserção que está sendo invocado. Isso pode ser feito referenciando programaticamente os controles da Web interface inserindo em s ObjectDataSource `Inserting` manipulador de eventos.

Esse tutorial conclui nossa olhada técnicas para melhorar a aparência de s GridView. O próximo conjunto de tutoriais irá examinar os dados de controles da Web e de como trabalhar com dados binários como imagens, PDFs, documentos do Word e assim por diante.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Bernadette Leigh. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-a-gridview-column-of-checkboxes-vb.md)
