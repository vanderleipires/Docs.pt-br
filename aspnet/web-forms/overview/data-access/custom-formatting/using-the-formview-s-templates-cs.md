---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
title: Usando os modelos de FormView (c#) | Microsoft Docs
author: rick-anderson
description: Ao contrário de DetailsView, FormView não é composto de campos. Em vez disso, FormView é renderizado usando modelos. Neste tutorial, examinaremos usando a F....
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d3f062af-88cf-426d-af44-e41f32c41672
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a7cf17d8cbd0a5a17a387b9a70336a1b06efde7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830390"
---
<a name="using-the-formviews-templates-c"></a>Usando os modelos de FormView (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe) ou [baixar PDF](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)

> Ao contrário de DetailsView, FormView não é composto de campos. Em vez disso, FormView é renderizado usando modelos. Neste tutorial, que vamos examinar usando o controle FormView para apresentar uma exibição menos rígida de dados.


## <a name="introduction"></a>Introdução

Nos dois últimos tutoriais que vimos como personalizar as saídas dos controles GridView e DetailsView Usando TemplateFields. TemplateFields permitir para o conteúdo para um campo específico ser altamente personalizados, mas no final o GridView e DetailsView têm uma aparência boxy em vez disso, como grade. Para muitos cenários de tal um layout de grade é ideal, mas às vezes uma exibição mais fluida, menos rígida é necessária. Ao exibir um único registro, tal um layout fluido é possível usar o controle FormView.

Ao contrário de DetailsView, FormView não é composto de campos. É possível adicionar um BoundField ou o TemplateField para um FormView. Em vez disso, FormView é renderizado usando modelos. Pense FormView como um controle DetailsView que contém um TemplateField único. FormView suporta os seguintes modelos:

- `ItemTemplate` usado para renderizar o registro específico exibido FormView
- `HeaderTemplate` usado para especificar uma linha de cabeçalho opcional
- `FooterTemplate` usado para especificar uma linha de rodapé opcional
- `EmptyDataTemplate` Quando o FormView `DataSource` não tem quaisquer registros, o `EmptyDataTemplate` é usado em vez do `ItemTemplate` para renderizar a marcação do controle
- `PagerTemplate` pode ser usado para personalizar a interface de paginação para FormViews com paginação habilitada
- `EditItemTemplate` / `InsertItemTemplate` usado para personalizar a interface de edição ou inserção para FormViews que dão suporte a essa funcionalidade

Neste tutorial, que vamos examinar usando o controle FormView para apresentar uma exibição menos rígida de produtos. Em vez de ter campos para o nome, categoria, fornecedor e da assim por diante, FormView `ItemTemplate` mostrará esses valores usando uma combinação de um elemento de cabeçalho e um `<table>` (veja a Figura 1).


[![FormView forçadas do Layout de grade como visto em DetailsView](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)

**Figura 1**: FormView foge do Layout de Grid-Like visto em DetailsView ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Etapa 1: Associando dados a FormView

Abra o `FormView.aspx` página e arraste um FormView da caixa de ferramentas para o Designer. Quando adicionar pela primeira vez FormView ele aparece como uma caixa cinza, instruindo-nos que um `ItemTemplate` é necessária.


[![FormView não pode ser renderizado no Designer, até que um ItemTemplate seja fornecido](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)

**Figura 2**: não o da FormView pode ser renderizado no Designer de até uma `ItemTemplate` é fornecido ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-cs/_static/image6.png))


O `ItemTemplate` possam ser criados manualmente (usando a sintaxe declarativa) ou pode ser criado automaticamente ao associar FormView para um controle de fonte de dados por meio do Designer. Isso é criado automaticamente `ItemTemplate` contém o HTML que lista o nome de cada campo e um rótulo de controle cuja `Text` propriedade está associada ao valor do campo. Essa abordagem também criará um `InsertItemTemplate` e `EditItemTemplate`, sendo que ambos são preenchidos com controles de entrada para cada um dos campos de dados retornados pelo controle de fonte de dados.

Se você quiser criar automaticamente o modelo, na marca inteligente de FormView adicionar um novo controle ObjectDataSource invoca o `ProductsBLL` da classe `GetProducts()` método. Isso criará um FormView com um `ItemTemplate`, `InsertItemTemplate`, e `EditItemTemplate`. Na exibição da fonte, remova os `InsertItemTemplate` e `EditItemTemplate` , pois não estamos interessados em Criando um FormView que dá suporte à edição ou inserindo ainda. Em seguida, desmarque out a marcação dentro de `ItemTemplate` para que tenhamos uma ficha limpa para trabalhar.

Se você preferir criar o `ItemTemplate` manualmente, você pode adicionar e configurar o ObjectDataSource, arrastando-o na caixa de ferramentas para o Designer. No entanto, não defina fonte de dados de FormView do Designer. Em vez disso, vá para a exibição da fonte e definir manualmente o FormView `DataSourceID` propriedade para o `ID` valor do ObjectDataSource. Em seguida, adicione manualmente o `ItemTemplate`.

Independentemente de qual abordagem você decidiu levar, no momento marcação declarativa de seu FormView deve a aparência:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample1.aspx)]

Dedique uns momentos para verificar a caixa de seleção Habilitar paginação na marca inteligente de FormView; Isso adicionará o `AllowPaging="True"` de atributo para a sintaxe declarativa de FormView. Além disso, defina o `EnableViewState` a propriedade como False.

## <a name="step-2-defining-theitemtemplates-markup"></a>Etapa 2: Definir o`ItemTemplate`da marcação

Com o FormView associada ao controle ObjectDataSource e configurado para dar suporte à paginação, estamos prontos para especificar o conteúdo para o `ItemTemplate`. Para este tutorial, vamos dar o nome do produto exibido em um `<h3>` título. Depois disso, vamos usar um HTML `<table>` para exibir as propriedades remanescentes do produto em uma tabela de quatro colunas em que a primeira coluna lista os nomes de propriedade e o segundo e quarto listam seus valores.

Essa marcação pode ser inserida por meio da interface de edição de modelo de FormView no Designer ou manualmente por meio da sintaxe declarativa. Ao trabalhar com modelos normalmente considero mais rápido para trabalhar diretamente com a sintaxe declarativa, mas fique à vontade para usar qualquer técnica que você está mais familiarizado.

A marcação a seguir mostra a marcação declarativa de FormView após o `ItemTemplate`da estrutura foi concluída:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample2.aspx)]

Observe que a sintaxe de associação de dados - `<%# Eval("ProductName") %>`, por exemplo pode ser injetado diretamente para a saída do modelo. Ou seja, ele não precisa ser atribuído a um controle de rótulo `Text` propriedade. Por exemplo, temos a `ProductName` valor exibido em um `<h3>` usando o elemento `<h3><%# Eval("ProductName") %></h3>`, que, para o produto Chai será renderizado como `<h3>Chai</h3>`.

O `ProductPropertyLabel` e `ProductPropertyValue` classes CSS são usadas para especificar o estilo dos nomes de propriedade do produto e valores no `<table>`. Essas classes CSS são definidas no `Styles.css` e fazer com que os nomes de propriedade estar em negrito e alinhado à direita e adicionar um direito de preenchimento para os valores de propriedade.

Como não há nenhum CheckBoxFields disponíveis com o FormView para mostrar o `Discontinued` valor como uma caixa de seleção devemos adicionar nosso próprio controle de caixa de seleção. O `Enabled` estiver definida como False, tornando-o somente leitura e a caixa de seleção `Checked` propriedade está associada ao valor da `Discontinued` campo de dados.

Com o `ItemTemplate` concluída, as informações de produto são exibidas de maneira muito mais fluida. Compare a saída de DetailsView do último tutorial (Figura 3) com a saída gerada pela FormView neste tutorial (Figura 4).


[![A saída de DetailsView rígida](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)

**Figura 3**: A saída de DetailsView rígida ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-cs/_static/image9.png))


[![A saída de FormView fluidos](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)

**Figura 4**: A saída de FormView fluido ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-cs/_static/image12.png))


## <a name="summary"></a>Resumo

Enquanto os controles GridView e DetailsView podem ter sua saída personalizada usando TemplateFields, ambos ainda apresentam seus dados em um formato boxy como grade. Para ocasiões quando um único registro precisa ser mostrada usando um layout menos rígido, FormView é uma opção ideal. Como o DetailsView FormView renderiza um registro individual de sua `DataSource`, mas ao contrário de DetailsView é composto apenas de modelos e não oferece suporte a campos.

Como vimos neste tutorial, permite que o FormView para um layout mais flexível ao exibir um único registro. Em tutoriais futuros, examinaremos os controles DataList e Repeater, que fornecem o mesmo nível de flexibilidade como o FormsView, mas são capazes de exibir vários registros (como GridView).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi E.R. Gomes. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-templatefields-in-the-detailsview-control-cs.md)
> [Próximo](displaying-summary-information-in-the-gridview-s-footer-cs.md)
