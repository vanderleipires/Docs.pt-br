---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
title: Lote de inserção (c#) | Microsoft Docs
author: rick-anderson
description: Saiba como inserir vários registros de banco de dados em uma única operação. Na camada de Interface do usuário estendemos GridView para permitir que o usuário insira várias n...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: cf025e08-48fc-4385-b176-8610aa7b5565
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
msc.type: authoredcontent
ms.openlocfilehash: c8995592d9206fb17a7769414212369946304c54
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888403"
---
<a name="batch-inserting-c"></a>Lote de inserção (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_CS.zip) ou [baixar PDF](batch-inserting-cs/_static/datatutorial66cs1.pdf)

> Saiba como inserir vários registros de banco de dados em uma única operação. Na camada de Interface do usuário estendemos GridView para permitir que o usuário insira vários novos registros. Na camada de acesso a dados encerrar as várias operações de inserção em uma transação para garantir que todas as inserções bem-sucedida ou todas as inserções são revertidas.


## <a name="introduction"></a>Introdução

No [atualização em lotes](batch-updating-cs.md) tutorial vimos Personalizando o controle GridView para apresentar uma interface em que vários registros foram editáveis. O usuário visitar a página pode fazer uma série de alterações e, em seguida, com um único clique de botão, executar uma atualização em lotes. Em situações em que os usuários geralmente atualizar vários registros de uma só vez, tal interface pode salvar o inúmeros cliques e alternâncias de contexto de teclado para mouse quando comparado com o padrão de recursos de edição que foram explorados primeiro novamente por linha de [um Visão geral de inserindo, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial.

Esse conceito também pode ser aplicado ao adicionar registros. Imagine que aqui da Northwind Traders comumente recebemos remessas de fornecedores que contêm um número de produtos para uma determinada categoria. Por exemplo, podemos pode receber uma remessa de seis chá diferente e produtos de café Tokyo Traders. Se um usuário inserir os seis produtos um por vez por meio de um controle DetailsView, será necessário escolher muitos dos mesmos valores repetidamente: será necessário escolher a mesma categoria (Bebidas), o mesmo fornecedor (Tokyo Traders), o mesmo Descontinuado (valor False) e as mesmas unidades no valor de ordem (0). Essa entrada de dados repetitivos não só é demorada, mas é propensa a erros.

Com um pouco de trabalho, podemos criar um lote de inserção de interface que permite ao usuário escolher o fornecedor e a categoria de uma vez, insira uma série de nomes de produtos e preços unitários e, em seguida, clique em um botão para adicionar os novos produtos no banco de dados (consulte a Figura 1). Como cada produto é adicionado, seus `ProductName` e `UnitPrice` campos de dados são atribuídos os valores inseridos nas caixas de texto, enquanto seu `CategoryID` e `SupplierID` valores são atribuídos os valores de DropDownLists no fo superior do formulário. O `Discontinued` e `UnitsOnOrder` valores são definidos para os valores embutidos do `false` e 0, respectivamente.


[![A Interface de inserção em lotes](batch-inserting-cs/_static/image2.png)](batch-inserting-cs/_static/image1.png)

**Figura 1**: A Interface de inserção em lotes ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image3.png))


Este tutorial, vamos criar uma página que implementa o lote inserindo interface mostrado na Figura 1. Como com dois tutoriais anteriores, podemos ajustará as inserções dentro do escopo de uma transação para garantir a atomicidade. Permitir que o s começar!

## <a name="step-1-creating-the-display-interface"></a>Etapa 1: Criar a Interface de exibição

Este tutorial consistirá em uma única página que é dividida em duas regiões: uma região de exibição e uma região de inserção. A interface de exibição, que vamos criar nesta etapa, mostra os produtos em um GridView e inclui um botão intitulado remessa de produtos do processo. Quando esse botão é clicado, a interface de exibição é substituída com a interface de inserção, o que é mostrada na Figura 1. A interface de exibição retorna após adicionar produtos de remessa ou botões de cancelamento são clicados. Vamos criar a interface inserindo na etapa 2.

Ao criar uma página que tem duas interfaces, apenas um deles é visível por vez, cada interface normalmente é colocado dentro de um [controle da Web do painel](http://www.w3schools.com/aspnet/control_panel.asp), que serve como um contêiner para outros controles. Portanto, nossa página terá dois controles de painel um para cada interface.

Comece abrindo o `BatchInsert.aspx` página o `BatchData` pasta e arraste um painel da caixa de ferramentas para o Designer (consulte a Figura 2). Definir o painel s `ID` propriedade `DisplayInterface`. Ao adicionar o painel para o Designer, seu `Height` e `Width` propriedades são definidas como 50px e 125px, respectivamente. Desmarque out esses valores de propriedade na janela Propriedades.


[![Arraste um painel da caixa de ferramentas para o Designer](batch-inserting-cs/_static/image5.png)](batch-inserting-cs/_static/image4.png)

**Figura 2**: arraste um painel da caixa de ferramentas para o Designer ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image6.png))


Em seguida, arraste um controle de botão e GridView para o painel. Definir o botão s `ID` propriedade `ProcessShipment` e sua `Text` propriedade a remessa de produtos do processo. Definir o GridView s `ID` propriedade `ProductsGrid` e, na marca inteligente, de associá-lo a um novo ObjectDataSource denominado `ProductsDataSource`. Configurar ObjectDataSource para efetuar o pull de seus dados a partir de `ProductsBLL` classe s `GetProducts` método. Como esse GridView é usado apenas para exibir dados, defina as listas suspensas na atualização, inserção e excluir guias como (nenhum). Clique em Concluir para concluir o Assistente Configurar fonte de dados.


[![Exibir os dados retornados pelo método GetProducts ProductsBLL classe s](batch-inserting-cs/_static/image8.png)](batch-inserting-cs/_static/image7.png)

**Figura 3**: exibir os dados retornados do `ProductsBLL` classe s `GetProducts` método ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image9.png))


[![Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum)](batch-inserting-cs/_static/image11.png)](batch-inserting-cs/_static/image10.png)

**Figura 4**: definir a lista suspensa na atualização, inserir e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image12.png))


Depois de concluir o assistente ObjectDataSource, o Visual Studio adicionará BoundFields e um CheckBoxField para os campos de dados de produto. Remover tudo, exceto o `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, e `Discontinued` campos. Fique à vontade para fazer as personalizações estéticos. Decidi formatar o `UnitPrice` campo como um valor de moeda, reordenar os campos e renomeado vários campos `HeaderText` valores. Também configure o GridView para incluir a paginação e a classificação de suporte, marcando as caixas de seleção Habilitar paginação e Habilitar classificação na marca inteligente s GridView.

Depois de adicionar os controles de painel, botão, GridView e ObjectDataSource e personalizar os campos de s GridView, sua marcação declarativa s de página deve ser semelhante ao seguinte:


[!code-aspx[Main](batch-inserting-cs/samples/sample1.aspx)]

Observe que a marcação do botão e GridView aparecem dentro de abertura e fechamento `<asp:Panel>` marcas. Desde que esses controles estão dentro do `DisplayInterface` painel, é possível ocultá-las, simplesmente definindo o painel s `Visible` propriedade `false`. Etapa 3 examina alterar programaticamente o painel s `Visible` propriedade em resposta a um botão de clique para mostrar uma interface ao ocultar a outra.

Dedicar um tempo para exibir nosso andamento por meio de um navegador. Como mostra a Figura 5, você verá um botão de remessa de produtos de processo acima um GridView que lista os produtos dez por vez.


[![Lista os produtos de GridView e oferece a classificação e paginação recursos](batch-inserting-cs/_static/image14.png)](batch-inserting-cs/_static/image13.png)

**Figura 5**: GridView lista os produtos e oferece a classificação e paginação recursos ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Etapa 2: Criar a Interface de inserção

Com a interface de exibição completa, podemos re pronto para criar a inserção de interface. Para este tutorial, permitem s criar uma interface de inserção que solicita um único valor de fornecedor e a categoria e, em seguida, permite que o usuário insira até cinco nomes de produtos e os valores de unidade de preço. Com esta interface, o usuário pode adicionar um a cinco novos produtos que todos compartilham a mesma categoria e fornecedor, mas têm nomes de produto exclusiva e preços.

Comece a arrastar um painel da caixa de ferramentas para o Designer, colocando-o abaixo existente `DisplayInterface` painel. Definir o `ID` propriedade deste adicionado recentemente o painel para `InsertingInterface` e defina seu `Visible` propriedade `false`. Vamos adicionar o código que define o `InsertingInterface` painel s `Visible` propriedade `true` na etapa 3. Limpar o painel s também `Height` e `Width` valores de propriedade.

Em seguida, é preciso criar a interface de inserção que foi exibida novamente na Figura 1. Essa interface pode ser criada por meio de uma variedade de técnicas HTML, mas vamos usar um bastante simples: uma tabela de quatro colunas, linhas de sete.

> [!NOTE]
> Ao inserir a marcação HTML `<table>` elementos, prefiro usar a exibição da fonte. Enquanto o Visual Studio tem ferramentas para adicionar `<table>` elementos por meio do Designer, o Designer parece todos os demais disposto injetar não solicitado para `style` configurações na marcação. Depois de ter criado o `<table>` marcação, geralmente retornar para o Designer para adicionar os controles da Web e definir suas propriedades. Ao criar tabelas com linhas e colunas predeterminadas prefiro usando HTML estático em vez de [controle Table Web](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) porque os controles da Web colocados dentro de um controle de tabela da Web só podem ser acessados usando o `FindControl("controlID")` padrão. , No entanto, uso controles da Web de tabela para tamanho dinamicamente tabelas (aqueles cujo linhas ou colunas se baseiam em algum banco de dados ou os critérios especificados pelo usuário), desde a Web da tabela de controle pode ser construído por meio de programação.


Insira a seguinte marcação dentro de `<asp:Panel>` marcas do `InsertingInterface` painel:


[!code-html[Main](batch-inserting-cs/samples/sample2.html)]

Isso `<table>` marcação não inclui quaisquer controles da Web ainda, adicionaremos os momentaneamente. Observe que cada `<tr>` elemento contém uma configuração específica da classe CSS: `BatchInsertHeaderRow` para a linha de cabeçalho em que o fornecedor e a categoria DropDownLists irá; `BatchInsertFooterRow` para a linha de rodapé onde adicionar produtos de remessa e os botões Cancelar irão; e alternando `BatchInsertRow` e `BatchInsertAlternatingRow` valores para as linhas que contém o produto e a unidade de preço controles de caixa de texto. Eu ve criado classes CSS correspondentes no `Styles.css` arquivo para fornecer a interface inserindo uma aparência semelhante a GridView e DetailsView controla ve usado esses tutoriais. Essas classes CSS são mostrados abaixo.


[!code-css[Main](batch-inserting-cs/samples/sample3.css)]

Com essa marcação inserida, retorne para o modo de exibição de Design. Isso `<table>` deve aparecer como uma tabela de quatro colunas, linha sete no Designer, conforme ilustra a Figura 6.


[![A Interface de inserção é composta de uma coluna de quatro, tabela de sete linhas](batch-inserting-cs/_static/image17.png)](batch-inserting-cs/_static/image16.png)

**Figura 6**: A Interface de inserção é composto de uma coluna de quatro, tabela de sete linhas ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image18.png))


Podemos re agora está pronto para adicionar os controles da Web para a interface de inserção. Arraste dois DropDownLists da caixa de ferramentas nas células de tabela para o fornecedor e outro para a categoria apropriadas.

Definir o fornecedor DropDownList s `ID` propriedade `Suppliers` e associá-lo a um novo ObjectDataSource denominado `SuppliersDataSource`. Configurar o novo ObjectDataSource para recuperar seus dados a partir de `SuppliersBLL` classe s `GetSuppliers` método e a atualização de conjunto guia lista suspensa de s como (nenhum). Clique em Concluir para concluir o assistente.


[![Configurar o ObjectDataSource para usar o método de GetSuppliers SuppliersBLL classe s](batch-inserting-cs/_static/image20.png)](batch-inserting-cs/_static/image19.png)

**Figura 7**: configurar o ObjectDataSource para usar o `SuppliersBLL` classe s `GetSuppliers` método ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image21.png))


Ter o `Suppliers` DropDownList exibição o `CompanyName` campo de dados e use o `SupplierID` do campo de dados como seu `ListItem` s valores.


[![Exibir o campo de dados CompanyName e usar SupplierID como o valor](batch-inserting-cs/_static/image23.png)](batch-inserting-cs/_static/image22.png)

**Figura 8**: exibição de `CompanyName` campo de dados e uso `SupplierID` como o valor ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image24.png))


Nome DropDownList segundo `Categories` e associá-lo a um novo ObjectDataSource denominado `CategoriesDataSource`. Configurar o `CategoriesDataSource` ObjectDataSource para usar o `CategoriesBLL` classe s `GetCategories` método; defina a lista suspensa mostra a atualizar e excluir guias como (nenhum) e clique em Concluir para concluir o assistente. Por fim, ter a exibição DropDownList a `CategoryName` campo de dados e use o `CategoryID` como o valor.

Depois que esses dois DropDownLists foram adicionados e associados ao ObjectDataSources adequadamente configurado, sua tela deve ser semelhante à Figura 9.


[![A linha de cabeçalho contém agora os fornecedores e categorias DropDownLists](batch-inserting-cs/_static/image26.png)](batch-inserting-cs/_static/image25.png)

**Figura 9**: O cabeçalho de linha agora contém o `Suppliers` e `Categories` DropDownLists ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image27.png))


Agora, precisamos criar caixas de texto para coletar o nome e o preço para cada novo produto. Arraste um controle de caixa de texto da caixa de ferramentas para o Designer para cada uma das linhas de nome e o preço de cinco produtos. Definir o `ID` propriedades das caixas de texto para `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`e assim por diante.

Adicionar um CompareValidator após cada do preço unitário caixas de texto, definindo o `ControlToValidate` propriedade apropriada `ID`. Também definir o `Operator` propriedade `GreaterThanEqual`, `ValueToCompare` como 0, e `Type` para `Currency`. Essas configurações instruem o CompareValidator para garantir que o preço, se inserido, é um valor de moeda válido que é maior que ou igual a zero. Definir o `Text` propriedade \*, e `ErrorMessage` ao preço deve ser maior ou igual a zero. Além disso, por favor, omita qualquer símbolo de moeda.

> [!NOTE]
> A interface de inserção não inclui quaisquer controles RequiredFieldValidator, embora o `ProductName` campo o `Products` tabela de banco de dados não permite `NULL` valores. Isso ocorre porque você deseja permitir que o usuário insira até cinco produtos. Por exemplo, se o usuário fornecer o produto nome e o preço unitário para as três primeiras linhas, deixando as duas últimas linhas em branco, d apenas adicionamos três novos produtos no sistema. Como `ProductName` é necessário, no entanto, precisamos verificar de forma programática garantir que, se um preço unitário é inserido que um valor de nome de produto correspondente é fornecido. Que abordaremos essa seleção na etapa 4.


Ao validar a entrada do usuário s, CompareValidator informa dados inválidos, se o valor contém um símbolo de moeda. Adicione um $ na frente de cada uma das caixas de texto para servir como uma indicação visual que instrui o usuário para omitir o símbolo de moeda ao inserir o preço do preço unitário.

Por fim, adicione um controle ValidationSummary dentro o `InsertingInterface` painel, as configurações de seu `ShowMessageBox` propriedade `true` e sua `ShowSummary` propriedade `false`. Com essas configurações, se o usuário insere um valor inválido de unidade, um asterisco aparecerá ao lado dos controles de caixa de texto incorretos e o ValidationSummary exibirá uma caixa de mensagem do lado do cliente que mostra a mensagem de erro que são especificados anteriormente.

Neste ponto, sua tela deve ser semelhante a Figura 10.


[![A Interface inserindo agora inclui caixas de texto para os produtos nomes e preços](batch-inserting-cs/_static/image29.png)](batch-inserting-cs/_static/image28.png)

**Figura 10**: A inserção de Interface agora inclui caixas de texto para os nomes de produtos e preços ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image30.png))


Em seguida, precisamos adicionar adicionar produtos de botões de remessa e Cancelar à linha de rodapé. Arraste dois controles de botão da caixa de ferramentas no rodapé da interface de inserção, definindo os botões `ID` propriedades `AddProducts` e `CancelButton` e `Text` propriedades adicionar produtos de remessa e Cancelar, respectivamente. Além disso, defina o `CancelButton` controle s `CausesValidation` propriedade `false`.

Por fim, precisamos adicionar um controle de rótulo Web que exibirá mensagens de status para as duas interfaces. Por exemplo, quando um usuário com êxito adiciona uma nova remessa de produtos, queremos retornar para a interface de exibição e exibir uma mensagem de confirmação. Se, no entanto, o usuário fornece um preço para um novo produto, mas deixa desativar o nome do produto, é necessário exibir uma mensagem de aviso desde o `ProductName` campo é obrigatório. Já que precisamos esta mensagem a ser exibida para ambas as interfaces, coloque-o na parte superior da página fora os painéis.

Arraste um controle da Web do rótulo da caixa de ferramentas na parte superior da página no Designer. Definir o `ID` propriedade para `StatusLabel`, desmarque-out a `Text` propriedade e defina o `Visible` e `EnableViewState` propriedades para `false`. Como vimos nos tutoriais anteriores, definindo o `EnableViewState` propriedade `false` nos permite alterar os valores de propriedade de rótulo s programaticamente e tê-los automaticamente reverter para padrão no postback subsequente. Isso simplifica o código para mostrar uma mensagem de status em resposta a uma ação de usuário desaparece no postback subsequente. Finalmente, defina o `StatusLabel` controle s `CssClass` propriedade para aviso, que é o nome de uma classe CSS definida no `Styles.css` que exibe o texto em uma fonte grande, itálico, em negrito e vermelha.

A Figura 11 mostra o Designer Visual Studio depois que o rótulo foi adicionado e configurado.


[![Posicione o controle StatusLabel acima dois controles de painel](batch-inserting-cs/_static/image32.png)](batch-inserting-cs/_static/image31.png)

**Figura 11**: coloque o `StatusLabel` controle acima a dois controles de painel ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Etapa 3: Alternar entre a exibição e inserção de Interfaces

Neste ponto, concluiu a marcação para nosso exibição e inserção de interfaces, mas estamos re ainda é deixada com duas tarefas:

- Alternar entre a exibição e inserção de interfaces
- Adicionar os produtos na remessa para o banco de dados

Atualmente, a interface de exibição está visível, mas a interface de inserção está oculta. Isso ocorre porque o `DisplayInterface` painel s `Visible` está definida como `true` (o valor padrão), enquanto o `InsertingInterface` painel s `Visible` está definida como `false`. Para alternar entre as duas interfaces, precisamos simplesmente alternar cada controle s `Visible` valor da propriedade.

Queremos mover da interface de exibição para a interface de inserção quando clicar no botão de remessa de produtos do processo. Portanto, criar um manipulador de eventos para este botão s `Click` eventos que contém o código a seguir:


[!code-csharp[Main](batch-inserting-cs/samples/sample4.cs)]

Esse código simplesmente oculta o `DisplayInterface` painel e mostra o `InsertingInterface` painel.

Em seguida, crie manipuladores de eventos para adicionar produtos de remessa e o botão Cancelar controles na interface de inserção. Quando um desses botões é clicado, é necessário reverter para a interface de exibição. Criar `Click` manipuladores de eventos para os controles de botão para que eles chamam `ReturnToDisplayInterface`, um método adicionaremos momentaneamente. Além de ocultar o `InsertingInterface` painel e mostrando o `DisplayInterface` painel, o `ReturnToDisplayInterface` método deve retornar os controles da Web para seu estado de edição previamente. Isso envolve a configuração de DropDownLists `SelectedIndex` propriedades como 0 e limpar o `Text` propriedades dos controles de caixa de texto.

> [!NOTE]
> Considere o que poderia acontecer se nós não foi retornar os controles de pré-edição estado antes de retornar para a interface de exibição. Um usuário pode clicar no botão de remessa de produtos de processo, insira os produtos de remessa e, em seguida, clique em Adicionar produtos de remessa. Isso seria adicionar os produtos e retornar o usuário para a interface de exibição. Neste ponto, o usuário talvez queira adicionar outra remessa. Ao clicar no botão de remessa de produtos de processo que eles poderiam retornar para a interface inserindo mas DropDownList seleções e valores de texto ainda seriam ser populados com os valores anteriores.


[!code-csharp[Main](batch-inserting-cs/samples/sample5.cs)]

Ambos `Click` simplesmente chamar manipuladores de eventos de `ReturnToDisplayInterface` método, embora retornaremos ao adicionar produtos de remessa `Click` manipulador de eventos na etapa 4 e adicione código para salvar os produtos. `ReturnToDisplayInterface` inicia, retornando o `Suppliers` e `Categories` DropDownLists suas opções primeiro. As duas constantes `firstControlID` e `lastControlID` marca inicial e final usados em nomes de produto nome e o preço unitário caixas de texto na inserção de interface e são usadas em limites de valores de controle de índice a `for` loop que define o `Text`propriedades dos controles TextBox de volta para uma cadeia de caracteres vazia. Por fim, os painéis `Visible` para que a interface de inserção está oculta e a interface de exibição mostrada as propriedades são redefinidas.

Dedicar um tempo para testar esta página em um navegador. Ao primeiro visitar a página você deve ver a interface de exibição, conforme mostrado na Figura 5. Clique no botão de remessa de produtos do processo. A página fará o postback e agora você deve ver a interface de inserção, como mostrado na Figura 12. Clicando em ambos os produtos adicionar botões de remessa ou em Cancelar, você retorna para a interface de exibição.

> [!NOTE]
> Ao exibir a interface de inserção, dedique alguns momentos para testar o CompareValidators no preço unitário caixas de texto. Você deve ver uma caixa de mensagem do lado do cliente de aviso quando clicar em Adicionar produtos de botão de remessa com valores de moeda inválido preços ou com um valor menor que zero.


[![A Interface de inserção é exibida depois de clicar no botão de remessa de produtos do processo](batch-inserting-cs/_static/image35.png)](batch-inserting-cs/_static/image34.png)

**Figura 12**: A Interface de inserção é exibida depois de clicar no botão de remessa de produtos de processo ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Etapa 4: Adicionando os produtos

Tudo permanece para este tutorial é salvar os produtos no banco de dados em Adicionar produtos de botão de remessa s `Click` manipulador de eventos. Isso pode ser feito criando um `ProductsDataTable` e adicionando um `ProductsRow` instância para cada um dos nomes de produto fornecidos. Uma vez que eles `ProductsRow` s foram adicionados faremos uma chamada para o `ProductsBLL` classe s `UpdateWithTransaction` método passando o `ProductsDataTable`. Lembre-se de que o `UpdateWithTransaction` método, que foi criado no [encapsulamento modificações de banco de dados em uma transação](wrapping-database-modifications-within-a-transaction-cs.md) tutorial, passa o `ProductsDataTable` para o `ProductsTableAdapter` s `UpdateWithTransaction` método. A partir daí, uma transação ADO.NET é iniciada e os problemas de TableAdatper um `INSERT` instrução no banco de dados para cada adicionado `ProductsRow` no DataTable. Supondo que todos os produtos são adicionados sem erro, que a transação é confirmada, caso contrário, ela será revertida.

O código para adicionar produtos de botão de remessa s `Click` manipulador de eventos também precisa executar uma pequena verificação de erros. Como não há nenhum RequiredFieldValidators usado na interface de inserção, um usuário pode inserir um preço de um produto, omitindo o seu nome. Como o nome do produto s é necessário, se dessas condições é revelado precisamos alertar o usuário e não continue com as inserções. Completo `Click` segue o código de manipulador de eventos:


[!code-csharp[Main](batch-inserting-cs/samples/sample6.cs)]

O manipulador de eventos é iniciado, garantindo que o `Page.IsValid` propriedade retorna um valor de `true`. Se ele retorna `false`, que significa que um ou mais o CompareValidators estão reportando os dados inválidos; caso não queremos tentativa de inserir os produtos inseridos ou Terminaremos com uma exceção durante a tentativa de atribuir o preço unitário inserido pelo usuário o valor para o `ProductsRow` s `UnitPrice` propriedade.

Em seguida, um novo `ProductsDataTable` instância é criada (`products`). Um `for` loop é usado para iterar por meio do nome e a unidade de preço do produto caixas de texto e o `Text` propriedades são lidas nas variáveis locais `productName` e `unitPrice`. Se o usuário tiver inserido um valor para o preço unitário, mas não para o nome do produto correspondente, o `StatusLabel` exibe a mensagem se você fornecer uma unidade de preço você também deve incluir o nome do produto e o manipulador de eventos é encerrado.

Se um nome de produto tiver sido fornecido, um novo `ProductsRow` instância é criada usando o `ProductsDataTable` s `NewProductsRow` método. Essa nova `ProductsRow` instância s `ProductName` está definida como o produto atual nome de caixa de texto ao `SupplierID` e `CategoryID` propriedades são atribuídas para o `SelectedValue` propriedades de DropDownLists no cabeçalho da interface s inserindo. Se o usuário inseriu um valor para o preço do produto s, ele está atribuído a `ProductsRow` instância s `UnitPrice` propriedade; caso contrário, a propriedade é esquerda não atribuída, o que resultará em um `NULL` valor para `UnitPrice` no banco de dados. Por fim, o `Discontinued` e `UnitsOnOrder` propriedades são atribuídas aos valores embutidos `false` e 0, respectivamente.

Depois que as propriedades foram atribuídas ao `ProductsRow` instância, ele é adicionado para o `ProductsDataTable`.

Após a conclusão do `for` loop, precisamos verificar se todos os produtos foram adicionados. O usuário pode, afinal, ter clicado em Adicionar produtos de remessa antes de inserir qualquer preços ou nomes de produto. Se houver pelo menos um produto de `ProductsDataTable`, o `ProductsBLL` classe s `UpdateWithTransaction` método é chamado. Em seguida, os dados é vinculada outra vez para o `ProductsGrid` GridView para que os produtos adicionados recentemente aparecerá na interface de exibição. O `StatusLabel` é atualizada para exibir uma mensagem de confirmação e o `ReturnToDisplayInterface` é invocado, ocultando inserindo interface e exibir a interface de exibição.

Se nenhum produto foram inserido, a interface de inserção permanece exibida, mas a mensagem que nenhum produto foram adicionado. Insira os nomes de produto e preços unitários nas caixas de texto é exibido.

S Figura 13, 14 e 15 mostram a inserir e exibir interfaces em ação. Na Figura 13, o usuário inseriu um valor do preço de unidade sem um nome de produto correspondente. Figura 14 mostra a interface de exibição após três novos produtos foram adicionados com êxito, enquanto a Figura 15 mostra dois dos produtos recém-adicionado em GridView (o terceiro é a página anterior).


[![Um nome de produto é necessária ao inserir um preço unitário](batch-inserting-cs/_static/image38.png)](batch-inserting-cs/_static/image37.png)

**Figura 13**: um nome de produto é necessária ao inserir um preço unitário ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image39.png))


[![Três legumes novos foram adicionados para o fornecedor Mayumi s](batch-inserting-cs/_static/image41.png)](batch-inserting-cs/_static/image40.png)

**Figura 14**: três novos legumes foram adicionados para o s Mayumi do fornecedor ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image42.png))


[![Os novos produtos podem ser encontrados na última página do GridView](batch-inserting-cs/_static/image44.png)](batch-inserting-cs/_static/image43.png)

**Figura 15**: O novo produtos podem ser encontradas na última página do GridView ([clique para exibir a imagem em tamanho normal](batch-inserting-cs/_static/image45.png))


> [!NOTE]
> O lote inserindo lógica usada neste tutorial encapsula as inserções dentro do escopo da transação. Para verificar isso, intencionalmente apresente um erro no nível de banco de dados. Por exemplo, em vez de atribuir a nova `ProductsRow` instância s `CategoryID` propriedade para o valor selecionado no `Categories` DropDownList, atribua-o para um valor como `i * 5`. Aqui `i` é o indexador de loop e tem valores que variam de 1 a 5. Portanto, quando adicionar dois ou mais produtos em lote insira o primeiro terá uma opção válida `CategoryID` valor (5), mas produtos subsequentes terá `CategoryID` valores que não correspondem aos `CategoryID` valores no `Categories` tabela. O efeito líquido é que durante a primeira `INSERT` terá êxito, os demais falhará com uma violação de restrição de chave estrangeira. Como a inserção de lote é atômica, a primeira `INSERT` será revertida, retornando o banco de dados para seu estado antes que o lote de inserção processo começou.


## <a name="summary"></a>Resumo

Sobre isso e os tutoriais anteriores de dois nós criamos interfaces que permitem atualizar, excluir, e inserir lotes de dados, que usado o suporte a transações adicionamos à camada de acesso a dados a [encapsulamento modificações de banco de dados em uma transação](wrapping-database-modifications-within-a-transaction-cs.md) tutorial. Para determinados cenários, essas interfaces de usuário de processamento em lotes melhorar significativamente a eficiência do usuário final ao ser recortado para baixo no número de cliques e postbacks alternâncias de contexto de teclado para mouse, além de manter a integridade dos dados subjacentes.

Este tutorial conclui nossa aparência em trabalhar com dados em lotes. O próximo conjunto de tutoriais explora uma variedade de cenários avançados de camada de acesso a dados, incluindo o uso de procedimentos armazenados nos métodos TableAdapter s, como definir configurações de nível de conexão e comando no DAL, criptografando cadeias de caracteres de conexão e muito mais!

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Gerar revisores para este tutorial foram ren Giesenow Hilton e S Jacob Lauritsen. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](batch-deleting-cs.md)
> [Próximo](wrapping-database-modifications-within-a-transaction-vb.md)
