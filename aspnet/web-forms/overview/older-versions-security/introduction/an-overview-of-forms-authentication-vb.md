---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: "Uma visão geral da autenticação de formulários (VB) | Microsoft Docs"
author: rick-anderson
description: "Neste tutorial, ativa de discussão simples para implementação; Especificamente, examinaremos a implementação da autenticação de formulários. W o aplicativo da web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 4c4564e5f1f71763e7e6a78622d30a25f1a6f640
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="an-overview-of-forms-authentication-vb"></a>Uma visão geral da autenticação de formulários (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> Neste tutorial, ativa de discussão simples para implementação; Especificamente, examinaremos a implementação da autenticação de formulários. Aplicativo da web, que vamos começar criando neste tutorial continuarão a ser construído em tutoriais subsequentes, como de autenticação de formulários simples a associação e funções.
> 
> Consulte este vídeo para obter mais informações sobre este tópico: [usando autenticação de formulários básica no ASP.NET](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Introdução

No [tutorial anterior](security-basics-and-asp-net-support-vb.md) abordamos as várias autenticação, autorização e usuário opções de conta fornecidas pelo ASP.NET. Neste tutorial, ativa de discussão simples para implementação; Especificamente, examinaremos a implementação da autenticação de formulários. Aplicativo da web, que vamos começar criando neste tutorial continuarão a ser construído em tutoriais subsequentes, como de autenticação de formulários simples a associação e funções.

Este tutorial começa com uma Visão aprofundada de fluxo de trabalho de autenticação de formulários, um tópico que abordamos no tutorial anterior. Depois disso, vamos criar um site ASP.NET por meio do qual a demonstrar os conceitos de autenticação de formulários. Em seguida, vamos configura o site para usar autenticação de formulários, criar uma página de logon simples e a determinar, no código, se um usuário é autenticado e, nesse caso, o nome de usuário fez logon.

Noções básicas sobre os formulários de fluxo de trabalho de autenticação, habilitá-la em um aplicativo da web e criar as páginas de logon e logoff são vitais todas as etapas da criação de um aplicativo ASP.NET que dá suporte a contas de usuário e autentica usuários por meio de uma página da web. Devido a isso, e porque esses tutoriais desenvolver uma da outra - seria sugiro que você trabalhe com este tutorial completo antes de avançarmos para o próximo, mesmo se você já tenha experiência Configurando a autenticação de formulários em projetos anteriores.

## <a name="understanding-the-forms-authentication-workflow"></a>Noções básicas sobre o fluxo de trabalho de autenticação de formulários

Quando o tempo de execução do ASP.NET processa uma solicitação para um recurso do ASP.NET, como uma página ASP.NET ou um serviço Web do ASP.NET, a solicitação eleva um número de eventos durante o ciclo de vida. Há eventos gerados no final muito começando e muito da solicitação, aqueles gerado quando a solicitação está sendo autenticada e autorizado, um evento gerado no caso de uma exceção sem tratamento e assim por diante. Para ver uma lista completa dos eventos, consulte o [eventos do objeto HttpApplication](https://msdn.microsoft.com/en-us/library/system.web.httpapplication_events.aspx).

*Módulos HTTP* são classes gerenciadas, cujo código é executado em resposta a um evento específico no ciclo de vida de solicitação. ASP.NET vem com um número de módulos HTTP que realizar tarefas essenciais em segundo plano. Dois módulos HTTP internos que são especialmente relevantes para nossa discussão são:

- **[FormsAuthenticationModule](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx)**  -autentica o usuário inspecionando o tíquete de autenticação de formulários, que normalmente é incluído na coleção de cookies do usuário. Se não houver nenhum tíquete de autenticação de formulários, o usuário é anônimo.
- **[UrlAuthorizationModule](https://msdn.microsoft.com/en-us/library/system.web.security.urlauthorizationmodule.aspx)**  -determina se o usuário atual está autorizado a acessar a URL solicitada. Esse módulo determina a autoridade consultando as regras de autorização especificadas nos arquivos de configuração do aplicativo. O ASP.NET também inclui o [FileAuthorizationModule](https://msdn.microsoft.com/en-us/library/system.web.security.fileauthorizationmodule.aspx) que determina a autoridade consultando as ACLs de arquivo (s) solicitado.

O FormsAuthenticationModule tenta autenticar o usuário antes do UrlAuthorizationModule (e FileAuthorizationModule) em execução. Se o usuário faz a solicitação não está autorizado a acessar o recurso solicitado, o módulo de autorização encerra a solicitação e retorna um [HTTP 401 não autorizado](http://www.checkupdown.com/status/E401.html) status. Em cenários de autenticação do Windows, o status do HTTP 401 é retornado para o navegador. Esse código de status faz com que o navegador solicitar ao usuário as credenciais por meio de uma caixa de diálogo modal. Com a autenticação de formulários, no entanto, o status HTTP 401 não autorizado nunca é enviado para o navegador como o FormsAuthenticationModule detecta esse status e o modifica para redirecionar o usuário para a página de logon, em vez disso, (por meio de um [redirecionamento de HTTP 302](http://www.checkupdown.com/status/E302.html) status).

É responsabilidade da página de logon para determinar se as credenciais do usuário são válidas e, nesse caso, para criar um tíquete de autenticação de formulários e redirecionar o usuário para a página estavam tentando executar a visitar. O tíquete de autenticação está incluído nas solicitações subsequentes para as páginas no site, que usa o FormsAuthenticationModule para identificar o usuário.


[![O fluxo de trabalho de autenticação de formulários](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**Figura 01**: O fluxo de trabalho de autenticação de formulários ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Lembre-se o tíquete de autenticação em visitas à página

Após o logon, o tíquete de autenticação de formulários deve ser enviado para o servidor web em cada solicitação para que o usuário permanece conectado que acessam o site. Normalmente, isso é feito colocando o tíquete de autenticação na coleção de cookies do usuário. [Cookies](http://en.wikipedia.org/wiki/HTTP_cookie) são pequenos arquivos de texto que residem no computador do usuário e são transmitidos em cabeçalhos HTTP em cada solicitação para o site que o criou. Portanto, depois que o tíquete de autenticação de formulários foi criado e armazenado em cookies do navegador, cada subsequente visita ao site envia o tíquete de autenticação junto com a solicitação, que identifica o usuário.

> [!NOTE]
> O aplicativo da web de demonstração usado em cada tutorial está disponível como um download. Este aplicativo para download foi criado com o Visual Web Developer 2008 direcionado para o .NET Framework versão 3.5. Desde que o aplicativo se destina ao .NET 3.5, o seu arquivo Web. config inclui elementos de configuração adicionais, 3.5 específica. História, se você ainda precisa instalar o .NET 3.5 em seu computador, em seguida, o aplicativo web para download não funcionará sem primeiro remover a marcação específica do 3.5 do Web. config.


Um aspecto de cookies é o prazo de expiração, que é a data e hora em que o navegador descarta o cookie. Quando expira o cookie de autenticação de formulários, o usuário pode não ser autenticado e, portanto, se tornar anônimo. Quando um usuário está visitando a partir de um terminal público, a probabilidade é que desejam seu tíquete de autenticação para expirar quando elas fecharem seu navegador. Ao visitar em casa, no entanto, esse mesmo usuário pode desejar o tíquete de autenticação a ser lembradas entre as reinicializações de navegador para que eles não têm a entrar novamente cada vez que visitarem o site. Essa decisão é geralmente feita pelo usuário na forma de uma lembrar-me a caixa de seleção na página de logon. Na etapa 3, vamos examinar como implementar uma caixa de seleção lembrar-me na página de logon. O tutorial a seguir aborda as configurações de tempo limite de tíquete de autenticação em detalhes.

> [!NOTE]
> É possível que o agente do usuário usado para fazer logon para o site pode não oferecer suporte a cookies. Nesse caso, o ASP.NET pode usar permissões de autenticação de formulários cookieless. Nesse modo, o tíquete de autenticação é codificado em URL. Examinaremos quando os tíquetes de autenticação sem cookies são usados e como elas são criadas e gerenciadas no tutorial Avançar.


### <a name="the-scope-of-forms-authentication"></a>O escopo de autenticação de formulários

O FormsAuthenticationModule é gerenciado de código que é parte do tempo de execução do ASP.NET. Antes da versão 7 da Microsoft [serviços de informações da Internet (IIS)](https://www.iis.net/) servidor web, houve uma barreira distinta entre o pipeline HTTP do IIS e o pipeline do ASP.NET runtime. Resumindo, no IIS 6 e versões anteriores, o FormsAuthenticationModule executa somente quando uma solicitação é delegada do IIS para o tempo de execução do ASP.NET. Por padrão, o IIS processa conteúdo estático em si – como páginas HTML e CSS e arquivos de imagem - e só envia solicitações para o tempo de execução do ASP.NET quando uma página com uma extensão. aspx,. asmx ou. ashx é solicitada.

IIS 7, no entanto, permite integrado do IIS e ASP.NET pipelines. Com algumas definições de configuração, você pode configurar o IIS 7 para invocar o FormsAuthenticationModule para *todos os* solicitações. Além disso, com o IIS 7, você pode definir regras de autorização de URL para arquivos de qualquer tipo. Para obter mais informações, consulte [alterações entre IIS6 e segurança do IIS7](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [sua segurança da plataforma Web](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), e [Noções básicas de autorização de URL de IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

História, em versões anteriores do IIS 7, você pode usar somente autenticação de formulários para proteger recursos controlados pelo tempo de execução do ASP.NET. Da mesma forma, as regras de autorização de URL são aplicadas apenas a recursos controlados pelo tempo de execução do ASP.NET. Mas, com o IIS 7, é possível integrar o FormsAuthenticationModule e UrlAuthorizationModule pipeline HTTP do IIS, estendendo assim essa funcionalidade para todas as solicitações.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Etapa 1: Criar um site ASP.NET para esta série de tutoriais

Para alcançar o público mais amplo possível, o site da Web ASP.NET criaríamos toda essa série será criado com a versão gratuita da Microsoft do Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Vamos implementar o armazenamento do usuário SqlMembershipProvider em uma [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/en-us/sql/Aa336346.aspx) banco de dados. Se você estiver usando o Visual Studio 2005 ou uma edição diferente do Visual Studio 2008 ou SQL Server, não se preocupe - as etapas serão quase idênticas e quaisquer diferenças não trivial serão indicadas.

Antes, pode configurar a autenticação de formulários, é preciso primeiro um site ASP.NET. Comece criando um novo arquivo com base em sistema site da Web ASP.NET. Para fazer isso, inicie o Visual Web Developer e vá para o menu Arquivo e escolha o novo Site da Web, exibindo a caixa de diálogo do novo Site da Web. Escolher o modelo de Site da Web ASP.NET, defina a lista suspensa do local do sistema de arquivos, escolha uma pasta para colocar o site da web e definir o idioma para VB. Isso criará um novo site com uma página Default.aspx ASP.NET, um aplicativo\_pasta de dados e um arquivo Web. config.

> [!NOTE]
> Visual Studio oferece suporte a dois modos de gerenciamento de projeto: os projetos de Site da Web e projetos de aplicativo Web. Projetos de Site não têm um arquivo de projeto, enquanto os projetos de aplicativo Web imitar a arquitetura de projeto no Visual Studio .NET 2002/2003 - eles incluem um arquivo de projeto e compilar o código-fonte do projeto em um único assembly, que é colocado na pasta /bin. O Visual Studio 2005 inicialmente apenas sites da Web com suporte de projetos, embora o modelo de projeto de aplicativo Web foi reintroduzido com Service Pack 1. O Visual Studio 2008 oferece os dois modelos de projeto. O Visual Web Developer 2005 e 2008 edições, no entanto, somente suportam a projetos de Site. Usarei o modelo de projeto de Site. Se você estiver usando uma edição não Express e deseja usar o [modelo de projeto de aplicativo Web](https://msdn.microsoft.com/en-us/library/aa730880(vs.80).aspx) em vez disso, fique à vontade para fazer isso, mas lembre-se de que pode haver algumas discrepâncias entre o que você vê na tela e as etapas que você deve tomar em relação a capturas de tela mostradas e instruções fornecidas nos tutoriais.


[![Criar um novo arquivo com base em sistema Site da Web](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**Figura 02**: criar um Site New File System-Based ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>Adicionando uma página mestra

Em seguida, adicione uma nova página mestra para o site no diretório raiz chamado Site.master. [Páginas mestras](https://msdn.microsoft.com/en-us/library/wtxbf3hh.aspx) permitem um desenvolvedor de página Definir um modelo de todo o site que pode ser aplicado a páginas ASP.NET. O principal benefício de páginas mestras é que a aparência geral do site pode ser definida em um único local, tornando fácil de atualizar ou ajustar o layout do site.


[![Adicionar uma página mestra chamado Site.master para o site](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**Figura 03**: adicionar um Site.master de chamada de página mestra para o site ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image9.png))


Defina o layout de página de todo o site aqui na página mestra. Você pode usar o modo de exibição de Design e adicionar qualquer controles de Layout ou Web necessários, ou você pode adicionar manualmente a marcação manualmente na exibição da fonte. Estruturado de layout da página Meu mestre para imitar o layout usado no meu  *[trabalhando com dados no ASP.NET 2.0](../../data-access/index.md)*  série de tutoriais (consulte a Figura 4). Usa a página mestra [folhas de estilo em cascata](http://www.w3schools.com/css/default.asp) para posicionamento e estilos com as configurações de CSS definidas no arquivo Style.css (que está incluído no download de associados neste tutorial). Enquanto você não pode dizer sobre a marcação mostrada abaixo, as regras de CSS são definidas, de modo que a navegação &lt;div&gt;do conteúdo é posicionado absolutamente para que ele é exibido à esquerda e tem uma largura fixa de 200 pixels.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

Uma página mestra define o layout de página estática e as regiões que podem ser editadas por páginas ASP.NET que usam a página mestra. Essas áreas de conteúdo editáveis são indicadas pelo controle ContentPlaceHolder, que pode ser visto no conteúdo &lt;div&gt;. Nossa página mestra tem um único ContentPlaceHolder (MainContent), mas a página mestra pode ter vários ContentPlaceHolders.

Com a marcação acima, a alternar para o modo de exibição de Design mostra o layout da página mestra. As páginas do ASP.NET que usem essa página mestre terá esse layout uniforme, com a capacidade de especificar a marcação para a região MainContent.


[![A página mestre, quando exibido por meio da exibição de Design](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**Figura 04**: A página mestre, quando exibido por meio do modo de Design ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>Criando páginas de conteúdo

Neste ponto, temos uma página Default.aspx em nosso site, mas não usa a página mestra que acabamos de criar. Embora seja possível manipular a marcação declarativa de uma página da web para usar uma página mestre, se a página não contém qualquer conteúdo ainda é mais fácil apenas excluí-la e adicioná-la novamente ao projeto, especificando a página mestra. Portanto, inicie excluindo Default.aspx do projeto.

Em seguida, clique com botão direito no nome do projeto no Gerenciador de soluções e escolha Adicionar um novo formulário da Web denominado Default. aspx. Neste momento, marque a caixa de seleção Selecionar página mestra e escolha a página mestra Site.master na lista.


[![Adicione uma nova página de Default.aspx escolhendo Selecionar uma página mestra](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**Figura 05**: adicionar um novo Default.aspx página Escolher para selecionar uma página mestra ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image15.png))


[![Use a página mestra Site.master](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**Figura 06**: Use a página mestra Site.master ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> Se você estiver usando o modelo de projeto de aplicativo Web a caixa de diálogo Adicionar Novo Item não inclui uma caixa de seleção Selecionar página mestra. Em vez disso, você precisa adicionar um item de tipo de formulário de conteúdo da Web. Depois de escolher a opção de formato de conteúdo da Web e clicar em Adicionar, Visual Studio exibirá o mesmo selecione um mestre de caixa de diálogo mostrada na Figura 6.


Marcação declarativa da nova página Default.aspx inclui apenas uma @Page diretiva especificando o caminho para o mestre de página de arquivo e um controle de conteúdo MainContent ContentPlaceHolder da página mestra.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

Por enquanto, deixe Default.aspx vazio. Podemos retornará a ele mais tarde neste tutorial para adicionar conteúdo.

> [!NOTE]
> Nossa página mestre inclui uma seção de um menu ou outra interface de navegação. Vamos criar tal interface em um tutorial futuras.


## <a name="step-2-enabling-forms-authentication"></a>Etapa 2: Habilitar a autenticação de formulários

Com o site da Web ASP.NET criado, nossa próxima tarefa é habilitar a autenticação de formulários. Configuração de autenticação do aplicativo é especificada por meio de [ &lt;autenticação&gt; elemento](https://msdn.microsoft.com/en-us/library/532aee0e.aspx) no Web. config. O &lt;autenticação&gt; elemento contém um único atributo chamado de modo que especifica o modelo de autenticação usado pelo aplicativo. Esse atributo pode ter um dos quatro valores a seguir:

- **Windows** - conforme discutido no tutorial anterior, quando um aplicativo usa a autenticação do Windows é responsabilidade do servidor web para autenticar o visitante e, geralmente, isso é feito por meio de básica, Digest ou integrada do Windows autenticação.
- **Formulários**-os usuários são autenticados por meio de um formulário em uma página da web.
- **Passport**-os usuários são autenticados usando Passport Network do Microsoft.
- **Nenhum**-nenhum modelo de autenticação é usado, todos os visitantes são anônimos.

Por padrão, os aplicativos ASP.NET usam a autenticação do Windows. Para alterar o tipo de autenticação para a autenticação de formulários, em seguida, é preciso modificar o &lt;autenticação&gt; atributo de modo do elemento para formulários.

Se seu projeto ainda não contém um arquivo Web. config, adicione um agora clicando no nome do projeto no Gerenciador de soluções, escolha Adicionar Novo Item e, em seguida, adicionar um arquivo de configuração da Web.


[![Se seu projeto ainda não inclui Web. config, adicione-o agora](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**Figura 07**: se o projeto Does não ainda incluir Web. config, adicione agora ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image21.png))


Em seguida, localize o &lt;autenticação&gt; elemento e atualize-o para usar a autenticação de formulários. Após essa alteração, a marcação do seu arquivo Web. config deve ser semelhante ao seguinte:

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Como o Web. config é um arquivo XML, maiusculas e minúsculas é importante. Certifique-se de que você defina o atributo de modo a formulários, com uma letra maiuscula F. Se você usar uma diferenciam maiusculas de minúsculas, como formulários, você receberá um erro de configuração ao visitar o site por meio de um navegador.


O &lt;autenticação&gt; elemento opcionalmente pode incluir um &lt;formulários&gt; elemento filho que contém configurações específicas de autenticação de formulários. Por enquanto, vamos usar apenas as configurações de autenticação de formulários padrão. Iremos explorar o &lt;formulários&gt; elemento filho com mais detalhes na próximo tutorial.

## <a name="step-3-building-the-login-page"></a>Etapa 3: Criar a página de logon

Para dar suporte à autenticação de formulários nosso site precisa de uma página de logon. Conforme discutido no entendimento a seção do fluxo de trabalho de autenticação de formulários, FormsAuthenticationModule redirecionará automaticamente o usuário para a página de logon se tentarem acessar uma página que não são autorizado a exibir. Também há controles da Web do ASP.NET que serão exibido um link para a página de logon para usuários anônimos. Isso levanta a questão, o que é a URL da página de logon?

Por padrão, o sistema de autenticação de formulários espera a página de logon seja nomeada como aspx e colocado no diretório raiz do aplicativo web. Se você deseja usar uma URL de página de logon diferente, você pode fazer isso especificando-o no Web. config. Veremos como fazer isso no tutorial subsequente.

A página de logon tem três responsabilidades:

1. Fornece uma interface que permite que o visitante inserir suas credenciais.
2. Determine se as credenciais enviadas são válidas.
3. Logon do usuário da criando tíquete de autenticação de formulários.

### <a name="creating-the-login-pages-user-interface"></a>Criar Interface do usuário da página de logon

Vamos começar com a primeira tarefa. Adicionar uma nova página ASP.NET para o diretório do site raiz denominado aspx e associá-la com a página mestra Site.master.


[![Adicionar uma nova página ASP.NET chamada Login. aspx](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**Figura 08**: adicionar um novo ASP.NET página chamada aspx ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image24.png))


A interface de página de logon típico consiste em duas caixas de texto - uma para o nome do usuário, uma de suas senhas - e um botão para enviar o formulário. Sites muitas vezes, incluem um checkbox lembrar-me que, se marcada, persiste o tíquete de autenticação resultante entre as reinicializações de navegador.

Adicione duas caixas de texto para Login.aspx e definir suas propriedades de ID de usuário e senha, respectivamente. Também defina a propriedade TextMode da senha para a senha. Em seguida, adicione um controle de caixa de seleção, definindo sua propriedade ID para RememberMe e sua propriedade Text para lembrar-me. Depois disso, adicione um botão chamado LoginButton cuja propriedade de texto é definida para logon. Finalmente, adicione um controle de rótulo Web e defina sua propriedade ID para InvalidCredentialsMessage, sua propriedade de texto para o nome de usuário ou senha é inválida. Tente novamente., sua propriedade ForeColor para vermelho e sua propriedade Visible como False.

Neste ponto, sua tela deve ser semelhante para a tela na Figura 9 e sintaxe declarativa da página deve estar como o seguinte:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![A página de logon contém duas caixas de texto, uma caixa de seleção, um botão e um rótulo](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**Figura 09**: O logon página contém duas caixas de texto, uma caixa de seleção, um botão e um rótulo ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image27.png))


Finalmente, crie um manipulador de eventos de clique do LoginButton evento. No Designer, simplesmente clique duas vezes no controle de botão para criar este manipulador de eventos.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Determinando se as credenciais fornecidas são válidas

Agora, precisamos implementar tarefa 2 clique do botão manipulador de eventos - determinando se as credenciais fornecidas são válidas. Para fazer isso deve ser um repositório do usuário que contém todas as credenciais dos usuários para que possamos determinar se as credenciais fornecidas correspondem a quaisquer credenciais conhecidas.

Antes do ASP.NET 2.0, os desenvolvedores foram responsáveis por implementar os seus próprios armazenamentos de usuário e escrever o código para validar as credenciais fornecidas no repositório. A maioria dos desenvolvedores implementaria o armazenamento do usuário em um banco de dados, criando uma tabela chamada usuários com colunas como nome de usuário, senha, Email, LastLoginDate e assim por diante. Esta tabela, em seguida, teria um registro por conta de usuário. Verificação de credenciais fornecido do usuário envolve a consultar o banco de dados para um nome de usuário correspondente e, em seguida, garantindo que a senha no banco de dados correspondia à senha fornecida.

Com o ASP.NET 2.0, os desenvolvedores devem usar um dos provedores de associação para gerenciar o armazenamento do usuário. Este tutorial série usaremos SqlMembershipProvider, que usa um banco de dados do SQL Server para o repositório do usuário. Ao usar o SqlMembershipProvider é necessário para implementar um esquema de banco de dados específico que inclui as tabelas, exibições e procedimentos armazenados esperados pelo provedor. Vamos examinar como implementar esse esquema de  *[criar o esquema de associação no SQL Server](../membership/creating-the-membership-schema-in-sql-server-vb.md)*  tutorial. Com o provedor de associação em vigor, validar as credenciais do usuário é tão simple quanto chamar o [classe associação](https://msdn.microsoft.com/en-us/library/system.web.security.membership.aspx)do [ValidateUser (*username*, *senha*) método](https://msdn.microsoft.com/en-us/library/system.web.security.membership.validateuser.aspx), que retorna um valor booliano que indica se a validade do *username* e *senha* combinação. Como podemos ainda não implementado repositório do usuário do SqlMembershipProvider, não podemos usar ValidateUser método a classe de associação neste momento.

Em vez de levar um tempo para criar nossa própria personalizada tabela de banco de dados de usuários (o que seria obsoleta quando implementamos o SqlMembershipProvider), vamos em vez disso, codificar as credenciais válidas dentro de logon da página em si. No LoginButton manipulador de evento, adicione o seguinte código:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

Como você pode ver, há três contas de usuário válidas Scott, Jisun e Sam - e todas as três têm a mesma senha (senha). O código percorre os conjuntos de usuários e senhas, procurando uma correspondência de nome de usuário e senha válida. Se o nome de usuário e a senha forem válidos, precisamos fazer logon do usuário e, em seguida, redirecioná-los para a página apropriada. Se as credenciais são inválidas, podemos exibir o rótulo de InvalidCredentialsMessage.

Quando um usuário insere credenciais válidas, mencionei que eles são redirecionados para a página apropriada. O que é a página apropriada, embora? Lembre-se de que quando um usuário acessa uma página que não estão autorizados a exibir, FormsAuthenticationModule redireciona automaticamente para a página de logon. Dessa forma, ele inclui a URL solicitada na querystring por meio do parâmetro ReturnUrl. Ou seja, se um usuário tentou visitar ProtectedPage.aspx, e eles não foram autorizados a fazer isso, o FormsAuthenticationModule seria redirecioná-los para:

Aspx? ReturnUrl=ProtectedPage.aspx

Após fazer logon com êxito, o usuário deve ser redirecionado para ProtectedPage.aspx. Como alternativa, os usuários podem visitar a página de logon em seu próprios volition. Nesse caso, após o logon do usuário elas devem ser enviadas para a página de Default.aspx da pasta raiz.

### <a name="logging-in-the-user"></a>Logon do usuário

Supondo que as credenciais fornecidas são válidas, precisamos criar um tíquete de autenticação de formulários, registro em log, portanto, o usuário para o site. O [classe FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.aspx) no [namespace System.Web.Security](https://msdn.microsoft.com/en-us/library/system.web.security.aspx) fornece diversos métodos para fazer logon no e fazer logoff de usuários por meio de formulários de sistema de autenticação. Existem vários métodos na classe FormsAuthentication, três que estamos interessados em atualmente são:

- [GetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.getauthcookie.aspx) -cria um tíquete de autenticação de formulários para o nome fornecido *nome de usuário*. Em seguida, esse método cria e retorna um objeto HttpCookie que contém o conteúdo do tíquete de autenticação. Se *persistCookie* for True, um cookie persistente é criado.
- [SetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.setauthcookie.aspx) -chama o GetAuthCookie (*username*, *persistCookie*) método para gerar o cookie de autenticação de formulários. Este método adiciona o cookie retornado por GetAuthCookie à coleção de Cookies (supondo que a autenticação de formulários baseados em cookies está sendo usado; caso contrário, este método chama uma classe interna que lida com a lógica de tíquete cookieless).
- [RedirectFromLoginPage (*username*, *persistCookie*)](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) -este método chama SetAuthCookie (*username*, *persistCookie* ) e, em seguida, redireciona o usuário para a página apropriada.

GetAuthCookie é útil quando você precisa modificar o tíquete de autenticação antes de gravar o cookie de para a coleção de Cookies. SetAuthCookie é útil se você deseja criar formulários de tíquete de autenticação e adicioná-lo à coleção de Cookies, mas não deseja redirecionar o usuário para a página apropriada. Talvez você queira mantê-los na página de logon ou enviá-los para alguma página alternativa.

Como desejamos fazer logon do usuário e redirecioná-los para a página apropriada, vamos usar RedirectFromLoginPage. Atualização clique do LoginButton manipulador de eventos, substituindo as duas linhas TODO comentadas com a seguinte linha de código:

RedirectFromLoginPage (UserName.Text, RememberMe.Checked)

Ao criar tíquete de autenticação de formulários, usamos a propriedade de texto de UserName TextBox para o tíquete de autenticação de formulários *username* parâmetro e o estado de ativação de RememberMe CheckBox para o  *persistCookie* parâmetro.

Para testar a página de logon, visite-o em um navegador. Inicie inserindo credenciais inválidas, como um nome de usuário de Nope e uma senha incorreta. Ao clicar no botão de logon um postback ocorrerá e o rótulo de InvalidCredentialsMessage será exibido.


[![O rótulo de InvalidCredentialsMessage é exibido ao inserir credenciais inválidas](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**Figura 10**: O rótulo de InvalidCredentialsMessage é exibido ao inserir credenciais inválidas ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image30.png))


Em seguida, insira credenciais válidas e clique no botão de logon. Neste momento quando o postback ocorre um tíquete de autenticação de formulários é criado e você será redirecionado automaticamente volta para Default.aspx. Neste ponto você fez site, embora não haja nenhum indicações visuais para indicar que você fez. Na etapa 4, que veremos como determinar programaticamente se um usuário é registrado no ou não, bem como a identificar o usuário visitar a página.

Etapa 5 examina técnicas para registrar um usuário desconectado do site.

### <a name="securing-the-login-page"></a>Protegendo a página de logon

Quando o usuário insere suas credenciais e envia o formulário da página de logon, as credenciais (incluindo sua senha) são transmitidas pela Internet para o servidor web no *texto sem formatação*. Isso significa que qualquer hacker detecção o tráfego de rede pode ver o nome de usuário e senha. Para evitar isso, é essencial para criptografar o tráfego de rede usando [camadas de soquete seguro (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Isso garantirá que as credenciais (bem como uma marcação HTML da página inteira) é criptografada desde o momento em que ele deixar o navegador até que eles sejam recebidos pelo servidor web.

A menos que seu site contiver informações confidenciais, você só precisará usar SSL na página de logon e em outras páginas em que a senha do usuário caso contrário, seria enviada eletronicamente em texto sem formatação. Você não precisa se preocupar sobre como proteger o tíquete de autenticação de formulários como, por padrão, ele é criptografado e assinado digitalmente (para evitar a violação). Uma discussão mais completa sobre segurança de tíquete de autenticação de formulários é apresentada no tutorial a seguir.

> [!NOTE]
> Muitos sites financeiros e de médicos estão configurados para usar SSL *todos os* páginas acessíveis para a usuários autenticados. Se você estiver criando esse tipo de site, você pode configurar o sistema de autenticação de formulários para que o tíquete de autenticação de formulários só é transmitido por uma conexão segura. Vamos examinar as várias opções de configuração de autenticação de formulários no tutorial de Avançar,  *[configuração de autenticação de formulários e tópicos avançados](../membership/creating-the-membership-schema-in-sql-server-vb.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Etapa 4: Detectando visitantes autenticados e determinar sua identidade

Agora temos habilitada a autenticação de formulários e criou uma página de logon rudimentar, mas ainda precisamos examinar como é possível determinar se um usuário é autenticado ou anônimo. Em determinados cenários que desejamos exibir diferentes dados ou informações dependendo se um usuário autenticado ou anônimo está visitando a página. Além disso, muitas vezes, é necessário saber a identidade do usuário autenticado.

Vamos ampliar a página Default.aspx existente para ilustrar as técnicas. Em Default.aspx, adicione dois controles de painel, um AuthenticatedMessagePanel nomeado e outro AnonymousMessagePanel nomeado. Adicione um controle de rótulo chamado WelcomeBackMessage no primeiro painel. No segundo painel Adicionar um controle de hiperlink, defina sua propriedade Text para Log e sua propriedade NavigateUrl para ~ / Login.aspx. Neste momento a marcação declarativa para Default.aspx deve ser semelhante ao seguinte:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

Como você provavelmente esperar até o momento, a ideia aqui é exibir apenas o AuthenticatedMessagePanel visitantes autenticados e apenas o AnonymousMessagePanel para visitantes anônimos. Para fazer isso, precisamos definir a propriedade visível esses painéis dependendo se o usuário estiver conectado ou não.

O [Request.IsAuthenticated propriedade](https://msdn.microsoft.com/en-us/library/system.web.httprequest.isauthenticated.aspx) retorna um valor booliano que indica se a solicitação foi autenticada. Insira o código a seguir para a página\_carregar o código de manipulador de eventos:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

Com esse código, visite Default.aspx através de um navegador. Supondo que você ainda precisa fazer logon, você verá um link para a página de logon (consulte a Figura 11). Clique neste link e fazer logon site. Como vimos na etapa 3, depois de inserir suas credenciais você será retornado como Default. aspx, mas neste momento a página mostra o bem-vindo novamente! mensagem (veja a Figura 12).


[![Quando visitar anonimamente, em um Link do Log é exibida](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**Figura 11**: quando visitar anonimamente, em um Link do Log é exibido ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image33.png))


[![Os usuários autenticados são mostrados o bem-vindo novamente! Mensagem](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**Figura 12**: usuários autenticados são mostrados o bem-vindo novamente! Mensagem ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image36.png))


É possível determinar a identidade do usuário conectado no momento por meio de [objeto HttpContext](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.aspx)do [propriedade do usuário](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.user.aspx). O objeto HttpContext representa informações sobre a solicitação atual e é a página inicial para esses objetos comuns do ASP.NET como resposta e solicitação de sessão, entre outros. A propriedade de usuário representa o contexto de segurança da solicitação HTTP atual e implementa o [interface IPrincipal](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.aspx).

A propriedade do usuário é definida pelo FormsAuthenticationModule. Especificamente, quando o FormsAuthenticationModule encontra um tíquete de autenticação de formulários na solicitação de entrada, ele cria um novo objeto GenericPrincipal e atribui a propriedade do usuário.

Objetos (como GenericPrincipal) fornecem informações sobre a identidade do usuário e as funções às quais eles pertencem. A interface IPrincipal define dois membros:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.isinrole.aspx) -um método que retorna um valor booliano que indica se a entidade de segurança pertence à função especificada.
- [Identidade](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.identity.aspx) -uma propriedade que retorna um objeto que implementa o [IIdentity interface](https://msdn.microsoft.com/en-us/library/system.security.principal.iidentity.aspx). A interface de IIdentity define três propriedades: [AuthenticationType](https://msdn.microsoft.com/en-us/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/en-us/library/system.security.principal.iidentity.isauthenticated.aspx), e [nome](https://msdn.microsoft.com/en-us/library/system.security.principal.iidentity.name.aspx).

É possível determinar o nome do visitante atual usando o seguinte código:

Dim currentUsersName As String = User.Identity.Name

Quando usar formulários de autenticação, um [FormsIdentity objeto](https://msdn.microsoft.com/en-us/library/system.web.security.formsidentity.aspx) é criado para a propriedade de identidade do GenericPrincipal. A classe FormsIdentity sempre retorna os formulários de cadeia de caracteres de sua propriedade AuthenticationType e True para sua propriedade IsAuthenticated. A propriedade Name retorna o nome de usuário especificado ao criar tíquete de autenticação de formulários. Além dessas três propriedades, FormsIdentity inclui acesso para o tíquete de autenticação subjacente por meio de seu [tíquete propriedade](https://msdn.microsoft.com/en-us/library/system.web.security.formsidentity.ticket.aspx). A propriedade de tíquete retorna um objeto do tipo [FormsAuthenticationTicket](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationticket.aspx), que tem propriedades, como a expiração, IsPersistent, IssueDate, nome e assim por diante.

O ponto importante a ser considerado aqui é que o *username* parâmetro especificado no FormsAuthentication.GetAuthCookie (*username*, *persistCookie*), FormsAuthentication.SetAuthCookie (*username*, *persistCookie*) e RedirectFromLoginPage (*nome de usuário*, *persistCookie*) métodos é o mesmo valor retornado por User.Identity.Name. Além disso, o tíquete de autenticação criado por esses métodos está disponível projeção Identity para um objeto FormsIdentity e, em seguida, acessando a propriedade de permissão:

Dim ident como FormsIdentity = CType (Identity, FormsIdentity)

Dim authTicket como FormsAuthenticationTicket = ident. Tíquete

Vamos fornecer uma mensagem mais personalizada em Default.aspx. Atualizar a página\_carregar o manipulador de eventos para que a propriedade Text do rótulo WelcomeBackMessage é atribuída a cadeia de caracteres de boas-vindas, *username*!

WelcomeBackMessage.Text = "Bem-vindo novamente," &amp; User.Identity.Name &amp; "!"

Figura 13 mostra o efeito dessa modificação (efetuar logon como usuário Scott).


[![A mensagem de boas-vinda inclui o conectado no momento em nome do usuário](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**Figura 13**: A mensagem de boas-vinda inclui nome do usuário no registrados atualmente ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>Usando os controles de LoginName e LoginView

Exibir conteúdo diferente para usuários autenticados e anônimos é um requisito comum; para que está exibindo o nome do usuário conectado no momento. Por esse motivo, o ASP.NET inclui dois controles da Web que fornecem a mesma funcionalidade mostrada na Figura 13, mas sem a necessidade de escrever uma única linha de código.

O [controle LoginView](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.loginview.aspx) é um controle de Web baseado em modelo que facilita a exibição de dados para usuários anônimos e autenticados. O LoginView inclui dois modelos predefinidos:

- LoginView - qualquer marcação adicionada a este modelo é exibida apenas para visitantes anônimos.
- LoggedInTemplate - marcação deste modelo é mostrado somente para usuários autenticados.

Vamos adicionar o controle LoginView a página mestra do nosso site, Site.master. Em vez de adicionar apenas o controle LoginView, porém, vamos adicionar um novo controle ContentPlaceHolder e, em seguida, coloque o controle LoginView dentro desse novo ContentPlaceHolder. A razão para essa decisão se tornará aparente em breve.

> [!NOTE]
> Além de LoginView e LoggedInTemplate, o controle LoginView pode incluir modelos específicos de função. Modelos específicos de função mostram marcação somente para os usuários que pertencem a uma função especificada. Vamos examinar os recursos baseados em função do controle LoginView em um tutorial futuras.


Comece adicionando um ContentPlaceHolder chamado LoginContent para a página mestra no painel de navegação &lt;div&gt; elemento. Você pode simplesmente arraste um controle ContentPlaceHolder da caixa de ferramentas para a exibição da fonte, colocando a marcação resultante logo acima do TODO: Menu ir aqui texto.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

Em seguida, adicione um controle LoginView dentro do LoginContent ContentPlaceHolder. Conteúdo colocado em controles ContentPlaceHolder da página mestra são considerados *conteúdo padrão* para o ContentPlaceHolder. Ou seja, páginas ASP.NET que usam essa página mestre podem especificar seu próprio conteúdo para cada ContentPlaceHolder ou usar conteúdo padrão da página mestra.

O LoginView e outros controles de logon estão localizados no guia de logon da caixa de ferramentas.


[![O controle LoginView na caixa de ferramentas](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**Figura 14**: O controle LoginView na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image42.png))


Em seguida, adicione dois &lt;br /&gt; elementos imediatamente após o controle LoginView, mas ainda dentro do ContentPlaceHolder. Neste ponto, a navegação &lt;div&gt; marcação do elemento deve ser semelhante ao seguinte:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

Modelos do LoginView podem ser definidos pelo Designer ou a marcação declarativa. No Designer do Visual Studio, expanda marca inteligente do LoginView, que lista os modelos configurados em uma lista suspensa. Digite o texto Hello, estranho em LoginView; em seguida, adicionar um controle de hiperlink e defina suas propriedades de texto e NavigateUrl para logon e ~ / aspx, respectivamente.

Depois de configurar o LoginView, alterne para o LoggedInTemplate e insira o texto, "Bem-vindo de volta,". Em seguida, arraste um controle LoginName da caixa de ferramentas para LoggedInTemplate, colocando-o imediatamente após "Bem-vindo," texto. O [controle LoginName](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.loginname.aspx), como o nome indica, exibe o nome do usuário conectado no momento. Internamente, o controle LoginName simplesmente gera a propriedade User.Identity.Name

Depois de fazer essas adições aos modelos do LoginView, a marcação deve ser semelhante ao seguinte:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Com essa adição à página mestra Site.master, cada página no nosso site exibirá uma mensagem diferente dependendo se o usuário é autenticado. Figura 15 mostra a página de Default.aspx quando visitado por meio de um navegador pelo usuário Jisun. Bem-vindo trás Jisun mensagem é repetida duas vezes: uma vez na seção de navegação da página mestra à esquerda (por meio do controle LoginView que acabamos de adicionar) e uma vez em que o Default.aspx conteúdo área (por meio de controles de painel e lógica de programação).


[![Bem-vindo exibe de controle LoginView back, Jisun.](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**Figura 15**: O LoginView controle exibe bem-vindo novamente, Jisun. ([Clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image45.png))


Como adicionamos a LoginView para a página mestra, ele pode aparecer em cada página no site. No entanto, pode haver páginas da web onde não queremos mostrar esta mensagem. Uma página desse tipo é a página de logon, como um link para a página de logon parece fora do local existe. Já que é colocado no controle LoginView em um ContentPlaceHolder na página mestra, podemos substituir essa marcação padrão em nossa página de conteúdo. Abra aspx e vá para o Designer. Como podemos não tiver definido um controle de conteúdo explicitamente aspx para o LoginContent ContentPlaceHolder na página mestra, a página de logon mostrará marcação de padrão da página mestra para esse ContentPlaceHolder. Você pode ver isso por meio do Designer - o LoginContent ContentPlaceHolder mostra a marcação padrão (o controle LoginView).


[![A página de logon mostra o padrão conteúdo de ContentPlaceHolder de LoginContent da página mestra](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**Figura 16**: A página de logon mostra o conteúdo de padrão de LoginContent ContentPlaceHolder's Page the Master ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image48.png))


Para substituir a marcação padrão para o LoginContent ContentPlaceHolder, clique na região no Designer e escolha a opção de criar conteúdo personalizado no menu de contexto. (Quando usar o Visual Studio 2008 a ContentPlaceHolder inclui uma marca inteligente que, quando selecionado, oferece a mesma opção.) Isso adiciona um novo controle de conteúdo para a marcação da página e, portanto, permite definir o conteúdo personalizado para essa página. Você pode adicionar uma mensagem personalizada aqui, como faça o logon, mas vamos apenas deixe em branco.

> [!NOTE]
> No Visual Studio 2005, criação de conteúdo personalizado cria vazio conteúdo controle em uma página ASP.NET. No Visual Studio 2008, no entanto, criar conteúdo personalizado copia conteúdo padrão da página mestra para o controle de conteúdo recém-criado. Se você estiver usando o Visual Studio 2008, em seguida, depois de criar o novo controle de conteúdo Certifique-se de apagar o conteúdo copiado da página mestra.


A Figura 17 mostra a página de Login.aspx quando visitado em um navegador depois de fazer essa alteração. Observe que não há nenhum Olá estranho ou bem-vindo novamente, *username* mensagem no painel de navegação esquerdo &lt;div&gt; que ocorrem quando visitar Default.aspx.


[![A página de logon oculta marcação de LoginContent ContentPlaceHolder o padrão](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**Figura 17**: A página de logon oculta marcação do padrão LoginContent ContentPlaceHolder ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>Etapa 5: Logout

Na etapa 3, examinamos a criação de uma página de logon para o logon de um usuário para o site, mas ainda precisamos saber como um usuário de logoff. Além dos métodos de registro em log de um usuário, a classe FormsAuthentication também fornece um [método SignOut](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.signout.aspx). O método SignOut simplesmente destrói o tíquete de autenticação de formulários, log, portanto, o usuário do site.

Oferecendo que um link de logoff é tal recurso comuns que o ASP.NET inclui um controle de especificamente projetado para um usuário de logoff. O [controle de status de logon](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.loginstatus.aspx) exibe um LinkButton de logon ou Logout LinkButton, dependendo do status de autenticação do usuário. Um LinkButton de logon é renderizado para usuários anônimos, enquanto um Logout LinkButton é exibido aos usuários autenticados. O texto para o logon e Logout LinkButtons pode ser configurado por meio do status de logon propriedades LoginText e LogoutText.

Clicar no LinkButton logon causa um postback, do qual um redirecionamento é emitido para a página de logon. Clicar no Logout LinkButton faz com que o controle de status de logon invocar o método FormsAuthentication.SignOff e, em seguida, redireciona o usuário para uma página. O log de página de logoff de usuário é redirecionado para depende da propriedade LogoutAction, que pode ser atribuída a um destes três valores:

- Atualize - o padrão. redireciona o usuário para a página que eles foram apenas a visita. Se a página que estavam visitando apenas não permitir que usuários anônimos, em seguida, FormsAuthenticationModule redirecionará automaticamente o usuário para a página de logon.

Você pode estar curioso sobre por que um redirecionamento é executado aqui. Se o usuário quer permanecerão na mesma página, por que a necessidade do redirecionamento explícita? O motivo é que quando o Logoff LinkButton é clicado, o usuário ainda tem o tíquete de autenticação de formulários em sua coleção de cookies. Portanto, a solicitação de postback é uma solicitação autenticada. O controle de status de logon chama o método SignOut, mas o que acontece depois que o FormsAuthenticationModule autenticou o usuário. Portanto, um redirecionamento explícito faz com que o navegador para a página de solicitação novamente. No momento em que o navegador novamente a página de solicitações, o tíquete de autenticação de formulários foi removido e, portanto, a solicitação de entrada é anônima.

- Redirecione - o usuário é redirecionado para a URL especificada pela propriedade de LogoutPageUrl do status de logon.
- RedirectToLoginPage - o usuário é redirecionado para a página de logon.

Vamos adicionar um controle de status de logon para a página mestra e configurá-lo para usar a opção de redirecionamento para enviar o usuário para uma página que exibe uma mensagem confirmando que eles foi desconectados. Comece criando uma página no diretório raiz chamado Logout.aspx. Não se esqueça de associar essa página a página mestra Site.master. Em seguida, digite uma mensagem na marcação da página explicando para o usuário que foi desconectados.

Em seguida, retornar para a página mestra Site.master e adicionar um controle de status de logon sob o LoginView no LoginContent ContentPlaceHolder. Defina propriedade de LogoutAction do controle do status de logon para redirecionamento e sua propriedade LogoutPageUrl para ~/Logout.aspx.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

Como o status de logon está fora do controle LoginView, ele será exibido para usuários anônimos e autenticados, mas isso é Okey porque o status de logon corretamente exibirá um logon ou Logout LinkButton. Com a adição do controle de status de logon, o Log no hiperlink de LoginView é supérfluo, então removê-lo.

A Figura 18 mostra Default.aspx quando Jisun visita. Observe que a coluna da esquerda exibe a mensagem, de boas-vindas, Jisun junto com um link para fazer logoff. Clicando o log de saída LinkButton causa um postback, assina Jisun do sistema e, em seguida, redireciona ela para Logout.aspx. Como mostra a Figura 19, no momento que jisun atingir Logout.aspx ela já foi desconectada e, portanto, é anônimo. Consequentemente, a coluna esquerda mostra o texto de boas-vindas, desconhecido e um link para a página de logon.


[![Default.aspx mostra bem-vindo novamente, Jisun juntamente com um Logout LinkButton](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**Figura 18**: Default.aspx mostra boas-vindas novamente, Jisun juntamente com um Logout LinkButton ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout.aspx mostra bem-vindo, estranho junto com um LinkButton de logon](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**Figura 19**: Logout.aspx mostra bem-vindo, estranho junto com um logon LinkButton ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> Recomendo que você personalize a página Logout.aspx para ocultar LoginContent ContentPlaceHolder da página mestra (como fizemos aspx na etapa 4). O motivo é que o logon LinkButton processada pelo controle status de logon (aquele abaixo Hello, estranho) envia o usuário para a página de logon passando a URL atual no parâmetro querystring ReturnUrl. Resumindo, se um usuário que tiver feito logon fora clica LinkButton de logon desse status de logon e de login, ele serão redirecionado para Logout.aspx, que pode facilmente confundir o usuário.


## <a name="summary"></a>Resumo

Neste tutorial de Introdução com um exame do fluxo de trabalho de autenticação de formulários e ativado para implementar a autenticação de formulários em um aplicativo ASP.NET. Autenticação de formulários é alimentada por FormsAuthenticationModule, que tem duas responsabilidades: identificar os usuários com base em seu tíquete de autenticação de formulários e redirecionar usuários não autorizados para a página de logon.

A classe do .NET Framework FormsAuthentication inclui métodos para criação, inspeção e remoção de tíquetes de autenticação de formulários. A propriedade Request.IsAuthenticated e o objeto de usuário fornecem suporte adicional para determinar se uma solicitação é autenticada e informações sobre a identidade do usuário. Também são os controles LoginName Web, LoginView e status de logon, que oferecem aos desenvolvedores uma maneira rápida e sem código para executar muitas tarefas comuns relacionadas ao logon. Vamos examinar esses e outros controles da Web relacionadas a logon mais detalhadamente em tutoriais futuros.

Este tutorial fornecida uma visão geral básica da autenticação de formulários. Nós não examine as opções de configuração variados, examinar como cookieless trabalho de tíquetes de autenticação de formulários ou explorar como ASP.NET protege o conteúdo do tíquete de autenticação de formulários. Abordaremos esses tópicos e muito mais no [tutorial próximo](forms-authentication-configuration-and-advanced-topics-vb.md).

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Alterações entre IIS6 e segurança do IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Controles de logon do ASP.NET](https://msdn.microsoft.com/en-us/library/d51ttbhx.aspx)
- [Professional ASP.NET 2.0 segurança, associação e gerenciamento de função](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [O &lt;autenticação&gt; elemento](https://msdn.microsoft.com/en-us/library/532aee0e.aspx)
- [O &lt;formulários&gt; elemento para &lt;autenticação&gt;](https://msdn.microsoft.com/en-us/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Treinamento em vídeo sobre tópicos contidos neste tutorial

- [Usando a autenticação de formulários básicos no ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial incluem Alicja Maziarz, John Suru e Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Anterior](security-basics-and-asp-net-support-vb.md)
[Próximo](forms-authentication-configuration-and-advanced-topics-vb.md)
