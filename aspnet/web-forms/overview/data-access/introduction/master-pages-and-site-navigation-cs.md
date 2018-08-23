---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Páginas mestras e navegação no Site (c#) | Microsoft Docs
author: rick-anderson
description: Uma característica comum de sites amigáveis é que eles têm um esquema de layout e a navegação de página consistente, todo o site. Este tutorial examina como y...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: d9ae2fb5a79817053a260e7d0f85992a011f471b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831651"
---
<a name="master-pages-and-site-navigation-c"></a>Páginas mestras e navegação no Site (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) ou [baixar PDF](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Uma característica comum de sites amigáveis é que eles têm um esquema de layout e a navegação de página consistente, todo o site. Este tutorial aborda como você pode criar uma aparência consistente em todas as páginas que podem ser facilmente atualizadas.


## <a name="introduction"></a>Introdução

Uma característica comum de sites amigáveis é que eles têm um esquema de layout e a navegação de página consistente, todo o site. O ASP.NET 2.0 apresenta dois novos recursos que simplificam bastante a implementação de ambos os uma página em todo o site de layout e a navegação esquema: páginas mestras e navegação no site. Páginas mestras permitem que os desenvolvedores criem um modelo de todo o site com regiões editáveis designados. Esse modelo, em seguida, pode ser aplicado às páginas do ASP.NET no site. Essas páginas ASP.NET apenas precisam fornecer conteúdo para a página mestra especificado do regiões editáveis todas as outras marcações na página mestra é idêntica em todas as páginas ASP.NET que usam a página mestra. Esse modelo permite que os desenvolvedores definam e centralizar um layout de página de todo o site, tornando mais fácil de criar uma aparência consistente em todas as páginas que podem ser facilmente atualizadas.

O [sistema de navegação de site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) fornece um mecanismo para os desenvolvedores de páginas definir um mapa de site tanto uma API para esse mapa de site a ser consultado por meio de programação. Os novos controles de Web de navegação a que no Menu, TreeView e SiteMapPath, facilitam renderizam todo ou parte do mapa do site em um elemento de interface de usuário de navegação comum. Usaremos o provedor de navegação do site padrão, que significa que nosso mapa de site será definido em um arquivo de formato XML.

Para ilustrar esses conceitos e tornar o nosso site tutoriais mais utilizável, vamos nos concentrar nesta lição, definir um layout de página em todo o site, a implementação de um mapa de site e adicionar a navegação da interface do usuário. No final deste tutorial, teremos um design bem acabados do site para a criação de páginas da web tutorial.


[![O resultado final deste tutorial](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Figura 1**: O resultado final do Tutorial deste ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>Etapa 1: Criar a página mestra

A primeira etapa é criar a página mestra para o site. Agora nosso site consiste em apenas o conjunto de dados tipado (`Northwind.xsd`, no `App_Code` pasta), as classes BLL (`ProductsBLL.cs`, `CategoriesBLL.cs`e assim por diante, tudo na `App_Code` pasta), o banco de dados (`NORTHWND.MDF`, no `App_Data` pasta), o arquivo de configuração (`Web.config`) e um arquivo de folha de estilos CSS (`Styles.css`). Eu limpo os essas páginas e os arquivos de demonstração usando a BLL e DAL dos dois primeiros tutoriais, já que estamos será reavaliando nesses exemplos mais detalhadamente em tutoriais futuros.


![Os arquivos em nosso projeto](master-pages-and-site-navigation-cs/_static/image4.png)

**Figura 2**: os arquivos em nosso projeto


Para criar uma página mestra, com o botão direito no nome do projeto no Gerenciador de soluções e escolha Add New Item. Em seguida, selecione o tipo de página mestra na lista de modelos e nomeie- `Site.master`.


[![Adicionar uma nova página mestra ao site](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Figura 3**: adicionar uma nova página mestra ao site ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image7.png))


Defina o layout de página em todo o site aqui na página mestra. Você pode usar o modo de exibição de Design e adicionar controles qualquer Layout ou da Web que você precisa, ou você pode adicionar manualmente a marcação manualmente na exibição da fonte. Na minha página mestra, usei [folhas de estilo em cascata](http://www.w3schools.com/css/default.asp) para posicionamento e estilos com as configurações de CSS definidas no arquivo externo `Style.css`. Enquanto você não pode dizer sobre a marcação mostrada abaixo, as regras CSS são definidas, de modo que a navegação `<div>`do conteúdo de uma posição absoluta para que ele é exibido à esquerda e tem uma largura fixa de 200 pixels.

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Uma página mestra define o layout de página estática e as regiões que podem ser editadas pelas páginas do ASP.NET que usam a página mestra. Essas áreas de conteúdo editáveis são indicadas pelo controle ContentPlaceHolder, que pode ser visto no conteúdo `<div>`. Nossa página mestre tem um único ContentPlaceHolder (`MainContent`), mas a página mestra pode ter vários ContentPlaceHolders.

Com a marcação inserida acima, a alternar para a exibição de Design mostra o layout da página mestra. Qualquer página do ASP.NET que use essa página mestre terá esse layout uniforme, com a capacidade de especificar a marcação para o `MainContent` região.


[![A página mestra, quando visualizado por meio da exibição de Design](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Figura 4**: A página mestra, quando exibidas por meio do modo de Design ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>Etapa 2: Adicionando uma home page para o site

Com a página mestra definida, estamos prontos para adicionar as páginas do ASP.NET para o site. Vamos começar adicionando `Default.aspx`, homepage do nosso site. Clique com botão direito no nome do projeto no Gerenciador de soluções e escolha Add New Item. Escolha a opção de formulário da Web na lista de modelo e o nome do arquivo `Default.aspx`. Além disso, verifique a caixa de seleção "Selecionar página mestra".


[![Adicione um novo formulário da Web, verificando a página mestra selecione caixa de seleção](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Figura 5**: Adicione um novo formulário da Web, verificando a página mestra selecione caixa de seleção ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image13.png))


Depois de clicar no botão Okey, somos solicitados a escolher qual página mestre essa nova página do ASP.NET deve usar. Embora você possa ter várias páginas mestras em seu projeto, temos apenas um.


[![Escolha a página mestra que Use essa página ASP.NET](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Figura 6**: escolha a página mestra deste ASP.NET página deve usar ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image16.png))


Depois de escolher a página mestra, as novas páginas ASP.NET conterá a seguinte marcação:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

No `@Page` diretiva lá é uma referência para o arquivo de página mestra usado (`MasterPageFile="~/Site.master"`), e a marcação da página ASP.NET contém um controle de conteúdo para cada um dos controles ContentPlaceHolder definidos na página mestra, com o controle `ContentPlaceHolderID` mapear o conteúdo do controle para um ContentPlaceHolder específico. O controle de conteúdo é onde você coloca a marcação você deseja que apareça no ContentPlaceHolder correspondente. Defina as `@Page` da diretiva `Title` atributo à página inicial e adicionar algum conteúdo as boas-vindas para o controle de conteúdo:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

O `Title` de atributo em de `@Page` diretiva nos permite definir o título da página da página ASP.NET, mesmo que o `<title>` elemento é definido na página mestra. Também podemos definir o título de forma programática, usando `Page.Title`. Observe também que referências da página mestra para folhas de estilo (como `Style.css`) são atualizadas automaticamente para que eles funcionem de qualquer página do ASP.NET, independentemente da pasta em que a página do ASP.NET está em relativa à página mestra.

Alternar para a exibição de Design que podemos ver qual será a aparência de nossa página em um navegador. Observe que, no Design, exibir para a página do ASP.NET que somente as conteúdo editáveis regiões são editáveis a marcação não ContentPlaceHolder definida na página mestra está esmaecida.


[![Modo de Design para a página ASP.NET mostra ambas as regiões editáveis e não editáveis](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Figura 7**: O modo de Design para o ASP.NET página mostra ambos os o editável e regiões de não editável ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image19.png))


Quando o `Default.aspx` página for visitada por um navegador, o mecanismo do ASP.NET mescla automaticamente o conteúdo da página da página mestra e o ASP. NET do conteúdo e renderiza o conteúdo mesclado no HTML final que é enviado para baixo para o navegador solicitante. Quando o conteúdo da página mestra é atualizado, todas as páginas ASP.NET que usam essa página mestra terá seu conteúdo mesclada novamente com o conteúdo da página mestra novo na próxima vez que forem solicitados. Em resumo, o modelo de página mestra permite para uma única página modelo de layout seja definido (a página mestra) cujas alterações são refletidas imediatamente em todo o site.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Adicionando páginas ASP.NET adicionais para o site

Vamos dedicar um tempo para adicionar stubs adicionais de página ASP.NET para o site que eventualmente irá conter as várias demonstrações de geração de relatórios. Haverá mais de 35 demonstrações no total, portanto, em vez de criar todas as páginas de stub vamos apenas criar poucas primeiro. Como também haverá muitas categorias de demonstrações, para gerenciar melhor as demonstrações adicionar uma pasta para as categorias. Adicione as seguintes três pastas agora:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Por fim, adicione novos arquivos conforme mostrado no Gerenciador de soluções na Figura 8. Ao adicionar cada arquivo, lembre-se de marcar a opção "Selecionar página mestra".


![Adicione os seguintes arquivos](master-pages-and-site-navigation-cs/_static/image20.png)

**Figura 8**: adicione os seguintes arquivos


## <a name="step-2-creating-a-site-map"></a>Etapa 2: Criar um mapa de Site

Um dos desafios de gerenciamento de um site da Web composta de mais de um punhado de páginas é fornecer uma maneira simples para que os visitantes navegar pelo site. Para começar, a estrutura de navegação do site deve ser definida. Em seguida, essa estrutura deve ser convertida em elementos de interface de usuário navegável, como menus ou trilhas. Por fim, todo esse processo precisa ser mantido e atualizado conforme novas páginas são adicionadas ao site e os existentes removidos. Antes do ASP.NET 2.0, os desenvolvedores eram sobre como seus próprios para criar estrutura de navegação do site, mantê-la e convertendo em elementos de interface do usuário navegável. Com o ASP.NET 2.0, no entanto, os desenvolvedores podem utilizar muito flexível criado no sistema de navegação do site.

O sistema de navegação de site do ASP.NET 2.0 fornece um meio para um desenvolvedor para definir um mapa do site e, em seguida, acessar essas informações por meio de uma API programática. ASP.NET é fornecido com um provedor de mapa de site que espera dados de mapa de site a ser armazenado em um arquivo XML formatado de uma maneira específica. Mas, como o sistema de navegação do site é criado na [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) ele pode ser estendido para dar suporte a modos alternativos para serializar as informações de mapa do site. Artigo de Jeff Prosise [o SQL Site mapa provedor você já foi aguardando para](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) mostra como criar um provedor de mapa de site que armazena o mapa do site em um banco de dados do SQL Server; outra opção é criar [um provedor de mapa de site com base em a estrutura do sistema de arquivos](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Para este tutorial, no entanto, vamos usar o provedor de mapa de site padrão que é fornecido com o ASP.NET 2.0. Para criar o mapa do site, simplesmente clique com botão direito no nome do projeto no Gerenciador de soluções, escolha Add New Item e escolha a opção de mapa do Site. Deixe o nome como `Web.sitemap` e clique no botão Adicionar.


[![Adicionar um mapa do Site ao seu projeto](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Figura 9**: adicionar um mapa de Site ao seu projeto ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image23.png))


O arquivo de mapa de site é um arquivo XML. Observe que o Visual Studio oferece IntelliSense para a estrutura do mapa de site. O arquivo de mapa de site deve ter o `<siteMap>` nó como nó raiz, que deve conter exatamente um `<siteMapNode>` elemento filho. Primeiro `<siteMapNode>` elemento, em seguida, pode conter um número arbitrário de descendentes `<siteMapNode>` elementos.

Defina o mapa de site para imitar a estrutura do sistema de arquivos. Ou seja, adicionar um `<siteMapNode>` elemento para cada uma das três pastas e filho `<siteMapNode>` elementos para cada uma das páginas ASP.NET nessas pastas, desta forma:

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

O mapa do site define a estrutura de navegação do site, que é uma hierarquia que descreve as várias seções do site. Cada `<siteMapNode>` elemento no `Web.sitemap` representa uma seção na estrutura de navegação do site.


[![O mapa de Site representa uma estrutura de Navegação hierárquica](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Figura 10**: O mapa de Site representa uma estrutura de Navegação hierárquica ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image26.png))


O ASP.NET expõe a estrutura do mapa do site por meio do .NET Framework [classe de mapa do site](https://msdn.microsoft.com/library/system.web.sitemap.aspx). Essa classe tem um `CurrentNode` propriedade, que retorna informações sobre a seção que o usuário está visitando atualmente; a `RootNode` propriedade retorna a raiz do mapa do site (Home, no nosso mapa de site). Tanto a `CurrentNode` e `RootNode` retorno de propriedades [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) instâncias que têm propriedades como `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`e assim por diante, que permitem o mapa do site hierarquia a ser percorrido.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Etapa 3: Exibir um Menu com base no mapa do Site

Acessando dados no ASP.NET 2.0 pode ser feito por meio de programação, como no ASP.NET 1. x, ou declarativamente, por meio da nova [controles de fonte de dados](https://msdn.microsoft.com/library/ms227679.aspx). Há vários controles de fonte de dados internos, como o controle SqlDataSource, para acessar dados do banco de dados relacional, o controle ObjectDataSource, para acessar dados de classes e outros. Você ainda pode criar seus próprios [controles de fonte de dados personalizados](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Os controles de fonte de dados servem como um proxy entre sua página ASP.NET e os dados subjacentes. Para exibir os dados recuperados de um controle da fonte de dados, vamos normalmente adiciona outro controle de Web para a página e associá-lo para o controle de fonte de dados. Para associar um controle da Web a um controle de fonte de dados, basta definir o controle de Web `DataSourceID` propriedade para o valor do controle de fonte de dados `ID` propriedade.

Para ajudar a trabalhar com dados do mapa do site, o ASP.NET inclui o controle SiteMapDataSource, que nos permite associar um controle de Web em relação ao mapa de site do nosso site. Dois controles de Web o TreeView e Menu são comumente usados para fornecer uma interface de usuário de navegação. Para associar os dados de mapa de site a um desses dois controles, basta adicionar um SiteMapDataSource para a página juntamente com um modo de exibição de árvore ou controle de Menu cuja `DataSourceID` propriedade está definida de acordo. Por exemplo, poderíamos adicionar um controle de Menu para a página mestra usando a seguinte marcação:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Um nível mais refinado de controle sobre o HTML emitido, podemos fazer a ligação o controle SiteMapDataSource para o controle Repeater, da seguinte forma:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

O controle SiteMapDataSource retorna o nível de hierarquia de um mapa do site por vez, começando com o nó raiz do mapa de site (Home, no nosso mapa de site), em seguida, o próximo nível (relatórios básicos, filtragem de relatórios e formatação personalizada) e assim por diante. Ao ligar a SiteMapDataSource para um repetidor, enumera o primeiro nível retornado e cria uma instância de `ItemTemplate` para cada `SiteMapNode` instância no primeiro nível. Para acessar uma propriedade específica do `SiteMapNode`, podemos usar `Eval(propertyName)`, que é como podemos obter cada `SiteMapNode`do `Url` e `Title` propriedades para o controle de hiperlink.

O exemplo de Repeater acima será renderizado a marcação a seguir:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Esses nós de mapa de site (relatórios básicos, filtragem de relatórios e formatação personalizada) compõem o *segundo* nível do mapa do site que está sendo processado, não o primeiro. Isso ocorre porque a SiteMapDataSource `ShowStartingNode` propriedade é definida como False, fazendo com que a SiteMapDataSource ignorar o nó raiz do mapa de site e em vez disso, iniciar, retornando o segundo nível na hierarquia do mapa do site.

Para exibir os filhos para relatórios básicos, filtragem de relatórios e formatação personalizada `SiteMapNode` s, podemos adicionar outro Repeater para o Repeater inicial `ItemTemplate`. Esse segundo Repeater será associado à `SiteMapNode` da instância `ChildNodes` propriedade, da seguinte forma:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Esses dois repetidores resultam na marcação a seguir (algumas marcações foi removida para fins de brevidade):


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Usando CSS estilos escolhido da [Rachel Andrew](http://www.rachelandrew.co.uk/)do livro [o antologia de CSS: dicas essenciais 101, truques, &amp; Hacks](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), o `<ul>` e `<li>` elementos são denominados, de modo que a marcação produz a seguinte saída visual:


![Um Menu composto de duas repetidores e algum da CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Figura 11**: um Menu composto de duas repetidores e algum da CSS


Esse menu é na página mestra e associados ao mapa de site definido em `Web.sitemap`, o que significa que qualquer alteração no mapa de site serão imediatamente refletida em todas as páginas que usam o `Site.master` página mestra.

## <a name="disabling-viewstate"></a>Desabilitar o ViewState

Todos os controles ASP.NET podem, opcionalmente, persiste seu estado para o [estado de exibição](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), que é serializado como um campo de formulário oculto no HTML renderizado. Estado de exibição é usado pelos controles para lembrar o seu estado alterado de forma programática entre postbacks, como os dados associados a um controle de Web de dados. Enquanto o estado de exibição permite que informações sejam lembradas entre postbacks, ele aumenta o tamanho da marcação que deve ser enviada ao cliente e pode levar a inchaço graves de página se não for monitorado de perto. Controles da Web de dados especialmente o GridView são particularmente famoso por adicionar dezenas de quilobytes extras de marcação para uma página. Embora esse aumento pode ser insignificante para usuários de banda larga ou da intranet, estado de exibição pode adicionar vários segundos para a viagem de ida e para usuários de dial-up.

Para ver o impacto do estado de exibição, visite uma página em um navegador e, em seguida, exibir o código-fonte enviado pela página da web (no Internet Explorer, vá até o menu Exibir e escolha a opção de origem). Você também pode ativar [rastreamento de página](https://msdn.microsoft.com/library/sfbfw58f.aspx) para ver a alocação de estado de exibição usada por cada um dos controles na página. As informações de estado de exibição são serializadas em um campo de formulário oculto chamado `__VIEWSTATE`, localizado em um `<div>` elemento imediatamente após a abertura `<form>` marca. Estado de exibição é persistido apenas quando há um formulário da Web que está sendo usada. Se sua página ASP.NET não incluir um `<form runat="server">` em sua sintaxe declarativa que não haja um `__VIEWSTATE` campo de formulário oculto na marcação renderizada.

O `__VIEWSTATE` campo de formulário gerado pela página mestre adiciona aproximadamente 1.800 bytes para a marcação da página gerada. Essa sobrecarga extra se deve principalmente ao controle Repeater, pois o conteúdo do controle SiteMapDataSource é persistido para o estado de exibição. Enquanto um bytes 1.800 extra pode não parecer muito para ficar animado sobre, ao usar um GridView com vários campos e registros, o estado de exibição pode facilmente ultrapassar por um fator de 10 ou mais.

Estado de exibição pode ser desabilitado no nível de página ou controle definindo a `EnableViewState` propriedade para `false`e, portanto, reduzindo o tamanho da marcação renderizada. Desde o estado de exibição para um controle persiste os dados associados aos dados de controle da Web em postagens de Web de dados, ao desabilitar o estado de exibição de dados de um controle de Web os dados devem ser associados em cada postagem. Na versão do ASP.NET 1.x essa responsabilidade caiu nos ombros do desenvolvedor de página. com o ASP.NET 2.0, no entanto, os controles da Web de dados serão associar novamente a seu controle de fonte de dados em cada postagem se necessário.

Para reduzir o estado de exibição da página Vamos definir do controle Repeater `EnableViewState` propriedade para `false`. Isso pode ser feito por meio da janela de propriedades no Designer ou de forma declarativa na exibição da fonte. Depois de fazer essa alteração marcação declarativa do repetidor deve parecer com:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Depois que essa alteração, a página renderizada exibir tamanho do estado foi reduzido para um mero 52 bytes, uma economia de 97% em estado de exibição tamanho! Nos tutoriais em toda esta série será desabilitamos o estado de exibição dos dados de controles da Web por padrão para reduzir o tamanho da marcação renderizada. Na maioria dos exemplos de `EnableViewState` propriedade será definida como `false` e feito isso sem mencionar. A única vez em que é de exibição de estado será discutido em cenários em que ele deve ser habilitado para que os dados da Web de controle para fornecer sua funcionalidade esperada.

## <a name="step-4-adding-breadcrumb-navigation"></a>Etapa 4: Adicionando navegação de trilha

Para concluir a página mestra, vamos adicionar um elemento de interface do usuário de navegação de trilha para cada página. A trilha de navegação mostra rapidamente aos usuários sua posição atual dentro da hierarquia do site. Adicionando uma trilha no ASP.NET 2.0 é fácil simplesmente adicionar um controle SiteMapPath para a página. Nenhum código é necessária.

Para nosso site, adicionar esse controle para o cabeçalho `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

A trilha de navegação mostra a página atual, o usuário está visitando na hierarquia do mapa do site, bem como o site "ancestrais," do nó de mapa todo o caminho até a raiz (Home, no nosso mapa de site).


![A trilha de navegação exibe a página atual e seus ancestrais no Site de hierarquia do mapa](master-pages-and-site-navigation-cs/_static/image28.png)

**Figura 12**: A trilha de navegação exibe a página atual e seus ancestrais no Site de hierarquia do mapa


## <a name="step-5-adding-the-default-page-for-each-section"></a>Etapa 5: Adicionar a página padrão para cada seção

Os tutoriais em nosso site são divididos em categorias diferentes relatórios básicos, filtragem, a formatação personalizada, e assim por diante, com uma pasta para cada categoria e os tutoriais correspondentes como páginas ASP.NET dentro dessa pasta. Além disso, cada pasta contém um `Default.aspx` página. Para essa página padrão, vamos exibir todos os tutoriais para a seção atual. Ou seja, para o `Default.aspx` no `BasicReporting` pasta teríamos links para `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, e `ProgrammaticParams.aspx`. Aqui, novamente, podemos usar o `SiteMap` classe e um controle de Web para exibir essas informações com base no mapa do site de dados definidos em `Web.sitemap`.

Vamos exibir uma lista não ordenada usando um repetidor novamente, mas desta vez, que vamos exibir o título e a descrição dos tutoriais. Já que a marcação e código para realizar este será precisam ser repetido para cada `Default.aspx` página, podemos pode encapsular essa lógica de interface do usuário em um [controle de usuário](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Crie uma pasta no site de chamada `UserControls` e adicione para que um novo item do tipo de controle de usuário da Web chamado `SectionLevelTutorialListing.ascx`e adicione a seguinte marcação:


[![Adicionar um novo controle de usuário da Web para a pasta UserControls](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Figura 13**: adicionar um novo controle de usuário da Web para o `UserControls` pasta ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs


[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

No exemplo anterior Repeater é associado a `SiteMap` dados para o Repeater declarativamente; o `SectionLevelTutorialListing` controle de usuário, no entanto, faz isso por meio de programação. No `Page_Load` manipulador de eventos, é feita uma verificação para garantir que essa página s URL é mapeada para um nó no mapa do site. Se esse controle de usuário é usado em uma página que não tem um correspondente `<siteMapNode>` entrada de `SiteMap.CurrentNode` retornará `null` e nenhum dado será associado ao Repetidor. Supondo que temos uma `CurrentNode`, podemos associar seu `ChildNodes` coleção para o Repeater. Já que nosso mapa de site está configurado, de modo que o `Default.aspx` página em cada seção é o nó pai de todos os tutoriais dentro dessa seção, esse código exibirá links para e descrições de todos os tutoriais da seção, conforme mostrado na captura de tela abaixo.

Quando esse Repeater tiver sido criado, abra o `Default.aspx` páginas em cada uma das pastas, vá para a exibição de Design e simplesmente arrastar o controle de usuário do Gerenciador de soluções para a superfície de Design onde você deseja que a lista de tutoriais apareça.


[![O controle de usuário tem que foram adicionados ao default. aspx](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Figura 14**: O controle de usuário tem foi adicionado ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image34.png))


[![Os tutoriais de Reporting básicos estão listados](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Figura 15**: os tutoriais do Reporting básicos estão listados ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image37.png))


## <a name="summary"></a>Resumo

Com o mapa de site definido e a página mestre concluída, agora temos um esquema de layout e a navegação de página consistente para nossos tutoriais relacionados a dados. Independentemente de quantas páginas que adicionamos ao nosso site, atualizar as informações de navegação de página em todo o site de layout ou o site é um processo rápido e simple devido a essas informações estão sendo centralizados. Especificamente, as informações de layout de página são definidas na página mestra `Site.master` e o site do mapa em `Web.sitemap`. Não precisamos escrever *qualquer* código para alcançar esse mecanismo de layout e a navegação de página em todo o site, e podemos manter o suporte do designer WYSIWYG completo no Visual Studio.

Ter concluído a camada de acesso a dados e a camada de lógica comercial e ter uma navegação de página consistente site e o layout definida, estamos prontos para começar a explorar os padrões comuns de geração de relatórios. No [próxima](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) três tutoriais examinaremos tarefas básicas de geração de relatórios exibindo dados recuperados da BLL em GridView, DetailsView e FormView controla.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Visão geral de páginas mestras do ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Páginas mestras no ASP.NET 2.0](http://odetocode.com/Articles/419.aspx)
- [Modelos do ASP.NET 2.0 de Design](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Visão geral de navegação do Site do ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Examinar o ASP.NET 2.0 da navegação do Site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Recursos de navegação do ASP.NET 2.0 Site](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Compreendendo o estado modo de exibição do ASP.NET](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Como: habilitar o rastreamento de uma página ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Controles de usuário ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisores de avanço para este tutorial foram Liz Shulok, Dennis Patterson e Hilton Giesenow. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-a-business-logic-layer-cs.md)
> [Próximo](creating-a-data-access-layer-vb.md)
