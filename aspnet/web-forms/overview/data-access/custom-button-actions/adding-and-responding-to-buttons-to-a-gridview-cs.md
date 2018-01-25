---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
title: "Adicionando e responder aos botões um GridView (c#) | Microsoft Docs"
author: rick-anderson
description: "Neste tutorial, examinaremos como adicionar botões personalizados, para um modelo e os campos de um controle GridView ou DetailsView. Em particular, será bui..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 128fdb5f-4c5e-42b5-b485-f3aee90a8e38
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
msc.type: authoredcontent
ms.openlocfilehash: 4f2a31f406bb1ed98e3620e216b4ad14fe59b32f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="adding-and-responding-to-buttons-to-a-gridview-c"></a>Adicionando e responder aos botões um GridView (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_CS.exe) ou [baixar PDF](adding-and-responding-to-buttons-to-a-gridview-cs/_static/datatutorial28cs1.pdf)

> Neste tutorial, examinaremos como adicionar botões personalizados, para um modelo e os campos de um controle GridView ou DetailsView. Em particular, criaremos uma interface que tem um FormView que permite que o usuário pesquise os fornecedores.


## <a name="introduction"></a>Introdução

Enquanto muitos cenários de relatório envolvem acesso somente leitura aos dados do relatório, não é incomum para relatórios incluem a capacidade de executar ações com base nos dados exibidos. Normalmente isso envolvido adicionando um controle de botão, LinkButton ou ImageButton Web com cada registro exibido no relatório que, quando clicado, causa um postback e invoca um código do lado do servidor. Editar e excluir dados em uma base de registro por registro são o exemplo mais comum. Na verdade, conforme vimos começando com o [visão geral de inserindo, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, edição e exclusão é tão comum que os controles GridView, DetailsView e FormView podem dar suporte a essa funcionalidade sem o é necessário para gravar uma única linha de código.

Além para editar e excluir botões, o GridView, DetailsView e FormView controles também podem incluir botões, botões de link ou ImageButtons que, quando clicado, executar alguns lógica personalizada do lado do servidor. Neste tutorial, examinaremos como adicionar botões personalizados, para um modelo e os campos de um controle GridView ou DetailsView. Em particular, criaremos uma interface que tem um FormView que permite que o usuário pesquise os fornecedores. Para um determinado fornecedor, FormView mostrará informações sobre o fornecedor junto com um controle de botão Web que, se clicado, marcará todos os seus produtos associados como descontinuado. Além disso, um GridView lista os produtos fornecidos pelo fornecedor selecionado, com cada linha que contém botões de preço de desconto que, se clicado, aumentar ou reduzir o produto e aumentar o preço `UnitPrice` em 10% (consulte a Figura 1).


[![O FormView e GridView contém botões que executam ações personalizadas](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image1.png)

**Figura 1**: ambos FormView e GridView contém botões que executar ações personalizadas ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Etapa 1: Adicionar as páginas da Web Tutorial do botão

Antes de examinarmos como adicionar um botões personalizados, primeiro vamos criar as páginas ASP.NET em nosso projeto de site que vamos precisar para este tutorial. Comece adicionando uma nova pasta chamada `CustomButtons`. Em seguida, adicione as duas páginas ASP.NET a seguir para a pasta, certificando-se de associar cada página com o `Site.master` página mestre:

- `Default.aspx`
- `CustomButtons.aspx`


![Adicionar as páginas ASP.NET para os tutoriais de relacionadas botões personalizados](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image4.png)

**Figura 2**: adicionar as páginas ASP.NET para os tutoriais de relacionadas botões personalizados


Como em outras pastas, `Default.aspx` no `CustomButtons` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicione este controle de usuário para `Default.aspx` arrastando-o no Gerenciador de soluções em modo de Design da página.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image5.png)

**Figura 3**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image7.png))


Por fim, adicione as páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após a paginação e a classificação `<siteMapNode>`:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample1.xml)]

Após a atualização `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para a edição, inserção e exclusão tutoriais.


![O mapa do Site agora inclui a entrada para o Tutorial de botões personalizados](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image8.png)

**Figura 4**: O mapa de Site agora inclui a entrada para o Tutorial de botões personalizados


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Etapa 2: Adicionando um FormView que lista os fornecedores

Vamos começar com este tutorial, adicionando o FormView que lista os fornecedores. Como discutido na introdução, este FormView permitirá que o usuário para percorrer os fornecedores, mostrando os produtos fornecidos pelo fornecedor em um controle GridView. Além disso, esse FormView incluirá um botão que, quando clicado, marcará todos os produtos do fornecedor como descontinuado. Antes que diz respeito a nós com a adição do botão personalizado para o FormView, primeiro vamos apenas criar FormView para que ele exibe as informações do fornecedor.

Comece abrindo o `CustomButtons.aspx` página o `CustomButtons` pasta. Adicionar um FormView à página arrastando-a da caixa de ferramentas para o Designer e defina seu `ID` propriedade `Suppliers`. De marca inteligente de FormView optar por criar um novo ObjectDataSource denominado `SuppliersDataSource`.


[![Criar um novo ObjectDataSource denominado SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image9.png)

**Figura 5**: criar um novo ObjectDataSource nomeado `SuppliersDataSource` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image11.png))


Configurar esse novo ObjectDataSource, de modo que as consultas a partir de `SuppliersBLL` da classe `GetSuppliers()` método (veja a Figura 6). Como esse FormView não fornece uma interface para atualizar as informações do fornecedor, selecione opção na lista suspensa na guia de atualização (nenhum).


[![Configurar a fonte de dados para usar a classe SuppliersBLL s GetSuppliers() método](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image12.png)

**Figura 6**: configurar a fonte de dados para usar o `SuppliersBLL` da classe `GetSuppliers()` método ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image14.png))


Depois de configurar o ObjectDataSource, o Visual Studio gerará um `InsertItemTemplate`, `EditItemTemplate`, e `ItemTemplate` de FormView. Remover o `InsertItemTemplate` e `EditItemTemplate` e modificar o `ItemTemplate` para que ele exibe apenas do fornecedor da empresa nome e número de telefone. Por fim, ativar o suporte de paginação para o FormView, marcando a caixa de seleção Habilitar paginação sua marca inteligente (ou definindo seu `AllowPaging` propriedade `True`). Após essas alterações declarativo da página deve ser semelhante ao seguinte:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample2.aspx)]

A Figura 7 mostra a página CustomButtons.aspx quando visualizada através de um navegador.


[![Lista de FormView CompanyName e campos de telefone do fornecedor selecionado no momento](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image15.png)

**Figura 7**: A lista de FormView o `CompanyName` e `Phone` campos de fornecedor atualmente selecionado ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-suppliers-products"></a>Etapa 3: Adicionando um controle GridView que lista os produtos do fornecedor selecionado

Antes de adicionar o botão interromper todos os produtos para o modelo de FormView, vamos adicionar primeiro uma GridView abaixo FormView que lista os produtos fornecidos pelo fornecedor selecionado. Para fazer isso, adicione um controle GridView à página, defina seu `ID` propriedade `SuppliersProducts`, e adicione um novo ObjectDataSource denominado `SuppliersProductsDataSource`.


[![Criar um novo ObjectDataSource denominado SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image18.png)

**Figura 8**: criar um novo ObjectDataSource nomeado `SuppliersProductsDataSource` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image20.png))


Configurar este ObjectDataSource para usar a classe de ProductsBLL `GetProductsBySupplierID(supplierID)` método (consulte a Figura 9). Enquanto este GridView permitirá preço do produto a ser ajustado, ele não irá usar internos, editar ou excluir recursos de GridView. Portanto, podemos definir a lista suspensa como (nenhum) para ObjectDataSource do UPDATE, INSERT e excluir guias.


[![Configurar a fonte de dados para usar a classe ProductsBLL s GetProductsBySupplierID(supplierID) método](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image21.png)

**Figura 9**: configurar a fonte de dados para usar o `ProductsBLL` da classe `GetProductsBySupplierID(supplierID)` método ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image23.png))


Desde o `GetProductsBySupplierID(supplierID)` método aceita um parâmetro de entrada, o assistente ObjectDataSource nos solicita a origem desse valor de parâmetro. Para passar o `SupplierID` valor de FormView, defina a lista suspensa de origem de parâmetros para o controle e a lista suspensa ControlID para `Suppliers` (ID de FormView criado na etapa 2).


[![Indicar que o supplierID parâmetro deve vir do controle FormView fornecedores](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image24.png)

**Figura 10**: indicar que o  *`supplierID`*  parâmetro deve vir do `Suppliers` controle FormView ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image26.png))


Depois de concluir o assistente ObjectDataSource, o GridView conterá um BoundField ou CheckBoxField para cada um dos campos de dados do produto. Vamos reduzir isso para mostrar apenas o `ProductName` e `UnitPrice` BoundFields juntamente com o `Discontinued` CheckBoxField; Além disso, vamos formatar o `UnitPrice` BoundField, de modo que seu texto é formatado como uma moeda. O GridView e `SuppliersProductsDataSource` declarativo do ObjectDataSource deve ser semelhante ao seguinte marcação:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample3.aspx)]

Neste ponto, nosso tutorial exibe um relatório de detalhes/mestre, permitindo que o usuário selecione um fornecedor de FormView na parte superior e exibir os produtos fornecidos por esse fornecedor por meio de GridView na parte inferior. A Figura 11 mostra uma captura de tela desta página, ao selecionar o fornecedor Tokyo Traders de FormView.


[![Os produtos do fornecedor selecionado são exibidos em GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image27.png)

**Figura 11**: produtos do fornecedor selecionado o são exibidos em GridView ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Etapa 4: Criando a DAL e BLL métodos para interromper todos os produtos de um fornecedor

Antes de adicionar um botão de FormView que, quando clicado, interrompe todos os produtos do fornecedor, primeiro precisamos adicionar um método com a DAL e BLL que para executar esta ação. Em particular, esse método será chamado `DiscontinueAllProductsForSupplier(supplierID)`. Quando o botão de FormView é clicado, podemos vai invocar esse método na camada de lógica de negócios, passando do fornecedor selecionado `SupplierID`; BLL chamará para baixo para o método correspondente de camada de acesso a dados, que emitirá um `UPDATE` instrução para o banco de dados que deixou de produtos do fornecedor especificado.

Como fizemos nossos tutoriais anteriores, vamos usar uma abordagem de baixo para cima, começando com o método DAL, em seguida, o método BLL, a criação e, por fim, a implementação da funcionalidade na página ASP.NET. Abra o `Northwind.xsd` conjunto de dados tipado no `App_Code/DAL` pasta e adicione um novo método para o `ProductsTableAdapter` (com o botão direito no `ProductsTableAdapter` e escolha Adicionar consulta). Isso abrirá o Assistente de configuração de consulta do TableAdapter, que nos orienta o processo de adicionar o novo método. Iniciar, indicando que o nosso método DAL usará uma instrução de SQL ad hoc.


[![Crie o método DAL usando uma instrução de SQL Ad Hoc](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image30.png)

**Figura 12**: criar o método DAL usando uma instrução de SQL Ad Hoc ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image32.png))


Em seguida, o assistente solicita conosco sobre que tipo de consulta para criar. Desde o `DiscontinueAllProductsForSupplier(supplierID)` método terá que atualizar o `Products` tabela de banco de dados, definindo o `Discontinued` campo 1 para todos os produtos fornecidos por especificado  *`supplierID`* , é preciso criar uma consulta que atualiza os dados.


[![Escolha o tipo de consulta de atualização](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image33.png)

**Figura 13**: escolha o tipo de consulta UPDATE ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image35.png))


A próxima tela do assistente fornece o TableAdapter existente `UPDATE` instrução, que atualiza cada um dos campos definidos no `Products` DataTable. Substitua este texto de consulta com a seguinte instrução:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample4.sql)]

Após a inserção dessa consulta e clicar em Avançar, a última tela do assistente pede para uso do nome do novo método `DiscontinueAllProductsForSupplier`. Conclua o assistente clicando no botão Concluir. Ao retornar para o Designer de conjunto de dados, você verá um novo método no `ProductsTableAdapter` chamado `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Nome do novo DiscontinueAllProductsForSupplier método DAL](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image36.png)

**Figura 14**: nome do novo método DAL `DiscontinueAllProductsForSupplier` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image38.png))


Com o `DiscontinueAllProductsForSupplier(supplierID)` método criado na camada de acesso a dados, nossa próxima tarefa é criar o `DiscontinueAllProductsForSupplier(supplierID)` método na camada de lógica de negócios. Para fazer isso, abra o `ProductsBLL` arquivo de classe e adicione o seguinte:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample5.cs)]

Este método simplesmente chama para baixo até o `DiscontinueAllProductsForSupplier(supplierID)` método DAL, passando fornecido  *`supplierID`*  valor do parâmetro. Se houver quaisquer regras de negócio que são permitidos apenas os produtos do fornecedor ser interrompido em determinadas circunstâncias, essas regras devem ser implementadas aqui, na BLL.

> [!NOTE]
> Ao contrário o `UpdateProduct` sobrecargas no `ProductsBLL` classe, o `DiscontinueAllProductsForSupplier(supplierID)` assinatura do método não inclui o `DataObjectMethodAttribute` atributo (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Isso impede o `DiscontinueAllProductsForSupplier(supplierID)` método na lista suspensa do Assistente de configurar fonte de dados do ObjectDataSource a guia de atualização. Eu ve omitido esse atributo porque estamos chamará o `DiscontinueAllProductsForSupplier(supplierID)` método diretamente de um manipulador de eventos em nossa página do ASP.NET.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Etapa 5: Adicionando um interromper todos os botões de produtos para o FormView

Com o `DiscontinueAllProductsForSupplier(supplierID)` método na BLL e DAL completa, a etapa final para adicionar a capacidade de interromper todos os produtos para o fornecedor selecionado é adicionar um controle de botão Web para o FormView `ItemTemplate`. Vamos adicionar um botão desse tipo abaixo do número de telefone do fornecedor com o texto do botão, interromper todos os produtos e um `ID` valor da propriedade `DiscontinueAllProductsForSupplier`. Você pode adicionar esse controle de botão Web por meio do Designer, clicando no link de editar modelos da marca inteligente de FormView (consulte a Figura 15), ou diretamente por meio de sintaxe declarativa.


[![Adicionar um interromper todos os produtos botão Web controle para ItemTemplate s FormView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image39.png)

**Figura 15**: Adicione um interromper todos os produtos Web controle Button para o FormView `ItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image41.png))


Quando o botão é clicado, visitar um usuário que a página, um postback tem lugar e de FormView [ `ItemCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) é acionado. Para executar código personalizado em resposta a esse botão está sendo clicado, podemos criar um manipulador de eventos para esse evento. Entender, porém, que o `ItemCommand` evento é acionado sempre que *qualquer* controle Button, LinkButton ou ImageButton Web é clicado em FormView. Isso significa que, quando o usuário move de uma página para outra em FormView, o `ItemCommand` evento ser acionado; a mesma coisa quando o usuário clica em novo, editar ou excluir em um FormView que dá suporte à inserção, atualização ou exclusão.

Desde que o `ItemCommand` é acionado independentemente de qual botão é clicado, no caso de manipulador precisamos de uma maneira de determinar se a interromper todos os produtos botão foi clicado, ou se foi algum outro botão. Para fazer isso, podemos definir o controle de botão Web `CommandName` propriedade para algum valor de identificação. Quando o botão é clicado, isso `CommandName` valor é passado para o `ItemCommand` manipulador de eventos, possibilitando a determinar se o botão interromper todos os produtos do botão clicado. Definir interromper todos os produtos do botão `CommandName` propriedade DiscontinueProducts.

Por fim, vamos usar uma caixa de diálogo de confirmação do lado do cliente para garantir que o usuário realmente deseja interromper a produtos do fornecedor selecionado. Como vimos no [' adicionando confirmação do lado do cliente quando excluindo](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs.md) tutorial, isso pode ser feito com um pouco de JavaScript. Em particular, defina a propriedade de OnClientClick do controle de botão Web`return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Depois de fazer essas alterações, a sintaxe declarativa de FormView deve parecer com o seguinte:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample6.aspx)]

Em seguida, crie um manipulador de eventos para o FormView `ItemCommand` eventos. Este manipulador de eventos precisamos primeiro determine se a interromper todos os produtos botão foi clicado. Se assim, desejamos criar uma instância do `ProductsBLL` de classe e chamar sua `DiscontinueAllProductsForSupplier(supplierID)` método, passando o `SupplierID` de FormView selecionado:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample7.cs)]

Observe que o `SupplierID` do fornecedor selecionado atual em FormView podem ser acessados usando o FormView [ `SelectedValue` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). O `SelectedValue` propriedade retorna os primeiros dados de chave, valor do registro que está sendo exibido em FormView. O FormView [ `DataKeyNames` propriedade](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), que indica os dados de campos do qual os dados de valores de chave são extraídos de, foi definida automaticamente para `SupplierID` pelo Visual Studio ao associar o ObjectDataSource em FormView volta na etapa 2.

Com o `ItemCommand` manipulador de eventos criado, reserve um tempo para testar a página. Navegue até o Cooperativa de Quesos 'Las Cabras' fornecedor (é o quinto fornecedor FormView para mim). Este fornecedor fornece dois produtos, Queso Cabrales e Queso Manchego La Pastora, que são *não* descontinuado.

Imagine que ir Cooperativa Quesos 'Las Cabras' está fora da empresa e, portanto, seus produtos para ser interrompido. Clique nos descontinuar o botão de todos os produtos. Isso exibirá a caixa de diálogo Confirmar cliente caixa (consulte a Figura 16).


[![Cooperativa de Quesos Las Cabras fornece dois produtos ativos](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image42.png)

**Figura 16**: Cooperativa de Quesos Las Cabras fornece dois produtos ativos ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image44.png))


Se você clicar em Okey na caixa de diálogo Confirmar do lado do cliente, o envio do formulário continuará, causando um postback no qual o FormView `ItemCommand` evento será disparado. O manipulador de eventos que criamos será executada, chamar o `DiscontinueAllProductsForSupplier(supplierID)` método e interrompendo o Queso Cabrales e Queso Manchego La Pastora produtos.

Se você tiver desabilitado o estado de exibição do GridView, GridView está sendo vinculada outra vez para o repositório de dados subjacente em cada postback e portanto imediatamente será atualizado para refletir que esses dois produtos são descontinuados (consulte a Figura 17). Se, no entanto, você não tiver desabilitado o estado de exibição em GridView, você precisará reassociar manualmente os dados do GridView depois de fazer essa alteração. Para fazer isso, basta fazer uma chamada para o GridView `DataBind()` método imediatamente após chamar o `DiscontinueAllProductsForSupplier(supplierID)` método.


[![Depois de clicar no botão interromper todos os produtos, os produtos de fornecedor s são atualizadas adequadamente.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image45.png)

**Figura 17**: depois de clicar no botão interromper todos os produtos, os produtos do fornecedor são atualizadas adequadamente ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-products-price"></a>Etapa 6: Criando uma sobrecarga UpdateProduct na camada de lógica de negócios para ajustar o preço do produto

Como com interromper todos os produtos botão FormView, para adicionar os botões para aumentar e diminuir o preço de um produto em GridView, precisamos adicionar primeiro os métodos apropriados de camada de acesso a dados e uma camada de lógica comercial. Como já temos um método que atualiza uma linha de produto único da DAL, podemos fornecer essa funcionalidade criando uma nova sobrecarga para o `UpdateProduct` método na BLL.

Nosso passado `UpdateProduct` sobrecargas entraram em uma combinação de campos de produto como valores escalares de entrada e, em seguida, atualizou apenas os campos para o produto especificado. Para essa sobrecarga vamos variar um pouco deste padrão e passar em vez disso, o produto `ProductID` e a porcentagem pela qual ajustar o `UnitPrice` (em vez de passar o novo, ajustada `UnitPrice` em si). Essa abordagem simplificará o código que é necessário gravar na classe por trás do código de página do ASP.NET, porque não precisamos se preocupar com a determinação do produto atual `UnitPrice`.

O `UpdateProduct` de sobrecarga para este tutorial é mostrado abaixo:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample8.cs)]

Essa sobrecarga recupera informações sobre o produto especificado através da DAL `GetProductByProductID(productID)` método. Em seguida, verifica se o produto `UnitPrice` é atribuído a um banco de dados `NULL` valor. Se for, o preço é deixado inalterado. Se, no entanto, há uma não -`NULL` `UnitPrice` valor, o método de atualizações do produto `UnitPrice` pelos especificado % (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Etapa 7: Adicionando os botões de redução e aumente a GridView

O GridView (e DetailsView) são ambas composto de uma coleção de campos. Além dos BoundFields, CheckBoxFields e TemplateFields, o ASP.NET inclui ButtonField, que, como o nome sugere, é renderizada como uma coluna com um botão, LinkButton ou ImageButton para cada linha. Semelhante de FormView, clicando em *qualquer* botão dentro de GridView botões de paginação, editar ou excluir botões, botões de classificação e assim por diante causa um postback e gera o GridView [ `RowCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

O ButtonField tem um `CommandName` propriedade que atribui o valor especificado para cada um dos seus botões `CommandName` propriedades. Como o FormView, o `CommandName` valor é usado pelo `RowCommand` manipulador de eventos para determinar qual botão foi clicado.

Vamos adicionar dois ButtonFields novo GridView, uma com um texto de botão preço + 10% e o outro com o texto preço -10%. Para adicionar esses ButtonFields, clique no link Editar colunas de marcas inteligentes do GridView, selecione o tipo de campo ButtonField da lista na parte superior esquerda e clique no botão Adicionar.


![Adicionar duas ButtonFields a GridView](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image48.png)

**Figura 18**: adicionar dois ButtonFields a GridView


Mova os dois ButtonFields para que eles apareçam como os dois primeiros campos de GridView. Em seguida, defina o `Text` propriedades desses dois ButtonFields a + 10% de preço e preço -10% e o `CommandName` propriedades IncreasePrice e DecreasePrice, respectivamente. Por padrão, um ButtonField processa sua coluna de botões como botões de link. Isso pode ser alterado, no entanto, por meio do ButtonField [ `ButtonType` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Vamos ter esses dois ButtonFields renderizados como regulares botões de ação; Portanto, definir o `ButtonType` propriedade `Button`. Figura 19 mostra os campos de caixa de diálogo depois que essas alterações foram feitas; a seguir que é declarativo do GridView.


![Configurar o texto ButtonFields, CommandName e propriedades ButtonType](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image49.png)

**Figura 19**: configurar o ButtonFields `Text`, `CommandName`, e `ButtonType` propriedades


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample9.aspx)]

Com essas ButtonFields criados, a etapa final é criar um manipulador de eventos para o GridView `RowCommand` eventos. Este manipulador de eventos, se acionada porque o preço + 10% ou os botões do preço -10% foi clicado, precisa determinar o `ProductID` para a linha cujo botão foi clicado e, em seguida, chamar o `ProductsBLL` da classe `UpdateProduct` método, passando apropriada `UnitPrice` ajuste de porcentagem junto com o `ProductID`. O código a seguir executa essas tarefas:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample10.cs)]

Para determinar o `ProductID` para a linha cujo preço + 10% ou botão de % de preço -10 foi clicado, é necessário consultar o GridView `DataKeys` coleção. Esta coleção contém os valores dos campos especificados no `DataKeyNames` propriedade para cada linha em GridView. Desde o GridView `DataKeyNames` propriedade foi definida como ProductID pelo Visual Studio ao associar o ObjectDataSource a GridView, `DataKeys(rowIndex).Value` fornece o `ProductID` especificado *rowIndex*.

O ButtonField passa automaticamente o *rowIndex* da linha cujo botão foi clicado por meio de `e.CommandArgument` parâmetro. Portanto, para determinar o `ProductID` para a linha cujo preço + 10% ou botão de % de preço -10 foi clicado, usamos: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Como com o botão de interromper todos os produtos, se você tiver desabilitado o estado de exibição do GridView, GridView está sendo vinculada outra vez para o repositório de dados subjacente em cada postback e portanto imediatamente será atualizado para refletir uma alteração de preços que ocorre clique qualquer um dos botões. Se, no entanto, você não tiver desabilitado o estado de exibição em GridView, você precisará reassociar manualmente os dados do GridView depois de fazer essa alteração. Para fazer isso, basta fazer uma chamada para o GridView `DataBind()` método imediatamente após chamar o `UpdateProduct` método.

Figura 20 mostra a página ao exibir os produtos fornecidos por casa avó Kelly. Figura 21 mostra os resultados após o preço + 10% botão foi clicado duas vezes para framboesa distribuídos do avó e botão de % preço -10 uma vez para Molho Northwoods Cranberry.


[![GridView inclui preço + 10% e preço -10% botões](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image50.png)

**Figura 20**: O preço do GridView inclui + 10% e preço -10% botões ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image52.png))


[![Os preços do produto primeiro e terceiro foram atualizados por meio do preço + 10% e preço -10% botões](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image53.png)

**Figura 21**: os preços para o primeiro e terceiro produto foram atualizados por meio do preço + 10% e preço -10% botões ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image55.png))


> [!NOTE]
> O GridView (e DetailsView) também podem ter botões, botões de link ou ImageButtons adicionado ao seu TemplateFields. Como com BoundField, esses botões quando clicado, serão induzir um postback, gerando o GridView `RowCommand` eventos. Ao adicionar botões em um TemplateField, no entanto, o botão `CommandArgument` não é automaticamente definida para o índice da linha que quando usando ButtonFields. Se você precisar determinar o índice de linha do botão que foi clicado dentro do `RowCommand` manipulador de eventos, você precisará definir manualmente o botão `CommandArgument` propriedade em sua sintaxe declarativa em TemplateField, usando código como:  
> `<asp:Button runat="server" ... CommandArgument='<%# ((GridViewRow) Container).RowIndex %>'`.


## <a name="summary"></a>Resumo

Os controles GridView, DetailsView e FormView podem incluir botões, botões de link ou ImageButtons. Esses botões quando clicado, causar um postback e gerar o `ItemCommand` eventos nos controles de FormView e DetailsView e `RowCommand` eventos em GridView. Esses controles da Web de dados tem uma funcionalidade interna para lidar com ações comuns relacionados ao comando, como excluir ou editar registros. No entanto, também podemos usar botões que, quando clicado, responder com nosso próprio código personalizado em execução.

Para fazer isso, precisamos criar um manipulador de eventos para o `ItemCommand` ou `RowCommand` eventos. Nesse manipulador de eventos, verifique primeiro de entrada `CommandName` valor para determinar qual botão foi clicado e, em seguida, tomar as devidas providências de personalizado. Neste tutorial, vimos como usar os botões e ButtonFields para interromper todos os produtos de um fornecedor especificado ou para aumentar ou diminuir o preço de um produto específico em 10%.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Avançar](adding-and-responding-to-buttons-to-a-gridview-vb.md)
