---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: Usando os modelos de FormView (VB) | Microsoft Docs
author: rick-anderson
description: Ao contrário de DetailsView FormView não é composto de campos. Em vez disso, o FormView é renderizado usando modelos. Neste tutorial, examinaremos usando a F...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: 16293960f5d8758c93646844bd159547f5e0f38c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-formviews-templates-vb"></a>Usando os modelos de FormView (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) ou [baixar PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> Ao contrário de DetailsView FormView não é composto de campos. Em vez disso, o FormView é renderizado usando modelos. Neste tutorial, examinaremos usando o controle FormView para apresentar uma exibição menos rígida de dados.


## <a name="introduction"></a>Introdução

Nos dois últimos tutoriais, vimos como personalizar saídas dos controles GridView e DetailsView Usando TemplateFields. TemplateFields permitir para o conteúdo de um campo específico seja altamente personalizada, mas no final de GridView e DetailsView tem uma aparência boxy em vez disso, a grade. Para muitos cenários de tal um layout de grade é ideal, mas às vezes uma exibição mais fluida, menos rígida é necessária. Ao exibir um único registro, tal um layout fluido é possível usar o controle FormView.

Ao contrário de DetailsView FormView não é composto de campos. Você não pode adicionar um BoundField ou TemplateField para um FormView. Em vez disso, o FormView é renderizado usando modelos. Pense em FormView como um controle DetailsView que contém um TemplateField único. O FormView suporta os seguintes modelos:

- `ItemTemplate` usado para renderizar o registro específico exibido em FormView
- `HeaderTemplate` usado para especificar uma linha de cabeçalho opcional
- `FooterTemplate` usado para especificar uma linha de rodapé opcional
- `EmptyDataTemplate` Quando o FormView `DataSource` não tem nenhum registro a `EmptyDataTemplate` é usado em vez do `ItemTemplate` para renderizar a marcação do controle
- `PagerTemplate` pode ser usado para personalizar a interface de paginação para FormViews com paginação habilitada
- `EditItemTemplate` / `InsertItemTemplate` usado para personalizar a interface de edição ou inserção para FormViews que dão suporte a essa funcionalidade

Neste tutorial, examinaremos usando o controle FormView para apresentar uma exibição menos rígida de produtos. Em vez de campos para o nome, categoria, fornecedor e do assim por diante, FormView `ItemTemplate` mostrará esses valores usando uma combinação de um elemento de cabeçalho e um `<table>` (consulte a Figura 1).


[![O FormView forçadas do Layout de grade visto em DetailsView](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**Figura 1**: FormView quebras fora o Layout de Grid-Like visto em DetailsView ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Etapa 1: Associação de dados em FormView

Abra o `FormView.aspx` página e arraste um FormView da caixa de ferramentas para o Designer. Quando adiciona pela primeira vez de FormView aparece como uma caixa cinza, instruindo nos que um `ItemTemplate` é necessária.


[![O FormView não pode ser renderizado no Designer de até que um ItemTemplate seja fornecido](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**Figura 2**: não de FormView a ser renderizado no Designer de até uma `ItemTemplate` é fornecido ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-vb/_static/image6.png))


O `ItemTemplate` possam ser criados manualmente (usando a sintaxe declarativa) ou podem ser criadas automaticamente ao associar o FormView para um controle de fonte de dados por meio do Designer. Isso é criado automaticamente `ItemTemplate` contém HTML que lista o nome de cada campo e um rótulo de controle cuja `Text` propriedade está associada ao valor do campo. Essa abordagem também criará uma `InsertItemTemplate` e `EditItemTemplate`, ambos são populados com controles de entrada para cada um dos campos de dados retornados pelo controle de fonte de dados.

Se você quiser criar automaticamente o modelo, de marca inteligente de FormView adicionar um novo controle ObjectDataSource que invoca o `ProductsBLL` da classe `GetProducts()` método. Isso criará um FormView com um `ItemTemplate`, `InsertItemTemplate`, e `EditItemTemplate`. Na exibição da fonte, remova o `InsertItemTemplate` e `EditItemTemplate` como não estamos interessados na criação de um FormView que oferece suporte a editando ou inserindo ainda. Em seguida, desmarque-out a marcação dentro de `ItemTemplate` para que tenhamos a partir do zero para trabalhar.

Se você preferir criar de `ItemTemplate` manualmente, você pode adicionar e configurar o ObjectDataSource arrastando-o na caixa de ferramentas para o Designer. No entanto, não defina fonte de dados de FormView do Designer. Em vez disso, vá para a exibição da fonte e definir manualmente o FormView `DataSourceID` propriedade para o `ID` valor do ObjectDataSource. Em seguida, adicione manualmente o `ItemTemplate`.

Independentemente de qual abordagem você decidiu levar, neste momento marcação declarativa de FormView deve o seguinte:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

Reserve um tempo para verificar a caixa de seleção Habilitar paginação em marca inteligente de FormView; Isso adicionará o `AllowPaging="True"` atributo sintaxe declarativa de FormView. Além disso, defina o `EnableViewState` a propriedade como False.

## <a name="step-2-defining-theitemtemplates-markup"></a>Etapa 2: Definir o`ItemTemplate`da marcação

O FormView associada ao controle ObjectDataSource e configurado para dar suporte a paginação estamos prontos para especificar o conteúdo para o `ItemTemplate`. Para este tutorial, vamos dar o nome do produto exibido em um `<h3>` título. Depois disso, vamos usar um HTML `<table>` para exibir as propriedades remanescentes do produto em uma tabela de quatro colunas em que a primeira coluna lista os nomes de propriedade e a segunda e a quarta listam seus valores.

Essa marcação pode ser inserida por meio da interface de edição de modelo de FormView no Designer ou manualmente por meio de sintaxe declarativa. Ao trabalhar com modelos, geralmente mais rápido trabalhar diretamente com a sintaxe declarativa, mas fique à vontade para usar qualquer técnica que você está mais familiarizado.

A marcação a seguir mostra a marcação declarativa FormView após o `ItemTemplate`da estrutura tiver sido concluída:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

Observe que a sintaxe de associação de dados - `<%# Eval("ProductName") %>`para o exemplo pode ser inserido diretamente na saída do modelo. Ou seja, ele não precisa ser atribuído a um controle de rótulo `Text` propriedade. Por exemplo, é necessário o `ProductName` valor exibido em um `<h3>` usando o elemento `<h3><%# Eval("ProductName") %></h3>`, que, para o produto Chai será processada como `<h3>Chai</h3>`.

O `ProductPropertyLabel` e `ProductPropertyValue` classes CSS são usados para especificar o estilo dos nomes de propriedade de produto e valores de `<table>`. Essas classes CSS são definidas em `Styles.css` e fazer com que os nomes de propriedade em negrito e alinhado à direita e adicionar um direito de preenchimento para os valores de propriedade.

Como não há nenhum CheckBoxFields disponíveis com o FormView, para mostrar o `Discontinued` valor como uma caixa de seleção devemos adicionar nosso próprio controle de caixa de seleção. O `Enabled` propriedade é definida como False, tornando-o somente leitura e a caixa de seleção `Checked` propriedade está associada ao valor da `Discontinued` campo de dados.

Com o `ItemTemplate` concluída, as informações de produto são exibidas de maneira muito mais fluida. Compare a saída de DetailsView do último tutorial (Figura 3) com a saída gerada pela FormView neste tutorial (Figura 4).


[![A saída de DetailsView rígida](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**Figura 3**: A saída de DetailsView rígida ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-vb/_static/image9.png))


[![A saída de FormView fluido](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**Figura 4**: A saída de FormView fluidos ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-vb/_static/image12.png))


## <a name="summary"></a>Resumo

Enquanto os controles GridView e DetailsView podem ter sua saída personalizada usando TemplateFields, ambos ainda apresentam seus dados em um formato de grade, boxy. Para ocasiões quando um único registro precisa ser mostrada usando um layout menos rígido, FormView é uma opção ideal. Como o DetailsView FormView processa um único registro de seu `DataSource`, mas ao contrário de DetailsView é composto apenas de modelos e não oferece suporte a campos.

Como vimos neste tutorial, FormView permite um layout mais flexível ao exibir um único registro. No futuro, tutoriais, examinaremos os controles DataList e repetidor, que fornecem o mesmo nível de flexibilidade como o FormsView, mas podem exibir vários registros (como GridView).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi E.R. Gomes. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-templatefields-in-the-detailsview-control-vb.md)
> [Próximo](displaying-summary-information-in-the-gridview-s-footer-vb.md)
