---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: Criar contas de usuário (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, exploraremos usando a estrutura de associação (por meio de SqlMembershipProvider) para criar novas contas de usuário. Veremos como criar novos nós...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: c7f5f4ec6ce27c1a52569e6414b8f96892f68574
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831627"
---
<a name="creating-user-accounts-vb"></a>Criar contas de usuário (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> Neste tutorial, exploraremos usando a estrutura de associação (por meio de SqlMembershipProvider) para criar novas contas de usuário. Veremos como criar novos usuários por meio de programação e por meio do ASP. Controle CreateUserWizard interno da rede.


## <a name="introduction"></a>Introdução

No <a id="_msoanchor_1"> </a> [anterior do tutorial](creating-the-membership-schema-in-sql-server-vb.md) instalamos o esquema de serviços de aplicativo em um banco de dados que são adicionados as tabelas, exibições e procedimentos necessários para armazenados os `SqlMembershipProvider` e `SqlRoleProvider`. Isso criou a infraestrutura que precisamos para o restante dos tutoriais desta série. Neste tutorial, exploraremos usando a estrutura de associação (por meio de `SqlMembershipProvider`) para criar novas contas de usuário. Veremos como criar novos usuários por meio de programação e por meio do ASP. Controle CreateUserWizard interno da rede.

Além de aprender a criar novas contas de usuário, também precisamos atualizar o site de demonstração criado pela primeira vez na *<a id="_msoanchor_2"> </a> [uma visão geral de formulários de autenticação](../introduction/an-overview-of-forms-authentication-vb.md)* tutorial e, em seguida, aprimorado na *<a id="_msoanchor_3"> </a> [configuração de autenticação de formulários e tópicos avançados](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* tutorial. Nosso aplicativo web de demonstração tem uma página de logon que valida as credenciais do usuário em relação a pares de nome de usuário/senha embutida. Além disso, `Global.asax` inclui código que cria personalizado `IPrincipal` e `IIdentity` objetos para os usuários autenticados. Atualizamos a página de logon para validar as credenciais do usuário em relação a estrutura de associação e remova a lógica personalizada de entidade de segurança e identidade.

Vamos começar!

## <a name="the-forms-authentication-and-membership-checklist"></a>A autenticação de formulários e a lista de verificação de associação

Antes de começarmos a trabalhar com a estrutura de associação, vamos dedicar um tempo para examinar as etapas importantes que levamos para chegar a este ponto. Ao usar a estrutura de associação com o `SqlMembershipProvider` em um cenário de autenticação baseada em formulários, as etapas a seguir precisam ser executadas antes de implementar a funcionalidade de associação em seu aplicativo web:

1. **Habilite a autenticação baseada em formulários.** Como discutimos no  *<a id="_msoanchor_4"> </a> [uma visão geral de formulários de autenticação](../introduction/an-overview-of-forms-authentication-vb.md)*, autenticação de formulários está habilitada por meio da edição `Web.config` e definindo o `<authentication>` prvku `mode` atributo `Forms`. Com a autenticação de formulários habilitada, cada solicitação de entrada é examinada para um *tíquete de autenticação de formulários*, que, se estiver presente, identifica o solicitante.
2. **Adicione o esquema de serviços de aplicativo ao banco de dados apropriado.** Ao usar o `SqlMembershipProvider` , precisamos instalar o esquema de serviços de aplicativo para um banco de dados. Normalmente, esse esquema é adicionado ao mesmo banco de dados que contém o modelo de dados do aplicativo. O *<a id="_msoanchor_5"> </a> [criando o esquema de associação no SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* tutorial examinou usando o `aspnet_regsql.exe` ferramenta para fazer isso.
3. **Personalize configurações do aplicativo da Web para o banco de dados de referência da etapa 2.** O *criando o esquema de associação no SQL Server* tutorial, mostramos duas maneiras de configurar o aplicativo web para que o `SqlMembershipProvider` usaria o banco de dados selecionado na etapa 2: modificando o `LocalSqlServer` nome de cadeia de caracteres de conexão; ou adicionando um novo provedor registrado à lista de provedores de estrutura de associação e personalizando esse novo provedor para usar o banco de dados na etapa 2.

Quando criar um aplicativo web que usa o `SqlMembershipProvider` e a autenticação baseada em formulários, você precisará seguir estas três etapas antes de usar o `Membership` classe ou os controles da Web de logon do ASP.NET. Uma vez que estamos já executou estas etapas nos tutoriais anteriores, estamos prontos para começar a usar a estrutura de associação!

## <a name="step-1-adding-new-aspnet-pages"></a>Etapa 1: Adicionar novas páginas do ASP.NET

Este tutorial e os próximos três nós será examinando vários recursos e funções relacionadas à associação. Precisaremos de uma série de páginas do ASP.NET para implementar os tópicos examinados durante esses tutoriais. Vamos criar essas páginas e, em seguida, um arquivo de mapa de site `(Web.sitemap)`.

Comece criando uma nova pasta no projeto chamado `Membership`. Em seguida, adicione cinco novas páginas do ASP.NET para o `Membership` pasta, vinculando cada página com o `Site.master` página mestra. Nomeie as páginas:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

Neste ponto, Gerenciador de soluções do seu projeto deve ser semelhante à mostrada na Figura 1 de captura de tela.


[![Cinco novas páginas foram adicionadas à pasta de associação](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**Figura 1**: cinco novas páginas foram adicionados para o `Membership` pasta ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image3.png))


Cada página, neste ponto, terá dois controles de conteúdo, uma para cada um dos ContentPlaceHolders da página mestra: `MainContent` e `LoginContent`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

Lembre-se de que o `LoginContent` marcação de padrão do ContentPlaceHolder exibe um link para fazer logon ou logoff do site, dependendo se o usuário é autenticado. A presença do `Content2` controle de conteúdo, no entanto, substituirá a marcação de padrão da página mestra. Como discutimos no *<a id="_msoanchor_6"> </a> [uma visão geral de formulários de autenticação](../introduction/an-overview-of-forms-authentication-vb.md)* tutorial, isso é útil em páginas onde não queremos exibir opções relacionadas ao logon na coluna à esquerda.

Para esses cinco páginas, no entanto, queremos mostrar a marcação de padrão da página mestra para o `LoginContent` ContentPlaceHolder. Portanto, remova a marcação declarativa para a `Content2` controle de conteúdo. Depois de fazer isso, cada uma a cinco marcação da página deve conter apenas um controle de conteúdo.

## <a name="step-2-creating-the-site-map"></a>Etapa 2: Criando o mapa do Site

Todos, exceto os sites da Web mais trivial precisará implementar alguma forma de uma interface do usuário de navegação. A interface do usuário de navegação pode ser uma simple lista de links para as várias seções do site. Como alternativa, esses links podem ser organizados em menus ou modos de exibição de árvore. Como os desenvolvedores de páginas, criando a interface do usuário de navegação é apenas metade da história. Também precisamos de alguns meios para definir a estrutura lógica do site de uma maneira fácil de manter e atualizável. Conforme novas páginas são adicionadas ou removidas páginas existentes, queremos ser capaz de atualizar uma única fonte - mapa do site - e reflita essas modificações na interface do usuário de navegação do site.

Essas duas tarefas - define o mapa de site e implementar uma interface do usuário de navegação com base no mapa do site - são fáceis de realizar graças a estrutura do mapa do Site e a navegação Web controles ASP.NET adicionado na versão 2.0. A estrutura do mapa do Site permite que um desenvolvedor definir um mapa do site e, em seguida, para acessá-lo por meio de uma API programática (o [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Controles da Web de navegação internos incluem uma [controle de Menu](https://msdn.microsoft.com/library/bz09dy46.aspx), o [controle TreeView](https://msdn.microsoft.com/library/3eafky27.aspx)e o [controle SiteMapPath](https://msdn.microsoft.com/library/3eafky27.aspx).

Como as estruturas de associação e funções, a estrutura do mapa do Site é construída sobre a [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). O trabalho da classe do provedor de mapa do Site é gerar a estrutura na memória usada pelo `SiteMap` classe a partir de um repositório de dados persistentes, como um arquivo XML ou uma tabela de banco de dados. O .NET Framework vem com um provedor de mapa de Site padrão que lê os dados de mapa do site de um arquivo XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), e esse é o provedor que usaremos neste tutorial. Para algumas implementações alternativas de provedor de mapa do Site, consulte a seção de leituras adicionais no final deste tutorial.

O provedor de mapa de Site padrão espera que um arquivo XML formatado corretamente chamado `Web.sitemap` existir o diretório raiz. Como estamos usando esse provedor padrão, é necessário adicionar esse arquivo e definir a estrutura do mapa do site no formato XML apropriado. Para adicionar o arquivo, clique com botão direito no nome do projeto no Gerenciador de soluções e escolha Add New Item. Na caixa de diálogo, optar por adicionar um arquivo do tipo de mapa de Site denominado `Web.sitemap`.


[![Adicionar um arquivo denominado Web. sitemap para o diretório raiz do projeto](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**Figura 2**: adicionar um arquivo nomeado `Web.sitemap` para o diretório raiz do projeto ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image6.png))


O arquivo de mapa de site XML define a estrutura do site como uma hierarquia. Essa relação hierárquica é modelada no arquivo XML por meio de ancestrais do `<siteMapNode>` elementos. O `Web.sitemap` deve começar com um `<siteMap>` nó pai que tem exatamente um `<siteMapNode>` filho. Nesse nível superior `<siteMapNode>` elemento representa a raiz da hierarquia e pode ter um número arbitrário de nós descendentes. Cada `<siteMapNode>` elemento deve incluir uma `title` do atributo e, opcionalmente, pode incluir `url` e `description` atributos, entre outros; cada vazio `url` atributo deve ser exclusivo.

Insira o seguinte XML para o `Web.sitemap` arquivo:

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

A marcação de mapa de site acima define a hierarquia mostrada na Figura 3.


[![O mapa de Site representa uma estrutura de Navegação hierárquica](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**Figura 3**: O mapa de Site representa uma estrutura de Navegação hierárquica ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Etapa 3: Atualizar a página mestra para incluir uma Interface do usuário de navegação

O ASP.NET inclui uma série de controles de Web relacionados a navegação para a criação de uma interface do usuário. Isso inclui os controles SiteMapPath, TreeView e Menu. Os controles Menu e TreeView processam a estrutura do mapa de site em um menu ou uma árvore, respectivamente, enquanto o SiteMapPath exibe um rastreamento que mostra o nó atual que está sendo visitado, bem como seus ancestrais. Os dados de mapa do site pode ser vinculado a outros dados de controles da Web usando o SiteMapDataSource e pode ser acessados programaticamente por meio de `SiteMap` classe.

Como uma discussão completa sobre a estrutura do mapa do Site e os controles de navegação está além do escopo desta série de tutoriais, em vez de gastar tempo para criar nossa própria interface do usuário de navegação vamos em vez disso, emprestar usada no meu *[ Trabalhando com dados no ASP.NET 2.0](../../data-access/index.md)* série de tutoriais, que usa um controle Repeater para exibir uma lista com marcadores de profundidade de dois dos links de navegação, conforme mostrado na Figura 4.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Adicionando uma lista de dois níveis de Links na coluna à esquerda

Para criar essa interface, adicione a seguinte marcação declarativa para a `Site.master` coluna esquerda da página mestra em que o texto TODO: Menu aparecerão aqui... reside no momento.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

A marcação acima associa um controle Repeater denominado `menu` a SiteMapDataSource, que retorna a hierarquia do mapa de site definida em `Web.sitemap`. Desde o controle de SiteMapDataSource [ `ShowStartingNode` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) é definido como False, ele começará a retornar a hierarquia do mapa de site começando com os descendentes do nó inicial. O Repeater exibe cada um de nós (atualmente, apenas a participação) em um `<li>` elemento. Repetidor interna e outro, em seguida, exibe os filhos do nó atual em uma lista não ordenada aninhada.

Figura 4 mostra a saída renderizada da marcação acima com a estrutura de mapa de site que criamos na etapa 2. O Repeater renderiza marcação da lista não ordenada de baunilha; as regras de folha de estilos em cascata definidas na `Styles.css` são responsável pelo layout estética forma. Para obter uma descrição mais detalhada de como funciona a marcação acima, consulte a [páginas mestras e navegação no Site](https://asp.net/learn/data-access/tutorial-03-vb.aspx) tutorial.


[![A Interface do usuário de navegação é renderizado usando aninhados não ordenada listas](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**Figura 4**: A Interface de usuário de navegação é renderizado usando aninhados não ordenada listas ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>Adicionando navegação estrutural

Além da lista de links na coluna esquerda, vamos também ter cada exibição de página uma [trilha](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Uma trilha é um elemento de interface do usuário de navegação que mostra os usuários rapidamente sua posição atual dentro da hierarquia do site. O controle SiteMapPath usa a estrutura do mapa do Site para determinar o local da página atual no mapa do site e, em seguida, exibe uma trilha com base nessas informações.

Especificamente, adicione uma `<span>` elemento ao cabeçalho da página mestra `<div>` elemento e definir a nova `<span>` do elemento `class` de atributo para navegação estrutural. (O `Styles.css` classe contém uma regra para uma classe estrutural.) Em seguida, adicione um SiteMapPath nesse novo `<span>` elemento.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

Figura 5 mostra a saída do SiteMapPath ao visitar `~/Membership/CreatingUserAccounts.aspx`.


[![A trilha de navegação exibe a página atual e seus ancestrais no Site do mapa](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**Figura 5**: A trilha de navegação exibe a página atual e seus ancestrais no mapa do Site ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Etapa 4: Remover a entidade personalizada e a lógica de identidade

No *<a id="_msoanchor_7"> </a> [configuração de autenticação de formulários e tópicos avançados](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* tutorial vimos como associar objetos personalizados de entidade de segurança e identidade para o usuário autenticado. Conseguimos isso com a criação de um manipulador de eventos no `Global.asax` para o aplicativo `PostAuthenticateRequest` evento, que é acionado depois que o `FormsAuthenticationModule` tiver autenticado o usuário. Esse manipulador de eventos substituímos o `GenericPrincipal` e `FormsIdentity` objetos adicionados pelo `FormsAuthenticationModule` com o `CustomPrincipal` e `CustomIdentity` objetos que criamos neste tutorial.

Embora os objetos de entidade de segurança e identidade personalizados são úteis em determinados cenários, na maioria dos casos, o `GenericPrincipal` e `FormsIdentity` objetos são suficientes. Consequentemente, acho que valeria a pena para retornar para o comportamento padrão. Fazer essa alteração por remover ou comentar a `PostAuthenticateRequest` manipulador de eventos ou excluindo o `Global.asax` totalmente.

> [!NOTE]
> Depois que você comentada ou removido o código na `Global.asax`, será necessário comentar o código na `Default.aspx's` classe code-behind que projeta o `User.Identity` propriedade para um `CustomIdentity` instância.


## <a name="step-5-programmatically-creating-a-new-user"></a>Etapa 5: Criando um novo usuário programaticamente

Para criar uma nova conta de usuário por meio do uso de estrutura de associação a `Membership` da classe [ `CreateUser` método](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx). Esse método tem parâmetros para o nome de usuário, senha e outros campos relacionados ao usuário de entrada. Na invocação, que delega a criação da nova conta de usuário para o provedor de associação configurado e, em seguida, retorna um [ `MembershipUser` objeto](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) que representa a conta de usuário recém-criada.

O `CreateUser` método tem quatro sobrecargas, cada um aceita um número diferente de parâmetros de entrada:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Esses quatro sobrecargas diferem na quantidade de informações que são coletadas. A primeira sobrecarga, por exemplo, requer apenas o nome de usuário e uma senha para a nova conta de usuário, enquanto o segundo é também requer o endereço de email do usuário.

Essas sobrecargas existem porque as informações necessárias para criar uma nova conta de usuário dependem de definições de configuração do provedor de associação. No *<a id="_msoanchor_8"> </a> [criando o esquema de associação no SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* tutorial, examinamos especifica definições de configuração de provedor de associação no `Web.config`. Tabela 2 incluído uma lista completa das definições de configuração.

Uma dessas configurações de provedor de associação que afeta o que `CreateUser` sobrecargas podem ser usadas é o `requiresQuestionAndAnswer` configuração. Se `requiresQuestionAndAnswer` é definido como `true` (o padrão), em seguida, ao criar uma nova conta de usuário, deve especificar uma pergunta de segurança e uma resposta. Essas informações são usadas mais tarde se o usuário precisar redefinir ou alterar sua senha. Especificamente, nesse momento, eles são mostrados a pergunta de segurança e eles devem digitar a resposta correta para redefinir ou alterar sua senha. Consequentemente, se o `requiresQuestionAndAnswer` é definido como `true` e em seguida, chamar qualquer uma das duas primeiras `CreateUser` sobrecarrega resulta em uma exceção porque a pergunta de segurança e a resposta estão ausentes. Como nosso aplicativo estiver configurado para exigir uma pergunta de segurança e uma resposta, precisamos usar uma das duas sobrecargas último durante a criação do usuário por meio de programação.

Para ilustrar o uso de `CreateUser` método, vamos criar uma interface do usuário em que podemos solicitar ao usuário seu nome, senha, email e uma resposta a uma pergunta de segurança predefinidos. Abra o `CreatingUserAccounts.aspx` página o `Membership` pasta e adicione os seguintes controles de Web para o controle de conteúdo:

- Uma caixa de texto chamada `Username`
- Uma caixa de texto denominada `Password`, cujo `TextMode` estiver definida como `Password`
- Uma caixa de texto chamada `Email`
- Um rótulo denominado `SecurityQuestion` com seu `Text` propriedade limpos
- Uma caixa de texto chamada `SecurityAnswer`
- Um botão chamado `CreateAccountButton` cujo `Text` propriedade é definida para criar a conta de usuário
- Um controle de rótulo denominado `CreateAccountResults` com seu `Text` propriedade limpos

Neste ponto, sua tela deve ser semelhante à mostrada na Figura 6 de captura de tela.


[![Adicionar vários controles da Web para a página CreatingUserAccounts.aspx](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**Figura 6**: adicione os diversos controles de Web para o `CreatingUserAccounts.aspx Page` ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image18.png))


O `SecurityQuestion` rótulo e `SecurityAnswer` TextBox destinam-se para exibir uma pergunta de segurança predefinida e coletar a resposta do usuário. Observe que a pergunta de segurança e a resposta são armazenadas em uma base por usuário, portanto, é possível permitir que cada usuário definir sua próprias pergunta de segurança. No entanto, para este exemplo, decidi usar uma pergunta de segurança universal, ou seja: o que é a sua cor favorita?

Para implementar essa pergunta de segurança predefinidos, adicione uma constante para a classe de code-behind da página denominada `passwordQuestion`, atribuindo-a pergunta de segurança. Em seguida, nos `Page_Load` manipulador de eventos, atribuir nesse constante para o `SecurityQuestion` do rótulo `Text` propriedade:

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

Em seguida, crie um manipulador de eventos para o `CreateAccountButton'` s `Click` eventos e adicione o seguinte código:

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

O `Click` manipulador de eventos começa definindo uma variável chamada `createStatus` do tipo [ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` é uma enumeração que indica o status do `CreateUser` operação. Por exemplo, se a conta de usuário é criada com êxito, resultante `MembershipCreateStatus` instância será definida como um valor de `Success;` por outro lado, se a operação falhar porque já existe um usuário com o mesmo nome de usuário, ele será definido como um valor de `DuplicateUserName`. No `CreateUser` sobrecarga que usamos, precisamos passar um `MembershipCreateStatus` instância para o método. Esse parâmetro é definido como o valor apropriado dentro do `CreateUser` método e podemos pode examinar seu valor após a chamada de método para determinar se a conta de usuário foi criada com êxito.

Depois de chamar `CreateUser`, passando `createStatus`, um `Select Case` instrução é usada para gerar uma mensagem apropriada, dependendo do valor atribuído a `createStatus`. As figuras 7 mostra a saída quando um novo usuário foi criado com êxito. As figuras 8 e 9 mostram a saída quando a conta de usuário não é criada. Na Figura 8, o visitante inseriu uma senha de cinco letras que não atende aos requisitos de força de senha escritos nas definições de configuração do provedor de associação. Figura 9, o visitante está tentando criar uma conta de usuário com um nome de usuário existente (aquele criado na Figura 7).


[![Uma nova conta de usuário é criado com êxito](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**Figura 7**: uma nova conta de usuário é criado com êxito ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image21.png))


[![A conta de usuário não é criada porque a senha fornecida é muito fraca](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**Figura 8**: A conta de usuário não é criada porque a senha fornecida é muito fraca ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image24.png))


[![A conta de usuário não é criado porque o nome de usuário já em uso](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**Figura 9**: A conta de usuário não é criado porque o nome de usuário já em uso ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image27.png))


> [!NOTE]
> Você pode estar se perguntando como determinar o êxito ou Falha ao usar uma das duas primeiras `CreateUser` sobrecargas do método, nem de que tem um parâmetro de tipo `MembershipCreateStatus`. Essas duas primeiras sobrecargas lançar uma [ `MembershipCreateUserException` exceção](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) diante de uma falha, que inclui um [ `StatusCode` propriedade](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) do tipo `MembershipCreateStatus`.


Depois de criar algumas contas de usuário, verifique se as contas foram criadas, listando o conteúdo do `aspnet_Users` e `aspnet_Membership` tabelas no `SecurityTutorials.mdf` banco de dados. Como mostra a Figura 10, adicionei dois usuários por meio de `CreatingUserAccounts.aspx` página: Tito e Bruce.


[![Há dois usuários na Store de usuário de associação: Tito e Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**Figura 10**: há dois usuários na Store de usuário de associação: Tito e Bruce ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image30.png))


Enquanto o repositório de usuário associado agora inclui informações de conta de Bruce e de Tito, ainda precisamos implementar a funcionalidade que permite Bruce ou Tito fazer logon no site. No momento, `Login.aspx` valida as credenciais do usuário em relação a um conjunto de embutido em código de pares de nome de usuário/senha - faz *não* validar as credenciais fornecidas em relação a estrutura de associação. Para ver agora novas contas de usuário a `aspnet_Users` e `aspnet_Membership` tabelas devem ser suficiente. No próximo tutorial  *<a id="_msoanchor_9"> </a> [Validar usuário as credenciais contra a associação de usuário Store](validating-user-credentials-against-the-membership-user-store-vb.md)*, atualizaremos a página de logon para validar em relação ao armazenamento de associação.

> [!NOTE]
> Se você não vir quaisquer usuários em sua `SecurityTutorials.mdf` banco de dados, é possível que seu aplicativo web está usando o provedor de associação padrão, `AspNetSqlMembershipProvider`, que usa o `ASPNETDB.mdf` banco de dados como seu repositório do usuário. Para determinar se esse é o problema, clique no botão Atualizar no Gerenciador de soluções. Se um banco de dados denominado `ASPNETDB.mdf` foi adicionado para o `App_Data` pasta, esse é o problema. Retornar à etapa 4 do *<a id="_msoanchor_10"> </a> [criando o esquema de associação no SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* tutorial para obter instruções sobre como configurar corretamente o provedor de associação.


Na maioria dos criar usuário cenários de conta, o visitante é apresentado com alguma interface para inserir seu nome de usuário, senha, email e outras informações essenciais, ponto em que uma nova conta é criada. Nesta etapa, examinamos a criação de uma interface desse tipo manualmente e, em seguida, viu como usar o `Membership.CreateUser` método adicionar programaticamente a nova conta de usuário com base em entradas do usuário. Nosso código, no entanto, acabou de criar a nova conta de usuário. Ele não realizou qualquer acompanhamento das ações, como registro em log o usuário para o site sob a conta de usuário recém-criada ou enviando um email de confirmação para o usuário. Estas etapas adicionais exigiria um código adicional no botão de `Click` manipulador de eventos.

ASP.NET é fornecido com o controle CreateUserWizard, que é projetado para lidar com o processo de criação de conta de usuário, da renderização de uma interface do usuário para a criação de novas contas de usuário, para criar a conta em que a estrutura de associação e a execução de pós-conta tarefas de criação, como enviar um email de confirmação e fazendo logon de usuário recém-criada no site. Usar o controle CreateUserWizard é tão simple quanto arrastar o controle CreateUserWizard Toolbox para uma página e, em seguida, definir algumas propriedades. Na maioria dos casos, você não precisará escrever uma única linha de código. Vamos explorar esse controle interessante em detalhes na etapa 6.

Se as novas contas de usuário só são criadas por meio de uma página da web de criar conta comum, é improvável que você nunca precisará escrever código que usa o `CreateUser` método, como o controle CreateUserWizard provavelmente atenderá suas necessidades. No entanto, o `CreateUser` método é útil em cenários em que você precisa de uma experiência de usuário altamente personalizada criar conta ou quando você precisa criar novas contas de usuário por meio de uma interface alternativa de forma programática. Por exemplo, você pode ter uma página que permite que um usuário carregue um arquivo XML que contém informações de usuário de algum outro aplicativo. A página pode analisa o conteúdo do XML carregado de arquivos e cria uma nova conta para cada usuário representado no XML, chamando o `CreateUser` método.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Etapa 6: Criando um novo usuário com o controle CreateUserWizard

ASP.NET é fornecido com um número de controles da Web de logon. Esses controles ajudam a muitos cenários comuns de usuário relacionadas a conta e logon. O [controle CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) é um exemplo desse controle é projetado para apresentar uma interface do usuário para adicionar uma nova conta de usuário para a estrutura de associação.

Como muitos dos outros controles de Web relacionadas ao logon, CreateUserWizard pode ser usado sem escrever uma única linha de código. Ele intuitivamente fornece uma interface do usuário com base em definições de configuração do provedor de associação e internamente chama o `Membership` da classe `CreateUser` método depois que o usuário insere as informações necessárias e clica no botão Create User. O controle CreateUserWizard é extremamente personalizável. Há uma série de eventos acionados durante vários estágios do processo de criação de conta. Podemos criar manipuladores de eventos, conforme necessário, para injetar a lógica personalizada no fluxo de trabalho de criação de conta. Além disso, a aparência do CreateUserWizard é muito flexível. Há uma série de propriedades que definem a aparência da interface padrão; Se necessário, o controle pode ser convertido em um modelo ou etapas de registro de usuário adicionais podem ser adicionadas.

Vamos começar com uma olhada no uso da interface padrão e o comportamento do controle CreateUserWizard. Em seguida, exploraremos como personalizar a aparência por meio de propriedades e eventos do controle.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Examinando o CreateUserWizard Interface padrão e comportamento

Volte para o `CreatingUserAccounts.aspx` página o `Membership` pasta, alterne para o modo de Design ou de divisão e, em seguida, adicione um controle CreateUserWizard na parte superior da página. O controle CreateUserWizard é arquivado na seção de controles de logon da caixa de ferramentas. Depois de adicionar o controle, defina suas `ID` propriedade para `RegisterUser`. Como a captura de tela na Figura 11 mostra, CreateUserWizard renderiza uma interface com caixas de texto para o novo nome de usuário, senha, endereço de email e pergunta de segurança e resposta.


[![Os renderizadores de controle CreateUserWizard um genérico criar a Interface do usuário](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**Figura 11**: O controle CreateUserWizard renderiza uma Interface de usuário criar genérica ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image33.png))


Vamos dedicar um tempo para comparar a interface do usuário padrão gerada pelo controle CreateUserWizard com a interface que criamos na etapa 5. Para os iniciantes, o controle CreateUserWizard permite que o visitante especificar a pergunta de segurança e a resposta, enquanto nossa interface criados manualmente usado uma pergunta de segurança predefinidos. Interface do controle CreateUserWizard também inclui controles de validação, enquanto ainda precisamos implementar a validação em campos de formulário da nossa interface. E a interface de controle CreateUserWizard inclui uma caixa de texto Confirmar senha (junto com um CompareValidator para garantir que o texto digitado a senha e caixas de texto de senha de comparação são iguais).

O interessante é que o controle CreateUserWizard consulta definições de configuração do provedor de associação durante a renderização de sua interface do usuário. Por exemplo, as caixas de texto de pergunta e resposta de segurança são exibidas apenas se `requiresQuestionAndAnswer` está definido como True. Da mesma forma, CreateUserWizard adiciona automaticamente um controle RegularExpressionValidator para garantir que os requisitos de força de senha são atendidos e define seu `ErrorMessage` e `ValidationExpression` as propriedades de base a `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`e `passwordStrengthRegularExpression` definições de configuração.

O controle CreateUserWizard, como o nome sugere, é derivado de [controle Wizard](https://msdn.microsoft.com/library/s2etd1ek.aspx). Controles de assistente são projetados para fornecer uma interface para concluir as tarefas de várias etapas. Um controle Wizard pode ter um número arbitrário de `WizardSteps`, cada um deles é um modelo que define o HTML e controles da Web para essa etapa. O controle Wizard exibe inicialmente o primeiro `WizardStep`, juntamente com controles de navegação que permitem que o usuário para passar de uma etapa para a próxima, ou para retornar às etapas anteriores.

Como mostra a marcação declarativa na Figura 11, a interface de padrão do controle CreateUserWizard inclui dois `WizardStep` s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? renderiza a interface para coletar informações sobre como criar a nova conta de usuário. Essa é a etapa mostrada na Figura 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? processa uma mensagem indicando que a conta foi criada corretamente.

O CreateUserWizard aparência e comportamento podem ser modificados, convertendo uma destas etapas para modelos ou adicionando seus próprios `WizardStep` s. Vamos examinar adicionando um `WizardStep` na interface de registro na *armazenando informações de usuário adicionais* tutorial.

Vamos ver o controle CreateUserWizard em ação. Visite o `CreatingUserAccounts.aspx` página por meio de um navegador. Inicie inserindo alguns valores inválidos na interface do CreateUserWizard. Tente inserir uma senha que não são compatíveis com os requisitos de força de senha ou deixar a caixa de texto de nome de usuário vazio. O CreateUserWizard exibirá uma mensagem de erro apropriado. Figura 12 mostra a saída ao tentar criar um usuário com uma senha forte insuficiente.


[![O CreateUserWizard injeta automaticamente os controles de validação](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**Figura 12**: O CreateUserWizard automaticamente injeta controles de validação ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image36.png))


Em seguida, insira os valores apropriados em CreateUserWizard e clique no botão Create User. Supondo que os campos obrigatórios foram inseridos e a intensidade da senha é suficiente, CreateUserWizard será criar uma nova conta de usuário por meio da estrutura de associação e, em seguida, exibirá o `CompleteWizardStep`da interface (consulte a Figura 13). Nos bastidores, o CreateUserWizard chama o `Membership.CreateUser` método, exatamente como fizemos na etapa 5.


[![Uma nova conta de usuário tiver sido criado com êxito](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**Figura 13**: uma nova conta de usuário tiver sido criado com êxito ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image39.png))


> [!NOTE]
> Como mostra a Figura 13, o `CompleteWizardStep`da interface inclui um botão continuar. No entanto, no momento ao clicar nele simplesmente executa um postback, deixando o visitante na mesma página. Na Personalizando o CreateUserWizard aparência e comportamento por meio de propriedades de sua seção, veremos como você pode ter esse botão enviar o visitante a `Default.aspx` (ou alguma outra página).


Depois de criar uma nova conta de usuário, retorne ao Visual Studio e examine os `aspnet_Users` e `aspnet_Membership` tabelas, como fizemos na Figura 10 para verificar se a conta foi criada com êxito.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Personalizando o CreateUserWizard comportamento e aparência por meio de suas propriedades

O CreateUserWizard pode ser personalizado de várias maneiras, por meio de propriedades, `WizardStep` s e manipuladores de eventos. Nesta seção vamos examinar como personalizar a aparência do controle por meio de suas propriedades; a próxima seção examina estendendo o comportamento do controle por meio de manipuladores de eventos.

Praticamente todo o texto exibido na interface do usuário padrão do controle CreateUserWizard pode ser personalizado por meio de sua grande quantidade de propriedades. Por exemplo, os rótulos de nome de usuário, senha, confirme a senha, email, pergunta de segurança e resposta de segurança exibidos à esquerda das caixas de texto podem ser personalizados com o [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), e [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) propriedades, respectivamente. Da mesma forma, há propriedades para especificar o texto para os botões de Create User e continuar a `CreateUserWizardStep` e `CompleteWizardStep`, bem como se esses botões são renderizados como botões, botões de link ou ImageButtons.

As cores, bordas, fontes e outros elementos visuais são configuráveis por meio de um host de propriedades de estilo. O controle CreateUserWizard em si tem as propriedades de estilo de controle da Web comuns - `BackColor`, `BorderStyle`, `CssClass`, `Font`e assim por diante – e há uma série de propriedades de estilo para definir a aparência para seções específicas das Interface do CreateUserWizard. O [ `TextBoxStyle` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), por exemplo, define os estilos para as caixas de texto no `CreateUserWizardStep`, enquanto o [ `TitleTextStyle` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) define o estilo do título (inscrever-se para seu novo Conta).

Além das propriedades relacionadas à aparência, há um número de propriedades que afetam o comportamento do controle CreateUserWizard. O [ `DisplayCancelButton` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), se definido como True, exibe um botão Cancelar ao lado do botão Create User (o valor padrão é False). Se você exibir o botão Cancelar, certifique-se também definir a [ `CancelDestinationPageUrl` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), que especifica a página que o usuário é enviado para depois de clicar em Cancelar. Conforme observado na seção anterior, o botão Continuar no `CompleteWizardStep`da interface causa um postback, mas deixa o visitante na mesma página. Para enviar o visitante a alguma outra página depois de clicar no botão para continuar, basta especificar a URL na [ `ContinueDestinationPageUrl` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Vamos atualizar o `RegisterUser` controle CreateUserWizard para mostrar um botão Cancelar e enviar o visitante a `Default.aspx` quando os botões cancelar ou continuar são clicados. Para fazer isso, defina a `DisplayCancelButton` propriedade para True e ambas as `CancelDestinationPageUrl` e `ContinueDestinationPageUrl` propriedades para ~ / default. aspx. Figura 14 mostra o CreateUserWizard atualizado quando visualizado por meio de um navegador.


[![O CreateUserWizardStep inclui um botão Cancelar](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**Figura 14**: O `CreateUserWizardStep` inclui um botão Cancelar ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image42.png))


Quando um visitante entra em um nome de usuário, senha, endereço de email e pergunta de segurança e resposta e clica em Create User, uma nova conta de usuário é criada e o visitante está registrado no que o usuário recém-criado. Supondo que a pessoa que visitar a página é criar uma nova conta por conta própria, esse é provavelmente o comportamento desejado. No entanto, você talvez queira permitir que os administradores adicionem novas contas de usuário. Dessa forma, seria possível criar a conta de usuário, mas o administrador permanecer conectado como administrador (e não como a conta recém-criada). Esse comportamento pode ser modificado por meio de booliana [ `LoginCreatedUser` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Contas de usuário na estrutura associação contém um sinalizador aprovado; os usuários que não foram aprovados são puder fazer logon no site. Por padrão, uma nova conta criada é marcada como aprovado, permitindo que o usuário faça logon no site imediatamente. No entanto, é possível, ter novas contas de usuário marcadas como não aprovados. Talvez você queira que um administrador para aprovar manualmente novos usuários antes de fazer logon em; ou talvez você queira verificar se o endereço de email inserido na inscrição é válido antes de permitir que um usuário faça logon. Tudo o que pode ser o caso que a conta de usuário recém-criado marcada como não aprovados, definindo o controle de CreateUserWizard [ `DisableCreatedUser` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) como True (o padrão é False).

Incluem outras propriedades relacionadas ao comportamento de nota `AutoGeneratePassword` e `MailDefinition`. Se o [ `AutoGeneratePassword` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) está definido como True, o `CreateUserWizardStep` não exibe as caixas de texto senha e Confirmar senha; em vez disso, senha do usuário recém-criado é gerada automaticamente usando o `Membership` da classe [ `GeneratePassword` método](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). O `GeneratePassword` método constrói uma senha de um comprimento especificado e com um número suficiente de caracteres não alfanuméricos para satisfazer os requisitos de força de senha configurada.

O [ `MailDefinition` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) é útil se você quiser enviar um email para o endereço de email especificado durante o processo de criação de conta. O `MailDefinition` propriedade inclui uma série de subpropriedades para definir as informações sobre a mensagem de email construído. Estas subpropriedades incluem opções como `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, e `BodyFileName`. O [ `BodyFileName` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) aponta para um arquivo de texto ou HTML que contém o corpo da mensagem de email. O corpo dá suporte a dois espaços reservados predefinidos: `<%UserName%>` e `<%Password%>`. Esses espaços reservados, se estiver presente no `BodyFileName` de arquivos, será substituído pelo nome e senha do usuário recém-criada.

> [!NOTE]
> O `CreateUserWizard` do controle `MailDefinition` propriedade especifica apenas os detalhes sobre a mensagem de email é enviado quando uma nova conta é criada. Ele não inclui todos os detalhes sobre como a mensagem de email, na verdade, é enviada (ou seja, se um diretório de recebimento de mensagem ou de servidor SMTP é usado, informações de autenticação e assim por diante). Esses detalhes de baixo nível precisam ser definidos na `<system.net>` seção `Web.config`. Para obter mais informações sobre essas definições de configuração e enviar email de ASP.NET 2.0 em geral, consulte o [perguntas frequentes em SystemNetMail.com](http://www.systemnetmail.com/) e o meu artigo [envio de Email no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Estendendo o comportamento do CreateUserWizard usando manipuladores de eventos

O controle CreateUserWizard eleva um número de eventos durante o fluxo de trabalho. Por exemplo, depois que um visitante de insere seu nome de usuário, senha e outras informações pertinentes e clica no botão Create User, o controle CreateUserWizard gera sua [ `CreatingUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Se houver um problema durante o processo de criação, o [ `CreateUserError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) é acionado; no entanto, se o usuário é criado com êxito, o [ `CreatedUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) é gerado. Há eventos de controle CreateUserWizard adicionais que são acionados, mas estes são três os mais similar.

Em determinados cenários podemos tocar no fluxo de trabalho CreateUserWizard, que pode ser feito criando um manipulador de eventos para o evento apropriado. Para ilustrar isso, vamos melhorar a `RegisterUser` controle CreateUserWizard incluir alguma validação personalizada em nome de usuário e senha. Em particular, vamos melhorar nosso CreateUserWizard, para que os nomes de usuário não podem conter espaços à esquerda ou à direita e o nome de usuário não pode aparecer em qualquer lugar na senha. Resumindo, gostaríamos de impedir que alguém criando um nome de usuário, como "Scott", ou ter uma combinação de nome de usuário/senha, como Scott e Scott.1234.

Para fazer isso, vamos criar um manipulador de eventos para o `CreatingUser` evento para executar nossas verificações de validação extra. Se os dados fornecidos não são válidos é necessário cancelar o processo de criação. Também precisamos adicionar um controle da Web do rótulo para a página para exibir uma mensagem explicando que o nome de usuário ou senha é inválida. Comece adicionando um controle de rótulo sob o controle CreateUserWizard, definindo sua `ID` propriedade para `InvalidUserNameOrPasswordMessage` e seu `ForeColor` propriedade `Red`. Limpar seus `Text` propriedade e defina sua `EnableViewState` e `Visible` propriedades como False.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

Em seguida, crie um manipulador de eventos do controle CreateUserWizard `CreatingUser` eventos. Para criar um manipulador de eventos, selecione o controle no Designer e, em seguida, vá para a janela de propriedades. A partir daí, clique no ícone de raio e, em seguida, clique duas vezes no evento apropriado para criar o manipulador de eventos.

Adicione o seguinte código ao manipulador de eventos do `CreatingUser`:

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

Observe que o nome de usuário e senha inseridos no controle CreateUserWizard estão disponíveis por meio de seu [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) e [ `Password` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx), respectivamente. Podemos usar essas propriedades no manipulador de eventos acima para determinar se o nome de usuário fornecido contém espaços à esquerda ou à direita e se o nome de usuário é encontrado dentro a senha. Se alguma dessas condições for atendida, uma mensagem de erro é exibida na `InvalidUserNameOrPasswordMessage` rótulo e o manipulador de eventos `e.Cancel` estiver definida como `True`. Se `e.Cancel` é definido como `True`, CreateUserWizard causam curto-circuito seu fluxo de trabalho, cancelando efetivamente o processo de criação de conta de usuário.

A Figura 15 mostra uma captura de tela de `CreatingUserAccounts.aspx` quando o usuário insere um nome de usuário com espaços à esquerda.


[![Os nomes de usuário à esquerda ou espaços à direita não são permitidos.](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**Figura 15**: não são permitidos nomes de usuário à esquerda ou espaços à direita ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image45.png))


> [!NOTE]
> Veremos um exemplo de uso do controle CreateUserWizard `CreatedUser` evento na *<a id="_msoanchor_11"> </a> [armazenar informações de usuário adicionais](storing-additional-user-information-vb.md)* tutorial.


## <a name="summary"></a>Resumo

O `Membership` da classe `CreateUser` método cria uma nova conta de usuário na estrutura de associação. Ele faz isso ao delegar a chamada para o provedor de associação configurado. No caso do `SqlMembershipProvider`, o `CreateUser` método adiciona um registro para o `aspnet_Users` e `aspnet_Membership` tabelas de banco de dados.

Enquanto as novas contas de usuário podem ser criadas por meio de programação (como vimos na etapa 5), uma abordagem mais rápida e fácil é usar o controle CreateUserWizard. Esse controle apresenta uma interface do usuário de várias etapas para coletar informações do usuário e criar um novo usuário na estrutura de associação. Nos bastidores, esse controle usa o mesmo `Membership.CreateUser` método como examinado na etapa 5, mas o controle cria a interface do usuário, controles de validação e responde a erros de criação de conta de usuário sem ter que escrever uma linha de código.

Neste ponto, temos a funcionalidade em vigor para criar novas contas de usuário. No entanto, a página de logon ainda é a validação em relação a essas credenciais embutidos que especificamos novamente no segundo tutorial. No <a id="_msoanchor_12"> </a> [próximo tutorial](validating-user-credentials-against-the-membership-user-store-vb.md) atualizaremos `Login.aspx` validar o usuário do fornecido as credenciais em relação a estrutura de associação.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [`CreateUser` Documentação técnica](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Visão geral do controle CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Criando um provedor de mapa de Site com base no sistema de arquivos](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Criando uma Interface do usuário passo a passo com o controle do ASP.NET 2.0 Assistente](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Examinar o ASP.NET 2.0 da navegação do Site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Páginas mestras e navegação no Site](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [O provedor de mapa de Site do SQL que você estava esperando para](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é  *[Sams Teach por conta própria ASP.NET 2.0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Teresa Murphy. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-the-membership-schema-in-sql-server-vb.md)
> [Próximo](validating-user-credentials-against-the-membership-user-store-vb.md)
