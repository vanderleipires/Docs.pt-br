---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
title: Adicionando controles de validação para o DataList da edição de Interface (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como é fácil adicionar controles de validação ao EditItemTemplate do DataList para fornecer um int de usuário de edição mais seguro.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 3ecc21c5-da0e-40ab-abb4-fac1e47398ad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 13582989aed60ec7949afcd683472546a91d171c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-c"></a>Adicionando controles de validação a Interface de edição do DataList (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_CS.exe) ou [baixar PDF](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/datatutorial39cs1.pdf)

> Neste tutorial, veremos como é fácil adicionar controles de validação ao EditItemTemplate do DataList para fornecer uma interface de usuário de edição mais segura.


## <a name="introduction"></a>Introdução

Em DataList edição tutoriais até o momento, os DataLists edição interfaces não incluídas nenhuma validação de entrada pró-ativo usuário mesmo que um usuário inválido de entrada como um nome de produto ausente ou resultados de preço negativo em uma exceção. No [tutorial anterior](handling-bll-and-dal-level-exceptions-cs.md) examinamos como adicionar código para o DataList s manipulação de exceção `UpdateCommand` manipulador de eventos para capturar e normalmente exibem informações sobre todas as exceções que foram gerados. Idealmente, no entanto, a interface de edição inclui controles de validação para impedir que um usuário inserir esses dados inválidos em primeiro lugar.

Este tutorial veremos como é fácil adicionar controles de validação para o DataList s `EditItemTemplate` para fornecer uma interface de usuário de edição mais segura. Especificamente, este tutorial usa o exemplo criado no tutorial anterior e aumenta a interface de edição para incluir validação apropriadas.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-csmd"></a>Etapa 1: Replicando o exemplo de[tratamento de exceções de nível BLL e DAL](handling-bll-and-dal-level-exceptions-cs.md)

No [tratamento BLL e exceções de nível de DAL](handling-bll-and-dal-level-exceptions-cs.md) tutorial criamos uma página que lista os nomes e preços de produtos em DataList duas colunas, editável. Nosso objetivo para este tutorial é aumentar a interface de edição de s DataList para incluir os controles de validação. Em particular, nossa lógica de validação será:

- Exigir que o nome do produto s ser fornecido
- Certifique-se de que o valor digitado para o preço é um formato de moeda válido
- Certifique-se de que o valor digitado para o preço é maior que ou igual a zero, pois um negativo `UnitPrice` valor é inválido

Antes, podemos ver aumentando o exemplo anterior para incluir validação, precisamos primeiro replicar o exemplo da `ErrorHandling.aspx` página o `EditDeleteDataList` pasta para a página para este tutorial, `UIValidation.aspx`. Para fazer isso, precisamos copiar em ambos os `ErrorHandling.aspx` página declarativo s e seu código-fonte. Primeiro copiar sobre a marcação declarativa executando as seguintes etapas:

1. Abra o `ErrorHandling.aspx` página no Visual Studio
2. Vá para a marcação de página s declarativa (clique no botão fonte na parte inferior da página)
3. Copie o texto dentro de `<asp:Content>` e `</asp:Content>` marcas (linhas de 3 a 32), como mostrado na Figura 1.


[![Copie o texto dentro de &lt;asp: Content&gt; controle](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image1.png)

**Figura 1**: copiar o texto dentro de `<asp:Content>` controle ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image3.png))


1. Abra o `UIValidation.aspx` página
2. Vá para a marcação declarativa de s de página
3. Cole o texto dentro de `<asp:Content>` controle.

Para copiar o código de origem, abra o `ErrorHandling.aspx.vb` página e copiar apenas o texto *em* o `EditDeleteDataList_ErrorHandling` classe. Copie os manipuladores de eventos de três (`Products_EditCommand`, `Products_CancelCommand`, e `Products_UpdateCommand`) junto com o `DisplayExceptionDetails` método, mas não **não** copiar a declaração de classe ou `using` instruções. Colar o texto copiado *em* o `EditDeleteDataList_UIValidation` classe em `UIValidation.aspx.vb`.

Depois de mover sobre o conteúdo e o código de `ErrorHandling.aspx` para `UIValidation.aspx`, reserve um tempo para testar as páginas em um navegador. Você deve ver a mesma saída e ter a mesma funcionalidade em cada uma dessas duas páginas (consulte a Figura 2).


[![A página UIValidation.aspx imita a funcionalidade no ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image4.png)

**Figura 2**: O `UIValidation.aspx` página imita a funcionalidade no `ErrorHandling.aspx` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Etapa 2: Adicionando os controles de validação para o DataList s EditItemTemplate

Quando estiver criando formulários de entrada de dados, é importante que os usuários insiram os campos necessários e todas as suas entradas fornecidas são valores válidos, formatado corretamente. Para ajudar a garantir que uma entrada de s do usuário é válidas, o ASP.NET fornece cinco controles de validação interna que são projetados para validar o valor de um controle de Web de entrada único:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garante que um valor foi fornecido
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valida um valor contra outro valor de controle de Web ou um valor constante ou garante que o formato do valor s é válido para um tipo de dados especificado
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garante que um valor está dentro do intervalo de valores
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valida um valor em relação a um [expressão regular](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valida um valor em relação a um método personalizado definido pelo usuário

Para obter mais informações sobre esses cinco controles se referem ao [adicionando controles de validação para a edição e inserção de Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) tutorial ou check-out de [seção controles de validação](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) da [ASP.NET tutoriais](https://quickstarts.asp.net).

Para nosso tutorial, precisará usar um RequiredFieldValidator para garantir que foi fornecido um valor para o nome do produto e um CompareValidator para garantir que o preço inserido tem um valor maior que ou igual a 0 e é apresentado em um formato de moeda válido.

> [!NOTE]
> Embora o ASP.NET 1. x tinha esses mesmos controles de validação de cinco, ASP.NET 2.0 adicionou uma série de melhorias, principal dois sendo script do lado do cliente oferecer suporte para navegadores além do Internet Explorer e a capacidade de controles de validação de partição em uma página em grupos de validação. Para obter mais informações sobre os novos recursos de controle de validação no 2.0, consulte [dissecando os controles de validação no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Permitem s comece adicionando os controles de validação necessárias à DataList s `EditItemTemplate`. Esta tarefa pode ser executada por meio do Designer, clicando no link Editar modelos de marca inteligente DataList s ou por meio de sintaxe declarativa. Permitem que a etapa s pelo processo usando a opção de editar modelos da exibição do Design. Depois de escolher Editar DataList s `EditItemTemplate`, adicionar um RequiredFieldValidator arrastando-o na caixa de ferramentas para a interface de edição de modelo, colocá-lo após o `ProductName` caixa de texto.


[![Adicionar um RequiredFieldValidator para o EditItemTemplate após a caixa de texto ProductName](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image7.png)

**Figura 3**: adicionar um RequiredFieldValidator para o `EditItemTemplate After` o `ProductName` caixa de texto ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image9.png))


Todos os controles de validação funcionam, validando a entrada de um único controle da Web do ASP.NET. Portanto, é necessário indicar que o RequiredFieldValidator que acabamos de adicionar deve validar em relação a `ProductName` caixa de texto; isso é feito definindo o controle de validação s [ `ControlToValidate` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) para o `ID` de o controle da Web apropriado (`ProductName`, neste exemplo). Em seguida, defina o [ `ErrorMessage` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) para você deve fornecer o nome do produto s e o [ `Text` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) para \*. O `Text` valor da propriedade, se fornecido, será o texto que é exibido pelo controle de validação se a validação falhar. O `ErrorMessage` valor da propriedade, que é necessário, é usado pelo controle ValidationSummary; se o `Text` o valor da propriedade for omitido, o `ErrorMessage` o valor da propriedade é exibido pelo controle de validação em uma entrada inválida.

Depois de definir essas três propriedades o RequiredFieldValidator, sua tela deve ser semelhante à Figura 4.


[![Definir propriedades de texto, RequiredFieldValidator s ControlToValidate e ErrorMessage](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image10.png)

**Figura 4**: definir a s RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, e `Text` propriedades ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image12.png))


Com o RequiredFieldValidator adicionado para o `EditItemTemplate`, todos os que permanece é adicionar a validação necessária para o preço do produto s caixa de texto. Desde que o `UnitPrice` é opcional ao editar um registro, podemos don precisa adicionar um RequiredFieldValidator. , No entanto, precisamos adicionar um CompareValidator para garantir que o `UnitPrice`, se fornecido, está corretamente formatado como uma moeda e é maior que ou igual a 0.

Adicionar CompareValidator no `EditItemTemplate` e defina seu `ControlToValidate` propriedade `UnitPrice`, sua `ErrorMessage` propriedade como o preço deve ser maior que ou igual a zero e não pode incluir o símbolo de moeda e sua `Text` propriedade \*. Para indicar que o `UnitPrice` valor deve ser maior que ou igual a 0, defina o s CompareValidator [ `Operator` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) para `GreaterThanEqual`, sua [ `ValueToCompare` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) como 0, e seu [ `Type` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) para `Currency`.

Depois de adicionar esses dois controles de validação, DataList s `EditItemTemplate` sintaxe declarativa s deve ser semelhante à seguinte:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Depois de fazer essas alterações, abra a página em um navegador. Se você tentar omitir o nome ou insira um valor inválido durante a edição de um produto, um asterisco é exibido ao lado da caixa de texto. Como mostra a Figura 5, um valor que inclui o símbolo de moeda como $19,95 é considerado inválido. O s CompareValidator `Currency` `Type` permite separadores de dígitos (como vírgulas ou pontos, dependendo das configurações de cultura) e um sinal de mais à esquerda ou sinal de subtração, mas *não* permite que um símbolo de moeda. Esse comportamento pode perplex usuários como a interface de edição atualmente renderiza o `UnitPrice` usando o formato de moeda.


[![Um asterisco é exibido ao lado de caixas de texto com uma entrada inválida](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image13.png)

**Figura 5**: um asterisco é exibido próximo a caixas de texto com uma entrada inválida ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image15.png))


Durante a validação funcionam-é, o usuário deve remover manualmente o símbolo de moeda ao editar um registro, que não é aceitável. Além disso, se houver inválido de entradas na edição interface nenhuma atualização nem cancelar botões, quando clicado, invoque um postback. Idealmente, o botão Cancelar retornaria DataList estado previamente edição, independentemente da validade de entradas de s do usuário. Além disso, precisamos garantir que os dados da página s são válidos antes de atualizar as informações de produto em DataList s `UpdateCommand` manipulador de eventos, como os controles de validação lógica do lado do cliente pode ser ignorada por usuários cujos navegadores don suporta JavaScript ou tem o suporte foi desabilitado.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Removendo o símbolo de moeda da caixa de texto de UnitPrice s EditItemTemplate

Ao usar o s CompareValidator `Currency``Type`, a entrada que está sendo validada não deve incluir qualquer símbolo de moeda. A presença de tais símbolos faz com que o CompareValidator marcar a entrada como inválido. No entanto, a nossa interface edição atualmente inclui um símbolo de moeda no `UnitPrice` caixa de texto, que significa que o usuário deve remover explicitamente o símbolo de moeda antes de salvar suas alterações. Para corrigir isso, temos três opções:

1. Configurar o `EditItemTemplate` para que o `UnitPrice` valor da caixa de texto não está formatado como uma moeda.
2. Permitir que o usuário insira um símbolo de moeda removendo CompareValidator e substituí-lo com um RegularExpressionValidator que verifica um valor de moeda formatada corretamente. O desafio aqui é que a expressão regular para validar um valor de moeda não é tão simples como CompareValidator e exigiria escrevendo código para incorporar as configurações de cultura.
3. Remover o controle de validação completamente e confiar na lógica de validação personalizada do lado do servidor em GridView s `RowUpdating` manipulador de eventos.

Permitir que o s ir com a opção 1 para este tutorial. Atualmente o `UnitPrice` é formatado como um valor de moeda devido à expressão de associação de dados para a caixa de texto no `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Alterar o `Eval` instrução `Eval("UnitPrice", "{0:n2}")`, que formata o resultado como um número com dois dígitos de precisão. Isso pode ser feito diretamente por meio da sintaxe declarativa ou clicando no link Editar DataBindings do `UnitPrice` TextBox em DataList s `EditItemTemplate`.

Com essa alteração, o preço formatado na interface de edição inclui vírgulas como separador de grupo e um ponto como o separador decimal, mas deixa desativar o símbolo de moeda.

> [!NOTE]
> Ao remover o formato de moeda da interface editável, eu seja útil para colocar o símbolo de moeda como texto fora da caixa de texto. Isso serve como uma dica para o usuário que não seja necessário fornecer o símbolo de moeda.


## <a name="fixing-the-cancel-button"></a>Corrigindo o botão Cancelar

Por padrão, os controles da Web de validação emitem JavaScript para executar a validação no lado do cliente. Quando um botão, LinkButton ou ImageButton é clicado, os controles de validação na página são verificados no lado do cliente antes que ocorra o postback. Se não houver nenhum dado inválido, a postagem é cancelada. Para determinados botões, no entanto, a validade dos dados pode ser irrelevante; Nesse caso, é o postback cancelado devido a dados inválidos de ter um incômodo.

O botão Cancelar é um exemplo. Imagine que um usuário insere dados inválidos, como omitir o nome do produto s e, em seguida, decide she t deseja salvar o produto depois que todas as e atinge o botão Cancelar. Atualmente, o botão Cancelar aciona os controles de validação na página, o qual relatam que o nome do produto está ausente e impedir que o postback. O usuário precisa digite algum texto para o `ProductName` TextBox para cancelar o processo de edição.

Felizmente, o botão, LinkButton e ImageButton têm um [ `CausesValidation` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) que pode indicar se ou não clicando no botão deve iniciar a lógica de validação (padrão é `True`). Definir o botão Cancelar s `CausesValidation` propriedade `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Verificar as entradas são válidos no manipulador de eventos UpdateCommand

Devido ao script de cliente emitido por controles de validação, se um usuário inserir uma entrada inválida os controles de validação cancelar qualquer postbacks iniciados pelo botão LinkButton, ou ImageButton controles cujo `CausesValidation` propriedades são `True` (o padrão). No entanto, se um usuário está visitando um navegador antiquado ou cujo suporte a JavaScript foi desabilitado, as verificações de validação do lado do cliente não serão executado.

Todos os controles de validação ASP.NET Repita sua lógica de validação imediatamente após o postback e relatar a validade geral das entradas de página s via o [ `Page.IsValid` propriedade](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). No entanto, o fluxo de página não é interrompido ou parado em qualquer modo com base no valor de `Page.IsValid`. Como os desenvolvedores, é nossa responsabilidade de garantir que o `Page.IsValid` propriedade tem um valor de `True` antes de prosseguir com o código que pressupõe válido de dados de entrada.

Se um usuário tiver JavaScript desabilitado, visite nossa página, edita um produto, insere um valor do preço de muito caro e clicar no botão de atualização, a validação do lado do cliente será ignorada e ocorrerá um postback. Em um postback, o ASP.NET página s `UpdateCommand` manipulador de eventos é executado e ocorrerá uma exceção ao tentar analisar muito caro um `Decimal`. Como temos o tratamento de exceções, essa exceção será tratada normalmente, mas podemos pode impedir que os dados inválidos passando por em primeiro lugar por continuar somente com o `UpdateCommand` manipulador de eventos se `Page.IsValid` tem um valor de `True`.

Adicione o seguinte código ao início do `UpdateCommand` manipulador de eventos, imediatamente antes do `Try` bloco:


[!code-csharp[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample2.cs)]

Com essa adição, o produto tentará ser atualizada somente se os dados enviados são válidos. A maioria dos usuários ganha t ser capaz de dados inválido devido aos scripts de cliente de controles de validação de postback, mas os usuários cujos navegadores don suporta JavaScript ou JavaScript com suporte desativado, podem ignorar as verificações do cliente e enviar dados inválidos.

> [!NOTE]
> O um leitor deve se lembrar que quando você atualizar os dados com o GridView, nós não foi necessário verificar explicitamente o `Page.IsValid` propriedade em nossa classe code-behind de páginas. Isso ocorre porque a consulta de GridView o `Page.IsValid` propriedade para nós e só prossegue com a atualização somente se ele retorna um valor de `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Etapa 3: Problemas de entrada de dados de resumo

Além dos controles de validação de cinco, ASP.NET inclui o [controle ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), que exibe o `ErrorMessage` s desses controles de validação que detectou dados inválidos. Estes resumos dados podem ser exibidos como texto na página da web ou por meio de uma caixa de mensagem modal, no lado do cliente. Permitir que o s aprimorar este tutorial para incluir uma messagebox cliente resumindo os problemas de validação.

Para fazer isso, arraste um controle ValidationSummary da caixa de ferramentas para o Designer. O local de t ValidationSummary controle importa, desde que re irá configurá-lo para exibir somente o resumo de como uma caixa de mensagem. Depois de adicionar o controle, defina seu [ `ShowSummary` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) para `False` e sua [ `ShowMessageBox` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) para `True`. Com essa adição, erros de validação são resumidos em uma caixa de mensagem do lado do cliente (veja a Figura 6).


[![Os erros de validação são resumidos em uma caixa de mensagem do lado do cliente](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image16.png)

**Figura 6**: A erros de validação são resumidos em uma caixa de mensagem do lado do cliente ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image18.png))


## <a name="summary"></a>Resumo

Neste tutorial, vimos como reduzir a probabilidade de exceções usando controles de validação para garantir proativamente nosso entradas de usuários são válidas antes de tentar usá-los no fluxo de trabalho de atualização. O ASP.NET fornece cinco controles da Web de validação que são projetados para inspecionar um determinado Web s entradas de controle e relatam sobre a validade de entrada s. Este tutorial usamos dois desses controles cinco o RequiredFieldValidator e CompareValidator para garantir que o nome do produto s foi fornecido e o preço tinha um formato de moeda com um valor maior que ou igual a zero.

Adicionando controles de validação para o s DataList interface de edição é tão simple quanto arrastando-os para o `EditItemTemplate` da caixa de ferramentas e definir algumas propriedades. Por padrão, os controles de validação emitir automaticamente o script de validação do lado do cliente; Eles também fornecem validação do lado do servidor em um postback, armazena o resultado acumulado no `Page.IsValid` propriedade. Para ignorar a validação do lado do cliente quando um botão, LinkButton ou ImageButton é clicado, defina o botão s `CausesValidation` propriedade `False`. Além disso, antes de executar qualquer tarefa com os dados enviados em um postback, certifique-se de que o `Page.IsValid` propriedade retorna `True`.

Todos os DataList edição tutoriais é ve examinou até o momento tiveram interfaces edição muito simples, uma caixa de texto para o nome do produto s e outro para o preço. No entanto, a interface de edição, pode conter uma mistura de diferentes controles da Web, como DropDownLists, calendários, botões, caixas de seleção e assim por diante. Nossa próxima tutorial, examinaremos a criação de uma interface que usa uma variedade de controles da Web.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Dennis Patterson, Ken Pespisa e Liz Shulok. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](handling-bll-and-dal-level-exceptions-cs.md)
> [Próximo](customizing-the-datalist-s-editing-interface-cs.md)
