---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: Usando TemplateFields no controle DetailsView (VB) | Microsoft Docs
author: rick-anderson
description: Os mesmos recursos de TemplateFields disponíveis com o GridView também estão disponíveis com o controle DetailsView. Neste tutorial, vamos exibir de um produto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 29a73a5f22048cf3a80d9f7d23c0a4a7b89882f0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365387"
---
<a name="using-templatefields-in-the-detailsview-control-vb"></a>Usando TemplateFields no controle DetailsView (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe) ou [baixar PDF](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> Os mesmos recursos de TemplateFields disponíveis com o GridView também estão disponíveis com o controle DetailsView. Neste tutorial vamos exibir um produto por vez usando um DetailsView contendo TemplateFields.


## <a name="introduction"></a>Introdução

O TemplateField oferece um maior grau de flexibilidade na renderização de dados que o BoundField, CheckBoxField, HyperLinkField e outros controles de campo de dados. No [tutorial anterior](using-templatefields-in-the-gridview-control-vb.md) analisamos usando o TemplateField em um GridView para:

- Exiba vários valores de campo de dados em uma coluna. Especificamente, ambas as `FirstName` e `LastName` campos foram combinados em uma coluna de GridView.
- Use um controle da Web alternativo para expressar um valor de campo de dados. Vimos como mostrar a `HiredDate` valor usando um controle de calendário.
- Mostre informações de status com base nos dados subjacentes. Enquanto o `Employees` tabela não contém uma coluna que retorna o número de dias que um funcionário estiver no trabalho, fomos capazes de exibir essas informações no exemplo GridView no tutorial anterior com o uso de um TemplateField e o método de formatação.

Os mesmos recursos de TemplateFields disponíveis com o GridView também estão disponíveis com o controle DetailsView. Neste tutorial vamos exibir um produto por vez usando um que contém dois TemplateFields DetailsView. O TemplateField primeiro combinará os `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` campos de dados em uma linha DetailsView. O segundo TemplateField exibirá o valor da `Discontinued` campo, mas usará um método de formatação para exibir "Sim" se `Discontinued` é `True`e "Não", caso contrário.


[![Dois TemplateFields são usados para personalizar a exibição](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**Figura 1**: TemplateFields dois são usados para personalizar a exibição ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))


Vamos começar!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Etapa 1: Associando dados a DetailsView

Conforme discutido no tutorial anterior, ao trabalhar com TemplateFields geralmente é mais fácil começar criando o controle DetailsView que contém apenas BoundFields e, em seguida, adicionar novos TemplateFields ou converter a BoundFields existente em TemplateFields como necessário . Portanto, inicie este tutorial adicionando um DetailsView para a página por meio do Designer e associação a um ObjectDataSource que retorna a lista de produtos. Essas etapas criará um DetailsView com BoundFields para cada um dos campos de valor não booleano do produto e um CheckBoxField para um campo de valor booliano (Descontinuado).

Abra o `DetailsViewTemplateField.aspx` página e arraste um DetailsView da caixa de ferramentas para o Designer. Na marca inteligente de DetailsView optar por adicionar um novo controle ObjectDataSource invoca o `ProductsBLL` da classe `GetProducts()` método.


[![Adicionar um novo controle ObjectDataSource que invoca o método GetProducts()](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**Figura 2**: adicionar um novo controle ObjectDataSource que invoca a `GetProducts()` método ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))


Para este relatório, remova os `ProductID`, `SupplierID`, `CategoryID`, e `ReorderLevel` BoundFields. Em seguida, reordenar o BoundFields para que o `CategoryName` e `SupplierName` BoundFields aparecer imediatamente após o `ProductName` BoundField. Fique à vontade ajustar o `HeaderText` propriedades e propriedades de formatação para o BoundFields como você considerar adequado. Como com o controle GridView, essas edições BoundField nível podem ser executadas por meio da caixa de diálogo de campos (pode ser acessada clicando no link Editar campos na marca inteligente de DetailsView) ou através de sintaxe declarativa. Por fim, limpar o DetailsView `Height` e `Width` valores de propriedade para permitir que o DetailsView controlar para expandir com base nos dados exibidos e marque a caixa de seleção Habilitar paginação na marca inteligente.

Depois de fazer essas alterações, marcação declarativa do seu controle DetailsView deve ser semelhante ao seguinte:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

Reserve um tempo para exibir a página por meio de um navegador. Neste ponto, você verá um único produto listado (Chai) com linhas que mostra o nome do produto, categoria, fornecedor, preço, unidades em estoque, unidades em ordem e seu status descontinuados.


[![Detalhes do produto são mostrados usando uma série de BoundFields](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**Figura 3**: detalhes do produto a são mostrados usando uma série de BoundFields ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Etapa 2: Combinando o preço, unidades em estoque e unidades no pedido em uma linha

DetailsView tem uma linha para o `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` campos. Podemos combinar esses campos de dados em uma única linha com um TemplateField adicionando um novo TemplateField ou convertendo um dos existentes `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` BoundFields em um TemplateField. Vamos praticar enquanto eu, pessoalmente, prefiro convertendo BoundFields existente, adicionando um novo TemplateField.

Comece clicando no link Editar campos na marca inteligente de DetailsView para abrir a caixa de diálogo de campos. Em seguida, adicione um novo TemplateField e defina suas `HeaderText` propriedade para "Preços e estoque" e mover o TemplateField novo para que ele está posicionado acima de `UnitPrice` BoundField.


[![Adicionar Novo TemplateField no controle DetailsView](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**Figura 4**: adicionar um novo TemplateField para o controle DetailsView ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))


Desde que essa nova TemplateField conterá os valores exibidos no momento a `UnitPrice`, `UnitsInStock`, e `UnitsOnOrder` BoundFields, vamos removê-los.

A última tarefa para esta etapa é definir o `ItemTemplate` marcação para o preço e o TemplateField de inventário, que pode ser feito por meio do modelo de DetailsView interface no Designer ou manualmente por meio de sintaxe declarativa do controle de edição. Assim como acontece com o controle GridView, interface de edição de modelo de DetailsView pode ser acessado clicando no link Editar modelos na marca inteligente. A partir daqui, você pode selecionar o modelo para editar na lista suspensa e, em seguida, adicionar todos os controles da Web na caixa de ferramentas.

Para este tutorial, comece por adicionar um controle de rótulo para o preço e do inventário TemplateField `ItemTemplate`. Em seguida, clique no link Editar DataBindings de smart tag do controle de Web Label e associar a `Text` propriedade para o `UnitPrice` campo.


[![Associar a propriedade de texto do rótulo para o campo de dados de UnitPrice](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**Figura 5**: associar o rótulo `Text` propriedade para o `UnitPrice` campo de dados ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>O preço de formatação como uma moeda

Com esse acréscimo, o controle de rótulo Web preço e o inventário TemplateField agora exibirá apenas o preço para o produto selecionado. Figura 6 mostra uma captura de tela de nosso progresso até o momento quando visualizado por meio de um navegador.


[![O preço e inventário TemplateField mostra o preço](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**Figura 6**: O preço e inventário TemplateField mostra o preço ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))


Observe que o preço do produto não está formatado como uma moeda. Com um BoundField formatação é possível, definindo o `HtmlEncode` propriedade para `False` e o `DataFormatString` propriedade `{0:formatSpecifier}`. Para um TemplateField, no entanto, quaisquer instruções de formatação devem ser especificadas na sintaxe de associação de dados ou com o uso de um método de formatação definido em qualquer lugar em que o código do aplicativo (como em classe de code-behind da página ASP.NET).

Para especificar a formatação para a sintaxe de associação de dados usada no controle da Web de rótulo, retorne à caixa de diálogo DataBindings clicando no link Editar DataBindings de marca inteligente do rótulo. Você pode digitar as instruções de formatação diretamente na lista suspensa de formato ou selecione uma das cadeias de caracteres de formato definido. Como com o BoundField `DataFormatString` propriedade, a formatação é especificada usando `{0:formatSpecifier}`.

Para o `UnitPrice` campo use a formatação de moeda especificada, selecionando o valor apropriado na lista suspensa ou digitando na `{0:C}` manualmente.


[![O preço de formato como uma moeda](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**Figura 7**: o preço como uma moeda de formato ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))


Declarativamente, a especificação de formatação é indicada como um segundo parâmetro para o `Bind` ou `Eval` métodos. As configurações feitas por meio de resultados de Designer na expressão de associação de dados a seguir na marcação declarativa:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Adicionando os campos de dados restantes para o TemplateField

Agora nós exibidos e formatados de `UnitPrice` dados de campo no preço e TemplateField de inventário, mas ainda precisa exibir o `UnitsInStock` e `UnitsOnOrder` campos. Vamos exibi-los em uma linha abaixo o preço e entre parênteses. Na interface edição do modelo no Designer, essa marcação pode ser adicionada posicionando o cursor dentro do modelo e simplesmente digitar o texto a ser exibido. Como alternativa, essa marcação pode ser inserida diretamente na sintaxe declarativa.

Adicione a marcação estática, controles de Web Label e sintaxe de associação de dados para que o preço e inventário TemplateField exibe as informações de preço e de estoque da seguinte forma:

*UnitPrice*  
(**Em estoque / na ordem:** *UnitsInStock* / *UnitsOnOrder*)

Depois de executar essa tarefa marcação declarativa de ovládacího prvku DetailsView deve ser semelhante ao seguinte:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

Com essas alterações consolidamos as informações de preços e estoque em uma única linha DetailsView.


[![O preço e informações de inventário são exibidos em uma única linha](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**Figura 8**: O preço e informações de inventário são exibidos em uma única linha ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Etapa 3: Personalizar as informações de campo Descontinuado

O `Products` da tabela `Discontinued` coluna é um valor de bit que indica se o produto foi descontinuado. Ao associar um DetailsView (ou GridView) a um controle de fonte de dados, os campos de valor booliano, como `Discontinued`, são implementados como CheckBoxFields, enquanto os campos de valor não boolianos, como `ProductID`, `ProductName`e assim por diante, são implementados como BoundFields. CheckBoxField é renderizado como uma caixa de seleção desabilitada será verificada se o valor do campo de dados é verdadeiro e não verificado, caso contrário.

Em vez de exibir o CheckBoxField podemos querer em vez disso, exibir o texto que indica se o produto foi descontinuado. Para fazer isso pudéssemos remover CheckBoxField de DetailsView e, em seguida, adicione um BoundField cujos `DataField` propriedade foi definida como `Discontinued`. Reserve um tempo para fazer isso. Após essa alteração DetailsView mostra o texto "True" para produtos descontinuados e "False" para os produtos que ainda estão ativos.


[![As cadeias de caracteres True e False são usadas para exibir o estado Descontinuado](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**Figura 9**: as cadeias de caracteres True e False são usados para exibir o estado de Discontinued ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))


Imagine que não queremos que as cadeias de caracteres "True" ou "False" para ser usado, mas "Sim" e "Não", em vez disso. Tal personalização pode ser executada com a Ajuda de um TemplateField e um método de formatação. Um método de formatação pode levar em qualquer número de parâmetros de entrada, mas deve retornar HTML (como uma cadeia de caracteres) para injetar no modelo.

Adicionar um método de formatação para o `DetailsViewTemplateField.aspx` classe de code-behind da página denominada `DisplayDiscontinuedAsYESorNO` que aceita um `Northwind.ProductsRow` objeto como um parâmetro de entrada e retorna uma cadeia de caracteres. Conforme discutido no tutorial anterior, esse método *devem* ser marcado como `Protected` ou `Public` para que seja acessível a partir do modelo.


[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

Esse método verifica o parâmetro de entrada (`discontinued`) e retorna "Sim" se ele for `True`, "Não", caso contrário.

> [!NOTE]
> No método de formatação examinado no recall de tutorial anterior que estava passando um campo de dados que pode conter `NULL` s e, portanto, é necessário para verificar se o funcionário `HiredDate` o valor da propriedade tinha um banco de dados `NULL` valor antes acessando o `EmployeesRow`do `HiredDate` propriedade. Essa verificação não é necessária aqui desde o `Discontinued` coluna nunca pode ter o banco de dados `NULL` valores atribuídos. Além disso, é por isso o método pode aceitar um valor booliano de entrada parâmetro em vez de precisar aceitar uma `ProductsRow` instância ou um parâmetro de tipo `Object`.


Com esse método de formatação completo, tudo o que resta é chamá-la do TemplateField `ItemTemplate`. Para criar o TemplateField remover as `Discontinued` BoundField e adicionar um novo TemplateField ou converter o `Discontinued` BoundField em um TemplateField. Em seguida, na exibição de marcação declarativa, edite o TemplateField para que ele contenha apenas um ItemTemplate que invoca a `DisplayDiscontinuedAsYESorNO` método, passando o valor do atual `ProductRow` da instância `Discontinued` propriedade. Isso pode ser acessado por meio de `Eval` método. Especificamente, a marcação do TemplateField deve parecer com:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

Isso fará com que o `DisplayDiscontinuedAsYESorNO` método a ser invocado durante a renderização DetailsView, passando a `ProductRow` da instância `Discontinued` valor. Desde que o `Eval` método retorna um valor do tipo `Object`, mas o `DisplayDiscontinuedAsYESorNO` método espera um parâmetro de entrada do tipo `Boolean`, podemos converter a `Eval` métodos retornam o valor a ser `Boolean`. O `DisplayDiscontinuedAsYESorNO` método, em seguida, retorna "YES" ou "Não", dependendo do valor que ele recebe. O valor retornado é o que é exibido neste DetailsView linha (veja a Figura 10).


[![Os valores Sim ou não são agora são mostrados na linha Descontinuado](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**Figura 10**: os valores Sim ou não são agora são mostrados na linha Discontinued ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))


## <a name="summary"></a>Resumo

O TemplateField no controle DetailsView permite um maior grau de flexibilidade na exibição de dados que está disponível com os outros controles de campo e são ideais para situações em que:

- Vários campos de dados precisam ser exibidos em uma coluna de GridView
- Os dados é melhor expressa usando um controle da Web em vez de texto sem formatação
- A saída depende dos dados subjacentes, como a exibição de metadados ou em reformatar os dados

Embora TemplateFields permitam uma maior flexibilidade no processamento de dados subjacentes de DetailsView, a saída de DetailsView ainda parece um pouco boxy como cada campo é processado como uma linha em um HTML `<table>`.

O controle FormView oferece um maior grau de flexibilidade para configurar a saída renderizada. FormView não contém campos, mas em vez disso, apenas uma série de modelos (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`e assim por diante). Veremos como usar o FormView para obter ainda mais controle de layout renderizado em nosso próximo tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Dan Jagers. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-templatefields-in-the-gridview-control-vb.md)
> [Próximo](using-the-formview-s-templates-vb.md)
