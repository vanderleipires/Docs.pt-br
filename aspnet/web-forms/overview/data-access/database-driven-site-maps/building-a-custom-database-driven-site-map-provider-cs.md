---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: "Criação de um provedor de mapa de Site personalizado para banco de dados (c#) | Microsoft Docs"
author: rick-anderson
description: "O provedor de mapa de site padrão no ASP.NET 2.0 recupera seus dados de um arquivo XML estático. Enquanto o provedor com base em XML é adequado para muitos pequeno e médio Taman..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: f09d8d24cea43e80e66b0d8ce50911fcb978c85e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="building-a-custom-database-driven-site-map-provider-c"></a>Criação de um provedor de mapa de Site personalizado para banco de dados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip) ou [baixar PDF](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> O provedor de mapa de site padrão no ASP.NET 2.0 recupera seus dados de um arquivo XML estático. Enquanto o provedor de XML é adequado para vários sites da Web de pequenos e médio porte, aplicativos Web maiores exigem um mapa de site mais dinâmico. Neste tutorial, criaremos um provedor de mapa de site personalizado que recupera os dados da camada de lógica de negócios, que por sua vez recupera dados do banco de dados.


## <a name="introduction"></a>Introdução

O ASP.NET 2.0 recurso de mapa de site de s permite que um desenvolvedor de página Definir um mapa do site da web aplicativo s em algum meio persistente, como em um arquivo XML. Quando definido, os dados de mapa de site podem ser acessados por meio de programação com o [ `SiteMap` classe](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx) no [ `System.Web` namespace](https://msdn.microsoft.com/en-us/library/system.web.aspx) ou por meio de uma variedade de navegação na Web controles, como o Controles SiteMapPath, Menu e TreeView. O sistema de mapa de site usa o [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) para que podem ser criadas e conectadas a um aplicativo web implementações de serialização de mapa de site diferente. O provedor de mapa de site padrão que é fornecido com o ASP.NET 2.0 mantém a estrutura de mapa de site em um arquivo XML. Volta o [páginas mestras e navegação de Site](../introduction/master-pages-and-site-navigation-cs.md) tutorial criamos um arquivo chamado `Web.sitemap` que contidos nessa estrutura e ter atualizando seu XML com cada nova seção tutorial.

O provedor de mapa de site baseado em XML padrão funciona bem se a estrutura de s do mapa de site é relativamente estática, por exemplo, para esses tutoriais. Em muitos cenários, no entanto, é necessário um mapa de site mais dinâmico. Considere o mapa do site mostrado na Figura 1, onde cada categoria e produto são exibidos como seções na estrutura do site. Com esse mapa de site, visite a página da web correspondente para o nó raiz pode listar todas as categorias, enquanto visita uma página da web de determinada categoria s lista os produtos dessa categoria e exibir uma página da web de determinado produto s mostra que o produto detalhes s.


[![As categorias e a composição de produtos a estrutura de mapa s do Site](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**Figura 1**: as categorias e o mapa do Site de s estrutura de composição de produtos ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))


Enquanto essa estrutura baseadas em produtos e categoria pode ser embutida no `Web.sitemap` arquivo, o arquivo precisa ser atualizada sempre que uma categoria ou produto foi adicionado, removido ou renomeado. Consequentemente, a manutenção de mapa de site seria bastante simplificada se sua estrutura foi recuperada do banco de dados ou, de preferência, da camada de lógica de negócios a s da arquitetura do aplicativo. Dessa forma, como categorias e produtos foram adicionadas, renomeado ou excluído, o mapa de site atualizará automaticamente para refletir essas alterações.

Como a serialização de mapa de site do ASP.NET 2.0 s é criada sobre o modelo de provedor, podemos criar nosso próprio provedor de mapa de site personalizado que obtém seus dados de um repositório de dados alternativos, como o banco de dados ou a arquitetura. Neste tutorial, criaremos um provedor personalizado que recupera os dados do BLL. Permitir que o s começar!

> [!NOTE]
> O provedor de mapa de site personalizado criado neste tutorial está acoplado para o modelo de dados e a arquitetura de s do aplicativo. Jeff Prosise s [armazenar mapas de Site no SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) e [o provedor de mapa de Site do SQL que você estava esperando](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) artigos examinar um método generalizado para armazenar dados de mapa de site no SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Etapa 1: Criar as páginas da Web de provedor de mapa de Site personalizado

Antes de começar a criação de um provedor de mapa de site personalizado, permitem s primeiro adicionar as páginas ASP.NET que vamos precisar para este tutorial. Comece adicionando uma nova pasta chamada `SiteMapProvider`. Em seguida, adicione as seguintes páginas do ASP.NET para a pasta, certificando-se de associar cada página com o `Site.master` página mestre:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Também adicionar uma `CustomProviders` subpasta para o `App_Code` pasta.


![Adicionar as páginas ASP.NET para os tutoriais de relacionadas ao provedor de mapa de Site](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**Figura 2**: adicionar as páginas ASP.NET para o Site relacionadas ao provedor tutoriais do mapa


Como há apenas um tutorial para essa seção, podemos don precisa `Default.aspx` para listar os tutoriais de seção s. Em vez disso, `Default.aspx` exibirá as categorias em um controle GridView. Que abordaremos isso na etapa 2.

Em seguida, atualize `Web.sitemap` para incluir uma referência para o `Default.aspx` página. Especificamente, adicione a seguinte marcação após o cache `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

Após a atualização `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui um item para o tutorial do provedor de mapa de site único.


![O mapa do Site agora inclui uma entrada para o Tutorial de provedor de mapa de Site](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**Figura 3**: O mapa de Site agora inclui uma entrada para o Tutorial de provedor de mapa de Site


Este tutorial principal foco é ilustrar criando um provedor de mapa de site personalizado e configurar o aplicativo web para usar esse provedor. Em particular, criaremos um provedor que retorna um mapa de site que inclui um nó raiz com um nó para cada categoria e produto, conforme mostrado na Figura 1. Em geral, cada nó no mapa do site pode especificar uma URL. Para o mapa de site, a URL raiz do nó s será `~/SiteMapProvider/Default.aspx`, que lista todas as categorias no banco de dados. Cada nó de categoria no mapa do site terá uma URL que aponta para `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, que listará todos os produtos do *categoryID*. Por fim, cada nó de mapa de site do produto apontará para `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, que exibirá os detalhes de produto específico s.

Para iniciar, precisamos criar o `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` páginas. Essas páginas são concluídas nas etapas 2, 3 e 4, respectivamente. Como o foco deste tutorial é os provedores de mapa de site, e como últimos tutoriais sobre como criar esses tipos de várias páginas mestre/detalhes relatórios, que será corra por meio das etapas 2 a 4. Se você precisar de um atualizador sobre como criar relatórios de detalhes/mestre que abrangem várias páginas, consulte novamente o [filtragem de mestre/detalhes em duas páginas](../masterdetail/master-detail-filtering-across-two-pages-cs.md) tutorial.

## <a name="step-2-displaying-a-list-of-categories"></a>Etapa 2: Exibindo uma lista de categorias

Abra o `Default.aspx` página o `SiteMapProvider` pasta e arraste um controle GridView da caixa de ferramentas para o Designer, definindo seu `ID` para `Categories`. De GridView s marca inteligente, associá-lo a um novo ObjectDataSource denominado `CategoriesDataSource` e configurá-lo para que ele recupere seus dados usando o `CategoriesBLL` classe s `GetCategories` método. Como esse GridView apenas exibe as categorias e não fornece capacidades de modificação de dados, definir as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Configurar o ObjectDataSource para retornar as categorias usando o método GetCategories](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**Figura 4**: configurar o ObjectDataSource para retornar categorias usando o `GetCategories` método ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))


[![Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**Figura 5**: definir a lista suspensa na atualização, inserir e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio irá adicionar um BoundField para `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, e `BrochurePath`. Editar o GridView para que ele contém apenas o `CategoryName` e `Description` BoundFields e atualizar o `CategoryName` BoundField s `HeaderText` propriedade à categoria.

Em seguida, adicione um HyperLinkField e posicione-o assim que o campo mais à esquerda. Definir o `DataNavigateUrlFields` propriedade `CategoryID` e o `DataNavigateUrlFormatString` propriedade `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Definir o `Text` propriedade para exibir os produtos.


![Adicionar um HyperLinkField a GridView categorias](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**Figura 6**: adicionar um HyperLinkField para o `Categories` GridView


Depois de criar o ObjectDataSource e personalizar os campos de s GridView, a marcação declarativa dois controles será a seguinte aparência:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

A Figura 7 mostra `Default.aspx` quando visualizada através de um navegador. Clique em uma categoria s exibir produtos link leva você à `ProductsByCategory.aspx?CategoryID=categoryID`, que vamos criar na etapa 3.


[![Cada categoria é listado junto com um Link de produtos do modo de exibição](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**Figura 7**: cada categoria é listado junto com um Link de produtos do modo de exibição ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Etapa 3: Lista os produtos a categoria selecionada

Abra o `ProductsByCategory.aspx` página e adicione um GridView, nomeando- `ProductsByCategory`. Na marca inteligente, de associar o GridView para um novo ObjectDataSource denominado `ProductsByCategoryDataSource`. Configurar o ObjectDataSource para usar o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método e o conjunto de listas suspensas como (nenhum) nas guias UPDATE, INSERT e DELETE.


[![Use o método de GetProductsByCategoryID(categoryID) ProductsBLL classe s](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**Figura 8**: Use o `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))


Solicita que a etapa final no Assistente para configurar a fonte de dados para uma fonte de parâmetro para *categoryID*. Como essas informações são passadas por meio do campo querystring `CategoryID`, selecione QueryString na lista suspensa e digite CategoryID na caixa de texto QueryStringField, conforme mostrado na Figura 9. Clique em Concluir para concluir o assistente.


[![Use o campo de Querystring CategoryID para o parâmetro categoryID](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**Figura 9**: Use o `CategoryID` Querystring Field para o *categoryID* parâmetro ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))


Depois de concluir o assistente, o Visual Studio adicionará BoundFields correspondente e um CheckBoxField GridView para os campos de dados de produto. Remover tudo, exceto o `ProductName`, `UnitPrice`, e `SupplierName` BoundFields. Personalizar esses três BoundFields `HeaderText` propriedades para ler o produto, preço e fornecedor, respectivamente. Formato de `UnitPrice` BoundField como uma moeda.

Em seguida, adicione um HyperLinkField e mova-o para a posição mais à esquerda. Definir seu `Text` propriedade para exibir detalhes, seu `DataNavigateUrlFields` propriedade `ProductID`e sua `DataNavigateUrlFormatString` propriedade para `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Adicionar um HyperLinkField de detalhes de exibição que aponta para ProductDetails.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**Figura 10**: adicionar um modo de exibição de detalhes HyperLinkField que aponta para`ProductDetails.aspx`


Depois de fazer essas personalizações, o GridView e ObjectDataSource s declarativo deve se parecer com o seguinte:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

Retornar à exibição `Default.aspx` por meio de um navegador e clique em Exibir produtos de link para bebidas. Isso o levará para `ProductsByCategory.aspx?CategoryID=1`, exibindo os nomes, os preços e os fornecedores dos produtos no banco de dados Northwind que pertencem à categoria de bebidas (consulte a Figura 11). Fique à vontade para aumentar ainda mais a esta página para incluir um link para retornar os usuários para a página de listagem de categoria (`Default.aspx`) e um controle DetailsView ou FormView que exibe o nome da categoria selecionada s e a descrição.


[![Os nomes de bebidas, preços e fornecedores são exibidos](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**Figura 11**: os nomes de bebidas, preços e fornecedores são exibidos ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Etapa 4: Mostrando uma s os detalhes do produto

A página final, `ProductDetails.aspx`, exibe os detalhes de produtos selecionados. Abra `ProductDetails.aspx` e arraste um DetailsView da caixa de ferramentas para o Designer. Definir o s DetailsView `ID` propriedade `ProductInfo` e limpar seu `Height` e `Width` valores de propriedade. De marca inteligente, associar DetailsView para um novo ObjectDataSource denominado `ProductDataSource`, configurando ObjectDataSource para efetuar o pull de seus dados a partir de `ProductsBLL` classe s `GetProductByProductID(productID)` método. Assim como acontece com as páginas da web anterior criado nas etapas 2 e 3, defina as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Configurar o ObjectDataSource para usar o método GetProductByProductID(productID)](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**Figura 12**: configurar o ObjectDataSource para usar o `GetProductByProductID(productID)` método ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))


Solicita que a última etapa do Assistente para configurar a fonte de dados da fonte de *productID* parâmetro. Como esses dados vem por meio do campo querystring `ProductID`, defina a lista suspensa QueryString e textbox QueryStringField ProductID. Por fim, clique no botão Concluir para concluir o assistente.


[![Configurar o parâmetro para extrair o valor do campo de Querystring ProductID productID](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**Figura 13**: configurar o *productID* parâmetro para extrair o valor da `ProductID` Querystring Field ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio criará BoundFields correspondente e um CheckBoxField em DetailsView para os campos de dados de produto. Remover o `ProductID`, `SupplierID`, e `CategoryID` BoundFields e configurar os campos restantes como desejar. Depois de uma série de configurações estéticos, meu DetailsView e ObjectDataSource s declarativo pesquisados semelhante ao seguinte:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

Para testar essa página, retorne ao `Default.aspx` e clique em Exibir produtos para a categoria de bebidas. Na lista de produtos de bebidas, clique no link Exibir detalhes para Chai chá. Isso o levará para `ProductDetails.aspx?ProductID=1`, que mostra um s Chai chá detalhes (consulte a Figura 14).


[![Chai chá s fornecedor, categoria, preço e outras informações é exibido](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**Figura 14**: Chai chá s fornecedor, categoria, preço e outras informações é exibido ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Etapa 5: Noções básicas sobre o funcionamento interno de um provedor de mapa de Site

O mapa do site é representado na memória de servidor s web como uma coleção de `SiteMapNode` instâncias que formam uma hierarquia. Deve haver exatamente uma raiz, todos os nós raiz não devem ter exatamente um nó pai e todos os nós podem ter um número arbitrário de filhos. Cada `SiteMapNode` objeto representa uma seção na estrutura de site s; essas seções normalmente têm uma página da web correspondente. Consequentemente, o [ `SiteMapNode` classe](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.aspx) tem propriedades como `Title`, `Url`, e `Description`, que fornecem informações para a seção de `SiteMapNode` representa. Também há um `Key` propriedade que identifica exclusivamente cada `SiteMapNode` na hierarquia, bem como as propriedades usadas para estabelecer essa hierarquia `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`e assim por diante.

Figura 15 mostra a estrutura de mapa de site geral da Figura 1, mas com os detalhes de implementação elaborados em mais detalhes.


[![Cada SiteMapNode tem propriedades como título, Url, chave e assim por diante](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**Figura 15**: cada `SiteMapNode` tem propriedades como `Title`, `Url`, `Key`, e assim por diante ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))


O mapa do site é acessível por meio de [ `SiteMap` classe](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx) no [ `System.Web` namespace](https://msdn.microsoft.com/en-us/library/system.web.aspx). Essa classe s `RootNode` propriedade retorna a raiz do mapa s site `SiteMapNode` instância; `CurrentNode` retorna o `SiteMapNode` cujo `Url` propriedade corresponde à URL da página solicitada no momento. Essa classe é usada internamente por controles da Web do ASP.NET 2.0 s navegação.

Quando o `SiteMap` propriedades de classe s são acessadas, ele deve serializar a estrutura do mapa do site de alguma mídia persistente na memória. No entanto, a lógica de serialização de mapa de site não é difícil codificado no `SiteMap` classe. Em vez disso, em tempo de execução de `SiteMap` classe determina qual mapa de site *provedor* a ser usado para serialização. Por padrão, o [ `XmlSiteMapProvider` classe](https://msdn.microsoft.com/en-us/library/system.web.xmlsitemapprovider.aspx) for usado, que lê a estrutura de mapa s do site de um arquivo XML formatado corretamente. No entanto, com um pouco de trabalho podemos criar nosso próprio provedor de mapa de site personalizado.

Todos os provedores de mapa de site devem ser derivados de [ `SiteMapProvider` classe](https://msdn.microsoft.com/en-us/library/system.web.sitemapprovider.aspx), que inclui os métodos essenciais e propriedades necessárias para o site mapear provedores, mas omite muitos dos detalhes de implementação. A segunda classe [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/en-us/library/system.web.staticsitemapprovider.aspx), estende o `SiteMapProvider` classe e contém uma implementação mais robusta do que a funcionalidade necessária. Internamente, o `StaticSiteMapProvider` armazena o `SiteMapNode` instâncias do site do mapa em uma `Hashtable` e fornece métodos como `AddNode(child, parent)`, `RemoveNode(siteMapNode),` e `Clear()` que adicionar e remover `SiteMapNode` s para interno `Hashtable`. `XmlSiteMapProvider`é derivado de `StaticSiteMapProvider`.

Quando criar um provedor de mapa de site personalizado que estende `StaticSiteMapProvider`, há dois métodos abstratos que devem ser substituídos: [ `BuildSiteMap` ](https://msdn.microsoft.com/en-us/library/system.web.staticsitemapprovider.buildsitemap.aspx) e [ `GetRootNodeCore` ](https://msdn.microsoft.com/en-us/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, como o nome sugere, é responsável por carregar a estrutura de mapa de site do armazenamento persistente e construi-la na memória. `GetRootNodeCore`Retorna o nó raiz no mapa do site.

Antes de uma web aplicativo pode usar um provedor de mapa de site que ele deve ser registrado na configuração de s do aplicativo. Por padrão, o `XmlSiteMapProvider` classe está registrada usando o nome `AspNetXmlSiteMapProvider`. Para registrar provedores de mapa de site adicionais, adicione a seguinte marcação para `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

O *nome* valor atribui um nome legível para o provedor ao *tipo* Especifica o nome de tipo totalmente qualificado do provedor de mapa do site. Vamos explorar concretos valores para o *nome* e *tipo* valores na etapa 7, após nós ve criado nosso provedor de mapa de site personalizado.

A classe de provedor de mapa de site é criada na primeira vez que ele é acessado a partir de `SiteMap` classe e permanece na memória para o tempo de vida do aplicativo web. Como há apenas uma instância do provedor de mapa do site que pode ser invocado vários simultâneas de visitantes do site, é necessário que os métodos do provedor s ser *thread-safe*.

Por motivos de escalabilidade, de desempenho e ele importante que o site na memória em cache mapear estrutura e retornar isso em cache estrutura em vez de recriá-la sempre que o `BuildSiteMap` método é invocado. `BuildSiteMap`pode ser chamado várias vezes por solicitação de página por usuário, dependendo dos controles de navegação em uso na página e a profundidade da estrutura de mapa do site. Em qualquer caso, se a estrutura de mapa de site em cache não `BuildSiteMap` , em seguida, cada vez que ele é invocado seria necessário recuperar novamente as informações de categoria e produto da arquitetura (que pode resultar em uma consulta no banco de dados). Conforme abordado nos tutoriais de cache anteriores, dados armazenados em cache podem se tornar obsoletos. Para combater isso, podemos usar o tempo - ou com base em dependência expirações de cache SQL.

> [!NOTE]
> Um provedor de mapa de site pode, opcionalmente, substituir o [ `Initialize` método](https://msdn.microsoft.com/en-us/library/system.web.sitemapprovider.initialize.aspx). `Initialize`é invocado quando o provedor de mapa de site é criado pela primeira vez e é passado qualquer atributos personalizados atribuídos para o provedor no `Web.config` no `<add>` elemento como: `<add name="name" type="type" customAttribute="value" />`. É útil se você quiser permitir que um desenvolvedor de página especificar várias configurações de relacionadas ao provedor do site mapa sem a necessidade de modificar o código do provedor s. Por exemplo, se nós foram lendo os dados de categoria e produtos diretamente do banco de dados em vez de por meio da arquitetura, podemos d provavelmente deseja permitem que o desenvolvedor de página especifique a cadeia de caracteres de conexão de banco de dados por meio de `Web.config` em vez de usar um codificado valor no código de provedor s. O provedor de mapa de site personalizado, criaremos na etapa 6 não substituir isso `Initialize` método. Para obter um exemplo de como usar o `Initialize` método, consulte [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [armazenar mapas de Site no SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) artigo.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Etapa 6: Criando o provedor de mapa de Site personalizado

Para criar um provedor de mapa de site personalizado que cria o mapa do site de categorias de produtos no banco de dados Northwind, é preciso criar uma classe que estende o `StaticSiteMapProvider`. Na etapa 1, solicitado para adicionar um `CustomProviders` pasta o `App_Code` pasta - adicionar uma nova classe para essa pasta denominada `NorthwindSiteMapProvider`. Adicione o seguinte código à classe `NorthwindSiteMapProvider`:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

Permitem s recomece explorar essa classe s `BuildSiteMap` método, que começa com um [ `lock` instrução](https://msdn.microsoft.com/en-us/library/c5kehkcz.aspx). O `lock` instrução permite apenas um thread por vez para inserir, assim, serializar o acesso ao seu código e impedindo que dois threads simultâneos passo a passo dos pés de s uma da outra.

O nível de classe `SiteMapNode` variável `root` é usado para armazenar a estrutura de mapa de site. Quando o mapa do site é criado pela primeira vez, ou pela primeira vez depois que os dados subjacentes tem sido modificados, `root` será `null` e a estrutura de mapa do site será construída. O nó raiz do site mapa s é atribuído a `root` durante a construção de processo para que na próxima vez que esse método é chamado, `root` não será `null`. Consequentemente, desde que `root` não é `null` a estrutura de mapa do site será retornada ao chamador sem ter que recriá-lo.

Se for raiz `null`, a estrutura de mapa de site é criada a partir de informações de produto e categoria. O mapa do site é criado por meio da criação de `SiteMapNode` instâncias e, em seguida, cria a hierarquia por meio de chamadas para o `StaticSiteMapProvider` classe s `AddNode` método. `AddNode`executa o sistema de controle interno, armazenando as diversas `SiteMapNode` instâncias em um `Hashtable`. Antes de começar a construção de hierarquia, começamos chamando o `Clear` método, que limpa os elementos de interno `Hashtable`. Em seguida, o `ProductsBLL` classe s `GetProducts` método e resultante `ProductsDataTable` são armazenados em variáveis locais.

A construção de mapa s site começa criando o nó raiz e atribuí-lo para `root`. A sobrecarga do [ `SiteMapNode` construtor s](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.sitemapnode.aspx) usado aqui e em todo este `BuildSiteMap` é passado as seguintes informações:

- Uma referência para o provedor de mapa de site (`this`).
- O `SiteMapNode` s `Key`. Isso necessário valor deve ser exclusivo para cada `SiteMapNode`.
- O `SiteMapNode` s `Url`. `Url`é opcional, mas, se fornecido, cada `SiteMapNode` s `Url` valor deve ser exclusivo.
- O `SiteMapNode` s `Title`, que é necessário.

O `AddNode(root)` chamada de método adiciona o `SiteMapNode` `root` para o mapa do site como a raiz. Em seguida, cada `ProductRow` no `ProductsDataTable` é enumerado. Se já existe um `SiteMapNode` para a categoria de produto s atual, ele é referenciado. Caso contrário, um novo `SiteMapNode` para a categoria é criada e adicionada como um filho do `SiteMapNode``root` por meio de `AddNode(categoryNode, root)` chamada de método. Após a categoria apropriada `SiteMapNode` nó foi encontrado ou criado, um `SiteMapNode` é criado para o produto atual e adicionado como um filho da categoria `SiteMapNode` via `AddNode(productNode, categoryNode)`. Observe que a categoria `SiteMapNode` s `Url` é o valor da propriedade `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` enquanto o produto `SiteMapNode` s `Url` atribuído a propriedade `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Os produtos que têm um banco de dados `NULL` valor para seus `CategoryID` são agrupadas em uma categoria `SiteMapNode` cujo `Title` propriedade está definida como None e cujo `Url` está definida como uma cadeia de caracteres vazia. Decidi definir `Url` para uma cadeia de caracteres vazia, desde o `ProductBLL` classe s `GetProductsByCategory(categoryID)` método atualmente não tem a capacidade de retornar apenas os produtos com um `NULL` `CategoryID` valor. Além disso, gostaria de demonstrar como os controles de navegação renderizam um `SiteMapNode` que não tem um valor para o seu `Url` propriedade. Você deve estender esse tutorial para que nenhuma `SiteMapNode` s `Url` propriedade aponta para `ProductsByCategory.aspx`, ainda exibe somente os produtos com `NULL` `CategoryID` valores.


Depois de construir o mapa do site, um objeto arbitrário é adicionado ao cache de dados usando uma dependência de cache SQL no `Categories` e `Products` tabelas por meio de um `AggregateCacheDependency` objeto. Podemos explorou o uso de dependências de cache SQL no tutorial anterior, *dependências de Cache de SQL usando*. O provedor de mapa de site personalizado, no entanto, usa uma sobrecarga do cache de dados s `Insert` método que possamos ve ainda para explorar. Essa sobrecarga aceita como seu parâmetro de entrada final um delegado que é chamado quando o objeto é removido do cache. Especificamente, podemos passar em uma nova [ `CacheItemRemovedCallback` delegar](https://msdn.microsoft.com/en-us/library/system.web.caching.cacheitemremovedcallback.aspx) que aponta para o `OnSiteMapChanged` método definido mais para baixo o `NorthwindSiteMapProvider` classe.

> [!NOTE]
> A representação na memória do mapa do site é armazenado em cache através da variável de nível de classe `root`. Como há apenas uma instância da classe do provedor de mapa de site personalizado e desde que essa instância é compartilhada entre todos os threads no aplicativo web, essa variável de classe serve como um cache. O `BuildSiteMap` método também usa o cache de dados, mas apenas como um meio para receber uma notificação quando a base de dados em banco de dados de `Categories` ou `Products` de tabelas de alterações. Observe que o valor colocar no cache de dados é apenas a data e hora atuais. Os dados de mapa de site real são *não* colocados no cache de dados.


O `BuildSiteMap` método é concluído, retornando o nó raiz do mapa do site.

Os métodos restantes são bastante simples. `GetRootNodeCore`é responsável por retornar o nó raiz. Como `BuildSiteMap` retorna a raiz, `GetRootNodeCore` simplesmente retorna `BuildSiteMap` s valor de retorno. O `OnSiteMapChanged` método conjuntos `root` para `null` quando o item de cache é removido. Com raiz definido novamente como `null`, na próxima vez `BuildSiteMap` é chamado, a estrutura de mapa de site será reconstruída. Por fim, o `CachedDate` propriedade retorna o valor de data e hora armazenado no cache de dados, se esse valor existe. Essa propriedade pode ser usada por um desenvolvedor de página para determinar quando os dados de mapa do site foi última armazenado em cache.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Etapa 7: Registrar o`NorthwindSiteMapProvider`

Para que nosso aplicativo web para usar o `NorthwindSiteMapProvider` provedor de mapa de site criado na etapa 6, é preciso registrá-lo no `<siteMap>` seção `Web.config`. Especificamente, adicione a seguinte marcação dentro de `<system.web>` elemento `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

Essa marcação faz duas coisas: primeiro, ele indica que o interno `AspNetXmlSiteMapProvider` é o provedor de mapa de site padrão; em segundo lugar, ele registra o provedor de mapa de site personalizado criado na etapa 6 com o nome amigável a humanos Northwind.

> [!NOTE]
> Para provedores de mapa de site, localizados em aplicativo s `App_Code` pasta, o valor de `type` é simplesmente o nome da classe. Como alternativa, o provedor de mapa de site personalizado pode ter sido criado em um projeto de biblioteca de classes separado com o assembly compilado colocado em aplicativo s web `/Bin` directory. Nesse caso, o `type` seria o valor do atributo *Namespace*. *Nome da classe*, *AssemblyName* .


Após a atualização `Web.config`, reserve um tempo para exibir qualquer página de tutoriais em um navegador. Observe que a interface de navegação à esquerda ainda mostra as seções e tutoriais definido em `Web.sitemap`. Isso ocorre porque foi deixado `AspNetXmlSiteMapProvider` como o provedor padrão. Para criar um elemento de interface do usuário de navegação que usa o `NorthwindSiteMapProvider`, precisaremos para especificar explicitamente que o provedor de mapa de site do Northwind deve ser usado. Veremos como fazer isso na etapa 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Etapa 8: Exibindo informações de mapa de Site usando o provedor de mapa de Site personalizado

Com um site personalizado, o provedor de mapa criado e registrado em `Web.config`, podemos re pronto para adicionar controles de navegação para o `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` páginas o `SiteMapProvider` pasta. Comece abrindo o `Default.aspx` página e arraste um `SiteMapPath` da caixa de ferramentas para o Designer. O controle SiteMapPath está localizado na seção de navegação da caixa de ferramentas.


[![Adicionar um SiteMapPath como Default. aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**Figura 16**: adicionar um SiteMapPath para `Default.aspx` ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))


O controle SiteMapPath exibe uma trilha de navegação, que indica o local de s de página atual no mapa do site. Adicionamos um SiteMapPath à parte superior da página mestra novamente o *páginas mestras e navegação de Site* tutorial.

Dedicar um tempo para exibir esta página por meio de um navegador. O SiteMapPath adicionado na Figura 16 usa o provedor de mapa de site padrão, extraindo seus dados de `Web.sitemap`. Portanto, a navegação estrutural mostra início &gt; Personalizando o mapa do Site, como navegação estrutural no canto superior direito.


[![A navegação de trilha usa o provedor de mapa de Site padrão](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**Figura 17**: navegação estrutural usa o provedor de mapa de Site padrão ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))


Para que o SiteMapPath adicionado na Figura 16 usar o provedor de mapa de site personalizado criado na etapa 6, defina seu [ `SiteMapProvider` propriedade](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) Northwind, o nome que é atribuído para o `NorthwindSiteMapProvider` em `Web.config`. Infelizmente, o Designer continua a usar o provedor de mapa de site padrão, mas se você visitar a página por meio de um navegador depois de fazer essa alteração de propriedade você verá que a navegação de trilha agora usa o provedor de mapa de site personalizado.


[![A navegação de trilha agora usa o NorthwindSiteMapProvider de provedor de mapa de Site personalizado](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**Figura 18**: navegação estrutural agora usa o provedor de mapa de Site personalizado `NorthwindSiteMapProvider` ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))


O controle SiteMapPath exibe uma interface de usuário mais funcional no `ProductsByCategory.aspx` e `ProductDetails.aspx` páginas. Adicionar um SiteMapPath essas páginas, definindo o `SiteMapProvider` propriedade no para a Northwind. De `Default.aspx` clique no link Exibir produtos para bebidas e, em seguida, no link Exibir detalhes para Chai chá. Como mostra a Figura 19, navegação estrutural inclui a seção de mapa de site atual (Chai chá) e seus ancestrais: bebidas e todas as categorias.


[![A navegação de trilha agora usa o NorthwindSiteMapProvider de provedor de mapa de Site personalizado](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**Figura 19**: navegação estrutural agora usa o provedor de mapa de Site personalizado `NorthwindSiteMapProvider` ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))


Outros elementos de interface do usuário de navegação podem ser usados além SiteMapPath, como os controles de Menu e TreeView. O `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` páginas de download para este tutorial incluem controles de Menu (consulte a Figura 20). Consulte [examinando o ASP.NET 2.0 s recursos de navegação do Site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) e [usando controles de navegação do Site](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) seção o [início rápido do ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/) para uma análise mais detalhada a controles de navegação e sistema de mapa de site no ASP.NET 2.0.


[![O controle de Menu lista cada uma das categorias e produtos](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**Figura 20**: O Menu controle lista cada uma das categorias de produtos ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))


Como mencionado anteriormente neste tutorial, a estrutura de mapa de site pode ser acessada por meio de programação com o `SiteMap` classe. O código a seguir retorna a raiz `SiteMapNode` do provedor padrão:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

Como o `AspNetXmlSiteMapProvider` é o provedor padrão para o nosso aplicativo, o código acima retornaria o nó raiz definido no `Web.sitemap`. Para fazer referência a um provedor de mapa de site diferente do padrão, use o `SiteMap` classe s [ `Providers` propriedade](https://msdn.microsoft.com/en-us/library/system.web.sitemap.providers.aspx) da seguinte forma:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

Onde *nome* é o nome do provedor de mapa do site personalizado (Northwind, para nosso aplicativo web).

Para acessar um membro específico para um provedor de mapa de site, use `SiteMap.Providers["name"]` para recuperar a instância do provedor e, em seguida, converta-o para o tipo apropriado. Por exemplo, para exibir o `NorthwindSiteMapProvider` s `CachedDate` propriedade em uma página ASP.NET, use o seguinte código:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> Certifique-se de testar o recurso de dependência de cache SQL. Depois de visitar o `Default.aspx`, `ProductsByCategory.aspx`, e `ProductDetails.aspx` páginas, vá para um dos tutoriais de edição, inserção e exclusão de seção e edite o nome de uma categoria ou produto. Retornar a uma das páginas no `SiteMapProvider` pasta. Supondo que já passou tempo suficiente para o mecanismo de sondagem alterar o banco de dados subjacente, o mapa do site deve ser atualizado para mostrar o novo nome do produto ou categoria.


## <a name="summary"></a>Resumo

ASP.NET 2.0 inclui recursos de mapa de site de s um `SiteMap` classe, um número de navegação interno Web controla e persistente de um provedor de mapa de site padrão que espera as informações de mapa de site para um arquivo XML. Para usar as informações de mapa de site de outra origem, como de um banco de dados, a arquitetura do aplicativo s ou um serviço Web remoto, precisamos criar um provedor de mapa de site personalizado. Isso envolve a criação de uma classe derivada, direta ou indiretamente, do `SiteMapProvider` classe.

Neste tutorial, vimos como criar um provedor de mapa de site personalizado que acordo o mapa do site com as informações de produto e categoria cortadas da arquitetura do aplicativo. Nosso provedor estendido a `StaticSiteMapProvider` classe e exigia a criação de um `BuildSiteMap` método que recuperou os dados, construída a hierarquia de mapa do site e em cache a estrutura resultante em uma variável de nível de classe. Usamos uma dependência de cache SQL com uma função de retorno de chamada para invalidar o cache quando da estrutura subjacente `Categories` ou `Products` dados são modificados.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Armazenamento de mapas de Site no SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) e [o provedor de mapa de Site do SQL que você estava esperando](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Modelo de provedor s uma olhada em ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [O Kit de ferramentas do provedor](https://msdn.microsoft.com/en-us/asp.net/aa336558.aspx)
- [Examinando o ASP.NET 2.0 s recursos de navegação de Site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Dave Gardner, Zack Jones, Teresa Murphy e Bernadette Leigh. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Avançar](building-a-custom-database-driven-site-map-provider-vb.md)
