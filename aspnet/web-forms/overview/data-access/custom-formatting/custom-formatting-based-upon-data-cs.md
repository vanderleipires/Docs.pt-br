---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
title: Formatação personalizada com base nos dados (c#) | Microsoft Docs
author: rick-anderson
description: Ajustar o formato do GridView, DetailsView ou FormView com base nos dados associados a ele pode ser feito de várias maneiras. Neste tutorial, vamos l...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 871a4574-f89c-4214-b786-79253ed3653b
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: ee9cdf19769ea63388fd9dd18a82bb2b4dcdef87
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825610"
---
<a name="custom-formatting-based-upon-data-c"></a>Formatação personalizada com base nos dados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe) ou [baixar PDF](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)

> Ajustar o formato do GridView, DetailsView ou FormView com base nos dados associados a ele pode ser feito de várias maneiras. Neste tutorial, examinaremos como realizar a formatação de dados associados com o uso de manipuladores de eventos DataBound e RowDataBound.


## <a name="introduction"></a>Introdução

A aparência dos controles GridView, DetailsView e FormView pode ser personalizada por meio de uma grande variedade de propriedades relacionadas a estilo. Propriedades como `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, e `Height`, entre outros, determinam a aparência geral do controle renderizado. Propriedades, incluindo `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, e outras permitem dessas mesmas configurações de estilo a ser aplicado a seções específicas. Da mesma forma, essas configurações de estilo podem ser aplicadas no nível do campo.

Em muitos cenários, os requisitos de formatação dependem do valor dos dados exibidos. Por exemplo, para chamar a atenção para fora de estoque produtos, um relatório que lista informações sobre o produto pode definir a cor do plano de fundo como amarelo para esses produtos cuja `UnitsInStock` e `UnitsOnOrder` campos são iguais a 0. Para realçar os produtos mais caros, podemos querer exibir os preços desses produtos que custam mais de US $75.00 em uma fonte em negrito.

Ajustar o formato do GridView, DetailsView ou FormView com base nos dados associados a ele pode ser feito de várias maneiras. Neste tutorial, examinaremos como realizar a formatação de dados associados com o uso do `DataBound` e `RowDataBound` manipuladores de eventos. No próximo tutorial, vamos explorar de uma abordagem alternativa.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Usando o controle de DetailsView`DataBound`manipulador de eventos

Quando os dados são associados a um DetailsView, de um controle de fonte de dados ou por meio da atribuição programaticamente os dados para o controle `DataSource` propriedade e chamar seu `DataBind()` ocorrer do método, a seguinte sequência de etapas:

1. Os controle da Web dos dados `DataBinding` evento é acionado.
2. Os dados serão associados aos dados de controle da Web.
3. Os controle da Web dos dados `DataBound` evento é acionado.

Lógica personalizada pode ser injetada imediatamente após as etapas 1 e 3 por meio de um manipulador de eventos. Criando um manipulador de eventos para o `DataBound` evento podemos determinar programaticamente os dados que tem sido associado ao controle de Web de dados e ajustar a formatação, conforme necessário. Para ilustrar isso vamos criar um DetailsView que listará informações gerais sobre um produto, mas exibirá os `UnitPrice` valor em uma ***fonte em negrito, itálico*** se ele exceder r $75.00.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Etapa 1: Exibindo as informações de produto em um DetailsView

Abra o `CustomColors.aspx` página na `CustomFormatting` pasta, arraste um controle DetailsView na caixa de ferramentas para o Designer, defina seu `ID` valor da propriedade `ExpensiveProductsPriceInBoldItalic`e associá-lo a um novo controle ObjectDataSource que invoca o `ProductsBLL` classe `GetProducts()` método. As etapas detalhadas para realizar isso são omitidas aqui para fins de brevidade, pois examinamos-los em detalhes nos tutoriais anteriores.

Depois que você tiver associado o ObjectDataSource a DetailsView, reserve um tempo para modificar a lista de campos. Optamos por para remover o `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, e `Discontinued` BoundFields e renomeado e reformatado o BoundFields restantes. Eu também limpo a `Width` e `Height` configurações. Como DetailsView exibe somente um único registro, é necessário habilitar a paginação para permitir que o usuário final exibir todos os produtos. Para fazer isso marcando a caixa de seleção Habilitar paginação na marca inteligente do ovládacího prvku DetailsView.


[![Marque a caixa de seleção Habilitar paginação na marca inteligente do ovládacího prvku DetailsView.](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)

**Figura 1**: Verifique a habilitar paginação de caixa de seleção Smart Tag de DetailsView ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-cs/_static/image3.png))


Após essas alterações, a marcação de DetailsView será:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample1.aspx)]

Reserve um tempo para testar esta página no navegador.


[![O controle DetailsView exibe um produto de cada vez](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)

**Figura 2**: O DetailsView exibe um produto de controle por vez ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-cs/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Etapa 2: Determinar o valor dos dados no manipulador de eventos de vinculação de dados programaticamente

Para exibir o preço em uma fonte em negrito, itálico para esses produtos cujos `UnitPrice` excede o valor de US $75.00, precisamos primeiro ser capaz de determinar de forma programática o `UnitPrice` valor. Para DetailsView, isso pode ser feito no `DataBound` manipulador de eventos. Para criar o evento manipulador de clique em DetailsView no Designer e navegue até a janela de propriedades. Pressione F4 para ativá-la, se não estiver visível, ou vá até o menu Exibir e selecione a opção de menu da janela Propriedades. Na janela Propriedades, clique no ícone de raio para listar os eventos de DetailsView. Em seguida, clique duas vezes o `DataBound` evento ou digite o nome do manipulador de eventos que você deseja criar.


![Criar um manipulador de eventos para o evento de vinculação de dados](custom-formatting-based-upon-data-cs/_static/image7.png)

**Figura 3**: criar um manipulador de eventos para o `DataBound` evento


Isso criará automaticamente o manipulador de eventos e levá-lo para a parte do código em que ele foi adicionado. Agora você verá:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample2.cs)]

Os dados associados a DetailsView podem ser acessados por meio de `DataItem` propriedade. Lembre-se de que estamos associando nossos controles a uma DataTable fortemente tipada, que é composta de uma coleção de instâncias de DataRow fortemente tipada. Quando a DataTable é vinculada a DetailsView, o primeiro DataRow na DataTable é atribuído a DetailsView `DataItem` propriedade. Especificamente, o `DataItem` propriedade é atribuída um `DataRowView` objeto. Podemos usar o `DataRowView`do `Row` propriedade para obter acesso ao objeto DataRow subjacente, que é realmente um `ProductsRow` instância. Assim que tivermos isso `ProductsRow` podemos fazer nossa decisão simplesmente inspecionando os valores de propriedade do objeto de instância.

O código a seguir ilustra como determinar se o `UnitPrice` associado ao controle DetailsView de valor é maior que US $75.00:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample3.cs)]

> [!NOTE]
> Uma vez que `UnitPrice` pode ter um `NULL` valor no banco de dados, nós primeiro verificamos para certificar-se de que estamos não lidando com um `NULL` valor antes de acessar o `ProductsRow`do `UnitPrice` propriedade. Essa verificação é importante porque se podemos tentar acessar o `UnitPrice` propriedade quando ele tem um `NULL` valor o `ProductsRow` objeto lançará um [StrongTypingException.&lt;1 exceção](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Etapa 3: Formatar o valor de UnitPrice em DetailsView

Agora podemos determinar se o `UnitPrice` valor associado a DetailsView tem um valor que excede US $75.00, mas temos ainda ver como ajustar de maneira programática DetailsView formatação do acordo. Para modificar a formatação de uma linha inteira em DetailsView, acessar de forma programática usando a linha `DetailsViewID.Rows[index]`; para modificar uma célula específica, o acesso usam `DetailsViewID.Rows[index].Cells[index]`. Assim que tivermos uma referência para a linha ou célula, em seguida, podemos ajustar sua aparência, definindo suas propriedades relacionadas a estilo.

Acessar uma linha por meio de programação requer que você saiba que o índice da linha que começa em 0. O `UnitPrice` linha é a quinta linha em DetailsView, dando a ele um índice de 4 e tornando-o acessível por meio de programação usando `ExpensiveProductsPriceInBoldItalic.Rows[4]`. Neste ponto poderíamos ter conteúdo do toda a linha exibido em uma fonte em negrito, itálico, usando o código a seguir:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample4.cs)]

No entanto, isso tornará *ambos* o rótulo (preço) e o valor em negrito e itálico. Se quisermos fazer apenas o valor em negrito e itálico, precisamos aplicar esta formatação para a segunda célula na linha, que pode ser feita usando o seguinte:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample5.cs)]

Como nossos tutoriais até o momento usou folhas de estilo para manter uma separação clara entre a marcação renderizada e informações relacionadas ao estilo, em vez de definir as propriedades de estilo específico, conforme mostrado acima vamos em vez disso, use uma classe CSS. Abra o `Styles.css` folha de estilos e adicione uma nova classe CSS denominada `ExpensivePriceEmphasis` com a seguinte definição:


[!code-css[Main](custom-formatting-based-upon-data-cs/samples/sample6.css)]

Em seguida, nos `DataBound` manipulador de eventos, defina a célula `CssClass` propriedade `ExpensivePriceEmphasis`. O seguinte código mostra o `DataBound` manipulador de eventos em sua totalidade:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample7.cs)]

Ao exibir Chai, que custa menos de US $75.00, o preço é exibido em uma fonte normal (veja a Figura 4). No entanto, ao exibir Mishi Kobe Niku, que tem um preço de US $97.00, o preço é exibido em uma fonte em negrito, itálico (consulte a Figura 5).


[![Os preços inferiores a US $75.00 são exibidos em uma fonte Normal](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)

**Figura 4**: os preços inferiores a US $75.00 são exibidos em uma fonte Normal ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-cs/_static/image10.png))


[![Os preços de produtos caros são exibidos em um negrito, itálico fonte](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)

**Figura 5**: os preços de produtos caros são exibidos em um negrito, itálico fonte ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-cs/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>Usando o controle de FormView`DataBound`manipulador de eventos

As etapas para determinar os dados subjacentes associados a um FormView são idênticas àquelas para um DetailsView, crie uma `DataBound` convertido do manipulador de eventos, o `DataItem` propriedade para o tipo de objeto apropriado associado ao controle e determinar como proceder. FormView e DetailsView forem diferentes, no entanto, em como a aparência da sua interface do usuário é atualizada.

FormView não contém qualquer BoundFields e, portanto, não tem o `Rows` coleção. Em vez disso, um FormView é composto de modelos, que podem conter uma combinação de HTML estático, controles da Web e a sintaxe de associação de dados. Ajustar o estilo de um FormView normalmente envolve o ajuste o estilo de um ou mais dos controles da Web dentro de modelos de FormView.

Para ilustrar isso, vamos usar um FormView para lista produtos, como no exemplo anterior, mas desta vez vamos exibir apenas o nome do produto e as unidades em estoque com as unidades em estoque exibido em uma fonte vermelha, se ele for menor que ou igual a 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Etapa 4: Exibindo as informações de produto em um FormView

Adicionar um FormView para o `CustomColors.aspx` página sob a DetailsView e defina sua `ID` propriedade `LowStockedProductsInRed`. Associe FormView ao controle ObjectDataSource criado na etapa anterior. Isso criará um `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate` para FormView. Remover o `EditItemTemplate` e `InsertItemTemplate` e simplificar a `ItemTemplate` para incluir apenas o `ProductName` e `UnitsInStock` valores, cada um em seus próprios controles de rótulo denominado adequadamente. Assim como acontece com DetailsView do exemplo anterior, verifique também a caixa de seleção Habilitar paginação na marca inteligente de FormView.

Após essas edições marcação de seu FormView deve ser semelhante ao seguinte:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample8.aspx)]

Observe que o `ItemTemplate` contém:

- **HTML estático** o texto "produto:" e "unidades no estoque:" junto com o `<br />` e `<b>` elementos.
- **Controles de Web** dois controles de rótulo `ProductNameLabel` e `UnitsInStockLabel`.
- **Sintaxe de associação de dados** as `<%# Bind("ProductName") %>` e `<%# Bind("UnitsInStock") %>` sintaxe, que atribui os valores desses campos para os controles de rótulo `Text` propriedades.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Etapa 5: Determinar o valor dos dados no manipulador de eventos de vinculação de dados programaticamente

Com a marcação de FormView completa, a próxima etapa é determinar programaticamente se a `UnitsInStock` valor é menor ou igual a 10. Isso é feito da mesma maneira exata FormView como era com DetailsView. Comece criando um manipulador de eventos para o FormView `DataBound` eventos.


![Criar o manipulador de eventos de vinculação de dados](custom-formatting-based-upon-data-cs/_static/image14.png)

**Figura 6**: criar o `DataBound` manipulador de eventos


No evento manipulador de conversão de FormView `DataItem` propriedade para um `ProductsRow` da instância e determinar se o `UnitsInPrice` valor é, de modo que precisamos para exibi-lo em uma fonte vermelha.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample9.cs)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Etapa 6: Formatação do controle de rótulo UnitsInStockLabel no ItemTemplate de FormView

A etapa final é formatar exibidos `UnitsInStock` valor em uma fonte vermelha se o valor é 10 ou menos. Para fazer isso, precisamos acessar programaticamente o `UnitsInStockLabel` no controlar o `ItemTemplate` e defina suas propriedades de estilo para que o texto é exibido em vermelho. Para acessar um controle da Web em um modelo, use o `FindControl("controlID")` método como este:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample10.cs)]

Para nosso exemplo, queremos acessar um rótulo de controle cuja `ID` valor é `UnitsInStockLabel`, portanto, usaríamos:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample11.cs)]

Assim que tivermos uma referência através de programação para o controle da Web, podemos modificar suas propriedades relacionadas a estilo conforme necessário. Como no exemplo anterior, eu criei uma classe CSS em `Styles.css` chamado `LowUnitsInStockEmphasis`. Para aplicar esse estilo para o controle da Web de rótulo, defina seu `CssClass` propriedade adequadamente.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample12.cs)]

> [!NOTE]
> A sintaxe de um modelo programaticamente acessando a Web usando o controle de formatação `FindControl("controlID")` e, em seguida, definir suas propriedades relacionadas a estilo também pode ser usado ao usar [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) na DetailsView ou GridView controles. Vamos examinar TemplateFields em nosso próximo tutorial.


As figuras 7 mostra FormView ao exibir um produto cujo `UnitsInStock` valor é maior que 10, enquanto o produto na Figura 8 tem seu valor menor que 10.


[![Para produtos com um suficientemente grande unidades no estoque, formatação de personalizada não é aplicada](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)

**Figura 7**: para produtos com um suficientemente grande unidades em estoque, formatação de personalizada não é aplicada ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-cs/_static/image17.png))


[![As unidades em estoque número é mostrada em vermelho para os produtos com valores de 10 ou menos](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)

**Figura 8**: A unidades em estoque número é mostrada em vermelho para os produtos com valores de 10 ou menos ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-cs/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Formatação com o GridView`RowDataBound`evento

Anteriormente, examinamos a sequência de etapas DetailsView e FormView controla o progresso por meio de durante a vinculação de dados. Vamos dar uma olhada sobre essas etapas novamente como um atualizador.

1. Os controle da Web dos dados `DataBinding` evento é acionado.
2. Os dados serão associados aos dados de controle da Web.
3. Os controle da Web dos dados `DataBound` evento é acionado.

Essas três etapas simples são suficientes para o DetailsView e FormView porque exibem apenas um único registro. Para o controle GridView, que exibe *todos os* registros associados a ele (e não apenas o primeiro), etapa 2 é um pouco mais envolvida.

Na etapa 2 GridView enumera a fonte de dados e, para cada registro, cria um `GridViewRow` da instância e associa o registro atual a ele. Para cada `GridViewRow` adicionado ao GridView, dois eventos são gerados:

- **`RowCreated`** é acionado depois que o `GridViewRow` foi criado
- **`RowDataBound`** é acionado depois que o registro atual foi associado para o `GridViewRow`.

Para o controle GridView, em seguida, a associação de dados com mais precisão é descrita pela seguinte sequência de etapas:

1. O GridView `DataBinding` evento é acionado.
2. Os dados serão associados ao GridView.   
  
   Para cada registro na fonte de dados 

    1. Criar um `GridViewRow` objeto
    2. Acionar o `RowCreated` evento
    3. Associar o registro para o `GridViewRow`
    4. Acionar o `RowDataBound` evento
    5. Adicione a `GridViewRow` para o `Rows` coleção
3. O GridView `DataBound` evento é acionado.

Para personalizar o formato dos registros de individuais do GridView, em seguida, precisamos criar um manipulador de eventos para o `RowDataBound` eventos. Para ilustrar isso, vamos adicionar um GridView para o `CustomColors.aspx` página que lista o nome, categoria e preço para cada produto, destacando aqueles produtos cujo preço é menor que US $10,00 com uma cor de plano de fundo amarelo.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Etapa 7: Exibindo informações de produto em um GridView

Adicione um GridView abaixo FormView do exemplo anterior e defina suas `ID` propriedade para `HighlightCheapProducts`. Como já temos um ObjectDataSource que retorna todos os produtos na página, fazer a ligação GridView. Por fim, edite BoundFields do GridView para incluir apenas dos produtos nomes, categorias e preços. Após essas edições, a marcação do GridView deve parecer com:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample13.aspx)]

Figura 9 mostra nosso progresso até o momento quando visualizado por meio de um navegador.


[![O GridView lista o nome, categoria e preço para cada produto](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)

**Figura 9**: O GridView lista o nome, categoria e preço para cada produto ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-cs/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Etapa 8: Determinar o valor dos dados no manipulador de evento RowDataBound programaticamente

Quando o `ProductsDataTable` está associado a GridView seus `ProductsRow` instâncias são enumerados e para cada `ProductsRow` um `GridViewRow` é criado. O `GridViewRow`do `DataItem` propriedade é atribuída à determinada `ProductRow`, após o qual o GridView `RowDataBound` manipulador de evento é gerado. Para determinar a `UnitPrice` o valor para cada produto ligado ao GridView, em seguida, precisamos criar um manipulador de eventos para o GridView `RowDataBound` eventos. Nesse manipulador de eventos, podemos pode inspecionar a `UnitPrice` valor atual `GridViewRow` e tomar uma decisão de formatação para aquela linha.

Esse manipulador de eventos pode ser criado usando a mesma série de etapas com o FormView e DetailsView.


![Criar um manipulador de eventos para eventos de RowDataBound do GridView](custom-formatting-based-upon-data-cs/_static/image24.png)

**Figura 10**: criar um manipulador de eventos para o GridView `RowDataBound` evento


Criar o manipulador de eventos dessa maneira fará com que o código a seguir sejam adicionados automaticamente à parte do código da página ASP.NET:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample14.cs)]

Quando o `RowDataBound` evento é acionado, o manipulador de eventos é passado como seu segundo parâmetro um objeto do tipo `GridViewRowEventArgs`, que tem uma propriedade chamada `Row`. Essa propriedade retorna uma referência para o `GridViewRow` que estava associada a dados apenas. Para acessar o `ProductsRow` instância associada ao `GridViewRow` usamos o `DataItem` propriedade desta forma:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample15.cs)]

Ao trabalhar com o `RowDataBound` manipulador de eventos, é importante ter em mente que o GridView é composto de diferentes tipos de linhas e que esse evento é disparado para *todos os* tipos de linha. Um `GridViewRow`do tipo pode ser determinado pelo seu `RowType` propriedade e pode ter um dos valores possíveis:

- `DataRow` uma linha que está associada a um registro do GridView `DataSource`
- `EmptyDataRow` a linha exibida se o GridView `DataSource` está vazio
- `Footer` a linha de rodapé. mostra se o GridView `ShowFooter` estiver definida como `true`
- `Header` a linha de cabeçalho; mostra se a propriedade de ShowHeader do GridView é definida como `true` (o padrão)
- `Pager` para do GridView que implementar a paginação, a linha que exibe a interface de paginação
- `Separator` não é usado para o GridView, mas usado pelo `RowType` controla a propriedades de DataList e Repeater, dois controles da Web que discutiremos no futuro tutoriais de dados

Uma vez que o `EmptyDataRow`, `Header`, `Footer`, e `Pager` linhas não são associadas com um `DataSource` registros, eles sempre terá uma `null` valor para seus `DataItem` propriedade. Por esse motivo, antes de tentar trabalhar com o atual `GridViewRow`do `DataItem` propriedade, podemos primeiro certifique-se de que estamos lidando com um `DataRow`. Isso pode ser feito verificando o `GridViewRow`do `RowType` propriedade desta forma:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample16.cs)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Etapa 9: Realce a linha amarela quando o UnitPrice valor for menor que US $10,00

A última etapa é realçar programaticamente todo o `GridViewRow` se o `UnitPrice` valor para aquela linha é menor que US $10,00. A sintaxe para acessar um GridView linhas ou células é igual ao DetailsView `GridViewID.Rows[index]` para acessar a linha inteira, `GridViewID.Rows[index].Cells[index]` para acessar uma determinada célula. No entanto, quando o `RowDataBound` manipulador de eventos é acionado o limite de dados `GridViewRow` ainda tem de ser adicionado a GridView `Rows` coleção. Portanto você não pode acessar o atual `GridViewRow` da instância da `RowDataBound` manipulador de eventos usando a coleção de linhas.

Em vez de `GridViewID.Rows[index]`, podemos fazer referência a atual `GridViewRow` da instância na `RowDataBound` manipulador de eventos usando `e.Row`. Ou seja, a fim de realçar atual `GridViewRow` da instância da `RowDataBound` usaríamos do manipulador de eventos:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample17.cs)]

Em vez de definir a `GridViewRow`do `BackColor` propriedade diretamente, vamos continuar com o uso de classes CSS. Eu criei uma classe CSS chamada `AffordablePriceEmphasis` que define a cor do plano de fundo como amarelo. Concluído `RowDataBound` manipulador de eventos segue:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample18.cs)]


[![Os produtos mais acessíveis são realçados amarelo](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)

**Figura 11**: A maioria de produtos acessíveis são realçados amarelo ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-cs/_static/image27.png))


## <a name="summary"></a>Resumo

Neste tutorial vimos como formatar o GridView, DetailsView e FormView com base nos dados associados ao controle. Para fazer isso, criamos um manipulador de eventos para o `DataBound` ou `RowDataBound` eventos, onde os dados subjacentes foi examinados, juntamente com uma alteração de formatação, se necessário. Para acessar os dados associados a um DetailsView ou FormView, podemos usar o `DataItem` propriedade no `DataBound` manipulador de eventos; para um GridView, cada `GridViewRow` da instância `DataItem` propriedade contém os dados associados a essa linha, que está disponível na `RowDataBound` manipulador de eventos.

A sintaxe para ajustar de forma programada a formatação do controle de Web de dados depende do controle da Web e como os dados a ser formatado são exibidos. Para DetailsView e GridView controles, as linhas e células podem ser acessadas por um índice ordinal. Para o FormView, que usa modelos, o `FindControl("controlID")` método normalmente é usado para localizar um controle de Web de dentro do modelo.

O próximo tutorial, examinaremos como usar modelos com GridView e DetailsView. Além disso, vamos ver outra técnica para personalizar a formatação com base nos dados subjacentes.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Levar os revisores para este tutorial não foram E.R. Gomes, Dennis Patterson e Dan Jagers. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](using-templatefields-in-the-gridview-control-cs.md)
