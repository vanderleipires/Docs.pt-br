---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: Especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra (VB) | Microsoft Docs
author: rick-anderson
description: Examina as técnicas diferentes para definir sortidas &lt;head&gt; elementos na página mestra da página de conteúdo.
ms.author: aspnetcontent
ms.date: 05/21/2008
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: a34b11f5ec8836cfdffc3a08c9cae847232dfcd8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833424"
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Examina as técnicas diferentes para definir sortidas &lt;head&gt; elementos na página mestra da página de conteúdo.


## <a name="introduction"></a>Introdução

Novas páginas mestras criadas no Visual Studio 2008 têm, por padrão, dois controles ContentPlaceHolder: um chamado `head`e está localizado em de `<head>` ; e um elemento denominado `ContentPlaceHolder1`, posicionado dentro do formulário da Web. A finalidade de `ContentPlaceHolder1` é definir uma região de formulário da Web que pode ser personalizado em uma base de página por página. O `head` ContentPlaceHolder permite que as páginas adicionar conteúdo personalizado para o `<head>` seção. (Obviamente, esses dois ContentPlaceHolders pode ser modificados ou removidos e ContentPlaceHolder adicional pode ser adicionado à página mestra. Nossa página mestre, `Site.master`, atualmente, tem quatro controles ContentPlaceHolder.)

O HTML `<head>` elemento serve como um repositório para obter informações sobre o documento de página da web que não faz parte do próprio documento. Isso inclui informações como o título da página da web, informações de metadados usado por mecanismos de pesquisa ou rastreadores internos e links para recursos externos, como RSS feeds, JavaScript e CSS arquivos. Algumas dessas informações podem ser relevantes para todas as páginas no site. Por exemplo, você talvez queira globalmente importar as mesmas regras CSS e arquivos de JavaScript para todas as páginas ASP.NET. No entanto, há partes do `<head>` elemento que são específicas da página. O título da página é um exemplo perfeito.

Neste tutorial, examinamos como definir globais e específicas de página `<head>` marcação da seção na página mestra e em suas páginas de conteúdo.

## <a name="examining-the-master-pagesheadsection"></a>Examinando a página mestra`<head>`seção

O arquivo de página mestra padrão criado pelo Visual Studio 2008 contém a marcação a seguir no seu `<head>` seção:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Observe que o `<head>` elemento contém um `runat="server"` atributo, que indica que ele é um controle de servidor (em vez de HTML estático). Todas as páginas ASP.NET derivam de [ `Page` classe](https://msdn.microsoft.com/library/system.web.ui.page.aspx), que está localizado no `System.Web.UI` namespace. Essa classe contém um [ `Header` propriedade](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) que fornece acesso para a página `<head>` região. Usando o `Header` propriedade podemos definir o título de uma página ASP.NET ou adicionar marcação adicional para o renderizado `<head>` seção. É possível, em seguida, para personalizar uma página de conteúdo `<head>` elemento escrevendo um pouco de código na página de `Page_Load` manipulador de eventos. Vamos examinar como definir programaticamente o título da página na etapa 1.

A marcação mostrada na `<head>` elemento acima também inclui um controle de ContentPlaceHolder chamado `head`. Esse controle ContentPlaceHolder não é necessário, como páginas de conteúdo podem adicionar conteúdo personalizado para o `<head>` elemento por meio de programação. É útil, no entanto, em situações em que precisa de uma página de conteúdo para adicionar marcação estática para o `<head>` elemento como a marcação estática pode ser adicionado declarativamente ao controle de conteúdo correspondente em vez de forma programática.

Além de `<title>` elemento e `head` ContentPlaceHolder, a página mestra `<head>` elemento deve conter nenhum `<head>`-marcação de nível que é comum a todas as páginas. Em nosso site, todas as páginas de usam as regras CSS definidas no `Styles.css` arquivo. Consequentemente, atualizamos o `<head>` elemento na [ *criar um Layout de todo o Site com páginas mestras* ](creating-a-site-wide-layout-using-master-pages-vb.md) tutorial para incluir um correspondente `<link>` elemento. Nossos `Site.master` atual da página mestra `<head>` marcação é mostrada abaixo.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Etapa 1: Definindo o título de página de conteúdo

Título da página da web é especificado por meio de `<title>` elemento. É importante definir o título da cada página para um valor apropriado. Ao visitar uma página, o título é exibido na barra de título do navegador. Além disso, quando uma página de uso de indicadores, os navegadores usam o título da página como o nome sugerido para o indicador. Além disso, muitos mecanismos de pesquisa mostram o título da página ao exibir os resultados da pesquisa.

> [!NOTE]
> Por padrão, o Visual Studio define o `<title>` elemento na página mestra para "Untitled Page". Da mesma forma, as páginas ASP.NET têm seus `<title>` definido como "Untitled Page", também. Como pode ser fácil esquecer de definir o título da página para um valor apropriado, há muitas páginas na Internet com o título "Untitled Page". A pesquisa do Google para páginas da web com esse título retorna aproximadamente 2,460,000 resultados. Até mesmo a Microsoft está suscetível ao publicar páginas da web com o título "Untitled Page". No momento da redação deste artigo, uma pesquisa do Google relatado 236 tais páginas da web no domínio Microsoft.com.


Uma página ASP.NET pode especificar o título de uma das seguintes maneiras:

- Colocando o valor diretamente dentro de `<title>` elemento
- Usando o `Title` de atributo no `<%@ Page %>` diretiva
- Configurando programaticamente a página `Title` propriedade usando código semelhante `Page.Title="title"` ou `Page.Header.Title="title"`.

Conteúdo de páginas não têm um `<title>` elemento, como ele é definido na página mestra. Portanto, para definir o título da página de conteúdo, você pode usar o `<%@ Page %>` da diretiva `Title` de atributo ou defina-o programaticamente.

### <a name="setting-the-pages-title-declaratively"></a>Definir o título da página de forma declarativa

Título da página de conteúdo pode ser definido declarativamente por meio de `Title` atributo do [ `<%@ Page %>` diretiva](https://msdn.microsoft.com/library/ydy4x04a.aspx). Essa propriedade pode ser definida ao modificar diretamente o `<%@ Page %>` diretiva ou na janela Propriedades. Vamos examinar as duas abordagens.

Na exibição da fonte, localize o `<%@ Page %>` diretiva, que é na parte superior da marcação declarativa da página. O `<%@ Page %>` diretiva `Default.aspx` segue:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

O `<%@ Page %>` diretiva especifica atributos específicos da página usados pelo mecanismo ASP.NET ao analisar e compilar a página. Isso inclui seu arquivo de página mestra, o local do seu arquivo de código e seu título, entre outras informações.

Por padrão, ao criar uma nova página de conteúdo, o Visual Studio define o `Title` "Untitled Page" do atributo. Alteração `Default.aspx`do `Title` de atributos de "Untitled Page" para "Tutoriais de página mestra" e, em seguida, exibir a página por meio de um navegador. Figura 1 mostra a barra de título do navegador, que reflete o novo título da página.


![Barra de título do navegador agora mostra &quot;tutoriais de página mestra&quot; em vez de &quot;página sem título&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Figura 01**: barra de título do navegador agora mostra "Tutoriais de página mestra" em vez de "Untitled Page"


O título da página também pode ser definido na janela Propriedades. Na janela Propriedades, selecione o documento na lista suspensa para propriedades de carga de nível de página, que inclui o `Title` propriedade. A Figura 2 mostra a janela de propriedades após `Title` foi definido como "Tutoriais de página mestra".


![Você pode configurar o título da janela Propriedades, também](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Figura 02**: você pode configurar o título da janela Propriedades, também


### <a name="setting-the-pages-title-programmatically"></a>Definir o título da página de forma programática

A página mestra `<head runat="server">` marcação é convertida em um [ `HtmlHead` classe](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) instâncias quando a página é processada pelo mecanismo ASP.NET. O `HtmlHead` classe tem um [ `Title` propriedade](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) cujo valor é refletido no renderizado `<title>` elemento. Esta propriedade é acessível pela classe code-behind de uma página ASP.NET por meio `Page.Header.Title`; essa mesma propriedade também pode ser acessada por meio de `Page.Title`.

Para praticar como o título da página de modo programático, navegue até a `About.aspx` de lógica da página de classe e criar um manipulador de eventos para a página `Load` eventos. Em seguida, defina o título da página "tutoriais de página mestra:: sobre:: *data*", onde *data* é a data atual. Depois de adicionar esse código seus `Page_Load` manipulador de eventos deve ser semelhante ao seguinte:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

A Figura 3 mostra a barra de título do navegador ao visitar o `About.aspx` página.


![O título da página é definida por meio de programação e inclui a data atual](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Figura 03**: título da página a é definido por meio de programação e inclui a data atual


## <a name="step-2-automatically-assigning-a-page-title"></a>Etapa 2: Atribuir automaticamente um título de página

Como vimos na etapa 1, título de uma página pode ser definido de forma declarativa ou programaticamente. Se você esquecer de alterar explicitamente o título para algo mais descritivo, no entanto, a página terá o título padrão, "Untitled Page". O ideal é que o título da página seria definido automaticamente para nós que não especificamos explicitamente seu valor. Por exemplo, se em tempo de execução, o título da página é "Untitled Page", queremos que têm o título atualizado automaticamente para ser o mesmo nome de arquivo da página ASP.NET. A boa notícia é que, com um pouco de trabalho antecipado, é possível ter o título atribuído automaticamente.

Todas as páginas da web ASP.NET derivam o `Page` classe no namespace UI. O `Page` classe define a funcionalidade mínima necessária para uma página ASP.NET e expõe as propriedades essenciais, como `IsPostBack`, `IsValid`, `Request`, e `Response`, entre muitas outras. Muitas vezes, todas as páginas em um aplicativo web requer recursos adicionais ou funcionalidade. Uma maneira comum de fornecer isso é criar uma classe de página de base personalizada. Uma classe de página de base personalizada é criar uma classe que deriva de `Page` de classe e inclui uma funcionalidade adicional. Quando essa classe base tiver sido criada, você pode ter suas páginas ASP.NET derivam dela (em vez de `Page` classe) e, portanto, oferecendo a funcionalidade estendida para suas páginas ASP.NET.

Nesta etapa, criamos uma página de base que define automaticamente o título da página ao nome do arquivo da página ASP.NET se o título do caso contrário, não foi explicitamente definido. Etapa 3 examina definindo o título da página com base no mapa do site.

> [!NOTE]
> Uma análise minuciosa da criação e uso de classes de página de base personalizada está além do escopo desta série de tutoriais. Para obter mais informações, leia [usando uma classe de Base personalizada para Code-Behind Classes suas páginas ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Criando a classe Base da página

Nossa primeira tarefa é criar uma classe de página de base, que é uma classe que estende o `Page` classe. Comece adicionando um `App_Code` pasta ao seu projeto, clicando duas vezes no nome do projeto no Solution Explorer, escolher Adicionar pasta ASP.NET e, em seguida, selecionando `App_Code`. Em seguida, clique duas vezes no `App_Code` pasta e adicione uma nova classe chamada `BasePage.vb`. A Figura 4 mostra o Gerenciador de soluções após a `App_Code` pasta e `BasePage.vb` classe foram adicionados.


![Adicionar uma pasta App_Code e uma classe chamada BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Figura 04**: adicionar um `App_Code` pasta e uma classe chamada `BasePage`


> [!NOTE]
> Visual Studio dá suporte a dois modos de gerenciamento de projeto: os projetos de Site da Web e projetos de aplicativos Web. O `App_Code` pasta foi projetada para ser usado com o modelo de projeto de Site. Se você estiver usando o modelo de projeto de aplicativo Web, coloque o `BasePage.vb` classe em uma pasta chamada algo diferente de `App_Code`, como `Classes`. Para obter mais informações sobre esse tópico, consulte [migrando um projeto de Site da Web a um projeto de aplicativo Web](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).


Como a página de base personalizada serve como a classe base para classes code-behind de páginas do ASP.NET, ele precisa estender o `Page` classe.


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Sempre que uma página ASP.NET é solicitada, ele passará por meio de uma série de estágios, culminando na página solicitada sendo renderizada em HTML. Podemos explorar um estágio, substituindo o `Page` da classe `OnEvent` método. Para nossa base de página Vamos definir automaticamente o título se ele não foi explicitamente especificado pelo `LoadComplete` estágio (que, como você deve ter adivinhado, ocorre após o `Load` estágio).

Para fazer isso, substitua o `OnLoadComplete` método e insira o código a seguir:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

O `OnLoadComplete` método começa determinando se o `Title` propriedade ainda não foi explicitamente definida. Se o `Title` é de propriedade `Nothing`, uma cadeia de caracteres vazia ou tem o valor "Untitled Page", ele é atribuído ao nome do arquivo da página ASP.NET solicitada. O caminho físico para a página ASP.NET solicitada - `C:\MySites\Tutorial03\Login.aspx`, por exemplo, é acessível por meio de `Request.PhysicalPath` propriedade. O `Path.GetFileNameWithoutExtension` método é usado para extrair apenas a parte do nome de arquivo, e esse nome de arquivo é então atribuído ao `Page.Title` propriedade.

> [!NOTE]
> Convido você a aprimorar essa lógica para melhorar o formato do título. Por exemplo, se o nome do arquivo da página é `Company-Products.aspx`, o código acima produzirá o título "Produtos da empresa", mas o ideal é que o traço deve ser substituído com um espaço, como em "Produtos da empresa". Além disso, considere a adição de um espaço sempre que houver uma alteração. Ou seja, considere a adição de código que transforma o nome do arquivo `OurBusinessHours.aspx` para um título de "Our horário comercial".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Ter as páginas de conteúdo que herdam a classe Base da página

Agora, precisamos atualizar as páginas do ASP.NET em nosso site para derivar a página de base personalizada (`BasePage`) em vez do `Page` classe. Para fazer isso, vá para cada classe code-behind e altere a declaração de classe de:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

Para:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Depois de fazer isso, visite o site por meio de um navegador. Se você visitar uma página com o título é definido explicitamente, como `Default.aspx` ou `About.aspx`, o título especificado explicitamente é usado. Se, no entanto, você visitar uma página com o título não foi alterado do padrão ("Untitled Page"), a classe de página de base define o título para o nome do arquivo da página.

A Figura 5 mostra o `MultipleContentPlaceHolders.aspx` página quando visualizado por meio de um navegador. Observe que o título é precisamente nome do arquivo da página (menos a extensão), "MultipleContentPlaceHolders".


[![Se um título não é especificado explicitamente, o nome do arquivo da página é usado automaticamente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Figura 05**: se um título não é especificado explicitamente, o nome do arquivo da página é usado automaticamente ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Etapa 3: Baseando o título da página no mapa do Site

O ASP.NET oferece uma estrutura de mapa de site robusta que permite que os desenvolvedores de páginas definir um mapa do site hierárquico em um recurso externo (como uma tabela de banco de dados ou arquivo XML), juntamente com os controles da Web para exibir informações sobre o mapa do site (como SiteMapPath, Menu e controles TreeView).

A estrutura do mapa de site também pode ser acessada por meio de programação da classe code-behind de uma página ASP.NET. Dessa maneira é automaticamente pode definir o título de uma página para o título do seu nó correspondente no mapa do site. Vamos melhorar a `BasePage` classe criada na etapa 2 para que ele oferece essa funcionalidade. Mas, primeiro precisamos criar um mapa de site para o nosso site.

> [!NOTE]
> Este tutorial presume que o leitor já esteja familiarizado com o ASP. Recursos de mapa de site da rede. Para obter mais informações sobre como usar o mapa do site, consulte minha série de artigos em várias partes, [examinando ASP. Navegação no Site da rede](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Criando o mapa do Site

O sistema de mapa de site é construído sobre a [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que separa o mapa do site API da lógica que serializa informações do mapa de site entre a memória e um armazenamento persistente. O .NET Framework vem com o [ `XmlSiteMapProvider` classe](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), que é o provedor de mapa de site padrão. Como o nome sugere, `XmlSiteMapProvider` usa um arquivo XML como seu repositório de mapa de site. Vamos usar esse provedor para definir o mapa de site.

Comece criando um arquivo de mapa de site na pasta raiz do site do chamado `Web.sitemap`. Para fazer isso, clique com botão direito no nome do site no Gerenciador de soluções, escolha Add New Item e selecione o modelo do mapa do Site. Certifique-se de que o arquivo é nomeado `Web.sitemap` e clique em Adicionar.


[![Adicionar um arquivo denominado Web. sitemap à pasta da raiz do site](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Figura 06**: adicionar um arquivo nomeado `Web.sitemap` para a pasta da raiz do site ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


Adicione o seguinte XML para o `Web.sitemap` arquivo:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Esse XML define a estrutura de mapa do site hierárquico mostrada na Figura 7.


![O mapa do Site é atualmente composto de três nós mapa de Site](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Figura 07**: O mapa do Site é atualmente composto de três nós mapa de Site


Atualizaremos a estrutura do mapa de site em tutoriais futuros à medida que adicionamos novos exemplos.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Atualizando a página mestra para incluir controles de navegação de Web

Agora que temos um mapa do site definido, vamos atualizar a página mestra para incluir controles de navegação de Web. Especificamente, vamos adicionar um controle ListView para a coluna esquerda na seção de lições que renderiza uma lista não ordenada com um item de lista para cada nó definida no mapa do site.

> [!NOTE]
> O controle ListView é novo para o ASP.NET versão 3.5. Se você estiver usando uma versão anterior do ASP.NET, use o controle Repeater. Para obter mais informações sobre o controle ListView, consulte [usando o ASP.NET 3.5 ListView e DataPager controles](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Inicie removendo a marcação existente da lista não ordenada da seção de lições. Em seguida, arraste um controle ListView da caixa de ferramentas e solte-o sob as lições título. O ListView está localizado na seção de dados da caixa de ferramentas, juntamente com os outros controles de exibição: o GridView, DetailsView e FormView. Definir o ListView `ID` propriedade para `LessonsList`.

No Assistente de configuração de fonte de dados, escolha para associar o ListView a um novo controle SiteMapDataSource chamado `LessonsDataSource`. O controle SiteMapDataSource retorna a estrutura hierárquica de mapa do sistema de site.


[![Associar um controle SiteMapDataSource ao controle ListView LessonsList](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Figura 08**: associar um controle SiteMapDataSource ao controle ListView LessonsList ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


Depois de criar o controle SiteMapDataSource, precisamos definir modelos do ListView para que ele renderize uma lista não ordenada com um item de lista para cada nó retornado pelo controle SiteMapDataSource. Isso pode ser feito usando a seguinte marcação de modelo:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

O `LayoutTemplate` gera a marcação para uma lista não ordenada (`<ul>...</ul>`) enquanto o `ItemTemplate` renderiza cada item retornado por SiteMapDataSource como um item de lista (`<li>`) que contém um link para a lição específica.

Depois de configurar os modelos do ListView, visite o site. Como mostra a Figura 9, a seção de lições contém um único item com marcadores, Home. Onde estão About e usando lições ContentPlaceHolder vários controles? SiteMapDataSource foi projetado para retornar um conjunto hierárquico de dados, mas o controle ListView só pode exibir um único nível da hierarquia. Consequentemente, somente o primeiro nível de nós de mapa de site retornados por SiteMapDataSource é exibido.


[![A seção de lições contém um único Item de lista](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Figura 09**: A seção de lições contém um único Item de lista ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


Para exibir vários níveis, poderá aninhar várias ListViews dentro a `ItemTemplate`. Essa técnica foi examinada na [ *páginas mestras e navegação no Site* tutorial](../../data-access/introduction/master-pages-and-site-navigation-vb.md) do meu [trabalhando com a série de tutoriais de dados](../../data-access/index.md). No entanto, para esta série de tutoriais nosso mapa do site conterá apenas um dois níveis: página inicial (o nível superior); e cada lição como um filho da página inicial. Em vez de criar um ListView aninhado, em vez disso, podemos pode instruir o SiteMapDataSource não retornar o nó inicial, definindo sua [ `ShowStartingNode` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) para `False`. O efeito líquido é que o SiteMapDataSource começa retornando a segunda camada de nós de mapa de site.

Com essa alteração, o ListView exibe itens com marcadores do About e usando vários controles ContentPlaceHolder lições, mas omite um item de marcador para a página inicial. Para corrigir isso, podemos explicitamente adicionar um item de marcador para casa no `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Configurando a SiteMapDataSource para omitir o nó inicial e adicionar explicitamente um item com marcador inicial, a seção de lições agora exibe a saída desejada.


[![A seção de lições contém um Item de marcador para uso doméstico e cada nó filho](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Figura 10**: A seção de lições contém um Item de marcador para uso doméstico e cada nó filho ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Definindo o título com base no mapa do Site

Com o mapa do site em vigor, podemos atualizar nosso `BasePage` classe para usar o título especificado no mapa do site. Como fizemos na etapa 2, só queremos usar o título do nó de mapa de site se o título da página não tiver sido definido explicitamente pelo desenvolvedor da página. Se a página que está sendo solicitada não tiver explicitamente definido título da página e não foi encontrada no mapa do site, em seguida, nós retornaremos usando o nome do arquivo da página solicitada (menos a extensão), como fizemos na etapa 2. Figura 11 ilustra esse processo de decisão.


![Na ausência de um explicitamente definir título da página, o título do nó das mapa de Site correspondente é usado](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Figura 11**: na ausência de um explicitamente definir título da página, o título do nó das mapa de Site correspondente é usado


Atualizar o `BasePage` da classe `OnLoadComplete` método para incluir o código a seguir:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Como antes, o `OnLoadComplete` método começa determinando se o título da página tiver sido definido explicitamente. Se `Page.Title` está `Nothing`, uma cadeia de caracteres vazia ou é atribuído o valor "Untitled Page", em seguida, o código atribui automaticamente um valor para `Page.Title`.

Para determinar o título a ser usado, o código começa fazendo referência a [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx)do [ `CurrentNode` propriedade](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode` Retorna o [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) instância no mapa do site que corresponde à página solicitada no momento. Supondo que a página atualmente solicitada for encontrado no mapa do site, o `SiteMapNode`do `Title` propriedade é atribuída para o título da página. Se a página atualmente solicitada não está no mapa do site, `CurrentNode` retorna `Nothing` e nome de arquivo da página solicitada é usado como o título (como foi feito na etapa 2).

A Figura 12 mostra o `MultipleContentPlaceHolders.aspx` página quando visualizado por meio de um navegador. Porque o título da página, esse não for definido explicitamente, o título do seu correspondente nó mapa de site é usado em vez disso.


![Título da página MultipleContentPlaceHolders.aspx é extraído do mapa do Site](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Figura 12**: título da página MultipleContentPlaceHolders.aspx o retirados do mapa do Site


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Etapa 4: Adicionando outra marcação específico da página para o`<head>`seção

As etapas 1, 2 e 3 examinou Personalizando o `<title>` elemento em uma base de página por página. Além `<title>`, o `<head>` seção pode conter `<meta>` elementos e `<link>` elementos. Conforme mencionado anteriormente neste tutorial `Site.master`do `<head>` seção inclui uma `<link>` elemento `Styles.css`. Porque isso `<link>` elemento é definido dentro da página mestra, ele está incluído no `<head>` seção para todas as páginas de conteúdo. Mas como nós vamos adicionando `<meta>` e `<link>` elementos em uma base de página por página?

A maneira mais fácil para adicionar conteúdo específico da página para o `<head>` seção é criar um controle ContentPlaceHolder na página mestra. Já temos tal um ContentPlaceHolder (chamado `head`). Portanto, para adicionar personalizado `<head>` marcação, criar uma correspondente do controle na página de conteúdo e coloque a marcação de lá.

Para ilustrar personalizado adicionando `<head>` marcação para uma página, vamos incluir um `<meta>` elemento de descrição para o nosso conjunto atual de páginas de conteúdo. O `<meta>` elemento descrição fornece uma breve descrição sobre a página da web; a maioria dos mecanismos de pesquisa incorporam essas informações de alguma forma, ao exibir os resultados da pesquisa.

Um `<meta>` elemento descrição tem a seguinte forma:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Para adicionar essa marcação para uma página de conteúdo, adicione o texto acima para o controle de conteúdo que é mapeado para a página mestra `head` ContentPlaceHolder. Por exemplo, para definir um `<meta>` elemento de descrição para `Default.aspx`, adicione a seguinte marcação:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Porque o `head` ContentPlaceHolder não está dentro do corpo da página HTML, a marcação adicionada ao controle de conteúdo não é exibida no modo de exibição de Design. Para ver os `<meta>` visita de elemento de descrição `Default.aspx` através de um navegador. Depois que a página foi carregada, exibir o código-fonte e observe que o `<head>` seção inclui a marcação especificada no controle de conteúdo.

Reserve um tempo para adicionar `<meta>` elementos descrição `About.aspx`, `MultipleContentPlaceHolders.aspx`, e `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Adicionando programaticamente a marcação para o`<head>`região

O `head` ContentPlaceHolder nos permite adicionar declarativamente marcação personalizada para a página mestra `<head>` região. Marcação personalizadas também pode ser adicionada programaticamente. Lembre-se de que o `Page` da classe `Header` propriedade retorna o `HtmlHead` instância definida na página mestra (o `<head runat="server">`).

Ser capaz de adicionar programaticamente o conteúdo para o `<head>` região é útil quando o conteúdo a adicionar é dinâmico. Talvez ele se baseia o usuário visitar a página; Talvez ele está sendo retirado de um banco de dados. Independentemente do motivo, você pode adicionar conteúdo para o `HtmlHead` adicionando controles ao seu `Controls` coleção da seguinte forma:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

O código acima adiciona o `<meta>` elemento de palavras-chave para o `<head>` região, que fornece uma lista delimitada por vírgulas de palavras-chave que descrevem a página. Observe que para adicionar um `<meta>` marca que você cria um [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) da instância, defina seu `Name` e `Content` propriedades e, em seguida, adicioná-lo para o `Header`do `Controls` coleção. Da mesma forma, adicionar programaticamente uma `<link>` elemento, crie um [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) do objeto, defina suas propriedades e, em seguida, adicioná-lo para o `Header`do `Controls` coleção.

> [!NOTE]
> Para adicionar marcação arbitrária, crie uma [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) da instância, defina seu `Text` propriedade e, em seguida, adicioná-lo para o `Header`do `Controls` coleção.


## <a name="summary"></a>Resumo

Neste tutorial vimos uma variedade de maneiras de adicionar `<head>` marcação de região em uma base de página por página. Uma página mestra deve incluir um `HtmlHead` instância (`<head runat="server">`) com um ContentPlaceHolder. O `HtmlHead` instância permite que as páginas de conteúdo para acessar programaticamente o `<head>` região e declarativamente e de forma programática, defina a página do título; o controle de ContentPlaceHolder permite que a marcação personalizada a ser adicionado ao `<head>` seção declarativamente por meio de um controle de conteúdo.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Definir dinamicamente o título da página no ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Examinando o ASP. Navegação no Site da rede](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Como usar marcas HTML Meta](http://searchenginewatch.com/showPage.html?page=2167931)
- [Páginas mestras no ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Usando o ASP.NET do 3.5 ListView e DataPager controles](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Usando uma classe Base personalizada para as Classes Code-Behind de suas páginas ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 3.5 in 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Zack Jones e Suchi Banerjee. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](multiple-contentplaceholders-and-default-content-vb.md)
> [Próximo](urls-in-master-pages-vb.md)
