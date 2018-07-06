---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: Exibindo vários registros por linha com o controle DataList (VB) | Microsoft Docs
author: rick-anderson
description: Neste breve tutorial vamos explorar como personalizar o layout do DataList por meio de suas propriedades RepeatColumns e RepeatDirection.
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 55e07159fd9d0f4c750a2522feb0538a1cfb4bea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831881"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Exibindo vários registros por linha com o controle DataList (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) ou [baixar PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> Neste breve tutorial vamos explorar como personalizar o layout do DataList por meio de suas propriedades RepeatColumns e RepeatDirection.


## <a name="introduction"></a>Introdução

Os exemplos de DataList podemos ve visto nos últimos dois tutoriais tiver processado cada registro da fonte de dados como uma linha em uma única coluna HTML `<table>`. Embora esse seja o comportamento de DataList padrão, é muito fácil personalizar a exibição do DataList, de modo que os itens de fonte de dados são distribuídos em uma tabela com várias coluna de várias linhas. Além disso, ele é possível ter todos os dados de origem itens exibidos em uma única linha, várias coluna DataList.

Podemos personalizar o layout do DataList s por meio de seu `RepeatColumns` e `RepeatDirection` propriedades, que, respectivamente, indicam o número de colunas é renderizado e se esses itens são dispostos verticalmente ou horizontalmente. Figura 1, por exemplo, mostra uma DataList que exibe informações sobre o produto em uma tabela com três colunas.


[![DataList mostra três produtos por linha](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Figura 1**: O DataList mostra três produtos por linha ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


Ao mostrar vários itens de fonte de dados por linha, DataList com mais eficiência pode utilizar espaço horizontal da tela. Neste breve tutorial vamos explorar essas duas propriedades de DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Etapa 1: Exibir informações sobre o produto em uma DataList

Antes de examinar a `RepeatColumns` e `RepeatDirection` propriedades, let s primeiro criar uma DataList em nossa página que lista informações sobre o produto usando o layout de tabela padrão de coluna única, várias linhas. Neste exemplo, deixe s exibem o nome do produto s, categoria e preço usando a seguinte marcação:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Podemos var visto como associar dados a uma DataList nos exemplos anteriores, por isso, passarei por essas etapas rapidamente. Comece abrindo o `RepeatColumnAndDirection.aspx` página o `DataListRepeaterBasics` pasta e arraste uma DataList da caixa de ferramentas para o Designer. Da marca inteligente DataList s, optar por criar um novo ObjectDataSource e configurá-lo para efetuar pull de seus dados a partir de `ProductsBLL` classe s `GetProducts` método, escolhendo (nenhum) a opção do Assistente de s INSERT, UPDATE e excluir guias.

Depois de criar e associar o novo ObjectDataSource a DataList, o Visual Studio criará automaticamente um `ItemTemplate` que exibe o nome e valor para cada um dos campos de dados do produto. Ajustar a `ItemTemplate` diretamente por meio de marcação declarativa ou editar modelos de opção na marca inteligente DataList s para que ele usa a marcação mostrada acima, substituindo o *nome do produto*, *nome da categoria* , e *preço* texto com controles de rótulo que usam a sintaxe de associação de dados apropriado para atribuir valores para seus `Text` propriedades. Depois de atualizar o `ItemTemplate`, sua marcação declarativa de página s deve ser semelhante ao seguinte:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Observe que eu ve incluído um especificador de formato na `Eval` sintaxe de associação de dados para o `UnitPrice`, formatar o valor retornado como uma moeda - `Eval("UnitPrice", "{0:C}").`

Reserve um tempo para visitar a página em um navegador. Como mostra a Figura 2, DataList é renderizado como uma tabela de coluna única, várias linhas de produtos.


[![Por padrão, os renderizadores de DataList como uma tabela de coluna única, várias linhas](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Figura 2**: por padrão, o DataList é renderizado como uma única coluna, tabela de várias linhas ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Etapa 2: Alterar a direção do Layout s DataList

Enquanto o comportamento padrão para DataList é dispor seus itens verticalmente em uma tabela de coluna única, várias linhas, esse comportamento pode ser alterado facilmente por meio do DataList s [ `RepeatDirection` propriedade](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). O `RepeatDirection` propriedade pode aceitar um dos dois valores possíveis: `Horizontal` ou `Vertical` (o padrão).

Alterando a `RepeatDirection` propriedade de `Vertical` para `Horizontal`, DataList processa seus registros em uma única linha, a criação de uma coluna por item de fonte de dados. Para ilustrar esse efeito, clique no DataList no Designer e em seguida, na janela Propriedades, altere o `RepeatDirection` propriedade de `Vertical` para `Horiztonal`. Imediatamente após fazer isso, o Designer ajusta o layout do DataList s, criando uma interface de linha única, várias coluna (veja a Figura 3).


[![Os itens de RepeatDirection propriedade determina como a direção de s DataList são apresentados Out](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Figura 3**: O `RepeatDirection` propriedade determina como os itens em direção a s DataList são apresentados Out ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


Ao exibir pequenas quantidades de dados, uma única linha, a tabela de várias colunas pode ser uma maneira ideal para maximizar o espaço na tela. Para volumes maiores de dados, no entanto, uma única linha exigirá várias colunas, que as envia os itens que o pode caber na tela de para a direita. Figura 4 mostra os produtos quando renderizado em uma única linha DataList. Como há muitos produtos (80), o usuário terá rolar à direita para exibir informações sobre cada um dos produtos.


[![Para fontes de dados grande o suficiente, uma única coluna DataList exigirá a rolagem Horizontal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Figura 4**: para suficientemente grandes fontes de dados, uma única coluna DataList será exigem rolagem Horizontal ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Etapa 3: Exibindo dados em uma tabela com várias coluna de várias linhas

Para criar um DataList de várias coluna, várias linhas, precisamos definir a [ `RepeatColumns` propriedade](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) ao número de colunas a serem exibidas. Por padrão, o `RepeatColumns` estiver definida como 0, o que fará com que o DataList exibir todos os seus itens em uma única linha ou uma coluna (dependendo do valor da `RepeatDirection` propriedade).

Para nosso exemplo, deixe s exibir três produtos por linha da tabela. Portanto, definir o `RepeatColumns` propriedade para 3. Depois de fazer essa alteração, reserve um tempo para exibir os resultados em um navegador. Como mostra a Figura 5, os produtos estão agora listados em uma tabela com três colunas de várias linhas.


[![Três produtos são exibidos por linha](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Figura 5**: três produtos são exibidos por linha ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


O `RepeatDirection` propriedade afeta como os itens no DataList estão dispostos. A Figura 5 mostra os resultados com o `RepeatDirection` propriedade definida como `Horizontal`. Observe que os três primeiros produtos Chai, Chang e Xarope de anis são dispostos da esquerda para a direita, de cima para baixo. Os próximos três produtos (começando com s chefe Anton Cajun Seasoning) são exibidos em uma linha abaixo as três primeiras. Alterando a `RepeatDirection` propriedade de volta para `Vertical`, no entanto, apresenta esses produtos de cima para baixo, da esquerda para a direita, conforme ilustra a Figura 6.


[![Aqui, os produtos são apresentados Out verticalmente](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Figura 6**: aqui, os produtos são apresentados Out verticalmente ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


O número de linhas exibidas na tabela resultante depende do número total de registros associado à DataList. Precisamente, ele s o teto do número total de itens de fonte de dados dividido pelo `RepeatColumns` valor da propriedade. Uma vez que o `Products` tabela atualmente tem 84 produtos, o que é divisível por 3, há 28 linhas. Se o número de itens na fonte de dados e o `RepeatColumns` valor da propriedade não são divisíveis, em seguida, a última linha ou coluna terá as células em branco. Se o `RepeatDirection` é definido como `Vertical`, em seguida, a última coluna terá células vazias; se `RepeatDirection` é `Horizontal`, em seguida, a última linha terá as células vazias.

## <a name="summary"></a>Resumo

Por padrão, DataList e lista seus itens em uma tabela de coluna única, várias linhas, que imita o layout de um controle GridView com um TemplateField único. Embora esse layout padrão seja aceitável, podemos pode maximizar o espaço na tela exibindo vários itens de fonte de dados por linha. Realizar isso é simplesmente uma questão de definir DataList s `RepeatColumns` propriedade para o número de colunas a serem exibidas por linha. Além disso, DataList s `RepeatDirection` propriedade pode ser usada para indicar se o conteúdo da tabela de várias coluna, várias linhas deve ser disposto horizontalmente da esquerda para a direita, de cima para baixo ou verticalmente de cima para baixo, da esquerda para a direita.

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi John Suru. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [Próximo](nested-data-web-controls-vb.md)
