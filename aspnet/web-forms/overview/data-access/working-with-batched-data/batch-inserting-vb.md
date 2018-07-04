---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: Lote de inserção (VB) | Microsoft Docs
author: rick-anderson
description: Saiba como inserir vários registros de banco de dados em uma única operação. Na camada de Interface do usuário, estendemos o GridView para permitir que o usuário insira várias n...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: 17a077ed0124a0a9e06c90d0ac137958693fc30e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390077"
---
<a name="batch-inserting-vb"></a>Lote de inserção (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip) ou [baixar PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> Saiba como inserir vários registros de banco de dados em uma única operação. Na camada de Interface do usuário, estendemos o GridView para permitir que o usuário insira vários novos registros. Na camada de acesso a dados, podemos encapsular as várias operações de inserção em uma transação para garantir que todas as inserções de êxito ou todas as inserções são revertidas.


## <a name="introduction"></a>Introdução

No [atualização em lotes](batch-updating-vb.md) tutorial vimos como personalizar o controle GridView para apresentar uma interface em que vários registros foram editáveis. O usuário visitar a página poderia fazer uma série de alterações e, com um único clique de botão, executar uma atualização em lotes. Para situações em que os usuários normalmente atualizar muitos registros de uma só vez, uma interface desse tipo pode economizar incontáveis cliques e alternâncias de contexto de teclado ao mouse, em comparação com o padrão de recursos de edição que foram exploradas pela primeira vez por linha o [um Visão geral do inserindo, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) tutorial.

Esse conceito também pode ser aplicado ao adicionar os registros. Imagine que aqui na Northwind Traders é comumente receber remessas de fornecedores que contêm um número de produtos para uma determinada categoria. Por exemplo, podemos pode receber uma remessa de seis diferente chá e produtos de café do Traders Tóquio. Se um usuário digita os seis produtos um por vez por meio de um controle DetailsView, será necessário escolher muitos dos mesmos valores repetidamente: será necessário escolher a mesma categoria (Bebidas), o mesmo fornecedor (Tóquio Traders), o mesmo descontinuados (valor False) e as mesmas unidades no valor de ordem (0). Essa entrada de dados repetitivos não só é demorada, mas é propensa a erros.

Com um pouco de trabalho, podemos criar um lote de inserção de interface que permite ao usuário escolher o fornecedor e a categoria de uma vez, insira uma série de nomes de produtos e preços unitários e, em seguida, clique em um botão para adicionar os novos produtos no banco de dados (veja a Figura 1). Como cada produto é adicionado, seus `ProductName` e `UnitPrice` campos de dados são atribuídos os valores inseridos nas caixas de texto, enquanto seus `CategoryID` e `SupplierID` valores são atribuídos os valores no DropDownLists no fo superior do formulário. O `Discontinued` e `UnitsOnOrder` valores são definidos como os valores embutidos de `False` e 0, respectivamente.


[![A Interface de inserção em lotes](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**Figura 1**: A Interface de inserção em lotes ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image3.png))


Neste tutorial, criaremos uma página que implementa o lote de inserção de interface mostrada na Figura 1. Como com os dois tutoriais anteriores, podemos irá encapsular as inserções dentro do escopo de uma transação para garantir a atomicidade. Permitir que o s começar!

## <a name="step-1-creating-the-display-interface"></a>Etapa 1: Criar a Interface de exibição

Este tutorial consistirá em uma única página que é dividida em duas regiões: uma região de exibição e uma região de inserção. A interface de exibição, o que vamos criar nesta etapa, mostra os produtos em um GridView e inclui um botão chamado remessa do produto de processo. Quando este botão é clicado, a interface de exibição é substituída com a interface de inserção, que é mostrada na Figura 1. A interface de exibição retorna depois de adicionar produtos de remessa ou botões Cancel são clicados. Vamos criar a interface de inserção na etapa 2.

Quando a criação de uma página que tem duas interfaces, somente um deles é visível por vez, cada interface normalmente é colocado dentro de um [controle Panel Web](http://www.w3schools.com/aspnet/control_panel.asp), que serve como um contêiner para outros controles. Portanto, nossa página terá dois controles de painel um para cada interface.

Comece abrindo o `BatchInsert.aspx` página o `BatchData` pasta e arrastar um painel da caixa de ferramentas para o Designer (consulte a Figura 2). Definir o painel de s `ID` propriedade para `DisplayInterface`. Ao adicionar o painel para o Designer, sua `Height` e `Width` propriedades são definidas como 50px e 125px, respectivamente. Desmarque out esses valores de propriedade na janela Propriedades.


[![Arraste um painel da caixa de ferramentas para o Designer](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**Figura 2**: arraste um painel da caixa de ferramentas para o Designer ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image6.png))


Em seguida, arraste um controle de botão e GridView para o painel. Definir o botão s `ID` propriedade para `ProcessShipment` e seu `Text` propriedade a remessa de produtos do processo. Definir o s GridView `ID` propriedade para `ProductsGrid` e, na marca inteligente, de associá-lo a um novo ObjectDataSource chamado `ProductsDataSource`. Configurar o ObjectDataSource para efetuar pull de seus dados a partir de `ProductsBLL` classe s `GetProducts` método. Como esse GridView é usado apenas para exibir dados, defina as listas suspensas na atualização, inserção e excluir guias como (nenhum). Clique em Concluir para concluir o Assistente Configurar fonte de dados.


[![Exibir os dados retornados do método GetProducts ProductsBLL classe s](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**Figura 3**: exibir os dados retornados do `ProductsBLL` classe s `GetProducts` método ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image9.png))


[![Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum)](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**Figura 4**: defina a lista suspensa no UPDATE, INSERT e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image12.png))


Depois de concluir o assistente ObjectDataSource, o Visual Studio adicionará BoundFields e um CheckBoxField para os campos de dados do produto. Remover tudo, exceto os `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, e `Discontinued` campos. Fique à vontade fazer qualquer personalização estética. Eu decidi formatar a `UnitPrice` campo como um valor de moeda, reordenadas os campos e renomeado vários campos `HeaderText` valores. Também configure o GridView para incluir a paginação e classificação de suporte, marcando as caixas de seleção Habilitar paginação e habilitar a classificação na marca inteligente s GridView.

Depois de adicionar os controles de painel, botão, GridView e ObjectDataSource e personalizar os campos de s GridView, sua marcação declarativa de página s deve ser semelhante ao seguinte:


[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

Observe que a marcação para o botão e GridView aparecer dentro de abertura e fechamento `<asp:Panel>` marcas. Desde que esses controles estão dentro de `DisplayInterface` painel, podemos pode ocultá-los, simplesmente definindo o painel s `Visible` propriedade para `False`. Etapa 3 examina alterar programaticamente o painel s `Visible` propriedade em resposta a um botão Clique para mostrar uma interface enquanto oculta o outro.

Reserve um tempo para exibir nosso progresso através de um navegador. Como mostra a Figura 5, você verá um botão de remessa de produtos do processo acima um GridView que lista os dez produtos por vez.


[![Lista os produtos de GridView e oferece classificação e paginação de recursos](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**Figura 5**: O GridView lista os produtos e oferece classificação e paginação de recursos ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Etapa 2: Criar a Interface de inserção

Com a interface de exibição completo, podemos está pronto para criar a interface de inserção. Para este tutorial, deixe a criar uma interface de inserção que solicita um único valor de categoria e fornecedor e, em seguida, permite que o usuário insira até cinco nomes de produtos e os valores de unidade de preço do s. Com essa interface, o usuário pode adicionar um a cinco novos produtos que compartilham a mesma categoria e fornecedor, mas têm nomes de produto exclusiva e preços.

Comece arrastando um painel da caixa de ferramentas para o Designer, colocando-o abaixo existente `DisplayInterface` painel. Defina a `ID` propriedade deste recém-adicionados painel para `InsertingInterface` e defina seu `Visible` propriedade para `False`. Vamos adicionar o código que define o `InsertingInterface` painel s `Visible` propriedade `True` na etapa 3. Limpar o painel s também `Height` e `Width` valores de propriedade.

Em seguida, precisamos criar a interface de inserção que foi mostrada na Figura 1. Essa interface pode ser criada por meio de uma variedade de técnicas HTML, mas usaremos um bastante simples: uma tabela de quatro colunas, linhas de sete.

> [!NOTE]
> Ao inserir uma marcação HTML `<table>` elementos, prefiro usar a exibição da fonte. Enquanto o Visual Studio tem ferramentas para inclusão `<table>` elementos por meio do Designer, o Designer parece tudo muito disposto a injetar não solicitado para `style` configurações na marcação. Depois de ter criado o `<table>` marcação, normalmente retornam para o Designer para adicionar os controles da Web e defina suas propriedades. Ao criar tabelas com linhas e colunas predeterminadas eu prefiro usar HTML estático em vez de [controle de tabela da Web](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) porque todos os controles da Web colocados dentro de um controle de tabela da Web só podem ser acessados usando o `FindControl("controlID")` padrão. No entanto, fazer, uso controles da Web de tabela para tabelas dimensionados dinamicamente (aqueles cujo linhas ou colunas são baseadas em algum banco de dados ou a critérios especificados pelo usuário), desde a Web da tabela de controle pode ser construído de forma programática.


Insira a seguinte marcação dentro de `<asp:Panel>` marcas do `InsertingInterface` painel:


[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

Isso `<table>` marcação não inclui todos os controles da Web ao mesmo tempo, vamos adicionar aqueles momentaneamente. Observe que cada `<tr>` elemento contém uma configuração específica da classe CSS: `BatchInsertHeaderRow` para a linha de cabeçalho em que o fornecedor e a categoria DropDownLists irá; `BatchInsertFooterRow` para a linha de rodapé onde adicionar produtos de remessa e os botões Cancelar irão; e alternando `BatchInsertRow` e `BatchInsertAlternatingRow` valores para as linhas que contém o produto e a unidade de preço controles de caixa de texto. Eu ve criado as classes CSS correspondentes no `Styles.css` arquivo para dar a interface inserindo uma aparência semelhante à GridView e DetailsView controla podemos ve usada em toda esses tutoriais. Essas classes CSS são mostrados abaixo.


[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

Com essa marcação inserida, retorne para o modo de exibição de Design. Isso `<table>` deve aparecer como uma tabela de quatro colunas e sete linha no Designer, conforme ilustra a Figura 6.


[![A Interface de inserção é composta de uma coluna de quatro, sete linhas de tabela](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**Figura 6**: A inserção de Interface é composta de uma coluna de quatro, sete linhas de tabela ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image18.png))


Podemos re agora está pronto para adicionar os controles da Web para a interface de inserção. Arraste duas DropDownLists da caixa de ferramentas nas células de tabela para o fornecedor e outro para a categoria apropriadas.

Defina o fornecedor s DropDownList `ID` propriedade para `Suppliers` e associá-lo para um novo ObjectDataSource chamado `SuppliersDataSource`. Configurar o novo ObjectDataSource para recuperar seus dados a partir de `SuppliersBLL` classe s `GetSuppliers` método e a atualização do conjunto guia lista suspensa de s como (nenhum). Clique em Concluir para concluir o assistente.


[![Configurar o ObjectDataSource para usar o método de GetSuppliers SuppliersBLL classe s](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**Figura 7**: configurar o ObjectDataSource para usar o `SuppliersBLL` classe s `GetSuppliers` método ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image21.png))


Ter o `Suppliers` exibição DropDownList a `CompanyName` campo de dados e use o `SupplierID` do campo de dados como seu `ListItem` valores s.


[![Exibir o campo de dados CompanyName e usar SupplierID como o valor](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**Figura 8**: exibir o `CompanyName` campo de dados e Use `SupplierID` como o valor ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image24.png))


Nomeie o segundo DropDownList `Categories` e associá-lo para um novo ObjectDataSource chamado `CategoriesDataSource`. Configurar o `CategoriesDataSource` ObjectDataSource para usar o `CategoriesBLL` classe s `GetCategories` método; defina na lista suspensa lista a atualizar e excluir guias como (nenhum) e clique em Concluir para concluir o assistente. Por fim, ter a exibição DropDownList a `CategoryName` campo de dados e use o `CategoryID` como o valor.

Depois que essas duas DropDownLists foram adicionados e associados ao ObjectDataSources adequadamente configurado, sua tela deve ser semelhante à Figura 9.


[![A linha de cabeçalho contém agora os fornecedores e categorias DropDownLists](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**Figura 9**: O cabeçalho de linha agora contém o `Suppliers` e `Categories` DropDownLists ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image27.png))


Agora, precisamos criar caixas de texto para coletar o nome e o preço para cada novo produto. Arraste um controle de caixa de texto da caixa de ferramentas para o Designer para cada uma das linhas de nome e o preço do cinco produto. Defina as `ID` propriedades das caixas de texto para `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`e assim por diante.

Adicione um CompareValidator após cada uma das caixas de texto, definir o preço da unidade a `ControlToValidate` propriedade para apropriado `ID`. Também defina as `Operator` propriedade para `GreaterThanEqual`, `ValueToCompare` como 0, e `Type` para `Currency`. Essas configurações instruem o CompareValidator para garantir que o preço, se inserido, é um valor de moeda válidos que é maior que ou igual a zero. Defina as `Text` propriedade para \*, e `ErrorMessage` ao preço deve ser maior que ou igual a zero. Além disso, omita quaisquer símbolos de moeda.

> [!NOTE]
> A interface de inserção não inclui todos os controles RequiredFieldValidator, mesmo que o `ProductName` campo o `Products` tabela de banco de dados não permite `NULL` valores. Isso ocorre porque queremos permitir que o usuário insira até cinco produtos. Por exemplo, se o usuário fornecer o preço de unidade e o nome do produto para as três primeiras linhas, deixando as duas últimas linhas em branco, d agregamos três novos produtos no sistema. Uma vez que `ProductName` é necessário, no entanto, precisamos verificar programaticamente garantir que, se um preço unitário é inserido que um valor de nome de produto correspondente é fornecido. Que abordaremos essa verificação na etapa 4.


Ao validar a entrada do usuário s, CompareValidator relata dados inválidos, se o valor contiver um símbolo de moeda. Adicione um $ na frente de cada uma das caixas de texto para servir como uma indicação visual que instrui o usuário omita o símbolo de moeda, ao inserir o preço de preço unitário.

Por fim, adicione um controle ValidationSummary dentro a `InsertingInterface` painel, as configurações de seu `ShowMessageBox` propriedade a ser `True` e sua `ShowSummary` propriedade para `False`. Com essas configurações, se o usuário insere um valor de preço de unidade inválida, um asterisco será exibido ao lado os controles de caixa de texto incorretos e o ValidationSummary exibirá uma caixa de mensagem do lado do cliente que mostra a mensagem de erro que especificamos anteriormente.

Neste ponto, sua tela deve ser semelhante à Figura 10.


[![A Interface inserindo agora inclui caixas de texto para os produtos de nomes e preços](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**Figura 10**: A inserção de Interface agora inclui caixas de texto para os nomes de produtos e os preços ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image30.png))


Em seguida, precisamos adicionar adicionar produtos da remessa e Cancelar botões à linha de rodapé. Arraste dois controles de botão da caixa de ferramentas no rodapé da interface inserindo, definindo os botões `ID` propriedades a serem `AddProducts` e `CancelButton` e `Text` propriedades adicionar produtos da remessa e Cancelar, respectivamente. Além disso, defina as `CancelButton` controle s `CausesValidation` propriedade `false`.

Por fim, precisamos adicionar um controle de Web de rótulo que exibirá mensagens de status para as duas interfaces. Por exemplo, quando um usuário com êxito adiciona uma nova remessa de produtos, queremos retornar para a interface de exibição e exibir uma mensagem de confirmação. Se, no entanto, o usuário fornece um preço para um novo produto, mas parou o nome do produto, é necessário exibir uma mensagem de aviso desde o `ProductName` campo é obrigatório. Já que precisamos essa mensagem a ser exibida para ambas as interfaces, coloque-o na parte superior da página fora os painéis.

Arraste um controle da Web do rótulo da caixa de ferramentas na parte superior da página no Designer. Definir a `ID` propriedade para `StatusLabel`, desmarque out a `Text` propriedade e o conjunto a `Visible` e `EnableViewState` propriedades a serem `False`. Como vimos nos tutoriais anteriores, definindo o `EnableViewState` propriedade para `False` nos permite alterar os valores de propriedade do rótulo s programaticamente e peça para reverter automaticamente para seus padrões no postback subsequente. Isso simplifica o código para mostrar uma mensagem de status em resposta a uma ação de usuário desaparece no postback subsequente. Por fim, defina as `StatusLabel` controle s `CssClass` propriedade para aviso, que é o nome de uma classe CSS definida no arquivo `Styles.css` que exibe texto em uma fonte grande, itálico, negrito, vermelha.

Figura 11 mostra o Designer do Visual Studio depois que o rótulo foi adicionado e configurado.


[![Posicione o controle StatusLabel acima os dois controles de painel](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**Figura 11**: coloque os `StatusLabel` controle acima a dois controles de painel ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Etapa 3: Alternar entre a exibição e inserção de Interfaces

Neste ponto, concluímos a marcação para nossa exibição e inserção de interfaces, mas podemos re ainda faltavam duas tarefas:

- Alternar entre a exibição e inserção de interfaces
- Adicionar os produtos na entrega ao banco de dados

Atualmente, a interface de exibição está visível, mas a interface de inserção está oculta. Isso ocorre porque o `DisplayInterface` painel s `Visible` estiver definida como `True` (o valor padrão), enquanto o `InsertingInterface` painel s `Visible` estiver definida como `False`. Para alternar entre as duas interfaces, basta alternar cada controle s `Visible` valor da propriedade.

Queremos mover da interface de exibição para a interface de inserção quando se clica no botão de remessa do produto de processo. Portanto, criar um manipulador de eventos para que esse botão s `Click` evento que contém o código a seguir:


[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

Esse código simplesmente a oculta a `DisplayInterface` painel e mostra o `InsertingInterface` painel.

Em seguida, crie manipuladores de eventos para adicionar produtos de remessa e o botão Cancelar controles na interface de inserção. Quando um desses botões é clicado, é necessário reverter para a interface de exibição. Crie `Click` manipuladores de eventos para ambos os controles de botão para que eles chamam `ReturnToDisplayInterface`, um método, adicionaremos momentaneamente. Além de ocultação a `InsertingInterface` painel e mostrando o `DisplayInterface` painel, o `ReturnToDisplayInterface` método deve retornar os controles da Web para seu estado de edição previamente. Isso envolve a definição de DropDownLists `SelectedIndex` propriedades como 0 e apagando o `Text` propriedades dos controles de caixa de texto.

> [!NOTE]
> Considere o que poderia acontecer se podemos t retornar os controles para seu estado de edição previamente antes de retornar para a interface de exibição. Um usuário pode clicar no botão de remessa do produto de processo, insira os produtos da remessa e, em seguida, clique em Adicionar produtos de remessa. Isso seria adicionar os produtos e o usuário retornará para a interface de exibição. Neste ponto, o usuário pode querer adicionar outra remessa. Ao clicar no botão de remessa do produto de processo retornarem para a inserção interface, mas a DropDownList seleções e valores de caixa de texto ainda seriam ser populados com os valores anteriores.


[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

Ambos `Click` manipuladores de eventos simplesmente chamam o `ReturnToDisplayInterface` método, embora retornaremos ao adicionar produtos de remessa `Click` manipulador de eventos na etapa 4 e adicione código para salvar os produtos. `ReturnToDisplayInterface` inicia, retornando o `Suppliers` e `Categories` DropDownLists para suas primeiras opções. As duas constantes `firstControlID` e `lastControlID` marcar o início e final de valores de índice do controle usados na nomeação de produto nome e o preço unitário caixas de texto na inserção de interface e são usadas nos limites do `For` loop que define os `Text`propriedades de controles de caixa de texto de volta para uma cadeia de caracteres vazia. Por fim, os painéis `Visible` as propriedades são redefinidas para que a interface de inserção está oculta e a interface de exibição mostrado.

Reserve um tempo para testar esta página em um navegador. Quando o primeiro visitando a página, você deve ver a interface de exibição conforme mostrado na Figura 5. Clique no botão de remessa do produto de processo. A página fará o postback e agora você deve ver a interface de inserção, conforme mostrado na Figura 12. Clicando em ambos os produtos adicionar nos botões remessa ou Cancelar retorna você para a interface de exibição.

> [!NOTE]
> Ao exibir a interface de inserção, reserve um tempo para testar o CompareValidators sobre o preço unitário de caixas de texto. Você deve ver uma caixa de mensagem do lado do cliente de aviso quando você clicar em Adicionar produtos do botão de remessa com valores de moeda inválido preços ou com um valor menor que zero.


[![A Interface de inserção é exibida depois de clicar no botão de remessa do produto de processo](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**Figura 12**: A Interface de inserção é exibida depois de clicar no botão de remessa do produto de processo ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Etapa 4: Adicionar os produtos

Tudo que resta para este tutorial é para salvar os produtos no banco de dados em Adicionar produtos do botão de remessa s `Click` manipulador de eventos. Isso pode ser feito com a criação de um `ProductsDataTable` e adicionando um `ProductsRow` instância para cada um dos nomes de produto fornecidos. Uma vez que eles `ProductsRow` s foram adicionados, faremos uma chamada para o `ProductsBLL` classe s `UpdateWithTransaction` método passando o `ProductsDataTable`. Lembre-se de que o `UpdateWithTransaction` método, que foi criado na [encapsulamento de modificações de banco de dados em uma transação](wrapping-database-modifications-within-a-transaction-vb.md) passa do tutorial, o `ProductsDataTable` para o `ProductsTableAdapter` s `UpdateWithTransaction` método. A partir daí, uma transação ADO.NET é iniciada e os problemas de TableAdatper uma `INSERT` instrução no banco de dados para cada adicionado `ProductsRow` na DataTable. Supondo que todos os produtos são adicionados sem erro, que a transação é confirmada, caso contrário, ela será revertida.

O código para adicionar produtos do botão de remessa s `Click` manipulador de eventos também deve executar um pouco de verificação de erro. Como não há nenhum RequiredFieldValidators usado na interface de inserção, um usuário pode inserir um preço de um produto, omitindo o seu nome. Como o nome do produto s é necessário, se uma condição desse tipo é revelado precisamos alertar o usuário e não continue com as inserções. Completo `Click` código do manipulador de eventos segue:


[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

O manipulador de eventos é iniciado, garantindo que o `Page.IsValid` propriedade retorna um valor de `True`. Se ele retornar `False`, em seguida, que significa que um ou mais o CompareValidators dados inválidos estão se comunicando; nesse caso, não queremos a tentativa de inserir os produtos inseridos ou podemos acabará com uma exceção ao tentar atribuir o preço unitário inserido pelo usuário valor para o `ProductsRow` s `UnitPrice` propriedade.

Em seguida, uma nova `ProductsDataTable` instância é criada (`products`). Um `For` loop é usado para iterar por meio do nome e a unidade de preço do produto caixas de texto e o `Text` propriedades são lidas nas variáveis locais `productName` e `unitPrice`. Se o usuário inseriu um valor para o preço unitário, mas não para o nome do produto correspondente, o `StatusLabel` exibe a mensagem se você fornecer uma unidade de preço, você também deve incluir o nome do produto e o manipulador de eventos é encerrado.

Se um nome de produto foi fornecido, uma nova `ProductsRow` instância for criada usando o `ProductsDataTable` s `NewProductsRow` método. Essa nova `ProductsRow` instância s `ProductName` estiver definida como o produto atual nomeie a caixa de texto enquanto o `SupplierID` e `CategoryID` propriedades são atribuídas ao `SelectedValue` propriedades do DropDownLists no cabeçalho da interface s inserindo. Se o usuário inseriu um valor para o preço do produto s, ele será atribuído para o `ProductsRow` instância s `UnitPrice` propriedade; caso contrário, a propriedade é esquerda não atribuída, o que resultará em um `NULL` de valor para `UnitPrice` no banco de dados. Por fim, o `Discontinued` e `UnitsOnOrder` propriedades são atribuídas aos valores embutidos `False` e 0, respectivamente.

Depois que as propriedades foram atribuídas à `ProductsRow` instância, ele é adicionado para o `ProductsDataTable`.

Ao concluir o `For` loop, verificamos se todos os produtos foram adicionados. O usuário pode, afinal de contas, ter clicado em Adicionar produtos de remessa antes de inserir quaisquer nomes de produto ou de preços. Se houver pelo menos um produto na `ProductsDataTable`, o `ProductsBLL` classe s `UpdateWithTransaction` método é chamado. Em seguida, os dados é ligado novamente para o `ProductsGrid` GridView para que os produtos adicionados recentemente aparecerá na interface de exibição. O `StatusLabel` é atualizada para exibir uma mensagem de confirmação e o `ReturnToDisplayInterface` é invocado, ocultando a inserção de interface e mostrando a interface de exibição.

Se nenhum produto foram inserido, a interface de inserção permanece exibida, mas a mensagem que sem produtos foram adicionada. Digite os nomes de produto e os preços da unidade nas caixas de texto é exibido.

S Figura 13, 14 e 15 mostram a inserção e exibam interfaces em ação. Na Figura 13, o usuário inseriu um valor de preço de unidade sem um nome de produto correspondente. Figura 14 mostra a interface de exibição após três novos produtos foram adicionados com êxito, embora a Figura 15 mostra dois dos produtos adicionados recentemente no GridView (o terceiro é na página anterior).


[![Um nome de produto é necessária ao inserir um preço de unidade](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**Figura 13**: um nome de produto é necessária ao inserir um preço de unidade ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image39.png))


[![Três legumes novos foram adicionados para o fornecedor Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**Figura 14**: três novos legumes foram adicionados para o fornecedor Mayumi s ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image42.png))


[![Os novos produtos podem ser encontrados na última página do GridView](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**Figura 15**: os novos produtos podem ser encontradas na última página do GridView ([clique para exibir a imagem em tamanho normal](batch-inserting-vb/_static/image45.png))


> [!NOTE]
> O lote de inserção de lógica usada neste tutorial envolve as inserções dentro do escopo da transação. Para verificar isso, propositadamente apresente um erro no nível de banco de dados. Por exemplo, em vez de atribuir o novo `ProductsRow` instância s `CategoryID` propriedade para o valor selecionado na `Categories` DropDownList, atribua-o para um valor, como `i * 5`. Aqui `i` é o indexador de loop e tem valores que variam de 1 a 5. Dessa forma, quando a adição de dois ou mais produtos em lote inserir o primeiro produto terá válida `CategoryID` terão o valor (5), mas produtos subsequentes `CategoryID` valores que não correspondem aos `CategoryID` os valores no `Categories` tabela. O efeito líquido é que, enquanto o primeiro `INSERT` terá êxito, os demais falhará com uma violação de restrição de chave estrangeira. Uma vez que a inserção de lote é atômica, a primeira `INSERT` será revertida, retornando o banco de dados para seu estado antes que o lote de inserção processo começou.


## <a name="summary"></a>Resumo

Sobre isso e os dois tutoriais anteriores, criamos a interfaces que permitem a atualização, exclusão, e inserção de lotes de dados, que usados o suporte a transações que adicionamos à camada de acesso a dados no [quebra automática de modificações de banco de dados dentro de uma transação](wrapping-database-modifications-within-a-transaction-vb.md) tutorial. Para determinados cenários, tais interfaces de usuário de processamento de lote melhoram muito a eficiência do usuário final ao diminuir o número de cliques, postbacks e alternâncias de contexto do teclado ao mouse, além de manter a integridade dos dados subjacentes.

Esse tutorial conclui nossa como trabalhar com dados em lote. O próximo conjunto de tutoriais explora uma variedade de cenários avançados de camada de acesso a dados, incluindo o uso de procedimentos armazenados nos métodos TableAdapter s, configurando as configurações de nível de conexão e comando da DAL, criptografando cadeias de caracteres de conexão e muito mais!

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Líder de revisores para este tutorial foram Hilton Giesenow e S ren Jacob Lauritsen. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](batch-deleting-vb.md)
