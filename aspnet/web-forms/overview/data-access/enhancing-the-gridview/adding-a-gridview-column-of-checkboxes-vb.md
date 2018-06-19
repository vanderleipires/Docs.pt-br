---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: Adicionando uma coluna de GridView das caixas de seleção (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial explica como adicionar uma coluna de caixas de seleção a um controle GridView para fornecer ao usuário uma maneira intuitiva de seleção de várias linhas do G...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: beb28de901805e07365f336192d20e914eeebb1e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892732"
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a>Adicionando uma coluna de GridView das caixas de seleção (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe) ou [baixar PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> Este tutorial explica como adicionar uma coluna de caixas de seleção para um controle GridView para fornecer ao usuário uma maneira intuitiva de seleção de várias linhas de GridView.


## <a name="introduction"></a>Introdução

No tutorial anterior, examinamos como adicionar uma coluna de botões de opção a GridView com a finalidade de selecionar um registro específico. Uma coluna de botões de opção é uma interface de usuário adequado quando o usuário é limitado a escolher a no máximo um item da grade. Às vezes, no entanto, podemos querer permitir que o usuário selecione um número arbitrário de itens na grade. Por exemplo, clientes de email com base na Web, normalmente exibem a lista de mensagens com uma coluna de caixas de seleção. O usuário pode selecionar um número arbitrário de mensagens e, em seguida, executar alguma ação, como mover os emails para outra pasta ou excluí-los.

Este tutorial veremos como adicionar uma coluna de caixas de seleção e como determinar quais caixas de seleção foram verificadas em um postback. Em particular, criaremos um exemplo que estreitamente imita a interface de usuário do cliente de email com base na web. Nosso exemplo incluirá um GridView paginável listando os produtos no `Products` tabela de banco de dados com uma caixa de seleção em cada linha (consulte a Figura 1). Um botão excluir produtos selecionados, quando clicado, excluirá os produtos selecionados.


[![Cada linha de produto inclui uma caixa de seleção](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**Figura 1**: cada linha de produto inclui uma caixa de seleção ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Etapa 1: Adicionar um controle GridView paginável que lista informações sobre o produto

Antes de nos preocupamos com a adição de uma coluna de caixas de seleção, deixe o foco primeiro na lista de produtos em um GridView que oferece suporte à paginação. Comece abrindo o `CheckBoxField.aspx` página o `EnhancedGridView` pasta e arraste um controle GridView da caixa de ferramentas para o Designer, definindo seu `ID` para `Products`. Em seguida, optar por associar GridView para um novo ObjectDataSource denominado `ProductsDataSource`. Configurar o ObjectDataSource para usar o `ProductsBLL` classe, chamar o `GetProducts()` método para retornar os dados. Como esse GridView serão somente leitura, defina as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Criar um novo ObjectDataSource denominado ProductsDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**Figura 2**: criar um novo ObjectDataSource nomeado `ProductsDataSource` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))


[![Configurar o ObjectDataSource para recuperar dados usando o método GetProducts()](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**Figura 3**: configurar o ObjectDataSource para recuperar dados usando o `GetProducts()` método ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))


[![Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum)](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**Figura 4**: definir a lista suspensa na atualização, inserir e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio irá criar automaticamente BoundColumns e um CheckBoxColumn para os campos de dados relacionados ao produto. Como foi feito no tutorial anterior, remova tudo, exceto o `ProductName`, `CategoryName`, e `UnitPrice` BoundFields e altere o `HeaderText` propriedades preço, categoria e produto. Configurar o `UnitPrice` BoundField para que seu valor seja formatado como uma moeda. Também configure o GridView para oferecer suporte a paginação, marcando a caixa de seleção Habilitar paginação a marca inteligente.

Permitir que o s também adicionar a interface do usuário para excluir os produtos selecionados. Adicionar um controle de botão Web abaixo GridView, definindo seu `ID` para `DeleteSelectedProducts` e sua `Text` propriedade para excluir produtos selecionados. Em vez de realmente excluir os produtos do banco de dados, para este exemplo é apenas exibirá uma mensagem informando os produtos que foi excluídos. Para acomodar isso, adicione um controle de rótulo Web sob o botão. Defina sua ID para `DeleteResults`, desmarque-out seu `Text` propriedade e defina seu `Visible` e `EnableViewState` propriedades `False`.

Depois de fazer essas alterações, o GridView, ObjectDataSource, botão e rótulo s declarativo deve semelhante à seguinte:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

Reserve um tempo para exibir a página em um navegador (consulte a Figura 5). Neste ponto, você verá o nome, a categoria e o preço dos dez primeiros produtos.


[![O nome, a categoria e o preço dos dez primeiros produtos listados](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**Figura 5**: O nome, categoria e o preço dos dez primeiros produtos listados ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Etapa 2: Adicionando uma coluna de caixas de seleção

Desde que o ASP.NET 2.0 inclui um CheckBoxField, alguém pode pensar que ela pode ser usada para adicionar uma coluna de caixas de seleção para um controle GridView. Infelizmente, esse não for o caso, como CheckBoxField foi projetado para trabalhar com um campo de dados booliano. Ou seja, para usar CheckBoxField, deve especificar o campo de dados subjacente cujo valor é consultado para determinar se a caixa de seleção renderizada é verificada. Não podemos usar CheckBoxField para incluir apenas uma coluna de caixas de seleção está desmarcadas.

Em vez disso, deveremos adicionar TemplateField e adicionar um controle de caixa de seleção Web para seu `ItemTemplate`. Vá em frente e adicione um TemplateField para o `Products` GridView e torná-lo o primeiro campo (à esquerda). De GridView s marca inteligente, clique no link Editar modelos e, em seguida, arraste um controle de Web de caixa de seleção da caixa de ferramentas para a `ItemTemplate`. Defina esta caixa de seleção s `ID` propriedade `ProductSelector`.


[![Adicionar um controle de caixa de seleção Web chamado ProductSelector para TemplateField s ItemTemplate](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**Figura 6**: adicionar uma caixa de seleção da Web controle chamado `ProductSelector` no s TemplateField `ItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))


Com o controle de Web de caixa de seleção e TemplateField adicionado, cada linha agora incluem uma caixa de seleção. A Figura 7 mostra essa página, quando visualizada através de um navegador, depois que a caixa de seleção e TemplateField foram adicionados.


[![Cada linha de produto agora inclui uma caixa de seleção](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**Figura 7**: cada linha de produto agora inclui uma caixa de seleção ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Etapa 3: Determinar quais caixas de seleção foi verificada no Postback

Neste ponto, temos uma coluna de caixas de seleção, mas nenhuma maneira de determinar quais caixas de seleção foram verificadas em um postback. Quando é clicado no botão excluir produtos selecionados, no entanto, precisamos saber quais caixas de seleção foram marcadas para excluir esses produtos.

O GridView s [ `Rows` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) fornece acesso às linhas de dados em GridView. É possível iterar por meio dessas linhas, acessar programaticamente o controle de caixa de seleção e, em seguida, consulte seu `Checked` propriedade para determinar se a caixa de seleção foi selecionada.

Criar um manipulador de eventos para o `DeleteSelectedProducts` controle de botão Web s `Click` evento e adicione o seguinte código:


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

O `Rows` propriedade retorna uma coleção de `GridViewRow` instâncias que composição as linhas de dados do GridView s. O `For Each` loop aqui enumera nesta coleção. Para cada `GridViewRow` do objeto, a linha s caixa de seleção é acessada programaticamente usando `row.FindControl("controlID")`. Se a caixa de seleção estiver marcada, a linha s correspondente `ProductID` valor é recuperado do `DataKeys` coleção. Neste exercício, podemos simplesmente exibir uma mensagem informativa no `DeleteResults` rótulo, embora em um aplicativo de trabalho d, em vez disso, fazer uma chamada para o `ProductsBLL` classe s `DeleteProduct(productID)` método.

Com a adição deste manipulador de eventos, clicando no botão excluir produtos selecionados agora exibe a `ProductID` s dos produtos selecionados.


[![Ao clicar no botão de produtos selecionadas Excluir o ProductIDs de produtos selecionados são listados](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**Figura 8**: quando a excluir selecionado produtos botão é clicado os produtos selecionados `ProductID` s são listados ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Etapa 4: Adicionando todas e desmarque todos os botões

Se um usuário deseja excluir todos os produtos na página atual, eles devem verificar cada uma das caixas de dez seleção. Podemos ajudar a agilizar a esse processo, adicionando um Check All botão que, quando clicado, seleciona todas as caixas de seleção na grade. Um botão desmarcar tudo seria igualmente útil.

Adicione dois controles da Web de botão à página, colocá-los acima GridView. Definir o primeiro um s `ID` para `CheckAll` e sua `Text` propriedade para verificar todos os; defina a um segundo s `ID` para `UncheckAll` e sua `Text` propriedade para desmarcar todos os.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

Em seguida, crie um método na classe por trás do código chamado `ToggleCheckState(checkState)` que, quando chamado, enumera o `Products` GridView s `Rows` coleção e define cada caixa de seleção s `Checked` propriedade o valor transmitido em *checkState*  parâmetro.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

Em seguida, crie `Click` manipuladores de eventos para o `CheckAll` e `UncheckAll` botões. Em `CheckAll` manipulador de eventos de s, simplesmente chamada `ToggleCheckState(True)`; na `UncheckAll`, chame `ToggleCheckState(False)`.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

Com esse código, clique no botão Verificar todos os causa um postback e verifica todas as caixas de seleção em GridView. Da mesma forma, clicando em desmarcar tudo desmarca todas as caixas de seleção. A Figura 9 mostra a tela depois no botão Verificar tudo foi verificado.


[![Clique a verificação de que todos os botão Seleciona todas as caixas de seleção](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**Figura 9**: clicar a verificar todos os botão Seleciona todas as caixas de seleção ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))


> [!NOTE]
> Quando a exibição de uma coluna de caixas de seleção, uma abordagem para selecionar ou se todas as caixas de seleção é por meio de uma caixa de seleção na linha de cabeçalho. Além disso, atual Verifique todos os / implementação desmarque todos os requer um postback. As caixas de seleção marcada ou desmarcada, porém, podem ser totalmente por meio de script do lado do cliente, fornecendo uma experiência de usuário mais fluida. Para explorar usando uma caixa de seleção de linha de cabeçalho para verificar todas as e desmarque todas as detalhadamente, juntamente com uma discussão sobre o uso de técnicas de cliente, confira [verificando todas as caixas de seleção em um Script do lado do cliente usando GridView e uma caixa de seleção de todos os Check](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Resumo

Em casos em que você precisar permitir que os usuários escolher um número arbitrário de linhas de um controle GridView antes de prosseguir, adicionando uma coluna de caixas de seleção é uma opção. Como vimos neste tutorial, incluindo uma coluna de caixas de seleção em GridView envolve adicionando um TemplateField com um controle de caixa de seleção. Usando um controle da Web (versus injetando marcação diretamente no modelo, como foi feito no tutorial anterior) ASP.NET automaticamente lembra o que as caixas de seleção foram e não foram verificadas em postback. Podemos também pode acessar programaticamente as caixas de seleção no código para determinar se uma determinada caixa de seleção estiver marcada, ou para alterar o estado de ativação.

Este tutorial e a última observou adicionando uma coluna de seletor de linha em GridView. Nossa próxima tutorial, examinaremos como, com um pouco de trabalho, podemos adicionar recursos inserindo a GridView.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-a-gridview-column-of-radio-buttons-vb.md)
> [Próximo](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
