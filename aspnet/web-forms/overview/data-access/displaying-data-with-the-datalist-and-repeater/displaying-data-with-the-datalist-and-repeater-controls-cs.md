---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: Exibindo dados com os controles DataList e Repeater (c#) | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores usamos o controle GridView para exibir dados. Começando com este tutorial, vamos examinar a criação de padrões comuns de geração de relatórios com...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: a58a9501a546a437b44e078c628d7db010700b5c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825196"
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Exibindo dados com os controles DataList e Repeater (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) ou [baixar PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> Nos tutoriais anteriores usamos o controle GridView para exibir dados. Começando com este tutorial, vamos examinar a criação de padrões comuns de geração de relatórios com os controles DataList e Repeater, começando com os conceitos básicos da exibição de dados com esses controles.


## <a name="introduction"></a>Introdução

Em todos os exemplos durante os últimos 28 tutoriais, se for necessário para exibir vários registros de uma fonte de dados são ativados para o controle GridView. O GridView renderiza uma linha para cada registro na fonte de dados, exibindo os campos de dados de registro s em colunas. Embora GridView facilita a exibição, paginar, classificar, editar e excluir dados, sua aparência é um pouco boxy. Além disso, a marcação responsável para a estrutura de s GridView é fixo e ele inclui um HTML `<table>` com uma linha da tabela (`<tr>`) para cada registro e uma célula de tabela (`<td>`) para cada campo.

Para fornecer um maior grau de personalização na aparência e a marcação renderizada ao exibir vários registros, o ASP.NET 2.0 oferece os controles DataList e Repeater (que também estavam disponíveis na versão do ASP.NET 1. x). Os controles DataList e Repeater renderizam seu conteúdo usando modelos em vez de BoundFields CheckBoxFields, ButtonFields e assim por diante. Como o GridView, DataList é renderizado como uma marca HTML `<table>`, mas permite que dados de vários registros de origem a serem exibidos por linha da tabela. O Repeater, por outro lado, não renderiza nenhuma marcação adicional que o que você explicitamente Especifica e é um candidato ideal quando você precisa de um controle preciso sobre a marcação emitida.

Sobre os lado dezenas de tutoriais, vamos examinar a criação de padrões comuns de geração de relatórios com os controles DataList e Repeater, começando com os conceitos básicos da exibição de dados com esses modelos de controles. Veremos como formatar esses controles, como alterar o layout dos registros de origem de dados no DataList, cenários comuns de detalhes/mestre, maneiras para editar e excluir dados, como página por meio de registros e assim por diante.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Etapa 1: Adicionando o DataList e Repeater Tutorial páginas de Web

Antes de começar este tutorial, deixe s primeiro dedique uns momentos para adicionar as páginas do ASP.NET, precisaremos para este tutorial e os próximo poucos tutoriais que lidar com a exibição de dados usando o DataList e Repeater. Comece criando uma nova pasta no projeto chamado `DataListRepeaterBasics`. Em seguida, adicione as seguintes páginas ASP.NET cinco nesta pasta, manter todos eles configurado para usar a página mestra `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Crie uma pasta DataListRepeaterBasics e adicione as páginas do Tutorial do ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Figura 1**: criar um `DataListRepeaterBasics` pasta e adicione as páginas do Tutorial do ASP.NET


Abra o `Default.aspx` da página e arraste o `SectionLevelTutorialListing.ascx` controle de usuário do `UserControls` pasta para a superfície de Design. Esse controle de usuário que criamos na [páginas mestras e navegação no Site](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumera o mapa do site e exibe os tutoriais da seção atual em uma lista com marcadores.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Figura 2**: Adicione o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


Para ter a exibição de lista com marcadores os tutoriais do DataList e Repeater, criaremos, precisamos adicioná-los ao mapa do site. Abra o `Web.sitemap` arquivo e adicione a marcação a seguir após a marcação de nó de mapa de site adicionando botões de personalizado:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![Atualizar o mapa de Site para incluir as novas páginas do ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Figura 3**: atualizar o mapa de Site para incluir as novas páginas do ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Etapa 2: Exibindo informações de produto com o DataList

Assim como FormView, o controle DataList s saída renderizada varia de acordo com modelos em vez de BoundFields, CheckBoxFields e assim por diante. Ao contrário de FormView, DataList foi projetado para exibir um conjunto de registros em vez de um solitário. Deixe o s começar este tutorial com uma análise de informações de produto de associação para uma DataList. Comece abrindo o `Basics.aspx` página o `DataListRepeaterBasics` pasta. Em seguida, arraste uma DataList da caixa de ferramentas para o Designer. Como ilustra a Figura 4, antes de especificar os modelos de DataList s, o Designer exibe como uma caixa cinza.


[![Arraste DataList da caixa de ferramentas para o Designer](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Figura 4**: arraste o DataList da caixa de ferramentas para o Designer ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


Do DataList s marca inteligente, adicione um novo ObjectDataSource e configurá-lo para usar o `ProductsBLL` classe s `GetProducts` método. Como re criando um DataList de somente leitura neste tutorial, definimos a lista suspensa como (nenhum) em s Assistente inserir, atualizar e excluir guias.


[![Optar por criar um novo ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Figura 5**: otimizado para criar um novo ObjectDataSource ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![Configurar o ObjectDataSource para usar a classe ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Figura 6**: configurar o ObjectDataSource para usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![Recuperar informações sobre todos os produtos usando o método GetProducts](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Figura 7**: recuperar informações sobre todos os de produtos usando o `GetProducts` método ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


Depois de configurar o ObjectDataSource e associá-lo com o DataList por meio de sua marca inteligente, o Visual Studio criará automaticamente um `ItemTemplate` no DataList que exibe o nome e valor de cada campo de dados retornado pela fonte de dados (consulte a marcação abaixo). Esse padrão `ItemTemplate` aparência é idêntica dos modelos criados automaticamente quando uma fonte de dados de associação para FormView por meio do Designer.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Lembre-se de que ao associar a uma fonte de dados a um controle FormView por meio da marca inteligente do FormView s, o Visual Studio criou uma `ItemTemplate`, `InsertItemTemplate`, e `EditItemTemplate`. Com o DataList, no entanto, somente um `ItemTemplate` é criado. Isso ocorre porque o DataList não tem interno mesmo edição e inserção de suporte oferecido pelo FormView. DataList conter eventos relacionados a edição e exclusão e edição e exclusão de suporte não podem ser adicionados com um pouco de código, mas s não existe nenhum suporte de out-of-the-box simples como FormView. Veremos como incluir a edição e exclusão de suporte com o DataList em um tutorial futuro.


Deixe o s dedique uns momentos para melhorar a aparência desse modelo. Em vez de exibir todos os campos de dados, deixe s exibir somente o s nome do produto, o fornecedor, a categoria, a quantidade por unidade e o preço unitário. Além disso, let s exibem o nome em uma `<h4>` título e formatar os campos restantes usando um `<table>` abaixo do título.

Use o recursos no Designer do DataList s smart tag clique no link Editar modelos ou você pode modificar o modelo manualmente por meio da sintaxe declarativa de s de página de edição de modelo para fazer essas alterações, que você pode. Se você usar a opção de editar modelos no Designer, sua marcação resultante pode não corresponder a marcação a seguir exatamente, mas quando visualizado por meio de um navegador deve parecer muito semelhante à mostrada na Figura 8 de captura de tela.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> O exemplo acima usa controles de Web Label cujo `Text` propriedade recebe o valor da sintaxe de associação de dados. Como alternativa, poderia omitimos os rótulos completamente, digitar apenas a sintaxe de associação de dados. Ou seja, em vez de usar `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` podemos poderia ter usado em vez disso, a sintaxe declarativa `<%# Eval("CategoryName") %>`.


No entanto, deixando nos controles da Web de rótulo, oferece duas vantagens. Primeiro, ele fornece um meio mais fácil para formatar os dados com base nos dados, como veremos no próximo tutorial. Em segundo lugar, a opção de editar modelos na sintaxe de vinculação de dados declarativa de Designer ainda não t exibição que aparece fora de algum controle da Web. Em vez disso, a interface de editar modelos foi projetada para facilitar o trabalho com marcação estática e controles de Web e pressupõe que qualquer associação de dados será feita por meio da caixa de diálogo Editar DataBindings, que é acessível a partir de marcas inteligentes de controles da Web.

Portanto, ao trabalhar com o controle DataList que fornece a opção de editar os modelos por meio do Designer, eu prefiro usar controles da Web de rótulo para que o conteúdo é acessível por meio da interface de editar modelos. Como você verá em breve, o Repeater requer que o modelo de conteúdo s ser editado da exibição da fonte. Consequentemente, ao criar os modelos de s Repeater geralmente omitirei Web rótulo controla a menos que eu sei que precisarei formatar a aparência dos dados associados texto com base em lógica programática.


[![Cada produto s saída é renderizado usando DataList s ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Figura 8**: cada produto s saída é renderizado usando s DataList `ItemTemplate` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Etapa 3: Melhorar a aparência do DataList

Como o GridView, DataList oferece uma série de propriedades relacionadas a estilo, como `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`e assim por diante. Ao trabalhar com os controles GridView e DetailsView, criamos os arquivos de capa na `DataWebControls` tema predefinidas a `CssClass` propriedades para esses dois controles e o `CssClass` propriedade para várias de suas subpropriedades (`RowStyle` `HeaderStyle`e assim por diante). Deixe o s faça o mesmo para DataList.

Conforme discutido na [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tutorial, um arquivo de capa Especifica as propriedades relacionadas à aparência padrão para um controle Web; um tema é uma coleção de arquivos de capa, CSS, imagem e JavaScript que definem uma determinada aparência para um site. No *exibindo dados com o ObjectDataSource* tutorial, criamos um `DataWebControls` tema (que é implementado como uma pasta dentro de `App_Themes` pasta) que tem, no momento, dois arquivos de capa - `GridView.skin` e `DetailsView.skin`. Deixe o s adicionar um terceiro arquivo de capa para especificar as configurações de estilo predefinido para DataList.

Para adicionar um arquivo de capa, clique com botão direito no `App_Themes/DataWebControls` pasta, escolha Adicionar um novo Item e selecione a opção de arquivo de capa da lista. Dê o nome `DataList.skin` para o arquivo.


[![Criar um novo arquivo de capa chamado DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Figura 9**: criar um novo nomeado de arquivo de capa `DataList.skin` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


Use a seguinte marcação para o `DataList.skin` arquivo:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Essas configurações atribuir as mesmas classes CSS para as propriedades de DataList adequadas conforme foram usados com os controles GridView e DetailsView. As classes CSS usadas aqui `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`e assim por diante são definidos na `Styles.css` de arquivo e foram adicionados nos tutoriais anteriores.

Com a adição desse arquivo de capa, a aparência de s DataList é atualizada no Designer (talvez seja necessário atualizar a exibição de Designer para ver os efeitos do novo arquivo de capa; a partir do menu Exibir, escolha Atualizar). Como mostra a Figura 10, cada produto alternado tem uma cor de plano de fundo rosa-claro.


[![Criar um novo arquivo de capa chamado DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Figura 10**: criar um novo nomeado de arquivo de capa `DataList.skin` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Etapa 4: Explorando DataList s outros modelos

Além de `ItemTemplate`, DataList dá suporte a seis modelos de opcionais:

- `HeaderTemplate` Se fornecido, adiciona uma linha de cabeçalho na saída e é usado para renderizar essa linha
- `AlternatingItemTemplate` usado para processar itens alternados
- `SelectedItemTemplate` usado para renderizar o item selecionado; o item selecionado é o item cujo índice corresponde ao s DataList [ `SelectedIndex` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` usado para renderizar o item que está sendo editado
- `SeparatorTemplate` Se fornecido, adiciona um separador entre cada item e é usado para renderizar esse separador
- `FooterTemplate` -Se fornecidos, adiciona uma linha de rodapé para a saída e é usado para renderizar essa linha

Ao especificar o `HeaderTemplate` ou `FooterTemplate`, DataList adiciona uma linha de cabeçalho ou rodapé adicional para a saída renderizada. Como com o GridView s cabeçalho e rodapé linhas, o cabeçalho e rodapé em um DataList não estão associados aos dados. Portanto, qualquer sintaxe de associação de dados na `HeaderTemplate` ou `FooterTemplate` que tentam acessar os dados associados retornará uma cadeia de caracteres em branco.

> [!NOTE]
> Como vimos na [exibindo informações de resumo em s GridView rodapé](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) tutorial, enquanto as linhas de cabeçalho e rodapé don sintaxe de associação de dados de suporte de t, informações específicas de dados pode ser injetado diretamente nessas linhas das S GridView `RowDataBound` manipulador de eventos. Essa técnica pode ser usada para os dois calculam os totais em execução ou outras informações dos dados associado ao controle, bem como atribuir essas informações para o rodapé. Este mesmo conceito pode ser aplicado aos controles DataList e Repeater; a única diferença é que, para o DataList e Repeater, crie um manipulador de eventos para o `ItemDataBound` evento (em vez de para o `RowDataBound` evento).


Para nosso exemplo, let s têm o título de informações do produto exibida na parte superior dos resultados DataList s em um `<h3>` título. Para fazer isso, adicione um `HeaderTemplate` com a marcação apropriada. No Designer, isso pode ser feito clicando no link Editar modelos na marca inteligente DataList s, escolhendo o modelo de cabeçalho da lista suspensa e digitar o texto depois de escolher a opção de título 3 do estilo de lista suspensa lista (veja a Figura 11).


[![Adicionar um HeaderTemplate com as informações de produto do texto](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Figura 11**: adicionar um `HeaderTemplate` com as informações de produto do texto ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


Como alternativa, isso pode ser adicionado declarativamente, inserindo a seguinte marcação dentro do `<asp:DataList>` marcas:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Para adicionar um pouco de espaço entre cada produto na listagem, permitem que s adicione um `SeparatorTemplate` que inclui uma linha entre cada seção. A marca da régua horizontal (`<hr>`), adiciona tal um divisor. Criar o `SeparatorTemplate` para que ele tenha a seguinte marcação:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Como o `HeaderTemplate` e `FooterTemplates`, o `SeparatorTemplate` não está associado a qualquer registro da fonte de dados e, portanto, não é diretamente associados à DataList de registros da fonte de dados do access.


Depois de fazer essa adição ao exibir a página por meio de um navegador deve ser semelhante à Figura 12. Observe a linha de cabeçalho e a linha entre cada produto na listagem.


[![DataList inclui uma linha de cabeçalho e uma régua Horizontal entre cada listagem de produto](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Figura 12**: DataList inclui uma linha de cabeçalho e um Horizontal regra entre cada listagem de produtos ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Etapa 5: Processamento de marcação específica com o controle Repeater

Se você fizer uma fonte de exibição/do seu navegador ao visitar o exemplo de DataList da Figura 12, você verá que DataList emite um HTML `<table>` que contém uma linha da tabela (`<tr>`) com uma única célula de tabela (`<td>`) para cada item associado para o DataList. Na verdade, essa saída é idêntica ao que seria ser emitido a partir de um GridView com um único TemplateField. Como veremos em um tutorial futuro, DataList permitir personalização adicional da saída, possibilitando a exibir vários registros da fonte de dados por linha da tabela.

E se você não deseja t para emitir uma marca HTML `<table>`, embora? Para controle total e completo sobre a marcação gerada por um controle de Web de dados, devemos usar o controle Repeater. Como DataList e Repeater é construído com base em modelos. O Repeater, no entanto, apenas oferece os seguintes cinco modelos:

- `HeaderTemplate` Se fornecido, adiciona a marcação especificada antes dos itens
- `ItemTemplate` usado para processar itens
- `AlternatingItemTemplate` Se fornecido, usado para processar itens alternados
- `SeparatorTemplate` Se fornecido, adiciona a marcação especificada entre cada item
- `FooterTemplate` -Se fornecido, adiciona a marcação especificada após os itens

No ASP.NET 1. x, o Repeater controle era comumente usado para exibir uma lista com marcadores cujos dados vem de alguma fonte de dados. Nesse caso, o `HeaderTemplate` e `FooterTemplates` conteria a abertura e fechamento `<ul>` marcas, respectivamente, enquanto o `ItemTemplate` conteria `<li>` elementos com a sintaxe de associação de dados. Essa abordagem ainda pode ser usada no ASP.NET 2.0 como vimos nos dois exemplos de [páginas mestras e navegação no Site](../introduction/master-pages-and-site-navigation-cs.md) tutorial:

- No `Site.master` página mestra, um repetidor foi usado para exibir uma lista com marcadores do conteúdo do mapa do site de nível superior (relatórios básicos, filtragem de relatórios, formatação personalizada e assim por diante); repetidor aninhado e outro foi usada para exibir as seções de filhos das seções de nível superior
- No `SectionLevelTutorialListing.ascx`, um repetidor foi usado para exibir uma lista com marcadores das seções filhos da seção de mapa de site atual

> [!NOTE]
> O ASP.NET 2.0 apresenta o novo [controle BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), que pode ser associado a um controle de fonte de dados para exibir uma lista com marcadores simple. Com o controle BulletedList não precisamos especificar qualquer um dos HTML relacionada à lista; em vez disso, podemos simplesmente indicar o campo de dados a ser exibido como o texto para cada item de lista.


O Repeater serve como uma captura todos os dados de controle da Web. Se não for um controle existente que gera a marcação necessária, o controle Repeater pode ser usado. Para ilustrar como usar o Repeater, deixe s a lista de categorias exibido acima do DataList de informações do produto criado na etapa 2. Em particular, let s têm as categorias exibidas em uma única linha HTML `<table>` com cada categoria exibida como uma coluna na tabela.

Para fazer isso, comece a arrastar um controle Repeater da caixa de ferramentas para o Designer, acima do DataList de informações do produto. Assim como acontece com o DataList e Repeater exibe inicialmente como uma caixa cinza até que seus modelos foram definidos.


[![Adicionar um repetidor para o Designer](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Figura 13**: adicionar um repetidor para o Designer ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


Somente uma opção ali s na marca inteligente Repeater s: Escolher fonte de dados. Optar por criar um novo ObjectDataSource e configurá-lo para usar o `CategoriesBLL` classe s `GetCategories` método.


[![Criar um novo ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Figura 14**: criar um novo ObjectDataSource ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![Configurar o ObjectDataSource para usar a classe CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Figura 15**: configurar o ObjectDataSource para usar o `CategoriesBLL` classe ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![Recuperar informações sobre todas as categorias usando o método GetCategories](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Figura 16**: recuperar informações sobre todas as categorias usando o `GetCategories` método ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


Ao contrário do DataList, o Visual Studio não cria automaticamente um ItemTemplate para repetidor depois de associá-lo a uma fonte de dados. Além disso, os modelos de s Repeater não podem ser configurados por meio do Designer e devem ser especificados de forma declarativa.

Para exibir as categorias como uma única linha `<table>` com uma coluna para cada categoria, precisamos que o Repeater para emitir marcação semelhante ao seguinte:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Uma vez que o `<td>Category X</td>` texto é a parte que se repete, isso será exibido no repetidor s ItemTemplate. A marcação que aparece antes de ele - `<table><tr>` -será colocada em de `HeaderTemplate` durante a marcação final - `</tr></table>` -será colocada no `FooterTemplate`. Para inserir essas configurações de modelo, vá para a parte declarativa da página ASP.NET, clicando no botão fonte no canto inferior esquerdo e digite a sintaxe a seguir:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

O Repeater emite marcação conforme especificado por seus modelos, nada mais, nada menos precisa. Figura 17 mostra a saída de s Repeater quando visualizado por meio de um navegador.


[![Uma única linha de HTML &lt;tabela&gt; lista cada categoria em uma coluna separada](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Figura 17**: uma única linha HTML `<table>` lista cada categoria em uma coluna separada ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Etapa 6: Melhorar a aparência do repetidor

Uma vez que o Repeater emite precisamente a marcação especificada por seus modelos, ele deve ser nenhuma surpresa que não existem propriedades relacionadas ao estilo para o Repeater. Para alterar a aparência do conteúdo gerado pelo Repeater, podemos deve adicionar manualmente o conteúdo HTML ou CSS necessário diretamente aos modelos Repeater s.

Para nosso exemplo, deixe s tem as colunas categoria alternativo cores de plano de fundo, como com as linhas alternadas no DataList. Para fazer isso, precisamos atribuir a `RowStyle` a classe CSS para cada item Repeater e o `AlternatingRowStyle` classe CSS para cada item Repeater alternada por meio do `ItemTemplate` e `AlternatingItemTemplate` modelos, da seguinte forma:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Deixe o s também adicionar uma linha de cabeçalho para a saída com o texto de categorias de produto. Já que estamos não t saber quantas colunas nosso resultando `<table>` será composta, a maneira mais simples para gerar uma linha de cabeçalho que é garantida para abranger todas as colunas é usar *dois* `<table>` s. A primeira `<table>` conterá duas linhas, a linha de cabeçalho e uma linha que contém a segunda, uma linha `<table>` que tem uma coluna para cada categoria no sistema. Ou seja, queremos emita a seguinte marcação:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

O seguinte `HeaderTemplate` e `FooterTemplate` resultar na marcação desejada:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

Figura 18 mostra o Repeater depois que essas alterações foram feitas.


[![As colunas categoria alternativo na cor do plano de fundo e inclui uma linha de cabeçalho](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Figura 18**: A categoria colunas alternativo na cor de plano de fundo e inclui uma linha de cabeçalho ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>Resumo

Enquanto o controle GridView torna fácil exibir, editar, excluir, classificar e paginar dados, a aparência é boxy e como grade. Para obter mais controle sobre a aparência, precisamos recorrer aos controles DataList ou Repeater. Ambos esses controles exibem um conjunto de registros usando modelos, em vez de BoundFields, CheckBoxFields e assim por diante.

DataList é renderizado como uma marca HTML `<table>` que, por padrão, exibe cada registro da fonte de dados em uma linha da tabela, assim como um GridView com um TemplateField único. No entanto, como veremos em um tutorial futuro, DataList permitem vários registros a serem exibidos por linha da tabela. O Repeater, por outro lado, estritamente emite a marcação especificada nos seus modelos; ele não adiciona qualquer marcação adicional e, portanto, normalmente é usado para exibir dados em elementos HTML diferente de um `<table>` (como em uma lista com marcadores).

Enquanto o DataList e Repeater oferecem mais flexibilidade em sua saída renderizada, eles não têm muitos dos recursos internos encontrados no GridView. Como nós examinaremos nos próximos tutoriais, alguns desses recursos podem ser conectada novamente sem muito esforço, mas fazer mantenha em mente que usar DataList ou Repeater no lugar de GridView limitar os recursos que você pode usar sem necessidade de implementar esses recursos por conta própria.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisores líder para este tutorial foram Yaakov Ellis, Liz Shulok, Randy Schmidt e Stacy Park. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
