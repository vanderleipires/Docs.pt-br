---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
title: "Adicionando uma coluna de GridView dos botões de opção (VB) | Microsoft Docs"
author: rick-anderson
description: "Este tutorial explica como adicionar uma coluna de botões de opção a um controle GridView para fornecer ao usuário uma maneira mais intuitiva de selecionar uma única linha de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 2e31b60b-8723-4f14-b7ee-37859454dc3b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
msc.type: authoredcontent
ms.openlocfilehash: a9a470efc9e9416cd06fd4268f4e9505393dbed3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-gridview-column-of-radio-buttons-vb"></a>Adicionando uma coluna de GridView dos botões de opção (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_VB.exe) ou [baixar PDF](adding-a-gridview-column-of-radio-buttons-vb/_static/datatutorial51vb1.pdf)

> Este tutorial explica como adicionar uma coluna de botões de opção para um controle GridView para fornecer ao usuário uma maneira mais intuitiva de selecionar uma única linha de GridView.


## <a name="introduction"></a>Introdução

O controle GridView oferece uma grande quantidade de funcionalidade interna. Ele inclui um número de campos diferentes para exibir o texto, imagens, hiperlinks e botões. Ele dá suporte a modelos de personalização adicional. Com alguns cliques do mouse, ele possível fazer um GridView onde cada linha podem ser selecionada por meio de um botão, ou para habilitar a edição ou exclusão de recursos. Apesar da grande quantidade de recursos fornecidos, geralmente será situações em que adicionais, recursos sem suporte precisará ser adicionado. Este tutorial e as próximas duas examinaremos como aprimorar a funcionalidade de s GridView para incluir recursos adicionais.

Este tutorial e o outro se concentrar em aprimorar o processo de seleção de linha. Como examinados o [mestre/detalhes usando um GridView mestre selecionável com um Details DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md), podemos adicionar um CommandField a GridView que inclui um botão de seleção. Quando clicado, um postback tem lugar e s a GridView `SelectedIndex` propriedade é atualizada para o índice da linha cuja selecione botão foi clicado. No [mestre/detalhes usando um GridView mestre selecionável com um Details DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) tutorial, vimos como usar esse recurso para exibir detalhes para a linha selecionada de GridView.

Enquanto o botão de seleção funciona em muitas situações, ele pode não funcionar bem para outras pessoas. Em vez de usar um botão, dois outros elementos de interface do usuário são geralmente usados para seleção: o botão de opção e a caixa de seleção. Podemos pode aumentar o GridView para que, em vez de um botão de seleção, cada linha contém um botão de opção ou a caixa de seleção. Em cenários em que o usuário pode selecionar apenas um dos registros de GridView, o botão de opção pode ser preferível o botão de seleção. Em situações em que o usuário pode potencialmente selecionar vários registros, como em um aplicativo de email com base na web, em que um usuário pode desejar selecionar várias mensagens para a caixa de seleção Excluir oferece funcionalidade que não está disponível no botão de seleção ou botão de opção interfaces do usuário.

Este tutorial explica como adicionar uma coluna de botões de opção a GridView. O tutorial continuar explora usando caixas de seleção.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Etapa 1: Criando a aprimorar as páginas da Web de GridView

Antes de começarmos a aprimorar o GridView para incluir uma coluna de botões de opção, permitem s primeiro dedicar um tempo para criar as páginas ASP.NET em nosso projeto de site que vamos precisar para este tutorial e as próximas duas. Comece adicionando uma nova pasta chamada `EnhancedGridView`. Em seguida, adicione as seguintes páginas do ASP.NET para a pasta, certificando-se de associar cada página com o `Site.master` página mestre:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Adicionar as páginas ASP.NET para os tutoriais de SqlDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.gif)

**Figura 1**: adicionar as páginas ASP.NET para os tutoriais de SqlDataSource


Como em outras pastas, `Default.aspx` no `EnhancedGridView` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicione este controle de usuário para `Default.aspx` arrastando-o no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.png)

**Figura 2**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.png))


Por fim, adicione esses quatro páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após o usando o controle SqlDataSource `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample1.xml)]

Após a atualização `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para a edição, inserção e exclusão tutoriais.


![O mapa do Site agora inclui entradas para a melhorar os tutoriais de GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.gif)

**Figura 3**: O mapa de Site agora inclui entradas para a melhorar os tutoriais de GridView


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Etapa 2: Exibir os fornecedores em um controle GridView

Para este tutorial permite s a criar um controle GridView que lista os fornecedores dos EUA, com cada linha GridView fornecendo um botão de opção. Depois de selecionar um fornecedor por meio do botão de opção, o usuário pode exibir os produtos do fornecedor, clicando em um botão. Enquanto essa tarefa pode parecer trivial, há um número de sutilezas que a tornam muito complicado. Antes de examinarmos esses sutilezas, permitem s primeiro obter um GridView listando os fornecedores.

Comece abrindo o `RadioButtonField.aspx` página o `EnhancedGridView` pasta arrastando um GridView da caixa de ferramentas para o Designer. Definir o GridView s `ID` para `Suppliers` e, na marca inteligente, de optar por criar uma nova fonte de dados. Especificamente, crie um ObjectDataSource denominado `SuppliersDataSource` que obtém seus dados a partir o `SuppliersBLL` objeto.


[![Criar um novo ObjectDataSource denominado SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.png)

**Figura 4**: criar um novo ObjectDataSource nomeado `SuppliersDataSource` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.png))


[![Configurar o ObjectDataSource para usar a classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.png)

**Figura 5**: configurar o ObjectDataSource para usar o `SuppliersBLL` classe ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.png))


Como só queremos lista esses fornecedores nos EUA, escolha o `GetSuppliersByCountry(country)` método na lista suspensa na guia SELECT.


[![Configurar o ObjectDataSource para usar a classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.png)

**Figura 6**: configurar o ObjectDataSource para usar o `SuppliersBLL` classe ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.png))


Na atualização guia, selecione (nenhum) a opção e clique em Avançar.


[![Configurar o ObjectDataSource para usar a classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.png)

**Figura 7**: configurar o ObjectDataSource para usar o `SuppliersBLL` classe ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.png))


Desde o `GetSuppliersByCountry(country)` método aceita um parâmetro, o Assistente Configurar fonte de dados nos solicita a origem do parâmetro. Para especificar um valor codificado (Estados Unidos, neste exemplo), deixe o parâmetro de lista suspensa de fonte definida como None e insira o valor padrão na caixa de texto. Clique em Concluir para concluir o assistente.


[![Use a USA como o valor padrão para o parâmetro do país](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.png)

**Figura 8**: Use EUA como o valor padrão para o `country` parâmetro ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.png))


Depois de concluir o assistente, GridView incluirá um BoundField para cada um dos campos de dados do fornecedor. Remover tudo, exceto o `CompanyName`, `City`, e `Country` BoundFields e renomeie o `CompanyName` BoundFields `HeaderText` propriedade ao fornecedor. Depois de fazer isso, a sintaxe declarativa GridView e ObjectDataSource deve ser semelhante ao seguinte.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample2.aspx)]

Para este tutorial, permitem s permitem ao usuário exibir o fornecedor selecionado os produtos na mesma página como lista de fornecedores, ou em uma página diferente. Para acomodar isso, adicione dois controles da Web de botão à página. Ve conjunto o `ID` s estes dois botões `ListProducts` e `SendToProducts`, com a ideia de que, quando `ListProducts` é clicado um postback ocorrerá e os produtos do fornecedor selecionado s serão listados na mesma página, mas quando `SendToProducts` é clicado, o usuário será ser movido para uma outra página que lista os produtos.

A Figura 9 mostra a `Suppliers` GridView e duas Web botão controla quando exibida por meio de um navegador.


[![Os fornecedores dos EUA têm seu nome, cidade e país informações listadas](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.png)

**Figura 9**: os fornecedores de EUA têm seus nome, cidade e país informações listadas ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Etapa 3: Adicionando uma coluna de botões de opção

Neste ponto o `Suppliers` GridView tem três BoundFields exibindo o nome da empresa, cidade e país de cada fornecedor nos EUA. Ele ainda estiver ausente no entanto uma coluna de botões de opção. Infelizmente, o GridView incluir um interno RadioButtonField, caso contrário, pode apenas adicionar que à grade e ser feitos. Em vez disso, podemos adicionar um TemplateField e configurar seu `ItemTemplate` para renderizar um botão de opção, resultando em um botão de opção para cada linha em GridView.

Inicialmente, talvez presumimos que a interface do usuário desejado pode ser implementada, adicionando um controle RadioButton Web para o `ItemTemplate` de um TemplateField. Enquanto isso realmente adicionará um botão de opção único para cada linha de GridView, os botões de opção não podem ser agrupados e, portanto, não são mutuamente exclusivos. Ou seja, um usuário final é capaz de selecionar vários botões de opção simultaneamente de GridView.

Embora usar um TemplateField dos controles RadioButton Web não oferece a funcionalidade que precisamos, permitem s implementar essa abordagem, como s vale a pena examinar o motivo pelo qual os botões de opção resultantes não estão agrupados. Comece adicionando um TemplateField a GridView fornecedores, tornando-o campo mais à esquerda. Em seguida, de GridView s marca inteligente, clique no link Editar modelos e arraste um controle RadioButton Web da caixa de ferramentas para o s TemplateField `ItemTemplate` (consulte a Figura 10). Definir o s RadioButton `ID` propriedade `RowSelector` e o `GroupName` propriedade `SuppliersGroup`.


[![Adicionar um controle RadioButton Web a ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.png)

**Figura 10**: adicionar um controle RadioButton Web para o `ItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.png))


Depois de fazer essas adições por meio do Designer, a marcação de s GridView deve ser semelhante ao seguinte:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample3.aspx)]

O botão de opção s [ `GroupName` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) é usado para agrupar uma série de botões de opção. Todos os controles de botão de opção com o mesmo `GroupName` valor são considerados agrupados; somente um botão pode ser selecionado de um grupo por vez. O `GroupName` propriedade especifica o valor para o botão de opção renderizado s `name` atributo. O navegador examina os botões de opção `name` atributos para determinar o rádio botão agrupamentos.

Com o controle RadioButton Web adicionado para o `ItemTemplate`, visite esta página por meio de um navegador e clique nos botões de opção nas linhas de grade s. Observe como os botões de opção não estão agrupados, tornando possível selecionar todas as linhas, como a Figura 11 mostra.


[![Os botões de opção GridView s não são agrupados](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.png)

**Figura 11**: botões de opção a GridView s não são agrupadas ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.png))


O motivo pelo qual os botões de opção não estão agrupados é porque seus renderizado `name` os atributos são diferentes, mesmo tendo o mesmo `GroupName` configuração de propriedade. Para ver essas diferenças, não uma fonte de exibição/do navegador e examine a marcação de botão de opção:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample4.html)]

Observe como o `name` e `id` atributos não são os valores exatos, conforme especificado na janela Propriedades, mas são prefixados com um número de outros `ID` valores. Adicional `ID` valores adicionados à frente do renderizado `id` e `name` atributos estão o `ID` s do rádio botões controles pai o `GridViewRow` s `ID` s, o s GridView `ID`, o Conteúdo do controle s `ID`e o formulário da Web s `ID`. Essas `ID` s são adicionados para que cada renderizado controle GridView Web tem uma única `id` e `name` valores.

Renderizados cada controle necessidades outro `name` e `id` porque isso é como o navegador identifica exclusivamente cada controle no lado do cliente e como ele identifica para o servidor web que ação ou ocorreu uma alteração em um postback. Por exemplo, imagine que queremos executar um código do lado do servidor sempre que um botão de opção s verificada estado foi alterado. Podemos pode fazer isso definindo o s RadioButton `AutoPostBack` propriedade `True` e criar um manipulador de eventos para o `CheckChanged` evento. No entanto, se o renderizado `name` e `id` valores para todos os botões de opção eram os mesmos no postback não foi possível determinar quais específico RadioButton foi clicado.

Curto dele é que não podemos criar uma coluna de botões de opção em um GridView usando o controle de botão de opção Web. Em vez disso, podemos deve usar técnicas arcaicos em vez disso, para garantir que a marcação apropriada é injetada em cada linha em GridView.

> [!NOTE]
> Como o controle RadioButton Web, o controle HTML, quando adicionada a um modelo de botão de opção incluirá exclusivos `name` atributo, tornando os botões de opção na grade desagrupado. Se você não estiver familiarizado com os controles HTML, fique à vontade para ignorar esta anotação, como controles HTML são usados raramente, especialmente no ASP.NET 2.0. Mas se você estiver interessado em saber mais, consulte [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) entrada de blog s [controles da Web e controles HTML](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Usando um controle Literal para injetar marcação de botão de opção

Para agrupar todos os botões de opção em GridView corretamente, precisamos inserir manualmente a marcação de botões de opção para o `ItemTemplate`. Cada botão de opção precisa que o mesmo `name` de atributo, mas deve ter uma única `id` (no caso, desejamos um botão de opção por meio de script do lado do cliente de acesso) do atributo. Depois que um usuário seleciona um botão de opção e postagens de volta a página, o navegador enviará de volta o valor do botão de opção selecionado s `value` atributo. Portanto, cada botão de opção precisará de uma única `value` atributo. Por fim, em um postback precisamos nos certificar-se de adicionar o `checked` atributo para o um botão de opção selecionado, caso contrário, depois que o usuário faz uma seleção e postagens de volta, os botões de opção retornará ao seu estado padrão (todos os não selecionados).

Há duas abordagens que podem ser executadas para injetar marcação de baixo nível em um modelo. Um é uma combinação de marcação e chamadas para métodos definidos na classe code-behind de formatação. Essa técnica foi discutida primeiro o [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial. Em nosso caso, ele poderia ser algo assim:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample5.aspx)]

Aqui, `GetUniqueRadioButton` e `GetRadioButtonValue` seria métodos definidos na classe por trás do código que retornou apropriada `id` e `value` valores para cada botão de opção de atributo. Essa abordagem funciona bem para atribuir o `id` e `value` atributos, mas deixa a desejar quando precisar especificar o `checked` valor de atributo porque a sintaxe de associação de dados é executada apenas quando dados pela primeira vez são associados à GridView. Portanto, se o GridView tiver habilitado o estado de exibição, os métodos de formatação só serão acionado quando a página for carregada pela primeira vez (ou quando o GridView é explicitamente vinculada outra vez para a fonte de dados) e, portanto, a função que define o `checked` atributo ganha t ser chamado em Executa postback. -S um problema bastante sutil e um pouco além do escopo deste artigo, então deixarei isso. No entanto, eu recomendamos que tente usar a abordagem acima e ele por meio de trabalho para o ponto em que você vai ficar presa. Enquanto essa um exercício ganha t obter próximas para uma versão de trabalho, ela ajuda a promover uma compreensão mais profunda de GridView e o ciclo de vida de associação de dados.

Outra abordagem para Insira personalizado, marcação de baixo nível em um modelo e a abordagem que usaremos para este tutorial é adicionar um [controle Literal](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) no modelo. Em seguida, em GridView s `RowCreated` ou `RowDataBound` manipulador de eventos, o controle Literal pode ser acessado por meio de programação e seu `Text` propriedade definida como a marcação de emissão.

Iniciar, removendo o botão de opção de s TemplateField `ItemTemplate`, substituindo-o por um controle Literal. Definir o controle Literal s `ID` para `RadioButtonMarkup`.


[![Adicionar um controle Literal para o ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.png)

**Figura 12**: adicionar um controle Literal para o `ItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.png))


Em seguida, crie um manipulador de eventos para o s GridView `RowCreated` eventos. O `RowCreated` evento é acionado para cada linha adicionada, ou não os dados estão sendo vinculada outra vez a GridView. Isso significa que mesmo em um postback quando os dados são recarregados do estado de exibição, o `RowCreated` ainda dispara um evento e este é o motivo pelo qual estamos usando ele em vez de `RowDataBound` (o que é acionado somente quando os dados é explicitamente associados aos dados do controle da Web).

No manipulador de eventos, só queremos continuar se podemos re lidar com uma linha de dados. Para cada linha de dados deseja referenciar programaticamente o `RadioButtonMarkup` controle Literal e defina seu `Text` propriedade para a marcação de emissão. Como mostra o código a seguir, a marcação emitida cria uma opção botão cuja `name` atributo é definido como `SuppliersGroup`, cujo `id` atributo é definido como `RowSelectorX`, onde *X* é o índice da linha GridView, e cuja `value` atributo é definido como o índice da linha GridView.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample6.vb)]

Quando uma linha GridView está selecionada e ocorre um postback, estamos interessados no `SupplierID` do fornecedor selecionado. Portanto, alguém pode pensar que o valor de cada botão de opção deve ser real `SupplierID` (em vez do índice da linha GridView). Enquanto isso pode funcionar em certas circunstâncias, poderá ser um risco à segurança cegamente aceitar e processar um `SupplierID`. Nosso GridView, por exemplo, lista apenas os fornecedores nos EUA. No entanto, se o `SupplierID` é passado diretamente usando o botão de opção, Novidades para interromper um usuário mal intencionado de manipulação de `SupplierID` valor enviado novamente postback? Usando o índice de linha como o `value`e, em seguida, obtendo o `SupplierID` na postagem do `DataKeys` coleção, podemos garantir que o usuário está usando somente um do `SupplierID` valores associados a uma das linhas de GridView.

Depois de adicionar este código de manipulador de eventos, reserve um minuto para testar a página em um navegador. Primeiro, observe que apenas uma opção botão na grade pode ser selecionada por vez. No entanto, quando a seleção de um botão de opção e clicar em um dos botões, ocorre um postback e todos os botões de opção voltarão ao estado inicial (ou seja, em um postback, o botão de opção selecionado é não selecionado). Para corrigir isso, é preciso aumentar o `RowCreated` manipulador de eventos para que ele inspeciona o índice de botão de opção selecionado enviado de postback e adiciona o `checked="checked"` a marcação emitido as correspondências de índice de linha de atributo.

Quando ocorre um postback, o navegador envia de volta a `name` e `value` do botão de opção selecionado. O valor pode ser recuperado por meio de programação usando `Request.Form("name")`. O [ `Request.Form` propriedade](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) fornece um [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) que representa as variáveis de formulário. As variáveis de formulário são os nomes e valores dos campos de formulário na página da web e são enviadas por navegador da web, sempre que um postback tem lugar. Porque o renderizado `name` atributo dos botões de opção em GridView é `SuppliersGroup`, quando a página da web é postada enviará o navegador `SuppliersGroup=valueOfSelectedRadioButton` volta para o servidor web (junto com os outros campos de formulário). Essas informações podem ser acessadas depois do `Request.Form` propriedade usando: `Request.Form("SuppliersGroup")`.

Desde será necessário para determinar o botão de opção selecionado de índice não somente no `RowCreated` manipulador de eventos, mas o `Click` manipuladores de eventos para os controles da Web de botão, permitem s adicionar um `SuppliersSelectedIndex` propriedade para a classe code-behind que retorna `-1`se nenhum botão de opção foi selecionada e o índice selecionado se um dos botões de opção é selecionado.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample7.vb)]

Com essa propriedade foi adicionada, sabemos para adicionar o `checked="checked"` marcação no `RowCreated` manipulador de eventos quando `SuppliersSelectedIndex` é igual a `e.Row.RowIndex`. O manipulador de eventos para incluir essa lógica de atualização:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample8.vb)]

Com essa alteração, o botão de opção selecionado permanece selecionado após um postback. Agora que temos a capacidade de especificar o botão de opção é selecionada, podemos pode alterar o comportamento para que quando a página foi visitada pela primeira vez, o primeiro botão de opção de linha s GridView foi selecionado (em vez de não ter nenhum botões de opção selecionado por padrão, que é atual comportamento). Para que o primeiro botão de opção selecionado por padrão, simplesmente altere o `If SuppliersSelectedIndex = e.Row.RowIndex Then` a seguinte instrução: `If SuppliersSelectedIndex = e.Row.RowIndex OrElse (Not Page.IsPostBack AndAlso e.Row.RowIndex = 0) Then`.

Neste ponto, adicionamos uma coluna de botões de opção agrupados a GridView que permite que uma única linha GridView para seleção e memorizados entre postbacks. Nossa próxima etapa será exibir os produtos fornecidos pelo fornecedor selecionado. Na etapa 4 veremos como redirecionar o usuário para outra página, enviar ao longo do selecionado `SupplierID`. Na etapa 5, veremos como exibir os produtos do fornecedor selecionado em um controle GridView na mesma página.

> [!NOTE]
> Em vez de usar um TemplateField (o foco dessa etapa 3 longa), criamos um personalizado `DataControlField` classe que renderiza a interface de usuário apropriada e a funcionalidade. O [ `DataControlField` classe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) é a classe base da qual derivam de BoundField, CheckBoxField, TemplateField e outros campos internos de GridView e DetailsView. Criar um personalizado `DataControlField` classe significa que a coluna de botões de opção pode ser adicionada apenas usando a sintaxe declarativa e também criam replicam a funcionalidade em outras páginas da web e outros aplicativos web muito mais fácil.


Se var já criou personalizado, compilado controles do ASP.NET, no entanto, você sabe que isso requer uma quantidade razoável de trabalho externo e carrega um host de sutilezas e casos de borda que devem ser tratados com cuidado. Portanto, podemos será abrir mão de implementação de uma coluna de botões de opção como um personalizado `DataControlField` classe agora e continuar com a opção TemplateField. Talvez, terá a chance de explorar criar, usar e implantando personalizadas `DataControlField` classes em um tutorial futuras!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Etapa 4: Exibindo os produtos do fornecedor selecionado em uma página separada

Depois que o usuário selecionou uma linha em GridView, precisamos mostrar os produtos de fornecedor selecionado. Em algumas circunstâncias, é aconselhável exibir esses produtos em uma página separada, em outros que nós talvez prefira fazê-lo na mesma página. Permitir que o s primeiro examinar como exibir os produtos em uma página separada. na etapa 5, examinaremos adicionando um controle GridView para `RadioButtonField.aspx` para exibir os produtos do fornecedor selecionado s.

Atualmente, há dois controles da Web de botão na página `ListProducts` e `SendToProducts`. Quando o `SendToProducts` botão é clicado, queremos enviar ao usuário `~/Filtering/ProductsForSupplierDetails.aspx`. Esta página foi criada no [filtragem de mestre/detalhes em duas páginas](../masterdetail/master-detail-filtering-across-two-pages-vb.md) tutorial e exibe os produtos para o fornecedor cujo `SupplierID` é passado por meio do campo querystring chamado `SupplierID`.

Para fornecer essa funcionalidade, criar um manipulador de eventos para o `SendToProducts` botão s `Click` eventos. Na etapa 3, adicionamos a `SuppliersSelectedIndex` propriedade, que retorna o índice da linha cujo botão de opção é selecionada. O correspondente `SupplierID` podem ser recuperados de GridView s `DataKeys` coleta e o usuário podem ser enviados para `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` usando `Response.Redirect("url")`.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample9.vb)]

Esse código muito funciona como um dos botões de opção é selecionado de GridView. Se, inicialmente, o GridView não tem qualquer botões de opção selecionado e o usuário clica o `SendToProducts` botão, `SuppliersSelectedIndex` será `-1`, que fará com que uma exceção seja lançada desde `-1` está fora do intervalo de índice a `DataKeys`coleção. Isso é não é uma preocupação, no entanto, se você decidir atualizar o `RowCreated` manipulador de eventos, conforme descrito na etapa 3 para que o primeiro botão de opção em GridView inicialmente selecionado.

Para acomodar uma `SuppliersSelectedIndex` valor `-1`, adicionar um controle de Web de rótulo para a página acima GridView. Definir seu `ID` propriedade `ChooseSupplierMsg`, sua `CssClass` propriedade para `Warning`, sua `EnableViewState` e `Visible` propriedades para `False`e sua `Text` propriedade tente escolher um fornecedor da grade. A classe CSS `Warning` exibe o texto em uma fonte vermelha, itálico, negrito, grande e é definido em `Styles.css`. Definindo o `EnableViewState` e `Visible` propriedades `False`, o rótulo não é renderizado exceto para somente aqueles postbacks onde o controle s `Visible` propriedade é definida por meio de programação como `True`.


[![Adicionar um controle de Web de rótulo acima GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image21.png)

**Figura 13**: adicionar um rótulo Web controle acima a GridView ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image22.png))


Em seguida, aumentar o `Click` manipulador de eventos para exibir o `ChooseSupplierMsg` rótulo se `SuppliersSelectedIndex` é menor que zero e redirecionar o usuário `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` caso contrário.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample10.vb)]

Visite a página em um navegador e clique no `SendToProducts` botão antes de selecionar um fornecedor de GridView. Como mostra a Figura 14, exibe o `ChooseSupplierMsg` rótulo. Em seguida, selecione um fornecedor e clique no `SendToProducts` botão. Isso será whisk você a uma página que lista os produtos fornecidos pelo fornecedor selecionado. A Figura 15 mostra o `ProductsForSupplierDetails.aspx` página quando o fornecedor Cervejaria pé grande foi selecionado.


[![O rótulo de ChooseSupplierMsg será exibido se o fornecedor não está selecionada](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image23.png)

**Figura 14**: O `ChooseSupplierMsg` rótulo é exibido se o fornecedor não está selecionado ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image24.png))


[![Os produtos do fornecedor selecionado são exibidos no ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image25.png)

**Figura 15**: os produtos do fornecedor selecionado são exibidos no `ProductsForSupplierDetails.aspx` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Etapa 5: Exibindo os produtos do fornecedor selecionado na mesma página

Na etapa 4, vimos como enviar o usuário para outra página da web para exibir o fornecedor selecionado os produtos. Como alternativa, os produtos do fornecedor selecionado s podem ser exibidos na mesma página. Para ilustrar isso, vamos adicionar outro GridView para `RadioButtonField.aspx` para exibir os produtos do fornecedor selecionado s.

Como só queremos este GridView de produtos para exibir quando um fornecedor tiver sido selecionado, adicionar um controle de painel Web sob o `Suppliers` GridView, definindo seu `ID` para `ProductsBySupplierPanel` e sua `Visible` propriedade `False`. Dentro do painel, adicione o texto produtos do fornecedor selecionado, seguido por um GridView denominado `ProductsBySupplier`. De GridView s marca inteligente, escolha para associá-lo para um novo ObjectDataSource denominado `ProductsBySupplierDataSource`.


[![Associar o ProductsBySupplier GridView para um novo ObjectDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image27.png)

**Figura 16**: associar o `ProductsBySupplier` GridView para um novo ObjectDataSource ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image28.png))


Em seguida, configure o ObjectDataSource para usar o `ProductsBLL` classe. Como só queremos recuperar os produtos fornecidos pelo fornecedor selecionado, especifique que o ObjectDataSource deve chamar o `GetProductsBySupplierID(supplierID)` método para recuperar seus dados. Selecione (nenhum) na lista suspensa na atualização, inserção e excluir guias.


[![Configurar o ObjectDataSource para usar o método GetProductsBySupplierID(supplierID)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image29.png)

**Figura 17**: configurar o ObjectDataSource para usar o `GetProductsBySupplierID(supplierID)` método ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image30.png))


[![Definir as listas suspensas para (nenhum) em UPDATE, INSERT e excluir guias](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image31.png)

**Figura 18**: definir a lista suspensa como (nenhum) na atualização, inserir e excluir guias ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image32.png))


Depois de configurar a selecionar, atualizar, inserir e excluir guias, clique em Avançar. Desde o `GetProductsBySupplierID(supplierID)` método espera um parâmetro de entrada, solicita que o assistente Criar fonte de dados para especificar a origem para o valor do parâmetro s.

Temos algumas opções aqui em especificando a origem do valor do parâmetro. Poderíamos usar o objeto de parâmetro padrão e atribuir programaticamente o valor de `SuppliersSelectedIndex` propriedade no parâmetro s `DefaultValue` propriedade em ObjectDataSource s `Selecting` manipulador de eventos. Voltar para o [definir programaticamente o valores do parâmetro ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb.md) tutorial para uma atualização em programaticamente atribuir valores para os parâmetros de s ObjectDataSource.

Como alternativa, podemos usar um ControlParameter e consulte o `Suppliers` GridView s [ `SelectedValue` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (consulte a Figura 19). O GridView s `SelectedValue` propriedade retorna o `DataKey` valor correspondente para o [ `SelectedIndex` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Em ordem para essa opção funcione, é preciso definir programaticamente a s GridView `SelectedIndex` propriedade na linha quando o `ListProducts` botão é clicado. Como benefício extra, definindo o `SelectedIndex`, terá o registro selecionado o `SelectedRowStyle` definido no `DataWebControls` tema (um plano de fundo amarelo).


[![Use um ControlParameter para especificar o GridView s SelectedValue como a origem do parâmetro](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image33.png)

**Figura 19**: Use um ControlParameter para especificar o s GridView SelectedValue como a origem do parâmetro ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image34.png))


Após concluir o assistente, o Visual Studio adicionará automaticamente os campos para os campos de dados do produto s. Remover tudo, exceto o `ProductName`, `CategoryName`, e `UnitPrice` BoundFields e altere o `HeaderText` propriedades preço, categoria e produto. Configurar o `UnitPrice` BoundField para que seu valor seja formatado como uma moeda. Depois de fazer essas alterações, o painel, GridView e ObjectDataSource s declarativo deve ser semelhante ao seguinte:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample11.aspx)]

Para concluir este exercício, precisamos definir a s GridView `SelectedIndex` propriedade para o `SelectedSuppliersIndex` e o `ProductsBySupplierPanel` painel s `Visible` propriedade `True` quando o `ListProducts` botão é clicado. Para fazer isso, crie um manipulador de eventos para o `ListProducts` controle de botão Web s `Click` evento e adicione o seguinte código:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample12.vb)]

Se um fornecedor não tiver sido selecionado de GridView, o `ChooseSupplierMsg` rótulo é exibido e o `ProductsBySupplierPanel` ocultado do painel. Caso contrário, se um fornecedor tiver sido selecionado, o `ProductsBySupplierPanel` é exibido e o GridView s `SelectedIndex` propriedade é atualizada.

Figura 20 mostra os resultados depois que o fornecedor Cervejaria pé grande foi selecionado e os produtos de Mostrar botão de página foi clicado.


[![Os produtos fornecidos pela Cervejaria pé grande são listados na mesma página](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image35.png)

**Figura 20**: os produtos fornecidos pela Cervejaria pé grande são listados na mesma página ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image36.png))


## <a name="summary"></a>Resumo

Conforme discutido no [mestre/detalhes usando um GridView mestre selecionável com um Details DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) tutorial, os registros podem ser selecionados de um controle GridView usando um CommandField cujo `ShowSelectButton` está definida como `True`. Mas o CommandField exibe seus botões como botões de ação regulares, links ou imagens. Uma interface de usuário de seleção alternativos de linha é fornecer um botão de opção ou a caixa de seleção em cada linha em GridView. Neste tutorial, examinamos como adicionar uma coluna de botões de opção.

Infelizmente, adicionando uma coluna de rádio botões não como direta ou simples, como se pode esperar. Não há nenhum RadioButtonField interno que pode ser adicionado com o clique de um botão e usando o controle RadioButton Web em um TemplateField introduz seu próprio conjunto de problemas. No final, para fornecer tal interface é necessário criar um personalizado `DataControlField` classe ou recorrer a injeção de HTML apropriado em um TemplateField durante o `RowCreated` evento.

Tendo explorados como adicionar uma coluna de botões de opção que voltar nossa atenção para adicionar uma coluna de caixas de seleção. Com uma coluna de caixas de seleção, um usuário pode selecionar uma ou mais linhas de GridView e, em seguida, executar alguma operação em todas as linhas selecionadas (como selecionar um conjunto de emails de um cliente de email com base na web e, em seguida, optar por excluir todos os emails selecionados). O tutorial Avançar veremos como adicionar uma coluna desse tipo.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi David Suru. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
[Próximo](adding-a-gridview-column-of-checkboxes-vb.md)
