---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Adicionando controles de validação para a edição e inserção de Interfaces (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como é fácil adicionar controles de validação ao EditItemTemplate e InsertItemTemplate de um controle da Web, de dados para fornecer uma mais...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: a2fc00426022513c6e2adc49b0df30f943403302
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366223"
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Adicionando controles de validação para a edição e inserção de Interfaces (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) ou [baixar PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> Neste tutorial, veremos como é fácil adicionar controles de validação ao EditItemTemplate e InsertItemTemplate de um controle da Web, de dados para fornecer uma interface de usuário mais segura.


## <a name="introduction"></a>Introdução

Os controles GridView e DetailsView nos exemplos exploramos nos últimos três tutoriais tem sido compostos de BoundFields e CheckBoxFields (os tipos de campo adicionados automaticamente pelo Visual Studio ao associar um GridView ou DetailsView a uma fonte de dados controle por meio da marca inteligente). Ao editar uma linha em um GridView ou DetailsView, esses BoundFields que não são somente leitura são convertidos em caixas de texto, do qual o usuário final pode modificar os dados existentes. Da mesma forma, quando inserir um novo registro em um controle DetailsView, esses BoundFields cuja `InsertVisible` estiver definida como `True` (o padrão) são renderizados como caixas de texto vazias no qual o usuário pode fornecer valores de campo do registro de novo. Da mesma forma, CheckBoxFields, que estão desabilitados na interface do padrão, somente leitura, são convertidos em caixas de seleção habilitadas na edição e inserção de interfaces.

Embora o padrão de edição e inserção de interfaces para o BoundField e CheckBoxField pode ser útil, a interface não tem qualquer tipo de validação. Se um usuário cometer um erro de entrada de dados - como omitir as `ProductName` campo ou a inserção de um valor inválido para `UnitsInStock` (por exemplo, -50) uma exceção será gerada de dentro a fundo a arquitetura do aplicativo. Embora essa exceção pode ser tratada normalmente como demonstrado na [tutorial anterior](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), o ideal é que a edição ou a inserção de interface do usuário inclui controles de validação para impedir que um usuário inserir esses dados inválidos no primeiro lugar.

Para fornecer uma edição personalizada ou uma interface de inserção, precisamos substituir o BoundField ou CheckBoxField por um TemplateField. TemplateFields, que eram o tópico de discussão na [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) e [Usando TemplateFields no controle DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) tutoriais, pode consistir em várias modelos definindo separam interfaces para os estados de linha diferente. O TemplateField `ItemTemplate` é usado para ao renderizar campos somente leitura ou linhas em controles DetailsView ou GridView, enquanto a `EditItemTemplate` e `InsertItemTemplate` indicam as interfaces a ser usado para a edição e inserção modos, respectivamente.

Neste tutorial, veremos como é fácil adicionar controles de validação para o TemplateField `EditItemTemplate` e `InsertItemTemplate` para fornecer uma interface de usuário mais segura. Especificamente, este tutorial usa o exemplo criado na [examinando os eventos associados inserindo, atualizando e excluindo](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) tutorial e amplia a edição e inserção interfaces para incluir validação apropriado.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Etapa 1: Replicar o exemplo de[examinando os eventos associados com inserindo, atualizando e excluindo](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

No [examinando os eventos associados inserindo, atualizando e excluindo](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) tutorial, criamos uma página que lista os nomes e os preços dos produtos em um GridView editável. Além disso, a página incluído um DetailsView cujos `DefaultMode` propriedade foi definida como `Insert`, renderização, assim, sempre no modo de inserção. Desse DetailsView, o usuário poderia insira o nome e o preço de um novo produto, clique em Inserir e que ele seja adicionado ao sistema (veja a Figura 1).


[![O exemplo anterior permite aos usuários adicionar novos produtos e editar as existentes](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Figura 1**: O exemplo permite que usuários anteriores para adicionar novos produtos e editar os existentes ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


Nosso objetivo para este tutorial é ampliar o DetailsView e GridView para fornecer controles de validação. Em particular, nossa lógica de validação será:

- Exigir que o nome ser fornecido ao inserir ou editar um produto
- Exigir que o preço ser fornecido ao inserir um registro. ao editar um registro, podemos ainda exigirá um preço, mas usará a lógica programática no GridView `RowUpdating` já está presente no tutorial anterior de manipulador de eventos
- Certifique-se de que o valor inserido para o preço é um formato de moeda válidos

Antes que possamos observar aumentando o exemplo anterior para incluir a validação, primeiro precisamos replicar o exemplo a partir de `DataModificationEvents.aspx` página para a página para este tutorial, `UIValidation.aspx`. Para fazer isso, precisamos copiar ambos o `DataModificationEvents.aspx` marcação declarativa da página e seu código-fonte. Primeiro copiar sobre a marcação declarativa, executando as seguintes etapas:

1. Abra o `DataModificationEvents.aspx` página no Visual Studio
2. Vá para a marcação da página declarativa (clique no botão Source na parte inferior da página)
3. Copie o texto dentro de `<asp:Content>` e `</asp:Content>` marcas (linhas 3 a 44), como mostrado na Figura 2.


[![Copie o texto dentro de &lt;asp: Content&gt; controle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Figura 2**: Copie o texto dentro de `<asp:Content>` controle ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. Abra o `UIValidation.aspx` página
2. Vá para a marcação da página declarativa
3. Cole o texto dentro de `<asp:Content>` controle.

Para copiar o código-fonte, abra o `DataModificationEvents.aspx.vb` da página e copiar apenas o texto *dentro* o `EditInsertDelete_DataModificationEvents` classe. Copie os manipuladores de eventos de três (`Page_Load`, `GridView1_RowUpdating`, e `ObjectDataSource1_Inserting`), mas fazer **não** copiar a declaração de classe. Colar o texto copiado *dentro de* as `EditInsertDelete_UIValidation` classe `UIValidation.aspx.vb`.

Depois de mover sobre o conteúdo e o código de `DataModificationEvents.aspx` para `UIValidation.aspx`, reserve um tempo para testar seu progresso em um navegador. Você deve ver a mesma saída e experimentar a mesma funcionalidade em cada uma dessas duas páginas (consulte a Figura 1 para uma captura de tela de `DataModificationEvents.aspx` em ação).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Etapa 2: Convertendo o BoundFields em TemplateFields

Para adicionar controles de validação às interfaces de edição e inserção, os BoundFields usados pelos controles DetailsView e GridView precisa ser convertido em TemplateFields. Para fazer isso, clique nos links Edit Columns e editar campos em um GridView e do DetailsView de marcas inteligentes, respectivamente. Lá, selecione cada uma da BoundFields e clique no link "Converter este campo em um TemplateField".


[![Converter cada um do GridView e do DetailsView BoundFields em TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Figura 3**: converter cada um do GridView e do DetailsView BoundFields em TemplateFields ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


Converter um BoundField em um TemplateField por meio da caixa de diálogo campos gera um TemplateField que exibe as mesmas interfaces somente leitura, editando e inserindo o BoundField em si. A marcação a seguir mostra a sintaxe declarativa para o `ProductName` campo DetailsView após ele ter sido convertido em um TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Observe que essa TemplateField tinha três modelos criados automaticamente `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate`. O `ItemTemplate` exibe um valor de campo de dados (`ProductName`) usando um controle de rótulo Web, enquanto o `EditItemTemplate` e `InsertItemTemplate` apresentar o valor do campo de dados em um controle de TextBox Web que associa o campo de dados com a caixa de texto `Text` propriedade usando vinculação de dados bidirecional. Como estamos apenas usando DetailsView nesta página para inserir, você pode remover o `ItemTemplate` e `EditItemTemplate` de TemplateFields os dois, embora não haja nenhum problema em deixá-los.

Como o GridView não oferece suporte o internos de inserção de recursos de DetailsView, convertendo o GridView `ProductName` campo em um TemplateField resulta em apenas um `ItemTemplate` e `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Ao clicar em "Converter este campo em um TemplateField", o Visual Studio criou um TemplateField cujos modelos imitam a interface do usuário do BoundField convertido. Você pode verificar isso ao visitar esta página por meio de um navegador. Você descobrirá que a aparência e comportamento do TemplateFields é idêntica à experiência, quando BoundFields foram usadas em vez disso.

> [!NOTE]
> Fique à vontade personalizar as interfaces de edição em modelos, conforme necessário. Por exemplo, podemos querer ter a caixa de texto na `UnitPrice` TemplateFields renderizado como uma caixa de texto menor do que o `ProductName` caixa de texto. Para fazer isso, você pode definir a caixa de texto `Columns` propriedade para um valor apropriado ou forneça uma largura absoluta por meio de `Width` propriedade. No próximo tutorial, veremos como personalizar completamente a interface de edição, substituindo a caixa de texto com uma controle da Web de entrada de dados alternativo.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Etapa 3: Adicionando controles de validação para o GridView`EditItemTemplate` s

Ao construir formulários de entrada de dados, é importante que os usuários insiram os campos necessários e que todas as entradas fornecidas são legais, formatado corretamente os valores. Para ajudar a garantir que as entradas do usuário sejam válidas, o ASP.NET fornece cinco controles de validação internas que são projetados para ser usado para validar o valor de um único controle de entrada:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garante que um valor foi fornecido
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valida um valor contra outro valor de controle de Web ou um valor constante ou garante que o formato do valor é válido para um tipo de dados especificado
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garante que um valor está dentro de um intervalo de valores
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valida um valor em relação a um [expressão regular](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valida um valor em relação a um método personalizado definido pelo usuário

Para obter mais informações sobre esses controles de cinco, confira a [seção controles de validação](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) da [tutoriais de início rápido do ASP.NET](https://asp.net/QuickStart/aspnet/).

Para nosso tutorial, precisará usar um RequiredFieldValidator do GridView e DetailsView `ProductName` TemplateFields e um RequiredFieldValidator v ovládacím prvku DetailsView `UnitPrice` TemplateField. Além disso, precisamos adicionar um CompareValidator para ambos os controles `UnitPrice` TemplateFields que garante que o preço inserido tem um valor maior que ou igual a 0 e é apresentado em um formato de moeda válidos.

> [!NOTE]
> Embora o ASP.NET 1.x tivesse esses mesmos controles de validação de cinco, ASP.NET 2.0 adicionou uma série de aprimoramentos, principal sendo que o script do lado do cliente dois dar suporte a navegadores diferentes do Internet Explorer e a capacidade de controles de validação de partição em uma página em grupos de validação. Para obter mais informações sobre os novos recursos de controle de validação no 2.0, consulte [dissecando os controles de validação no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Vamos começar pela adição de controles de validação necessárias para o `EditItemTemplate` s em TemplateFields do GridView. Para fazer isso, clique no link Editar modelos na marca inteligente do GridView para abrir a interface de edição de modelo. A partir daqui, você pode selecionar qual modelo deseja editar na lista suspensa. Como queremos ampliar a interface de edição, precisamos adicionar controles de validação para o `ProductName` e `UnitPrice`do `EditItemTemplate` s.


[![É necessário estender o ProductName e EditItemTemplates do UnitPrice](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Figura 4**: É necessário estender o `ProductName` e `UnitPrice`do `EditItemTemplate` s ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


No `ProductName` `EditItemTemplate`, adicionar um RequiredFieldValidator arrastando-o na caixa de ferramentas para a interface de edição de modelo, colocando após a caixa de texto.


[![Adicionar um RequiredFieldValidator ao ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Figura 5**: adicionar um RequiredFieldValidator para o `ProductName` `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


Todos os controles de validação funcionam, validando a entrada de um único controle da Web do ASP.NET. Portanto, precisamos indicar que o RequiredFieldValidator que acabamos de adicionar deve validar em relação a caixa de texto a `EditItemTemplate`; isso é feito definindo o controle de validação [propriedade ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) para o `ID` do controle da Web apropriado. Atualmente, a caixa de texto tem bastante nondescript `ID` de `TextBox1`, mas vamos alterá-lo para algo mais adequado. Clique na caixa de texto no modelo e em seguida, na janela Propriedades, altere o `ID` partir `TextBox1` para `EditProductName`.


[![Alterar a ID da caixa de texto para EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Figura 6**: alterar a caixa de texto `ID` à `EditProductName` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


Em seguida, defina o RequiredFieldValidator `ControlToValidate` propriedade para `EditProductName`. Por fim, defina as [propriedade ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) "Você deve fornecer o nome do produto" e o [propriedade Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) para "\*". O `Text` valor da propriedade, se fornecido, é o texto exibido pelo controle de validação se a validação falhar. O `ErrorMessage` valor da propriedade, que é necessário, é usado pelo controle ValidationSummary; se o `Text` o valor da propriedade for omitido, o `ErrorMessage` valor da propriedade também é o texto exibido pelo controle de validação em uma entrada inválida.

Depois de definir essas três propriedades do RequiredFieldValidator, sua tela deve ser semelhante a Figura 7.


[![Defina o RequiredFieldValidator ControlToValidate ErrorMessage e propriedades de texto](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Figura 7**: defina o RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, e `Text` propriedades ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


Com o RequiredFieldValidator adicionado para o `ProductName` `EditItemTemplate`, tudo o que resta fazer é adicionar a validação necessária para o `UnitPrice` `EditItemTemplate`. Uma vez que decidimos que, para essa página, o `UnitPrice` é opcional quando a edição de um registro, não precisamos adicionar um RequiredFieldValidator. , No entanto, precisamos adicionar um CompareValidator para garantir que o `UnitPrice`, se fornecido, está formatado corretamente como uma moeda e é maior que ou igual a 0.

Antes de adicionarmos CompareValidator para o `UnitPrice` `EditItemTemplate`, vamos primeiro alterar a ID do controle da TextBox Web de `TextBox2` para `EditUnitPrice`. Depois de fazer essa alteração, adicione CompareValidator, definindo sua `ControlToValidate` propriedade para `EditUnitPrice`, sua `ErrorMessage` propriedade como "o preço deve ser maior que ou igual a zero e não pode incluir o símbolo de moeda" e seu `Text` propriedade para "\*".

Para indicar que o `UnitPrice` valor deve ser maior que ou igual a 0, defina o CompareValidator [propriedade Operator](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) à `GreaterThanEqual`, sua [ValueToCompare propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) "0" e seu [ A propriedade Type](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) para `Currency`. A seguir mostra a sintaxe declarativa do `UnitPrice` do TemplateField `EditItemTemplate` depois que essas alterações foram feitas:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Depois de fazer essas alterações, abra a página em um navegador. Se você tentar se omitir o nome ou insira um valor de preço inválido ao editar um produto, um asterisco é exibido ao lado da caixa de texto. Como mostra a Figura 8, um valor de preço que inclui o símbolo de moeda como US $19,95 é considerado inválido. O CompareValidator `Currency` `Type` permite separadores de dígitos (como vírgulas ou períodos, dependendo das configurações de cultura) e um sinal de mais à esquerda ou sinal de subtração, mas faz *não* permitir um símbolo de moeda. Esse comportamento pode perplex usuários conforme a interface de edição no momento, renderiza o `UnitPrice` usando o formato de moeda.

> [!NOTE]
> Lembre-se de que, no *eventos associado inserindo, atualizando e excluindo* tutorial, definimos o BoundField `DataFormatString` propriedade `{0:c}` para formatá-la como uma moeda. Além disso, definimos a `ApplyFormatInEditMode` propriedade como true, fazendo com que o GridView de edição de interface para formatar o `UnitPrice` como uma moeda. Ao converter o BoundField em um TemplateField, o Visual Studio observado essas configurações e formatado da caixa de texto `Text` a propriedade como uma moeda usando a sintaxe de associação de dados `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Um asterisco é exibido ao lado de caixas de texto com uma entrada inválida](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Figura 8**: um asterisco é exibido próximo para as caixas de texto com uma entrada inválida ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


Embora a funciona de validação como-está, o usuário deve remover manualmente o símbolo de moeda ao editar um registro que não é aceitável. Para corrigir isso, temos três opções:

1. Configurar o `EditItemTemplate` para que o `UnitPrice` valor não está formatado como uma moeda.
2. Permitir que o usuário insira um símbolo de moeda, removendo CompareValidator e substituí-la por um RegularExpressionValidator que verifica um valor de moeda corretamente formatado corretamente. O problema aqui é que a expressão regular para validar um valor de moeda não é muito e exigiria escrevendo código, se quiséssemos incorporar as configurações de cultura.
3. Remover completamente o controle de validação e contam com lógica de validação do lado do servidor em que o GridView `RowUpdating` manipulador de eventos.

Vamos com opção #1 para este exercício. Atualmente, o `UnitPrice` é formatado como uma moeda devido à expressão de associação de dados para a caixa de texto a `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Altere a declaração de associação para `Bind("UnitPrice", "{0:n2}")`, que formata o resultado como um número com dois dígitos de precisão. Isso pode ser feito diretamente por meio da sintaxe declarativa ou clicando no link Editar DataBindings do `EditUnitPrice` caixa de texto de `UnitPrice` do TemplateField `EditItemTemplate` (consulte as figuras 9 e 10).


[![Clique no link de Editar DataBindings da caixa de texto](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Figura 9**: clique no link de Editar DataBindings da caixa de texto ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![Especificar o especificador de formato na instrução Bind](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Figura 10**: especifique o especificador de formato em de `Bind` instrução ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


Com essa alteração, o preço formatado na interface de edição inclui vírgula como separador de grupo e um ponto como separador decimal, mas deixa desativar o símbolo de moeda.

> [!NOTE]
> O `UnitPrice` `EditItemTemplate` não inclui um RequiredFieldValidator, permitindo que o postback para acontecer e a lógica de atualização para iniciar. No entanto, o `RowUpdating` copiado do manipulador de eventos a *examinando os eventos associados inserindo, atualizando e excluindo* tutorial inclui uma verificação programática que garante que o `UnitPrice` é fornecido. Fique à vontade remover essa lógica, deixá-lo em como-está, ou adicionar um RequiredFieldValidator para o `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Etapa 4: Resumo de problemas de entrada de dados

Além dos controles de validação de cinco, o ASP.NET inclui o [controle ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), que exibe o `ErrorMessage` s desses controles de validação que detectou dados inválidos. Esses dados de resumo podem ser exibidos como texto na página da web ou por meio de uma caixa de mensagem modal, no lado do cliente. Vamos melhorar este tutorial para incluir uma caixa de mensagem do lado do cliente resumindo quaisquer problemas de validação.

Para fazer isso, arraste um controle ValidationSummary da caixa de ferramentas para o Designer. O local do controle de validação não importa, já que vamos configurá-lo para exibir somente o resumo como uma caixa de mensagem. Depois de adicionar o controle, defina suas [propriedade ShowSummary](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) à `False` e sua [propriedade ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) para `True`. Com esse acréscimo, erros de validação são resumidos em uma caixa de mensagem do lado do cliente.


[![Os erros de validação são resumidos em uma caixa de mensagem do lado do cliente](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Figura 11**: os erros de validação são resumidos em uma caixa de mensagem do lado do cliente ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Etapa 5: Adicionando controles de validação a DetailsView`InsertItemTemplate`

Tudo o que resta para este tutorial é adicionar os controles de validação à interface de inserção de DetailsView. O processo de adicionar controles de validação para modelos de DetailsView é idêntico ao que examinado na etapa 3; Portanto, vamos vai breeze por meio da tarefa nesta etapa. Como fizemos com o GridView `EditItemTemplate` s, eu recomendo que você renomear o `ID` s das caixas de texto do nondescript `TextBox1` e `TextBox2` para `InsertProductName` e `InsertUnitPrice`.

Adicionar um RequiredFieldValidator para o `ProductName` `InsertItemTemplate`. Defina a `ControlToValidate` para o `ID` da caixa de texto no modelo, seu `Text` propriedade para "\*" e seu `ErrorMessage` propriedade como "Você deve fornecer o nome do produto".

Uma vez que o `UnitPrice` é necessário para esta página ao adicionar um novo registro, adicione um RequiredFieldValidator para o `UnitPrice` `InsertItemTemplate`, definindo seu `ControlToValidate`, `Text`, e `ErrorMessage` propriedades adequadamente. Por fim, adicione um CompareValidator para o `UnitPrice` `InsertItemTemplate` Além disso, configurando seus `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, e `ValueToCompare` propriedades exatamente como fizemos com o `UnitPrice`do CompareValidator no GridView `EditItemTemplate`.

Depois de adicionar esses controles de validação, um novo produto não pode ser adicionado ao sistema se o seu nome não for fornecido ou se seu preço é um número negativo ou ilegalmente formatado.


[![Lógica de validação foi adicionada à Interface de inserção de DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Figura 12**: lógica de validação foi adicionada à Interface de inserção de DetailsView ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Etapa 6: Particionar os controles de validação em grupos de validação

Nossa página consiste em dois conjuntos logicamente distintos de controles de validação: aquelas que correspondem a GridView de edição de interface e aquelas que correspondem a DetailsView da inserção de interface. Por padrão, quando ocorre um postback *todos os* controles de validação na página são verificados. No entanto, ao editar um registro não queremos que os controles de validação da interface de DetailsView inserindo para validar. Figura 13 ilustra nosso dilema atual quando um usuário está editando um produto com valores perfeitamente legais, clicando em atualização faz com que um erro de validação porque os valores de nome e o preço na interface de inserção são em branco.


[![Atualizar um produto faz com que controles de validação de inserção da Interface a ser acionada](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Figura 13**: atualizar um produto faz com que os controles de validação da Interface inserindo a ser acionada ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


Os controles de validação no ASP.NET 2.0 podem ser particionados em grupos de validação por meio de seus `ValidationGroup` propriedade. Para associar um conjunto de controles de validação em um grupo, basta definir seus `ValidationGroup` propriedade com o mesmo valor. Para nosso tutorial, defina as `ValidationGroup` propriedades de controles de validação no TemplateFields do GridView para `EditValidationControls` e o `ValidationGroup` propriedades de TemplateFields de DetailsView para `InsertValidationControls`. Essas alterações podem ser feitas diretamente na marcação declarativa ou por meio da janela de propriedades ao usar o Designer editar a interface do modelo.

Além de validação de controles, o botão e controles relacionados ao botão no ASP.NET 2.0 também incluem um `ValidationGroup` propriedade. Os validadores de um grupo de validação são verificados quanto à validade somente quando um postback é induzido por um botão que tem o mesmo `ValidationGroup` configuração da propriedade. Por exemplo, na ordem do botão de inserção de DetailsView disparar a `InsertValidationControls` grupo de validação, precisamos definir o CommandField `ValidationGroup` propriedade `InsertValidationControls` (veja a Figura 14). Além disso, defina o GridView do CommandField `ValidationGroup` propriedade para `EditValidationControls`.


[![Propriedade de ValidationGroup do CommandField para InsertValidationControls de conjunto DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Figura 14**: defina o DetailsView do CommandField `ValidationGroup` propriedade a ser `InsertValidationControls` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


Após essas alterações, do GridView e DetailsView TemplateFields e CommandFields deve ser semelhante ao seguinte:

O DetailsView TemplateFields e CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

O GridView CommandField e TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

Neste momento os controles de validação específicos de edição são acionados somente quando o botão de atualização do GridView é clicado e a incêndio de controles de validação específica de inserção somente quando o botão de inserção de DetailsView é clicado, resolvendo o problema destacado pela Figura 13. No entanto, com essa alteração nosso controle ValidationSummary não exibe mais ao inserir dados inválidos. O controle ValidationSummary também contém um `ValidationGroup` propriedade e só mostra informações de resumo para esses controles de validação em seu grupo de validação. Portanto, precisamos ter dois controles de validação nesta página, uma para o `InsertValidationControls` grupo de validação e outra para `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Com essa adição nosso tutorial está completo!

## <a name="summary"></a>Resumo

Enquanto BoundFields pode fornecer tanto uma interface de edição e inserção, a interface não é personalizável. Normalmente, queremos adicionar controles de validação para a edição e inserção de interface para garantir que o usuário insere entradas necessárias em um formato válido. Para fazer isso, devemos converter o BoundFields em TemplateFields e adicione os controles de validação para o modelo apropriado (s). Neste tutorial, estendemos o exemplo a partir de *examinando os eventos associados inserindo, atualizando e excluindo* tutorial adicionando controles de validação para ambos os DetailsView da inserção de interface e o GridView interface de edição. Além disso, vimos como exibir informações de resumo de validação usando o controle ValidationSummary e como particionar os controles de validação na página em grupos distintos de validação.

Como vimos neste tutorial, TemplateFields permitem que as interfaces de edição e inserção ser ampliada para incluir controles de validação. TemplateFields também pode ser estendido para incluir controles de Web de entrada adicionais, habilitando a caixa de texto a ser substituído por um controle de Web mais adequado. Em nosso próximo tutorial, veremos como substituir o controle de caixa de texto com um controle DropDownList associado a dados, que é ideal quando você editar uma chave estrangeira (como `CategoryID` ou `SupplierID` no `Products` tabela).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Liz Shulok e Zack Jones. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [Próximo](customizing-the-data-modification-interface-vb.md)
