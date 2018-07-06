---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-vb
title: Autorização baseada em usuário (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, estamos examinará limitando o acesso a páginas e restringindo a funcionalidade de nível de página por meio de uma variedade de técnicas.
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: bc937e9d-5c14-4fc4-aec7-440da924dd18
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 107983494350ddc06b6d3a20557baff4f4e6f9f4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834368"
---
<a name="user-based-authorization-vb"></a>Autorização baseada em usuário (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_vb.pdf)

> Neste tutorial, estamos examinará limitando o acesso a páginas e restringindo a funcionalidade de nível de página por meio de uma variedade de técnicas.


## <a name="introduction"></a>Introdução

A maioria dos aplicativos web que oferecem as contas de usuário faça isso em parte para restringir determinados visitantes de acessar determinadas páginas dentro do site. Em sites messageboard mais online, por exemplo, todos os usuários - anônimos e autenticados - são capazes de exibir postagens do messageboard, mas somente os usuários autenticados podem visitar a página da web para criar uma nova postagem. E pode haver páginas administrativas que são acessíveis apenas a um usuário específico (ou um conjunto específico de usuários). Além disso, a funcionalidade de nível de página pode diferir em uma base por usuário. Ao exibir uma lista de postagens, usuários autenticados são mostrados uma interface para a classificação de cada postagem, enquanto que essa interface não está disponível para visitantes anônimos.

ASP.NET torna fácil definir regras de autorização baseada em usuário. Com apenas um pouco de marcação em `Web.config`, páginas da web específicas ou diretórios inteiros podem ser bloqueados para que eles só são acessíveis a um subconjunto de usuários especificado. Funcionalidade de nível de página pode ser ativada ou desativado com base no usuário conectado no momento por meio de programação declarativo e.

Neste tutorial, estamos examinará limitando o acesso a páginas e restringindo a funcionalidade de nível de página por meio de uma variedade de técnicas. Vamos começar!

## <a name="a-look-at-the-url-authorization-workflow"></a>Vejamos o fluxo de trabalho de autorização de URL

Conforme discutido na [ *visão geral de uma de autenticação de formulários* ](../introduction/an-overview-of-forms-authentication-vb.md) tutorial, quando o tempo de execução do ASP.NET processa uma solicitação para um recurso do ASP.NET a solicitação gera um número de eventos durante seu ciclo de vida. *Módulos HTTP* são classes gerenciadas cujo código é executado em resposta a um evento específico no ciclo de vida de solicitação. ASP.NET é fornecido com um número de módulos HTTP que realizar tarefas essenciais em segundo plano.

É um módulo desse tipo HTTP [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Conforme discutido nos tutoriais anteriores, a função primária do `FormsAuthenticationModule` é determinar a identidade da solicitação atual. Isso é feito ao inspecionar o tíquete de autenticação de formulários, que está localizado em um cookie ou embutido na URL. Essa identificação ocorre durante o [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Outro módulo HTTP importante é a [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), que é gerado em resposta ao [ `AuthorizeRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (que ocorre após o `AuthenticateRequest` evento). O `UrlAuthorizationModule` examina a marcação de configuração no `Web.config` para determinar se a identidade atual tem autoridade para visitar a página especificada. Esse processo é conhecido como *autorização de URL*.

Vamos examinar a sintaxe para as regras de autorização de URL na etapa 1, mas primeiro vamos examinar o que o `UrlAuthorizationModule` faz, dependendo se a solicitação está autorizada ou não. Se o `UrlAuthorizationModule` determina que a solicitação é autorizada, em seguida, ele não faz nada, e a solicitação continuará pelo seu ciclo de vida. No entanto, se a solicitação for *não* autorizado, o `UrlAuthorizationModule` anula o ciclo de vida e instrui o `Response` objeto para retornar uma [HTTP 401 não autorizado](http://www.checkupdown.com/status/E401.html) status. Ao usar a autenticação de formulários esse status HTTP 401 nunca é retornada ao cliente, porque se a `FormsAuthenticationModule` detecta um 401 HTTP status é modifica-o para um [redirecionamento HTTP 302](http://www.checkupdown.com/status/E302.html) à página de logon.

A Figura 1 ilustra o fluxo de trabalho de pipeline do ASP.NET, o `FormsAuthenticationModule`e o `UrlAuthorizationModule` quando chega uma solicitação não autorizada. Em particular, a Figura 1 mostra uma solicitação por um visitante anônimo para `ProtectedPage.aspx`, que é uma página que nega o acesso a usuários anônimos. Uma vez que o visitante é anônimo, o `UrlAuthorizationModule` anula a solicitação e retorna um status HTTP 401 não autorizado. O `FormsAuthenticationModule` , em seguida, converte o status de 401 em um redirecionamento 302 para página de logon. Depois que o usuário é autenticado por meio da página de logon, ele é redirecionado para `ProtectedPage.aspx`. Desta vez o `FormsAuthenticationModule` identifica o usuário com base em seu tíquete de autenticação. Agora que o visitante é autenticado, o `UrlAuthorizationModule` permite o acesso à página.


[![A autenticação de formulários e fluxo de trabalho de autorização de URL](user-based-authorization-vb/_static/image2.png)](user-based-authorization-vb/_static/image1.png)

**Figura 1**: A autenticação de formulários e fluxo de trabalho de autorização de URL ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image3.png))


Figura 1 ilustra a interação que ocorre quando um visitante anônimo tenta acessar um recurso que não está disponível para usuários anônimos. Nesse caso, o visitante anônimo é redirecionado à página de logon com a página que ela tentou visitar especificado na cadeia de consulta. Depois que o usuário efetuou logon com êxito, ela será redirecionada automaticamente para o recurso que ela foi inicialmente tentando exibir.

Quando a solicitação não autorizada é feita por um usuário anônimo, esse fluxo de trabalho é simples e é mais fácil para o visitante a entender o que aconteceu e por quê. Mas tenha em mente que o `FormsAuthenticationModule` redirecionará *qualquer* não autorizado de usuário para a página de logon, mesmo se a solicitação é feita por um usuário autenticado. Isso pode resultar em uma experiência confusa do usuário se um usuário autenticado tenta visitar uma página para o qual ela não tem autoridade.

Imagine que o nosso site teve suas regras de autorização de URL configuradas, de modo que a página ASP.NET `OnlyTito.aspx` era acessível apenas Tito. Agora, imagine que Sam visita o site, faz logon e, em seguida, tenta visitar o `OnlyTito.aspx`. O `UrlAuthorizationModule` irá interromper o ciclo de vida de solicitação e retornar um status HTTP 401 não autorizado, que o `FormsAuthenticationModule` detectará e, em seguida, redirecionar Sam para a página de logon. Uma vez que o Sam já tiver conectado, no entanto, ela deve estar imaginando por que ela foi enviada para a página de logon. Ela pode motivo que suas credenciais de logon foram perdidas alguma forma ou que ela inseriu credenciais inválidas. Se suas credenciais na página de logon torna a entrar nele Sam ela será conectada (novamente) e redirecionada para `OnlyTito.aspx`. O `UrlAuthorizationModule` detectará que o Sam não é possível visitar esta página e ela será retornada para a página de logon.

Figura 2 ilustra esse fluxo de trabalho confuso.


[![O fluxo de trabalho padrão pode levar a um ciclo confuso](user-based-authorization-vb/_static/image5.png)](user-based-authorization-vb/_static/image4.png)

**Figura 2**: O padrão fluxo de trabalho pode levar a um ciclo confuso ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image6.png))


O fluxo de trabalho ilustrado na Figura 2 pode rapidamente befuddle até mesmo o maioria dos computador experientes visitante. Vamos examinar formas de evitar isso confuso ciclo na etapa 2.

> [!NOTE]
> O ASP.NET usa dois mecanismos para determinar se o usuário atual pode acessar uma página da web específico: autorização de URL e a autorização de arquivo. Autorização de arquivo é implementada pela [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), que determina a autoridade consultando as ACLs de arquivo (s) solicitado. Autorização de arquivo é mais comumente usada com a autenticação do Windows porque as ACLs são as permissões que se aplicam a contas do Windows. Ao usar a autenticação de formulários, todas as solicitações de nível de sistema de arquivo e sistema operacional são executadas da mesma conta do Windows, independentemente do usuário visitar o site. Uma vez que esta série de tutoriais se concentra na autenticação de formulários, não abordaremos autorização de arquivo.


### <a name="the-scope-of-url-authorization"></a>O escopo de autorização de URL

O `UrlAuthorizationModule` é código gerenciado que faz parte do tempo de execução do ASP.NET. Antes da versão 7 da Microsoft [serviços de informações da Internet (IIS)](https://www.iis.net/) servidor web, não havia uma barreira distinta entre o pipeline HTTP do IIS e o pipeline do tempo de execução ASP.NET. Em outras palavras, no IIS 6 e versões anteriores, ASP. Do NET `UrlAuthorizationModule` é executada apenas quando uma solicitação é delegada do IIS no tempo de execução do ASP.NET. Por padrão, o IIS processa o conteúdo estático em si – como páginas HTML e CSS, JavaScript e arquivos de imagem – e transmite apenas as solicitações para o tempo de execução do ASP.NET quando uma página com a extensão `.aspx`, `.asmx`, ou `.ashx` é solicitada.

IIS 7, no entanto, permite integrado do IIS e ASP.NET pipelines. Com alguns parâmetros de configuração, você pode configurar o IIS 7 para invocar o `UrlAuthorizationModule` para *todos os* solicitações, o que significa que as regras de autorização de URL podem ser definidas para arquivos de qualquer tipo. Além disso, o IIS 7 inclui seu próprio mecanismo de autorização de URL. Para obter mais informações sobre a integração do ASP.NET e a funcionalidade de autorização de URL nativa do IIS 7, consulte [Noções básicas de autorização de URL de IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Para obter uma visão mais detalhada sobre a integração do ASP.NET e IIS 7, adquira um exemplar do livro de Shahram Khosravi, *Professional IIS 7 e programação integrado do ASP.NET* (ISBN: 978 0470152539).

Em poucas palavras, em versões anteriores do IIS 7, as regras de autorização de URL são aplicadas apenas para recursos controlados pelo runtime do ASP.NET. Mas com o IIS 7, é possível usar o recurso de autorização de URL nativo do IIS ou a integrar o ASP. Do NET `UrlAuthorizationModule` ao pipeline de HTTP do IIS, estendendo, assim, essa funcionalidade a todas as solicitações.

> [!NOTE]
> Há algumas diferenças sutis, porém importantes de ASP. Do NET `UrlAuthorizationModule` e recurso de autorização de URL do IIS 7 processar as regras de autorização. Este tutorial examina a funcionalidade de autorização de URL do IIS 7 ou as diferenças em como ele analisa as regras de autorização em comparação comparadas o `UrlAuthorizationModule`. Para obter mais informações sobre esses tópicos, consulte a documentação do IIS 7 no MSDN ou no [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Etapa 1: Definindo regras de autorização de URL no`Web.config`

O `UrlAuthorizationModule` determina se é necessário conceder ou negar acesso a um recurso solicitado para uma identidade específica com base nas regras de autorização de URL definidas na configuração do aplicativo. As regras de autorização são indicadas claramente na [ `<authorization>` elemento](https://msdn.microsoft.com/library/8d82143t.aspx) na forma de `<allow>` e `<deny>` elementos filho. Cada `<allow>` e `<deny>` elemento filho pode especificar:

- Um usuário específico
- Uma lista delimitada por vírgulas de usuários
- Todos os usuários anônimos, indicados por um ponto de interrogação (?)
- Todos os usuários, indicados por um asterisco (\*)

A marcação a seguir ilustra como usar as regras de autorização de URL para permitir que os usuários Tito e Scott e negar todos os outros:

[!code-xml[Main](user-based-authorization-vb/samples/sample1.xml)]

O `<allow>` elemento define quais usuários têm permissão - Tito e Scott - enquanto a `<deny>` instrui o elemento que *todos os* usuários serão negados.

> [!NOTE]
> O `<allow>` e `<deny>` elementos também podem especificar regras de autorização para funções. Vamos examinar a autorização baseada em função em um tutorial futuro.


A configuração a seguir concede acesso a qualquer pessoa que não seja o Sam (incluindo visitantes anônimos):

[!code-xml[Main](user-based-authorization-vb/samples/sample2.xml)]

Para permitir que somente usuários autenticados, use a configuração a seguir, que nega o acesso a todos os usuários anônimos:

[!code-xml[Main](user-based-authorization-vb/samples/sample3.xml)]

As regras de autorização são definidas dentro de `<system.web>` elemento no `Web.config` e se aplicam a todos os recursos do ASP.NET no aplicativo web. Muitas vezes, um aplicativo tem regras de autorização diferentes para diferentes seções. Por exemplo, em um site de comércio eletrônico, todos os visitantes podem examinar os produtos, consulte as análises de produtos, pesquisar no catálogo e assim por diante. No entanto, somente usuários autenticados podem alcançar o check-out ou as páginas para gerenciar o histórico de envio de um. Além disso, pode haver partes do site que só são acessíveis por selecionar usuários, como os administradores de sites.

ASP.NET torna fácil definir regras de autorização diferentes para diferentes arquivos e pastas no site. As regras de autorização especificadas na pasta raiz `Web.config` arquivo se aplicam a todos os recursos do ASP.NET no site. No entanto, essas configurações de autorização padrão podem ser substituídas por uma determinada pasta com a adição de um `Web.config` com um `<authorization>` seção.

Vamos atualizar nosso site para que somente usuários autenticados podem visitar as páginas do ASP.NET no `Membership` pasta. Para fazer isso, precisamos adicionar um `Web.config` o arquivo para o `Membership` pasta e definir suas configurações de autorização para negar a usuários anônimos. Clique com botão direito do `Membership` pasta no Gerenciador de soluções, escolha o menu Adicionar Novo Item no menu de contexto e adicione um novo arquivo de configuração Web denominada `Web.config`.


[![Adicionar um arquivo Web. config na pasta de associação](user-based-authorization-vb/_static/image8.png)](user-based-authorization-vb/_static/image7.png)

**Figura 3**: adicionar um `Web.config` do arquivo para o `Membership` pasta ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image9.png))


Neste momento a seu projeto deve conter dois `Web.config` arquivos: um no diretório raiz e outro no `Membership` pasta.


[![Seu aplicativo agora deve conter dois arquivos Web. config](user-based-authorization-vb/_static/image11.png)](user-based-authorization-vb/_static/image10.png)

**Figura 4**: seu aplicativo deve agora contêm dois `Web.config` arquivos ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image12.png))


Atualizar o arquivo de configuração no `Membership` pasta, assim que ele proíbe o acesso a usuários anônimos.

[!code-xml[Main](user-based-authorization-vb/samples/sample4.xml)]

Isso é tudo para ele!

Para testar essa alteração, visite a home page em um navegador e verifique se que você estiver desconectado. Como o comportamento padrão de um aplicativo ASP.NET é permitir que todos os visitantes, e como não fazemos as modificações de autorização para o diretório de raiz `Web.config` arquivo, somos capazes de visitar os arquivos no diretório raiz, como um visitante anônimo.

Clique no link criar contas de usuário encontrado na coluna à esquerda. Isso levará você até o `~/Membership/CreatingUserAccounts.aspx`. Uma vez que o `Web.config` do arquivo na `Membership` pasta define as regras de autorização para proibir o acesso anônimo, o `UrlAuthorizationModule` anula a solicitação e retorna um status HTTP 401 não autorizado. O `FormsAuthenticationModule` modifica isso com o status de redirecionamento 302, enviando-nos para a página de logon. Observe que a página nós estavam tentando acessar (`CreatingUserAccounts.aspx`) é passada para a página de logon por meio de `ReturnUrl` parâmetro querystring.


[![Desde a URL de autorização regras proibir o acesso anônimo, podemos serão redirecionados para a página de logon](user-based-authorization-vb/_static/image14.png)](user-based-authorization-vb/_static/image13.png)

**Figura 5**: uma vez que a autorização de URL regras proibir o acesso anônimo, podemos serão redirecionados para a página de logon ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image15.png))


Após fazer logon com êxito, podemos são redirecionados para o `CreatingUserAccounts.aspx` página. Desta vez o `UrlAuthorizationModule` permite o acesso à página porque não estamos mais anônimos.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Aplicando regras de autorização de URL para um local específico

As configurações de autorização definidas na `<system.web>` seção de `Web.config` se aplicam a todos os recursos do ASP.NET no diretório e seus subdiretórios (até que seja substituído por outro `Web.config` arquivo). Em alguns casos, no entanto, podemos todos os recursos do ASP.NET em um determinado diretório tenha uma configuração específica de autorização, exceto para uma ou duas páginas específicas. Isso pode ser feito pela adição de um `<location>` elemento no `Web.config`, apontá-lo para o arquivo cujas regras de autorização são diferentes e definindo suas regras de autorização exclusiva contido nele.

Para ilustrar a usando o `<location>` elemento para substituir as definições de configuração para um recurso específico, vamos personalizar as configurações de autorização, de modo que apenas Tito pode visitar `CreatingUserAccounts.aspx`. Para fazer isso, adicione uma `<location>` elemento para o `Membership` da pasta `Web.config` de arquivo e atualize sua marcação para que ele é semelhante ao seguinte:

[!code-xml[Main](user-based-authorization-vb/samples/sample5.xml)]

O `<authorization>` elemento no `<system.web>` define as regras de autorização de URL padrão para recursos ASP.NET no `Membership` pasta e suas subpastas. O `<location>` elemento nos permite substituir essas regras para um recurso específico. Na marcação acima de `<location>` referências de elemento a `CreatingUserAccounts.aspx` de páginas e especifica sua autorização regras, por exemplo, para permitir Tito, mas negar todas as outras pessoas.

Para testar essa alteração de autorização, comece visitando o site como um usuário anônimo. Se você tentar visitar qualquer página na `Membership` pasta, como `UserBasedAuthorization.aspx`, o `UrlAuthorizationModule` negará a solicitação e você será redirecionado à página de logon. Depois de fazer logon como, digamos, Scott, você pode visitar qualquer página na `Membership` pasta *exceto* para `CreatingUserAccounts.aspx`. Tentando visitar `CreatingUserAccounts.aspx` logon como qualquer pessoa, mas Tito resultará em uma tentativa de acesso não autorizado, redirecionando você para a página de logon.

> [!NOTE]
> O `<location>` elemento deve aparecer fora da configuração `<system.web>` elemento. Você precisa usar um separado `<location>` elemento para cada recurso cujas configurações de autorização que você deseja substituir.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Uma olhada em como o`UrlAuthorizationModule`usa as regras de autorização para conceder ou negar acesso

O `UrlAuthorizationModule` determina se a autorizar uma identidade específica para uma URL específica, analisando a autorização de URL regras um de cada vez, começando do primeiro e percorrendo seu caminho para baixo. Assim que uma correspondência for encontrada, o usuário é concedido ou negado acesso, dependendo se a correspondência foi encontrada em um `<allow>` ou `<deny>` elemento. <strong>Se nenhuma correspondência for encontrada, o usuário recebe acesso.</strong> Consequentemente, se você quiser restringir o acesso, é imperativo que você use um `<deny>` como o último elemento na configuração de autorização de URL. <strong>Se você omitir um</strong><strong>`<deny>`</strong><strong>elemento, todos os usuários terão acesso.</strong>

Para entender melhor o processo usado pelo `UrlAuthorizationModule` para determinar a autoridade, considere o exemplo de regras de autorização de URL, demos uma olhada no início desta etapa. A primeira regra é um `<allow>` elemento que permite acesso ao Tito e Scott. As regras de segundo é um `<deny>` elemento que nega o acesso a todos. Se um usuário anônimo visita, o `UrlAuthorizationModule` começa por solicitar, é anônimo Scott ou Tito? A resposta, obviamente, é não, portanto, ele prossegue para a segunda regra. É anônimo no conjunto de todas as pessoas? Desde a resposta Sim, aqui está o `<deny>` regra é colocada em vigor e o visitante é redirecionado para a página de logon. Da mesma forma, se está visitando Jisun, o `UrlAuthorizationModule` é iniciado perguntando é Jisun Scott ou Tito? Já que ela é não, o `UrlAuthorizationModule` prossegue para a segunda pergunta, Jisun está no conjunto de todas as pessoas? Ela é, portanto, ela, e também tem acesso negada. Por fim, se Tito visita, a primeira pergunta impostas pelo `UrlAuthorizationModule` é uma resposta afirmativa, portanto, Tito é concedido acesso.

Uma vez que o `UrlAuthorizationModule` processos a autorização de regras de cima para baixo, parando em qualquer correspondência, é importante ter as regras mais específicas a vir antes dos menos específicos. Ou seja, para definir regras de autorização que proíbem Jisun e os usuários anônimos, mas permitir que todos os outros usuários autenticados, você seria começar com a regra mais específica - o um Jisun impactantes - e, em seguida, vá para as regras a menos específicas - as permitindo que todos os outros os usuários autenticados, mas negar todos os usuários anônimos. As regras de autorização de URL a seguir implementa essa política pela primeira vez, negando Jisun e, em seguida, negando qualquer usuário anônimo. Qualquer usuário autenticado que não seja Jisun será concedido acesso porque nenhum dos dois `<deny>` instruções fará a correspondência.

[!code-xml[Main](user-based-authorization-vb/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Etapa 2: Corrigir o fluxo de trabalho para que os usuários não autorizados, autenticados

Conforme discutimos neste tutorial em uma aparência a seção de fluxo de trabalho de autorização de URL, a qualquer momento uma solicitação não autorizada ocorre, o `UrlAuthorizationModule` anula a solicitação e retorna um status HTTP 401 não autorizado. Esse status 401 é modificado pelo `FormsAuthenticationModule` em uma 302 redirecionar status que envia o usuário para a página de logon. Esse fluxo de trabalho ocorre em qualquer solicitação não autorizada, mesmo se o usuário é autenticado.

Retornando um usuário autenticado para a página de logon é provavelmente confundi-los, pois eles já fez logon no sistema. Com um pouco de trabalho, podemos melhorar esse fluxo de trabalho, redirecionando os usuários autenticados que fazem solicitações não autorizadas para uma página que explica o que eles tentaram acessar uma página restrita.

Comece criando uma nova página ASP.NET na pasta de raiz do aplicativo web chamada `UnauthorizedAccess.aspx`; não se esqueça de associar a esta página com o `Site.master` página mestra. Depois de criar essa página, remova o controle de conteúdo que faz referência a `LoginContent` ContentPlaceHolder para que a página mestra conteúdo padrão do será exibida. Em seguida, adicione uma mensagem que explica a situação, ou seja, o usuário tentou acessar um recurso protegido. Depois de adicionar a mensagem, o `UnauthorizedAccess.aspx` marcação declarativa da página deve ser semelhante ao seguinte:

[!code-aspx[Main](user-based-authorization-vb/samples/sample7.aspx)]

Agora, é necessário alterar o fluxo de trabalho para que se uma solicitação não autorizada é realizada por um usuário autenticado que elas são enviadas para o `UnauthorizedAccess.aspx` página em vez da página de logon. A lógica que redireciona as solicitações não autorizadas para a página de logon que esteja escondida dentro de um método particular do `FormsAuthenticationModule` de classe, portanto, podemos não é possível personalizar esse comportamento. O que podemos fazer, no entanto, é adicionar nossa própria lógica para a página de logon que redireciona o usuário para `UnauthorizedAccess.aspx`, se necessário.

Quando o `FormsAuthenticationModule` redireciona um visitante não autorizado para a página de logon, ele acrescenta a URL solicitada, não autorizada a cadeia de consulta com o nome `ReturnUrl`. Por exemplo, se um usuário não autorizado tentou visitar `OnlyTito.aspx`, o `FormsAuthenticationModule` seria redirecioná-las para `Login.aspx?ReturnUrl=OnlyTito.aspx`. Portanto, se a página de logon for atingida por um usuário autenticado com uma cadeia de consulta que inclui o `ReturnUrl` parâmetro, em seguida, podemos saber que esse usuário não autenticado apenas tentou visitar uma página que ela não está autorizada a exibir. Nesse caso, queremos redirecionar dela para `UnauthorizedAccess.aspx`.

Para fazer isso, adicione o seguinte código para a página de logon `Page_Load` manipulador de eventos:

[!code-vb[Main](user-based-authorization-vb/samples/sample8.vb)]

O código acima redireciona os usuários não autorizados, autenticados para o `UnauthorizedAccess.aspx` página. Para ver essa lógica em ação, visite o site como um visitante anônimo e clique no link de criação de contas de usuário na coluna à esquerda. Isso levará você até o `~/Membership/CreatingUserAccounts.aspx` página, que, na etapa 1, configuramos apenas para permitir o acesso ao Tito. Uma vez que os usuários anônimos são proibidos, o `FormsAuthenticationModule` redireciona-nos de volta para a página de logon.

Neste ponto, podemos são anônimos, portanto `Request.IsAuthenticated` retorna `False` e nós não serão redirecionados para `UnauthorizedAccess.aspx`. Em vez disso, a página de logon é exibida. Faça logon como um usuário diferente Tito como Bruce. Depois de inserir as credenciais apropriadas, o logon página redireciona-nos de volta ao `~/Membership/CreatingUserAccounts.aspx`. No entanto, como essa página só está acessível para Tito, vamos Anexações não autorizados para exibi-lo e são retornados imediatamente para a página de logon. Neste momento, no entanto, `Request.IsAuthenticated` retorna `True` (e o `ReturnUrl` parâmetro querystring existe), portanto, podemos são redirecionados para o `UnauthorizedAccess.aspx` página.


[![Usuários não autorizados autenticado, serão redirecionados para UnauthorizedAccess.aspx](user-based-authorization-vb/_static/image17.png)](user-based-authorization-vb/_static/image16.png)

**Figura 6**: autenticado, usuários não autorizados são redirecionados para `UnauthorizedAccess.aspx` ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image18.png))


Esse fluxo de trabalho personalizado apresenta uma experiência de usuário mais sensata e simples por curto-circuito o ciclo descrito na Figura 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Etapa 3: Limitando a funcionalidade com base no usuário conectado no momento

Autorização de URL torna mais fácil especificar regras de autorização grosso. Como vimos na etapa 1, com autorização de URL é podem sutilmente quais identidades têm permissão e quais delas são negadas de visualizar uma página específica ou todas as páginas em uma pasta. Em determinados cenários, no entanto, podemos querer permitir que todos os usuários visitam uma página, mas limitar a funcionalidade da página com base no usuário visitá-lo.

Considere o caso de um site de comércio eletrônico que permite que os visitantes autenticados examinar seus produtos. Quando um usuário anônimo visita uma página de produto, eles seriam veja apenas as informações de produto e não receberia a oportunidade de deixar uma revisão. No entanto, um usuário autenticado visitando a página mesma veria a interface de revisão. Se o usuário autenticado tinha ainda não foram examinados neste produto, a interface permitisse enviar uma análise; Caso contrário, ele seria mostrá-los de sua revisão enviados anteriormente. Para dar um passo para esse cenário ainda mais, a página de produto pode mostrar informações adicionais e oferecem recursos estendidos para os usuários que trabalham na empresa de comércio eletrônico. Por exemplo, a página de produto pode listar o inventário de estoque e incluem opções para editar o preço do produto e a descrição quando visitado por um funcionário.

Essas regras de autorização de alta granularidade podem ser implementadas de forma declarativa ou programaticamente (ou através de uma combinação dos dois). Na próxima seção, veremos como implementar a autorização de alta granularidade por meio do controle LoginView. Depois disso, iremos explorar técnicas de programação. Antes que possamos observar a aplicação de regras de autorização de alta granularidade, no entanto, primeiro precisamos criar uma página cuja funcionalidade depende do usuário visitá-lo.

Vamos criar uma página que lista os arquivos em um diretório específico dentro de um GridView. Junto com a listagem de nome de cada arquivo, tamanho e outras informações GridView incluirá duas colunas de botões de link: um modo de exibição e uma exclusão intitulada, denominado. Se você clicar no LinkButton do modo de exibição, o conteúdo do arquivo selecionado será exibido; Se você clicar no botão Excluir LinkButton, o arquivo será excluído. Vamos inicialmente criar essa página, de modo que sua funcionalidade de exibição e delete está disponível para todos os usuários. Usando as seções de controle LoginView e limitando a funcionalidade por meio de programação, veremos como habilitar ou desabilitar esses recursos com base no usuário visitar a página.

> [!NOTE]
> A página do ASP.NET que estamos prestes a criar usa um controle GridView para exibir uma lista de arquivos. Desde neste tutorial na série se concentra em formulários de autenticação, autorização, contas de usuário e funções, não quero dedicar muito tempo discutindo o funcionamento interno do controle GridView. Enquanto este tutorial fornece instruções passo a passo específicas para a configuração nesta página, ele não me aprofundar nos detalhes de por que determinadas escolhas feitas ou o que tem propriedades específicas do efeito na saída renderizada. Para obter uma análise minuciosa do controle GridView, consulte minha *[trabalhando com dados no ASP.NET 2.0](../../data-access/index.md)* série de tutoriais.


Comece abrindo o `UserBasedAuthorization.aspx` arquivo o `Membership` pasta e adicionando um controle GridView para a página chamada `FilesGrid`. Na marca inteligente do GridView, clique no link de editar colunas para iniciar a caixa de diálogo de campos. A partir daqui, desmarque a opção Gerar automaticamente campos seleção no canto inferior esquerdo. Em seguida, adicione um botão de seleção, um botão de exclusão e dois BoundFields do canto esquerdo superior (os botões Selecionar e excluir podem ser encontrados no tipo CommandField). Defina o botão de seleção `SelectText` propriedade para o modo de exibição e do BoundField primeiro `HeaderText` e `DataField` propriedades como nome. Definir o BoundField de segundo `HeaderText` propriedade ao tamanho em Bytes, seu `DataField` propriedade para o comprimento, seu `DataFormatString` propriedade a ser {0:N0} e sua `HtmlEncode` propriedade como False.

Depois de configurar as colunas do GridView, clique em Okey para fechar a caixa de diálogo de campos. Na janela Propriedades, defina o GridView `DataKeyNames` propriedade para `FullName`. Neste ponto, marcação declarativa do GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](user-based-authorization-vb/samples/sample9.aspx)]

Com a marcação do GridView criada, estamos prontos para escrever o código que irá recuperar os arquivos em um diretório específico e associá-las a GridView. Adicione o seguinte código para a página `Page_Load` manipulador de eventos:

[!code-vb[Main](user-based-authorization-vb/samples/sample10.vb)]

O código acima usa o [ `DirectoryInfo` classe](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) para obter uma lista dos arquivos na pasta raiz do aplicativo. O [ `GetFiles()` método](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) retorna todos os arquivos no diretório, como uma matriz de [ `FileInfo` objetos](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), que é então associado ao GridView. O `FileInfo` objeto tem uma variedade de propriedades, como `Name`, `Length`, e `IsReadOnly`, entre outros. Como você pode ver na sua marcação declarativa, GridView exibe apenas o `Name` e `Length` propriedades.

> [!NOTE]
> O `DirectoryInfo` e `FileInfo` classes são encontradas na [ `System.IO` namespace](https://msdn.microsoft.com/library/system.io.aspx). Portanto, você precisará dos preceda esses nomes de classe com seus nomes de namespace ou ter o namespace importado para o arquivo de classe (via `Imports System.IO`).


Reserve um tempo para visitar esta página por meio de um navegador. Ele exibirá a lista de arquivos que residem no diretório raiz do aplicativo. Clicar em qualquer um da exibição ou excluir LinkButtons fará com que um postback, mas nenhuma ação ocorrerá porque ainda não encontramos criar manipuladores de eventos necessários.


[![O GridView lista os arquivos no diretório raiz do aplicativo de Web](user-based-authorization-vb/_static/image20.png)](user-based-authorization-vb/_static/image19.png)

**Figura 7**: O GridView lista os arquivos no diretório raiz do aplicativo de Web ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image21.png))


Precisamos de um meio para exibir o conteúdo do arquivo selecionado. Retorne ao Visual Studio e adicione uma caixa de texto chamada `FileContents` acima GridView. Defina suas `TextMode` propriedade para `MultiLine` e seu `Columns` e `Rows` propriedades a 95% e 10, respectivamente.

[!code-aspx[Main](user-based-authorization-vb/samples/sample11.aspx)]

Em seguida, crie um manipulador de eventos para o GridView [ `SelectedIndexChanged` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) e adicione o seguinte código:

[!code-vb[Main](user-based-authorization-vb/samples/sample12.vb)]

Esse código usa o GridView `SelectedValue` propriedade para determinar o nome de arquivo completo do arquivo selecionado. Internamente, o `DataKeys` coleção é referenciada para obter o `SelectedValue`, portanto, é imperativo que você defina o GridView `DataKeyNames` propriedade para o nome, conforme descrito anteriormente nesta etapa. O [ `File` classe](https://msdn.microsoft.com/library/system.io.file.aspx) é usado para ler o conteúdo do arquivo selecionado em uma cadeia de caracteres, que é então atribuído ao `FileContents` da caixa de texto `Text` propriedade, assim, exibindo o conteúdo do arquivo selecionado na página.


[![Conteúdo do arquivo selecionado é exibido na caixa de texto](user-based-authorization-vb/_static/image23.png)](user-based-authorization-vb/_static/image22.png)

**Figura 8**: do conteúdo do arquivo selecionado o é exibido na caixa de texto ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image24.png))


> [!NOTE]
> Se você exibir o conteúdo de um arquivo que contém a marcação HTML e, em seguida, tenta exibir ou excluir um arquivo, você receberá um `HttpRequestValidationException` erro. Isso ocorre porque no postback conteúdo da caixa de texto é enviado para o servidor web. Por padrão, o ASP.NET gera um `HttpRequestValidationException` erro sempre que o conteúdo potencialmente perigoso de postback, como marcação HTML, é detectado. Para desabilitar esse erro ocorra, desativar a validação de solicitação para a página adicionando `ValidateRequest="false"` para o `@Page` diretiva. Para obter mais informações sobre os benefícios de validação de solicitação como bem como quais precauções você deve tomar quando desabilitá-lo, leia [solicitação de validação – impedindo ataques de Script](https://asp.net/learn/whitepapers/request-validation/).


Por fim, adicione um manipulador de eventos com o seguinte código para o GridView [ `RowDeleting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-vb[Main](user-based-authorization-vb/samples/sample13.vb)]

O código simplesmente exibe o nome completo do arquivo a ser excluído na `FileContents` caixa de texto *sem* realmente excluir o arquivo.


[![Clicar no botão de exclusão não exclui o arquivo](user-based-authorization-vb/_static/image26.png)](user-based-authorization-vb/_static/image25.png)

**Figura 9**: clicando o excluir botão não exclui o arquivo ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image27.png))


Na etapa 1, configuramos as regras de autorização de URL para proibir os usuários anônimos exibam as páginas no `Membership` pasta. Para melhor apresentar a autenticação de alta granularidade, vamos permitir que usuários anônimos, visite o `UserBasedAuthorization.aspx` página, mas com funcionalidade limitada. Para abrir essa página para ser acessado por todos os usuários, adicione o seguinte `<location>` elemento para o `Web.config` arquivo no `Membership` pasta:

[!code-xml[Main](user-based-authorization-vb/samples/sample14.xml)]

Depois de adicionar esse `<location>` elemento, testar as novas regras de autorização de URL ao fazer logoff do site. Como um usuário anônimo você deve receber permissão para visitar o `UserBasedAuthorization.aspx` página.

Atualmente, qualquer usuário autenticado ou anônimo pode visitar o `UserBasedAuthorization.aspx` página e exibir ou excluir arquivos. Vamos torná-lo para que somente usuários autenticados podem exibir o conteúdo de um arquivo e Tito só pode excluir um arquivo. Tais regras de autorização de alta granularidade podem ser aplicadas declarativamente, programaticamente ou por meio de uma combinação de ambos os métodos. Vamos usar a abordagem declarativa para limitar quem pode exibir o conteúdo de um arquivo; Vamos usar a abordagem programática para limitar quem pode excluir um arquivo.

### <a name="using-the-loginview-control"></a>Usar o controle LoginView

Como vimos nos tutoriais anteriores, o controle LoginView é útil para exibir as interfaces diferentes para usuários autenticados e anônimos e oferece uma maneira fácil para ocultar a funcionalidade que não está acessível a usuários anônimos. Uma vez que os usuários anônimos não podem exibir ou excluir arquivos, precisamos apenas Mostrar o `FileContents` caixa de texto quando a página for visitada por um usuário autenticado. Para fazer isso, adicione um controle LoginView para a página, nomeie- `LoginViewForFileContentsTextBox`e mover os `FileContents` a marcação declarativa da caixa de texto no controle de LoginView `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-vb/samples/sample15.aspx)]

Os controles da Web em modelos do LoginView não são diretamente acessíveis na classe code-behind. Por exemplo, o `FilesGrid` do GridView `SelectedIndexChanged` e `RowDeleting` manipuladores de eventos atualmente fazem referência a `FileContents` , como controle de caixa de texto com o código:

[!code-aspx[Main](user-based-authorization-vb/samples/sample16.aspx)]

No entanto, esse código não é mais válido. Movendo o `FileContents` caixa de texto para o `LoggedInTemplate` a caixa de texto não pode ser acessada diretamente. Em vez disso, devemos usar o `FindControl("controlId")` método a referenciar programaticamente o controle. Atualização de `FilesGrid` manipuladores de eventos para fazer referência a caixa de texto da seguinte forma:

[!code-vb[Main](user-based-authorization-vb/samples/sample17.vb)]

Depois de mover a caixa de texto para o LoginView `LoggedInTemplate` e atualizar o código da página para referência usando a caixa de texto o `FindControl("controlId")` padrão, visite a página como um usuário anônimo. Como mostra a Figura 10, o `FileContents` não será exibida a caixa de texto. No entanto, o modo de exibição LinkButton ainda é exibido.


[![O controle LoginView renderiza apenas a caixa de texto FileContents para usuários autenticados](user-based-authorization-vb/_static/image29.png)](user-based-authorization-vb/_static/image28.png)

**Figura 10**: O LoginView controle processa somente as `FileContents` caixa de texto para usuários autenticados ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image30.png))


Uma maneira para ocultar o botão de modo de exibição para usuários anônimos é converter o campo de GridView em um TemplateField. Isso irá gerar um modelo que contém a marcação declarativa para o modo de exibição LinkButton. Podemos, em seguida, adicione um controle LoginView para o TemplateField e colocar o LinkButton dentro do LoginView `LoggedInTemplate`e, portanto, ocultar o botão de visualização de visitantes anônimos. Para fazer isso, clique no link Edit Columns de Smart Tag do GridView para iniciar a caixa de diálogo de campos. Em seguida, selecione o botão de seleção na lista no canto inferior esquerdo e, em seguida, clique em converter este campo em um TemplateField link. Ao fazer isso, você modificará a marcação declarativa do campo de:

[!code-aspx[Main](user-based-authorization-vb/samples/sample18.aspx)]

 Para: 

[!code-aspx[Main](user-based-authorization-vb/samples/sample19.aspx)]

 Neste ponto, podemos adicionar um LoginView para o TemplateField. A marcação a seguir exibe o LinkButton exibição somente para usuários autenticados. 

[!code-aspx[Main](user-based-authorization-vb/samples/sample20.aspx)]

Como mostra a Figura 11, o resultado final não é que muito como a exibição coluna continua sendo exibida, embora os botões de link de exibição dentro da coluna estão ocultos. Vamos examinar como ocultar a coluna de GridView inteira (e não apenas no LinkButton) na próxima seção.


[![O controle LoginView oculta os botões de link do modo de exibição para visitantes anônimos](user-based-authorization-vb/_static/image32.png)](user-based-authorization-vb/_static/image31.png)

**Figura 11**: O controle LoginView oculta os botões de link do modo de exibição para visitantes anônimos ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Limitando a funcionalidade de forma programática

Em algumas circunstâncias, declarativas técnicas são insuficientes para limitar a funcionalidade a uma página. Por exemplo, a disponibilidade de algumas funcionalidades de página pode ser dependente de critérios, além de se o usuário visitar a página está autenticado ou anônimo. Nesses casos, os vários elementos de interface do usuário podem ser exibidos ou ocultados por meio de programação.

Para limitar a funcionalidade por meio de programação, é necessário executar duas tarefas:

1. Determinar se o usuário visitar a página pode acessar a funcionalidade, e
2. Modificar programaticamente a interface do usuário com base em se o usuário tem acesso à funcionalidade em questão.

Para demonstrar a aplicação dessas duas tarefas, vamos apenas permitir Tito excluir arquivos do GridView. Nossa primeira tarefa, em seguida, é determinar se ele é Tito visitando a página. Depois de determinar que, é necessário ocultar coluna de exclusão do GridView (ou mostrar uma). As colunas do GridView são acessíveis por meio de seu `Columns` propriedade; uma coluna é renderizada somente se seu `Visible` estiver definida como `True` (o padrão).

Adicione o seguinte código para o `Page_Load` manipulador de eventos antes dos dados de associação para o GridView:

[!code-vb[Main](user-based-authorization-vb/samples/sample21.vb)]

Como discutimos na [ *visão geral de uma de autenticação de formulários* ](../introduction/an-overview-of-forms-authentication-vb.md) tutorial, `User.Identity.Name` retorna o nome da identidade. Isso corresponde ao nome de usuário inserido no controle de logon. Se ele é Tito visitando a página, segunda coluna do GridView `Visible` estiver definida como `True`; caso contrário, ele é definido como `False`. O resultado líquido é que, quando alguém que não seja Tito visita a página, outro usuário autenticado ou um usuário anônimo, a coluna de exclusão não é renderizada (veja a Figura 12); No entanto, quando Tito visita a página, a coluna de exclusão está presente (veja a Figura 13).


[![Excluir coluna não é processado quando visitado por alguém diferente de Tito (por exemplo, Bruce)](user-based-authorization-vb/_static/image35.png)](user-based-authorization-vb/_static/image34.png)

**Figura 12**: A excluir a coluna não é processado quando visitado por alguém diferente de Tito (por exemplo, Bruce) ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image36.png))


[![Excluir coluna é renderizada para Tito](user-based-authorization-vb/_static/image38.png)](user-based-authorization-vb/_static/image37.png)

**Figura 13**: A excluir a coluna é renderizada para Tito ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Etapa 4: Aplicar as regras de autorização a Classes e métodos

Na etapa 3, não permite a usuários anônimos exibam o conteúdo do arquivo e proibidos todos os usuários mas Tito de exclusão de arquivos. Isso foi feito, ocultando os elementos de interface do usuário associado para visitantes não autorizados por meio de técnicas programáticas e declarativas. Para nosso exemplo simple, ocultar corretamente os elementos de interface do usuário foi simples, mas e quanto mais complexas sites onde pode haver muitas maneiras diferentes de executar a mesma funcionalidade? Limitar essa funcionalidade para usuários não autorizados, o que acontece se esquecermos de ocultar ou desabilitar todos os elementos de interface do usuário aplicável?

Uma maneira fácil de garantir que uma parte específica da funcionalidade não pode ser acessada por um usuário não autorizado é decorar a classe ou método com o [ `PrincipalPermission` atributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Quando o tempo de execução do .NET usa uma classe ou executa um de seus métodos, ele verifica para garantir que o contexto de segurança atual tem permissão para usar a classe ou o método execute. O `PrincipalPermission` atributo fornece um mecanismo por meio do qual podemos definir essas regras.

Vamos demonstrar o uso de `PrincipalPermission` atributo do GridView `SelectedIndexChanged` e `RowDeleting` manipuladores de eventos para proibir a execução por usuários anônimos e que não seja Tito, respectivamente. Tudo o que precisamos fazer é adicionar o atributo apropriado por cima de cada definição de função:

[!code-vb[Main](user-based-authorization-vb/samples/sample22.vb)]

O atributo para o `SelectedIndexChanged` impõe de manipulador de eventos que somente usuários autenticados pode executar o manipulador de eventos, enquanto que o atributo no `RowDeleting` a execução do Tito limita o manipulador de eventos.

> [!NOTE]
> Um atributo pode ser aplicado a uma classe, método, propriedade ou evento. Ao adicionar um atributo, ele deve ser parte da classe, método, propriedade ou instrução de declaração de evento. Como o Visual Basic usa quebras de linha como delimitadores de instrução, atributos precisa aparecer na mesma linha da declaração ou diretamente acima dele com um caractere de continuação de linha (sublinhado). No trecho de código acima, o caractere de continuação de linha é usado para colocar o atributo em uma linha e a declaração de método em outro.


Se, de alguma forma, um usuário diferente Tito tenta executar o `RowDeleting` manipulador de eventos ou um usuário não autenticado tenta executar o `SelectedIndexChanged` manipulador de eventos, o tempo de execução do .NET irá gerar um `SecurityException`.


[![Se o contexto de segurança não está autorizado a executar o método, um SecurityException é gerado](user-based-authorization-vb/_static/image41.png)](user-based-authorization-vb/_static/image40.png)

**Figura 14**: se o contexto de segurança não está autorizado a executar o método, uma `SecurityException` é gerada ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image42.png))


> [!NOTE]
> Para permitir que vários contextos de segurança acessar uma classe ou método, decore a classe ou método com um `PrincipalPermission` atributo para cada contexto de segurança. Ou seja, para permitir Tito e Bruce executar o `RowDeleting` manipulador de eventos, adicione *dois* `PrincipalPermission` atributos:


[!code-vb[Main](user-based-authorization-vb/samples/sample23.vb)]

Além das páginas ASP.NET, muitos aplicativos também têm uma arquitetura que inclui várias camadas, como lógica de negócios e as camadas de acesso de dados. Essas camadas normalmente são implementadas como bibliotecas de classes e classes e métodos para executar a funcionalidade de relacionadas a dados e lógica de negócios da oferta. O `PrincipalPermission` atributo é útil para aplicar as regras de autorização para essas camadas.

Para obter mais informações sobre como usar o `PrincipalPermission` atributo para definir regras de autorização em classes e métodos, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)da entrada de blog [adicionando regras de autorização para os negócios e camadas de dados usando `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Resumo

Neste tutorial vimos como aplicar regras de autorização com base no usuário. Começamos com uma olhada no ASP. Estrutura de autorização de URL da rede. Em cada solicitação, o mecanismo do ASP.NET `UrlAuthorizationModule` inspeciona as regras de autorização de URL definidas na configuração do aplicativo para determinar se a identidade está autorizada a acessar o recurso solicitado. Em resumo, a autorização de URL torna mais fácil especificar regras de autorização para uma página específica ou para todas as páginas em um diretório específico.

A estrutura de autorização de URL aplica regras de autorização em uma base de página por página. Com a autorização de URL, a identidade de solicitante está autorizada a acessar um recurso específico, ou não. Muitos cenários, no entanto, chamar para regras de autorização mais refinada. Em vez de definir quem tem permissão para acessar uma página, talvez seja necessário para permitir que qualquer pessoa acesse uma página, mas mostrar dados diferentes ou oferecer funcionalidade diferente, dependendo do usuário visitar a página. Autorização de nível de página geralmente envolve a ocultar elementos da interface do usuário específico para impedir que usuários não autorizados acessem a funcionalidade proibida. Além disso, é possível usar atributos para restringir o acesso a classes e a execução de seus métodos para determinados usuários.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Adicionando regras de autorização para os negócios e camadas de dados usando `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autorização de ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Alterações entre o IIS6 e segurança do IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Configurando os subdiretórios e arquivos específicos](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Limitando a funcionalidade de modificação de dados com base no usuário](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb.md)
- [Guias de início rápido controle LoginView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Noções básicas sobre a autorização de URL do IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` Documentação técnica](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Trabalhando com dados no ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é  *[Sams Teach por conta própria ASP.NET 2.0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](validating-user-credentials-against-the-membership-user-store-vb.md)
> [Próximo](storing-additional-user-information-vb.md)
