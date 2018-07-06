---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Uma visão geral de inserção, atualização e exclusão de dados (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como mapear Insert () um ObjectDataSource, Update (), e classes de métodos Delete () para os métodos da BLL, bem como para configu...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: abe42d01cc31ea46c97888f31d768ebfede64abd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821690"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Uma visão geral de inserção, atualização e exclusão de dados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) ou [baixar PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> Neste tutorial, veremos como mapear Insert () um ObjectDataSource, Update (), e classes de métodos Delete () para os métodos da BLL, bem como configurar os controles GridView, DetailsView e FormView para fornecer recursos de modificação de dados.


## <a name="introduction"></a>Introdução

Sobre os vários tutoriais anteriores, examinamos como exibir dados em uma página ASP.NET usando os controles GridView, DetailsView e FormView. Esses controles simplesmente trabalham com dados fornecidos para eles. Normalmente, esses controles de acessam os dados com o uso de um controle de fonte de dados, como o ObjectDataSource. Já vimos como o ObjectDataSource atua como um proxy entre a página ASP.NET e os dados subjacentes. Quando um GridView precisa exibir dados, ele invoca o ObjectDataSource `Select()` método, que por sua vez chama um método de nossos negócios lógica BLL (camada), que chama um método na apropriado dados da camada de acesso (DAL) TableAdapter, que por sua vez envia um `SELECT` consulta ao banco de dados Northwind.

Lembre-se de que quando criamos a TableAdapters em DAL na [nosso tutorial primeiro](../introduction/creating-a-data-access-layer-cs.md), o Visual Studio adicionou automaticamente métodos para inserir, atualizar, e a tabela de banco de dados de exclusão de dados de subjacente. Além disso, no [criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-cs.md) projetamos métodos na BLL que chamou para esses métodos DAL de modificação de dados.

Além seus `Select()` método, também tem o ObjectDataSource `Insert()`, `Update()`, e `Delete()` métodos. Como o `Select()` método, esses três métodos que podem ser mapeados para métodos em um objeto subjacente. Quando configurado para inserir, atualizar ou excluir dados, os controles GridView, DetailsView e FormView fornecem uma interface do usuário para modificar os dados subjacentes. Essa interface do usuário chama o `Insert()`, `Update()`, e `Delete()` métodos do ObjectDataSource, que, em seguida, chamar o objeto subjacente associada ao métodos (consulte a Figura 1).


[![Insert (), Update () e métodos Delete () do ObjectDataSource servem como um Proxy para a BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Figura 1**: O ObjectDataSource `Insert()`, `Update()`, e `Delete()` métodos servir como um Proxy para a BLL ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


Neste tutorial, veremos como mapear o ObjectDataSource `Insert()`, `Update()`, e `Delete()` métodos para métodos de classes na BLL, bem como configurar os controles GridView, DetailsView e FormView para fornecer a modificação de dados recursos.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Etapa 1: Criando o Insert, Update e Delete as páginas da Web de tutoriais

Antes de começarmos a explorar como inserir, atualizar e excluir dados, primeiro vamos criar as páginas do ASP.NET em nosso projeto de site que precisaremos para este tutorial e as próximas várias. Comece adicionando uma nova pasta chamada `EditInsertDelete`. Em seguida, adicione as seguintes páginas do ASP.NET para essa pasta, certificando-se de associar cada página com o `Site.master` página mestra:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Adicione as páginas do ASP.NET para que os tutoriais relacionados à modificação de dados](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Figura 2**: adicionar as páginas do ASP.NET para que os tutoriais relacionados à modificação de dados


Como em outras pastas `Default.aspx` no `EditInsertDelete` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicionar esse controle de usuário `Default.aspx` arrastando-no Gerenciador de soluções no modo de exibição de Design da página.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Figura 3**: Adicione o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


Por fim, adicione as páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a marcação a seguir após a formatação personalizada `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para a edição, inserindo e excluindo tutoriais.


![O mapa do Site agora inclui entradas para a edição, inserindo e excluindo tutoriais](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Figura 4**: O mapa do Site agora inclui entradas para a edição, inserindo e excluindo tutoriais


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Etapa 2: Adicionando e configurando o controle ObjectDataSource

Desde o GridView, DetailsView e FormView cada diferem em seus recursos de modificação de dados e layout, vamos examinar cada um deles individualmente. Em vez de ter cada controle usando seu próprio ObjectDataSource, no entanto, vamos criar um ObjectDataSource único que podem ser compartilhada por todos os exemplos de controle de três.

Abra o `Basics.aspx` página, arraste um ObjectDataSource da caixa de ferramentas para o Designer e clique no link configurar fonte de dados na sua marca inteligente. Uma vez que o `ProductsBLL` é a única classe BLL que fornece a edição, inserção e exclusão de métodos, configuram o ObjectDataSource para usar essa classe.


[![Configurar o ObjectDataSource para usar a classe ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Figura 5**: configurar o ObjectDataSource para usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


Na próxima tela, podemos especificar quais métodos do `ProductsBLL` classe são mapeados para o ObjectDataSource `Select()`, `Insert()`, `Update()`, e `Delete()` selecionando a guia apropriada e escolhendo o método na lista suspensa. Figura 6, que deve ser familiar agora, mapeia o ObjectDataSource `Select()` método para o `ProductsBLL` da classe `GetProducts()` método. O `Insert()`, `Update()`, e `Delete()` métodos podem ser configurados, selecionando a guia apropriada na lista na parte superior.


[![O ObjectDataSource retornar todos os produtos](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Figura 6**: tem o ObjectDataSource retornar todos os produtos ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


Guias de figuras 7, 8 e 9 para mostrar o ObjectDataSource UPDATE, INSERT e DELETE. Configurar essas guias, de modo que o `Insert()`, `Update()`, e `Delete()` invocam métodos de `ProductsBLL` da classe `UpdateProduct`, `AddProduct`, e `DeleteProduct` métodos, respectivamente.


[![Mapear o método de Update () do ObjectDataSource para o ProductBLL método da classe UpdateProduct](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Figura 7**: mapeie o ObjectDataSource `Update()` método para o `ProductBLL` da classe `UpdateProduct` método ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![Mapear o método de Insert () do ObjectDataSource para o ProductBLL método da classe AddProduct](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Figura 8**: mapeie o ObjectDataSource `Insert()` método para o `ProductBLL` Add da classe `Product` método ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![Mapear o método de Delete () do ObjectDataSource para o ProductBLL método da classe DeleteProduct](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Figura 9**: mapeie o ObjectDataSource `Delete()` método para o `ProductBLL` da classe `DeleteProduct` método ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


Você talvez tenha observado que as listas suspensas nas guias de atualização, inserção e exclusão já tinham esses métodos selecionados. Isso é graças ao nosso uso do `DataObjectMethodAttribute` que decora os métodos de `ProducstBLL`. Por exemplo, o método DeleteProduct tem a seguinte assinatura:


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

O `DataObjectMethodAttribute` atributo indica a finalidade de cada método, seja para selecionar, inserir, atualizar, ou excluindo e ou não é o valor padrão. Se você omitir esses atributos ao criar suas classes BLL, você vai precisar para selecionar manualmente os métodos de atualização, inserir e excluir guias.

Depois de garantir que o apropriada `ProductsBLL` métodos são mapeados para o ObjectDataSource `Insert()`, `Update()`, e `Delete()` métodos, clique em Concluir para concluir o assistente.

## <a name="examining-the-objectdatasources-markup"></a>Examinando a marcação do ObjectDataSource

Depois de configurar o ObjectDataSource por meio de seu assistente, vá para a exibição da fonte para examinar a marcação declarativa gerada. O `<asp:ObjectDataSource>` marca Especifica o objeto subjacente e os métodos para invocar. Além disso, há `DeleteParameters`, `UpdateParameters`, e `InsertParameters` que são mapeados para os parâmetros de entrada para o `ProductsBLL` da classe `AddProduct`, `UpdateProduct`, e `DeleteProduct` métodos:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

O ObjectDataSource inclui um parâmetro para cada um dos parâmetros de entrada para seus métodos associados, assim como uma lista de `SelectParameter` s está presente quando o ObjectDataSource está configurado para chamar um método de seleção que espera um parâmetro de entrada (como `GetProductsByCategoryID(categoryID)`). Como você verá em breve, os valores para essas `DeleteParameters`, `UpdateParameters`, e `InsertParameters` são definidos automaticamente pelo GridView, DetailsView e FormView antes de invocar o ObjectDataSource `Insert()`, `Update()`, ou `Delete()` método. Esses valores também podem ser definidos programaticamente, conforme necessário, como discutiremos em um tutorial futuro.

Um efeito colateral de usar o Assistente para configurar o ObjectDataSource é que o Visual Studio define o [propriedade OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) para `original_{0}`. Esse valor da propriedade é usado para incluir os valores originais dos dados que está sendo editados e é útil em dois cenários:

- Se, ao editar um registro, os usuários são capazes de alterar o valor de chave primária. Nesse caso, o novo valor de chave primária e o valor de chave primária original devem ser fornecidos para que o registro com o valor de chave primária original pode ser encontrado e ter seu valor atualizado adequadamente.
- Ao usar a simultaneidade otimista. Otimista de simultaneidade é uma técnica para garantir que dois usuários simultâneos não substitua as alterações uma da outra e é o tópico para obter um tutorial futuro.

O `OldValuesParameterFormatString` propriedade indica o nome dos parâmetros de entrada em métodos de exclusão para os valores originais e atualização do objeto subjacente. Abordaremos essa propriedade e sua finalidade mais detalhadamente quando podemos explorar a simultaneidade otimista. Posso ativá-lo agora, no entanto, porque os métodos do nosso BLL não espera que os valores originais e, portanto, é importante que podemos remover essa propriedade. Deixando o `OldValuesParameterFormatString` propriedade definida como algo diferente do padrão (`{0}`) causará um erro quando um controle da Web de dados tenta invocar o ObjectDataSource `Update()` ou `Delete()` métodos porque será o ObjectDataSource Tente passar em ambas as `UpdateParameters` ou `DeleteParameters` especificado, bem como os parâmetros de valor original.

Se isso não é tão claro nesse momento, não se preocupe, vamos examinar essa propriedade e sua utilidade em um tutorial futuro. Por enquanto, apenas certifique-se remover esta declaração de propriedade inteiramente da sintaxe declarativa ou defina o valor como o valor padrão ({0}).

> [!NOTE]
> Se você simplesmente limpar o `OldValuesParameterFormatString` valor da propriedade na janela Propriedades, na exibição de Design, a propriedade ainda existirá na sintaxe declarativa, mas ser definido como uma cadeia de caracteres vazia. Isso, infelizmente, ainda resultará no mesmo problema discutido acima. Portanto, remova a propriedade completamente da sintaxe declarativa ou, na janela Propriedades, defina o valor como o padrão, `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Etapa 3: Adicionando um controle de Web de dados e configurá-lo para modificação de dados

Depois que o ObjectDataSource foi adicionado à página e configurado, estamos prontos para adicionar dados de controles da Web para a página para exibir os dados e fornecem um meio para o usuário final para modificá-lo. Vamos examinar o GridView, DetailsView e FormView separadamente, como esses controles da Web de dados diferem em seus recursos de modificação de dados e configuração.

Como veremos no restante deste artigo, adicionando muito básico de edição, inserindo e excluindo o suporte por meio de GridView, DetailsView e FormView controla é realmente tão simples quanto verificar algumas das caixas de seleção. Há muitas sutilezas e casos de borda no mundo real que dificultam o fornecimento de tal funcionalidade mais envolvido do que simplesmente aponte e clique em. Neste tutorial, no entanto, se concentra exclusivamente em fornecer recursos de modificação de dados simples. Tutoriais futuros irá examinar os problemas que certamente surgirão em uma configuração do mundo real.

## <a name="deleting-data-from-the-gridview"></a>Exclusão de dados do GridView

Comece arrastando um GridView da caixa de ferramentas para o Designer. Em seguida, associe o ObjectDataSource para o GridView, selecionando-o na lista suspensa na marca inteligente do GridView. Neste ponto, marcação declarativa do GridView será:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Associando o GridView a ObjectDataSource por meio de sua marca inteligente tem dois benefícios:

- BoundFields e CheckBoxFields são criados automaticamente para cada um dos campos retornados pelo ObjectDataSource. Além disso, o BoundField e do CheckBoxField propriedades são definidas com base nos metadados do campo subjacente. Por exemplo, o `ProductID`, `CategoryName`, e `SupplierName` campos são marcados como somente leitura no `ProductsDataTable` e, portanto, não deve ser atualizável ao editar. Para acomodar esse, essas BoundFields [propriedades ReadOnly](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) são definidos como `true`.
- O [propriedade DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) é atribuído para o campo de chave primário do objeto subjacente. Isso é essencial quando usando GridView para edição ou exclusão de dados, como essa propriedade indica o campo (ou conjunto de campos) exclusivo que identifica cada registro. Para obter mais informações sobre o `DataKeyNames` propriedade, voltar para o [mestre/detalhes usando um GridView mestre selecionável com um Details DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) tutorial.

Embora GridView pode ser associado a ObjectDataSource através da janela Propriedades ou a sintaxe declarativa, isso exige que você adicionar manualmente o BoundField apropriado e `DataKeyNames` marcação.

O controle GridView fornece suporte interno para o nível de linha de edição e exclusão. Configurando um GridView para dar suporte à exclusão adiciona uma coluna de botões de exclusão. Quando o usuário final clica no botão Excluir para uma linha específica, um postback massacre e GridView executa as seguintes etapas:

1. O ObjectDataSource `DeleteParameters` recebem o valor (es)
2. O ObjectDataSource `Delete()` método é invocado, excluindo o registro especificado
3. O GridView associa novamente em si o ObjectDataSource, invocando seus `Select()` método

Os valores atribuídos para o `DeleteParameters` são os valores da `DataKeyNames` campo (s) para a linha cuja exclusão foi clicada. Portanto, é vital que um GridView `DataKeyNames` propriedade ser definida corretamente. Se ele estiver ausente, o `DeleteParameters` será atribuído um `null` valor na etapa 1, que por sua vez não resultará em qualquer registros excluídos na etapa 2.

> [!NOTE]
> O `DataKeys` coleção é armazenada no estado do controle GridView s, o que significa que o `DataKeys` valores serão lembrados em postback, mesmo se o estado de exibição GridView s foi desabilitado. No entanto, é muito importante que o estado de exibição permanece ativado para GridViews que dão suporte à edição ou exclusão (o comportamento padrão). Se você definir o s GridView `EnableViewState` propriedade para `false`, a edição e exclusão de comportamento funcionará bem para um único usuário, mas se houver usuários simultâneos, exclusão de dados, existe a possibilidade de ocorrência que esses usuários simultâneos podem ser acidentalmente excluir ou edição de registros que eles t pretende. Consulte minha postagem do blog [Aviso: problema de simultaneidade com o ASP.NET 2.0 GridViews/DetailsView/FormViews esse suporte de edição e/ou exclusão e cujo estado de exibição está desabilitado](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), para obter mais informações.


Esse aviso mesmo também se aplica a DetailsViews e FormViews.

Para adicionar recursos de exclusão em um GridView, vá para a sua marca inteligente e marque a caixa de seleção Habilitar exclusão.


![Verifique a habilitar a exclusão de caixa de seleção](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Figura 10**: verificar a habilitar a exclusão de caixa de seleção


Marcando a caixa de seleção Habilitar exclusão da marca inteligente adiciona um CommandField GridView. O CommandField renderiza uma coluna em GridView com botões para executar uma ou mais das seguintes tarefas: selecionando um registro, edição de um registro e exclusão de um registro. Vimos anteriormente o CommandField in action com a seleção de registros na [mestre/detalhes usando um GridView mestre selecionável com um Details DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) tutorial.

O CommandField contém um número de `ShowXButton` propriedades que indicam quais série de botões é exibido no CommandField. Marcando a caixa de seleção Habilitar exclusão um CommandField cujos `ShowDeleteButton` é de propriedade `true` foi adicionado à coleção de colunas do GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

Neste ponto, acredite ou não, concluímos a parte adicionando suporte a exclusão a GridView! Como mostra a Figura 11, quando visitar esta página por meio de um navegador, uma coluna de botões de exclusão está presente.


[![O CommandField adiciona uma coluna de botões de exclusão](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Figura 11**: O CommandField adiciona uma coluna de excluir botões ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


Se você já foi criando neste tutorial desde o início por conta própria, ao testar essa página clicando no botão Excluir gerarão uma exceção. Continue lendo para saber mais sobre por que essas exceções foram geradas e como corrigi-los.

> [!NOTE]
> Se você estiver acompanhando usando o download que acompanha este tutorial, esses problemas já foram considerados. No entanto, eu recomendo que você leia os detalhes listados abaixo para ajudar a identificar problemas que podem surgir e soluções alternativas adequadas.


Se, ao tentar excluir um produto, você receberá uma exceção que cuja mensagem é semelhante a "*ObjectDataSource 'ObjectDataSource1' não foi possível encontrar um método não genérico 'DeleteProduct' que tem parâmetros: productID, original\_ ProductID*, "você provavelmente se esqueceu de remover o `OldValuesParameterFormatString` propriedade do ObjectDataSource. Com o `OldValuesParameterFormatString` propriedade for especificada, o ObjectDataSource tenta passar em ambas `productID` e `original_ProductID` parâmetros de entrada para o `DeleteProduct` método. `DeleteProduct`, no entanto, só aceita um único parâmetro de entrada, portanto, a exceção. Removendo o `OldValuesParameterFormatString` propriedade (ou defini-lo como `{0}`) instrui o ObjectDataSource para não tentar passar no parâmetro de entrada original.


[![Certifique-se de que a propriedade OldValuesParameterFormatString foi limpo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Figura 12**: Certifique-se de que o `OldValuesParameterFormatString` propriedade tiver sido desmarcada Out ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


Mesmo se você tivesse removido o `OldValuesParameterFormatString` propriedade, você ainda receberá uma exceção ao tentar excluir um produto com a mensagem: "*a instrução DELETE entram em conflito com a restrição de referência ' FK\_ordem\_detalhes \_Produtos*. " O banco de dados Northwind contém uma restrição de chave estrangeira entre a `Order Details` e `Products` tabela, o que significa que um produto não pode ser excluído do sistema, se houver um ou mais registros para ele no `Order Details` tabela. Uma vez que todos os produtos no banco de dados Northwind não tem pelo menos um registro na `Order Details`, nós não podemos excluir todos os produtos até que o primeiro, podemos excluir registros de detalhes do pedido associado do produto.


[![Uma restrição Foreign Key impede a exclusão dos produtos](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Figura 13**: uma restrição de chave estrangeira proíbe a exclusão de produtos ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


Para nosso tutorial, vamos apenas excluir todos os registros da `Order Details` tabela. Em um aplicativo do mundo real seria preciso um:

- Ter outra tela para gerenciar informações de detalhes do pedido
- Aumentar o `DeleteProduct` método para incluir a lógica para excluir detalhes do pedido do produto especificado
- Modificar a consulta SQL usada pelo TableAdapter para incluir a exclusão dos detalhes de pedido do produto especificado

Vamos excluir todos os registros de apenas o `Order Details` tabela contornar a restrição de chave estrangeira. Vá para o Gerenciador de servidores no Visual Studio, clique com botão direito no `NORTHWND.MDF` nó e escolha a nova consulta. Em seguida, na janela de consulta, execute a seguinte instrução SQL: `DELETE FROM [Order Details]`


[![Exclua todos os registros da tabela de detalhes do pedido](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Figura 14**: excluir todos os registros dos `Order Details` tabela ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


Depois de limpar o `Order Details` tabela clicando no botão Excluir excluirá o produto sem erro. Se clicar no botão de exclusão não exclui o produto, verifique se que o GridView `DataKeyNames` estiver definida como o campo de chave primária (`ProductID`).

> [!NOTE]
> Ao clicar no botão Excluir massacre um postback e o registro é excluído. Isso pode ser perigoso, pois é fácil acidentalmente, clique no botão de exclusão da linha errado. Um tutorial futuro veremos como adicionar uma confirmação do lado do cliente ao excluir um registro.


## <a name="editing-data-with-the-gridview"></a>Edição de dados com o GridView

Junto com a exclusão, o controle GridView também fornece suporte interno de edição de nível de linha. Configurando um GridView para dar suporte à edição adiciona uma coluna de botões de edição. Da perspectiva do usuário final, clicando em causas de botão de edição de uma linha essa linha se torne editável, transformando as células em caixas de texto que contém os valores existentes e substituir os botões de cancelamento e o botão de edição com a atualização. Depois de fazer as alterações desejadas, o usuário final pode clicar no botão de atualização para confirmar as alterações ou o botão Cancelar para descartá-las. Em ambos os casos, depois de clicar em atualizar ou cancelar a retorna GridView para seu estado de edição previamente.

Nossa perspectiva como desenvolvedor de página, quando o usuário final clica no botão Editar para uma linha específica, um postback massacre e GridView executa as seguintes etapas:

1. O GridView `EditItemIndex` propriedade é atribuída ao índice da linha cuja edição foi clicada
2. O GridView associa novamente em si o ObjectDataSource, invocando seus `Select()` método
3. O índice da linha que corresponde a `EditItemIndex` é renderizado no "modo de edição". Nesse modo, o botão Editar é substituído por botões atualizar e Cancelar e BoundFields cujos `ReadOnly` propriedades forem False (padrão) são renderizados como controles de TextBox Web cuja `Text` propriedades são atribuídas a valores de campos de dados.

Neste ponto, a marcação é retornada ao navegador, permitindo que o usuário final fazer quaisquer alterações nos dados da linha. Quando o usuário clica no botão de atualização, ocorre um postback e GridView executa as seguintes etapas:

1. O ObjectDataSource `UpdateParameters` valor (es) é atribuídos os valores inseridos pelo usuário final em interface de edição do GridView
2. O ObjectDataSource `Update()` método é invocado, atualizando o registro especificado
3. O GridView associa novamente em si o ObjectDataSource, invocando seus `Select()` método

Os valores de chave primária atribuídos para o `UpdateParameters` na etapa 1 vêm os valores especificados no `DataKeyNames` propriedade, enquanto os valores de chave não primária vêm do texto em controles da Web de caixa de texto para a linha editada. Como a exclusão, é vital que um GridView `DataKeyNames` propriedade ser definida corretamente. Se ele estiver ausente, o `UpdateParameters` valor de chave primária será atribuído um `null` valor na etapa 1, que por sua vez não resultará em qualquer atualizado registros na etapa 2.

Pode ser ativada a funcionalidade de edição, simplesmente marcando a caixa de seleção Habilitar edição na marca inteligente do GridView.


![Verifique a habilitar a caixa de seleção de edição](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Figura 15**: verificar a habilitar a caixa de seleção de edição


Verificando a caixa de seleção Habilitar edição adicionará um CommandField (se necessário) e defina suas `ShowEditButton` propriedade para `true`. Como vimos anteriormente, o CommandField contém um número de `ShowXButton` propriedades que indicam quais série de botões é exibido no CommandField. Marcando a caixa de seleção Habilitar edição adiciona o `ShowEditButton` propriedade para o CommandField existente:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Que é tudo o que há para adicionar suporte de edição rudimentar. Como mostra a Figure16, a interface de edição é bastante crua cada BoundField cujos `ReadOnly` estiver definida como `false` (o padrão) é processado como uma caixa de texto. Isso inclui campos como `CategoryID` e `SupplierID`, que são chaves para outras tabelas.


[![Clicar em botão de edição do Chai s exibe a linha no modo de edição](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Figura 16**: s Chai clicando em Editar botão exibe a linha em modo de edição ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


Além de solicitar que os usuários editem valores de chave estrangeira diretamente, a interface da interface de edição é deficiente em das seguintes maneiras:

- Se o usuário insere um `CategoryID` ou `SupplierID` que não existe no banco de dados, o `UPDATE` violar uma restrição de chave estrangeira, fazendo com que uma exceção seja gerada.
- A interface de edição não inclui nenhuma validação. Se você não fornecer um valor obrigatório (como `ProductName`), ou insira um valor de cadeia de caracteres em que um valor numérico é esperado (por exemplo, inserindo "Muito!" para o `UnitPrice` caixa de texto), uma exceção será gerada. Um tutorial futuro irá examinar como adicionar controles de validação à interface do usuário de edição.
- No momento, *todos os* campos de produto que não são somente leitura devem ser incluídos no GridView. Se precisássemos remover um campo de GridView, digamos `UnitPrice`, ao atualizar os dados GridView não definiria o `UnitPrice` `UpdateParameters` valor, o que mudaria o registro de banco de dados `UnitPrice` para um `NULL` valor. Da mesma forma, se um campo obrigatório, tais como `ProductName`, é removido do GridView, a atualização falhará com o mesmo "*'ProductName' da coluna não permite nulls*" exceção mencionado acima.
- A formatação de interface edição deixa muito a desejar. O `UnitPrice` é exibida com quatro pontos decimais. O ideal é que o `CategoryID` e `SupplierID` valores conteria DropDownLists que lista as categorias e os fornecedores no sistema.

Essas são todas as limitações que teremos de conviver com por enquanto, mas será corrigido em tutoriais futuros.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Inserindo, editando e excluindo dados com DetailsView

Como vimos nos tutoriais anteriores, o controle DetailsView exibe um registro por vez e, como o GridView, permite a edição e exclusão do registro atualmente exibido. Os dois a experiência do usuário final com a edição e exclusão de itens de um DetailsView e o fluxo de trabalho do lado do ASP.NET é idêntica de GridView. Onde o DetailsView difere do GridView é que ele também fornece suporte interno a inserção.

Para demonstrar os recursos de modificação de dados do GridView, comece adicionando um DetailsView para o `Basics.aspx` página acima GridView existente e associá-lo a ObjectDataSource existente por meio de marca inteligente do ovládacího prvku DetailsView. Em seguida, desmarque out a DetailsView `Height` e `Width` propriedades e marque a opção habilitar paginação da marca inteligente. Para habilitar a edição, inserção e exclusão de suporte, basta marcar as caixas de seleção Habilitar edição, inserção de habilitar e habilitar exclusão na marca inteligente.


![Configurar DetailsView para suporte de edição, inserção e exclusão](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Figura 17**: configurar DetailsView para suporte de edição, inserção e exclusão


Como com o GridView, adicionando a edição, inserção ou exclusão de suporte adiciona um CommandField ao DetailsView, como a seguir mostra a sintaxe declarativa:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Observe que, para o CommandField DetailsView aparece no final da coleção de colunas por padrão. Uma vez que os campos de DetailsView são renderizados como linhas, o CommandField aparece como uma linha com Insert, editar e excluir os botões na parte inferior de DetailsView.


[![Configurar DetailsView para suporte de edição, inserção e exclusão](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Figura 18**: Configure DetailsView para suporte de edição, inserção e exclusão ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


A mesma sequência de eventos clicando no botão Excluir é iniciado assim como acontece com o GridView: um postback; seguido por DetailsView popular seu ObjectDataSource `DeleteParameters` com base nas `DataKeyNames` valores; e concluído com uma chamada seu ObjectDataSource `Delete()` método, que, na verdade, remove o produto do banco de dados. Editando em DetailsView também funciona de forma idêntica do GridView.

Para inserir, o usuário final é apresentado com um novo botão que, quando clicado, renderiza DetailsView no "modo de inserção". Com o "modo de inserção" botão novo é substituído por botões de inserção e Cancelar e apenas esses BoundFields cujos `InsertVisible` estiver definida como `true` (o padrão) são exibidos. Esses campos de dados identificados como campos de incremento automático, como `ProductID`, têm seus [propriedade InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) definido como `false` ao associar DetailsView à fonte de dados por meio da marca inteligente.

Ao associar uma fonte de dados a um DetailsView por meio da marca inteligente, o Visual Studio define o `InsertVisible` propriedade para `false` somente para campos de incremento automático. Campos somente leitura, como `CategoryName` e `SupplierName`, será exibido na interface do usuário de "modo de inserção", a menos que seus `InsertVisible` estiver explicitamente definida como `false`. Dedique uns momentos para definir esses dois campos `InsertVisible` propriedades a serem `false`, por meio de sintaxe declarativa de DetailsView ou editar campos conecte na marca inteligente. Figura 19 mostra a configuração de `InsertVisible` propriedades a serem `false` clicando nos campos de editar link.


[![Agora, o Northwind Traders oferece Acme chá](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Figura 19**: Northwind Traders agora oferece Acme chá ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


Depois de definir a `InsertVisible` propriedades, de modo a `Basics.aspx` página em um navegador e clique no botão novo. A Figura 20 mostra DetailsView ao adicionar uma novo Bebidas, Acme chá, nossa linha de produtos.


[![Agora, o Northwind Traders oferece Acme chá](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Figura 20**: Northwind Traders agora oferece Acme chá ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


Depois de inserir os detalhes para Acme chá e clicando no botão Inserir, um postback massacre e o novo registro é adicionado para o `Products` tabela de banco de dados. Uma vez que esse DetailsView lista os produtos em ordem com o qual existe na tabela de banco de dados, podemos deve página até o último produto para ver o novo produto.


[![Detalhes de chá Acme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Figura 21**: detalhes de chá Acme ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> O DetailsView [propriedade CurrentMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) indica a interface que está sendo exibida e pode ser um dos seguintes valores: `Edit`, `Insert`, ou `ReadOnly`. O [DefaultMode propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) indica o modo de DetailsView retorna ao após uma edição ou inserção foi concluído e é útil para exibir um DetailsView permanentemente na edição ou o modo de inserção.


O ponto e clique em Inserir e editar os recursos de DetailsView apresentam as mesmas limitações que o GridView: o usuário deve inserir existente `CategoryID` e `SupplierID` valores por meio de uma caixa de texto; o não tem interface qualquer lógica de validação; tudo campos de produto que não permitem `NULL` valores ou se não tiver um padrão especificado no nível de banco de dados de valor deve ser incluído na interface de inserção e assim por diante.

As técnicas podemos examinará em busca de extensão e o aprimoramento interface de edição do GridView no futuro artigos podem ser aplicados a do controle DetailsView edição e inserção de interfaces bem.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Usando o FormView para uma Interface de usuário de modificação de dados mais flexível

FormView oferece suporte interno para inserção, edição e exclusão de dados, mas porque ele usa os modelos em vez de campos não há um local para adicionar o BoundFields ou o CommandField usado pelos controles GridView e DetailsView para fornecer os dados interface de modificação. Em vez disso, essa interface, os controles da Web para coletar o usuário ao adicionar um novo item de entrada ou editar uma já existente, juntamente com o novo, editar, excluir, inserir, atualizar e Cancelar botões deve ser adicionado manualmente para os modelos apropriados. Felizmente, o Visual Studio criará automaticamente a interface necessária ao associar FormView a uma fonte de dados por meio da lista suspensa na sua marca inteligente.

Para ilustrar essas técnicas, comece adicionando um FormView para o `Basics.aspx` página e, na marca inteligente de FormView, associá-lo a ObjectDataSource já criado. Isso irá gerar uma `EditItemTemplate`, `InsertItemTemplate`, e `ItemTemplate` para FormView com controles de TextBox Web para a coleta de entrada do usuário e controles da Web de botão para o novo, editar, excluir, inserir, atualizar e Cancelar botões. Além disso, o do FormView `DataKeyNames` estiver definida como o campo de chave primária (`ProductID`) do objeto retornado por ObjectDataSource. Por fim, verifique a opção de habilitar a paginação na marca inteligente de FormView.

A seguir mostra a marcação declarativa para de FormView `ItemTemplate` depois FormView foi associado a ObjectDataSource. Por padrão, cada campo de valor não booleano de produto é vinculado à `Text` propriedade de um controle de rótulo Web enquanto cada campo de valor booliano (`Discontinued`) está associado a `Checked` propriedade de um controle de caixa de seleção Web desabilitado. A ordem dos botões de novo, editar e excluir disparar o comportamento específico FormView quando clicado, é fundamental que suas `CommandName` valores ser definida como `New`, `Edit`, e `Delete`, respectivamente.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

A Figura 22 mostra o FormView `ItemTemplate` quando visualizado por meio de um navegador. Cada campo de produto é listado com os botões de novo, editar e excluir na parte inferior.


[![O ItemTemplate de FormView padrão lista cada campo de produto, juntamente com novo, editar e excluir botões](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Figura 22**: O FormView de padrão `ItemTemplate` lista cada produto campo junto com o novo, editar e excluir botões ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


Como com GridView e DetailsView, clicando no botão excluir ou qualquer botão, LinkButton ou ImageButton cujos `CommandName` propriedade é definida como faz com que a exclusão de um postback, preenche o ObjectDataSource `DeleteParameters` com base em de FormView `DataKeyNames`valor e, em seguida, invoca o ObjectDataSource `Delete()` método.

Quando o botão Editar é clicado massacre um postback e os dados é ligado novamente para o `EditItemTemplate`, que é responsável por processar a interface de edição. Essa interface inclui os controles da Web para editar dados, juntamente com os botões Cancelar e de atualização. O padrão `EditItemTemplate` gerado pelo Visual Studio contém um rótulo para os campos de incremento automático (`ProductID`), uma caixa de texto para cada campo de valor não booliano e uma caixa de seleção para cada campo de valor booliano. Esse comportamento é muito semelhante ao BoundFields gerado automaticamente nos controles GridView e DetailsView.

> [!NOTE]
> Um pequeno problema com a geração automática de FormView do `EditItemTemplate` é que ele renderiza TextBox Web controles para os campos que são somente para leitura, como `CategoryName` e `SupplierName`. Veremos como levar isso em conta em breve.


Controles de caixa de texto na `EditItemTemplate` ter seus `Text` propriedade associada ao valor de seu campo de dados correspondente usando *vinculação de dados bidirecional*. Associação de dados bidirecional, indicada por `<%# Bind("dataField") %>`, executa databinding os dois ao associar dados ao modelo e ao popular os parâmetros do ObjectDataSource para inserção ou edição de registros. Ou seja, quando o usuário clica no botão de edição do `ItemTemplate`, o `Bind()` método retorna o valor do campo de dados especificado. Depois que o usuário faz com que suas alterações e clica em atualizar, os valores postada de volta que correspondem aos campos de dados especificados usando `Bind()` são aplicados a ObjectDataSource `UpdateParameters`. Como alternativa, a associação de dados unidirecional, indicada por `<%# Eval("dataField") %>`, somente recupera os valores de campo de dados ao associar dados ao modelo e faz *não* retornar os valores inseridos pelo usuário para os parâmetros da fonte de dados em um postback.

A seguinte marcação declarativa mostra o FormView `EditItemTemplate`. Observe que o `Bind()` método é usado na sintaxe de associação de dados aqui e que os controles da Web de botão Cancelar e de atualização têm seus `CommandName` propriedades definidas adequadamente.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Nosso `EditItemTemplate`, neste ponto, fará com que uma exceção seja gerada se podemos tentar usá-lo. O problema é que o `CategoryName` e `SupplierName` campos são renderizados como controles de TextBox Web no `EditItemTemplate`. Também precisamos alterar essas caixas de texto em rótulos ou removê-los completamente. Vamos simplesmente exclui-los totalmente a partir de `EditItemTemplate`.

Figura 23 mostra FormView em um navegador após ser clicado no botão Editar para Chai. Observe que o `SupplierName` e `CategoryName` campos mostrados na `ItemTemplate` não estão mais presentes, como acabou de removê-las a partir de `EditItemTemplate`. Quando o botão de atualização é clicado FormView continua com a mesma sequência de etapas que os controles GridView e DetailsView.


[![Por padrão, o EditItemTemplate mostra cada campo editável de produto como uma caixa de texto ou a caixa de seleção](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Figura 23**: por padrão, o `EditItemTemplate` mostra cada produto campo editável como uma caixa de texto ou a caixa de seleção ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


Quando o botão de inserção é clicado de FormView `ItemTemplate` massacre um postback. No entanto, nenhum dado é associado ao FormView porque está sendo adicionado a um novo registro. O `InsertItemTemplate` interface inclui os controles da Web para adicionar um novo registro, juntamente com os botões de inserção e Cancelar. O padrão `InsertItemTemplate` gerado pelo Visual Studio contém uma caixa de texto para cada campo de valor não booliano e uma caixa de seleção para cada campo de valor booliano, semelhante ao geradas automaticamente `EditItemTemplate`da interface. Os controles de caixa de texto têm seus `Text` propriedade associada ao valor do campo de dados correspondente usando vinculação de dados bidirecional.

A seguinte marcação declarativa mostra o FormView `InsertItemTemplate`. Observe que o `Bind()` método é usado na sintaxe de associação de dados aqui e que têm os controles de inserção e da Web de botão Cancelar seus `CommandName` propriedades definidas adequadamente.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Há uma sutileza aqui com geração automática de FormView do `InsertItemTemplate`. Especificamente, os controles da Web de caixa de texto são criados até mesmo para os campos que são somente para leitura, como `CategoryName` e `SupplierName`. Como com o `EditItemTemplate`, é preciso remover essas caixas de texto do `InsertItemTemplate`.

Figura 24 mostra FormView em um navegador ao adicionar um novo produto, café Acme. Observe que o `SupplierName` e `CategoryName` campos mostrados na `ItemTemplate` não estão mais presentes, como acabou de removê-los. Quando o botão de inserção é clicado o continua FormView por meio da mesma sequência de etapas que o controle DetailsView, adicionar um novo registro para o `Products` tabela. Figura 25 mostra detalhes do produto de café Acme FormView após ele ter sido inserido.


[![O InsertItemTemplate determina a Interface de inserção de FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Figura 24**: O `InsertItemTemplate` determina a Interface de inserção de FormView ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![Os detalhes para o novo produto, Acme café, são exibidos no FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Figura 25**: os detalhes para o novo produto, Acme café, são exibidos no FormView ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


Separando a somente leitura, edição e inserção interfaces em três modelos separados, FormView permite um grau de controle em relação a essas interfaces mais que o GridView e DetailsView.

> [!NOTE]
> Como o DetailsView, FormView `CurrentMode` propriedade indica a interface que está sendo exibida e seu `DefaultMode` propriedade indica o modo retorna o FormView para após uma edição ou inserção foi concluída.


## <a name="summary"></a>Resumo

Neste tutorial, examinamos as Noções básicas de inserção, editando e excluindo dados usando o GridView, DetailsView e FormView. Todos os três controles fornecem algum nível de recursos de modificação de dados internos que podem ser utilizadas sem escrever uma única linha de código na página ASP.NET, graças aos controles da Web de dados e o ObjectDataSource. No entanto, o simples aponte e clique em técnicas de renderização frail bastante ingênua interface de usuário de modificação de dados. Para fornecer validação, injetar valores programáticos, lidar normalmente com exceções, personalizar a interface do usuário e assim por diante, vamos precisar contar com muitas das técnicas que será discutido sobre os próxima vários tutoriais.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Avançar](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
