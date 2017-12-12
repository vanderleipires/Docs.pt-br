---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
title: "Tratamento de exceções de nível BLL e DAL em uma página ASP.NET (c#) | Microsoft Docs"
author: rick-anderson
description: "Neste tutorial, veremos como exibir uma mensagem de erro informativa e amigável se uma exceção ocorrer durante uma inserção, atualização ou operação de exclusão de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 49d8a66c-3ea8-4087-839f-179d1d94512a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 17157d595e8283628371ff6ad39fe71879e96a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-c"></a>Tratamento de exceções de nível BLL e DAL em uma página ASP.NET (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_CS.exe) ou [baixar PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/datatutorial18cs1.pdf)

> Neste tutorial, veremos como exibir uma mensagem de erro informativa e amigável se uma exceção ocorrer durante uma inserção, atualização ou operação de exclusão de um controle da Web de dados do ASP.NET.


## <a name="introduction"></a>Introdução

Trabalhando com dados de um aplicativo web do ASP.NET usando uma arquitetura de aplicativo em camadas envolve três etapas gerais a seguir:

1. Determine qual método de camada de lógica de negócios precisa ser chamado e o parâmetro de valores para passá-la. Os valores de parâmetro podem ser codificado, por meio de programação atribuído ou entradas inseridas pelo usuário.
2. Invoque o método.
3. Processe os resultados. Ao chamar um método BLL que retorna os dados, isso pode envolver a associação dos dados a um controle da Web de dados. Para métodos BLL que modificam dados, isso pode incluir a executar alguma ação com base em um valor de retorno ou tratar normalmente qualquer exceção que ocorreu na etapa 2.

Como vimos no [tutorial anterior](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md), ObjectDataSource e os dados de controles da Web fornecem pontos de extensibilidade para as etapas 1 e 3. GridView, por exemplo, dispara seu `RowUpdating` evento antes de atribuir seus valores de campo para sua ObjectDataSource `UpdateParameters` coleção; seu `RowUpdated` evento é gerado após o ObjectDataSource concluiu a operação.

Já examinamos os eventos acionam durante a etapa 1 e viu como eles podem ser usados para personalizar os parâmetros de entrada ou cancelar a operação. Neste tutorial voltaremos nossa atenção aos eventos que acionam após a conclusão da operação. Com esses manipuladores de evento de pós-nível que possível, entre outras coisas, determinar se ocorreu uma exceção durante a operação e tratar normalmente, exibindo uma mensagem de erro amigável, informativo na tela, em vez de padrão é a padrão do ASP.NET página de exceção.

Para ilustrar a trabalhar com esses eventos pós-níveis, vamos criar uma página que lista os produtos em um GridView editável. Quando a atualização de um produto, se uma exceção é gerada nosso ASP.NET página será exibida uma mensagem curta acima GridView explicando o que ocorreu um problema. Vamos começar!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Etapa 1: Criando um GridView editável de produtos

No tutorial anterior, criamos um GridView editável com apenas dois campos, `ProductName` e `UnitPrice`. Isso é necessário criar uma sobrecarga adicional para o `ProductsBLL` da classe `UpdateProduct` método, que só aceita três parâmetros de entrada (nome do produto, preço unitário e ID) contrário como um parâmetro para cada campo de produto. Para este tutorial, vamos praticar essa técnica novamente, criando um GridView editável que exibe o nome do produto, quantidade por unidade, o preço unitário e unidades em estoque, mas permite apenas o nome, o preço unitário e a unidades em estoque a ser editado.

Para acomodar esse cenário, precisaremos outra sobrecarga do `UpdateProduct` método, que aceita quatro parâmetros: o nome do produto, preço unitário, unidades em estoque e ID. Adicione o seguinte método para o `ProductsBLL` classe:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample1.cs)]

Com esse método completo, estamos prontos para criar a página do ASP.NET que permite editar estes quatro campos de determinado produto. Abra o `ErrorHandling.aspx` página o `EditInsertDelete` pasta e adicionar um controle GridView à página por meio do Designer. Associar GridView para um novo ObjectDataSource, mapeamento de `Select()` método para o `ProductsBLL` da classe `GetProducts()` método e o `Update()` método para o `UpdateProduct` sobrecarga que acabou de criar.


[![Use a sobrecarga de método UpdateProduct que aceita quatro parâmetros de entrada](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image1.png)

**Figura 1**: Use o `UpdateProduct` método sobrecarga que aceita quatro parâmetros de entrada ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image3.png))


Isso criará um ObjectDataSource com um `UpdateParameters` coleção com quatro parâmetros e um GridView com um campo para cada um dos campos de produto. Marcação declarativa do ObjectDataSource atribui o `OldValuesParameterFormatString` o valor da propriedade `original_{0}`, que fará com que uma exceção porque nossa classe BLL não espera um parâmetro de entrada denominado `original_productID` ser passado. Não se esqueça de remover essa configuração completamente da sintaxe declarativa (ou defini-lo como o valor padrão, `{0}`).

Em seguida, diminuir o GridView para incluir somente o `ProductName`, `QuantityPerUnit`, `UnitPrice`, e `UnitsInStock` BoundFields. Também fique à vontade para aplicar qualquer formatação de nível de campo necessário (como alterar a `HeaderText` propriedades).

No tutorial anterior analisamos como formatar o `UnitPrice` BoundField como uma moeda no modo somente leitura e o modo de edição. Vamos fazer a mesma aqui. Lembre-se de que isso necessário definir o BoundField `DataFormatString` propriedade `{0:c}`, sua `HtmlEncode` propriedade `false`e sua `ApplyFormatInEditMode` para `true`, conforme mostrado na Figura 2.


[![Configurar o UnitPrice BoundField à exibição como uma moeda](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image4.png)

**Figura 2**: configurar o `UnitPrice` BoundField à exibição como uma moeda ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image6.png))


Formatação de `UnitPrice` como uma moeda na interface de edição requer a criação de um manipulador de eventos do GridView `RowUpdating` evento que analisa a cadeia de caracteres de formato de moeda em um `decimal` valor. Lembre-se de que o `RowUpdating` manipulador de eventos do último tutorial também são verificados para garantir que o usuário fornecido um `UnitPrice` valor. No entanto, para este tutorial vamos permitir que o usuário omitir o preço.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample2.cs)]

Nosso GridView inclui um `QuantityPerUnit` BoundField, mas esse BoundField deve ser apenas para fins de exibição e não deve ser editado pelo usuário. Para organizar isso, basta definir 'BoundFields `ReadOnly` propriedade `true`.


[![Verifique o QuantityPerUnit BoundField somente leitura](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image7.png)

**Figura 3**: tornar o `QuantityPerUnit` BoundField somente leitura ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image9.png))


Finalmente, marque a caixa de seleção Habilitar edição de marcas inteligentes do GridView. Após concluir essas etapas de `ErrorHandling.aspx` Designer da página deve ser semelhante à Figura 4.


[![Remova todos deixando apenas o necessário BoundFields e verifique a habilitar edição de caixa de seleção](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image10.png)

**Figura 4**: Remover tudo, exceto o necessário BoundFields e verifique se a edição de caixa de seleção Habilitar ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image12.png))


Neste ponto, temos uma lista de todos os produtos `ProductName`, `QuantityPerUnit`, `UnitPrice`, e `UnitsInStock` campos; no entanto, somente o `ProductName`, `UnitPrice`, e `UnitsInStock` campos podem ser editados.


[![Os usuários agora podem facilmente editar os nomes dos produtos, preços e unidades em estoque campos](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image13.png)

**Figura 5**: nomes de usuários pode agora facilmente editar produtos, preços e campos de unidades em estoque ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image15.png))


## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Etapa 2: Normalmente tratamento de exceções de nível de DAL

Embora nosso GridView editável muito funcione quando os usuários inserir valores válidos para o nome do produto editado, preço e unidades em estoque, inserir valores inválidos resulta em uma exceção. Por exemplo, omitindo o `ProductName` valor faz com que um [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/en-us/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) seja lançada desde o `ProductName` propriedade no `ProdcutsRow` classe tem seu `AllowDBNull` propriedade definida como `false`; se a banco de dados está inoperante, um `SqlException` será lançada pelo TableAdapter ao tentar se conectar ao banco de dados. Sem realizar nenhuma ação, essas exceções bolhas backup da camada de acesso a dados para a camada de lógica de negócios, em seguida, para a página do ASP.NET e, finalmente, no tempo de execução do ASP.NET.

Dependendo de como seu aplicativo web é configurado e se você está visitando o aplicativo de `localhost`, uma exceção não tratada pode resultar em uma página de erro genérico de servidor, um relatório de erro detalhada ou uma página da web fácil de usar. Consulte [Web tratamento de erros do aplicativo no ASP.NET](http://www.15seconds.com/issue/030102.htm) e [elemento customErrors](https://msdn.microsoft.com/en-US/library/h0hfz6fc(VS.80).aspx) para obter mais informações sobre como o tempo de execução do ASP.NET responde a uma exceção não tratada.

A Figura 6 mostra a tela encontrada ao tentar atualizar um produto sem especificar o `ProductName` valor. Esse é o padrão de relatório de erro detalhadas exibido quando recebidas por meio de `localhost`.


[![Omitindo nomes exibirão detalhes da exceção do produto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image16.png)

**Figura 6**: omissão nomes irá exibir detalhes do produto da exceção ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image18.png))


Embora esses detalhes da exceção sejam úteis ao testar um aplicativo, apresentar um usuário final com tela caso ocorra uma exceção é o ideal. Um usuário final provavelmente não sabe o que um `NoNullAllowedException` é ou por que ele foi causado. Uma abordagem melhor é apresentar ao usuário uma mensagem mais amigável explicando que ocorreram problemas ao tentar atualizar o produto.

Se ocorrer uma exceção ao executar a operação, os eventos pós-níveis em ObjectDataSource e o controle da Web de dados fornecem um meio para detectá-lo e cancelar a exceção de ser propagado para o tempo de execução do ASP.NET. Para nosso exemplo, vamos criar um manipulador de eventos do GridView `RowUpdated` evento que determina se uma exceção foi disparado e, nesse caso, exibe os detalhes da exceção em um controle de rótulo.

Comece adicionando um rótulo para a página do ASP.NET, definindo seu `ID` propriedade `ExceptionDetails` e limpar seu `Text` propriedade. Para chamar a atenção do usuário para essa mensagem, defina seu `CssClass` propriedade `Warning`, que é uma classe CSS que adicionamos ao `Styles.css` arquivo no tutorial anterior. Lembre-se de que essa classe CSS faz com que o texto do rótulo a ser exibido em uma fonte vermelha, itálico, negrito, extra grande.


[![Adicionar um controle de Web de rótulo para a página](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image19.png)

**Figura 7**: adicionar um controle de Web de rótulo para a página ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image21.png))


Como queremos que este controle de rótulo Web fique visível apenas imediatamente após uma exceção, defina seu `Visible` propriedade como false no `Page_Load` manipulador de eventos:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample3.cs)]

Com esse código, na primeira visita de página e postagens subsequentes a `ExceptionDetails` controle terá seu `Visible` propriedade definida como `false`. No caso de uma exceção de nível de DAL ou BLL, que podemos detectar o GridView `RowUpdated` manipulador de eventos, vamos definir a `ExceptionDetails` do controle `Visible` propriedade como true. Como manipuladores de eventos de controle de Web ocorrerem após o `Page_Load` manipulador de eventos do ciclo de vida da página, o rótulo será exibido. No entanto, durante o próximo postback, o `Page_Load` manipulador de eventos será revertido a `Visible` propriedade `false`, ocultá-lo novamente do modo de exibição.

> [!NOTE]
> Como alternativa, foi possível remover a necessidade de configuração o `ExceptionDetails` do controle `Visible` propriedade `Page_Load` atribuindo seu `Visible` propriedade `false` na sintaxe declarativa e desativar seu estado de exibição (Configurando seu `EnableViewState` propriedade `false`). Vamos usar essa abordagem alternativa em um tutorial futuras.


Com o controle de rótulo adicionado, nossa próxima etapa é criar o manipulador de eventos para o GridView `RowUpdated` eventos. Seleciona o GridView no Designer, vá para a janela de propriedades e clique no ícone de raio, listando os eventos do GridView. Já deve haver uma entrada para o GridView `RowUpdating` evento, que é criado um manipulador de eventos para esse evento anteriormente neste tutorial. Criar um manipulador de eventos para o `RowUpdated` evento bem.


![Criar um manipulador de eventos para eventos de RowUpdated do GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image22.png)

**Figura 8**: criar um manipulador de eventos do GridView `RowUpdated` evento


> [!NOTE]
> Você também pode criar o manipulador de eventos por meio de listas suspensas na parte superior do arquivo de classe code-behind. Selecione GridView na lista suspensa à esquerda e o `RowUpdated` evento à direita.


Criar este manipulador de eventos, você adicionará o código a seguir para a classe de code-behind da página ASP.NET:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample4.cs)]

Segundo parâmetro de entrada do manipulador de eventos é um objeto do tipo [GridViewUpdatedEventArgs](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), que tem três propriedades de interesse para tratamento de exceções:

- `Exception`uma referência para a exceção lançada; Se nenhuma exceção tiver sido lançada, essa propriedade tem um valor de`null`
- `ExceptionHandled`um valor booliano que indica se a exceção foi tratada no `RowUpdated` manipulador de eventos; se `false` (padrão), a exceção é lançada novamente o percolating até o tempo de execução do ASP.NET
- `KeepInEditMode`Se definido como `true` a linha GridView editada permanece no modo de edição; se `false` (padrão), a linha GridView reverte para o modo somente leitura

Nosso código, em seguida, verifique se `Exception` não é `null`, que significa que uma exceção foi gerada ao executar a operação. Se esse for o caso, desejamos:

- Exiba uma mensagem amigável no `ExceptionDetails` rótulo
- Indicar que a exceção foi tratada
- Manter a linha GridView no modo de edição

O código a seguir realiza esses objetivos:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample5.cs)]

Começa a este manipulador de eventos para verificar se `e.Exception` é `null`. Se não estiver, o `ExceptionDetails` do rótulo `Visible` está definida como `true` e sua `Text` propriedade como "Houve um problema ao atualizar o produto." Os detalhes da exceção real que foi lançada residem no `e.Exception` do objeto `InnerException` propriedade. A exceção interna é examinada e, caso se trate de um determinado tipo, uma mensagem adicional, útil é anexada ao `ExceptionDetails` do rótulo `Text` propriedade. Por fim, o `ExceptionHandled` e `KeepInEditMode` propriedades são definidas como `true`.

A Figura 9 mostra uma captura de tela desta página ao omitir o nome do produto; A Figura 10 mostra os resultados ao inserir um ilegal `UnitPrice` valor (-50).


[![O ProductName BoundField deve conter um valor](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image23.png)

**Figura 9**: O `ProductName` BoundField deve conter um valor ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image25.png))


[![Valores negativos de UnitPrice não são permitido](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image26.png)

**Figura 10**: negativo `UnitPrice` os valores não são permitido ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image28.png))


Definindo o `e.ExceptionHandled` propriedade `true`, o `RowUpdated` manipulador de eventos indicou que tratou a exceção. Portanto, a exceção não se propaga até o tempo de execução do ASP.NET.

> [!NOTE]
> Figuras 9 e 10 mostram uma forma amigável para lidar com exceções geradas devido a entrada de usuário inválido. Idealmente, no entanto, esse de entrada inválido será nunca alcance a camada de lógica de negócios em primeiro lugar, como a página ASP.NET deve garantir que entradas do usuário são válidas antes de chamar o `ProductsBLL` da classe `UpdateProduct` método. Em nosso tutorial Avançar que veremos como adicionar controles de validação para as interfaces de edição e inserção para garantir que os dados enviados para a camada de lógica comercial está em conformidade com as regras de negócio. Os controles de validação não somente impedirá que a chamada do `UpdateProduct` método até que os dados fornecidos pelo usuário é válidos, mas também fornecem uma experiência de usuário mais informativa para identificar problemas de entrada de dados.


## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Etapa 3: Normalmente tratamento de exceções de nível BLL

Ao inserir, atualizar ou excluir os dados, a camada de acesso a dados pode gerar uma exceção caso ocorra um erro de dados. O banco de dados pode estar offline, uma coluna de tabela de banco de dados necessários pode não ter tido um valor especificado ou uma restrição de nível de tabela pode foram violada. Além de exceções estritamente relacionadas a dados, a camada de lógica comercial pode usar exceções para indicar quando as regras de negócio foram violadas. No [criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-cs.md) tutorial, por exemplo, adicionamos uma verificação de regra de negócios ao valor original `UpdateProduct` sobrecarga. Especificamente, se o usuário foi marcar um produto descontinuado, é necessário que ele não seja o único fornecido pelo seu fornecedor. Se essa condição foi violada, um `ApplicationException` foi lançada.

Para o `UpdateProduct` sobrecarga criada neste tutorial, vamos adicionar uma regra de negócio que proíbe o `UnitPrice` campo de ser definida para um novo valor que é mais de duas vezes o original `UnitPrice` valor. Para fazer isso, ajustar o `UpdateProduct` sobrecarga para que ele executa essa verificação e gera um `ApplicationException` se a regra for violada. O método de atualização a seguir:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample6.cs)]

Com essa alteração, qualquer atualização de preço é mais de duas vezes o preço existente fará com que um `ApplicationException` seja gerada. Assim como a exceção gerada da DAL, gerado nesse BLL `ApplicationException` podem ser detectados e tratados no GridView `RowUpdated` manipulador de eventos. Na verdade, o `RowUpdated` código do manipulador de eventos, como escrito, corretamente detectará essa exceção e exibir o `ApplicationException`do `Message` o valor da propriedade. A Figura 11 mostra uma quando um usuário tenta atualizar o preço de Chai a r $50,00, que é mais do que o dobro seu preço atual de US $19,95 de captura de tela.


[![As regras de negócio não permitir aumento de preço mais de duas vezes preço do produto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image29.png)

**Figura 11**: aumentos de preço não permitir que mais de duas vezes o preço de um produto de regras de negócios ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image31.png))


> [!NOTE]
> Idealmente nossas regras de lógica de negócios fosse refatoradas do `UpdateProduct` sobrecargas de método e, em um método comum. Isso é deixado como um exercício para o leitor.


## <a name="summary"></a>Resumo

Durante a inserção, atualização e exclusão de operações, o controle de dados Web e ObjectDataSource envolvidos disparar eventos de pré e pós-níveis que a operação real de delimitador. Como vimos neste tutorial e o anterior, ao trabalhar com um GridView editável a GridView `RowUpdating` evento ser acionado, seguido do ObjectDataSource `Updating` evento, no ponto em que o comando de atualização é feito a ObjectDataSource objeto subjacente. Depois que a operação for concluída, o ObjectDataSource `Updated` evento ser acionado, seguido do GridView `RowUpdated` eventos.

Podemos criar manipuladores de eventos para os eventos de níveis previamente para personalizar os parâmetros de entrada ou para os eventos pós-níveis para inspecionar e responder aos resultados da operação. Manipuladores de evento de pós-nível são mais comumente usados para detectar se ocorreu uma exceção durante a operação. Caso ocorra uma exceção, esses manipuladores de evento de pós-nível opcionalmente podem lidar com a exceção por conta própria. Neste tutorial, vimos como lidar com tal exceção exibindo uma mensagem de erro amigável.

No tutorial de Avançar, veremos como diminuir a probabilidade de exceções decorrentes de problemas de formatação de dados (como inserir um negativo `UnitPrice`). Especificamente, vamos examinar como adicionar controles de validação para as interfaces de edição e inserção.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Liz Shulok. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
[Próximo](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
