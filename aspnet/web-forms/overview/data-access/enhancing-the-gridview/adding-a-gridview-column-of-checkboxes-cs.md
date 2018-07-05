---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: Adicionando uma coluna de GridView de caixas de seleção (c#) | Microsoft Docs
author: rick-anderson
description: Este tutorial explica como adicionar uma coluna de caixas de seleção a um controle GridView para fornecer ao usuário uma maneira intuitiva de seleção de várias linhas de a G....
ms.author: aspnetcontent
ms.date: 03/06/2007
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 481ae436ef644bcc4d5a13d060ed87671cfcb4dc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814766"
---
<a name="adding-a-gridview-column-of-checkboxes-c"></a>Adicionando uma coluna de GridView de caixas de seleção (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe) ou [baixar PDF](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)

> Este tutorial aborda como adicionar uma coluna de caixas de seleção a um controle GridView para fornecer ao usuário uma maneira intuitiva de seleção de várias linhas de GridView.


## <a name="introduction"></a>Introdução

O tutorial anterior, examinamos como adicionar uma coluna de botões de opção a GridView com a finalidade de selecionar um registro específico. Uma coluna de botões de opção é uma interface de usuário adequado quando o usuário é limitado a escolha de no máximo um item na grade. Às vezes, no entanto, podemos querer permitir que o usuário escolher um número arbitrário de itens na grade. Por exemplo, clientes de email baseado na Web, normalmente exibem a lista de mensagens com uma coluna de caixas de seleção. O usuário pode selecionar um número arbitrário de mensagens e, em seguida, realizar alguma ação, como mover os emails para outra pasta ou excluí-los.

Neste tutorial veremos como adicionar uma coluna de caixas de seleção e como determinar quais caixas de seleção foram marcadas em um postback. Em particular, vamos criar um exemplo que imita a interface de usuário do cliente de email baseado na web. Nosso exemplo incluirá um GridView paginado listando os produtos a `Products` tabela de banco de dados com uma caixa de seleção em cada linha (veja a Figura 1). Um botão excluir produtos selecionados, quando clicado, excluirá esses produtos selecionados.


[![Cada linha de produto inclui uma caixa de seleção](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)

**Figura 1**: cada linha de produto inclui uma caixa de seleção ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Etapa 1: Adicionando um GridView paginado que lista informações sobre o produto

Antes de nos preocupamos adicionando uma coluna de caixas de seleção, deixe o foco primeiro sobre a lista de produtos em um GridView que dá suporte à paginação. Comece abrindo o `CheckBoxField.aspx` página o `EnhancedGridView` pasta e arraste um controle GridView da caixa de ferramentas para o Designer, definindo seu `ID` para `Products`. Em seguida, optar por associar o GridView para um novo ObjectDataSource chamado `ProductsDataSource`. Configurar o ObjectDataSource para usar o `ProductsBLL` classe, chamando o `GetProducts()` método para retornar os dados. Já que essa GridView será somente leitura, defina as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Criar um novo ObjectDataSource chamado ProductsDataSource](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)

**Figura 2**: criar um novo ObjectDataSource nomeado `ProductsDataSource` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))


[![Configurar o ObjectDataSource para recuperar dados usando o método GetProducts()](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)

**Figura 3**: configurar o ObjectDataSource para recuperar dados usando o `GetProducts()` método ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))


[![Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum)](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)

**Figura 4**: defina a lista suspensa no UPDATE, INSERT e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio criará automaticamente BoundColumns e um CheckBoxColumn para os campos de dados relacionados ao produto. Como fizemos no tutorial anterior, remova tudo, exceto os `ProductName`, `CategoryName`, e `UnitPrice` BoundFields e altere o `HeaderText` propriedades preço, categoria e produto. Configurar o `UnitPrice` BoundField para que seu valor é formatado como uma moeda. Também configure o GridView para dar suporte à paginação, marcando a caixa de seleção Habilitar paginação a marca inteligente.

Deixe o s também adicionar a interface do usuário para excluir os produtos selecionados. Adicionar um controle de botão Web sob o controle GridView, definindo sua `ID` à `DeleteSelectedProducts` e seu `Text` propriedade excluir produtos selecionados. Em vez de excluir, na verdade, os produtos do banco de dados, para este exemplo vamos apenas exibir uma mensagem informando que os produtos que teria sido excluídos. Para acomodar isso, adicione um controle de rótulo Web sob o botão. Defina sua ID para `DeleteResults`, desmarque out seus `Text` propriedade e defina seu `Visible` e `EnableViewState` propriedades a serem `false`.

Depois de fazer essas alterações, a marcação declarativa de s do GridView, ObjectDataSource, botão e Label deve semelhante ao seguinte:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

Reserve um tempo para exibir a página em um navegador (consulte a Figura 5). Neste ponto, você verá o nome, categoria e preço de dez primeiros produtos.


[![O nome, categoria e preço dos dez primeiros produtos são listados](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)

**Figura 5**: O nome, categoria e preço dos dez primeiros produtos são listados ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Etapa 2: Adicionando uma coluna de caixas de seleção

Uma vez que o ASP.NET 2.0 inclui um CheckBoxField, alguém pode pensar que ele pode ser usado para adicionar uma coluna de caixas de seleção em um GridView. Infelizmente, que não é o caso, como o CheckBoxField foi projetado para trabalhar com um campo de dados booliano. Ou seja, para usar o CheckBoxField podemos deve especificar o campo de dados subjacente cujo valor é consultado para determinar se a caixa de seleção renderizada é verificada. Não podemos usar CheckBoxField para incluir apenas uma coluna de caixas de seleção desmarcadas.

Em vez disso, devemos adicionar um TemplateField e adicionar um controle de caixa de seleção Web para seu `ItemTemplate`. Vá em frente e adicione um TemplateField para o `Products` GridView e torná-lo o primeiro campo (à esquerda). De GridView s marca inteligente, clique no link Editar modelos e, em seguida, arraste um controle de Web de caixa de seleção da caixa de ferramentas para o `ItemTemplate`. Definir s essa caixa de seleção `ID` propriedade para `ProductSelector`.


[![Adicione um controle de caixa de seleção Web chamado ProductSelector para ItemTemplate s TemplateField](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)

**Figura 6**: adicionar um controle de Web de caixa de seleção denominada `ProductSelector` para o s TemplateField `ItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))


Com o controle de Web de caixa de seleção e TemplateField adicionado, cada linha agora inclui uma caixa de seleção. Figura 7 mostra essa página, quando visualizado por meio de um navegador, depois que a caixa de seleção e TemplateField foram adicionadas.


[![Cada linha de produto agora inclui uma caixa de seleção](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)

**Figura 7**: cada linha de produto agora inclui uma caixa de seleção ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Etapa 3: Determinar quais caixas de seleção foram verificadas no Postback

Neste ponto, temos uma coluna de caixas de seleção, mas nenhuma maneira de determinar quais caixas de seleção foram marcadas em um postback. Quando é clicado no botão excluir produtos selecionados, no entanto, precisamos saber quais caixas de seleção foram marcadas para excluir esses produtos.

O s GridView [ `Rows` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) fornece acesso às linhas de dados em um GridView. Podemos pode iterar por meio dessas linhas, acessar programaticamente o controle de caixa de seleção e, em seguida, consultar seu `Checked` propriedade para determinar se a caixa de seleção tiver sido selecionada.

Crie um manipulador de eventos para o `DeleteSelectedProducts` controle de Web de botão s `Click` eventos e adicione o seguinte código:


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

O `Rows` propriedade retorna uma coleção de `GridViewRow` instâncias essa composição as linhas de dados do GridView s. O `foreach` loop aqui enumera nesta coleção. Para cada `GridViewRow` do objeto, a linha s caixa de seleção por meio de programação é acessada usando `row.FindControl("controlID")`. Se a caixa de seleção estiver marcada, a linha s correspondente `ProductID` valor é recuperado do `DataKeys` coleção. Neste exercício, podemos simplesmente exibir uma mensagem informativa na `DeleteResults` rotular, embora em um aplicativo funcional d em vez disso, fazemos uma chamada para o `ProductsBLL` classe s `DeleteProduct(productID)` método.

Com a adição desse manipulador de eventos, clicando no botão excluir produtos selecionados agora exibe o `ProductID` s dos produtos selecionados.


[![Quando se clica no botão de produtos selecionadas Excluir o ProductIDs de produtos selecionada são listadas](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)

**Figura 8**: quando o selecionado produtos botão Delete é clicado os produtos selecionados `ProductID` s são listados ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Etapa 4: Adicionando todas e desmarque todos os botões

Se um usuário deseja excluir todos os produtos na página atual, eles devem verificar cada um dos dez as caixas de seleção. Podemos ajudar a acelerar esse processo, adicionando uma todos verificar os botão que, quando clicado, seleciona todas as caixas de seleção na grade. Um botão desmarcar tudo seria igualmente útil.

Adicione dois controles da Web de botão para a página, colocá-los acima GridView. Definir o primeiro um s `ID` à `CheckAll` e seu `Text` propriedade para verificar todos os; defina o segundo uma s `ID` para `UncheckAll` e sua `Text` propriedade para desmarcar todos os.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

Em seguida, crie um método em que a classe code-behind chamado `ToggleCheckState(checkState)` que, quando invocado, enumera os `Products` s GridView `Rows` coleção e define cada caixa de seleção s `Checked` propriedade o valor passado em *checkState*  parâmetro.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

Em seguida, crie `Click` manipuladores de eventos para o `CheckAll` e `UncheckAll` botões. Na `CheckAll` manipulador de eventos de s, chamada simplesmente `ToggleCheckState(true)`; na `UncheckAll`, chame `ToggleCheckState(false)`.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

Com esse código, clicando no botão Verificar todos os faz com que um postback e verificará todas as caixas de seleção no GridView. Da mesma forma, clicando em desmarcar tudo desmarca todas as caixas de seleção. Figura 9 mostra a tela depois no botão Verificar tudo foi verificado.


[![Clicar a verificação de que todas as botão Seleciona todas as caixas de seleção](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)

**Figura 9**: clicando em verificar todos os botão Seleciona todas as caixas de seleção ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))


> [!NOTE]
> Quando a exibição de uma coluna de caixas de seleção, uma abordagem para marcar ou desmarcar todas as caixas de seleção é por meio de uma caixa de seleção na linha de cabeçalho. Além disso, o atual Verifique todos os / desmarque toda a implementação requer um postback. As caixas de seleção podem ser marcado ou desmarcado, no entanto, inteiramente por meio de script do lado do cliente, oferecendo uma experiência de usuário mais fluida. Para explorar usando uma caixa de seleção de linha de cabeçalho para desmarcar todas as e verificar todos os detalhadamente, juntamente com uma discussão sobre como usar técnicas do lado do cliente, fazer check-out [verificando todas as caixas de seleção no Script do lado do cliente usando um GridView e uma caixa de seleção de todos os Check](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Resumo

Em casos em que você precisa para permitir aos usuários escolher um número arbitrário de linhas de um GridView antes de continuar, a adição de uma coluna de caixas de seleção é uma opção. Como vimos neste tutorial, incluindo uma coluna de caixas de seleção no GridView envolve adicionar um TemplateField com um controle de caixa de seleção. Usando um controle da Web (em vez de injetar marcação diretamente no modelo, como fizemos no tutorial anterior) ASP.NET automaticamente se lembra que foram as caixas de seleção e não foram marcadas entre postback. Podemos pode também acessar programaticamente as caixas de seleção no código para determinar se uma determinada caixa de seleção estiver marcada, ou para alterar o estado de ativação.

Este tutorial e o último deles examinou adicionando uma coluna de seletor de linha para o GridView. Nosso próximo tutorial, examinaremos como, com um pouco de trabalho, é possível adicionar funcionalidades de inserção para o GridView.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-a-gridview-column-of-radio-buttons-cs.md)
> [Próximo](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
