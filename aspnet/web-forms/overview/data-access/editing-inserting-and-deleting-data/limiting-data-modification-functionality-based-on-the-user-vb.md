---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Limitar a funcionalidade de modificação de dados com base no usuário (VB) | Microsoft Docs
author: rick-anderson
description: Em um aplicativo web que permite aos usuários editar dados, contas de usuário diferentes podem ter privilégios de edição de dados diferentes. Neste tutorial, examinaremos como t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: eccb8e114e0ecf772680a2e5b36a761736cf8025
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887867"
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Limitar a funcionalidade de modificação de dados com base no usuário (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) ou [baixar PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> Em um aplicativo web que permite aos usuários editar dados, contas de usuário diferentes podem ter privilégios de edição de dados diferentes. Neste tutorial, examinaremos como ajustar dinamicamente as capacidades de modificação de dados com base no usuário visitante.


## <a name="introduction"></a>Introdução

Um número de aplicativos da web dá suporte a contas de usuário e fornece opções diferentes, relatórios e recursos com base no usuário conectado. Por exemplo, com nossos tutoriais podemos desejar permitir que os usuários das empresas supplier fazer logon para as atualização de site e informações gerais sobre seus produtos - o nome e a quantidade por unidade, talvez - juntamente com informações do fornecedor, como o nome da sua empresa, endereço, informações de contato s e assim por diante. Além disso, é aconselhável incluir algumas contas de usuário para pessoas de nossa empresa para que eles possam fazer logon e atualizar informações de produto, como unidades em estoque, nível de estoque e assim por diante. Nosso aplicativo web também pode permitir que usuários anônimos visite (pessoas que não fizeram logon), mas limita para exibir apenas os dados. Com esse usuário conta sistema em vigor, queremos os dados de controles da Web em páginas ASP.NET para oferecer a inserir, editar e excluir recursos apropriados para o usuário conectado no momento.

Neste tutorial, examinaremos como ajustar dinamicamente as capacidades de modificação de dados com base no usuário visitante. Em particular, vamos criar uma página que exibe as informações de fornecedores em um DetailsView editável junto com um controle GridView que lista os produtos fornecidos pelo fornecedor. Se o usuário visitar a página for de nossa empresa, eles podem: exibir quaisquer informações de fornecedor s; Editar seu endereço; e editar as informações para qualquer produto fornecida pelo fornecedor. Se, no entanto, o usuário é de uma empresa específica, eles só podem exibir e editar suas próprias informações de endereço e só pode editar seus produtos que não foram marcados como descontinuado.


[![Um usuário de nossa empresa pode editar qualquer informação de s do fornecedor](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Figura 1**: um usuário de nossa empresa pode editar qualquer fornecedor s informações ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![Um usuário de um fornecedor específico somente podem exibir e editar suas informações](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Figura 2**: um usuário de um determinado fornecedor podem apenas exibir e editar suas informações ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


Permitir que o s começar!

> [!NOTE]
> O ASP.NET 2.0 s sistema de associação fornece uma plataforma extensível padronizada para criar, gerenciar e validar as contas de usuário. Como um exame do sistema de associação está além do escopo esses tutoriais, este tutorial em vez disso, "fakes" associação, permitindo que os visitantes anônimos escolher se eles são de um fornecedor específico ou de nossa empresa. Para obter mais informações sobre associação, consulte Meus [examinando o ASP.NET 2.0 s associação, funções e perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) série do artigo.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Etapa 1: Permitir que o usuário especifique seus direitos de acesso

Em um aplicativo web do mundo real, um informações de conta do usuário s incluiria se elas funcionaram para nossa empresa ou para um fornecedor específico, e essas informações estariam acessíveis por meio de programação de nossas páginas ASP.NET quando o usuário faz logon no site. Essa informação pode ser capturada por meio do sistema de funções do ASP.NET 2.0 s, como informações da conta de nível de usuário por meio do sistema de perfil, ou por meio de alguma maneira personalizada.

Como o objetivo deste tutorial é demonstrar ajustando as capacidades de modificação de dados com base no usuário conectado e não é destinado a associação de s showcase ASP.NET 2.0, funções e sistemas de perfil, vamos usar um mecanismo muito simples para determinar o recursos para o usuário visitar a página - DropDownList do qual o usuário pode indicar se eles devem ser capazes de exibir e editar qualquer uma das informações de fornecedores ou, Alternativamente, o que informações de fornecedor específico s, eles podem exibir e editar. Se o usuário indica que ela pode exibir e editar todas as informações do fornecedor (o padrão), ela pode ver todos os fornecedores, editar informações de endereço do fornecedor s e edite o nome e a quantidade por unidade de qualquer produto fornecida pelo fornecedor selecionado. Se o usuário indica que ela só pode exibir e editar um fornecedor específico, no entanto, em seguida, ela só pode exibir os detalhes e produtos para que um fornecedor e só pode atualizar o nome e a quantidade de informações de unidade para os produtos que são por *não* descontinuado.

A primeira etapa neste tutorial, em seguida, é criar este DropDownList e preenchê-la com os fornecedores no sistema. Abra o `UserLevelAccess.aspx` página o `EditInsertDelete` pasta, adicionar DropDownList cujo `ID` está definida como `Suppliers`e associar esse DropDownList para um novo ObjectDataSource denominado `AllSuppliersDataSource`.


[![Criar um novo ObjectDataSource denominado AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Figura 3**: criar um novo ObjectDataSource nomeado `AllSuppliersDataSource` ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


Como queremos que este DropDownList para incluir todos os fornecedores, configurar o ObjectDataSource para invocar o `SuppliersBLL` classe s `GetSuppliers()` método. Também verifique se o s ObjectDataSource `Update()` método é mapeado para o `SuppliersBLL` classe s `UpdateSupplierAddress` método, como essa ObjectDataSource também será usado por DetailsView incluiremos na etapa 2.

Depois de concluir o assistente ObjectDataSource, conclua as etapas ao configurar o `Suppliers` DropDownList, de modo que ela mostra o `CompanyName` campo de dados e usa o `SupplierID` campo de dados como o valor para cada `ListItem`.


[![Configurar a DropDownList de fornecedores para usar o CompanyName e campos de dados SupplierID](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Figura 4**: configurar o `Suppliers` DropDownList para usar o `CompanyName` e `SupplierID` campos de dados ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


Neste ponto, DropDownList lista os nomes de empresa dos fornecedores no banco de dados. No entanto, também é preciso incluir uma opção "Mostrar/editar todos os fornecedores" DropDownList. Para fazer isso, defina o `Suppliers` DropDownList s `AppendDataBoundItems` propriedade `true` e, em seguida, adicione um `ListItem` cujo `Text` propriedade é "Mostrar/editar todos os fornecedores" e cujo valor é `-1`. Isso pode ser adicionado diretamente por meio de marcação declarativa ou por meio do Designer indo para a janela Propriedades e clicando nas elipses em s DropDownList `Items` propriedade.

> [!NOTE]
> Voltar para o [ *filtragem de mestre/detalhes com um DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) tutorial para uma discussão mais detalhada sobre como adicionar um item Selecionar tudo para uma ligação de dados DropDownList.


Após o `AppendDataBoundItems` propriedade foi definida e o `ListItem` adicionado, a marcação declarativa de s DropDownList deve parecer com:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

A Figura 5 mostra uma captura de tela de nosso progresso atual, quando visualizada através de um navegador.


[![DropDownList fornecedores contém uma apresentação ListItem todos, mais um para cada fornecedor](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Figura 5**: O `Suppliers` DropDownList contém um Mostrar tudo `ListItem`, além de um para cada fornecedor ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


Como queremos atualizar a interface do usuário imediatamente depois que o usuário alterou sua seleção, defina o `Suppliers` DropDownList s `AutoPostBack` propriedade `true`. Na etapa 2, criaremos um controle DetailsView que mostrará as informações para o supplier(s) com base na seleção DropDownList. Em seguida, na etapa 3, vamos criar um manipulador de eventos para este s DropDownList `SelectedIndexChanged` evento, em que vamos adicionar código que associa as informações do fornecedor apropriado para o DetailsView com base no fornecedor selecionado.

## <a name="step-2-adding-a-detailsview-control"></a>Etapa 2: Adicionando um controle DetailsView

Permitir que o s use um DetailsView para mostrar informações de fornecedor. Para o usuário que pode exibir e editar todos os fornecedores, DetailsView dará suporte a paginação, permitindo que o usuário percorrer a um registro do fornecedor informações cada vez. Se o usuário trabalha para um fornecedor específico, no entanto, DetailsView mostrará apenas esse fornecedor específico informações e não incluem uma interface de paginação. Em ambos os casos, DetailsView deve permitir que o usuário edite o fornecedor s endereço, cidade e campos de país.

Adicionar um DetailsView para a página abaixo o `Suppliers` DropDownList, defina seu `ID` propriedade `SupplierDetails`e associá-lo para o `AllSuppliersDataSource` ObjectDataSource criado na etapa anterior. Em seguida, marque as caixas de seleção Habilitar paginação e habilitar edição de marca inteligente s DetailsView.

> [!NOTE]
> Se uma opção de habilitar edição em DetailsView s inteligente de ver don t rotulá-lo s porque você não especificou o s ObjectDataSource `Update()` método para o `SuppliersBLL` classe s `UpdateSupplierAddress` método. Dedique alguns momentos para voltar e alterar essa configuração, após o qual a opção Habilitar edição deve aparecer na marca inteligente s DetailsView.


Como o `SuppliersBLL` classe s `UpdateSupplierAddress` método só aceita quatro parâmetros - `supplierID`, `address`, `city`, e `country` -modificar a s DetailsView BoundFields para que o `CompanyName` e `Phone` BoundFields são somente leitura. Além disso, remova o `SupplierID` BoundField ao mesmo tempo. Por fim, o `AllSuppliersDataSource` ObjectDataSource atualmente tem seu `OldValuesParameterFormatString` propriedade definida como `original_{0}`. Reserve um tempo para remover essa configuração de propriedade de totalmente a sintaxe declarativa ou defina-o como o valor padrão, `{0}`.

Depois de configurar o `SupplierDetails` DetailsView e `AllSuppliersDataSource` ObjectDataSource, teremos a seguinte marcação declarativa:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

Neste momento pode ser paginada DetailsView por meio e as informações de endereço do fornecedor selecionado s podem ser atualizadas, independentemente da seleção feita no `Suppliers` DropDownList (veja a Figura 6).


[![Quaisquer informações de fornecedores podem ser exibidas e seu endereço atualizado](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Figura 6**: Any fornecedores informações podem ser exibidas e seu endereço atualizado ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Etapa 3: Exibir apenas as informações do fornecedor selecionado s

Atualmente, nossa página exibe as informações para todos os fornecedores, independentemente se um fornecedor específico foi selecionado o `Suppliers` DropDownList. Para exibir apenas as informações do fornecedor para o fornecedor selecionado precisamos adicionar outro ObjectDataSource para nossa página, que recupera informações sobre um fornecedor específico.

Adicionar um novo ObjectDataSource para a página, nomeando- `SingleSupplierDataSource`. De marca inteligente, clique no link configurar fonte de dados e fazer com que ele use o `SuppliersBLL` classe s `GetSupplierBySupplierID(supplierID)` método. Assim como acontece com o `AllSuppliersDataSource` ObjectDataSource, ter o `SingleSupplierDataSource` ObjectDataSource s `Update()` método mapeado para o `SuppliersBLL` classe s `UpdateSupplierAddress` método.


[![Configurar o SingleSupplierDataSource ObjectDataSource para usar o método GetSupplierBySupplierID(supplierID)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Figura 7**: configurar o `SingleSupplierDataSource` ObjectDataSource para usar o `GetSupplierBySupplierID(supplierID)` método ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


Em seguida, podemos re solicitado para especificar a origem de parâmetro para o `GetSupplierBySupplierID(supplierID)` método s `supplierID` parâmetro de entrada. Como queremos mostrar as informações para o fornecedor selecionado na lista suspensa, use o `Suppliers` DropDownList s `SelectedValue` propriedade como a origem do parâmetro.


[![Usar DropDownList fornecedores como a origem do parâmetro supplierID](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Figura 8**: Use o `Suppliers` DropDownList como o `supplierID` origem do parâmetro ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


Mesmo com essa segunda ObjectDataSource adicionado, o controle DetailsView está configurado para usar sempre o `AllSuppliersDataSource` ObjectDataSource. Precisamos adicionar lógica para ajustar a fonte de dados usada por DetailsView dependendo do `Suppliers` DropDownList item selecionado. Para fazer isso, crie um `SelectedIndexChanged` DropDownList fornecedores manipulador de eventos. Isso pode ser criado facilmente clicando duas vezes em DropDownList no Designer. Este manipulador de eventos precisa determinar qual fonte de dados a ser usado e deve associar novamente os dados de DetailsView. Isso é feito com o código a seguir:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

O manipulador de eventos começa, determinando se a opção "Mostrar/editar todos os fornecedores" foi selecionada. Se ela foi, ele define o `SupplierDetails` DetailsView s `DataSourceID` para `AllSuppliersDataSource` e retorna o usuário para o primeiro registro no conjunto de fornecedores, definindo o `PageIndex` propriedade como 0. Se, no entanto, o usuário selecionou um fornecedor específico na lista suspensa, o s DetailsView `DataSourceID` é atribuído a `SingleSuppliersDataSource`. Independentemente de quais dados de origem for usada, o `SuppliersDetails` modo é revertido para o modo somente leitura e os dados é vinculada outra vez a DetailsView por uma chamada para o `SuppliersDetails` controle s `DataBind()` método.

Com este manipulador de eventos em vigor, o controle DetailsView agora mostra o fornecedor selecionado, a menos que a opção "Mostrar/editar todos os fornecedores" foi selecionada, caso em que todos os fornecedores podem ser exibidos por meio da interface de paginação. A Figura 9 mostra a página com a opção "Mostrar/editar todos os fornecedores" selecionada. Observe que a interface de paginação está presente, permitindo que o usuário acesse e atualize qualquer fornecedor. A Figura 10 mostra a página com o fornecedor de Ma Maison selecionado. Apenas informações de s Ma Maison são visível e editável nesse caso.


[![Todas as informações de fornecedores podem ser exibidas e editadas](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Figura 9**: todos os fornecedores informações podem ser exibidas e editada ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![Somente as informações do fornecedor selecionado s podem ser exibidas e editadas](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Figura 10**: somente o s informações do fornecedor selecionado pode ser exibido e editada ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> Para este tutorial, o DropDownList e DetailsView controlam s `EnableViewState` deve ser definido como `true` (o padrão) porque o s DropDownList `SelectedIndex` e s a DetailsView `DataSourceID` alterações de propriedade s devem ser lembradas entre postbacks.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Etapa 4: Listando os produtos de fornecedores em um GridView editável

Com DetailsView completa, nossa próxima etapa é incluir um GridView editável que lista os produtos fornecidos pelo fornecedor selecionado. Este GridView deve permitir a edição de somente o `ProductName` e `QuantityPerUnit` campos. Além disso, se o usuário visitar a página for de um fornecedor específico, ele só deve permitir atualizações para os produtos que são *não* descontinuado. Para fazer isso, precisamos adicionar primeiro uma sobrecarga o `ProductsBLL` classe s `UpdateProducts` método que usa apenas o `ProductID`, `ProductName`, e `QuantityPerUnit` campos como entradas. Podemos ve percorrida esse processo com antecedência vários tutoriais, deixe-s olhar para o código aqui, o que deve ser adicionado ao `ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Com essa sobrecarga criada, podemos re pronto para adicionar o controle GridView e seu ObjectDataSource associado. Adicionar um novo GridView à página, defina seu `ID` propriedade `ProductsBySupplier`e configure-o para usar um novo ObjectDataSource denominado `ProductsBySupplierDataSource`. Como queremos que este GridView para listar os produtos pelo fornecedor selecionado, use o `ProductsBLL` classe s `GetProductsBySupplierID(supplierID)` método. Também mapear o `Update()` método para o novo `UpdateProduct` sobrecarga que acabamos de criar.


[![Configurar o ObjectDataSource para usar a sobrecarga de UpdateProduct recém-criado](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Figura 11**: configurar o ObjectDataSource para usar o `UpdateProduct` sobrecarregar acabou de criar ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


Podemos re solicitado a selecionar a origem de parâmetro para o `GetProductsBySupplierID(supplierID)` método s `supplierID` parâmetro de entrada. Como queremos mostrar os produtos para o fornecedor selecionado em DetailsView, use o `SuppliersDetails` controle DetailsView s `SelectedValue` propriedade como a origem do parâmetro.


[![Use a propriedade SelectedValue SuppliersDetails DetailsView s como a origem de parâmetro](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Figura 12**: Use o `SuppliersDetails` DetailsView s `SelectedValue` propriedade como a origem do parâmetro ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


Retornando a GridView, remova todos os campos de GridView com exceção de `ProductName`, `QuantityPerUnit`, e `Discontinued`, marcando o `Discontinued` CheckBoxField como somente leitura. Além disso, verifique a opção Habilitar edição de marca inteligente s GridView. Depois que essas alterações foram feitas, a marcação declarativa para a GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Assim como acontece com nosso ObjectDataSources anterior, este um s `OldValuesParameterFormatString` está definida como `original_{0}`, que causará problemas ao tentar atualizar um nome de produto s ou quantidade por unidade. Remova a propriedade da sintaxe declarativa completamente ou defina-o para seu padrão, `{0}`.

Com essa configuração for concluída, nossa página agora lista os produtos fornecidos pelo fornecedor selecionado em GridView (consulte a Figura 13). No momento *qualquer* nome do produto s ou quantidade por unidade pode ser atualizada. No entanto, precisamos atualizar nossa lógica de página para que essa funcionalidade é proibida para produtos descontinuados para usuários associados a um fornecedor específico. Que abordaremos essa parte final na etapa 5.


[![Os produtos fornecidos pelo fornecedor selecionado são exibidos](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Figura 13**: os produtos fornecidos pelo fornecedor selecionado são exibidas ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> Com a adição desse GridView editável o `Suppliers` DropDownList s `SelectedIndexChanged` manipulador de eventos deve ser atualizado para retornar o GridView para um estado somente leitura. Caso contrário, se um fornecedor diferente for selecionado no meio de edição de informações de produto, o índice correspondente em GridView para o novo fornecedor também será editável. Para evitar isso, basta definir o GridView s `EditIndex` propriedade `-1` no `SelectedIndexChanged` manipulador de eventos.


Além disso, lembre-se de que é importante que o GridView s estado de exibição ser habilitado (o comportamento padrão). Se você definir o GridView s `EnableViewState` propriedade `false`, você corre o risco de ter acidentalmente excluir ou editar registros de usuários simultâneos. Consulte [Aviso: problema de simultaneidade com ASP.NET 2.0 GridViews/DetailsView/FormViews que suporte a edição e/ou excluindo e cujo estado de exibição é desativado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obter mais informações.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Etapa 5: Não permitir edição Descontinuado produtos quando exibir/editar todos os fornecedores é não selecionado

Enquanto o `ProductsBySupplier` GridView é totalmente funcional, atualmente concede muito mais acesso aos usuários que são de um fornecedor específico. Por nossas regras de negócio, esses usuários não devem ser capazes de atualizar os produtos descontinuados. Para impor isso, podemos pode ocultar (ou desabilitar) o botão Editar dessas linhas de GridView com produtos descontinuados quando a página for visitada por um usuário de um fornecedor.

Criar um manipulador de eventos para o s GridView `RowDataBound` eventos. Este manipulador de eventos, precisamos determinar se o usuário é associado um fornecedor específico, que, para este tutorial, pode ser determinado verificando o s fornecedores DropDownList `SelectedValue` propriedade - se ele algo diferente de -1 e, em seguida, o usuário é associado a um fornecedor específico. Para esses usuários, em seguida, precisamos determinar se o produto foi descontinuado. Podemos pode obter uma referência para o valor real `ProductRow` instância associada à linha GridView por meio de `e.Row.DataItem` propriedade, conforme discutido no [ *exibindo informações de resumo em GridView s rodapé* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) tutorial. Se o produto foi descontinuado, podemos pode obter uma referência de programação para o botão de edição em GridView s CommandField usando as técnicas discutidas no tutorial anterior, [ *Adicionar cliente confirmação quando excluindo* ](adding-client-side-confirmation-when-deleting-vb.md). Se houver uma referência, em seguida, é possível ocultar ou desabilitar o botão.


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

Com esse evento manipulador no local, quando visitar essa página como um usuário de um fornecedor específico os produtos que foram descontinuados não são editáveis, como o botão Editar está oculta para esses produtos. Por exemplo, s chefe Anton mistura para Gumbo é um produto descontinuado para o fornecedor de New Orleans Cajun Delights. Ao visitar a página para esse fornecedor específico, o botão Editar para este produto está oculta da Vista (consulte a Figura 14). No entanto, ao visitar usando o "Mostrar/editar todos os fornecedores", o botão Editar está disponível (consulte a Figura 15).


[![Para usuários específicos do fornecedor no botão Editar para s chefe Anton mistura para Gumbo está oculto](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Figura 14**: para usuários específicos do fornecedor no botão Editar para s chefe Anton mistura para Gumbo está oculto ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![Para exibir/editar todos os usuários de fornecedores, é exibido no botão Editar para s chefe Anton mistura para Gumbo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Figura 15**: para exibir/editar todos os fornecedores usuários, o botão Editar para s chefe Anton mistura para Gumbo é exibida ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Verificação de direitos de acesso na camada de lógica de negócios

Neste tutorial, o ASP.NET página manipula toda a lógica em relação a quais informações o usuário pode ver e que produtos que ele pode atualizar. Idealmente, essa lógica também estarão presente na camada de lógica de negócios. Por exemplo, o `SuppliersBLL` classe s `GetSuppliers()` método (que retorna todos os fornecedores) pode incluir uma verificação para garantir que o usuário conectado no momento é *não* associado a um fornecedor específico. Da mesma forma, o `UpdateSupplierAddress` método pode incluir uma verificação para garantir que o usuário conectado no momento ou trabalhou para a empresa (e, portanto, pode atualizar todas as informações de endereço de fornecedores) ou está associado com o fornecedor de dados está sendo atualizados.

Eu não incluiu essas verificações de camada BLL aqui porque em nosso tutorial os direitos de usuário s são determinados pela DropDownList em uma página, que não é possível acessar as classes BLL. Ao usar o sistema de associação ou um dos esquemas de autenticação de fora da caixa fornecidos pelo ASP.NET (como a autenticação do Windows), o conectado no momento no usuário s informações e funções podem ser acessadas de BLL, tornando esse acesso direitos verifica possíveis em ambas as camadas de apresentação e BLL.

## <a name="summary"></a>Resumo

A maioria dos sites que fornecem as contas de usuário necessário personalizar a interface de modificação de dados com base no usuário conectado. Usuários administrativos poderá excluir e editar qualquer registro, enquanto que os usuários não administrativos podem ser limitados a apenas atualizar ou excluir registros criados por eles mesmos. Pode ser qualquer cenário, os dados de controles da Web, ObjectDataSource, e classes de camada de lógica comercial podem ser estendidos para adicionar ou negar determinadas funcionalidades com base no usuário conectado. Neste tutorial, vimos como limitar os dados visíveis e editáveis dependendo se o usuário foi associado um fornecedor específico ou se elas funcionaram para nossa empresa.

Este tutorial conclui nossa análise do inserindo, atualizando e excluindo dados usando os controles GridView, DetailsView e FormView. Começando com o tutorial Avançar, voltaremos nossa atenção à adição de paginação e a classificação de suporte.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-client-side-confirmation-when-deleting-vb.md)
