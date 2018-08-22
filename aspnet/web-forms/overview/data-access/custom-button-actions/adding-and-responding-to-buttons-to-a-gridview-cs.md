---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
title: Adicionar e responder a botões em um GridView (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos como adicionar botões personalizados, tanto para um modelo para os campos de um controle GridView ou DetailsView. Em particular, vamos bui...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 128fdb5f-4c5e-42b5-b485-f3aee90a8e38
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
msc.type: authoredcontent
ms.openlocfilehash: 3a081b1633e7762560aea68500f5bd614e4fb5a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824773"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-c"></a>Adicionar e responder a botões em um GridView (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_CS.exe) ou [baixar PDF](adding-and-responding-to-buttons-to-a-gridview-cs/_static/datatutorial28cs1.pdf)

> Neste tutorial, examinaremos como adicionar botões personalizados, tanto para um modelo para os campos de um controle GridView ou DetailsView. Em particular, vamos criar uma interface que tem um FormView que permite ao usuário a percorrer os fornecedores.


## <a name="introduction"></a>Introdução

Embora muitos cenários de relatório envolvem acesso somente leitura aos dados do relatório, não é incomum para relatórios incluem a capacidade de executar ações com base nos dados exibidos. Normalmente isso envolvido a adição de um controle de botão, LinkButton ou ImageButton Web com cada registro exibido no relatório que, quando clicado, faz com que um postback e invoca um código do lado do servidor. Editar e excluir os dados em uma base de registro por registro são o exemplo mais comum. Na verdade, como vimos começando com o [visão geral de inserindo, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, edição e exclusão é tão comum que os controles GridView, DetailsView e FormView podem oferecer suporte a essa funcionalidade sem o necessário para escrever uma única linha de código.

Além para editar e excluir os botões, o GridView, DetailsView e FormView controles também podem incluir botões, botões de link ou ImageButtons que, quando clicado, executar alguma lógica personalizada do lado do servidor. Neste tutorial, examinaremos como adicionar botões personalizados, tanto para um modelo para os campos de um controle GridView ou DetailsView. Em particular, vamos criar uma interface que tem um FormView que permite ao usuário a percorrer os fornecedores. Para um determinado fornecedor, FormView mostrará informações sobre o fornecedor, junto com um controle da Web de botão que, se clicada, marcará todos os seus produtos associados como descontinuado. Além disso, um GridView lista esses produtos fornecidos pelo fornecedor selecionado, com cada linha que contém o preço de aumentar e botões de preço de desconto que, se clicada, aumentar ou reduzir o produto `UnitPrice` em 10% (veja a Figura 1).


[![FormView e GridView contenham botões que executam ações personalizadas](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image1.png)

**Figura 1**: os dois FormView e GridView contém botões que executam ações personalizadas ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Etapa 1: Adicionar as páginas da Web Tutorial do botão

Antes, vamos examinar como adicionar um botões personalizados, primeiro vamos criar as páginas do ASP.NET em nosso projeto de site que precisaremos para este tutorial. Comece adicionando uma nova pasta chamada `CustomButtons`. Em seguida, adicione as duas páginas do ASP.NET a seguir para essa pasta, certificando-se de associar cada página com o `Site.master` página mestra:

- `Default.aspx`
- `CustomButtons.aspx`


![Adicione as páginas do ASP.NET para que os tutoriais de relacionadas de botões personalizados](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image4.png)

**Figura 2**: adicionar as páginas do ASP.NET para que os tutoriais de relacionadas de botões personalizados


Como em outras pastas `Default.aspx` no `CustomButtons` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicionar esse controle de usuário `Default.aspx` arrastando-no Gerenciador de soluções no modo de exibição de Design da página.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image5.png)

**Figura 3**: Adicione o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image7.png))


Por fim, adicione as páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após a paginação e classificação `<siteMapNode>`:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para a edição, inserindo e excluindo tutoriais.


![O mapa do Site agora inclui a entrada para o Tutorial de botões personalizados](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image8.png)

**Figura 4**: O mapa do Site agora inclui a entrada para o Tutorial de botões personalizados


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Etapa 2: Adicionando um FormView que lista os fornecedores

Vamos começar com este tutorial adicionando FormView que lista os fornecedores. Conforme discutido na introdução, este FormView permitirá que o usuário à página por meio de fornecedores, mostrando os produtos fornecidos pelo fornecedor em um GridView. Além disso, esse FormView incluirá um botão que, quando clicado, marcará todos os produtos do fornecedor como descontinuado. Antes de nós mesmos estamos envolvem com a adição de botão personalizado para FormView, primeiro vamos simplesmente criar FormView para que ele exiba as informações do fornecedor.

Comece abrindo o `CustomButtons.aspx` página o `CustomButtons` pasta. Adicionar um FormView para a página arrastando-o na caixa de ferramentas para o Designer e o conjunto de seu `ID` propriedade para `Suppliers`. Na marca inteligente de FormView, optar por criar um novo ObjectDataSource chamado `SuppliersDataSource`.


[![Criar um novo ObjectDataSource chamado SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image9.png)

**Figura 5**: criar um novo ObjectDataSource nomeado `SuppliersDataSource` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image11.png))


Configurar esse novo ObjectDataSource, de modo que ele consulta a partir de `SuppliersBLL` da classe `GetSuppliers()` método (veja a Figura 6). Como esse FormView não fornece uma interface para atualizar as informações do fornecedor, selecione opção na lista suspensa na guia de atualização (nenhum).


[![Configurar a fonte de dados para usar a classe SuppliersBLL s GetSuppliers() método](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image12.png)

**Figura 6**: configurar a fonte de dados para usar o `SuppliersBLL` da classe `GetSuppliers()` método ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image14.png))


Depois de configurar o ObjectDataSource, o Visual Studio gerará um `InsertItemTemplate`, `EditItemTemplate`, e `ItemTemplate` para FormView. Remover o `InsertItemTemplate` e `EditItemTemplate` e modifique o `ItemTemplate` para que ele exiba apenas do fornecedor da empresa nome e número de telefone. Por fim, ativar o suporte de paginação para FormView marcando a caixa de seleção Habilitar paginação na marca inteligente de (ou definindo sua `AllowPaging` propriedade para `True`). Após essas alterações marcação declarativa de sua página deve ser semelhante ao seguinte:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample2.aspx)]

Figura 7 mostra a página de CustomButtons.aspx quando visualizado por meio de um navegador.


[![Lista de FormView CompanyName e campos de telefone do fornecedor selecionado no momento](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image15.png)

**Figura 7**: A lista de FormView a `CompanyName` e `Phone` campos de fornecedor atualmente selecionado ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-suppliers-products"></a>Etapa 3: Adicionando um GridView que lista os produtos do fornecedor selecionado

Antes de adicionar o botão interromper todos os produtos para o modelo de FormView, vamos adicionar primeiro um GridView abaixo FormView que lista os produtos fornecidos pelo fornecedor selecionado. Para fazer isso, adicione um GridView à página, defina suas `ID` propriedade para `SuppliersProducts`, e adicione um novo ObjectDataSource chamado `SuppliersProductsDataSource`.


[![Criar um novo ObjectDataSource chamado SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image18.png)

**Figura 8**: criar um novo ObjectDataSource nomeado `SuppliersProductsDataSource` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image20.png))


Configurar este ObjectDataSource para usar a classe de ProductsBLL `GetProductsBySupplierID(supplierID)` método (veja a Figura 9). Enquanto esse GridView permitirá para preço de um produto a ser ajustado, ele não usará internos, editar ou excluir recursos de GridView. Portanto, podemos definir a lista suspensa como (nenhum) para o ObjectDataSource do UPDATE, INSERT e exclui as guias.


[![Configurar a fonte de dados para usar a classe ProductsBLL s GetProductsBySupplierID(supplierID) método](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image21.png)

**Figura 9**: configurar a fonte de dados para usar o `ProductsBLL` da classe `GetProductsBySupplierID(supplierID)` método ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image23.png))


Uma vez que o `GetProductsBySupplierID(supplierID)` método aceita um parâmetro de entrada, o assistente ObjectDataSource nos solicita a origem desse valor de parâmetro. Para passar o `SupplierID` FormView de valor, defina a lista de lista suspensa de origem do parâmetro para o controle e a lista suspensa de ControlID para `Suppliers` (a ID do FormView criado na etapa 2).


[![Indicar que o supplierID parâmetro deva vir do controle FormView de fornecedores](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image24.png)

**Figura 10**: indica que o *`supplierID`* parâmetro deve vir do `Suppliers` controle FormView ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image26.png))


Depois de concluir o assistente ObjectDataSource, o GridView conterá uma BoundField ou CheckBoxField para cada um dos campos de dados do produto. Vamos reduzir isso para mostrar apenas o `ProductName` e `UnitPrice` BoundFields juntamente com o `Discontinued` CheckBoxField; Além disso, vamos formatar o `UnitPrice` BoundField, de modo que seu texto é formatado como uma moeda. O GridView e `SuppliersProductsDataSource` marcação declarativa do ObjectDataSource deve ser semelhante à seguinte marcação:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample3.aspx)]

Neste ponto, nosso tutorial exibe um relatório de detalhes/mestre, permitindo que o usuário escolher um fornecedor de FormView na parte superior e para exibir os produtos fornecidos por esse fornecedor por meio de GridView na parte inferior. Figura 11 mostra uma captura de tela desta página, ao selecionar o fornecedor Tokyo Traders do FormView.


[![Os produtos do fornecedor selecionado são exibidos em GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image27.png)

**Figura 11**: produtos do fornecedor selecionado o são exibidos no GridView ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Etapa 4: Criar métodos BLL e DAL para interromper todos os produtos para um fornecedor

Antes de podermos adicionar um botão para FormView que, quando clicado, interrompe todos os produtos do fornecedor, primeiro precisamos adicionar um método para o DAL e BLL que executa essa ação. Em particular, esse método será chamado `DiscontinueAllProductsForSupplier(supplierID)`. Quando o botão de FormView é clicado, podemos irá invocar esse método na camada de lógica de negócios, passando do fornecedor selecionado `SupplierID`; a BLL, em seguida, chamará para baixo para o método de camada de acesso de dados correspondente, que emitirá um `UPDATE` instrução para o banco de dados que deixou de produtos do fornecedor especificado.

Como fizemos nos nossos tutoriais anteriores, vamos usar uma abordagem de baixo para cima, começando com o método DAL, em seguida, o método BLL, de criação e, finalmente, a implementação da funcionalidade na página ASP.NET. Abra o `Northwind.xsd` digitado o conjunto de dados no `App_Code/DAL` pasta e adicione um novo método para o `ProductsTableAdapter` (com o botão direito no `ProductsTableAdapter` e escolha Add Query). Isso abrirá o Assistente de configuração de consulta do TableAdapter, que nos conduz pelo processo de adicionar o novo método. Iniciar, indicando que o nosso método DAL usará uma instrução de SQL ad hoc.


[![Crie o método DAL usando uma instrução de SQL Ad Hoc](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image30.png)

**Figura 12**: criar o método DAL usando uma instrução de SQL Ad Hoc ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image32.png))


Em seguida, o assistente solicita conosco sobre que tipo de consulta para criar. Uma vez que o `DiscontinueAllProductsForSupplier(supplierID)` método será necessário atualizar o `Products` tabela de banco de dados, definindo o `Discontinued` campo como 1 para todos os produtos fornecidos por especificado *`supplierID`*, precisamos criar uma consulta que atualiza os dados.


[![Escolha o tipo de consulta de atualização](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image33.png)

**Figura 13**: escolha o tipo de consulta de atualização ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image35.png))


A próxima tela do assistente fornece o TableAdapter existente `UPDATE` instrução, que atualiza cada um dos campos definidos na `Products` DataTable. Substitua este texto de consulta com a seguinte instrução:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample4.sql)]

Depois de inserir essa consulta e clicar em Avançar, a última tela do assistente pede para uso do nome do novo método `DiscontinueAllProductsForSupplier`. Conclua o assistente clicando no botão Concluir. Ao retornar para o Designer de conjunto de dados, você deverá ver um novo método na `ProductsTableAdapter` chamado `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Nomeie o novo DiscontinueAllProductsForSupplier DAL método](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image36.png)

**Figura 14**: Nomeie o novo método DAL `DiscontinueAllProductsForSupplier` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image38.png))


Com o `DiscontinueAllProductsForSupplier(supplierID)` método criado na camada de acesso a dados, nossa próxima tarefa é criar o `DiscontinueAllProductsForSupplier(supplierID)` método na camada de lógica de negócios. Para fazer isso, abra o `ProductsBLL` arquivo de classe e adicione o seguinte:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample5.cs)]

Esse método simplesmente chama para baixo até a `DiscontinueAllProductsForSupplier(supplierID)` método em que a DAL, passando fornecido *`supplierID`* valor do parâmetro. Se houver quaisquer regras de negócio que são permitidos apenas os produtos do fornecedor ser interrompido em determinadas circunstâncias, essas regras devem ser implementadas aqui, na BLL.

> [!NOTE]
> Ao contrário o `UpdateProduct` sobrecargas na `ProductsBLL` classe, o `DiscontinueAllProductsForSupplier(supplierID)` assinatura de método não inclui o `DataObjectMethodAttribute` atributo (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Isso impede o `DiscontinueAllProductsForSupplier(supplierID)` método na lista suspensa do Assistente de configurar fonte de dados do ObjectDataSource na guia de atualização. Eu ve omitido este atributo porque estamos chamará o `DiscontinueAllProductsForSupplier(supplierID)` método diretamente de um manipulador de eventos em nossa página do ASP.NET.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Etapa 5: Adicionando um interromper todos os botões de produtos para FormView

Com o `DiscontinueAllProductsForSupplier(supplierID)` método na BLL e DAL completa, a etapa final para adicionar a capacidade de interromper todos os produtos para o fornecedor selecionado é adicionar um controle da Web de botão para de FormView `ItemTemplate`. Vamos adicionar o botão abaixo do número de telefone do fornecedor com o texto do botão, interromper todos os produtos e uma `ID` valor da propriedade `DiscontinueAllProductsForSupplier`. Você pode adicionar esse controle da Web de botão por meio do Designer clicando no link Editar modelos na marca inteligente de FormView (consulte a Figura 15), ou diretamente por meio da sintaxe declarativa.


[![Adicionar um interromper todos os controle de Web de botão de produtos para o ItemTemplate de FormView s](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image39.png)

**Figura 15**: Adicione um descontinuar todos os produtos Web controle Button para o FormView `ItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image41.png))


Quando o botão é clicado, uma visita do usuário massacre de página, um postback e de FormView [ `ItemCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) é acionado. Para executar código personalizado em resposta a esse botão sendo clicado, podemos criar um manipulador de eventos para este evento. Entenda, no entanto, que o `ItemCommand` evento será disparado sempre que *qualquer* controle de botão, LinkButton ou ImageButton Web é clicado em FormView. Isso significa que, quando o usuário move de uma página para outra em FormView, o `ItemCommand` evento é acionado; mesmo quando o usuário clica em novo, editar ou excluir em um FormView que dá suporte à inserção, atualização ou exclusão.

Uma vez que o `ItemCommand` é acionado independentemente de qual botão é clicado, no caso de manipulador precisamos de uma maneira de determinar se clicou no botão interromper todos os produtos ou se ele tiver sido algum outro botão. Para fazer isso, podemos definir o controle de botão Web `CommandName` propriedade para algum valor de identificação. Quando o botão é clicado, isso `CommandName` valor é passado para o `ItemCommand` manipulador de eventos, possibilitando a determinar se o botão interromper todos os produtos era o botão clicado. Definir interromper todos os produtos do botão `CommandName` propriedade DiscontinueProducts.

Por fim, vamos usar uma caixa de diálogo de confirmação do lado do cliente para garantir que o usuário realmente deseja descontinuar o produtos do fornecedor selecionado. Como vimos na [' adicionando confirmação do lado do cliente quando excluindo](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs.md) tutorial, isso pode ser feito com um pouco de JavaScript. Em particular, defina a propriedade de OnClientClick do controle da Web de botão `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Depois de fazer essas alterações, a sintaxe declarativa de FormView deve ser semelhante ao seguinte:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample6.aspx)]

Em seguida, crie um manipulador de eventos para o FormView `ItemCommand` eventos. Nesse manipulador de eventos, precisamos determinar primeiro se clicou no botão interromper todos os produtos. Se assim, desejamos criar uma instância das `ProductsBLL` de classe e chamar seu `DiscontinueAllProductsForSupplier(supplierID)` método, passando o `SupplierID` do FormView selecionado:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample7.cs)]

Observe que o `SupplierID` do fornecedor selecionado atual FormView podem ser acessados usando o FormView [ `SelectedValue` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). O `SelectedValue` propriedade retorna o valor do registro que está sendo exibido no FormView os primeiros dados da chave. O FormView [ `DataKeyNames` propriedade](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), que indica os dados de campos do qual os dados de valores de chave são extraídos de, foi definida automaticamente como `SupplierID` pelo Visual Studio ao associar o ObjectDataSource a FormView novamente na etapa 2.

Com o `ItemCommand` manipulador de eventos criado, reserve um tempo para testar a página. Navegue até o Cooperativa de Quesos supplier 'Las Cabras' (ele é o quinto fornecedor FormView para mim). Este fornecedor oferece dois produtos, Queso Cabrales e Queso Pastora de La Manchego, sendo que ambos *não* descontinuado.

Imagine que a Alemanha Cooperativa Quesos 'Las Cabras' tiver saído do negócio e, portanto, seus produtos estão para ser interrompido. Clique em de descontinuar o botão de todos os produtos. Isso exibirá a caixa de diálogo de confirmação do lado do cliente caixa (veja a Figura 16).


[![Cooperativa de Quesos Las Cabras fornece dois produtos ativos](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image42.png)

**Figura 16**: Cooperativa de Quesos Las Cabras fornece dois produtos ativos ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image44.png))


Se você clicar em Okey na caixa de diálogo de confirmação do lado do cliente, o envio do formulário continuará, causando um postback em que o FormView `ItemCommand` evento será disparado. O manipulador de eventos que criamos, em seguida, executará, invocando o `DiscontinueAllProductsForSupplier(supplierID)` método e descontinuar o Queso Cabrales e Queso Manchego La Pastora produtos.

Se você tiver desabilitado o estado de exibição do GridView, GridView é que está sendo ligado novamente o armazenamento de dados subjacente em cada postagem e, portanto, imediatamente será atualizada para refletir que esses dois produtos agora são descontinuados (consulte a Figura 17). Se, no entanto, você não tiver desabilitado o estado de exibição do GridView, você precisará manualmente depois de fazer essa alteração, associar novamente os dados para o GridView. Para fazer isso, basta fazer uma chamada para o GridView `DataBind()` método imediatamente depois de invocar o `DiscontinueAllProductsForSupplier(supplierID)` método.


[![Depois de clicar no botão interromper todos os produtos, os produtos de fornecedor s são atualizadas adequadamente.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image45.png)

**Figura 17**: depois de clicar no botão interromper todos os produtos, os produtos do fornecedor são atualizados adequadamente ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-products-price"></a>Etapa 6: Criando uma sobrecarga UpdateProduct na camada de lógica de negócios para o ajuste de preço de um produto

Como com interromper todos os produtos botão FormView, para adicionar botões para aumentar e diminuir o preço de um produto GridView é necessário primeiro adicionar os métodos apropriados de camada de acesso a dados e camada de lógica comercial. Como já temos um método que atualiza uma linha única de produto a DAL, podemos fornecer essa funcionalidade, criando uma nova sobrecarga para o `UpdateProduct` método na BLL.

Nosso passado `UpdateProduct` sobrecargas entraram em alguma combinação de campos de produto como valores escalares de entrada e, em seguida, atualizou apenas os campos para o produto especificado. Para essa sobrecarga vamos variar levemente com relação esse padrão e o produto em vez disso, passe `ProductID` e a porcentagem pela qual ajustar a `UnitPrice` (em vez de passar nos novos, ajustados `UnitPrice` em si). Essa abordagem será simplificam o código que precisamos escrever na classe ASP.NET página code-behind, já que não será preciso se preocupar com a determinação do produto atual `UnitPrice`.

O `UpdateProduct` sobrecarga para este tutorial é mostrado abaixo:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample8.cs)]

Essa sobrecarga recupera informações sobre o produto especificado através da DAL `GetProductByProductID(productID)` método. Em seguida, ele verifica se o produto `UnitPrice` for atribuído a um banco de dados `NULL` valor. Se for, o preço é deixado inalterado. Se, no entanto, há um não -`NULL` `UnitPrice` valor, o método de atualizações do produto `UnitPrice` pelo percentual especificado (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Etapa 7: Adicionar o aumento e diminuição botões a GridView

O GridView (e DetailsView) são ambos composto por uma coleção de campos. Além de BoundFields, CheckBoxFields e TemplateFields, o ASP.NET inclui ButtonField, que, como o nome sugere, é renderizada como uma coluna com um botão, LinkButton ou ImageButton para cada linha. Semelhante de FormView, clicando em *qualquer* botão dentro de GridView botões de paginação, editar ou excluir botões, botões de classificação e assim por diante causa um postback e aciona o GridView [ `RowCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

O ButtonField tem um `CommandName` propriedade que atribui o valor especificado para cada um dos seus botões `CommandName` propriedades. Como com FormView, o `CommandName` valor é usado pelo `RowCommand` manipulador de eventos para determinar qual botão foi clicado.

Vamos adicionar dois ButtonFields novo para o GridView, uma com um texto do botão preço + 10% e o outro com o texto do preço -10%. Para adicionar esses ButtonFields, clique no link Edit Columns na marca inteligente do GridView, selecione o tipo de campo ButtonField na lista na parte superior esquerda e clique no botão Adicionar.


![Adicionar dois ButtonFields a GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image48.png)

**Figura 18**: adicionar duas ButtonFields a GridView


Mova os dois ButtonFields para que eles apareçam como os primeiros dois campos de GridView. Em seguida, defina as `Text` propriedades desses dois ButtonFields preço + 10% e preço -10% e o `CommandName` propriedades IncreasePrice e DecreasePrice, respectivamente. Por padrão, um ButtonField processa sua coluna de botões como botões de link. Isso pode ser alterado, no entanto, por meio do ButtonField [ `ButtonType` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Vamos ter esses dois ButtonFields renderizados como botões de envio regulares; Portanto, defina as `ButtonType` propriedade para `Button`. Figura 19 mostra os campos de caixa de diálogo depois que essas alterações foram feitas; a seguir que é a marcação declarativa do GridView.


![Configurar o texto de ButtonFields, CommandName e ButtonType propriedades](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image49.png)

**Figura 19**: configurar o ButtonFields `Text`, `CommandName`, e `ButtonType` propriedades


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample9.aspx)]

Com essas ButtonFields criados, a etapa final é criar um manipulador de eventos para o GridView `RowCommand` eventos. Esse manipulador de eventos, se acionada porque o preço + 10% ou botões de % do preço -10 foram clicados, é preciso determinar o `ProductID` da linha cujo botão foi clicado e, em seguida, chamar o `ProductsBLL` da classe `UpdateProduct` método, passando o apropriado `UnitPrice` percentual de ajuste juntamente com o `ProductID`. O código a seguir executa essas tarefas:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample10.cs)]

Para determinar a `ProductID` da linha cujo preço + 10% ou botão de % do preço -10 foi clicado, é preciso consultar o GridView `DataKeys` coleção. Esta coleção mantém os valores dos campos especificados no `DataKeyNames` propriedade para cada linha de GridView. Desde o GridView `DataKeyNames` propriedade foi definida como ProductID pelo Visual Studio ao associar o ObjectDataSource a GridView `DataKeys(rowIndex).Value` fornece o `ProductID` especificado *rowIndex*.

O ButtonField passa automaticamente a *rowIndex* da linha cujo botão foi clicado por meio do `e.CommandArgument` parâmetro. Portanto, para determinar a `ProductID` da linha cujo preço + 10% ou botão de % do preço -10 foi clicado, usamos: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Como com o botão interromper todos os produtos, se você tiver desabilitado o estado de exibição do GridView, GridView é que está sendo ligado novamente o armazenamento de dados subjacente em cada postagem e, portanto, imediatamente será atualizada para refletir uma alteração de preço que ocorre entre o clique qualquer um dos botões. Se, no entanto, você não tiver desabilitado o estado de exibição do GridView, você precisará manualmente depois de fazer essa alteração, associar novamente os dados para o GridView. Para fazer isso, basta fazer uma chamada para o GridView `DataBind()` método imediatamente depois de invocar o `UpdateProduct` método.

Figura 20 mostra a página ao exibir os produtos oferecidos por vovó Kelly Homestead. Figura 21 mostra os resultados após o preço + 10% botão foi clicado duas vezes para distribuir de Framboesa da Vovó e o botão do preço -10% uma vez para Molho Oxicoco ingrediente.


[![O GridView inclui preço + 10% e preço -10% botões](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image50.png)

**Figura 20**: O preço do GridView inclui + 10% e botões de % do preço -10 ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image52.png))


[![Os preços para o primeiro e terceiro produto foram atualizados por meio de preço + 10% e preço -10% botões](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image53.png)

**Figura 21**: os preços para o primeiro e terceiro produto foram atualizados por meio de preço + 10% e botões de % do preço -10 ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image55.png))


> [!NOTE]
> O GridView (e DetailsView) também podem ter botões, botões de link ou ImageButtons adicionado ao seu TemplateFields. Como com BoundField, esses botões, quando clicado, serão induzir um postback, gerando o GridView `RowCommand` eventos. Quando adicionar botões em um TemplateField, no entanto, o botão `CommandArgument` não são definidos automaticamente para o índice da linha conforme ela é ao usar ButtonFields. Se você precisar determinar o índice da linha do botão que foi clicado dentro de `RowCommand` manipulador de eventos, você precisará definir manualmente no botão `CommandArgument` propriedade em sua sintaxe declarativa em um TemplateField, usando um código como:  
> `<asp:Button runat="server" ... CommandArgument='<%# ((GridViewRow) Container).RowIndex %>'`.


## <a name="summary"></a>Resumo

Os controles GridView, DetailsView e FormView podem incluir os botões, botões de link ou ImageButtons. Esses botões, quando clicado, fazer com que um postback e disparar o `ItemCommand` eventos nos controles de FormView e DetailsView e o `RowCommand` evento no GridView. Esses controles da Web de dados tem uma funcionalidade interna para lidar com ações comuns relacionadas ao comando, como excluir ou editar registros. No entanto, também podemos usar botões que, quando clicado, responda com nosso próprio código personalizado em execução.

Para fazer isso, precisamos criar um manipulador de eventos para o `ItemCommand` ou `RowCommand` eventos. Nesse manipulador de eventos, verificamos primeiro a entrada `CommandName` valor para determinar qual botão foi clicado e, em seguida, tomar as devidas providências de personalizado. Neste tutorial vimos como usar os botões e ButtonFields para interromper todos os produtos para um fornecedor especificado ou para aumentar ou diminuir o preço de um produto específico, 10%.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Avançar](adding-and-responding-to-buttons-to-a-gridview-vb.md)
