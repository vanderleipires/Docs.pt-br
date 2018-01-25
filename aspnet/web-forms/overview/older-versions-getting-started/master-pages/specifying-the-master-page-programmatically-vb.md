---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: "Especificando a página mestra programaticamente (VB) | Microsoft Docs"
author: rick-anderson
description: "Examina a configuração do conteúdo da página mestra programaticamente por meio do manipulador de eventos PreInit."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: 191de7546e2ba913fda0c8c8a8bfd3531b53336e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="specifying-the-master-page-programmatically-vb"></a>Especificando a página mestra programaticamente (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> Examina a configuração do conteúdo da página mestra programaticamente por meio do manipulador de eventos PreInit.


## <a name="introduction"></a>Introdução

Desde o Exemplo inaugural [ *criar um Layout de todo o Site usando páginas mestras*](creating-a-site-wide-layout-using-master-pages-vb.md), todo o conteúdo páginas referenciou sua página mestra declarativamente por meio de `MasterPageFile` atributo o `@Page`diretiva. Por exemplo, a seguinte `@Page` diretiva vincula a página de conteúdo para a página mestra `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

O [ `Page` classe](https://msdn.microsoft.com/library/system.web.ui.page.aspx) no `System.Web.UI` namespace inclui um [ `MasterPageFile` propriedade](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) que retorna o caminho para o conteúdo da página mestra; é esta propriedade definida pelo `@Page` diretiva. Essa propriedade também pode ser usada para especificar de forma programática o conteúdo da página mestra. Essa abordagem é útil se você quiser atribuir dinamicamente a página mestra com base em fatores externos, como o usuário visitar a página.

Neste tutorial, adicionar uma segunda página mestra nosso site e dinamicamente decidir qual página mestra para usar em tempo de execução.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Etapa 1: Examinar o ciclo de vida da página

Sempre que uma solicitação chega ao servidor da web para uma página do ASP.NET é uma página de conteúdo, o mecanismo do ASP.NET deve unir a página conteúdo controles ContentPlaceHolder correspondente do controles na página mestra. Esta fusão cria uma hierarquia de controle único que pode, em seguida, passe o ciclo de vida da página típico.

A Figura 1 ilustra este fusão. Etapa 1 na Figura 1 mostra o conteúdo inicial e as hierarquias de controle de página mestra. No final da parte final do estágio PreInit o conteúdo controles na página são adicionados para o ContentPlaceHolders correspondente na página mestra (etapa 2). Após essa fusão, a página mestra serve como a raiz da hierarquia de controle mescladas. Isso fundido controle hierarquia é adicionada à página para produzir a hierarquia de controle finalizadas (etapa 3). O resultado é que a hierarquia da página controle inclui a hierarquia de controle mescladas.


[![A página mestra e hierarquias de controle da página de conteúdo são fundido juntas durante o estágio de PreInit](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**Figura 01**: hierarquias de controle de página mestra e da página de conteúdo são fundido juntas durante o estágio de PreInit ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-vb/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Etapa 2: Definir o`MasterPageFile`propriedade de código

Qual página mestra partakes nesta fusão depende do valor da `Page` do objeto `MasterPageFile` propriedade. Definindo o `MasterPageFile` atributo no `@Page` diretiva tem o efeito de atribuição a `Page`do `MasterPageFile` propriedade durante a fase de inicialização, que é o primeiro estágio do ciclo de vida da página. Como alternativa, pode definir essa propriedade programaticamente. No entanto, é essencial que esta propriedade seja definida antes que ocorra de fusão na Figura 1.

No início do estágio PreInit o `Page` objeto gera seu [ `PreInit` evento](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) e chama seu [ `OnPreInit` método](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Para definir a página mestra programaticamente, em seguida, podemos pode criar um manipulador de eventos para o `PreInit` evento ou substituição de `OnPreInit` método. Vamos examinar as duas abordagens.

Comece abrindo `Default.aspx.vb`, o arquivo de classe code-behind para a home page do nosso site. Adicionar um manipulador de eventos para a página `PreInit` evento digitando o seguinte código:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

Aqui nós podemos definir o `MasterPageFile` propriedade. Atualize o código para que ele atribui o valor "~ / Site.master" para o `MasterPageFile` propriedade.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

Se você definir um ponto de interrupção e iniciar a depuração, você verá que sempre que o `Default.aspx` página for visitada ou sempre que houver um postback para essa página, o `Page_PreInit` manipulador de eventos é executado e o `MasterPageFile` propriedade é atribuída a "~ / Site.master".

Como alternativa, você pode substituir o `Page` da classe `OnPreInit` método e defina o `MasterPageFile` propriedade existe. Neste exemplo, vamos não definir a página mestra em uma página específica, mas em vez de `BasePage`. Lembre-se de que criamos uma classe base de página personalizado (`BasePage`) novamente o [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial. No momento `BasePage` substitui o `Page` da classe `OnLoadComplete` método, onde ele define a página `Title` propriedade com base nos dados de mapa do site. Vamos atualizar `BasePage` também substituir o `OnPreInit` método para especificar a página mestra de forma programática.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

Como todas as nossas páginas de conteúdo derivam da `BasePage`, todos eles agora tem sua página mestra atribuída por meio de programação. Neste ponto o `PreInit` manipulador de eventos `Default.aspx.vb` é supérflua; fique à vontade para removê-lo.

### <a name="what-about-thepagedirective"></a>Por que o`@Page`diretiva?

O que pode ser um pouco confuso é que o conteúdo das páginas `MasterPageFile` propriedades agora estão sendo especificadas em dois lugares: programaticamente no `BasePage` da classe `OnPreInit` método, bem como por meio de `MasterPageFile` atributo em cada página de conteúdo `@Page` diretiva.

O primeiro estágio do ciclo de vida da página é a fase de inicialização. Durante esse estágio o `Page` do objeto `MasterPageFile` é atribuído o valor da propriedade a `MasterPageFile` atributo o `@Page` diretiva (se ele é fornecido). O estágio de PreInit segue o estágio de inicialização, e é aqui onde podemos definir programaticamente o `Page` do objeto `MasterPageFile` , substituindo o valor atribuído de propriedade a `@Page` diretiva. Porque está sendo configurado a `Page` do objeto `MasterPageFile` propriedade programaticamente, foi possível remover o `MasterPageFile` de atributo da `@Page` diretiva sem afetar a experiência do usuário final. Convencer de isso, vá em frente e remover o `MasterPageFile` de atributo do `@Page` diretiva em `Default.aspx` e, em seguida, visite a página por meio de um navegador. Como você poderia esperar, a saída é o mesmo que antes do atributo foi removido.

Se o `MasterPageFile` propriedade é definida por meio de `@Page` diretiva ou programaticamente, é irrelevante à experiência do usuário final. No entanto, o `MasterPageFile` atributo no `@Page` diretiva é usada pelo Visual Studio durante o tempo de design para produzir o WYSIWYG exibição no Designer. Se você retornar a `Default.aspx` no Visual Studio e navegue para o Designer, você verá a mensagem "Erro de página mestra: A página tem controles que exigem uma referência de página mestra, mas nenhum for especificado" (consulte a Figura 2).

Em resumo, você precisa sair do `MasterPageFile` atributo no `@Page` diretiva aproveitar uma experiência avançada de tempo de design no Visual Studio.


[![O Visual Studio usa o @Page MasterPageFile atributo da diretiva para renderizar a exibição de Design](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**Figura 02**: Visual Studio usa o `@Page` da diretiva `MasterPageFile` atributo para processar o modo de exibição de Design ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-vb/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>Etapa 3: Criando uma página mestra alternativa

Porque um conteúdo da página mestra pode ser definida por meio de programação em tempo de execução é possível carregar dinamicamente uma página mestra específica com base em alguns critérios externos. Essa funcionalidade pode ser útil em situações em que o layout do site precisa para variar com base no usuário. Por exemplo, um aplicativo de web do mecanismo de blog pode permitir que seus usuários escolher um layout para o blog, onde cada layout é associada uma página mestre diferente. Em tempo de execução, quando um visitante está exibindo o blog do usuário, o aplicativo web precisa determinar o layout do blog e associar dinamicamente a página mestra correspondente com a página de conteúdo.

Vamos examinar como carregar dinamicamente uma página mestra em tempo de execução com base em alguns critérios externos. Nosso site atualmente contém apenas uma página mestra (`Site.master`). Precisamos de outra página mestra para ilustrar a escolha de uma página mestra em tempo de execução. Esta etapa se concentra em criar e configurar a nova página mestra. Etapa 4 olha para determinar qual página mestra para usar em tempo de execução.

Criar uma nova página mestra na pasta raiz chamada `Alternate.master`. Também adicionar uma nova folha de estilo para o site da Web denominado `AlternateStyles.css`.


[![Adicione outro arquivo de página mestra e CSS para o site](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**Figura 03**: arquivo de CSS para o site e adicionar outra página mestra ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-vb/_static/image9.png))


Eu criei o `Alternate.master` página mestra para o título exibido na parte superior da página, centralizado e em um plano de fundo azul marinho. Eu liberado da coluna esquerda e movidos conteúdo abaixo o `MainContent` controle ContentPlaceHolder, que agora se estende por toda a largura da página. Além disso, eu nixed a lista não ordenada de lições e substituído por uma lista horizontal acima `MainContent`. Atualizei também as fontes e cores usadas pela página mestre (e, por extensão, suas páginas de conteúdo). A Figura 4 mostra `Default.aspx` ao usar o `Alternate.master` página mestra.

> [!NOTE]
> O ASP.NET inclui a capacidade de definir *temas*. Um tema é uma coleção de imagens, arquivos CSS e configurações relacionadas a estilo Web controle propriedade que podem ser aplicadas a uma página em tempo de execução. Os temas são a melhor opção se os layouts do site diferem apenas em imagens exibidas e por suas regras CSS. Se os layouts diferem mais consideravelmente, como o uso de controles da Web diferentes ou tendo um layout radicalmente diferente, em seguida, você precisará usar páginas mestras separadas. Consulte a seção de leitura adicional no final deste tutorial para saber mais sobre temas.


[![Nossas páginas de conteúdo agora podem usar uma nova aparência](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**Figura 04**: nossas páginas de conteúdo agora podem usar uma nova aparência ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-vb/_static/image12.png))


Quando o mestre e marcação de páginas de conteúdo são combinadas, a `MasterPage` classe verificações para garantir que todo o conteúdo controle na página de conteúdo faz referência a um ContentPlaceHolder na página mestra. Uma exceção é gerada se um controle de conteúdo que faz referência a um ContentPlaceHolder não existente foi encontrado. Em outras palavras, é fundamental que a página mestra que está sendo atribuída à página de conteúdo tem um ContentPlaceHolder para cada controle na página de conteúdo de conteúdo.

O `Site.master` página mestra inclui quatro controles ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Algumas das páginas de conteúdo em nosso site incluem apenas um ou dois controles de conteúdo; outras incluem um controle de conteúdo para cada o ContentPlaceHolders disponíveis. Se nossa nova página mestra (`Alternate.master`) nunca pode ser atribuído a essas páginas de conteúdo que tem controles de conteúdo para todos os ContentPlaceHolders em `Site.master` , em seguida, é essencial que `Alternate.master` também incluem os mesmos controles ContentPlaceHolder como `Site.master`.

Para obter o `Alternate.master` página mestra para ser semelhante ao explorar (consulte a Figura 4), comece definindo estilos da página mestra a `AlternateStyles.css` folha de estilos. Adicionar as seguintes regras em `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

Em seguida, adicione a seguinte marcação declarativa para `Alternate.master`. Como você pode ver, `Alternate.master` contém quatro controles ContentPlaceHolder com o mesmo `ID` valores que os controles ContentPlaceHolder `Site.master`. Além disso, ele inclui um controle ScriptManager, que é necessário para as páginas de nosso site que usam a estrutura do ASP.NET AJAX.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Testar a nova página mestra

Para testar essa nova atualização de página mestra a `BasePage` da classe `OnPreInit` método para que o `MasterPageFile` é atribuído o valor da propriedade `"~/Alternate.maser"` e, em seguida, visite o site. Cada página deve funcionar sem erros, exceto dois: `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx`. Adicionar um produto a DetailsView em `~/Admin/AddProduct.aspx` resulta em uma `NullReferenceException` da linha de código que tenta definir a página mestra `GridMessageText` propriedade. Ao visitar `~/Admin/Products.aspx` um `InvalidCastException` é lançada no carregamento de página com a mensagem: "não é possível converter o objeto do tipo ' ASP.alternate\_mestre ' no tipo ' ASP.site\_mestre '."

Esses erros ocorrem porque o `Site.master` classe code-behind inclui eventos públicos, propriedades e métodos que não estão definidos na `Alternate.master`. A parte de marcação dessas duas páginas têm um `@MasterType` diretiva que faz referência a `Site.master` página mestra.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

Do Além disso, o DetailsView `ItemInserted` manipulador de eventos `~/Admin/AddProduct.aspx` inclui o código que converte o tipadas vagamente `Page.Master` propriedade para um objeto do tipo `Site`. O `@MasterType` diretiva (usada dessa maneira) e a conversão no `ItemInserted` manipulador de eventos agrupa rigidamente a `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` páginas para o `Site.master` página mestra.

Para interromper esse acoplamento forte, podemos ter `Site.master` e `Alternate.master` derivam de uma classe base comum que contém definições para os membros públicos. Depois disso, podemos atualizar o `@MasterType` diretiva para fazer referência a esse tipo de base comum.

### <a name="creating-a-custom-base-master-page-class"></a>Criando uma classe de página mestra de Base personalizada

Adicionar um novo arquivo de classe para o `App_Code` pasta denominada `BaseMasterPage.vb` que derivam de `System.Web.UI.MasterPage`. Precisamos definir o `RefreshRecentProductsGrid` método e o `GridMessageText` propriedade em `BaseMasterPage`, mas não podemos simplesmente mover eles de `Site.master` porque esses membros trabalham com os controles da Web que são específicos para o `Site.master` página mestra (o `RecentProducts` GridView e `GridMessage` rótulo).

O que precisamos fazer é configurar `BaseMasterPage` de forma que esses membros são definidos nele, mas, na verdade, são implementados por `BaseMasterPage`de classes derivadas (`Site.master` e `Alternate.master`). Esse tipo de herança é possível marcar a classe como `MustInherit` e seus membros como `MustOverride`. Em resumo, adicionar essas palavras-chave para a classe e seus dois membros anuncia que `BaseMasterPage` não implementado `RefreshRecentProductsGrid` e `GridMessageText`, mas que suas classes derivadas.

Também é necessário definir o `PricesDoubled` evento `BaseMasterPage` e fornecem um meio por classes derivadas para gerar o evento. O padrão usado no .NET Framework para facilitar esse comportamento é criar um evento público na classe base e adicionar um método protegido e substituível chamado `OnEventName`. Classes derivadas podem chamar este método para gerar o evento ou podem substituí-la para executar código imediatamente antes ou depois que o evento é gerado.

Atualização de seu `BaseMasterPage` classe para que ele contém o código a seguir:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

Em seguida, vá para o `Site.master` por trás do código de classe e a derivam `BaseMasterPage`. Porque `BaseMasterPage` contém membros marcados `MustOverride` é preciso substituir os membros em `Site.master`. Adicionar o `Overrides` palavra-chave para as definições de método e propriedade. Também atualizar o código que gera a `PricesDoubled` evento o `DoublePrice` do botão `Click` manipulador de eventos com uma chamada para a classe base `OnPricesDoubled` método.

Após essas modificações a `Site.master` classe code-behind deve conter o código a seguir:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

Também precisamos atualizar `Alternate.master`da classe code-behind derivar `BaseMasterPage` e substituir os dois `MustOverride` membros. Mas como `Alternate.master` não contém um GridView que lista os produtos mais recentes nem um rótulo que exibe uma mensagem depois de um novo produto é adicionado ao banco de dados, esses métodos não precisa fazer nada.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>Referência à classe de página mestra Base

Agora que já foram concluídos o `BaseMasterPage` classe e nossas duas páginas mestras estendê-lo, a etapa final é atualizar o `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` páginas para se referir a esse tipo comum. Inicie alterando a `@MasterType` diretiva em ambas as páginas de:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

Para:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

Em vez de fazer referência a um caminho de arquivo, o `@MasterType` propriedade agora referencia o tipo base (`BaseMasterPage`). Consequentemente, o tipo mais acentuado `Master` usada em classes de code-behind de ambas as páginas de propriedade agora é do tipo `BaseMasterPage` (em vez do tipo `Site`). Com essa alteração em vigor revisitar `~/Admin/Products.aspx`. Anteriormente, isso resultou em um erro de conversão porque a página está configurada para usar o `Alternate.master` página mestre, mas o `@MasterType` diretiva referenciada o `Site.master` arquivo. Mas, agora a página é processada sem erro. Isso ocorre porque o `Alternate.master` página mestra pode ser convertida em um objeto do tipo `BaseMasterPage` (desde que ele estende).

Há uma pequena alteração que precisa ser feita no `~/Admin/AddProduct.aspx`. O controle de DetailsView `ItemInserted` manipulador de eventos usa ambos o fortemente tipada `Master` propriedade e o tipadas vagamente `Page.Master` propriedade. Corrigimos a referência de tipo mais acentuado quando atualizamos o `@MasterType` diretiva, mas ainda precisa atualizar a referência tipadas vagamente. Substitua a linha de código a seguir:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

Com o seguinte, que converte `Page.Master` para o tipo de base:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Etapa 4: Determinar qual página mestra para vincular a páginas de conteúdo

Nosso `BasePage` classe atualmente define todas as páginas conteúdas `MasterPageFile` propriedades para um valor embutido no PreInit estágio do ciclo de vida da página. Podemos atualizar esse código para basear a página mestra em alguns fatores externos. Talvez a página mestra para carregar depende das preferências do usuário conectado no momento. Nesse caso, estamos precisa escrever código no `OnPreInit` método `BasePage` que procura as atualmente visitando preferências do usuário página mestra.

Vamos criar uma página da web que permite que o usuário escolha qual página mestra - `Site.master` ou `Alternate.master` - e salvar essa opção em uma variável de sessão. Comece criando uma nova página da web no diretório raiz chamado `ChooseMasterPage.aspx`. Ao criar esta página (ou qualquer outras conteúdas páginas daqui em diante) não é necessário para associá-lo a uma página mestra porque a página mestra está definida por meio de programação `BasePage`. No entanto, se você não associar a nova página a uma página mestre, em seguida, declarativo da nova página padrão contém um formulário da Web e outros tipos de conteúdo fornecido pela página mestre. Você precisará substituir manualmente esta marcação com controles de conteúdo apropriados. Por esse motivo, acho mais fácil associar a nova página ASP.NET para uma página mestra.

> [!NOTE]
> Porque `Site.master` e `Alternate.master` têm o mesmo conjunto de controles ContentPlaceHolder não importa qual página mestra que você escolhe ao criar a nova página de conteúdo. Para manter a consistência, sugiro usando `Site.master`.


[![Adicione uma nova página de conteúdo para o site](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**Figura 05**: adicionar uma nova página de conteúdo para o site ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-vb/_static/image15.png))


Atualização de `Web.sitemap` arquivo para incluir uma entrada para esta lição. Adicione a seguinte marcação sob o `<siteMapNode>` da lição páginas mestras e ASP.NET AJAX:


[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

Antes de adicionar qualquer conteúdo para o `ChooseMasterPage.aspx` página dedicar um tempo para atualizar a classe de code-behind da página para que deriva de `BasePage` (em vez de `System.Web.UI.Page`). Em seguida, adicionar um controle DropDownList para a página, defina seu `ID` propriedade `MasterPageChoice`, e adicionar dois ListItems com o `Text` valores de "~ / Site.master" e "~ / Alternate.master".

Adicionar um controle de botão Web para a página e definir seu `ID` e `Text` propriedades `SaveLayout` e "Salvar opções de Layout," respectivamente. Neste ponto declarativo da página deve ser semelhante ao seguinte:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

Quando a página for visitada primeiro, precisamos exibir a opção do usuário selecionado no momento de página mestra. Criar um `Page_Load` manipulador de eventos e adicione o seguinte código:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

O código acima é executado somente na primeira visita de página (e não em postagens subsequentes). Ele primeiro verifica se a variável de sessão `MyMasterPage` existe. Em caso afirmativo, ele tenta localizar o item de lista correspondente no `MasterPageChoice` DropDownList. Se um item de lista correspondente for encontrado, seu `Selected` está definida como `True`.

Também precisamos de código que salva a escolha do usuário para o `MyMasterPage` variável de sessão. Criar um manipulador de eventos para o `SaveLayout` do botão `Click` evento e adicione o seguinte código:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> No momento o `Click` manipulador de eventos é executado em um postback, a página mestra já foi selecionada. Portanto, seleção de lista suspensa do usuário não terá efeito até que a próxima página visitar. O `Response.Redirect` força o navegador a solicitar novamente `ChooseMasterPage.aspx`.


Com o `ChooseMasterPage.aspx` página conclua, nossa tarefa final é ter `BasePage` atribuir o `MasterPageFile` propriedade com base no valor da `MyMasterPage` variável de sessão. Se a variável de sessão não está definida `BasePage` padrão `Site.master`.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> Posso mover o código que atribui o `Page` do objeto `MasterPageFile` propriedade do `OnPreInit` manipulador de eventos e em dois métodos separados. Esse método primeiro, `SetMasterPageFile`, atribui o `MasterPageFile` propriedade para o valor retornado pelo método segundo, `GetMasterPageFileFromSession`. Marco a `SetMasterPageFile` método `Overridable` para que as futuras classes que estendem `BasePage` pode substituí-la para implementar a lógica personalizada, opcionalmente, se necessário. Vamos ver um exemplo de substituição `BasePage`do `SetMasterPageFile` propriedade no tutorial Avançar.


Com esse código, visite o `ChooseMasterPage.aspx` página. Inicialmente, o `Site.master` página mestra é selecionado (consulte a Figura 6), mas o usuário pode selecionar uma página mestra diferente na lista suspensa.


[![Páginas de conteúdo são exibidas usando a página mestra Site.master](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**Figura 06**: páginas de conteúdo são exibidos usando o `Site.master` página mestra ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-vb/_static/image18.png))


[![Páginas de conteúdo agora são exibidas usando a página mestra Alternate.master](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**Figura 07**: páginas de conteúdo são exibidos usando o `Alternate.master` página mestra ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-vb/_static/image21.png))


## <a name="summary"></a>Resumo

Quando uma página de conteúdo é visitada, seu controles de conteúdo é combinado com controles ContentPlaceHolder da sua página mestra. O conteúdo da página mestra é indicada pelo `Page` da classe `MasterPageFile` propriedade, que é atribuída para o `@Page` da diretiva `MasterPageFile` atributo durante a fase de inicialização. Como este tutorial mostrada, é possível atribuir um valor para o `MasterPageFile` propriedade desde que fazemos antes do final do estágio PreInit. Ser capaz de especificar de forma programática a página mestra abre a porta para cenários mais avançados, como associação dinamicamente uma página de conteúdo a uma página mestra com base em fatores externos.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Diagrama de ciclo de vida de página do ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Visão geral do ciclo de vida da página ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Visão geral sobre capas e temas do ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Páginas mestras: Dicas, truques e interceptações](http://www.odetocode.com/articles/450.aspx)
- [Temas do ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 3.5 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou em seu blog [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Suchi Banerjee. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha em[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](master-pages-and-asp-net-ajax-vb.md)
[Próximo](nested-master-pages-vb.md)
