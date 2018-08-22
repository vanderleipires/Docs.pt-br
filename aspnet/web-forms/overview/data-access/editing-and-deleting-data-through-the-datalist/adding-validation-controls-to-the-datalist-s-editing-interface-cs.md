---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
title: Adicionando controles de validação à DataList de edição de Interface (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como é fácil adicionar controles de validação ao EditItemTemplate do DataList para fornecer um int mais infalível de usuário de edição...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 3ecc21c5-da0e-40ab-abb4-fac1e47398ad
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 2fe85d6513a229f11b3aad7c7cc6c7124c94d70f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825614"
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-c"></a>Adicionando controles de validação à Interface de edição do DataList (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_CS.exe) ou [baixar PDF](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/datatutorial39cs1.pdf)

> Neste tutorial, veremos como é fácil adicionar controles de validação ao EditItemTemplate do DataList para fornecer uma interface de usuário de edição mais à prova de falhas.


## <a name="introduction"></a>Introdução

No DataList edição tutoriais até o momento, os DataLists interfaces de edição não incluíam qualquer validação de entrada do usuário proativo, mesmo que a entrada de usuário inválido, como um nome de produto ausente ou resultados de preço negativo em uma exceção. No [tutorial anterior](handling-bll-and-dal-level-exceptions-cs.md) examinamos como adicionar código à DataList s manipulação de exceção `UpdateCommand` manipulador de eventos para capturar e normalmente exibem informações sobre todas as exceções que foram gerados. O ideal é que, no entanto, a interface de edição inclui controles de validação para impedir que um usuário inserir esses dados inválidos em primeiro lugar.

Neste tutorial, veremos como é fácil adicionar controles de validação à DataList s `EditItemTemplate` para fornecer uma interface de usuário de edição mais infalível. Especificamente, este tutorial usa o exemplo criado no tutorial anterior e aumenta a interface de edição para incluir validação apropriado.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-csmd"></a>Etapa 1: Replicar o exemplo de[tratamento de exceções de nível BLL e DAL](handling-bll-and-dal-level-exceptions-cs.md)

No [tratamento BLL - e exceções de nível DAL](handling-bll-and-dal-level-exceptions-cs.md) tutorial, criamos uma página que são listados os nomes e os preços dos produtos em um DataList de duas colunas, editável. Nosso objetivo para este tutorial é para ampliar a interface de edição de s DataList para incluir controles de validação. Em particular, nossa lógica de validação será:

- Exigir que o nome do produto s ser fornecido
- Certifique-se de que o valor inserido para o preço é um formato de moeda válidos
- Certifique-se de que o valor inserido para o preço é maior que ou igual a zero, desde um negativo `UnitPrice` valor é ilegal

Antes que possamos observar aumentando o exemplo anterior para incluir a validação, primeiro precisamos replicar o exemplo do `ErrorHandling.aspx` página o `EditDeleteDataList` pasta para a página para este tutorial, `UIValidation.aspx`. Para fazer isso, precisamos copiar ambos o `ErrorHandling.aspx` página marcação declarativa de s e seu código-fonte. Primeiro copiar sobre a marcação declarativa, executando as seguintes etapas:

1. Abra o `ErrorHandling.aspx` página no Visual Studio
2. Vá para a marcação declarativa de página s (clique no botão Source na parte inferior da página)
3. Copie o texto dentro de `<asp:Content>` e `</asp:Content>` marcas (linhas 3 a 32), como mostrado na Figura 1.


[![Copie o texto dentro de &lt;asp: Content&gt; controle](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image1.png)

**Figura 1**: Copie o texto dentro de `<asp:Content>` controle ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image3.png))


1. Abra o `UIValidation.aspx` página
2. Vá para a marcação declarativa de s de página
3. Cole o texto dentro de `<asp:Content>` controle.

Para copiar o código-fonte, abra o `ErrorHandling.aspx.vb` da página e copiar apenas o texto *dentro* o `EditDeleteDataList_ErrorHandling` classe. Copie os manipuladores de eventos de três (`Products_EditCommand`, `Products_CancelCommand`, e `Products_UpdateCommand`) junto com o `DisplayExceptionDetails` método, mas não **não** copiar a declaração de classe ou `using` instruções. Colar o texto copiado *dentro de* as `EditDeleteDataList_UIValidation` classe `UIValidation.aspx.vb`.

Depois de mover sobre o conteúdo e o código de `ErrorHandling.aspx` para `UIValidation.aspx`, reserve um tempo para testar as páginas em um navegador. Você deve ver a mesma saída e experimentar a mesma funcionalidade em cada uma dessas duas páginas (consulte a Figura 2).


[![A página UIValidation.aspx imita a funcionalidade no ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image4.png)

**Figura 2**: O `UIValidation.aspx` página imita a funcionalidade no `ErrorHandling.aspx` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Etapa 2: Adicionar os controles de validação ao EditItemTemplate s DataList

Ao construir formulários de entrada de dados, é importante que os usuários insiram os campos necessários e que todas as suas entradas fornecidas são legais, formatado corretamente os valores. Para ajudar a garantir que as entradas de s um usuário sejam válidos, o ASP.NET fornece cinco controles de validação internas que são projetados para validar o valor de um único controle de Web de entrada:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garante que um valor foi fornecido
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valida um valor contra outro valor de controle de Web ou um valor constante ou garante que o formato do valor s é válido para um tipo de dados especificado
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garante que um valor está dentro de um intervalo de valores
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valida um valor em relação a um [expressão regular](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valida um valor em relação a um método personalizado definido pelo usuário

Para obter mais informações sobre esses cinco controles voltar para o [adicionando controles de validação para a edição e inserção de Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) tutorial ou check-out a [seção controles de validação](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) dos [Tutoriais de início rápido do ASP.NET](https://quickstarts.asp.net).

Para nosso tutorial precisaremos usar para garantir que um valor para o nome do produto foi fornecido um RequiredFieldValidator e um CompareValidator para garantir que o preço inserido tem um valor maior que ou igual a 0 e é apresentado em um formato de moeda válidos.

> [!NOTE]
> Embora o ASP.NET 1.x tivesse esses mesmos controles de validação de cinco, ASP.NET 2.0 adicionou uma série de aprimoramentos, principal sendo que o script do lado do cliente dois suportam para navegadores além do Internet Explorer e a capacidade de controles de validação de partição em uma página em grupos de validação. Para obter mais informações sobre os novos recursos de controle de validação no 2.0, consulte [dissecando os controles de validação no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Permitir que o s comece adicionando os controles de validação necessárias à DataList s `EditItemTemplate`. Essa tarefa pode ser executada por meio do Designer clicando no link Editar modelos da marca inteligente s DataList, ou através de sintaxe declarativa. Deixe s percorra o processo usando a opção de editar modelos de exibição de Design. Depois de optar por editar s DataList `EditItemTemplate`, adicione um RequiredFieldValidator, arrastando-a na caixa de ferramentas para a interface de edição de modelo, colocando-o após o `ProductName` caixa de texto.


[![Adicione um RequiredFieldValidator ao EditItemTemplate após a caixa de texto ProductName](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image7.png)

**Figura 3**: adicionar um RequiredFieldValidator para o `EditItemTemplate After` as `ProductName` caixa de texto ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image9.png))


Todos os controles de validação funcionam, validando a entrada de um único controle da Web do ASP.NET. Portanto, precisamos indicar que o RequiredFieldValidator que acabamos de adicionar deve validar em relação a `ProductName` caixa de texto; isso é feito definindo o controle de validação s [ `ControlToValidate` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) para o `ID` de o controle da Web apropriado (`ProductName`, nesta instância). Em seguida, defina as [ `ErrorMessage` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) para você deve fornecer o nome do produto s e o [ `Text` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) para \*. O `Text` valor da propriedade, se fornecido, é o texto exibido pelo controle de validação se a validação falhar. O `ErrorMessage` valor da propriedade, que é necessário, é usado pelo controle ValidationSummary; se o `Text` o valor da propriedade for omitido, o `ErrorMessage` valor da propriedade é exibido pelo controle de validação em uma entrada inválida.

Depois de definir essas três propriedades do RequiredFieldValidator, sua tela deve ser semelhante à Figura 4.


[![Defina as propriedades de texto, ErrorMessage e RequiredFieldValidator s ControlToValidate](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image10.png)

**Figura 4**: definir o s RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, e `Text` propriedades ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image12.png))


Com o RequiredFieldValidator adicionado para o `EditItemTemplate`, tudo o que resta fazer é adicionar a validação necessária para o preço do produto s caixa de texto. Uma vez que o `UnitPrice` é opcional ao editar um registro, podemos don t é necessário adicionar um RequiredFieldValidator. , No entanto, precisamos adicionar um CompareValidator para garantir que o `UnitPrice`, se fornecido, está formatado corretamente como uma moeda e é maior que ou igual a 0.

Adicionar CompareValidator para o `EditItemTemplate` e defina sua `ControlToValidate` propriedade a ser `UnitPrice`, sua `ErrorMessage` propriedade ao preço deve ser maior que ou igual a zero e não pode incluir o símbolo de moeda e seu `Text` propriedade para \*. Para indicar que o `UnitPrice` valor deve ser maior que ou igual a 0, defina o s CompareValidator [ `Operator` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) para `GreaterThanEqual`, sua [ `ValueToCompare` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) como 0, e sua [ `Type` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) para `Currency`.

Depois de adicionar esses dois controles de validação, DataList s `EditItemTemplate` sintaxe declarativa do s deve ser semelhante ao seguinte:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Depois de fazer essas alterações, abra a página em um navegador. Se você tentar se omitir o nome ou insira um valor de preço inválido ao editar um produto, um asterisco é exibido ao lado da caixa de texto. Como mostra a Figura 5, um valor de preço que inclui o símbolo de moeda como US $19,95 é considerado inválido. O s CompareValidator `Currency` `Type` permite separadores de dígitos (como vírgulas ou períodos, dependendo das configurações de cultura) e um sinal de mais à esquerda ou sinal de subtração, mas faz *não* permitir um símbolo de moeda. Esse comportamento pode perplex usuários conforme a interface de edição no momento, renderiza o `UnitPrice` usando o formato de moeda.


[![Um asterisco é exibido ao lado de caixas de texto com uma entrada inválida](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image13.png)

**Figura 5**: um asterisco é exibido próximo para as caixas de texto com uma entrada inválida ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image15.png))


Embora a funciona de validação como-está, o usuário deve remover manualmente o símbolo de moeda ao editar um registro que não é aceitável. Além disso, se houver inválido de entradas na edição interface nem a atualização nem cancelar botões, quando clicado, invocará um postback. O ideal é que o botão Cancelar retornaria DataList para seu estado de edição previamente, independentemente da validade de entradas do usuário de s. Além disso, precisamos garantir que os dados da página s seja válidos antes de atualizar as informações do produto no DataList s `UpdateCommand` manipulador de eventos, como os controles de validação lógica do lado do cliente pode ser ignorada por usuários cujos navegadores don dão suporte a t JavaScript ou tem o suporte foi desabilitado.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Removendo o símbolo de moeda de EditItemTemplate s UnitPrice TextBox

Ao usar o s CompareValidator `Currency``Type`, a entrada que está sendo validada não deve incluir quaisquer símbolos de moeda. A presença de tais símbolos faz com que o CompareValidator marcar a entrada como inválida. No entanto, nossa interface de edição atualmente, inclui um símbolo de moeda no `UnitPrice` TextBox, que significa que o usuário deve remover explicitamente o símbolo de moeda antes de salvar suas alterações. Para corrigir isso, temos três opções:

1. Configurar o `EditItemTemplate` para que o `UnitPrice` valor de caixa de texto não está formatado como uma moeda.
2. Permitir que o usuário insira um símbolo de moeda, removendo CompareValidator e substituí-la por um RegularExpressionValidator que verifica se há um valor de moeda formatada corretamente. O desafio aqui é que a expressão regular para validar um valor de moeda não é tão simples quanto CompareValidator e exigiria escrevendo código, se quiséssemos incorporar as configurações de cultura.
3. Remover completamente o controle de validação e contam com lógica de validação personalizada do lado do servidor em s GridView `RowUpdating` manipulador de eventos.

Deixe s com a opção 1 para este tutorial. Atualmente, o `UnitPrice` é formatado como um valor de moeda devido à expressão de associação de dados para a caixa de texto a `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Alterar o `Eval` instrução para `Eval("UnitPrice", "{0:n2}")`, que formata o resultado como um número com dois dígitos de precisão. Isso pode ser feito diretamente por meio da sintaxe declarativa ou clicando no link Editar DataBindings do `UnitPrice` caixa de texto no DataList s `EditItemTemplate`.

Com essa alteração, o preço formatado na interface de edição inclui vírgula como separador de grupo e um ponto como separador decimal, mas deixa desativar o símbolo de moeda.

> [!NOTE]
> Ao remover o formato de moeda da interface editável, considero útil para colocar o símbolo de moeda como texto fora da caixa de texto. Isso serve como uma dica para o usuário que não seja necessário fornecer o símbolo de moeda.


## <a name="fixing-the-cancel-button"></a>Corrigindo o botão Cancelar

Por padrão, os controles da Web de validação emitem JavaScript para executar a validação no lado do cliente. Quando um botão, LinkButton ou ImageButton é clicado, os controles de validação na página são verificados no lado do cliente antes que o postback ocorra. Se houver uma todos os dados inválidos, o postback será cancelado. Para determinados botões, no entanto, a validade dos dados pode ser irrelevante; Nesse caso, ter o postback cancelado devido a dados inválidos é um incômodo.

O botão Cancelar é um exemplo. Imagine que um usuário insere dados inválidos, como omitir o nome do produto s e, em seguida, decide she t deseja salvar o produto Afinal de contas e pressiona o botão Cancelar. Atualmente, o botão Cancelar dispara os controles de validação na página, que relatam que o nome do produto está ausente e impedir que o postback. Nosso usuário deve digitar um texto no `ProductName` caixa de texto apenas para cancele o processo de edição.

Felizmente, o botão, LinkButton e ImageButton tem um [ `CausesValidation` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) que pode indicar se clicar no botão deve iniciar a lógica de validação (o padrão é `True`). Definir o botão Cancelar s `CausesValidation` propriedade para `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Garantir que as entradas são válido no manipulador de eventos UpdateCommand

Pois o script do lado do cliente emitido por controles de validação, se um usuário insere uma entrada inválida os controles de validação Cancelar quaisquer postagens iniciadas pelo botão LinkButton, ou ImageButton controles cuja `CausesValidation` são propriedades `True` (o padrão). No entanto, se um usuário está visitando com um navegador antiquado ou cujo suporte a JavaScript foi desabilitado, as verificações de validação do lado do cliente não serão executado.

Todos os controles de validação ASP.NET Repita sua lógica de validação imediatamente após o postback e a validade geral das entradas s de página por meio de relatórios do [ `Page.IsValid` propriedade](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). No entanto, o fluxo de página não é interrompido ou interrompido em qualquer forma, com base no valor de `Page.IsValid`. Como desenvolvedores, é nossa responsabilidade para garantir que o `Page.IsValid` propriedade tem um valor de `True` antes de prosseguir com o código que pressupõe válido de dados de entrada.

Se um usuário tiver o JavaScript desabilitado, visita nossa página, edita um produto, entra em um valor de preço de muito caro e clica no botão de atualização, a validação do lado do cliente será ignorada e ocorrerá um postback. No postback, o ASP.NET página s `UpdateCommand` executa o manipulador de eventos e uma exceção é gerada ao tentar analisar muito caro para um `Decimal`. Já que temos de manipulação de exceção, essa exceção será ser manipulada com elegância, mas podemos pode impedir que os dados inválidos passando por em primeiro lugar ao continuar apenas com o `UpdateCommand` manipulador de eventos se `Page.IsValid` tem um valor de `True`.

Adicione o seguinte código ao início dos `UpdateCommand` manipulador de eventos, imediatamente antes do `Try` bloco:


[!code-csharp[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample2.cs)]

Com esse acréscimo, o produto tentará ser atualizada somente se os dados enviados são válidos. A maioria dos usuários ganharam t ser capaz de dados inválidos devido aos scripts de cliente de controles de validação de postback, mas os usuários cujos navegadores don dão suporte a t JavaScript ou que têm JavaScript dar suporte a desabilitado, podem ignorar as verificações do lado do cliente e enviar dados inválidos.

> [!NOTE]
> O leitor astuto deve se lembrar que quando a atualização de dados com o controle GridView, podemos t precisa verificar explicitamente o `Page.IsValid` propriedade na nossa classe de code-behind da página s. Isso ocorre porque a consulta de GridView de `Page.IsValid` propriedade para nós e só prossegue com a atualização somente se ele retorna um valor de `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Etapa 3: Problemas de entrada de dados de resumo

Além dos controles de validação de cinco, o ASP.NET inclui o [controle ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), que exibe o `ErrorMessage` s desses controles de validação que detectou dados inválidos. Esses dados de resumo podem ser exibidos como texto na página da web ou por meio de uma caixa de mensagem modal, no lado do cliente. Deixe o s aprimorar este tutorial para incluir uma caixa de mensagem do lado do cliente resumindo quaisquer problemas de validação.

Para fazer isso, arraste um controle ValidationSummary da caixa de ferramentas para o Designer. O local de t ValidationSummary controle importa, desde que criamos re vai configurá-lo para exibir somente o resumo como uma caixa de mensagem. Depois de adicionar o controle, defina suas [ `ShowSummary` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) à `False` e sua [ `ShowMessageBox` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) para `True`. Com esse acréscimo, erros de validação são resumidos em uma caixa de mensagem do lado do cliente (veja a Figura 6).


[![Os erros de validação são resumidos em uma caixa de mensagem do lado do cliente](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image16.png)

**Figura 6**: os erros de validação são resumidos em uma caixa de mensagem do lado do cliente ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image18.png))


## <a name="summary"></a>Resumo

Neste tutorial vimos como reduzir a probabilidade de exceções usando controles de validação para garantir proativamente nosso entradas de usuários são válidas antes de tentar usá-los no fluxo de trabalho atualização. O ASP.NET fornece cinco controles de Web de validação que são projetados para inspecionar uma Web específica s entradas de controle e relatam sobre a validade da entrada s. Neste tutorial usamos dois desses cinco controles o RequiredFieldValidator e CompareValidator para garantir que o nome do produto s foi fornecido e que o preço tinha um formato de moeda com um valor maior que ou igual a zero.

Adicionando controles de validação para o s DataList editando a interface é tão simple quanto arrastá-las para o `EditItemTemplate` da caixa de ferramentas e definir algumas propriedades. Por padrão, os controles de validação emitem automaticamente script de validação do lado do cliente; elas também fornecem validação do lado do servidor no postback, armazenando o resultado cumulativo no `Page.IsValid` propriedade. Para ignorar a validação do lado do cliente quando um botão, LinkButton ou ImageButton é clicado, defina o botão s `CausesValidation` propriedade para `False`. Além disso, antes de executar quaisquer tarefas com os dados enviados em um postback, certifique-se de que o `Page.IsValid` propriedade retorna `True`.

Todos DataList edição tutoriais podemos ve examinei até agora tenha tido interfaces de edição muito simples, uma caixa de texto para o nome do produto s e outro para o preço. A interface de edição, no entanto, pode conter uma mistura de diferentes controles de Web, como DropDownLists, calendários, botões de opção, caixas de seleção e assim por diante. Nosso próximo tutorial, examinaremos a criação de uma interface que usa uma variedade de controles da Web.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Dennis Patterson, Ken Pespisa e Liz Shulok. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](handling-bll-and-dal-level-exceptions-cs.md)
> [Próximo](customizing-the-datalist-s-editing-interface-cs.md)
