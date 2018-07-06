---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
title: Formatação do DataList e Repeater com base nos dados (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial vamos ver exemplos de como podemos formatar a aparência dos controles DataList e Repeater, usando funções de formatação com...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: 83e3d759-82b8-41e6-8d62-f0f4b3edec41
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: cfb23a65c288ed155625a1f8a4d7db1330ab2407
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840252"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-c"></a>Formatação do DataList e Repeater com base nos dados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_CS.exe) ou [baixar PDF](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/datatutorial30cs1.pdf)

> Neste tutorial vamos ver exemplos de como podemos formatar a aparência dos controles DataList e Repeater, usando funções de formatação nos modelos ou manipulando o evento de vinculação de dados.


## <a name="introduction"></a>Introdução

Como vimos no tutorial anterior, DataList oferece uma série de propriedades relacionadas ao estilo que afetam sua aparência. Em particular, vimos como atribuir padrão classes CSS a s DataList `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, e `SelectedItemStyle` propriedades. Além dessas quatro propriedades, DataList inclui um número de outras propriedades relacionadas a estilo, como `Font`, `ForeColor`, `BackColor`, e `BorderWidth`, para citar alguns. O controle Repeater não contém quaisquer propriedades relacionadas a estilo. Essas configurações de estilo devem ser feitas diretamente dentro da marcação nos modelos de s Repeater.

Muitas vezes, no entanto, como os dados devem ser formatados dependam dos dados em si. Por exemplo, ao listar produtos queremos exibir as informações de produto em uma cor de fonte cinza claro, se ele foi descontinuado ou queremos destacar o `UnitsInStock` valor se for zero. Como vimos nos tutoriais anteriores, o GridView, DetailsView e FormView oferecem duas maneiras distintas para formatar a aparência com base em seus dados:

- **O `DataBound` evento** criar um manipulador de eventos apropriado `DataBound` evento, que é acionado depois que os dados foi associados a cada item (para o GridView era o `RowDataBound` evento; para DataList e Repeater é o `ItemDataBound`evento). Nesse evento manipulador, apenas os dados associados pode ser examinado e decisões de formatação feitas. Examinamos essa técnica na [formatação com base em dados personalizados](../custom-formatting/custom-formatting-based-upon-data-cs.md) tutorial.
- **Funções em modelos de formatação** ao uso de TemplateFields em DetailsView ou GridView controles ou um modelo no controle FormView, podemos adicionar uma função de formatação para a classe de code-behind s de página ASP.NET, a camada de lógica de negócios ou qualquer outra biblioteca de classe é acessível do aplicativo web. Essa função de formatação pode aceitar um número arbitrário de parâmetros de entrada, mas deve retornar o HTML para renderizar no modelo. Funções de formatação foram examinadas pela primeira vez na [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) tutorial.

Ambas essas técnicas de formatação estão disponíveis com os controles DataList e Repeater. Neste tutorial vamos ver exemplos que usam ambas as técnicas para ambos os controles.

## <a name="using-theitemdataboundevent-handler"></a>Usando o`ItemDataBound`manipulador de eventos

Quando os dados são associados a um DataList de um controle de fonte de dados ou por meio da atribuição programaticamente os dados para o controle s `DataSource` propriedade e chamar seu `DataBind()` método, DataList s `DataBinding` evento é acionado, a fonte de dados enumerada, e cada registro de dados está associado à DataList. Para cada registro na fonte de dados, DataList cria uma [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) objeto que é então associado ao registro atual. Durante esse processo, DataList gera dois eventos:

- **`ItemCreated`** é acionado depois que o `DataListItem` foi criado
- **`ItemDataBound`** é acionado depois que o registro atual foi associado para o `DataListItem`

As etapas a seguir descrevem o processo de associação de dados para o controle DataList.

1. S DataList [ `DataBinding` evento](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) é acionado
2. Os dados serão associados à DataList  
  
   Para cada registro na fonte de dados 

    1. Criar um `DataListItem` objeto
    2. Acionar o [ `ItemCreated` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Associar o registro para o `DataListItem`
    4. Acionar o [ `ItemDataBound` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Adicione a `DataListItem` para o `Items` coleção

Ao associar dados ao controle Repeater, ele percorre a mesma sequência exata de etapas. A única diferença é que, em vez de `DataListItem` repetidor de instâncias que está sendo criadas, usa [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> O leitor astuto deve ter notado uma anomalia de pequena entre a sequência de etapas que são realizadas quando DataList e Repeater são associados a dados em vez de quando o GridView está associado a dados. No final do processo de associação de dados, o GridView aciona o `DataBound` evento; no entanto, nem o DataList nem Repeater controle tem tal evento. Isso ocorre porque os controles DataList e Repeater foram criados no período de tempo do ASP.NET 1. x, antes que o padrão de manipulador de evento de pré e pós-nível se tornaram comuns.


Como com o controle GridView, uma opção para formatação com base nos dados é criar um manipulador de eventos para o `ItemDataBound` eventos. Esse manipulador de eventos seria inspecionar os dados que tinham apenas foi associados à `DataListItem` ou `RepeaterItem` e afetam a formatação do controle, conforme necessário.

Para o controle DataList, alterações de formatação para o item inteiro pode ser implementado usando o `DataListItem` relacionadas a estilo propriedades, que incluem o padrão `Font`, `ForeColor`, `BackColor`, `CssClass`e assim por diante. Para afetar a formatação de controles da Web específicos dentro do modelo de s DataList, precisamos acessar e modificar o estilo desses controles Web programaticamente. Vimos como realizar essa back na *formatação com base em dados personalizados* tutorial. Como o controle Repeater, o `RepeaterItem` classe não tem nenhuma propriedade relacionadas ao estilo; portanto, todas as alterações relacionadas a estilo feitas uma `RepeaterItem` no `ItemDataBound` manipulador de eventos deve ser feito por meio de programação acessando e atualizando controles da Web dentro do o modelo.

Uma vez que o `ItemDataBound` técnica de formatação para o DataList e Repeater são praticamente idênticos, nosso exemplo se concentrará no uso do DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Etapa 1: Exibir informações sobre o produto no DataList

Antes de nos preocupamos com a formatação, let s primeiro criar uma página que usa uma DataList para exibir informações sobre o produto. No [tutorial anterior](displaying-data-with-the-datalist-and-repeater-controls-cs.md) criamos uma DataList cujo `ItemTemplate` exibido cada nome de produto s, a categoria, o fornecedor, a quantidade por unidade e preço. Deixe o s repetir essa funcionalidade aqui neste tutorial. Para fazer isso, você pode recriar ou DataList e seu ObjectDataSource do zero ou você pode copiar sobre esses controles de página criada no tutorial anterior (`Basics.aspx`) e cole-os na página para este tutorial (`Formatting.aspx`).

Depois que você tiver replicado a funcionalidade do DataList e ObjectDataSource da `Basics.aspx` em `Formatting.aspx`, reserve um tempo para alterar o s DataList `ID` propriedade do `DataList1` para um mais descritivo `ItemDataBoundFormattingExample`. Em seguida, exiba DataList em um navegador. Como mostra a Figura 1, a única diferença formatação entre cada produto é que a cor do plano de fundo alternativos.


[![Os produtos são listados no controle DataList](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image1.png)

**Figura 1**: os produtos listados no controle DataList ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image3.png))


Para este tutorial, deixe s DataList de formato, de modo que quaisquer produtos com um preço menor que US $20,00 terá tanto seu nome e o preço de unidade realçado em amarelo.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Etapa 2: Determinar o valor dos dados no manipulador de evento ItemDataBound programaticamente

Como apenas os produtos com um preço de US $20,00 irá tem a formatação personalizada aplicada, podemos deve ser capazes de determinar o preço de s cada produto. Ao associar dados a uma DataList, DataList enumera os registros na fonte de dados e, para cada registro, cria uma `DataListItem` instância, associando o registro de fonte de dados para o `DataListItem`. Depois que o registro específico s dados foi associados ao atual `DataListItem` objeto, DataList s `ItemDataBound` evento é disparado. Podemos criar um manipulador de eventos para esse evento inspecionar os valores de dados atual `DataListItem` e, com base nesses valores, faça as alterações de formatação necessário.

Criar um `ItemDataBound` evento para o DataList e adicione o seguinte código:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample1.cs)]

Embora o conceito e a semântica do DataList s `ItemDataBound` manipulador de eventos são as mesmas usadas pelo s GridView `RowDataBound` manipulador de eventos na *formatação com base em dados personalizados* difere do tutorial, a sintaxe um pouco. Quando o `ItemDataBound` evento é acionado, o `DataListItem` apenas associado a dados é passado para o manipulador de evento correspondente por meio `e.Item` (em vez de `e.Row`, assim como acontece com o GridView s `RowDataBound` manipulador de eventos). S DataList `ItemDataBound` manipulador de eventos é acionado para *cada* linha adicionada à DataList, incluindo linhas de cabeçalho, rodapé linhas e linhas de separador. No entanto, as informações do produto estão associadas apenas às linhas de dados. Portanto, ao usar o `ItemDataBound` eventos para inspecionar os dados associados à DataList, precisamos primeiro certifique-se de que estamos está trabalhando com um item de dados. Isso pode ser feito verificando o `DataListItem` s [ `ItemType` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), que pode ter um dos [os seguintes valores de oito](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Ambos `Item` e `AlternatingItem``DataListItem` itens de dados de composição a s DataList s. Supondo que estamos está trabalhando com um `Item` ou `AlternatingItem`, podemos acessar real `ProductsRow` instância que estava associada ao atual `DataListItem`. O `DataListItem` s [ `DataItem` propriedade](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) contém uma referência para o `DataRowView` objeto cuja `Row` propriedade fornece uma referência para o real `ProductsRow` objeto.

Em seguida, verificamos a `ProductsRow` instância s `UnitPrice` propriedade. Porque a tabela de produtos s `UnitPrice` campo permite `NULL` valores antes de tentar acessar o `UnitPrice` propriedade devem primeiro verificamos para ver se ele tem um `NULL` valor usando o `IsUnitPriceNull()` método. Se o `UnitPrice` valor não é `NULL`, podemos então verifique para ver se ele menor que US $ 20,00 por s. Se for, de fato, em US $20,00, em seguida, precisamos aplicar a formatação personalizada.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Etapa 3: Realce o nome do produto s e o preço

Uma vez que soubermos que o preço de um produto s é menor que US $20,00, tudo o que resta é destacar seu nome e o preço. Para fazer isso, deve primeiro programaticamente referenciamos os controles de rótulo no `ItemTemplate` que exibem o nome do produto s e o preço. Em seguida, precisamos que eles exibem um plano de fundo amarelo. Essas informações de formatação podem ser aplicadas ao modificar diretamente os rótulos `BackColor` propriedades (`LabelID.BackColor = Color.Yellow`); o ideal é que, no entanto, todos os assuntos relacionados a exibição devem ser expressa por meio de folhas de estilo em cascata. Na verdade, já temos uma folha de estilos que fornece a formatação desejados definidos no `Styles.css`  -  `AffordablePriceEmphasis`, que foi criado e discutido os *formatação com base em dados personalizados* tutorial.

Para aplicar a formatação, basta definir os dois controles de Web Label `CssClass` propriedades a serem `AffordablePriceEmphasis`, conforme mostrado no código a seguir:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample2.cs)]

Com o `ItemDataBound` concluída do manipulador de eventos, examine o `Formatting.aspx` página em um navegador. Como ilustra a Figura 2, esses produtos com um preço de US $ 20,00 por tem seu nome e o preço realçado.


[![Esses produtos inferiores a US $20,00 são realçadas](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image4.png)

**Figura 2**: os produtos de menos de US $20,00 são realçadas ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image6.png))


> [!NOTE]
> Uma vez que DataList é renderizado como uma marca HTML `<table>`, sua `DataListItem` instâncias têm propriedades relacionadas a estilo que podem ser definidas para aplicar um estilo específico para o item inteiro. Por exemplo, se desejamos destacar os *todo* item amarelo quando seu preço era menor que US $20,00, poderia ter substituímos o código que os rótulos de referenciado e defina seus `CssClass` propriedades com a seguinte linha de código: `e.Item.CssClass = "AffordablePriceEmphasis"` (consulte a Figura 3).


O `RepeaterItem` s que compõem o controle Repeater, no entanto, don t oferecem tais propriedades de nível de estilo. Portanto, a aplicação da formatação personalizada para o Repeater requer a aplicação de propriedades de estilo para controles da Web nos modelos Repeater s, exatamente como fizemos na Figura 2.


[![O Item de produto inteiro é realçado para produtos em US $20,00](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image7.png)

**Figura 3**: O Item de produto inteiro é realçado para produtos em US $20,00 ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Usando funções de formatação de dentro do modelo

No *Usando TemplateFields no controle GridView* tutorial vimos como usar uma função de formatação em um GridView TemplateField para aplicar a formatação personalizada com base nos dados associados às linhas s GridView. Uma função de formatação é um método que pode ser invocado a partir de um modelo e retorna o HTML a ser emitida em seu lugar. Funções de formatação podem residir na classe de code-behind s de página ASP.NET ou pode ser centralizadas em arquivos de classe no `App_Code` pasta ou em um projeto de biblioteca de classes separado. Mover a função de formatação fora da classe de code-behind da página s ASP.NET é ideal se você planeja usar a mesma função de formatação em várias páginas ASP.NET ou em outros aplicativos da web ASP.NET.

Para demonstrar as funções de formatação, let s tem as informações de produto incluem o texto [DISCONTINUED] ao lado do nome do produto s se ele s descontinuado. Além disso, let s ter if preço realçado amarelo-s menor que US $20,00 (como fizemos no `ItemDataBound` exemplo de manipulador de eventos); se o preço é US $20,00 ou s maior, let não exibir o preço real, mas em vez disso, o texto, tente chamar uma cotação de preço. Figura 4 mostra uma captura de tela dos produtos listando com essas regras de formatação aplicadas.


[![Para produtos cara, o preço é substituído pelo texto,. chame uma cotação de preço](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image10.png)

**Figura 4**: para produtos caros, o preço é substituído pelo texto,. chame uma cotação de preço ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Etapa 1: Criar funções de formatação

Para este exemplo precisamos de duas funções de formatação, um que exibe o nome do produto, juntamente com o texto DISCONTINUED, se necessário e outro que exibe um preço de realçado se ele s menor que US $20,00 ou o texto,. chame uma cotação de preço caso contrário. Permitir que o s criar essas funções em que a classe de code-behind s de página ASP.NET e nomeá-los `DisplayProductNameAndDiscontinuedStatus` e `DisplayPrice`. Precisam de ambos os métodos retornam o HTML renderizar como uma cadeia de caracteres e ambos precisam ser marcado `Protected` (ou `Public`) para que sejam invocados da parte de sintaxe declarativa de s de página do ASP.NET. O código para esses dois métodos a seguir:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample3.cs)]

Observe que o `DisplayProductNameAndDiscontinuedStatus` método aceita os valores da `productName` e `discontinued` campos de dados como valores escalares, enquanto o `DisplayPrice` método aceita um `ProductsRow` instância (em vez de uma `unitPrice` valor escalar). Qualquer uma das abordagens funcionará; No entanto, se a função de formatação está trabalhando com valores escalares que podem conter o banco de dados `NULL` valores (como `UnitPrice`; nem `ProductName` nem `Discontinued` permitir `NULL` valores), cuidado especial precisa ser tomado em tratá-los entradas de escalares.

Em particular, o parâmetro de entrada deve ser do tipo `Object` como o valor de entrada pode ser um `DBNull` instância em vez do tipo de dados esperado. Além disso, deve ser feita uma verificação para determinar se o valor de entrada é um banco de dados `NULL` valor. Ou seja, se quiséssemos os `DisplayPrice` método aceitem o preço como um valor escalar, podemos d precisa usar o código a seguir:


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample4.cs)]

Observe que o `unitPrice` parâmetro de entrada é do tipo `Object` e que a instrução condicional foi modificada para averiguar se `unitPrice` é `DBNull` ou não. Além disso, desde o `unitPrice` parâmetro de entrada é passado como um `Object`, ele deve ser convertido em um valor decimal.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Etapa 2: Chamando a função de formatação do DataList s ItemTemplate

Com as funções de formatação adicionadas à nossa classe de code-behind s de página ASP.NET, tudo o que resta é chamar essas funções a partir do DataList s de formatação `ItemTemplate`. Para chamar uma função de formatação de um modelo, coloque a chamada de função dentro da sintaxe de vinculação de dados:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample5.aspx)]

No DataList s `ItemTemplate` as `ProductNameLabel` controle de rótulo Web atualmente exibe o nome do produto s atribuindo seu `Text` propriedade o resultado de `<%# Eval("ProductName") %>`. Para que ele exiba o nome, além do texto DISCONTINUED, se necessário, atualize a sintaxe declarativa para que, em vez disso, ele atribui a `Text` o valor da propriedade do `DisplayProductNameAndDiscontinuedStatus` método. Ao fazer isso, podemos deve passar o nome do produto s e descontinuados valores usando o `Eval("columnName")` sintaxe. `Eval` Retorna um valor do tipo `Object`, mas o `DisplayProductNameAndDiscontinuedStatus` método espera parâmetros de entrada do tipo `String` e `Boolean`; portanto, é necessário converter os valores retornados pelo `Eval` método para os tipos de parâmetro de entrada, da seguinte forma:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample6.aspx)]

Para exibir o preço, podemos simplesmente definir a `UnitPriceLabel` rótulo s `Text` o valor retornado pela propriedade o `DisplayPrice` método, assim como estamos fez para exibir o nome do produto s e [DESCONTINUADO] o texto. No entanto, em vez de passar o `UnitPrice` como um parâmetro de entrada escalar, em vez disso, passamos em todo o `ProductsRow` instância:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample7.aspx)]

Com as chamadas para as funções de formatação em vigor, reserve um tempo para exibir nosso progresso em um navegador. Sua tela deve ser semelhante à Figura 5, com os produtos descontinuados, incluindo o texto [DISCONTINUED] e esses produtos que custam mais de US $ 20,00 por ter seu preço substituído pelo texto, a chamada para uma cotação de preço.


[![Para produtos cara, o preço é substituído pelo texto,. chame uma cotação de preço](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image13.png)

**Figura 5**: para produtos caros, o preço é substituído pelo texto,. chame uma cotação de preço ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image15.png))


## <a name="summary"></a>Resumo

O conteúdo de um controle DataList ou Repeater com base nos dados de formatação pode ser feito usando duas técnicas. A primeira técnica é criar um manipulador de eventos para o `ItemDataBound` evento, que é acionado como cada registro na fonte de dados está associado a um novo `DataListItem` ou `RepeaterItem`. No `ItemDataBound` manipulador de eventos, os dados atuais do item s podem ser examinados e, em seguida, a formatação pode ser aplicada ao conteúdo do modelo ou, para `DataListItem` s, para o item inteiro em si.

Como alternativa, formatação personalizada pode ser alcançada por meio de funções de formatação. Uma função de formatação é um método que pode ser invocado a partir do DataList Repeater s modelos ou que retorna o HTML para emitir em seu lugar. Muitas vezes, o HTML retornado por uma função de formatação é determinado pelos valores que está sendo associados ao item atual. Esses valores podem ser passados para a função de formatação, como valores escalares ou passando o objeto inteiro que está sendo associado ao item (como o `ProductsRow` instância).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisores líder para este tutorial foram Yaakov Ellis, Randy Schmidt e Liz Shulok. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-data-with-the-datalist-and-repeater-controls-cs.md)
> [Próximo](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
