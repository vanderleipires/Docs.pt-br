---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: Usando TemplateFields no controle GridView (VB) | Microsoft Docs
author: rick-anderson
description: Para fornecer flexibilidade, GridView oferece o TemplateField, que renderiza usando um modelo. Um modelo pode incluir uma combinação de HTML estático, controles de Web, e...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 411a3a2e4067d0e9b41143d85ddfb9b1031fd684
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829159"
---
<a name="using-templatefields-in-the-gridview-control-vb"></a>Usando TemplateFields no controle GridView (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe) ou [baixar PDF](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> Para fornecer flexibilidade, GridView oferece o TemplateField, que renderiza usando um modelo. Um modelo pode incluir uma combinação de HTML estático, controles da Web e a sintaxe de associação de dados. Neste tutorial, examinaremos como usar o TemplateField para atingir um maior grau de personalização com o controle GridView.


## <a name="introduction"></a>Introdução

O GridView é composto de um conjunto de campos que indicam quais propriedades do `DataSource` devem ser incluídos na saída renderizada, juntamente com, como os dados serão exibidos. O tipo de campo mais simples é BoundField, que exibe um valor de dados como texto. Outros tipos de campo exibem os dados usando os elementos de HTML alternativos. CheckBoxField, por exemplo, é renderizado como uma caixa de seleção cujo estado marcado depende do valor de um campo de dados especificado; o ImageField renderiza uma imagem cuja origem da imagem se baseia em um campo de dados especificado. Hiperlinks e botões cujo estado depende de um valor de campo de dados subjacente podem ser renderizado usando os tipos de campo HyperLinkField e ButtonField.

Embora os tipos de campo CheckBoxField, ImageField, HyperLinkField e ButtonField permitam uma exibição alternativa dos dados, eles ainda são bastante limitados em relação a formatação. Um CheckBoxField só pode exibir uma caixa de seleção, enquanto um ImageField só pode exibir uma única imagem. E se precisa de um determinado campo para exibir um texto, uma caixa de seleção *e* uma imagem, tudo com base nos valores de campo de dados diferentes? Ou, e se quiséssemos exibir os dados usando um controle de Web que não seja a caixa de seleção, imagem, hiperlink ou botão? Além disso, o BoundField limita sua exibição para um único campo de dados. E se quiséssemos mostrar dois ou mais valores de campo de dados em uma única coluna de GridView?

Para acomodar esse nível de flexibilidade GridView oferece o TemplateField, que renderiza usando um *modelo*. Um modelo pode incluir uma combinação de HTML estático, controles da Web e a sintaxe de associação de dados. Além disso, o TemplateField tem uma variedade de modelos que podem ser usados para personalizar a renderização para diferentes situações. Por exemplo, o `ItemTemplate` é usado por padrão para renderizar a célula para cada linha, mas o `EditItemTemplate` modelo pode ser usado para personalizar a interface quando a edição de dados.

Neste tutorial, examinaremos como usar o TemplateField para atingir um maior grau de personalização com o controle GridView. No [tutorial anterior](custom-formatting-based-upon-data-vb.md) vimos como personalizar a formatação com base em dados subjacente usando a `DataBound` e `RowDataBound` manipuladores de eventos. Outra maneira de personalizar a formatação com base nos dados subjacentes é chamando métodos de formatação de dentro de um modelo. Vamos examinar essa técnica também neste tutorial.

Para este tutorial, usaremos TemplateFields para personalizar a aparência de uma lista de funcionários. Especificamente, podemos irá listar todos os funcionários, mas exibirá o funcionário nomes e sobrenomes em uma coluna, a data de contratação em um controle de calendário e uma coluna de status que indica o número de dias que eles já foram utilizados na empresa.


[![Três TemplateFields são usados para personalizar a exibição](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**Figura 1**: três TemplateFields são usados para personalizar a exibição ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Etapa 1: Associando dados a GridView

Em cenários em que você precisa usar TemplateFields para personalizar a aparência de relatório, posso encontrá-la mais fácil começar criando um controle GridView que contém apenas BoundFields primeiro e, em seguida, para adicionar novos TemplateFields ou converter a BoundFields existente para TemplateFields conforme necessário. Portanto, vamos começar este tutorial adicionando um GridView à página por meio do Designer e associação a um ObjectDataSource que retorna a lista de funcionários. Essas etapas criará um GridView com BoundFields para cada um dos campos employee.

Abra o `GridViewTemplateField.aspx` página e arraste um controle GridView na caixa de ferramentas para o Designer. Na marca inteligente do GridView optar por adicionar um novo controle ObjectDataSource invoca o `EmployeesBLL` da classe `GetEmployees()` método.


[![Adicionar um novo controle ObjectDataSource que invoca o método GetEmployees)](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**Figura 2**: adicionar um novo controle ObjectDataSource que invoca a `GetEmployees()` método ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image6.png))


Associando o GridView dessa maneira adiciona automaticamente um BoundField para cada uma das propriedades de funcionário: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, e `Country`. Para este relatório vamos não se preocupar com exibindo o `EmployeeID`, `ReportsTo`, ou `Country` propriedades. Para remover esses BoundFields, você pode:

- Use a caixa de diálogo campos clicar no link Edit Columns na marca inteligente do GridView para abrir essa caixa de diálogo. Em seguida, selecione os BoundFields do canto inferior esquerdo lista e clique no X vermelho de botão para remover o BoundField.
- Editar a sintaxe declarativa do GridView manualmente da exibição da fonte, exclua o `<asp:BoundField>` elemento para o BoundField que você deseja remover.

Depois de ter removido a `EmployeeID`, `ReportsTo`, e `Country` BoundFields, marcação de seu do GridView deve parecer com:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

Reserve um tempo para exibir nosso progresso em um navegador. Neste ponto, você deve ver uma tabela com um registro para cada funcionário e quatro colunas: uma para o funcionário sobrenome, um para seu nome, um para seu título e uma para a data de contratação.


[![O sobrenome, nome, título e HireDate campos são exibidos para cada funcionário](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**Figura 3**: O `LastName`, `FirstName`, `Title`, e `HireDate` campos são exibidos para cada funcionário ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Etapa 2: Exibir os nomes e sobrenomes em uma única coluna

Atualmente, cada funcionário do primeiro e último nomes são exibidos em uma coluna separada. Seria bom combiná-los em uma única coluna. Para fazer isso, precisamos usar o TemplateField. Podemos pode adicionar um novo TemplateField, adicionar a ele a marcação necessária e a sintaxe de associação de dados e, em seguida, exclua o `FirstName` e `LastName` BoundFields, ou podemos pode converter o `FirstName` BoundField em um TemplateField, edite o TemplateField para incluir o `LastName` valor e, em seguida, remova o `LastName` BoundField.

Ambas as abordagens net o mesmo resultado, mas pessoalmente gosto de conversão BoundFields em TemplateFields quando possível, porque a conversão adiciona automaticamente um `ItemTemplate` e `EditItemTemplate` com controles da Web e a sintaxe de associação de dados para simular a aparência e a funcionalidade do BoundField. O benefício é que precisaremos fazer menos trabalho com o TemplateField conforme o processo de conversão será fizeram parte do trabalho para nós.

Para converter um BoundField existente em um TemplateField, clique no link Edit Columns na marca inteligente do GridView, abrir a caixa de diálogo de campos. Selecione o BoundField converter na lista no canto inferior esquerdo e, em seguida, clique no link de "Convert esse campo em um TemplateField" no canto inferior direito.


[![Converter um BoundField em um TemplateField da caixa de diálogo de campos](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**Figura 4**: converter um BoundField em um TemplateField da caixa de diálogo Fields ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image12.png))


Vá em frente e converter a `FirstName` BoundField em um TemplateField. Após essa alteração, não há nenhuma diferença visão no Designer. Isso ocorre porque a conversão a BoundField em um TemplateField cria um TemplateField que mantém a aparência do BoundField. Apesar de existir nenhuma diferença visual neste momento no Designer, esse processo de conversão substituiu a sintaxe declarativa do BoundField - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` – com a seguinte sintaxe TemplateField:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

Como você pode ver, o TemplateField consiste em dois modelos de um `ItemTemplate` que tem um rótulo cujo `Text` estiver definida como o valor da `FirstName` campo de dados e uma `EditItemTemplate` com uma caixa de texto do controle cuja `Text` propriedade também é definida para o `FirstName` campo de dados. A sintaxe de associação de dados - `<%# Bind("fieldName") %>` -indica que o campo de dados *`fieldName`* está associado à propriedade de controle da Web especificada.

Para adicionar o `LastName` valor para este TemplateField, precisamos adicionar outro controle de Web do rótulo do campo de dados a `ItemTemplate` e associar seu `Text` propriedade `LastName`. Isso pode ser feito manualmente ou por meio do Designer. Para fazer isso à mão, basta adicionar a sintaxe declarativa apropriada para o `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

Para adicioná-lo por meio do Designer, clique no link Editar modelos na marca inteligente do GridView. Isso exibirá a interface de edição de modelo do GridView. Nesta marca inteligente da interface é uma lista dos modelos em um GridView. Como só temos um TemplateField neste momento, somente os modelos listados na lista suspensa são esses modelos para o `FirstName` TemplateField juntamente com o `EmptyDataTemplate` e `PagerTemplate`. O `EmptyDataTemplate` modelo, se especificado, será usado para renderizar a saída do GridView se não houver nenhum resultado nos dados ligados ao GridView; o `PagerTemplate`, se especificada, é usado para renderizar a interface de paginação para um GridView que dá suporte à paginação.


[![Modelos do GridView podem ser editados por meio do Designer](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**Figura 5**: modelos pode ser editado por meio do Designer do GridView ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image15.png))


Para exibir também o `LastName` no `FirstName` TemplateField arrastar o controle de rótulo da caixa de ferramentas na `FirstName` do TemplateField `ItemTemplate` no GridView da edição de modelo interface.


[![Adicionar um controle de Web do rótulo para ItemTemplate de FirstName TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**Figura 6**: adicionar um controle de Web de rótulo para o `FirstName` ItemTemplate do TemplateField ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image18.png))


Neste ponto, o controle de rótulo Web adicionado para o TemplateField tem seu `Text` propriedade definida como "Label". Precisamos alterar isso para que essa propriedade está associada ao valor da `LastName` em vez disso, o campo de dados. Para realizar esse clique na marca inteligente do controle de rótulo e escolha a opção de Editar DataBindings.


[![Escolha a opção de Editar DataBindings na marca inteligente do rótulo](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**Figura 7**: escolha a opção de DataBindings editar a partir de marca do rótulo inteligente ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image21.png))


Isso abrirá a caixa de diálogo DataBindings. A partir daqui, você pode selecionar a propriedade para participar de vinculação de dados na lista à esquerda e escolha o campo para associar os dados na lista suspensa à direita. Escolha o `Text` propriedade da esquerda e o `LastName` campo à direita e clique em Okey.


[![Associar a propriedade de texto para o campo de dados do sobrenome](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**Figura 8**: associar o `Text` propriedade para o `LastName` campo de dados ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image24.png))


> [!NOTE]
> A caixa de diálogo DataBindings permite que você indique se deseja executar a vinculação de dados bidirecional. Se você deixar isso desmarcado, a sintaxe de associação de dados `<%# Eval("LastName")%>` será usado em vez de `<%# Bind("LastName")%>`. Qualquer uma das abordagens é adequado para este tutorial. Associação de dados bidirecional se torna importante ao inserir e editar dados. Para exibir apenas os dados, no entanto, qualquer uma das abordagens funcionará igualmente bem. Discutiremos a vinculação de dados bidirecional em detalhes em tutoriais futuros.


Reserve um tempo para exibir esta página por meio de um navegador. Como você pode ver, o GridView ainda inclui quatro colunas; No entanto, o `FirstName` coluna lista agora *ambos* o `FirstName` e `LastName` valores de campo de dados.


[![Os valores LastName e FirstName são mostrados em uma única coluna](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**Figura 9**: tanto o `FirstName` e `LastName` valores são mostrados em uma única coluna ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image27.png))


Para concluir esta etapa primeiro, remova os `LastName` BoundField e renomeie o `FirstName` do TemplateField `HeaderText` propriedade como "Name". Após essas alterações marcação declarativa do GridView deve ser semelhante ao seguinte:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]


[![Primeiro e último nomes de cada funcionário são exibidos em uma coluna](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**Figura 10**: primeiro e último nomes de cada funcionário são exibidos em uma coluna ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Etapa 3: Usando o controle de calendário para exibir o`HiredDate`campo

Exibir um valor de campo de dados como texto em um GridView é tão simple quanto usar um BoundField. Para determinados cenários, no entanto, os dados é melhor expressa usando um determinado controle de Web em vez de apenas texto. Tal personalização da exibição de dados é possível com TemplateFields. Por exemplo, em vez disso, que exibe a data de contratação do funcionário como texto, podemos pode mostrar um calendário (usando [o controle de calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) com suas datas de contratação realçado.

Para fazer isso, inicie, convertendo o `HiredDate` BoundField em um TemplateField. Simplesmente ir para a marca inteligente do GridView e clique no link Edit Columns, abrir a caixa de diálogo de campos. Selecione o `HiredDate` BoundField e clique em "convertem este campo em um TemplateField."


[![Converter o HiredDate BoundField em um TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**Figura 11**: converter o `HiredDate` BoundField em um TemplateField ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image33.png))


Como vimos na etapa 2, isso substituirá o BoundField com um TemplateField que contém um `ItemTemplate` e `EditItemTemplate` com um rótulo e uma caixa de texto cujo `Text` propriedades são vinculadas ao `HiredDate` valor usando a sintaxe de associação de dados `<%# Bind("HiredDate")%>`.

Para substituir o texto com um controle de calendário, edite o modelo, removendo o rótulo e adicionando um controle de calendário. No Designer, selecione Editar modelos de marca inteligente do GridView e escolha o `HireDate` do TemplateField `ItemTemplate` na lista suspensa. Em seguida, exclua o controle de rótulo e arraste um controle de calendário da caixa de ferramentas para a interface de edição de modelo.


[![Adicionar um controle de calendário para o HireDate ItemTemplate do TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**Figura 12**: adicionar um controle de calendário para o `HireDate` do TemplateField `ItemTemplate` ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image36.png))


Neste ponto, cada linha de GridView conterá um controle de calendário no seu `HiredDate` TemplateField. No entanto, o funcionário real da `HiredDate` valor não está definido em qualquer lugar no controle de calendário, fazendo com que cada controle de calendário padrão para mostrar a data e o mês atual. Para corrigir isso, precisamos atribuir cada funcionário `HiredDate` para o controle de calendário [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) e [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) propriedades.

A partir de smart tag do controle de calendário, escolha Editar DataBindings. Em seguida, associe ambas `SelectedDate` e `VisibleDate` propriedades para o `HiredDate` campo de dados.


[![Associar o SelectedDate e VisibleDate propriedades para o campo de dados HiredDate](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**Figura 13**: associar o `SelectedDate` e `VisibleDate` propriedades para o `HiredDate` campo de dados ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image39.png))


> [!NOTE]
> Data de selecionada do controle de calendário não precisa estar visível necessariamente. Por exemplo, um calendário pode ter 1 de agosto<sup>st</sup>, 1999 como a data selecionada, mas ser mostrando o mês e ano. As datas selecionadas e visíveis são especificados pelo controle de calendário `SelectedDate` e `VisibleDate` propriedades. Como queremos ambas as selecione do funcionário `HiredDate` e certifique-se de que é mostrado precisaremos ligar a ambas as propriedades para o `HireDate` campo de dados.


Ao exibir a página em um navegador, o calendário agora mostra o mês da data de contratado do funcionário e seleciona esse data específica.


[![HiredDate do funcionário é mostrada no controle de calendário](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**Figura 14**: O funcionário `HiredDate` é mostrado no controle de calendário ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image42.png))


> [!NOTE]
> Ao contrário de todos os exemplos que vimos até agora, para este tutorial fizemos *não* definir `EnableViewState` propriedade `False` para este GridView. A razão para essa decisão é porque clicar as datas do controle de calendário faz com que um postback, definir a data do calendário selecionada como a data simplesmente clicada. Se o estado de exibição do GridView estiver desabilitado, no entanto, em cada postagem dados do GridView é ligado novamente a fonte de dados subjacente, que faz com que a data do calendário selecionada ser definido *volta* para o funcionário `HireDate`, sobrescrever a data escolhida pelo usuário.


Para este tutorial é um debate sentido, pois o usuário não é possível atualizar o funcionário `HireDate`. Provavelmente seria melhor configurar o controle de calendário para que as datas não são selecionáveis. Independentemente disso, este tutorial mostra que em algumas circunstâncias estado de exibição deve ser habilitado para fornecer determinadas funcionalidades.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Etapa 4: Mostrar o número de dias que o funcionário trabalhou para a empresa

Até agora, vimos dois aplicativos de TemplateFields:

- Combinando dois ou mais valores de campo de dados em uma coluna, e
- Expressando um valor de campo de dados usando um controle da Web em vez de texto

Um terceiro uso de TemplateFields é exibir metadados sobre o GridView de dados subjacente. Além de Mostrar datas de contratação dos funcionários, por exemplo, podemos também poderá ter uma coluna que exibe o número de dias total foram no trabalho.

Ainda outro uso de TemplateFields surge em cenários quando os dados subjacentes precisam ser exibidos diferentemente no relatório de página da web que, no formato, ele é armazenado no banco de dados. Imagine que o `Employees` tabela tinha uma `Gender` campo que o caractere de armazenados `M` ou `F` para indicar o sexo do funcionário. Ao exibir essas informações em uma página da web que queremos mostrar o sexo, como "Masculino" ou "Feminino", em vez de apenas "M" ou "F".

Ambos os cenários podem ser tratados com a criação de um *método de formatação* na classe de code-behind da página ASP.NET (ou em uma biblioteca de classe separada, implementado como um `Shared` método) que é chamado com o modelo. Tal um método de formatação é invocado do modelo usando a mesma sintaxe de associação de dados Vista anteriormente. O método de formatação pode levar em qualquer número de parâmetros, mas deve retornar uma cadeia de caracteres. Essa cadeia de caracteres retornada é o HTML que é injetado no modelo.

Para ilustrar esse conceito, vamos incrementar nosso tutorial para mostrar uma coluna que lista o número total de dias que um funcionário estiver no trabalho. Esse método de formatação entrarão em um `Northwind.EmployeesRow` objeto e retornar o número de dias que o funcionário foi utilizado como uma cadeia de caracteres. Esse método pode ser adicionado à classe de code-behind da página ASP.NET, mas *devem* ser marcado como `Protected` ou `Public` para que seja acessível a partir do modelo.


[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

Uma vez que o `HiredDate` campo pode conter `NULL` valores devemos primeiro garantir que o valor não é do banco de dados `NULL` antes de continuar com o cálculo. Se o `HiredDate` valor é `NULL`, podemos simplesmente retornar a cadeia de caracteres "Desconhecido"; se não for `NULL`, podemos computar a diferença entre a hora atual e o `HiredDate` de valor e retornar o número de dias.

Para utilizar esse método, precisamos invocá-lo de um TemplateField no GridView usando a sintaxe de associação de dados. Comece adicionando um novo TemplateField a GridView clicando no link Edit Columns na marca inteligente do GridView e adicionando um novo TemplateField.


[![Adicionar um novo TemplateField a GridView](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**Figura 15**: adicionar um novo TemplateField a GridView ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image45.png))


Definir este TemplateField novos `HeaderText` propriedade como "Dias no trabalho" e seu `ItemStyle`do `HorizontalAlign` propriedade `Center`. Para chamar o `DisplayDaysOnJob` método de modelo, adicione um `ItemTemplate` e use a seguinte sintaxe de associação de dados:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem` Retorna um `DataRowView` objeto correspondente a `DataSource` registro associado para o `GridViewRow`. Sua `Row` propriedade retorna o tipo mais acentuado `Northwind.EmployeesRow`, que é passado para o `DisplayDaysOnJob` método. Essa sintaxe de associação de dados pode aparecer diretamente na `ItemTemplate` (conforme mostrado na sintaxe declarativa abaixo) ou podem ser atribuídos ao `Text` propriedade de um controle de Web de rótulo.

> [!NOTE]
> Como alternativa, em vez de passar em um `EmployeesRow` instância, podemos simplesmente passar na `HireDate` valor usando `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. No entanto, o `Eval` método retorna um `Object`, portanto, temos que alterar nosso `DisplayDaysOnJob` assinatura do método para aceitar um parâmetro de entrada do tipo `Object`, em vez disso. Podemos cegamente não é possível converter o `Eval("HireDate")` chamada para um `DateTime` porque o `HireDate` coluna no `Employees` tabela pode conter `NULL` valores. Portanto, seria preciso aceitar uma `Object` como o parâmetro de entrada para o `DisplayDaysOnJob` método, verifique se ele tivesse um banco de dados `NULL` valor (que pode ser feito usando `Convert.IsDBNull(objectToCheck)`) e, em seguida, proceda conforme necessário.


Devido a essas sutilezas, optei para passar em todo o `EmployeesRow` instância. No próximo tutorial, veremos um exemplo de ajuste mais para usar o `Eval("columnName")` sintaxe para passar um parâmetro de entrada em um método de formatação.

A seguir mostra a sintaxe declarativa para nosso GridView depois que o TemplateField foi adicionado e o `DisplayDaysOnJob` método chamado do `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

A Figura 16 mostra tutorial concluído, quando visualizado por meio de um navegador.


[![O número de dias que o funcionário tem sido sobre o trabalho é exibido](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**Figura 16**: O número de dias o funcionário tem sido sobre o trabalho é exibido ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-vb/_static/image48.png))


## <a name="summary"></a>Resumo

O TemplateField no controle GridView permite um maior grau de flexibilidade na exibição de dados que está disponível com os outros controles de campo. TemplateFields são ideais para situações em que:

- Vários campos de dados precisam ser exibidos em uma coluna de GridView
- Os dados é melhor expressa usando um controle da Web em vez de texto sem formatação
- A saída depende dos dados subjacentes, como a exibição de metadados ou em reformatar os dados

Além de personalizar a exibição de dados, TemplateFields também são usados para personalizar as interfaces do usuário usadas para a edição e inserção de dados, como veremos no futuro tutoriais.

Os próximos dois tutoriais continuam explorando modelos, começando com uma olhada no uso de TemplateFields em um DetailsView. Depois disso, voltaremos a FormView, que usa modelos em vez de campos para fornecer maior flexibilidade no layout e estrutura de dados.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Dan Jagers. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](custom-formatting-based-upon-data-vb.md)
> [Próximo](using-templatefields-in-the-detailsview-control-vb.md)
