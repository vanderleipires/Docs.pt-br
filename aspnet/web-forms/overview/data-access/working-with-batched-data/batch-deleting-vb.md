---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: Lote excluindo (VB) | Microsoft Docs
author: rick-anderson
description: Saiba como excluir vários registros de banco de dados em uma única operação. Na camada de Interface do usuário que criamos em um GridView avançado criado no anterior tut...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d17ec6ec20be65b3ec9369f1c5a08d5970ec0dd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880109"
---
<a name="batch-deleting-vb"></a>Lote excluindo (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip) ou [baixar PDF](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> Saiba como excluir vários registros de banco de dados em uma única operação. Na camada de Interface do usuário é baseiam-se um GridView avançado criado um tutorial anterior. Na camada de acesso a dados encerrar as várias operações de exclusão em uma transação para garantir que todas as exclusões de êxito ou todas as exclusões são revertidas.


## <a name="introduction"></a>Introdução

O [tutorial anterior](batch-updating-vb.md) explorados como criar um lote edição interface usando um GridView totalmente editável. Em situações onde usuários são geralmente editando vários registros de uma vez, um lote edição interface exigirá muito menos postbacks e contexto de teclado para mouse comutadores, melhorando a eficiência de s do usuário final. Essa técnica é útil da mesma forma para páginas em que é comum que os usuários excluir vários registros de uma só vez.

Qualquer pessoa que tenha usado um cliente de email online já esteja familiarizada com um dos mais comuns lote excluindo interfaces: uma caixa de seleção em cada linha em uma grade com um correspondente excluir todos os itens verificados botão (consulte a Figura 1). Este tutorial é curto em vez disso, porque podemos ve feita todo o trabalho de disco rígido nos tutoriais anteriores ao criar a interface baseada na web e um método para excluir uma série de registros como uma única operação atômica. No [adicionando uma coluna de GridView das caixas de seleção](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) tutorial criamos um GridView com uma coluna de caixas de seleção e no [encapsulamento modificações de banco de dados em uma transação](wrapping-database-modifications-within-a-transaction-vb.md) tutorial criamos um método em a BLL que usa uma transação para excluir um `List<T>` de `ProductID` valores. Neste tutorial, vamos construir em cima e mesclar nossas experiências anteriores para criar um lote de trabalho excluindo exemplo.


[![Cada linha incluem uma caixa de seleção](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**Figura 1**: cada linha incluem uma caixa de seleção ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>Etapa 1: Criar lote excluindo Interface

Como já criamos o lote excluindo interface no [adicionando uma coluna de GridView das caixas de seleção](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) tutorial, podemos pode simplesmente copiar a `BatchDelete.aspx` em vez de criá-lo a partir do zero. Comece abrindo o `BatchDelete.aspx` página o `BatchData` pasta e o `CheckBoxField.aspx` página o `EnhancedGridView` pasta. Do `CheckBoxField.aspx` página, vá para o modo de exibição de fonte e copie a marcação entre o `<asp:Content>` marcas conforme mostrado na Figura 2.


[![Copie a marcação declarativa de CheckBoxField.aspx na área de transferência](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**Figura 2**: copiar a marcação declarativa de `CheckBoxField.aspx` na área de transferência ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image4.png))


Em seguida, vá para a exibição da fonte `BatchDelete.aspx` e cole o conteúdo da área de transferência dentro de `<asp:Content>` marcas. Também, copie e cole o código de dentro da classe por trás do código em `CheckBoxField.aspx.vb` para dentro da classe por trás do código em `BatchDelete.aspx.vb` (o `DeleteSelectedProducts` botão s `Click` manipulador de eventos, o `ToggleCheckState` método e o `Click` manipuladores de eventos para o `CheckAll` e `UncheckAll` botões). Depois de copiar esse conteúdo, a `BatchDelete.aspx` classe code-behind de página s deve conter o código a seguir:


[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

Depois de copiar as declarativa marcação e código-fonte, reserve um tempo para testar `BatchDelete.aspx` exibindo-o por meio de um navegador. Você deve ver um GridView listando os dez primeiros produtos em um GridView com cada linha que lista o nome do produto s, categoria e preço junto com uma caixa de seleção. Deve haver três botões: Verifique todos os, desmarque todas as e excluir produtos selecionados. Clicar no botão Verificar todos os seleciona todas as caixas de seleção, enquanto desmarque todos os limpa todas as caixas de seleção. Clicando em excluir produtos selecionados exibe uma mensagem que lista o `ProductID` valores dos produtos selecionados, mas não excluir, na verdade, os produtos.


[![A Interface de CheckBoxField.aspx foi movida para BatchDeleting.aspx](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**Figura 3**: A Interface de `CheckBoxField.aspx` foi movido para `BatchDeleting.aspx` ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Etapa 2: Excluindo os produtos verificados usando transações

Com o lote interface copiado com êxito para a exclusão `BatchDeleting.aspx`, tudo o que resta é atualizar o código para que o botão excluir produtos selecionados exclui os produtos verificados usando o `DeleteProductsWithTransaction` método o `ProductsBLL` classe. Esse método, adicionado no [encapsulamento modificações de banco de dados em uma transação](wrapping-database-modifications-within-a-transaction-vb.md) tutorial, aceita como entrada uma `List(Of T)` de `ProductID` valores e exclui cada correspondente `ProductID` dentro do escopo de um transação.

O `DeleteSelectedProducts` botão s `Click` manipulador de eventos atualmente usa os seguintes `For Each` para iterar por meio de cada linha GridView:


[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

Para cada linha, o `ProductSelector` controle da Web da caixa de seleção é referenciado por meio de programação. Se estiver marcada, a linha s `ProductID` é recuperado do `DataKeys` coleção e o `DeleteResults` rótulo s `Text` propriedade é atualizada para incluir uma mensagem indicando que a linha foi selecionada para exclusão.

O código acima, na verdade, não exclui qualquer registro como a chamada para o `ProductsBLL` classe s `Delete` método está marcado como ignorado. Foram essa lógica de exclusão a ser aplicado, o código excluirá os produtos, mas não dentro de uma operação atômica. Ou seja, se as exclusões de alguns primeiro na sequência foi bem-sucedida, mas um posterior falha (talvez devido a uma violação de restrição de chave estrangeira), uma exceção será lançada, mas os produtos já excluídos permaneceria excluídos.

Para garantir a atomicidade, precisamos em vez disso, use o `ProductsBLL` classe s `DeleteProductsWithTransaction` método. Como esse método aceita uma lista de `ProductID` valores, é preciso primeiro compilar esta lista da grade e, em seguida, passa-a como um parâmetro. Primeiro, criamos uma instância de um `List(Of T)` do tipo `Integer`. Dentro de `For Each` loop que precisamos adicionar os produtos selecionados `ProductID` valores para este `List(Of T)`. Após o loop isso `List(Of T)` devem ser passados para o `ProductsBLL` classe s `DeleteProductsWithTransaction` método. Atualização de `DeleteSelectedProducts` botão s `Click` manipulador de eventos com o código a seguir:


[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

O código atualizado cria um `List(Of T)` do tipo `Integer` (`productIDsToDelete`) e o preenche com o `ProductID` valores para excluir. Após o `For Each` loop, se houver pelo menos um produto selecionado, o `ProductsBLL` classe s `DeleteProductsWithTransaction` método é chamado e passado a esta lista. O `DeleteResults` rótulo também é exibido e os dados religados a GridView (de modo que os registros excluídos recentemente não aparecem como linhas na grade).

A Figura 4 mostra o GridView, após um número de linhas foram selecionado para exclusão. Figura 5 mostra a tela imediatamente após clicou no botão excluir produtos selecionados. Observe que na Figura 5 a `ProductID` valores de registros excluídos são exibidos no rótulo abaixo GridView e as linhas não estão mais em GridView.


[![Os produtos selecionados serão excluídos](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**Figura 4**: O selecionado produtos serão excluídos ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image8.png))


[![Os valores de ProductID produtos excluído são listados abaixo a GridView](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**Figura 5**: os produtos excluído `ProductID` valores estão listados abaixo a GridView ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image10.png))


> [!NOTE]
> Para testar o `DeleteProductsWithTransaction` atomicidade método s, adicione manualmente uma entrada de um produto a `Order Details` de tabela e, em seguida, tente excluir o produto (junto com outros). Você receberá uma violação de restrição de chave estrangeira durante a tentativa de excluir o produto com um pedido associado, mas observe como as outras exclusões de produtos selecionados serão revertidas.


## <a name="summary"></a>Resumo

Criar um lote excluindo interface envolve a adição de um GridView com uma coluna de caixas de seleção e controle da Web de um botão que, quando clicado, excluirá todas as linhas selecionadas como uma única operação atômica. Neste tutorial, criamos tal interface por montando o trabalho feito em dois tutoriais anteriores, [adicionando uma coluna de GridView das caixas de seleção](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) e [encapsulamento modificações de banco de dados em uma transação](wrapping-database-modifications-within-a-transaction-vb.md). No primeiro tutorial, criamos um GridView com uma coluna de caixas de seleção e no último implementamos um método na BLL que, quando passado um `List(Of T)` de `ProductID` excluído valores, eles todos dentro do escopo de uma transação.

O seguinte tutorial, criaremos uma interface para executar inserções em lotes.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Giesenow Hilton e Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](batch-updating-vb.md)
> [Próximo](batch-inserting-vb.md)
