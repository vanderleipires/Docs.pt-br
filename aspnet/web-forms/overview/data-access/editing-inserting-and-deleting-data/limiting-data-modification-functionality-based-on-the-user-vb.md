---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Limitando a funcionalidade de modificação de dados com base no usuário (VB) | Microsoft Docs
author: rick-anderson
description: Em um aplicativo web que permite aos usuários editarem dados, contas de usuário diferentes podem ter diferentes privilégios de edição de dados. Neste tutorial, examinaremos como t...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: 23e23c288ccceab7f7e1c07aa9a902bef4045de0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836798"
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Limitando a funcionalidade de modificação de dados com base no usuário (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) ou [baixar PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> Em um aplicativo web que permite aos usuários editarem dados, contas de usuário diferentes podem ter diferentes privilégios de edição de dados. Neste tutorial, examinaremos como ajustar dinamicamente os recursos de modificação de dados com base no usuário visitante.


## <a name="introduction"></a>Introdução

Um número de aplicativos da web dá suporte a contas de usuário e fornece opções diferentes, relatórios e funcionalidade com base no usuário conectado. Por exemplo, com nossos tutoriais queremos permitir que os usuários de fazer logon para os site e atualizar informações gerais sobre seus produtos - seu nome e a quantidade por unidade, talvez – juntamente com informações de fornecedor, como o nome da empresa, as empresas de fornecedor endereço, informações da pessoa de contato s e assim por diante. Além disso, queremos incluir algumas contas de usuário para as pessoas de nossa empresa, para que eles podem fazer logon e atualizar informações do produto, como unidades em estoque, nível de estoque e assim por diante. Nosso aplicativo web também pode permitir que usuários anônimos visite (pessoas que não fizeram logon), mas limitar para exibir apenas os dados. Com esse usuário conta sistema em vigor, queremos seria que os dados de controles da Web em nossas páginas ASP.NET para oferecer a inserir, editar e excluir recursos apropriados para o usuário conectado no momento.

Neste tutorial, examinaremos como ajustar dinamicamente os recursos de modificação de dados com base no usuário visitante. Em particular, vamos criar uma página que exibe as informações de fornecedores em um DetailsView editável junto com um GridView que lista os produtos fornecidos pelo fornecedor. Se o usuário visitar a página for de nossa empresa, eles podem: exibir quaisquer informações de fornecedor s; Editar seu endereço de; e editar as informações para qualquer produto fornecida pelo fornecedor. Se, no entanto, o usuário for de uma determinada empresa, eles só podem exibir e editar suas próprias informações de endereço e só pode editar seus produtos que não foram marcados como descontinuado.


[![Um usuário de nossa empresa pode editar qualquer informação de s do fornecedor](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Figura 1**: um usuário de nossa empresa pode editar qualquer fornecedor s informações ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![Um usuário de um fornecedor específico somente podem exibir e editar suas informações](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Figura 2**: um usuário de um determinado fornecedor pode somente exibir e editar suas informações de ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


Permitir que o s começar!

> [!NOTE]
> O ASP.NET 2.0 s sistema de associação fornece uma plataforma padronizada e extensível para criar, gerenciar e validar as contas de usuário. Uma vez que um exame do sistema de associação está além do escopo destes tutoriais, este tutorial em vez disso, "fakes" associação, permitindo que os visitantes anônimos escolher se eles são de um fornecedor específico ou de nossa empresa. Para obter mais informações sobre associação, consulte a minha [examinando o ASP.NET 2.0 s associação, funções e perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) série de artigos.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Etapa 1: Permitir que o usuário especifique seus direitos de acesso

Em um aplicativo web do mundo real, um informações de conta do usuário s incluiria se eles trabalharam em conjunto para nossa empresa ou para um determinado fornecedor, e essas informações seria acessíveis por meio de programação de nossas páginas ASP.NET depois que o usuário fez logon no site. Essas informações poderiam ser capturadas por meio do sistema de funções do ASP.NET 2.0 s, como informações de conta no nível de usuário por meio do sistema de perfil ou alguns meios personalizados.

Como o objetivo deste tutorial é demonstrar a ajustar os recursos de modificação de dados com base no usuário conectado e não é destinado a associação de s showcase ASP.NET 2.0, funções e sistemas de perfil, vamos usar um mecanismo muito simples para determinar o recursos para o usuário visitar a página - DropDownList da qual o usuário pode indicar se eles devem ser capazes de exibir e editar qualquer uma das informações de fornecedores ou, Alternativamente, o que informações de fornecedor específico s, eles podem exibir e editar. Se o usuário indicar que ela pode exibir e editar todas as informações de fornecedor (o padrão), ela pode percorrer todos os fornecedores, editar qualquer informação de endereço do fornecedor s e edite o nome e a quantidade por unidade para qualquer produto fornecida pelo fornecedor selecionado. Se o usuário indica que ela só pode exibir e editar um fornecedor específico, no entanto, em seguida, ela só pode exibir os detalhes e produtos para que um fornecedor e só pode atualizar o nome e a quantidade de informações de unidade para esses produtos que estão por *não* descontinuado.

Nossa primeira etapa neste tutorial, em seguida, é criar esse DropDownList e preenchê-lo com os fornecedores no sistema. Abra o `UserLevelAccess.aspx` página na `EditInsertDelete` pasta, adicionar uma DropDownList cujo `ID` estiver definida como `Suppliers`e associar esse DropDownList a um novo ObjectDataSource chamado `AllSuppliersDataSource`.


[![Criar um novo ObjectDataSource chamado AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Figura 3**: criar um novo ObjectDataSource nomeado `AllSuppliersDataSource` ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


Como queremos que essa DropDownList para incluir todos os fornecedores, configure o ObjectDataSource para invocar o `SuppliersBLL` classe s `GetSuppliers()` método. Além disso, verifique o s ObjectDataSource `Update()` método é mapeado para o `SuppliersBLL` classe s `UpdateSupplierAddress` método, como este ObjectDataSource também será usado pelo DetailsView incluiremos na etapa 2.

Depois de concluir o assistente ObjectDataSource, conclua as etapas ao configurar o `Suppliers` DropDownList, de modo que ele mostra a `CompanyName` campo de dados e o `SupplierID` campo de dados como o valor para cada `ListItem`.


[![Configurar a DropDownList fornecedores para usar o CompanyName e campos de dados SupplierID](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Figura 4**: configurar o `Suppliers` DropDownList para usar o `CompanyName` e `SupplierID` campos de dados ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


Neste ponto, DropDownList lista os nomes de empresa dos fornecedores no banco de dados. No entanto, também precisamos incluir uma opção "Mostrar/editar todos os fornecedores" ao DropDownList. Para fazer isso, defina a `Suppliers` s DropDownList `AppendDataBoundItems` propriedade `true` e, em seguida, adicione um `ListItem` cujo `Text` propriedade é "Mostrar/editar todos os fornecedores" e cujo valor é `-1`. Isso pode ser adicionado diretamente por meio de marcação declarativa ou por meio do Designer indo para a janela Propriedades e clicar nas elipses em s DropDownList `Items` propriedade.

> [!NOTE]
> Voltar para o [ *filtragem de mestre/detalhes com uma DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) tutorial para uma discussão mais detalhada sobre como adicionar um item Selecionar tudo para uma associação de dados DropDownList.


Após o `AppendDataBoundItems` propriedade foi definida e o `ListItem` adicionado, a marcação declarativa de s DropDownList deve parecer com:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

Figura 5 mostra uma captura de tela de nosso progresso atual, quando visualizado por meio de um navegador.


[![Fornecedores DropDownList contém uma apresentação ListItem todos, mais um para cada fornecedor](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Figura 5**: O `Suppliers` DropDownList contém um Mostrar tudo `ListItem`, além de um para cada fornecedor ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


Como queremos atualizar a interface do usuário imediatamente depois que o usuário alterou sua seleção, defina as `Suppliers` s DropDownList `AutoPostBack` propriedade `true`. Na etapa 2, criaremos um controle DetailsView que mostrará as informações para o supplier(s) com base na seleção DropDownList. Em seguida, na etapa 3, vamos criar um manipulador de eventos para este s DropDownList `SelectedIndexChanged` evento, em que vamos adicionar código que associa as informações de fornecedor apropriadas a DetailsView com base no fornecedor selecionado.

## <a name="step-2-adding-a-detailsview-control"></a>Etapa 2: Adicionando um controle DetailsView

Permitir que o s usem um DetailsView para mostrar informações de fornecedor. Para o usuário que pode exibir e editar todos os fornecedores, DetailsView oferecerá suporte a paginação, permitindo que o usuário percorrer a um registro do fornecedor informações cada vez. Se o usuário trabalha para um determinado fornecedor, no entanto, DetailsView mostrará apenas esse fornecedor específico informações s e não incluirá uma interface de paginação. Em ambos os casos, DetailsView precisa permitir que o usuário edite o fornecedor s endereço, cidade e campos de país.

Adicionar um DetailsView para a página abaixo de `Suppliers` DropDownList, defina seu `ID` propriedade a ser `SupplierDetails`e associá-lo para o `AllSuppliersDataSource` ObjectDataSource criado na etapa anterior. Em seguida, marque as caixas de seleção Habilitar paginação e habilitar a edição da marca inteligente DetailsView s.

> [!NOTE]
> Se você don t vir uma opção de habilitar edição em s DetailsView inteligente marcá-lo s porque você não especificou o s ObjectDataSource `Update()` método para o `SuppliersBLL` classe s `UpdateSupplierAddress` método. Reserve um tempo para voltar e alterar essa configuração, após o qual a opção de habilitar edição deve aparecer na marca inteligente DetailsView s.


Uma vez que o `SuppliersBLL` classe s `UpdateSupplierAddress` método só aceita quatro parâmetros - `supplierID`, `address`, `city`, e `country` -modificar o s DetailsView BoundFields para que o `CompanyName` e `Phone` BoundFields são somente leitura. Além disso, remova o `SupplierID` BoundField totalmente. Por fim, o `AllSuppliersDataSource` ObjectDataSource atualmente tem seu `OldValuesParameterFormatString` propriedade definida como `original_{0}`. Reserve um tempo para remover essa configuração de propriedade de totalmente a sintaxe declarativa ou defini-lo como o valor padrão, `{0}`.

Depois de configurar o `SupplierDetails` DetailsView e `AllSuppliersDataSource` ObjectDataSource, temos a seguinte marcação declarativa:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

Neste momento pode ser paginada DetailsView por meio do e as informações de endereço do fornecedor selecionado s podem ser atualizadas, independentemente da seleção feita na `Suppliers` DropDownList (veja a Figura 6).


[![Nenhuma informação de fornecedores pode ser exibida e atualizados do seu endereço](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Figura 6**: Any fornecedores informações podem ser exibidas e seu endereço atualizado ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Etapa 3: Exibir apenas as informações do fornecedor selecionado s

Nossa página no momento, exibe as informações para todos os fornecedores, independentemente se um fornecedor particular foi selecionado o `Suppliers` DropDownList. Para exibir apenas as informações de fornecedor para fornecedor selecionado, precisamos adicionar outro ObjectDataSource para nossa página, que recupera informações sobre um determinado fornecedor.

Adicionar um novo ObjectDataSource para a página, nomeando- `SingleSupplierDataSource`. Na sua marca inteligente, clique no link configurar fonte de dados e fazer com que ele use o `SuppliersBLL` classe s `GetSupplierBySupplierID(supplierID)` método. Assim como acontece com o `AllSuppliersDataSource` ObjectDataSource, ter o `SingleSupplierDataSource` s ObjectDataSource `Update()` método mapeado para o `SuppliersBLL` classe s `UpdateSupplierAddress` método.


[![Configurar o SingleSupplierDataSource ObjectDataSource para usar o método GetSupplierBySupplierID(supplierID)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Figura 7**: configurar o `SingleSupplierDataSource` ObjectDataSource para usar o `GetSupplierBySupplierID(supplierID)` método ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


Em seguida, podemos re solicitado para especificar a origem do parâmetro para o `GetSupplierBySupplierID(supplierID)` método s `supplierID` parâmetro de entrada. Como queremos mostrar as informações para o fornecedor selecionado na lista suspensa, use o `Suppliers` DropDownList s `SelectedValue` propriedade como a origem do parâmetro.


[![Usar a DropDownList fornecedores como a origem do parâmetro supplierID](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Figura 8**: Use o `Suppliers` DropDownList como o `supplierID` origem do parâmetro ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


Mesmo com essa segunda ObjectDataSource adicionado, o controle DetailsView está configurado para usar sempre o `AllSuppliersDataSource` ObjectDataSource. Precisamos adicionar lógica para ajustar a fonte de dados usada pelo DetailsView dependendo a `Suppliers` DropDownList item selecionado. Para fazer isso, crie um `SelectedIndexChanged` DropDownList fornecedores no manipulador de eventos. Isso pode ser criado com mais facilidade clicando duas vezes a DropDownList no Designer. Esse manipulador de eventos precisa determinar qual fonte de dados a ser usado e deve associar novamente os dados a serem DetailsView. Isso é feito com o código a seguir:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

O manipulador de eventos começa determinando se a opção "Mostrar/editar todos os fornecedores" foi selecionada. Se foi, ele define o `SupplierDetails` s DetailsView `DataSourceID` à `AllSuppliersDataSource` e retorna o usuário para o primeiro registro no conjunto de fornecedores, definindo o `PageIndex` propriedade como 0. Se, no entanto, o usuário tiver selecionado um fornecedor específico na lista suspensa, o s DetailsView `DataSourceID` for atribuído a `SingleSuppliersDataSource`. Independentemente de quais dados de origem for usada, o `SuppliersDetails` modo é revertido para o modo somente leitura e os dados são reassociados ao DetailsView por uma chamada para o `SuppliersDetails` controle s `DataBind()` método.

Com este manipulador de eventos em vigor, o controle DetailsView agora mostra o fornecedor selecionado, a menos que a opção "Mostrar/editar todos os fornecedores" foi selecionada, caso em que todos os fornecedores podem ser exibidos por meio da interface de paginação. Figura 9 mostra a página com a opção "Mostrar/editar todos os fornecedores" selecionada. Observe que a interface de paginação estiver presente, permitindo que o usuário visitar e atualizar qualquer fornecedor. Figura 10 mostra a página com o fornecedor de Ma Maison selecionado. Apenas as informações de s Ma Maison são visível e editável nesse caso.


[![Todas as informações de fornecedores podem ser exibidas e editadas](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Figura 9**: todos os fornecedores informações pode ser exibido e editado ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![Somente as informações do fornecedor selecionado s podem ser exibidas e editadas](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Figura 10**: somente o s informações do fornecedor selecionado pode ser exibido e editado ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> Para este tutorial, a DropDownList e DetailsView controlam s `EnableViewState` deve ser definida como `true` (o padrão) porque o s DropDownList `SelectedIndex` e o s DetailsView `DataSourceID` alterações de propriedade s devem ser memorizadas entre postbacks.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Etapa 4: Listagem de produtos de fornecedores em um GridView editável

Com DetailsView completa, nossa próxima etapa é incluir um GridView editável que lista os produtos fornecidos pelo fornecedor selecionado. Este GridView deve permitir a edição somente o `ProductName` e `QuantityPerUnit` campos. Além disso, se o usuário visitar a página for de um fornecedor específico, ele só deve permitir atualizações para esses produtos que são *não* descontinuado. Para fazer isso, será necessário primeiro adicionar uma sobrecarga da `ProductsBLL` classe s `UpdateProducts` método que usa apenas o `ProductID`, `ProductName`, e `QuantityPerUnit` campos como entradas. Podemos var percorreu esse processo com antecedência em vários tutoriais, então deixe s olhar aqui, o código que deve ser adicionado ao `ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Com essa sobrecarga criada, podemos está pronto para adicionar o controle GridView e seu associado ObjectDataSource. Adicione um novo GridView à página, defina suas `ID` propriedade para `ProductsBySupplier`e configurá-lo para usar um novo ObjectDataSource chamado `ProductsBySupplierDataSource`. Como queremos que essa GridView para listar esses produtos pelo fornecedor selecionado, use o `ProductsBLL` classe s `GetProductsBySupplierID(supplierID)` método. Mapear também o `Update()` método para o novo `UpdateProduct` sobrecarga que acabamos de criar.


[![Configurar o ObjectDataSource para usar a sobrecarga de UpdateProduct acabou de criar](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Figura 11**: configurar o ObjectDataSource para usar o `UpdateProduct` sobrecarregar acabou de criar ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


Podemos re solicitado a selecionar a fonte de parâmetro para o `GetProductsBySupplierID(supplierID)` método s `supplierID` parâmetro de entrada. Como queremos mostrar os produtos para o fornecedor selecionado em DetailsView, use o `SuppliersDetails` controle DetailsView s `SelectedValue` propriedade como a origem do parâmetro.


[![Use a propriedade SelectedValue SuppliersDetails DetailsView s como a origem do parâmetro](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Figura 12**: Use o `SuppliersDetails` s DetailsView `SelectedValue` propriedade como a origem do parâmetro ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


Retornando para o controle GridView, remova todos os campos, com exceção de GridView `ProductName`, `QuantityPerUnit`, e `Discontinued`, a marcação a `Discontinued` CheckBoxField como somente leitura. Além disso, verifique a opção Habilitar edição da marca inteligente s GridView. Depois de fazer essas alterações, a marcação declarativa para GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Assim como acontece com nosso ObjectDataSources anterior, este um s `OldValuesParameterFormatString` estiver definida como `original_{0}`, que causará problemas ao tentar atualizar um nome de produto s ou uma quantidade por unidade. Remover esta propriedade da sintaxe declarativa completamente ou defina-o como seu padrão, `{0}`.

Com essa configuração for concluída, nossa página agora lista os produtos fornecidos pelo fornecedor selecionado em um GridView (veja a Figura 13). No momento *qualquer* nome do produto s ou quantidade por unidade pode ser atualizada. No entanto, precisamos atualizar nossa lógica de página para que essa funcionalidade é proibida para produtos descontinuados para os usuários associados a um determinado fornecedor. Que abordaremos essa peça final da etapa 5.


[![Os produtos fornecidos pelo fornecedor selecionado são exibidos](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Figura 13**: os produtos fornecidos pelo fornecedor selecionado são exibidas ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> Com a adição desse GridView editável a `Suppliers` DropDownList s `SelectedIndexChanged` manipulador de eventos deve ser atualizado para retornar o GridView para um estado somente leitura. Caso contrário, se um fornecedor diferente é selecionado no meio de edição de informações do produto, o índice correspondente em GridView para o novo fornecedor também será editável. Para evitar isso, basta definir o s GridView `EditIndex` propriedade para `-1` no `SelectedIndexChanged` manipulador de eventos.


Além disso, lembre-se de que é importante que o GridView, o estado de exibição s ser habilitada (o comportamento padrão). Se você definir o s GridView `EnableViewState` propriedade para `false`, você corre o risco de ter acidentalmente excluir ou editar registros de usuários simultâneos. Ver [Aviso: problema de simultaneidade com o ASP.NET 2.0 GridViews/DetailsView/FormViews esse suporte de edição e/ou exclusão e cujo estado de exibição está desabilitado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obter mais informações.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Etapa 5: Não permitir a edição para Descontinuado produtos quando mostrar/editar todos os fornecedores não é selecionada

Enquanto o `ProductsBySupplier` GridView é totalmente funcional, ele atualmente concede muito mais acesso aos usuários que são de um fornecedor específico. Por nossas regras comerciais, esses usuários não devem ser capazes de atualizar produtos descontinuados. Para impor isso, podemos ocultar (ou desabilitar) o botão Editar as linhas de GridView com produtos descontinuados quando a página está sendo visitada por um usuário de um fornecedor.

Criar um manipulador de eventos para o s GridView `RowDataBound` eventos. Nesse manipulador de eventos, precisamos determinar se o usuário é associado um determinado fornecedor, que, para este tutorial, pode ser determinado, verificando o s fornecedores DropDownList `SelectedValue` propriedade – se ele s algo diferente de -1 e, em seguida, o usuário é associado a um determinado fornecedor. Para esses usuários, em seguida, precisamos determinar se o produto foi descontinuado. Capturamos uma referência para o real `ProductRow` instância associada à linha GridView via o `e.Row.DataItem` propriedade, conforme discutido na [ *exibindo informações de resumo em s GridView rodapé* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) tutorial. Se o produto foi descontinuado, podemos pode Obtenha uma referência através de programação para o botão Editar em s GridView CommandField usando as técnicas discutidas no tutorial anterior, [ *adicionando cliente confirmação quando excluindo* ](adding-client-side-confirmation-when-deleting-vb.md). Assim que tivermos uma referência, em seguida, podemos ocultar ou desabilitar o botão.


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

Com esse evento manipulador em vigor, quando visitar esta página como um usuário de um fornecedor específico desses produtos que estão descontinuados não são editáveis, como o botão Editar está oculta para esses produtos. Por exemplo, o chefe Anton s mistura para Gumbo é um produto descontinuado para o fornecedor de New Orleans Cajun prazeres. Ao visitar a página para esse determinado fornecedor, o botão Editar para este produto é oculta de visão (veja a Figura 14). No entanto, ao visitar usando o "Mostrar/editar todos os fornecedores", no botão Editar está disponível (consulte a Figura 15).


[![Para usuários específicos do fornecedor no botão Editar para s chefe Anton mistura para Gumbo está oculto](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Figura 14**: para usuários específicos do fornecedor no botão Editar para s chefe Anton mistura para Gumbo está oculta ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![Para mostrar/editar todos os usuários de fornecedores, no botão Editar para s chefe Anton mistura para Gumbo é exibido](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Figura 15**: para mostrar/editar todos os fornecedores usuários, o botão Editar para s chefe Anton mistura para Gumbo é exibida ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Verificação de direitos de acesso na camada de lógica de negócios

Neste tutorial do ASP.NET, página manipula toda a lógica em relação a quais informações o usuário pode ver e quais produtos que ele pode atualizar. Idealmente, essa lógica também seria presente na camada de lógica de negócios. Por exemplo, o `SuppliersBLL` classe s `GetSuppliers()` método (que retorna todos os fornecedores) pode incluir uma verificação para garantir que o usuário conectado no momento é *não* associado com um fornecedor específico. Da mesma forma, o `UpdateSupplierAddress` método poderia incluir uma verificação para garantir que o usuário conectado no momento ou trabalhou para nossa empresa (e, portanto, pode atualizar todas as informações de endereço de fornecedores) ou está associado com o fornecedor cujos dados estão sendo atualizados.

Eu não incluía essas verificações de camada de BLL aqui como nosso tutorial, os direitos de usuário s são determinados por uma DropDownList em uma página, que as classes BLL não é possível acessar. Ao usar o sistema de associação ou um dos esquemas de autenticação fora da caixa fornecidos pelo ASP.NET (como a autenticação do Windows), o conectado no momento em usuário s informações e as funções podem ser acessados de BLL, tornando esse tipo de acesso direitos verifica possíveis nas camadas BLL e a apresentação.

## <a name="summary"></a>Resumo

A maioria dos sites que fornecem as contas de usuário precisa personalizar a interface de modificação de dados com base no usuário conectado. Os usuários administrativos podem ser capazes de excluir e editar qualquer registro, enquanto que os usuários não administrativos podem ser limitados a apenas a atualização ou exclusão de registros criados por eles mesmos. Seja qual for o cenário pode ser, os controles da Web, ObjectDataSource, dados e classes de camada de lógica comercial podem ser estendidas para adicionar ou negar determinada funcionalidade com base no usuário conectado. Neste tutorial vimos como limitar os dados visíveis e editáveis, dependendo se o usuário foi associado um determinado fornecedor ou se eles trabalharam para nossa empresa.

Esse tutorial conclui nossa análise do inserindo, atualizando e excluindo dados usando os controles GridView, DetailsView e FormView. Começando com o próximo tutorial, voltaremos nossa atenção à adição de paginação e classificação de suporte.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-client-side-confirmation-when-deleting-vb.md)
