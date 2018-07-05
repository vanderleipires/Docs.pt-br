---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: Uma visão geral da autenticação de formulários (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial nós o transformaremos da discussão mero para implementação; em particular, vamos examinar a implementação da autenticação de formulários. W o aplicativo web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: df8b0595cbc02a99a39c5b39be9ddb2e92bc34aa
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385885"
---
<a name="an-overview-of-forms-authentication-vb"></a>Uma visão geral da autenticação de formulários (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> Neste tutorial nós o transformaremos da discussão mero para implementação; em particular, vamos examinar a implementação da autenticação de formulários. O aplicativo web que podemos começar a construir neste tutorial continuará a ser construído nos tutoriais subsequentes, conforme movemos da autenticação de formulários simples para associações e funções.
> 
> Consulte este vídeo para obter mais informações sobre este tópico: [usando autenticação de formulários básica no ASP.NET](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Introdução

No [tutorial anterior](security-basics-and-asp-net-support-vb.md) discutimos as várias autenticação, autorização e usuário opções de conta fornecidas pelo ASP.NET. Neste tutorial nós o transformaremos da discussão mero para implementação; em particular, vamos examinar a implementação da autenticação de formulários. O aplicativo web que podemos começar a construir neste tutorial continuará a ser construído nos tutoriais subsequentes, conforme movemos da autenticação de formulários simples para associações e funções.

Este tutorial começa com um olhar detalhado sobre o fluxo de trabalho de autenticação de formulários, um tópico que abordamos após no tutorial anterior. Depois disso, vamos criar um site do ASP.NET por meio do qual os conceitos de autenticação de formulários de demonstração. Em seguida, configuraremos o site para usar autenticação de formulários, criar uma página de logon simples e veja como determinar, no código, se um usuário é autenticado e, nesse caso, o nome de usuário está conectado.

Noções básicas sobre os formulários de fluxo de trabalho de autenticação, habilitá-la em um aplicativo web e criar as páginas de logon e logoff são vitais todas as etapas da criação de um aplicativo ASP.NET que oferece suporte a contas de usuário e autentica usuários por meio de uma página da web. Por isso - e porque esses tutoriais aproveitam uma outra - recomendo que você trabalhe com este tutorial completo antes de passar para o próximo, mesmo se você já tenha tido a experiência de configurar a autenticação de formulários em projetos anteriores.

## <a name="understanding-the-forms-authentication-workflow"></a>Noções básicas sobre o fluxo de trabalho de autenticação de formulários

Quando o tempo de execução do ASP.NET processa uma solicitação para um recurso do ASP.NET, como uma página ASP.NET ou serviço da Web do ASP.NET, a solicitação eleva um número de eventos durante seu ciclo de vida. Há eventos no final muito iniciante e muito da solicitação, aqueles gerado quando a solicitação está sendo autenticada e autorizado, um evento gerado no caso de uma exceção sem tratamento e assim por diante. Para ver uma listagem completa dos eventos, consulte o [eventos do objeto HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*Módulos HTTP* são classes gerenciadas cujo código é executado em resposta a um evento específico no ciclo de vida de solicitação. ASP.NET é fornecido com um número de módulos HTTP que realizar tarefas essenciais em segundo plano. Dois módulos internos do HTTP são especialmente relevantes para nossa discussão são:

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  -autentica o usuário inspecionando o tíquete de autenticação de formulários, que normalmente é incluído na coleção de cookies do usuário. Se nenhum tíquete de autenticação de formulários estiver presente, o usuário é anônimo.
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**  -determina se o usuário atual está autorizado a acessar a URL solicitada. Esse módulo determina a autoridade consultando as regras de autorização especificadas nos arquivos de configuração do aplicativo. O ASP.NET também inclui o [FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) que determina a autoridade consultando as ACLs de arquivo (s) solicitado.

O FormsAuthenticationModule tenta autenticar o usuário antes do UrlAuthorizationModule (e FileAuthorizationModule) em execução. Se o usuário que fez a solicitação não está autorizado a acessar o recurso solicitado, o módulo de autorização encerra a solicitação e retorna um [HTTP 401 não autorizado](http://www.checkupdown.com/status/E401.html) status. Em cenários de autenticação do Windows, o status HTTP 401 será retornado ao navegador. Esse código de status faz com que o navegador solicitar ao usuário as credenciais por meio de uma caixa de diálogo modal. Com a autenticação de formulários, no entanto, o status HTTP 401 não autorizado nunca é enviado para o navegador porque o FormsAuthenticationModule detecta esse status e o modifica para redirecionar o usuário para a página de logon, em vez disso (via um [deredirecionamentodoHTTP302](http://www.checkupdown.com/status/E302.html) status).

É responsabilidade da página de logon determinar se as credenciais do usuário são válidas e, nesse caso, para criar um tíquete de autenticação de formulários e redirecionar o usuário voltar à página estavam tentando executar a visitar. O tíquete de autenticação está incluído nas solicitações subsequentes para as páginas no site, que usa o FormsAuthenticationModule para identificar o usuário.


[![O fluxo de trabalho de autenticação de formulários](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**Figura 01**: O fluxo de trabalho de autenticação de formulários ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Lembrar-se o tíquete de autenticação em visitas à página

Depois de fazer logon, o tíquete de autenticação de formulários deve ser enviado para o servidor web em cada solicitação para que o usuário permaneça conectado conforme navegam pelo site. Normalmente, isso é feito colocando o tíquete de autenticação na coleção de cookies do usuário. [Cookies](http://en.wikipedia.org/wiki/HTTP_cookie) são pequenos arquivos de texto que residem no computador do usuário e são transmitidos em cabeçalhos HTTP em cada solicitação para o site que o criou. Portanto, depois que o tíquete de autenticação de formulários foi criado e armazenado em cookies do navegador, cada visita subsequente ao site envia o tíquete de autenticação junto com a solicitação, assim, identificando o usuário.

> [!NOTE]
> Aplicativo web de demonstração usado em cada tutorial está disponível como um download. Este aplicativo para download foi criado com o Visual Web Developer 2008 direcionado para o .NET Framework versão 3.5. Uma vez que o aplicativo se destina o .NET 3.5, o seu arquivo Web. config inclui elementos de configuração adicionais, específicos de 3.5. Encurtar a história, se você ainda precisa instalar o .NET 3.5 em seu computador, em seguida, o aplicativo web que pode ser baixado não funcionará sem primeiro remover a marcação específica do 3.5 da Web. config.


Um aspecto de cookies é sua expiração, o que é a data e hora em que o navegador descarta o cookie. Quando o cookie de autenticação de formulários expira, o usuário pode não ser autenticado e, portanto, se tornar anônimo. Quando um usuário está visitando em um terminal público, provavelmente que eles querem seu tíquete de autenticação expirar ao fechar seu navegador. Ao visitar em casa, no entanto, mesmo que o usuário poderá o tíquete de autenticação a ser lembrada entre as reinicializações do navegador para que eles não precisam fazer logon novamente cada vez que visitam o site. Essa decisão é geralmente feita pelo usuário na forma de um checkbox lembrar-me na página de logon. Etapa 3, vamos examinar como implementar uma caixa de seleção lembrar-me na página de logon. O tutorial a seguir aborda as configurações de tempo limite do tíquete de autenticação em detalhes.

> [!NOTE]
> É possível que o agente do usuário usado para fazer logon no site pode não oferecer suporte a cookies. Nesse caso, o ASP.NET pode usar os tíquetes de autenticação de formulários sem cookies. Nesse modo, o tíquete de autenticação é codificado na URL. Vamos examinar quando tíquetes de autenticação sem cookies são usados e como elas são criadas e gerenciadas no próximo tutorial.


### <a name="the-scope-of-forms-authentication"></a>O escopo de autenticação de formulários

O FormsAuthenticationModule é gerenciado de código que é parte do tempo de execução do ASP.NET. Antes da versão 7 da Microsoft [serviços de informações da Internet (IIS)](https://www.iis.net/) servidor web, não havia uma barreira distinta entre o pipeline HTTP do IIS e o pipeline do tempo de execução ASP.NET. Em resumo, no IIS 6 e versões anteriores, o FormsAuthenticationModule é executada apenas quando uma solicitação é delegada do IIS no tempo de execução do ASP.NET. Por padrão, o IIS processa conteúdo estático em si – como páginas HTML e CSS e arquivos de imagem – e entrega somente solicitações para o tempo de execução do ASP.NET, quando uma página com a extensão. aspx,. asmx ou. ashx é solicitada.

IIS 7, no entanto, permite integrado do IIS e ASP.NET pipelines. Com alguns parâmetros de configuração, você pode configurar o IIS 7 para invocar o FormsAuthenticationModule para *todos os* solicitações. Além disso, com o IIS 7, você pode definir regras de autorização de URL para arquivos de qualquer tipo. Para obter mais informações, consulte [alterações entre o IIS6 e segurança do IIS7](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [sua segurança da plataforma Web](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), e [Noções básicas de autorização de URL de IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Encurtar a história, em versões anteriores do IIS 7, você pode usar somente autenticação de formulários para proteger recursos controlados pelo runtime do ASP.NET. Da mesma forma, as regras de autorização de URL são aplicadas apenas para recursos controlados pelo runtime do ASP.NET. Mas com o IIS 7, é possível integrar o FormsAuthenticationModule e UrlAuthorizationModule ao pipeline de HTTP do IIS, estendendo, assim, essa funcionalidade a todas as solicitações.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Etapa 1: Criando um site ASP.NET para esta série de tutoriais

Para acessar o maior público possível, criaríamos em toda esta série de site do ASP.NET será criado com a versão gratuita da Microsoft do Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Nós implementaremos o repositório do usuário SqlMembershipProvider em uma [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) banco de dados. Se você estiver usando o Visual Studio 2005 ou uma edição diferente do Visual Studio 2008 ou SQL Server, não se preocupe – as etapas serão quase idênticas e quaisquer diferenças não triviais serão apontadas.

Antes que podemos configurar autenticação de formulários, primeiro precisamos de um site ASP.NET. Comece criando um novo arquivo com base no sistema site da Web ASP.NET. Para fazer isso, inicie o Visual Web Developer e vá para o menu Arquivo e escolha o novo Site, exibindo a caixa de diálogo Novo Site da Web. Escolha o modelo de Site da Web ASP.NET, defina a lista suspensa de local para o sistema de arquivos, escolha uma pasta para colocar o site da web e definir a linguagem para VB. Isso criará um novo site com uma página Default. aspx ASP.NET, um aplicativo\_pasta de dados e um arquivo Web. config.

> [!NOTE]
> Visual Studio dá suporte a dois modos de gerenciamento de projeto: os projetos de Site da Web e projetos de aplicativos Web. Projetos de Site não têm um arquivo de projeto, enquanto que o Web Application Projects imitar a arquitetura do projeto no Visual Studio .NET 2002/2003 – eles incluem um arquivo de projeto e compilar o código-fonte do projeto em um único assembly, que é colocado na pasta /bin. O Visual Studio 2005 inicialmente apenas sites da Web com suporte de projetos, embora o modelo de projeto de aplicativo Web foi reintroduzido com Service Pack 1. O Visual Studio 2008 oferece os dois modelos de projeto. Visual Web Developer 2005 e edições de 2008, no entanto, somente dão suporte a projetos de Site da Web. Usarei o modelo de projeto de Site. Se você estiver usando uma edição não Express e deseja usar o [modelo de projeto de aplicativo Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) em vez disso, fique à vontade para fazer isso, mas lembre-se de que pode haver algumas discrepâncias entre o que você vê na tela e as etapas que você deve tomar em comparação com o capturas de tela mostradas e instruções fornecidas nestes tutoriais.


[![Criar um novo arquivo com base no sistema Web Site](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**Figura 02**: criar um Site New File System-Based ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>Adicionando uma página mestra

Em seguida, adicione uma nova página mestra para o site no diretório raiz chamado Master. [Páginas mestras](https://msdn.microsoft.com/library/wtxbf3hh.aspx) habilitam um desenvolvedor de página para definir um modelo de todo o site que pode ser aplicado às páginas ASP.NET. O principal benefício das páginas mestras é que a aparência geral do site pode ser definida em um único local, facilitando assim atualizar ou ajustar o layout do site.


[![Adicionar uma página mestra chamado site a site](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**Figura 03**: adicionar um master de chamada de página mestra ao site ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image9.png))


Defina o layout de página em todo o site aqui na página mestra. Você pode usar o modo de exibição de Design e adicionar controles qualquer Layout ou da Web que você precisa, ou você pode adicionar manualmente a marcação manualmente na exibição da fonte. Estruturado de layout da minha página mestra para imitar o layout usado no meu *[trabalhando com dados no ASP.NET 2.0](../../data-access/index.md)* série de tutoriais (veja a Figura 4). Usa a página mestra [folhas de estilo em cascata](http://www.w3schools.com/css/default.asp) para posicionamento e estilos com as configurações de CSS definidas no arquivo Style. CSS (que está incluído no download de associado deste tutorial). Enquanto você não pode dizer sobre a marcação mostrada abaixo, as regras CSS são definidas, de modo que a navegação &lt;div&gt;do conteúdo de uma posição absoluta para que ele é exibido à esquerda e tem uma largura fixa de 200 pixels.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

Uma página mestra define o layout de página estática e as regiões que podem ser editadas pelas páginas do ASP.NET que usam a página mestra. Essas áreas de conteúdo editáveis são indicadas pelo controle ContentPlaceHolder, que pode ser visto no conteúdo &lt;div&gt;. Nossa página mestre tem um único ContentPlaceHolder (MainContent), mas a página mestra pode ter vários ContentPlaceHolders.

Com a marcação inserida acima, a alternar para a exibição de Design mostra o layout da página mestra. Qualquer página do ASP.NET que use essa página mestre terá esse layout uniforme, com a capacidade de especificar a marcação para a região MainContent.


[![A página mestra, quando visualizado por meio da exibição de Design](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**Figura 04**: A página mestra, quando exibidas por meio do modo de Design ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>Criando páginas de conteúdo

Neste ponto, temos uma página Default. aspx em nosso site, mas não usa a página mestra que acabamos de criar. Embora seja possível manipular a marcação declarativa de uma página da web para usar uma página mestra, se a página não contiver nenhum conteúdo ainda é mais fácil simplesmente excluir a página e adicioná-la novamente ao projeto, especificando a página mestra para usar. Portanto, comece a exclusão de Default. aspx do projeto.

Em seguida, clique com botão direito no nome do projeto no Gerenciador de soluções e escolha Adicionar um novo formulário da Web denominado Default. aspx. Desta vez, marque a caixa de seleção Selecionar página mestra e escolha a página mestra do site na lista.


[![Adicione uma nova página Default. aspx, optando por selecionar uma página mestra](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**Figura 05**: adicionar um novo default. aspx página optar por selecionar uma página mestra ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image15.png))


[![Use a página mestra do site](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**Figura 06**: Use a página mestra do site ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> Se você estiver usando o modelo de projeto de aplicativo Web a caixa de diálogo Adicionar Novo Item não inclui uma caixa de seleção Selecionar página mestra. Em vez disso, você precisará adicionar um item do tipo de formulário de conteúdo da Web. Depois de escolher a opção de formulário de conteúdo da Web e clicando em Adicionar, o Visual Studio exibirá o mesmo selecione um mestre de caixa de diálogo mostrada na Figura 6.


Marcação declarativa de novo da página Default. aspx inclui apenas uma @Page diretiva especificando o caminho para o mestre de página de arquivo e um controle de conteúdo MainContent ContentPlaceHolder da página mestra.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

Por enquanto, deixe a opção default. aspx vazio. Voltaremos a ele posteriormente no tutorial para adicionar conteúdo.

> [!NOTE]
> Nossa página mestre inclui uma seção de um menu ou alguma outra interface de navegação. Vamos criar uma interface desse tipo em um tutorial futuro.


## <a name="step-2-enabling-forms-authentication"></a>Etapa 2: Habilitar a autenticação de formulários

Com o site do ASP.NET criado, nossa próxima tarefa é habilitar a autenticação de formulários. Configuração de autenticação do aplicativo é especificada por meio de [ &lt;autenticação&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx) no Web. config. O &lt;autenticação&gt; elemento contém um único atributo chamado de modo que especifica o modelo de autenticação usado pelo aplicativo. Esse atributo pode ter um dos quatro valores a seguir:

- **Windows** - conforme discutido no tutorial anterior, quando um aplicativo usa a autenticação do Windows é responsabilidade do servidor web para autenticar o visitante e, geralmente, isso é feito por meio do Basic, Digest ou integrada do Windows autenticação.
- **Formulários**-os usuários são autenticados por meio de um formulário em uma página da web.
- **Passport**-os usuários são autenticados usando a rede da Microsoft Passport.
- **Nenhum**-nenhum modelo de autenticação é usado; todos os visitantes são anônimos.

Por padrão, os aplicativos ASP.NET usam a autenticação do Windows. Para alterar o tipo de autenticação para a autenticação de formulários, em seguida, precisamos modificar a &lt;autenticação&gt; atributo do modo do elemento para formulários.

Se seu projeto ainda não contém um arquivo Web. config, adicione um agora clicando no nome do projeto no Gerenciador de soluções, escolha Add New Item e, em seguida, adicionando um arquivo de configuração da Web.


[![Se seu projeto ainda não inclui Web. config, adicione-o agora](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**Figura 07**: se seu projeto faz não ainda incluem Web. config, adicione agora ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image21.png))


Em seguida, localize o &lt;autenticação&gt; elemento e atualize-o para usar a autenticação de formulários. Após essa alteração, a marcação do seu arquivo Web. config deve ser semelhante ao seguinte:

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Como o Web. config é um arquivo XML, maiusculas e minúsculas é importante. Certifique-se de que você defina o atributo de modo a formulários, com uma letra maiuscula F. Se você usar uma maiusculas e minúsculas diferentes, como formulários, você receberá um erro de configuração ao visitar o site por meio de um navegador.


O &lt;autenticação&gt; elemento pode incluir opcionalmente um &lt;formulários&gt; elemento filho que contém configurações específicas de autenticação de formulários. Por enquanto, vamos simplesmente usar as configurações de autenticação de formulários padrão. Vamos explorar as &lt;formulários&gt; elemento filho mais detalhadamente no próximo tutorial.

## <a name="step-3-building-the-login-page"></a>Etapa 3: Criar a página de logon

Para dar suporte à autenticação de formulários nosso site precisa de uma página de logon. Conforme discutido na Noções básicas sobre a seção de fluxo de trabalho de autenticação de formulários, o FormsAuthenticationModule redireciona automaticamente o usuário para a página de logon se eles tentam acessar uma página que não são autorizado a exibir. Também são controles da Web ASP.NET que serão exibido um link para a página de logon para usuários anônimos. Isso levanta a questão, o que é a URL da página de logon?

Por padrão, o sistema de autenticação de formulários espera que a página de logon para ser chamada Login. aspx e colocado no diretório raiz do aplicativo web. Se você quiser usar uma URL de página de logon diferente, você pode fazer isso especificando-o no Web. config. Veremos como fazer isso no tutorial subsequente.

A página de logon tem três responsabilidades:

1. Fornece uma interface que permite que o visitante a inserir suas credenciais.
2. Determine se as credenciais enviadas são válidas.
3. Fazer logon do usuário com a criação de formulários de tíquete de autenticação.

### <a name="creating-the-login-pages-user-interface"></a>Criando Interface do usuário da página de logon

Vamos começar com a primeira tarefa. Adicionar uma nova página ASP.NET para o diretório do site raiz chamado login. aspx e associá-la com a página mestra do site.


[![Adicionar uma nova página ASP.NET chamada Login. aspx](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**Figura 08**: adicionar um novo ASP.NET página chamada Login. aspx ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image24.png))


A interface de página de logon típico consiste em duas caixas de texto - um para o nome do usuário, uma de suas senhas - e um botão para enviar o formulário. Sites muitas vezes, incluem um checkbox lembrar-me que, se estiver marcada, persiste o tíquete de autenticação resultante entre as reinicializações do navegador.

Adicione duas caixas de texto para login. aspx e defina suas propriedades de identificação como nome de usuário e senha, respectivamente. Também defina a propriedade TextMode de senha a senha. Em seguida, adicione um controle de caixa de seleção, definindo sua propriedade de ID como RememberMe e sua propriedade Text para lembrar-me. Depois disso, adicione um botão chamado LoginButton cuja propriedade de texto é definida para logon. Por fim, adicione um controle de Web de rótulo e defina sua propriedade de ID para InvalidCredentialsMessage, sua propriedade Text como seu nome de usuário ou senha é inválida. Tente novamente., sua propriedade de cor de primeiro plano para vermelho e sua propriedade Visible como False.

Neste ponto, sua tela deve ser semelhante para a tela na Figura 9, e a sintaxe declarativa da sua página deve ter a aparência a seguir:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![A página de logon contém duas caixas de texto, uma caixa de seleção, um botão e um rótulo](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**Figura 09**: O logon página contém duas caixas de texto, uma caixa de seleção, um botão e um rótulo ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image27.png))


Por fim, crie um manipulador de eventos de clique do LoginButton eventos. No Designer, ou simplesmente clique duas vezes no controle de botão para criar esse manipulador de eventos.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Determinando se as credenciais fornecidas são válidos

Agora, precisamos implementar a tarefa 2 no clique do botão manipulador de eventos - determinando se as credenciais fornecidas são válidas. Para fazer isso é preciso haver um armazenamento de usuário que contém todas as credenciais dos usuários para que podemos determinar se as credenciais fornecidas correspondem a quaisquer credenciais conhecidas.

Antes do ASP.NET 2.0, os desenvolvedores eram responsáveis pela implementação de ambos os seus próprios armazenamentos de usuário e escrever o código para validar as credenciais fornecidas no repositório. A maioria dos desenvolvedores implementaria o repositório do usuário em um banco de dados, criando uma tabela denominada Users com colunas como o nome de usuário, senha, Email, data do último logon e assim por diante. Essa tabela, em seguida, teria um registro por conta de usuário. Verificar as credenciais fornecidas de um usuário envolveria consultando o banco de dados para um nome de usuário correspondente e, em seguida, garantindo que a senha no banco de dados correspondessem à senha fornecida.

Com o ASP.NET 2.0, os desenvolvedores devem usar um dos provedores de associação para gerenciar o armazenamento do usuário. Nessa série de tutoriais usaremos SqlMembershipProvider, que usa um banco de dados do SQL Server para o repositório do usuário. Ao usar o SqlMembershipProvider, precisamos implementar um esquema de banco de dados específico que inclua as tabelas, exibições e procedimentos armazenados esperados pelo provedor. Vamos examinar como implementar esse esquema na *[criação do esquema de associação no SQL Server](../membership/creating-the-membership-schema-in-sql-server-vb.md)* tutorial. Com o provedor de associação em vigor, validar as credenciais do usuário é tão simple quanto chamar o [classe Membership](https://msdn.microsoft.com/library/system.web.security.membership.aspx)do [ValidateUser (*username*, *senha*) método](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), que retorna um valor booliano que indica se a validade do *nome de usuário* e *senha* combinação. Vendo como estamos ainda não implementou o repositório do usuário do SqlMembershipProvider, não podemos usar método de ValidateUser a classe de associação neste momento.

Em vez de levar um tempo para criar nossa própria personalizada tabela de banco de dados de usuários (que seria obsoleta depois implementamos o SqlMembershipProvider), vamos em vez disso, codificar as credenciais válidas dentro do logon de página em si. No LoginButton manipulador de eventos de clique, adicione o seguinte código:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

Como você pode ver, há três contas de usuário válidas - Scott, Jisun e Sam - e todas as três têm a mesma senha (senha). O código percorre os conjuntos de usuários e senhas para localizar uma correspondência de nome de usuário e senha válida. Se o nome de usuário e a senha forem válidos, precisamos fazer logon do usuário e, em seguida, redirecioná-las para a página apropriada. Se as credenciais forem inválidas, podemos exibir o rótulo de InvalidCredentialsMessage.

Quando um usuário insere credenciais válidas, mencionei que, em seguida, eles são redirecionados para a página apropriada. O que é a página apropriada, no entanto? Lembre-se de que quando um usuário visita uma página que não estão autorizados a exibir, o FormsAuthenticationModule redireciona automaticamente para a página de logon. Dessa forma, ele inclui a URL solicitada na sequência de consulta por meio do parâmetro ReturnUrl. Ou seja, se um usuário tentou visitar ProtectedPage.aspx e eles não foram autorizados a fazer isso, o FormsAuthenticationModule seria redirecioná-las para:

Login.aspx?ReturnUrl=ProtectedPage.aspx

Após fazer logon com êxito, o usuário deve ser redirecionado para ProtectedPage.aspx. Como alternativa, os usuários podem visitar a página de logon em seu próprios volition. Nesse caso, após o logon do usuário devem ser enviados para a página de Default. aspx da pasta raiz.

### <a name="logging-in-the-user"></a>Registro em log do usuário

Supondo que as credenciais fornecidas sejam válidas, precisamos criar um tíquete de autenticação de formulários, registro em log, assim, o usuário para o site. O [classe FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) na [Security namespace](https://msdn.microsoft.com/library/system.web.security.aspx) fornece diversos métodos para fazer logon no e no registro em log os usuários por meio de formulários de sistema de autenticação. Embora haja vários métodos na classe FormsAuthentication, os três que estamos interessados nesse momento são:

- [GetAuthCookie (*nome de usuário*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) – cria um tíquete de autenticação de formulários para o nome fornecido *username*. Em seguida, esse método cria e retorna um objeto de HttpCookie que armazena o conteúdo de que o tíquete de autenticação. Se *persistCookie* for True, um cookie persistente é criado.
- [SetAuthCookie (*nome de usuário*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) -chama o GetAuthCookie (*username*, *persistCookie*) método para gerar o cookie de autenticação de formulários. Esse método, em seguida, adiciona o cookie retornado por GetAuthCookie à coleção de Cookies (supondo que a autenticação de formulários baseados em cookies está sendo usado; caso contrário, esse método chama uma classe interna que manipula a lógica de tíquete sem cookies).
- [RedirectFromLoginPage (*nome de usuário*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) -esse método chama SetAuthCookie (*username*, *persistCookie*) e, em seguida, redireciona o usuário para a página apropriada.

GetAuthCookie é útil quando você precisa modificar o tíquete de autenticação antes de gravar o cookie de à coleção de Cookies. SetAuthCookie é útil se você deseja criar tíquete de autenticação de formulários e adicioná-lo à coleção de Cookies, mas não deseja redirecionar o usuário para a página apropriada. Talvez você queira mantê-los na página de logon ou enviá-los para alguma página alternativa.

Como queremos fazer logon do usuário e redirecioná-las para a página apropriada, vamos usar o RedirectFromLoginPage. Atualizar clique do LoginButton manipulador de eventos, substituindo as duas linhas TODO comentadas com a seguinte linha de código:

O FormsAuthentication. RedirectFromLoginPage (UserName.Text, RememberMe.Checked)

Ao criar tíquete de autenticação de formulários podemos usar a propriedade de texto de UserName TextBox para o tíquete de autenticação de formulários *nome de usuário* parâmetro e o estado marcado do RememberMe CheckBox para o  *persistCookie* parâmetro.

Para testar a página de logon, visite-o em um navegador. Inicie inserindo credenciais inválidas, como um nome de usuário do Nope e uma senha de errado. Ao clicar no botão logon ocorre um postback e o rótulo de InvalidCredentialsMessage será exibido.


[![O rótulo de InvalidCredentialsMessage é exibido ao inserir credenciais inválidas](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**Figura 10**: O rótulo de InvalidCredentialsMessage é exibido ao inserir credenciais inválidas ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image30.png))


Em seguida, insira credenciais válidas e clique no botão de logon. Desta vez, quando o postback ocorre um tíquete de autenticação de formulários é criada e você será redirecionado automaticamente para default. aspx. Neste ponto você ter conectado ao site, embora não haja nenhum indicações visuais para indicar que você fez. Na etapa 4, que veremos como determinar programaticamente se um usuário é registrado no ou não, bem como como identificar o usuário visitar a página.

Etapa 5 examina as técnicas para registrar em log a um usuário fora do site.

### <a name="securing-the-login-page"></a>Protegendo a página de logon

Quando o usuário insere suas credenciais e envia o formulário da página de logon, as credenciais - incluindo senha - são transmitidas pela Internet para o servidor web no *texto sem formatação*. Isso significa que qualquer hacker detecção o tráfego de rede pode ver o nome de usuário e senha. Para evitar isso, é essencial para criptografar o tráfego de rede usando [camadas de soquete seguro (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Isso garantirá que as credenciais (bem como uma marcação HTML da página inteira) é criptografada desde o momento em que eles deixam o navegador até que elas são recebidas pelo servidor web.

A menos que seu site contiver informações confidenciais, você só precisará usar SSL na página de logon e em outras páginas em que a senha do usuário caso contrário, seria enviada eletronicamente em texto sem formatação. Você não precisa se preocupar sobre como proteger o tíquete de autenticação de formulários, já que, por padrão, ela é tanto criptografada e assinada digitalmente (para evitar a violação). Uma discussão mais completa sobre segurança de tíquete de autenticação de formulários é apresentada no tutorial a seguir.

> [!NOTE]
> Muitos sites financeiros e médicos são configurados para usar SSL *todos os* páginas acessíveis para a usuários autenticados. Se você estiver criando esse tipo de site, você pode configurar o sistema de autenticação de formulários, para que o tíquete de autenticação de formulários é transmitido apenas por uma conexão segura. Vamos examinar as várias opções de configuração de autenticação de formulários no próximo tutorial  *[configuração de autenticação de formulários e tópicos avançados](../membership/creating-the-membership-schema-in-sql-server-vb.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Etapa 4: Detecção de visitantes autenticados e determinar sua identidade

Neste momento temos habilitou a autenticação de formulários e criado uma página de logon rudimentar, mas ainda precisamos examinar como podemos determinar se um usuário é autenticado ou anônimo. Em determinados cenários que desejamos exibir diferentes dados ou informações dependendo se um usuário autenticado ou anônimo está visitando a página. Além disso, muitas vezes, precisamos saber a identidade do usuário autenticado.

Vamos ampliar a página Default. aspx existente para ilustrar essas técnicas. Em Default. aspx, adicione dois controles de painel, um AuthenticatedMessagePanel nomeado e outro AnonymousMessagePanel nomeado. Adicione um controle de rótulo chamado WelcomeBackMessage no primeiro painel. No segundo painel Adicionar um controle de hiperlink, defina sua propriedade Text para fazer logon e sua propriedade NavigateUrl para ~ / login. aspx. Neste ponto, a marcação declarativa para default. aspx deve ser semelhante ao seguinte:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

Como você provavelmente ter adivinhado agora, a ideia aqui é exibir apenas o AuthenticatedMessagePanel aos visitantes autenticados e apenas o AnonymousMessagePanel para visitantes anônimos. Para fazer isso, precisamos definir a propriedade visível desses painéis, dependendo se o usuário estiver conectado ou não.

O [Request.IsAuthenticated propriedade](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) retorna um valor booliano que indica se a solicitação foi autenticada. Insira o código a seguir para a página\_carregar código do manipulador de eventos:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

Com esse código, visite o default. aspx através de um navegador. Supondo que você ainda precisa fazer logon, você verá um link para a página de logon (veja a Figura 11). Clique neste link e faça logon no site. Como vimos na etapa 3, depois de inserir suas credenciais você será retornado para default. aspx, mas desta vez, a página mostra o bem-vindo! mensagem (veja a Figura 12).


[![Quando visitar anonimamente e, em um Link de Log é exibido](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**Figura 11**: quando visitar anonimamente, em um Link de Log é exibido ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image33.png))


[![Os usuários autenticados são mostrados o bem-vindo! Mensagem](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**Figura 12**: os usuários autenticados são mostrados o bem-vindo! Mensagem ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image36.png))


Podemos determinar a identidade do usuário conectado no momento por meio de [objeto HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)do [propriedade usuário](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). O objeto HttpContext representa informações sobre a solicitação atual e é o lar dos objetos ASP.NET comuns como resposta, a solicitação e a sessão, entre outros. A propriedade do usuário representa o contexto de segurança da solicitação HTTP atual e implementa o [interface IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

A propriedade do usuário é definida pelo FormsAuthenticationModule. Especificamente, quando o FormsAuthenticationModule encontra um tíquete de autenticação de formulários na solicitação de entrada, ele cria um novo objeto GenericPrincipal e atribui a propriedade do usuário.

Objetos de entidade (como GenericPrincipal) fornecem informações sobre a identidade do usuário e as funções às quais eles pertencem. A interface IPrincipal define dois membros:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) -um método que retorna um valor booliano que indica se a entidade de segurança pertence à função especificada.
- [Identidade](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) -uma propriedade que retorna um objeto que implementa o [interface IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). A interface IIdentity define três propriedades: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), e [nome](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Podemos determinar o nome do visitante atual usando o seguinte código:

Dim currentUsersName As String = User.Identity.Name

Quando usar formulários de autenticação, uma [FormsIdentity objeto](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) é criado para a propriedade de identidade do GenericPrincipal. A classe FormsIdentity sempre retorna os formulários de cadeia de caracteres de sua propriedade AuthenticationType e True para sua propriedade IsAuthenticated. A propriedade Name retorna o nome de usuário especificado ao criar tíquete de autenticação de formulários. Além dessas três propriedades, FormsIdentity inclui acesso ao tíquete de autenticação subjacente por meio de seu [tíquete propriedade](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). A propriedade de tíquete retorna um objeto do tipo [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), que tem propriedades como a expiração, IsPersistent, IssueDate, nome e assim por diante.

O ponto importante a ser considerado aqui é que o *nome de usuário* parâmetro especificado no FormsAuthentication.GetAuthCookie (*nome de usuário*, *persistCookie*), FormsAuthentication.SetAuthCookie (*nome de usuário*, *persistCookie*) e FormsAuthentication. RedirectFromLoginPage (*username*, *persistCookie*) métodos é o mesmo valor retornado por User.Identity.Name. Além disso, o tíquete de autenticação criado por esses métodos está disponível por projeção Identity para um objeto FormsIdentity e, em seguida, acessando a propriedade do tíquete:

Dim ident como FormsIdentity = CType (Identity, FormsIdentity)

Dim authTicket como FormsAuthenticationTicket = ident. Tíquete

Vamos fornecer uma mensagem mais personalizada em Default. aspx. Atualizar a página\_carregar o manipulador de eventos para que a propriedade de texto do rótulo WelcomeBackMessage é atribuída a cadeia de caracteres bem-vindo, *nome de usuário*!

WelcomeBackMessage.Text = "Welcome back, " &amp; User.Identity.Name &amp; "!"

Figura 13 mostra o efeito desta modificação (ao fazer logon como usuário Scott).


[![A mensagem de boas-vinda inclui conectada no momento em nome do usuário](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**Figura 13**: A mensagem de boas-vinda inclui nome do usuário no registrados atualmente ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>Usando os controles de LoginName e LoginView

Exibir conteúdo diferente para usuários autenticados e anônimos é um requisito comum; Portanto, está exibindo o nome do usuário conectado no momento. Por esse motivo, o ASP.NET inclui dois controles de Web que fornecem a mesma funcionalidade que o mostrado na Figura 13, mas sem a necessidade de escrever uma única linha de código.

O [controle LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) é um controle de Web com base no modelo que torna mais fácil de exibir dados diferentes para usuários anônimos e autenticados. O LoginView inclui dois modelos predefinidos:

- LoginView - qualquer marcação adicionada a esse modelo é exibido apenas aos visitantes anônimos.
- LoggedInTemplate - marcação desse modelo é mostrado somente para usuários autenticados.

Vamos adicionar o controle LoginView a página mestra do nosso site, Master. Em vez de adicionar apenas o controle LoginView, no entanto, vamos adicionar um novo controle ContentPlaceHolder e, em seguida, coloque o controle LoginView dentro desse novo ContentPlaceHolder. A lógica para essa decisão se tornará aparente logo.

> [!NOTE]
> Além do LoginView e LoggedInTemplate, o controle LoginView pode incluir modelos de função específica. Modelos específicos de função mostram marcação somente para os usuários que pertencem a uma função especificada. Vamos examinar os recursos baseados em função do controle LoginView em um tutorial futuro.


Comece adicionando um ContentPlaceHolder chamado LoginContent na página mestra dentro de navegação &lt;div&gt; elemento. Você pode simplesmente arrastar um controle ContentPlaceHolder Toolbox para a exibição da fonte, colocando o consequente resultado logo acima de tarefas Pendentes: Menu aparecerão aqui texto.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

Em seguida, adicione um controle LoginView dentro de LoginContent ContentPlaceHolder. Conteúdo colocado em controles de ContentPlaceHolder da página mestra são considerados *conteúdo padrão* para o ContentPlaceHolder. Ou seja, páginas ASP.NET que usam essa página mestra podem especificar seu próprio conteúdo para cada ContentPlaceHolder ou usar o conteúdo de padrão da página mestra.

O LoginView e outros controles de logon estão localizados na guia de logon da caixa de ferramentas.


[![O controle LoginView na caixa de ferramentas](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**Figura 14**: O controle LoginView na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image42.png))


Em seguida, adicione duas &lt;br /&gt; elementos imediatamente após o controle LoginView, mas ainda dentro de ContentPlaceHolder. Neste ponto, a navegação &lt;div&gt; marcação do elemento deve ser semelhante ao seguinte:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

Modelos do LoginView podem ser definidos de Designer ou marcação declarativa. No Designer do Visual Studio, expanda a marca inteligente do LoginView, que lista os modelos configurados em uma lista suspensa. Digite o texto Hello, estranho em LoginView; em seguida, adicione um controle de hiperlink e defina suas propriedades de texto e NavigateUrl para fazer logon e ~ / login. aspx, respectivamente.

Depois de configurar o LoginView, alterne para o LoggedInTemplate e insira o texto, "Bem-vindo,". Em seguida, arraste um controle LoginName da caixa de ferramentas em LoggedInTemplate, colocando-o imediatamente após "Bem-vindo de volta," texto. O [controle LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), como o nome indica, exibe o nome do usuário conectado no momento. Internamente, o controle LoginName simplesmente gera a propriedade User.Identity.Name

Depois de fazer essas adições aos modelos do LoginView, a marcação deve ser semelhante ao seguinte:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Com essa adição à página mestra master, cada página no nosso site exibirá uma mensagem diferente dependendo se o usuário é autenticado. Figura 15 mostra a página Default. aspx, quando acessadas por meio de um navegador por usuário Jisun. Bem-vindos novamente, Jisun mensagem é repetida duas vezes: uma vez na seção de navegação da página mestra à esquerda (por meio do controle LoginView que acabamos de adicionar) e uma vez em que o default. aspx conteúdo área (por meio de controles de painel e a lógica programática).


[![O LoginView controle exibe bem-vindo, Jisun.](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**Figura 15**: O LoginView controle exibe bem-vindo, Jisun. ([Clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image45.png))


Como nós adicionamos o LoginView para a página mestra, ele pode aparecer em cada página em nosso site. No entanto, pode haver páginas da web em que não queremos mostrar esta mensagem. Uma página dessas é a página de logon, uma vez que um link para a página de logon parece deslocada lá. Uma vez que colocamos o controle LoginView em um ContentPlaceHolder na página mestra, podemos substituir essa marcação padrão em nossa página de conteúdo. Abra o login. aspx e vá para o Designer. Uma vez que estamos explicitamente não definiu um controle de conteúdo no login. aspx para o LoginContent ContentPlaceHolder na página mestra, a página de logon mostrará marcação de padrão da página mestra para esse ContentPlaceHolder. Você pode ver isso por meio do Designer - o LoginContent ContentPlaceHolder mostra a marcação padrão (o controle LoginView).


[![A página de logon mostra o padrão conteúdo para LoginContent ContentPlaceHolder a página mestra](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**Figura 16**: A página de logon mostra o conteúdo de padrão para LoginContent ContentPlaceHolder's Page the Master ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image48.png))


Para substituir a marcação padrão para o LoginContent ContentPlaceHolder, clique com botão direito na região no Designer e escolha a opção de criar conteúdo personalizado do menu de contexto. (Quando usando o Visual Studio 2008 o ContentPlaceHolder inclui uma marca inteligente que, quando selecionado, oferece a mesma opção.) Isso adiciona um novo controle de conteúdo para a marcação da página e, portanto, nos permite definir o conteúdo personalizado para essa página. Você pode adicionar uma mensagem personalizada aqui, como faça o logon, mas vamos apenas deixar em branco.

> [!NOTE]
> No Visual Studio 2005, a criação de conteúdo personalizado cria uma vazia controle na página ASP.NET de conteúdo. No Visual Studio 2008, no entanto, a criação de conteúdo personalizado copia conteúdo de padrão da página mestra para o controle de conteúdo criado recentemente. Se você estiver usando o Visual Studio 2008, em seguida, depois de criar o novo controle de conteúdo Certifique-se limpar o conteúdo copiado ao longo da página mestra.


Figura 17 mostra a página de login. aspx, quando acessadas a partir de um navegador depois de fazer essa alteração. Observe que não há nenhum Hello estranho ou bem-vindo, *nome de usuário* mensagem no painel de navegação esquerdo &lt;div&gt; que ocorrem quando visitar default. aspx.


[![A página de logon oculta a marcação de LoginContent ContentPlaceHolder o padrão](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**Figura 17**: A página de logon oculta marcação do padrão LoginContent ContentPlaceHolder ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>Etapa 5: Fazer logoff

Na etapa 3 nós examinamos a criação de uma página de logon para o logon de um usuário para o site, mas ainda temos que ver como um usuário de logoff. Além dos métodos para registrar em log a um usuário, a classe FormsAuthentication também fornece um [SignOut método](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). O método SignOut simplesmente destrói o tíquete de autenticação de formulários, log, assim, o usuário do site.

Oferecendo que um link de logoff é tal um recurso comum que o ASP.NET inclui um controle projetado especificamente para um usuário de logoff. O [controle LoginStatus](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) exibe LinkButton um logon ou Logout LinkButton, dependendo do status de autenticação do usuário. Logon LinkButton é renderizado para usuários anônimos, enquanto um Logout LinkButton é exibido aos usuários autenticados. O texto para o logon e Logout LinkButtons pode ser configurado via o LoginStatus propriedades LoginText e LogoutText.

Clicar no LinkButton logon faz com que um postback, da qual um redirecionamento é emitido para a página de logon. Clicar no Logout LinkButton faz com que o controle LoginStatus invocar o método FormsAuthentication.SignOff e, em seguida, redireciona o usuário para uma página. Conectado a página de logoff de usuário é redirecionado para depende da propriedade LogoutAction, que pode ser atribuída a um dos três seguintes valores:

- Atualize - o padrão; redireciona o usuário para a página que eles foram apenas a visita. Se a página que eles foram apenas a visita não permitir que usuários anônimos, em seguida, o FormsAuthenticationModule automaticamente redirecionará o usuário para a página de logon.

Você pode estar curioso para saber por que um redirecionamento é executado aqui. Se o usuário deseja permanecer na mesma página, por que a necessidade do redirecionamento explícita? O motivo é que quando o Logoff LinkButton for clicado, o usuário ainda tem o tíquete de autenticação de formulários em sua coleção de cookies. Consequentemente, a solicitação de postback é uma solicitação autenticada. O controle LoginStatus chama o método de saída, mas o que acontece depois que o FormsAuthenticationModule tiver autenticado o usuário. Portanto, um redirecionamento explícito faz com que o navegador solicitar novamente a página. No momento em que o navegador solicita novamente a página, o tíquete de autenticação de formulários foi removido e, portanto, a solicitação de entrada é anônima.

- Redirecionamento – o usuário é redirecionado para a URL especificada pela propriedade de LogoutPageUrl do LoginStatus.
- RedirectToLoginPage - o usuário é redirecionado para a página de logon.

Vamos adicionar um controle LoginStatus para a página mestra e configurá-lo para usar a opção de redirecionamento para enviar o usuário para uma página que exibe uma mensagem confirmando que eles foi desconectados. Comece criando uma página no diretório raiz chamado Logout.aspx. Não se esqueça de associar a esta página a página mestra do site. Em seguida, digite uma mensagem na marcação da página explicar ao usuário que foi desconectados.

Em seguida, retornar à página mestra Master e adicione um controle de LoginStatus sob o LoginView no LoginContent ContentPlaceHolder. Defina a propriedade do controle LoginStatus LogoutAction redirecionamento e sua propriedade de LogoutPageUrl para ~/Logout.aspx.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

Como o LoginStatus está fora do controle LoginView, ele será exibido para usuários anônimos e autenticados, mas que é Okey porque o LoginStatus corretamente exibirá um logon ou Logout LinkButton. Com a adição do controle LoginStatus, o Log no hiperlink o LoginView é supérfluo, portanto, removê-lo.

Figura 18 mostra default. aspx, quando Jisun visita. Observe que a coluna à esquerda exibe a mensagem, bem-vindo novamente, Jisun juntamente com um link para fazer logoff. Na LinkButton logoff causa um postback, sai Jisun o sistema e, em seguida, redireciona dela para Logout.aspx. Como mostra a Figura 19, no momento que jisun atinge Logout.aspx ela já foi desconectada e, portanto, é anônima. Consequentemente, a coluna esquerda mostra o texto de boas-vindas, stranger e um link para a página de logon.


[![Default. aspx mostra bem-vindo, Jisun juntamente com um Logout LinkButton](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**Figura 18**: default. aspx mostra-vindo de volta, Jisun juntamente com um Logout LinkButton ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout.aspx mostra bem-vindo, estranho, juntamente com um logon LinkButton](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**Figura 19**: Logout.aspx mostra bem-vindo, estranho, juntamente com um logon LinkButton ([clique para exibir a imagem em tamanho normal](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> Eu recomendo que você personalize a página Logout.aspx para ocultar LoginContent ContentPlaceHolder da página mestra (como fizemos para login. aspx na etapa 4). O motivo é que o logon LinkButton processado pelo controle LoginStatus (aquele abaixo Hello, estranho) envia o usuário para a página de logon, passando o parâmetro de cadeia de consulta ReturnUrl a URL atual. Em resumo, se um usuário que tiver feito logoff clica neste LoginStatus logon LinkButton e, em seguida, registra em log, ele serão redirecionados para Logout.aspx, que poderia facilmente confundir o usuário.


## <a name="summary"></a>Resumo

Neste tutorial começamos com uma análise do fluxo de trabalho de autenticação de formulários e, em seguida, ativado para implementar a autenticação de formulários em um aplicativo ASP.NET. Autenticação de formulários é alimentada pelo FormsAuthenticationModule, que tem duas responsabilidades: identificar os usuários com base em seu tíquete de autenticação de formulários e redirecionar os usuários não autorizados para a página de logon.

Classe de FormsAuthentication do .NET Framework inclui métodos para criar, inspeção e remoção de tíquetes de autenticação de formulários. A propriedade Request.IsAuthenticated e o objeto de usuário dão suporte de programação adicional para determinar se uma solicitação é autenticada e informações sobre a identidade do usuário. Há também os controles LoginName Web, LoginView e LoginStatus, que oferecem aos desenvolvedores uma maneira rápida e sem código para realizar muitas tarefas comuns relacionadas ao logon. Vamos examinar esses e outros controles de Web relacionadas ao logon mais detalhadamente em tutoriais futuros.

Este tutorial forneceu uma visão geral superficial da autenticação de formulários. Não podemos examinar as opções de configuração variados, examinar o trabalho de tíquetes de autenticação de formulários como sem cookies ou explorar como o ASP.NET protege o conteúdo do tíquete de autenticação de formulários. Abordaremos esses tópicos e muito mais na [próximo tutorial](forms-authentication-configuration-and-advanced-topics-vb.md).

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Alterações entre o IIS6 e segurança do IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Controles de logon do ASP.NET](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2.0 segurança, associação e gerenciamento de função](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698 do MSSQLSERVER-8)
- [O &lt;autenticação&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx)
- [O &lt;formulários&gt; elemento para &lt;autenticação&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Treinamento em vídeo sobre os tópicos contidos neste tutorial

- [Uso da autenticação de formulários básica no ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é  *[Sams Teach por conta própria ASP.NET 2.0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial incluem Alicja Maziarz, John Suru e Teresa Murphy. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](security-basics-and-asp-net-support-vb.md)
> [Próximo](forms-authentication-configuration-and-advanced-topics-vb.md)
