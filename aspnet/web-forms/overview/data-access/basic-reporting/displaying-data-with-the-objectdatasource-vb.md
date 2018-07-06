---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: Exibindo dados com o ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial examina o controle ObjectDataSource usando esse controle, que você pode associar dados recuperados da BLL criada no tutorial anterior sem havi...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 759403bf175cfb2d44cc53ab497c55c354733f15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832718"
---
<a name="displaying-data-with-the-objectdatasource-vb"></a>Exibindo dados com o ObjectDataSource (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) ou [baixar PDF](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> Este tutorial examina o controle ObjectDataSource usando esse controle, que você pode associar dados recuperados da BLL criada no tutorial anterior sem ter que escrever uma linha de código!


## <a name="introduction"></a>Introdução

Com nosso aplicativo site da Web e arquitetura de layout de página completo, estamos prontos para começar a explorar como realizar uma variedade de tarefas comuns de dados – e relacionadas a relatórios. Nos tutoriais anteriores, vimos como associar programaticamente os dados da BLL e DAL em um controle de Web em uma página ASP.NET de dados. Essa sintaxe de atribuição dos dados controle da Web `DataSource` propriedade aos dados para exibição e, em seguida, chamar o controle `DataBind()` método era o padrão usado em aplicativos ASP.NET 1.x e pode continuar a ser usado em seus 2.0 aplicativos. No entanto, novos controles de fonte de dados do ASP.NET 2.0 oferecem uma maneira declarativa para trabalhar com dados. Usando esses controles, você pode associar dados recuperados da BLL criada no tutorial anterior sem ter que escrever uma linha de código!

O ASP.NET 2.0 é fornecido com cinco controles de fonte de dados internos [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), e [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx) Embora você possa construir seu próprio [controles de fonte de dados personalizados](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), se necessário. Uma vez que desenvolvemos uma arquitetura para nosso aplicativo do tutorial, usaremos o ObjectDataSource em relação a nossas classes BLL.


![O ASP.NET 2.0 inclui cinco controles de fonte de dados internos](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Figura 1**: ASP.NET 2.0 inclui cinco controles de fonte de dados internos


O ObjectDataSource serve como um proxy para trabalhar com algum outro objeto. Para configurar o ObjectDataSource especificamos isso base do objeto e como seus métodos são mapeados para o ObjectDataSource `Select`, `Insert`, `Update`, e `Delete` métodos. Depois que esse objeto subjacente foi especificado e seus métodos mapeados para o ObjectDataSource, em seguida, podemos ligar o ObjectDataSource em um controle da Web de dados. ASP.NET é fornecido com muitos dados a controles da Web, incluindo o GridView, DetailsView, RadioButtonList e DropDownList, entre outros. Durante o ciclo de vida da página, os dados de controle de Web talvez precise acessar os dados que ele está vinculado, que ele realizará invocando seu ObjectDataSource `Select` método; se os dados de controle da Web dá suporte à inserção, atualização, ou excluir, chamadas podem ser feitas para seu ObjectDataSource `Insert`, `Update`, ou `Delete` métodos. Essas chamadas, em seguida, são roteadas por ObjectDataSource para métodos do objeto subjacente apropriado conforme ilustra o diagrama a seguir.


[![O ObjectDataSource serve como um Proxy](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**Figura 2**: O ObjectDataSource serve como um Proxy ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image4.png))


Enquanto o ObjectDataSource pode ser usado para invocar métodos de inserção, atualização ou exclusão de dados, vamos apenas nos concentrar no retorno de dados; exploram tutoriais futuras usando o ObjectDataSource e dados de controles da Web que modificam dados.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Etapa 1: Adicionando e configurando o controle ObjectDataSource

Comece abrindo o `SimpleDisplay.aspx` página o `BasicReporting` pasta, alterne para modo Design e, em seguida, arraste um controle ObjectDataSource da caixa de ferramentas na superfície de design da página. O ObjectDataSource aparece como uma caixa cinza na superfície de design porque não produz nenhuma marcação; ele simplesmente acessa dados invocando um método de um objeto especificado. Os dados retornados por um ObjectDataSource podem ser exibidos pelos dados de um controle de Web, como o GridView, DetailsView, FormView e assim por diante.

> [!NOTE]
> Como alternativa, você primeiro de deve adicionar os dados de controle da Web para a página e, na sua marca inteligente, escolha o &lt;nova fonte de dados&gt; opção na lista suspensa.


Para especificar o ObjectDataSource objeto subjacente e como os métodos desse objeto são mapeados para o ObjectDataSource, clique no link configurar fonte de dados na marca inteligente do ObjectDataSource.


[![Clique a configurar o vínculo da fonte de dados da marca inteligente](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Figura 3**: clique no Link da fonte de dados configurar da marca inteligente ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image7.png))


Isso abre o Assistente Configurar fonte de dados. Primeiro, vamos deve especificar o objeto que é trabalhar com o ObjectDataSource. Se a caixa de seleção "Mostrar apenas os componentes de dados" estiver marcada, a lista suspensa essa tela contém apenas os objetos que foram decorados com o `DataObject` atributo. Atualmente, nossa lista inclui os TableAdapters no conjunto de dados tipados e as classes BLL que criamos no tutorial anterior. Se você se esqueceu de adicionar o `DataObject` atributo às classes de camada de lógica comercial, você não vê-los nessa lista. Nesse caso, desmarque a caixa de seleção "Mostrar apenas os componentes de dados" para exibir todos os objetos, que devem incluir as classes BLL (juntamente com as outras classes no conjunto de dados tipado o DataTables, DataRows e assim por diante).

Nesta primeira tela, escolha o `ProductsBLL` classe na lista suspensa e clique em Avançar.


[![Especifique o objeto para uso com o controle ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Figura 4**: especifique o objeto para uso com o controle ObjectDataSource ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image10.png))


A próxima tela do assistente solicita que você selecione qual método o ObjectDataSource deve invocar. Na lista suspensa lista esses métodos que retornam dados no objeto selecionado na tela anterior. Aqui, vemos `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, e `GetProductsBySupplierID`. Selecione o `GetProducts` método a partir de lista suspensa e clique em Concluir (se você tiver adicionado a `DataObjectMethodAttribute` para o `ProductBLL`do métodos conforme mostrado no tutorial anterior, essa opção serão selecionados por padrão).


[![Escolha o método para retornar dados a partir da guia SELECT](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Figura 5**: escolha o método para retornar os dados na guia Selecionar ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Configurar o ObjectDataSource manualmente

Assistente de configurar fonte de dados do ObjectDataSource oferece uma maneira rápida para especificar o objeto que ele usa e para associar a quais métodos do objeto são invocados. No entanto, você pode configurar o ObjectDataSource por meio de suas propriedades na janela Propriedades ou diretamente na marcação declarativa. Basta definir a `TypeName` propriedade para o tipo do objeto base a ser usada e o `SelectMethod` para o método a ser invocado quando a recuperação de dados.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Mesmo se você preferir que o Assistente Configurar fonte de dados que pode haver ocasiões em que você precisa configurar manualmente o ObjectDataSource, como o assistente lista apenas as classes criadas pelo desenvolvedor. Se você quiser associar o ObjectDataSource para uma classe no .NET Framework, como o [classe Membership](https://msdn.microsoft.com/library/system.web.security.membership.aspx), para acessar informações de conta de usuário, ou o [classe Active](https://msdn.microsoft.com/library/system.io.directory.aspx) para trabalhar com informações do sistema de arquivos Você precisará definir manualmente as propriedades do ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Etapa 2: Adicionando um controle de Web de dados e associá-lo a ObjectDataSource

Depois que o ObjectDataSource foi adicionado à página e configurado, estamos prontos para adicionar dados de controles da Web para a página para exibir os dados retornados pelo ObjectDataSource `Select` método. Nenhum dado de controle da Web pode ser associado a um ObjectDataSource; Vamos dar uma olhada em como exibir dados do ObjectDataSource em um GridView, DetailsView e FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Associando um GridView a ObjectDataSource

Adicionar um controle GridView na caixa de ferramentas para `SimpleDisplay.aspx`da superfície de design. Na marca inteligente do GridView, escolha o controle ObjectDataSource, adicionamos na etapa 1. Isso criará automaticamente um BoundField no GridView para cada propriedade retornada pelos dados do ObjectDataSource `Select` método (ou seja, as propriedades definidas por produtos DataTable).


[![Foi adicionado um GridView à página e associado a ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Figura 6**: um GridView foi adicionado à página e associação a ObjectDataSource ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image16.png))


Em seguida, você pode personalizar, reorganizar ou remover BoundFields do GridView, clicando na opção Editar colunas da marca inteligente.


[![Gerenciar BoundFields do GridView por meio da caixa de diálogo Editar colunas](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Figura 7**: gerenciar BoundFields por meio das colunas de caixa do GridView de diálogo Editar ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image19.png))


Dedique uns momentos para modificar BoundFields do GridView, removendo o `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` BoundFields. Basta selecionar o BoundField da lista na parte inferior esquerda e clique no botão Excluir (o X vermelho) para removê-los. Em seguida, reorganizar as BoundFields, de modo que o `CategoryName` e `SupplierName` BoundFields preceder o `UnitPrice` BoundField selecionando esses BoundFields e clicando na seta para cima. Defina as `HeaderText` propriedades do BoundFields restantes para `Products`, `Category`, `Supplier`, e `Price`, respectivamente. Em seguida, o `Price` BoundField formatado como uma moeda definindo o BoundField `HtmlEncode` propriedade como False e seu `DataFormatString` propriedade `{0:c}`. Por fim, alinhar horizontalmente o `Price` à direita e o `Discontinued` caixa de seleção no Centro por meio de `ItemStyle` / `HorizontalAlign` propriedade.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]


[![BoundFields do GridView foram personalizados](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Figura 8**: BoundFields foram personalizados a GridView ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Usando temas para uma aparência consistente

Esses tutoriais se esforçam para remover as configurações de estilo de nível de controle, em vez de usar folhas de estilo em cascata definidas em um arquivo externo sempre que possível. O `Styles.css` arquivo contém `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, e `AlternatingRowStyle` usados nestes tutoriais de controles de Web de classes CSS que devem ser usadas para definir a aparência dos dados. Para fazer isso, poderíamos definido o GridView `CssClass` propriedade para `DataWebControlStyle`e seu `HeaderStyle`, `RowStyle`, e `AlternatingRowStyle` propriedades `CssClass` propriedades adequadamente.

Se definirmos esses `CssClass` adicionado para nossos tutoriais de controle de Web de propriedades seria preciso Lembre-se de definir explicitamente esses valores de propriedade para todos os dados para o controle da Web. Uma abordagem mais gerenciável é definir as propriedades relacionadas a CSS do padrão de GridView, DetailsView e FormView controla usando um tema. Um tema é uma coleção de configurações de propriedade de nível de controle, imagens e classes CSS que podem ser aplicadas às páginas em um site para impor uma aparência comum.

Nosso tema não incluirá todas as imagens ou arquivos CSS (vamos deixar a folha de estilos `Styles.css` como-está, definido na pasta raiz do aplicativo web), mas incluirá dois capas. Uma capa é um arquivo que define as propriedades padrão para um controle da Web. Especificamente, teremos um arquivo de capa para controles GridView e DetailsView, que indica o padrão `CssClass`-propriedades relacionadas.

Comece adicionando um novo arquivo de capa ao seu projeto chamado `GridView.skin` clicando duas vezes no nome do projeto no Gerenciador de soluções e escolhendo Adicionar Novo Item.


[![Adicione um arquivo de capa chamado GridView.skin](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Figura 9**: adicionar um arquivo de capa nomeado `GridView.skin` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image25.png))


Arquivos de capa precisam ser colocados em um tema, que estão localizado no `App_Themes` pasta. Como ainda não temos uma pasta, o Visual Studio oferecerá gentilmente para criá-lo para nós quando adicionar nossa capa primeiro. Clique em Sim para criar o `App_Theme` pasta e coloque o novo `GridView.skin` arquivo existe.


[![Deixe o Visual Studio crie a pasta App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Figura 10**: permitir que o Visual Studio criar o `App_Theme` pasta ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image28.png))


Isso criará um novo tema na `App_Themes` pasta chamada GridView com o arquivo de capa `GridView.skin`.


![O tema de GridView tem foi adicionado à pasta App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Figura 11**: O tema de GridView tem foi adicionado para o `App_Theme` pasta


Renomeie o tema de GridView para DataWebControls (com o botão direito na pasta do GridView do `App_Theme` pasta e escolha Renomear). Em seguida, insira a seguinte marcação para o `GridView.skin` arquivo:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Isso define as propriedades padrão para o `CssClass`-propriedades relacionadas para qualquer GridView em qualquer página que usa o tema DataWebControls. Vamos adicionar outro capa para DetailsView, dados de um controle de Web que usaremos em breve. Adicionar uma nova capa para o tema DataWebControls chamado `DetailsView.skin` e adicione a seguinte marcação:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

Com nosso tema definido, a última etapa é aplicar o tema à nossa página do ASP.NET. Um tema pode ser aplicado em uma base de página por página ou para todas as páginas em um site. Vamos usar este tema para todas as páginas no site. Para fazer isso, adicione a seguinte marcação ao `Web.config`do `<system.web>` seção:


[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

Isso é tudo para ele! O `styleSheetTheme` configuração indica que as propriedades especificadas no tema devem *não* substituir as propriedades especificadas no nível de controle. Para especificar que as configurações de tema devem prevalecem sobre as configurações de controle, use o `theme` de atributo no lugar de `styleSheetTheme`; Infelizmente, as configurações de tema não aparecem na exibição de Design do Visual Studio. Consulte a [visão geral de capas e temas do ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx) e [usando temas do lado do servidor estilos](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) para obter mais informações sobre temas e capas; consulte [How To: aplicar temas do ASP.NET](https://msdn.microsoft.com/library/0yy5hxdk%28VS.80%29.aspx) para obter mais informações sobre Configurar uma página para usar um tema.


[![O GridView exibe o nome do produto, categoria, fornecedor, preço e informações descontinuadas](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Figura 12**: O GridView exibe o nome do produto, categoria, fornecedor, preço e informações descontinuados ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Exibindo um registro por vez em DetailsView

O GridView exibe uma linha para cada registro retornado pelo controle de fonte de dados ao qual ele está associado. Há vezes, no entanto, quando podemos querer exibir um único registro ou apenas um registro por vez. O [controle DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) oferece essa funcionalidade, como uma marca HTML de renderização `<table>` com duas colunas e uma linha para cada coluna ou propriedade associada ao controle. Você pode pensar DetailsView como um GridView com um único registro girada em 90 graus.

Comece adicionando um controle DetailsView *acima* GridView no `SimpleDisplay.aspx`. Em seguida, associá-lo ao mesmo controle ObjectDataSource como GridView. Como com o controle GridView, um BoundField será adicionado ao DetailsView para cada propriedade no objeto retornado pelo ObjectDataSource `Select` método. A única diferença é BoundFields de DetailsView são dispostos horizontalmente em vez de verticalmente.


[![Adicione um DetailsView à página e associá-lo a ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Figura 13**: Adicione um DetailsView à página e associá-lo para o ObjectDataSource ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image35.png))


Como o GridView, BoundFields de DetailsView pode ser alterado para fornecer uma exibição mais personalizada dos dados retornados pelo ObjectDataSource. Figura 14 mostra DetailsView após sua BoundFields e `CssClass` propriedades tiverem sido configuradas para tornar sua aparência semelhante ao exemplo GridView.


[![DetailsView mostra um único registro](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Figura 14**: DetailsView mostra um único registro ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image38.png))


Observe que o DetailsView exibe apenas o primeiro registro retornado pela fonte de dados. Para permitir que o usuário percorrer todos os registros, um de cada vez, podemos deve habilitar a paginação para DetailsView. Para fazer isso, retorne ao Visual Studio e marque a caixa de seleção Habilitar paginação na marca inteligente do ovládacího prvku DetailsView.


[![Habilitar a paginação no controle DetailsView](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Figura 15**: habilitar a paginação no controle DetailsView ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image41.png))


[![Com paginação habilitada, DetailsView permite que o usuário exiba qualquer um dos produtos](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**Figura 16**: com paginação Enabled, DetailsView permite ao usuário exibir qualquer um dos produtos ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image44.png))


Falaremos mais sobre tutoriais de paginação no futuro.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Um Layout mais flexível para mostrar um registro de cada vez

DetailsView é bastante rígida em como ele exibe cada registro retornado do ObjectDataSource. Podemos uma exibição mais flexível dos dados. Por exemplo, em vez de mostrar o nome do produto, categoria, fornecedor, preço e descontinuadas informações em uma linha separada, podemos querer mostrar o nome do produto e o preço em um `<h4>` título, com as informações de categoria e fornecedor que aparecem abaixo do nome e o preço em uma fonte menor. E talvez não nos importamos mostrar os nomes de propriedade (produto, categoria e assim por diante) ao lado de valores.

O [controle FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) fornece esse nível de personalização. Em vez de usar campos (como GridView e DetailsView fazem), FormView usa modelos, que permitem uma mistura de controles da Web, o HTML estático, e [sintaxe de associação de dados](http://www.15seconds.com/issue/040630.htm). Se você estiver familiarizado com o controle Repeater do ASP.NET 1.x, você pode pensar FormView como o Repeater para mostrar um único registro.

Adicionar um controle FormView para o `SimpleDisplay.aspx` superfície de design da página. FormView exibe inicialmente como um bloco cinza, informando-no que precisamos fornecer, no mínimo, o controle `ItemTemplate`.


[![FormView devem incluir um ItemTemplate](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Figura 17**: O FormView deve incluir uma `ItemTemplate` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image47.png))


Você pode vincular FormView diretamente a um controle de fonte de dados por meio de marca inteligente de FormView, que criará um padrão `ItemTemplate` automaticamente (junto com um `EditItemTemplate` e `InsertItemTemplate`, se o controle de ObjectDatatSource `InsertMethod` e `UpdateMethod` são definidas). No entanto, para este exemplo vamos associar os dados ao FormView e especifique seu `ItemTemplate` manualmente. Comece definindo o FormView `DataSourceID` propriedade para o `ID` do controle ObjectDataSource, `ObjectDataSource1`. Em seguida, crie o `ItemTemplate` para que ele exibe o nome e o preço do produto um `<h4>` elemento e os nomes de categoria e transportadora abaixo do que em uma fonte menor.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]


[![O primeiro produto (Chai) é exibido em um formato personalizado](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**Figura 18**: O primeiro produto (Chai) é exibido em um formato personalizado ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image50.png))


O `<%# Eval(propertyName) %>` é a sintaxe de associação de dados. O `Eval` método retorna o valor da propriedade especificada para o objeto atual que está sendo associado ao controle FormView. Confira o artigo de Alex Homer [simplificado e estendidos sintaxe de associação de dados no ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) para obter mais informações sobre as vantagens e desvantagens da associação de dados.

Como o DetailsView FormView mostra apenas o primeiro registro retornado do ObjectDataSource. Você pode habilitar a paginação FormView para permitir que os visitantes percorrer os produtos um por vez.

## <a name="summary"></a>Resumo

Acessar e exibir dados de uma camada de lógica de negócios podem ser feitos sem escrever uma linha de código, graças ao controle do ObjectDataSource do ASP.NET 2.0. O ObjectDataSource invoca um método especificado de uma classe e retorna os resultados. Esses resultados podem ser exibidos em um controle de Web que está associado a ObjectDataSource de dados. Neste tutorial, examinamos os controles GridView, DetailsView e FormView de associação para o ObjectDataSource.

Até agora só vimos como usar o ObjectDataSource para invocar um método sem parâmetros, mas e se quisermos invocar um método que espera parâmetros de entrada, como o `ProductBLL` da classe `GetProductsByCategoryID(categoryID)`? Para chamar um método que espera um ou mais parâmetros, deve configurar o ObjectDataSource para especificar os valores para esses parâmetros. Veremos como fazer isso no nosso [próximo tutorial](declarative-parameters-vb.md).

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Criar seus próprios controles de fonte de dados](https://msdn.microsoft.com/library/ms364049.aspx)
- [Exemplos de GridView do ASP.NET 2.0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Simplificamos e dados de sintaxe de associação do ASP.NET 2.0 estendido](http://www.15seconds.com/issue/040630.htm)
- [Temas no ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Estilos de lado do servidor usando temas](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Como Aplicar temas do ASP.NET por meio de programação](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Hilton Giesenow. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
> [Próximo](declarative-parameters-vb.md)
