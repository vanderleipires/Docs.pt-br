---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: "Botões personalizados no DataList e repetidor (c#) | Microsoft Docs"
author: rick-anderson
description: "Neste tutorial, criaremos uma interface que usa um repetidor para listar as categorias no sistema, com cada categoria, fornecendo um botão para mostrar seu associ..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 9a072ae18bbb19d086eb825c6e72b68d40b2e429
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Botões personalizados no DataList e repetidor (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) ou [baixar PDF](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> Neste tutorial, criaremos uma interface que usa um repetidor para listar as categorias no sistema, com cada categoria, fornecendo um botão para mostrar seus produtos associados usando um controle de BulletedList.


## <a name="introduction"></a>Introdução

Durante os últimos tutoriais de dezessete DataList e repetidor, podemos ve criado os exemplos de somente leitura e edição e exclusão de exemplos. Para facilitar a edição e exclusão de recursos dentro de DataList, adicionamos botões para DataList s `ItemTemplate` que, quando clicado, causou um postback e gerou um evento de DataList correspondente ao botão s `CommandName` propriedade. Por exemplo, adicionando um botão para o `ItemTemplate` com um `CommandName` DataList s faz com que o valor da propriedade de edição `EditCommand` seja acionado em um postback; com o `CommandName` excluir gera o `DeleteCommand`.

Além para editar e excluir botões, os controles DataList e repetidor também podem incluir botões, botões de link ou ImageButtons que, quando clicado, executar alguns lógica personalizada do lado do servidor. Neste tutorial, criaremos uma interface que usa um repetidor para listar as categorias no sistema. Para cada categoria, repetidor incluirá um botão para mostrar a categoria de produtos s associado usando um controle BulletedList (consulte a Figura 1).


[![Clicar no exibirá de Link de produtos de mostrar os produtos de s categoria em uma lista com marcadores](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Figura 1**: clicando o exibe Mostrar de Link de produtos na categoria s produtos em uma lista com marcadores ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Etapa 1: Adicionar as páginas da Web Tutorial do botão personalizado

Antes de examinarmos como adicionar um botão personalizado, deixe-s primeiro dedicar um tempo para criar as páginas ASP.NET em nosso projeto de site que vamos precisar para este tutorial. Comece adicionando uma nova pasta chamada `CustomButtonsDataListRepeater`. Em seguida, adicione as duas páginas ASP.NET a seguir para a pasta, certificando-se de associar cada página com o `Site.master` página mestre:

- `Default.aspx`
- `CustomButtons.aspx`


![Adicionar as páginas ASP.NET para os tutoriais de relacionadas botões personalizados](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**Figura 2**: adicionar as páginas ASP.NET para os tutoriais de relacionadas botões personalizados


Como em outras pastas, `Default.aspx` no `CustomButtonsDataListRepeater` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Adicionar este controle de usuário para `Default.aspx` arrastando-o no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Figura 3**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))


Por fim, adicione as páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após a paginação e a classificação com o controle DataList e repetidor `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

Após a atualização `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para a edição, inserção e exclusão tutoriais.


![O mapa do Site agora inclui a entrada para o Tutorial de botões personalizados](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Figura 4**: O mapa de Site agora inclui a entrada para o Tutorial de botões personalizados


## <a name="step-2-adding-the-list-of-categories"></a>Etapa 2: Adicionando a lista de categorias

Para este tutorial, precisamos criar um repetidor que lista todas as categorias junto com LinkButton produtos mostram que, quando clicado, exibe os produtos de categoria associada s em uma lista com marcadores. Permitir que o s primeiro criar um repetidor simple que lista as categorias no sistema. Comece abrindo o `CustomButtons.aspx` página o `CustomButtonsDataListRepeater` pasta. Arraste um repetidor da caixa de ferramentas para o Designer e defina seu `ID` propriedade `Categories`. Em seguida, crie um novo controle de fonte de dados de marca inteligente s Repetidor. Especificamente, criar um novo controle ObjectDataSource chamado `CategoriesDataSource` que seleciona seus dados a partir de `CategoriesBLL` classe s `GetCategories()` método.


[![Configurar o ObjectDataSource para usar o método de GetCategories() CategoriesBLL classe s](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Figura 5**: configurar o ObjectDataSource para usar o `CategoriesBLL` classe s `GetCategories()` método ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))


Ao contrário do controle DataList, para que o Visual Studio cria um padrão `ItemTemplate` com base na fonte de dados, os modelos de Repetidor s devem ser definidos manualmente. Além disso, os modelos de Repetidor s devem ser criados e editados declarativamente (ou seja, há s sem editar modelos de opção na marca inteligente repetidor s).

Clique na guia fonte, no canto inferior esquerdo e adicione um `ItemTemplate` que exibe o nome da categoria s em um `<h3>` elemento e sua descrição de um parágrafo marca; incluir uma `SeparatorTemplate` que exibe uma régua horizontal (`<hr />`) entre cada categoria. Também adicione um LinkButton com seu `Text` propriedade definida para mostrar os produtos. Depois de concluir essas etapas, a marcação declarativa de s de página deve ser semelhante ao seguinte:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

A Figura 6 mostra a página quando exibida por meio de um navegador. Cada nome de categoria e a descrição é listado. Mostrar produtos quando o botão é clicado, causa um postback, mas ainda não executará qualquer ação.


[![Cada categoria s nome e a descrição é exibida, juntamente com LinkButton Mostrar produtos](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Figura 6**: s de cada categoria nome e a descrição é exibida, juntamente com Mostrar produtos LinkButton ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Etapa 3: Executando no servidor lógica quando a mostrar produtos LinkButton é clicado

Sempre que um botão, LinkButton ou ImageButton dentro de um modelo em DataList ou repetidor é clicado, ocorre um postback e os s DataList ou repetidor `ItemCommand` evento ser acionado. Além de `ItemCommand` evento, DataList controle também pode gerar mais específico, outro evento se botão s `CommandName` propriedade é definida como um das cadeias de caracteres reservadas (excluir, editar, cancelar, atualização ou selecione), mas o `ItemCommand` evento é *sempre* acionado.

Quando um botão é clicado em uma DataList ou repetidor, muitas vezes, é preciso passar qual botão foi clicado (no caso de que pode haver vários botões dentro do controle, como uma edição ambos e botão de exclusão) e talvez algumas informações adicionais (como o valor de chave primária do item cuja botão foi clicado). O botão, LinkButton e ImageButton fornecem duas propriedades cujos valores são passados para o `ItemCommand` manipulador de eventos:

- `CommandName`uma cadeia de caracteres que normalmente é usada para identificar cada botão no modelo
- `CommandArgument`normalmente usado para armazenar o valor do campo de dados, como o valor de chave primária

Neste exemplo, defina o s LinkButton `CommandName` propriedade ShowProducts e vincular o atual valor de chave de registro s primária `CategoryID` para o `CommandArgument` usando a sintaxe de associação de dados de propriedade `CategoryArgument='<%# Eval("CategoryID") %>'`. Depois de especificar essas duas propriedades, a sintaxe declarativa de s LinkButton deve parecer com o seguinte:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

Quando o botão é clicado, ocorre um postback e os s DataList ou repetidor `ItemCommand` evento ser acionado. O manipulador de eventos é passado no botão s `CommandName` e `CommandArgument` valores.

Criar um manipulador de eventos para repetidor s `ItemCommand` eventos e observe o segundo parâmetro passado para o manipulador de eventos (chamado `e`). Esse segundo parâmetro é do tipo [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) e tem as seguintes quatro propriedades:

- `CommandArgument`o valor do botão clicado s `CommandArgument` propriedade
- `CommandName`o valor do botão s `CommandName` propriedade
- `CommandSource`uma referência para o controle de botão foi clicado
- `Item`uma referência para o [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) que contém o botão que foi clicado; cada registro associado a repetidor manifestado como um`RepeaterItem`

Desde a categoria selecionada s `CategoryID` é passado por meio de `CommandArgument` propriedade, podemos obter o conjunto de produtos associados a categoria selecionada no `ItemCommand` manipulador de eventos. Esses produtos, em seguida, podem ser associados a um controle BulletedList no `ItemTemplate` (que é dicionar ainda). Tudo o que permanece, em seguida, é adicionar BulletedList, fazem referência a ele no `ItemCommand` manipulador de eventos e vinculá-lo ao conjunto de produtos para a categoria selecionada, que abordaremos na etapa 4.

> [!NOTE]
> DataList s `ItemCommand` manipulador de eventos é passado um objeto do tipo [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), que oferece as quatro propriedades mesmo como o `RepeaterCommandEventArgs` classe.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Etapa 4: Exibindo os produtos a categoria selecionada em uma lista com marcadores

Os produtos da categoria selecionada s podem ser exibidos no repetidor s `ItemTemplate` usando qualquer número de controles. Poderíamos adicionar que outro aninhados repetidor, DataList, DropDownList, GridView e assim por diante. Como queremos exibir os produtos como uma lista com marcadores, porém, vamos usar o controle de BulletedList. Retornando para o `CustomButtons.aspx` declarativo s de página, adicione um controle BulletedList para o `ItemTemplate` após o LinkButton produtos Mostrar. Definir o s BulletedLists `ID` para `ProductsInCategory`. A BulletedList exibe o valor do campo de dados especificado por meio de `DataTextField` propriedade; pois este controle terá informações sobre o produto associado a ele, defina o `DataTextField` propriedade para `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

No `ItemCommand` manipulador de eventos, fazer referência a esse controle usando `e.Item.FindControl("ProductsInCategory")` e associá-lo ao conjunto de produtos associados a categoria selecionada.


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

Antes de executar qualquer ação no `ItemCommand` manipulador de eventos,-s, convém primeiro, verifique o valor da entrada `CommandName`. Como o `ItemCommand` manipulador de eventos é acionado quando *qualquer* botão é clicado, se houver vários botões do uso do modelo de `CommandName` valor para distinguir a ação a tomar. Verificando o `CommandName` aqui é sentido, já que temos somente um único botão, mas é um bom hábito ao formulário. Em seguida, o `CategoryID` da categoria selecionada é recuperado do `CommandArgument` propriedade. O controle de BulletedList no modelo, em seguida, é referenciado e associado aos resultados do `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método.

Nos tutoriais anteriores que usadas nos botões dentro de DataList, como [uma visão geral da edição de e excluindo dados em DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), determinamos que o valor de chave primária de um determinado item por meio de `DataKeys` coleção. Embora essa abordagem funciona bem com DataList, repetidor não possuem um `DataKeys` propriedade. Em vez disso, podemos deve usar uma abordagem alternativa para fornecer o valor de chave primária, como por meio do botão s `CommandArgument` propriedade ou atribuindo o valor de chave primária para um controle de rótulo Web oculto dentro do modelo e ler seu valor no `ItemCommand`manipulador de eventos usando `e.Item.FindControl("LabelID")`.

Depois de concluir o `ItemCommand` manipulador de eventos, reserve um tempo para testar esta página em um navegador. Como mostra a Figura 7, clicando em Mostrar produtos link causa um postback e exibe os produtos para a categoria selecionada uma BulletedList. Além disso, observe que essas informações de produto permanecem, mesmo que outras categorias de links de mostrar os produtos sejam clicadas.

> [!NOTE]
> Se você quiser modificar o comportamento deste relatório, de modo que os produtos de apenas uma categoria s são listados por vez, basta definir o controle de BulletedList s `EnableViewState` propriedade `False`.


[![Um BulletedList é usado para exibir os produtos da categoria selecionada](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Figura 7**: um BulletedList é usado para exibir os produtos da categoria selecionada ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))


## <a name="summary"></a>Resumo

Os controles DataList e repetidor podem incluir qualquer número de botões, botões de link ou ImageButtons dentro de seus modelos. Esses botões quando clicado, causar um postback e gerar o `ItemCommand` evento. Para associar a ação personalizada do lado do servidor com um botão que está sendo clicado, criar um manipulador de eventos para o `ItemCommand` evento. Este manipulador de eventos primeiro verifique a entrada `CommandName` valor para determinar qual botão foi clicado. Informações adicionais, opcionalmente, podem ser fornecidas por meio do botão s `CommandArgument` propriedade.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Dennis Patterson. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Avançar](custom-buttons-in-the-datalist-and-repeater-vb.md)
