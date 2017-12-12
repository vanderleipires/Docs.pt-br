---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: "Uma visão geral de editar e excluir dados em DataList (c#) | Microsoft Docs"
author: rick-anderson
description: "Enquanto não tiver DataList internos de editar e excluir recursos, este tutorial veremos como criar uma DataList que suporta editar e excluir o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c7a1c7a9839f2f56658618958c234e0064cb427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>Uma visão geral de editar e excluir dados em DataList (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe) ou [baixar PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> Enquanto não tiver DataList internos de editar e excluir recursos, este tutorial veremos como criar uma DataList que dá suporte à edição e exclusão de seus dados subjacentes.


## <a name="introduction"></a>Introdução

No [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial vimos como inserir, atualizar e excluir dados usando a arquitetura do aplicativo, um ObjectDataSource e GridView, DetailsView e FormView controles. Com o ObjectDataSource e esses controles da Web de três data, implementando interfaces de modificação de dados simples foi um snap e envolvidos simplesmente marcar uma caixa de seleção de uma marca inteligente. Nenhum código necessário para ser gravado.

Infelizmente, DataList não tem internos de editar e excluir recursos inerentes o controle GridView. Essa funcionalidade ausente ocorre em parte no fato de que o DataList é um relic da versão anterior do ASP.NET, quando controles de fonte de dados declarativa e páginas de modificação de dados sem código não estavam disponíveis. Enquanto DataList no ASP.NET 2.0 não oferece o mesmo sem a necessidade de capacidades de modificação de dados como o GridView, podemos usar técnicas de 1. x do ASP.NET para incluir essa funcionalidade. Essa abordagem exige um pouco de código, mas, conforme veremos neste tutorial, DataList tem algumas propriedades e eventos em vigor para ajudar neste processo.

Neste tutorial, veremos como criar uma DataList que dá suporte à edição e exclusão de seus dados subjacentes. Tutoriais futuros examinará mais avançadas de edição e exclusão de cenários, incluindo a validação de campo de entrada, normalmente tratamento de exceções geradas do acesso a dados ou as camadas de lógica de negócios e assim por diante.

> [!NOTE]
> Como o DataList controle repetidor não tem fora da funcionalidade de caixa de inserção, atualização ou exclusão. Embora essa funcionalidade pode ser adicionada, DataList inclui propriedades e eventos não encontrados no repetidor que simplificam a adição de recursos. Portanto, este tutorial e outras que examinar a edição e exclusão focará estritamente DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Etapa 1: Criando páginas da Web de tutoriais de edição e exclusão

Antes de começarmos a explorar como atualizar e excluir dados de DataList, permitem s primeiro dedicar um tempo para criar as páginas ASP.NET em nosso projeto de site que vamos precisar para este tutorial e as várias Avançar. Comece adicionando uma nova pasta chamada `EditDeleteDataList`. Em seguida, adicione as seguintes páginas do ASP.NET para a pasta, certificando-se de associar cada página com o `Site.master` página mestre:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Adicionar as páginas ASP.NET para os tutoriais](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image1.png)

**Figura 1**: adicionar as páginas ASP.NET para os tutoriais


Como em outras pastas, `Default.aspx` no `EditDeleteDataList` pasta lista os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicione este controle de usuário para `Default.aspx` arrastando-o no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**Figura 2**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))


Por fim, adicione as páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após os relatórios mestre/detalhes com o controle DataList e repetidor `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

Após a atualização `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para DataList edição e exclusão de tutoriais.


![O mapa do Site agora inclui entradas de DataList edição e exclusão de tutoriais](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**Figura 3**: O mapa de Site agora inclui entradas de DataList edição e exclusão de tutoriais


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Etapa 2: Examinar técnicas para atualizando e excluindo dados

Editar e excluir dados com o GridView são tão fácil porque, nos bastidores, o GridView e ObjectDataSource trabalham em conjunto. Como discutido o [examinando os eventos associados inserindo, atualizando e excluindo](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) tutorial, quando um botão de atualização de linha s é clicado, o GridView atribui automaticamente seus campos usados associação de dados bidirecional com o `UpdateParameters` coleção de seu ObjectDataSource e, em seguida, invoca que s ObjectDataSource `Update()` método.

Infelizmente, DataList não fornece essa funcionalidade interna. É nossa responsabilidade de garantir que os valores de usuário s são atribuídos para os parâmetros de s ObjectDataSource e que seu `Update()` método é chamado. Para nos ajudar nessa tarefa, DataList fornece as propriedades e os eventos a seguir:

- **O [ `DataKeyField` propriedade](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  ao atualizar ou excluir, precisamos poder identificar exclusivamente cada item em DataList. Defina essa propriedade para o campo de chave primária dos dados exibidos. Isso preencherá DataList s [ `DataKeys` coleção](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) com especificado `DataKeyField` valor para cada item de DataList.
- **O [ `EditCommand` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  é acionado quando um botão, LinkButton ou ImageButton cujo `CommandName` está definida como editar é clicado.
- **O [ `CancelCommand` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  é acionado quando um botão, LinkButton ou ImageButton cujo `CommandName` está definida como cancelar é clicado.
- **O [ `UpdateCommand` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  é acionado quando um botão, LinkButton ou ImageButton cujo `CommandName` está definida como atualização é clicada.
- **O [ `DeleteCommand` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  é acionado quando um botão, LinkButton ou ImageButton cujo `CommandName` está definida como Delete é clicado.

Usando essas propriedades e eventos, há quatro abordagens que podemos usar para atualizar e excluir dados de DataList:

1. **Usando técnicas de 1. x ASP.NET** DataList existia antes do ASP.NET 2.0 e ObjectDataSources e foi capaz de atualizar e excluir dados inteiramente por meio de programação. Essa técnica ditches ObjectDataSource completamente e requer que podemos associar os dados para o DataList diretamente da camada de lógica de negócios, ambos recuperar dados para exibir e ao atualizar ou excluir um registro.
2. **Usando um único controle ObjectDataSource na página para selecionar, atualizando e excluindo** enquanto DataList perde o GridView s inerente editar e excluir recursos, s não há nenhum motivo podemos t adicioná-las em nós. Com essa abordagem é usar um ObjectDataSource exatamente como nos exemplos a GridView, mas deve criar um manipulador de eventos para o DataList s `UpdateCommand` evento onde podemos definir os parâmetros de s ObjectDataSource e chame seu `Update()` método.
3. **Usando um controle ObjectDataSource para selecionar, mas atualizando e excluindo diretamente em relação a BLL** ao usar a opção 2, é preciso escrever um pouco de código a `UpdateCommand` evento, atribuir valores de parâmetro e assim por diante. Em vez disso, é possível fique com usando o ObjectDataSource para selecionar, mas verifique as chamadas de atualização e exclusão diretamente com BLL (como com opção 1). Na minha opinião, atualizando dados por interface diretamente com o BLL leva a um código mais legível que atribuir a s ObjectDataSource `UpdateParameters` e chamar sua `Update()` método.
4. **Usando meios declarativa por meio de vários ObjectDataSources** as três abordagens anteriores a todos os exigem um trecho de código. Se você d em vez disso, mantenha usando como sintaxe declarativa muito possível, uma opção final é incluir vários ObjectDataSources na página. O primeiro ObjectDataSource recupera os dados de BLL e associa-a do DataList. Para a atualização, outro ObjectDataSource é adicionado, mas adicionado diretamente dentro do DataList s `EditItemTemplate`. Para incluir suporte a exclusão, ainda ObjectDataSource outro seria necessário no `ItemTemplate`. Com essa abordagem, eles inseridos ObjectDataSource s usam `ControlParameters` declarativamente associar os parâmetros de s ObjectDataSource para os controles de entrada do usuário (em vez de ter que especificá-las programaticamente em DataList s `UpdateCommand` manipulador de eventos). Essa abordagem ainda exige um pouco de código, precisamos chamar o embedded s ObjectDataSource `Update()` ou `Delete()` comando mas exige muito menor do que com os outros três abordagens. A desvantagem aqui é que o ObjectDataSources vários sobrecarregar a página desviar a atenção da legibilidade geral.

Se forçado a usar apenas uma dessas abordagens, d, escolha a opção 1 porque ele fornece maior flexibilidade e como DataList foi originalmente projetado para acomodar esse padrão. Enquanto o DataList foi estendido para trabalhar com os controles de fonte de dados do ASP.NET 2.0, ele não tem todos os pontos de extensibilidade de recursos dos dados de ASP.NET 2.0 oficiais controles da Web (o GridView, DetailsView e FormView). Opções de 2 a 4 têm núcleo, embora.

Isso futuras edição e exclusão tutoriais usará um ObjectDataSource para recuperar os dados para exibir e direcionar chamadas para BLL para atualizar e excluir dados (opção 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Etapa 3: Adicionando DataList e configurando seu ObjectDataSource

Neste tutorial, criaremos uma DataList que lista as informações de produto e, para cada produto fornece aos usuários a capacidade de editar o nome e o preço e para excluir o produto completamente. Em particular, vamos recuperar os registros a serem exibidos usando um ObjectDataSource, mas executar a atualização e excluir ações de interface diretamente com o BLL. Antes que se preocupar sobre como implementar os recursos de edição e exclusão para o DataList, permitem s primeiro obter a página para exibir os produtos em uma interface somente leitura. Desde ve examinadas estas etapas nos tutoriais anteriores, demonstrarei por eles rapidamente.

Comece abrindo o `Basics.aspx` página o `EditDeleteDataList` pasta e, na exibição de Design, adicione uma DataList para a página. Em seguida, de DataList s marca inteligente, crie um novo ObjectDataSource. Já que estamos trabalhando com dados de produto, configurá-lo para usar o `ProductsBLL` classe. Para recuperar *todos os* produtos, escolha o `GetProducts()` método na guia SELECT.


[![Configurar o ObjectDataSource para usar a classe ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**Figura 4**: configurar o ObjectDataSource para usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))


[![Retornar as informações de produto usando o método GetProducts()](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**Figura 5**: retornar as informações de produto usando o `GetProducts()` método ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))


DataList, como o GridView, não foi projetado para inserir novos dados; Portanto, selecione opção na lista suspensa na guia Inserir (nenhum). Também escolha (nenhum) para as guias UPDATE e DELETE desde as atualizações e exclusões serão executadas programaticamente por meio de BLL.


[![Confirme se a lista suspensa em ObjectDataSource s INSERT, UPDATE e excluir guias estiverem definidos como (nenhum)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**Figura 6**: confirmar que a lista suspensa na ObjectDataSource s INSERT, UPDATE e excluir guias estão definido como (nenhum) ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))


Depois de configurar o ObjectDataSource, clique em Concluir, retornando para o Designer. Como podemos ver visto nos exemplos anteriores, quando concluir a configuração de ObjectDataSource, Visual Studio automaticamente cria um `ItemTemplate` para DropDownList, exibindo cada um dos campos de dados. Substituir `ItemTemplate` por um que exibe apenas o nome do produto s e o preço. Além disso, defina o `RepeatColumns` propriedade para 2.

> [!NOTE]
> Conforme discutido no *visão geral de inserindo, atualizando e excluindo dados* tutorial, ao modificar dados usando o ObjectDataSource nossa arquitetura exige que removemos o `OldValuesParameterFormatString` propriedade de s ObjectDataSource marcação declarativa (ou redefini-lo para seu valor padrão, `{0}`). Neste tutorial, no entanto, estamos usando o ObjectDataSource somente para recuperar dados. Portanto, não sendo necessário modificar o s ObjectDataSource `OldValuesParameterFormatString` valor da propriedade (embora ele t prejudicar a fazer isso).


Depois de substituir o padrão DataList `ItemTemplate` com um personalizado, a marcação declarativa em sua página deve ser semelhante à seguinte:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

Dedicar um tempo para exibir nosso andamento por meio de um navegador. Como mostra a Figura 7, DataList exibe o preço do produto nome e a unidade de cada produto em duas colunas.


[![Os nomes de produtos e os preços são exibidos em duas colunas DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**Figura 7**: os nomes de produtos e os preços são exibidos em DataList duas colunas ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))


> [!NOTE]
> DataList tem um número de propriedades que são necessários para o processo de atualização e exclusão, e esses valores são armazenados no estado de exibição. Portanto, quando criar uma DataList que dá suporte a edição ou exclusão de dados, é essencial que o estado de exibição DataList s ser habilitado.  
>   
>  O leitor um deve se lembrar que foi possível desabilitar o estado de exibição ao criar editável GridViews, DetailsViews e FormViews. Isso ocorre porque os controles da Web ASP.NET 2.0 podem incluir *controlar o estado*, que é o estado persistente em postagens como o estado de exibição, mas considerado essenciais.


Desabilitando a exibição de estado em GridView simplesmente omite informações de estado comum, mas mantém o estado de controle (que inclui o estado necessário para editar e excluir). DataList, ter sido criada no período de tempo de 1. x ASP.NET, não utilize o estado de controle e, portanto, deve ter habilitado o estado de exibição. Consulte [vs de estado do controle. Estado de exibição](https://msdn.microsoft.com/en-us/library/1whwt1k7.aspx) para obter mais informações sobre a finalidade do estado de controle e como ela difere do estado de exibição.

## <a name="step-4-adding-an-editing-user-interface"></a>Etapa 4: Adicionando uma Interface de usuário de edição

O controle GridView é composto de uma coleção de campos (BoundFields, CheckBoxFields, TemplateFields e assim por diante). Esses campos podem ajustar sua marcação renderizada dependendo de seu modo. Por exemplo, no modo somente leitura, um BoundField exibe o valor do campo de dados como texto. Quando estiver no modo de edição, ele processa uma caixa de texto Web controle cuja `Text` propriedade é atribuída o valor do campo de dados.

DataList, por outro lado, processa seus itens usando modelos. Itens de somente leitura são processados usando o `ItemTemplate` enquanto os itens no modo de edição são renderizados por meio de `EditItemTemplate`. Neste ponto, nosso DataList tem apenas um `ItemTemplate`. Para dar suporte à funcionalidade de edição de nível de item que precisamos adicionar um `EditItemTemplate` que contém a marcação a ser exibida para o item editável. Para este tutorial, vamos usar controles da Web de caixa de texto para editar o produto s nome e o preço unitário.

O `EditItemTemplate` podem ser criados declarativamente ou por meio do Designer (selecionando a opção de editar modelos de marca inteligente DataList s). Para usar a opção de editar modelos, primeiro clique no link de editar modelos da marca inteligente e, em seguida, selecione o `EditItemTemplate` item da lista suspensa.


[![Aceitar o trabalho com o DataList s EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**Figura 8**: aceitar o trabalho com DataList s `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))


Em seguida, digite o nome de produto: e preço: e, em seguida, arraste dois controles de caixa de texto da caixa de ferramentas para a `EditItemTemplate` interface no Designer. Defina as caixas de texto `ID` propriedades `ProductName` e `UnitPrice`.


[![Adicionar uma caixa de texto para o s nome do produto e o preço](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**Figura 9**: adicionar uma caixa de texto para o s nome do produto e o preço ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))


É preciso associar os valores de campo de dados produto correspondente para o `Text` propriedades das duas caixas de texto. Das marcas inteligentes de caixas de texto, clique no link Editar DataBindings e, em seguida, associe o campo de dados apropriado com a `Text` propriedade, conforme mostrado na Figura 10.

> [!NOTE]
> Ao associar o `UnitPrice` campo de dados para o preço TextBox s `Text` campo, você pode formatá-los como um valor de moeda (`{0:C}`), um número geral (`{0:N}`), ou deixe o campo formatado.


![Associar os campos de dados de UnitPrice e ProductName para as propriedades de texto das caixas de texto](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**Figura 10**: associar o `ProductName` e `UnitPrice` campos de dados para o `Text` propriedades das caixas de texto


Observe como a caixa de diálogo Editar DataBindings na Figura 10 faz *não* incluem a caixa de seleção de associação de dados bidirecional que está presente quando você editar um TemplateField em GridView ou DetailsView ou um modelo em FormView. O recurso de associação de dados bidirecional permitido o valor inserido no controle de entrada da Web a ser atribuído automaticamente para o s ObjectDataSource correspondente `InsertParameters` ou `UpdateParameters` ao inserir ou atualizar dados. DataList não dá suporte a associação de dados bidirecional conforme veremos mais adiante neste tutorial, após o usuário faz a alterações e está pronto para atualizar os dados, será necessário acessar essas caixas de texto de forma programática `Text` propriedades e passar em seus valores para o apropriado `UpdateProduct` método o `ProductsBLL` classe.

Por fim, precisamos adicionar atualização e Cancelar botões para o `EditItemTemplate`. Conforme vimos no [mestre/detalhes usando uma lista com marcadores de registros de mestre com DataList detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial, quando um botão, LinkButton ou ImageButton cujo `CommandName` está definida é clicado de dentro de um repetidor ou DataList, o S repetidor ou DataList `ItemCommand` é gerado. Para o DataList, se o `CommandName` propriedade é definida para um valor determinado, um evento adicional pode ser gerado também. Especiais `CommandName` valores de propriedade incluem, entre outros:

- Cancelar gera o `CancelCommand` evento
- Editar gera o `EditCommand` evento
- Atualizar gera o `UpdateCommand` evento

Lembre-se que esses eventos são disparados *além* o `ItemCommand` evento.

Adicionar ao `EditItemTemplate` dois controles da Web de botão, uma cujo `CommandName` está definido para atualização e os outros s definido como ' Cancelar '. Depois de adicionar esses dois controles da Web de botão Designer deve ser semelhante ao seguinte:


[![Atualização de adicionar os botões para o EditItemTemplate e Cancelar](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**Figura 11**: Adicionar atualizar e Cancelar botões para o `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))


Com o `EditItemTemplate` completa sua marcação declarativa DataList s deve ser semelhante à seguinte:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Etapa 5: Adicionando os detalhes técnicos para entrar no modo de edição

Neste ponto, nosso DataList tem uma interface de edição definida por meio de seu `EditItemTemplate`; no entanto, há s atualmente nenhuma maneira de um usuário visite nossa página para indicar que deseja editar as informações de produto s. Precisamos adicionar um botão de edição para cada produto que, quando clicado, processa esse DataList item no modo de edição. Comece adicionando um botão de edição para o `ItemTemplate`, por meio do Designer ou declarativamente. Certifique-se de definir o botão Editar s `CommandName` propriedade para edição.

Depois de ter adicionado esse botão de edição, dedique alguns momentos para exibir a página por meio de um navegador. Com essa adição, cada produto na listagem deve incluir um botão de edição.


[![Atualização de adicionar os botões para o EditItemTemplate e Cancelar](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**Figura 12**: Adicionar atualizar e Cancelar botões para o `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))


Clicar no botão causa um postback, mas *não* colocar o produto listando em modo de edição. Para tornar o produto editável, é preciso:

1. Definir o DataList s [ `EditItemIndex` propriedade](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) para o índice do `DataListItem` cujo botão Editar apenas foi clicado.
2. Associar novamente os dados para o DataList. Quando o DataList é processada novamente, o `DataListItem` cujo `ItemIndex` corresponde à DataList s `EditItemIndex` será renderizado usando seu `EditItemTemplate`.

Desde o DataList s `EditCommand` evento é acionado quando o botão de edição é clicado, crie um `EditCommand` manipulador de eventos com o código a seguir:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

O `EditCommand` manipulador de eventos é passado um objeto do tipo `DataListCommandEventArgs` como seu segundo parâmetro de entrada, que inclui uma referência para o `DataListItem` cuja edição botão foi clicado (`e.Item`). O manipulador de eventos primeiro define DataList s `EditItemIndex` para o `ItemIndex` do editável `DataListItem` e, em seguida, reconecta os dados para o DataList chamando DataList s `DataBind()` método.

Depois de adicionar este manipulador de eventos, revise a página em um navegador. Clicar no botão Editar agora faz com que o produto clicado editável (consulte a Figura 13).


[![Clique a botão de edição torna o produto editável](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**Figura 13**: clicar no botão Editar faz com que o produto editável ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Etapa 6: Salvar as alterações de s do usuário

Clicar no produto editado s Update ou Cancelar botões não faz nada neste ponto; Para adicionar essa funcionalidade, precisamos criar manipuladores de eventos para o DataList s `UpdateCommand` e `CancelCommand` eventos. Comece criando a `CancelCommand` manipulador de eventos, que será executado quando o botão de cancelamento de produto editado s é clicado e ele encarregados retornando DataList para seu estado de edição previamente.

Para que o DataList processar todos os seus itens no modo somente leitura, é necessário:

1. Definir o DataList s [ `EditItemIndex` propriedade](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) para o índice de um inexistente `DataListItem` índice. `-1`é uma opção segura, como o `DataListItem` índices começam em `0`.
2. Associar novamente os dados para o DataList. Desde não `DataListItem` `ItemIndex` es correspondem à DataList s `EditItemIndex`, DataList inteiro será renderizado em um modo somente leitura.

Essas etapas podem ser realizadas com o código de manipulador de eventos a seguir:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

Com essa adição, clicando na retornar do botão Cancelar DataList estado previamente edição.

É o último manipulador de eventos é necessário para concluir o `UpdateCommand` manipulador de eventos. Este manipulador de eventos deve:

1. Acessar de forma programática o nome do produto inserido pelo usuário e preços, bem como o produto editado s `ProductID`.
2. Inicie o processo de atualização chamando apropriada `UpdateProduct` de sobrecarga no `ProductsBLL` classe.
3. Definir o DataList s [ `EditItemIndex` propriedade](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) para o índice de um inexistente `DataListItem` índice. `-1`é uma opção segura, como o `DataListItem` índices começam em `0`.
4. Associar novamente os dados para o DataList. Desde não `DataListItem` `ItemIndex` es correspondem à DataList s `EditItemIndex`, DataList inteiro será renderizado em um modo somente leitura.

As etapas 1 e 2 serão responsáveis por salvar o usuário alterações s; as etapas 3 e 4 retornam DataList estado previamente edição depois que as alterações foram salvas e são idênticas às etapas executadas no `CancelCommand` manipulador de eventos.

Para obter o nome do produto atualizado e o preço, é preciso usar o `FindControl` método programaticamente a referência TextBox Web controles dentro de `EditItemTemplate`. Também precisamos obter o produto editado s `ProductID` valor. Quando é inicialmente vinculado ObjectDataSource para DataList, Visual Studio atribuída DataList s `DataKeyField` propriedade para o valor de chave primária da fonte de dados (`ProductID`). Esse valor, em seguida, pode ser recuperado de DataList s `DataKeys` coleção. Reserve um tempo para garantir que o `DataKeyField` propriedade realmente está definida como `ProductID`.

O código a seguir implementa as quatro etapas:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

O manipulador de eventos é iniciado com a leitura do produto editado s `ProductID` do `DataKeys` coleção. Em seguida, as duas caixas de texto no `EditItemTemplate` são referenciados e seus `Text` propriedades armazenadas em variáveis locais, `productNameValue` e `unitPriceValue`. Usamos o `Decimal.Parse()` método ler o valor da `UnitPrice` caixa de texto para que, se o valor inserido tem um símbolo de moeda, ele pode ser convertido ainda corretamente em um `Decimal` valor.

> [!NOTE]
> Os valores da `ProductName` e `UnitPrice` caixas de texto são atribuídas somente para as variáveis productNameValue e unitPriceValue se as propriedades de texto de caixas de texto tem um valor especificado. Caso contrário, um valor de `Nothing` é usado para as variáveis, que tem o efeito de atualizar os dados com um banco de dados `NULL` valor. Ou seja, nosso código trata converte cadeias de caracteres para o banco de dados vazias `NULL` valores, que é o comportamento padrão da interface edição nos controles do GridView, DetailsView e FormView.


Depois de ler os valores de `ProductsBLL` classe s `UpdateProduct` método é chamado, passando o nome do produto s, preço, e `ProductID`. O manipulador de eventos é concluída retornando DataList para seu estado de edição previamente usando a mesma lógica exatamente como no `CancelCommand` manipulador de eventos.

Com o `EditCommand`, `CancelCommand`, e `UpdateCommand` manipuladores de eventos concluído, um visitante pode editar o nome e o preço de um produto. Figuras 14-16 mostram o fluxo de trabalho de edição em ação.


[![Quando o primeiro visitar a página, todos os produtos estão no modo somente leitura](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**Figura 14**: ao primeiro visitar a página, todos os produtos estão no modo somente leitura ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))


[![Para atualizar um s nome ou o preço do produto, clique no botão de edição](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**Figura 15**: para atualizar um produto s nome ou o preço, clique no botão Editar ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png))


[![Depois de alterar o valor, clique em atualizar para retornar para o modo somente leitura](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**Figura 16**: depois de alterar o valor, clique em atualizar para retornar para o modo somente leitura ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Etapa 7: Adicionando recursos de exclusão

As etapas para adicionar recursos de exclusão para uma DataList são semelhantes da adição de recursos de edição. Em resumo, precisamos adicionar um botão de exclusão para o `ItemTemplate` que, quando clicado:

1. Lê o produto correspondente s `ProductID` por meio de `DataKeys` coleção.
2. Executa a exclusão chamando o `ProductsBLL` classe s `DeleteProduct` método.
3. Reconecta os dados para o DataList.

Permitir que o s comece adicionando um botão de exclusão para o `ItemTemplate`.

Quando clicado, um botão cujo `CommandName` é edição, atualização, ou Cancelar gera DataList s `ItemCommand` evento juntamente com um evento adicional (por exemplo, ao usar edição o `EditCommand` é gerado também). Da mesma forma, qualquer botão, LinkButton ou ImageButton em DataList cujo `CommandName` está definida como excluir faz com que o `DeleteCommand` evento seja acionado (juntamente com `ItemCommand`).

Adicionar um botão de exclusão ao lado do botão de edição no `ItemTemplate`, configuração seu `CommandName` propriedade para exclusão. Depois de adicionar esse botão controle DataList s `ItemTemplate` sintaxe declarativa deve se parecer com:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

Em seguida, crie um manipulador de eventos para o DataList s `DeleteCommand` evento, usando o seguinte código:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

Clicando no botão Excluir causa um postback e dispara o DataList s `DeleteCommand` eventos. No manipulador de eventos, o produto clicado s `ProductID` valor é acessado a partir de `DataKeys` coleção. Em seguida, o produto for excluído, chamando o `ProductsBLL` classe s `DeleteProduct` método.

Depois de excluir o produto, ela importante que podemos associar novamente os dados para o DataList (`DataList1.DataBind()`), caso contrário, o DataList continuarão mostrar o produto que acabou de ser excluído.

## <a name="summary"></a>Resumo

Enquanto DataList não tem o ponto e clique em Editar e excluir suporte aproveitado pelo GridView, com um pouco curto de código ele pode ser aprimorado para incluir esses recursos. Neste tutorial, vimos como criar uma lista de duas colunas de produtos que pode ser excluído e cujo nome e o preço podem ser editados. Adição, edição e exclusão de suporte é uma questão de incluindo os controles da Web apropriados a `ItemTemplate` e `EditItemTemplate`, criação de manipuladores de eventos correspondente, lendo os valores de chave primários e inserido pelo usuário e uma interface com o Business Camada de lógica.

Enquanto adicionamos edição básica e excluir recursos de DataList, ele não tem recursos mais avançados. Por exemplo, não há nenhuma validação de campo de entrada - se um usuário inserir um preço de muito dispendiosa, uma exceção será lançada pelo `Decimal.Parse` ao tentar converter muito caro em um `Decimal`. Da mesma forma, se houver um problema ao atualizar os dados em que a lógica de negócios ou camadas de acesso de dados o usuário verá a tela de erro padrão. Sem nenhum tipo de confirmação no botão de exclusão, é muito provavelmente a exclusão acidental de um produto.

No futuro, tutoriais, veremos como melhorar o usuário de edição experiência.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Zack Jones, Ken Pespisa e Randy Schmidt. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Avançar](performing-batch-updates-cs.md)
