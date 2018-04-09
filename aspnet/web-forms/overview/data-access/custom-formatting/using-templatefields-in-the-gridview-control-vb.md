---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: Usando TemplateFields no controle GridView (VB) | Microsoft Docs
author: rick-anderson
description: Para fornecer flexibilidade, GridView oferece TemplateField, que renderiza usando um modelo. Um modelo pode incluir uma combinação de HTML estático, controles da Web, e...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: f236c1cfaaeaa00f30b6a90553ad4e468e05ca23
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-templatefields-in-the-gridview-control-vb"></a>Usando TemplateFields no controle GridView (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe) ou [baixar PDF](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> Para fornecer flexibilidade, GridView oferece TemplateField, que renderiza usando um modelo. Um modelo pode incluir uma combinação de HTML estático, controles da Web e a sintaxe de associação de dados. Neste tutorial, examinaremos como usar o TemplateField para alcançar um grau maior de personalização com o controle GridView.


## <a name="introduction"></a>Introdução

GridView é composta de um conjunto de campos que indicam quais propriedades da `DataSource` devem ser incluídas na saída renderizada com como os dados serão exibidos. O tipo de campo mais simples é BoundField, que exibe um valor de dados como texto. Outros tipos de campo exibem os dados usando os elementos de HTML alternativos. CheckBoxField, por exemplo, é renderizada como uma caixa de seleção cujo estado selecionado depende do valor de um campo de dados especificado. o ImageField renderiza uma imagem cuja origem da imagem é baseada em um campo de dados especificado. Hiperlinks e botões cujo estado depende de um valor de campo de dados subjacente podem ser renderizado usando os tipos de campo HyperLinkField e ButtonField.

Ao permitir que os tipos de campo CheckBoxField, ImageField, HyperLinkField e ButtonField para um modo de exibição alternativo dos dados, eles ainda são bastante limitados em relação a formatação. Um CheckBoxField só pode exibir uma caixa de seleção, enquanto um ImageField só pode exibir uma única imagem. Se um determinado campo deve exibir um texto, uma caixa de seleção, *e* uma imagem, tudo com base nos valores de campo de dados diferente? Ou, se quiséssemos exibir os dados usando um controle da Web que não sejam a caixa de seleção, a imagem, o hiperlink ou o botão? Além disso, o BoundField limita sua exibição para um único campo de dados. E se quiséssemos mostrar dois ou mais valores de campo de dados em uma única coluna de GridView?

Para acomodar esse nível de flexibilidade GridView oferece TemplateField, que renderiza usando um *modelo*. Um modelo pode incluir uma combinação de HTML estático, controles da Web e a sintaxe de associação de dados. Além disso, o TemplateField tem uma variedade de modelos que podem ser usados para personalizar a renderização em situações diferentes. Por exemplo, o `ItemTemplate` é usado por padrão para renderizar a célula para cada linha, mas o `EditItemTemplate` modelo pode ser usado para personalizar a interface durante a edição de dados.

Neste tutorial, examinaremos como usar o TemplateField para alcançar um grau maior de personalização com o controle GridView. No [tutorial anterior](custom-formatting-based-upon-data-vb.md) , vimos como personalizar a formatação com base nos dados subjacentes usando a `DataBound` e `RowDataBound` manipuladores de eventos. Outra maneira de personalizar a formatação com base nos dados subjacentes é chamando métodos de formatação de dentro de um modelo. Vamos examinar essa técnica também neste tutorial.

Para este tutorial, usaremos TemplateFields para personalizar a aparência de uma lista de funcionários. Especificamente, podemos vai listar todos os funcionários, mas exibirá o funcionário nomes e sobrenomes em uma coluna, data de contratação em um controle de calendário e uma coluna de status que indica o número de dias já foi empregados na empresa.


[![Três TemplateFields são usados para personalizar a exibição](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**Figura 1**: TemplateFields três são usados para personalizar a exibição ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Etapa 1: Associação dos dados a GridView

Em cenários em que você precisa usar TemplateFields para personalizar a aparência de relatórios, seja mais fácil começar criando um controle GridView que contém apenas BoundFields primeiro e, em seguida, adicionar novos TemplateFields ou converter a BoundFields existente para TemplateFields conforme necessário. Portanto, vamos começar este tutorial adicionando um controle GridView à página por meio do Designer e vinculá-la a um ObjectDataSource que retorna a lista de funcionários. Essas etapas criará um GridView com BoundFields para cada um dos campos employee.

Abra o `GridViewTemplateField.aspx` página e arraste um controle GridView da caixa de ferramentas para o Designer. Na marca inteligente do GridView escolha para adicionar um novo controle ObjectDataSource que invoca o `EmployeesBLL` da classe `GetEmployees()` método.


[![Adicionar um novo controle ObjectDataSource que chama o método GetEmployees)](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**Figura 2**: adicionar um novo controle ObjectDataSource que invoca o `GetEmployees()` método ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image6.png))


Associação de GridView dessa maneira adicionará automaticamente um BoundField para cada uma das propriedades do funcionário: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, e `Country`. Para este relatório vamos não se preocupe com a exibição de `EmployeeID`, `ReportsTo`, ou `Country` propriedades. Para remover esses BoundFields, você pode:

- Use a caixa de diálogo de campos clique no link Editar colunas de marcas inteligentes do GridView para abrir essa caixa de diálogo. Em seguida, selecione os BoundFields do canto inferior esquerdo lista e clique no X vermelho no botão para remover o BoundField.
- Editar a sintaxe declarativa do GridView manualmente da exibição da fonte, exclua o `<asp:BoundField>` elemento para o BoundField que deseja remover.

Depois de ter removido o `EmployeeID`, `ReportsTo`, e `Country` BoundFields, marcação do GridView deve parecer com:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

Dedicar um tempo para exibir nosso progresso em um navegador. Agora você deve ver uma tabela com um registro para cada funcionário e quatro colunas: uma para o funcionário sobrenome, uma para seu nome, um para o título e uma para sua data de contratação.


[![O sobrenome, nome, título e HireDate campos são exibidos para cada funcionário](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**Figura 3**: O `LastName`, `FirstName`, `Title`, e `HireDate` campos são exibidos para cada funcionário ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Etapa 2: Exibir os nomes e sobrenomes em uma única coluna

Atualmente, cada funcionário do primeiro e último nomes são exibidos em uma coluna separada. Talvez seja adequado para combiná-las em uma única coluna. Para fazer isso, precisamos usar um TemplateField. Podemos pode adicionar um novo TemplateField, adicione a ele a marcação necessária e a sintaxe de associação de dados e, em seguida, excluir o `FirstName` e `LastName` BoundFields ou é possível converter o `FirstName` BoundField em um TemplateField Editar TemplateField para incluir o `LastName` valor e, em seguida, remova o `LastName` BoundField.

Ambas as abordagens net o mesmo resultado, mas pessoal gosto convertendo BoundFields em TemplateFields quando possível, porque a conversão adiciona automaticamente um `ItemTemplate` e `EditItemTemplate` com controles da Web e a sintaxe de associação de dados para simular a aparência e a funcionalidade de BoundField. A vantagem é que, precisamos fazer menos trabalho com o TemplateField como o processo de conversão será executou parte do trabalho para nós.

Para converter um BoundField existente em um TemplateField, clique no link Editar colunas de marca inteligente do GridView, abrir a caixa de diálogo de campos. Selecione o BoundField converter na lista no canto inferior esquerdo e, em seguida, clique no link de "Converter este campo em um TemplateField" no canto inferior direito.


[![Converter um BoundField em um TemplateField na caixa de diálogo campos](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**Figura 4**: converter um BoundField em um TemplateField da caixa de diálogo Fields ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image12.png))


Vá em frente e converter a `FirstName` BoundField em um TemplateField. Após essa alteração, não há nenhuma diferença perceptive no Designer. Isso ocorre porque a conversão de BoundField em um TemplateField cria um TemplateField que mantém a aparência do BoundField. Apesar de existir nenhuma diferença visual neste momento no Designer, o processo de conversão substituiu a sintaxe declarativa de BoundField - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` - com a seguinte sintaxe TemplateField:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

Como você pode ver, o TemplateField consiste em dois modelos de um `ItemTemplate` que tem um rótulo cujo `Text` propriedade é definida como o valor da `FirstName` campo de dados e um `EditItemTemplate` com uma caixa de texto do controle cuja `Text` propriedade também está definida para o `FirstName` campo de dados. A sintaxe de associação de dados - `<%# Bind("fieldName") %>` -indica que o campo de dados *`fieldName`* está associado à propriedade de controle da Web especificada.

Para adicionar o `LastName` valor para este TemplateField que precisamos adicionar outro controle de rótulo Web no campo de dados a `ItemTemplate` e associar seu `Text` propriedade `LastName`. Isso pode ser feito manualmente ou por meio do Designer. Para fazer isso manualmente, basta adicionar a sintaxe declarativa apropriada para o `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

Para adicioná-lo por meio do Designer, clique no link Editar modelos de marca inteligente do GridView. Isso exibirá a interface de edição de modelo do GridView. Na marca inteligente da interface é uma lista de modelos em GridView. Como só temos um TemplateField neste ponto, apenas modelos listados na lista suspensa são esses modelos para o `FirstName` TemplateField junto com o `EmptyDataTemplate` e `PagerTemplate`. O `EmptyDataTemplate` modelo, se especificado, será usado para renderizar a saída do GridView se não houver nenhum resultado os dados associados a GridView; o `PagerTemplate`, se especificada, é usado para renderizar a interface de paginação para um controle GridView que oferece suporte à paginação.


[![Modelos do GridView podem ser editados por meio do Designer](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**Figura 5**: O GridView modelos pode ser editado por meio do Designer ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image15.png))


Para exibir também o `LastName` no `FirstName` TemplateField arraste o controle de rótulo da caixa de ferramentas para a `FirstName` do TemplateField `ItemTemplate` em GridView da edição do modelo de interface.


[![Adicionar um controle de Web de rótulo para ItemTemplate de FirstName TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**Figura 6**: adicionar um controle de Web de rótulo para o `FirstName` ItemTemplate do TemplateField ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image18.png))


Neste ponto, o controle de rótulo Web adicionado para o TemplateField tem seu `Text` propriedade definida como "Rótulo". Precisamos alterar isso para que essa propriedade é associada ao valor da `LastName` em vez disso, o campo de dados. Para realizar esse clique na marca inteligente do controle de rótulo e escolha a opção Editar DataBindings.


[![Escolha a opção de ligações de dados de edição de marcas inteligentes do rótulo](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**Figura 7**: escolha a opção de DataBindings editar de marca inteligente do rótulo ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image21.png))


Isso abrirá a caixa de diálogo DataBindings. Aqui você pode selecionar a propriedade para participar de associação de dados da lista à esquerda e escolha o campo para associar os dados na lista suspensa à direita. Escolha o `Text` propriedade da esquerda e o `LastName` campo da direita e clique em Okey.


[![Associar a propriedade de texto para o campo de dados sobrenome](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**Figura 8**: associar o `Text` propriedade para o `LastName` campo de dados ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image24.png))


> [!NOTE]
> A caixa de diálogo DataBindings permite que você indique se deseja executar associação de dados bidirecional. Se você deixar isso desmarcada, a sintaxe de associação de dados `<%# Eval("LastName")%>` será usado em vez de `<%# Bind("LastName")%>`. Qualquer abordagem é bom para este tutorial. Associação de dados bidirecional se torna importante inserir e editar dados. Para exibir apenas os dados, no entanto, qualquer abordagem funcionará igualmente bem. Discutiremos a associação de dados bidirecional em detalhes em tutoriais futuros.


Dedicar um tempo para exibir esta página por meio de um navegador. Como você pode ver, GridView ainda inclui quatro colunas; No entanto, o `FirstName` coluna agora lista *ambos* o `FirstName` e `LastName` valores de campo de dados.


[![Os valores de LastName e FirstName são mostrados em uma única coluna](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**Figura 9**: tanto o `FirstName` e `LastName` valores são mostrados em uma única coluna ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image27.png))


Para concluir esta etapa primeiro, remova o `LastName` BoundField e renomear o `FirstName` do TemplateField `HeaderText` propriedade como "Name". Após essas alterações declarativo do GridView deve parecer com o seguinte:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]


[![Primeiro e último nomes de cada funcionário são exibidos em uma coluna](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**Figura 10**: primeiro e últimos nomes de cada funcionário são exibidos em uma coluna ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Etapa 3: Usando o controle de calendário para exibir o`HiredDate`campo

Exibir um valor de campo de dados como texto em um GridView é tão simple quanto usar um BoundField. Para determinados cenários, no entanto, os dados são melhor expressos usando um controle de Web específico em vez de texto apenas. Personalização da exibição de dados é possível com TemplateFields. Por exemplo, em vez disso, que exibe a data de contratação do funcionário como texto, pode mostrar um calendário (usando [o controle de calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) com suas datas de contratação realçado.

Para fazer isso, inicie convertendo o `HiredDate` BoundField em um TemplateField. Basta ir para a marca inteligente do GridView e clique no link de editar colunas, abrir a caixa de diálogo de campos. Selecione o `HiredDate` BoundField e clique em "convertem este campo em um TemplateField."


[![Converter o HiredDate BoundField em um TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**Figura 11**: converter o `HiredDate` BoundField em um TemplateField ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image33.png))


Como vimos na etapa 2, isso substituirá o BoundField TemplateField que contém um `ItemTemplate` e `EditItemTemplate` com um rótulo e a caixa de texto cujo `Text` propriedades associadas a `HiredDate` valor usando a sintaxe de associação de dados `<%# Bind("HiredDate")%>`.

Para substituir o texto com um controle de calendário, edite o modelo, removendo o rótulo e adicionando um controle de calendário. No Designer, clique em Editar modelos de marca inteligente do GridView e escolha o `HireDate` do TemplateField `ItemTemplate` na lista suspensa. Em seguida, exclua o controle de rótulo e arraste um controle de calendário da caixa de ferramentas para a interface de edição de modelo.


[![Adicionar um controle de calendário para o HireDate ItemTemplate do TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**Figura 12**: adicionar um controle de calendário para o `HireDate` do TemplateField `ItemTemplate` ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image36.png))


Neste ponto, cada linha em GridView conterá um controle de calendário no seu `HiredDate` TemplateField. No entanto, o funcionário real da `HiredDate` valor não está definido em qualquer lugar no controle de calendário, fazendo com que cada controle de calendário padrão para mostrando a data e o mês atual. Para corrigir isso, é preciso atribuir a cada funcionário `HiredDate` para o controle de calendário [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) e [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) propriedades.

Na marca inteligente do controle de calendário, escolha Editar DataBindings. Em seguida, associe ambas `SelectedDate` e `VisibleDate` propriedades para o `HiredDate` campo de dados.


[![Associar as propriedades de VisibleDate e SelectedDate ao campo de dados HiredDate](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**Figura 13**: associar o `SelectedDate` e `VisibleDate` propriedades para o `HiredDate` campo de dados ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image39.png))


> [!NOTE]
> Data selecionada do controle de calendário não precisa estar visível necessariamente. Por exemplo, um calendário pode ter 1 de agosto<sup>st</sup>, 1999 como a data selecionada, mas ser mostrando o mês e ano. A data selecionada e a data visível são especificadas pelo controle de calendário `SelectedDate` e `VisibleDate` propriedades. Como queremos ambos selecione o funcionário `HiredDate` e certifique-se de que é exibido, precisamos associar ambas essas propriedades para o `HireDate` campo de dados.


Ao exibir a página em um navegador, o calendário agora mostra o mês da data contratado do funcionário e seleciona a data específica.


[![HiredDate do funcionário é mostrado no controle de calendário](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**Figura 14**: O funcionário `HiredDate` é mostrado no controle de calendário ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image42.png))


> [!NOTE]
> Ao contrário de todos os exemplos que vimos até aqui, para este tutorial foi *não* definir `EnableViewState` propriedade `False` para este GridView. A razão para essa decisão é porque clicar as datas do controle de calendário causa um postback, definindo a data selecionada do calendário à data simplesmente clicada. Se o estado de exibição do GridView é desativado, no entanto, em cada postback dados do GridView é vinculada outra vez à sua fonte de dados subjacente, que faz com que a data selecionada do calendário a ser definido *novamente* para o funcionário `HireDate`, sobrescrever a data escolhida pelo usuário.


Para este tutorial é um debate sentido desde que o usuário não é capaz de atualizar o funcionário `HireDate`. Provavelmente seria melhor configurar o controle de calendário para que as datas não são selecionáveis. Independentemente, este tutorial mostra que em algumas circunstâncias estado de exibição deve ser habilitado para fornecer algumas funcionalidades.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Etapa 4: Mostrar o número de dias que o funcionário trabalha para a empresa

Até agora, vimos TemplateFields duas aplicações:

- Combinando dois ou mais valores de campo de dados em uma coluna, e
- Expressando um valor de campo de dados usando um controle da Web em vez de texto

Um terceiro uso TemplateFields está na exibição de metadados sobre o GridView de dados subjacente. Além de Mostrar datas de contratação dos funcionários, por exemplo, podemos também poderá ter uma coluna que exibe o número total de dias estarem no trabalho.

Ainda outro uso de TemplateFields surge em cenários quando os dados subjacentes precisam ser exibidos diferentemente no relatório de página da web, não no formato é armazenado no banco de dados. Imagine que o `Employees` tabela tinha um `Gender` campo armazenado o caractere `M` ou `F` para indicar o sexo do funcionário. Ao exibir essas informações em uma página da web, desejamos mostrar o sexo, como "Masculino" ou "Feminino", em vez de apenas "M" ou "F".

Ambos os cenários podem ser tratados com a criação de um *formatação método* na classe de code-behind da página ASP.NET (ou em uma biblioteca de classe separada, implementado como um `Shared` método) que é invocado do modelo. Tal método formatação é chamado de modelo usando a mesma sintaxe de associação de dados Vista anteriormente. O método de formatação pode levar em qualquer número de parâmetros, mas deve retornar uma cadeia de caracteres. Essa cadeia de caracteres retornada é o HTML que é injetado no modelo.

Para ilustrar esse conceito, vamos incrementar nosso tutorial para mostrar uma coluna que lista o número total de dias que um funcionário estiver no trabalho. Esse método de formatação entrarão em um `Northwind.EmployeesRow` de objeto e retorna o número de dias que o funcionário foi empregado como uma cadeia de caracteres. Esse método pode ser adicionado a classe de code-behind da página ASP.NET, mas *deve* ser marcado como `Protected` ou `Public` para que seja acessível a partir do modelo.


[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

Como o `HiredDate` campo pode conter `NULL` valores primeiro deve garantir que o valor não é do banco de dados `NULL` antes de continuar com o cálculo. Se o `HiredDate` valor é `NULL`, podemos simplesmente retornar a cadeia de caracteres "Desconhecido"; se não for `NULL`, podemos calcular a diferença entre a hora atual e o `HiredDate` valor e retornar o número de dias.

Para utilizar este método é necessário chamá-la da TemplateField em GridView usando a sintaxe de associação de dados. Comece adicionando um novo TemplateField a GridView clicando no link Editar colunas na marca inteligente do GridView e adicionando um novo TemplateField.


[![Adicionar um novo TemplateField a GridView](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**Figura 15**: adicionar um novo TemplateField a GridView ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image45.png))


Definir este TemplateField novo `HeaderText` propriedade como "Dias no trabalho" e seu `ItemStyle`do `HorizontalAlign` propriedade `Center`. Para chamar o `DisplayDaysOnJob` método do modelo, adicione um `ItemTemplate` e use a seguinte sintaxe de associação de dados:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem` Retorna um `DataRowView` objeto correspondente para o `DataSource` registro associado para o `GridViewRow`. Seu `Row` propriedade retorna o tipo mais acentuado `Northwind.EmployeesRow`, que é passado para o `DisplayDaysOnJob` método. Essa sintaxe de associação de dados pode aparecer diretamente no `ItemTemplate` (conforme mostrado na sintaxe declarativa abaixo) ou podem ser atribuídos ao `Text` propriedade de um controle de rótulo Web.

> [!NOTE]
> Como alternativa, em vez de passar uma `EmployeesRow` instância, pode passar no `HireDate` valor usando `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. No entanto, o `Eval` método retorna um `Object`, portanto, seria preciso alterar nosso `DisplayDaysOnJob` assinatura do método para aceitar um parâmetro de entrada do tipo `Object`, em vez disso. Nós cegamente não é possível converter o `Eval("HireDate")` chamada para um `DateTime` porque o `HireDate` coluna o `Employees` tabela pode conter `NULL` valores. Portanto, seria necessário aceitar um `Object` como o parâmetro de entrada para o `DisplayDaysOnJob` método, verifique se ele tivesse um banco de dados `NULL` valor (que pode ser realizado usando `Convert.IsDBNull(objectToCheck)`) e, em seguida, proceda conforme necessário.


Devido a essas sutilezas eu tenha optado para passar em todo o `EmployeesRow` instância. No tutorial Avançar veremos um exemplo mais de ajuste para usar o `Eval("columnName")` sintaxe para passar um parâmetro de entrada para um método de formatação.

O seguinte mostra a sintaxe declarativa para nosso GridView após o TemplateField foi adicionado e o `DisplayDaysOnJob` método chamado a partir de `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

A Figura 16 mostra o tutorial concluído, quando visualizada através de um navegador.


[![O número de dias que o funcionário tem sido o trabalho é exibido.](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**Figura 16**: O número de dias o funcionário tem sido o trabalho é exibido ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image48.png))


## <a name="summary"></a>Resumo

TemplateField no controle GridView possibilita um maior grau de flexibilidade na exibição de dados que está disponível com os outros controles de campo. TemplateFields são ideais para situações em que:

- Vários campos de dados precisam ser exibidos em uma coluna de GridView
- Os dados é melhor expressa usando um controle da Web em vez de texto sem formatação
- A saída depende dos dados subjacentes, como exibir metadados ou no reformatar os dados

Personalizando a exibição de dados, além de TemplateFields também são usados para personalizar as interfaces de usuário usadas para editar e inserir dados, conforme veremos no futuro tutoriais.

Os próximos dois tutoriais continuam explorando modelos, começando com uma olhada Usando TemplateFields em um DetailsView. Depois disso, voltaremos a FormView, que usa modelos em vez de campos para fornecer maior flexibilidade para o layout e a estrutura dos dados.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Dan Jagers. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](custom-formatting-based-upon-data-vb.md)
> [Próximo](using-templatefields-in-the-detailsview-control-vb.md)
