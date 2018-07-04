---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: Uma visão geral de editar e excluir dados no DataList (VB) | Microsoft Docs
author: rick-anderson
description: Enquanto DataList não tiver a edição internos e excluir recursos, este tutorial veremos como criar uma DataList que dá suporte à edição e exclusão e s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5e2a0f672a9be074abd3ab92eb5b36c18d64d1b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364009"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>Uma visão geral de editar e excluir dados no DataList (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe) ou [baixar PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> Embora DataList não tiver a edição internos e excluir recursos, este tutorial veremos como criar uma DataList que dá suporte à edição e exclusão de seus dados subjacentes.


## <a name="introduction"></a>Introdução

No [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial vimos como inserir, atualizar e excluir dados usando a arquitetura do aplicativo, um ObjectDataSource e o GridView, DetailsView e FormView controles. Com o ObjectDataSource e esses controles da Web de três data, implementando interfaces de modificação de dados simples foi muito fácil e envolvidos tique meramente uma caixa de seleção de uma marca inteligente. Sem código necessário a ser gravado.

Infelizmente, DataList não tem interno de editar e excluir recursos inerentes no controle GridView. Essa funcionalidade ausente é devida em parte, ao fato de que DataList é uma relíquia da versão anterior do ASP.NET, quando os controles de fonte de dados declarativa e páginas de modificação de dados sem código não estavam disponíveis. Embora DataList no ASP.NET 2.0 não oferece o mesmo fora da caixa recursos de modificação de dados como GridView, podemos usar técnicas do ASP.NET 1.x para incluir essa funcionalidade. Essa abordagem exige um pouco de código, mas, como veremos neste tutorial, DataList tem algumas propriedades e eventos em vigor para ajudar nesse processo.

Neste tutorial, veremos como criar uma DataList que dá suporte à edição e exclusão de seus dados subjacentes. Tutoriais futuros examinará mais avançadas de edição e exclusão de cenários, incluindo a validação do campo de entrada, normalmente tratando as exceções geradas do acesso a dados ou camadas de lógica comercial e assim por diante.

> [!NOTE]
> Como DataList, o controle Repeater não tem a fora da funcionalidade para inserção, atualização ou exclusão. Embora essa funcionalidade pode ser adicionada, DataList inclui propriedades e eventos não encontrados no repetidor que simplificam a adição de tais recursos. Portanto, este tutorial e outras que examinar a edição e exclusão focará estritamente DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Etapa 1: Criando páginas da Web de tutoriais de edição e exclusão

Antes de começarmos a explorar como atualizar e excluir dados de uma DataList, permitem que s levar alguns instantes para criar as páginas do ASP.NET em nosso projeto de site que precisaremos para este tutorial e as próximas várias pela primeira vez. Comece adicionando uma nova pasta chamada `EditDeleteDataList`. Em seguida, adicione as seguintes páginas do ASP.NET para essa pasta, certificando-se de associar cada página com o `Site.master` página mestra:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Adicione as páginas do ASP.NET para que os tutoriais](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**Figura 1**: adicionar as páginas do ASP.NET para que os tutoriais


Como em outras pastas `Default.aspx` no `EditDeleteDataList` pasta lista os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicionar esse controle de usuário `Default.aspx` arrastando-no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**Figura 2**: Adicione o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))


Por fim, adicione as páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após os relatórios mestre/detalhes com o DataList e Repeater `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para DataList, edição e exclusão de tutoriais.


![O mapa do Site agora inclui entradas para DataList editando e excluindo tutoriais](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**Figura 3**: O mapa do Site agora inclui entradas para DataList editando e excluindo tutoriais


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Etapa 2: Analisando técnicas para atualizando e excluindo dados

Editando e excluindo dados com o GridView é tão fácil, porque, nos bastidores, GridView e ObjectDataSource trabalham em conjunto. Conforme discutido na [examinando os eventos associados inserindo, atualizando e excluindo](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) tutorial, quando um botão de atualização de linha s é clicado, o GridView atribui automaticamente seus campos que são usados na associação de dados bidirecional para a `UpdateParameters` coleção de seu ObjectDataSource e, em seguida, invoca que s ObjectDataSource `Update()` método.

Infelizmente, DataList não fornece essa funcionalidade interna. É nossa responsabilidade para garantir que os valores de usuário s são atribuídos para os parâmetros do ObjectDataSource s e que seu `Update()` método é chamado. Para ajudar-desse esforço, DataList fornece as propriedades e os eventos a seguir:

- **O [ `DataKeyField` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  ao atualizar ou excluir, precisamos poder identificar exclusivamente cada item no DataList. Defina essa propriedade para o campo de chave primária dos dados exibidos. Isso preencherá s DataList [ `DataKeys` coleção](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) com especificado `DataKeyField` valor para cada item DataList.
- **O [ `EditCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  é acionado quando um botão, LinkButton ou ImageButton cujo `CommandName` estiver definida como editar é clicado.
- **O [ `CancelCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  é acionado quando um botão, LinkButton ou ImageButton cujo `CommandName` estiver definida como cancelar é clicado.
- **O [ `UpdateCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  é acionado quando um botão, LinkButton ou ImageButton cujo `CommandName` estiver definida como atualização é clicada.
- **O [ `DeleteCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  é acionado quando um botão, LinkButton ou ImageButton cujo `CommandName` estiver definida como excluir é clicado.

Usando essas propriedades e eventos, há quatro abordagens que podemos usar para atualizar e excluir dados do DataList:

1. **Usando técnicas do ASP.NET 1.x** DataList existiam antes do ASP.NET 2.0 e ObjectDataSources e foi capaz de atualizar e excluir dados inteiramente por meio de programação. Essa técnica ditches ObjectDataSource completamente e requer que podemos associar os dados à DataList diretamente da camada de lógica de negócios, tanto ao recuperar os dados serem exibidos e ao atualizar ou excluir um registro.
2. **Usando um único controle ObjectDataSource, na página para selecionar, atualização e exclusão** enquanto DataList não tiver a edição de GridView s inerente e excluir recursos, s não existe nenhum motivo t, podemos adicioná-los em nós mesmos. Com essa abordagem, podemos usar um ObjectDataSource assim como nos exemplos de GridView, mas deve criar um manipulador de eventos para o s DataList `UpdateCommand` evento onde definimos os parâmetros do ObjectDataSource s e chame seu `Update()` método.
3. **Usando um controle ObjectDataSource para selecionar, mas atualizando e excluindo diretamente em relação a BLL** ao usar a opção 2, precisamos escrever um pouco de código no `UpdateCommand` evento, atribuir valores de parâmetro e assim por diante. Em vez disso, podemos pode ficar com usando o ObjectDataSource para a seleção, mas fazer as chamadas de atualização e exclusão diretamente em relação a BLL (como com opção 1). Na minha opinião, a atualização de dados através de interface diretamente com a BLL leva a um código mais legível de atribuir o s ObjectDataSource `UpdateParameters` e chamar seu `Update()` método.
4. **Usando o meio declarativo por meio de vários ObjectDataSources** as três abordagens anteriores com todos os exigem um pouco de código. Se você d em vez disso, continuar a usar como a sintaxe declarativa muito possível, uma opção final é incluir vários ObjectDataSources na página. O ObjectDataSource primeiro recupera os dados da BLL e associá-lo à DataList. Para a atualização, outro ObjectDataSource é adicionado, mas adicionada diretamente dentro do DataList s `EditItemTemplate`. Para incluir suporte a exclusão, ainda outro ObjectDataSource seria necessários no `ItemTemplate`. Com essa abordagem, eles inseridos uso ObjectDataSource `ControlParameters` declarativamente associar os parâmetros do ObjectDataSource s para os controles de entrada do usuário (em vez de especificá-los programaticamente no DataList s `UpdateCommand` manipulador de eventos). Essa abordagem ainda exige um pouco de código, precisamos chamar o embedded s ObjectDataSource `Update()` ou `Delete()` comando, mas requer muito menor do que com as outras três abordagens. A desvantagem aqui é que os vários ObjectDataSources sobrecarregar a página desvia a atenção a legibilidade geral.

Se forçado a usar apenas uma dessas abordagens, d, escolha a opção 1 porque ele fornece mais flexibilidade e porque DataList foi projetado originalmente para acomodar esse padrão. Embora DataList foi estendido para trabalhar com os controles de fonte de dados do ASP.NET 2.0, ele não tem todos os pontos de extensibilidade ou recursos de dados ASP.NET 2.0 controles da Web (o GridView, DetailsView e FormView) oficiais. Opções de 2 a 4 não estão sem mérito, no entanto.

Isso futuras editando e excluindo tutoriais usará um ObjectDataSource para recuperar os dados para exibir e direcionar as chamadas para a BLL, atualizar e excluir dados (opção 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Etapa 3: Adicionando DataList e configurando seu ObjectDataSource

Neste tutorial, criaremos uma DataList que lista informações sobre o produto e, para cada produto, fornece aos usuários a capacidade de editar o nome e o preço e para excluir completamente o produto. Em particular, estamos recuperará os registros a serem exibidos usando um ObjectDataSource, mas executar a atualização e ações de exclusão pela interface diretamente com a BLL. Antes de nos preocupamos Implementando os recursos de edição e exclusão para DataList, deixe s primeiro obter a página para exibir os produtos em uma interface somente leitura. Desde que criamos ve examinado estas etapas nos tutoriais anteriores, demonstrarei abordá-los rapidamente.

Comece abrindo o `Basics.aspx` página o `EditDeleteDataList` pasta e, na exibição de Design, adicione uma DataList à página. Em seguida, do DataList s marca inteligente, crie um novo ObjectDataSource. Uma vez que estamos trabalhando com dados de produto, configurá-lo para usar o `ProductsBLL` classe. Para recuperar *todos os* produtos, escolha o `GetProducts()` método na guia SELECT.


[![Configurar o ObjectDataSource para usar a classe ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**Figura 4**: configurar o ObjectDataSource para usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))


[![Retornar as informações de produto usando o método GetProducts()](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**Figura 5**: retornar as informações de produto usando o `GetProducts()` método ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))


DataList, como o GridView, não é criado para inserir novos dados; Portanto, selecione opção na lista suspensa na guia Inserir (nenhum). Também escolha (nenhum) para as guias de atualização e exclusão, pois as atualizações e exclusões serão executadas programaticamente por meio da BLL.


[![Confirme se as listas suspensas em s ObjectDataSource INSERT, UPDATE e excluir guias estiverem definidas como (nenhum)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**Figura 6**: Confirme que as listas suspensas no ObjectDataSource s INSERT, UPDATE e excluir guias estão definidas como (nenhum) ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))


Depois de configurar o ObjectDataSource, clique em Concluir, retornando para o Designer. Como podemos ve visto nos exemplos anteriores, ao concluir a configuração ObjectDataSource, o Visual Studio automaticamente cria um `ItemTemplate` para DropDownList, exibindo cada um dos campos de dados. Substituir isso `ItemTemplate` por um que exibe apenas o nome do produto s e o preço. Além disso, defina o `RepeatColumns` propriedade como 2.

> [!NOTE]
> Conforme discutido na *visão geral de inserção, atualização e exclusão de dados* tutorial, ao modificar dados usando o ObjectDataSource nossa arquitetura exige que removemos o `OldValuesParameterFormatString` propriedade de s ObjectDataSource marcação declarativa (ou redefini-lo para seu valor padrão, `{0}`). Neste tutorial, no entanto, estamos usando o ObjectDataSource somente para recuperação de dados. Portanto, não precisamos modificar o s ObjectDataSource `OldValuesParameterFormatString` valor da propriedade (embora ele t prejudicar para fazer isso).


Depois de substituir o padrão DataList `ItemTemplate` com um personalizado, a marcação declarativa em sua página deve ser semelhante ao seguinte:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

Reserve um tempo para exibir nosso progresso através de um navegador. Como mostra a Figura 7, DataList exibe o preço de unidade e o nome do produto para cada produto em duas colunas.


[![Os nomes de produtos e os preços são exibidos em um DataList de duas colunas](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**Figura 7**: os nomes de produtos e os preços são exibidos em um DataList de duas colunas ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))


> [!NOTE]
> DataList tem um número de propriedades que são necessários para o processo de atualização e exclusão, e esses valores são armazenados no estado de exibição. Portanto, quando criar uma DataList que dá suporte a edição ou exclusão de dados, é essencial que o estado de exibição do DataList s ser habilitada.  
>   
>  O leitor astuto deve se lembrar de que fomos capazes de desabilitar o estado de exibição ao criar GridViews, DetailsViews e FormViews editável. Isso ocorre porque os controles da Web do ASP.NET 2.0 podem incluir *estado de controle*, que é o estado persistente entre postbacks, como estado de exibição, mas considerado essencial.


Desabilitar a exibição de estado no GridView simplesmente omite informações de estado trivial, mas mantém o estado de controle (que inclui o estado necessário para editar e excluir). DataList, ter sido criado no período de tempo do ASP.NET 1. x, não utiliza o estado do controle e, portanto, deve ter habilitado o estado de exibição. Consulte [vs do estado do controle. Estado de exibição](https://msdn.microsoft.com/library/1whwt1k7.aspx) para obter mais informações sobre a finalidade do estado do controle e como ela difere do estado de exibição.

## <a name="step-4-adding-an-editing-user-interface"></a>Etapa 4: Adicionando uma Interface de usuário de edição

O controle GridView é composto de uma coleção de campos (BoundFields, CheckBoxFields, TemplateFields e assim por diante). Esses campos podem ajustar sua marcação renderizada, dependendo do seu modo. Por exemplo, no modo somente leitura, um BoundField exibe o valor do campo de dados como texto; no modo de edição, ele processa uma TextBox Web controle cuja `Text` propriedade é atribuída o valor do campo de dados.

DataList, por outro lado, processa seus itens usando modelos. Itens somente leitura são renderizados usando o `ItemTemplate` , enquanto os itens no modo de edição são renderizados por meio de `EditItemTemplate`. Neste ponto, nosso DataList tem apenas um `ItemTemplate`. Para dar suporte à funcionalidade de edição de nível de item, precisamos adicionar um `EditItemTemplate` que contém a marcação a ser exibido para o item editável. Para este tutorial, vamos usar controles da Web de caixa de texto para editar o preço de unidade e o nome do produto s.

O `EditItemTemplate` pode ser criado declarativamente ou por meio do Designer (selecionando a opção de editar modelos da marca inteligente DataList s). Para usar a opção de editar modelos, primeiro clique no link Editar modelos na marca inteligente e, em seguida, selecione o `EditItemTemplate` item da lista suspensa.


[![Otimizado para trabalhar com o DataList s EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**Figura 8**: otimizado para trabalhar com o DataList s `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))


Em seguida, digite o nome de produto: e preço: e, em seguida, arraste dois controles de caixa de texto da caixa de ferramentas para o `EditItemTemplate` interface no Designer. Defina as caixas de texto `ID` propriedades a serem `ProductName` e `UnitPrice`.


[![Adicione uma caixa de texto para o nome do produto s e o preço](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**Figura 9**: adicionar uma caixa de texto para o s o nome do produto e o preço ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))


É necessário associar os valores de campo de dados produto correspondente para o `Text` propriedades das duas caixas de texto. Das marcas inteligentes de caixas de texto, clique no link Editar DataBindings e, em seguida, associe o campo de dados apropriado com o `Text` propriedade, conforme mostrado na Figura 10.

> [!NOTE]
> Ao associar o `UnitPrice` campo de dados para o preço s TextBox `Text` campo, você pode formatá-los como um valor de moeda (`{0:C}`), um número geral (`{0:N}`), ou deixá-lo sem formatação.


![Associar os campos de dados de UnitPrice e ProductName para as propriedades de texto das caixas de texto](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**Figura 10**: associar o `ProductName` e `UnitPrice` campos de dados para o `Text` propriedades das caixas de texto


Observe como a caixa de diálogo Editar DataBindings na Figura 10 faz *não* incluem a caixa de seleção de associação de dados bidirecional que está presente quando você editar um TemplateField no GridView ou DetailsView ou um modelo de FormView. O recurso de vinculação de dados bidirecional permitido o valor inserido no controle de Web de entrada a ser atribuído automaticamente para o correspondente s ObjectDataSource `InsertParameters` ou `UpdateParameters` ao inserir ou atualizar dados. DataList não oferece suporte a vinculação de dados bidirecional como veremos mais adiante neste tutorial, após o usuário faz dela é alterado e está pronto para atualizar os dados, será necessário acessar essas caixas de texto de forma programática `Text` propriedades e passar os valores para o apropriado `UpdateProduct` método no `ProductsBLL` classe.

Por fim, precisamos adicionar atualizar e Cancelar botões para o `EditItemTemplate`. Como vimos na [mestre/detalhes usando uma lista com marcadores de registros de mestre com um DataList de detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial, quando um botão, LinkButton ou ImageButton cujo `CommandName` propriedade está definida é clicado a partir de dentro de um repetidor ou DataList e o S Repeater ou DataList `ItemCommand` é gerado. Para DataList, se o `CommandName` estiver definida como um valor determinado, um evento adicional poderá ser gerado também. Especiais `CommandName` valores de propriedade incluem, entre outros:

- Cancelar gera o `CancelCommand` evento
- Editar gera o `EditCommand` evento
- Atualizar gera o `UpdateCommand` evento

Lembre-se de que esses eventos são acionados *além* o `ItemCommand` eventos.

Adicionar para o `EditItemTemplate` dois controles da Web de botão, um cujo `CommandName` é definido como a atualização e os outros s definido como ' Cancelar '. Depois de adicionar esses dois controles de botão Web Designer deve ser semelhante ao seguinte:


[![Adicionar atualização botões e Cancelar ao EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**Figura 11**: Adicionar atualizar e Cancelar botões para o `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))


Com o `EditItemTemplate` completa sua marcação declarativa de DataList s deve ser semelhante ao seguinte:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Etapa 5: Adicionar os detalhes técnicos para entrar no modo de edição

Neste ponto, nosso DataList tem uma interface de edição definida por meio de seu `EditItemTemplate`; no entanto, há s atualmente nenhuma maneira para um usuário visitar nossa página para indicar que deseja editar as informações de produto s. Precisamos adicionar um botão Editar para cada produto que, quando clicado, processa esse DataList item no modo de edição. Comece adicionando um botão Editar para o `ItemTemplate`, por meio do Designer ou declarativamente. Certifique-se de definir o botão Editar s `CommandName` propriedade para edição.

Depois de adicionar esse botão de edição, reserve um tempo para exibir a página por meio de um navegador. Com esse acréscimo, cada produto na listagem deve incluir um botão Editar.


[![Adicionar atualização botões e Cancelar ao EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**Figura 12**: Adicionar atualizar e Cancelar botões para o `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))


Clicar no botão faz com que um postback, mas faz *não* trazer o produto listagem no modo de edição. Para tornar o produto editável, é preciso:

1. Definir s DataList [ `EditItemIndex` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) para o índice do `DataListItem` cujo botão Editar apenas foi clicado.
2. Associar novamente os dados a serem DataList. Quando DataList é processada novamente, o `DataListItem` cujos `ItemIndex` corresponde à DataList s `EditItemIndex` será renderizado usando seu `EditItemTemplate`.

Desde o s DataList `EditCommand` evento é acionado quando o botão Editar é clicado, crie um `EditCommand` manipulador de eventos com o código a seguir:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

O `EditCommand` manipulador de eventos é passado em um objeto do tipo `DataListCommandEventArgs` como seu segundo parâmetro de entrada, que inclui uma referência para o `DataListItem` cuja edição botão foi clicado (`e.Item`). O manipulador de eventos primeiro define s DataList `EditItemIndex` para o `ItemIndex` da editável `DataListItem` e, em seguida, associa novamente os dados a serem DataList chamando DataList s `DataBind()` método.

Depois de adicionar esse manipulador de eventos, examine a página em um navegador. Clique no botão Editar agora torna o produto clicado editável (consulte a Figura 13).


[![Clicar o botão de edição torna o produto editável](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**Figura 13**: clique no botão Editar torna editável do produto ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Etapa 6: Salvar as alterações do usuário s

Clicar no produto editado s Update ou Cancelar botões não faz nada no momento; Para adicionar essa funcionalidade, precisamos criar manipuladores de eventos para o s DataList `UpdateCommand` e `CancelCommand` eventos. Comece criando o `CancelCommand` manipulador de eventos, que será executada quando o botão Cancelar produto editado s é clicado e ele encarregado de retornar DataList para seu estado de edição previamente.

Para fazer com DataList renderizar todos os seus itens no modo somente leitura, é preciso:

1. Definir s DataList [ `EditItemIndex` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) para o índice de um inexistente `DataListItem` índice. `-1` é uma opção segura, já que o `DataListItem` índices começam em `0`.
2. Associar novamente os dados a serem DataList. Desde que nenhum `DataListItem` `ItemIndex` es correspondem à DataList s `EditItemIndex`, DataList inteiro será renderizado em um modo somente leitura.

Essas etapas podem ser realizadas com o seguinte código de manipulador de eventos:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

Com esse acréscimo, clicando em retorna o botão de cancelar DataList para seu estado de edição previamente.

É o último manipulador de eventos, precisamos concluir o `UpdateCommand` manipulador de eventos. Esse manipulador de eventos precisa:

1. Acessar programaticamente o nome do produto inserida pelo usuário e preço, bem como o produto editado s `ProductID`.
2. Iniciar o processo de atualização chamando apropriado `UpdateProduct` sobrecarga no `ProductsBLL` classe.
3. Definir s DataList [ `EditItemIndex` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) para o índice de um inexistente `DataListItem` índice. `-1` é uma opção segura, já que o `DataListItem` índices começam em `0`.
4. Associar novamente os dados a serem DataList. Desde que nenhum `DataListItem` `ItemIndex` es correspondem à DataList s `EditItemIndex`, DataList inteiro será renderizado em um modo somente leitura.

As etapas 1 e 2 são responsáveis por salvar o usuário alterações s; as etapas 3 e 4 retornam DataList para seu estado previamente edição depois que as alterações foram salvas e são idênticas às etapas executadas no `CancelCommand` manipulador de eventos.

Para obter o nome do produto atualizado e o preço, precisamos usar o `FindControl` método programaticamente a referência TextBox Web controles dentro de `EditItemTemplate`. Também precisamos obter o produto editado s `ProductID` valor. Quando estamos associado inicialmente o ObjectDataSource para DataList, o Visual Studio atribuída DataList s `DataKeyField` propriedade para o valor de chave primária da fonte de dados (`ProductID`). Esse valor, em seguida, pode ser recuperado do DataList s `DataKeys` coleção. Reserve um tempo para garantir que o `DataKeyField` propriedade, de fato, é definida como `ProductID`.

O código a seguir implementa as quatro etapas:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

O manipulador de eventos é iniciado com a leitura no produto s editado `ProductID` do `DataKeys` coleção. Em seguida, as duas caixas de texto na `EditItemTemplate` são referenciados e suas `Text` propriedades armazenadas em variáveis locais, `productNameValue` e `unitPriceValue`. Podemos usar o `Decimal.Parse()` método para ler o valor da `UnitPrice` caixa de texto para que, se o valor inserido tem um símbolo de moeda, ele pode ser convertido ainda corretamente em um `Decimal` valor.

> [!NOTE]
> Os valores da `ProductName` e `UnitPrice` caixas de texto são atribuídas a variáveis productNameValue e unitPriceValue apenas se as propriedades de texto de caixas de texto tem um valor especificado. Caso contrário, um valor de `Nothing` é usado para as variáveis, que tem o efeito de atualizar os dados com um banco de dados `NULL` valor. Ou seja, o nosso código trata converte cadeias de caracteres para o banco de dados vazias `NULL` valores, que é o comportamento padrão da interface de edição nos controles GridView, DetailsView e FormView.


Depois de ler os valores, o `ProductsBLL` classe s `UpdateProduct` método é chamado, passando o nome do produto s, preço, e `ProductID`. O manipulador de eventos for concluído, retornando DataList para seu estado de edição previamente usando a mesma lógica exatamente como no `CancelCommand` manipulador de eventos.

Com o `EditCommand`, `CancelCommand`, e `UpdateCommand` concluir de manipuladores de eventos, um visitante pode editar o nome e o preço de um produto. As figuras 14-16 mostram esse fluxo de trabalho de edição em ação.


[![Quando o primeiro visitando a página, todos os produtos estão no modo somente leitura](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**Figura 14**: quando o primeiro visitando a página, todos os produtos estão no modo somente leitura ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))


[![Para atualizar um s nome ou o preço do produto, clique no botão Editar](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**Figura 15**: para atualizar um produto s nome ou o preço, clique no botão Editar ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))


[![Depois de alterar o valor, clique em atualizar para o retorno para o modo somente leitura](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**Figura 16**: após a alteração do valor, clique em atualizar para o retorno para o modo somente leitura ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Etapa 7: Adicionando recursos de exclusão

As etapas para adicionar recursos de exclusão a um DataList são semelhantes para adicionar recursos de edição. Em resumo, precisamos adicionar um botão Excluir para o `ItemTemplate` que, quando clicado:

1. Lê no produto correspondente s `ProductID` por meio de `DataKeys` coleção.
2. Executa a exclusão chamando o `ProductsBLL` classe s `DeleteProduct` método.
3. Associa novamente os dados a serem DataList.

Permitir que o s comece adicionando um botão Excluir para o `ItemTemplate`.

Quando clicado, um botão cuja `CommandName` está edição, atualização, ou Cancelar gera DataList s `ItemCommand` evento junto com um evento adicional (por exemplo, ao usar Editar o `EditCommand` é gerado, bem). Da mesma forma, qualquer botão, LinkButton ou ImageButton no DataList cujos `CommandName` estiver definida como excluir faz com que o `DeleteCommand` evento seja acionado (juntamente com `ItemCommand`).

Adicionar um botão Excluir ao lado do botão Editar na `ItemTemplate`, definindo seu `CommandName` propriedade para exclusão. Depois de adicionar esse botão controle DataList s `ItemTemplate` sintaxe declarativa deve se parecer com:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

Em seguida, crie um manipulador de eventos para DataList s `DeleteCommand` evento, usando o seguinte código:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

Clicando no botão Excluir causa um postback e dispara DataList s `DeleteCommand` eventos. No manipulador de eventos, o produto clicado s `ProductID` valor é acessado a partir de `DataKeys` coleção. Em seguida, o produto for excluído chamando o `ProductsBLL` classe s `DeleteProduct` método.

Depois de excluir o produto, ele importante que podemos associar novamente os dados a serem DataList (`DataList1.DataBind()`), caso contrário, DataList continuará mostrando o produto que acabou de ser excluído.

## <a name="summary"></a>Resumo

Embora DataList não tem o ponto e clique em Editar e excluir suporte experimentado pelos GridView, com um breve trecho de código ela pode ser aprimorada para incluir esses recursos. Neste tutorial vimos como criar uma listagem de duas colunas de produtos que pode ser excluído e cujo nome e o preço podem ser editados. Adicionar, editando e excluindo o suporte é uma questão de controles da Web apropriados no, incluindo o `ItemTemplate` e `EditItemTemplate`, criando os manipuladores de eventos correspondente, lendo os valores de chave primários e inseridos pelo usuário e fazer interface com os negócios Camada de lógica.

Embora tenhamos adicionado básico de edição e exclusão de recursos para DataList, ele não tem recursos mais avançados. Por exemplo, há nenhuma validação de campo de entrada - se um usuário inserir um preço de excesso caro, uma exceção será lançada pelo `Decimal.Parse` ao tentar converter muito cara em um `Decimal`. Da mesma forma, se houver um problema ao atualizar os dados em que a lógica de negócios ou camadas de acesso de dados o usuário verá a tela de erro padrão. Sem qualquer tipo de confirmação no botão Excluir, a exclusão acidental de um produto é tudo muito provável.

No futuro experiência de tutoriais, veremos como melhorar o usuário de edição.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Zack Jones, Ken Pespisa e Randy Schmidt. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](customizing-the-datalist-s-editing-interface-cs.md)
> [Próximo](performing-batch-updates-vb.md)
