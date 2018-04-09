---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: Exibindo vários registros por linha com o controle DataList (VB) | Microsoft Docs
author: rick-anderson
description: Este breve tutorial exploraremos como personalizar o layout do DataList através de suas propriedades RepeatColumns e RepeatDirection.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c85e5a1d7b88a9ed53ed8392a300d5118363bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Exibindo vários registros por linha com o controle DataList (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) ou [baixar PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> Este breve tutorial exploraremos como personalizar o layout do DataList através de suas propriedades RepeatColumns e RepeatDirection.


## <a name="introduction"></a>Introdução

Os exemplos de DataList é var visto nos últimos dois tutoriais tiver renderizado cada registro da fonte de dados como uma linha em uma única coluna HTML `<table>`. Embora esse seja o comportamento de DataList padrão, é muito fácil personalizar a exibição de DataList, de modo que os itens de fonte de dados são distribuídos entre uma tabela de várias coluna, várias linhas. Além disso, ele s possível ter todos os dados da fonte de itens exibidos em uma única linha, várias coluna DataList.

Podemos personalizar o layout de DataList s por meio de seu `RepeatColumns` e `RepeatDirection` propriedades, que, respectivamente, indicam o número de colunas é renderizado e se esses itens são apresentados verticalmente ou horizontalmente. Figura 1, por exemplo, mostra uma DataList que exibe informações sobre o produto em uma tabela com três colunas.


[![DataList mostra três produtos por linha](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Figura 1**: O DataList mostra três produtos por linha ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


Mostrando vários itens de fonte de dados por linha, DataList pode utilizar com mais eficiência espaço horizontal da tela. Este breve tutorial exploraremos essas duas propriedades DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Etapa 1: Exibir informações de produto em DataList

Antes de examinar o `RepeatColumns` e `RepeatDirection` propriedades, permitem s primeiro criar uma DataList em nossa página que lista informações sobre o produto usando o layout de tabela padrão de coluna única, várias linhas. Neste exemplo, permitem s exibe a s nome do produto, a categoria e o preço usando a seguinte marcação:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Podemos ver visto como associar dados a uma DataList nos exemplos anteriores, portanto passarei através dessas etapas rapidamente. Comece abrindo o `RepeatColumnAndDirection.aspx` página o `DataListRepeaterBasics` pasta e arraste uma DataList da caixa de ferramentas para o Designer. Marca inteligente DataList s, escolher para criar um novo ObjectDataSource e configurá-lo para receber seus dados a partir de `ProductsBLL` classe s `GetProducts` método, escolhendo (nenhum) a opção do assistente s INSERT, UPDATE e excluir guias.

Depois de criar e associar o novo ObjectDataSource para DataList, Visual Studio criará automaticamente um `ItemTemplate` que exibe o nome e valor para cada um dos campos de dados de produto. Ajustar o `ItemTemplate` diretamente por meio de marcação declarativa ou de editar modelos de opção na marca inteligente DataList s para que ele usa a marcação mostrada acima, substituindo o *nome do produto*, *nome da categoria* , e *preço* texto com controles de rótulo que usam a sintaxe de associação de dados apropriado para atribuir valores para seus `Text` propriedades. Depois de atualizar o `ItemTemplate`, sua marcação declarativa s de página deve ser semelhante à seguinte:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Observe que, var incluídos em um especificador de formato o `Eval` sintaxe de associação de dados para o `UnitPrice`, formatar o valor retornado como uma moeda - `Eval("UnitPrice", "{0:C}").`

Reserve um momento para visitar a página em um navegador. Como mostra a Figura 2, DataList processado como uma tabela de coluna única, várias linhas de produtos.


[![Por padrão, a DataList é renderizada como uma tabela de coluna única, várias linhas](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Figura 2**: por padrão, o DataList é renderizada como uma única coluna, tabela de várias linhas ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Etapa 2: Alterar a direção do Layout s DataList

Enquanto o comportamento padrão para o DataList é organizar seus itens verticalmente em uma tabela de coluna única, várias linhas, esse comportamento pode ser alterado facilmente por meio do DataList s [ `RepeatDirection` propriedade](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). O `RepeatDirection` propriedade pode aceitar um dos dois valores possíveis: `Horizontal` ou `Vertical` (o padrão).

Alterando o `RepeatDirection` propriedade `Vertical` para `Horizontal`, DataList processa seus registros em uma única linha, a criação de uma coluna por item de fonte de dados. Para ilustrar esse efeito, clique em DataList no Designer e, na janela Propriedades, altere o `RepeatDirection` propriedade `Vertical` para `Horiztonal`. Imediatamente após fazer isso, o Designer ajusta o layout de DataList s, criando uma interface de linha única, várias coluna (consulte a Figura 3).


[![Os itens de RepeatDirection propriedade determina como a direção de DataList s são apresentados Out](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Figura 3**: O `RepeatDirection` propriedade determina como os itens de direção de DataList s são apresentados Out ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


Quando a exibição de pequenas quantidades de dados, uma única linha, várias coluna tabela pode ser uma maneira ideal para maximizar o espaço na tela. Para volumes maiores de dados, no entanto, uma única linha exigirá várias colunas, que coloca os itens que o pode caber na tela de para a direita. A Figura 4 mostra os produtos quando renderizado em uma única linha DataList. Como há muitos produtos (80), o usuário terá que rolar para a direita para exibir informações sobre cada um dos produtos.


[![Para fontes de dados for grande o suficiente, uma única coluna DataList exigirá a rolagem Horizontal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Figura 4**: para suficientemente grandes fontes de dados, uma única coluna DataList será exigem rolagem Horizontal ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Etapa 3: Exibindo dados em uma tabela de várias coluna, várias linhas

Para criar uma DataList várias coluna, várias linhas, precisamos definir o [ `RepeatColumns` propriedade](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) para o número de colunas a serem exibidas. Por padrão, o `RepeatColumns` propriedade é definida como 0, o que fará com que o DataList exibir todos os seus itens em uma única linha ou uma coluna (dependendo do valor da `RepeatDirection` propriedade).

Para nosso exemplo, permitem s exibir três produtos por linha da tabela. Portanto, definir o `RepeatColumns` propriedade 3. Depois de fazer essa alteração, dedique alguns momentos para exibir os resultados em um navegador. Como mostra a Figura 5, os produtos agora são listados em uma tabela com três colunas de várias linhas.


[![Três produtos são exibidos por linha](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Figura 5**: três produtos são exibidos por linha ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


O `RepeatDirection` propriedade afeta como os itens em DataList são dispostos. A Figura 5 mostra os resultados com o `RepeatDirection` propriedade definida como `Horizontal`. Observe que os primeiros três produtos Chai, ALT e Aniseed Syrup são dispostos da esquerda para direita, de cima para baixo. Os próximos três produtos (começando com s chefe Anton Cajun Seasoning) aparecem em uma linha abaixo as três primeiras. Alterando o `RepeatDirection` propriedade `Vertical`, no entanto, apresenta esses produtos de cima para baixo, da esquerda para direita, conforme ilustra a Figura 6.


[![Aqui, os produtos são apresentados Out verticalmente](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Figura 6**: aqui, os produtos são apresentados Out verticalmente ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


O número de linhas exibidas na tabela resultante depende do número total de registros associado à DataList. Precisamente, ele s o limite do número total de itens de fonte de dados é dividido pelo `RepeatColumns` o valor da propriedade. Desde o `Products` tabela tem atualmente 84 produtos, que é divisível por 3, há 28 linhas. Se o número de itens na fonte de dados e o `RepeatColumns` valor de propriedade não é divisível e, em seguida, a última linha ou coluna terá as células em branco. Se o `RepeatDirection` é definido como `Vertical`, em seguida, a última coluna terá células vazias; se `RepeatDirection` é `Horizontal`, a última linha terá as células vazias.

## <a name="summary"></a>Resumo

DataList, por padrão, lista seus itens em uma tabela de coluna única, várias linhas, que reflete o layout de um controle GridView com um TemplateField único. Enquanto este layout padrão é aceitável, podemos pode maximizar o espaço na tela exibindo vários itens de fonte de dados por linha. Fazer isso é simplesmente uma questão de configuração do DataList s `RepeatColumns` propriedade para o número de colunas a serem exibidas por linha. Além disso, o DataList s `RepeatDirection` propriedade pode ser usada para indicar se o conteúdo da tabela de várias coluna, várias linhas deve ser disposto horizontalmente da esquerda para direita, de cima para baixo ou verticalmente, de cima para baixo, da esquerda para a direita.

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi John Suru. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [Próximo](nested-data-web-controls-vb.md)
