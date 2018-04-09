---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Adicionando e responder aos botões um GridView (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos como adicionar botões personalizados, para um modelo e os campos de um controle GridView ou DetailsView. Em particular, será bui...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 58b570c897810eeaa182a201616a182c02e9d92c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Adicionando e responder aos botões um GridView (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) ou [baixar PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> Neste tutorial, examinaremos como adicionar botões personalizados, para um modelo e os campos de um controle GridView ou DetailsView. Em particular, criaremos uma interface que tem um FormView que permite que o usuário pesquise os fornecedores.


## <a name="introduction"></a>Introdução

Enquanto muitos cenários de relatório envolvem acesso somente leitura aos dados do relatório, ele s não é incomum para relatórios incluem a capacidade de executar ações com base nos dados exibidos. Normalmente isso envolvido adicionando um controle de botão, LinkButton ou ImageButton Web com cada registro exibido no relatório que, quando clicado, causa um postback e invoca um código do lado do servidor. Editar e excluir dados em uma base de registro por registro são o exemplo mais comum. Na verdade, conforme vimos começando com o [visão geral de inserindo, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) tutorial, edição e exclusão é tão comum que os controles GridView, DetailsView e FormView podem dar suporte a essa funcionalidade sem o é necessário para gravar uma única linha de código.

Além para editar e excluir botões, o GridView, DetailsView e FormView controles também podem incluir botões, botões de link ou ImageButtons que, quando clicado, executar alguns lógica personalizada do lado do servidor. Neste tutorial, examinaremos como adicionar botões personalizados, para um modelo e os campos de um controle GridView ou DetailsView. Em particular, criaremos uma interface que tem um FormView que permite que o usuário pesquise os fornecedores. Para um determinado fornecedor, FormView mostrará informações sobre o fornecedor junto com um controle de botão Web que, se clicado, marcará todos os seus produtos associados como descontinuado. Além disso, um GridView lista os produtos fornecidos pelo fornecedor selecionado, com cada linha que contém o preço aumentar e botões de preço de desconto que, se clicado, aumentar ou reduzir o produto s `UnitPrice` em 10% (consulte a Figura 1).


[![O FormView e GridView contém botões que executam ações personalizadas](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Figura 1**: ambos FormView e GridView contém botões que executar ações personalizadas ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Etapa 1: Adicionar as páginas da Web Tutorial do botão

Antes de examinarmos como adicionar um botões personalizados, deixe-s primeiro dedicar um tempo para criar as páginas ASP.NET em nosso projeto de site que vamos precisar para este tutorial. Comece adicionando uma nova pasta chamada `CustomButtons`. Em seguida, adicione as duas páginas ASP.NET a seguir para a pasta, certificando-se de associar cada página com o `Site.master` página mestre:

- `Default.aspx`
- `CustomButtons.aspx`


![Adicionar as páginas ASP.NET para os tutoriais de relacionadas botões personalizados](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Figura 2**: adicionar as páginas ASP.NET para os tutoriais de relacionadas botões personalizados


Como em outras pastas, `Default.aspx` no `CustomButtons` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicione este controle de usuário para `Default.aspx` arrastando-o no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Figura 3**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


Por fim, adicione as páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após a paginação e a classificação `<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Após a atualização `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para a edição, inserção e exclusão tutoriais.


![O mapa do Site agora inclui a entrada para o Tutorial de botões personalizados](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Figura 4**: O mapa de Site agora inclui a entrada para o Tutorial de botões personalizados


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Etapa 2: Adicionando um FormView que lista os fornecedores

Permitir que o s começar a usar este tutorial, adicionando o FormView que lista os fornecedores. Como discutido na introdução, este FormView permitirá que o usuário para percorrer os fornecedores, mostrando os produtos fornecidos pelo fornecedor em um controle GridView. Além disso, esse FormView incluirá um botão que, quando clicado, marcará todos os produtos de fornecedor s como descontinuado. Antes que diz respeito a nós com a adição do botão personalizado para o FormView, permitem s primeiro crie apenas FormView para que ele exibe as informações do fornecedor.

Comece abrindo o `CustomButtons.aspx` página o `CustomButtons` pasta. Adicionar um FormView à página arrastando-a da caixa de ferramentas para o Designer e defina seu `ID` propriedade `Suppliers`. De FormView s marca inteligente, optar por criar um novo ObjectDataSource denominado `SuppliersDataSource`.


[![Criar um novo ObjectDataSource denominado SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Figura 5**: criar um novo ObjectDataSource nomeado `SuppliersDataSource` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


Configurar esse novo ObjectDataSource, de modo que as consultas a partir de `SuppliersBLL` classe s `GetSuppliers()` método (veja a Figura 6). Como esse FormView não fornece uma interface para atualizar as informações do fornecedor, selecione opção na lista suspensa na guia de atualização (nenhum).


[![Configurar a fonte de dados para usar a classe SuppliersBLL s GetSuppliers() método](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Figura 6**: configurar a fonte de dados para usar o `SuppliersBLL` classe s `GetSuppliers()` método ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


Depois de configurar o ObjectDataSource, o Visual Studio gerará um `InsertItemTemplate`, `EditItemTemplate`, e `ItemTemplate` de FormView. Remover o `InsertItemTemplate` e `EditItemTemplate` e modificar o `ItemTemplate` para que ele exibe apenas o fornecedor s empresa nome e número de telefone. Por fim, ativar o suporte de paginação para o FormView, marcando a caixa de seleção Habilitar paginação sua marca inteligente (ou definindo seu `AllowPaging` propriedade `True`). Após essas alterações sua marcação declarativa de s de página deve ser semelhante ao seguinte:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

A Figura 7 mostra a página CustomButtons.aspx quando visualizada através de um navegador.


[![Lista de FormView CompanyName e campos de telefone do fornecedor selecionado no momento](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Figura 7**: A lista de FormView o `CompanyName` e `Phone` campos de fornecedor atualmente selecionado ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Etapa 3: Adicionando um controle GridView que lista os produtos do fornecedor selecionado

Antes de adicionar o botão interromper todos os produtos no modelo de s FormView, deixamos s primeiro adicione um controle GridView abaixo FormView que lista os produtos fornecidos pelo fornecedor selecionado. Para fazer isso, adicione um controle GridView à página, defina seu `ID` propriedade `SuppliersProducts`, e adicione um novo ObjectDataSource denominado `SuppliersProductsDataSource`.


[![Criar um novo ObjectDataSource denominado SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Figura 8**: criar um novo ObjectDataSource nomeado `SuppliersProductsDataSource` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


Configurar este ObjectDataSource para usar a classe ProductsBLL s `GetProductsBySupplierID(supplierID)` método (consulte a Figura 9). Enquanto esse GridView permitirá um preço do produto s a ser ajustado, ele venceu t usando internos, editar ou excluir recursos de GridView. Portanto, podemos definir a lista suspensa como (nenhum) para o s ObjectDataSource guias UPDATE, INSERT e DELETE.


[![Configurar a fonte de dados para usar a classe ProductsBLL s GetProductsBySupplierID(supplierID) método](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Figura 9**: configurar a fonte de dados para usar o `ProductsBLL` classe s `GetProductsBySupplierID(supplierID)` método ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


Desde o `GetProductsBySupplierID(supplierID)` método aceita um parâmetro de entrada, o assistente ObjectDataSource nos solicita a origem desse valor de parâmetro. Para passar o `SupplierID` valor de FormView, defina a lista suspensa de origem de parâmetros para o controle e a lista suspensa ControlID para `Suppliers` (ID de FormView criado na etapa 2).


[![Indicar que o supplierID parâmetro deve vir do controle FormView fornecedores](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Figura 10**: indicar que o *`supplierID`* parâmetro deve vir do `Suppliers` controle FormView ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


Depois de concluir o assistente ObjectDataSource, o GridView conterá um BoundField ou CheckBoxField para cada produto s campos de dados. Permitem s reduzir isso para mostrar apenas o `ProductName` e `UnitPrice` BoundFields juntamente com o `Discontinued` CheckBoxField; Além disso, permitir que o formato de s a `UnitPrice` BoundField, de modo que seu texto é formatado como uma moeda. O GridView e `SuppliersProductsDataSource` declarativo ObjectDataSource s deve ser semelhante ao seguinte marcação:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

Neste ponto, nosso tutorial exibe um relatório de detalhes/mestre, permitindo que o usuário selecione um fornecedor de FormView na parte superior e exibir os produtos fornecidos por esse fornecedor por meio de GridView na parte inferior. A Figura 11 mostra uma captura de tela desta página, ao selecionar o fornecedor Tokyo Traders de FormView.


[![Os produtos do fornecedor selecionado são exibidos em GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Figura 11**: os produtos do fornecedor selecionado são exibidos em GridView ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Etapa 4: Criando a DAL e BLL métodos para interromper todos os produtos de um fornecedor

Antes de adicionar um botão de FormView que, quando clicado, interrompe todos os produtos supplier s, primeiro precisamos adicionar um método com a DAL e BLL que para executar esta ação. Em particular, esse método será chamado `DiscontinueAllProductsForSupplier(supplierID)`. Quando o s FormView botão é clicado, podemos vai invocar esse método na camada de lógica de negócios, passando no fornecedor selecionado s `SupplierID`; BLL chamará para baixo para o método correspondente de camada de acesso a dados, que emitirá um `UPDATE` instrução o banco de dados que interrompe os produtos de fornecedor especificado s.

Como fizemos nossos tutoriais anteriores, vamos usar uma abordagem de baixo para cima, começando com o método DAL, em seguida, o método BLL, a criação e, por fim, a implementação da funcionalidade na página ASP.NET. Abra o `Northwind.xsd` conjunto de dados tipado no `App_Code/DAL` pasta e adicione um novo método para o `ProductsTableAdapter` (com o botão direito no `ProductsTableAdapter` e escolha Adicionar consulta). Isso abrirá o Assistente de configuração de consulta do TableAdapter, que nos orienta o processo de adicionar o novo método. Iniciar, indicando que o nosso método DAL usará uma instrução de SQL ad hoc.


[![Crie o método DAL usando uma instrução de SQL Ad Hoc](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Figura 12**: criar o método DAL usando uma instrução de SQL Ad Hoc ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


Em seguida, o assistente solicita conosco sobre que tipo de consulta para criar. Desde o `DiscontinueAllProductsForSupplier(supplierID)` método terá que atualizar o `Products` tabela de banco de dados, definindo o `Discontinued` campo 1 para todos os produtos fornecidos por especificado *`supplierID`*, é preciso criar uma consulta que atualiza os dados.


[![Escolha o tipo de consulta de atualização](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Figura 13**: escolha o tipo de consulta UPDATE ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


A próxima tela do assistente fornece s TableAdapter existente `UPDATE` instrução, que atualiza cada um dos campos definidos no `Products` DataTable. Substitua este texto de consulta com a seguinte instrução:


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Depois de inserir essa consulta e clicar em Avançar, a última tela do assistente solicita o nome do novo método s usar `DiscontinueAllProductsForSupplier`. Conclua o assistente clicando no botão Concluir. Ao retornar para o Designer de conjunto de dados, você verá um novo método no `ProductsTableAdapter` chamado `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Nome do novo DiscontinueAllProductsForSupplier método DAL](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Figura 14**: nome do novo método DAL `DiscontinueAllProductsForSupplier` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


Com o `DiscontinueAllProductsForSupplier(supplierID)` método criado na camada de acesso a dados, nossa próxima tarefa é criar o `DiscontinueAllProductsForSupplier(supplierID)` método na camada de lógica de negócios. Para fazer isso, abra o `ProductsBLL` arquivo de classe e adicione o seguinte:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Este método simplesmente chama para baixo até o `DiscontinueAllProductsForSupplier(supplierID)` método DAL, passando fornecido *`supplierID`* valor do parâmetro. Se houver quaisquer regras de negócio que permitido apenas um fornecedor de produtos ser interrompido em determinadas circunstâncias, essas regras devem ser implementadas aqui, na BLL.

> [!NOTE]
> Ao contrário o `UpdateProduct` sobrecargas no `ProductsBLL` classe, o `DiscontinueAllProductsForSupplier(supplierID)` assinatura do método não inclui o `DataObjectMethodAttribute` atributo (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Isso impede o `DiscontinueAllProductsForSupplier(supplierID)` método da lista ObjectDataSource s s do Assistente para configurar a fonte de dados suspenso na guia de atualização. Eu ve omitido esse atributo porque estamos chamará o `DiscontinueAllProductsForSupplier(supplierID)` método diretamente de um manipulador de eventos em nossa página do ASP.NET.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Etapa 5: Adicionando um interromper todos os botões de produtos para o FormView

Com o `DiscontinueAllProductsForSupplier(supplierID)` método na BLL e DAL concluir, a etapa final para adicionar a capacidade de interromper todos os produtos para o fornecedor selecionado é adicionar um controle de botão Web para o s FormView `ItemTemplate`. Permitem s adicionar este botão abaixo do número de telefone do fornecedor s com o texto do botão, interromper todos os produtos e um `ID` valor da propriedade `DiscontinueAllProductsForSupplier`. Você pode adicionar esse controle de botão Web por meio do Designer clicando no link Editar modelos na marca inteligente FormView s (consulte a Figura 15), ou diretamente por meio de sintaxe declarativa.


[![Adicionar um interromper todos os produtos botão Web controle para ItemTemplate s FormView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Figura 15**: Adicione um interromper todos os produtos Web controle Button para o s FormView `ItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


Quando o botão é clicado, visitar um usuário que a página, um postback tem lugar e a s FormView [ `ItemCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) é acionado. Para executar código personalizado em resposta a esse botão está sendo clicado, podemos criar um manipulador de eventos para esse evento. Entender, porém, que o `ItemCommand` evento é acionado sempre que *qualquer* controle Button, LinkButton ou ImageButton Web é clicado em FormView. Isso significa que, quando o usuário move de uma página para outra em FormView, o `ItemCommand` evento ser acionado; a mesma coisa quando o usuário clica em novo, editar ou excluir em um FormView que dá suporte à inserção, atualização ou exclusão.

Desde que o `ItemCommand` é acionado independentemente de qual botão é clicado, no caso de manipulador precisamos de uma maneira de determinar se a interromper todos os produtos botão foi clicado, ou se foi algum outro botão. Para fazer isso, podemos definir o controle de botão Web s `CommandName` propriedade para algum valor de identificação. Quando o botão é clicado, isso `CommandName` valor é passado para o `ItemCommand` manipulador de eventos, possibilitando a determinar se o botão interromper todos os produtos do botão clicado. Definir o s interromper todos os produtos botão `CommandName` propriedade DiscontinueProducts.

Por fim, permitem s usar uma caixa de diálogo de confirmação do lado do cliente para garantir que o usuário realmente deseja interromper os produtos do fornecedor selecionado s. Como vimos no [' adicionando confirmação do lado do cliente quando excluindo](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) tutorial, isso pode ser feito com um pouco de JavaScript. Em particular, defina a propriedade do botão Web controle s OnClientClick `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Depois de fazer essas alterações, a sintaxe declarativa de s FormView deve parecer com o seguinte:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Em seguida, crie um manipulador de eventos para o s FormView `ItemCommand` eventos. Este manipulador de eventos precisamos primeiro determine se a interromper todos os produtos botão foi clicado. Se assim, desejamos criar uma instância do `ProductsBLL` de classe e chamar sua `DiscontinueAllProductsForSupplier(supplierID)` método, passando o `SupplierID` de FormView selecionado:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Observe que o `SupplierID` do fornecedor selecionado atual em FormView podem ser acessados usando o s FormView [ `SelectedValue` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). O `SelectedValue` propriedade retorna os primeiros dados de chave, valor do registro que está sendo exibido em FormView. O FormView s [ `DataKeyNames` propriedade](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), que indica os dados de campos do qual os dados de valores de chave são extraídos de, foi definida automaticamente para `SupplierID` pelo Visual Studio ao associar o ObjectDataSource para trás FormView na etapa 2.

Com o `ItemCommand` manipulador de eventos criado, reserve um tempo para testar a página. Navegue até o Cooperativa de Quesos 'Las Cabras' fornecedor (-s quinto fornecedor em FormView para mim). Este fornecedor fornece dois produtos, Queso Cabrales e Queso Manchego La Pastora, que são *não* descontinuado.

Imagine que ir Cooperativa Quesos 'Las Cabras' está fora da empresa e, portanto, seus produtos para ser interrompido. Clique nos descontinuar o botão de todos os produtos. Isso exibirá a caixa de diálogo Confirmar cliente caixa (consulte a Figura 16).


[![Cooperativa de Quesos Las Cabras fornece dois produtos ativos](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Figura 16**: Cooperativa de Quesos Las Cabras fornece dois produtos ativos ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


Se você clicar em Okey na caixa de diálogo Confirmar do lado do cliente, o envio do formulário continuará, causando um postback em que o s FormView `ItemCommand` evento será disparado. O manipulador de eventos que criamos será executada, chamar o `DiscontinueAllProductsForSupplier(supplierID)` método e interrompendo o Queso Cabrales e Queso Manchego La Pastora produtos.

Se você tiver desabilitado o estado de exibição do GridView s, GridView está sendo vinculada outra vez para o repositório de dados subjacente em cada postback e portanto imediatamente será atualizado para refletir que esses dois produtos são descontinuados (consulte a Figura 17). Se, no entanto, você não tiver desabilitado o estado de exibição em GridView, você precisará reassociar manualmente os dados do GridView depois de fazer essa alteração. Para fazer isso, basta fazer uma chamada para o s GridView `DataBind()` método imediatamente após chamar o `DiscontinueAllProductsForSupplier(supplierID)` método.


[![Depois de clicar no botão interromper todos os produtos, os produtos de fornecedor s são atualizadas adequadamente.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Figura 17**: depois de clicar no botão interromper todos os produtos, os produtos de fornecedor s são atualizadas adequadamente ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Etapa 6: Criando uma sobrecarga UpdateProduct na camada de lógica de negócios para ajustar um preço do produto s

Como com interromper todos os produtos botão FormView, para adicionar os botões para aumentar e diminuir o preço de um produto em GridView, precisamos adicionar primeiro os métodos apropriados de camada de acesso a dados e uma camada de lógica comercial. Como já temos um método que atualiza uma linha de produto único da DAL, podemos fornecer essa funcionalidade criando uma nova sobrecarga para o `UpdateProduct` método na BLL.

Nosso passado `UpdateProduct` sobrecargas entraram em uma combinação de campos de produto como valores escalares de entrada e, em seguida, atualizou apenas os campos para o produto especificado. Para essa sobrecarga vamos variar um pouco deste padrão e passar em vez disso, o produto s `ProductID` e a porcentagem pela qual ajustar o `UnitPrice` (em vez de passar o novo, ajustada `UnitPrice` em si). Essa abordagem simplificará o código que é necessário para gravar na classe por trás do código de página ASP.NET, já que estamos não precisa se preocupar com a determinação do produto atual s `UnitPrice`.

O `UpdateProduct` de sobrecarga para este tutorial é mostrado abaixo:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Essa sobrecarga recupera informações sobre o produto especificado por meio de s DAL `GetProductByProductID(productID)` método. Em seguida, verifica se o produto s `UnitPrice` é atribuído a um banco de dados `NULL` valor. Se for, o preço é deixado inalterado. Se, no entanto, há uma não -`NULL` `UnitPrice` valor, o método atualiza o produto s `UnitPrice` pelos especificado % (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Etapa 7: Adicionando os botões de redução e aumente a GridView

O GridView (e DetailsView) são ambas composto de uma coleção de campos. Além dos BoundFields, CheckBoxFields e TemplateFields, o ASP.NET inclui ButtonField, que, como o nome sugere, é renderizada como uma coluna com um botão, LinkButton ou ImageButton para cada linha. Semelhante de FormView, clicando em *qualquer* botão dentro de GridView botões de paginação, editar ou excluir botões, botões de classificação e assim por diante causa um postback e gera o GridView s [ `RowCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

O ButtonField tem um `CommandName` propriedade que atribui o valor especificado para cada um dos seus botões `CommandName` propriedades. Como o FormView, o `CommandName` valor é usado pelo `RowCommand` manipulador de eventos para determinar qual botão foi clicado.

Permitem s adicionar dois ButtonFields novo GridView, uma com um texto de botão preço + 10% e o outro com o texto preço -10%. Para adicionar esses ButtonFields, clique no link Editar colunas de marca inteligente GridView s, selecione o tipo de campo ButtonField da lista na parte superior esquerda e clique no botão Adicionar.


![Adicionar duas ButtonFields a GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Figura 18**: adicionar dois ButtonFields a GridView


Mova os dois ButtonFields para que eles apareçam como os dois primeiros campos de GridView. Em seguida, defina o `Text` propriedades desses dois ButtonFields a + 10% de preço e preço -10% e o `CommandName` propriedades IncreasePrice e DecreasePrice, respectivamente. Por padrão, um ButtonField processa sua coluna de botões como botões de link. Isso pode ser alterado, no entanto, por meio de s ButtonField [ `ButtonType` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Permite que o s tenham essas duas ButtonFields renderizados como regulares botões de ação; Portanto, definir o `ButtonType` propriedade `Button`. Figura 19 mostra os campos de caixa de diálogo depois que essas alterações foram feitas; Depois disso é a marcação declarativa de s GridView.


![Configurar o texto ButtonFields, CommandName e propriedades ButtonType](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Figura 19**: configurar o ButtonFields `Text`, `CommandName`, e `ButtonType` propriedades


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Com essas ButtonFields criados, a etapa final é criar um manipulador de eventos para o s GridView `RowCommand` eventos. Este manipulador de eventos, se acionada porque o preço + 10% ou os botões do preço -10% foi clicado, precisa determinar o `ProductID` para a linha cujo botão foi clicado e, em seguida, chamar o `ProductsBLL` classe s `UpdateProduct` método, passando apropriada `UnitPrice` ajuste de porcentagem junto com o `ProductID`. O código a seguir executa essas tarefas:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Para determinar o `ProductID` para a linha cujo preço + 10% ou botão de % de preço -10 foi clicado, é necessário consultar o GridView s `DataKeys` coleção. Esta coleção contém os valores dos campos especificados no `DataKeyNames` propriedade para cada linha em GridView. Desde o GridView s `DataKeyNames` propriedade foi definida como ProductID pelo Visual Studio ao associar o ObjectDataSource a GridView, `DataKeys(rowIndex).Value` fornece o `ProductID` especificado *rowIndex*.

O ButtonField passa automaticamente o *rowIndex* da linha cujo botão foi clicado por meio de `e.CommandArgument` parâmetro. Portanto, para determinar o `ProductID` para a linha cujo preço + 10% ou botão de % de preço -10 foi clicado, usamos: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Como com o botão de interromper todos os produtos, se você tiver desabilitado o estado de exibição do GridView s, GridView está sendo vinculada outra vez para o repositório de dados subjacente em cada postback e portanto imediatamente será atualizado para refletir uma alteração de preços que ocorre clique qualquer um dos botões. Se, no entanto, você não tiver desabilitado o estado de exibição em GridView, você precisará reassociar manualmente os dados do GridView depois de fazer essa alteração. Para fazer isso, basta fazer uma chamada para o s GridView `DataBind()` método imediatamente após chamar o `UpdateProduct` método.

Figura 20 mostra a página ao exibir os produtos fornecidos por casa avó Kelly. Figura 21 mostra os resultados após o preço + 10% botão foi clicado duas vezes para framboesa distribuídos do avó e botão de % preço -10 uma vez para Molho Northwoods Cranberry.


[![GridView inclui preço + 10% e preço -10% botões](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Figura 20**: O preço do GridView inclui + 10% e preço -10% botões ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![Os preços do produto primeiro e terceiro foram atualizados por meio do preço + 10% e preço -10% botões](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Figura 21**: os preços para o primeiro e terceiro produto foram atualizados por meio do preço + 10% e preço -10% botões ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> O GridView (e DetailsView) também podem ter botões, botões de link ou ImageButtons adicionado ao seu TemplateFields. Como com BoundField, esses botões quando clicado, serão induzir um postback, gerando o GridView s `RowCommand` eventos. Quando adicionar botões em um TemplateField, no entanto, o botão s `CommandArgument` não é automaticamente definida para o índice da linha que quando usando ButtonFields. Se você precisar determinar o índice de linha do botão que foi clicado dentro do `RowCommand` manipulador de eventos, você precisará definir manualmente o botão s `CommandArgument` propriedade em sua sintaxe declarativa em TemplateField, usando código como:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.


## <a name="summary"></a>Resumo

Os controles GridView, DetailsView e FormView podem incluir botões, botões de link ou ImageButtons. Esses botões quando clicado, causar um postback e gerar o `ItemCommand` eventos nos controles de FormView e DetailsView e `RowCommand` eventos em GridView. Esses controles da Web de dados tem uma funcionalidade interna para lidar com ações comuns relacionados ao comando, como excluir ou editar registros. No entanto, também podemos usar botões que, quando clicado, responder com nosso próprio código personalizado em execução.

Para fazer isso, precisamos criar um manipulador de eventos para o `ItemCommand` ou `RowCommand` eventos. Nesse manipulador de eventos, verifique primeiro de entrada `CommandName` valor para determinar qual botão foi clicado e, em seguida, tomar as devidas providências de personalizado. Neste tutorial, vimos como usar os botões e ButtonFields para interromper todos os produtos de um fornecedor especificado ou para aumentar ou diminuir o preço de um produto específico em 10%.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-and-responding-to-buttons-to-a-gridview-cs.md)
