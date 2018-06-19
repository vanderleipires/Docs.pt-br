---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: Usando TemplateFields no controle DetailsView (c#) | Microsoft Docs
author: rick-anderson
description: Os mesmos recursos de TemplateFields disponíveis com o GridView também estão disponíveis com o controle DetailsView. Neste tutorial exibiremos um produto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: f1d2e8312451c0bd1b3aba448963317f5fe06029
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879095"
---
<a name="using-templatefields-in-the-detailsview-control-c"></a>Usando TemplateFields no controle DetailsView (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe) ou [baixar PDF](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> Os mesmos recursos de TemplateFields disponíveis com o GridView também estão disponíveis com o controle DetailsView. Neste tutorial, exibiremos um produto no momento usando um DetailsView contendo TemplateFields.


## <a name="introduction"></a>Introdução

O TemplateField oferece um maior grau de flexibilidade nos dados de renderização que o BoundField, CheckBoxField, HyperLinkField e outros controles de campo de dados. No [tutorial anterior](using-templatefields-in-the-gridview-control-cs.md) examinamos usando o TemplateField em um GridView para:

- Exiba vários valores de campo de dados em uma coluna. Especificamente, ambos o `FirstName` e `LastName` campos foram combinados em uma coluna de GridView.
- Use um controle de Web alternativo para expressar um valor de campo de dados. Vimos como mostrar o `HiredDate` valor usando um controle de calendário.
- Mostra informações de status com base nos dados subjacentes. Enquanto o `Employees` tabela não contém uma coluna que retorna o número de dias que um funcionário estiver no trabalho, fomos capazes de exibir essas informações no exemplo GridView no tutorial anterior com o uso de um TemplateField e o método de formatação.

Os mesmos recursos de TemplateFields disponíveis com o GridView também estão disponíveis com o controle DetailsView. Neste tutorial, exibiremos um produto no momento usando um DetailsView que contém dois TemplateFields. O primeiro TemplateField combinará o `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` campos de dados em uma linha de DetailsView. O segundo TemplateField exibirá o valor da `Discontinued` campo, mas usará um método de formatação para exibir "Sim" se `Discontinued` é `true`e "Não", caso contrário.


[![Dois TemplateFields são usados para personalizar a exibição](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**Figura 1**: TemplateFields dois são usados para personalizar a exibição ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))


Vamos começar!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Etapa 1: Associação de dados de DetailsView

Conforme discutido no tutorial anterior, ao trabalhar com TemplateFields geralmente é mais fácil começar criando o controle DetailsView que contém apenas BoundFields e, em seguida, adicionar novo TemplateFields ou converter a BoundFields existente TemplateFields como necessário . Portanto, inicie este tutorial adicionando um DetailsView para a página por meio do Designer e a associação a um ObjectDataSource que retorna a lista de produtos. Essas etapas criará um DetailsView com BoundFields para cada um dos campos de valor não booleano do produto e um CheckBoxField para um campo de valor booliano (Descontinuado).

Abra o `DetailsViewTemplateField.aspx` página e arraste um DetailsView da caixa de ferramentas para o Designer. Na marca inteligente de DetailsView escolha para adicionar um novo controle ObjectDataSource que invoca o `ProductsBLL` da classe `GetProducts()` método.


[![Adicionar um novo controle ObjectDataSource que chama o método GetProducts()](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**Figura 2**: adicionar um novo controle ObjectDataSource que invoca o `GetProducts()` método ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))


Este relatório de remover o `ProductID`, `SupplierID`, `CategoryID`, e `ReorderLevel` BoundFields. Em seguida, reordenar os BoundFields para que o `CategoryName` e `SupplierName` BoundFields aparecem imediatamente após o `ProductName` BoundField. Fique à vontade para ajustar o `HeaderText` propriedades e propriedades de formatação para BoundFields como você desejar. Como com o GridView, essas edições BoundField nível podem ser executadas por meio da caixa de diálogo campos (pode ser acessada clicando no link Editar campos na marca inteligente de DetailsView) ou a sintaxe declarativa. Por fim, limpar o DetailsView `Height` e `Width` valores de propriedade para permitir que o DetailsView controlar para expandir com base nos dados exibidos e marque a caixa de seleção Habilitar paginação na marca inteligente.

Depois de fazer essas alterações, declarativo do seu controle DetailsView deve ser semelhante ao seguinte:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

Dedicar um tempo para exibir a página por meio de um navegador. Neste ponto, você verá um único produto listado (Chai) com linhas mostrando o nome do produto, categoria, fornecedor, preço, unidades em estoque, unidades em ordem e seu status descontinuados.


[![Detalhes do produto são mostrados usando uma série de BoundFields](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**Figura 3**: detalhes do produto a são mostrados usando uma série de BoundFields ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Etapa 2: Combinando o preço, unidades em estoque e unidades na ordem em uma linha

O DetailsView tem uma linha para o `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` campos. Podemos combinar esses campos de dados em uma única linha com um TemplateField adicionando um novo TemplateField ou convertendo um dos existentes `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` BoundFields em um TemplateField. Vamos praticar enquanto eu prefiro convertendo BoundFields existente, adicionando um novo TemplateField.

Inicie o clicando no link Editar campos a marca inteligente de DetailsView para exibir a caixa de diálogo de campos. Em seguida, adicione um novo TemplateField e defina seu `HeaderText` propriedade para "Price e estoque" e mover o novo TemplateField para que ele está posicionado acima de `UnitPrice` BoundField.


[![Adicionar um novo TemplateField ao controle DetailsView](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**Figura 4**: adicionar um novo TemplateField ao controle DetailsView ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))


Como esse novo TemplateField conterá os valores exibidos no momento o `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` BoundFields, vamos removê-los.

A última tarefa desta etapa é definir o `ItemTemplate` marcação para o preço e inventário TemplateField, que pode ser feito por meio do modelo de DetailsView interface no Designer ou manualmente por meio da sintaxe declarativa do controle de edição. Assim como em GridView, a interface de edição de modelo de DetailsView pode ser acessado clicando no link Editar modelos da marca inteligente. Aqui você pode selecionar o modelo para editar na lista suspensa e, em seguida, adicionar os controles da Web da caixa de ferramentas.

Para este tutorial, comece adicionando um controle de rótulo para o preço e do inventário TemplateField `ItemTemplate`. Em seguida, clique no link Editar DataBindings de marca inteligente do controle da Web de rótulo e associar o `Text` propriedade para o `UnitPrice` campo.


[![Associar a propriedade de texto do rótulo para o campo de dados UnitPrice](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**Figura 5**: associar o rótulo `Text` propriedade para o `UnitPrice` campo de dados ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>O preço de formatação como uma moeda

Com essa adição, o controle de rótulo Web preço e inventário TemplateField agora exibirá apenas o preço do produto selecionado. A Figura 6 mostra uma captura de tela de nosso progresso até o momento quando visualizada através de um navegador.


[![O preço e inventário TemplateField mostra o preço](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**Figura 6**: O preço e inventário TemplateField mostra o preço ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))


Observe que o preço do produto não está formatado como uma moeda. Com um BoundField formatação é possível definindo o `HtmlEncode` propriedade `false` e o `DataFormatString` propriedade `{0:formatSpecifier}`. Para um TemplateField, no entanto, as instruções de formatação devem ser especificadas na sintaxe de associação de dados ou com o uso de um método de formatação definido em algum lugar no código do aplicativo para (como classe de code-behind da página ASP.NET).

Para especificar a formatação para a sintaxe de associação de dados usada no controle de rótulo Web, retorne à caixa de diálogo DataBindings clicando no link Editar DataBindings de marca inteligente do rótulo. Você pode digitar as instruções de formatação diretamente na lista suspensa de formato ou selecione uma das cadeias de caracteres de formato definido. Como com o BoundField `DataFormatString` propriedade, a formatação é especificada usando `{0:formatSpecifier}`.

Para o `UnitPrice` campo use a formatação de moeda especificada, selecionando o valor apropriado da lista suspensa ou digitando `{0:C}` manualmente.


[![Formate o preço como uma moeda](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**Figura 7**: Formatar o preço como uma moeda ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))


Declarativamente, a especificação de formatação é indicada como um segundo parâmetro para o `Bind` ou `Eval` métodos. As configurações feitas por meio do resultado de Designer na expressão de associação de dados a seguir na marcação declarativa:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Adicionando campos de dados restantes para o TemplateField

Agora nós exibidos e formatados de `UnitPrice` dados campo no preço e TemplateField de inventário, mas ainda é necessário exibir o `UnitsInStock` e `UnitsOnOrder` campos. Vamos exibi-los em uma linha abaixo o preço e entre parênteses. Interface edição do modelo no Designer, tal marcação pode ser adicionada posicionando o cursor dentro do modelo e simplesmente digitar o texto a ser exibido. Como alternativa, essa marcação pode ser inserida diretamente na sintaxe declarativa.

Adicionar a marcação estática, controles da Web de rótulo e sintaxe de associação de dados para que o preço e inventário TemplateField exibe as informações de preço e inventário da seguinte forma:

*UnitPrice*  
(**Em estoque / na ordem:** *UnitsInStock* / *UnitsOnOrder*)

Depois de executar essa tarefa marcação declarativa de DetailsView deve ser semelhante ao seguinte:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

Com essas alterações, você consolidados as informações de preço e inventário em uma única linha de DetailsView.


[![O preço e informações de inventário são exibidos em uma única linha](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**Figura 8**: informações de inventário e o preço é exibido em uma única linha ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Etapa 3: Personalizando as informações de campo descontinuados

O `Products` da tabela `Discontinued` coluna é um valor de bit que indica se o produto foi descontinuado. Ao associar um DetailsView (ou GridView) a um controle de fonte de dados, como os campos de valor booliano, `Discontinued`, são implementados como CheckBoxFields enquanto os campos de valor não boolianos, como `ProductID`, `ProductName`e assim por diante, são implementados como BoundFields. CheckBoxField é renderizada como uma caixa de seleção desabilitada será verificada se o valor do campo de dados é verdadeiro e desmarcada, caso contrário.

Em vez de exibir CheckBoxField, desejamos em vez disso, exibir o texto que indica se o produto foi descontinuado ou não. Para fazer isso poderíamos remover CheckBoxField de DetailsView e, em seguida, adicione um BoundField cujo `DataField` propriedade foi definida como `Discontinued`. Dedique alguns momentos para fazer isso. Após essa alteração DetailsView mostra o texto "True" para os produtos descontinuados e "Falso" para os produtos que ainda estão ativos.


[![As cadeias de caracteres True e False são usadas para exibir o estado Descontinuado](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**Figura 9**: as cadeias de caracteres True e False são usados para exibir o estado Descontinuado ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))


Imagine que não queremos que as cadeias de caracteres "Verdadeiras" ou "False" a ser usada, mas "Sim" e "Não", em vez disso. Essa personalização pode ser executada com a Ajuda de um TemplateField e um método de formatação. Um método de formatação pode levar em qualquer número de parâmetros de entrada, mas deve retornar o HTML (como uma cadeia de caracteres) para injetar o modelo.

Adicionar um método de formatação para o `DetailsViewTemplateField.aspx` classe de code-behind da página chamada `DisplayDiscontinuedAsYESorNO` que aceita um valor booleano como um parâmetro de entrada e retorna uma cadeia de caracteres. Conforme discutido no tutorial anterior, este método *deve* ser marcado como `protected` ou `public` para que seja acessível a partir do modelo.


[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

Esse método verifica se o parâmetro de entrada (`discontinued`) e retorna "Sim" se ele for `true`, "Não", caso contrário.

> [!NOTE]
> No método formatação examinado com o cancelamento de tutorial anterior que foram passando um campo de dados que pode conter `NULL` s e, portanto, é necessário verificar se o funcionário `HiredDate` o valor da propriedade com um banco de dados `NULL` valor antes de acessando o `EmployeesRow`do `HiredDate` propriedade. Uma verificação desse tipo não é necessária aqui desde o `Discontinued` coluna não pode ter o banco de dados `NULL` valores atribuídos. Além disso, é por isso o método pode aceitar um valor booleano de entrada parâmetro em vez de precisar aceitar um `ProductsRow` instância ou um parâmetro de tipo `object`.


Com esse método de formatação completo, tudo o que resta é chamá-lo do TemplateField `ItemTemplate`. Para criar o TemplateField remover o `Discontinued` BoundField e adicione um TemplateField novo ou converta o `Discontinued` BoundField em um TemplateField. Em seguida, na exibição de marcação declarativa, edite o TemplateField para que ele contém apenas um ItemTemplate que invoca o `DisplayDiscontinuedAsYESorNO` método, passando o valor atual `ProductRow` da instância `Discontinued` propriedade. Isso pode ser acessado por meio de `Eval` método. Especificamente, a marcação do TemplateField deve parecer com:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

Isso fará com que o `DisplayDiscontinuedAsYESorNO` método a ser invocado durante a renderização de DetailsView, passando o `ProductRow` da instância `Discontinued` valor. Como o `Eval` método retorna um valor do tipo `object`, mas o `DisplayDiscontinuedAsYESorNO` método espera um parâmetro de entrada do tipo `bool`, é convertido a `Eval` métodos retornam um valor para `bool`. O `DisplayDiscontinuedAsYESorNO` método retornará "Sim" ou "Não" dependendo do valor que ele recebe. O valor retornado é o que é exibido neste DetailsView linha (consulte a Figura 10).


[![Os valores Sim ou não são agora são mostrados na linha Descontinuado](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**Figura 10**: Sim ou não valores são agora são mostrados na linha Descontinuado ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))


## <a name="summary"></a>Resumo

O TemplateField no controle DetailsView permite um maior grau de flexibilidade na exibição de dados que está disponível com os outros controles de campo e são ideais para situações em que:

- Vários campos de dados precisam ser exibidos em uma coluna de GridView
- Os dados é melhor expressa usando um controle da Web em vez de texto sem formatação
- A saída depende dos dados subjacentes, como exibir metadados ou no reformatar os dados

Enquanto TemplateFields permitir uma maior flexibilidade no processamento de dados subjacentes de DetailsView, a saída de DetailsView ainda parece um pouco boxy como cada campo é renderizado como uma linha em um HTML `<table>`.

O controle FormView oferece maior flexibilidade na configuração de saída renderizada. O FormView não contém campos, mas em vez disso, apenas uma série de modelos (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`e assim por diante). Veremos como usar o FormView para obter ainda mais o controle de layout renderizado em nosso tutorial Avançar.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Dan Jagers. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-templatefields-in-the-gridview-control-cs.md)
> [Próximo](using-the-formview-s-templates-cs.md)
