---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: "Uma visão geral de inserir, atualizar e excluir dados (c#) | Microsoft Docs"
author: rick-anderson
description: "Neste tutorial, veremos como mapear Insert () de um ObjectDataSource, Update (), e métodos de Delete () para os métodos de BLL classes, bem como a configu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: e483c37cc773a7255f18c26bc3609d68f71dff7d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Uma visão geral de inserir, atualizar e excluir dados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) ou [baixar PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> Neste tutorial, veremos como mapear Insert () de um ObjectDataSource, Update (), e métodos Delete () para os métodos de BLL classes, e como configurar os controles GridView, DetailsView e FormView para fornecer capacidades de modificação de dados.


## <a name="introduction"></a>Introdução

Sobre os vários tutoriais anteriores, examinamos como exibir dados em uma página ASP.NET usando os controles GridView, DetailsView e FormView. Esses controles simplesmente trabalham com dados fornecidos a eles. Normalmente, esses controles de acessam os dados com o uso de um controle de fonte de dados, como ObjectDataSource. Já vimos como ObjectDataSource atua como um proxy entre a página ASP.NET e os dados subjacentes. Quando precisa de um controle GridView exibir dados, ele chama seu ObjectDataSource `Select()` método, que por sua vez chama um método de nossos negócios lógica BLL (camada), que chama um método em apropriado dados acesso da camada (DAL) TableAdapter, que por sua vez envia um `SELECT` consulta ao banco de dados Northwind.

Lembre-se de que quando criamos os TableAdapters em DAL em [nosso primeiro tutorial](../introduction/creating-a-data-access-layer-cs.md), o Visual Studio adicionou automaticamente métodos para inserir, atualizar, e excluir dados de subjacente tabela de banco de dados. Além disso, em [criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-cs.md) projetamos métodos na BLL que chamou para esses métodos DAL de modificação de dados.

Além de seu `Select()` método, também tem o ObjectDataSource `Insert()`, `Update()`, e `Delete()` métodos. Como o `Select()` método, esses três métodos que podem ser mapeados para métodos em um objeto subjacente. Quando configurado para inserir, atualizar ou excluir dados, os controles GridView, DetailsView e FormView fornecem uma interface de usuário para modificar os dados subjacentes. Essa interface do usuário chama o `Insert()`, `Update()`, e `Delete()` métodos de ObjectDataSource, que, em seguida, chamar o objeto subjacente associada métodos (consulte a Figura 1).


[![Insert (), Update () e métodos de Delete () do ObjectDataSource servem como um Proxy em BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Figura 1**: do ObjectDataSource `Insert()`, `Update()`, e `Delete()` métodos servir como um Proxy para o BLL ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


Neste tutorial, veremos como mapear o ObjectDataSource `Insert()`, `Update()`, e `Delete()` métodos para métodos de classes na BLL, bem como configurar os controles GridView, DetailsView e FormView para fornecer a modificação de dados recursos.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Etapa 1: Criando o Insert, Update e Delete tutoriais páginas da Web

Antes de começarmos a explorar como inserir, atualizar e excluir dados, primeiro vamos criar as páginas ASP.NET em nosso projeto de site que vamos precisar para este tutorial e as várias Avançar. Comece adicionando uma nova pasta chamada `EditInsertDelete`. Em seguida, adicione as seguintes páginas do ASP.NET para a pasta, certificando-se de associar cada página com o `Site.master` página mestre:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Adicionar as páginas ASP.NET para os tutoriais relacionados a modificação de dados](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Figura 2**: adicionar as páginas ASP.NET para os tutoriais relacionados a modificação de dados


Como em outras pastas, `Default.aspx` no `EditInsertDelete` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicione este controle de usuário para `Default.aspx` arrastando-o no Gerenciador de soluções em modo de Design da página.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Figura 3**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


Por fim, adicione as páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após a formatação personalizada `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Após a atualização `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para a edição, inserção e exclusão tutoriais.


![O mapa do Site agora inclui entradas para a edição, inserção e exclusão tutoriais](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Figura 4**: O mapa de Site agora inclui entradas para a edição, inserção e exclusão tutoriais


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Etapa 2: Adicionando e configurando o controle ObjectDataSource

Desde o GridView, DetailsView e FormView cada diferem em suas capacidades de modificação de dados e layout, vamos examinar cada um deles individualmente. Em vez de ter cada controle usando seu próprio ObjectDataSource, no entanto, vamos apenas criar um único ObjectDataSource que podem ser compartilhados por todos os exemplos de controle de três.

Abra o `Basics.aspx` página, arraste um ObjectDataSource da caixa de ferramentas para o Designer e clique no link configurar fonte de dados de marca inteligente. Desde que o `ProductsBLL` é a única classe BLL que fornece a edição, inserção e exclusão de métodos, configuram o ObjectDataSource para usar essa classe.


[![Configurar o ObjectDataSource para usar a classe ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Figura 5**: configurar o ObjectDataSource para usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


Na próxima tela, podemos especificar quais métodos do `ProductsBLL` classe são mapeados para o ObjectDataSource `Select()`, `Insert()`, `Update()`, e `Delete()` selecionando a guia apropriada e escolhendo o método na lista suspensa. Figura 6, que deve parecer familiar agora, mapeia o ObjectDataSource `Select()` método para o `ProductsBLL` da classe `GetProducts()` método. O `Insert()`, `Update()`, e `Delete()` métodos podem ser configurados, selecionando a guia apropriada da lista na parte superior.


[![ObjectDataSource retornar todos os produtos](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Figura 6**: tem o ObjectDataSource retornar todos os produtos ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


Guias de figuras 7, 8 e 9 para mostrar o ObjectDataSource UPDATE, INSERT e DELETE. Esses guias de configuração para que o `Insert()`, `Update()`, e `Delete()` invocar métodos de `ProductsBLL` da classe `UpdateProduct`, `AddProduct`, e `DeleteProduct` métodos, respectivamente.


[![Mapear o método de Update () do ObjectDataSource para o ProductBLL método da classe UpdateProduct](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Figura 7**: mapear o ObjectDataSource `Update()` método para o `ProductBLL` da classe `UpdateProduct` método ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![Mapear o método de Insert () do ObjectDataSource para o ProductBLL método da classe AddProduct](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Figura 8**: mapear o ObjectDataSource `Insert()` método para o `ProductBLL` adicionar da classe `Product` método ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![Mapear o método de Delete () do ObjectDataSource para o ProductBLL método da classe DeleteProduct](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Figura 9**: mapear o ObjectDataSource `Delete()` método para o `ProductBLL` da classe `DeleteProduct` método ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


Você pode ter observado que as listas suspensas nas guias UPDATE, INSERT e DELETE já tinham esses métodos selecionados. Isso é graças ao nosso uso do `DataObjectMethodAttribute` que decora os métodos de `ProducstBLL`. Por exemplo, o método DeleteProduct tem a seguinte assinatura:


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

O `DataObjectMethodAttribute` atributo indica a finalidade de cada método, seja para selecionar, inserir, atualizar, ou excluindo e ou não é o valor padrão. Se você tiver omitido esses atributos durante a criação de suas classes BLL, você será necessário selecionar manualmente os métodos de atualização, inserir e excluir guias.

Depois de garantir que as `ProductsBLL` métodos são mapeados para o ObjectDataSource `Insert()`, `Update()`, e `Delete()` métodos, clique em Concluir para concluir o assistente.

## <a name="examining-the-objectdatasources-markup"></a>Examinando a marcação do ObjectDataSource

Depois de configurar o ObjectDataSource por meio de seu assistente, vá para o modo de exibição de fonte para examinar a marcação declarativa gerada. O `<asp:ObjectDataSource>` marca Especifica o objeto subjacente e os métodos para chamar. Além disso, há `DeleteParameters`, `UpdateParameters`, e `InsertParameters` que mapeiam para os parâmetros de entrada para o `ProductsBLL` da classe `AddProduct`, `UpdateProduct`, e `DeleteProduct` métodos:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

ObjectDataSource inclui um parâmetro para cada um dos parâmetros de entrada para seus métodos associados, como uma lista de `SelectParameter` s está presente quando o ObjectDataSource está configurado para chamar um método select que espera um parâmetro de entrada (como `GetProductsByCategoryID(categoryID)`). Como você verá em breve, valores para essas `DeleteParameters`, `UpdateParameters`, e `InsertParameters` são definidas automaticamente o GridView, DetailsView e FormView antes de invocar o ObjectDataSource `Insert()`, `Update()`, ou `Delete()` método. Esses valores também podem ser definidos programaticamente, conforme necessário, como abordaremos em um tutorial futuras.

Um efeito colateral de usar o Assistente para configurar o ObjectDataSource é que o Visual Studio define o [propriedade OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) para `original_{0}`. O valor da propriedade é usado para incluir os valores originais dos dados que está sendo editados e é útil em dois cenários:

- Se, ao editar um registro, os usuários são capazes de alterar o valor de chave primária. Nesse caso, o novo valor de chave primária e o valor de chave primária original devem ser fornecidos para que o registro com o valor de chave primária original pode ser encontrado e ter seu valor atualizado adequadamente.
- Ao usar a simultaneidade otimista. Otimista de simultaneidade é uma técnica para garantir que dois usuários simultâneos não substitua as alterações uma da outra e é o tópico para obter um tutorial futuras.

O `OldValuesParameterFormatString` propriedade indica o nome dos parâmetros de entrada na atualização do objeto subjacente e os métodos de exclusão para os valores originais. Discutiremos essa propriedade e sua finalidade mais detalhadamente quando exploraremos simultaneidade otimista. Eu exibi-la agora, no entanto, como métodos do nosso BLL não espera que os valores originais e, portanto, é importante que podemos remover essa propriedade. Deixar o `OldValuesParameterFormatString` propriedade definida como algo diferente do padrão (`{0}`) causará um erro quando um controle da Web de dados tentar invocar o ObjectDataSource `Update()` ou `Delete()` métodos porque será ObjectDataSource tentar passar em ambos os `UpdateParameters` ou `DeleteParameters` especificado, bem como parâmetros de valor original.

Se isso não é muito claro neste momento, não se preocupe, vamos examinar essa propriedade e seu utilitário em um tutorial futuras. Por enquanto, apenas certifique-se remova esta declaração de propriedade totalmente da sintaxe declarativa ou defina o valor para o valor padrão ({0}).

> [!NOTE]
> Se você simplesmente limpar o `OldValuesParameterFormatString` valor da propriedade na janela de propriedades na exibição de Design, a propriedade ainda existirá na sintaxe declarativa, mas a ser definido como uma cadeia de caracteres vazia. Isso, infelizmente, ainda resultará no mesmo problema discutido acima. Portanto, remova a propriedade completamente da sintaxe declarativa ou, na janela Propriedades, defina o valor como o padrão, `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Etapa 3: Adicionar um controle de Web de dados e configurá-lo para modificação de dados

Depois que o ObjectDataSource foi adicionado à página e configurado, estamos prontos para adicionar dados de controles da Web para a página para exibir os dados e fornece um meio para o usuário final para modificá-la. Vamos examinar o GridView, DetailsView e FormView separadamente, pois esses controles da Web de dados diferem em suas capacidades de modificação de dados e configuração.

Como veremos no restante deste artigo, adicionando a edição básica, inserir e excluir suporte por meio do GridView, DetailsView e FormView controla é realmente simples, como verificação de algumas das caixas de seleção. Há muitos sutilezas e casos de borda do mundo real que tornam fornecendo essa funcionalidade mais envolvida que simplesmente aponte e clique em. Neste tutorial, no entanto, se concentra apenas em fornecer capacidades de modificação de dados simples. Tutoriais futuros examinará preocupações certamente surgirão em uma configuração do mundo real.

## <a name="deleting-data-from-the-gridview"></a>Excluindo dados de GridView

Iniciar arrastando um GridView da caixa de ferramentas para o Designer. Em seguida, associe o ObjectDataSource a GridView selecionando-o na lista suspensa na marca inteligente do GridView. Neste ponto, declarativo do GridView será:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Associação de GridView para ObjectDataSource por meio de sua marca inteligente tem dois benefícios:

- BoundFields e CheckBoxFields são criados automaticamente para cada um dos campos retornados por ObjectDataSource. Além disso, as propriedades de BoundField e do CheckBoxField são definidas com base nos metadados do campo subjacente. Por exemplo, o `ProductID`, `CategoryName`, e `SupplierName` campos são marcados como somente leitura no `ProductsDataTable` e, portanto, não deve ser atualizável ao editar. Para acomodar essa, essas BoundFields [propriedades ReadOnly](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) são definidos como `true`.
- O [propriedade DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) é atribuído para o campo de chave primário do objeto subjacente. Isso é essencial quando usando GridView para edição ou exclusão de dados, já que essa propriedade indica que o campo (ou conjunto de campos) exclusivo que identifica cada registro. Para obter mais informações sobre o `DataKeyNames` propriedade, consulte o [mestre/detalhes usando um GridView mestre selecionável com um Details DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) tutorial.

Enquanto o GridView pode ser vinculado a ObjectDataSource através da janela Propriedades ou sintaxe declarativa, isso exige que você adicione manualmente o BoundField apropriado e `DataKeyNames` marcação.

O controle GridView fornece suporte interno para o nível de linha de edição e exclusão. Configurando um GridView para oferecer suporte a exclusão adiciona uma coluna de botões de exclusão. Quando o usuário final clicar no botão Excluir para uma linha específica, um postback tem lugar e GridView executa as seguintes etapas:

1. O ObjectDataSource `DeleteParameters` valores atribuídos
2. O ObjectDataSource `Delete()` método é chamado, excluindo o registro especificado
3. GridView reconecta próprio a ObjectDataSource invocando seu `Select()` método

Os valores atribuídos ao `DeleteParameters` são os valores da `DataKeyNames` campo (s) para a linha cuja exclusão foi clicada. Portanto, é vital que um GridView `DataKeyNames` propriedade ser definida corretamente. Se ele estiver ausente, o `DeleteParameters` será atribuído um `null` valor na etapa 1, que por sua vez não resultará em qualquer registros excluídos na etapa 2.

> [!NOTE]
> O `DataKeys` coleção é armazenada no estado do controle GridView s, que significa que o `DataKeys` valores serão lembrados em postback, mesmo se o estado de exibição do GridView s foi desabilitado. No entanto, é muito importante que o estado de exibição seja habilitado para GridViews que oferecem suporte a edição ou exclusão (o comportamento padrão). Se você definir o GridView s `EnableViewState` propriedade `false`, a edição e exclusão de comportamento funciona bem para um único usuário, mas se houver excluindo dados de usuários simultâneos, existe a possibilidade de que esses usuários simultâneos podem acidentalmente excluir ou edição de registros que elas não foi a intenção. Consulte minha entrada de blog, [Aviso: problema de simultaneidade com ASP.NET 2.0 GridViews/DetailsView/FormViews que suporte a edição e/ou excluindo e cujo estado de exibição é desativado](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), para obter mais informações.


Esse aviso mesmo também se aplica a DetailsViews e FormViews.

Para adicionar recursos excluindo um GridView, vá para a marca inteligente e marque a caixa de seleção Habilitar a exclusão.


![Verifique a habilitar a exclusão de caixa de seleção](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Figura 10**: verificar a habilitar a exclusão de caixa de seleção


Verificando a caixa de seleção Habilitar a exclusão de marca inteligente adiciona um CommandField GridView. O CommandField renderiza uma coluna em GridView com botões para executar uma ou mais das seguintes tarefas: selecionando um registro, edição de um registro e exclusão de um registro. Vimos anteriormente CommandField em ação com a seleção de registros de [mestre/detalhes usando um GridView mestre selecionável com um Details DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) tutorial.

O CommandField contém um número de `ShowXButton` propriedades que indicam qual série de botões é exibido no CommandField. Marcando a caixa de seleção Habilitar excluindo um CommandField cujo `ShowDeleteButton` é de propriedade `true` foi adicionado à coleção de colunas do GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

Neste ponto, acredite ou não, terminamos adicionando suporte excluindo a GridView! Como mostra a Figura 11, quando visitar esta página por meio de um navegador de uma coluna de botões de exclusão está presente.


[![O CommandField adiciona uma coluna de botões de exclusão](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Figura 11**: O CommandField adiciona uma coluna de excluir botões ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


Se você criando neste tutorial desde o início por conta própria, durante o teste nesta página clicando no botão Excluir gerará uma exceção. Continue lendo para saber mais sobre por que essas exceções foram geradas e como corrigi-los.

> [!NOTE]
> Se você estiver acompanhando usando o download que acompanha este tutorial, esses problemas tem já foram considerados. No entanto, recomendo que você leia os detalhes abaixo para ajudar a identificar problemas que podem surgir e soluções alternativas adequadas.


Se, durante a tentativa de excluir um produto, você receberá uma exceção que cuja mensagem é semelhante a "*ObjectDataSource 'ObjectDataSource1' não foi possível encontrar um método não genérico 'DeleteProduct' que tem parâmetros: productID, original\_ ProductID*, "você provavelmente esqueceu de remover o `OldValuesParameterFormatString` propriedade ObjectDataSource. Com o `OldValuesParameterFormatString` propriedade for especificada, o ObjectDataSource tenta transmitir em ambos os `productID` e `original_ProductID` parâmetros de entrada de `DeleteProduct` método. `DeleteProduct`, no entanto, só aceita um único parâmetro de entrada, portanto, a exceção. Removendo o `OldValuesParameterFormatString` propriedade (ou defini-la como `{0}`) instrui o ObjectDataSource não tente passar o parâmetro de entrada original.


[![Certifique-se de que a propriedade OldValuesParameterFormatString foi limpo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Figura 12**: Certifique-se de que o `OldValuesParameterFormatString` propriedade tiver sido desmarcada Out ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


Mesmo se você tiver removido o `OldValuesParameterFormatString` propriedade, você ainda receberá uma exceção ao tentar excluir um produto com a mensagem: "*a instrução DELETE entrou em conflito com a restrição de referência ' FK\_ordem\_detalhes \_Produtos*. " O banco de dados Northwind contém uma restrição de chave estrangeira entre a `Order Details` e `Products` tabela, o que significa que um produto não pode ser excluído do sistema se há um ou mais registros no `Order Details` tabela. Uma vez que todos os produtos no banco de dados Northwind tem pelo menos um registro `Order Details`, não é possível excluir todos os produtos até, primeiro exclua os registros de detalhes do pedido associado do produto.


[![Uma restrição Foreign Key impede a exclusão de produtos](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Figura 13**: uma restrição de chave estrangeira impede a exclusão de produtos ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


Para nosso tutorial, vamos apenas exclua todos os registros da `Order Details` tabela. Em um aplicativo do mundo real é precisa em:

- Ter outra tela para gerenciar informações de detalhes do pedido
- Aumentar o `DeleteProduct` método para incluir a lógica para excluir detalhes do pedido do produto especificado
- Modificar a consulta SQL usada pelo TableAdapter para incluir a exclusão de detalhes do pedido do produto especificado

Vamos apenas exclua todos os registros da `Order Details` tabela contornar a restrição de chave estrangeira. Vá para o Gerenciador de servidores no Visual Studio, clique com botão direito no `NORTHWND.MDF` nó e escolha nova consulta. Em seguida, na janela de consulta, execute a seguinte instrução SQL:`DELETE FROM [Order Details]`


[![Excluir todos os registros da tabela de detalhes do pedido](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Figura 14**: excluir todos os registros da `Order Details` tabela ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


Depois de limpar o `Order Details` tabela clicando no botão Excluir excluirá o produto sem erro. Se clicar no botão de exclusão não exclui o produto, verifique se que o GridView `DataKeyNames` propriedade é definida para o campo de chave primária (`ProductID`).

> [!NOTE]
> Ao clicar no botão de exclusão um postback tem lugar e o registro será excluído. Isso pode ser perigoso, pois é fácil acidentalmente, clique no botão de exclusão da linha errado. Um tutorial futuro veremos como adicionar uma confirmação do lado do cliente quando a exclusão de um registro.


## <a name="editing-data-with-the-gridview"></a>Editando dados com o GridView

Junto com a exclusão, o controle GridView também fornece suporte interno de edição de nível de linha. Configurando um GridView para oferecer suporte à edição adiciona uma coluna de botões de edição. Da perspectiva do usuário final, clicando em causas de botão de edição da linha que a linha se torne editável, transformando as células em caixas de texto que contém os valores existentes e substituir os botões de cancelamento e botão de edição com a atualização. Depois de fazer as alterações desejadas, o usuário final pode clicar no botão de atualização para confirmar as alterações ou o botão Cancelar para descartá-las. Ambos os casos, depois de clicar em atualizar ou em Cancelar GridView retorna ao estado previamente edição.

Em nossa perspectiva como um desenvolvedor de página, quando o usuário final clicar no botão Editar para uma linha específica, um postback tem lugar e GridView executa as seguintes etapas:

1. O GridView `EditItemIndex` propriedade é atribuída para o índice da linha cuja edição foi clicada
2. GridView reconecta próprio a ObjectDataSource invocando seu `Select()` método
3. O índice da linha que corresponde a `EditItemIndex` é renderizado no "modo de edição." Nesse modo, o botão de edição é substituído por botões atualizar e Cancelar e BoundFields cujo `ReadOnly` propriedades são False (padrão) são renderizados como controles de caixa de texto Web cujo `Text` propriedades são atribuídas aos valores de campos de dados.

Neste ponto, a marcação é retornada no navegador, permitindo que o usuário final fazer quaisquer alterações nos dados da linha. Quando o usuário clica no botão de atualização, ocorre um postback e GridView executa as seguintes etapas:

1. O ObjectDataSource `UpdateParameters` valor (es) é atribuídos valores inseridos pelo usuário final em interface de edição do GridView
2. O ObjectDataSource `Update()` método é chamado, atualizando o registro especificado
3. GridView reconecta próprio a ObjectDataSource invocando seu `Select()` método

Valores de chave primária atribuídos para o `UpdateParameters` na etapa 1 vêm os valores especificados no `DataKeyNames` propriedade, enquanto os valores de chave primária não são provenientes o texto nos controles da Web de caixa de texto para a linha editada. Como com exclusão, é essencial que um GridView `DataKeyNames` propriedade ser definida corretamente. Se ele estiver ausente, o `UpdateParameters` valor de chave primária será atribuído um `null` valor na etapa 1, que por sua vez não resultará em qualquer atualizado registros na etapa 2.

Funcionalidade de edição pode ser ativada, simplesmente marcando a caixa de seleção Habilitar edição na marca inteligente do GridView.


![Verifique a habilitar edição de caixa de seleção](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Figura 15**: verificar a habilitar edição de caixa de seleção


Verificando a caixa de seleção Habilitar edição adicionará um CommandField (se necessário) e defina seu `ShowEditButton` propriedade `true`. Como vimos anteriormente, o CommandField contém um número de `ShowXButton` propriedades que indicam qual série de botões é exibido no CommandField. Verificando a caixa de seleção Habilitar edição adiciona o `ShowEditButton` propriedade para o CommandField existente:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Isso é tudo que é necessário para adicionar suporte a edição rudimentar. Como mostra a Figure16, a interface de edição é bastante crua cada BoundField cujo `ReadOnly` está definida como `false` (o padrão) é renderizado como uma caixa de texto. Isso inclui campos como `CategoryID` e `SupplierID`, que são chaves para outras tabelas.


[![Botão de edição do clicando em Chai s exibe a linha no modo de edição](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Figura 16**: s Chai clicando em Editar botão exibe a linha em modo de edição ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


Além de solicitar aos usuários editar valores de chave estrangeira diretamente, a interface da interface edição estiver ausente das seguintes maneiras:

- Se o usuário insere um `CategoryID` ou `SupplierID` que não existe no banco de dados, o `UPDATE` violar uma restrição de chave estrangeira, fazendo com que uma exceção seja gerada.
- A interface de edição não inclui nenhuma validação. Se você não fornecer um valor obrigatório (como `ProductName`), ou insira um valor de cadeia de caracteres onde um valor numérico é esperado (por exemplo, digitar "Muito bem!" para o `UnitPrice` caixa de texto), uma exceção será lançada. Um tutorial futuras examinará como adicionar controles de validação para a interface de usuário de edição.
- Atualmente, *todos os* campos de produto que não são somente leitura devem ser incluídos em GridView. Se tivéssemos remover um campo de GridView, digamos que `UnitPrice`, ao atualizar os dados de GridView não definiria o `UnitPrice` `UpdateParameters` valor, que alteraria o registro de banco de dados `UnitPrice` para um `NULL` valor. Da mesma forma, se um campo obrigatório, como `ProductName`, é removido do GridView, a atualização falhará com o mesmo "*'ProductName' da coluna não permite nulls*" exceção mencionados acima.
- A formatação de interface edição deixa muito a desejar. O `UnitPrice` é mostrada com quatro pontos decimais. O ideal é a `CategoryID` e `SupplierID` valores conteria DropDownLists que lista as categorias e os fornecedores no sistema.

Essas são todas as falhas que teremos dinâmicas com agora, mas serão abordados nas tutoriais futuros.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Inserindo, edição e exclusão de dados com o DetailsView

Como vimos nos tutoriais anteriores, o controle DetailsView exibe um registro de cada vez e, como o GridView, permite a edição e exclusão do registro exibido no momento. Os dois a experiência do usuário final com editar e excluir itens de um DetailsView e o fluxo de trabalho do lado do ASP.NET é idêntica de GridView. Qual DetailsView é diferente do GridView é que ele também fornece suporte interno a inserção.

Para demonstrar os recursos de modificação de dados do GridView, comece adicionando um DetailsView para o `Basics.aspx` página acima GridView existente e associá-lo a ObjectDataSource existente por meio de marca inteligente de DetailsView. Em seguida, desmarque-out de DetailsView `Height` e `Width` propriedades e verifique se a opção habilitar paginação de marca inteligente. Para habilitar a edição, inserção e exclusão de suporte, marque as caixas de seleção Permitir edição, permitir inserção e habilitar a exclusão da marca inteligente.


![Configurar o DetailsView para suporte de edição, inserção e exclusão](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Figura 17**: configurar o DetailsView para suporte de edição, inserção e exclusão


Como com o GridView, adicionando editando, inserindo ou excluindo suporte adiciona um CommandField a DetailsView, como a seguir mostra a sintaxe declarativa:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Observe que para o CommandField DetailsView aparece no final da coleção de colunas por padrão. Como os campos de DetailsView são renderizados como linhas, o CommandField aparece como uma linha com Insert, editar e excluir botões na parte inferior de DetailsView.


[![Configurar o DetailsView para suporte de edição, inserção e exclusão](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Figura 18**: configurar o DetailsView para suporte a edição, inserção e exclusão ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


Clicando no botão Excluir inicia a mesma sequência de eventos como em GridView: um postback; seguido de DetailsView preencher seu ObjectDataSource `DeleteParameters` com base no `DataKeyNames` valores; e concluído com uma chamada seu ObjectDataSource `Delete()` método, que, na verdade, remove o produto do banco de dados. Edição em DetailsView também funciona de modo idêntico de GridView.

Para inserir, o usuário final é apresentado com um novo botão que, quando clicado, renderiza o DetailsView no "modo de inserção". Com o "modo de inserção" no novo botão é substituído por botões Insert e Cancelar e apenas esses BoundFields cujo `InsertVisible` está definida como `true` (o padrão) são exibidos. Esses campos de dados identificados como campos de incremento automático, como `ProductID`, têm seus [propriedade InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) definida como `false` ao associar o DetailsView à fonte de dados por meio da marca inteligente.

Ao associar a uma fonte de dados a um DetailsView por meio de marca inteligente, o Visual Studio define o `InsertVisible` propriedade `false` apenas para campos de incremento automático. Como campos somente leitura, `CategoryName` e `SupplierName`, será exibido na interface do usuário "modo de inserção", a menos que seus `InsertVisible` propriedade é definida explicitamente como `false`. Reserve um tempo para definir esses dois campos `InsertVisible` propriedades `false`, por meio da sintaxe declarativa de DetailsView ou por meio de editar campos link na marca inteligente. Figura 19 mostra a configuração de `InsertVisible` propriedades `false` clicando em Editar campos link.


[![A Northwind Traders agora oferece Acme chá](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Figura 19**: Northwind Traders agora oferece Acme chá ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


Depois de definir o `InsertVisible` propriedades, de modo a `Basics.aspx` página em um navegador e clique no botão novo. Figura 20 mostra DetailsView ao adicionar uma novo Bebidas, chá Acme, nossa linha de produtos.


[![A Northwind Traders agora oferece Acme chá](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Figura 20**: Northwind Traders agora oferece Acme chá ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


Depois de inserir os detalhes de chá Acme e clicar no botão de inserção, um postback tem lugar e o novo registro é adicionado para o `Products` tabela de banco de dados. Desde que este DetailsView lista os produtos em ordem com o qual existe na tabela de banco de dados, podemos deve página para o último produto para ver o novo produto.


[![Detalhes de chá Acme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Figura 21**: detalhes de chá Acme ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> O DetailsView [CurrentMode propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) indica que a interface que está sendo exibida, e pode ser um dos seguintes valores: `Edit`, `Insert`, ou `ReadOnly`. O [DefaultMode propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) indica o modo de DetailsView retorna para depois de uma edição ou inserir foi concluído e é útil para exibir um DetailsView permanentemente em edição ou modo de inserção.


O ponto e clique em Inserir e editar recursos de DetailsView tem as mesmas limitações que o GridView: o usuário deve inserir existente `CategoryID` e `SupplierID` valores por meio de uma caixa de texto; a falta de interface qualquer lógica de validação; tudo campos de produto que não permitem `NULL` valores ou não tem um padrão especificado no nível de banco de dados de valor deve ser incluído na interface de inserção e assim por diante.

As técnicas examinaremos para estender e aprimorar interface de edição do GridView no futuro artigos podem ser aplicados para o controle de DetailsView edição e inserção de interfaces bem.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Usando o FormView para uma Interface de usuário de modificação de dados mais flexível

O FormView oferece suporte interno para inserção, edição e exclusão de dados, mas porque ele usa modelos em vez de campos não há um local para adicionar o BoundFields ou o CommandField usadas pelos controles GridView e DetailsView para fornecer os dados interface de modificação. Em vez disso, essa interface os controles da Web para coletar o usuário ao adicionar um novo item de entrada ou editando uma existente com o novo, editar, excluir, inserir, atualizar e Cancelar botões deve ser adicionado manualmente para os modelos apropriados. Felizmente, o Visual Studio criará automaticamente a interface necessária ao associar o FormView a uma fonte de dados através da lista suspensa na sua marca inteligente.

Para ilustrar as técnicas, comece adicionando um FormView para o `Basics.aspx` página e de marca inteligente de FormView associá-lo a ObjectDataSource já criado. Isso irá gerar um `EditItemTemplate`, `InsertItemTemplate`, e `ItemTemplate` para FormView com controles de caixa de texto Web para coleta de entrada e controles da Web de botão do usuário para o novo, editar, excluir, inserir, atualizar e Cancelar botões. Do Além disso, o FormView `DataKeyNames` propriedade é definida para o campo de chave primária (`ProductID`) do objeto retornado por ObjectDataSource. Por fim, verifique a opção habilitar paginação em marca inteligente de FormView.

A seguir mostra a marcação declarativa para o FormView `ItemTemplate` depois FormView foi associado a ObjectDataSource. Por padrão, cada campo de valor não booleano de produto está vinculado a `Text` propriedade de um controle de rótulo Web durante cada campo de valor booliano (`Discontinued`) está associado ao `Checked` propriedade de um controle de caixa de seleção Web desabilitado. Para os botões novo, editar e excluir disparar um determinado comportamento de FormView quando clicado, é fundamental que suas `CommandName` valores definidos como `New`, `Edit`, e `Delete`, respectivamente.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

A Figura 22 mostra o FormView `ItemTemplate` quando visualizada através de um navegador. Cada campo de produto é listado com os botões novo, editar e excluir na parte inferior.


[![ItemTemplate FormView padrão lista cada campo produto junto com novo, editar e excluir botões](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**A Figura 22**: O FormView de padrão `ItemTemplate` lista cada produto campo junto com o novo, editar e excluir botões ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


Como com o GridView e DetailsView, clicando no botão excluir ou qualquer botão, LinkButton ou ImageButton cujo `CommandName` propriedade é definida como Delete faz com que um postback, preenche o ObjectDataSource `DeleteParameters` com base em FormView `DataKeyNames`valor e, em seguida, invoca o ObjectDataSource `Delete()` método.

Ao clicar no botão Editar um postback tem lugar e os dados é vinculada outra vez para o `EditItemTemplate`, que é responsável por processar a interface de edição. Essa interface inclui os controles da Web para editar dados junto com os botões Cancelar e de atualização. O padrão `EditItemTemplate` gerado pelo Visual Studio contém um rótulo para os campos de incremento automático (`ProductID`), uma caixa de texto para cada campo de valor não booleano e uma caixa de seleção para cada campo de valor booliano. Esse comportamento é muito semelhante do BoundFields gerado automaticamente nos controles GridView e DetailsView.

> [!NOTE]
> Um pequeno problema a geração de FormView automática do `EditItemTemplate` é que ele renderiza TextBox Web controles para os campos que são somente para leitura, como `CategoryName` e `SupplierName`. Vamos ver como isso em consideração em breve.


Controles de caixa de texto no `EditItemTemplate` ter suas `Text` propriedade associada ao valor do seu campo de dados correspondente usando *databinding bidirecional*. Associação de dados bidirecional, indicado por `<%# Bind("dataField") %>`, executa databinding ambos ao associar dados ao modelo e ao preencher os parâmetros do ObjectDataSource para inserir ou editar registros. Ou seja, quando o usuário clica no botão de edição do `ItemTemplate`, o `Bind()` método retorna o valor do campo de dados especificado. Depois que o usuário faz suas alterações e clica em atualizar, os valores de postback que correspondem aos campos de dados especificados usando `Bind()` são aplicadas a ObjectDataSource `UpdateParameters`. Como alternativa, a associação de dados unidirecional, indicado por `<%# Eval("dataField") %>`, somente recupera os valores de campo de dados ao associar dados ao modelo e faz *não* retornar os valores inseridos pelo usuário para os parâmetros da fonte de dados em um postback.

A seguinte marcação declarativa mostra o FormView `EditItemTemplate`. Observe que o `Bind()` método é usado aqui a sintaxe de associação de dados e os controles da Web de botão Cancelar e de atualização com seus `CommandName` propriedades definidas adequadamente.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Nosso `EditItemTemplate`, neste ponto, fará com que uma exceção seja lançada se podemos tentar usá-lo. O problema é que o `CategoryName` e `SupplierName` campos são renderizados como controles de caixa de texto Web no `EditItemTemplate`. É necessário alterar essas caixas de texto para rótulos ou removê-los completamente. Vamos simplesmente exclua-os completamente do `EditItemTemplate`.

Figura 23 mostra FormView em um navegador após ser clicado no botão Editar para Chai. Observe que o `SupplierName` e `CategoryName` campos mostrados na `ItemTemplate` não estão mais presentes, como acabou de removê-los do `EditItemTemplate`. Ao clicar no botão de atualização FormView passa para a mesma sequência de etapas que os controles GridView e DetailsView.


[![Por padrão, o EditItemTemplate mostra cada campo editável de produto como uma caixa de texto ou a caixa de seleção](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Figura 23**: por padrão, o `EditItemTemplate` mostra cada produto campo editável como uma caixa de texto ou a caixa de seleção ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


Quando o botão de inserção é clicado de FormView `ItemTemplate` um postback tem lugar. No entanto, nenhum dado está associado a FormView porque está sendo adicionado a um novo registro. O `InsertItemTemplate` interface inclui os controles da Web para adicionar um novo registro junto com os botões de inserção e Cancelar. O padrão `InsertItemTemplate` gerado pelo Visual Studio contém uma caixa de texto para cada campo de valor não boolianos e uma caixa de seleção para cada campo de valor booliano, semelhante a gerado automaticamente `EditItemTemplate`da interface. Os controles de caixa de texto têm seus `Text` propriedade associada ao valor do seu campo de dados correspondente usando associação de dados bidirecional.

A seguinte marcação declarativa mostra o FormView `InsertItemTemplate`. Observe que o `Bind()` método é usado aqui a sintaxe de associação de dados e os controles de inserção e da Web de botão Cancelar com seus `CommandName` propriedades definidas adequadamente.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Há um recurso com a geração automática de FormView do `InsertItemTemplate`. Especificamente, os controles da caixa de texto Web são criados até mesmo para os campos que são somente para leitura, como `CategoryName` e `SupplierName`. Como com o `EditItemTemplate`, é preciso remover essas caixas de texto do `InsertItemTemplate`.

Figura 24 mostra FormView em um navegador, ao adicionar um novo produto, café Acme. Observe que o `SupplierName` e `CategoryName` campos mostrados na `ItemTemplate` não estão mais presentes, como acabou de removê-los. Quando o botão de inserção é clicado continua o FormView por meio da mesma sequência de etapas que o controle DetailsView, adicionar um novo registro para o `Products` tabela. Figura 25 mostra detalhes do produto Acme café em FormView após ele ter sido inserido.


[![O InsertItemTemplate define a Interface de inserção de FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Figura 24**: O `InsertItemTemplate` define a Interface de inserção de FormView ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![Os detalhes do novo produto, Acme café, são exibidos em FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Figura 25**: detalhes do novo produto, Acme café, são exibidos no FormView ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


Por separar somente leitura, edição e inserindo interfaces três modelos separados, FormView permite um grau maior de controle sobre essas interfaces que o GridView e DetailsView.

> [!NOTE]
> Como o DetailsView, FormView `CurrentMode` propriedade indica a interface que está sendo exibida e sua `DefaultMode` propriedade indica o modo retorna o FormView para depois de uma edição ou inserção já foi concluída.


## <a name="summary"></a>Resumo

Neste tutorial, examinamos os fundamentos da inserção, edição e exclusão de dados usando o GridView, DetailsView e FormView. Todos os três controles fornecem algum nível de capacidades de modificação de dados internos que podem ser utilizados sem escrever uma única linha de código na página ASP.NET graças os controles da Web de dados e o ObjectDataSource. No entanto, simples aponte e clique em técnicas de renderização frail bastante simples interface de usuário de modificação de dados. Para fornecer a validação, inserir valores programáticos, normalmente lidar com exceções, personalizar a interface do usuário e assim por diante, vamos precisar contar com muitas das técnicas que serão discutidos em vários tutoriais os próximos.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Avançar](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
