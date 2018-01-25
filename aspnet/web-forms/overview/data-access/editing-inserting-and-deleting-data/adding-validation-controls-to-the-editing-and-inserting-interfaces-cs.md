---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
title: "Adicionando controles de validação para a edição e a inserção de Interfaces (c#) | Microsoft Docs"
author: rick-anderson
description: "Neste tutorial, veremos como é fácil adicionar controles de validação ao EditItemTemplate e InsertItemTemplate de um controle da Web, de dados para fornecer uma mais..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 2086cb1a-ab78-49ae-9c0b-03891c69776a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
msc.type: authoredcontent
ms.openlocfilehash: b8b05705629b5e8a9acfc5d23517ef1b3cfa7cd6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-c"></a>Adicionando controles de validação para a edição e a inserção de Interfaces (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_CS.exe) ou [baixar PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/datatutorial19cs1.pdf)

> Neste tutorial, veremos como é fácil adicionar controles de validação ao EditItemTemplate e InsertItemTemplate de um controle da Web, de dados para fornecer uma interface de usuário mais segura.


## <a name="introduction"></a>Introdução

Os controles GridView e DetailsView nos exemplos é depois de explorar nos últimos três tutoriais tem sido compostas de BoundFields e CheckBoxFields (os tipos de campo adicionados automaticamente pelo Visual Studio ao associar um GridView ou DetailsView a uma fonte de dados controlar por meio de marca inteligente). Ao editar uma linha em GridView ou DetailsView, esses BoundFields que não são somente leitura são convertidos em caixas de texto, do qual o usuário final pode modificar os dados existentes. Da mesma forma, quando inserir um novo registro em um controle DetailsView, essas BoundFields cujo `InsertVisible` está definida como `true` (o padrão) são renderizados como caixas de texto vazias, no qual o usuário pode fornecer valores de campos do novo registro. Da mesma forma, CheckBoxFields, que estão desabilitados na interface padrão, somente leitura, são convertidos em caixas de seleção habilitadas na edição e inserção de interfaces.

Enquanto o padrão de edição e inserção de interfaces para o BoundField e CheckBoxField pode ser útil, a interface não tem qualquer tipo de validação. Se um usuário fizer um erro de entrada de dados - como omitir o `ProductName` campo ou inserir um valor inválido para `UnitsInStock` (por exemplo, -50) uma exceção será gerada de dentro de fundo da arquitetura do aplicativo. Embora essa exceção pode ser tratada normalmente como demonstrado no [tutorial anterior](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md), idealmente a edição ou a inserção de interface do usuário inclui controles de validação para impedir que um usuário inserir esses dados inválidos no primeiro lugar.

Para fornecer uma edição personalizada ou uma interface inserindo, precisamos substitua o BoundField ou CheckBoxField TemplateField. TemplateFields, que eram o tópico de discussão no [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) e [Usando TemplateFields no controle DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-cs.md) tutoriais, pode consistir em vários modelos de definição de separam as interfaces para estados de linha diferente. O TemplateField `ItemTemplate` é usado para ao renderizar campos somente leitura ou linhas em controles DetailsView ou GridView, enquanto o `EditItemTemplate` e `InsertItemTemplate` indicam as interfaces a ser usado para a edição e inserção modos, respectivamente.

Este tutorial veremos como é fácil adicionar controles de validação para o TemplateField `EditItemTemplate` e `InsertItemTemplate` para fornecer uma interface de usuário mais segura. Especificamente, este tutorial usa o exemplo criado na [examinando os eventos associados inserindo, atualizando e excluindo](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) tutorial e aumenta a edição e a inserção de todas as interfaces para incluir validação apropriadas.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-csmd"></a>Etapa 1: Replicando o exemplo de[examinando os eventos associados inserindo, atualizando e excluindo](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)

No [examinando os eventos associados inserindo, atualizando e excluindo](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) tutorial criamos uma página que lista os nomes e preços de produtos em um GridView editável. Além disso, a página incluído um DetailsView cujo `DefaultMode` propriedade foi definida como `Insert`, assim, sempre renderização no modo de inserção. Dessa DetailsView o usuário pode inserir o nome e o preço de um novo produto, clique Insert e que ele seja adicionado ao sistema (consulte a Figura 1).


[![O exemplo anterior permite aos usuários adicionar novos produtos e editar as existentes](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image1.png)

**Figura 1**: O anterior exemplo permite que os usuários para adicionar novos produtos e editar as existentes ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image3.png))


Nosso objetivo para este tutorial é aumentar a DetailsView e GridView para fornecer controles de validação. Em particular, nossa lógica de validação será:

- Exigir que o nome ser fornecido ao inserir ou editar um produto
- Exigir que o preço fornecido ao inserir um registro. ao editar um registro, ainda será necessário um preço mas usará a lógica de programação do GridView `RowUpdating` já presente do tutorial anterior do manipulador de eventos
- Certifique-se de que o valor digitado para o preço é um formato de moeda válido

Antes, podemos ver aumentando o exemplo anterior para incluir validação, precisamos primeiro replicar o exemplo da `DataModificationEvents.aspx` página para a página para este tutorial, `UIValidation.aspx`. Para fazer isso, precisamos copiar em ambos os `DataModificationEvents.aspx` declarativo da página e seu código-fonte. Primeiro copiar sobre a marcação declarativa executando as seguintes etapas:

1. Abra o `DataModificationEvents.aspx` página no Visual Studio
2. Vá para a marcação declarativa da página (clique no botão fonte na parte inferior da página)
3. Copie o texto dentro de `<asp:Content>` e `</asp:Content>` marcas (linhas de 3 a 44), como mostrado na Figura 2.


[![Copie o texto dentro de &lt;asp: Content&gt; controle](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image4.png)

**Figura 2**: copiar o texto dentro de `<asp:Content>` controle ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image6.png))


1. Abra o `UIValidation.aspx` página
2. Vá para a marcação declarativa da página
3. Cole o texto dentro de `<asp:Content>` controle.

Para copiar o código de origem, abra o `DataModificationEvents.aspx.cs` página e copiar apenas o texto *em* o `EditInsertDelete_DataModificationEvents` classe. Copie os manipuladores de eventos de três (`Page_Load`, `GridView1_RowUpdating`, e `ObjectDataSource1_Inserting`), mas **não** copiar a declaração de classe ou `using` instruções. Colar o texto copiado *em* o `EditInsertDelete_UIValidation` classe em `UIValidation.aspx.cs`.

Depois de mover sobre o conteúdo e o código de `DataModificationEvents.aspx` para `UIValidation.aspx`, reserve um tempo para testar seu progresso em um navegador. Você verá a mesma saída e ter a mesma funcionalidade em cada uma dessas duas páginas (consultar Figura 1 para uma captura de tela de `DataModificationEvents.aspx` em ação).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Etapa 2: Convertendo a BoundFields em TemplateFields

Para adicionar controles de validação para as interfaces de edição e inserção, os BoundFields usados pelos controles DetailsView e GridView precisa ser convertido em TemplateFields. Para fazer isso, clique nos links Editar colunas e editar campos em GridView e de DetailsView as marcas inteligentes, respectivamente. Nele, selecione cada o BoundFields e clique no link "Converter este campo em um TemplateField".


[![Converter cada BoundFields de DetailsView de GridView TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image7.png)

**Figura 3**: converter cada a GridView e de DetailsView BoundFields em TemplateFields ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image9.png))


Converter um BoundField em um TemplateField através da caixa de diálogo campos gera um TemplateField que exibe as mesmas interfaces de somente leitura, editando e inserindo como o BoundField em si. A marcação a seguir mostra a sintaxe declarativa para o `ProductName` campo a DetailsView após ele ter sido convertido em um TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample1.aspx)]

Observe que essa TemplateField tinha três modelos criados automaticamente `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate`. O `ItemTemplate` exibe um valor de campo de dados único (`ProductName`) usando um controle de rótulo Web, enquanto o `EditItemTemplate` e `InsertItemTemplate` apresentar o valor do campo de dados em um controle de caixa de texto Web que associa o campo de dados com a caixa de texto `Text` propriedade usando associação de dados bidirecional. Já que estamos apenas usando o DetailsView nesta página para inserir, você pode remover o `ItemTemplate` e `EditItemTemplate` de TemplateFields os dois, embora não haja nenhum problema em deixando-as.

Como o GridView não oferece suporte interno inserindo recursos de DetailsView, convertendo o GridView `ProductName` campo em um TemplateField resulta em apenas um `ItemTemplate` e `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample2.aspx)]

Clicando em "Converter este campo em um TemplateField", o Visual Studio criou um TemplateField cujos modelos imitam a interface do usuário da BoundField convertido. Você pode verificar isso visitando a página através de um navegador. Você descobrirá que a aparência e comportamento do TemplateFields é idêntica à experiência, quando BoundFields foram usados.

> [!NOTE]
> Fique à vontade para personalizar as interfaces de edição nos modelos conforme necessário. Por exemplo, podemos querer ter a caixa de texto no `UnitPrice` TemplateFields renderizado como uma caixa de texto menor que o `ProductName` caixa de texto. Para fazer isso, você pode definir a caixa de texto `Columns` propriedade para um valor apropriado ou forneça uma largura absoluta por meio de `Width` propriedade. O tutorial Avançar veremos como personalizar completamente a interface de edição, substituindo a caixa de texto com uma entrada de dados alternativo controle da Web.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Etapa 3: Adicionando controles de validação a GridView`EditItemTemplate` s

Quando estiver criando formulários de entrada de dados, é importante que os usuários insiram os campos necessários e todas as entradas fornecidas são valores válidos, formatado corretamente. Para ajudar a garantir que as entradas do usuário são válidas, o ASP.NET fornece cinco controles de validação interna que são projetados para ser usado para validar o valor de um controle de entrada único:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garante que um valor foi fornecido
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valida um valor contra outro valor de controle de Web ou um valor constante ou garante que o formato do valor é válido para um tipo de dados especificado
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garante que um valor está dentro do intervalo de valores
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valida um valor em relação a um [expressão regular](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valida um valor em relação a um método personalizado definido pelo usuário

Para obter mais informações sobre esses controles de cinco, confira o [seção controles de validação](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) do [ASP.NET tutoriais](https://asp.net/QuickStart/aspnet/).

Para nosso tutorial, precisamos usar um RequiredFieldValidator na DetailsView e do GridView `ProductName` TemplateFields e um RequiredFieldValidator de DetailsView `UnitPrice` TemplateField. Além disso, que precisamos adicionar um CompareValidator para ambos os controles `UnitPrice` TemplateFields que garante que o preço inserido tem um valor maior que ou igual a 0 e é apresentado em um formato de moeda válido.

> [!NOTE]
> Embora o ASP.NET 1. x tinha esses mesmos controles de validação de cinco, ASP.NET 2.0 adicionou uma série de melhorias, principal dois sendo script do lado do cliente oferecer suporte para navegadores que não seja o Internet Explorer e a capacidade de controles de validação de partição em uma página em grupos de validação. Para obter mais informações sobre os novos recursos de controle de validação no 2.0, consulte [dissecando os controles de validação no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Vamos começar adicionando os controles de validação necessárias para o `EditItemTemplate` s em TemplateFields do GridView. Para fazer isso, clique no link Editar modelos de marca inteligente do GridView para exibir a interface de edição de modelo. A partir daqui, você pode selecionar o modelo para editar na lista suspensa. Como queremos ampliar a interface de edição, é preciso adicionar controles de validação para o `ProductName` e `UnitPrice`do `EditItemTemplate` s.


[![É necessário estender o ProductName e EditItemTemplates do UnitPrice](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image10.png)

**Figura 4**: É necessário estender o `ProductName` e `UnitPrice`do `EditItemTemplate` s ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image12.png))


No `ProductName` `EditItemTemplate`, adicionar um RequiredFieldValidator arrastando-o na caixa de ferramentas para a interface de edição de modelo, colocando após a caixa de texto.


[![Adicionar um controle RequiredFieldValidator para o ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image13.png)

**Figura 5**: adicionar um RequiredFieldValidator para o `ProductName` `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image15.png))


Todos os controles de validação funcionam, validando a entrada de um único controle da Web do ASP.NET. Portanto, é necessário indicar que o RequiredFieldValidator que acabamos de adicionar deve validar em relação a caixa de texto no `EditItemTemplate`; isso é feito definindo o controle de validação [propriedade ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) para o `ID` do controle da Web apropriado. A caixa de texto tem atualmente em vez disso, nondescript `ID` de `TextBox1`, mas vamos alterá-lo para algo mais apropriado. Clique na caixa de texto no modelo e, em seguida, na janela Propriedades, altere o `ID` de `TextBox1` para `EditProductName`.


[![Alterar a ID da caixa de texto para EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image16.png)

**Figura 6**: alterar a caixa de texto `ID` para `EditProductName` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image18.png))


Em seguida, defina o RequiredFieldValidator `ControlToValidate` propriedade `EditProductName`. Finalmente, defina o [propriedade ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) "Você deve fornecer o nome do produto" e o [propriedade Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) para "\*". O `Text` valor da propriedade, se fornecido, será o texto que é exibido pelo controle de validação se a validação falhar. O `ErrorMessage` valor da propriedade, que é necessário, é usado pelo controle ValidationSummary; se o `Text` o valor da propriedade for omitido, o `ErrorMessage` o valor da propriedade também é o texto exibido pelo controle de validação em uma entrada inválida.

Depois de definir essas três propriedades o RequiredFieldValidator, sua tela deve ser semelhante a Figura 7.


[![Definir o RequiredFieldValidator ControlToValidate ErrorMessage e propriedades de texto](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image19.png)

**Figura 7**: definir o RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, e `Text` propriedades ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image21.png))


Com o RequiredFieldValidator adicionado para o `ProductName` `EditItemTemplate`, todos os que permanece é adicionar a validação necessária para o `UnitPrice` `EditItemTemplate`. Desde que decidimos que, para essa página, o `UnitPrice` é opcional quando a edição de um registro, não precisamos adicionar um RequiredFieldValidator. , No entanto, precisamos adicionar um CompareValidator para garantir que o `UnitPrice`, se fornecido, está corretamente formatado como uma moeda e é maior que ou igual a 0.

Antes de adicionar o CompareValidator para o `UnitPrice` `EditItemTemplate`, vamos primeiro alterar a ID do controle TextBox Web de `TextBox2` para `EditUnitPrice`. Depois de fazer essa alteração, adicionar CompareValidator, definindo seu `ControlToValidate` propriedade `EditUnitPrice`, sua `ErrorMessage` propriedade como "o preço deve ser maior que ou igual a zero e não pode incluir o símbolo de moeda" e sua `Text` propriedade "\*".

Para indicar que o `UnitPrice` valor deve ser maior que ou igual a 0, defina o CompareValidator [propriedade Operator](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) para `GreaterThanEqual`, sua [ValueToCompare propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) "0" e seu [ A propriedade Type](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) para `Currency`. A seguir mostra a sintaxe declarativa de `UnitPrice` do TemplateField `EditItemTemplate` depois que essas alterações foram feitas:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample3.aspx)]

Depois de fazer essas alterações, abra a página em um navegador. Se você tentar omitir o nome ou insira um valor inválido durante a edição de um produto, um asterisco é exibido ao lado da caixa de texto. Como mostra a Figura 8, um valor que inclui o símbolo de moeda como $19,95 é considerado inválido. O CompareValidator `Currency` `Type` permite separadores de dígitos (como vírgulas ou pontos, dependendo das configurações de cultura) e um sinal de mais à esquerda ou sinal de subtração, mas *não* permite que um símbolo de moeda. Esse comportamento pode perplex usuários como a interface de edição atualmente renderiza o `UnitPrice` usando o formato de moeda.

> [!NOTE]
> Lembre-se de que o *eventos associados inserindo, atualizando e excluindo* tutorial definimos o BoundField `DataFormatString` propriedade `{0:c}` para formatá-la como uma moeda. Além disso, definimos o `ApplyFormatInEditMode` propriedade como true, fazendo com que o GridView de edição do interface para formatar o `UnitPrice` como uma moeda. Ao converter o BoundField em um TemplateField, Visual Studio observado essas configurações e formatados a caixa de texto `Text` a propriedade como uma moeda usando a sintaxe de associação de dados `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Um asterisco é exibido ao lado de caixas de texto com uma entrada inválida](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image22.png)

**Figura 8**: um asterisco é exibido próximo a caixas de texto com uma entrada inválida ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image24.png))


Durante a validação funcionam-é, o usuário deve remover manualmente o símbolo de moeda ao editar um registro, que não é aceitável. Para corrigir isso, temos três opções:

1. Configurar o `EditItemTemplate` para que o `UnitPrice` valor não está formatado como uma moeda.
2. Permitir que o usuário insira um símbolo de moeda removendo CompareValidator e substituí-lo com um RegularExpressionValidator que verifica um valor de moeda corretamente formatado corretamente. O problema aqui é que a expressão regular para validar um valor de moeda não é muito e exigiria escrevendo código para incorporar as configurações de cultura.
3. Remover o controle de validação completamente e confiar na lógica de validação do lado do servidor em que o GridView `RowUpdating` manipulador de eventos.

Vamos falar com a opção #1 para este exercício. Atualmente o `UnitPrice` é formatada como uma moeda devido à expressão de associação de dados para a caixa de texto no `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Altere a declaração de ligação `Bind("UnitPrice", "{0:n2}")`, que formata o resultado como um número com dois dígitos de precisão. Isso pode ser feito diretamente por meio da sintaxe declarativa ou clicando no link Editar DataBindings do `EditUnitPrice` TextBox no `UnitPrice` do TemplateField `EditItemTemplate` (consulte figuras 9 e 10).


[![Clique no link de Editar DataBindings da caixa de texto](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image25.png)

**Figura 9**: clique no link de Editar DataBindings da caixa de texto ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image27.png))


[![Especificar o especificador de formato na instrução de ligação](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image28.png)

**Figura 10**: especifique o especificador de formato no `Bind` instrução ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image30.png))


Com essa alteração, o preço formatado na interface de edição inclui vírgulas como separador de grupo e um ponto como o separador decimal, mas deixa desativar o símbolo de moeda.

> [!NOTE]
> O `UnitPrice` `EditItemTemplate` não inclui um RequiredFieldValidator, permitindo que o postback causar e a lógica de atualização para iniciar. No entanto, o `RowUpdating` manipulador de eventos copiado do *examinando os eventos associados inserindo, atualizando e excluindo* tutorial inclui uma verificação de programação que garante que o `UnitPrice` é fornecido. Fique à vontade para remover essa lógica, deixe-nos como-está ou adicionar um RequiredFieldValidator para o `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Etapa 4: Resumo de problemas de entrada de dados

Além dos controles de validação de cinco, ASP.NET inclui o [controle ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), que exibe o `ErrorMessage` s desses controles de validação que detectou dados inválidos. Estes resumos dados podem ser exibidos como texto na página da web ou por meio de uma caixa de mensagem modal, no lado do cliente. Vamos melhorar este tutorial para incluir uma messagebox cliente resumindo os problemas de validação.

Para fazer isso, arraste um controle ValidationSummary da caixa de ferramentas para o Designer. O local do controle de validação não importa, desde que vamos configurá-lo para exibir somente o resumo de como uma caixa de mensagem. Depois de adicionar o controle, defina seu [propriedade ShowSummary](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) para `false` e sua [propriedade ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) para `true`. Com essa adição, erros de validação são resumidos em uma caixa de mensagem do lado do cliente.


[![Os erros de validação são resumidos em uma caixa de mensagem do lado do cliente](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image31.png)

**Figura 11**: A erros de validação são resumidos em uma caixa de mensagem do lado do cliente ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Etapa 5: Adicionando controles de validação a DetailsView`InsertItemTemplate`

Tudo o que resta para este tutorial é adicionar os controles de validação a interface de inserção de DetailsView. O processo de adicionar controles de validação para modelos de DetailsView é idêntico ao examinado na etapa 3; Portanto, podemos será breeze por meio da tarefa nesta etapa. Como foi feito com o GridView `EditItemTemplate` s, recomendo que você renomear o `ID` s das caixas de texto do nondescript `TextBox1` e `TextBox2` para `InsertProductName` e `InsertUnitPrice`.

Adicionar um controle RequiredFieldValidator para o `ProductName` `InsertItemTemplate`. Definir o `ControlToValidate` para o `ID` da caixa de texto no modelo, seu `Text` propriedade como "\*" e seu `ErrorMessage` propriedade como "Você deve fornecer o nome do produto".

Como o `UnitPrice` é necessário para essa página, ao adicionar um novo registro, adicionar um controle RequiredFieldValidator para o `UnitPrice` `InsertItemTemplate`, configuração seu `ControlToValidate`, `Text`, e `ErrorMessage` propriedades adequadamente. Finalmente, adicione um CompareValidator para o `UnitPrice` `InsertItemTemplate` , configurando seus `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, e `ValueToCompare` propriedades como com o `UnitPrice`do CompareValidator no GridView `EditItemTemplate`.

Depois de adicionar esses controles de validação, um novo produto não pode ser adicionado ao sistema, se seu nome não for fornecido ou se seu preço é um número negativo ou ilegalmente formatado.


[![Lógica de validação foi adicionada à Interface de inserção de DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image34.png)

**Figura 12**: lógica de validação foi adicionada à Interface de inserção de DetailsView ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Etapa 6: Particionar os controles de validação em grupos de validação

Nossa página consiste em dois conjuntos distintos logicamente de controles de validação: aqueles que correspondem a GridView da edição de interface e aqueles que correspondem a DetailsView da inserção de interface. Por padrão, quando ocorre um postback *todos os* controles de validação na página são verificados. No entanto, ao editar um registro não queremos que os controles de validação da interface de DetailsView inserindo para validar. Figura 13 ilustra nosso dilema atual quando um usuário estiver editando um produto com valores permitidos, clicando em atualização faz com que um erro de validação porque os valores de nome e o preço na interface de inserção são em branco.


[![Um produto de atualização faz com que os controles de validação da Interface inserindo para acionar](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image37.png)

**Figura 13**: atualizar um produto faz com que os controles de validação da Interface inserindo para acionar ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image39.png))


Controles de validação no ASP.NET 2.0 podem ser particionados em grupos de validação por meio de seus `ValidationGroup` propriedade. Para associar um conjunto de controles de validação em um grupo, basta definir seus `ValidationGroup` propriedade com o mesmo valor. Para nosso tutorial, defina o `ValidationGroup` propriedades de controles de validação no TemplateFields do GridView para `EditValidationControls` e `ValidationGroup` propriedades de TemplateFields de DetailsView para `InsertValidationControls`. Essas alterações podem ser feitas diretamente na marcação declarativa ou usando a janela de propriedades ao usar o Designer Editar interface do modelo.

Além da validação controles, o botão e controles de botão relacionados no ASP.NET 2.0 também incluem um `ValidationGroup` propriedade. Os validadores de um grupo de validação são verificados quanto à validade somente quando um postback seja induzido por um botão que tem o mesmo `ValidationGroup` configuração de propriedade. Por exemplo, em ordem de botão de inserção de DetailsView disparar o `InsertValidationControls` grupo de validação, precisamos definir o CommandField `ValidationGroup` propriedade `InsertValidationControls` (consulte a Figura 14). Além disso, defina o GridView do CommandField `ValidationGroup` propriedade `EditValidationControls`.


[![Conjunto de DetailsView's propriedade ValidationGroup do CommandField InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image40.png)

**Figura 14**: conjunto de DetailsView do CommandField `ValidationGroup` propriedade `InsertValidationControls` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image42.png))


Após essas alterações, o DetailsView do GridView TemplateFields e CommandFields deve ser semelhante ao seguinte:

O DetailsView TemplateFields e CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample4.aspx)]

O GridView CommandField e TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample5.aspx)]

Agora os controles de validação específicas ao editar acionam somente quando o botão de atualização do GridView é clicado e o validação específicas ao inserir controles acionado somente quando o botão de inserção de DetailsView é clicado, resolver o problema realçado por Figura 13. No entanto, com essa alteração nosso controle ValidationSummary não exibe mais ao inserir dados inválidos. O controle ValidationSummary também contém um `ValidationGroup` propriedade e só mostra informações de resumo para os controles de validação em seu grupo de validação. Portanto, precisamos ter dois controles de validação nesta página, uma para o `InsertValidationControls` grupo de validação e outro para `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample6.aspx)]

Com essa adição nosso tutorial está concluído!

## <a name="summary"></a>Resumo

Enquanto BoundFields pode fornecer uma interface de inserção e edição, a interface não é personalizável. Normalmente, queremos adicionar controles de validação para a edição e a inserção interface para garantir que o usuário insere entradas necessárias em um formato válido. Para fazer isso, deve converter o BoundFields TemplateFields e adicione os controles de validação para o modelo apropriado (s). Neste tutorial, estendido o exemplo da *examinando os eventos associados inserindo, atualizando e excluindo* tutorial, adicionando controles de validação para ambos os DetailsView do inserindo interface e o GridView interface de edição. Além disso, vimos como exibir informações de resumo de validação usando o controle ValidationSummary e como particionar os controles de validação na página em grupos distintos de validação.

Como vimos neste tutorial, TemplateFields permitem que as interfaces de edição e inserção ser ampliada para incluir os controles de validação. TemplateFields também pode ser estendido para incluir os controles de Web de entrada adicionais, permitindo que a caixa de texto a ser substituído por um controle da Web mais adequado. Em nosso tutorial Avançar veremos como substituir o controle de caixa de texto com um controle DropDownList associado a dados, que é ideal quando você editar uma chave estrangeira (como `CategoryID` ou `SupplierID` no `Products` tabela).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Liz Shulok e Zack Jones. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
[Próximo](customizing-the-data-modification-interface-cs.md)
