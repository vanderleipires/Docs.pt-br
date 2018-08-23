---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
title: Especificando a página mestra programaticamente (c#) | Microsoft Docs
author: rick-anderson
description: Examina a definir o conteúdo da página mestra programaticamente por meio do manipulador de eventos PreInit.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 7c4a3445-2440-4aee-b9fd-779c05e6abb2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
msc.type: authoredcontent
ms.openlocfilehash: 58ecd01a8a18cd7dcf9eba96313e40d881d90af5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830848"
---
<a name="specifying-the-master-page-programmatically-c"></a>Especificando a página mestra programaticamente (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.pdf)

> Examina a definir o conteúdo da página mestra programaticamente por meio do manipulador de eventos PreInit.


## <a name="introduction"></a>Introdução

Desde o Exemplo inaugural [ *criando um Layout de todo o Site usando páginas mestras*](creating-a-site-wide-layout-using-master-pages-cs.md), todo o conteúdo páginas referenciou sua página mestra declarativamente por meio o `MasterPageFile` atributo no `@Page`diretiva. Por exemplo, a seguinte `@Page` diretiva vincula a página de conteúdo para a página mestra `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample1.aspx)]

O [ `Page` classe](https://msdn.microsoft.com/library/system.web.ui.page.aspx) no `System.Web.UI` namespace inclui uma [ `MasterPageFile` propriedade](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) que retorna o caminho para o conteúdo da página mestra; é essa propriedade definida pelo `@Page` diretiva. Essa propriedade também pode ser usada para especificar de forma programática o conteúdo da página mestra. Essa abordagem é útil se você quiser atribuir dinamicamente a página mestra com base em fatores externos, como o usuário visitar a página.

Neste tutorial adicionamos uma segunda página mestra ao nosso site e dinamicamente decidir qual página mestre a ser usada em tempo de execução.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Etapa 1: Examinar o ciclo de vida da página

Sempre que uma solicitação chega ao servidor web para uma página ASP.NET que é uma página de conteúdo, o mecanismo do ASP.NET deve fuse da página de conteúdo de controles ContentPlaceHolder correspondente do controles na página mestra. Este fusion cria uma hierarquia de controle único pode, em seguida, percorrer o ciclo de vida da página típico.

Figura 1 ilustra essa fusion. Etapa 1 na Figura 1 mostra o conteúdo inicial e a hierarquias de controle de página mestra. No final de parte final do estágio PreInit o conteúdo de controles da página são adicionados ao ContentPlaceHolders correspondente na página mestra (etapa 2). Após essa fusion, a página mestra serve como a raiz da hierarquia de controle de adição múltipla. Isso fundida controle hierarquia, em seguida, é adicionada à página para produzir a hierarquia de controle finalizado (etapa 3). O resultado líquido é que a hierarquia de controle da página inclui a hierarquia de controle de adição múltipla.


[![A página mestra e hierarquias de controle da página de conteúdo são combinados juntos durante o estágio de PreInit](specifying-the-master-page-programmatically-cs/_static/image2.png)](specifying-the-master-page-programmatically-cs/_static/image1.png)

**Figura 01**: hierarquias de controle de página mestra e a página de conteúdo são combinados juntos durante o estágio de PreInit ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Etapa 2: Definindo o`MasterPageFile`propriedade do código

Qual página mestre partakes nesse fusion depende do valor da `Page` do objeto `MasterPageFile` propriedade. Definindo o `MasterPageFile` de atributo no `@Page` diretiva tem o efeito líquido de atribuir o `Page`do `MasterPageFile` propriedade durante o estágio de inicialização, que é o primeiro estágio do ciclo de vida da página. Como alternativa, podemos pode definir essa propriedade programaticamente. No entanto, é imperativo que essa propriedade ser definida antes que a fusão na Figura 1 ocorra.

No início do estágio PreInit a `Page` objeto gera sua [ `PreInit` evento](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) e chama seu [ `OnPreInit` método](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Para definir a página mestra programaticamente, em seguida, podemos pode criar um manipulador de eventos para o `PreInit` evento ou substituir o `OnPreInit` método. Vamos examinar as duas abordagens.

Comece abrindo `Default.aspx.cs`, o arquivo de classe code-behind para a home page do nosso site. Adicionar um manipulador de eventos para a página `PreInit` evento digitando o seguinte código:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample2.cs)]

Aqui podemos definir o `MasterPageFile` propriedade. Atualize o código para que ele atribui o valor "~ / Master" para o `MasterPageFile` propriedade.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample3.cs)]

Se você definir um ponto de interrupção e iniciar com depuração, você verá que sempre que o `Default.aspx` página for visitada ou sempre que houver um postback para essa página, o `Page_PreInit` executa o manipulador de eventos e o `MasterPageFile` propriedade é atribuída a "~ / Master".

Como alternativa, você pode substituir a `Page` da classe `OnPreInit` método e defina o `MasterPageFile` propriedade existe. Neste exemplo, vamos não definir a página mestra em uma página específica, mas ao invés de `BasePage`. Lembre-se de que criamos uma classe de página de base personalizada (`BasePage`) volta a [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial. No momento `BasePage` substitui o `Page` da classe `OnLoadComplete` método, em que ele define a página `Title` propriedade com base nos dados de mapa do site. Vamos atualizar `BasePage` também substituir o `OnPreInit` método para especificar de forma programática a página mestra.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample4.cs)]

Como todas as nossas páginas de conteúdo derivam `BasePage`, todos eles agora tem a sua página mestra programaticamente atribuída. Neste momento a `PreInit` manipulador de eventos no `Default.aspx.cs` é supérfluo; fique à vontade para removê-lo.

### <a name="what-about-thepagedirective"></a>E quanto a`@Page`diretiva?

O que pode ser um pouco confuso é que o conteúdo das páginas `MasterPageFile` propriedades agora estão sendo especificadas em dois lugares: por meio de programação na `BasePage` da classe `OnPreInit` método, bem como por meio o `MasterPageFile` atributo em cada página de conteúdo `@Page` diretiva.

O primeiro estágio do ciclo de vida da página é o estágio de inicialização. Durante esse estágio a `Page` do objeto `MasterPageFile` é atribuído o valor da propriedade de `MasterPageFile` atributo no `@Page` diretiva (se ele for fornecido). O estágio de PreInit segue o estágio de inicialização, e é aqui onde podemos definir programaticamente a `Page` do objeto `MasterPageFile` , substituindo o valor atribuído da propriedade a `@Page` diretiva. Porque estamos definindo a `Page` do objeto `MasterPageFile` propriedade por meio de programação, podemos pode remover o `MasterPageFile` de atributos da `@Page` diretiva sem afetar a experiência do usuário final. Para convencido de que isso, vá em frente e remova os `MasterPageFile` de atributos do `@Page` diretiva `Default.aspx` e, em seguida, visite a página por meio de um navegador. Como você poderia esperar, a saída é o mesmo que antes do atributo foi removido.

Se o `MasterPageFile` propriedade é definida por meio de `@Page` diretiva ou por meio de programação é irrelevante para a experiência do usuário final. No entanto, o `MasterPageFile` de atributo no `@Page` diretiva é usada pelo Visual Studio durante o tempo de design para produzir o WYSIWYG exibição no Designer. Se você retornar ao `Default.aspx` no Visual Studio e navegue para o Designer, você verá a mensagem "Erro de página mestra: A página tem controles que exigem uma referência de página mestra, mas não for especificado nenhum" (veja a Figura 2).

Em resumo, você deve deixar o `MasterPageFile` de atributo no `@Page` diretiva para aproveitar uma experiência avançada de tempo de design no Visual Studio.


[![Visual Studio usa o @Page atributo de MasterPageFile da diretiva para renderizar a exibição de Design](specifying-the-master-page-programmatically-cs/_static/image5.png)](specifying-the-master-page-programmatically-cs/_static/image4.png)

**Figura 02**: Visual Studio usa o `@Page` da diretiva `MasterPageFile` o modo de exibição de Design de atributo para renderização ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>Etapa 3: Criando uma página mestra alternativa

Porque um conteúdo da página mestra pode ser definida por meio de programação em tempo de execução é possível carregar dinamicamente uma página mestra específica com base em alguns critérios externos. Essa funcionalidade pode ser útil em situações onde layout do site precisará variam com base no usuário. Por exemplo, um aplicativo web do mecanismo de blog pode permitir que seus usuários a escolher um layout para o seu blog, onde cada layout é associada uma página mestre diferente. Em tempo de execução, quando um visitante está exibindo o blog de um usuário, o aplicativo web precisa determinar o layout do blog e associar dinamicamente a página mestra correspondente com a página de conteúdo.

Vamos examinar como carregar dinamicamente uma página mestra em tempo de execução com base em alguns critérios externos. Nosso site no momento, contém apenas uma página mestra (`Site.master`). Precisamos de outra página mestra para ilustrar a escolha de uma página mestra em tempo de execução. Esta etapa se concentra em criar e configurar a nova página mestra. Etapa 4 examina determinando qual página mestre para usar em tempo de execução.

Criar uma nova página mestre na pasta raiz chamada `Alternate.master`. Também adicione uma nova folha de estilo para o site da Web denominado `AlternateStyles.css`.


[![Adicione outro arquivo de página mestra e CSS para o site](specifying-the-master-page-programmatically-cs/_static/image8.png)](specifying-the-master-page-programmatically-cs/_static/image7.png)

**Figura 03**: adicionar outra página mestra e arquivo CSS para o site ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image9.png))


Eu criei o `Alternate.master` página mestra para o título exibido na parte superior da página, centralizada e em um plano de fundo azul marinho. Eu liberadas da coluna esquerda e movido abaixo que o conteúdo a `MainContent` controle ContentPlaceHolder, que agora se estende por toda a largura da página. Além disso, eu nixed a lista não ordenada de lições e substituímos por uma lista horizontal acima `MainContent`. Atualizei também as fontes e cores usadas pela página mestre (e, por extensão, suas páginas de conteúdo). A Figura 4 mostra `Default.aspx` ao usar o `Alternate.master` página mestra.

> [!NOTE]
> O ASP.NET inclui a capacidade de definir *temas*. Um tema é uma coleção de imagens, arquivos CSS e relacionadas a estilo Web controle configurações de propriedade que podem ser aplicadas a uma página em tempo de execução. Os temas são a melhor opção se os layouts do seu site diferem apenas em imagens exibidas e por suas regras CSS. Se os layouts mais substancialmente, diferem como o uso de controles da Web diferentes ou com um layout radicalmente diferente, em seguida, você precisará usar separado de páginas mestras. Consulte a seção leitura adicional no final deste tutorial para obter mais informações sobre temas.


[![Nossas páginas de conteúdo agora podem usar uma nova aparência](specifying-the-master-page-programmatically-cs/_static/image11.png)](specifying-the-master-page-programmatically-cs/_static/image10.png)

**Figura 04**: nossas páginas de conteúdo agora podem usar uma nova aparência ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image12.png))


Quando o mestre e a marcação de páginas de conteúdo são combinados, o `MasterPage` classe verificações para garantir que o conteúdo de cada controle na página de conteúdo faz referência a um ContentPlaceHolder na página mestra. Uma exceção é lançada se um controle de conteúdo que faz referência a um ContentPlaceHolder inexistente for encontrado. Em outras palavras, é imperativo que a página mestra que está sendo atribuída à página de conteúdo tenha um ContentPlaceHolder para cada controle na página de conteúdo de conteúdo.

O `Site.master` página mestra inclui quatro controles ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Algumas das páginas de conteúdo no nosso site incluem apenas um ou dois controles de conteúdo; outras incluem um controle de conteúdo para cada um do ContentPlaceHolders disponíveis. Se nossa nova página mestra (`Alternate.master`) nunca pode ser atribuído a essas páginas de conteúdo que tem controles de conteúdo para todos os ContentPlaceHolders na `Site.master` e em seguida, é essencial que `Alternate.master` também incluem os mesmos controles de ContentPlaceHolder `Site.master`.

Para obter sua `Alternate.master` página mestra para ser semelhante ao explorar (veja a Figura 4), comece definindo estilos da página mestra no `AlternateStyles.css` folha de estilos. Adicione as seguintes regras em `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-cs/samples/sample5.css)]

Em seguida, adicione a seguinte marcação declarativa para `Alternate.master`. Como você pode ver, `Alternate.master` contém quatro controles ContentPlaceHolder com o mesmo `ID` valores como controles ContentPlaceHolder na `Site.master`. Além disso, ele inclui um controle ScriptManager, que é necessário para as páginas que usam a estrutura ASP.NET AJAX em nosso site.


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Teste a nova página mestra

Para testar essa nova atualização de página mestra a `BasePage` da classe `OnPreInit` método para que o `MasterPageFile` é atribuído o valor da propriedade "~ / Alternate.master" e, em seguida, visite o site. Todas as páginas devem funcionar sem erros, exceto dois: `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx`. Adição de um produto a DetailsView na `~/Admin/AddProduct.aspx` resulta em uma `NullReferenceException` da linha de código que tenta definir a página mestra `GridMessageText` propriedade. Ao visitar `~/Admin/Products.aspx` uma `InvalidCastException` é gerada no carregamento da página com a mensagem: "não é possível converter o objeto do tipo ' ASP.alternate\_mestre ' para o tipo ' ASP.site\_mestre '."

Esses erros acontecem porque o `Site.master` classe code-behind inclui eventos públicos, propriedades e métodos que não estão definidos na `Alternate.master`. A parte de marcação dessas duas páginas têm um `@MasterType` diretiva que faz referência a `Site.master` página mestra.


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample7.aspx)]

Além disso, ovládacího prvku DetailsView `ItemInserted` manipulador de eventos no `~/Admin/AddProduct.aspx` inclui o código que projeta a tipagem `Page.Master` propriedade para um objeto do tipo `Site`. O `@MasterType` diretiva (usado dessa forma) e a conversão em de `ItemInserted` acople de manipulador de eventos a `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` páginas para o `Site.master` página mestra.

Para interromper esse acoplamento rígido, podemos ter `Site.master` e `Alternate.master` derivam de uma classe base comum que contém definições para os membros públicos. Depois disso, podemos atualizar o `@MasterType` diretiva para fazer referência a esse tipo de base comum.

### <a name="creating-a-custom-base-master-page-class"></a>Criando uma classe de página mestra de Base personalizada

Adicione um novo arquivo de classe para o `App_Code` pasta chamada `BaseMasterPage.cs` que derivam de `System.Web.UI.MasterPage`. Precisamos definir a `RefreshRecentProductsGrid` método e o `GridMessageText` propriedade na `BaseMasterPage`, mas estamos simplesmente não é possível movê-los lá de `Site.master` porque esses membros trabalham com controles de Web que são específicos para o `Site.master` página mestra (o `RecentProducts` GridView e `GridMessage` rótulo).

O que precisamos fazer é configurar `BaseMasterPage` de tal forma que esses membros são definidos por lá, mas, na verdade, são implementados por `BaseMasterPage`de classes derivadas (`Site.master` e `Alternate.master`). Esse tipo de herança é possível marcar a classe e seus membros como `abstract`. Em resumo, adicionando a `abstract` palavra-chave para esses dois membros anuncia que `BaseMasterPage` ainda não implementado `RefreshRecentProductsGrid` e `GridMessageText`, mas que serão de suas classes derivadas.

Também precisamos definir a `PricesDoubled` evento no `BaseMasterPage` e fornecem um meio por classes derivadas para gerar o evento. O padrão usado no .NET Framework para facilitar esse comportamento é criar um evento público na classe base e adicionar um protegido `virtual` método chamado `OnEventName`. As classes derivadas, em seguida, podem chamar esse método para gerar o evento ou podem substituí-la para executar código imediatamente antes ou depois que o evento é gerado.

Atualização de seu `BaseMasterPage` de classe para que ele contenha o código a seguir:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample8.cs)]

Em seguida, vá para o `Site.master` de lógica de classe e fazer com que ele derivam `BaseMasterPage`. Porque `BaseMasterPage` está `abstract` precisamos substituem aqueles `abstract` membros em `Site.master`. Adicionar o `override` palavra-chave para as definições de método e propriedade. Também atualizar o código que gera o `PricesDoubled` evento na `DoublePrice` do botão `Click` manipulador de eventos com uma chamada para a classe base `OnPricesDoubled` método.

Após essas modificações a `Site.master` classe code-behind deve conter o código a seguir:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample9.cs)]

Também precisamos atualizar `Alternate.master`da classe code-behind derivar `BaseMasterPage` e substitua os dois `abstract` membros. Mas, como `Alternate.master` não contém um GridView que lista os produtos mais recentes, nem um rótulo que exibe uma mensagem depois de um novo produto é adicionado ao banco de dados, esses métodos não precisam fazer nada.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample10.cs)]

### <a name="referencing-the-base-master-page-class"></a>Referência à classe de página mestre da Base de dados de

Agora que concluímos a `BaseMasterPage` da classe e ter nossas duas páginas mestras estendê-lo, a etapa final é atualizar o `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` páginas para se referir a esse tipo comum. Comece alterando o `@MasterType` diretiva em ambas as páginas de:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample11.aspx)]

Para:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample12.aspx)]

Em vez de referenciar um caminho de arquivo, o `@MasterType` propriedade agora referencia o tipo base (`BaseMasterPage`). Consequentemente, o tipo mais acentuado `Master` usada em classes de code-behind das duas páginas de propriedade agora é do tipo `BaseMasterPage` (em vez do tipo `Site`). Com essa alteração em vigor revisitar `~/Admin/Products.aspx`. Anteriormente, isso resultou em um erro de conversão porque a página está configurada para usar o `Alternate.master` página mestra, mas o `@MasterType` diretiva referenciada a `Site.master` arquivo. Mas agora a página é renderizada sem erro. Isso ocorre porque o `Alternate.master` página mestra pode ser convertida em um objeto do tipo `BaseMasterPage` (já que ele estende-o).

Há uma pequena alteração que precisa ser feita no `~/Admin/AddProduct.aspx`. O controle de DetailsView `ItemInserted` manipulador de eventos usa ambos os fortemente tipado `Master` propriedade e a tipagem `Page.Master` propriedade. Corrigimos a referência fortemente tipada quando atualizamos a `@MasterType` diretiva, mas estamos ainda precisará atualizar a referência fracamente tipada. Substitua a linha de código a seguir:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample13.cs)]

Com o seguinte, que converte `Page.Master` para o tipo de base:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample14.cs)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Etapa 4: Determinar qual página mestra para vincular a páginas de conteúdo

Nossos `BasePage` classe atualmente define todas as páginas conteúdas `MasterPageFile` propriedades para um valor embutido em código no estágio do ciclo de vida de página PreInit. Podemos atualizar esse código para basear a página mestra em alguns fatores externos. Talvez a página mestra para carregar depende das preferências do usuário conectado no momento. Nesse caso, seria preciso escrever o código na `OnPreInit` método no `BasePage` que procura as preferências de página mestra do usuário no momento, visitando.

Vamos criar uma página da web que permite que o usuário escolha qual página mestre usar - `Site.master` ou `Alternate.master` - e salvar essa opção em uma variável de sessão. Comece criando uma nova página da web no diretório raiz chamado `ChooseMasterPage.aspx`. Ao criar essa página (ou quaisquer outras páginas de conteúdo daqui em diante) que você não precisa vinculá-la a uma página mestra, porque a página mestra está definida por meio de programação `BasePage`. No entanto, se você não associar a nova página a uma página mestra, em seguida, marcação declarativa de padrão da nova página contém um formulário da Web e outros tipos de conteúdo fornecido pela página mestre. Você precisará substituir manualmente essa marcação com os controles de conteúdo apropriados. Por esse motivo, acho mais fácil associar a nova página ASP.NET para uma página mestra.

> [!NOTE]
> Porque `Site.master` e `Alternate.master` têm o mesmo conjunto de controles ContentPlaceHolder não importa o que você escolher ao criar a nova página de conteúdo de página mestra. Para manter a consistência, eu sugeriria usando `Site.master`.


[![Adicione uma nova página de conteúdo para o site](specifying-the-master-page-programmatically-cs/_static/image14.png)](specifying-the-master-page-programmatically-cs/_static/image13.png)

**Figura 05**: Adicione uma nova página de conteúdo para o site ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image15.png))


Atualização de `Web.sitemap` arquivo para incluir uma entrada para esta lição. Adicione a seguinte marcação abaixo o `<siteMapNode>` da lição páginas mestras e AJAX ASP.NET:


[!code-xml[Main](specifying-the-master-page-programmatically-cs/samples/sample15.xml)]

Antes de adicionar qualquer conteúdo para o `ChooseMasterPage.aspx` página Reserve um tempo para atualizar a classe de code-behind da página, de modo que ele deriva `BasePage` (em vez de `System.Web.UI.Page`). Em seguida, adicione um controle DropDownList para a página, defina suas `ID` propriedade para `MasterPageChoice`, e adicione dois ListItems com o `Text` valores de "~ / Master" e "~ / Alternate.master".

Adicione um controle da Web de botão para a página e defina suas `ID` e `Text` propriedades a serem `SaveLayout` e "Salvar Layout escolha", respectivamente. Neste ponto marcação declarativa de sua página deve ser semelhante ao seguinte:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample16.aspx)]

Quando a página é visitada primeiro é necessário exibir a opção do usuário selecionado no momento de página mestra. Criar um `Page_Load` manipulador de eventos e adicione o seguinte código:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample17.cs)]

O código acima é executado somente na primeira visita de página (e não em postbacks subsequentes). Ele primeiro verifica para ver se a variável de sessão `MyMasterPage` existe. Em caso afirmativo, ele tenta localizar o item de lista correspondente no `MasterPageChoice` DropDownList. Se um item de lista correspondente for encontrado, sua `Selected` estiver definida como `true`.

Também precisamos de código que salva a escolha do usuário para o `MyMasterPage` variável de sessão. Crie um manipulador de eventos para o `SaveLayout` do botão `Click` eventos e adicione o seguinte código:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample18.cs)]

> [!NOTE]
> No momento o `Click` manipulador de eventos é executado em um postback, a página mestra já foi selecionada. Portanto, a seleção do usuário lista suspensa não entrarão em vigor até que a próxima página visitar. O `Response.Redirect` força o navegador para solicitar novamente `ChooseMasterPage.aspx`.


Com o `ChooseMasterPage.aspx` página completa, nossa tarefa final é ter `BasePage` atribuir a `MasterPageFile` propriedade com base no valor da `MyMasterPage` variável de sessão. Se não for definida a variável de sessão têm `BasePage` padrão para `Site.master`.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample19.cs)]

> [!NOTE]
> Eu Movi o código que atribui a `Page` do objeto `MasterPageFile` propriedade do `OnPreInit` manipulador de eventos e em dois métodos separados. Esse método primeiro, `SetMasterPageFile`, atribui o `MasterPageFile` propriedade com o valor retornado pelo método segundo, `GetMasterPageFileFromSession`. Eu fiz a `SetMasterPageFile` método `virtual` para que as futuras classes que estendem `BasePage` pode substituí-la para implementar a lógica personalizada, opcionalmente, se necessário. Veremos um exemplo de substituição `BasePage`do `SetMasterPageFile` propriedade no próximo tutorial.


Com esse código, visite o `ChooseMasterPage.aspx` página. Inicialmente, o `Site.master` página mestra é selecionado (consulte a Figura 6), mas o usuário pode selecionar uma página mestra diferente na lista suspensa.


[![Páginas de conteúdo são exibidas usando a página mestra do site](specifying-the-master-page-programmatically-cs/_static/image17.png)](specifying-the-master-page-programmatically-cs/_static/image16.png)

**Figura 06**: as páginas de conteúdo são exibidos usando o `Site.master` página mestra ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image18.png))


[![Páginas de conteúdo agora são exibidas usando a página mestra Alternate.master](specifying-the-master-page-programmatically-cs/_static/image20.png)](specifying-the-master-page-programmatically-cs/_static/image19.png)

**Figura 07**: as páginas de conteúdo são agora exibidos usando o `Alternate.master` página mestra ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image21.png))


## <a name="summary"></a>Resumo

Quando uma página de conteúdo é visitada, seus controles de conteúdo são combinados com controles de ContentPlaceHolder da sua página mestra. O conteúdo da página mestra é indicada pela `Page` da classe `MasterPageFile` propriedade, que é atribuída para o `@Page` da diretiva `MasterPageFile` atributo durante o estágio de inicialização. Como este tutorial mostrado, podemos atribuir um valor para o `MasterPageFile` , desde que fazemos antes do final do estágio PreInit da propriedade. Ser capaz de especificar de forma programática a página mestra abre as portas para cenários mais avançados, como associação dinâmica de uma página de conteúdo a uma página mestra com base em fatores externos.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Diagrama de ciclo de vida de página do ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Visão geral do ciclo de vida de página ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Visão geral de capas e temas do ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Páginas mestras: Dicas, truques e armadilhas](http://www.odetocode.com/articles/450.aspx)
- [Temas do ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 3.5 in 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Suchi Banerjee. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, escreva-me em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-pages-and-asp-net-ajax-cs.md)
> [Próximo](nested-master-pages-cs.md)
