---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: Lote de exclusão (VB) | Microsoft Docs
author: rick-anderson
description: Saiba como excluir vários registros de banco de dados em uma única operação. Na camada de Interface do usuário que criamos em um GridView aprimorado criado em uma sessão tut...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: c7a0db8708c95cc348619cadd21514f4bea2e553
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815983"
---
<a name="batch-deleting-vb"></a>Lote de exclusão (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip) ou [baixar PDF](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> Saiba como excluir vários registros de banco de dados em uma única operação. Na camada de Interface do usuário, aproveitam um GridView aprimorado criado em um tutorial anterior. Na camada de acesso a dados, podemos encapsular as várias operações de exclusão em uma transação para garantir que todas as exclusões de êxito ou todas as exclusões são revertidas.


## <a name="introduction"></a>Introdução

O [tutorial anterior](batch-updating-vb.md) explorou como criar um lote usando um GridView editável totalmente de interface de edição. Em situações onde os usuários são normalmente editando vários registros ao mesmo tempo, um lote de interface de edição será exigem muito menos postbacks e contexto de teclado ao mouse switches, melhorando a eficiência do usuário final s. Essa técnica é da mesma forma útil para páginas em que é comum que os usuários excluam muitos registros de uma só vez.

Qualquer pessoa que tenha usado um cliente de email online já está familiarizada com uma das mais comuns lote excluindo interfaces: uma caixa de seleção em cada linha em uma grade com um correspondente excluir todos os itens verificados botão (consulte a Figura 1). Este tutorial é curto em vez disso porque estamos ve feita todo o trabalho pesado nos tutoriais anteriores na criação de interface baseada na web e um método para excluir uma série de registros como uma única operação atômica. No [adicionando uma coluna de GridView de caixas de seleção](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) tutorial, criamos um GridView com uma coluna de caixas de seleção e, nas [encapsulamento de modificações de banco de dados em uma transação](wrapping-database-modifications-within-a-transaction-vb.md) tutorial, criamos um método em a BLL que usaria uma transação para excluir uma `List<T>` de `ProductID` valores. Neste tutorial, vamos aproveitam e nossas experiências anteriores para criar um lote de trabalho, excluindo o exemplo de mesclagem.


[![Cada linha inclui uma caixa de seleção](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**Figura 1**: cada linha inclui uma caixa de seleção ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>Etapa 1: Criando o lote de exclusão de Interface

Como já criamos o lote excluindo a interface na [adicionando uma coluna de GridView de caixas de seleção](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) tutorial, podemos pode simplesmente copiá-lo para `BatchDelete.aspx` em vez de criá-lo a partir do zero. Comece abrindo o `BatchDelete.aspx` página o `BatchData` pasta e o `CheckBoxField.aspx` página no `EnhancedGridView` pasta. Dos `CheckBoxField.aspx` página, vá para a exibição da fonte e copie a marcação entre o `<asp:Content>` marcas conforme mostrado na Figura 2.


[![A marcação declarativa de CheckBoxField.aspx na área de transferência de cópia](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**Figura 2**: Copie a marcação declarativa de `CheckBoxField.aspx` na área de transferência ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image4.png))


Em seguida, vá para a exibição da fonte no `BatchDelete.aspx` e cole o conteúdo da área de transferência dentro a `<asp:Content>` marcas. Copie e cole o código de dentro a classe code-behind no também `CheckBoxField.aspx.vb` para dentro a classe code-behind no `BatchDelete.aspx.vb` (o `DeleteSelectedProducts` botão s `Click` manipulador de eventos, o `ToggleCheckState` método e o `Click` manipuladores de eventos para o `CheckAll` e `UncheckAll` botões). Depois de copiar sobre esse conteúdo, o `BatchDelete.aspx` classe code-behind de página s deve conter o código a seguir:


[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

Depois de copiar sobre a marcação declarativa e código-fonte, reserve um tempo para testar `BatchDelete.aspx` exibindo-o por meio de um navegador. Você deve ver um GridView listando os dez primeiros produtos em um GridView com cada linha que lista o nome do produto s, categoria e preço, juntamente com uma caixa de seleção. Deve haver três botões: verificar todos os desmarcar todos os produtos e excluir selecionado. Clicar no botão Verificar todos os seleciona todas as caixas de seleção, enquanto desmarcar todos os limpa todas as caixas de seleção. Clicar em excluir produtos selecionados exibe uma mensagem que lista o `ProductID` valores dos produtos selecionados, mas não exclui os produtos.


[![A Interface de CheckBoxField.aspx foi movida para BatchDeleting.aspx](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**Figura 3**: A Interface do `CheckBoxField.aspx` foi movido para `BatchDeleting.aspx` ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Etapa 2: Excluir os produtos verificados usando transações

Com o lote interface copiada com êxito para a exclusão `BatchDeleting.aspx`, tudo o que resta fazer é atualizar o código para que o botão excluir produtos selecionados exclui os produtos verificados usando o `DeleteProductsWithTransaction` método no `ProductsBLL` classe. Esse método, adicionado na [de encapsulamento de modificações de banco de dados em uma transação](wrapping-database-modifications-within-a-transaction-vb.md) tutorial, aceita como entrada uma `List(Of T)` de `ProductID` valores e exclui cada um correspondendo `ProductID` dentro do escopo de um transação.

O `DeleteSelectedProducts` botão s `Click` manipulador de eventos atualmente usa o seguinte `For Each` para iterar por meio de cada linha de GridView:


[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

Para cada linha, o `ProductSelector` controle da Web de caixa de seleção é referenciado por meio de programação. Se estiver marcada, a linha s `ProductID` é recuperado do `DataKeys` coleção e o `DeleteResults` rótulo s `Text` propriedade é atualizada para incluir uma mensagem indicando que a linha foi selecionada para exclusão.

O código acima não exclui todos os registros que a chamada para o `ProductsBLL` classe s `Delete` método é comentado. Foram essa exclusão lógica a ser aplicado, o código excluiria os produtos, mas não dentro de uma operação atômica. Ou seja, se as exclusões de alguns primeiro na sequência foi bem-sucedida, mas um posterior falha (talvez devido a uma violação de restrição de chave estrangeira), uma exceção seria lançada, mas esses produtos já excluídos permanecerão excluídos.

Para garantir a atomicidade, precisamos usar em vez disso, o `ProductsBLL` classe s `DeleteProductsWithTransaction` método. Como esse método aceita uma lista de `ProductID` valores, precisamos primeiro compilar essa lista na grade e, em seguida, passá-lo como um parâmetro. Primeiro, criamos uma instância de um `List(Of T)` do tipo `Integer`. Dentro de `For Each` loop, precisamos adicionar os produtos selecionados `ProductID` valores a esse `List(Of T)`. Após o loop isso `List(Of T)` deve ser passado para o `ProductsBLL` classe s `DeleteProductsWithTransaction` método. Atualizar o `DeleteSelectedProducts` botão s `Click` manipulador de eventos com o código a seguir:


[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

O código atualizado cria uma `List(Of T)` do tipo `Integer` (`productIDsToDelete`) e a preenche com o `ProductID` valores para excluir. Após o `For Each` loop, se houver pelo menos um produto selecionado, o `ProductsBLL` classe s `DeleteProductsWithTransaction` método é chamado e passado nesta lista. O `DeleteResults` rótulo também é exibido e os dados se a GridView (de modo que os registros excluídos recentemente não serão exibidos como linhas na grade).

Figura 4 mostra o GridView após um número de linhas foram selecionado para exclusão. Figura 5 mostra a tela imediatamente depois que foi clicado no botão excluir produtos selecionados. Observe que na Figura 5 o `ProductID` valores dos registros excluídos são exibidos no rótulo abaixo GridView e essas linhas não estão mais em GridView.


[![Os produtos selecionados serão excluídos](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**Figura 4**: O selecionado produtos serão excluídos ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image8.png))


[![Os valores de ProductID de produtos excluídos são listados sob o controle GridView](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**Figura 5**: os produtos excluídos `ProductID` valores são listados sob o controle GridView ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image10.png))


> [!NOTE]
> Para testar o `DeleteProductsWithTransaction` atomicidade método s, adicionar manualmente uma entrada de um produto a `Order Details` de tabela e, em seguida, tente excluir o produto (juntamente com outras pessoas). Você receberá uma violação de restrição de chave estrangeira durante a tentativa de excluir o produto com um pedido associado, mas observe como as outras exclusões de produtos selecionados serão revertidas.


## <a name="summary"></a>Resumo

Criação de um lote de exclusão de interface envolve a adição de um GridView com uma coluna de caixas de seleção e controlar uma Web de botão que, quando clicado, excluirá todas as linhas selecionadas como uma única operação atômica. Neste tutorial, criamos uma interface desse tipo por reunir o trabalho feito em dois tutoriais anteriores, [adicionando uma coluna de GridView de caixas de seleção](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) e [encapsulamento de modificações de banco de dados em uma transação](wrapping-database-modifications-within-a-transaction-vb.md). No primeiro tutorial, criamos um GridView com uma coluna de caixas de seleção e no último implementamos um método na BLL que, quando passado um `List(Of T)` de `ProductID` excluído valores, eles todos dentro do escopo de uma transação.

O próximo tutorial, criaremos uma interface para executar inserções em lotes.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Hilton Giesenow e Teresa Murphy. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](batch-updating-vb.md)
> [Próximo](batch-inserting-vb.md)
