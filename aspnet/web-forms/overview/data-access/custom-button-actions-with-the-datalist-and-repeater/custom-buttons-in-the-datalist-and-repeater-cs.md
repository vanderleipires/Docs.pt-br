---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: Botões personalizados no DataList e Repeater (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, criaremos uma interface que usa um repetidor para listar as categorias no sistema, com cada categoria que fornece um botão para mostrar seu associ...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: a10acd00dd8243f92c1b255acb8328e2b76e87cc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823585"
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Botões personalizados no DataList e Repeater (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) ou [baixar PDF](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> Neste tutorial, criaremos uma interface que usa um repetidor para listar as categorias no sistema, com cada categoria que fornece um botão para mostrar seus produtos associados usando um controle BulletedList.


## <a name="introduction"></a>Introdução

Durante os últimos tutoriais de dezessete DataList e Repeater, podemos ve criou os exemplos de somente leitura e edição e exclusão de exemplos. Para facilitar a edição e exclusão de recursos dentro de uma DataList, adicionamos botões para s DataList `ItemTemplate` que, quando clicado, causou um postback e gerou um evento de DataList correspondente ao botão s `CommandName` propriedade. Por exemplo, adicionando um botão para o `ItemTemplate` com um `CommandName` DataList s faz com que o valor da propriedade de edição `EditCommand` seja acionado em um postback; um com o `CommandName` excluir gera o `DeleteCommand`.

Além para editar e excluir botões, os controles DataList e Repeater também podem incluir botões, botões de link ou ImageButtons que, quando clicado, executar alguma lógica personalizada do lado do servidor. Neste tutorial, criaremos uma interface que usa um repetidor para listar as categorias no sistema. Para cada categoria, repetidor incluirá um botão para mostrar a categoria de produtos de s associados usando um controle BulletedList (veja a Figura 1).


[![Clicar no exibirá de Link de produtos de mostrar os categoria os produtos em uma lista com marcadores](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Figura 1**: clicando o exibe Mostrar de Link de produtos na categoria s produtos em uma lista com marcadores ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Etapa 1: Adicionar as páginas da Web Tutorial do botão personalizado

Antes de observarmos como adicionar um botão personalizado, deixe s primeiro dedique uns momentos para criar as páginas do ASP.NET em nosso projeto de site que precisaremos para este tutorial. Comece adicionando uma nova pasta chamada `CustomButtonsDataListRepeater`. Em seguida, adicione as duas páginas do ASP.NET a seguir para essa pasta, certificando-se de associar cada página com o `Site.master` página mestra:

- `Default.aspx`
- `CustomButtons.aspx`


![Adicione as páginas do ASP.NET para que os tutoriais de relacionadas de botões personalizados](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**Figura 2**: adicionar as páginas do ASP.NET para que os tutoriais de relacionadas de botões personalizados


Como em outras pastas `Default.aspx` no `CustomButtonsDataListRepeater` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Adicionar esse controle de usuário para `Default.aspx` arrastando-no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Figura 3**: Adicione o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))


Por fim, adicione as páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após a paginação e classificação com o DataList e Repeater `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para a edição, inserindo e excluindo tutoriais.


![O mapa do Site agora inclui a entrada para o Tutorial de botões personalizados](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Figura 4**: O mapa do Site agora inclui a entrada para o Tutorial de botões personalizados


## <a name="step-2-adding-the-list-of-categories"></a>Etapa 2: Adicionando a lista de categorias

Para este tutorial é necessário criar um repetidor que lista todas as categorias, juntamente com um botão LinkButton Mostrar produtos que, quando clicado, exibe os produtos de categoria associada s em uma lista com marcadores. Deixe o s primeiro criar um repetidor simple que lista as categorias no sistema. Comece abrindo o `CustomButtons.aspx` página o `CustomButtonsDataListRepeater` pasta. Arraste um repetidor da caixa de ferramentas para o Designer e o conjunto de seu `ID` propriedade para `Categories`. Em seguida, crie um novo controle de fonte de dados da marca inteligente s Repeater. Especificamente, crie um novo controle ObjectDataSource chamado `CategoriesDataSource` que seleciona os seus dados a partir de `CategoriesBLL` classe s `GetCategories()` método.


[![Configurar o ObjectDataSource para usar o método de GetCategories() CategoriesBLL classe s](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Figura 5**: configurar o ObjectDataSource para usar o `CategoriesBLL` classe s `GetCategories()` método ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))


Ao contrário do controle DataList, para que o Visual Studio cria um padrão `ItemTemplate` com base na fonte de dados, os modelos de Repeater s devem ser definidos manualmente. Além disso, os modelos de Repeater s devem ser criados e editados declarativamente (ou seja, s não existe nenhum editar modelos de opção na marca inteligente Repeater s).

Clique na guia fonte, no canto inferior esquerdo e adicione uma `ItemTemplate` que exibe o nome da categoria s em um `<h3>` elemento e sua descrição em um parágrafo de marca, incluir um `SeparatorTemplate` que exibe uma régua horizontal (`<hr />`) entre cada categoria. Adicione também um LinkButton com seu `Text` propriedade definida para mostrar os produtos. Depois de concluir essas etapas, sua marcação declarativa de s de página deve ser semelhante ao seguinte:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

Figura 6 mostra a página quando visualizado por meio de um navegador. Cada nome de categoria e a descrição é listado. Mostrar produtos quando o botão é clicado, faz com que um postback, mas não executa qualquer ação ainda.


[![Cada categoria s nome e a descrição é exibida, juntamente com um botão LinkButton Mostrar produtos](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Figura 6**: cada categoria s nome e a descrição é exibida, juntamente com um botão LinkButton Mostrar produtos ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Etapa 3: Executar o servidor lógico quando a mostrar produtos LinkButton é clicado.

Sempre que um botão, LinkButton ou ImageButton dentro de um modelo em um DataList ou Repeater é clicado, ocorre um postback e o s DataList ou Repeater `ItemCommand` evento é acionado. Além a `ItemCommand` evento, DataList controle também pode acionar o evento de outro, mais específico se o botão s `CommandName` estiver definida como uma das cadeias de reservado (excluir, editar, cancelar, atualização ou selecione), mas o `ItemCommand` evento é *sempre* disparado.

Quando um botão é clicado dentro de uma DataList ou Repeater, muitas vezes, precisamos passar ao longo do qual botão foi clicado (no caso de que pode haver vários botões dentro do controle, como de uma edição e botão de exclusão) e talvez algumas informações adicionais (como o valor de chave primária do item cujo botão foi clicado). O botão, LinkButton e ImageButton fornecem duas propriedades cujos valores são passados para o `ItemCommand` manipulador de eventos:

- `CommandName` uma cadeia de caracteres que normalmente é usada para identificar cada botão no modelo
- `CommandArgument` normalmente usado para manter o valor do campo de dados, como o valor de chave primária

Neste exemplo, defina o s LinkButton `CommandName` propriedade ShowProducts e associar o atual valor de chave de registro s primária `CategoryID` para o `CommandArgument` usando a sintaxe de associação de dados de propriedade `CategoryArgument='<%# Eval("CategoryID") %>'`. Depois de especificar essas duas propriedades, a sintaxe declarativa do LinkButton s deve ser semelhante ao seguinte:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

Quando o botão é clicado, ocorre um postback e o s DataList ou Repeater `ItemCommand` evento é acionado. O manipulador de eventos é passado no botão s `CommandName` e `CommandArgument` valores.

Criar um manipulador de eventos para o s Repeater `ItemCommand` evento e observe o segundo parâmetro passado para o manipulador de eventos (chamado `e`). Esse segundo parâmetro é do tipo [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) e tem as seguintes quatro propriedades:

- `CommandArgument` o valor do botão clicado s `CommandArgument` propriedade
- `CommandName` o valor do botão s `CommandName` propriedade
- `CommandSource` uma referência ao controle de botão foi clicado
- `Item` uma referência para o [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) que contém o botão que foi clicado; cada registro associado a repetidor manifestado como um `RepeaterItem`

Desde a categoria selecionada s `CategoryID` é passado por meio de `CommandArgument` propriedade, podemos obter o conjunto de produtos associados à categoria selecionada no `ItemCommand` manipulador de eventos. Esses produtos, em seguida, podem ser associados a um controle BulletedList no `ItemTemplate` (quais podemos dicionar ainda). Tudo o que permanece, em seguida, é adicionar BulletedList, referenciá-lo no `ItemCommand` manipulador de eventos e associá-los o conjunto de produtos para a categoria selecionada, o qual que abordaremos na etapa 4.

> [!NOTE]
> S DataList `ItemCommand` manipulador de eventos recebe um objeto do tipo [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), que oferece as mesmas quatro propriedades que o `RepeaterCommandEventArgs` classe.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Etapa 4: Exibindo produtos a categoria selecionada em uma lista com marcadores

Os produtos da categoria selecionada s podem ser exibidos dentro do repetidor s `ItemTemplate` usando qualquer número de controles. Poderíamos adicionar que outro aninhados Repeater, DataList, DropDownList, um GridView e assim por diante. Como queremos exibir os produtos como uma lista com marcadores, no entanto, vamos usar o controle BulletedList. Retornando para o `CustomButtons.aspx` marcação declarativa de s de página, adicione um controle BulletedList para o `ItemTemplate` depois no botão LinkButton Mostrar produtos. Definir o s BulletedLists `ID` para `ProductsInCategory`. Vinculados à BulletedList exibe o valor do campo de dados especificado por meio de `DataTextField` propriedade; já que esse controle terá informações sobre o produto associado a ele, defina a `DataTextField` propriedade para `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

No `ItemCommand` manipulador de eventos, fazer referência a esse controle usando `e.Item.FindControl("ProductsInCategory")` e associá-lo ao conjunto de produtos associados a categoria selecionada.


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

Antes de executar qualquer ação na `ItemCommand` manipulador de eventos, ele s prudente verificar primeiro o valor da entrada `CommandName`. Uma vez que o `ItemCommand` manipulador de eventos é acionada quando *qualquer* botão é clicado, se houver vários botões no modelo, use o `CommandName` valor para distinguir qual ação será tomada. Verificando o `CommandName` aqui é sentido, já que temos apenas um único botão, mas é um hábito ao formulário. Em seguida, o `CategoryID` da categoria selecionada é recuperado do `CommandArgument` propriedade. O controle de BulletedList no modelo, em seguida, é referenciado e associado aos resultados do `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método.

Nos tutoriais anteriores que usados os botões dentro uma DataList, como [An de visão geral de edição e exclusão de dados no DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), determinamos que o valor de chave primária de um determinado item por meio de `DataKeys` coleção. Embora essa abordagem funciona bem com DataList, repetidor não tem um `DataKeys` propriedade. Em vez disso, devemos usar uma abordagem alternativa para fornecer o valor de chave primária, como por meio do botão s `CommandArgument` propriedade ou atribuindo o valor de chave primária para um controle de rótulo Web oculto dentro do modelo e ler seu valor em de `ItemCommand`manipulador de eventos usando `e.Item.FindControl("LabelID")`.

Depois de concluir o `ItemCommand` manipulador de eventos, reserve um tempo para testar esta página em um navegador. Como mostra a Figura 7, clicando em Mostrar produtos link causa um postback e exibe os produtos para a categoria selecionada em um BulletedList. Além disso, observe que essas informações de produto permanecem, mesmo se outros links de produtos Mostrar categorias são clicadas.

> [!NOTE]
> Se você quiser modificar o comportamento deste relatório, de modo que os produtos de apenas uma categoria s são listados por vez, basta definir o controle BulletedList s `EnableViewState` propriedade para `False`.


[![Um BulletedList é usado para exibir os produtos da categoria selecionada](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Figura 7**: um BulletedList é usado para exibir os produtos da categoria selecionada ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))


## <a name="summary"></a>Resumo

Os controles DataList e Repeater podem incluir qualquer número de botões, botões de link ou ImageButtons dentro de seus modelos. Esses botões, quando clicado, fazer com que um postback e disparar o `ItemCommand` eventos. Para associar a ação personalizada do lado do servidor com um botão sendo clicado, crie um manipulador de eventos para o `ItemCommand` eventos. Nesse manipulador de eventos de entrada primeiro verificar `CommandName` valor para determinar qual botão foi clicado. Informações adicionais, opcionalmente, podem ser fornecidas por meio do botão s `CommandArgument` propriedade.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Dennis Patterson. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](custom-buttons-in-the-datalist-and-repeater-vb.md)
