---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: Exibindo dados com os controles de Repetidor (c#) e DataList | Microsoft Docs
author: rick-anderson
description: "Nos tutoriais anteriores usamos o controle GridView para exibir dados. A partir deste tutorial, vamos examinar a criação de padrões comuns de relatório com..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f626731e79d83785057498c53cdf49aecb90261
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Exibindo dados com o DataList e controles de Repetidor (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) ou [baixar PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> Nos tutoriais anteriores usamos o controle GridView para exibir dados. A partir deste tutorial, vamos examinar a criação de padrões comuns de relatório com os controles DataList e repetidor, começando com os conceitos básicos de exibição de dados com esses controles.


## <a name="introduction"></a>Introdução

Em todos os exemplos durante os últimos 28 tutoriais, se necessário para exibir vários registros de uma fonte de dados são ativados para o controle GridView. GridView processa uma linha para cada registro da fonte de dados, exibindo os campos de dados de registro s em colunas. Embora GridView facilita a exibição, paginar, classificar, editar e excluir dados, sua aparência é um pouco boxy. Além disso, a marcação responsável para a estrutura GridView é fixo ele inclui um HTML `<table>` com uma linha de tabela (`<tr>`) para cada registro e uma célula de tabela (`<td>`) para cada campo.

Para fornecer um grau maior de personalização na aparência e a marcação renderizada ao exibir vários registros, o ASP.NET 2.0 oferece os controles DataList e repetidor (que também estavam disponíveis na versão do ASP.NET 1. x). O controle DataList e repetidor processar seu conteúdo usando modelos em vez de BoundFields, CheckBoxFields, ButtonFields e assim por diante. Como o GridView, DataList renderiza como um HTML `<table>`, mas permite que dados de vários registros de origem a serem exibidos por linha da tabela. Repetidor, por outro lado, não processa nenhuma marcação adicional que o que você explicitamente especifique e é um candidato ideal quando precisar de mais controle sobre a marcação emitido.

Sobre os lado dezenas de tutoriais, vamos examinar a criação de padrões comuns de relatório com os controles DataList e repetidor, começando com os conceitos básicos de exibição de dados com esses modelos de controles. Veremos como formatar esses controles, como alterar o layout dos registros de dados em DataList, cenários comuns de detalhes/mestre, maneiras para editar e excluir dados, como por meio de registros da página e assim por diante.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Etapa 1: Adicionar DataList e as páginas da Web do Tutorial repetidor

Antes de começar este tutorial, permitem s primeiro dedicar um tempo para adicionar as páginas ASP.NET que vamos precisar para este tutorial e os tutoriais de alguns Avançar lidar com a exibição de dados usando o DataList e Repetidor. Comece criando uma nova pasta no projeto chamado `DataListRepeaterBasics`. Em seguida, adicione as seguintes páginas ASP.NET cinco nesta pasta, ter todos os configurado para usar a página mestra `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Crie uma pasta DataListRepeaterBasics e adicionar as páginas ASP.NET Tutorial](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Figura 1**: criar um `DataListRepeaterBasics` pasta e adicionar as páginas ASP.NET Tutorial


Abra o `Default.aspx` página e arraste o `SectionLevelTutorialListing.ascx` controle de usuário da `UserControls` pasta para a superfície de Design. Este controle de usuário que criamos no [páginas mestras e navegação de Site](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumera o mapa do site e exibe os tutoriais na seção atual em uma lista com marcadores.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Figura 2**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


Para ter a exibição de lista com marcadores os tutoriais de DataList e repetidor que é criado, precisamos adicioná-los ao mapa do site. Abra o `Web.sitemap` de arquivo e adicione a seguinte marcação após a marcação de nó de mapa de site adicionando botões de personalizado:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![Atualizar o mapa de Site para incluir as novas páginas ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Figura 3**: atualizar o mapa de Site para incluir as novas páginas ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Etapa 2: Exibindo informações de produto com o controle DataList

Semelhante ao FormView, o controle DataList s saída renderizada depende de modelos em vez de BoundFields, CheckBoxFields e assim por diante. Ao contrário de FormView, DataList foi projetado para exibir um conjunto de registros em vez de uma única. Permitir que o s começar este tutorial com uma análise de informações de produto de associação para uma DataList. Comece abrindo o `Basics.aspx` página o `DataListRepeaterBasics` pasta. Em seguida, arraste uma DataList da caixa de ferramentas para o Designer. Como ilustra a Figura 4, antes de especificar os modelos de DataList s, o Designer exibe como uma caixa cinza.


[![Arraste o DataList da caixa de ferramentas para o Designer](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Figura 4**: arraste o DataList da caixa de ferramentas para o Designer ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


De DataList s marca inteligente, adicione um novo ObjectDataSource e configurá-lo para usar o `ProductsBLL` classe s `GetProducts` método. Como re criando uma DataList somente leitura neste tutorial, definimos a lista suspensa como (nenhum) em s Assistente inserir, atualizar e excluir guias.


[![Optar por criar um novo ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Figura 5**: optar por criar um novo ObjectDataSource ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![Configurar o ObjectDataSource para usar a classe ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Figura 6**: configurar o ObjectDataSource para usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![Recuperar informações sobre todos os produtos usando o método GetProducts](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Figura 7**: recuperar informações sobre todos os do produtos usando a `GetProducts` método ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


Depois de configurar o ObjectDataSource e associá-lo com DataList por meio de sua marca inteligente, o Visual Studio criará automaticamente um `ItemTemplate` em DataList que exibe o nome e valor de cada campo de dados retornado pela fonte de dados (consulte a marcação abaixo). Esse padrão `ItemTemplate` aparência s é idêntica ao que os modelos criados automaticamente quando uma fonte de dados de associação para o FormView por meio do Designer.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Lembre-se de que quando uma fonte de dados de associação a um controle de FormView através da marca inteligente de s FormView, Visual Studio é criada uma `ItemTemplate`, `InsertItemTemplate`, e `EditItemTemplate`. Com o controle DataList, no entanto, apenas um `ItemTemplate` é criado. Isso ocorre porque DataList não tem o mesmo interno edição e inserção suporte oferecido por FormView. DataList conter eventos relacionados a editar e excluir e editar e excluir suporte podem ser adicionados com um pouco de código, mas não há s sem suporte de caixa simple como FormView. Veremos como incluir edição e exclusão de suporte com DataList em um tutorial futuras.


Permitir que o s dedicar um tempo para melhorar a aparência desse modelo. Em vez de exibir todos os campos de dados, permitem s exibir apenas a s nome do produto, o fornecedor, a categoria, a quantidade por unidade e preço unitário. Além disso, permitem s exibir o nome de um `<h4>` título e formatar os campos restantes usando um `<table>` abaixo do título.

Para fazer essas alterações, que você pode use o modelo de recursos no Designer de DataList s inteligente marca, clique no link Editar modelos ou você pode modificar o modelo manualmente por meio de sintaxe declarativa de s de página de edição. Se você usar a opção de editar modelos no Designer, a marcação resultante pode não coincidir com a seguinte marcação exatamente, mas quando visualizados por meio de um navegador deve ser muito semelhante ao mostrado na Figura 8 de captura de tela.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> O exemplo acima usa controles de rótulo Web cujo `Text` propriedade é atribuída o valor da sintaxe de associação de dados. Como alternativa, podemos poderia omitir os rótulos, digitando a sintaxe de associação de dados. Isto é, em vez de usar `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` pode em vez disso, usamos a sintaxe declarativa `<%# Eval("CategoryName") %>`.


No entanto, deixando os controles da Web de rótulo, oferece duas vantagens. Primeiro, ele fornece uma maneira mais fácil para formatar os dados com base nos dados, como você verá o seguinte tutorial. Em segundo lugar, a opção de editar modelos na sintaxe de associação de dados declarativo do Designer t exibição que aparece fora algum controle da Web. Em vez disso, a interface de editar modelos foi projetada para facilitar o trabalho com marcação estática e Web controla e pressupõe que qualquer associação de dados será feita por meio da caixa de diálogo Editar DataBindings, que é acessível a partir de marcas inteligentes de controles da Web.

Portanto, ao trabalhar com DataList, que fornece a opção de edição de modelos por meio do Designer, prefiro usar controles da Web de rótulo para que o conteúdo está acessível por meio da interface de editar modelos. Como você verá em breve, repetidor requer que o modelo de conteúdo s ser editada da exibição da fonte. Consequentemente, quando criar os modelos de s repetidor irei geralmente omiti Web rótulo controla a menos que eu sei que precisarei formatar a aparência dos dados vinculados texto com base na lógica de programação.


[![Cada produto s saída é renderizado usando s ItemTemplate DataList](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Figura 8**: cada produto saída é renderizado usando DataList s `ItemTemplate` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Etapa 3: Melhorar a aparência do DataList

Como o GridView, DataList oferece uma série de propriedades relacionadas a estilo, como `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`e assim por diante. Ao trabalhar com os controles GridView e DetailsView, criamos os arquivos de capa no `DataWebControls` tema predefinidas a `CssClass` propriedades para esses dois controles e `CssClass` propriedade para vários dos seus subpropriedades (`RowStyle` `HeaderStyle`e assim por diante). Permitir que o s faça o mesmo para DataList.

Como discutido o [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tutorial, um arquivo de capa Especifica as propriedades relacionadas a aparência do padrão para um controle da Web; um tema é uma coleção de arquivos de capa, CSS, imagem e JavaScript definem uma determinada aparência para um site. No *exibindo dados com o ObjectDataSource* tutorial, criamos um `DataWebControls` tema (que é implementado como uma pasta dentro de `App_Themes` pasta) que tem, no momento, dois arquivos de capa - `GridView.skin` e `DetailsView.skin`. Permitir que o s adicione um terceiro arquivo de capa para especificar as definições de estilo predefinido de DataList.

Para adicionar um arquivo de capa, clique duas vezes no `App_Themes/DataWebControls` pasta, escolha Adicionar um novo Item e selecione a opção de arquivo de capa da lista. Dê o nome `DataList.skin` para o arquivo.


[![Criar um novo arquivo de capa denominado DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Figura 9**: criar um arquivo de capa novo chamado `DataList.skin` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


Use a seguinte marcação para o `DataList.skin` arquivo:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Essas configurações atribua as mesmas classes CSS para as propriedades adequadas de DataList que foram usadas com os controles GridView e DetailsView. As classes CSS usadas aqui `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`, e assim por diante são definidos no `Styles.css` de arquivo e foram adicionados em tutoriais anteriores.

Com a adição deste arquivo de capa, a aparência de s DataList é atualizada no Designer (talvez seja necessário atualizar a exibição de Designer para ver os efeitos do novo arquivo de capa; no menu Exibir, escolha Atualizar). Como mostra a Figura 10, cada produto alternado tem uma cor de plano de fundo rosa claro.


[![Criar um novo arquivo de capa denominado DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Figura 10**: criar um arquivo de capa novo chamado `DataList.skin` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Etapa 4: Explorando os modelos DataList s outros

Além de `ItemTemplate`, DataList dá suporte a seis outros modelos opcionais:

- `HeaderTemplate`Se fornecido, adiciona uma linha de cabeçalho para a saída e é usado para processar essa linha
- `AlternatingItemTemplate`usado para processar itens alternados
- `SelectedItemTemplate`usado para renderizar o item selecionado; o item selecionado é o item cujo índice corresponde à DataList s [ `SelectedIndex` propriedade](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate`usado para renderizar o item que está sendo editado
- `SeparatorTemplate`Se fornecido, adiciona um separador entre cada item e é usado para processar esse separador
- `FooterTemplate`-Se fornecido, adiciona uma linha de rodapé para a saída e é usado para processar essa linha

Ao especificar o `HeaderTemplate` ou `FooterTemplate`, DataList adiciona uma linha de cabeçalho ou rodapé de página adicional para a saída renderizada. Como com o GridView s cabeçalho e rodapé linhas, o cabeçalho e rodapé em DataList não estão associados aos dados. Portanto, qualquer sintaxe de associação de dados no `HeaderTemplate` ou `FooterTemplate` que tenta acessar os dados associados retornará uma cadeia de caracteres em branco.

> [!NOTE]
> Como vimos no [exibindo informações de resumo em GridView s rodapé](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) tutorial, enquanto as linhas de cabeçalho e rodapé don sintaxe de associação de dados de suporte de t, informações específicas de dados pode ser inserida diretamente para essas linhas da GridView s `RowDataBound` manipulador de eventos. Essa técnica pode ser usada para ambos calculam totais em execução ou outras informações dos dados associada ao controle, bem como atribuir essas informações para o rodapé. Este mesmo conceito que pode ser aplicado aos controles DataList e repetidor; a única diferença é que, para o controle DataList e repetidor criam um manipulador de eventos para o `ItemDataBound` evento (em vez de para o `RowDataBound` evento).


Para nosso exemplo, permitem s têm o título exibido na parte superior dos resultados DataList s em informações sobre o produto um `<h3>` título. Para fazer isso, adicione um `HeaderTemplate` com a marcação apropriada. No Designer, isso pode ser feito clicando no link Editar modelos da marca inteligente DataList s, escolher o modelo de cabeçalho na lista suspensa e digitando o texto depois de escolher a opção de título 3 do estilo de lista suspensa lista (veja a Figura 11).


[![Adicionar um HeaderTemplate com as informações de produto do texto](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Figura 11**: adicionar um `HeaderTemplate` com as informações de produto de texto ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


Como alternativa, isso pode ser adicionado declarativamente inserindo a seguinte marcação dentro de `<asp:DataList>` marcas:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Para adicionar um pouco de espaço entre cada produto na listagem, deixe s adicionar um `SeparatorTemplate` que inclui uma linha entre cada seção. A marca da régua horizontal (`<hr>`), adiciona uma linha de divisória. Criar o `SeparatorTemplate` para que ele tenha a seguinte marcação:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Como o `HeaderTemplate` e `FooterTemplates`, o `SeparatorTemplate` não está associado a qualquer registro da fonte de dados e, portanto, não é diretamente associados à DataList de registros da fonte de dados de acesso.


Depois de fazer essa adição, ao exibir a página por meio de um navegador deve ser semelhante à Figura 12. Observe a linha de cabeçalho e a linha entre cada produto na listagem.


[![DataList inclui uma linha de cabeçalho e uma régua Horizontal entre cada listagem do produto](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Figura 12**: DataList inclui uma linha de cabeçalho e um Horizontal regra entre cada produto listando ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Etapa 5: Processamento de marcação específica com o controle repetidor

Se você fizer uma fonte de exibição/do seu navegador ao visitar o exemplo de DataList da Figura 12, você verá que o DataList emite um HTML `<table>` que contém uma linha de tabela (`<tr>`) com uma única célula de tabela (`<td>`) para cada item vinculado para o DataList. Essa saída, na verdade, é idêntica ao que poderia ser emitido a partir um GridView com um único TemplateField. Como você verá um tutorial futuras, DataList permitir personalização adicional de saída, que nos permite exibir vários registros de fonte de dados por linha da tabela.

Se você não quiser t para emitir um HTML `<table>`, embora? Para controle total e completa sobre a marcação gerada por um controle de Web de dados, podemos deve usar o controle Repetidor. Como o DataList repetidor é construído com base em modelos. Repetidor, no entanto, apenas oferece os seguintes cinco modelos:

- `HeaderTemplate`Se fornecido, adiciona a marcação especificada antes dos itens
- `ItemTemplate`usado para processar itens
- `AlternatingItemTemplate`Se fornecido, usado para processar itens alternados
- `SeparatorTemplate`Se fornecido, adiciona a marcação especificada entre cada item
- `FooterTemplate`-Se fornecido, adiciona a marcação especificada após os itens

No ASP.NET 1. x, repetidor controle foi comumente usado para exibir uma lista com marcadores cujos dados veio algumas fontes de dados. Nesse caso, o `HeaderTemplate` e `FooterTemplates` conteria a abertura e fechamento `<ul>` marcas, respectivamente, enquanto o `ItemTemplate` conteria `<li>` elementos com a sintaxe de associação de dados. Essa abordagem ainda pode ser usada no ASP.NET 2.0 como vimos nos dois exemplos de [páginas mestras e navegação de Site](../introduction/master-pages-and-site-navigation-cs.md) tutorial:

- No `Site.master` página mestre, um repetidor foi usado para exibir uma lista com marcadores do conteúdo do mapa do site de nível superior (relatórios básicos, filtragem de relatórios, formatação personalizada e assim por diante); outro repetidor aninhada foi usado para exibir as seções de filhos das seções de nível superior
- Em `SectionLevelTutorialListing.ascx`, um repetidor foi usado para exibir uma lista com marcadores das seções filhos da seção de mapa de site atual

> [!NOTE]
> O ASP.NET 2.0 apresenta o novo [controle BulletedList](https://msdn.microsoft.com/en-us/library/ms228101.aspx), que pode ser associada a um controle de fonte de dados para exibir uma lista com marcadores simple. Com o controle de BulletedList não precisamos especificar qualquer HTML relacionada à lista; em vez disso, podemos simplesmente indicar o campo de dados a ser exibida como o texto para cada item da lista.


Repetidor serve como uma captura todos os dados de controle da Web. Se não houver um controle existente que gera a marcação necessária, o controle de Repetidor pode ser usado. Para ilustrar usando repetidor, permitem s tiverem a lista de categorias exibida acima do DataList de informações de produto criado na etapa 2. Em particular, permitem s têm as categorias exibidas em uma única linha HTML `<table>` com cada categoria exibida como uma coluna na tabela.

Para fazer isso, inicie arrastando um controle repetidor da caixa de ferramentas para o Designer, acima do DataList de informações do produto. Assim como acontece com DataList, repetidor exibe inicialmente como uma caixa cinza até que seu modelo foram definido.


[![Adicionar um repetidor para o Designer](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Figura 13**: adicionar um repetidor para o Designer ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


Apenas uma opção de lá s na marca inteligente repetidor s: Escolher fonte de dados. Optar por criar um novo ObjectDataSource e configurá-lo para usar o `CategoriesBLL` classe s `GetCategories` método.


[![Criar um novo ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Figura 14**: criar um novo ObjectDataSource ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![Configurar o ObjectDataSource para usar a classe CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Figura 15**: configurar o ObjectDataSource para usar o `CategoriesBLL` classe ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![Recuperar informações sobre todas as categorias usando o método GetCategories](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Figura 16**: recuperar informações sobre todas as categorias usando o `GetCategories` método ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


Ao contrário do DataList, Visual Studio não cria automaticamente um ItemTemplate para repetidor após a associação a uma fonte de dados. Além disso, os modelos de s repetidor não podem ser configurados por meio do Designer e devem ser especificados declarativamente.

Para exibir as categorias como uma única linha `<table>` com uma coluna para cada categoria, precisamos repetidor emitir marcação semelhante à seguinte:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Desde o `<td>Category X</td>` texto é a parte que se repete, isso será exibido no repetidor s ItemTemplate. A marcação que aparece antes de ele - `<table><tr>` -será colocada no `HeaderTemplate` durante a marcação final - `</tr></table>` -será colocada no `FooterTemplate`. Para inserir essas configurações de modelo, vá para a parte declarativa de página ASP.NET clicando no botão fonte no canto inferior esquerdo e digite a seguinte sintaxe:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Repetidor emite marcação conforme especificado por seus modelos, nada mais, nada menos precisa. Figura 17 mostra a saída de s repetidor quando visualizada através de um navegador.


[![Uma única linha HTML &lt;tabela&gt; lista cada categoria em uma coluna separada](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Figura 17**: uma única linha HTML `<table>` lista cada categoria em uma coluna separada ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Etapa 6: Melhorar a aparência do repetidor

Desde o repetidor emite precisamente a marcação especificada por seus modelos, ele deve ter nenhuma surpresa que não existem propriedades relacionadas a estilo para Repetidor. Para alterar a aparência do conteúdo gerado pelo repetidor, podemos deve adicionar manualmente o conteúdo HTML ou CSS necessário diretamente aos modelos repetidor s.

Para nosso exemplo, permitem s tem as colunas categoria alternativa cores de plano de fundo, como com as linhas alternadas em DataList. Para fazer isso, é preciso atribuir a `RowStyle` classe CSS para cada item repetidor e o `AlternatingRowStyle` classe CSS para cada item de alternância repetidor por meio de `ItemTemplate` e `AlternatingItemTemplate` modelos, da seguinte forma:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Permitir que o s também adicionar uma linha de cabeçalho para a saída com o texto de categorias de produto. Já que estamos não t saber quantas colunas nosso resultante `<table>` será composta, a maneira mais simples para gerar uma linha de cabeçalho que é garantida para abranger todas as colunas é usar *duas* `<table>` s. A primeira `<table>` conterá duas linhas, a linha de cabeçalho e uma linha que contém a segunda, uma linha `<table>` que tem uma coluna para cada categoria no sistema. Ou seja, queremos emita a seguinte marcação:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

O seguinte `HeaderTemplate` e `FooterTemplate` resultar na marcação desejada:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

A Figura 18 mostra repetidor depois que essas alterações foram feitas.


[![As colunas categoria alternam na cor de plano de fundo e inclui uma linha de cabeçalho](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Figura 18**: A categoria colunas alternativo na cor de plano de fundo e inclui uma linha de cabeçalho ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>Resumo

Enquanto o controle GridView facilita a exibir, editar, excluir, classificar e paginar através dos dados, a aparência está muito boxy e grade. Para obter mais controle sobre a aparência, é necessário ativar a controles de DataList ou Repetidor. Esses dois controles exibem um conjunto de registros usando modelos em vez de BoundFields, CheckBoxFields e assim por diante.

DataList renderiza como um HTML `<table>` que, por padrão, exibe cada registro da fonte de dados em uma única linha da tabela, assim como um GridView com um TemplateField único. No entanto, conforme veremos um tutorial futuras, DataList permitir vários registros a serem exibidos por linha da tabela. Repetidor, por outro lado, estritamente emite a marcação especificada em seus modelos; ele não adicione qualquer marcação adicional e, portanto, normalmente é usado para exibir dados em elementos HTML diferente de um `<table>` (como em uma lista com marcadores).

Enquanto o DataList e repetidor oferecem mais flexibilidade em sua saída renderizada, eles não têm muitos dos recursos internos encontrados em GridView. Como examinaremos em tutoriais futuros, alguns desses recursos podem ser conectado novamente sem muito esforço, mas é manter em mente que o uso do DataList ou repetidor em vez de GridView limitar os recursos que você pode usar sem a necessidade de implementar esses recursos por conta própria.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Yaakov Ellis, Liz Shulok, Randy Schmidt e Stacy Park. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Avançar](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
