---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: Examinando os eventos associados com a inserção, atualização e exclusão (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, que vamos examinar usando os eventos que ocorrem antes, durante e após uma inserção, atualização ou operação de exclusão de um controle da Web de dados do ASP.NET. W...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: cec25809038cbe6a114b0e36ef7265d84bbe9b92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363294"
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Examinando os eventos associados à inserção, atualização e exclusão (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) ou [baixar PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> Neste tutorial, que vamos examinar usando os eventos que ocorrem antes, durante e após uma inserção, atualização ou operação de exclusão de um controle da Web de dados do ASP.NET. Também veremos como personalizar o interface de edição para atualizar apenas um subconjunto dos campos do produto.


## <a name="introduction"></a>Introdução

Ao usar a inserção interna, editando ou excluindo os recursos dos controles GridView, DetailsView ou FormView, uma variedade de etapas ocorrer quando o usuário final conclui o processo de adicionar um novo registro ou atualizando ou excluindo um registro existente. Como discutimos na [tutorial anterior](an-overview-of-inserting-updating-and-deleting-data-cs.md), quando uma linha é editada no GridView no botão Editar é substituído por botões Cancelar e de atualização e a ativar BoundFields nas caixas de texto. Depois que o usuário final atualiza os dados e clica em atualizar, as etapas a seguir são executadas em um postback:

1. O GridView preenche seu ObjectDataSource `UpdateParameters` com campos de identificação exclusivo do Registro editado (via o `DataKeyNames` propriedade) junto com os valores inseridos pelo usuário
2. O GridView invoca seu ObjectDataSource `Update()` método, que por sua vez chama o método apropriado no objeto subjacente (`ProductsDAL.UpdateProduct`, em nosso tutorial anterior)
3. Os dados subjacentes, que agora inclui as alterações atualizadas, são reassociados ao GridView

Durante esta sequência de etapas, um número de eventos acionados, possibilitando a criar manipuladores de eventos para adicionar lógica personalizada quando for necessário. Por exemplo, antes do etapa 1, o GridView `RowUpdating` evento é acionado. Neste ponto, podemos pode, cancelar a solicitação de atualização se houver algum erro de validação. Quando o `Update()` método é invocado, o ObjectDataSource `Updating` evento é acionado, fornecendo uma oportunidade de adicionar ou personalizar os valores de qualquer um do `UpdateParameters`. Depois que o ObjectDataSource a base do método do objeto completou a execução, o ObjectDataSource `Updated` é gerado. Um manipulador de eventos para o `Updated` evento pode inspecionar os detalhes sobre a operação de atualização, como quantas linhas foram afetadas e ou não ocorreu uma exceção. Por fim, após a do etapa 2, o GridView `RowUpdated` evento é acionado; um manipulador de eventos para esse evento pode examinar informações adicionais sobre a operação de atualização apenas executadas.

Figura 1 ilustra essa série de etapas e eventos ao atualizar um GridView. O padrão de evento na Figura 1 não é exclusivo para a atualização com um GridView. Inserindo, atualizando ou excluindo dados de GridView, DetailsView ou FormView precipitates a mesma sequência de eventos de pré e pós-níveis para o controle de Web de dados e o ObjectDataSource.


[![Uma série de pré e acionar pós-eventos ao atualizar dados em um GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Figura 1**: uma série de pré e pós-eventos incêndio quando atualizando dados em um GridView ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))


Neste tutorial, que vamos examinar usando esses eventos para estender a inserção interna, atualizar e excluir recursos de dados ASP.NET controles da Web. Também veremos como personalizar o interface de edição para atualizar apenas um subconjunto dos campos do produto.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Etapa 1: Atualizar um produto`ProductName`e`UnitPrice`campos

As interfaces de edição do tutorial anterior *todos os* campos de produto que não eram somente leitura tinha que ser incluído. Se estivéssemos remover um campo de GridView - digamos `QuantityPerUnit` - ao atualizar os dados de controle de Web de dados não definiria o ObjectDataSource `QuantityPerUnit` `UpdateParameters` valor. O ObjectDataSource, em seguida, transmita um `null` de valor na `UpdateProduct` método de camada de lógica de negócios (BLL), o que mudaria o registro de banco de dados editado `QuantityPerUnit` coluna para um `NULL` valor. Da mesma forma, se um campo obrigatório, tais como `ProductName`, é removido da interface de edição, a atualização falhará com um "*'ProductName' da coluna não permite nulls*" exceção. O motivo para esse comportamento foi porque o ObjectDataSource foi configurado para chamar o `ProductsBLL` da classe `UpdateProduct` método, que espera um parâmetro de entrada para cada um dos campos do produto. Portanto, do ObjectDataSource `UpdateParameters` coleção continha um parâmetro para cada método de parâmetros de entrada.

Se queremos fornecer um controle de Web que permite ao usuário final atualizar apenas um subconjunto dos campos de dados, então precisamos por definir programaticamente o ausente `UpdateParameters` valores no ObjectDataSource `Updating` manipulador de eventos ou criar e chamar um método BLL que espera apenas um subconjunto dos campos. Vamos explorar essa última abordagem.

Especificamente, vamos criar uma página que exibe apenas o `ProductName` e `UnitPrice` campos em um GridView editável. Interface de edição do GridView esse só permitirá que o usuário atualizar os dois campos exibidos `ProductName` e `UnitPrice`. Como essa interface de edição fornece apenas um subconjunto dos campos de um produto, é necessário criar um ObjectDataSource que usa a BLL existente `UpdateProduct` método e tem os valores de campo ausente do produto definida por meio de programação em seu `Updating` evento manipulador, ou podemos precisa criar um novo método BLL que espera apenas o subconjunto de campos definidos na GridView. Para este tutorial, vamos usar a última opção e criar uma sobrecarga da `UpdateProduct` método, que tem apenas três parâmetros de entrada: `productName`, `unitPrice`, e `productID`:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Como o original `UpdateProduct` essa sobrecarga de método, começa verificando se há um produto no banco de dados com o `ProductID`. Se não, ele retorna `false`, indicando que a solicitação para atualizar as informações do produto falhou. Caso contrário, ele atualiza o registro de produto existente `ProductName` e `UnitPrice` campos adequadamente e confirma a atualização chamando o TableAdpater `Update()` método, passando o `ProductsRow` instância.

Com esse acréscimo à nossa `ProductsBLL` classe, estamos prontos para criar a interface simplificada do GridView. Abra o `DataModificationEvents.aspx` no `EditInsertDelete` pasta e adicione um controle GridView à página. Criar um novo ObjectDataSource e configurá-lo para usar o `ProductsBLL` classe com seus `Select()` mapear de método para `GetProducts` e sua `Update()` mapear de método para o `UpdateProduct` sobrecarga que recebe apenas a `productName`, `unitPrice`, e `productID` parâmetros de entrada. A Figura 2 mostra o assistente Criar fonte de dados ao mapear o ObjectDataSource `Update()` método para o `ProductsBLL` da nova classe `UpdateProduct` sobrecarga de método.


[![Mapear o método de Update () do ObjectDataSource para a nova sobrecarga UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Figura 2**: mapeie o ObjectDataSource `Update()` método para o novo `UpdateProduct` sobrecarregar ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))


Uma vez que o nosso exemplo inicialmente apenas precisará da capacidade para editar dados, mas não para inserir ou excluir registros, dedique uns momentos para indicar explicitamente que o ObjectDataSource `Insert()` e `Delete()` métodos não devem ser mapeados para qualquer um do `ProductsBLL` métodos da classe indo para as guias INSERT e DELETE e escolhendo (nenhum) na lista suspensa.


[![Escolha (nenhum) na lista suspensa para as guias DELETE e INSERT](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Figura 3**: escolha (nenhum) da lista suspensa para a inserção e excluir guias ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))


Depois de concluir este assistente, marque a caixa de seleção Habilitar edição na marca inteligente do GridView.

Com a conclusão do assistente Criar fonte de dados e a associação que para o GridView, o Visual Studio criou a sintaxe declarativa para ambos os controles. Vá para a exibição da fonte para inspecionar a marcação declarativa do ObjectDataSource, que é mostrado abaixo:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Como não há nenhum mapeamento para o ObjectDataSource `Insert()` e `Delete()` métodos, há nenhuma `InsertParameters` ou `DeleteParameters` seções. Além disso, desde o `Update()` método é mapeado para o `UpdateProduct` sobrecarga de método que só aceita três parâmetros de entrada, o `UpdateParameters` seção tem apenas três `Parameter` instâncias.

Observe que o ObjectDataSource `OldValuesParameterFormatString` estiver definida como `original_{0}`. Essa propriedade é definida automaticamente pelo Visual Studio ao usar o Assistente Configurar fonte de dados. No entanto, uma vez que nossos métodos BLL não espere que o original `ProductID` valor a ser passada, remova a atribuição de propriedade completamente a sintaxe declarativa do ObjectDataSource.

> [!NOTE]
> Se você simplesmente limpar o `OldValuesParameterFormatString` valor da propriedade na janela Propriedades, na exibição de Design, a propriedade ainda existirá na sintaxe declarativa, mas será definido como uma cadeia de caracteres vazia. Remova a propriedade completamente da sintaxe declarativa ou, na janela Propriedades, defina o valor como o padrão, `{0}`.


Enquanto o ObjectDataSource tem apenas `UpdateParameters` para nome, preço e ID do produto, Visual Studio adicionou um BoundField ou CheckBoxField no GridView para cada um dos campos do produto.


[![GridView contém um BoundField ou CheckBoxField para cada um dos campos do produto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Figura 4**: O GridView contém um BoundField ou CheckBoxField para cada um dos campos do produto ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))


Quando o usuário final edita um produto e clica no botão de atualização, o GridView enumera os campos que não eram somente leitura. Ele define o valor do parâmetro correspondente no ObjectDataSource `UpdateParameters` coleção para o valor inserido pelo usuário. Se não houver um parâmetro correspondente, o GridView adiciona um à coleção. Portanto, se nosso GridView contiver BoundFields e CheckBoxFields para todos os campos do produto, o ObjectDataSource termina invocando o `UpdateProduct` sobrecarga que utiliza em todos esses parâmetros, apesar do fato de que o ObjectDataSource marcação declarativa especifica apenas três parâmetros de entrada (consulte a Figura 5). Da mesma forma, se houver alguma combinação de não-somente leitura produto campos no GridView que não corresponde aos parâmetros de entrada para um `UpdateProduct` de sobrecarga, uma exceção será gerada ao tentar atualizar.


[![O GridView será adicionar parâmetros a coleção de UpdateParameters do ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Figura 5**: O GridView será adicionar parâmetros a serem o ObjectDataSource `UpdateParameters` coleção ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))


Para garantir que o ObjectDataSource invoca o `UpdateProduct` sobrecarga que usa apenas o produto nome, preço e ID, é preciso restringir GridView a ter campos editáveis para apenas o `ProductName` e `UnitPrice`. Isso pode ser feito removendo a BoundFields e CheckBoxFields, definindo os outros campos de `ReadOnly` propriedade para `true`, ou por uma combinação dos dois. Para este tutorial vamos simplesmente remover todos os campos de GridView, exceto o `ProductName` e `UnitPrice` BoundFields, após o qual marcação declarativa do GridView terá a seguinte aparência:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

Embora o `UpdateProduct` sobrecarga espera três parâmetros de entrada, temos apenas dois BoundFields em nossa GridView. Isso ocorre porque o `productID` parâmetro de entrada é um valor de chave primária e passado por meio do valor da `DataKeyNames` propriedade da linha editada.

Nosso GridView, juntamente com o `UpdateProduct` sobrecarregar, permite que o usuário edite apenas o nome e o preço de um produto sem perder qualquer um dos campos do produto.


[![A Interface permite que apenas o produto nome e preço de edição](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Figura 6**: permite a Interface que apenas o produto nome e preço de edição ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))


> [!NOTE]
> Conforme discutido no tutorial anterior, é extremamente importante que o GridView, o estado de exibição s ser habilitada (o comportamento padrão). Se você definir o s GridView `EnableViewState` propriedade para `false`, você corre o risco de ter acidentalmente excluir ou editar registros de usuários simultâneos. Ver [Aviso: problema de simultaneidade com o ASP.NET 2.0 GridViews/DetailsView/FormViews esse suporte de edição e/ou exclusão e cujo estado de exibição está desabilitado](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) para obter mais informações.


## <a name="improving-theunitpriceformatting"></a>Melhorando o`UnitPrice`formatação

Embora o exemplo de GridView, mostrado na Figura 6 funciona, o `UnitPrice` campo não está formatado, resultando em uma exibição de preço que não tem qualquer moeda símbolos e tem quatro casas decimais. Para aplicar uma formatação para as linhas não editável de moedas, basta definir a `UnitPrice` do BoundField `DataFormatString` propriedade a ser `{0:c}` e sua `HtmlEncode` propriedade `false`.


[![Definir o UnitPrice DataFormatString e propriedades HtmlEncode adequadamente](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Figura 7**: defina o `UnitPrice`do `DataFormatString` e `HtmlEncode` propriedades adequadamente ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))


Com essa alteração, as linhas não editável formatar o preço como uma moeda; No entanto, a linha editada, ainda exibe o valor sem o símbolo de moeda e com quatro casas decimais.


[![Linhas não editável são agora formatados como valores de moeda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Figura 8**: as linhas não editável são agora são formatados como valores de moeda ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))


As instruções de formatação especificadas na `DataFormatString` propriedade pode ser aplicada à interface de edição, definindo o BoundField `ApplyFormatInEditMode` propriedade `true` (o padrão é `false`). Reserve um tempo para definir essa propriedade como `true`.


[![Definir propriedade de ApplyFormatInEditMode de UnitPrice BoundField como true](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Figura 9**: defina o `UnitPrice` do BoundField `ApplyFormatInEditMode` propriedade `true` ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))


Com essa alteração, o valor da `UnitPrice` exibido no editado linha também é formatada como uma moeda.


[![Valor de UnitPrice da linha editada é agora formatado como uma moeda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Figura 10**: O editado dessa linha `UnitPrice` valor é agora são formatadas como uma moeda ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))


No entanto, atualizar um produto com o símbolo de moeda na caixa de texto, como $19.00 lança um `FormatException`. Quando tenta atribuir os valores fornecidos pelo usuário para o ObjectDataSource GridView `UpdateParameters` coleção não é possível converter o `UnitPrice` "$19.00" da cadeia de caracteres para o `decimal` exigido pelo parâmetro (veja a Figura 11). Para corrigir isso podemos criar um manipulador de eventos para o GridView `RowUpdating` evento e fazê-lo a analisar o usuário forneceu `UnitPrice` como um formato de moeda `decimal`.

O GridView `RowUpdating` evento aceita como seu segundo parâmetro um objeto do tipo [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), que inclui um `NewValues` dicionário como uma de suas propriedades que contém os valores prontos para ser fornecido pelo usuário atribuído a ObjectDataSource `UpdateParameters` coleção. Podemos pode substituir o existente `UnitPrice` o valor a `NewValues` coleção com um valor decimal analisado usando o formato de moeda com as seguintes linhas de código no `RowUpdating` manipulador de eventos:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Se o usuário tiver fornecido um `UnitPrice` valor (por exemplo, "$19.00"), esse valor é substituído com o valor decimal computado pelo [Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), análise do valor como uma moeda. Isso irá analisar corretamente o decimal no caso de quaisquer símbolos de moeda, vírgulas, pontos decimais e assim por diante e usa o [enumeração NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) na [System. Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) namespace.

A Figura 11 mostra o problema causado por símbolos de moeda em que o usuário forneceu `UnitPrice`, juntamente com como o GridView `RowUpdating` manipulador de eventos pode ser utilizado para analisar corretamente essa entrada.


[![Valor de UnitPrice da linha editada é agora formatado como uma moeda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Figura 11**: O editado dessa linha `UnitPrice` valor é agora são formatadas como uma moeda ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Etapa 2: proibindo`NULL UnitPrices`

Enquanto o banco de dados está configurado para permitir `NULL` os valores em de `Products` da tabela `UnitPrice` coluna, podemos querer impedir que os usuários a visitar essa página em particular da especificação de uma `NULL` `UnitPrice` valor. Ou seja, se um usuário não insere uma `UnitPrice` valor ao editar uma linha de produto, em vez de salvar os resultados no banco de dados que desejamos exibir uma mensagem informando ao usuário que, por meio desta página, todos os produtos editados devem ter um preço especificado.

O `GridViewUpdateEventArgs` objeto passado para o GridView `RowUpdating` manipulador de eventos contém uma `Cancel` propriedade que, se definido como `true`, encerra o processo de atualização. Vamos estender o `RowUpdating` manipulador de eventos para definir `e.Cancel` à `true` e exibir uma mensagem explicando o motivo se o `UnitPrice` valor na `NewValues` coleção é `null`.

Comece adicionando um controle da Web do rótulo para a página chamada `MustProvideUnitPriceMessage`. Esse controle de rótulo será exibido se o usuário não especificar um `UnitPrice` ao atualizar um produto de valor. Defina o rótulo `Text` propriedade como "Você deve fornecer um preço para o produto". Também criei uma nova classe CSS na `Styles.css` chamado `Warning` com a seguinte definição:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Por fim, defina o rótulo `CssClass` propriedade para `Warning`. Neste ponto o Designer deve mostrar a mensagem de aviso em um vermelho, negrito, itálico, tamanho da fonte extra grande acima GridView, conforme mostrado na Figura 12.


[![Foi adicionado um rótulo acima GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Figura 12**: um rótulo tem sido adicionadas acima. o controle GridView ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))


Por padrão, este rótulo deve ser ocultado, portanto, defina suas `Visible` propriedade para `false` no `Page_Load` manipulador de eventos:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Se o usuário tenta atualizar um produto sem especificar o `UnitPrice`, queremos cancelar a atualização e exibir o rótulo de aviso. Aumentar o GridView `RowUpdating` manipulador de eventos da seguinte maneira:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Se um usuário tenta salvar um produto sem especificar um preço, a atualização será cancelada e será exibida uma mensagem útil. Enquanto o banco de dados (e a lógica de negócios) permite `NULL` `UnitPrice` s, essa página ASP.NET em particular não faz isso.


[![Um usuário não é possível deixar em branco de UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Figura 13**: um usuário não pode deixar `UnitPrice` em branco ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))


Até agora, vimos como usar o GridView `RowUpdating` evento para alterar programaticamente os valores de parâmetro com o ObjectDataSource `UpdateParameters` coleção, bem como para cancelar a atualização processar completamente. Esses conceitos são transferidas para os controles DetailsView e FormView e também se aplicam a inserção e exclusão.

Essas tarefas também podem ser feitas no nível do ObjectDataSource por meio de manipuladores de eventos para seus `Inserting`, `Updating`, e `Deleting` eventos. Esses eventos acionam antes que o método associado do objeto subjacente é invocado e oferecem uma oportunidade de última chance para modificar a coleção de parâmetros de entrada ou cancelar a operação imediatamente. Os manipuladores de eventos para esses três eventos são passados a um objeto do tipo [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) que tem duas propriedades de interesse:

- [Cancelar](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), que, se definido como `true`, cancela a operação que está sendo executada
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), que é a coleção de `InsertParameters`, `UpdateParameters`, ou `DeleteParameters`, dependendo se o manipulador de eventos é para o `Inserting`, `Updating`, ou `Deleting` evento

Para ilustrar a trabalhar com os valores de parâmetro no nível do ObjectDataSource, vamos incluir um DetailsView em nossa página que permite aos usuários adicionar um novo produto. Este DetailsView será usado para fornecer uma interface para adicionar rapidamente um novo produto no banco de dados. Para manter uma interface do usuário consistente quando adicionar um novo produto vamos permitir que o usuário apenas inserir valores para o `ProductName` e `UnitPrice` campos. Por padrão, os valores que não são fornecidos na interface de inserção de DetailsView serão definidos um `NULL` valor do banco de dados. No entanto, podemos usar o ObjectDataSource `Inserting` evento injetar os valores padrão diferentes, como veremos daqui a pouco.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Etapa 3: Fornecer uma Interface para adicionar novos produtos

Arraste um DetailsView da caixa de ferramentas para o Designer acima GridView, limpe seu `Height` e `Width` propriedades e bind-o para o ObjectDataSource já estão presentes na página. Isso adicionará um BoundField ou CheckBoxField para cada um dos campos do produto. Como queremos usar essa DetailsView para adicionar novos produtos, é necessário marcar a opção de habilitar a inserção da marca inteligente; No entanto, há essa opção não porque o ObjectDataSource `Insert()` método não está mapeado para um método no `ProductsBLL` classe (Lembre-se de que definimos esse mapeamento (nenhum) quando a configuração da fonte de dados Consulte a Figura 3).

Para configurar o ObjectDataSource, selecione o link configurar fonte de dados na sua marca inteligente, iniciando o assistente. A primeira tela permite que você altere o objeto subjacente que o ObjectDataSource está associado Deixe-a configurada para `ProductsBLL`. A próxima tela lista os mapeamentos entre os métodos do ObjectDataSource para o objeto subjacente. Mesmo que explicitamente indicado que o `Insert()` e `Delete()` métodos não devem ser mapeados para todos os métodos, se você for para as guias INSERT e DELETE você verá que um mapeamento está lá. Isso ocorre porque o `ProductsBLL`do `AddProduct` e `DeleteProduct` métodos usam o `DataObjectMethodAttribute` atributo para indicar que eles são os métodos padrão para `Insert()` e `Delete()`, respectivamente. Portanto, o assistente ObjectDataSource seleciona essa cada vez que você executar o assistente, a menos que haja algum outro valor especificado explicitamente.

Deixe o `Insert()` método apontando para o `AddProduct` método, mas definir novamente a lista de suspensa da guia exclusão como (nenhum).


[![Defina o na lista suspensa da guia Inserir para o método AddProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Figura 14**: definir a lista de suspensa da guia Inserir os `AddProduct` método ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))


[![Defina a lista suspensa de exclusão da guia como (nenhum)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Figura 15**: lista de lista suspensa da guia excluir como (nenhum) ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))


Depois de fazer essas alterações, a sintaxe declarativa do ObjectDataSource será expandida para incluir um `InsertParameters` coleção, conforme mostrado abaixo:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

Executar novamente o assistente adicionado novamente a `OldValuesParameterFormatString` propriedade. Reserve um tempo para limpar essa propriedade definindo-a para o valor padrão (`{0}`) ou removê-lo totalmente da sintaxe declarativa.

Com o ObjectDataSource fornecendo recursos de inserção, marca inteligente de DetailsView agora incluem a caixa de seleção Permitir inserção; Retorne ao Designer e marque essa opção. Em seguida, diminuir DetailsView para que ele tenha apenas dois BoundFields - `ProductName` e `UnitPrice` - e o CommandField. Neste ponto, a sintaxe declarativa de DetailsView deve ser semelhante:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

Figura 16 mostra essa página quando exibido no momento por meio de um navegador. Como você pode ver, DetailsView lista o nome e o preço do produto primeiro (Chai). No entanto, o que queremos, é uma interface de inserção que fornece um meio para que o usuário adicionar rapidamente um novo produto no banco de dados.


[![DetailsView é renderizado no momento no modo somente leitura](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Figura 16**: O DetailsView é renderizado no momento no modo somente leitura ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))


Para mostrar o DetailsView no modo de inserção, precisamos definir a `DefaultMode` propriedade para `Inserting`. Isso renderiza DetailsView no modo de inserção quando visitado pela primeira vez e os mantém lá após a inserção de um novo registro. Como mostra a Figura 17, tal um DetailsView fornece uma interface rápida para adicionar um novo registro.


[![DetailsView fornece uma Interface para adicionar rapidamente um novo produto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Figura 17**: DetailsView fornece uma Interface para adicionar rapidamente um novo produto ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))


Quando o usuário insere um nome de produto e o preço (por exemplo, "Acme água" e 1.99, como na Figura 17) e clicar em Insert, um postback massacre e começa o fluxo de trabalho de inserção, culminando em um novo registro de produto que está sendo adicionado ao banco de dados. DetailsView mantém sua interface de inserção e GridView é automaticamente se a fonte de dados para incluir o novo produto, conforme mostrado na Figura 18.


![O produto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Figura 18**: O produto "Água Acme" foi adicionado ao banco de dados


Embora GridView na Figura 18 não mostre, os campos de produto na falta da interface DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`e assim por diante são atribuídos `NULL` valores do banco de dados. Você pode ver isso executando as seguintes etapas:

1. Vá para o Gerenciador de servidores no Visual Studio
2. Expandindo o `NORTHWND.MDF` nó banco de dados
3. Clique com botão direito no `Products` nó da tabela de banco de dados
4. Selecione Mostrar dados da tabela

Isso listará todos os registros no `Products` tabela. Como a Figura 19 mostra, todas as colunas do nosso novo produto diferente de `ProductID`, `ProductName`, e `UnitPrice` ter `NULL` valores.


[![O produto campos não fornecido em DetailsView são atribuídos valores nulos](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Figura 19**: O produto campos não fornecido em DetailsView recebem `NULL` valores ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))


Queremos fornecer um valor padrão diferente de `NULL` para um ou mais dos seguintes valores de coluna, ou porque `NULL` não é a melhor opção padrão ou porque a coluna de banco de dados em si não permite `NULL` s. Para fazer isso podemos programaticamente definir os valores dos parâmetros do prvku DetailsView `InputParameters` coleção. Essa atribuição pode ser feita tanto no evento manipulador para o DetailsView `ItemInserting` evento ou o ObjectDataSource `Inserting` eventos. Como que já tratamos nível de controle usando os eventos de pré e pós-níveis os dados da Web, vamos explorar usando eventos do ObjectDataSource neste momento.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Etapa 4: Atribuir valores para o`CategoryID`e`SupplierID`parâmetros

Para este tutorial vamos imaginar que nosso aplicativo ao adicionar um novo produto por meio dessa interface deve ser atribuído um `CategoryID` e `SupplierID` valor 1. Como mencionado anteriormente, o ObjectDataSource tem um par de eventos de pré e pós-níveis que disparem durante o processo de modificação de dados. Quando seu `Insert()` método é invocado, o ObjectDataSource primeiro dispara seu `Inserting` evento, em seguida, chama o método que seu `Insert()` método tiver sido mapeado para e, finalmente, gera o `Inserted` eventos. O `Inserting` manipulador de eventos permite-em uma última oportunidade para ajustar os parâmetros de entrada ou cancelar a operação imediatamente.

> [!NOTE]
> Em um aplicativo do mundo real provavelmente seria recomendável para permitir que o usuário especifique a categoria e fornecedor ou seria escolha esse valor para elas com base em alguns critérios ou negócios lógica (em vez de cegamente selecionando uma ID de 1). Independentemente disso, o exemplo ilustra como definir programaticamente o valor de um parâmetro de entrada de evento de pré-nível do ObjectDataSource.


Reserve um tempo para criar um manipulador de eventos para o ObjectDataSource `Inserting` eventos. Observe que o segundo parâmetro de entrada do manipulador de eventos é um objeto do tipo `ObjectDataSourceMethodEventArgs`, que tem uma propriedade para acessar a coleção de parâmetros (`InputParameters`) e uma propriedade para cancelar a operação (`Cancel`).


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

Neste ponto, o `InputParameters` propriedade contém o ObjectDataSource `InsertParameters` coleção com os valores atribuídos de DetailsView. Para alterar o valor de um desses parâmetros, basta usar: `e.InputParameters["paramName"] = value`. Portanto, para definir a `CategoryID` e `SupplierID` para os valores de 1, ajustar o `Inserting` manipulador de eventos para a seguinte aparência:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Esse tempo ao adicionar um novo produto (como Acme Soda), o `CategoryID` e `SupplierID` colunas do novo produto são definidas como 1 (veja a Figura 20).


[![Novos produtos agora têm seus CategoryID e SupplierID valores definidos como 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Figura 20**: novos produtos agora têm seus `CategoryID` e `SupplierID` valores definidos como 1 ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))


## <a name="summary"></a>Resumo

Durante a edição, inserção e exclusão de processo, o controle de Web de dados e o ObjectDataSource continuar por meio de um número de eventos de pré e pós-níveis. Neste tutorial examinado os eventos de pré-níveis e viu como usá-los para personalizar os parâmetros de entrada ou cancelar a operação de modificação de dados completamente ambos a partir do controle de Web de dados e eventos do ObjectDataSource. O próximo tutorial, examinaremos a criando e usando manipuladores de eventos para os eventos pós-níveis.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Jackie Goor e Liz Shulok. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](an-overview-of-inserting-updating-and-deleting-data-cs.md)
> [Próximo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
