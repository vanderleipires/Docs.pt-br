---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: Inserindo um novo registro de rodapé do GridView (VB) | Microsoft Docs
author: rick-anderson
description: Enquanto o controle GridView não oferece suporte interno para inserir um novo registro de dados, este tutorial mostra como aumentar o GridView para incluir um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 32f3cb23805813135bf463720e7479f5f819deb7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888364"
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>Inserindo um novo registro de rodapé do GridView (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe) ou [baixar PDF](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> Enquanto o controle GridView não oferece suporte interno para inserir um novo registro de dados, este tutorial mostra como aumentar o GridView para incluir uma interface de inserção.


## <a name="introduction"></a>Introdução

Como discutido o [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) tutorial, os controles GridView, DetailsView e FormView Web cada incluem capacidades de modificação de dados interno. Quando usado com controles de fonte de dados declarativo, esses três controles da Web podem ser rapidamente e facilmente configurados para modificar dados - e em cenários sem a necessidade de escrever uma única linha de código. Infelizmente, somente os controles de DetailsView e FormView fornecem inserindo internas, editar e excluir recursos. GridView oferece apenas a edição e exclusão de suporte. No entanto, com um pouco graxa do ângulo, podemos pode aumentar GridView para incluir uma interface de inserção.

Adicionar recursos inserindo a GridView, somos responsáveis para decidir como novos registros serão adicionados, criar a interface de inserção e escrever o código para inserir o novo registro. Neste tutorial, que examinaremos adicionando a interface de inserção para o rodapé do GridView s linha (consulte a Figura 1). A célula de rodapé para cada coluna inclui o elemento de dados apropriado coleção usuário interface (uma caixa de texto para o nome do produto s, DropDownList para o fornecedor e assim por diante). Também precisamos de uma coluna para adicionar botão que, quando clicado, causar um postback e inserir um novo registro para o `Products` usando os valores fornecidos na linha de rodapé da tabela.


[![A linha de rodapé fornece uma Interface para a adição de novos produtos](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**Figura 1**: A linha de rodapé fornece uma Interface para adição de novos produtos ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Etapa 1: Exibir informações de produto em um controle GridView

Antes de em nós preocupar com a criação da interface de inserção no rodapé s GridView, deixe o primeiro foco sobre como adicionar um controle GridView para a página que lista os produtos no banco de dados. Comece abrindo o `InsertThroughFooter.aspx` página o `EnhancedGridView` pasta e arraste um controle GridView da caixa de ferramentas para o Designer, definindo o GridView s `ID` propriedade `Products`. Em seguida, use a marca inteligente de s GridView para associá-lo para um novo ObjectDataSource denominado `ProductsDataSource`.


[![Criar um novo ObjectDataSource denominado ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**Figura 2**: criar um novo ObjectDataSource nomeado `ProductsDataSource` ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


Configurar o ObjectDataSource para usar o `ProductsBLL` classe s `GetProducts()` método para recuperar informações sobre o produto. Para este tutorial, permitem que o foco estritamente na adição de recursos de inserção e não se preocupe com a edição e exclusão. Portanto, certifique-se de que a lista suspensa na guia Inserir é definida como `AddProduct()` e que as listas suspensas nas guias de UPDATE e DELETE são definidas como (nenhum).


[![Mapear o método AddProduct ao método ObjectDataSource s Insert)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**Figura 3**: mapa de `AddProduct` método para o s ObjectDataSource `Insert()` método ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![Define as listas suspensas UPDATE e DELETE guias como (nenhum)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**Figura 4**: Configure a atualização e excluir guias listas suspensas para (nenhum) ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


Depois de concluir o Assistente para configurar fonte de dados de s ObjectDataSource, Visual Studio automaticamente adicionará campos a GridView para cada um dos campos de dados correspondentes. Por enquanto, deixe todos os campos adicionados pelo Visual Studio. Posteriormente neste tutorial, vamos voltar e remover alguns dos campos cujos valores don t precisam ser especificado ao adicionar um novo registro.

Como há quase 80 produtos no banco de dados, um usuário terá que rolar até a parte inferior da página da web para adicionar um novo registro. Portanto, permitem s habilitar paginação fazer a interface inserindo mais visível e acessível. Para ativar a paginação, basta marcar a caixa de seleção Habilitar paginação a marca inteligente de s GridView.

Neste ponto, a marcação de declarativa s GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![Todos os campos de dados de produto são exibidos em um GridView paginável](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**Figura 5**: todos os campos de dados de produto são exibidos em um GridView paginável ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>Etapa 2: Adicionar uma linha de rodapé

Juntamente com seu cabeçalho e linhas de dados, o GridView inclui uma linha de rodapé. As linhas de cabeçalho e rodapé são exibidas, dependendo dos valores de s a GridView [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) e [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) propriedades. Para mostrar a linha de rodapé, basta definir o `ShowFooter` propriedade `True`. Como ilustra a Figura 6, definindo o `ShowFooter` propriedade `True` adiciona uma linha de rodapé à grade.


[![Para exibir a linha de rodapé, defina ShowFooter como True](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**Figura 6**: para exibir a linha de rodapé, defina `ShowFooter` para `True` ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))


Observe que a linha de rodapé de página tem uma cor de plano de fundo vermelho escuro. Isso ocorre devido ao tema DataWebControls é criado e aplicado a todas as páginas no [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) tutorial. Especificamente, o `GridView.skin` arquivo configura o `FooterStyle` propriedade tal que usa o `FooterStyle` classe CSS. O `FooterStyle` classe é definida em `Styles.css` da seguinte maneira:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> Podemos ve explorado usando a linha de rodapé GridView s nos tutoriais anteriores. Se necessário, consultar o [exibindo informações de resumo no rodapé do GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) tutorial para uma atualização.


Depois de definir o `ShowFooter` propriedade `True`, reserve um tempo para exibir a saída em um navegador. Atualmente o rodapé linha contém qualquer texto ou controles da Web. Na etapa 3, modificaremos o rodapé para cada campo de GridView para que ele inclui a interface apropriada da inserção.


[![A linha de rodapé vazia é exibido acima a paginação controles de Interface](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**Figura 7**: rodapé vazio a linha é exibida acima a paginação controles de Interface ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Etapa 3: Personalizando a linha de rodapé

Volta o [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial vimos como muito personalizar a exibição de uma coluna em particular GridView usando TemplateFields (em vez de BoundFields ou CheckBoxFields); no [ Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) examinamos Usando TemplateFields para personalizar a interface de edição em um controle GridView. Lembre-se de que um TemplateField é composto de um número de modelos que define a mistura de marcação, controles da Web e sintaxe de associação de dados usada para determinados tipos de linhas. O `ItemTemplate`, por exemplo, especifica o modelo usado para linhas de somente leitura, enquanto o `EditItemTemplate` define o modelo para a linha editável.

Juntamente com o `ItemTemplate` e `EditItemTemplate`, o TemplateField também inclui um `FooterTemplate` que especifica o conteúdo da linha de rodapé. Portanto, podemos adicionar os controles da Web necessários para cada s campo inserindo a interface para o `FooterTemplate`. Para iniciar, converta todos os campos em GridView para TemplateFields. Isso pode ser feito clicando no link Editar colunas do GridView s marca inteligente, selecionando cada campo no canto inferior esquerdo e, em seguida, clicando em converter este campo em um TemplateField link.


![Converter cada campo em um TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**Figura 8**: converter cada campo em um TemplateField


Clicar em converter este campo em um TemplateField ativa o tipo atual de campo em um TemplateField equivalente. Por exemplo, cada BoundField é substituído por um TemplateField com um `ItemTemplate` que contém um rótulo que exibe o campo de dados correspondente e um `EditItemTemplate` que exibe o campo de dados em uma caixa de texto. O `ProductName` BoundField foi convertido na seguinte marcação TemplateField:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

Da mesma forma, o `Discontinued` CheckBoxField foi convertido em um TemplateField cujo `ItemTemplate` e `EditItemTemplate` contém um controle de caixa de seleção Web (com a `ItemTemplate` s caixa de seleção desabilitada). Somente leitura `ProductID` BoundField foi convertido em um TemplateField com um controle de rótulo em ambos os `ItemTemplate` e `EditItemTemplate`. Em resumo, o campo em um TemplateField convertendo um GridView existente é uma maneira rápida e fácil para alternar para o mais personalizável TemplateField sem perder qualquer campo s funcionalidade existente.

Desde o GridView, está trabalhando com t suporte de edição, fique à vontade para remover o `EditItemTemplate` de cada TemplateField, deixando apenas o `ItemTemplate`. Depois de fazer isso, sua marcação declarativa do GridView s deve ser semelhante ao seguinte:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

Agora que cada campo de GridView é convertido em um TemplateField, podemos inserir inserindo interface apropriada em cada campo s `FooterTemplate`. Alguns dos campos não terá uma interface inserindo (`ProductID`, por exemplo); outros variam nos controles da Web usados para coletar as novas informações de produto s.

Para criar a interface de edição, escolha o link Editar modelos de marca inteligente s GridView. Em seguida, na lista suspensa, selecione o campo apropriado s `FooterTemplate` e arraste o controle apropriado na caixa de ferramentas para o Designer.


[![Adicionar a Interface apropriada da inserção para cada FooterTemplate s de campo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**Figura 9**: adicionar a Interface apropriada da inserção para cada campo s `FooterTemplate` ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


A lista com marcadores a seguir enumera os campos de GridView, especificando a interface inserir para adicionar:

- `ProductID` Nenhum.
- `ProductName` Adicione uma caixa de texto e defina seu `ID` para `NewProductName`. Adicione um controle RequiredFieldValidator para garantir que o usuário insira um valor para o novo nome de produto s.
- `SupplierID` Nenhum.
- `CategoryID` Nenhum.
- `QuantityPerUnit` Adicionar uma caixa de texto, definindo seu `ID` para `NewQuantityPerUnit`.
- `UnitPrice` Adicionar uma caixa de texto denominada `NewUnitPrice` e um CompareValidator que garante que o valor inserido é um valor de moeda maior que ou igual a zero.
- `UnitsInStock` usar uma caixa de texto cuja `ID` é definido como `NewUnitsInStock`. Inclua um CompareValidator que garante que o valor inserido é um valor inteiro maior ou igual a zero.
- `UnitsOnOrder` usar uma caixa de texto cuja `ID` é definido como `NewUnitsOnOrder`. Inclua um CompareValidator que garante que o valor inserido é um valor inteiro maior ou igual a zero.
- `ReorderLevel` usar uma caixa de texto cuja `ID` é definido como `NewReorderLevel`. Inclua um CompareValidator que garante que o valor inserido é um valor inteiro maior ou igual a zero.
- `Discontinued` Adicionar uma caixa de seleção, definindo seu `ID` para `NewDiscontinued`.
- `CategoryName` Adicione um DropDownList e defina seu `ID` para `NewCategoryID`. Associá-lo a um novo ObjectDataSource denominado `CategoriesDataSource` e configurá-lo para usar o `CategoriesBLL` classe s `GetCategories()` método. Ter o s DropDownList `ListItem` exibição s a `CategoryName` dados campo, usando o `CategoryID` campo de dados como seus valores.
- `SupplierName` Adicione um DropDownList e defina seu `ID` para `NewSupplierID`. Associá-lo a um novo ObjectDataSource denominado `SuppliersDataSource` e configurá-lo para usar o `SuppliersBLL` classe s `GetSuppliers()` método. Ter o s DropDownList `ListItem` exibição s a `CompanyName` dados campo, usando o `SupplierID` campo de dados como seus valores.

Para cada um dos controles de validação, limpar o `ForeColor` propriedade para que o `FooterStyle` cor de primeiro plano branca classe s CSS será usado no lugar do padrão vermelho. Usar também o `ErrorMessage` propriedade para uma descrição detalhada, mas definir o `Text` propriedade como um asterisco. Para impedir que o texto de validação de controle s fazendo com que a interface de inserção para ficar em duas linhas, definir o `FooterStyle` s `Wrap` a propriedade como false para cada uma da `FooterTemplate` s que usam um controle de validação. Finalmente, adicione um controle ValidationSummary abaixo a GridView e defina seu `ShowMessageBox` propriedade `True` e sua `ShowSummary` propriedade `False`.

Ao adicionar um novo produto, é preciso fornecer o `CategoryID` e `SupplierID`. Essas informações são capturadas por meio de DropDownLists nas células do rodapé para o `CategoryName` e `SupplierName` campos. Decidi usar esses campos em vez do `CategoryID` e `SupplierID` TemplateFields como linhas de usuário na grade s dados é provavelmente mais interessado em ver os nomes de categoria e fornecedor em vez de seus valores de ID. Como o `CategoryID` e `SupplierID` valores agora estão sendo capturados no `CategoryName` e `SupplierName` interfaces de inserção do campo s, é possível remover o `CategoryID` e `SupplierID` TemplateFields de GridView.

Da mesma forma, o `ProductID` não é usado ao adicionar um novo produto, portanto, o `ProductID` TemplateField também pode ser removido. No entanto, permitem s deixar o `ProductID` campo na grade. Além de caixas de texto, DropDownLists, caixas de seleção e controles de validação que compõem a interface inserindo, precisaremos também adicionar botão que, quando clicado, executa a lógica para adicionar o novo produto no banco de dados. Na etapa 4 nós incluirá um botão Adicionar na interface de inserção no `ProductID` TemplateField s `FooterTemplate`.

Fique à vontade para melhorar a aparência de vários campos de GridView. Por exemplo, você talvez queira formatar o `UnitPrice` Alinhar à direita de valores de moeda, o `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` campos e atualizar o `HeaderText` valores para o TemplateFields.

Depois de criar a enorme quantidade de inserção de interfaces de `FooterTemplate` s, removendo o `SupplierID`, e `CategoryID` TemplateFields e melhorar a estética da grade por meio de formatação e alinhando TemplateFields, seu s GridView declarativo marcação deve ser semelhante ao seguinte:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

Quando visualizado por meio de um navegador, a linha de rodapé GridView s agora inclui concluído inserindo interface (consulte a Figura 10). Neste ponto, a inserção interface incluir um meio para o usuário indicar que o s she inseriu os dados para o novo produto e deseja inserir um novo registro no banco de dados. Além disso, podemos ve ainda para tratar como os dados inseridos no rodapé serão convertidos em um novo registro no `Products` banco de dados. Na etapa 4, examinaremos como incluir um botão Adicionar para a interface de inserção e como executar o código em postback quando ele s clicado. Etapa 5 mostra como inserir um novo registro de usando os dados do rodapé.


[![O rodapé de GridView fornece uma Interface para adicionar um novo registro](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**Figura 10**: rodapé GridView fornece uma Interface para adicionar um novo registro ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Etapa 4: Incluindo um botão Adicionar na Interface de inserção

É preciso incluir um botão Adicionar em algum lugar na interface inserindo desde que o s de linha de rodapé inserindo interface atualmente não tem os meios para o usuário indicar que eles terminar de inserir as novas informações de produto s. Isso pode ser colocado em um destes existente `FooterTemplate` s, poderá adicionar uma nova coluna à grade para essa finalidade. Para este tutorial, vamos s colocar no botão Adicionar no `ProductID` TemplateField s `FooterTemplate`.

No Designer, clique no link Editar modelos na marca inteligente GridView s e, em seguida, escolha o `ProductID` campo s `FooterTemplate` na lista suspensa. Adicionar um controle de botão Web (ou um LinkButton ou ImageButton, se você preferir) para a configuração do modelo, sua ID `AddProduct`, sua `CommandName` para inserir e sua `Text` propriedade para adicionar, conforme mostrado na Figura 11.


[![Coloque o botão Adicionar em ProductID TemplateField s FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**Figura 11**: colocar o botão Adicionar no `ProductID` TemplateField s `FooterTemplate` ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


Depois que você tiver incluído no botão Adicionar, teste a página em um navegador. Observe que, ao clicar no botão Adicionar com dados inválidos na interface de inserção, a postagem é circuited curta e controle ValidationSummary indica os dados inválidos (veja a Figura 12). Com dados apropriados inseridos, clicando no botão Adicionar causa um postback. Nenhum registro é adicionado ao banco de dados, no entanto. Precisamos escrever um pouco de código para realmente executar a inserção.


[![O botão Adicionar s Postback é Circuited curto se há dados inválidos na Interface de inserção](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**Figura 12**: O botão Adicionar Postback é Circuited curto se há dados inválidos na Interface de inserção ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> Os controles de validação na interface de inserção não foram atribuídos a um grupo de validação. Isso funciona bem desde que a interface de inserção é o único conjunto de controles de validação na página. Se, no entanto, há outros controles de validação na página (por exemplo, controles de validação na interface de edição de grade s), os controles de validação na inserção de interface e adicionar um botão s `ValidationGroup` propriedades devem ter o mesmo valor so como para Associe esses controles a um grupo de validação em particular. Consulte [dissecando os controles de validação no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) para obter mais informações sobre particionamento os controles de validação e botões em uma página em grupos de validação.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Etapa 5: Inserindo um novo registro para o`Products`tabela

Ao usar os recursos internos de edição de GridView, GridView controla automaticamente todo o trabalho necessário para executar a atualização. Em particular, quando clicar no botão de atualização copia os valores inseridos a partir da interface de edição para os parâmetros em ObjectDataSource s `UpdateParameters` coleção e é ativada, a atualização invocando o s ObjectDataSource `Update()` método. Como GridView não fornece funcionalidade interna para inserir, devemos implementar o código que chama o s ObjectDataSource `Insert()` método e copia os valores de inserção da interface para o s ObjectDataSource `InsertParameters` coleção .

Essa lógica de inserção deve ser executada após ser clicado no botão Adicionar. Conforme discutido no [e responder aos botões em um GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md) tutorial, sempre que um botão, LinkButton ou ImageButton em um GridView é clicado, o s GridView `RowCommand` evento ser acionado em um postback. Esse evento é acionado se o botão, LinkButton ou ImageButton foi adicionado explicitamente como o botão Adicionar na linha de rodapé ou se ele foi adicionado automaticamente por GridView (como os botões de link na parte superior de cada coluna quando habilitar classificação é selecionado, ou o Botões de link na interface de paginação quando habilitar paginação está selecionada).

Portanto, para responder se o usuário clicar no botão Adicionar, precisamos criar um manipulador de eventos para o s GridView `RowCommand` eventos. Como esse evento é acionado sempre que *qualquer* Button, LinkButton ou ImageButton em GridView é clicado, ele s vital que apenas continuar com a lógica de inserção se o `CommandName` propriedade passados para os mapeamentos de manipulador de eventos para o `CommandName` valor do botão Adicionar (inserir). Além disso, podemos também somente deverá continuar se os controles de validação de dados válidos de relatório. Para acomodar isso, crie um manipulador de eventos para o `RowCommand` evento com o código a seguir:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> Você deve estar se perguntando por que o manipulador de eventos incomodá verificando o `Page.IsValid` propriedade. Afinal, t ganha o postback suprimidos se dados inválido são fornecidos na interface inserindo? Essa pressuposição está correta, desde que o usuário não tiver desabilitado o JavaScript ou procurou contornar a lógica de validação do lado do cliente. Resumindo, um nunca dependa estritamente validação do lado do cliente; sempre deve ser executada uma verificação do lado do servidor para validade antes de trabalhar com os dados.


Na etapa 1 criamos o `ProductsDataSource` ObjectDataSource, de modo que seu `Insert()` método é mapeado para o `ProductsBLL` classe s `AddProduct` método. Para inserir o novo registro para o `Products` tabela, podemos simplesmente pode invocar o s ObjectDataSource `Insert()` método:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

Agora que o `Insert()` método foi chamado, tudo o que resta é copiar os valores da interface inserindo para os parâmetros passados para o `ProductsBLL` classe s `AddProduct` método. Como vimos no [examinando os eventos associados inserindo, atualizando e excluindo](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) tutorial, isso pode ser feito por meio de s ObjectDataSource `Inserting` eventos. No `Inserting` evento precisamos referenciar programaticamente os controles do `Products` rodapé do GridView s linha e atribuir seus valores para o `e.InputParameters` coleção. Se o usuário omite um valor como deixar o `ReorderLevel` caixa de texto vazio é necessário especificar que o valor inserido no banco de dados deve ser `NULL`. Como o `AddProducts` método aceita tipos anuláveis para os campos de banco de dados permite valor nulo, basta usar um tipo anulável e defina seu valor como `Nothing` no caso em que a entrada do usuário for omitida.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

Com o `Inserting` manipulador de eventos concluído, podem ser adicionados novos registros de `Products` tabela de banco de dados por meio da linha de rodapé GridView s. Vá em frente e tente adicionar vários novos produtos.

## <a name="enhancing-and-customizing-the-add-operation"></a>Aprimorar e personalizar a operação de adição

No momento, clicando no botão Adicionar adiciona um novo registro à tabela de banco de dados mas não fornece qualquer tipo de comentários visuais que o registro tenha sido adicionado com êxito. Idealmente, um rótulo Web controle ou cliente alerta caixa deve informar ao usuário que sua inserção foi concluída com êxito. Posso deixar isso como um exercício para o leitor.

O GridView usada neste tutorial não se aplica a uma ordem de classificação para os produtos listados, nem permite que o usuário final classificar os dados. Consequentemente, os registros são ordenados como estão no banco de dados, o campo de chave primária. Como cada novo registro tem um `ProductID` valor maior que o último deles, sempre que um novo produto é adicionado, ele é incluído no até o final da grade. Portanto, convém enviar automaticamente o usuário para a última página do GridView depois de adicionar um novo registro. Isso pode ser feito adicionando a seguinte linha de código após a chamada a `ProductsDataSource.Insert()` no `RowCommand` manipulador de eventos para indicar que o usuário precisa ser enviada para a última página após a associação dos dados a GridView:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` é uma variável booleana de nível de página que inicialmente é atribuída um valor de `False`. Em GridView s `DataBound` manipulador de eventos, se `SendUserToLastPage` for false, o `PageIndex` propriedade é atualizada para enviar o usuário para a última página.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

O motivo pelo qual o `PageIndex` propriedade é definida no `DataBound` manipulador de eventos (em vez do `RowCommand` manipulador de eventos) ocorre porque quando o `RowCommand` manipulador de eventos é acionado, var para adicionar o novo registro para o `Products` tabela de banco de dados. Portanto, o `RowCommand` o último índice de página de manipulador de eventos (`PageCount - 1`) representa o último índice de página *antes de* foi adicionado o novo produto. Para a maioria dos produtos que estão sendo adicionados, o último índice de página é o mesmo depois de adicionar o novo produto. Mas quando o produto adicional resulta em um novo último índice de página, se podemos atualizar incorretamente o `PageIndex` no `RowCommand` manipulador de eventos, em seguida, podemos serão levados para a segunda à última página (o último índice de página antes de adicionar o novo produto) em vez de última página nova i ndice. Como o `DataBound` manipulador de eventos é acionado depois que foi adicionado ao novo produto e os dados religados à grade, definindo o `PageIndex` propriedade existe sabemos esteja tirando o último índice de página corretos.

Finalmente, o GridView usada neste tutorial é muito grande devido ao número de campos que devem ser coletados para adicionar um novo produto. Devido a essa largura, um layout vertical do DetailsView s pode ser preferencial. O GridView s largura geral poderia ser reduzida com a coleta de entradas menos. Talvez, don precisa coletar o `UnitsOnOrder`, `UnitsInStock`, e `ReorderLevel` campos ao adicionar um novo produto, caso em que esses campos podem ser removidos do GridView.

Para ajustar os dados coletados, podemos usar uma das duas abordagens:

- Continuar a usar o `AddProduct` método que espera valores para o `UnitsOnOrder`, `UnitsInStock`, e `ReorderLevel` campos. No `Inserting` manipulador de eventos, forneça embutidos, valores usar para essas entradas que foram removidas da interface inserindo padrão.
- Criar uma novo sobrecarga do `AddProduct` método o `ProductsBLL` classe que não aceita entradas para o `UnitsOnOrder`, `UnitsInStock`, e `ReorderLevel` campos. Em seguida, na página do ASP.NET, configure ObjectDataSource para usar essa sobrecarga de novo.

Qualquer opção funcionará igualmente bem. Após tutoriais usamos a última opção, criando várias sobrecargas para o `ProductsBLL` classe s `UpdateProduct` método.

## <a name="summary"></a>Resumo

GridView não possui os recursos internos de inserção encontrados no DetailsView e FormView, mas com um pouco de esforço, uma interface de inserção pode ser adicionada à linha de rodapé. Para exibir a linha de rodapé em um controle GridView simplesmente definir seu `ShowFooter` propriedade `True`. O conteúdo da linha de rodapé de página pode ser personalizado para cada campo, convertendo o campo em um TemplateField e adicionando a inserção de interface para o `FooterTemplate`. Como vimos neste tutorial, o `FooterTemplate` pode conter botões, caixas de texto, DropDownLists, caixas de seleção, os controles de fonte de dados para popular controladas por dados controles da Web (como DropDownLists) e controles de validação. Junto com os controles para coletar a entrada do usuário s, é necessário um botão Adicionar, LinkButton ou ImageButton.

Quando o botão Adicionar é clicado, o s ObjectDataSource `Insert()` método é chamado para iniciar o fluxo de trabalho de inserção. ObjectDataSource chamará o método de inserção configurado (o `ProductsBLL` classe s `AddProduct` método neste tutorial). É necessário copiar os valores de GridView s inserindo a interface para o s ObjectDataSource `InsertParameters` coleção antes do método de inserção que está sendo invocado. Isso pode ser feito consultando programaticamente os controles da Web interface inserindo em ObjectDataSource s `Inserting` manipulador de eventos.

Este tutorial conclui nossa aparência técnicas para melhorar a aparência de s GridView. O próximo conjunto de tutoriais examinará os dados de controles da Web e de como trabalhar com dados binários, como imagens, PDFs, documentos do Word e assim por diante.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Bernadette Leigh. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-a-gridview-column-of-checkboxes-vb.md)
