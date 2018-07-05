---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: Tratamento de exceções de nível BLL e DAL em uma página ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como exibir uma mensagem de erro informativa e amigável se uma exceção ocorrer durante uma inserção, atualização ou operação de exclusão de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 41a0ec0db4cb2093d7fadd2de7e65ee4874f0063
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394677"
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>Tratamento de exceções de nível BLL e DAL em uma página ASP.NET (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe) ou [baixar PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> Neste tutorial, veremos como exibir uma mensagem de erro informativa e amigável se uma exceção ocorrer durante uma inserção, atualização ou operação de exclusão de um controle da Web de dados do ASP.NET.


## <a name="introduction"></a>Introdução

Trabalhar com dados de um aplicativo web do ASP.NET usando uma arquitetura de aplicativo em camadas envolve três etapas gerais a seguir:

1. Determine qual método de camada de lógica de negócios precisa ser invocado e valores de parâmetro de quais para passá-la. Os valores de parâmetro podem ser codificado, por meio de programação atribuída ou entradas inseridas pelo usuário.
2. Invoque o método.
3. Processe os resultados. Ao chamar um método BLL que retorna os dados, isso pode envolver a associação dos dados a um controle da Web de dados. Para métodos BLL que modificam dados, isso pode incluir a executar alguma ação com base em um valor de retorno ou normalmente lidar com qualquer exceção que surgiu na etapa 2.

Como vimos na [tutorial anterior](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md), o ObjectDataSource e os dados de controles da Web oferecem pontos de extensibilidade para as etapas 1 e 3. O GridView, por exemplo, é acionado seus `RowUpdating` evento antes de atribuir a seus valores de campo para seu ObjectDataSource `UpdateParameters` coleção; sua `RowUpdated` evento é gerado após o ObjectDataSource tenha concluído a operação.

Já examinamos os eventos acionados durante a etapa 1 e viram como eles podem ser usados para personalizar os parâmetros de entrada ou cancelar a operação. Neste tutorial, voltaremos nossa atenção para os eventos acionados depois que a operação foi concluída. Com esses manipuladores de evento de pós-nível que possível, entre outras coisas, determinar se ocorreu uma exceção durante a operação e tratá-la normalmente, exibindo uma mensagem de erro informativa e amigável na tela, em vez de padronizar para o ASP.NET padrão página de exceção.

Para trabalhar com esses eventos pós-níveis de ilustrar, vamos criar uma página que lista os produtos em um GridView editável. Quando a atualização de um produto, se uma exceção é gerada nosso ASP.NET página será exibida uma mensagem curta acima GridView explicando o que ocorreu um problema. Vamos começar!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Etapa 1: Criando um GridView editável de produtos

No tutorial anterior, criamos um GridView editável com apenas dois campos, `ProductName` e `UnitPrice`. Isso necessário criar uma sobrecarga adicional para o `ProductsBLL` da classe `UpdateProduct` método, que só aceita três parâmetros de entrada (nome do produto, preço unitário e ID) oposto como um parâmetro para cada campo de produto. Para este tutorial, vamos treinar essa técnica novamente, criando um GridView editável que exibe o nome do produto, quantidade por unidade, o preço unitário e unidades em estoque, mas só permite que o nome, o preço unitário e a unidades em estoque a ser editado.

Para acomodar esse cenário, precisaremos outra sobrecarga da `UpdateProduct` método, que aceita quatro parâmetros: o nome do produto, preço unitário, unidades em estoque e ID. Adicione o seguinte método para o `ProductsBLL` classe:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

Com esse método completo, estamos prontos para criar a página do ASP.NET que permite a edição dessas quatro campos de produto específico. Abra o `ErrorHandling.aspx` página o `EditInsertDelete` pasta e adicione um controle GridView à página por meio do Designer. Associar o GridView para um novo ObjectDataSource, mapeamento a `Select()` método para o `ProductsBLL` da classe `GetProducts()` método e o `Update()` método para o `UpdateProduct` sobrecarga que acabou de criar.


[![Use a sobrecarga de método UpdateProduct que aceita quatro parâmetros de entrada](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**Figura 1**: Use o `UpdateProduct` método sobrecarga que aceita quatro parâmetros de entrada ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))


Isso criará um ObjectDataSource com uma `UpdateParameters` coleção com quatro parâmetros e um GridView com um campo para cada um dos campos do produto. Marcação declarativa do ObjectDataSource atribui a `OldValuesParameterFormatString` o valor da propriedade `original_{0}`, que fará com que uma exceção, pois a nossa classe BLL não espera um parâmetro de entrada chamado `original_productID` ser passados. Não se esqueça de remover essa configuração completamente da sintaxe declarativa (ou defina-o como o valor padrão, `{0}`).

Em seguida, diminuir o GridView para incluir somente o `ProductName`, `QuantityPerUnit`, `UnitPrice`, e `UnitsInStock` BoundFields. Também fique à vontade aplicar qualquer formatação de nível de campo necessário (por exemplo, alterar o `HeaderText` propriedades).

No tutorial anterior analisamos como formatar o `UnitPrice` BoundField como uma moeda no modo somente leitura e modo de edição. Vamos fazer o mesmo aqui. Lembre-se de que essa configuração exigida a BoundField `DataFormatString` propriedade para `{0:c}`, sua `HtmlEncode` propriedade `false`e sua `ApplyFormatInEditMode` para `true`, conforme mostrado na Figura 2.


[![Configurar o UnitPrice BoundField à exibição como uma moeda](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**Figura 2**: configurar o `UnitPrice` BoundField à exibição como uma moeda ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))


Formatação de `UnitPrice` como uma moeda na interface de edição exige a criação de um manipulador de eventos para o GridView `RowUpdating` evento que analisa a cadeia de caracteres de formato de moeda em um `decimal` valor. Lembre-se de que o `RowUpdating` manipulador de eventos do último tutorial também são verificados para garantir que o usuário fornecido um `UnitPrice` valor. No entanto, para este tutorial vamos permitir que o usuário omita o preço.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

Nosso GridView inclui um `QuantityPerUnit` BoundField, mas esse BoundField deve ser apenas para fins de exibição e não deve ser editável pelo usuário. Para organizar a isso, basta definir 'BoundFields `ReadOnly` propriedade para `true`.


[![Tornar o QuantityPerUnit BoundField somente leitura](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**Figura 3**: Verifique a `QuantityPerUnit` BoundField somente leitura ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))


Por fim, marque a caixa de seleção Habilitar edição na marca inteligente do GridView. Depois de concluir essas etapas o `ErrorHandling.aspx` Designer da página deve ser semelhante à Figura 4.


[![Remover tudo, exceto os necessários BoundFields e verifique a habilitar a caixa de seleção de edição](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**Figura 4**: Remover tudo, exceto as necessárias BoundFields e a habilitar a edição de caixa de seleção ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))


Neste ponto, temos uma lista de todos os produtos `ProductName`, `QuantityPerUnit`, `UnitPrice`, e `UnitsInStock` campos; no entanto, apenas o `ProductName`, `UnitPrice`, e `UnitsInStock` campos podem ser editados.


[![Os usuários agora podem editar facilmente os nomes dos produtos, preços e unidades no estoque campos](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**Figura 5**: nomes de usuários pode agora facilmente editar produtos, preços e campos de unidades em estoque ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Etapa 2: Normalmente tratamento de exceções de nível de DAL

Embora nosso GridView editável brilhante funcione quando os usuários inserem valores válidos para o nome do produto editado, preço e unidades no estoque, inserir valores ilegais resulta em uma exceção. Por exemplo, omitindo o `ProductName` valor faz com que um [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) seja lançada desde o `ProductName` propriedade no `ProdcutsRow` classe tem seu `AllowDBNull` propriedade definida como `false`; se a banco de dados está inoperante, uma `SqlException` serão geradas por TableAdapter ao tentar se conectar ao banco de dados. Sem realizar nenhuma ação, essas exceções propagam para cima da camada de acesso de dados para a camada de lógica de negócios, em seguida, para a página do ASP.NET e, finalmente, para o tempo de execução do ASP.NET.

Dependendo de como seu aplicativo web é configurado e se você está visitando o aplicativo de `localhost`, uma exceção sem tratamento pode resultar em uma página de erro de servidor genérico, um relatório de erro detalhadas ou uma página da web fácil de usar. Ver [tratamento de erros de aplicativo do Web em ASP.NET](http://www.15seconds.com/issue/030102.htm) e o [customErrors elemento](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) para obter mais informações sobre como o tempo de execução do ASP.NET responde a uma exceção não percebida.

Figura 6 mostra a tela encontrada ao tentar atualizar um produto sem especificar o `ProductName` valor. Esse é o padrão de relatório de erro detalhadas exibido quando chegando `localhost`.


[![Detalhes da exceção do produto nome exibirão a omissão](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**Figura 6**: a omissão de detalhes da exibição serão exceção do produto nome ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))


Embora esses detalhes da exceção são úteis ao testar um aplicativo, apresentando um usuário final com a tela diante de uma exceção é inferior ao ideal. Um usuário final provavelmente não sabe o que um `NoNullAllowedException` é ou por que ele foi causado. Uma abordagem melhor é apresentar ao usuário uma mensagem mais amigável, explicando que ocorreram problemas ao tentar atualizar o produto.

Se ocorrer uma exceção ao executar a operação, os eventos pós-níveis em ObjectDataSource e o controle de Web de dados fornecem um meio para detectá-lo e cancelar a exceção seja propagado para o tempo de execução do ASP.NET. Para nosso exemplo, vamos criar um manipulador de eventos para o GridView `RowUpdated` evento que determina se uma exceção foi disparado e, em caso afirmativo, exibe os detalhes da exceção em um controle de Web de rótulo.

Comece adicionando um rótulo para a página do ASP.NET, definindo sua `ID` propriedade para `ExceptionDetails` e a limpeza dos seus `Text` propriedade. Para chamar a atenção do usuário a essa mensagem, defina suas `CssClass` propriedade para `Warning`, que é uma classe CSS que adicionamos o `Styles.css` arquivo no tutorial anterior. Lembre-se de que essa classe CSS faz com que o texto do rótulo a ser exibido em uma fonte vermelha, itálico, negrito, extra grande.


[![Adicionar um controle de Web do rótulo para a página](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**Figura 7**: adicionar um controle de Web do rótulo para a página ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))


Como queremos que esse controle de rótulo Web fique visível apenas imediatamente após uma exceção ocorreu, defina suas `Visible` a propriedade como false no `Page_Load` manipulador de eventos:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

Com esse código, na primeira visita de página e postbacks subsequentes de `ExceptionDetails` controle terá sua `Visible` propriedade definida como `false`. Diante de uma exceção de DAL ou de nível BLL, que podemos detectar o GridView `RowUpdated` manipulador de eventos, definiremos o `ExceptionDetails` do controle `Visible` propriedade como true. Uma vez que os manipuladores de eventos de controle de Web ocorrem após o `Page_Load` manipulador de eventos do ciclo de vida da página, o rótulo será exibido. No entanto, durante o próximo postback, o `Page_Load` manipulador de eventos será revertido a `Visible` propriedade de volta para `false`, ocultando-o de modo de exibição novamente.

> [!NOTE]
> Como alternativa, podemos pode remover a necessidade de configuração de `ExceptionDetails` do controle `Visible` propriedade no `Page_Load` atribuindo seu `Visible` propriedade `false` na sintaxe declarativa e desabilitando o estado de exibição (definindo seu `EnableViewState` propriedade para `false`). Vamos usar essa abordagem alternativa em um tutorial futuro.


Com o controle de rótulo adicionado, nossa próxima etapa é criar o manipulador de eventos para o GridView `RowUpdated` eventos. Seleciona o GridView no Designer, vá para a janela de propriedades e clique no ícone de raio, listando os eventos do GridView. Já deve haver uma entrada para o GridView `RowUpdating` evento, como criamos um manipulador de eventos para este evento neste tutorial. Crie um manipulador de eventos para o `RowUpdated` evento também.


![Criar um manipulador de eventos para eventos de RowUpdated do GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**Figura 8**: criar um manipulador de eventos para o GridView `RowUpdated` evento


> [!NOTE]
> Você também pode criar o manipulador de eventos por meio de listas suspensas na parte superior do arquivo de classe code-behind. Selecione o GridView na lista suspensa à esquerda e o `RowUpdated` evento à direita.


Criar esse manipulador de eventos, você adicionará o código a seguir à classe de code-behind da página ASP.NET:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

Segundo parâmetro de entrada do manipulador de eventos é um objeto do tipo [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), que tem três propriedades de interesse para tratamento de exceções:

- `Exception` uma referência para a exceção lançada; Se nenhuma exceção foi gerada, essa propriedade terá um valor de `null`
- `ExceptionHandled` um valor booliano que indica se a exceção foi tratada na `RowUpdated` manipulador de eventos; se `false` (padrão), a exceção é lançada novamente, o se originou bem antes até o tempo de execução do ASP.NET
- `KeepInEditMode` Se definido como `true` da linha editada do GridView permanecerá no modo de edição; se `false` (padrão), a linha de GridView será revertido novamente para o modo somente leitura

Nosso código, em seguida, verifique se `Exception` não é `null`, que significa que uma exceção foi gerada ao executar a operação. Se esse for o caso, desejamos:

- Exiba uma mensagem amigável no `ExceptionDetails` rótulo
- Indicar que a exceção foi tratada
- Manter a linha de GridView no modo de edição

O código a seguir realiza esses objetivos:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

Esse manipulador de eventos começa verificando se `e.Exception` é `null`. Se não estiver, o `ExceptionDetails` do rótulo `Visible` estiver definida como `true` e sua `Text` propriedade como "Ocorreu um problema ao atualizar o produto." Os detalhes da exceção real que foi lançada residem na `e.Exception` do objeto `InnerException` propriedade. A exceção interna é examinada e, se ele for de um tipo específico, uma mensagem adicional e útil é acrescentada para o `ExceptionDetails` do rótulo `Text` propriedade. Por fim, o `ExceptionHandled` e `KeepInEditMode` propriedades são definidas como `true`.

A Figura 9 mostra uma captura de tela desta página ao omitir o nome do produto; Figura 10 mostra os resultados ao inserir um ilegal `UnitPrice` valor (-50).


[![O ProductName BoundField deve conter um valor](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**Figura 9**: O `ProductName` BoundField deve conter um valor ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))


[![Os valores de UnitPrice negativos não são permitido](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**Figura 10**: negativo `UnitPrice` valores não são permitido ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))


Definindo o `e.ExceptionHandled` propriedade para `true`, o `RowUpdated` manipulador de eventos indicou que tratou a exceção. Portanto, a exceção não serão propagadas até o tempo de execução do ASP.NET.

> [!NOTE]
> As figuras 9 e 10 mostram uma forma amigável para lidar com as exceções geradas devido a entrada do usuário inválido. O ideal é que, no entanto, tal de entrada inválido será nunca alcance a camada de lógica de negócios em primeiro lugar, como a página ASP.NET deve garantir entradas do usuário são válidas antes de invocar o `ProductsBLL` da classe `UpdateProduct` método. Em nosso próximo tutorial, veremos como adicionar controles de validação às interfaces de edição e inserção para garantir que os dados enviados para a camada de lógica comercial está em conformidade com as regras de negócio. Os controles de validação não somente impedirá que a invocação do `UpdateProduct` método até que os dados fornecidos pelo usuário é válidos, mas também fornecem uma experiência de usuário mais informativa para identificar problemas de entrada de dados.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Etapa 3: Normalmente tratamento de exceções de nível BLL

Ao inserir, atualizar ou excluir dados, a camada de acesso a dados pode gerar uma exceção em caso de um erro relacionado a dados. O banco de dados pode estar offline, uma coluna de tabela do banco de dados necessário não pode ter tido um valor especificado ou uma restrição de nível de tabela têm foi violada. Além de exceções de dados estritamente relacionados, a camada de lógica comercial pode usar exceções para indicar quando as regras de negócio foram violadas. No [criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-vb.md) tutorial, por exemplo, adicionamos uma verificação de regra de negócios para o original `UpdateProduct` de sobrecarga. Especificamente, se o usuário foi marcar um produto como Descontinuado, é necessário que ele não seja o único fornecido pelo seu fornecedor. Se essa condição foi violada, um `ApplicationException` foi lançada.

Para o `UpdateProduct` sobrecarga criado neste tutorial, vamos adicionar uma regra de negócio que proíbe a `UnitPrice` campo que está sendo definido como um novo valor que é mais de duas vezes o original `UnitPrice` valor. Para fazer isso, ajustar o `UpdateProduct` sobrecarga para que ele executa essa verificação e lança um `ApplicationException` se a regra é violada. O método atualizado a seguir:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

Com essa alteração, qualquer atualização de preço é mais de duas vezes o preço existente fará com que um `ApplicationException` seja lançada. Assim como a exceção gerada da DAL, acionado nesse BLL `ApplicationException` pode ser detectado e manipulado no GridView `RowUpdated` manipulador de eventos. Na verdade, o `RowUpdated` código do manipulador de eventos, como escrito, corretamente detectará essa exceção e exibirá o `ApplicationException`do `Message` valor da propriedade. Figura 11 mostra uma tela de captura quando um usuário tenta atualizar o preço de Chai para US $50,00, que é a mais de duas vezes seu preço atual de US $ 19,95 para venda.


[![As regras de negócio não permitir a aumentos de preço que mais de duas vezes o preço de um produto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**Figura 11**: os aumentos de preço não permitir que mais de duas vezes o preço de um produto de regras de negócios ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))


> [!NOTE]
> O ideal é que nossas regras de lógica de negócios fosse refatoradas do `UpdateProduct` sobrecargas de método e, em um método comum. Isso é deixado como um exercício para o leitor.


## <a name="summary"></a>Resumo

Durante a inserção, atualização e exclusão de operações, o controle de Web de dados e o ObjectDataSource envolvidos disparam eventos de pré e pós-níveis que a operação real de delimitador. Como vimos neste tutorial e o anterior, ao trabalhar com um GridView editável do GridView `RowUpdating` evento é acionado, seguido do ObjectDataSource `Updating` evento, no ponto em que o comando de atualização é feito para o ObjectDataSource objeto subjacente. Depois que a operação for concluída, o ObjectDataSource `Updated` evento é acionado, seguido do GridView `RowUpdated` eventos.

Podemos criar manipuladores de eventos para os eventos de pré-níveis para personalizar os parâmetros de entrada ou para os eventos pós-níveis para inspecionar e responder aos resultados da operação. Manipuladores de evento de pós-nível são mais comumente usados para detectar se uma exceção ocorreu durante a operação. Diante de uma exceção, esses manipuladores de evento de pós-nível opcionalmente podem lidar com a exceção por conta própria. Neste tutorial vimos como tratar essa exceção, exibindo uma mensagem de erro amigável.

No próximo tutorial, veremos como diminuir a probabilidade de exceções decorrentes de problemas de formatação de dados (como inserir um negativo `UnitPrice`). Especificamente, vamos examinar como adicionar controles de validação às interfaces de edição e inserção.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Liz Shulok. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [Próximo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
