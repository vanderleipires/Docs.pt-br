---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: "O modelo do ASP.NET 2.0 página | Microsoft Docs"
author: microsoft
description: "No ASP.NET 1. x, os desenvolvedores precisavam escolher entre um modelo de código em linha e um modelo de código code-behind. Por trás do código podem ser implementado usando ambos os attr Src..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: e008f197cf08bec81c560018f2d42306598f9e6d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="the-aspnet-20-page-model"></a>O modelo do ASP.NET 2.0 de página
====================
por [Microsoft](https://github.com/microsoft)

> No ASP.NET 1. x, os desenvolvedores precisavam escolher entre um modelo de código em linha e um modelo de código code-behind. Por trás do código podem ser implementado usando o atributo Src ou o atributo CodeBehind do @Page diretiva. No ASP.NET 2.0, os desenvolvedores ainda pode escolher entre o código embutido e code-behind, mas houve aprimoramentos significativos para o modelo de code-behind.


No ASP.NET 1. x, os desenvolvedores precisavam escolher entre um modelo de código em linha e um modelo de código code-behind. Por trás do código podem ser implementado usando o atributo Src ou o atributo CodeBehind do @Page diretiva. No ASP.NET 2.0, os desenvolvedores ainda pode escolher entre o código embutido e code-behind, mas houve aprimoramentos significativos para o modelo de code-behind.

## <a name="improvements-in-the-code-behind-model"></a>Melhorias no modelo de Code-Behind

Para compreender totalmente as alterações no modelo de code-behind no ASP.NET 2.0, o melhor desempenho para analisar rapidamente o modelo como ele existia no ASP.NET 1. x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>O modelo de Code-Behind no ASP.NET 1. x

No ASP.NET 1. x, o modelo de code-behind consistia em um arquivo ASPX (o formulário da Web) e um arquivo de code-behind que contém o código de programação. Os dois arquivos foram conectados usando o @Page diretiva no arquivo ASPX. Cada controle na página. ASPX possui uma declaração correspondente no arquivo code-behind como uma variável de instância. O arquivo code-behind também contivesse um código de associação de evento e gerou o código necessário para o designer do Visual Studio. Esse modelo funcionou muito bem, mas porque cada elemento ASP.NET na página ASPX necessário código correspondente no arquivo code-behind, não havia nenhum verdadeira separação de código e conteúdo. Por exemplo, se um designer adicionado um novo controle de servidor para um arquivo ASPX fora do IDE do Visual Studio, o aplicativo interrompe devido à ausência de uma declaração para o controle no arquivo code-behind.

## <a name="the-code-behind-model-in-aspnet-20"></a>O modelo de Code-Behind no ASP.NET 2.0

O ASP.NET 2.0 melhora significativamente esse modelo. No ASP.NET 2.0, por trás do código é implementado usando o novo *classes parciais* fornecido no ASP.NET 2.0. A classe code-behind no ASP.NET 2.0 está definidos como uma classe parcial, que significa que ele contém apenas uma parte da definição de classe. A parte restante da definição de classe é gerada dinamicamente pelo ASP.NET 2.0 usando a página ASPX em tempo de execução ou quando o site da Web é pré-compilado. O vínculo entre o arquivo code-behind e a página ASPX ainda será estabelecido usando a diretiva @ Page. No entanto, em vez de um atributo Src ou de code-behind, o ASP.NET 2.0 agora usa o atributo CodeFile. O atributo Inherits também é usado para especificar o nome da classe para a página.

Uma diretiva de página @ típica pode ter esta aparência:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Uma definição de classe comum em um arquivo de code-behind ASP.NET 2.0 pode ter esta aparência:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# e Visual Basic são as únicas linguagens gerenciadas que oferecem suporte a classes parciais no momento. Portanto, os desenvolvedores que usam j# não poderá usar o modelo de code-behind em ASP.NET 2.0.


O novo modelo aprimora o modelo de code-behind porque os desenvolvedores agora têm arquivos de código que contêm apenas o código que ele criou. Ele também fornece uma separação true de código e conteúdo porque não há nenhum declarações de variável de instância no arquivo code-behind.

> [!NOTE]
> Porque a classe parcial para a página ASPX é onde a associação de evento ocorre, os desenvolvedores do Visual Basic podem perceber um aumento pequeno de desempenho usando a palavra-chave Handles no code-behind para associar a eventos. C# não tem nenhuma palavra-chave equivalente.


## <a name="new--page-directive-attributes"></a>Novos atributos de diretiva de página @

O ASP.NET 2.0 adiciona muitos novos atributos para a diretiva @ Page. Os atributos a seguir são novos no ASP.NET 2.0.

## <a name="async"></a>Async

O atributo Async permite configurar página a ser executada de forma assíncrona. Também abrange páginas assíncronas neste módulo.

## <a name="asynctimeout"></a>AsyncTimeout

Especificar o tempo limite para as páginas assíncronas. O padrão é 45 segundos.

## <a name="codefile"></a>CodeFile

O atributo CodeFile é a substituição para o atributo CodeBehind no Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

O atributo CodeFileBaseClass é usado em casos onde você deseja várias páginas para derivar de uma única classe base. Devido a implementação de classes parciais no ASP.NET, sem esse atributo, de uma classe base que usa campos comuns compartilhados para fazer referência a controles declarados em uma página ASPX não funcionará corretamente porque ASP. Mecanismo de compilação de redes criará automaticamente os novos membros com base nos controles na página. Portanto, se você quiser uma classe base comum para duas ou mais páginas no ASP.NET, você precisará definir especifique sua classe base no atributo ' CodeFileBaseClass e, em seguida, derive cada classe de páginas de classe base. O atributo CodeFile também é necessário quando este atributo é usado.

## <a name="compilationmode"></a>compilationMode

Este atributo permite que você defina a propriedade CompilationMode da página ASPX. A propriedade CompilationMode é uma enumeração que contém os valores **sempre**, **automática**, e **nunca**. O padrão é **sempre**. O **automática** configuração impedirá ASP.NET dinamicamente a página de compilação se possível. Excluindo páginas de compilação dinâmica aumenta o desempenho. No entanto, se uma página que foi excluída contém código que deve ser compilado, um erro será gerado quando a página é acessada.

## <a name="enableeventvalidation"></a>EnableEventValidation

Esse atributo especifica se eventos de postback e retorno de chamada são validados. Quando habilitada, argumentos de postback ou de eventos de retorno de chamada são verificados para garantir que eles foram originados o controle de servidor que originalmente os processou.

## <a name="enabletheming"></a>EnableTheming

Esse atributo especifica se ou não os temas do ASP.NET são usados em uma página. O padrão é **false**. Temas do ASP.NET são abordados em [módulo 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Esse atributo especifica se pragmas de linha devem ser adicionados durante a compilação. Pragmas de linha são opções usadas pelo depuradores para marcar seções específicas de código.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Esse atributo especifica se JavaScript é injetado para a página para manter a posição de rolagem entre postbacks. Esse atributo é **false** por padrão.

Quando esse atributo é **true**, ASP.NET adicionará um &lt;script&gt; bloco em um postback tem esta aparência:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Observe que a origem para este bloco de script é WebResource. Este recurso não é um caminho físico. Quando esse script é solicitado, o ASP.NET cria dinamicamente o script.

### <a name="masterpagefile"></a>MasterPageFile

Esse atributo especifica o arquivo de página mestra para a página atual. O caminho pode ser relativo ou absoluto. Páginas mestras são abordadas em [módulo 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Este atributo permite que você substitua a propriedades de aparência da interface do usuário definidas por um tema ASP.NET 2.0. Temas são abordados em [módulo 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>{1&gt;Tema&lt;1}

Especifica o tema da página. Se um valor não for especificado para o atributo StyleSheetTheme, o atributo tema substitui todos os estilos aplicados aos controles na página.

## <a name="title"></a>Título

Define o título da página. O valor especificado aqui aparecerão no &lt;título&gt; elemento da página renderizada.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Define o valor de enumeração ViewStateEncryptionMode. Os valores disponíveis são **sempre**, **automática**, e **nunca**. O valor padrão é **automática**. Quando esse atributo é definido como um valor de **automática**, viewstate está criptografado é um controle solicitá-lo chamando o **RegisterRequiresViewStateEncryption** método.

## <a name="setting-public-property-values-via-the--page-directive"></a>Definir valores de propriedade pública via @ diretiva de página

Outro novo recurso da diretiva @ Page no ASP.NET 2.0 é a capacidade de definir o valor inicial de propriedades públicas de uma classe base. Suponha que, por exemplo, você tem uma propriedade pública denominada **SomeText** em sua classe base e você gostaria que ele seja inicializada para **Hello** quando uma página for carregada. Você pode fazer isso definindo o valor simplesmente na diretiva de página @ da seguinte forma:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

O **SomeText** atributo da diretiva @ Page define o valor inicial da propriedade SomeText na classe base para *Olá!*. O vídeo abaixo está um passo a passo de configuração do valor inicial de uma propriedade pública em uma classe base usando a diretiva @ Page.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Abra vídeo de tela inteira](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Novas propriedades públicas da classe de página

As propriedades públicas a seguir são novas no ASP.NET 2.0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Retorna o caminho relativo de aplicativo para a página ou controle. Por exemplo, para uma página localizada em http://app/folder/page.aspx, a propriedade retorna ~ / pasta.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Retorna o caminho relativo para a página ou controle. Por exemplo para uma página localizada em http://app/folder/page.aspx, a propriedade retorna ~ / folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Obtém ou define o limite usado para manipulação de página assíncrona. (Páginas assíncronas serão abordadas neste módulo.)

## <a name="clientquerystring"></a>ClientQueryString

Uma propriedade somente leitura que retorna a parte da cadeia de caracteres de consulta da URL solicitada. Esse valor é codificada de URL. Você pode usar o método UrlDecode da classe HttpServerUtility para decodificá-lo.

## <a name="clientscript"></a>ClientScript

Essa propriedade retorna um objeto de ClientScriptManager que pode ser usado para gerenciar a emissão de ASP.NETs de script do lado do cliente. (A classe ClientScriptManager é abordada neste módulo).

## <a name="enableeventvalidation"></a>EnableEventValidation

Essa propriedade controla se validação do evento está habilitada para eventos de postback e retorno de chamada. Quando habilitada, os argumentos de postback ou de eventos de retorno de chamada são verificados para garantir que eles foram originados o controle de servidor que originalmente os processou.

## <a name="enabletheming"></a>EnableTheming

Essa propriedade obtém ou define um valor booleano que especifica se ou não um tema ASP.NET 2.0 aplica-se à página.

## <a name="form"></a>Formulário

Essa propriedade retorna o formulário HTML na página ASPX como um objeto HtmlForm.

## <a name="header"></a>Cabeçalho

Essa propriedade retorna uma referência a um objeto de HtmlHead que contém o cabeçalho da página. Você pode usar o objeto retornado de HtmlHead para get/set folhas de estilo, marcas Meta, etc.

## <a name="idseparator"></a>IdSeparator

Essa propriedade somente leitura obtém o caractere que é usado para separar os identificadores de controle quando o ASP.NET está construindo uma ID exclusiva para controles em uma página. Ele não se destina a ser usado diretamente no seu código.

## <a name="isasync"></a>É assíncrono

Essa propriedade permite páginas assíncronas. Páginas assíncronas são discutidas neste módulo.

## <a name="iscallback"></a>IsCallback

Essa propriedade somente leitura retorna **true** se a página é o resultado de um retorno de chamada. Retornos de chamada são discutidos neste módulo.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Essa propriedade somente leitura retorna **true** se a página for parte de uma postagem entre páginas. Várias páginas são abordadas neste módulo.

## <a name="items"></a>Itens

Retorna uma referência a uma instância de IDictionary que contém todos os objetos armazenados no contexto de páginas. Você pode adicionar itens a esse objeto IDictionary e eles estarão disponíveis para você durante o tempo de vida do contexto.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Essa propriedade controla se o ASP.NET emite JavaScript que mantém que as páginas de posição de rolagem no navegador após a ocorrência de um postback. (Detalhes dessa propriedade foram discutidos neste módulo).

## <a name="master"></a>mestre

Essa propriedade somente leitura retorna uma referência à instância do MasterPage para uma página para a qual uma página mestra foi aplicada.

## <a name="masterpagefile"></a>MasterPageFile

Obtém ou define o nome do arquivo de página mestra para a página. Essa propriedade só pode ser definida no método PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Esta propriedade obtém ou define o comprimento máximo para o estado de páginas em bytes. Se a propriedade é definida como um número positivo, o estado de exibição de páginas será ser dividido em vários campos ocultos para que ele não excede o número de bytes especificado. Se a propriedade for um número negativo, o estado de exibição não será dividido em partes.

## <a name="pageadapter"></a>PageAdapter

Retorna uma referência ao objeto PageAdapter que modifica a página para o navegador solicitante.

## <a name="previouspage"></a>PreviousPage

Retorna uma referência à página anterior em caso de uma transferência de servidor ou um postback da página.

## <a name="skinid"></a>SkinID

Especifica a capa do ASP.NET 2.0 para aplicar à página.

## <a name="stylesheettheme"></a>StyleSheetTheme

Essa propriedade obtém ou define a folha de estilo que é aplicada a uma página.

## <a name="templatecontrol"></a>TemplateControl

Retorna uma referência para o controle recipiente para a página.

## <a name="theme"></a>{1&gt;Tema&lt;1}

Obtém ou define o nome do tema ASP.NET 2.0 aplicado à página. Esse valor deve ser definido antes do método PreInit.

## <a name="title"></a>Título

Essa propriedade obtém ou define o título da página como obtido do cabeçalho de páginas.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Obtém ou define o ViewStateEncryptionMode da página. Consulte uma discussão detalhada sobre essa propriedade neste módulo.

## <a name="new-protected-properties-of-the-page-class"></a>Novas propriedades protegidas de classe de página

A seguir estão as novas propriedades protegidas da classe de página no ASP.NET 2.0.

## <a name="adapter"></a>Adaptador

Retorna uma referência para o ControlAdapter que renderiza a página no dispositivo que solicitou que ela.

## <a name="asyncmode"></a>AsyncMode

Essa propriedade indica se a página é processada de forma assíncrona. Ele é destinado ao uso pelo tempo de execução e não diretamente no código.

## <a name="clientidseparator"></a>ClientIDSeparator

Essa propriedade retorna o caractere usado como separador ao criar IDs de cliente exclusivos para controles. Ele é destinado ao uso pelo tempo de execução e não diretamente no código.

## <a name="pagestatepersister"></a>PageStatePersister

Essa propriedade retorna o objeto PageStatePersister para a página. Essa propriedade é usada principalmente por desenvolvedores de controle do ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Essa propriedade retorna um suffic exclusivo que é acrescentado ao caminho do arquivo de cache de navegadores. O valor padrão é \_ \_ufps = e um número de 6 dígitos.

## <a name="new-public-methods-for-the-page-class"></a>Novos métodos públicos da classe de página

Os métodos públicos a seguir são novos para a classe de página no ASP.NET 2.0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Este método registra delegados de manipulador de eventos para execução da página assíncrona. Páginas assíncronas são discutidas neste módulo.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Aplica-se as propriedades em uma folha de estilos de páginas para a página.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Seres esse método uma tarefa assíncrona.

### <a name="getvalidators"></a>GetValidators

Retorna uma coleção de validadores para o grupo de validação especificada ou o grupo de validação padrão se nenhum for especificado.

## <a name="registerasynctask"></a>RegisterAsyncTask

Este método registra uma nova tarefa assíncrona. Páginas assíncronas são abordadas neste módulo.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Esse método informa ao ASP.NET que o estado de controle de páginas deve ser persistente.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Esse método informa ao ASP.NET que o viewstate páginas requer criptografia.

## <a name="resolveclienturl"></a>ResolveClientUrl

Retorna uma URL relativa que pode ser usada para solicitações de cliente para imagens, etc.

## <a name="setfocus"></a>SetFocus

Esse método definirá o foco para o controle que é especificado quando a página for carregada inicialmente.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Esse método irá cancelar o registro de controle que é passado como não há mais a necessidade de persistência de estado do controle.

## <a name="changes-to-the-page-lifecycle"></a>Alterações para o ciclo de vida da página

O ciclo de vida da página no ASP.NET 2.0 não mudou significativamente, mas há alguns novos métodos que você deve estar atento. O ciclo de vida de página do ASP.NET 2.0 é descrito a seguir.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (novo no ASP.NET 2.0)

O evento PreInit é o mais antigo estágio do ciclo de vida que um desenvolvedor pode acessar. A adição desse evento torna possível alterar temas ASP.NET 2.0, páginas mestras programaticamente, acessar as propriedades de um perfil do ASP.NET 2.0, etc. Se você estiver em um estado de postback, importante perceber Viewstate ainda não foram aplicada aos controles neste ponto do ciclo de vida. Portanto, se um desenvolvedor de uma propriedade de um controle é alterado neste estágio, ele provavelmente será substituído posteriormente no ciclo de vida de páginas.

## <a name="init"></a>Init

O evento Init não mudou de ASP.NET 1. x. Isso é onde você deseja ler ou inicializar propriedades de controles na página. Neste estágio, páginas mestras, temas, etc. já estiverem aplicados para a página.

## <a name="initcomplete-new-in-20"></a>InitComplete (novo no 2.0)

O evento InitComplete é chamado no final do estágio de inicialização de páginas. Neste ponto do ciclo de vida, você pode acessar controles na página, mas seu estado ainda não foi populado.

## <a name="preload-new-in-20"></a>Pré-carregamento (nova no 2.0)

Este evento é chamado após ter sido aplicada a todos os dados de postagem e apenas antes da página\_carga.

## <a name="load"></a>Carregamento

O evento de carregamento não foi alterado de ASP.NET 1. x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (novo no 2.0)

O evento LoadComplete é o último evento no estágio de carregamento de páginas. Neste estágio, todos os dados de postagem e viewstate foi aplicado à página.

## <a name="prerender"></a>PreRender

Se desejar que o viewstate ser mantidas corretamente para controles que são adicionados dinamicamente para a página do evento PreRender é a última oportunidade para adicioná-los.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (novo no 2.0)

No estágio PreRenderComplete, todos os controles foram adicionados para a página e a página está pronta para ser processado. O evento PreRenderComplete é o último evento gerado antes do viewstate de páginas é salvo.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (novo no 2.0)

O evento SaveStateComplete é chamado imediatamente depois que todos os estados de viewstate e controle de página foi salvo. Este é o último evento antes da página, na verdade, é renderizada no navegador.

## <a name="render"></a>Renderizar

O método de processamento não foi alterado desde o ASP.NET 1. x. Isso é onde o HtmlTextWriter é inicializado e a página é renderizada no navegador.

## <a name="cross-page-postback-in-aspnet-20"></a>Postback da página no ASP.NET 2.0

No ASP.NET 1. x, postbacks precisavam post para a mesma página. Várias páginas não eram permitidas. O ASP.NET 2.0 adiciona a capacidade de volta para uma página diferente por meio da interface IButtonControl. Qualquer controle que implementa a interface IButtonControl novo (botão, LinkButton e ImageButton além de controles personalizados de terceiros) pode aproveitar essa nova funcionalidade com o uso do atributo PostBackUrl. O código a seguir mostra um controle de botão que envia de volta para uma segunda página.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Quando a página é enviada de volta, a página que inicia a postagem é acessível por meio da propriedade PreviousPage na segunda página. Essa funcionalidade é implementada por meio do novo formulário da Web\_DoPostBackWithOptions função de cliente que o ASP.NET 2.0 processa para a página quando um controle envia de volta para uma página diferente. Esta função JavaScript é fornecida pelo manipulador WebResource novo que emite o script para o cliente.

O vídeo abaixo está um passo a passo de um postback da página.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Abra vídeo de tela inteira](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Para obter mais detalhes em várias páginas

### <a name="viewstate"></a>ViewState

Você pode ter solicitado por conta própria já sobre o que acontece com o viewstate desde a primeira página em um cenário de postback da página. Afinal, qualquer controle que não implementam IPostBackDataHandler persistirá seu estado por meio de viewstate, então para ter acesso às propriedades de controle na segunda página de uma postagem entre páginas, você deve ter acesso para o viewstate para a página. O ASP.NET 2.0 cuida desse cenário usando um novo campo oculto na segunda página chamado \_ \_PREVIOUSPAGE. O \_ \_campo de formulário PREVIOUSPAGE contém o viewstate para a primeira página para que você possa ter acesso às propriedades de todos os controles na segunda página.

### <a name="circumventing-findcontrol"></a>Evitar FindControl

Vídeo passo a passo de uma postagem entre páginas, usei o método FindControl para obter uma referência para o controle de caixa de texto na primeira página. Esse método funciona bem para essa finalidade, mas FindControl é caro e precisar gravar código adicional. Felizmente, o ASP.NET 2.0 fornece uma alternativa para FindControl para essa finalidade que funcionarão em vários cenários. A diretiva PreviousPageType permite que você tenha uma referência fortemente tipada para a página anterior usando o atributo VirtualPath ou o TypeName. O atributo TypeName permite que você especifique o tipo de página anterior, enquanto o atributo VirtualPath permite que você se refira à página anterior usando um caminho virtual. Depois de configurar a diretiva PreviousPageType, em seguida, você deve expor a controles, etc. para o qual você deseja permitir o acesso usando as propriedades públicas.

## <a name="lab-1-cross-page-postback"></a>Postagem entre páginas de laboratório 1

Neste laboratório, você criará um aplicativo que usa a nova funcionalidade de postagem entre páginas do ASP.NET 2.0.

1. Abra o Visual Studio 2005 e crie um novo site da Web do ASP.NET.
2. Adicione um novo formulário da Web chamado Page2. aspx.
3. Abra o Default.aspx no modo Design e adicione um controle de botão e um controle de caixa de texto. 

    1. Dê o controle de botão de uma ID de **SubmitButton** e a caixa de texto de uma ID de controle **nome de usuário**.
    2. Defina a propriedade de PostBackUrl do botão como Page2. aspx.
4. Abra Page2. aspx no modo de exibição de origem.
5. Adicione uma diretiva @ PreviousPageType conforme mostrado abaixo:
6. Adicione o seguinte código para a página\_carga do code-behind do Page2. aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Compile o projeto clicando na compilação no menu compilar.
8. Adicione o seguinte código para code-behind para Default.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Altere a página\_Load em Page2. aspx ao seguinte: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Compile o projeto.
11. Execute o projeto.
12. Digite seu nome na caixa de texto e clique no botão.
13. O que é o resultado?

## <a name="asynchronous-pages-in-aspnet-20"></a>Páginas assíncronas no ASP.NET 2.0

Muitos problemas de contenção no ASP.NET são causados por latência de chamadas externas (como chamadas de banco de dados ou serviço da Web), latência de e/s de arquivo, etc. Quando uma solicitação é feita em relação a um aplicativo ASP.NET, o ASP.NET usa um dos seus threads de trabalho para que a solicitação de serviço. Essa solicitação é responsável por esse thread até que a solicitação é concluída e a resposta foi enviada. ASP.NET 2.0 tenta resolver problemas de latência com esses tipos de problemas, adicionando a capacidade de executar as páginas de forma assíncrona. Isso significa que um thread de trabalho pode iniciar a solicitação e, em seguida, entregar execução adicionais para outro thread, retornando, assim, ao pool de threads disponíveis rapidamente. Quando tiver concluído a e/s de arquivo, chamada de banco de dados, etc., um novo segmento é obtido do pool de threads para concluir a solicitação.

É a primeira etapa para fazer com que uma página executadas de forma assíncrona definir o **Async** atributo da diretiva de página da seguinte forma:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Esse atributo diz ao ASP.NET para implementar IHttpAsyncHandler para a página.

A próxima etapa é chamar o método AddOnPreRenderCompleteAsync em um ponto no ciclo de vida da página antes de PreRender. (Normalmente, esse método é chamado na página\_carga.) O método AddOnPreRenderCompleteAsync utiliza dois parâmetros; um BeginEventHandler e um EndEventHandler. O BeginEventHandler retorna um IAsyncResult, que é então passado como um parâmetro para o EndEventHandler.

O vídeo abaixo está um passo a passo de uma solicitação de página assíncrona.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Abra vídeo de tela inteira](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> Uma página assíncrona não processa para o navegador até que o EndEventHandler seja concluída. Sem dúvida, mas que alguns desenvolvedores serão considerado de solicitações assíncronas como sendo similares para retornos de chamada assíncrono. É importante observar que não são. O benefício para solicitações assíncronas é que o primeiro thread de trabalho pode ser retornado ao pool de threads para atender novas solicitações, reduzindo a contenção por estarem associados de e/s, etc.


## <a name="script-callbacks-in-aspnet-20"></a>Retornos de chamada de script no ASP.NET 2.0

Os desenvolvedores da Web sempre procuramos por maneiras de evitar a oscilação associada a um retorno de chamada. No ASP.NET 1. x, SmartNavigation foi o método mais comum para evitar a cintilação, mas SmartNavigation causou problemas para alguns desenvolvedores devido à complexidade de sua implementação no cliente. O ASP.NET 2.0 resolve esse problema com retornos de chamada de script. Retornos de chamada de script utilizam XMLHttp para fazer solicitações ao servidor Web por meio de JavaScript. A solicitação XMLHttp retorna dados XML que, em seguida, podem ser manipulados por meio de DOM. do navegador Código de XMLHttp é ocultado do usuário pelo novo manipulador WebResource.

Há várias etapas que são necessárias para configurar um retorno de chamada de script no ASP.NET 2.0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Etapa 1: Implementar a Interface ICallbackEventHandler

Em ordem para o ASP.NET reconhece sua página como participando de um retorno de chamada de script, você deve implementar a interface ICallbackEventHandler. Você pode fazer isso no seu arquivo code-behind da seguinte forma:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Você também pode fazer isso usando o tipo de diretiva @ implementa assim:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Normalmente você usaria a diretiva @ implementa ao usar o código ASP.NET embutido.

## <a name="step-2--call-getcallbackeventreference"></a>Etapa 2: Chamada GetCallbackEventReference

Conforme mencionado anteriormente, a chamada de XMLHttp é encapsulada no manipulador WebResource. Quando a página é processada, o ASP.NET irá adicionar uma chamada para o formulário da Web\_DoCallback, um script de cliente que é fornecido pelo WebResource. O formulário da Web\_DoCallback função substitui o \_ \_doPostBack função de retorno de chamada. Lembre-se de que \_ \_doPostBack programaticamente envia o formulário na página. Em um cenário de retorno de chamada, você deseja impedir que um postback, portanto \_ \_doPostBack não será suficiente.

> [!NOTE]
> \_\_doPostBack ainda é processado para a página em um cenário de retorno de chamada de script de cliente. No entanto, ele não é usado para o retorno de chamada.


Os argumentos para o formulário da Web\_DoCallback função do lado do cliente são fornecidos por meio da função de servidor GetCallbackEventReference que normalmente poderia ser chamado na página\_carga. Uma chamada típica para GetCallbackEventReference pode ter esta aparência:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> Nesse caso, cm é uma instância de ClientScriptManager. A classe ClientScriptManager será abordada neste módulo.


Há várias versões sobrecarregadas GetCallbackEventReference. Nesse caso, os argumentos são da seguinte maneira:

`this`

Uma referência para o controle onde GetCallbackEventReference está sendo chamado. Nesse caso, é a página propriamente dita.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Um argumento de cadeia de caracteres que será passado do código do lado do cliente para o evento do lado do servidor. Nesse caso, passando o valor de uma lista suspensa de Im chamado ddlCompany.

`ShowCompanyName`

O nome da função do lado do cliente que aceita o valor de retorno (como cadeia de caracteres) a partir do evento de retorno de chamada do lado do servidor. Essa função será chamada apenas quando o retorno de chamada do lado do servidor foi bem-sucedida. Portanto, para manter a eficiência, geralmente é recomendável usar a versão sobrecarregada de GetCallbackEventReference que usa um argumento de cadeia de caracteres adicionais especificando o nome de uma função de cliente para executar em caso de erro.

`null`

Uma cadeia de caracteres que representa uma função de cliente que iniciou antes do retorno de chamada para o servidor. Nesse caso, não há nenhum desses scripts, para que o argumento for nulo.

`true`

Um booliano que especifica se deve ou não realizar o retorno de chamada de forma assíncrona.

A chamada para o formulário da Web\_DoCallback no cliente passará esses argumentos. Portanto, quando esta página é renderizada no cliente, esse código parecerá da seguinte forma:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Observe que a assinatura de função no cliente é um pouco diferente. A função do lado do cliente passa 5 cadeias de caracteres e um valor booleano. A cadeia de caracteres adicional (que é nula no exemplo acima) contém a função do lado do cliente que vai manipular os erros de retorno de chamada do lado do servidor.

## <a name="step-3--hook-the-client-side-control-event"></a>Etapa 3: Conectar-se com o evento de controle do lado do cliente

Observe que o valor de retorno de GetCallbackEventReference acima foi atribuído a uma variável de cadeia de caracteres. Essa cadeia de caracteres é usada para conectar-se com um evento no lado do cliente para o controle que inicia o retorno de chamada. Neste exemplo, o retorno de chamada é iniciado por uma lista suspensa na página, portanto, gostaria de gancho de *OnChange* eventos.

Para conectar o evento no lado do cliente, basta adicione um manipulador para a marcação do lado do cliente da seguinte maneira:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Lembre-se de que *cbRef* é o valor de retorno da chamada para GetCallbackEventReference. Ele contém a chamada para o formulário da Web\_DoCallback foi mostrada acima.

## <a name="step-4--register-the-client-side-script"></a>Etapa 4: Registrar o Script do lado do cliente

Lembre-se que a chamada para GetCallbackEventReference especificado que um script de cliente chamado **ShowCompanyName** seria executado quando o retorno de chamada do lado do servidor é bem-sucedida. Esse script deve ser adicionado à página usando uma instância ClientScriptManager. (A classe ClientScriptManager será dicussed neste módulo). Você pode fazer que like assim:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Etapa 5: Chamar os métodos da Interface ICallbackEventHandler

O ICallbackEventHandler contém dois métodos que você precisa implementar em seu código. Eles são **RaiseCallbackEvent** e **GetCallbackEvent**.

**RaiseCallbackEvent** usa uma cadeia de caracteres como um argumento e retorna nada. O argumento de cadeia de caracteres é passado da chamada do lado do cliente para o formulário da Web\_DoCallback. Nesse caso, esse valor é o *valor* atributo da lista suspensa chamado ddlCompany. O código do lado do servidor deve ser colocado no método RaiseCallbackEvent. Por exemplo, se o retorno de chamada está fazendo uma WebRequest em relação a um recurso externo, esse código deve ser colocado no RaiseCallbackEvent.

**GetCallbackEvent** é responsável por processar o retorno da chamada de retorno ao cliente. Ele não utiliza argumentos e retorna uma cadeia de caracteres. Retorna a cadeia de caracteres será passada como um argumento para a função do lado do cliente, nesse caso *ShowCompanyName*.

Depois de concluir as etapas acima, você está pronto para executar um retorno de chamada de script no ASP.NET 2.0.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Abra vídeo de tela inteira](the-asp-net-2-0-page-model/_static/callback1.wmv)


Retornos de chamada de script no ASP.NET têm suporte em qualquer navegador que dá suporte a chamadas de XMLHttp fazer. Que inclui todos os navegadores modernos em uso atualmente. Internet Explorer usa o objeto XMLHttp ActiveX enquanto outros navegadores modernos (incluindo o IE 7 futuras) usam um objeto intrínseco do XMLHttp. Para determinar programaticamente se um navegador dá suporte a retornos de chamada, você pode usar o **Request.Browser.SupportCallback** propriedade. Esta propriedade retornará **true** se o cliente solicitante dá suporte a retornos de chamada de script.

## <a name="working-with-client-script-in-aspnet-20"></a>Trabalhando com Script de cliente no ASP.NET 2.0

Scripts de cliente no ASP.NET 2.0 são gerenciados com o uso da classe ClientScriptManager. A classe ClientScriptManager controla de scripts de cliente usando um tipo e um nome. Isso impede que o mesmo script programaticamente inseridos em uma página mais de uma vez.

> [!NOTE]
> Depois que um script tiver sido registrado com êxito em uma página, qualquer tentativa subsequente para registrar o mesmo script simplesmente resultará no script não está sendo registrado uma segunda vez. Nenhum script duplicado é adicionada e nenhuma exceção ocorrer. Para evitar o desnecessária computação, há métodos que você pode usar para determinar se um script já está registrado para que você não tente registrá-lo mais de uma vez.


Os métodos de ClientScriptManager devem estar familiarizados com todos os desenvolvedores ASP.NET atuais:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Este método adiciona um script para a parte superior da página renderizada. Isso é útil para a adição de funções que serão chamadas explicitamente no cliente.

Há duas versões sobrecarregadas desse método. Três dos quatro argumentos são comuns entre eles. Elas são:

`type (string)`

O ***tipo*** argumento identifica um tipo para o script. Geralmente é uma boa ideia usar tipo a página do (isso. GetType()) para o tipo.

`key (string)`

O ***chave*** argumento é uma chave definida pelo usuário para o script. Isso deve ser exclusivo para cada script. Se você tentar adicionar um script com a mesma chave e tipo de um script já adicionado, ele não será adicionado.

`script (string)`

O ***script*** argumento é uma cadeia de caracteres que contém o script real para adicionar. É recomendável que você use StringBuilder para criar o script e, em seguida, use o método ToString () na StringBuilder para atribuir o ***script*** argumento.

Se você usar o RegisterClientScriptBlock sobrecarregado que só usa três argumentos, você deve incluir elementos de script (&lt;script&gt; e &lt;/script&gt;) em seu script.

Você pode optar por usar a sobrecarga do RegisterClientScriptBlock que usa um argumento quarto. O quarto argumento é um valor booleano que especifica se o ASP.NET deve adicionar elementos de script para você. Se esse argumento for **true**, o script não deve incluir os elementos de script explicitamente.

Use o método IsClientScriptBlockRegistered para determinar se um script já foi registrado. Isso permite que você evite tentar registrar novamente um script que já foi registrado.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (novo no 2.0)

A marca RegisterClientScriptInclude cria um bloco de script que vincula a um arquivo de script externo. Ele tem duas sobrecargas. Um usa uma chave e uma URL. A segunda adiciona um terceiro argumento de especificação do tipo.

Por exemplo, o código a seguir gera um bloco de script que vincula a jsfunctions.js na raiz da pasta de scripts do aplicativo:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Esse código gera o código a seguir na página renderizada:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> O bloco de script é renderizado na parte inferior da página.


Use o método IsClientScriptIncludeRegistered para determinar se um script já foi registrado. Isso permite que você evite tentar registrar novamente um script.

## <a name="registerstartupscript"></a>RegisterStartupScript

O método de RegisterStartupScript usa os mesmos argumentos que o método RegisterClientScriptBlock. Um script registrado com RegisterStartupScript executa depois que a página for carregada, mas antes do evento OnLoad do lado do cliente. 1. x, scripts registrados com RegisterStartupScript foram colocados antes do fechamento &lt;/formulário&gt; enquanto scripts registrados com RegisterClientScriptBlock foram feitos imediatamente após a abertura da tag &lt;formulário&gt; marca. No ASP.NET 2.0, ambos são colocadas imediatamente antes do fechamento &lt;/formulário&gt; marca.

> [!NOTE]
> Se você registrar uma função com RegisterStartupScript, essa função não será executado até que você chamá-lo explicitamente no código do lado do cliente.


Use o método IsStartupScriptRegistered para determinar se um script já foi registrado e evitar uma tentativa de registrar novamente um script.

## <a name="other-clientscriptmanager-methods"></a>Outros métodos de ClientScriptManager

Aqui estão alguns dos outros métodos úteis da classe ClientScriptManager.

| **GetCallbackEventReference** | Consulte os retornos de chamada de script neste módulo. |
| --- | --- |
| **GetPostBackClientHyperlink** | Obtém uma referência de JavaScript (javascript:&lt;chamar&gt;) que pode ser usado para lançar novamente de um evento no lado do cliente. |
| **GetPostBackEventReference** | Obtém uma cadeia de caracteres que pode ser usada para iniciar uma postagem de volta do cliente. |
| **GetWebResourceUrl** | Retorna uma URL para um recurso que é inserido em um assembly. Deve ser usado em conjunto com **RegisterClientScriptResource**. |
| **RegisterClientScriptResource** | Registra um recurso da Web com a página. Estes são os recursos incorporados em um assembly e manipuladas pelo novo manipulador WebResource. |
| **RegisterHiddenField** | Registra um campo de formulário oculto com a página. |
| **RegisterOnSubmitStatement** | Registra o código do lado do cliente que é executado quando o formulário HTML é enviado. |
