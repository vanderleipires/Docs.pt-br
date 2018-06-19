---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-vb
title: Autorização baseada em usuário (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial examinaremos limitando o acesso a páginas e restringindo a funcionalidade de nível de página por meio de uma variedade de técnicas.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: bc937e9d-5c14-4fc4-aec7-440da924dd18
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 4073f349c7965a89b39a4b1b672f0e84fc96f287
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
ms.locfileid: "30891939"
---
<a name="user-based-authorization-vb"></a>Autorização baseada em usuário (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_vb.pdf)

> Neste tutorial examinaremos limitando o acesso a páginas e restringindo a funcionalidade de nível de página por meio de uma variedade de técnicas.


## <a name="introduction"></a>Introdução

A maioria dos aplicativos web que oferecem as contas de usuário fazer isso em parte para restringir determinados visitantes de acessar certas páginas no site. Em sites messageboard mais online, por exemplo, todos os usuários - anônimos e autenticados - sejam capazes de visualizar postagens do messageboard, mas somente os usuários autenticados podem visitar a página da web para criar uma nova postagem. E pode haver páginas administrativas que são acessíveis apenas a um usuário específico (ou um conjunto específico de usuários). Além disso, a funcionalidade de nível de página pode diferir em uma base por usuário. Ao exibir uma lista de mensagens, os usuários autenticados são mostrados uma interface para a classificação de cada postagem, enquanto esta interface não está disponível para visitantes anônimos.

ASP.NET facilita definir regras de autorização baseada em usuário. Com apenas um pouco de marcação em `Web.config`, páginas da web específicas ou diretórios inteiros podem ser bloqueados para que eles são acessíveis apenas a um subconjunto especificado de usuários. Funcionalidade de nível de página pode ser ativada ou desativados com base no usuário conectado no momento por meio de forma programática e declarativa.

Neste tutorial examinaremos limitando o acesso a páginas e restringindo a funcionalidade de nível de página por meio de uma variedade de técnicas. Vamos começar!

## <a name="a-look-at-the-url-authorization-workflow"></a>Examinar o fluxo de trabalho de autorização de URL

Como discutido o [ *uma visão geral de formulários de autenticação* ](../introduction/an-overview-of-forms-authentication-vb.md) tutorial, quando o tempo de execução do ASP.NET processa uma solicitação para um recurso do ASP.NET a solicitação eleva um número de eventos durante o ciclo de vida. *Módulos HTTP* são classes gerenciadas, cujo código é executado em resposta a um evento específico no ciclo de vida de solicitação. ASP.NET vem com um número de módulos HTTP que realizar tarefas essenciais em segundo plano.

É um módulo tais HTTP [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Conforme discutido em tutoriais anteriores, a principal função do `FormsAuthenticationModule` é determinar a identidade da solicitação atual. Isso é feito ao inspecionar o tíquete de autenticação de formulários, que está localizado em um cookie ou incorporado a URL. Essa identificação ocorre durante o [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Outro módulo HTTP importante é o [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), que é gerado em resposta ao [ `AuthorizeRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (que ocorre após o `AuthenticateRequest` evento). O `UrlAuthorizationModule` examina a marcação de configuração em `Web.config` para determinar se a identidade atual tem autoridade para visitar a página especificada. Esse processo é conhecido como *autorização de URL*.

Vamos examinar a sintaxe para as regras de autorização de URL na etapa 1, mas primeiro vamos examinar o que o `UrlAuthorizationModule` dependendo se a solicitação está autorizada ou não. Se o `UrlAuthorizationModule` determina que a solicitação for autorizada, e em seguida, ele não faz nada e continua a solicitação por meio de seu ciclo de vida. No entanto, se a solicitação for *não* autorizado, o `UrlAuthorizationModule` anula o ciclo de vida e instrui o `Response` objeto para retornar um [HTTP 401 não autorizado](http://www.checkupdown.com/status/E401.html) status. Ao usar a autenticação de formulários esse status HTTP 401 nunca é retornada ao cliente, porque se o `FormsAuthenticationModule` detecta um é de status do HTTP 401 modifica um [redirecionamento HTTP 302](http://www.checkupdown.com/status/E302.html) a página de logon.

A Figura 1 ilustra o fluxo de trabalho do pipeline do ASP.NET, o `FormsAuthenticationModule`e o `UrlAuthorizationModule` quando chega uma solicitação não autorizada. Em particular, a Figura 1 mostra uma solicitação por um visitante anônimo para `ProtectedPage.aspx`, que é uma página que nega o acesso a usuários anônimos. Como o visitante é anônimo, a `UrlAuthorizationModule` anula a solicitação e retorna um status HTTP 401 não autorizado. O `FormsAuthenticationModule` converte o status 401 em um redirecionamento 302 para página de logon. Depois que o usuário é autenticado por meio da página de logon, ele é redirecionado para `ProtectedPage.aspx`. Neste momento o `FormsAuthenticationModule` identifica o usuário com base em seu tíquete de autenticação. Agora que o visitante é autenticado, o `UrlAuthorizationModule` permite o acesso à página.


[![A autenticação de formulários e o fluxo de trabalho de autorização de URL](user-based-authorization-vb/_static/image2.png)](user-based-authorization-vb/_static/image1.png)

**Figura 1**: A autenticação de formulários e fluxo de trabalho de autorização de URL ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image3.png))


A Figura 1 mostra a interação que ocorre quando um visitante anônimo tenta acessar um recurso que não está disponível para usuários anônimos. Nesse caso, o visitante anônimo é redirecionado para a página de logon com a página que tentou visitar especificado na querystring. Depois que o usuário faz logon com êxito, ela será automaticamente redirecionada para o recurso que ela foi inicialmente tentando exibir.

Quando a solicitação não autorizada é feita por um usuário anônimo, esse fluxo de trabalho é simples e fácil para o visitante entender o que aconteceu e por quê. Mas tenha em mente que o `FormsAuthenticationModule` redirecionará *qualquer* não autorizado de usuário para a página de logon, mesmo se a solicitação é feita por um usuário autenticado. Isso pode resultar em uma experiência de usuário confuso, se um usuário autenticado tenta visita uma página para que ela não tem autoridade.

Imagine que nosso site teve suas regras de autorização de URL configuradas, de modo que a página ASP.NET `OnlyTito.aspx` era acessível apenas Tito. Agora, imagine que Sam visita o site, logon e, em seguida, tenta visite `OnlyTito.aspx`. O `UrlAuthorizationModule` irá interromper o ciclo de vida de solicitação e retornar um status HTTP 401 não autorizado, que o `FormsAuthenticationModule` detectará e, em seguida, redirecione Sam para a página de logon. Desde que o Sam já registrou em, no entanto, ela deve estar imaginando por que ela foi enviada para a página de logon. Ela pode motivo que suas credenciais de logon foram perdidas alguma forma, ou que ele entrou credenciais inválidas. Se Sam reiniciará as credenciais na página de logon será conectada (novamente) e redirecionada para `OnlyTito.aspx`. O `UrlAuthorizationModule` detectará que Sam não visite essa página, e ela será retornada para a página de logon.

A Figura 2 representa o fluxo de trabalho confuso.


[![O fluxo de trabalho padrão pode resultar em um ciclo confuso](user-based-authorization-vb/_static/image5.png)](user-based-authorization-vb/_static/image4.png)

**Figura 2**: O padrão fluxo de trabalho pode levar a um ciclo confuso ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image6.png))


O fluxo de trabalho ilustrado na Figura 2 pode rapidamente befuddle até mesmo o maioria dos computador experientes visitante. Examinaremos maneiras de evitar que isso confuso ciclo na etapa 2.

> [!NOTE]
> O ASP.NET usa dois mecanismos para determinar se o usuário atual pode acessar uma página da web específico: autorização de URL e a autorização do arquivo. Autorização do arquivo é implementada pelo [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), que determina a autoridade consultando as ACLs de arquivo (s) solicitado. Autorização do arquivo é mais comumente usada com autenticação do Windows porque as ACLs são permissões que se aplicam a contas do Windows. Ao usar a autenticação de formulários, todas as solicitações de nível de sistema de arquivo e sistema operacional são executadas pela mesma conta do Windows, independentemente do usuário visitar o site. Desde que esta série tutorial concentra-se na autenticação de formulários, não abordaremos autorização do arquivo.


### <a name="the-scope-of-url-authorization"></a>O escopo de autorização de URL

O `UrlAuthorizationModule` é código gerenciado que faz parte do tempo de execução do ASP.NET. Antes da versão 7 da Microsoft [serviços de informações da Internet (IIS)](https://www.iis.net/) servidor web, houve uma barreira distinta entre o pipeline HTTP do IIS e o pipeline do ASP.NET runtime. Em outras palavras, no IIS 6 e versões anteriores, ASP. Do NET `UrlAuthorizationModule` executa somente quando uma solicitação é delegada do IIS para o tempo de execução do ASP.NET. Por padrão, o IIS processa o conteúdo estático em si – como páginas HTML e CSS, JavaScript e arquivos de imagem - e só encaminha solicitações para o tempo de execução do ASP.NET quando uma página com uma extensão de `.aspx`, `.asmx`, ou `.ashx` é solicitado.

IIS 7, no entanto, permite integrado do IIS e ASP.NET pipelines. Com algumas definições de configuração, você pode configurar o IIS 7 para invocar o `UrlAuthorizationModule` para *todos os* solicitações, o que significa que as regras de autorização de URL podem ser definidas para arquivos de qualquer tipo. Além disso, o IIS 7 inclui seu próprio mecanismo de autorização de URL. Para obter mais informações sobre a integração do ASP.NET e a funcionalidade de autorização de URL nativo do IIS 7, consulte [Noções básicas de autorização de URL de IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Para uma visão mais detalhada de integração do ASP.NET e IIS 7, obtenha uma cópia do livro de Shahram Khosravi, *Professional IIS 7 e ASP.NET integrado programação* (ISBN: 0470152539 978).

Resumindo, em versões anteriores do IIS 7, regras de autorização de URL são aplicadas apenas a recursos controlados pelo tempo de execução do ASP.NET. Mas, com o IIS 7, é possível usar o recurso de autorização de URL nativo do IIS ou integrar ASP. Do NET `UrlAuthorizationModule` no pipeline HTTP do IIS, estendendo assim essa funcionalidade para todas as solicitações.

> [!NOTE]
> Há algumas diferenças sutis, porém importantes, como ASP. Do NET `UrlAuthorizationModule` e o recurso de autorização de URL do IIS 7 processar as regras de autorização. Este tutorial não examina a funcionalidade de autorização de URL do IIS 7 ou as diferenças em como ele analisa as regras de autorização em comparação comparadas o `UrlAuthorizationModule`. Para obter mais informações sobre esses tópicos, consulte a documentação do IIS 7 no MSDN ou na [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Etapa 1: Definir regras de autorização de URL no`Web.config`

O `UrlAuthorizationModule` determina se é necessário conceder ou negar acesso a um recurso solicitado para uma identidade específica com base nas regras de autorização de URL definidas na configuração do aplicativo. As regras de autorização são detalhadas no [ `<authorization>` elemento](https://msdn.microsoft.com/library/8d82143t.aspx) na forma de `<allow>` e `<deny>` elementos filho. Cada `<allow>` e `<deny>` elemento filho pode especificar:

- Um usuário específico
- Uma lista delimitada por vírgulas de usuários
- Todos os usuários anônimos, indicados por um ponto de interrogação (?)
- Todos os usuários, indicados por um asterisco (\*)

A marcação a seguir ilustra como usar as regras de autorização de URL para permitir que os usuários Tito e Scott e recusar todos os outros:

[!code-xml[Main](user-based-authorization-vb/samples/sample1.xml)]

O `<allow>` elemento define quais usuários têm permissão - Tito e Scott - enquanto o `<deny>` elemento instrui que *todos os* usuários serão negados.

> [!NOTE]
> O `<allow>` e `<deny>` elementos também podem especificar regras de autorização para funções. Vamos examinar a autorização baseada em função em um tutorial futuras.


A configuração a seguir concede acesso a qualquer pessoa que não seja o Sam (incluindo visitantes anônimos):

[!code-xml[Main](user-based-authorization-vb/samples/sample2.xml)]

Para permitir que somente usuários autenticados, use a configuração a seguir, que nega o acesso a todos os usuários anônimos:

[!code-xml[Main](user-based-authorization-vb/samples/sample3.xml)]

As regras de autorização são definidas dentro de `<system.web>` elemento no `Web.config` e se aplicam a todos os recursos do ASP.NET no aplicativo web. Muitas vezes, um aplicativo tem diferentes regras de autorização para seções diferentes. Por exemplo, em um site de comércio eletrônico, todos os visitantes podem examinar os produtos, consulte as análises do produto, o catálogo de pesquisa e assim por diante. No entanto, somente usuários autenticados podem alcançar o check-out ou as páginas para gerenciar um histórico de envio. Além disso, pode haver partes do site que são acessíveis apenas pelos selecionar usuários, como os administradores de sites.

ASP.NET facilita definir regras de autorização diferentes para diferentes arquivos e pastas no site. As regras de autorização especificadas na pasta raiz `Web.config` arquivo se aplicam a todos os recursos do ASP.NET no site. No entanto, essas configurações de autorização padrão podem ser substituídas para uma pasta específica, adicionando um `Web.config` com um `<authorization>` seção.

Vamos atualizar nosso site para que somente usuários autenticados podem visitar páginas ASP.NET o `Membership` pasta. Para fazer isso, precisamos adicionar um `Web.config` o arquivo para o `Membership` pasta e definir suas configurações de autorização para negar a usuários anônimos. Clique com botão direito do `Membership` pasta no Gerenciador de soluções, escolha o menu Adicionar Novo Item de menu de contexto e adicionar um novo arquivo de configuração da Web denominado `Web.config`.


[![Adicionar um arquivo Web. config na pasta de associação](user-based-authorization-vb/_static/image8.png)](user-based-authorization-vb/_static/image7.png)

**Figura 3**: adicionar um `Web.config` o arquivo para o `Membership` pasta ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image9.png))


O projeto agora deve conter dois `Web.config` arquivos: um no diretório raiz e outro no `Membership` pasta.


[![Seu aplicativo agora deve conter dois arquivos Web. config](user-based-authorization-vb/_static/image11.png)](user-based-authorization-vb/_static/image10.png)

**Figura 4**: seu aplicativo deve agora contém duas `Web.config` arquivos ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image12.png))


Atualizar o arquivo de configuração no `Membership` pasta para que ele impede o acesso a usuários anônimos.

[!code-xml[Main](user-based-authorization-vb/samples/sample4.xml)]

Isso é tudo que é necessário para que ele!

Para testar essa alteração, visite a home page em um navegador e verifique se que você estiver desconectado. Como o comportamento padrão de um aplicativo ASP.NET é permitir que todos os visitantes, e como podemos não faça as modificações de autorização para do diretório raiz `Web.config` arquivo, somos capazes de visitar os arquivos no diretório raiz como um visitante anônimo.

Clique no link criar contas de usuário encontrado na coluna esquerda. Isso o levará para a `~/Membership/CreatingUserAccounts.aspx`. Desde o `Web.config` arquivo o `Membership` pasta define as regras de autorização para proibir o acesso anônimo, o `UrlAuthorizationModule` anula a solicitação e retorna um status HTTP 401 não autorizado. O `FormsAuthenticationModule` modifica isso para um status 302 do redirecionamento, enviar a página de logon. Observe que a página é está tentando acessar (`CreatingUserAccounts.aspx`) é passada para a página de logon por meio de `ReturnUrl` parâmetro querystring.


[![Desde o URL de autorização regras proibir o acesso anônimo, podemos são redirecionados para a página de logon](user-based-authorization-vb/_static/image14.png)](user-based-authorization-vb/_static/image13.png)

**Figura 5**: desde que a autorização de URL regras proibir o acesso anônimo, são redirecionados para a página de logon ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image15.png))


Após fazer logon com êxito, são redirecionados para a `CreatingUserAccounts.aspx` página. Neste momento o `UrlAuthorizationModule` permite o acesso para a página porque não estamos mais anônimos.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Aplicar regras de autorização de URL para um local específico

As configurações de autorização definidas no `<system.web>` seção `Web.config` se aplicam a todos os recursos do ASP.NET no diretório e seus subdiretórios (até que seja substituído por outro `Web.config` arquivo). Em alguns casos, porém, devemos todos os recursos do ASP.NET em um diretório especificado para ter uma configuração específica de autorização, exceto uma ou duas páginas específicas. Isso pode ser feito adicionando uma `<location>` elemento `Web.config`apontá-lo para o arquivo cujas regras de autorização são diferentes e definindo suas regras de autorização exclusiva nele.

Para ilustrar o uso de `<location>` elemento para substituir as definições de configuração para um recurso específico, vamos personalizar as configurações de autorização para que somente Tito pode visitar `CreatingUserAccounts.aspx`. Para fazer isso, adicione um `<location>` elemento para o `Membership` da pasta `Web.config` de arquivos e atualizar sua marcação para que ele se parece com o seguinte:

[!code-xml[Main](user-based-authorization-vb/samples/sample5.xml)]

O `<authorization>` elemento `<system.web>` define as regras de autorização de URL padrão para recursos ASP.NET no `Membership` pasta e suas subpastas. O `<location>` elemento nos permite substituir essas regras para um recurso específico. Na marcação acima de `<location>` referências de elementos de `CreatingUserAccounts.aspx` página e especifica sua autorização regras para permitir Tito, mas negar todas as outras pessoas.

Para testar essa alteração de autorização, inicie visitando o site como um usuário anônimo. Se você tentar visitar qualquer página no `Membership` pasta, tais como `UserBasedAuthorization.aspx`, o `UrlAuthorizationModule` negará a solicitação e você será redirecionado à página de logon. Depois de fazer logon como, digamos, Scott, você pode visitar qualquer página de `Membership` pasta *exceto* para `CreatingUserAccounts.aspx`. Tentando visite `CreatingUserAccounts.aspx` logon como qualquer pessoa, mas Tito resultará em uma tentativa de acesso não autorizado, redirecionando você volta para a página de logon.

> [!NOTE]
> O `<location>` elemento deve aparecer fora da configuração `<system.web>` elemento. Você precisa usar um separado `<location>` elemento para cada recurso cujas configurações de autorização que você deseja substituir.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Uma olhada em como o`UrlAuthorizationModule`usa as regras de autorização para conceder ou negar acesso

O `UrlAuthorizationModule` determina se autorizar uma identidade específica de uma URL específica, analisando a autorização de URL regras para um de cada vez, começando da primeira e trabalhar sua forma para baixo. Assim que uma correspondência for encontrada, o usuário é concedido ou negado o acesso, dependendo se a correspondência foi encontrada em um `<allow>` ou `<deny>` elemento. <strong>Se nenhuma correspondência for encontrada, é concedido acesso ao usuário.</strong> Consequentemente, se você quiser restringir o acesso, é essencial que você use um `<deny>` como o último elemento na configuração de autorização de URL. <strong>Se você omitir um</strong><strong>`<deny>`</strong><strong>elemento, todos os usuários terão acesso.</strong>

Para entender melhor o processo usado pelo `UrlAuthorizationModule` para determinar a autoridade, considere o exemplo observamos anteriormente nessa etapa de regras de autorização de URL. A primeira regra é um `<allow>` elemento que permite o acesso a Tito e Scott. As regras de segundo é um `<deny>` elemento que nega o acesso a todos os usuários. Se um usuário anônimo acessa, o `UrlAuthorizationModule` começa por solicitar, é anônimo Scott ou Tito? A resposta, obviamente, é não, portanto ele prossegue para a segunda regra. É anônimo no conjunto de todas as pessoas? Desde que a resposta Sim, aqui está o `<deny>` regra é colocada em vigor e o visitante é redirecionado para a página de logon. Da mesma forma, se está visitando Jisun, o `UrlAuthorizationModule` começa perguntando é Jisun Scott ou Tito? Desde que ela não for, o `UrlAuthorizationModule` continua para a segunda pergunta, Jisun está no conjunto de todas as pessoas? Ela é, portanto ela, também terá o acesso negada. Por fim, se Tito visita, a primeira pergunta representado pelo `UrlAuthorizationModule` é uma resposta afirmativa, portanto, Tito tem acesso.

Desde o `UrlAuthorizationModule` processos de regras de autorização de cima para baixo, parando em qualquer correspondência, é importante para que as regras mais específicas vir antes dos menos específicos. Ou seja, para definir regras de autorização que proíbam Jisun e os usuários anônimos, mas permitir que todos os outros usuários autenticados, você deve iniciar com a regra mais específica - o Jisun afetar um - e vá para as regras menos específicas - as permitindo que todos os outros os usuários autenticados, mas negar a todos os usuários anônimos. As regras de autorização de URL a seguir implementa esta diretiva Negar primeiro Jisun e, em seguida, negando qualquer usuário anônimo. Qualquer usuário autenticado que não seja Jisun será concedido acesso porque nenhuma delas `<deny>` instruções fará a correspondência.

[!code-xml[Main](user-based-authorization-vb/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Etapa 2: Corrigir o fluxo de trabalho para que os usuários não autorizados, autenticados

Conforme abordado anteriormente neste tutorial na examinar a seção do fluxo de trabalho de autorização de URL, a qualquer momento uma solicitação não autorizada ocorre, o `UrlAuthorizationModule` anula a solicitação e retorna um status HTTP 401 não autorizado. Esse status 401 é modificado o `FormsAuthenticationModule` em um 302 redirecionamento status que envia o usuário para a página de logon. Este fluxo de trabalho ocorre em qualquer solicitação não autorizada, mesmo se o usuário é autenticado.

Retornar um usuário autenticado para a página de logon é provavelmente confundidos porque eles já fez no sistema. Com um pouco de trabalho, podemos melhorar o fluxo de trabalho redirecionando os usuários autenticados que fazem solicitações não autorizadas em uma página que explica o que eles tentaram acessar uma página restrita.

Comece criando uma nova página ASP.NET na pasta raiz do aplicativo da web do denominada `UnauthorizedAccess.aspx`; não se esqueça de associar a essa página com o `Site.master` página mestra. Depois de criar essa página, remova o controle de conteúdo que faz referência a `LoginContent` ContentPlaceHolder para que a página mestra conteúdo padrão do será exibida. Em seguida, adicione uma mensagem que explica a situação, isto é que o usuário tentou acessar um recurso protegido. Depois de adicionar a mensagem, o `UnauthorizedAccess.aspx` declarativo da página deve ser semelhante à seguinte:

[!code-aspx[Main](user-based-authorization-vb/samples/sample7.aspx)]

Agora, precisamos alterar o fluxo de trabalho para que se uma solicitação não autorizada é executada por um usuário autenticado que elas são enviadas para o `UnauthorizedAccess.aspx` página em vez da página de logon. A lógica que redireciona solicitações não autorizadas a página de logon é inserida dentro de um método privado a `FormsAuthenticationModule` classe, portanto, não é possível personalizar esse comportamento. O que podemos fazer, no entanto, é adicionar nossa própria lógica para a página de logon que redireciona o usuário para `UnauthorizedAccess.aspx`, se necessário.

Quando o `FormsAuthenticationModule` redireciona um visitante não autorizado a página de logon, ele adiciona a URL solicitada, não autorizada para querystring com o nome `ReturnUrl`. Por exemplo, se um usuário não autorizado tentou visitar `OnlyTito.aspx`, o `FormsAuthenticationModule` seria redirecioná-los para `Login.aspx?ReturnUrl=OnlyTito.aspx`. Portanto, se a página de logon é alcançada por um usuário autenticado com uma querystring que inclui o `ReturnUrl` parâmetro, em seguida, podemos saber que esse usuário não autenticado apenas tentou visitar uma página que não está autorizada a exibir. Nesse caso, queremos redirecionar para `UnauthorizedAccess.aspx`.

Para fazer isso, adicione o seguinte código para a página de logon `Page_Load` manipulador de eventos:

[!code-vb[Main](user-based-authorization-vb/samples/sample8.vb)]

O código acima redireciona autenticados, não autorizados aos usuários o `UnauthorizedAccess.aspx` página. Para ver essa lógica em ação, visite o site como um visitante anônimo e clique no link criar contas de usuário na coluna esquerda. Isso o levará para a `~/Membership/CreatingUserAccounts.aspx` página, que, na etapa 1 são configurados apenas para permitir o acesso a Tito. Desde que os usuários anônimos são proibidos, o `FormsAuthenticationModule` redireciona nos de volta para a página de logon.

Agora estamos anônimos, portanto `Request.IsAuthenticated` retorna `False` e não são redirecionados para `UnauthorizedAccess.aspx`. Em vez disso, a página de logon é exibida. Faça logon como um usuário diferente Tito como Bruce. Depois de inserir as credenciais apropriadas, o logon página redireciona nos de volta ao `~/Membership/CreatingUserAccounts.aspx`. No entanto, como essa página só está acessível para Tito, vamos são não autorizados para exibi-lo e retorna imediatamente para a página de logon. Neste momento, no entanto, `Request.IsAuthenticated` retorna `True` (e o `ReturnUrl` parâmetro querystring existe), portanto, podemos são redirecionados para o `UnauthorizedAccess.aspx` página.


[![Autenticado, os usuários não autorizados são redirecionados para UnauthorizedAccess.aspx](user-based-authorization-vb/_static/image17.png)](user-based-authorization-vb/_static/image16.png)

**Figura 6**: autenticado, os usuários não autorizados são redirecionados para `UnauthorizedAccess.aspx` ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image18.png))


Este fluxo de trabalho personalizado apresenta uma experiência de usuário mais adequado e simples por circuito pequeno o ciclo ilustrado na Figura 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Etapa 3: Limitar funcionalidade com base no usuário conectado no momento

Autorização de URL facilita especificar regras de autorização grosso. Como vimos na etapa 1, com a autorização de URL é pode sutilmente quais identidades são permitidas e quais têm permissão para exibir uma página específica ou todas as páginas em uma pasta. Em determinados cenários, no entanto, podemos querer que todos os usuários a visitar uma página, mas limitar a funcionalidade da página com base no usuário visitando.

Considere o caso de um site de comércio eletrônico que permite que os visitantes autenticados examinar seus produtos. Quando um usuário anônimo acessa a página do produto, eles seriam veja apenas as informações de produto e não seriam fornecidos a oportunidade de deixar uma revisão. No entanto, um usuário autenticado visitando a mesma página veria a interface de revisão. Se o usuário autenticado ainda não tiver revisado para este produto, a interface permitisse ao enviar uma revisão; Caso contrário, ele seria mostrá-los a análise enviados anteriormente. Para dar um passo para esse cenário ainda mais, a página pode mostrar informações adicionais e oferecem recursos estendidos para os usuários que trabalham para a empresa de comércio eletrônico. Por exemplo, a página pode listar o inventário em estoque e incluem opções para editar o preço do produto e a descrição quando visitado por um funcionário.

Essas regras de autorização refinadas podem ser implementadas declarativamente ou por meio de programação (ou por meio de uma combinação dos dois). Na próxima seção, veremos como implementar a autorização refinadas por meio do controle LoginView. Depois disso, iremos explorar as técnicas de programação. Antes que podemos ver a aplicação de regras de autorização refinadas, no entanto, primeiro precisamos criar uma página cuja funcionalidade depende do usuário visitando.

Vamos criar uma página que lista os arquivos em um diretório específico dentro de um controle GridView. Junto com cada nome de arquivo, tamanho e outras informações de listagem, GridView inclui duas colunas de botões de link: um modo de exibição e exclua intitulada, denominado. Se o modo de exibição LinkButton é clicado, o conteúdo do arquivo selecionado será exibido; Se você clicar no botão Excluir LinkButton, o arquivo será excluído. Vamos criar inicialmente nessa página, de modo que sua funcionalidade de exibição e delete está disponível para todos os usuários. No usando as seções de controle LoginView e limitar funcionalidade por meio de programação, veremos como habilitar ou desabilitar esses recursos com base no usuário visitar a página.

> [!NOTE]
> A página do ASP.NET que está prestes a criar usa um controle GridView para exibir uma lista de arquivos. Desde que este tutorial série se concentra em formulários de autenticação, autorização, contas de usuário e funções, não desejo gastar muito tempo discutindo o funcionamento interno do controle GridView. Enquanto este tutorial fornece instruções passo a passo específicas para a configuração nesta página, ele não entre em detalhes sobre por que certas escolhas feitas ou o que tem propriedades específicas do efeito na saída renderizada. Para obter um exame completo do controle GridView, consulte Meus *[trabalhando com dados no ASP.NET 2.0](../../data-access/index.md)* série de tutoriais.


Comece abrindo o `UserBasedAuthorization.aspx` arquivo o `Membership` pasta e adicionando um controle GridView para a página chamada `FilesGrid`. Marca inteligente do GridView, clique no link de editar colunas para abrir a caixa de diálogo de campos. Aqui, desmarque a caixa de campos de geração automática de seleção no canto inferior esquerdo. Em seguida, adicione um botão de seleção, um botão de exclusão e dois BoundFields do canto superior esquerdo (os botões de seleção e Delete podem ser encontrados no tipo CommandField). Definir o botão de seleção `SelectText` propriedade para exibição e o BoundField primeiro `HeaderText` e `DataField` propriedades de nome. Definir o BoundField segundo `HeaderText` propriedade para o tamanho em Bytes, seu `DataField` propriedade de comprimento, seu `DataFormatString` propriedade {0: n0} e sua `HtmlEncode` a propriedade como False.

Depois de configurar as colunas do GridView, clique em Okey para fechar a caixa de diálogo de campos. Na janela Propriedades, defina o GridView `DataKeyNames` propriedade `FullName`. Neste ponto declarativo do GridView deve parecer com o seguinte:

[!code-aspx[Main](user-based-authorization-vb/samples/sample9.aspx)]

Com a marcação do GridView criada, estamos prontos para escrever o código que irá recuperar os arquivos em um diretório específico e associá-las a GridView. Adicione o seguinte código para a página `Page_Load` manipulador de eventos:

[!code-vb[Main](user-based-authorization-vb/samples/sample10.vb)]

O código acima usa o [ `DirectoryInfo` classe](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) para obter uma lista dos arquivos na pasta raiz do aplicativo. O [ `GetFiles()` método](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) retorna todos os arquivos no diretório como uma matriz de [ `FileInfo` objetos](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), que é associado a GridView. O `FileInfo` objeto tem uma variedade de propriedades, como `Name`, `Length`, e `IsReadOnly`, entre outros. Como você pode ver na sua marcação declarativa, GridView exibe apenas o `Name` e `Length` propriedades.

> [!NOTE]
> O `DirectoryInfo` e `FileInfo` classes são encontradas no [ `System.IO` namespace](https://msdn.microsoft.com/library/system.io.aspx). Portanto, você vai precisar iniciar esses nomes de classe com seus nomes de namespace ou ter o namespace importado para o arquivo de classe (via `Imports System.IO`).


Aproveite a visitar a página através de um navegador. Ele exibirá a lista de arquivos que reside no diretório raiz do aplicativo. Clicar em qualquer um dos botões de link excluir ou exibição fará com que um postback, mas nenhuma ação ocorrerá porque temos ainda para criar os manipuladores de eventos necessários.


[![GridView lista os arquivos no diretório raiz do aplicativo Web](user-based-authorization-vb/_static/image20.png)](user-based-authorization-vb/_static/image19.png)

**Figura 7**: GridView lista os arquivos no diretório raiz do aplicativo Web ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image21.png))


Precisamos de uma forma de exibir o conteúdo do arquivo selecionado. Retorne ao Visual Studio e adicionar uma caixa de texto denominada `FileContents` acima GridView. Definir seu `TextMode` propriedade `MultiLine` e sua `Columns` e `Rows` propriedades 95% e 10, respectivamente.

[!code-aspx[Main](user-based-authorization-vb/samples/sample11.aspx)]

Em seguida, crie um manipulador de eventos do GridView [ `SelectedIndexChanged` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) e adicione o seguinte código:

[!code-vb[Main](user-based-authorization-vb/samples/sample12.vb)]

Esse código usa o GridView `SelectedValue` propriedade para determinar o nome de arquivo completo do arquivo selecionado. Internamente, o `DataKeys` coleção é referenciada para obter o `SelectedValue`, portanto, é fundamental que você defina o GridView `DataKeyNames` propriedade com nome, conforme descrito anteriormente nesta etapa. O [ `File` classe](https://msdn.microsoft.com/library/system.io.file.aspx) é usado para ler o conteúdo do arquivo selecionado em uma cadeia de caracteres, que é então atribuído ao `FileContents` da caixa de texto `Text` propriedade, portanto, exibindo o conteúdo do arquivo selecionado na página.


[![Conteúdo do arquivo selecionados é exibido na caixa de texto](user-based-authorization-vb/_static/image23.png)](user-based-authorization-vb/_static/image22.png)

**Figura 8**: do conteúdo do arquivo selecionado o é exibido na caixa de texto ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image24.png))


> [!NOTE]
> Se você exibir o conteúdo de um arquivo que contém a marcação HTML e, em seguida, tenta exibir ou excluir um arquivo, você receberá um `HttpRequestValidationException` erro. Isso ocorre porque, em um postback conteúdo da caixa de texto é enviado ao servidor web. Por padrão, o ASP.NET gera uma `HttpRequestValidationException` erro sempre que o conteúdo de postback potencialmente perigoso, como marcação HTML, é detectado. Para desabilitar esse erro ocorra, desative a validação de solicitação para a página adicionando `ValidateRequest="false"` para o `@Page` diretiva. Para obter mais informações sobre os benefícios de validação de solicitação como bem como que precauções você deve tomar quando desabilitá-lo, leia [validação de solicitação - impedindo ataques de Script](https://asp.net/learn/whitepapers/request-validation/).


Finalmente, adicione um manipulador de eventos com o seguinte código para o GridView [ `RowDeleting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-vb[Main](user-based-authorization-vb/samples/sample13.vb)]

O código simplesmente exibe o nome completo do arquivo para excluir o `FileContents` TextBox *sem* realmente excluir o arquivo.


[![Clique no botão de exclusão não realmente excluir o arquivo](user-based-authorization-vb/_static/image26.png)](user-based-authorization-vb/_static/image25.png)

**Figura 9**: clicar a excluir botão não, na verdade, exclui o arquivo ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image27.png))


Na etapa 1, configuramos as regras de autorização de URL para proibir que usuários anônimos exibindo as páginas de `Membership` pasta. Para exibir melhor autenticação refinados, vamos permitir que usuários anônimos, visite o `UserBasedAuthorization.aspx` página, mas com funcionalidade limitada. Para abrir essa página para ser acessado por todos os usuários, adicione o seguinte `<location>` elemento para o `Web.config` arquivo o `Membership` pasta:

[!code-xml[Main](user-based-authorization-vb/samples/sample14.xml)]

Depois de adicionar isso `<location>` elemento, testar as novas regras de autorização de URL ao fazer logoff do site. Como um usuário anônimo você tem permissão para visitar o `UserBasedAuthorization.aspx` página.

Atualmente, qualquer usuário autenticado ou anônimo pode visitar o `UserBasedAuthorization.aspx` página e exibir ou excluir arquivos. Vamos torná-lo para que somente usuários autenticados podem exibir o conteúdo de um arquivo e Tito só pode excluir um arquivo. Essas regras de autorização granulares podem ser aplicadas declarativamente, programaticamente ou por meio de uma combinação de ambos os métodos. Vamos usar a abordagem declarativa para limitar quem pode exibir o conteúdo de um arquivo. usaremos a abordagem programática para limitar quem pode excluir um arquivo.

### <a name="using-the-loginview-control"></a>Usando o controle LoginView

Como vimos nos tutoriais anteriores, o controle LoginView é útil para exibir as interfaces diferentes para usuários anônimos e autenticados e oferece uma maneira fácil para ocultar a funcionalidade que não está acessível a usuários anônimos. Desde que os usuários anônimos não é possível exibir ou excluir arquivos, precisamos apenas Mostrar o `FileContents` caixa de texto quando a página for visitada por um usuário autenticado. Para fazer isso, adicione um controle LoginView à página, nomeie-o `LoginViewForFileContentsTextBox`e mover o `FileContents` declarativo da caixa de texto do controle LoginView `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-vb/samples/sample15.aspx)]

Os controles da Web em modelos do LoginView não são diretamente acessíveis pela classe code-behind. Por exemplo, o `FilesGrid` do GridView `SelectedIndexChanged` e `RowDeleting` manipuladores de eventos atualmente fazem referência a `FileContents` como o controle de caixa de texto com o código:

[!code-aspx[Main](user-based-authorization-vb/samples/sample16.aspx)]

No entanto, esse código não é válido. Movendo o `FileContents` caixa de texto para o `LoggedInTemplate` a caixa de texto não pode ser acessada diretamente. Em vez disso, é necessário usar o `FindControl("controlId")` método para referenciar programaticamente o controle. Atualização de `FilesGrid` manipuladores de eventos para fazer referência a caixa de texto da seguinte forma:

[!code-vb[Main](user-based-authorization-vb/samples/sample17.vb)]

Depois de mover a caixa de texto para o LoginView `LoggedInTemplate` e atualizar o código da página de referência usando a caixa de texto de `FindControl("controlId")` padrão, visite a página como um usuário anônimo. Como mostra a Figura 10, o `FileContents` não é exibida a caixa de texto. No entanto, o modo de exibição LinkButton ainda será exibido.


[![O controle LoginView processa somente a caixa de texto FileContents para usuários autenticados](user-based-authorization-vb/_static/image29.png)](user-based-authorization-vb/_static/image28.png)

**Figura 10**: O LoginView controle só processa o `FileContents` TextBox para usuários autenticados ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image30.png))


Uma maneira para ocultar o botão de modo de exibição para usuários anônimos é converta o campo de GridView em um TemplateField. Isso irá gerar um modelo que contém a marcação declarativa para LinkButton o modo de exibição. Em seguida, podemos adicionar um controle LoginView para o TemplateField e colocar LinkButton dentro do LoginView `LoggedInTemplate`e, portanto, ocultando botão modo de exibição de visitantes anônimos. Para fazer isso, clique no link Editar colunas do GridView de marca inteligente para iniciar a caixa de diálogo de campos. Em seguida, selecione o botão de seleção na lista no canto inferior esquerdo e, em seguida, clique em converter este campo em um TemplateField link. Isso irá modificar declarativo do campo de:

[!code-aspx[Main](user-based-authorization-vb/samples/sample18.aspx)]

 Para: 

[!code-aspx[Main](user-based-authorization-vb/samples/sample19.aspx)]

 Neste ponto, podemos adicionar um LoginView para o TemplateField. A seguinte marcação exibe LinkButton o modo de exibição somente para usuários autenticados. 

[!code-aspx[Main](user-based-authorization-vb/samples/sample20.aspx)]

Como mostra a Figura 11, o resultado final não é que muito como a exibição da coluna ainda será exibida, embora os botões de link de exibição dentro da coluna estão ocultos. Examinaremos como ocultar a coluna inteira de GridView (e não apenas o LinkButton) na próxima seção.


[![O controle LoginView oculta os botões de link de exibição para visitantes anônimos](user-based-authorization-vb/_static/image32.png)](user-based-authorization-vb/_static/image31.png)

**Figura 11**: O controle LoginView oculta os botões de link de exibição para visitantes anônimos ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Funcionalidade de limitação programaticamente

Em algumas circunstâncias, técnicas declarativas são insuficientes para limitar a funcionalidade a uma página. Por exemplo, a disponibilidade de alguns recursos de página pode ser dependente critérios além se o usuário visitar a página é autenticado ou anônimo. Nesses casos, os vários elementos de interface do usuário podem ser exibidos ou ocultados por meio de programação.

Para limitar a funcionalidade por meio de programação, é necessário executar duas tarefas:

1. Determinar se o usuário visitar a página pode acessar a funcionalidade e
2. Modificar programaticamente a interface do usuário com base em se o usuário tem acesso à funcionalidade em questão.

Para demonstrar o aplicativo dessas duas tarefas, vamos apenas permitir Tito excluir arquivos de GridView. A primeira tarefa, em seguida, é determinar se ele é Tito visitar a página. Depois que tiver sido determinado, precisamos (Mostrar ou ocultar) coluna de exclusão do GridView. As colunas do GridView são acessíveis por meio de seu `Columns` propriedade; uma coluna é renderizada apenas se seu `Visible` está definida como `True` (o padrão).

Adicione o seguinte código para o `Page_Load` manipulador de eventos antes de associar os dados a GridView:

[!code-vb[Main](user-based-authorization-vb/samples/sample21.vb)]

Conforme abordado no [ *uma visão geral de formulários de autenticação* ](../introduction/an-overview-of-forms-authentication-vb.md) tutorial, `User.Identity.Name` retorna o nome da identidade. Isso corresponde ao nome de usuário inserido no controle de logon. Se é Tito visitar a página, segunda coluna do GridView `Visible` está definida como `True`; caso contrário, ele é definido como `False`. O resultado é que, quando alguém que não seja Tito visitar a página, outro usuário autenticado ou um usuário anônimo, a coluna de exclusão não é renderizada (veja a Figura 12); No entanto, quando Tito visita a página, a coluna de exclusão está presente (consulte a Figura 13).


[![Excluir coluna não é processado quando visitado por alguém diferente de Tito (como Bruce)](user-based-authorization-vb/_static/image35.png)](user-based-authorization-vb/_static/image34.png)

**Figura 12**: A excluir a coluna não é processado quando visitado por alguém diferente de Tito (como Bruce) ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image36.png))


[![Excluir coluna é renderizada para Tito](user-based-authorization-vb/_static/image38.png)](user-based-authorization-vb/_static/image37.png)

**Figura 13**: A excluir a coluna é renderizada para Tito ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Etapa 4: Aplicar regras de autorização para Classes e métodos

Na etapa 3, não permitida a usuários anônimos exibam o conteúdo do arquivo e proibidos todos os usuários, mas Tito de exclusão de arquivos. Isso foi feito, ocultando os elementos da interface de usuário associada aos visitantes não autorizados por meio de técnicas programáticas e declarativas. Para nosso exemplo simple, ocultar corretamente os elementos de interface do usuário foi simples, mas e quanto mais complexos sites onde pode haver diferentes maneiras de executar a mesma funcionalidade? Limitar essa funcionalidade para usuários não autorizados, o que acontece se esquecemos ocultar ou desativar todos os elementos de interface do usuário aplicável?

É uma maneira fácil de garantir que uma determinada parte da funcionalidade não pode ser acessada por um usuário não autorizado decorar dessa classe ou método com o [ `PrincipalPermission` atributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Quando o tempo de execução .NET usa uma classe ou executa um de seus métodos, ele verifica para garantir que o contexto de segurança atual tem permissão para usar a classe ou o método execute. O `PrincipalPermission` atributo fornece um mecanismo pelo qual podemos definir essas regras.

Vamos demonstrar o uso de `PrincipalPermission` atributo o GridView `SelectedIndexChanged` e `RowDeleting` manipuladores de eventos para proibir a execução por usuários anônimos e que não seja Tito, respectivamente. Tudo que precisamos fazer é adicionar o atributo apropriado sobre cada definição de função:

[!code-vb[Main](user-based-authorization-vb/samples/sample22.vb)]

O atributo para o `SelectedIndexChanged` impõe de manipulador de eventos que somente usuários autenticados pode executar o manipulador de eventos, enquanto que o atributo de `RowDeleting` a execução Tito limita o manipulador de eventos.

> [!NOTE]
> Um atributo pode ser aplicado a uma classe, método, propriedade ou evento. Ao adicionar um atributo, ele deve ser parte de classe, método, propriedade ou declaração de evento. Como o Visual Basic usa quebras de linha como delimitadores de instrução, atributos devem aparecer na mesma linha como a declaração ou diretamente acima dele com um caractere de continuação de linha (sublinhado). No trecho de código acima, o caractere de continuação de linha é usado para colocar o atributo em uma linha e a declaração de método em outro.


Se, de alguma forma, um usuário diferente Tito tenta executar o `RowDeleting` manipulador de eventos ou de um usuário autenticado não tenta executar o `SelectedIndexChanged` manipulador de eventos, o tempo de execução .NET irá gerar um `SecurityException`.


[![Se o contexto de segurança não está autorizado a executar o método, um SecurityException é gerada](user-based-authorization-vb/_static/image41.png)](user-based-authorization-vb/_static/image40.png)

**Figura 14**: se o contexto de segurança não está autorizado a executar o método, uma `SecurityException` é gerada ([clique para exibir a imagem em tamanho normal](user-based-authorization-vb/_static/image42.png))


> [!NOTE]
> Para permitir que vários contextos de segurança acessar uma classe ou método, decorar a classe ou método com um `PrincipalPermission` atributo para cada contexto de segurança. Ou seja, para permitir Tito e Bruce executar o `RowDeleting` manipulador de eventos, adicione *duas* `PrincipalPermission` atributos:


[!code-vb[Main](user-based-authorization-vb/samples/sample23.vb)]

Além de páginas ASP.NET, muitos aplicativos também têm uma arquitetura que inclui várias camadas, como camadas de acesso de dados e lógica de negócios. Essas camadas são normalmente implementadas como bibliotecas de classes e oferecem classes e métodos para executar a funcionalidade de relacionadas a dados e lógica de negócios. O `PrincipalPermission` atributo é útil para aplicar regras de autorização a essas camadas.

Para obter mais informações sobre como usar o `PrincipalPermission` atributo para definir regras de autorização em classes e métodos, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)da entrada de blog [adicionando regras de autorização para os negócios e dados camadas usando `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Resumo

Neste tutorial vimos como aplicar as regras de autorização baseada em usuário. Começamos examinando ASP. Estrutura de autorização de URL do NET. Em cada solicitação, o mecanismo do ASP.NET `UrlAuthorizationModule` inspeciona as regras de autorização de URL definidas na configuração do aplicativo para determinar se a identidade está autorizada a acessar o recurso solicitado. Em resumo, a autorização da URL torna fácil especificar regras de autorização para uma página específica ou para todas as páginas em um diretório específico.

A estrutura de autorização de URL aplica as regras de autorização em uma base de página por página. Com a autorização de URL, ou a identidade do solicitante está autorizada a acessar um recurso específico ou não. Muitos cenários, no entanto, chamar para regras de autorização mais granulares. Em vez de definir quem pode acessar uma página, podemos talvez seja necessário para permitir que alguém acessar uma página, mas mostrar dados diferentes ou oferecer funcionalidade diferente dependendo do usuário visitar a página. Autorização em nível de página geralmente envolve ocultando elementos de interface do usuário específico para impedir que usuários não autorizados acessem funcionalidade proibida. Além disso, é possível usar atributos para restringir o acesso a classes e a execução de seus métodos para determinados usuários.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Adicionando regras de autorização para os negócios e camadas de dados usando `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autorização de ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Alterações entre IIS6 e segurança do IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Configurando arquivos específicos e subpastas](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Limitar a funcionalidade de modificação de dados com base no usuário](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb.md)
- [Início rápido do controle LoginView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Noções básicas de autorização de URL de IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` Documentação técnica](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Trabalhando com dados no ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](validating-user-credentials-against-the-membership-user-store-vb.md)
> [Próximo](storing-additional-user-information-vb.md)
