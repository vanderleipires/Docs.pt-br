---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: Criação de um provedor de mapa de Site personalizado controlado por banco de dados (c#) | Microsoft Docs
author: rick-anderson
description: O provedor de mapa de site padrão no ASP.NET 2.0 recupera seus dados de um arquivo XML estático. Enquanto o provedor baseado em XML é adequado para muitos pequeno e médio Taman...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: 1160711911d53dcf889ced1a8422c2dfcf72b240
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368690"
---
<a name="building-a-custom-database-driven-site-map-provider-c"></a>Criação de um provedor de mapa de Site personalizado controlado por banco de dados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip) ou [baixar PDF](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> O provedor de mapa de site padrão no ASP.NET 2.0 recupera seus dados de um arquivo XML estático. Enquanto o provedor baseado em XML é adequado para muitos sites da Web de pequeno e médio porte, aplicativos da Web maiores exigem um mapa do site mais dinâmico. Neste tutorial, criaremos um provedor de mapa de site personalizado que recupera seus dados da camada de lógica de negócios, que por sua vez recupera dados do banco de dados.


## <a name="introduction"></a>Introdução

O ASP.NET 2.0 recurso de mapa de site de s permite que um desenvolvedor de página Definir um mapa do site da web aplicativo s em algum meio persistente, como em um arquivo XML. Depois de definido, os dados de mapa de site podem ser acessados por meio de programação com o [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx) no [ `System.Web` namespace](https://msdn.microsoft.com/library/system.web.aspx) ou por meio de uma variedade de navegação Web controles, como o Controles SiteMapPath, Menu e TreeView. O sistema de mapa de site usa o [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) para que as implementações de serialização de mapa de site diferente podem ser criadas e conectadas a um aplicativo web. O provedor de mapa de site padrão que é fornecido com o ASP.NET 2.0 mantém uma estrutura de mapa de site em um arquivo XML. Volta a [páginas mestras e navegação no Site](../introduction/master-pages-and-site-navigation-cs.md) tutorial, criamos um arquivo chamado `Web.sitemap` que contido nessa estrutura e ter sido atualizando seu XML com cada nova seção do tutorial.

O provedor de mapa de site baseado em XML padrão funciona bem se a estrutura de s de mapa de site é razoavelmente estática, como para esses tutoriais. Em muitos cenários, no entanto, é necessário um mapa do site mais dinâmico. Considere o mapa do site mostrado na Figura 1, onde cada categoria e produto aparecem como seções na estrutura do site. Com esse mapa de site, visitando a página da web correspondente para o nó raiz pode listar todas as categorias, ao passo que visitar uma página da web de determinada categoria s listaria os produtos dessa categoria e exibir uma página da web de determinado produto s mostraria produto detalhes s.


[![As categorias e produtos de composição a estrutura de s de mapa de Site](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**Figura 1**: as categorias e produtos maquiagem o s estrutura do mapa do Site ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))


Embora essa estrutura baseado em categoria e produto poderia ser embutidos em código no `Web.sitemap` arquivo, o arquivo precisa ser atualizada sempre que uma categoria ou produto foi adicionado, removido ou renomeado. Consequentemente, a manutenção de mapa de site seria bastante simplificada se sua estrutura foi recuperada do banco de dados ou, de modo ideal, da camada de lógica de negócios da arquitetura do aplicativo de s. Dessa forma, como categorias e produtos foram adicionadas, renomeados ou excluídos, o mapa do site atualizará automaticamente para refletir essas alterações.

Uma vez que a serialização de mapa de site do ASP.NET 2.0 s é construída sobre o modelo de provedor, podemos criar nosso próprio provedor de mapa de site personalizado que obtém seus dados de um repositório de dados alternativos, como o banco de dados ou a arquitetura. Neste tutorial, criaremos um provedor personalizado que recupera os dados da BLL. Permitir que o s começar!

> [!NOTE]
> O provedor de mapa de site personalizada criado neste tutorial está acoplado ao modelo de aplicativo s arquitetura e os dados. S de Jeff Prosise [armazenando mapas de Site no SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) e [o provedor de mapa de Site do SQL ve você estava esperando](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) artigos examinar uma abordagem generalizada para armazenar dados de mapa de site no SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Etapa 1: Criar as páginas da Web de provedor de mapa de Site personalizada

Antes de começarmos a criação de um provedor de mapa de site personalizado, permitem que s primeiro adicione as páginas do ASP.NET, precisaremos para este tutorial. Comece adicionando uma nova pasta chamada `SiteMapProvider`. Em seguida, adicione as seguintes páginas do ASP.NET para essa pasta, certificando-se de associar cada página com o `Site.master` página mestra:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Adicione também uma `CustomProviders` subpasta para a `App_Code` pasta.


![Adicione as páginas do ASP.NET para que os tutoriais de relacionadas ao provedor de mapa de Site](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**Figura 2**: adicionar as páginas do ASP.NET para o Site de tutoriais relacionados ao provedor do mapa


Como há apenas um tutorial para essa seção, podemos don precisa `Default.aspx` para listar os tutoriais de seção s. Em vez disso, `Default.aspx` exibirá as categorias em um controle GridView. Que abordaremos isso na etapa 2.

Em seguida, atualize `Web.sitemap` incluir uma referência para o `Default.aspx` página. Especificamente, adicione a marcação a seguir após o Caching `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui um item para o tutorial do provedor de mapa do site único.


![O mapa do Site agora inclui uma entrada para o Tutorial de provedor de mapa de Site](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**Figura 3**: O mapa do Site agora inclui uma entrada para o Tutorial de provedor de mapa de Site


Este tutorial principal foco é ilustrar a criar um provedor de mapa de site personalizado e configurar um aplicativo web para usar esse provedor. Em particular, vamos criar um provedor que retorna um mapa de site que inclui um nó raiz, juntamente com um nó para cada categoria e produto, conforme ilustrado na Figura 1. Em geral, cada nó no mapa do site pode especificar uma URL. Para nosso mapa do site, a URL raiz do nó s será `~/SiteMapProvider/Default.aspx`, que listará todas as categorias no banco de dados. Cada nó de categoria no mapa do site terá uma URL que aponta para `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, que lista todos os produtos especificado *categoryID*. Por fim, cada nó de mapa de site do produto apontará para `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, que exibirá os detalhes do produto específica s.

Para começar, precisamos criar a `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` páginas. Essas páginas são concluídas nas etapas 2, 3 e 4, respectivamente. Como o foco deste tutorial é provedores de mapa de site, e como últimos tutoriais abordei a criação de relatórios esses tipos de mestre/detalhes com várias páginas, podemos será corra por meio de etapas de 2 a 4. Se você precisar de um lembrete sobre a criação de relatórios mestre/detalhes que abrangem várias páginas, voltar para o [filtragem de mestre/detalhes em duas páginas](../masterdetail/master-detail-filtering-across-two-pages-cs.md) tutorial.

## <a name="step-2-displaying-a-list-of-categories"></a>Etapa 2: Exibindo uma lista de categorias

Abrir o `Default.aspx` página o `SiteMapProvider` pasta e arraste um controle GridView da caixa de ferramentas para o Designer, definindo seu `ID` para `Categories`. Da GridView s marca inteligente, associá-lo a um novo ObjectDataSource denominado `CategoriesDataSource` e configure-o para que ele recupere seus dados usando o `CategoriesBLL` classe s `GetCategories` método. Como esse GridView apenas exibe as categorias e não fornece recursos de modificação de dados, defina as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Configurar o ObjectDataSource para retornar as categorias usando o método GetCategories](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**Figura 4**: configurar o ObjectDataSource para retornar categorias usando o `GetCategories` método ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))


[![Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**Figura 5**: defina a lista suspensa no UPDATE, INSERT e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio adicionará um BoundField para `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, e `BrochurePath`. Editar o GridView para que ele contém apenas o `CategoryName` e `Description` BoundFields e atualizar o `CategoryName` BoundField s `HeaderText` propriedade à categoria.

Em seguida, adicione um HyperLinkField e posicioná-lo assim que ele s o campo mais à esquerda. Definir a `DataNavigateUrlFields` propriedade para `CategoryID` e o `DataNavigateUrlFormatString` propriedade `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Defina o `Text` propriedade para exibir produtos.


![Adicionar um HyperLinkField a GridView categorias](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**Figura 6**: adicionar um HyperLinkField para o `Categories` GridView


Depois de criar o ObjectDataSource e personalizar os campos de s GridView, a marcação declarativa de dois controles ficará semelhante ao seguinte:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

A Figura 7 mostra `Default.aspx` quando visualizado por meio de um navegador. Clicar em uma categoria s exibir produtos link leva você à `ProductsByCategory.aspx?CategoryID=categoryID`, que vamos criar na etapa 3.


[![Cada categoria é listada junto com um Link de produtos do modo de exibição](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**Figura 7**: cada categoria é listada junto com um Link de produtos do modo de exibição ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Etapa 3: Listando os produtos a categoria selecionada

Abra o `ProductsByCategory.aspx` da página e adicionar um GridView, nomeando- `ProductsByCategory`. Na marca inteligente, de associar o GridView para um novo ObjectDataSource chamado `ProductsByCategoryDataSource`. Configurar o ObjectDataSource para usar o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método e o conjunto na lista suspensa lista como (nenhum) nas guias UPDATE, INSERT e DELETE.


[![Use o método de GetProductsByCategoryID(categoryID) ProductsBLL classe s](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**Figura 8**: Use o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))


Solicita que a etapa final no Assistente Configurar fonte de dados para uma fonte de parâmetro para *categoryID*. Uma vez que essas informações são passadas por meio do campo querystring `CategoryID`, selecione QueryString na lista suspensa e insira CategoryID na caixa de texto QueryStringField, conforme mostrado na Figura 9. Clique em Concluir para concluir o assistente.


[![Use o campo de Querystring CategoryID para o parâmetro categoryID](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**Figura 9**: Use o `CategoryID` Querystring Field para o *categoryID* parâmetro ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))


Depois de concluir o assistente, o Visual Studio adicionará BoundFields correspondente e um CheckBoxField a GridView para os campos de dados do produto. Remover tudo, exceto os `ProductName`, `UnitPrice`, e `SupplierName` BoundFields. Personalizar esses três BoundFields `HeaderText` propriedades para ler o produto, preço e fornecedor, respectivamente. Formato de `UnitPrice` BoundField como uma moeda.

Em seguida, adicione um HyperLinkField e mova-o para a posição mais à esquerda. Definir seus `Text` propriedade para exibir detalhes, seu `DataNavigateUrlFields` propriedade a ser `ProductID`e sua `DataNavigateUrlFormatString` propriedade para `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Adicionar um HyperLinkField de detalhes do modo de exibição que aponta para ProductDetails.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**Figura 10**: adicionar um modo de exibição de detalhes HyperLinkField que aponta para `ProductDetails.aspx`


Depois de fazer essas personalizações, GridView e ObjectDataSource s marcação declarativa deve se parecer com o seguinte:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

Retornar à exibição `Default.aspx` por meio de um navegador e clique em Exibir produtos vincular para bebidas. Isso levará você para `ProductsByCategory.aspx?CategoryID=1`, exibindo os nomes, os preços e os fornecedores dos produtos no banco de dados Northwind que pertencem à categoria de bebidas (veja a Figura 11). Fique à vontade aprimorar ainda mais essa página para incluir um link para retornar os usuários para a página de listagem de categoria (`Default.aspx`) e um controle DetailsView ou FormView que exibe o nome da categoria selecionada s e a descrição.


[![Os nomes de bebidas, preços e fornecedores são exibidos](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**Figura 11**: os nomes de bebidas, preços e fornecedores são exibidos ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Etapa 4: Mostrando uma s os detalhes do produto

A página final, `ProductDetails.aspx`, exibe os detalhes de produtos selecionados. Abra `ProductDetails.aspx` e arraste um DetailsView da caixa de ferramentas para o Designer. Definir o s DetailsView `ID` propriedade para `ProductInfo` e limpe seu `Height` e `Width` valores de propriedade. Na marca inteligente, de associar DetailsView para um novo ObjectDataSource denominado `ProductDataSource`, configurando o ObjectDataSource para efetuar pull de seus dados a partir de `ProductsBLL` classe s `GetProductByProductID(productID)` método. Assim como acontece com as páginas da web anterior criado nas etapas 2 e 3, defina as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Configurar o ObjectDataSource para usar o método GetProductByProductID(productID)](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**Figura 12**: configurar o ObjectDataSource para usar o `GetProductByProductID(productID)` método ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))


Solicita que a última etapa do Assistente Configurar fonte de dados para a fonte do *productID* parâmetro. Uma vez que esses dados estão divididos por meio do campo querystring `ProductID`, defina a lista suspensa para QueryString e textbox QueryStringField ProductID. Por fim, clique no botão Concluir para concluir o assistente.


[![Configurar o parâmetro para efetuar Pull de seu valor do campo ProductID Querystring productID](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**Figura 13**: configurar o *productID* parâmetro para efetuar Pull de seu valor a partir de `ProductID` Querystring Field ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio criará BoundFields correspondente e um CheckBoxField em DetailsView para os campos de dados do produto. Remover o `ProductID`, `SupplierID`, e `CategoryID` BoundFields e configurar os campos restantes conforme necessário. Depois de um punhado de configurações estéticas, minha marcação declarativa de s do DetailsView e ObjectDataSource a aparência a seguir:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

Para testar essa página, retorne ao `Default.aspx` e clique em Exibir produtos para a categoria de bebidas. Na listagem de produtos de bebidas, clique no link Exibir detalhes de chá Chai. Isso levará você para `ProductDetails.aspx?ProductID=1`, que mostra um s Chai chá detalhes (veja a Figura 14).


[![Chai chá s fornecedor, categoria, preço e outras informações é exibida](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**Figura 14**: Chai chá s fornecedor, categoria, preço e outras informações é exibida ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Etapa 5: Noções básicas sobre o funcionamento interno de um provedor de mapa de Site

O mapa do site é representado na memória do servidor s web como uma coleção de `SiteMapNode` instâncias que formam uma hierarquia. Deve haver exatamente uma raiz, todos os nós de não-raiz devem ter exatamente um nó pai e todos os nós podem ter um número arbitrário de filhos. Cada `SiteMapNode` objeto representa uma seção na estrutura de site s; essas seções geralmente têm uma página da web correspondente. Consequentemente, o [ `SiteMapNode` classe](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) tem propriedades como `Title`, `Url`, e `Description`, que fornecem informações para a seção de `SiteMapNode` representa. Há também uma `Key` propriedade que identifica exclusivamente cada `SiteMapNode` na hierarquia, bem como as propriedades usadas para estabelecer essa hierarquia `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`e assim por diante.

Figura 15 mostra a estrutura do mapa de site geral da Figura 1, mas com os detalhes de implementação descritos detalhadamente.


[![Cada SiteMapNode tem propriedades, como título, Url, chave e assim por diante](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**Figura 15**: cada `SiteMapNode` tem propriedades como `Title`, `Url`, `Key`e assim por diante ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))


O mapa do site é acessível por meio de [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx) no [ `System.Web` namespace](https://msdn.microsoft.com/library/system.web.aspx). Essa classe s `RootNode` propriedade retorna a raiz do mapa s site `SiteMapNode` instância; `CurrentNode` retorna o `SiteMapNode` cuja `Url` propriedade corresponde à URL da página solicitada no momento. Essa classe é usada internamente por controles da Web do ASP.NET 2.0 s navegação.

Quando o `SiteMap` propriedades da classe s são acessadas, ele deve serializar a estrutura do mapa do site de alguma mídia persistente na memória. No entanto, a lógica de serialização de mapa de site não é difícil codificado no `SiteMap` classe. Em vez disso, em tempo de execução de `SiteMap` classe determina qual mapa do site *provedor* a ser usado para serialização. Por padrão, o [ `XmlSiteMapProvider` classe](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) for usado, que lê a estrutura de s de mapa do site de um arquivo XML formatado corretamente. No entanto, com um pouco de trabalho podemos criar nosso próprio provedor de mapa de site personalizadas.

Todos os provedores de mapa de site devem ser derivados de [ `SiteMapProvider` classe](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), que inclui os métodos essenciais e as propriedades necessárias para o site mapear provedores, mas omite muitos dos detalhes de implementação. Uma segunda classe, [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), estende o `SiteMapProvider` de classe e contém uma implementação mais robusta da funcionalidade necessária. Internamente, o `StaticSiteMapProvider` armazena o `SiteMapNode` instâncias do site do mapa em uma `Hashtable` e fornece métodos como `AddNode(child, parent)`, `RemoveNode(siteMapNode),` e `Clear()` que adicionar e remover `SiteMapNode` s para o interno `Hashtable`. `XmlSiteMapProvider` é derivado de `StaticSiteMapProvider`.

Quando a criação de um provedor de mapa de site personalizado que estende `StaticSiteMapProvider`, há dois métodos abstratos que devem ser substituídos: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) e [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, como o nome sugere, é responsável por carregar a estrutura do mapa de site do armazenamento persistente e construí-la na memória. `GetRootNodeCore` Retorna o nó raiz no mapa do site.

Antes de uma web de aplicativo pode usar um provedor de mapa de site que ele deve ser registrado na configuração do aplicativo de s. Por padrão, o `XmlSiteMapProvider` classe está registrado usando o nome `AspNetXmlSiteMapProvider`. Para registrar os provedores de mapa de site adicionais, adicione a seguinte marcação ao `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

O *nome* valor atribui um nome legível para o provedor ao *tipo* Especifica o nome de tipo totalmente qualificado do provedor de mapa de site. Vamos explorar valores concretos para o *nome* e *tipo* valores na etapa 7, depois podemos ve criado nosso provedor de mapa de site personalizadas.

A classe de provedor de mapa de site é instanciada na primeira vez que ele é acessado a partir de `SiteMap` classe e permanece na memória para o tempo de vida do aplicativo web. Como há apenas uma instância do provedor de mapa de site que pode ser chamado várias, simultâneas de visitantes do site, é imperativo que os métodos do provedor s seja *thread-safe*.

Por motivos de escalabilidade, de desempenho e ele importante que o site na memória em cache mapeie a estrutura e retornar isso armazenado em cache a estrutura em vez de recriá-la sempre que o `BuildSiteMap` método é invocado. `BuildSiteMap` pode ser chamado várias vezes por solicitação de página por usuário, dependendo dos controles de navegação em uso na página e a profundidade da estrutura de mapa do site. Em qualquer caso, se não armazenamos em cache a estrutura de mapa de site no `BuildSiteMap` e em seguida, cada vez que ele é invocado seria preciso recuperar novamente as informações de produto e categoria da arquitetura (o que resultaria em uma consulta ao banco de dados). Como discutimos no cache tutoriais anteriores, dados armazenados em cache podem se tornar obsoletos. Para combater isso, podemos usar Temp ou expirações de dependência com base no cache SQL.

> [!NOTE]
> Um provedor de mapa de site pode, opcionalmente, substituir os [ `Initialize` método](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` é invocado quando o provedor de mapa de site é instanciado pela primeira vez e é passado a todos os atributos personalizados atribuídos ao provedor na `Web.config` no `<add>` elemento como: `<add name="name" type="type" customAttribute="value" />`. É útil se você quiser permitir que um desenvolvedor de página especificar várias configurações de relacionadas ao provedor do site mapa sem ter que modificar o código do provedor de s. Por exemplo, se estamos foram lendo os dados de categoria e produtos diretamente do banco de dados em vez de por meio da arquitetura, podemos d provavelmente deseja permitir que o desenvolvedor da página especificar a cadeia de caracteres de conexão de banco de dados por meio de `Web.config` em vez de usar um embutido valor no código do provedor s. O provedor de mapa de site personalizadas que criaremos na etapa 6 não substituí-la `Initialize` método. Para obter um exemplo de como usar o `Initialize` método, consulte [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [armazenando mapas de Site no SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) artigo.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Etapa 6: Criando o provedor de mapa de Site personalizada

Para criar um provedor de mapa de site personalizado que cria o mapa do site das categorias e produtos no banco de dados Northwind, precisamos criar uma classe que estende `StaticSiteMapProvider`. Na etapa 1 perguntei a adição de um `CustomProviders` pasta na `App_Code` pasta - adicionar uma nova classe para essa pasta chamada `NorthwindSiteMapProvider`. Adicione o seguinte código à classe `NorthwindSiteMapProvider`:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

Permitir que o s começará explorando essa classe s `BuildSiteMap` método, que começa com um [ `lock` instrução](https://msdn.microsoft.com/library/c5kehkcz.aspx). O `lock` instrução permite apenas um thread por vez para inserir, serializar o acesso ao seu código, assim e impedindo que dois threads simultâneos passo a passo em uma outra s dedos.

O nível de classe `SiteMapNode` variável `root` é usado para armazenar em cache a estrutura do mapa de site. Quando o mapa do site é construído pela primeira vez, ou pela primeira vez depois que os dados subjacentes tem sido modificados, `root` será `null` e a estrutura do mapa de site será criada. O nó raiz do site mapa s é atribuído a `root` durante a construção de processo para que na próxima vez que esse método é chamado, `root` não será `null`. Consequentemente, enquanto `root` não é `null` a estrutura do mapa de site será retornada ao chamador sem ter que recriá-la.

Se for raiz `null`, a estrutura do mapa de site é criada a partir de informações de produto e categoria. O mapa do site é criado, criando os `SiteMapNode` instâncias e, em seguida, formando a hierarquia por meio de chamadas para o `StaticSiteMapProvider` classe s `AddNode` método. `AddNode` executa o sistema de controle interno, armazenando o variados `SiteMapNode` instâncias em um `Hashtable`. Antes de começar a construir a hierarquia, começamos chamando o `Clear` método, que limpa os elementos de interno `Hashtable`. Em seguida, o `ProductsBLL` classe s `GetProducts` método e resultante `ProductsDataTable` são armazenados em variáveis locais.

A construção de mapa s site começa criando o nó raiz e atribuí-lo à `root`. A sobrecarga da [ `SiteMapNode` construtor s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) usado aqui e em todo este `BuildSiteMap` é passado as seguintes informações:

- Uma referência para o provedor de mapa de site (`this`).
- O `SiteMapNode` s `Key`. Isso exigia o valor deve ser exclusivo para cada `SiteMapNode`.
- O `SiteMapNode` s `Url`. `Url` é opcional, mas, se fornecido, cada `SiteMapNode` s `Url` valor deve ser exclusivo.
- O `SiteMapNode` s `Title`, que é necessário.

O `AddNode(root)` chamada de método adiciona o `SiteMapNode` `root` ao mapa de site como a raiz. Em seguida, cada `ProductRow` no `ProductsDataTable` é enumerada. Se já existe um `SiteMapNode` para a categoria de produto s atual, ele é referenciado. Caso contrário, uma nova `SiteMapNode` para a categoria é criada e adicionada como um filho do `SiteMapNode``root` por meio de `AddNode(categoryNode, root)` chamada de método. Após a categoria apropriada `SiteMapNode` nó foi encontrado ou criado, um `SiteMapNode` é criado para o produto atual e adicionado como um filho da categoria `SiteMapNode` por meio de `AddNode(productNode, categoryNode)`. Observe que a categoria `SiteMapNode` s `Url` é o valor da propriedade `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` enquanto o produto `SiteMapNode` s `Url` atribuído da propriedade `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Produtos que têm um banco de dados `NULL` valor para seus `CategoryID` são agrupadas em uma categoria `SiteMapNode` cujo `Title` estiver definida como None e cujo `Url` estiver definida como uma cadeia de caracteres vazia. Eu decidi definir `Url` como uma cadeia vazia desde o `ProductBLL` classe s `GetProductsByCategory(categoryID)` método atualmente não tem a capacidade de retornar apenas os produtos com um `NULL` `CategoryID` valor. Além disso, eu queria demonstrar como os controles de navegação renderizam uma `SiteMapNode` que não tem um valor para seus `Url` propriedade. Eu recomendo que você estenda este tutorial, de modo que a nenhum `SiteMapNode` s `Url` propriedade aponta para `ProductsByCategory.aspx`, ainda exibe somente os produtos com `NULL` `CategoryID` valores.


Depois de construir o mapa do site, um objeto arbitrário é adicionado ao cache de dados usando uma dependência de cache SQL no `Categories` e `Products` tabelas por meio de um `AggregateCacheDependency` objeto. Exploramos usando dependências de cache SQL no tutorial anterior, *dependências de Cache de SQL usando*. O provedor de mapa de site personalizado, no entanto, usa uma sobrecarga do cache de dados s `Insert` método que podemos ve ainda para explorar. Essa sobrecarga aceita como seu parâmetro de entrada final um delegado que é chamado quando o objeto é removido do cache. Especificamente, podemos passar em uma nova [ `CacheItemRemovedCallback` delegar](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) que aponta para o `OnSiteMapChanged` método definido mais para baixo o `NorthwindSiteMapProvider` classe.

> [!NOTE]
> A representação na memória do mapa do site é armazenado em cache por meio da variável de nível de classe `root`. Como há apenas uma instância da classe do provedor de mapa de site personalizada e uma vez que essa instância é compartilhada entre todos os threads no aplicativo web, essa variável de classe serve como um cache. O `BuildSiteMap` método também usa o cache de dados, mas somente como um meio para receber notificação quando subjacente dados no banco de dados do `Categories` ou `Products` tabelas de alterações. Observe que o valor colocar no cache de dados é apenas a data e hora atuais. Os dados de mapa de site real são *não* colocados no cache de dados.


O `BuildSiteMap` método for concluído, retornando o nó raiz do mapa do site.

Os métodos restantes são bastante simples. `GetRootNodeCore` é responsável por retornar o nó raiz. Uma vez que `BuildSiteMap` retorna a raiz `GetRootNodeCore` simplesmente retorna `BuildSiteMap` s valor de retorno. O `OnSiteMapChanged` método conjuntos `root` voltar ao `null` quando o item de cache é removido. Com a raiz definido novamente como `null`, na próxima vez `BuildSiteMap` é invocado, a estrutura do mapa de site será reconstruída. Por fim, o `CachedDate` propriedade retorna o valor de data e hora armazenado no cache de dados, se existir um valor desse tipo. Essa propriedade pode ser usada por um desenvolvedor de página para determinar quando os dados de mapa do site foi pela última vez armazenado em cache.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Etapa 7: Registrar o`NorthwindSiteMapProvider`

Para que nosso aplicativo web para usar o `NorthwindSiteMapProvider` provedor de mapa de site criado na etapa 6, é preciso registrá-lo na `<siteMap>` seção `Web.config`. Especificamente, adicione a seguinte marcação dentro de `<system.web>` elemento no `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

Essa marcação faz duas coisas: primeiro, ele indica que o interno `AspNetXmlSiteMapProvider` é o provedor de mapa de site padrão; em segundo lugar, ele registra o provedor de mapa de site personalizado criado na etapa 6 com o nome amigável a humanos Northwind.

> [!NOTE]
> Para provedores de mapa de site localizados em aplicativo s `App_Code` pasta, o valor da `type` atributo é simplesmente o nome de classe. Como alternativa, o provedor de mapa de site personalizadas poderia ter sido criado em um projeto de biblioteca de classes separado com o assembly compilado colocado no s do aplicativo web `/Bin` directory. Nesse caso, o `type` seria o valor do atributo *Namespace*. *ClassName*, *AssemblyName* .


Depois de atualizar `Web.config`, reserve um tempo para exibir qualquer página com os tutoriais em um navegador. Observe que a interface de navegação à esquerda ainda mostra as seções e tutoriais definidos em `Web.sitemap`. Isso ocorre porque deixamos `AspNetXmlSiteMapProvider` como o provedor padrão. Para criar um elemento de interface do usuário de navegação que usa o `NorthwindSiteMapProvider`, precisaremos especificar explicitamente que o provedor de mapa de site do Northwind deve ser usado. Veremos como fazer isso na etapa 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Etapa 8: Exibindo informações de mapa de Site usando o provedor de mapa de Site personalizada

Com o site personalizado, o provedor de mapa de criado e registrado no `Web.config`, podemos está pronto para adicionar controles de navegação para o `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` páginas no `SiteMapProvider` pasta. Comece abrindo o `Default.aspx` da página e arraste um `SiteMapPath` da caixa de ferramentas para o Designer. O controle SiteMapPath está localizado na seção de navegação da caixa de ferramentas.


[![Adicionar um SiteMapPath para default. aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**Figura 16**: adicionar um SiteMapPath para `Default.aspx` ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))


O controle SiteMapPath exibe uma navegação de trilha, que indica o local de s de página atual no mapa do site. Adicionamos um SiteMapPath na parte superior da página mestra volta a *páginas mestras e navegação no Site* tutorial.

Reserve um tempo para exibir esta página por meio de um navegador. O SiteMapPath adicionado na Figura 16 usa o provedor de mapa de site padrão, extraindo seus dados de `Web.sitemap`. Portanto, a trilha de navegação mostra início &gt; Personalizando o mapa do Site, assim como a trilha de navegação no canto superior direito.


[![A trilha de navegação usa o provedor de mapa de Site padrão](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**Figura 17**: A trilha de navegação usa o provedor de mapa de Site padrão ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))


Para que o SiteMapPath adicionado na Figura 16 para usar o provedor de mapa de site personalizado que criamos na etapa 6, defina suas [ `SiteMapProvider` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) para a Northwind, o nome que é atribuído à `NorthwindSiteMapProvider` em `Web.config`. Infelizmente, o Designer continua a usar o provedor de mapa de site padrão, mas se você visitar a página por meio de um navegador depois de fazer essa alteração de propriedade você verá que a trilha de navegação agora usa o provedor de mapa de site personalizadas.


[![A trilha de navegação agora usa o NorthwindSiteMapProvider de provedor de mapa de Site personalizada](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**Figura 18**: agora, a trilha de navegação usa o provedor de mapa de Site personalizado `NorthwindSiteMapProvider` ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))


O controle SiteMapPath exibe uma interface do usuário mais funcional na `ProductsByCategory.aspx` e `ProductDetails.aspx` páginas. Adicionar um SiteMapPath para essas páginas, definindo o `SiteMapProvider` propriedade à Northwind. De `Default.aspx` clique no link Exibir produtos para bebidas e, em seguida, no link Exibir detalhes de chá Chai. Como mostra a Figura 19, a trilha de navegação inclui a seção de mapa de site atual (Chai chá) e seus ancestrais: bebidas e todas as categorias.


[![A trilha de navegação agora usa o NorthwindSiteMapProvider de provedor de mapa de Site personalizada](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**Figura 19**: agora, a trilha de navegação usa o provedor de mapa de Site personalizado `NorthwindSiteMapProvider` ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))


Outros elementos de interface do usuário de navegação podem ser usados além SiteMapPath, como os controles Menu e TreeView. O `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` páginas no download para este tutorial, por exemplo, todos incluem controles de Menu (consulte a Figura 20). Consulte [examinando o ASP.NET 2.0 s recursos de navegação do Site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) e o [usando controles de navegação do Site](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) seção o [guias de início rápido do ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/) para uma análise mais detalhada de controles de navegação e o sistema de mapa de site no ASP.NET 2.0.


[![O controle Menu lista cada uma das categorias e produtos](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**Figura 20**: O Menu de controle de lista cada das categorias e produtos ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))


Conforme mencionado anteriormente neste tutorial, a estrutura do mapa de site pode ser acessada por meio de programação com o `SiteMap` classe. O código a seguir retorna a raiz `SiteMapNode` do provedor padrão:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

Uma vez que o `AspNetXmlSiteMapProvider` é o provedor padrão para nosso aplicativo, o código acima retornará o nó raiz definido em `Web.sitemap`. Para fazer referência a um provedor de mapa de site diferente do padrão, use o `SiteMap` classe s [ `Providers` propriedade](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) da seguinte forma:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

Em que *nome* é o nome do provedor de mapa de site personalizado (Northwind, para nosso aplicativo web).

Para acessar um membro específico para um provedor de mapa de site, use `SiteMap.Providers["name"]` para recuperar a instância do provedor e, em seguida, convertê-lo para o tipo apropriado. Por exemplo, para exibir o `NorthwindSiteMapProvider` s `CachedDate` propriedade em uma página ASP.NET, use o seguinte código:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> Certifique-se de testar o recurso de dependência de cache SQL. Depois de visitar o `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` páginas, vá até um dos tutoriais na edição, inserção e exclusão de seção e edite o nome de uma categoria ou produto. Em seguida, retornar para uma das páginas no `SiteMapProvider` pasta. Supondo que já passou tempo suficiente para o mecanismo de sondagem alterar o banco de dados subjacente, o mapa do site deve ser atualizado para mostrar o novo produto ou o nome da categoria.


## <a name="summary"></a>Resumo

O ASP.NET 2.0 inclui recursos de mapa de site de s um `SiteMap` classe, um número de navegação interna Web controles e persistentes de um provedor de mapa de site padrão que espera que as informações de mapa de site para um arquivo XML. Para usar as informações de mapa de site de alguma outra origem, como de um banco de dados, a arquitetura do aplicativo s ou um serviço Web remoto, que precisamos criar um provedor de mapa de site personalizado. Isso envolve a criação de uma classe derivada, direta ou indiretamente, da `SiteMapProvider` classe.

Neste tutorial vimos como criar um provedor de mapa de site personalizado que se baseia o mapa do site nas informações de produto e categoria cortadas da arquitetura do aplicativo. Nosso provedor estendido a `StaticSiteMapProvider` classe e exigia a criação de um `BuildSiteMap` método que os dados recuperados construído da hierarquia do mapa de site e armazenados em cache a estrutura resultante em uma variável de nível de classe. Usamos uma dependência de cache SQL com uma função de retorno de chamada para invalidar o cache estrutura quando subjacente `Categories` ou `Products` dados são modificados.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Armazenamento de mapas de Site no SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) e [o provedor de mapa de Site do SQL ve você estava esperando](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Modelo de provedor de s uma olhada no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [O Kit de ferramentas do provedor](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Examinando o ASP.NET 2.0 s recursos de navegação do Site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Dave Gardner, Zack Jones, Teresa Murphy e Bernadette Leigh. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](building-a-custom-database-driven-site-map-provider-vb.md)
