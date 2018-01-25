---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: Examinando os eventos associados inserindo, atualizando e excluindo (c#) | Microsoft Docs
author: rick-anderson
description: "Neste tutorial, examinaremos usando os eventos que ocorrem antes, durante e após uma inserção, atualização ou exclusão de operação de um controle da Web de dados do ASP.NET. W..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 93da23d58d1ba73c5b97f42631d036dd364de24d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Examinando os eventos associados ao inserindo, atualizando e excluindo (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) ou [baixar PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> Neste tutorial, examinaremos usando os eventos que ocorrem antes, durante e após uma inserção, atualização ou exclusão de operação de um controle da Web de dados do ASP.NET. Também veremos como personalizar a interface de edição para atualizar apenas um subconjunto dos campos do produto.


## <a name="introduction"></a>Introdução

Ao usar a inserção internas, editar ou excluir recursos de controles GridView, DetailsView ou FormView, várias etapas ocorrer quando o usuário final conclui o processo de adicionar um novo registro ou atualizar ou excluir um registro existente. Conforme abordado no [tutorial anterior](an-overview-of-inserting-updating-and-deleting-data-cs.md), quando uma linha é editada em GridView no botão de edição é substituído por botões Cancelar e de atualização e a ativar BoundFields em caixas de texto. Depois que o usuário final atualiza os dados e clica em atualizar, as etapas a seguir são executadas em um postback:

1. GridView preenche seu ObjectDataSource `UpdateParameters` com campos de identificação exclusivo do Registro editado (por meio de `DataKeyNames` propriedade) junto com os valores inseridos pelo usuário
2. GridView invoca seu ObjectDataSource `Update()` método, que por sua vez chama o método apropriado no objeto subjacente (`ProductsDAL.UpdateProduct`, em nosso tutorial anterior)
3. Os dados subjacentes, que agora incluem as alterações atualizadas, é vinculada outra vez a GridView

Durante essa sequência de etapas, um número de eventos acionados, permitindo a criação de manipuladores de eventos para adicionar lógica personalizada quando for necessário. Por exemplo, antes do etapa 1, GridView `RowUpdating` evento ser acionado. Neste ponto, podemos pode, cancelar a solicitação de atualização se houver algum erro de validação. Quando o `Update()` método é chamado, o ObjectDataSource `Updating` evento ser acionado, fornecendo uma oportunidade de adicionar ou personalize os valores de qualquer um do `UpdateParameters`. Depois que o ObjectDataSource a base do método do objeto completou a execução, o ObjectDataSource `Updated` é gerado. Um manipulador de eventos para o `Updated` evento pode inspecionar os detalhes sobre a operação de atualização, como quantas linhas foram afetadas e se ocorreu uma exceção. Por fim, após a do etapa 2, GridView `RowUpdated` evento ser acionado; um manipulador de eventos para esse evento pode examinar informações adicionais sobre a operação de atualização apenas executadas.

A Figura 1 mostra esta série de eventos e etapas ao atualizar um GridView. O padrão de evento na Figura 1 não é exclusivo para a atualização com um controle GridView. Inserindo, atualizando ou excluindo dados de GridView, DetailsView ou FormView precipitates a mesma sequência de eventos de pré e pós-níveis para o controle de dados Web e ObjectDataSource.


[![Uma série de pré e pós-eventos acionados quando a atualização de dados em um controle GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Figura 1**: uma série de pré e pós-eventos acionados quando atualizando dados em um controle GridView ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))


Neste tutorial, que examinaremos usando esses eventos para estender o internos inserindo, atualizando e excluindo recursos dos dados ASP.NET controles da Web. Também veremos como personalizar a interface de edição para atualizar apenas um subconjunto dos campos do produto.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Etapa 1: Atualizar um produto`ProductName`e`UnitPrice`campos

As interfaces de edição do tutorial anterior *todos os* campos de produto que não eram somente leitura tinha que ser incluído. Se tivéssemos remover um campo de GridView - dizer `QuantityPerUnit` - ao atualizar os dados de controle de Web de dados não definiria o ObjectDataSource `QuantityPerUnit` `UpdateParameters` valor. ObjectDataSource deve passar em uma `null` valor para o `UpdateProduct` método de camada de lógica de negócios (BLL), que alteraria o registro de banco de dados editados `QuantityPerUnit` coluna para um `NULL` valor. Da mesma forma, se um campo obrigatório, como `ProductName`, é removido da interface de edição, a atualização falhará com um "*'ProductName' da coluna não permite nulls*" exceção. O motivo para esse comportamento foi porque o ObjectDataSource foi configurado para chamar o `ProductsBLL` da classe `UpdateProduct` método, que espera um parâmetro de entrada para cada um dos campos de produto. Portanto, do ObjectDataSource `UpdateParameters` coleção continha um parâmetro para cada método de parâmetros de entrada.

Se você deseja fornecer um controle da Web que permite ao usuário final atualizar somente um subconjunto de campos de dados, precisamos programaticamente definir ausentes `UpdateParameters` valores do ObjectDataSource `Updating` manipulador de eventos ou criar e chamar um método BLL que espera apenas um subconjunto dos campos. Vamos explorar essa última abordagem.

Especificamente, vamos criar uma página que exibe apenas o `ProductName` e `UnitPrice` campos em um GridView editável. Interface de edição do GridView este só permitirá que o usuário atualizar os dois campos exibidos, `ProductName` e `UnitPrice`. Como essa interface edição fornece somente um subconjunto de campos de um produto, é necessário criar um ObjectDataSource que usa o BLL existente `UpdateProduct` método e os valores de campo ausente do produto definiu programaticamente no seu `Updating` evento manipulador de ou é necessário criar um novo método BLL que espera apenas o subconjunto de campos definidos em GridView. Para este tutorial, vamos usar a última opção e criar uma sobrecarga de `UpdateProduct` método, que leva em apenas três parâmetros de entrada: `productName`, `unitPrice`, e `productID`:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Como o original `UpdateProduct` essa sobrecarga de método, inicia para verificar se há um produto no banco de dados com especificado `ProductID`. Se não, ele retorna `false`, indicando que houve falha na solicitação para atualizar as informações de produto. Caso contrário, ele atualiza o registro de produto existente `ProductName` e `UnitPrice` campos adequadamente e confirma a atualização chamando o TableAdpater `Update()` método, passando o `ProductsRow` instância.

Com essa adição a nossas `ProductsBLL` classe, estamos prontos para criar a interface de GridView simplificada. Abra o `DataModificationEvents.aspx` no `EditInsertDelete` pasta e adicionar um controle GridView à página. Criar um novo ObjectDataSource e configurá-lo para usar o `ProductsBLL` classe com seu `Select()` mapeamento de método `GetProducts` e sua `Update()` mapeamento de método para o `UpdateProduct` sobrecarga que recebe apenas o `productName`, `unitPrice`, e `productID` parâmetros de entrada. A Figura 2 mostra o assistente Criar fonte de dados ao mapear o ObjectDataSource `Update()` método para o `ProductsBLL` classe do novo `UpdateProduct` sobrecarga do método.


[![Mapear o método de Update () do ObjectDataSource para a sobrecarga de UpdateProduct novo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Figura 2**: mapear o ObjectDataSource `Update()` método para o novo `UpdateProduct` de sobrecarga ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))


Como o nosso exemplo inicialmente apenas precisará da capacidade para editar dados, mas não para inserir ou excluir registros, reserve um tempo para indicar explicitamente que o ObjectDataSource `Insert()` e `Delete()` métodos não devem ser mapeados para qualquer uma da `ProductsBLL` métodos da classe indo para as guias INSERT e DELETE e escolhendo (nenhum) na lista suspensa.


[![Escolha (nenhum) na lista suspensa para a inserção e excluir guias](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Figura 3**: escolha (nenhum) da lista suspensa para a inserção e excluir guias ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))


Depois de concluir este assistente, marque a caixa de seleção Habilitar edição de marcas inteligentes do GridView.

Com a conclusão do assistente Criar fonte de dados e a associação que o GridView, o Visual Studio criou a sintaxe declarativa para ambos os controles. Vá para o modo de exibição de fonte para inspecionar declarativo do ObjectDataSource, que é mostrado abaixo:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Como não há nenhum mapeamento para o ObjectDataSource `Insert()` e `Delete()` métodos, não há nenhum `InsertParameters` ou `DeleteParameters` seções. Além disso, desde o `Update()` método é mapeado para o `UpdateProduct` sobrecarga do método que só aceita três parâmetros de entrada, o `UpdateParameters` seção tem apenas três `Parameter` instâncias.

Observe que o ObjectDataSource `OldValuesParameterFormatString` está definida como `original_{0}`. Essa propriedade é definida automaticamente pelo Visual Studio ao usar o Assistente Configurar fonte de dados. No entanto, desde que nossos métodos BLL não espera que o original `ProductID` valor a ser passado, remova essa atribuição de propriedade completamente sintaxe declarativa do ObjectDataSource.

> [!NOTE]
> Se você simplesmente limpar o `OldValuesParameterFormatString` valor da propriedade na janela de propriedades na exibição de Design, a propriedade ainda existirá na sintaxe declarativa, mas será definido como uma cadeia de caracteres vazia. Remova a propriedade completamente da sintaxe declarativa ou, na janela Propriedades, defina o valor como o padrão, `{0}`.


Enquanto o ObjectDataSource só `UpdateParameters` para nome, preço e ID do produto, o Visual Studio adicionou um BoundField ou CheckBoxField em GridView para cada um dos campos do produto.


[![GridView contém um BoundField ou CheckBoxField para cada um dos campos do produto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Figura 4**: GridView contém um BoundField ou CheckBoxField para cada um dos campos do produto ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))


Quando o usuário final edita um produto e clica no botão de atualização, o GridView enumera os campos que não eram somente leitura. Ele define o valor do parâmetro correspondente no ObjectDataSource `UpdateParameters` coleção para o valor inserido pelo usuário. Se não houver um parâmetro correspondente, GridView adiciona um à coleção. Portanto, se o nosso GridView contém BoundFields e CheckBoxFields para todos os campos do produto, ObjectDataSource termina invocando o `UpdateProduct` sobrecarga que recebe todos esses parâmetros, apesar do fato de que o ObjectDataSource marcação declarativa especifica apenas três parâmetros de entrada (consulte a Figura 5). Da mesma forma, se houver alguma combinação de não somente leitura produto campos em GridView que não corresponde aos parâmetros de entrada para um `UpdateProduct` sobrecarga, será gerada uma exceção ao tentar atualizar.


[![O GridView será adicionar parâmetros a coleção de UpdateParameters do ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Figura 5**: O GridView será adicionar parâmetros o ObjectDataSource `UpdateParameters` coleção ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))


Para garantir que o ObjectDataSource invoca o `UpdateProduct` sobrecarga que usa apenas o produto nome, preço e ID, é preciso restringir GridView a ter campos editáveis para apenas o `ProductName` e `UnitPrice`. Isso pode ser feito com a remoção de BoundFields e CheckBoxFields, definindo os outros campos `ReadOnly` propriedade `true`, ou por uma combinação dos dois. Para este tutorial vamos simplesmente remova todos os campos de GridView, exceto o `ProductName` e `UnitPrice` BoundFields, após o qual declarativo do GridView será semelhante a:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

Embora o `UpdateProduct` sobrecarga espera três parâmetros de entrada, somente temos duas BoundFields em nosso GridView. Isso ocorre porque o `productID` parâmetro de entrada é um valor de chave primária e passado por meio do valor da `DataKeyNames` propriedade da linha editada.

Nosso GridView, juntamente com o `UpdateProduct` sobrecarga, permite que o usuário edite apenas o nome e o preço de um produto sem perder qualquer um dos campos do produto.


[![A Interface permite a edição apenas o produto nome e preços](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Figura 6**: O permite que a Interface edição apenas o produto nome e preço ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))


> [!NOTE]
> Como discutido no tutorial anterior, é extremamente importante que o GridView s estado de exibição ser habilitado (o comportamento padrão). Se você definir o GridView s `EnableViewState` propriedade `false`, você corre o risco de ter acidentalmente excluir ou editar registros de usuários simultâneos. Consulte [Aviso: problema de simultaneidade com ASP.NET 2.0 GridViews/DetailsView/FormViews que suporte a edição e/ou excluindo e cujo estado de exibição é desativado](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) para obter mais informações.


## <a name="improving-theunitpriceformatting"></a>Melhorando o`UnitPrice`formatação

Embora o exemplo de GridView mostrado na Figura 6 funciona, o `UnitPrice` campo não está formatado, resultando em uma exibição de preços que não tem qualquer moeda símbolos e tem quatro casas decimais. Para aplicar uma formatação para as linhas não editável de moeda, basta definir o `UnitPrice` do BoundField `DataFormatString` propriedade `{0:c}` e sua `HtmlEncode` propriedade `false`.


[![Definir o UnitPrice DataFormatString e propriedades HtmlEncode adequadamente](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Figura 7**: definir o `UnitPrice`do `DataFormatString` e `HtmlEncode` propriedades adequadamente ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))


Com essa alteração, as linhas não editável formatar o preço como uma moeda; No entanto, a linha editada, ainda exibe o valor sem o símbolo de moeda e com quatro casas decimais.


[![Linhas não editável são agora formatados como valores de moeda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Figura 8**: linhas não editável são agora são formatados como valores de moeda ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))


As instruções de formatação especificadas no `DataFormatString` propriedade pode ser aplicada para a interface de edição, definindo o BoundField `ApplyFormatInEditMode` propriedade `true` (o padrão é `false`). Reserve um tempo para definir essa propriedade como `true`.


[![Definir propriedade de ApplyFormatInEditMode de UnitPrice BoundField como true](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Figura 9**: definir o `UnitPrice` do BoundField `ApplyFormatInEditMode` propriedade `true` ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))


Com essa alteração, o valor da `UnitPrice` exibido no editado linha também é formatada como uma moeda.


[![UnitPrice valor da linha editada é formatado agora como uma moeda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Figura 10**: O editado linha `UnitPrice` valor é formatado agora como uma moeda ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))


No entanto, atualizar um produto com o símbolo de moeda na caixa de texto, como $19.00 lança um `FormatException`. Quando tenta atribuir os valores fornecidos pelo usuário para o ObjectDataSource GridView `UpdateParameters` coleção não é possível converter o `UnitPrice` cadeia de caracteres "$19.00" para o `decimal` exigido pelo parâmetro (consulte a Figura 11). Para corrigir isso podemos criar um manipulador de eventos do GridView `RowUpdating` eventos e colocá-lo a analisar o usuário forneceu `UnitPrice` como um formato de moeda `decimal`.

O GridView `RowUpdating` evento aceita como seu segundo parâmetro, um objeto do tipo [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), que inclui um `NewValues` dicionário como uma de suas propriedades que contém os valores fornecidos pelo usuário prontos para ser atribuído a ObjectDataSource `UpdateParameters` coleção. Podemos pode substituir a existente `UnitPrice` valor o `NewValues` coleção com um valor decimal analisado usando o formato de moeda com as seguintes linhas de código no `RowUpdating` manipulador de eventos:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Se o usuário forneceu um `UnitPrice` valor (como "$19.00"), esse valor é substituído com o valor decimal calculado pelo [Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), ao analisar o valor como uma moeda. Isso irá analisar corretamente o decimal no caso de qualquer símbolos de moeda, vírgulas, pontos decimais e assim por diante e usa o [enumeração NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) no [System. Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) namespace.

A Figura 11 mostra ambos o problema causado pelo símbolos de moeda em que o usuário forneceu `UnitPrice`, juntamente com como o GridView `RowUpdating` manipulador de eventos pode ser usado para analisar corretamente essa entrada.


[![UnitPrice valor da linha editada é formatado agora como uma moeda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Figura 11**: O editado linha `UnitPrice` valor é formatado agora como uma moeda ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Etapa 2: proibindo`NULL UnitPrices`

Enquanto o banco de dados está configurado para permitir `NULL` valores no `Products` da tabela `UnitPrice` coluna, podemos desejar impedir que os usuários visitar essa página específica da especificação de um `NULL` `UnitPrice` valor. Ou seja, se um usuário não insere uma `UnitPrice` valor ao editar uma linha de produto, em vez de salvar os resultados para o banco de dados que você deseja exibir uma mensagem informando ao usuário que, por meio desta página, todos os produtos editados devem ter um preço especificado.

O `GridViewUpdateEventArgs` objeto passado para o GridView `RowUpdating` manipulador de eventos contém um `Cancel` propriedade que, se definido como `true`, encerra o processo de atualização. Vamos estender o `RowUpdating` manipulador de eventos para definir `e.Cancel` para `true` e exibirá uma mensagem explicando por que se o `UnitPrice` valor o `NewValues` coleção é `null`.

Comece adicionando um controle da Web de rótulo para a página chamada `MustProvideUnitPriceMessage`. Este controle de rótulo será exibido se o usuário não especificar um `UnitPrice` valor ao atualizar um produto. Defina o rótulo `Text` propriedade como "Você deve fornecer um preço para o produto". Também criei uma nova classe CSS em `Styles.css` chamado `Warning` com a seguinte definição:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Finalmente, defina o rótulo `CssClass` propriedade `Warning`. Neste ponto o Designer deve mostrar a mensagem de aviso em um vermelho, negrito, itálico, tamanho da fonte extra grande acima GridView, conforme mostrado na Figura 12.


[![Foi adicionado um rótulo acima GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Figura 12**: um rótulo tem sido adicionados acima a GridView ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))


Por padrão, este rótulo deve ficar oculto, portanto, definir seu `Visible` propriedade `false` no `Page_Load` manipulador de eventos:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Se o usuário tenta atualizar um produto sem especificar o `UnitPrice`, você deseja cancelar a atualização e exibir o rótulo de aviso. Aumentar o GridView `RowUpdating` manipulador de eventos da seguinte maneira:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Se um usuário tenta salvar um produto sem especificar um preço, a atualização será cancelada e será exibida uma mensagem útil. Enquanto o banco de dados (e a lógica de negócios) permite `NULL` `UnitPrice` s, essa página específica do ASP.NET não.


[![Um usuário não é possível deixar em branco de UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Figura 13**: um usuário não pode sair `UnitPrice` em branco ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))


Até agora, vimos como usar o GridView `RowUpdating` eventos programaticamente alterar os valores de parâmetro atribuídos a ObjectDataSource `UpdateParameters` coleção, bem como para cancelar a atualização processar completamente. Esses conceitos são transferidas para os controles de DetailsView e FormView e também se aplicam a inserção e exclusão.

Essas tarefas também podem ser feitas no nível de ObjectDataSource por meio de manipuladores de eventos para sua `Inserting`, `Updating`, e `Deleting` eventos. Esses eventos acionam antes do método associado do objeto subjacente é chamado e fornecem uma oportunidade de última chance para modificar a coleção de parâmetros de entrada ou cancelar a operação imediatamente. Manipuladores de eventos para esses três eventos são passados a um objeto do tipo [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) que tem duas propriedades de interesse:

- [Cancelar](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), que, se definido como `true`, cancela a operação que está sendo executada
- [ParâmetrosDeEntrada](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), que é a coleção de `InsertParameters`, `UpdateParameters`, ou `DeleteParameters`, dependendo se o manipulador de eventos é para o `Inserting`, `Updating`, ou `Deleting` evento

Para ilustrar a trabalhar com os valores de parâmetro no nível de ObjectDataSource, vamos incluir um DetailsView em nossa página que permite que os usuários adicionar um novo produto. Este DetailsView será usado para fornecer uma interface para adicionar rapidamente um novo produto no banco de dados. Para manter uma interface de usuário consistente quando adicionar um novo produto vamos permitir que o usuário insira apenas valores para o `ProductName` e `UnitPrice` campos. Por padrão, os valores que não são fornecidos na interface de inserção de DetailsView serão definidos um `NULL` valor de banco de dados. No entanto, podemos usar o ObjectDataSource `Inserting` evento inserir valores padrão diferentes, como você verá em breve.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Etapa 3: Fornecer uma Interface para adicionar novos produtos

Arraste um DetailsView da caixa de ferramentas para o Designer acima GridView, limpe seu `Height` e `Width` propriedades e vincular a ObjectDataSource já está presente na página. Isso adicionará um BoundField ou CheckBoxField para cada um dos campos do produto. Como queremos usar este DetailsView para adicionar novos produtos, é necessário marcar a opção Permitir inserção de marca inteligente; No entanto, há essa opção não porque o ObjectDataSource `Insert()` método não está mapeado para um método em de `ProductsBLL` classe (Lembre-se de que podemos definir esse mapeamento (nenhum) ao configurar a fonte de dados Consulte a Figura 3).

Para configurar o ObjectDataSource, selecione o link de configurar fonte de dados de marca inteligente, iniciar o assistente. A primeira tela permite que você altere o objeto subjacente que ObjectDataSource está associado a; Deixe-a configurada para `ProductsBLL`. A próxima tela lista os mapeamentos entre os métodos do ObjectDataSource para o objeto subjacente. Mesmo que explicitamente indicado que o `Insert()` e `Delete()` métodos não devem ser mapeados para todos os métodos, se você for para inserir e excluir guias você verá que um mapeamento está lá. Isso ocorre porque o `ProductsBLL`do `AddProduct` e `DeleteProduct` métodos usam o `DataObjectMethodAttribute` atributo para indicar que eles são os métodos padrão para `Insert()` e `Delete()`, respectivamente. Portanto, o Assistente de ObjectDataSource seleciona essa cada vez que você executar o assistente, a menos que haja algum outro valor explicitamente especificado.

Deixe o `Insert()` método apontando para o `AddProduct` método, mas novamente definido lista suspensa de exclusão da guia como (nenhum).


[![Defina lista suspensa lista a guia Inserir ao método AddProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Figura 14**: altere na lista suspensa da guia Inserir para o `AddProduct` método ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))


[![Definição suspensa lista da guia DELETE como (nenhum)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Figura 15**: definição suspensa lista da guia excluir como (nenhum) ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))


Depois de fazer essas alterações, a sintaxe declarativa do ObjectDataSource será expandida para incluir um `InsertParameters` coleção, conforme mostrado abaixo:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

Executar novamente o assistente adicionado novamente a `OldValuesParameterFormatString` propriedade. Reserve um tempo para limpar esta propriedade definindo-a como o valor padrão (`{0}`) ou removê-lo totalmente da sintaxe declarativa.

Com o ObjectDataSource fornecendo recursos de inserção, marca inteligente de DetailsView agora incluem a caixa de seleção Permitir inserção; Volte para o Designer e marque esta opção. Em seguida, diminuir DetailsView para que ele tem apenas dois BoundFields - `ProductName` e `UnitPrice` - e o CommandField. Neste ponto, a sintaxe declarativa de DetailsView deve parecer com:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

A Figura 16 mostra essa página quando exibido neste momento por meio de um navegador. Como você pode ver, DetailsView lista o nome e o preço do produto (Chai) primeiro. No entanto, o que queremos, é uma interface de inserção que fornece um meio para o usuário adicionar rapidamente um novo produto no banco de dados.


[![O DetailsView é renderizada atualmente no modo somente leitura](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Figura 16**: O DetailsView é renderizada atualmente no modo somente leitura ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))


Para mostrar o DetailsView no modo de inserção, precisamos definir o `DefaultMode` propriedade `Inserting`. Isso renderiza DetailsView no modo de inserção quando visitado primeiro e mantém-lo depois de inserir um novo registro. Como mostra a Figura 17, tal um DetailsView fornece uma interface rápida para adicionar um novo registro.


[![O DetailsView fornece uma Interface para adicionar rapidamente um novo produto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Figura 17**: DetailsView fornece uma Interface para adicionar rapidamente um novo produto ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))


Quando o usuário insere um nome de produto e o preço (como "Água Acme" e 1.99, como na Figura 17) e clicar em Inserir, um postback tem lugar e inicia o fluxo de trabalho inserindo, culminando em um novo registro de produto que está sendo adicionado ao banco de dados. O DetailsView mantém sua interface de inserção e GridView é automaticamente vinculada outra vez para a fonte de dados para incluir o novo produto, conforme mostrado na Figura 18.


![O produto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Figura 18**: O produto "Água Acme" foi adicionado ao banco de dados


Embora o GridView na Figura 18 não mostre, os campos de produto ausente na interface do DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`, e assim por diante são atribuídos `NULL` valores do banco de dados. Você pode ver isso executando as seguintes etapas:

1. Vá para o Gerenciador de servidores no Visual Studio
2. Expandindo o `NORTHWND.MDF` nó banco de dados
3. Clique com botão direito no `Products` nó da tabela de banco de dados
4. Selecione Mostrar dados de tabela

Isso listará todos os registros no `Products` tabela. Como a Figura 19 mostra, todas as colunas do nosso novo produto diferente de `ProductID`, `ProductName`, e `UnitPrice` ter `NULL` valores.


[![O produto campos não fornecido em DetailsView são atribuídos valores nulos](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Figura 19**: O produto campos não fornecido em DetailsView recebem `NULL` valores ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))


Podemos desejar fornecer um valor padrão diferente de `NULL` para um ou mais dos seguintes valores de coluna, ou porque `NULL` não é a melhor opção padrão ou porque a coluna de banco de dados em si não permite `NULL` s. Para fazer isso, pode definir os valores dos parâmetros de DetailsView `InputParameters` coleção. Essa atribuição pode ser feita ou no evento manipulador para o DetailsView `ItemInserting` evento ou o ObjectDataSource `Inserting` eventos. Como já encontramos no usando os eventos de pré e pós-níveis os dados da Web controlar o nível, vamos explorar usando eventos do ObjectDataSource neste momento.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Etapa 4: Atribuir valores para o`CategoryID`e`SupplierID`parâmetros

Para este tutorial vamos imaginar que nosso aplicativo ao adicionar um novo produto por meio dessa interface deve ser atribuído um `CategoryID` e `SupplierID` valor de 1. Como mencionado anteriormente, o ObjectDataSource tem um par de pré e pós-níveis de eventos que disparem durante o processo de modificação de dados. Quando seu `Insert()` método é invocado, ObjectDataSource primeiro dispara seu `Inserting` evento, em seguida, chama o método que seu `Insert()` método foi mapeado para e, finalmente, gera o `Inserted` evento. O `Inserting` manipulador de eventos nos dá uma última oportunidade de ajustar os parâmetros de entrada ou cancelar a operação imediatamente.

> [!NOTE]
> Em um aplicativo do mundo real provavelmente seria recomendável para permitir que o usuário especificar o fornecedor e a categoria ou escolheria esse valor para eles com base em alguns critérios ou business lógica (em vez de cegamente selecionando uma ID de 1). Independentemente, o exemplo ilustra como definir programaticamente o valor de um parâmetro de entrada de evento de pré-nível do ObjectDataSource.


Reserve um tempo para criar um manipulador de eventos para o ObjectDataSource `Inserting` eventos. Observe que o segundo parâmetro de entrada do manipulador de eventos é um objeto do tipo `ObjectDataSourceMethodEventArgs`, que tem uma propriedade para acessar a coleção de parâmetros (`InputParameters`) e uma propriedade para cancelar a operação (`Cancel`).


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

Neste ponto, o `InputParameters` propriedade contém o ObjectDataSource `InsertParameters` coleção com os valores atribuídos de DetailsView. Para alterar o valor de um desses parâmetros, basta usar: `e.InputParameters["paramName"] = value`. Portanto, para definir o `CategoryID` e `SupplierID` para valores de 1, ajustar o `Inserting` manipulador de eventos para se parecer com o seguinte:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Esse tempo ao adicionar um novo produto (como Soda Acme), o `CategoryID` e `SupplierID` colunas do novo produto são definidas como 1 (consulte a Figura 20).


[![Novos produtos agora tem CategoryID e SupplierID valores definidos como 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Figura 20**: novos produtos agora têm seus `CategoryID` e `SupplierID` valores definidos como 1 ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))


## <a name="summary"></a>Resumo

Durante a edição, inserção e exclusão de processo, o controle de dados Web e ObjectDataSource continuar por meio de um número de eventos de pré e pós-níveis. Neste tutorial examinou os eventos de níveis previamente e viu como usá-los para personalizar os parâmetros de entrada ou cancelar a operação de modificação de dados completamente ambos a partir do controle da Web de dados e eventos do ObjectDataSource. O seguinte tutorial, examinaremos a criando e usando manipuladores de eventos para os eventos pós-níveis.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Jackie Goor e Liz Shulok. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](an-overview-of-inserting-updating-and-deleting-data-cs.md)
[Próximo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
