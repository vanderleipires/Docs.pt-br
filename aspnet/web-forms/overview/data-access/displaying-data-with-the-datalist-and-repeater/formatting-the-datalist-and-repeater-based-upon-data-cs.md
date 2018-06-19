---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
title: Formatação de DataList e repetidor com base nos dados (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, orientaremos através de exemplos de como podemos formatar a aparência dos controles DataList e repetidor, usando funções de formatação com...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 83e3d759-82b8-41e6-8d62-f0f4b3edec41
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 00dac460ad905d34632bca3249e019ddc626e440
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876209"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-c"></a>Formatação de DataList e repetidor com base nos dados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_CS.exe) ou [baixar PDF](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/datatutorial30cs1.pdf)

> Neste tutorial, orientaremos através de exemplos de como podemos formatar a aparência dos controles DataList e repetidor, usando funções de formatação nos modelos ou ao manipular o evento de ligação de dados.


## <a name="introduction"></a>Introdução

Como vimos no tutorial anterior, DataList oferece um número de propriedades relacionadas a estilo que afetam sua aparência. Em particular, vimos como atribuir padrão classes CSS à DataList s `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, e `SelectedItemStyle` propriedades. Além dessas quatro propriedades, DataList inclui uma série de outras propriedades relacionadas a estilo, como `Font`, `ForeColor`, `BackColor`, e `BorderWidth`, para citar alguns. Controle repetidor não contém todas as propriedades relacionadas ao estilo. Essas configurações de estilo devem ser feitas diretamente na marcação nos modelos de s Repetidor.

Muitas vezes, no entanto, como os dados devem ser formatados dependam dos dados em si. Por exemplo, ao listar os produtos é talvez queira exibir as informações de produto em uma cor cinza claro se ele foi descontinuado ou queremos realçar o `UnitsInStock` valor se for zero. Como vimos nos tutoriais anteriores, o GridView, DetailsView e FormView oferecem dois métodos distintos para formatar a aparência com base nos seus dados:

- **O `DataBound` evento** criar um manipulador de eventos apropriado `DataBound` evento, que é acionado depois que os dados foi associados a cada item (do GridView era o `RowDataBound` evento; para o DataList e repetidor é o `ItemDataBound`evento). Nesse evento manipulador, apenas os dados associados pode ser examinado e formatação decisões feitas. Examinamos dessa técnica a [personalizado formatação com base em dados](../custom-formatting/custom-formatting-based-upon-data-cs.md) tutorial.
- **As funções em modelos de formatação** ao usar TemplateFields em controles de DetailsView ou GridView ou um modelo no controle FormView, podemos adicionar uma função de formatação para a classe de code-behind de páginas ASP.NET, a camada de lógica de negócios ou qualquer outra biblioteca de classe que é acessível do aplicativo web. Essa função formatação pode aceitar um número arbitrário de parâmetros de entrada, mas deve retornar o HTML para renderizar o modelo. Funções de formatação foram examinadas primeiro o [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) tutorial.

Ambos formatação técnicas estão disponíveis com os controles DataList e Repetidor. Neste tutorial, orientaremos através de exemplos que usam ambas as técnicas para ambos os controles.

## <a name="using-theitemdataboundevent-handler"></a>Usando o`ItemDataBound`manipulador de eventos

Quando os dados são associados a uma DataList, de um controle de fonte de dados ou por meio de atribuição por meio de programação de dados para o controle s `DataSource` propriedade e chamar sua `DataBind()` método, DataList s `DataBinding` evento é acionado, a fonte de dados enumerada, e cada registro de dados é associado à DataList. Para cada registro na fonte de dados, DataList cria um [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) do objeto que é então associada ao registro atual. Durante esse processo, DataList gera dois eventos:

- **`ItemCreated`** é acionado depois que o `DataListItem` foi criado
- **`ItemDataBound`** dispara após o registro atual foi associado para o `DataListItem`

As etapas a seguir descrevem o processo de associação de dados para o controle DataList.

1. DataList s [ `DataBinding` evento](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) é acionado
2. Os dados serão associados à DataList  
  
   Para cada registro na fonte de dados 

    1. Criar um `DataListItem` objeto
    2. Acionar o [ `ItemCreated` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Associar o registro para o `DataListItem`
    4. Acionar o [ `ItemDataBound` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Adicionar o `DataListItem` para o `Items` coleção

Ao associar dados ao controle repetidor, processa a mesma sequência exata de etapas. A única diferença é que, em vez de `DataListItem` instâncias que estão sendo criadas, repetidor usa [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> O leitor um pode ter observado uma ligeira anomalias entre a sequência de etapas que ocorrer quando o controle DataList e repetidor estão associados aos dados versus quando GridView está associada aos dados. No final da parte final do processo de associação de dados, o GridView gera o `DataBound` evento; no entanto, o controle de DataList nem repetidor ter esse evento. Isso ocorre porque os controles DataList e repetidor foram criados no período de tempo do ASP.NET 1. x, antes do padrão de manipulador de evento de pré e pós-nível tornou comum.


Como com o GridView, uma opção para formatação com base nos dados é para criar um manipulador de eventos para o `ItemDataBound` evento. Este manipulador de eventos deve inspecionar os dados que tinham apenas foi associados ao `DataListItem` ou `RepeaterItem` e afetar a formatação do controle conforme necessário.

Para o controle DataList, alterações de formatação para o item inteiro pode ser implementado usando o `DataListItem` relacionadas a estilo propriedades, que incluem o padrão `Font`, `ForeColor`, `BackColor`, `CssClass`e assim por diante. Para afetar a formatação de controles da Web específicos dentro do modelo de s DataList, precisamos acessar e modificar o estilo desses controles da Web de forma programática. Vimos como realizar essa novamente o *personalizado formatação com base em dados* tutorial. Como o controle repetidor, o `RepeaterItem` classe não tem nenhuma propriedade de estilo; portanto, todas as alterações relacionadas a estilo feitas um `RepeaterItem` no `ItemDataBound` manipulador de eventos deve ser feito por meio de programação acessando e atualizando controles da Web dentro o modelo.

Desde o `ItemDataBound` formatação técnica para o DataList e repetidor são praticamente idênticos, nosso exemplo foco o uso do DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Etapa 1: Exibir informações de produto em DataList

Antes de nos preocupamos com a formatação, permitem s primeiro criar uma página que usa uma DataList para exibir informações sobre o produto. No [tutorial anterior](displaying-data-with-the-datalist-and-repeater-controls-cs.md) criamos uma DataList cujo `ItemTemplate` exibido cada nome de produto s, a categoria, o fornecedor, a quantidade por unidade e preço. Permitir que o s Repita essa funcionalidade aqui neste tutorial. Para fazer isso, você pode recriar ou DataList e seu ObjectDataSource do zero ou você pode copiar sobre esses controles da página criada no tutorial anterior (`Basics.aspx`) e colá-las na página para este tutorial (`Formatting.aspx`).

Depois que forem replicadas a funcionalidade DataList e ObjectDataSource `Basics.aspx` em `Formatting.aspx`, reserve um tempo para alterar o DataList s `ID` propriedade de `DataList1` para um mais descritivo `ItemDataBoundFormattingExample`. Em seguida, exiba o DataList em um navegador. Como mostra a Figura 1, a única diferença formatação entre cada produto é que alterna a cor de plano de fundo.


[![Os produtos são listados no controle DataList](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image1.png)

**Figura 1**: os produtos listados no controle DataList ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image3.png))


Para este tutorial, permitem s Formatar DataList, de modo que todos os produtos com um preço menor que US $20,00 terá dois seu nome e o preço de unidade realçada em amarelo.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Etapa 2: Determinar o valor dos dados no manipulador de eventos ItemDataBound programaticamente

Uma vez que apenas os produtos com um preço de US $20,00 será têm a formatação personalizada aplicada, podemos deve ser capazes de determinar o preço de s cada produto. Ao associar dados a uma DataList, DataList enumera os registros na fonte de dados e, para cada registro, cria um `DataListItem` instância, associando o registro da fonte de dados para o `DataListItem`. Depois que o registro específico s dados foi associados ao atual `DataListItem` objeto, DataList s `ItemDataBound` evento é acionado. Podemos criar um manipulador de eventos para esse evento inspecionar os valores de dados atual `DataListItem` e, com base nesses valores, faça as alterações de formatação necessários.

Criar um `ItemDataBound` evento DataList e adicione o seguinte código:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample1.cs)]

Enquanto o conceito e a semântica do DataList s `ItemDataBound` manipulador de eventos são as mesmas usadas pelo s a GridView `RowDataBound` manipulador de eventos de *personalizado formatação com base em dados* difere do tutorial, a sintaxe um pouco. Quando o `ItemDataBound` evento ser acionado, o `DataListItem` apenas associado a dados é passado para o manipulador de eventos correspondente por meio de `e.Item` (em vez de `e.Row`, assim como acontece com os s GridView `RowDataBound` manipulador de eventos). DataList s `ItemDataBound` manipulador de eventos é acionado para *cada* linha adicionada à DataList, incluindo linhas de cabeçalho, rodapé linhas e linhas de separador. No entanto, as informações do produto só são associadas às linhas de dados. Portanto, ao usar o `ItemDataBound` evento para inspecionar os dados vinculado à DataList, precisamos garantir que o primeiro que está trabalhando com um item de dados. Isso pode ser feito verificando o `DataListItem` s [ `ItemType` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), que pode ter um dos [os seguintes valores de oito](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Ambos `Item` e `AlternatingItem``DataListItem` itens de dados de composição de DataList s s. Supondo está trabalhando com um `Item` ou `AlternatingItem`, podemos acessar real `ProductsRow` instância que estava associada ao atual `DataListItem`. O `DataListItem` s [ `DataItem` propriedade](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) contém uma referência para o `DataRowView` objeto cujo `Row` propriedade fornece uma referência para o valor real `ProductsRow` objeto.

Em seguida, marcamos a `ProductsRow` instância s `UnitPrice` propriedade. Porque a tabela de produtos s `UnitPrice` campo permite `NULL` valores antes de tentar acessar o `UnitPrice` propriedade deve primeiro verificamos para ver se ele tem um `NULL` valor usando o `IsUnitPriceNull()` método. Se o `UnitPrice` valor não é `NULL`, podemos depois verifique para ver se ele s menor que US $20,00. Se for realmente em US $20,00, precisamos, em seguida, aplicar formatação personalizada.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Etapa 3: Realce o nome do produto s e o preço

Já sabemos que o preço de um produto s é menor que US $20,00, tudo o que permanece é destacar seu nome e o preço. Para fazer isso, podemos deve primeiro referenciar programaticamente os controles de rótulo de `ItemTemplate` que exibem o nome do produto s e o preço. Em seguida, é necessário que eles exibem um plano de fundo amarelo. Essas informações de formatação podem ser aplicadas ao modificar diretamente os rótulos `BackColor` propriedades (`LabelID.BackColor = Color.Yellow`); o ideal é, no entanto, todos os assuntos relacionados à exibição devem ser expresso através de folhas de estilo em cascata. Na verdade, já temos uma folha de estilos que fornece a formatação desejado definido no `Styles.css`  -  `AffordablePriceEmphasis`, que foi criado e discutido a *personalizado formatação com base em dados* tutorial.

Para aplicar a formatação, basta definir dois controles de rótulo Web `CssClass` propriedades `AffordablePriceEmphasis`, conforme mostrado no código a seguir:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample2.cs)]

Com o `ItemDataBound` manipulador de eventos concluído, revise o `Formatting.aspx` página em um navegador. Como ilustra a Figura 2, os produtos com um preço de US $20,00 tem seu nome e o preço realçado.


[![Esses produtos menor que US $20,00 são realçados](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image4.png)

**Figura 2**: os produtos de menor que US $20,00 são realçados ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image6.png))


> [!NOTE]
> Desde o DataList é renderizado como uma marca HTML `<table>`, sua `DataListItem` instâncias têm propriedades relacionadas a estilo que podem ser definidas para aplicar um estilo específico para o item inteiro. Por exemplo, se quiséssemos realçar o *todo* item amarelo quando seu preço era menor que US $20,00, poderia ter substituímos o código que os rótulos de referenciado e defina seus `CssClass` propriedades com a seguinte linha de código: `e.Item.CssClass = "AffordablePriceEmphasis"` (consulte a Figura 3).


O `RepeaterItem` que compõem o controle repetidor, no entanto, Anibal t oferecem tais propriedades de nível de estilo. Portanto, aplicar formatação personalizada para repetidor requer que o aplicativo de propriedades de estilo para os controles da Web nos modelos repetidor s, como na Figura 2.


[![O Item de produto inteiro é realçado para produtos em US $20,00](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image7.png)

**Figura 3**: O Item de produto inteiro é realçado para produtos em US $20,00 ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Usando funções de formatação de dentro do modelo

No *Usando TemplateFields no controle GridView* tutorial vimos como usar uma função de formatação em um GridView TemplateField para aplicar formatação personalizada com base nos dados associados às linhas s GridView. Uma função de formatação é um método que pode ser chamado a partir de um modelo e retorna o HTML para ser emitida em seu lugar. Funções de formatação podem residir na classe de code-behind de páginas ASP.NET ou podem ser centralizadas em arquivos de classe no `App_Code` pasta ou em um projeto de biblioteca de classes separado. Mover a função formatação fora da classe de code-behind da página s ASP.NET é ideal se você planeja usar a mesma função de formatação em várias páginas ASP.NET ou em outros aplicativos da web ASP.NET.

Para demonstrar as funções de formatação, permitem s tem as informações de produto incluem o texto [DESCONTINUADO] ao lado do nome do produto s se ele s descontinuado. Além disso, permitem s ter se preço realçado amarelo-s menor que US $20,00 (como foi o `ItemDataBound` exemplo de manipulador de eventos); se o preço é de US $20,00 ou superior, permitem s não exibir o preço real, mas em vez disso, o texto, tente chamar uma cotação de preço. A Figura 4 mostra uma captura de tela dos produtos listando com essas regras de formatação aplicadas.


[![Para produtos caros, o preço é substituído com o texto,. chame uma cotação de preço](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image10.png)

**Figura 4**: caro de produtos, o preço é substituído com o texto, chame para uma cotação ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Etapa 1: Criar as funções de formatação

Para este exemplo precisamos duas funções de formatação, que exibe o nome do produto junto com o texto DISCONTINUED, se necessário e outra que exibe um preço de realçado se ele s menor que US $20,00 ou o texto, chame para uma cotação caso contrário. Permitir que o s criar essas funções em que a classe de code-behind de páginas ASP.NET e nomeá-los `DisplayProductNameAndDiscontinuedStatus` e `DisplayPrice`. Ambos os métodos retornar o HTML para renderizar como uma cadeia de caracteres e ambos precisarem ser marcado `Protected` (ou `Public`) para ser chamado da parte de sintaxe declarativa de páginas do ASP.NET. O código para esses dois métodos a seguir:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample3.cs)]

Observe que o `DisplayProductNameAndDiscontinuedStatus` método aceita os valores da `productName` e `discontinued` campos de dados como valores escalares, enquanto o `DisplayPrice` método aceita um `ProductsRow` instância (em vez de `unitPrice` valor escalar). Qualquer abordagem funcionará; No entanto, se a função formatação está trabalhando com valores escalares que podem conter o banco de dados `NULL` valores (como `UnitPrice`; nem `ProductName` nem `Discontinued` permitir `NULL` valores), deve ter especial cuidado em tratar estes entradas de escalares.

Em particular, o parâmetro de entrada deve ser do tipo `Object` como o valor de entrada pode ser um `DBNull` instância em vez do tipo de dados esperado. Além disso, uma verificação deve ser feita para determinar se o valor de entrada é um banco de dados `NULL` valor. Ou seja, se quiséssemos o `DisplayPrice` método para aceitar o preço como um valor escalar, podemos d precisa usar o código a seguir:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample4.cs)]

Observe que o `unitPrice` parâmetro de entrada é do tipo `Object` e que foi modificada para verificar se a instrução condicional `unitPrice` é `DBNull` ou não. Além disso, desde o `unitPrice` parâmetro de entrada é passado como um `Object`, ela deve ser convertida em um valor decimal.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Etapa 2: Chamando a função de formatação do ItemTemplate DataList s

Com as funções de formatação adicionadas à nossa classe de code-behind de páginas ASP.NET, tudo o que resta é chamar essas funções de DataList s de formatação `ItemTemplate`. Para chamar uma função de formatação de um modelo, coloque a chamada de função dentro da sintaxe de associação de dados:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample5.aspx)]

Em DataList s `ItemTemplate` o `ProductNameLabel` controle de rótulo Web atualmente exibe o nome do produto s atribuindo seu `Text` propriedade o resultado de `<%# Eval("ProductName") %>`. Para que exiba o nome NetBIOS e o texto DISCONTINUED, se necessário, atualize a sintaxe declarativa para que, em vez disso, ele atribui o `Text` o valor da propriedade do `DisplayProductNameAndDiscontinuedStatus` método. Ao fazer isso, podemos deve passar o nome de produto s e descontinuados valores usando o `Eval("columnName")` sintaxe. `Eval` Retorna um valor do tipo `Object`, mas o `DisplayProductNameAndDiscontinuedStatus` método espera parâmetros de entrada do tipo `String` e `Boolean`; portanto, é necessário converter os valores retornados pelo `Eval` método para os tipos de parâmetro de entrada esperados, da seguinte forma:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample6.aspx)]

Para exibir o preço, podemos simplesmente definir o `UnitPriceLabel` rótulo s `Text` propriedade para o valor retornado pelo `DisplayPrice` método, como podemos não para exibir o nome do produto s e texto [DESCONTINUADO]. No entanto, em vez de passar o `UnitPrice` como um parâmetro de entrada escalar, em vez disso, passamos em todo o `ProductsRow` instância:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample7.aspx)]

Com as chamadas para as funções de formatação em vigor, dedique alguns momentos para exibir nosso progresso em um navegador. Sua tela deve ser semelhante à Figura 5, com os produtos descontinuados, incluindo o texto [DESCONTINUADO] e os produtos que custam mais de US $20,00 com seu preço substituído com o texto a chamada para uma cotação.


[![Para produtos caros, o preço é substituído com o texto,. chame uma cotação de preço](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image13.png)

**Figura 5**: caro de produtos, o preço é substituído com o texto, chame para uma cotação ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image15.png))


## <a name="summary"></a>Resumo

O conteúdo de um controle DataList ou repetidor com base nos dados de formatação pode ser feito usando duas técnicas. A primeira técnica é criar um manipulador de eventos para o `ItemDataBound` evento, que é acionado como cada registro da fonte de dados está associado a um novo `DataListItem` ou `RepeaterItem`. No `ItemDataBound` manipulador de eventos, os dados atuais do item s podem ser examinados e, em seguida, a formatação pode ser aplicada para o conteúdo do modelo ou para `DataListItem` s, para o próprio item inteiro.

Como alternativa, a formatação personalizada pode ser alcançada por meio de funções de formatação. Uma função de formatação é um método que pode ser chamado do DataList repetidor s modelos ou que retorna o HTML de emissão em seu lugar. Muitas vezes, o HTML retornado por uma função de formatação é determinado pelos valores que está sendo associados ao item atual. Esses valores podem ser passados para a função de formatação, como valores escalares ou passando todo o objeto que está sendo associado ao item (como o `ProductsRow` instância).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Yaakov Ellis Randy Schmidt e Liz Shulok. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-data-with-the-datalist-and-repeater-controls-cs.md)
> [Próximo](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
