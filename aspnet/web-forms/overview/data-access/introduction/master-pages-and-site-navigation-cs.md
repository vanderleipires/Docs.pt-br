---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: "Páginas mestras e navegação de Site (c#) | Microsoft Docs"
author: rick-anderson
description: "Uma característica comum de sites amigável é que eles têm um esquema de layout e navegação de página consistente, todo o site. Este tutorial explica como y..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: 32ddda8d883a99805d2448c9673e585bfe9ef2f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="master-pages-and-site-navigation-c"></a>Páginas mestras e navegação de Site (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) ou [baixar PDF](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Uma característica comum de sites amigável é que eles têm um esquema de layout e navegação de página consistente, todo o site. Este tutorial analisa como você pode criar uma aparência consistente em todas as páginas que podem ser facilmente atualizadas.


## <a name="introduction"></a>Introdução

Uma característica comum de sites amigável é que eles têm um esquema de layout e navegação de página consistente, todo o site. O ASP.NET 2.0 apresenta dois novos recursos que simplificam bastante a implementação de ambos os uma página de todo o site layout e navegação de esquema: páginas mestras e navegação do site. Páginas mestras permitem aos desenvolvedores criar um modelo de todo o site com editáveis designados. Este modelo, em seguida, pode ser aplicado a páginas ASP.NET no site. Essas páginas ASP.NET só precisam fornecer conteúdo para a página mestra especificadas editáveis todos os outros marcação na página mestra é idêntica em todas as páginas ASP.NET que usam a página mestra. Esse modelo permite que os desenvolvedores definir e centralizar um layout de página de todo o site, tornando mais fácil de criar uma aparência consistente em todas as páginas que podem ser facilmente atualizadas.

O [sistema de navegação de site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) fornece um mecanismo para desenvolvedores de página Definir um mapa de site e uma API para esse mapa de site a ser consultado por meio de programação. Novos controles de navegação da Web a que no Menu, TreeView e SiteMapPath, facilitam renderizam todo ou parte do mapa do site em um elemento de interface de usuário de navegação comuns. Usaremos o provedor de navegação do site padrão, que significa que o mapa do site será definido em um arquivo de formato XML.

Para ilustrar esses conceitos e tornar nosso site tutoriais mais utilizável, vamos nos concentrar nesta lição, definir um layout de página de todo o site, Implementando um mapa de site e adicionar a navegação da interface do usuário. No final deste tutorial teremos um design de site refinada para a criação de páginas da web tutorial.


[![O resultado final deste tutorial](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Figura 1**: O resultado final do Tutorial este ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>Etapa 1: Criar a página mestra

A primeira etapa é criar a página mestra para o site. Agora nosso site consiste apenas o conjunto de dados tipado (`Northwind.xsd`, no `App_Code` pasta), as classes BLL (`ProductsBLL.cs`, `CategoriesBLL.cs`, e assim por diante, tudo no `App_Code` pasta), o banco de dados (`NORTHWND.MDF`, no `App_Data` pasta), o arquivo de configuração (`Web.config`) e um arquivo de folha de estilos CSS (`Styles.css`). Eu limpou as páginas e os arquivos de demonstração usando a DAL e BLL dos dois primeiros tutoriais, já que estamos será reavaliando nesses exemplos mais detalhadamente em tutoriais futuros.


![Os arquivos em nosso projeto](master-pages-and-site-navigation-cs/_static/image4.png)

**Figura 2**: os arquivos em nosso projeto


Para criar uma página mestre, com o botão direito no nome do projeto no Gerenciador de soluções e escolha Adicionar Novo Item. Em seguida, selecione o tipo de página mestra na lista de modelos e nomeie-o `Site.master`.


[![Adicionar uma nova página mestra para o site](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Figura 3**: adicionar uma nova página mestra para o site ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image7.png))


Defina o layout de página de todo o site aqui na página mestra. Você pode usar o modo de exibição de Design e adicionar qualquer controles de Layout ou Web necessários, ou você pode adicionar manualmente a marcação manualmente na exibição da fonte. Minha página mestre, uso [folhas de estilo em cascata](http://www.w3schools.com/css/default.asp) para posicionamento e estilos com as configurações de CSS definidas no arquivo externo `Style.css`. Enquanto você não pode dizer sobre a marcação mostrada abaixo, as regras de CSS são definidas, de modo que a navegação `<div>`do conteúdo é posicionado absolutamente para que ele é exibido à esquerda e tem uma largura fixa de 200 pixels.

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Uma página mestra define o layout de página estática e as regiões que podem ser editadas por páginas ASP.NET que usam a página mestra. Essas áreas de conteúdo editáveis são indicadas pelo controle ContentPlaceHolder, que pode ser visto no conteúdo `<div>`. Nossa página mestra tem um único ContentPlaceHolder (`MainContent`), mas a página mestra pode ter vários ContentPlaceHolders.

Com a marcação acima, a alternar para o modo de exibição de Design mostra o layout da página mestra. As páginas do ASP.NET que usem essa página mestre terá esse layout uniforme, com a capacidade de especificar a marcação para o `MainContent` região.


[![A página mestre, quando exibido por meio da exibição de Design](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Figura 4**: A página mestre, quando exibido por meio do modo de Design ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>Etapa 2: Adicionando uma página inicial para o site

Com a página mestra definida, estamos prontos para adicionar as páginas ASP.NET para o site. Vamos começar adicionando `Default.aspx`, homepage do nosso site. Clique com botão direito no nome do projeto no Gerenciador de soluções e escolha Adicionar Novo Item. Escolha a opção de formulário da Web na lista de modelo e o nome do arquivo `Default.aspx`. Além disso, verifique a caixa de seleção "Selecione página mestra".


[![Adicionar um novo formulário da Web, a página mestra selecione caixa de seleção](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Figura 5**: adicionar um novo formulário da Web, a página mestra selecione caixa de seleção ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image13.png))


Depois de clicar no botão Okey, podemos for solicitados a escolher qual página mestre deve usar essa nova página ASP.NET. Embora você possa ter várias páginas mestras em seu projeto, temos somente um.


[![Escolha a página mestra que deve usar essa página ASP.NET](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Figura 6**: escolha a página mestra esse uso de deve de página do ASP.NET ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image16.png))


Depois de escolher a página mestra, as novas páginas ASP.NET conterá a seguinte marcação:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

No `@Page` existe diretiva é uma referência para o arquivo de página mestra usada (`MasterPageFile="~/Site.master"`), e a marcação da página ASP.NET contém um controle de conteúdo para cada um dos controles ContentPlaceHolder definidos na página mestre, com o controle `ContentPlaceHolderID` mapear o conteúdo de controle para um ContentPlaceHolder específico. O controle de conteúdo é onde você coloca a marcação que devem aparecer em ContentPlaceHolder correspondente. Definir o `@Page` da diretiva `Title` de atributos para a página inicial e acrescente conteúdo de boas-vindas para o controle de conteúdo:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

O `Title` atributo o `@Page` diretiva nos permite definir o título da página da página do ASP.NET, embora o `<title>` elemento é definido na página mestra. Podemos também definir o título programaticamente, usando `Page.Title`. Observe também que referências da página mestra para folhas de estilos (como `Style.css`) são atualizados automaticamente para que elas funcionam em qualquer página do ASP.NET, independentemente da pasta em que a página ASP.NET está em relativa à página mestre.

Alternar para o modo de Design, que podemos ver a aparência de nossa página em um navegador. Observe que no Design da exibição para a página do ASP.NET que somente as conteúdo editáveis regiões são editáveis a marcação ContentPlaceHolder não definida na página mestre está esmaecido.


[![O modo de exibição de Design para a página ASP.NET mostra as regiões editáveis e não editáveis.](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Figura 7**: O modo de Design para o ASP.NET página mostra ambas as o editável e não editável regiões ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image19.png))


Quando o `Default.aspx` página for visitada por um navegador, o mecanismo do ASP.NET automaticamente mescla o conteúdo da página da página mestra e o ASP. NET de conteúdo e renderiza o conteúdo mesclado no HTML final que é enviado para o navegador do solicitante. Quando o conteúdo da página mestra é atualizado, todas as páginas ASP.NET que usam essa página mestra terá seu conteúdo mesclada novamente com a nova página mestra conteúda na próxima vez que forem solicitados. Em resumo, o modelo de página mestra permite para uma única página modelo de layout para ser definido (a página mestra) cujas alterações são refletidas imediatamente em todo o site.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Adicionando páginas ASP.NET adicionais para o site

Vamos adicionar adicionais stubs de página ASP.NET para o site que eventualmente armazenará as várias demonstrações de relatório. Mais de 35 demonstrações no total, portanto, em vez de criar todas as páginas de stub vamos apenas criar poucas primeiro. Como também haverá muitas categorias de demonstrações, para gerenciar melhor as demonstrações adicionar uma pasta para as categorias. Adicione as seguintes três pastas agora:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Finalmente, adicione novos arquivos, conforme mostrado no Gerenciador de soluções na Figura 8. Ao adicionar cada arquivo, lembre-se de marcar a caixa de seleção "Selecione página mestra".


![Adicione os seguintes arquivos](master-pages-and-site-navigation-cs/_static/image20.png)

**Figura 8**: adicione os seguintes arquivos


## <a name="step-2-creating-a-site-map"></a>Etapa 2: Criar um mapa de Site

Um dos desafios de gerenciamento de um site composto de mais de uma série de páginas é fornecer uma maneira simples para os visitantes navegar pelo site. Em primeiro lugar, a estrutura de navegação do site deve ser definida. Em seguida, essa estrutura deve ser convertida em elementos de interface de usuário navegável, como menus ou trilha. Por fim, todo esse processo precisa ser mantida e atualizadas à medida que novas páginas são adicionadas ao site e os existentes removidos. Antes do ASP.NET 2.0, os desenvolvedores estavam em seus próprios para criar estrutura de navegação do site, mantê-lo e converter em elementos de interface do usuário navegável. Com o ASP.NET 2.0, entretanto, os desenvolvedores podem utilizar muito flexível criado no sistema de navegação do site.

O sistema de navegação de site do ASP.NET 2.0 fornece um meio para um desenvolvedor para definir um mapa de site e, em seguida, acessar essas informações por meio de uma API através de programação. ASP.NET vem com um provedor de mapa de site que espera dados de mapa de site a ser armazenado em um arquivo XML formatado de uma maneira específica. Mas, como o sistema de navegação do site é criado no [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) pode ser estendido para dar suporte a modos alternativos para serializar as informações de mapa do site. Artigo de Jeff Prosise [o SQL Site mapa provedor que já foram aguardando](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) mostra como criar um provedor de mapa de site que armazena o mapa do site em um banco de dados do SQL Server; outra opção é criar [um provedor de mapa de site com base em a estrutura de sistema de arquivos](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Para este tutorial, no entanto, vamos usar o provedor de mapa de site padrão que é fornecido com o ASP.NET 2.0. Para criar o mapa do site, simplesmente clique no nome do projeto no Gerenciador de soluções, escolha Adicionar Novo Item e escolha a opção de mapa do Site. Deixe o nome como `Web.sitemap` e clique no botão Adicionar.


[![Adicionar um mapa de Site ao seu projeto](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Figura 9**: adicionar um mapa de Site ao seu projeto ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image23.png))


O arquivo de mapa de site é um arquivo XML. Observe que o Visual Studio fornece IntelliSense para a estrutura de mapa de site. O arquivo de mapa de site deve ter o `<siteMap>` nó como o nó raiz, que deve conter exatamente um `<siteMapNode>` elemento filho. Primeira `<siteMapNode>` elemento, em seguida, pode conter um número arbitrário de descendentes `<siteMapNode>` elementos.

Defina o mapa de site para simular a estrutura de sistema de arquivos. Ou seja, adicionar um `<siteMapNode>` elemento para cada uma das três pastas e filho `<siteMapNode>` elementos para cada uma das páginas ASP.NET nessas pastas, da seguinte forma:

Sitemap


[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

O mapa de site define a estrutura de navegação do site, que é uma hierarquia que descreve as várias seções do site. Cada `<siteMapNode>` elemento `Web.sitemap` representa uma seção na estrutura de navegação do site.


[![O mapa de Site representa uma estrutura de Navegação hierárquica](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Figura 10**: O mapa de Site representa uma estrutura hierárquica de navegação ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image26.png))


O ASP.NET expõe a estrutura do mapa de site por meio do .NET Framework [SiteMap classe](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx). Essa classe tem um `CurrentNode` propriedade, que retorna informações sobre o usuário atualmente está visitando; o `RootNode` propriedade retorna a raiz do mapa do site (Home, o mapa de site). Tanto o `CurrentNode` e `RootNode` propriedades retorno [SiteMapNode](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.aspx) instâncias que têm propriedades como `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`e assim por diante, que permitem o mapa do site hierarquia a ser percorrido.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Etapa 3: Exibir um Menu com base no mapa do Site

Acessando dados no ASP.NET 2.0 pode ser realizado por meio de programação, como no ASP.NET 1. x, ou declarativamente, por meio do novo [controles da fonte de dados](https://msdn.microsoft.com/en-us/library/ms227679.aspx). Há vários controles de fonte de dados internos, como o controle SqlDataSource, para acessar dados do banco de dados relacional, o controle ObjectDataSource, para acessar dados de classes e outros. Você ainda pode criar seus próprios [controles da fonte de dados personalizados](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/en-us/dnvs05/html/DataSourceCon1.asp).

Os controles de fonte de dados servem como um proxy entre sua página ASP.NET e os dados subjacentes. Para exibir os dados recuperados de um controle da fonte de dados, vamos normalmente adiciona outro controle de Web para a página e associá-lo ao controle de fonte de dados. Para associar um controle da Web a um controle de fonte de dados, basta definir o controle de Web `DataSourceID` propriedade para o valor do controle de fonte de dados `ID` propriedade.

Para ajudar a trabalhar com dados do mapa do site, o ASP.NET inclui o controle SiteMapDataSource, que permite associar um controle de Web de mapa de site do nosso site. Dois controles da Web de TreeView e Menu são usados para fornecer uma interface de usuário de navegação. Para associar os dados de mapa de site a um desses dois controles, basta adicionar SiteMapDataSource para a página como um modo de exibição de árvore ou Menu controle cuja `DataSourceID` propriedade será definida adequadamente. Por exemplo, poderíamos adicionar um controle de Menu para a página mestra usando a seguinte marcação:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Um nível mais refinado de controle sobre o HTML emitido, podemos associar o controle SiteMapDataSource ao controle repetidor, da seguinte forma:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

O controle SiteMapDataSource retorna o nível de hierarquia um mapa do site por vez, começando com o nó raiz do mapa de site (Home, o mapa de site), em seguida, o próximo nível (relatórios básicos, filtragem de relatórios e formatação personalizada) e assim por diante. Ao associar SiteMapDataSource a um repetidor, enumera o primeiro nível retornado e instancia o `ItemTemplate` para cada `SiteMapNode` instância no primeiro nível. Para acessar uma propriedade específica do `SiteMapNode`, podemos usar `Eval(propertyName)`, que é como obtemos cada `SiteMapNode`do `Url` e `Title` propriedades para o controle de hiperlink.

O exemplo de Repetidor acima processará a marcação a seguir:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Esses nós de mapa de site (relatórios básicos, filtragem de relatórios e formatação personalizada) compõem o *segundo* nível do mapa do site que está sendo processado, não o primeiro. Isso ocorre porque o SiteMapDataSource `ShowStartingNode` propriedade é definida como False, causando SiteMapDataSource ignorar o nó raiz do mapa de site e comece em vez disso, retornando o segundo nível na hierarquia de mapa do site.

Para exibir os filhos para o relatório básico, filtragem de relatórios e formatação personalizada `SiteMapNode` s, podemos adicionar outro repetidor a inicial do repetidor `ItemTemplate`. Essa segunda repetidor será associado ao `SiteMapNode` da instância `ChildNodes` propriedade, da seguinte forma:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Esses dois repetidores resultam na seguinte marcação (algumas marcações foi removida por questão de brevidade):


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Usando CSS estilos escolhido de [Rachel Andrew](http://www.rachelandrew.co.uk/)do livro [o antologia de CSS: 101 de essencial dicas, truques &amp; experimenta](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), o `<ul>` e `<li>` elementos são denominados, de forma que a marcação produz a seguinte saída visual:


![Um Menu composto de duas repetidores e CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Figura 11**: um Menu composto de duas repetidores e CSS


Esse menu é na página mestra e associados ao mapa do site definido no `Web.sitemap`, o que significa que qualquer alteração feita no mapa do site será refletida imediatamente em todas as páginas que usam o `Site.master` página mestra.

## <a name="disabling-viewstate"></a>Desabilitando o ViewState

Todos os controles do ASP.NET, opcionalmente, podem manter seu estado para o [exibir estado](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), que é serializado como um campo de formulário oculto no HTML renderizado. Estado de exibição é usado por controles de lembrar seu estado alterado de forma programática em postagens, como os dados associados a um controle de Web de dados. Enquanto o estado de exibição permite que informações sejam lembradas em postagens, aumenta o tamanho da marcação que deve ser enviada para o cliente e pode levar a inchaço graves de página se não for monitorada atentamente. Controles da Web de dados especialmente GridView são particularmente famoso por adicionando dezenas de quilobytes extras de marcação para uma página. Esse aumento pode ser muito importantes para os usuários da intranet ou de banda larga, o estado de exibição pode adicionar vários segundos para a viagem de ida e para usuários de dial-up.

Para ver o impacto do estado de exibição, visite uma página em um navegador e, em seguida, exibir o código-fonte enviado pela página da web (no Internet Explorer, vá para o menu de exibição e escolha a opção de origem). Você também pode ativar [rastreamento de página](https://msdn.microsoft.com/en-us/library/sfbfw58f.aspx) para ver a alocação de estado de exibição usada por cada um dos controles na página. As informações de estado de exibição são serializadas em um campo de formulário oculto chamado `__VIEWSTATE`, localizado em um `<div>` elemento imediatamente após a abertura `<form>` marca. Estado de exibição é persistido apenas quando há um formulário da Web que está sendo usada. Se sua página ASP.NET não incluir um `<form runat="server">` em sua sintaxe declarativa não haverá um `__VIEWSTATE` campo de formulário oculto na marcação renderizada.

O `__VIEWSTATE` campo de formulário gerado pela página mestre adiciona aproximadamente 1.800 bytes para marcação gerada da página. Este inchaço extra está vencido principalmente para o controle repetidor, como o conteúdo do controle SiteMapDataSource é mantido para o estado de exibição. Enquanto uma bytes 1.800 extras podem não parecer muito obter empolgados, ao usar um GridView com muitos registros e campos, o estado de exibição pode ultrapassar facilmente por um fator de 10 ou mais.

Estado de exibição pode ser desabilitado no nível de página ou controle definindo o `EnableViewState` propriedade `false`e, portanto, reduzindo o tamanho da marcação renderizada. Desde o estado de exibição para um dado controle persiste os dados associados aos dados de controle da Web em postagens de Web, ao desabilitar o estado de exibição para um dado controle Web os dados devem ser associados em cada postback. No ASP.NET versão 1. x essa responsabilidade ficou em ombros do desenvolvedor de página. com o ASP.NET 2.0, entretanto, os controles da Web de dados serão vinculado novamente para seu controle de fonte de dados em cada postback se necessário.

Para reduzir o estado de exibição da página permite definir o controle de Repetidor `EnableViewState` propriedade `false`. Isso pode ser feito através da janela de propriedades no Designer ou declarativamente na exibição da fonte. Depois de fazer essa alteração declarativo do repetidor deve parecer com:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Após essa alteração, a página renderizada exibição tamanho de estado pode ser reduzido para um simples 52 bytes, um aumento de 97% em estado de exibição tamanho! Nos tutoriais de toda essa série será Desabilitaremos o estado de exibição dos dados de controles da Web por padrão para reduzir o tamanho da marcação renderizada. Na maioria dos exemplos de `EnableViewState` propriedade será definida como `false` e feito sem menção. A única vez em que é de exibição de estado será discutido em cenários em que ele deve ser habilitado para que os dados da Web de controle para fornecer sua funcionalidade esperada.

## <a name="step-4-adding-breadcrumb-navigation"></a>Etapa 4: Adicionando navegação estrutural

Para concluir a página mestra, vamos adicionar um elemento de interface do usuário de navegação de trilha de navegação para cada página. Navegação estrutural mostra rapidamente usuários sua posição atual dentro da hierarquia do site. Adicionando navegação estrutural no ASP.NET 2.0 é fácil apenas adicionar um controle SiteMapPath para a página. Nenhum código é necessária.

Para nosso site, adicione este controle para o cabeçalho `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

Navegação estrutural mostra a página atual, o usuário está visitando na hierarquia de mapa do site, bem como "ancestrais," do nó de mapa site até a raiz (base, em nosso mapa de site).


![A trilha de navegação exibe a página atual e seus ancestrais no Site da hierarquia do mapa](master-pages-and-site-navigation-cs/_static/image28.png)

**Figura 12**: navegação estrutural exibe a página atual e seus ancestrais no Site da hierarquia do mapa


## <a name="step-5-adding-the-default-page-for-each-section"></a>Etapa 5: Adicionar a página padrão para cada seção

Os tutoriais no nosso site são divididos em diferentes categorias básicas de emissão de relatórios, filtragem, formatação personalizada, e assim por diante com uma pasta para cada categoria e os tutoriais correspondentes como páginas ASP.NET dentro dessa pasta. Além disso, cada pasta contém um `Default.aspx` página. Para essa página padrão, vamos exibir todos os tutoriais para a seção atual. Ou seja, para o `Default.aspx` no `BasicReporting` pasta haveria links para `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, e `ProgrammaticParams.aspx`. Aqui, novamente, podemos usar o `SiteMap` definidos na classe e um controle da Web para exibir essas informações com base no mapa do site de dados `Web.sitemap`.

Permite exibir uma lista não ordenada usando um repetidor novamente, mas desta vez que exibiremos o título e a descrição dos tutoriais. A marcação e código para realizar esta será precisam ser repetido para cada `Default.aspx` página, pode encapsular essa lógica de interface do usuário em um [controle de usuário](https://msdn.microsoft.com/en-us/library/y6wb1a0e.aspx). Crie uma pasta no site de chamada `UserControls` e adicionar para que um novo item do tipo de controle de usuário da Web chamado `SectionLevelTutorialListing.ascx`e adicione a seguinte marcação:


[![Adicionar um novo controle de usuário da Web para a pasta UserControls](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Figura 13**: adicionar um novo controle de usuário da Web para o `UserControls` pasta ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs


[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

No exemplo anterior repetidor é associado a `SiteMap` dados repetidor declarativamente; o `SectionLevelTutorialListing` controle de usuário, no entanto, é feito por meio de programação. No `Page_Load` manipulador de eventos, é feita uma verificação para garantir que essa página s URL é mapeada para um nó no mapa do site. Se este controle de usuário é usado em uma página que não tem um `<siteMapNode>` entrada, `SiteMap.CurrentNode` retornará `null` e nenhum dado será associado a Repetidor. Supondo que temos uma `CurrentNode`, podemos associar seu `ChildNodes` coleção Repetidor. Desde que o mapa de site está configurado, de modo que o `Default.aspx` página de cada seção é o nó pai de todos os tutoriais dentro dessa seção, esse código irá exibir links para e descrições de todos os tutoriais da seção, como mostrado na captura de tela abaixo.

Quando este repetidor tiver sido criado, abra a `Default.aspx` páginas em cada uma das pastas, vá para o modo de exibição de Design e simplesmente arraste o controle de usuário no Gerenciador de soluções para a superfície de Design onde você deseja que o tutorial lista apareça.


[![O controle de usuário tem que foram adicionados ao Default.aspx](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Figura 14**: O controle de usuário tem foi adicionado ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image34.png))


[![Os tutoriais de Reporting básicos estão listados](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Figura 15**: são listados os tutoriais de Reporting básica ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image37.png))


## <a name="summary"></a>Resumo

Com o mapa de site definido e a página mestra completa, agora temos um esquema de layout e navegação de página consistente para nossos tutoriais relacionados a dados. Independentemente de quantas páginas que adicionarmos ao nosso site, atualizar as informações de navegação de página de todo o site de layout ou de site é um processo rápido e simple devido a essas informações estão sendo centralizados. Especificamente, as informações de layout de página são definidas na página mestra `Site.master` e o site do mapa em `Web.sitemap`. Não é necessário gravar *qualquer* de código para atingir esse mecanismo de layout e navegação de página de todo o site, e podemos manter WYSIWYG total suporte do designer no Visual Studio.

Ao concluir a camada de acesso a dados e a camada de lógica comercial e ter uma navegação de página consistente layout e site definida, estamos prontos para começar a explorar os padrões comuns de relatório. No [próximo](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) três tutoriais analisaremos tarefas básicas de relatório exibir dados recuperados de BLL em GridView, DetailsView e FormView controla.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Visão geral de páginas mestras do ASP.NET](https://msdn.microsoft.com/en-us/library/wtxbf3hh.aspx)
- [Páginas mestras no ASP.NET 2.0](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 Design modelos](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Visão geral de navegação do Site do ASP.NET](https://msdn.microsoft.com/en-us/library/e468hxky.aspx)
- [Examinar o ASP.NET 2.0 da navegação do Site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Recursos de navegação do ASP.NET 2.0 de Site](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Noções básicas sobre estado de exibição de ASP.NET](https://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnaspp/html/viewstate.asp)
- [Como: ativar o rastreamento para uma página ASP.NET](https://msdn.microsoft.com/en-us/library/94c55d08%28VS.80%29.aspx)
- [Controles de usuário do ASP.NET](https://msdn.microsoft.com/en-us/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Liz Shulok, Dennis Patterson e Giesenow Hilton. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](creating-a-business-logic-layer-cs.md)
[Próximo](creating-a-data-access-layer-vb.md)
