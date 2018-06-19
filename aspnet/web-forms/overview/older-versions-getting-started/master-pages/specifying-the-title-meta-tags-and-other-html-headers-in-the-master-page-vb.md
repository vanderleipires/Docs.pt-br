---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: Especificar o título, marcas Meta e outros cabeçalhos HTML na página mestra (VB) | Microsoft Docs
author: rick-anderson
description: Examina as técnicas diferentes para definir sortidas &lt;head&gt; elementos da página mestra da página de conteúdo.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: b8bf9d32eee3e35ffc84521f7f82f7beecc99a0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891439"
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Especificar o título, marcas Meta e outros cabeçalhos HTML na página mestra (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Examina as técnicas diferentes para definir sortidas &lt;head&gt; elementos da página mestra da página de conteúdo.


## <a name="introduction"></a>Introdução

Novas páginas mestras criadas no Visual Studio 2008 têm, por padrão, dois controles ContentPlaceHolder: um chamado `head`e está localizado no `<head>` elemento; e um chamado `ContentPlaceHolder1`, colocada dentro do formulário da Web. A finalidade de `ContentPlaceHolder1` é definir uma região de formulário da Web que podem ser personalizados em uma base de página por página. O `head` ContentPlaceHolder permite páginas para adicionar conteúdo personalizado para o `<head>` seção. (É claro, esses dois ContentPlaceHolders pode ser modificados ou removidos e ContentPlaceHolder adicional pode ser adicionado para a página mestra. Nossa página mestre, `Site.master`, atualmente tem quatro controles ContentPlaceHolder.)

O HTML `<head>` elemento serve como um repositório para obter informações sobre o documento de página da web que não faz parte do documento em si. Isso inclui informações como o título da página da web, informações de metadados usados por mecanismos de pesquisa ou rastreadores internos e links para recursos externos, como feeds RSS, JavaScript e CSS arquivos. Algumas dessas informações podem ser pertinentes a todas as páginas no site. Por exemplo, você talvez queira globalmente importar as mesmas regras CSS e os arquivos JavaScript para todas as páginas ASP.NET. No entanto, há partes de `<head>` elemento que são específicas da página. O título da página é um excelente exemplo.

Neste tutorial, podemos examinar como definir global e específico de página `<head>` marcação de seção na página mestra e em suas páginas de conteúdo.

## <a name="examining-the-master-pagesheadsection"></a>Examinando da página mestra`<head>`seção

O arquivo de página mestra padrão criado pelo Visual Studio 2008 contém a seguinte marcação no seu `<head>` seção:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Observe que o `<head>` elemento contém um `runat="server"` atributo, que indica que ele é um controle de servidor (em vez de HTML estático). Todas as páginas ASP.NET derivam o [ `Page` classe](https://msdn.microsoft.com/library/system.web.ui.page.aspx), que está localizado no `System.Web.UI` namespace. Essa classe contém um [ `Header` propriedade](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) que fornece acesso para a página `<head>` região. Usando o `Header` propriedade, pode definir o título de uma página ASP.NET ou adicionar marcação adicional para o renderizado `<head>` seção. É possível, em seguida, para personalizar uma página de conteúdo `<head>` elemento escrevendo um trecho de código na página de `Page_Load` manipulador de eventos. Examinamos como definir programaticamente o título da página na etapa 1.

A marcação que mostra o `<head>` elemento acima também inclui um controle ContentPlaceHolder chamado `head`. Esse controle ContentPlaceHolder não é necessário, como páginas de conteúdo podem adicionar conteúdo personalizado para o `<head>` elemento programaticamente. É útil, no entanto, em situações em que uma página de conteúdo precisa adicionar marcação estática para o `<head>` elemento como a marcação estático pode ser adicionado declarativamente para o controle de conteúdo correspondente em vez de forma programática.

Além de `<title>` elemento e `head` ContentPlaceHolder, a página mestra `<head>` elemento deve conter nenhum `<head>`-nível marcação que é comuns a todas as páginas. Em nosso site, todas as páginas de usam as regras CSS definidas no `Styles.css` arquivo. Consequentemente, atualizamos o `<head>` elemento o [ *criando um Layout de todo o Site com páginas mestras* ](creating-a-site-wide-layout-using-master-pages-vb.md) tutorial para incluir um correspondente `<link>` elemento. Nosso `Site.master` atual da página mestra `<head>` marcação é mostrada abaixo.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Etapa 1: Configuração título da página de conteúdo um

Título da página da web é especificado por meio de `<title>` elemento. É importante definir o título de cada página para um valor apropriado. Quando visita uma página, o título é exibido na barra de título do navegador. Além disso, quando uma página de uso de indicadores, os navegadores usam o título da página como o nome sugerido para o indicador. Além disso, vários mecanismos de pesquisa mostram o título da página, ao exibir os resultados da pesquisa.

> [!NOTE]
> Por padrão, o Visual Studio define o `<title>` elemento na página mestra "Página sem título". Da mesma forma, novas páginas ASP.NET têm seus `<title>` definido como "Página sem título" muito. Como é fácil esquecer de definir o título da página para um valor apropriado, há muitas páginas na Internet com o título "Página sem título". A pesquisa do Google para páginas da web com esse título retorna aproximadamente 2,460,000 resultados. Até mesmo a Microsoft está sujeita a publicar páginas da web com o título "Página sem título". No momento da redação deste artigo, uma pesquisa do Google relatado 236 essas páginas da web no domínio Microsoft.com.


Uma página ASP.NET pode especificar seu título em uma das seguintes maneiras:

- Colocando o valor diretamente dentro de `<title>` elemento
- Usando o `Title` atributo o `<%@ Page %>` diretiva
- Configurando programaticamente a página `Title` propriedade usando código como `Page.Title="title"` ou `Page.Header.Title="title"`.

Conteúdo de páginas não têm um `<title>` elemento, como ele está definido na página mestra. Portanto, para definir o título de uma página de conteúdo, você pode usar o `<%@ Page %>` da diretiva `Title` atributo ou defina-o por meio de programação.

### <a name="setting-the-pages-title-declaratively"></a>Definir o título da página declarativamente

Título de uma página de conteúdo pode ser definido declarativamente por meio de `Title` atributo do [ `<%@ Page %>` diretiva](https://msdn.microsoft.com/library/ydy4x04a.aspx). Essa propriedade pode ser definida diretamente modificando o `<%@ Page %>` diretiva ou usando a janela Propriedades. Vamos examinar as duas abordagens.

Na exibição da fonte, localize a `<%@ Page %>` diretiva, que está no topo da marcação declarativa da página. O `<%@ Page %>` diretiva para `Default.aspx` segue:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

O `<%@ Page %>` diretiva especifica atributos específicos de página usados pelo mecanismo ASP.NET quando a análise e compilação de página. Isso inclui o arquivo de página mestra, o local do seu arquivo de código e seu título, entre outras informações.

Por padrão, ao criar uma nova página de conteúdo, o Visual Studio define o `Title` atributo "Página sem título". Alterar `Default.aspx`do `Title` de atributo de "Página sem título" para "Tutoriais de página mestra" e, em seguida, exibir a página por meio de um navegador. A Figura 1 mostra a barra de título do navegador, que reflete o novo título da página.


![Barra de título do navegador agora mostra &quot;tutoriais de página mestra&quot; em vez de &quot;página sem título&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Figura 01**: barra de título do navegador agora mostra "Tutoriais de página mestra" em vez de "Página sem título"


O título da página também pode ser definido na janela de propriedades. Na janela Propriedades, selecione o documento na lista suspensa para propriedades de nível de página de carga, que inclui o `Title` propriedade. A Figura 2 mostra a janela de propriedades após `Title` foi definida como "Tutoriais de página mestra".


![Você pode configurar o título da janela Propriedades, muito](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Figura 02**: você pode configurar o título da janela Propriedades, muito


### <a name="setting-the-pages-title-programmatically"></a>Definir o título da página programaticamente

A página mestra `<head runat="server">` marcação é convertida em uma [ `HtmlHead` classe](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) instância quando a página é processada pelo mecanismo ASP.NET. O `HtmlHead` classe tiver um [ `Title` propriedade](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) cujo valor é refletido no renderizado `<title>` elemento. Esta propriedade é acessível na classe code-behind de uma página ASP.NET por meio de `Page.Header.Title`; essa mesma propriedade também pode ser acessada via `Page.Title`.

Para praticar a definir o título da página programaticamente, navegue até o `About.aspx` por trás do código da página de classe e criar um manipulador de eventos para a página `Load` eventos. Em seguida, definir o título da página "tutoriais de página mestra:: sobre:: *data*", onde *data* é a data atual. Depois de adicionar esse código de seu `Page_Load` manipulador de eventos deve ser semelhante à seguinte:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

A Figura 3 mostra a barra de título do navegador ao visitar o `About.aspx` página.


![O título da página é definido por meio de programação e inclui a data atual](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Figura 03**: título da página é definido por meio de programação e inclui a data atual


## <a name="step-2-automatically-assigning-a-page-title"></a>Etapa 2: Atribuir automaticamente um título da página

Como vimos na etapa 1, o título da página pode ser definido declarativamente ou programaticamente. Se você se esquecer de alterar explicitamente o título para algo mais descritivo, no entanto, a página terá o título padrão, "Página sem título". Idealmente, o título da página seria definido automaticamente para nós que nós não especificar explicitamente seu valor. Por exemplo, se em tempo de execução, o título da página é "Página sem título", é aconselhável têm o título atualizado automaticamente para ser o mesmo nome de arquivo da página ASP.NET. A boa notícia é que, com um pouco de trabalho inicial, é possível ter o título atribuído automaticamente.

Todas as páginas da web ASP.NET derivam de `Page` classe no namespace System.Web.UI. O `Page` classe define a funcionalidade mínima necessária para uma página ASP.NET e expõe as propriedades essenciais como `IsPostBack`, `IsValid`, `Request`, e `Response`, entre outros. Em geral, todas as páginas em um aplicativo web requer recursos adicionais ou funcionalidade. Uma maneira comum de fornecer isso é criar uma classe base de página personalizado. Uma classe de página de base personalizada é criar uma classe que deriva de `Page` classe e inclui a funcionalidade adicional. Quando essa classe base tiver sido criado, você pode ter suas páginas ASP.NET derivado dele (em vez de `Page` classe) e, portanto, oferecendo a funcionalidade estendida para suas páginas ASP.NET.

Nesta etapa, podemos criar uma página de base que define o título da página ao nome do arquivo da página ASP.NET automaticamente se o título caso contrário, não foi explicitamente definido. Etapa 3 olha para definir o título da página com base no mapa do site.

> [!NOTE]
> Um exame completo de criação e uso de classes de página de base personalizada está além do escopo desta série tutorial. Para obter mais informações, leia [usando uma classe de Base personalizada para Code-Behind Classes suas páginas ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Criando a classe Base da página

A primeira tarefa é criar uma classe de página de base, que é uma classe que estende o `Page` classe. Comece adicionando um `App_Code` pasta ao seu projeto clicando duas vezes no nome do projeto no Gerenciador de soluções, escolha Adicionar pasta do ASP.NET e, em seguida, selecionando `App_Code`. Em seguida, clique duas vezes no `App_Code` pasta e adicione uma nova classe chamada `BasePage.vb`. A Figura 4 mostra o Gerenciador de soluções após o `App_Code` pasta e `BasePage.vb` classe foram adicionados.


![Adicionar uma pasta App_Code e uma classe denominada BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Figura 04**: adicionar um `App_Code` pasta e uma classe denominada `BasePage`


> [!NOTE]
> Visual Studio oferece suporte a dois modos de gerenciamento de projeto: os projetos de Site da Web e projetos de aplicativo Web. O `App_Code` pasta foi projetada para ser usado com o modelo de projeto de Site. Se você estiver usando o modelo de projeto de aplicativo Web, coloque o `BasePage.vb` classe em uma pasta chamada algo diferente de `App_Code`, como `Classes`. Para obter mais informações sobre este tópico, consulte [migrando um projeto de Site para um projeto de aplicativo Web](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).


Porque a página de base personalizada serve como a classe base para classes de code-behind de páginas do ASP.NET, ele precisa estender o `Page` classe.


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Sempre que uma página ASP.NET é solicitada, ele prossegue por meio de uma série de etapas, culminando na página solicitada está sendo renderizada em HTML. Podemos pode tocar em um estágio, substituindo o `Page` da classe `OnEvent` método. Para nossa base de página Vamos definir automaticamente o título, se ele não foi explicitamente especificado pelo `LoadComplete` estágio (que, como você poderia esperar, ocorre após o `Load` estágio).

Para fazer isso, substitua o `OnLoadComplete` método e insira o código a seguir:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

O `OnLoadComplete` método inicia determinando se o `Title` propriedade ainda não foi explicitamente definida. Se o `Title` é de propriedade `Nothing`, uma cadeia de caracteres vazia ou tem o valor "Página sem título", ele é atribuído ao nome do arquivo da página solicitada do ASP.NET. O caminho físico para a página solicitada do ASP.NET - `C:\MySites\Tutorial03\Login.aspx`, por exemplo - está acessível por meio de `Request.PhysicalPath` propriedade. O `Path.GetFileNameWithoutExtension` método é usado para extrair apenas a parte do nome do arquivo, e esse nome de arquivo é então atribuído ao `Page.Title` propriedade.

> [!NOTE]
> Eu convido você a aumentar essa lógica para melhorar o formato do título. Por exemplo, se o nome do arquivo da página é `Company-Products.aspx`, o código acima produzirá o título "Produtos da empresa", mas o ideal é o traço deve ser substituído com um espaço, como "Produtos da empresa". Além disso, considere a adição de um espaço sempre que houver uma alteração. Ou seja, considere adicionar o código que transforma o nome do arquivo `OurBusinessHours.aspx` para um título de "nosso horário comercial".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Ter as páginas de conteúdo que herdam a classe Base da página

Agora, precisamos atualizar as páginas ASP.NET em nosso site derivar a página de base personalizada (`BasePage`) em vez da `Page` classe. Para fazer isso, vá para cada classe code-behind e altere a declaração de classe de:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

Para:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Depois de fazer isso, visite o site por meio de um navegador. Se você visita uma página cujo título é definido explicitamente, como `Default.aspx` ou `About.aspx`, o título explicitamente especificado é usado. Se, no entanto, você visita uma página cujo título não foi alterado do padrão ("página sem título"), a classe da página base define o título para o nome de arquivo da página.

A Figura 5 mostra o `MultipleContentPlaceHolders.aspx` página quando visualizada através de um navegador. Observe que o título é precisamente nome de arquivo da página (menos a extensão), "MultipleContentPlaceHolders".


[![Se um título não é especificado explicitamente, o nome de arquivo da página é usado automaticamente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Figura 05**: se um título não é especificado explicitamente, o nome de arquivo da página é usado automaticamente ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Etapa 3: Baseando o título da página no mapa do Site

O ASP.NET oferece uma estrutura de mapa de site robusto que permite que os desenvolvedores de página Definir um mapa de site hierárquica em um recurso externo (como uma tabela de banco de dados ou arquivo XML) junto com os controles da Web para exibir informações sobre o mapa do site (como SiteMapPath, Menu e controles TreeView).

A estrutura de mapa de site também pode ser acessada por meio de programação de classe code-behind de uma página ASP.NET. Dessa maneira é possível definir automaticamente o título da página para o título do seu nó correspondente no mapa do site. Vamos melhorar a `BasePage` classe criada na etapa 2 para que ele oferece essa funcionalidade. Mas primeiro, precisamos criar um mapa de site para o nosso site.

> [!NOTE]
> Este tutorial presume que o leitor já esteja familiarizado com o ASP. Recursos de mapa de site do NET. Para obter mais informações sobre como usar o mapa do site, consulte meu série de artigos em várias partes, [examinando ASP. Navegação no Site da rede](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Criar o mapa de Site

O sistema de mapa do site é criado sobre a [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que separa o mapa do site API da lógica que serializa informações de mapa de site entre a memória e um armazenamento persistente. O .NET Framework vem com o [ `XmlSiteMapProvider` classe](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), que é o provedor de mapa de site padrão. Como o nome sugere, `XmlSiteMapProvider` usa um arquivo XML como seu armazenamento de mapa de site. Vamos usar esse provedor para definir o mapa de site.

Comece criando um arquivo de mapa de site na pasta raiz do site do denominada `Web.sitemap`. Para fazer isso, clique no nome do site no Gerenciador de soluções, escolha Adicionar Novo Item e selecione o modelo de mapa do Site. Certifique-se de que o arquivo é nomeado `Web.sitemap` e clique em Adicionar.


[![Adicionar um arquivo chamado SiteMap para pasta de raiz do site](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Figura 06**: adicionar um arquivo chamado `Web.sitemap` para a pasta da raiz do site ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


Adicione o seguinte XML para o `Web.sitemap` arquivo:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Esse XML define a estrutura de mapa de site hierárquica mostrada na Figura 7.


![O mapa do Site é atualmente composta de três mapa de nós de Site](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Figura 07**: O mapa do Site é atualmente composta de três mapa de nós de Site


Atualizaremos a estrutura de mapa de site em tutoriais futuros como adicionamos novos exemplos.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Atualizar a página mestra para incluir os controles de navegação de Web

Agora que temos um mapa do site definido, vamos atualizar a página mestra para incluir os controles de navegação de Web. Especificamente, vamos adicionar um controle ListView para a coluna esquerda na seção lições que renderiza uma lista não ordenada com um item de lista para cada nó definido no mapa do site.

> [!NOTE]
> O controle ListView é novo para o ASP.NET versão 3.5. Se você estiver usando uma versão anterior do ASP.NET, use o controle Repetidor. Para obter mais informações sobre o controle ListView, consulte [usando ASP.NET 3.5 controles ListView e DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Iniciar, removendo a marcação de lista não ordenada existente da seção lições. Em seguida, arraste um controle ListView da caixa de ferramentas e solte-o sob as lições título. ListView está localizado na seção de dados da caixa de ferramentas, juntamente com outros controles de exibição: o GridView, DetailsView e FormView. Definir o ListView `ID` propriedade `LessonsList`.

O Assistente de configuração de fonte de dados optar por associar ListView para um novo controle SiteMapDataSource chamado `LessonsDataSource`. O controle SiteMapDataSource retorna a estrutura hierárquica de mapa do sistema de site.


[![Associar um controle SiteMapDataSource ao controle LessonsList ListView](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Figura 08**: associar um controle SiteMapDataSource ao controle ListView LessonsList ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


Depois de criar o controle SiteMapDataSource, precisamos definir modelos de ListView para que ele renderiza uma lista não ordenada com um item de lista para cada nó retornado pelo controle SiteMapDataSource. Isso pode ser feito usando a seguinte marcação de modelo:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

O `LayoutTemplate` gera a marcação para uma lista não ordenada (`<ul>...</ul>`) enquanto o `ItemTemplate` renderiza cada item retornado por SiteMapDataSource como um item de lista (`<li>`) que contém um link para a lição específica.

Depois de configurar modelos de ListView, visite o site. Como mostra a Figura 9, a seção de lições contém um único item com marcadores inicial. Onde estão o sobre e meio de controles ContentPlaceHolder várias lições? SiteMapDataSource foi projetado para retornar um conjunto hierárquico de dados, mas o controle ListView só pode exibir um único nível da hierarquia. Consequentemente, somente o primeiro nível de nós de mapa de site retornados por SiteMapDataSource é exibido.


[![A seção de lições contém um Item de lista única](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Figura 09**: A seção de lições contém um único Item de lista ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


Para exibir vários níveis pode aninhar várias ListViews dentro de `ItemTemplate`. Essa técnica é examinada no [ *páginas mestras e navegação de Site* tutorial](../../data-access/introduction/master-pages-and-site-navigation-vb.md) do meu [trabalhando com série de tutoriais de dados](../../data-access/index.md). No entanto, para esta série de tutoriais o mapa do site conterá apenas um dois níveis: Home (nível superior); e cada lição como um filho da página inicial. Em vez de criação de um ListView aninhado, em vez disso, pode instruir o SiteMapDataSource não retornar o nó inicial, definindo seu [ `ShowStartingNode` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) para `False`. O efeito líquido é que o SiteMapDataSource inicia, retornando o segundo nível de nós de mapa de site.

Com essa alteração, ListView exibe itens com marcadores de sobre e usando vários controles ContentPlaceHolder lições, mas omite um item de marcador para a página inicial. Para corrigir isso, podemos explicitamente adicionar um item de marcador home no `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Configurando o SiteMapDataSource para omitir o nó inicial e adicionar explicitamente um item de marcador de início, a seção de lições agora exibe a saída desejada.


[![A seção de lições contém um Item de marcador para a página inicial e cada nó filho](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Figura 10**: A seção de lições contém um Item de marcador para a página inicial e cada nó de filho ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Definindo o título com base no mapa do Site

Com o mapa de site no lugar, podemos atualizar nosso `BasePage` classe para usar o título especificado no mapa do site. Como foi feito na etapa 2, só queremos usar o título do nó de mapa de site se o título da página não foi explicitamente definido pelo desenvolvedor de página. Se a página solicitada não tem explicitamente definido título da página e não foi encontrada no mapa do site, em seguida, nós retornaremos usando o nome de arquivo da página solicitada (menos a extensão), como foi feito na etapa 2. A Figura 11 ilustra esse processo de decisão.


![Na ausência de um explicitamente definir título da página, o título do nó das mapa de Site correspondente é usado](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Figura 11**: ausência de um explicitamente definir título da página, título do nó das mapa de Site correspondente é usado


Atualização de `BasePage` da classe `OnLoadComplete` método para incluir o código a seguir:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Como antes, o `OnLoadComplete` método inicia, determinando se o título da página foi definido explicitamente. Se `Page.Title` é `Nothing`, uma cadeia de caracteres vazia ou é atribuído o valor "Página sem título", em seguida, o código automaticamente atribui um valor para `Page.Title`.

Para determinar o título a ser usado, o código começa consultando o [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx)do [ `CurrentNode` propriedade](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode` Retorna o [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) instância no mapa do site que corresponde à página solicitada no momento. Supondo que a página solicitada no momento está localizado no mapa do site, o `SiteMapNode`do `Title` propriedade é atribuída para o título da página. Se a página solicitada no momento não está no mapa do site, `CurrentNode` retorna `Nothing` e nome de arquivo da página solicitada é usado como o título (como foi feito na etapa 2).

A Figura 12 mostra o `MultipleContentPlaceHolders.aspx` página quando visualizada através de um navegador. Como título desta página não é definido explicitamente, título correspondente site mapa do seu nó será usado.


![Título da página MultipleContentPlaceHolders.aspx retirado do mapa do Site](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Figura 12**: título da página de MultipleContentPlaceHolders.aspx o retirados do mapa do Site


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Etapa 4: Adicionando outra marcação específico de página para o`<head>`seção

As etapas 1, 2 e 3 observou Personalizando o `<title>` elemento em uma base de página por página. Além `<title>`, o `<head>` seção pode conter `<meta>` elementos e `<link>` elementos. Conforme observado anteriormente neste tutorial, `Site.master`do `<head>` seção inclui uma `<link>` elemento `Styles.css`. Porque isso `<link>` elemento é definido dentro da página mestra, ele está incluído no `<head>` seção para todas as páginas de conteúdo. Mas como podemos adicionar `<meta>` e `<link>` elementos em uma base de página por página?

A maneira mais fácil para adicionar conteúdo específico de página para o `<head>` seção está criando um controle ContentPlaceHolder na página mestra. Já existe um ContentPlaceHolder tal (chamado `head`). Portanto, para adicionar personalizado `<head>` marcação, crie um correspondente controle na página de conteúdo e posicione a marcação de lá.

Para ilustrar personalizado adicionando `<head>` marcação para uma página, vamos incluem um `<meta>` elemento description nosso conjunto de páginas de conteúdo atual. O `<meta>` elemento description fornece uma breve descrição sobre a página da web; a maioria dos mecanismos de pesquisa incorporar essas informações de alguma forma, ao exibir os resultados da pesquisa.

Um `<meta>` elemento description tem a seguinte forma:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Para adicionar essa marcação para uma página de conteúdo, adicione o texto acima para o controle de conteúdo que é mapeado para a página mestra `head` ContentPlaceHolder. Por exemplo, para definir um `<meta>` elemento description para `Default.aspx`, adicione a seguinte marcação:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Porque o `head` ContentPlaceHolder não está dentro do corpo da página HTML, a marcação adicionada ao controle de conteúdo não será exibida no modo de exibição de Design. Para ver o `<meta>` descrição elemento visite `Default.aspx` através de um navegador. Depois que a página é carregada, exibir o código-fonte e observe que o `<head>` seção inclui a marcação especificada no controle de conteúdo.

Reserve um tempo para adicionar `<meta>` elementos descrição `About.aspx`, `MultipleContentPlaceHolders.aspx`, e `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Adicionar programaticamente a marcação para o`<head>`região

O `head` ContentPlaceHolder nos permite adicionar declarativamente marcação personalizada para a página mestra `<head>` região. Marcação personalizada também pode ser adicionada por meio de programação. Lembre-se de que o `Page` da classe `Header` propriedade retorna o `HtmlHead` instância definida na página mestra (o `<head runat="server">`).

Ser capaz de adicionar programaticamente o conteúdo para o `<head>` região é útil quando o conteúdo a ser adicionado é dinâmico. Talvez ela é baseada no usuário visitar a página. Talvez ele está sendo retirado de um banco de dados. Independentemente do motivo, você pode adicionar conteúdo para o `HtmlHead` , adicionando controles a seu `Controls` coleção da seguinte forma:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

O código acima adiciona o `<meta>` elemento palavras-chave para o `<head>` região, que fornece uma lista delimitada por vírgulas das palavras-chave que descrevem a página. Observe que para adicionar um `<meta>` marca que você criar um [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) instância, defina seu `Name` e `Content` propriedades e, em seguida, adicioná-lo para o `Header`do `Controls` coleção. Da mesma forma, para adicionar programaticamente uma `<link>` elemento, criar um [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) do objeto, defina suas propriedades e, em seguida, adicioná-lo para o `Header`do `Controls` coleção.

> [!NOTE]
> Para adicionar marcação arbitrária, crie um [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) instância, defina seu `Text` propriedade e, em seguida, adicioná-lo para o `Header`do `Controls` coleção.


## <a name="summary"></a>Resumo

Neste tutorial vimos uma variedade de maneiras de adicionar `<head>` marcação de região em uma base de página por página. Uma página mestre deve incluir um `HtmlHead` instância (`<head runat="server">`) com um ContentPlaceHolder. O `HtmlHead` instância permite que as páginas de conteúdo para acessar programaticamente o `<head>` região e declarativamente e programaticamente definir a página do título; o controle ContentPlaceHolder permite que a marcação personalizadas a serem adicionadas à `<head>` seção declarativamente por meio de um controle de conteúdo.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Definir dinamicamente o título da página no ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Examinando ASP. Navegação do Site da rede](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Como usar marcas HTML Meta](http://searchenginewatch.com/showPage.html?page=2167931)
- [Páginas mestras no ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Usando o ASP.NET do 3.5 controles ListView e DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Usando uma classe Base personalizada para Code-Behind Classes suas páginas ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 3.5 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Zack Jones e Suchi Banerjee. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](multiple-contentplaceholders-and-default-content-vb.md)
> [Próximo](urls-in-master-pages-vb.md)
