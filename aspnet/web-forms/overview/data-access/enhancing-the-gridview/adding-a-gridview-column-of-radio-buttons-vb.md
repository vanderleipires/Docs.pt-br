---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
title: Adicionando uma coluna de GridView de botões de opção (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial explica como adicionar uma coluna de botões de opção em um controle GridView para fornecer ao usuário uma maneira mais intuitiva de selecionar uma única linha de...
ms.author: aspnetcontent
ms.date: 03/06/2007
ms.assetid: 2e31b60b-8723-4f14-b7ee-37859454dc3b
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
msc.type: authoredcontent
ms.openlocfilehash: 56aa37392ab51306112f934f8dbff4151f232d35
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835606"
---
<a name="adding-a-gridview-column-of-radio-buttons-vb"></a>Adicionando uma coluna de GridView de botões de opção (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_VB.exe) ou [baixar PDF](adding-a-gridview-column-of-radio-buttons-vb/_static/datatutorial51vb1.pdf)

> Este tutorial aborda como adicionar uma coluna de botões de opção em um controle GridView para fornecer ao usuário uma maneira mais intuitiva de selecionar uma única linha de GridView.


## <a name="introduction"></a>Introdução

O controle GridView oferece muita funcionalidade interna. Ele inclui um número de campos diferentes para a exibição de texto, imagens, hiperlinks e botões. Ele dá suporte a modelos para personalização adicional. Com alguns cliques do mouse, ela é possível fazer um GridView onde cada linha podem ser selecionada por meio de um botão ou para habilitar a edição ou exclusão de recursos. Apesar da grande quantidade de recursos fornecidos, normalmente haverá situações em que adicionais, os recursos sem suporte serão necessário a ser adicionado. Este tutorial e as próximas duas examinaremos como aprimorar a funcionalidade de s GridView para incluir recursos adicionais.

Este tutorial e o outro se concentrar em melhorar o processo de seleção de linha. Como examinado na [mestre/detalhes usando um GridView mestre selecionável com um Details DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md), podemos adicionar um CommandField a GridView que inclui um botão de seleção. Quando clicado, um postback massacre e o GridView s `SelectedIndex` propriedade é atualizada para o índice da linha cujo selecione botão foi clicado. No [mestre/detalhes usando um GridView mestre selecionável com um Details DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) tutorial vimos como usar esse recurso para exibir os detalhes para a linha selecionada de GridView.

Enquanto o botão de seleção funciona em muitas situações, ele pode não funcionar bem para outras pessoas. Em vez de usar um botão, os dois outros elementos de interface do usuário normalmente são usados para seleção: o botão de opção e a caixa de seleção. Podemos pode ampliar o GridView para que, em vez de um botão de seleção, cada linha contém um botão de opção ou a caixa de seleção. Em cenários em que o usuário pode selecionar apenas um dos registros de GridView, o botão de opção pode ser preferível o botão de seleção. Em situações em que o usuário pode potencialmente selecionar vários registros, como em um aplicativo de email baseado na web, em que um usuário talvez queira selecionar várias mensagens para a caixa de seleção Excluir oferece funcionalidade que não está disponível no botão de seleção ou botão de opção interfaces do usuário.

Este tutorial aborda como adicionar uma coluna de botões de opção ao GridView. O tutorial continuar explora o uso de caixas de seleção.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Etapa 1: Criando o Aprimorando as páginas da Web de GridView

Antes de começarmos a aprimorar o GridView para incluir uma coluna de botões de opção, permitem que s levar alguns instantes para criar as páginas do ASP.NET em nosso projeto de site que precisaremos para este tutorial e as próximas duas pela primeira vez. Comece adicionando uma nova pasta chamada `EnhancedGridView`. Em seguida, adicione as seguintes páginas do ASP.NET para essa pasta, certificando-se de associar cada página com o `Site.master` página mestra:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Adicione as páginas do ASP.NET para que os tutoriais relacionados SqlDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.gif)

**Figura 1**: adicionar as páginas do ASP.NET para que os tutoriais relacionados SqlDataSource


Como em outras pastas `Default.aspx` no `EnhancedGridView` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicionar esse controle de usuário `Default.aspx` arrastando-no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.png)

**Figura 2**: Adicione o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.png))


Por fim, adicione esses quatro páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação depois usando o controle SqlDataSource `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para a edição, inserindo e excluindo tutoriais.


![O mapa do Site agora inclui entradas para a aprimorar os tutoriais de GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.gif)

**Figura 3**: O mapa do Site agora inclui entradas para a aprimorar os tutoriais de GridView


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Etapa 2: Exibindo os fornecedores em um GridView

Para este tutorial deixe s compilar um GridView que lista os fornecedores dos EUA, com cada linha de GridView, fornecendo um botão de opção. Depois de selecionar um fornecedor por meio do botão de opção, o usuário poderá exibir os produtos do fornecedor, clicando em um botão. Enquanto essa tarefa pode parecer trivial, há um número de sutilezas que o tornam especialmente delicado. Antes de nos aprofundarmos nesses sutilezas, permitem que s primeiro obter um GridView listando os fornecedores.

Comece abrindo o `RadioButtonField.aspx` página o `EnhancedGridView` pasta arrastando um GridView da caixa de ferramentas para o Designer. Definir o s GridView `ID` para `Suppliers` e, na marca inteligente, de optar por criar uma nova fonte de dados. Especificamente, crie um ObjectDataSource denominado `SuppliersDataSource` que efetua pull de seus dados a partir de `SuppliersBLL` objeto.


[![Criar um novo ObjectDataSource chamado SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.png)

**Figura 4**: criar um novo ObjectDataSource nomeado `SuppliersDataSource` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.png))


[![Configurar o ObjectDataSource para usar a classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.png)

**Figura 5**: configurar o ObjectDataSource para usar o `SuppliersBLL` classe ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.png))


Como queremos listar esses fornecedores nos EUA, escolha o `GetSuppliersByCountry(country)` método na lista suspensa na guia SELECT.


[![Configurar o ObjectDataSource para usar a classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.png)

**Figura 6**: configurar o ObjectDataSource para usar o `SuppliersBLL` classe ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.png))


Na atualização guia, selecione (nenhum) opção e clique em Avançar.


[![Configurar o ObjectDataSource para usar a classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.png)

**Figura 7**: configurar o ObjectDataSource para usar o `SuppliersBLL` classe ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.png))


Uma vez que o `GetSuppliersByCountry(country)` método aceita um parâmetro, o Assistente Configurar fonte de dados nos solicita a origem desse parâmetro. Para especificar um valor codificado (EUA, neste exemplo), deixe o parâmetro de origem na lista suspensa definida como None e insira o valor padrão na caixa de texto. Clique em Concluir para concluir o assistente.


[![Use a USA como o valor padrão para o parâmetro de país](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.png)

**Figura 8**: Use EUA como o valor padrão para o `country` parâmetro ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.png))


Depois de concluir o assistente, o GridView incluirá um BoundField para cada um dos campos de dados do fornecedor. Remover tudo, exceto os `CompanyName`, `City`, e `Country` BoundFields e renomeie o `CompanyName` BoundFields `HeaderText` propriedade ao fornecedor. Depois de fazer isso, a sintaxe declarativa GridView e ObjectDataSource deve ser semelhante ao seguinte.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample2.aspx)]

Para este tutorial, deixe s permitir ao usuário exibir o fornecedor selecionado os produtos na mesma página de lista de fornecedores ou em uma página diferente. Para acomodar isso, adicione dois controles da Web de botão para a página. Eu ve conjunto a `ID` s desses dois botões para `ListProducts` e `SendToProducts`, com a ideia de que, quando `ListProducts` for clicado um postback ocorrerá e os produtos do fornecedor selecionado s serão listados na mesma página, mas quando `SendToProducts` é clicado, o usuário será ser movido para uma outra página que lista os produtos.

A Figura 9 mostra o `Suppliers` GridView e na Web de botão dois controles quando visualizado por meio de um navegador.


[![Esses fornecedores dos EUA têm seu nome, cidade e país informações listadas](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.png)

**Figura 9**: os fornecedores do EUA têm o nome, cidade e país informações listadas ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Etapa 3: Adicionando uma coluna de botões de opção

Neste ponto o `Suppliers` GridView tem três BoundFields exibindo o nome da empresa, cidade e país de cada fornecedor nos EUA. Ele ainda não existe uma coluna de botões de opção, no entanto. Infelizmente, o t do GridView inclui um interno RadioButtonField, caso contrário, poderia simplesmente adicioná-lo à grade e ser feitas. Em vez disso, podemos adicionar um TemplateField e configurar seu `ItemTemplate` para renderizar um botão de opção, resultando em um botão de opção para cada linha de GridView.

Inicialmente, podemos pode pressupor que a interface do usuário desejado pode ser implementada adicionando um controle RadioButton Web para o `ItemTemplate` de um TemplateField. Enquanto isso, de fato, adicionará um único botão de opção para cada linha de GridView, os botões de opção não podem ser agrupados e, portanto, não são mutuamente exclusivos. Ou seja, um usuário final é capaz de selecionar vários botões de opção simultaneamente de GridView.

Mesmo que usar um TemplateField de controles da Web de botão de opção não oferece a funcionalidade que precisamos, let s implementar essa abordagem, pois ele s que vale a pena examinar o motivo pelo qual os botões de opção resultantes não estão agrupados. Comece adicionando um TemplateField para fornecedores GridView, tornando-o campo mais à esquerda. Em seguida, da GridView s marca inteligente, clique no link Editar modelos e arraste um controle de Web de botão de opção da caixa de ferramentas para o s TemplateField `ItemTemplate` (veja a Figura 10). Definir o s RadioButton `ID` propriedade para `RowSelector` e o `GroupName` propriedade `SuppliersGroup`.


[![Adicionar um controle de Web de botão de opção para o ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.png)

**Figura 10**: adicionar um controle de Web de botão de opção para o `ItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.png))


Depois de fazer essas adições por meio do Designer, sua marcação de s GridView deve ser semelhante ao seguinte:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample3.aspx)]

O s RadioButton [ `GroupName` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) é o que é usado para agrupar uma série de botões de opção. Todos os controles de botão de opção com o mesmo `GroupName` valor são consideradas agrupados; apenas um botão de opção pode ser selecionado de um grupo por vez. O `GroupName` propriedade especifica o valor para o botão de opção renderizado s `name` atributo. O navegador examina os botões de opção `name` atributos para determinar o rádio botão agrupamentos.

Com o controle de Web de botão de opção adicionado para o `ItemTemplate`, visite esta página por meio de um navegador e clicar nos botões de rádio nas linhas de grade s. Observe como os botões de opção não são agrupados, tornando possível selecionar todas as linhas, como a Figura 11 mostra.


[![Os botões de opção s GridView não são agrupadas](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.png)

**Figura 11**: botões de opção de s. O controle GridView são agrupados não ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.png))


O motivo pelo qual os botões de opção não estão agrupados é porque seus renderizado `name` atributos são diferentes, apesar de ter o mesmo `GroupName` configuração da propriedade. Para ver essas diferenças, não uma fonte de exibição/do navegador e examinar a marcação de botão de opção:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample4.html)]

Observe como os dois os `name` e `id` atributos não são os valores exatos, conforme especificado na janela Propriedades, mas são prefixados com um número de outros `ID` valores. Adicional `ID` valores adicionados para a frente da renderizado `id` e `name` atributos são os `ID` s do rádio botões controles pai a `GridViewRow` s `ID` s, o GridView s `ID`, o S de controle de conteúdo `ID`e o formulário da Web s `ID`. Eles `ID` s são adicionados para que renderizados cada um controle de Web GridView tem uma única `id` e `name` valores.

Renderizados cada um controle necessidades outro `name` e `id` porque isso é como o navegador identifica exclusivamente cada controle no lado do cliente e como ele identifica para o servidor web que ação ou alteração ocorreu em um postback. Por exemplo, imagine que queremos executar algum código do lado do servidor sempre que um botão de opção s verificada estado foi alterado. Poderia fazer isso, definindo o s RadioButton `AutoPostBack` propriedade para `True` e criar um manipulador de eventos para o `CheckChanged` eventos. No entanto, se o renderizado `name` e `id` os valores para todos os botões de rádio eram os mesmos no postback não foi possível determinar quais específicas RadioButton foi clicado.

O é que não podemos criar uma coluna de botões de opção em um GridView usando o controle de Web de botão de opção. Em vez disso, podemos deve usar técnicas arcaicos em vez disso, para garantir que a marcação apropriada é injetada em cada linha de GridView.

> [!NOTE]
> Como o controle de Web de botão de opção, o controle HTML, quando adicionado a um modelo de botão de opção incluirá o exclusivo `name` atributo, tornando os botões de opção da grade não agrupados. Se você não estiver familiarizado com os controles HTML, à vontade ignorar essa observação, como controles HTML são usados raramente, especialmente no ASP.NET 2.0. Mas se você estiver interessado em saber mais, consulte [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) entrada de blog s [controles da Web e controles HTML](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Usando um controle Literal para injetar a marcação de botão de rádio

Para agrupar todos os botões de opção em GridView corretamente, precisamos inserir manualmente a marcação de botões de rádio para o `ItemTemplate`. Cada botão de opção precisa que o mesmo `name` de atributo, mas deve ter um único `id` (no caso que desejamos acessar um botão de opção por meio de script do lado do cliente) do atributo. Depois que um usuário seleciona um botão de opção e postagens de volta a página, o navegador enviará de volta o valor do botão de opção selecionado s `value` atributo. Portanto, cada botão de opção terá um único `value` atributo. Por fim, em um postback, precisamos tornar-se de adicionar o `checked` atributo para o um botão de opção selecionado, caso contrário, depois que o usuário faz uma seleção e postagens de volta, os botões de opção retornará ao seu estado padrão (todos os não selecionados).

Há duas abordagens que podem ser executadas para injetar a marcação de nível baixo em um modelo. Uma é fazer uma combinação de marcação e chamadas para métodos definidos na classe code-behind de formatação. Primeiro, essa técnica foi discutida na [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial. Em nosso caso, ele pode parecer algo como:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample5.aspx)]

Aqui, `GetUniqueRadioButton` e `GetRadioButtonValue` seria métodos definidos na classe code-behind que retornado apropriado `id` e `value` valores para cada botão de opção de atributo. Essa abordagem funciona bem para atribuir a `id` e `value` atributos, mas fica aquém quando precisar especificar o `checked` valor do atributo porque a sintaxe de associação de dados é executada apenas quando dados pela primeira vez são associados a GridView. Portanto, se o GridView tiver habilitado o estado de exibição, os métodos de formatação só serão acionado quando a página é carregada pela primeira vez (ou quando o GridView é reassociado explicitamente à fonte de dados) e, portanto, a função que define o `checked` atributo ganhou um t ser chamado em postback. -S um problema bastante sutil e um pouco além do escopo deste artigo, então deixarei isso. No entanto, eu fazer encorajá-lo para tentar usar a abordagem acima e funcionam por meio para o ponto em que você vai ficar preso. Embora tal um t exercício ganhou obter deixa mais próximo para uma versão de trabalho, ele ajuda a estimular uma compreensão mais profunda de GridView e o ciclo de vida de associação de dados.

A outra abordagem personalizada de injeção, marcação de nível baixo em um modelo e a abordagem que usaremos para este tutorial é adicionar um [controle Literal](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) ao modelo. Em seguida, no s GridView `RowCreated` ou `RowDataBound` manipulador de eventos, o controle Literal pode ser acessado por meio de programação e seu `Text` propriedade definida como a marcação para emitir.

Inicie removendo o RadioButton de s TemplateField `ItemTemplate`, substituindo-o por um controle Literal. Defina o controle Literal s `ID` para `RadioButtonMarkup`.


[![Adicionar um controle Literal para o ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.png)

**Figura 12**: adicionar um controle Literal para o `ItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.png))


Em seguida, crie um manipulador de eventos para o s GridView `RowCreated` eventos. O `RowCreated` evento é acionado para cada linha adicionada, ou não os dados é que está sendo ligado novamente para o GridView. Isso significa que, mesmo em um postback quando os dados são recarregados do estado de exibição, o `RowCreated` evento ainda é disparado e esse é o motivo pelo qual estamos usando em vez da `RowDataBound` (o que é acionado somente quando os dados é explicitamente associados aos dados de controle da Web).

No manipulador de eventos, só queremos continuar se podemos re lidar com uma linha de dados. Para cada linha de dados que desejamos referenciar programaticamente os `RadioButtonMarkup` controle Literal e defina seu `Text` propriedade para a marcação para emitir. Como mostra o código a seguir, a marcação emitida cria um rádio botão cuja `name` atributo é definido como `SuppliersGroup`, cujo `id` atributo é definido como `RowSelectorX`, onde *X* é o índice da linha GridView, e cujo `value` atributo é definido como o índice da linha GridView.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample6.vb)]

Quando uma linha de GridView está selecionada e ocorre um postback, estamos interessados no `SupplierID` do fornecedor selecionado. Portanto, é possível imaginar que o valor de cada botão de opção deve ser verdadeira `SupplierID` (em vez do índice da linha GridView). Embora isso possa funcionar em certas circunstâncias, seria um risco à segurança cegamente aceitar e processar um `SupplierID`. Nosso GridView, por exemplo, lista apenas esses fornecedores nos EUA. No entanto, se o `SupplierID` é passado diretamente usando o botão de opção, Novidades para interromper um usuário mal intencionado de manipular o `SupplierID` valor enviado de volta no postback? Usando o índice de linha como o `value`e, em seguida, obtendo o `SupplierID` em um postback da `DataKeys` coleção, podemos garantir que o usuário está usando apenas um do `SupplierID` valores associados a uma das linhas GridView.

Depois de adicionar esse código de manipulador de eventos, reserve um minuto para testar a página em um navegador. Primeiro, observe que apenas uma opção botão na grade pode ser selecionado por vez. No entanto, quando selecionar um botão de opção e clicar em um dos botões, ocorre um postback e todos os botões de opção voltarão ao estado inicial (ou seja, no postback, o botão de opção selecionado não será mais selecionado). Para corrigir isso, é preciso aumentar o `RowCreated` manipulador de eventos para que ele inspeciona o índice do botão de opção selecionado enviado do postback e adiciona o `checked="checked"` à marcação emitida as correspondências de índice de linha de atributo.

Quando ocorre um postback, o navegador envia de volta a `name` e `value` do botão de opção selecionado. O valor pode ser recuperado por meio de programação usando `Request.Form("name")`. O [ `Request.Form` propriedade](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) fornece uma [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) que representam as variáveis de formulário. As variáveis de formulário são os nomes e valores dos campos de formulário na página da web e são enviadas novamente pelo navegador da web sempre que um postback massacre. Porque o renderizado `name` é de atributo dos botões de opção em um GridView `SuppliersGroup`, quando a página da web é lançada de volta o navegador enviará `SuppliersGroup=valueOfSelectedRadioButton` volta para o servidor web (juntamente com os outros campos de formulário). Essas informações podem ser acessadas depois do `Request.Form` propriedade usando: `Request.Form("SuppliersGroup")`.

Desde que criamos será necessário para determinar o botão de opção selecionado indexar não apenas na `RowCreated` manipulador de eventos, mas na `Click` let s de manipuladores de eventos para os controles da Web de botão, adicione um `SuppliersSelectedIndex` propriedade para a classe code-behind que retorna `-1`se nenhum botão de opção foi selecionada e o índice selecionado se um dos botões de opção é selecionado.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample7.vb)]

Com essa propriedade foi adicionada, sabemos para adicionar o `checked="checked"` marcação em de `RowCreated` manipulador de eventos quando `SuppliersSelectedIndex` é igual a `e.Row.RowIndex`. Atualize o manipulador de eventos para incluir essa lógica:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample8.vb)]

Com essa alteração, o botão de opção selecionado permanece selecionado depois de um postback. Agora que temos a capacidade de especificar qual botão de opção é selecionado, podemos poderia mudar o comportamento para que quando a página foi visitada pela primeira vez, o primeiro botão de opção de linha s GridView foi selecionado (em vez de ter que não há botões de opção selecionada por padrão, que é atual comportamento). Para que o primeiro botão de opção selecionado por padrão, basta alterar o `If SuppliersSelectedIndex = e.Row.RowIndex Then` instrução para o seguinte: `If SuppliersSelectedIndex = e.Row.RowIndex OrElse (Not Page.IsPostBack AndAlso e.Row.RowIndex = 0) Then`.

Neste ponto, adicionamos uma coluna de botões agrupados para o GridView que permite que uma única linha de GridView sejam selecionadas e memorizados entre postbacks. Nossos próximos passos são para exibir os produtos fornecidos pelo fornecedor selecionado. Na etapa 4, veremos como redirecionar o usuário para outra página, enviando ao longo do selecionado `SupplierID`. Na etapa 5, veremos como exibir os produtos do fornecedor selecionado em um GridView na mesma página.

> [!NOTE]
> Em vez de usar o TemplateField (o foco neste demorada etapa 3), poderíamos criar um personalizado `DataControlField` classe que renderiza a interface de usuário apropriada e a funcionalidade. O [ `DataControlField` classe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) é a classe base da qual derivam o BoundField, CheckBoxField, TemplateField e outros campos internos de GridView e DetailsView. Criar um personalizado `DataControlField` classe significaria que a coluna de botões de opção pode ser adicionada usando apenas a sintaxe declarativa e também tornaria replicam a funcionalidade em outras páginas da web e outros aplicativos da web muito mais fácil.


Se você ve já criado personalizado, compilado controles do ASP.NET, no entanto, você sabe que isso requer uma quantidade razoável de trabalho externo e carrega um host de sutilezas e casos de borda que devem ser tratados com cuidado. Portanto, podemos será abrem mão da implementação de uma coluna de botões de opção como um personalizado `DataControlField` classe por enquanto e fique com a opção TemplateField. Talvez, teremos a oportunidade de explorar a criação, usando e implantando personalizadas `DataControlField` classes em um tutorial futuro!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Etapa 4: Exibindo produtos s fornecedor selecionado em uma página separada

Depois que o usuário selecionou uma linha de GridView, precisamos mostrar os produtos de fornecedor selecionado. Em algumas circunstâncias, podemos querer exibir esses produtos em uma página separada, em outros que nós talvez prefira fazê-lo na mesma página. Permitir que o s primeiro examinar como exibir os produtos em uma página separada; na etapa 5, examinaremos adicionando um GridView para `RadioButtonField.aspx` para exibir os produtos do fornecedor selecionado s.

Atualmente, há dois controles da Web de botão na página `ListProducts` e `SendToProducts`. Quando o `SendToProducts` botão é clicado, queremos enviar ao usuário `~/Filtering/ProductsForSupplierDetails.aspx`. Esta página foi criada na [filtragem de mestre/detalhes em duas páginas](../masterdetail/master-detail-filtering-across-two-pages-vb.md) tutorial e exibe os produtos para o fornecedor cujas `SupplierID` é passado por meio do campo querystring chamado `SupplierID`.

Para fornecer essa funcionalidade, crie um manipulador de eventos para o `SendToProducts` botão s `Click` eventos. Na etapa 3, adicionamos o `SuppliersSelectedIndex` propriedade, que retorna o índice da linha cujo botão de opção está selecionada. Correspondente `SupplierID` podem ser recuperados de s GridView `DataKeys` coleção e o usuário podem ser enviados ao `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` usando `Response.Redirect("url")`.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample9.vb)]

Esse código funciona brilhante, desde que um dos botões de opção é selecionado do GridView. Se, inicialmente, o GridView não tem qualquer botões de opção selecionado, e o usuário clica o `SendToProducts` botão, `SuppliersSelectedIndex` serão `-1`, que fará com que uma exceção seja lançada desde `-1` está fora do intervalo de índice da `DataKeys`coleção. Isso é não é uma preocupação, no entanto, se você decidiu atualizar o `RowCreated` manipulador de eventos, conforme discutido na etapa 3 para que o primeiro botão de opção no GridView selecionado inicialmente.

Para acomodar uma `SuppliersSelectedIndex` valor de `-1`, adicione um controle de Web de rótulo para a página acima GridView. Definido seu `ID` propriedade para `ChooseSupplierMsg`, sua `CssClass` propriedade a ser `Warning`, sua `EnableViewState` e `Visible` propriedades a serem `False`e seu `Text` propriedade tente escolher um fornecedor da grade. A classe CSS `Warning` exibe o texto em uma fonte vermelha, itálico, negrito, grande e é definido em `Styles.css`. Definindo o `EnableViewState` e `Visible` propriedades a serem `False`, o rótulo não é renderizado, exceto para somente os postbacks onde o controle s `Visible` propriedade é definida por meio de programação como `True`.


[![Adicionar um controle de Web de rótulo acima GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image21.png)

**Figura 13**: adicionar um rótulo da Web controle acima. o controle GridView ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image22.png))


Em seguida, aumentar o `Click` manipulador de eventos para exibir o `ChooseSupplierMsg` rotular se `SuppliersSelectedIndex` é menor que zero e redirecionar o usuário para `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` caso contrário.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample10.vb)]

Visite a página em um navegador e clique no `SendToProducts` botão antes de selecionar um fornecedor de GridView. Como mostra a Figura 14, exibe o `ChooseSupplierMsg` rótulo. Em seguida, selecione um fornecedor e clique no `SendToProducts` botão. Isso será whisk você a uma página que lista os produtos fornecidos pelo fornecedor selecionado. A Figura 15 mostra o `ProductsForSupplierDetails.aspx` página quando o fornecedor Cervejaria pé grande foi selecionado.


[![O rótulo de ChooseSupplierMsg será exibido se o fornecedor não está selecionada](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image23.png)

**Figura 14**: O `ChooseSupplierMsg` rótulo será exibido se o fornecedor não está selecionado ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image24.png))


[![Os produtos do fornecedor selecionado são exibidos no ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image25.png)

**Figura 15**: O fornecedor selecionado os produtos são exibidos no `ProductsForSupplierDetails.aspx` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Etapa 5: Exibindo produtos s fornecedor selecionado na mesma página

Na etapa 4, vimos como enviar o usuário para outra página da web para exibir o fornecedor selecionado os produtos. Como alternativa, os produtos do fornecedor selecionado s podem ser exibidos na mesma página. Para ilustrar isso, vamos adicionar outro GridView para `RadioButtonField.aspx` para exibir os produtos do fornecedor selecionado s.

Como só queremos esse GridView de produtos para exibir quando um fornecedor tiver sido selecionado, adicione um controle de painel Web sob o `Suppliers` GridView, definindo seu `ID` para `ProductsBySupplierPanel` e sua `Visible` propriedade para `False`. Dentro do painel, adicione o texto produtos do fornecedor selecionado, seguido por um GridView chamado `ProductsBySupplier`. Da GridView s marca inteligente, escolha vinculá-la a um novo ObjectDataSource chamado `ProductsBySupplierDataSource`.


[![Associar o ProductsBySupplier GridView a um novo ObjectDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image27.png)

**Figura 16**: associar o `ProductsBySupplier` GridView para um novo ObjectDataSource ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image28.png))


Em seguida, configure o ObjectDataSource para usar o `ProductsBLL` classe. Como queremos recuperar esses produtos fornecidos pelo fornecedor selecionado, especifique que o ObjectDataSource deve invocar o `GetProductsBySupplierID(supplierID)` método para recuperar seus dados. Selecione (nenhum) nas listas suspensos em UPDATE, INSERT e excluir guias.


[![Configurar o ObjectDataSource para usar o método GetProductsBySupplierID(supplierID)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image29.png)

**Figura 17**: configurar o ObjectDataSource para usar o `GetProductsBySupplierID(supplierID)` método ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image30.png))


[![Definir as listas suspensas para (nenhum) na atualização, inserção e excluir guias](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image31.png)

**Figura 18**: defina a lista suspensa como (nenhum) no UPDATE, INSERT e excluir guias ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image32.png))


Depois de configurar o selecione, atualizar, inserir e excluir guias, clique em Avançar. Uma vez que o `GetProductsBySupplierID(supplierID)` método espera um parâmetro de entrada, o assistente Criar fonte de dados nos solicita para especificar a origem para o valor do parâmetro s.

Temos algumas opções aqui em especificando a origem do valor do parâmetro de s. Poderíamos usar o objeto de parâmetro padrão e, por meio de programação, atribua o valor da `SuppliersSelectedIndex` propriedade no parâmetro s `DefaultValue` propriedade em s ObjectDataSource `Selecting` manipulador de eventos. Voltar para o [definir programaticamente o valores do parâmetro ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb.md) tutorial para relembrar programaticamente atribuir valores para os parâmetros do ObjectDataSource s.

Como alternativa, podemos usar um ControlParameter e consulte a `Suppliers` s GridView [ `SelectedValue` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (consulte a Figura 19). O s GridView `SelectedValue` propriedade retorna o `DataKey` valor que corresponde a [ `SelectedIndex` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Em ordem para essa opção funcione, precisamos definir programaticamente a s GridView `SelectedIndex` propriedade a ser selecionado de linha quando o `ListProducts` botão é clicado. Como um benefício extra, definindo o `SelectedIndex`, o registro selecionado entrarão na `SelectedRowStyle` definidos na `DataWebControls` tema (um plano de fundo amarelo).


[![Use um ControlParameter para especificar o GridView s SelectedValue como a origem do parâmetro](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image33.png)

**Figura 19**: Use um ControlParameter para especificar o s GridView SelectedValue como a origem do parâmetro ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image34.png))


Após a conclusão do assistente, o Visual Studio adicionará automaticamente os campos para os campos de dados do produto s. Remover tudo, exceto os `ProductName`, `CategoryName`, e `UnitPrice` BoundFields e altere o `HeaderText` propriedades preço, categoria e produto. Configurar o `UnitPrice` BoundField para que seu valor é formatado como uma moeda. Depois de fazer essas alterações, o painel, GridView e ObjectDataSource s marcação declarativa deve ser semelhante ao seguinte:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample11.aspx)]

Para concluir este exercício, precisamos definir o s GridView `SelectedIndex` propriedade para o `SelectedSuppliersIndex` e o `ProductsBySupplierPanel` painel s `Visible` propriedade a ser `True` quando o `ListProducts` botão é clicado. Para fazer isso, crie um manipulador de eventos para o `ListProducts` controle de Web de botão s `Click` eventos e adicione o seguinte código:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample12.vb)]

Se um fornecedor não tiver sido selecionado de GridView, o `ChooseSupplierMsg` rótulo é exibido e o `ProductsBySupplierPanel` ocultado do painel. Caso contrário, se um fornecedor tiver sido selecionado, o `ProductsBySupplierPanel` é exibida e o GridView s `SelectedIndex` propriedade é atualizada.

Figura 20 mostra os resultados depois que o fornecedor Cervejaria pé grande foi selecionado e os produtos de mostrar no botão de página foi clicado.


[![Os produtos fornecidos pela Cervejaria pé grande são listados na mesma página](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image35.png)

**Figura 20**: os produtos fornecidos pela Cervejaria pé grande são listados na mesma página ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-vb/_static/image36.png))


## <a name="summary"></a>Resumo

Conforme discutido no [mestre/detalhes usando um GridView mestre selecionável com um Details DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) tutorial registros podem ser selecionados de um GridView usando um CommandField cujo `ShowSelectButton` estiver definida como `True`. Mas o CommandField exibirá seus botões como imagens, botões de envio regulares ou links. Uma interface do usuário de seleção de linha alternativa é fornecer um botão de opção ou a caixa de seleção em cada linha de GridView. Neste tutorial, examinamos como adicionar uma coluna de botões de opção.

Infelizmente, adicionando uma coluna de t de não é de botões de rádio como direta ou simples, como se pode esperar. Não há nenhum RadioButtonField internos que pode ser adicionado com o clique de um botão e usando o controle de Web de botão de opção em um TemplateField introduz seu próprio conjunto de problemas. No final, para fornecer uma interface desse tipo também temos que criar um personalizado `DataControlField` classe ou recorrer a injetar o HTML apropriado em um TemplateField durante o `RowCreated` eventos.

Ter explorado como adicionar uma coluna de botões de opção, fale com a voltar nossas atenções para a adição de uma coluna de caixas de seleção. Com uma coluna de caixas de seleção, um usuário pode selecionar um ou mais linhas de GridView e, em seguida, executar alguma operação em todas as linhas selecionadas (como selecionar um conjunto de emails de um cliente de email baseado na web e, em seguida, optando por excluir todos os emails selecionados). No próximo tutorial, veremos como adicionar uma coluna desse tipo.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi David Suru. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
> [Próximo](adding-a-gridview-column-of-checkboxes-vb.md)
